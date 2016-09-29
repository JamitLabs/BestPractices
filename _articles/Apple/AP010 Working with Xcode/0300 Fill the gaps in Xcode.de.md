---
section:    Apple
topicid:    AP010
refid:      AP010-0300
permalink:  /:language/articles/AP010-0300.html
title:      Ergänzen fehlender Xcode-Funktionen
date:       2016-01-01 00:00:00
author:     Cihat Gündüz
language:   de

prev_refid: AP010-0200
next_refid: AP020-0100
---

In diesem Artikel geht es darum gewisse Schwächen von Xcode mithilfe von Tools und Programmierlösungen auszugleichen.
Hierzu wird für jedes Thema zunächst das Problem mit Xcode und dessen mögliche Folgen in der Praxis erläutert, woraufhin
ein Lösungsvorschlag gegeben wird.


## Fehlende Aktualisierung von Übersetzungen

### Problem

Xcode bietet die Möglichkeit mithilfe des in der `Foundation` Library definierten Makros `NSLocalizedString(key,
comment)` Texte programmatisch zu lokalisieren. Dies ermöglicht das Anbieten einer App in vielen verschiedenen
Programmiersprachen, deren Übersetzungen in den `Localizable.strings`-Dateien zusammengefasst sind (für jede Sprache
eine eigene). Dabei wird im Quellcode lediglich ein Key verwendet, welcher in den `.strings`-Dateien die jeweiligen
Texte in den jeweiligen Sprachen zugewiesen werden.

Eine `Localizable.strings` für die deutsche Sprache sieht etwa so aus:

```swift
"HOME.NAV_BAR.TITLE" = "Start";
"HOME.SETTINGS_BUTTON.NORMAL_TITLE" = "Einstellungen";
"SETTINGS.NAV_BAR.TITLE" = "Einstellungen";
```

Die zugehörige englische Version dann etwa so:

```swift
"HOME.NAV_BAR.TITLE" = "Home";
"HOME.SETTINGS_BUTTON.NORMAL_TITLE" = "Settings";
"SETTINGS.NAV_BAR.TITLE" = "Settings";
```

Im Code wird das Ganze dann folgendermaßen verwendet:

```swift
titleLabel.text = NSLocalizedString("HOME.NAV_BAR.TITLE", "")
```

Führt man eine App aus, sucht sie sich auf dem Endgerät je nach Geräteeinstellung die passenden Texte automatisch heraus
und ersetzt die Keys, die im Quellcode verwendet wurden. Leider muss man in Xcode aktuell jedoch jeden neuen Key, den
man im Code mittels `NSLocalizedString` verwendet in jeder einzelnen Sprachfassung der `.strings` Dateien einzeln
händisch hinzufügen. Dies kann je nach Projekt sehr viel Zeit in Anspruch nehmen und ist zudem fehleranfällig.

Außerdem lassen sich in Xcode ähnlich wie beim `NSLocalizedString`-Makro auch textbasierte *Interface*-Elemente über
Keys lokalisieren, wobei hier jedoch eine XIB-Datei oder ein Storyboard in der Sprache "Base" zum Einsatz kommt, worin
das Design für alle Sprachen festgelegt wird (siehe [Base Internationalization](https://developer.apple.com/library/ios/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)).
Auch hier bietet Xcode keine Option, neu hinzugefügte Textelemente im Nachhinein in den dazugehörigen `.strings`-Dateien
zu ergänzen, sondern erneut ist nervige händische Arbeit gefragt.

Fügt man in einem Storyboard oder einer XIB etwa ein neues `UILabel`-Element hinzu und vergibt dort den Text
"Nutzername", so bleiben die zugehörigen Strings-Dateien zum Storyboard oder XIB leer und erhalten nicht automatisch das
neue `UILabel` als einen neuen Key-Eintrag. Die einzige Ausnahme bildet hier der Moment, in dem man ein Storyboard oder
XIB *erstmals* lokalisiert. Beim Lokalisierungsvorgang durchsucht Xcode die Interface-Elemente und tut, was es
eigentlich regelmäßig tun sollte, nämlich die gefundenen Keys in allen Sprachdateien automatisch anzulegen – dies ist in
einer Welt mit verändernden Anforderungen jedoch keine asureichende Lösung. Hier muss dringend eine ergänzend
funktionierende Lösung her.

### Lösungsvorschlag

Das Open Source Tool [BartyCrouch](https://github.com/Flinesoft/BartyCrouch) wurde genau zur Lösung dieser beiden
Probleme geboren. Die Installation findet via Homebrew mit den Befehlen `brew tap flinesoft/bartycrouch` und `brew
install flinesoft/bartycrouch/bartycrouch` statt. Anschließend konfiguriert man BartyCrouch mittels eines Build-Scripts,
welches im einfachen Fall folgendermaßen aussieht:

```shell
if which bartycrouch > /dev/null; then
    # Incrementally update all Storyboards/XIBs strings files
    bartycrouch interfaces -p "$PROJECT_DIR/Sources"

    # Add new keys to Localizable.strings files from NSLocalizedString in code
    bartycrouch code -p "$PROJECT_DIR/Sources" -l "$PROJECT_DIR/Sources" -a
else
    echo "warning: BartyCrouch not installed, download it from https://github.com/Flinesoft/BartyCrouch"
fi
```

Wie man in einem Projekt ein Build Script hinzufügt wird [hier](https://github.com/Flinesoft/BartyCrouch#build-script)
ausführlich erklärt. Ist das einmal geschehen wird BartyCrouch fortan automatisch alle Base-lokalisierten Storyboards
und XIBs im Projektordner durchsuchen und aus den gefundenen Texten Einträge in den Strings-Dateien erstellen. Das
Gleiche wird auch mit Objective-C und Swift-Dateien getan und entsprechend die `Localizable.strings` Dateien
aktualisiert.

Da man beim Entwickeln von Natur aus immer wieder builden muss, braucht man sich also nach dem einmaligen Einrichten des
Skripts pro Projekt um nichts mehr zu kümmern, da nun alle neuen Lokalisierungen bei jedem Build aktuell gehalten
werden. Die Übersetzungen werden hierbei übrigens leer gelassen, diese müssen dann natürlich noch ausgefüllt werden.

#### Ausnahmen

BartyCrouch funktioniert grundsätzlich so, dass es *sämtliche* übersetzbaren Interface-Elemente in Storyboards und XIBs
in die `.strings` Dateien aufnimmt. Manchmal legt man jedoch etwa ein `UILabel`-Element im Interface Builder an mit der
Absicht dessen Wert erst später programmatisch zu setzen – eine Übersetzung wäre dafür also sinnlos. In solchen Fällen
bietet BartyCrouch die Möglichkeit mithilfe der Markierung `#bc-ignore!` das Ignorieren des Interface Elements bei der
automatischen Übersetzung zu erzwingen. Das so markierte Element wird dann nicht in die `.strings` Dateien eingefügt.
Dies kann etwa so aussehen:

![Beispielbild mit teilweise automatisch generierten
String-Keys](https://github.com/Flinesoft/BartyCrouch/raw/stable/Exclusion-Example.png)
*Hier werden die Werte (rechts) für die Bezeichner (links) programmatisch gesetzt und können deshalb von der Übersetzung
ausgeschlossen werden.*


## Dynamisch referenzierte Ressourcen

### Problem

Wie weiter oben bereits erläutert bietet die `Foundation` Library das Makro `NSLocalizedString(key, comment)` an, um
**Texte** in Xcode-Projekten **zu übersetzen**. Der `key` ist dabei vom Typ `String`, was den großen Nachteil hat, dass
alle Automatismen von statischer Typisierung beim Builden von Code nicht mehr greifen. Das bedeutet konkret, dass Xcode
nicht überprüfen kann, ob zu den Werten, die als Keys im Code genutzt werden auch tatsächlich Übersetzungen existieren.
So kann ein Tippfehler, eine Änderung des Keys im Code oder fehlende Übersetzungen zu neuen Keys zu dem Problem führen,
dass am Ende leere Texte in der App angezeigt werden und dies beim Entwickeln und Builden gar nicht auffällt.

Gleiches gilt auch für **Bilder**, die  mit `UIImage(named:)` durch einen String initialisiert werden. Xcode durchsucht
dann automatisch sowohl die `.xcassets`-Ordner des Projekts, als auch alle direkt hinzugefügten Bilder und gleicht deren
Namen mit dem übergebenen `String` ab, um so das korrekte Bild zu finden. **Interfaces** lädt man auf ähnliche Weise
mittels Strings von Storyboards oder XIBs, **Farben** werden meist mit RGB-Werten bei Notwendigkeit ad-hoc angelegt.
Analog zu den dynamisch referenzierten Übersetzungen verhält es sich auch bei per Strings referenzierten Bildern oder
Interfaces. Tippfehler oder  Umbenennungen im Code oder den jeweiligen Ressourcen führen dazu, dass beim Ausführen der
App Bilder fehlen, Interfaces nicht gefunden werden können oder die Farben an vielen unterschiedlichen Stellen gesucht
und geändert werden müssen. Xcode zeigt in Fehlerfällen weder Warnungen noch Fehlermeldungen an, da es die tatsächlich
existierenden Ressourcen schlichtweg nicht kennt, weil sie dynamisch mit den Strings oder den RGB-Werten geladen werden.

Übersetzungen, Bilder, Interfaces und Farben seien im Folgenden unter "Ressourcen" zusammen gefasst.

### Lösungsvorschlag

Ressourcen sollten statisch über automatisch generierten Code gepflegt und geladen werden, sodass man einerseits gar
nicht mehr mit fehlenden Ressourcen builden kann (Xcode zeigt Fehlermeldungen an, wenn referenzierter Code nicht
vorhanden ist) und andererseits eine zentrale Stelle zum Verwalten und Ändern hat. Letzteres ist vor allem bei den
Farben wichtig, die in den meisten Apps einheitlich sein sollten. Das Tool
[SwiftGen](https://github.com/AliSoftware/SwiftGen) leistet hierbei hervorragende Dienste und durchsucht das Projekt
automatisch nach Übersetzungen, Bildern, Interfaces und Farben.

Installieren lässt sich SwiftGen am einfachsten mit dem Befehl `brew install swiftgen`, vorausgesetzt
[Homebrew](http://brew.sh/index_de.html) ist installiert. Die Konfiguration in einem Projekt geschieht analog zu
BartyCrouch mithilfe eines Build Scriptes, in welchem man die verschiedenen Befehle und Pfade, die man nutzen möchte
angibt. Im Folgenden werden die einzelnen Funktionen erläutert, am Ende folgt ein Beispiel-Build-Script.

#### Übersetzungen

Was Übersetzungen betrifft untersucht SwiftGen automatisch alle `.strings`-Dateien auf ihre Keys und generiert aus
diesen eine Code-Struktur namens `Strings.swift` worin sämtliche Keys per Dot-Syntax erreichbar sind und als Ergebnis
die jeweilige Übersetzung liefern. Dies ermöglicht es alle Aufrufe von
`NSLocalizedString("SETTINGS.NAVIGATION_BAR.TITLE", comment)` durch Aufrufe wie `L10n.NavigationBar.Title` zu ersetzen.
Dies löst zum Einen das Problem der statischen Überprüfung, da Xcode bereits automatisch Code auf Existenz prüft, wenn
er kompiliert wird und bietet als Nebeneffekt auch den Vorteil, dass man für sämtliche existierende Keys nun auch die
Autovervollständigung nutzen kann, was bei reinen Strings nicht möglich wäre.

Zusätzlich zur Verwendung von SwiftGen sei an dieser Stelle noch eine Übersetzungs-Key-Struktur dabei empfohlen, die die
Autovervollständigungsfunktion maximal ausreizt, Übersetzungskommentare als Kontext-Informanten überflüssig macht und
das Unterscheiden von Übersetzungs-Keys und tatsächlichen Übersetzungen vereinfacht. Die Struktur sollte folgende Regeln
einhalten:

* Die Keys werden KOMPLETT IN GROSSBUCHSTABEN GESCHRIEBEN
* Die Keys werden hierarchisch.mit.Punkten.getrennt
* Trennungen zwischen Wörtern in den Keys werden mit_Unterstrichen_umgesetzt

Nachfolgend sind ein paar Beispiel-Keys aufgelistet, die diese Vorgaben einhalten:

```swift
SETTINGS.TITLE
SETTINGS.FONT_SECTION.SIZE
SETTINGS.FONT_SECTION.COLOR
MODEL.ARTICLE.TITLE
MODEL.ARTICLE.RELEASE_DATE
```

Mit SwiftGen können sie dann folgendermaßen verwendet werden:

```swift
self.title = L10n.Settings.Title
articleCell.titleLabel.text = L10n.Model.Article.Title
```

#### Farben

Zur Verwendung der Farbenfunktionalität sollte eine `Colors.txt`-Datei im Hauptverzeichnis des Projekts erstellt werden,
deren Inhalt etwa folgendermaßen aussehen kann:

```txt
Primary     : #40657d
Secondary   : #657d40
Accent      : #b7d3e3
```

Die Benutzung der konfigurierten Farben sieht dann etwa so aus:

```swift
self.view.backgroundColor = UIColor(named: .Primary)
self.view.tintColor = UIColor(named: .Accent)
```

#### Bilder & Interfaces

Zur Nutzung der Bilder- und Interfaces-Funktionen ist keine spezielle Konfiguration vonnöten. Die Nutzung ist auch
einfach:

```swift
let image = UIImage(asset: .Banana)
let wizardViewCtrl = StoryboardScene.Wizard.initialViewController()
```

#### Build Script

Das kombinierte Build Script, um alle obigen Funktionen zu nutzen, sieht wie folgt aus (auf Wunsch sind einzelne Zeilen
entfernbar):

```shell
if which swiftgen > /dev/null; then
    swiftgen strings "$PROJECT_DIR/Sources" -t dot-syntax --output "$PROJECT_DIR/Sources/Constants/Strings.swift"
    swiftgen storyboards "$PROJECT_DIR/Sources" --output "$PROJECT_DIR/Sources/Constants/Storyboards.swift"
    swiftgen images "$PROJECT_DIR/Sources" --output "$PROJECT_DIR/Sources/Constants/Images.swift"
    swiftgen colors "$PROJECT_DIR/Colors.txt" --output "$PROJECT_DIR/Sources/Constants/Colors.swift"
else
    echo "warning: SwiftGen not installed, download it from https://github.com/AliSoftware/SwiftGen"
fi
```
*Hinweis: Man beachte, dass beim Einfügen des Scripts in das Build-Script-Feld in Xcode die Formatierung des Textes
verloren geht. Diese lässt sich mit ein paar Einrückungen jedoch schnell wieder herstellen, um den Code im Script
lesbarer zu halten.*

Die Ergebnis-Dateien werden im Ordner `Sources/Constants` abgelegt, welcher vorher erstellt werden sollte. Fortan werden
die Dateien bei jedem Build neu erzeugt, sofern es seit der letzten Generierung Änderungen an den jeweiligen Ressourcen
gab.


## Fehlende Code Conventions und TODO-Warnungen

Swift erlaubt es wie die meisten Programmiersprachen das gleiche Funktionsverhalten einer App auf viele verschiedene
Weisen zu implementieren. Während man im Allgemeinen nicht sagen kann, dass es "die perfekte" Implementierung einer
Funktion gibt, kristallisieren sich dennoch mit der Zeit für jede Programmiersprache und Plattform eine ganze Menge von
zu vermeidenden und eher zu bevorzugenden Implementierungen heraus. Um möglichst hohe Code-Qualität zu erreichen lohnt
es sich daher für jede längerfristig angelegte Arbeit, besonders wenn mehrere Entwickler beteiligt sind
Code-Konventionen zu erarbeiten und sich an diesen zu orientieren.

Während das offizielle Buch der Swift-Entwickler [The Swift Programming
Language](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/index.html)
beim Erläutern der einzelnen verfügbaren Funktionen bereits viele sinnvolle Beispiele für guten Programmierstil vorgibt,
ist es diesbezüglich doch eher zu zeitaufwändig und unvollständig (wenn auch trotzdem lesenswert). Als offizielle
Apple-Quelle seien ergänzend die [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
empfohlen. Als bessere Alternative gibt es die zusammengefassten und mit Beispielen versehenen Swift Code Conventions
von [GitHub](https://github.com/github/swift-style-guide) und von
[raywenderlich.com](https://github.com/raywenderlich/swift-style-guide), an die auch wir uns bei Jamit Labs halten.

Das Open Source Projekt [SwiftLint](https://github.com/realm/SwiftLint) hat es sich zur Aufgabe gemacht die GitHub
Conventions automatisiert in Xcode zu überprüfen. Nach einer Installation des SwiftLint-Tools via Homebrew (`brew
install
swiftlint`) kann nun jedes Projekt durch drei einfache Schritte um eine automatisierten Warnings- und Error-Generator
bei Nichteinhaltung von bestimmten Code-Konventionen erweitert werden:

* Füge ein Build-Script mit diesem Inhalt für das App- oder Framework-Target hinzu:

```shell
if which swiftlint > /dev/null; then
    swiftlint
else
    echo "warning: SwiftLint not installed, download it from https://github.com/realm/SwiftLint"
fi
```
*Hinweis: Man beachte, dass beim Einfügen des Scripts in das Build-Script-Feld in Xcode die Formatierung des Textes
verloren geht. Diese lässt sich mit ein paar Einrückungen jedoch schnell wieder herstellen, um den Code im Script
lesbarer zu halten.*


* Erstelle danach im Hauptverzeichnis des Projekts eine Datei namens `.swiftlint.yml` mit folgender Konfiguration (bei
Bedarf änderbar):

```yaml
# some rules are only opt-in
opt_in_rules:
- empty_count
- missing_docs

# paths to include during linting. `--path` is ignored if present.
included:
- Sources

# paths to ignore during linting. Takes precedence over `included`.
excluded:
- Carthage
- Sources/Constants

# configurable rules can be customized from this configuration file
line_length: 180
```

Als drittes sollte nun noch eine Einstellung von Xcode angepasst werden, da die Regel `trailing_newline` sonst beim
Standardverhalten von Xcode Warnungen wegen falsch gesetzter Leerzeichen meldet, die man ganz einfach vermeiden kann:
Gehe in die Einstellungen von Xcode ("Preferences"), wähle dort den Reiter "Text Editing" und stelle sicher, dass neben
"Automatically trim trailing whitespace" auch "Including whitespace-only lines" angehakt ist. Dies stellt sicher, dass
bei Umbrüchen und leer gelassenen Zeilen entstehende Leerzeichen automatisch gelöscht werden. Hier kann übrigens auch
gleich die Empfehlungslinie für die Zeilenbreite in Xcode angepasst werden unter "Page guide at column" - wir empfehlen
180 als Kompromiss zwischen ständigen Zeilenumbrüchen und Lesbarkeit von Code.

Diese drei Schritte sorgen dafür, dass bei jedem Entwickler, der den Swift Linter installiert hat automatisch
Warnungen oder in bestimmten Fällen sogar Errors in Xcode generiert und angezeigt werden, wenn die konfigurierten
Konventionen nicht eingehalten werden. Falls jemand das Tool SwiftLint nicht installiert hat wird wenigstens eine
Warnung angezeigt, dass der Swift Linter nicht installiert ist. Als netten Nebeneffekt zeigt SwiftLint zusätzlich auch
Code-Kommentare, die ein `TODO:` oder `FIXME` enthalten als Warnungen an, damit man diese nicht so schnell vergessen
oder übersehen kann.


## Weiterführende Links

- [BartyCrouch Dokumentation](https://github.com/Flinesoft/BartyCrouch)
- [SwiftGen Dokumentation](https://github.com/AliSoftware/SwiftGen)
- [SwiftLint Dokumentation](https://github.com/realm/SwiftLint)
