---
section:  Apple
topic:    Working with Xcode
refid:    "AP01#02"
title:    Durchdachte Dateistruktur
date:     2016-01-01 00:00:00
author:   Cihat Gündüz
---


## Wie finde ich meine Dateien schnell in Xcode?

Während Xcode über seine mehr als 13 Jahre Lebenszeit immer mehr nützliche Funktionen erhalten hat, die das Arbeiten damit immer weiter vereinfachten (wie Tabs, Assistant-Editor, Quick-Open mit `Cmd+Umschalt+o`) so bleibt es in der Praxis dabei, dass der Project-Navigator meist der Dreh- und Angelpunkt der Arbeit in Xcode darstellt. Dort können nicht nur neue Dateien gefunden, geöffnet, erstellt und umbenannt werden, sondern auch das Sortieren von verschiedenen zusammengehörigen Dateien verschiedenster Art in sogenannten Groups ist möglich. Diese Gruppen sind von der Dateistruktur im Finder (das Mac-Pendant zum Windows Explorer) komplett unabhängig, sodass es möglich ist (wie in vielen älteren Projekten auch üblich) sämtliche Dateien innerhalb des Hauptprojektverzeichnisses zu belassen und die Strukturierung und Gruppierung der Dateien komplett in Xcode vorzunehmen. Eine neue Gruppe erstellt man mit einem Rechtsklick im Project-Navigator und dem Eintrag "New Group".

Das hilfreichste dabei, die richtige Datei schnell in einem Projekt wiederzufinden, ist es, den Aufbau der Dateigruppen im jeweiligen Projekt zu kennen. Diese Struktur ist im Allgemeinen bei jedem Entwickler unterschiedlich, ein Problem, das nicht nur bei der iOS-Entwicklung auftritt. Andere Plattformen wie z.B. Ruby on Rails (kurz Rails) lösen dieses Problem mit gemeinsamen Konventionen, die klar festlegen, wo welche Datei hingehört – das dahinterstehende Prinzip wird ["Convention over Configuration"](https://en.wikipedia.org/wiki/Convention_over_configuration) – kurz "CoC" genannt. Beim Erstellen eines Rails-Projektes wird dabei etwa automatisch eine ganze Projektverzeichnisstruktur geschaffen, in die man seine Dateien nur noch korrekt einfügen muss. Und selbst dafür gibt es bei Rails hilfreiche Kommandos.

Während wir bei iOS/Android *noch* nicht soweit sind ein Projekt automatisch generieren und die Dateien an die richtigen Stellen einfügen zu lassen, so können wir aber durchaus schon eine Projektstruktur mit semantischer Bedeutung vorgeben, die einerseits das Auffinden aber andererseits auch das Entscheiden vereinfacht, wo sich eine Datei befinden soll. Im besten Falle sollte eine solche Projektstruktur zugleich eine sinnvolle Trennung von Programmlogik in Bestandteile mit klaren Verantwortlichkeiten sowie andere Prinzipien gut entwickelter Software begünstigen (siehe etwa [hier](http://www.oodesign.com/design-principles.html), [hier](https://msdn.microsoft.com/en-us/library/ee658124.aspx) oder [hier](http://code.tutsplus.com/tutorials/3-key-software-principles-you-must-understand--net-25161) – bzw. in Wikipedia-Artikeln gesprochen [hier](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design), [hier](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design) oder [hier](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)).


## Wie sollten meine Dateien innerhalb/außerhalb von Xcode strukturiert sein?

Wegen der Unabhängigkeit der Groups-Funktion (Struktur in Xcode) und der tatsächlichen Datei-Struktur (im Finder) gilt es dabei grundsätzlich zu beachten, dass eine Konvention Vorgaben für beide Strukturen machen sollte und diese sich im besten Falle gut ergänzen. Da im Regelfall mit Dateien innerhalb von Xcode hantiert wird, sollte sich die tatsächliche Dateistruktur an der Gruppen-Struktur orientieren und diese möglichst genau nachstellen. Es ist daher zunächst naheliegend beide Strukturen genau gleich aufzubauen, dies stellt sich jedoch in der Praxis eher als Handicap heraus: Einerseits muss dadurch jede Änderung doppelt vorgenommen werden (einmal im Xcode Project-Navigator und zusätzlich im Finder), außerdem muss bei jeder Änderung im Finder der Path zur geänderten Datei in Xcode manuell neu vorgegeben werden, wodurch der Aufwand nochmal zusätzlich steigt (aus doppeltem Aufwand wird also sogar dreifacher Aufwand).

Ziel unserer Konvention sollte es von daher sein, dass wir die Xcode-Projektstruktur möglichst nachstellen, jedoch auch maximal flexibel dabei bleiben wollen, um die Dateien im Project-Navigator ohne Zusatzaufwand neu gruppieren zu können. Unser Vorschlag dazu ist, dass im Finder lediglich eine Trennung nach Art der Datei stattfindet, wobei meist mindestens folgende Arten vorhanden sind:

* **Code**: Alle Dateien, die Code mit App-Logik enthalten. Typische Endungen: `.swift`, `.h`, `.m`.
* **User Interface**: Alle Interface-Builder Dateien, die das Design der App vorgeben. Typische Endungen: `.storyboard`, `.xib`.
* **Assets**: Alle Medien-Inhalte der App. Typische Endungen: `.xcassets`, `.png`, `.jpg`.
* **Constants**: Alle durch Werkzeuge automatisch generierten Klassen, die Konstanten anbieten. Mehr dazu in [diesem](#) Artikel.
* **Supporting Files**: Alle Dateien, die in gewisser Weise unterstützend/konfigurierend das Projekt ergänzen, jedoch nicht eigenständig als Bestandteil zusammenfassbar sind. Typischerweise `Info.plist`, `...-Bridging-Header.h`, `...PrefixHeader.pch`, `Localizable.strings`.

Je nach Projekt können mehr Arten vorhanden sein oder sogar eine Unterteilung innerhalb von Ordnern stattfinden. Nachfolgend seien bereits benutzte Beispiele genannt:

* **Data Models**: Alle Dateien, die Datenmodelle definieren (z.B. via Core Data). Typische Endungen: `.xcdatamodeld`.
* **User Interface/iOS** und **User Interface/tvOS**: Bei Frameworks, die sowohl iOS als auch tvOS unterstützen müssen alle Interface Builder Dateien jeweils für die unterschiedlichen Plattformen getrennt angelegt werden.
* **Assets**, **Assets/Fonts**, **Assets/PNGs**, **Assets/SVGs**: Bei Vorhandensein etwa von vielen Fonts, PNGs und SVGs lohnt es sich diese zusammenzufassen und den Rest weiterhin direkt im `Assets`-Ordner zu belassen.

Aus Gründen der Einheitlichkeit sollte dieselbe Struktur gleichermaßen in App-Projekten wie in Frameworks verwendet werden (mehr zur Erstellung von Dynamic Frameworks [hier](#)). Frameworks können auf unterschiedliche Weise in Projekte eingebunden werden (mehr zu Dependency Management [hier](#)), die Zukunft des Framework-Einbindens ist aber mit aller Wahrscheinlichkeit der offizielle Weg über den sogenannten [Swift Package Manager](https://swift.org/package-manager/) (SPM), der sich derzeit [noch in Entwicklung befindet](https://github.com/apple/swift-package-manager), jedoch schon mit Swift 3 in erster Version fertig werden soll. Damit der SPM alle Dateien die als Teil des Frameworks gebuildet werden sollen leicht findet, hat der SPM selbst eine Minimalvorgabe (siehe [hier](https://github.com/apple/swift-package-manager/blob/master/Documentation/SourceLayouts.md)) wonach alle Dateien, die Teil des Frameworks sein sollen in einem Unterordner namens `Sources` platziert sein sollten. Für die Tests, die die korrekte Funktionsweise eines Frameworks langfristig sicherstellen wird der Unterordner `Tests` empfohlen (mehr zum Testen von Code [hier](#)).


## Empfohlene Dateistruktur

Verbindet man die Anforderungen des Swift Package Managers und unsere Dateitypen-Strukturierung, so ergibt sich, dass für Dateien, die Teil des Hauptprojekts bzw. des Frameworks sind, innerhalb des Ordners `Sources` in die genannten Typ-Ordner (z.B. `Code`) einsortiert werden sollten. Das gleiche wird auch mit Dateien getan, die Teil der Tests sind, nur innerhalb des Ordners `Tests`. Hieraus folgt sowohl für App-Projekte, als auch für Frameworks eine Struktur, die etwa so aussieht:

![example-project-file-structure](public/images/xcode-file-structure/example-project-file-structure.png)

Für UI-Tests kann zusätzlich ein Ordner Namens `UI Tests` eingeführt werden, worin wiederum die empfohlene Struktur angewandt wird. Inwiefern dies sich mit dem Swift Package Manager verträgt muss jedoch noch geklärt werden.

## Empfohlene Gruppen-Struktur & Einrichtungs-Tipps

TODO: Bei diesem Paragraphen empfehle ich, dass das Beispiel-Projekt gleich mit erstellt wird und die einzelnen Schritte noch im Detail erklärt und mit Screenshots versehen werden. Auch die bereits im Text genannten Beispiele (etwa "Sources/Code/Home") sollten mit den jeweils im Beispiel-Projekt verwendeten Screen-Namen ersetzt werden (sofern sie anders heißen).

TODO: Ergänzen wofür "Relative to Group", "Relative to Project" steht.

Passend zur Dateistruktur sollten die Gruppen im Projekt eingerichtet werden. Dabei sollte im ersten Schritt jeder Ordner im Projekt-Verzeichnis mit einer Gruppe vertreten sein und genau die Dateien enthalten, die auch im jeweiligen Verzeichnis enthalten sind. Beim Hinzufügen von neuen Dateien kann dies künftig mit einem Rechtsklick auf die jeweilige Gruppe und "New File" geschehen und Xcode wird automatisch den korrekten Ordner im Dateiverzeichnis auswählen, wodurch man nach dem ersten Einrichten nie mehr direkt am Dateien-Verzeichnis Dateien hin und her schieben muss.

TODO: Bild erstellen (erst wenn vorheriges TODO geklärt)
![correct-path](public/images/xcode-file-structure/correct-path.png)
*Im File Inspector findet man den Path einer Datei*

Hierzu müssen aber die Gruppen, für die es jeweils einen Ordner gibt (z.B. `Sources` und darin `Code`) den korrekten Path gesetzt haben, was man im Nachhinein mit dem Ordner-Symbol im File-Inspector rechts in Xcode unter "Location" ändern kann. Am einfachsten ist es jedoch, einmal sämtliche Gruppen in Xcode zu entfernen (wichtig: nur "Remove References" auswählen) und die Ordner Sources, Tests und evtl. UI Tests einmal vom Finder links in den Project-Navigator per Drag-and-Drop direkt unterhalb des Projekts zu ziehen und im anschließenden Dialog sicherzustellen, dass "Create Groups" angehakt ist. So ist die ganze Datei-Struktur schon einmal korrekt im Project-Navigator. Es lohnt sich, diesen Schritt getrennt für die Ordner "Sources", "Tests" und "UI Tests" zu machen und dabei beim Import das jeweils korrekte Target anzuhaken.

![correct-path](public/images/xcode-file-structure/add-files.png)
*Beim Hinzufügen von Dateien sollten diese Haken gesetzt sein*

TODO: Folgende Beispielpfade anpassen

Ist die Grundstruktur erst einmal im Project-Navigator vorhanden, so kann man fortan innerhalb einer Gruppe (z.B. `Code`) beliebig weiter Untergruppen erstellen. Hierbei empfiehlt es sich jeweils nach zugehörigen Screens (sofern vorhanden) oder generell Funktionen zu gruppieren. So kann ein Projekt etwa die Untergruppen "Sources/Code/ScreenHome", "Sources/Code/Settings", "Sources/Code/Networking", "Sources/Code/Core Data", "Sources/Code/Extensions", "Sources/Code/Helpers", "Sources/User Interface/Home", "Sources/User Interface/Settings", "Tests/Code/Home", "Tests/Code/Settings", "Tests/Code/Core Data", "Tests/Code/Extensions" und "Tests/Code/Helpers" enthalten. Zugehörige Untergruppen in verschiedenen Typen und Targets sollten dabei gleich genannt werden, wie in den voran genannten Beispielen jeweils mit "Home" und "Settings" geschehen. Nachfolgendes Bild soll dies veranschaulichen:

![correct-path](public/images/xcode-file-structure/xcode-project-navigator.png)
*Ein fertig eingerichteter Project Navigator*
