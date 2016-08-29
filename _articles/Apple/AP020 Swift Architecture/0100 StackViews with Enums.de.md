---
section:    Apple
topic:      Swift Architecture
refid:      AP020-0100
permalink:  /articles/AP020-0100.html
title:      StackViews mit Enums
date:       2016-08-23 00:00:00
author:     Florian Pfisterer
---

Dieser Artikel befasst sich mit der seit iOS 9 verfügbaren Klasse `UIStackView`,
mit Hilfe der man auch komplexe UIs ohne unübersichtlich viele Constraints im Storyboard simpel umsetzen kann. Wenn man
StackViews jedoch im ViewController erstellt - zum Beispiel weil man dynamisch Daten vom Server bekommt - wird das
schnell übersichtlich. In diesem Artikel wird eine Architektur vorgestellt, mit der man auch im Code erstellte
StackViews sehr übersichtlich und wartbar gestalten kann.  

## Kurze Einführung: Warum StackViews?
Oft baut man UIs, die eine bestimmte lineare Struktur von Views enthalten. Als Beispiel dient ein Artikel wie dieser,
der aus verschiedenen Typen von Views besteht, die in einer vertikalen Struktur angeordnet sind. Es gibt Überschriften,
Code-Blöcke, Bilder, Tabellen und mehr.

Würde man einen Artikel wie diesen hier in einem Storyboard umsetzen, müsste man die verschiedenen `UILabels`,
`UITextViews`, `UIImageViews` und eventuelle individuelle `UIViews` einzeln im Interface Builder hinzufügen, dann die
richtigen Constraints innerhalb des ViewControllers und untereinander erstellen und hoffen, dass keine 'Ambiguity
Issues' oder 'Conflicting Constraints' auftreten.

Die Lösung ist die Klasse `UIStackView`. Man fügt die verschiedenen UI-Elemente als 'Arranged Subview' dem StackView
hinzu und wählt dann aus einigen Konfigurationsmöglichkeiten, wie das StackView seine Subviews darstellen soll. Im
Interface Builder sieht das so aus:

![StackView

Konfigurationsmöglichkeiten](../../../BestPractices/public/images/AP020/0100/stackview-configuration-options-in-interface-builder.png)

Man kann also folgende Konfigurationen vornehmen:

* **Axis**: die Achse, an der die Subviews ausgerichtet werden sollen - entweder vertikal oder horizontal
* **Alignment**: die vertikale (horizontale) Anordnung der Subviews wenn als Achse horizontal (vertikal) gewählt ist -
Fill, Leading, Trailing oder Center (siehe Bild unten)
* **Distribution**: die Verteilung der Subviews entlang der Achse - Fill, Fill Equally, Fill Proportionally, Equal
Spacing oder Equal Centering (siehe Bild unten)
* **Spacing**: der Abstand zwischen den Subviews auf der Achse - wird ignoriert, wenn als Distribution _nicht_ Equal
Spacing gewählt ist  (siehe Bild unten)

![StackView Konfigurationen als
Grafik](../../../BestPractices/public/images/AP020/0100/stackview-configurations-scheme.png)

Bildquelle: [Apple Developer Class
Reference](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIStackView_Class_Reference/)

Arbeitet man mit dem Interface Builder, ist das sehr viel übersichtlicher als einzelne Constraints für jedes View, und
durch 'Nested StackViews' - also StackViews als 'Arranged Subviews' von StackViews - kann man auch so gut wie jede UI
damit darstellen.

### TL;DR
StackViews sind UI-Elemente, die Subviews entlang einer Achse durch Konfigurationen automatisch positionieren.

## Das Problem
Das Problem ist, dass man nicht immer alles im Interface Builder machen kann und je nach Anwendungsfall UIs auch
dynamisch erstellen muss - zum Beispiel basiert auf Daten aus dem Backend oder auf Einstellungen des Benutzers. Und dann
wird es leicht unübersichtlich im ViewController - was man auf jeden Fall vermeiden sollte.

Folgendermaßen würde man beispielsweise ein Label im Code erstellen und einem StackView hinzufügen:

```swift
let label = UILabel()
label.text = Daten.aus(dem: backend)
label.font = UIFont.systemFontOfSize(16)
// ...
stackview.addArrangedSubview(label)
```
Das sieht zwar alleine noch ganz in Ordnung aus, baut man sich jedoch aus verschiedenen Elementen eines Katalogs
beispielsweise - Bilder, Überschriften, Texte, Tabellen, etc. ein StackView zusammen, wird es zu unübersichtlich.

## Der Ansatz
Wir definieren uns einen Datentyp, der verschiedene Arten von StackView-Elementen darstellen und in `UIViews` umwandeln
kann. Damit isolieren wir das einfache "Übersetzen" vom Inhalt der UI in StackView-Subviews aus dem ViewController. In
Swift bieten sich dafür **Enums** an, da Enums eine Sammlung von vordefinierten Typen sind, die in Swift 'associated
values' - also zugeordnete Werte - enthalten können. Mehr dazu unten.

## Die Lösung
Wir bekommen verschiedene Inhalte vom Server, wobei auch der Typ mitgeteilt wird - also ob es sich um eine URL zu einem
Bild, um Überschrifts-Text oder normalen Text handelt beispielsweise. Um den Inhalt dann letzendlich darzustellen,
entwerfen wir folgenden Enum-Typ:

```swift
enum StackViewElement
{
    case text(String)
    case image(UIImage)
    // ... weitere Typen wenn gebraucht
}
```

Dieser kann die für unser Beispiel relevanten 2 Typen von Informationen - Text und Bilder - speichern. Möchte man jetzt
also einen Textblock in dieser Struktur darstellen, kann man ganz einfach schreiben `let title: StackViewElement =
.text("StackViews mit Enums")`. So kann man ein Array von Strings, das man aus der `.plist`-Datei liest, ganz einfach in
ein Array von `StackViewElement` umwandeln.

### Und wie bekommt man das jetzt auf den Screen?
Ganz einfach: man baut sich eine 'computed property' auf `StackViewElement`, die basiert auf dem jeweiligen 'Enum Case'
ein `UIView` zurückgibt:

```swift
extension StackViewElement
{
    var view: UIView {
        switch self
        {
            case .text(let content):
                let label = UILabel()
                label.text = content
                // weitere Konfigurationen ...
                return label

            case .image(let image):
                let imageView = UIImageView(image: image)
                imageView.contentMode = .ScaleAspectFill
                return imageView
        }
    }
}
```

Um unser Array von `StackViewElement` nun in ein fertiges `UIStackView` umzuwandeln schreiben wir uns einen eigenen
'convenience Initializer' für `UIStackViews`:

```swift
extension UIStackView
{
    convenience init(elements: [StackViewElement])
    {
        self.init()
        // bei Bedarf können wir hier schon Konfigurationen vornehmen:
        self.translatesAutoresizingMaskIntoConstraints = false
        self.axis = .Vertical
        self.spacing = 10

        // berechnet von jedem Element das View und fügt es hinzu
        elements.forEach { self.addArrangedSubview($0.view) }
    }
}
```

## Weiterführende Gedanken

### StackViewController
Um unseren ViewController noch sauberer zu gestalten, können wir die StackView-Logik noch einen Schritt weiter bringen
und direkt in eine `StackViewController` Klasse einbauen. In einer Übersicht über verschiedene Artikel beispielsweise
muss man dann nur noch ein Array von  `StackViewElement` aus den Serverdaten erstellen und damit einen
`StackViewController` initialisieren.

```swift
final class StackViewController: UIViewController
{
    private let elements: [StackViewElement]

    init(elements: [StackViewElement])
    {
        self.elements = elements
        super.init(nibName: nil, bundle: nil)
    }

    required init?(coder aDecoder: NSCoder)
    {
        super.init(coder: aDecoder)
    }
}
```
Und in `viewDidLoad` erstellen wir dann aus der `elements`-Property ein `UIStackView`:

```swift
extension StackViewController
{
    override func viewDidLoad()
    {
        super.viewDidLoad()

        let stackView = UIStackView(elements: self.elements)
        self.view.addSubview(stackView)
        // ... Constraints hinzufügen
    }
}
```

### Enums Allgemein
Dadurch, dass man in Swift zu Enum-Typen sogenannte `associated types` hinzufügen kann, also in diesem Beispiel der
`String` als Inhalt eines Labels (`StackViewElement.text("String")`), bieten Enums in Swift viele Möglichkeiten. Es
lohnt sich also auf jeden Fall, bevor man eine neue Klasse erstellt oder alles im ViewController schreibt, darüber
nachzudenken, ob man das Problem nicht auch mit einem Enum lösen könnte. Mehr dazu in anderen Artikeln.


_Dieser Artikel ist durch eine Swift Talk Episode von objc.io inspiriert:_
[Swift Talk by objc.io](https://talk.objc.io/episodes/S01E07-stack-views-with-enums)
