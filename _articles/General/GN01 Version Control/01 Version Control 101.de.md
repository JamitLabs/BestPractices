---
section:  General
topic:    Version Control
refid:    GN01-01
title:    "Versionsverwaltung: Eine Einführung"
date:     2016-01-01 00:00:00
author:   Cihat Gündüz
---

## Was ist Versionsverwaltung?

Versionsverwaltungssysteme ermöglichen das Festhalten von Änderungen an textbasierten Dateien inklusive Kommentar und erlauben somit das gezielte Rückkehren zu jedem vorherigen Zustand bei der schrittweisen Entwicklung eines Projekts. Bei der Softwareentwicklung gehören sie zu den wichtigsten Werkzeugen, da sie die Fehlerauffindung und das gemeinsame Arbeiten stark vereinfachen.


## Warum brauche ich eine Versionsverwaltung?

Angenommen ein Projekt wird ohne Versionsverwaltung umgesetzt und es gibt einen Fehler. Zur Behebung des Fehlers machst du Änderungen am Code des Projekts und wie so oft musst du verschiedene Ansätze ausprobieren, damit du den richtigen Fix findest. Du stellst dabei irgendwann fest, dass irgendetwas anderes nicht mehr funktioniert, weil du vor dem Beginn eines neuen Ansatzes den vorherigen Ansatz nicht komplett rückgängig gemacht hast und irgendetwas vergessen hast, was jetzt Auswirkungen auf deinen neuen Ansatz hat. Und das Ganze geht immer weiter, je mehr Änderungen du machst, desto schlimmer wird es, bis das Projekt irgendwann gar nicht mehr funktioniert.

Mithilfe eines Versionskontrollsystems kannst du nach jedem Ansatz praktisch mit einem Klick zum vorigen Stand zurückkehren. Außerdem solltest du jeden erfolgreichen Ansatz mit einem Kommentar versehen abspeichern (ein sogenannter **Commit**), wodurch du den neuen Stand zum Zurückkehren markierst – die noch weiter zurückliegenden Stände sind auch alle weiterhin verfügbar (in der sogenannten **History**).

Ganz vom obigen Beispiel abgesehen erschließt es sich außerdem leicht, warum kommentierte gespeicherte Zustände, zu denen man jederzeit zurückkehren kann und deren Änderungen man auch sehen kann (**Commit Changes**) das gemeinsame Arbeiten erleichtern. Versionskontrollsysteme haben außerdem stets eine Funktion, mithilfe der sie mehrere Commits zu einem gemeinsamen Ergebnis zusammenführen können (ein sogenannter **Merge**). Jeder kann also getrennt arbeiten, seine Arbeit commiten und am Ende kann alles ohne großen Aufwand zusammengeführt werden (sofern nicht an derselben Stelle gearbeitet wurde). Das spart viel Zeit und Nerven.


## Welches Versionsverwaltungssystem sollte ich nutzen?

![Logos](/public/images/GN01/01/logos.png)
*Meist begegnet man SVN, Git oder Mercurial.*

Es gibt verschiedene Versionsverwaltungssysteme [mit unterschiedlichen Zielen sowie Vor- und Nachteilen](http://stackoverflow.com/a/875). Die bei der Softwareentwicklung heutzutage am meisten verbreiteten sind Subversion (kurz SVN), Git und Mercurial. Allen 3 gemein ist, dass die Daten auf einem Server gehostet werden um so mehreren Nutzern Zugriff auf die Daten zu bieten. SVN gilt dabei als veraltet, da es sich noch um ein zentral verwaltetes Versionskontrollsystem handelt und man Änderungen am Projekt direkt auf den Server merged. Git und Mercurial sind dezentrale Versionskontrollsysteme, die das getrennte Arbeiten und Verwalten von Projekten durch den Einsatz lokaler Arbeitskopien deutlich verbessern. So hat man auch offline Zugriff auf alle Daten und ist nicht durchwegs auf eine Internetverbindung angewiesen, um am Projekt zu arbeiten. Da die lokale Kopie nicht automatisch auf den Stand des serverseitigen Originals synchronisiert wird muss man dies manuell erledigen (dazu später mehr).

Funktional sind Git und Mercurial miteinander für die üblichen Projektzwecke [durchaus vergleichbar](http://stackoverflow.com/a/892688), jedoch stand hinter Git (vor allem aus [historischen Gründen](https://de.wikipedia.org/wiki/Git)) stets eine deutlich größere Community, sodass Git mit der Zeit immer mehr zum dominierenden Versionskontrollsystem aufgestiegen ist. Die starke Verbreitung, die vielen verfügbaren Werkzeuge und die Unterstützung auf vielen Plattformen zur Projektverwaltung machen die Entscheidung sehr leicht: Git ist das Versionsverwaltungssystem der Wahl.


## Welche Funktionen beherrscht Git?

Mit Git lassen sich Code-Projekte in verschiedenen Ausarbeitungen mit Kommentaren versehen speichern (**Commits**), außerdem unterstützt Git das Verwalten verschiedener gleichzeitig entwickelter Arbeitslinien (**Branches**). Git kann verschiedene Branches oder die Änderungen von verschiedenen Commits automatisch zusammenführen (**Merge**) sofern nicht an den selben Code-Stellen in denselben Dateien unterschiedliche Änderungen vorliegen (**Merge Conflict**). Falls diese doch vorliegen greift man einfach von Hand ein und ändert den Code entsprechend wie gewünscht (die Stellen mit einem Konflikt sind dann mit `>>>>>>` und `<<<<<<` markiert).

![Merge](/public/images/GN01/01/merge.png)
*Ein Merge führt Änderungen in einem Master Branch zusammen oder bringt einen Working Branch auf den Stand des Master Branches.*

Da Git ein dezentrales Versionsverwaltungssystem ist, sollte man seinen lokalen Stand regelmäßig mit dem serverseitigen Stand synchronisieren. Durch regelmäßiges Abgleichen ermöglicht man anderen Teammitgliedern den Zugriff auf die eigenen Änderungen am Projekt und erhält gleichzeitig Zugriff auf die Änderungen anderer Teammitglieder. Ein weiterer Vorteil ist das Vorbeugen von eventuellem Datenverlust, da eigene Änderungen dann nicht mehr nur auf dem eigenen System sondern auch auf dem Server vorhanden sind. Außerdem sind so Merce Conflicts unwahrscheinlicher.
Dazu besitzt Git die beiden Funktionen **Push** und **Pull**. Push erlaubt dem Nutzer Commits von lokalen Branches auf den Server zu "schieben" (**pushen**). Das Gegenstück dazu, ein Pull, ermöglicht es, neue Commits von den serverseitigen Branches in seine lokalen Branches zu "ziehen" (**pullen**).

![Push & Pull](public/images/GN01/01/push-pull.png)
*Um lokale und serverseitige Branches synchron zu halten, sollte man regelmäßig pushen und pullen.*

Falls man das erste mal mit einem dezentralen Versionsverwaltungssystem in Berührung kommt kann es leicht zu Verwechslungen zwischen den Funktionen Merge sowie Push und Pull kommen da diese eine ähnliche Aufgabe besitzen. Man sollte sich deshalb klar machen, dass Push oder Pull immer benutzt werden wenn man Branches zwischen Server und lokalem System synchronisieren möchte. Ein Merge hingegen synchronisiert Branches auf dem gleichen System.

## Wie kann ich Git einfach nutzen?

Git ist ein Werkzeug für die Kommandozeile und ist auf Mac-Systemen standardmäßig vorinstalliert. Eine aktuelle Version kann auf der [offiziellen Download-Seite](https://git-scm.com/downloads) heruntergeladen werden. Wir empfehlen jedoch gerade Git-Anfängern (aber auch Erfahrenen) die Nutzung eines guten Git-Programms mit Nutzeroberfläche. [SourceTree](https://www.sourcetreeapp.com) ist dabei sowohl für Mac als auch für Windows verfügbar und das Programm unserer Wahl. Es ist kostenlos (Registrierung nach 30 Tagen notwendig), recht übersichtlich und unterstützt neben einer integrierten aktuellen Git-Version auch weitergehende Funktionen wie Git-Flow (dazu mehr in [diesem Artikel](#)).

![SourceTree]()
*SourceTree erlaubt die Nutzung von Git ohne Kommandozeile*

## Wann sollte ich Änderungen commiten?

Diese Entscheidung hängt davon ab, mit welchem Ziel Commits verwendet werden. Da Commits mehrere Vorteile haben – Verhinderung von Datenverlust, Dokumentierung von Änderungen, Teilen von Code mit anderen – hängt diese Entscheidung vom Zweck ab. Ein paar Faustregeln können hier aber gegeben werden:

* Grundsätzlich sollte die Arbeit an einem Projekt (sofern denn Änderungen zustande kamen) spätestens nach 2h einmal commitet werden, sofern man die getane Arbeit nicht verlieren möchte (auch wenn sie noch nicht komplettiert ist). Dies dient dem Ziel die Folgen von Datenverlust zu minimieren und bei Teams den Fortschritt zu teilen.

* Jede zusammengehörige Veränderung (auch wenn mehrere Dateien beteiligt sind), die eine Sache fertig stellt, sollte commitet werden. Bei typischen Programmieraufgaben werden bei eingespielten Programmierern zwischen Commits etwa 2-15 Minuten Zeit vergehen, je nach Größe der gerade überlegten Änderung. Hierbei drückt ein Commit eine erdachte Änderung aus, wobei der Gedanke dann als Kommentar festgehalten wird (mehr zur korrekten Dokumentierung von Commits gibt es [hier](#)).

## Weiterführende Links
[Offizielle Git Dokumentation](https://git-scm.com/doc)
