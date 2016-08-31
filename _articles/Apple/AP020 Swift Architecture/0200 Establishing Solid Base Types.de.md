---
section:    Apple
topicid:    AP020
refid:      AP020-0200
permalink:  /:language/articles/AP020-0200.html
title:      Wichtige Basis-Typen für jedes Projekt
date:       2016-08-30 00:00:00
author:     Florian Pfisterer
language:   de
---

Dieser Artikel soll eine Reihe an wichtigen Basis-Typen vorstellen, die man in so gut wie jedem Swift-Projekt nutzen
kann. Diese Typen, wenn man sie intelligent benutzt, machen den Code sauberer, aussagekräftiger und einfacher zu testen.

## 1. Der Result Typ

In so gut wie jedem Projekt gibt es Aufrufe von Funktionen, die fehlschlagen können. In Swift ist es in so einem Fall
üblich, dass die Funktion entweder einen Error wirft oder ``nil`` zurück gibt. Auch wenn das in einigen Fällen passend
ist, möchte man in anderen Fällen weder Errors werfen und weitergeben (``rethrow``) noch möchte man die ganze Zeit
Optionals unwrappen.

Um dieses Problem zu lösen, betrachte man den folgenden generischen Result Typ:

```swift
enum Result<T>
{
    case success(T)
    case error(ErrorType)
}
```

Dieser Typ, wenn man ihn als Ergebnis eines Funktionsaufrufs benutzt beispielsweise, repräsentiert einen der folgenden
Fälle:

1. Der Aufruf war erfolgreich, und hier ist das Ergebnis: ein Objekt des Typs `T`
2. Irgendwo ist ein Error passiert, und hier ist er: ein `ErrorType` `case` (man könnte auch `NSError` benutzen)

Der Result Typ erweitert eigentlich nur den Swift Standard-Optional-Typ, indem bei Misserfolg auch gleich der Error
mitgeliefert wird, warum kein Ergebnisobjekt (``T``) vorliegt.

```swift
enum Optional<T>
{
    case some(T)
    case none
}
```

Ein Swift Optional ist wirklich nicht mehr als das, die "**?**"s und "**!**"s sind nur syntaktische Vereinfachungen.

### Beispiel-Implementierung

Man betrachte die folgende Funktion, die asynchron `Users` aus einer Netzwerkdatenbank lädt (zum Beispiel innerhalb
einer Klasse `DatabaseClient`).

```swift
static func loadUsers(completion: (Result<[User]>) -> Void)
{
    let queue =
        dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_HIGH, 0)

    dispatch_async(queue, {
        do
        {
            // aufwändiger Netzwerkaufruf, der einen Error werfen könnte
            let users: [User] = try networkOperationThatMightThrow()
            completion(.success(users))
        }
        catch let error
        {
            completion(.error(error))
        }
    })
}
```

Wenn wir diese Funktion dann zum Beispiel in unserem  `ViewController` benutzen, müssen wir den Aufruf weder in einem
`do-try-catch` Block einbauen, noch müssen wir Optionals unwrappen. Unser Code bleibt sauber, wartbar und verständlich.

```swift
DatabaseClient.loadUsers { result in
    switch result
    {
    case .error(let error):
        // einen Alert zeigen etc.
    case .success(let users):
        // die UI updaten und die Users zeigen
    }
}
```

## 2. Ein Model Typ

Die allermeisten iOS Apps speichern und laden Daten aus einer Datenbank.

Wenn man Realm oder CoreData als lokale Datenbank benutzt, bekommt man 'von Haus aus' bereits eine Superklasse, von der
die eigenen Model-Typen erben (`Object` bei Realm und `NSManagedObject` bei CoreData). Aber wenn man diese Bibliotheken
nicht benutzt - beispielsweise wenn man mit einer selbst definierten REST API auf einem Server arbeitet - muss man einen
solchen Basis-Typ für seine Model-Typen selbst erstellen.
Warum? Weil dies der Wiederholung von Code vorbeugt und den Code so einfacher zu warten macht. Anstatt, dass man eine
`save` Funktion für jeden einzelnen Model-Typ schreiben muss, können alle davon die dafür notwendigen Properties teilen
und so alle **eine** Implementierung einer `save` Funktion nutzen.

### Wie implementiere ich diesen Model Basis-Typ?

Die erste Idee könnte sein, einfach eine Klasse namens Model zu erstellen, von der die anderen Model-Typen alle erben.
Diese Klasse könnte dann bestimmte Instanzvariablen wie eine `id` sowie Funktionalität wie eine `save` Funktion
beinhalten. Das Problem mit dieser Herangehensweise ist, dass es in den allermeisten Fällen besser ist 'value-type'
`structs` anstatt von 'reference-type' `classes` zu nutzen. Hier werde ich nicht näher auf diese Thematik eingehen, aber
man kann in einem anderen Artikel mehr über den Unterschied zwischen 'value-types' und 'reference-types' erfahren
**TODO: Link einfügen**.

Wie also teilen wir Funktionalität zwischen `structs`? Vererbung ist hier keine Option.

### Wir benutzen 'Protocol Extensions'

In Swift kann man nun `protocol extensions` benutzen, um Funktionalität zu `protocols` hinzuzufügen. Man betrachte die
folgende erste Implementierung unseres Model-Typs:

```swift
protocol Model
{
    var id: String { get }

    var createdAt: NSDate { get }
    var updatedAt: NSDate { get set }
}
```

Jeder Model-Typ (wie zum Beispiel Kunde, Produkt, Rechnung, etc.) muss jetzt diese Properties implementieren. Noch wird
keine Funktionalität geteilt, diese fügen wir jetzt aber mit Hilfe von Swifts `protocol extensions` hinzu:

```swift
extension Model
{
    func save(completion: Result<Self>)
    {
        // das Objekt z.B. mithilfe der `id` speichern
    }
}
```

Man könnte weitere Funktionen wie `update`, `delete`, etc. implementieren, aber der Punkt ist, dass jeder Model-Typ, den
wir für unsere Anwendung brauchen nun diese Funktionalität definiert in unserem Model `protocol` nutzen kann. Wenn sich
etwas an der Datenbank etc. ändert, müssen wir somit den Code nur an **einer** Stelle verändern.

### Eine kleine Zusatzinformation

Wenn manche Model-Typen aufgrund der Business-Logik ihre eigene Implementierung der `save` Funktion benötigen, muss man
die Deklaration dieser Funktion neben der `extension` auch in der `protocol` Deklaration direkt einbauen. So wird dann,
wenn man `save` auf einem Objekt jener Klassen aufruft, die richtige Implementierung verwendet.
