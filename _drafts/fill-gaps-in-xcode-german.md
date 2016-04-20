---
layout: post
title:  "Fülle die Lücken in Xcode"
categories: ios
---

In diesem Artikel geht es darum gewisse Schwächen von Xcode mithilfe von Tools und Programmierlösungen auszugleichen. Hierzu wird für jedes Thema zunächst das Problem mit Xcode und deren mögliche Folgen in der Praxis erläutert, woraufhin ein Lösungsvorschlag gegeben wird.

## Fehlende Erkennung von neuen Übersetzungs-Keys

### Problembeschreibung

Bei der Entwicklung mit Xcode gibt es die Möglichkeit mithilfe des in der `Foundation` Library definierten Makros `NSLocalizedString(key, comment)` Texte programmatisch zu lokalisieren. Dies ermöglicht das Anbieten einer App in vielen verschiedenen Programmiersprachen, deren Übersetzungen typischerweise zusammengefasst sind in der Datei `Localizable.strings`, welche für jede Sprache eine eigene Version hat. Dabei wird im Quellcode lediglich ein Key verwendet, welcher in den `.strings`-Dateien die jeweiligen Texte in den jeweiligen Sprachen zugewiesen werden. Die App sucht sich dann auf dem Endgerät je nach Geräteeinstellung die passenden Texte automatisch heraus und ersetzt die Keys, die im Quellcode verwendet wurden. Leider beherrscht Xcode jedoch nicht die Möglichkeit neue Keys, die im Quellcode bei der Programmierung hinzugefügt wurden zu erkennen und diese automatisch in allen Sprachen den `.strings`-Dateien hinzuzufügen.

### Lösungsvorschlag

TODO: Am besten wäre eine Lösung, die vollständig in ein Open Source Tool integriert ist (BartyCrouch um Scan erweitern?)

Aktuell ist uns keine perfekte Lösung für dieses Problem bekannt. In der Praxis bewährt hat sich jedoch eine Kombination aus der nicht kostenfreien App [Linguan](http://linguanapp.com) und der automatischen Übersetzungs-Funktion vom Open-Source Tool [BartyCrouch](https://github.com/Flinesoft/BartyCrouch).

Mit Linguan durchsucht man dabei nach Hinzufügen von neuen Keys im Code das gesamte Projekt nach neuen Keys ab ("Scan sources") und trägt sogleich die korrekte Übersetzung in einer Quellsprache ab (z.B. Deutsch oder Englisch) – Linguan ergänzt jedoch nur `.strings`-Dateien mit den neuen Keys, für deren Sprache man eine Übersetzung anbietet, weshalb für die Komplettlokalisierung in allen Sprachen BartyCrouch ins Spiel kommt.

Ist BartyCrouch [richtig konfiguriert](https://github.com/Flinesoft/BartyCrouch#build-script) (nutze die Translate-Funktion mit der `Localizable.strings`-Datei) so braucht man nur noch das Projekt in Xcode zu öffnen und zu builden. Beim Build wird durch das konfigurierte BartyCrouch-Skript die Quellübersetzung gelesen, maschinell in alle definierten anderen Sprachen übersetzt und alle übersetzten `Localizable.strings`-Dateien mit den neuen Keys befüllt. Diese Vorgehensweise löst das beschriebene Problem und hat auch den netten Nebeneffekt, dass die Übersetzungen später schneller gehen, da viele der kürzeren maschinellen Übersetzungen meist sogar brauchbar sind.


## Unstatisch referenzierte Übersetzungen

### Problembeschreibung

Wie weiter oben bereits erläutert bietet die `Foundation` Library das Makro `NSLocalizedString(key, comment)` um Texte in Xcode-Projekten zu übersetzen. Der `key` ist dabei vom Typ `String`, was den großen Nachteil hat, dass alle Vorteile von statischer Typisierung beim Builden von Code wegfallen. Das bedeutet konkret, dass Xcode nicht automatisiert überprüfen kann, ob zu den Werten, die als Keys im Code genutzt werden auch tatsächlich Übersetzungen existieren. So kann ein Vertipper, eine Änderung des Keys im Code oder fehlende Übersetzungen zu neuen Keys zu dem Problem führen, dass am Ende leere Texte in der App angezeigt werden und dies beim Entwickeln und builden gar nicht auffällt.

### Lösungsvorschlag

Wie für Code gibt es auch für Übersetzungen mithilfe von Tools die Möglichkeit eine Überprüfung ihrer Existens durchzuführen und ein Build fehlschlagen zu lassen, wenn ein Vertipper vorkommt oder eine Übersetzung komplett fehlt. Das Open Source Tool [Laurine](https://github.com/JiriTrecak/Laurine) untersucht nämlich automatisch alle `.strings`-Dateien auf ihre Keys und generiert aus diesen eine Code-Struktur namens `Localizations` worin sämtliche Keys per Dot-Syntax erreichbar sind und als Ergebnis die jeweilige Übersetzung liefern. Dies ermöglicht es alles Aufrufe von `NSLocalizedString(key, comment)` durch Aufrufe wie `Localizations.KeyName` zu ersetzen. Dies löst zum Einen das Problem, da Xcode bereits automatisch Code auf Existenz prüft, wenn er kompiliert wird und bietet als Nebeneffekt auch den Vorteil, dass man für sämtliche existierende Keys nun auch die Autovervollständigung nutzen kann, was bei reinen Strings nicht möglich war.

Zusätzlich zur Verwendung von Laurine sei an dieser Stelle noch eine Übersetzungs-Key-Struktur empfohlen, die die Autovervollständigungsfunktion maximal ausreizt, Übersetzungskommentare als Kontext-Informanten überflüssig macht und das Unterscheiden von Übersetzungs-Keys und tatsächlichen Übersetzungen vereinfacht. Die Struktur sollte folgende Regeln einhalten:

* Die Keys werden KOMPLETT IN GROSSBUCHSTABEN GESCHRIEBEN
* Die Keys werden hierarchisch.mit.Punkten.getrennt
* Trennungen zwischen Wörtern in den Keys werden mit_Unterschtrichen_umgesetzt

Nachfolgend seien ein paar Beispiel-Keys aufgelistet, die diese Vorgaben einhalten:

```swift
SETTINGS.TITLE
SETTINGS.FONT_SECTION.SIZE
SETTINGS.FONT_SECTION.COLOR
MODEL.ARTICLE.TITLE
MODEL.ARTICLE.RELEASE_DATE
```

In Laurine können sie dann folgendermaßen verwendet werden:

```swift
self.title = Localizations.Settings.Title
articleCell.titleLabel.text = Localizations.Model.Article.Title
```

Wir empfehlen für die Verwendung von Laurine neben der Installation per Homebrew folgendes Build-Script im Xcode-Projekt nach dem Build-Script zu BartyCrouch zu hinterlegen:

```shell
if which LaurineGenerator.swift > /dev/null; then
    # Get path to main localization file.
    SOURCE_PATH="$PROJECT_DIR/Sources/Supporting Files/en.lproj/Localizable.strings"

    # Get path to output.
    OUTPUT_PATH="$PROJECT_DIR/Sources/Constants/Localizations.swift"

    # Generate Output file.
    LaurineGenerator.swift -i "$SOURCE_PATH" -c -o "$OUTPUT_PATH"
else
    echo "warning: Laurine not installed, download it from https://github.com/JiriTrecak/Laurine"
fi
```

Das Skript setzt den Pfad der resultierenden `Localizations.swift` Datei im Ordner `Code/Constants`, benutzt Englisch als Quellsprache für die Keys (falls Englisch nicht existiert sollte der Path `en.lproj` angepasst werden) und konfiguriert die Output-Formatierung mittels `-c` Option mit CamelCasing.

## Unstatisch referenzierte Bilder, Interfaces und Farben

### Problembeschreibung

Bilder lädt man in iOS am Einfachsten mit dem Initializer der Klasse `UIImage` und übergibt dabei den Namen des Bildes. Xcode durchsucht dann automatisch sowohl die `.xcassets`-Ordner des Projekts, als auch alle direkt hinzugefügten Bilder und gleicht deren Namen mit dem übergebenen `String` ab, um so das korrekte Bild zu finden. Interfaces lädt man auf ähnliche Weise mittels Strings von Storyboards oder XIBs, Farben werden meist mit RGB-Werten bei Notwendigkeit ad-hoc angelegt. Analog zu den unstatisch referenzierten Übersetzungen (s.o.) verhält es sich auch bei per Strings referenzierten Bildern oder Interfaces so, dass Vertipper, Umbenennungen im Code oder jeweiligen Ressourcen sowie das Laden der Ressourcen im Code, die man hinzuzufügen vergisst. Das alles führt dazu, dass beim Ausführen der App Bilder fehlen, Interfaces nicht gefunden werden können oder schlichtweg die Farben an vielen unterschiedlichen Stellen gesucht und geändert werden müssen. Xcode zeigt in Fehlerfällen weder Warnungen noch Fehlermeldungen an, da es die tatsächlich existierenden Ressourcen schlichtweg nicht kennt, da sie dynamisch mit den Strings oder den RGB-Werten geladen werden.

### Lösungsvorschlag

Ressourcen sollten statisch über automatisch generierten Code gefplegt und geladen werden, sodass man einerseits gar nicht mehr mit fehlenden Ressourcen builden kann (Xcode zeigt Fehlermeldungen an, wenn referenzierter Code nicht vorhanden ist) und andererseits eine zentrale Stelle zum Verwalten und Ändern hat. Letzteres ist vor allem bei den Farben wichtig, die in den meisten Apps einheitlich sein sollten. Das Tool [SwiftGen](https://github.com/AliSoftware/SwiftGen) leistet hierbei hervorragende Dienste und durchsucht automatisch das Projekt nach Bildern, Interfaces und Farben.

Nach der Installation über Homebrew sollte für die Einrichtung eine `Colors.txt`-Datei im Hauptverzeichnis des Projekts erstellt werden, deren Inhalt etwa folgendermaßen aussehen kann:

```txt
Primary     : #40657d
Secondary   : #657d40
Accent      : #b7d3e3
```

Im nächsten Schritt konfiguriert man auch für SwifGen ein Build-Script, das folgendermaßen aussehen könnte:

```shell
if which swiftgen > /dev/null; then
    swiftgen storyboards "$PROJECT_DIR/Sources" --output "$PROJECT_DIR/Sources/Constants/Storyboards.swift"
    swiftgen images "$PROJECT_DIR/Sources" --output "$PROJECT_DIR/Sources/Constants/Images.swift"
    swiftgen colors "$PROJECT_DIR/Colors.txt" --output "$PROJECT_DIR/Sources/Constants/Colors.swift"
else
    echo "warning: SwiftGen not installed, download it from https://github.com/AliSoftware/SwiftGen"
fi
```

Auch hier werden die Ergebnis-Dateien im Ordner `Sources/Constants` abgelegt. Fortan werden die Dateien bei jedem Build neu erzeugt, sofern es seit der letzten Generierung Änderungen an den jeweiligen Ressourcen gab. Die Benutzung von Ressourcen im Code wird fortan statisch erledigt, die Ersetzungen sehen folgendermaßen aus:

```swift
// Laden aus einem Storyboard

// Ohne SwiftGen:
let homeController = UIStoryboard(name: "Home", bundle: nil).instantiateInitialViewController() as? HomeViewController
// Mit SwiftGen (empfohlen):
let homeController = StoryboardScene.Home.initialViewController() as? HomeViewController

// Laden eines Bild

// Ohne SwiftGen:
if let logo = UIImage(named: "Logo") { ... }
// Mit SwiftGen (empfohlen):
let logo = UIImage.Asset.Logo.image

// Nutzen einer Farbe

// Ohne SwiftGen:
self.view.backgroundColor = UIColor(red: 1.0, green: 0.2, blue: 0.16, alpha: 1.0)
// Mit SwiftGen (empfohlen):
self.view.backgroundColor = UIColor.Name.Primary.color
```

## Fehlende Code Conventions und TODO-Warnungen

## Manuelle Erstellung verschiedener Bilder-Auflösungen

## Fehlende Aktualisierung von Interface-Übersetzungen
