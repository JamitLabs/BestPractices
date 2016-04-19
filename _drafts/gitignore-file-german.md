---
layout: post
title:  "Nutzung von .gitignore-Dateien"
categories: ios android
---

## Was ist ein .gitignore-File?

Eines der Hauptgründe für die Verwendung von Versionskontrollsystemen ist die starke Vereinfachung der Zusammenarbeit von mehreren Entwicklern in einem Projekt. Eine Hürde stellen dabei jedoch Dateien dar, die lediglich zur Konfiguration der Projektumgebung der Entwickler vorhanden sind, die typischerweise von Entwicklungsumgebungen wie Xcode oder Android Studio erstellt werden. In diesen Dateien sind Dinge gespeichert wie die zuletzt geöffnete Datei oder die Konfiguration der einzelnen Panes (Teilfenster) in der Entwicklungsumgebung, zu denen Entwickler sehr unterschiedliche Geschmäcker haben. Damit diese Einstellungen nicht von anderen Entwicklern übernommen werden, sondern jeder seine eigenen Einstellungen behalten kann gibt es bei Git die Möglichkeit, bestimmte Dateien von Commits generell auszuschließen.

Das Ignorieren bestimmter Dateien ist auch dann nützlich, wenn man vermeiden möchte, dass durch regelmäßig neu generierte Dateien (wie Build-Artifakte) die Größe des Git-Repos immer weiter wächst. Außerdem können so sicherheitsrelevante Informationen wie Datenbank-Passwörter in Dateien ausgelagert werden, die ebenfalls ignoriert werden, um die Informationen vor eher außenstehenden Entwickler/Partnern zu schützen, denen man kurzfristig Zugriff zum Repo gewährt. Das Ignorieren von Dateien findet in Git durch eine simple Auflistung in einer Textdatei namens `.gitignore` statt. Darin werden alle zu ignorierenden Dateien oder Ordner per Pfad relativ zum Git-Repo in einer eigenen Zeile genannt und Git wird diese Dateien fortan nicht weiter verfolgen.

Ein einfacher `.gitignore` kann etwa so aussehen (Zeilen, die mit einem `#` beginnen sind Kommentare):
``` ruby
# Ignore Finder artifacts on OSX
.DS_Store

# Ignore Build artifacts (`/` is optional and indicates that this is a folder)
build/
```

## Einmalige Einrichtung einer globalen .gitignore

Manche Artefakte und Konfigurationsdateien sind abhängig davon, welches Betriebssystem oder welche Entwicklungsumgebungen man typischerweise einsetzt. So sollten diese im besten Falle für alle Projekte ignoriert werden, die man auf dem entsprechenden Mac oder PC bearbeitet. Dies ist in Git möglich durch die sogenannte "globale" Gitignore-Datei, wobei mit global hier "projektübergreifend" gemeint ist und sich auf die Git-Installation auf einem Computer bezieht. In SourceTree kann die globale Gitignore-Datei über die SourceTree-Einstellungen im Reiter Git ganz oben schnell gefunden und mit einem Klick auf "Datei bearbeiten" editiert werden.

## Projektspezifische .gitignore Datei

Viele zu ignorierende Dateien sind projektspezifisch, weshalb man in Git pro Repo eine eigene `.gitignore` Datei definieren kann. Diese wird als Textdatei in das Hauptverzeichnis des Repos angelegt und kann in SourceTree sogar noch einfacher erstellt oder editiert werden. Hierzu öffnet man das Git-Repo in SourceTree, klickt rechts oben auf "Einstellungen" und gelangt so in die projektspezifischen Einstellungen. Nun findet man im Reiter "Erweitert" sowohl den Pfad zur Gitignore-Datei als auch einen Knopf namens "Bearbeiten", der die Datei im Texteditor öffnet, wo man sie ändern und via `Cmd-S` Tastenkombination speichern kann.

## Was sollte in eine Gitignore-Datei?

Grundsätzlich gehört alles hinein, was sich als Hürde für die gemeinsame Zusammenarbeit herausstellt oder aus anderen Gründen nicht geteilt werden sollte (Speicherplatz, Sicherheit). Die entsprechenden Dateien ergeben sich aus der Praxis und es lohnt sich meist, eine eigene Samllung an zu ignorierenden Dateien zu führen und den Projektmitarbeitern zukommen zu lassen. Dabei kommt in die globale Gitignore-Datei alles, was sich auf den Produktivrechner bezieht, etwa bestimmte Artefakte, die das Betriebssystem im Projekteverzeichnis automatisch erzeugt. Artefakte, die projektspezifisch sind und ignoriert werden sollen, wie etwa der Build-Ordner (spart Speicherplatz, da sonst nach jedem Build commitet werden muss) gehören hingegen in die projektspezifische Gitignore-Datei.

Für den Anfang sei das [Gitignore-Repo auf GitHub](https://github.com/github/gitignore#a-collection-of-gitignore-templates) empfohlen, welches von vielen Entwicklern regelmäßig gepflegt und mit sinnvollen zu ignorierenden Dateien ausgestattet ist. Anfangs sucht man sich für die globale Gitignore-Datei aus der [globalen Datei-Liste](https://github.com/github/gitignore/tree/master/Global) all diejenigen aus, welche für die eigene Arbeit relevant sind und kopiert sie alle in die globale Gitignore-Datei rein. Für die App-Entwicklung unter iOS wären dies typischerweise "OS X" und "Xcode", bei der Android-Entwicklung würde man den Inhalt von "Xcode" mit dem von "JetBrains" ersetzen (Android Studio ist von JetBrains). Dieser Schritt muss nur einmal pro Entwicklungsrechner erledigt werden. Fortan kopiert man als zweiten Schritt nach Erstellen eines neuen Git-Repos stets die passenden Gitignore-Empfehlungen aus der [Projekt-Dateiliste](https://github.com/github/gitignore). Bei der iOS-Entwicklung wäre dies etwa typischerweise die Datei mit dem Namen "Swift", bei der Android-Entwicklung "Android".


## Wie kann ich bereits commitete Dateien ignorieren?
