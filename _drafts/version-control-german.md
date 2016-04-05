---
layout: post
title:  "Versionsverwaltung"
categories: ios android
---

## Was ist Versionsverwaltung?

Versionsverwaltungssysteme ermöglichen das Festhalten von Änderungen an textbasierten Dateien inklusive Kommentar und erlauben somit das gezielte Rückkehren zu jedem vorherigen Zustand bei der schrittweisen Entwicklung eines Projekts. Bei der Softwareentwicklung gehören sie zu den wichtigsten Werkzeugen, da sie die Fehlerauffindung und das gemeinsame Arbeiten stark vereinfachen.


## Warum brauche ich eine Versionsverwaltung?

Angenommen ein Projekt wird ohne Versionsverwaltung umgesetzt und es gibt einen Fehler. Zur Behebung des Fehlers machst du Änderungen am Code des Projekts und wie so oft musst du verschiedene Ansätze ausprobieren, damit du den richtigen Fix findest. Du stellst dabei irgendwann fest, dass irgendetwas anderes nicht mehr funktioniert, weil du vor dem Beginn eines neuen Ansatzes den vorherigen Ansatz nicht komplett rückgängig gemacht hast und irgendetwas vergessen hast, was jetzt Auswirkungen auf deinen neuen Ansatz hat. Und das ganze geht immer weiter, je mehr Änderungen du machst, desto schlimmer wird es, bis das Projekt irgendwann gar nicht mehr funktioniert.

Mithilfe eines Versionskontrollsystems kannst du nach jedem Ansatz paktisch mit einem Klick zum vorhigen Stand zurückkehren. Außerdem kannst du jeden erfolgreichen Ansatz mit einem Kommentar versehen abspeichern (ein sogenannter "commit"), wodurch du den neuen Stand zum Zurückkehren markierst – die noch weiter vorherigen funktionierenden Stände sind auch alle weiterhin verfügbar (in der sogenannten "history").

Ganz vom obigen Beispiel abgesehen erschließt es sich außerdem leicht, warum kommentierte gespeicherte Zustände, zu denen man jederzeit zurückkehren kann und deren Änderungen man auch sehen kann ("commit changes") das gemeinsame Arbeiten erleichtern. Versionkontrollsysteme haben außerdem stets eine Funktion, mithilfe der sie mehrere commits zu einem gemeinsamen Ergebnis zusammenführen können (ein sogenannter "merge"). Jeder kann also getrennt arbeiten, seine Arbeit "commiten" und am Ende kann alles ohne grosen Aufwand zusammengeführt werden (sofern nicht an derselben Stelle gearbeitet wurde). Das spart viel Zeit und Nerven.


## Welches Versionsverwaltungssystem sollte ich nutzen?

Es gibt verschiedene Versionsverwaltungssysteme mit unterschiedlichen Zielen sowie Vor- und Nachteilen. Die bei der Softwareentwicklung heutzutage am meisten verbreiteten sind SVN, git und Mercurial. SVN gilt dabei als veraltet, da es sich noch um ein zentral verwaltetes Versionskontrollsystem handelt. git und Mercurial sind dezentrale Versionkontrollsysteme, die das getrennte Arbeiten und Verwalten von Projekten deutlich verbessern. Funktional sind git und Mercurial miteinander für die üblichen Projektzwecke durchaus vergleichbar, jedoch stand hinter git (vor allem aus historischen Gründen) stets eine deutlich größere Community, sodass git mit der Zeit immer mehr zum dominierenden Versionskontrollsystem aufgestiegen ist. Die starke Verbreitung, die vielen verfügbaren Werkzeuge und die Unterstützung auf vielen Plattformen zur Projektverwaltung machen die Entscheidung sehr leicht: git ist das Versionsverwaltungssystem der Wahl.


## Welche Funktionen beherrscht git?

Mit git lassen sich Code-Projekte in verschiedenen Ausarbeitungen mit Kommentaren versehen speichern ("commits"), außerdem unterstützt git das Verwalten verschiedener gleichzeitig entwickelter Arbeitslinien ("branches"). git kann verschiedene branches oder die Änderungen von verschiedenen commits automatisch zusammenführen ("merge") sofern nicht an den selben Code-Stellen in denselben Dateien unterschiedliche Änderungen vorliegen ("merge conflict"). Falls diese doch vorliegen greift man einfach von Hand ein und ändert den Code entsprechend wie gewünscht (die Stellen mit einem Konflikt sind dann mit ">>>>>>" und "<<<<<<<" markiert).

Eine sehr nützliche Funktion von git in Situationen, bei denen man wissen möchte, wann, von wem oder warum ein bestimmtes Stück Code geschrieben wurde, ist "git blame". Da die meisten Dienste zum Hosten von git-Projekten (wie GitHub, GitLab) die blame-Funktion bereits integriert haben ist der einfachste Weg zur Nutzung von git blame die entsprechende Datei in der Web-Oberfläche zu öffnen und auf den Button "blame" zu drücken. Dann sieht man in einer tabellarischen Darstellung rechts den Code und links dazu den jeweils letzten commit, in der der Code geändert wurde inklusive dem Kommentar und dem Namen desjednigen, der die Änderung durchgeführt hat. Diese Informationen können helfen Entscheidungen zum Löschen oder Verändern von bestimmten Code-Zeilen zu treffen.


## Wie kann ich git einfach nutzen?

git ist ein Werkzeug für die Kommandozeile und ist auf Mac-Systemen standardmäßig vorinstalliert. Eine aktuelle Version kann auf der [offiziellen Downoad-Seite](https://git-scm.com/downloads) heruntergeladen werden. Wir empfehlen jedoch gerade git Anfängern (aber auch Erfahrenen) die Nutzung eines guten git-Programms mit Nutzeroberfläche. [SourceTree](https://www.sourcetreeapp.com) ist dabei sowohl für Mac als auch für Windows verfügbar und das Programm unserer Wahl. Es ist kostenlos (Registrierung nach 30 Tagen notwendig), recht übersichtlich und unterstützt neben einer integrierten aktuellen git-Version auch weitergehende Funktionen wie git-flow (dazu mehr in [diesem Artikel](#)).


## Wann sollte ich Änderungen commiten?

Diese Entscheidung davon ab, mit welchem Ziel commits verwendet werden. Da commits mehrere Vorteile haben – Verhinderung von Datenverlust, Erklärung von Änderungen, Teilen von Code mit anderen – hängt diese Entscheidung vom Zweck ab. Ein paar Faustregeln können hier aber gegeben werden:

* Grundsätzlich sollte die Arbeit an einem Projekt (sofern denn Änderungen zustande kamen) spätestens nach 2h einmal commitet werden, sofern man die getane Arbeit nicht verlieren möchte (auch wenn sie noch nicht komplettiert ist). Dies dient dem Ziel die Folgen von Datenverlust zu minimieren und bei Teams den Fortschritt zu teilen.

* Jede zusammengehörige Veränderung (auch wenn mehrere Dateien beteiligt sind), die eine Sache fertig stellt, sollte commitet werden. Bei typischen Programmieraufgaben werden bei eingespielten Programmierern zwischen commits etwa 2-15 Minuten Zeit vergehen, je nach Größe der gerade überlegten Änderung. Hierbei drückt ein commit eine erdachte Änderung aus, wobei der Gedanke dann als Kommentar festgehalten wird.
