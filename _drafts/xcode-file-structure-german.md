---
layout: post
title:  "Dateistrukturen in Xcode"
categories: ios
---

## Wie finde ich meine Dateien in schnell in Xcode?

Während Xcode über seine mehr als 13 Jahre Lebenszeit immer mehr nützliche Funktionen erhalten hat, die das Arbeiten damit immer weiter vereinfachten (wie Tabs, Assistant-Editor, Quick-Open mit `Cmd+Umschalt+o`) so bleibt es in der Praxis dabei, dass der Project-Navigator meist der Dreh- und Angelpunkt der Arbeit in Xcode darstellt. Dort können nicht nur neue Dateien gefunden, geöffnet, erstellt und umbenannt werden, sondern auch das Sortieren von verschiedenen zusammengehörigen Dateien verschiedenster Art in sogenannten Groups ist möglich. Diese Gruppen sind von der Dateistruktur im Finder (das Mac-Pendant zum Windows Explorer) komplett unabhängig, sodass es möglich ist (wie in vielen älteren Projekten auch üblich) sämtliche Dateien innerhalb des Hauptprojektverzeichnisses zu belassen und die Strukturierung und Gruppierung der Dateien komplett in Xcode vorzunehmen. Eine neue Gruppe erstellt man mit einem Rechtsklick im Project-Navigator und dem Eintrag "New Group".

Das hilfreichste dabei, die richtige Datei schnell in einem Projekt wieder zu finden – sowohl bei häufigem Wechsel zwischen Projekten, die mitunter von anderen angefangen oder geändert wurden, als auch bei älteren eigenen Projekten – ist es den Aufbau der Dateigruppen zu kennen. Diese Struktur ist im Allgemeinen bei jedem Entwickler unterschiedlich, ein Problem, das nicht nur bei der iOS-Entwicklung auftritt. Andere Plattformen wie z.B. Ruby on Rails (kurz Rails) lösen dieses Problem mit gemeinsamen Konventionen, die klar festlegen, wo welche Datei hin gehört – das dahinter stehende Prinzip wird ["Convention over Configuration"](https://en.wikipedia.org/wiki/Convention_over_configuration) – kurz "CoC" genannt. Beim Erstellen eines Rails-Projektes wird dabei etwa automatisch eine ganze Projektverzeichnisstruktur geschaffen, in die man seine Dateien nur noch korrekt einfügen muss. Und selbst dafür gibt es bei Rails hilfreiche Kommandos.

Während wir bei iOS/Android *noch* nicht soweit sind ein Projekt automatisch generieren und die Dateien an die richtigen Stellen einfügen zu lassen, so können wir aber durchaus schon eine Projektstruktur mit semantischer Bedeutung vorgeben, die einerseits das Auffinden aber andererseits auch das Entscheiden vereinfacht, wo sich eine Datei befinden soll. Im besten Falle sollte eine solche Projektstruktur zugleich eine sinnvolle Trennung von Programmlogik in Bestandteile mit klaren Verantwortlichkeiten sowie andere Prinzipien gut entwickelter Software begünstigen (siehe etwa [hier](http://www.oodesign.com/design-principles.html), [hier](https://msdn.microsoft.com/en-us/library/ee658124.aspx) oder [hier](http://code.tutsplus.com/tutorials/3-key-software-principles-you-must-understand--net-25161)) – bzw. in Wikipedia-Artikeln gesprochen [hier](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design), [hier](https://en.wikipedia.org/wiki/GRASP_(object-oriented_design)) oder [hier](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)).


## Wie sollten meine Dateien innerhalb/außerhalb von Xcode strukturiert sein?

Wegen der Unabhängigkeit der Groups-Funktion (Struktur in Xcode) und der tatsächlichen Datei-Struktur (im Finder) gilt es dabei grundsätzlich zu beachten, dass eine Konvention Vorgaben für beide Strukturen machen sollte und diese sich im besten Falle gut ergänzen. Da im Regelfall mit Dateien innerhalb von Xcode hantiert wird, sollte sich die tatsächliche Dateistruktur an der Gruppen-Struktur orientieren und diese möglichst genau nachstellen. Es ist daher zunächst nahelegend beide Strukturen einfach gleich aufzubauen, dies stellt sich jedoch in der Praxis eher als Handicap heraus: Einerseits muss dadurch jede Änderung doppelt vorgenommen werden (einmal im Xcode Project-Navigator und zusätzlich im Finder), außerdem muss bei jeder Änderung im Finder der Path zur geänderten Datein in Xcode manuell neu vorgegeben werden, wodurch der Aufwand nochmal zusätzlich steigt (aus doppeltem Aufwand wird also sogar dreifacher Aufwand).

Ziel unserer Konvention sollte es von daher sein, dass wir die Xcode-Projektstruktur möglichst nachstellen, jedoch auch maximal flexibel dabei bleiben wollen, um die Dateien im Project-Navigator ohne Zusatzaufwand neu gruppieren zu können. Unser Vorschlag dazu ist, dass im Finder lediglich eine Trennung nach Art der Datei stattfindet, wobei die üblichen Arten folgende sind:

* **Code**: Alle Dateien, die Code mit App-Logik enthalten. Typische Endungen: `.swift`, `.h`, `.m`.
* **User Interface**: Alle Interface-Builder Dateien, die das Design der App vorgeben. Typische Endungen: `.storyboard`, `.xib`.
* **Assets**: Alle Medien-Inhalte der App. Typische Endungen: `.xcassets`, `.png`, `.jpg`.



## Empfohlene Dateistruktur



## Wie kann ich diese Struktur auf bestehende Projekte anwenden?
