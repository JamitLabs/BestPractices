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


## Wie kann ich git effizient nutzen?



## Wann sollte ich Änderungen commiten?
