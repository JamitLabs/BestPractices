---
section:  General
topic:    Version Control
refid:    GN010-0200
title:    Nutzung von Gitignore-Dateien
date:     2016-01-01 00:00:00
author:   Cihat Gündüz
---

## Was ist eine Gitignore-Datei?

Einer der Hauptgründe für die Verwendung von Versionskontrollsystemen ist die starke Vereinfachung der Zusammenarbeit mehrerer Entwickler in einem Projekt. Eine Hürde stellen dabei jedoch Dateien dar, die lediglich zur Konfiguration der Projektumgebung eines einzelnen Entwicklers vorhanden sind. Diese werden typischerweise von Entwicklungsumgebungen wie Xcode oder Android Studio erstellt und darin sind Dinge gespeichert wie die zuletzt geöffnete Datei oder die Konfiguration der einzelnen Panes (Teilfenster) in der Entwicklungsumgebung. Damit diese Einstellungen nicht von anderen Entwicklern übernommen werden sondern jeder seine eigenen Einstellungen behalten kann, gibt es bei Git die Möglichkeit, bestimmte Dateien von Commits generell auszuschließen.

Das Ignorieren bestimmter Dateien ist auch dann nützlich, wenn man vermeiden möchte, dass durch regelmäßig neu generierte Dateien (wie Build-Artefakte) die Größe des Git-Repos immer weiter wächst. Außerdem können so sicherheitsrelevante Informationen wie Datenbank-Passwörter in Dateien ausgelagert werden, die ebenfalls ignoriert werden, um die Informationen vor eher außenstehenden Entwickler/Partnern zu schützen, denen man kurzfristig Zugriff zum Repo gewährt. Das Ignorieren von Dateien findet in Git durch eine simple Auflistung in einer Textdatei namens `.gitignore` statt. Darin werden alle zu ignorierenden Dateien oder Ordner per Pfad relativ zum Git-Repo in einer eigenen Zeile genannt und Git wird diese Dateien fortan nicht weiter verfolgen.

Eine einfache `.gitignore` kann etwa so aussehen (Zeilen, die mit einem `#` beginnen sind Kommentare):

``` ruby
# Ignore Finder artifacts on OSX
.DS_Store

# Ignore Build artifacts (`/` is optional and indicates that this is a folder)
build/
```

## Einmalige Einrichtung einer globalen Gitignore-Datei

Manche Artefakte und Konfigurationsdateien sind abhängig davon, welches Betriebssystem oder welche Entwicklungsumgebungen man einsetzt. So sollten diese im besten Falle für alle Projekte ignoriert werden, die man auf dem entsprechenden Mac oder PC bearbeitet. Dies ist in Git durch die sogenannte "globale" Gitignore-Datei möglich, wobei mit global hier "projektübergreifend" gemeint ist und sich auf die Git-Installation auf einem Computer bezieht. In SourceTree kann die globale Gitignore-Datei über SourceTree->Einstellungen im Reiter Git ganz oben schnell gefunden und mit einem Klick auf "Datei bearbeiten" editiert werden.

Falls Du kein SourceTree verwendest, findest du sie bei Mac/Linux in deinem Home-Verzeichnis unter `~/.gitignore_global`. Falls Dein Home-Verzeichnis noch keine `.gitignore_global` enthält kannst Du sie dort einfach selbst erstellen.

## Projektspezifische Gitignore-Datei

Viele zu ignorierende Dateien sind projektspezifisch, weshalb man in Git pro Repo eine eigene `.gitignore` Datei definieren kann. Diese wird als Textdatei im Hauptverzeichnis des Repos angelegt und kann durch Einsatz von SourceTree sogar noch einfacher erstellt oder editiert werden. Hierzu öffnet man das Git-Repo in SourceTree, klickt rechts oben auf "Einstellungen" und gelangt so in die projektspezifischen Einstellungen. Nun findet man im Reiter "Erweitert" sowohl den Pfad zur Gitignore-Datei als auch einen Knopf namens "Bearbeiten", der die Datei im Texteditor öffnet, in dem man sie ändern und via `Cmd-S` Tastenkombination speichern kann.

TODO: Besser erklären
Es ist wichtig, dass man die projektspezifische `.gitignore`-Datei selbst committet und nicht (etwa durch Ignorieren durch sich selbst) aus den künftigen Commits ausschließt. Dies stellt sicher, dass jeder, der an dem Projekt mit arbeitet ebenfalls dieselben Dateien nicht committet.

## Was sollte in eine Gitignore-Datei?

Grundsätzlich gehört alles hinein, was sich als Hürde für die gemeinsame Zusammenarbeit herausstellt oder aus anderen Gründen nicht geteilt werden sollte (Speicherplatz, Sicherheit). Die entsprechenden Dateien ergeben sich aus der Praxis und es lohnt sich meist, eine eigene Sammlung an zu ignorierenden Dateien zu führen und den Projektmitarbeitern zukommen zu lassen. Dabei kommt in die globale Gitignore-Datei alles, was sich nur auf den Produktivrechner bezieht, etwa bestimmte Artefakte, die das Betriebssystem im Projekteverzeichnis automatisch erzeugt. Artefakte, die projektspezifisch sind und ignoriert werden sollen, wie etwa der Build-Ordner (spart Speicherplatz, da er sonst nach jedem Build commitet werden muss) gehören hingegen in die projektspezifische Gitignore-Datei.

Für den Anfang sei das [Gitignore-Repo auf GitHub](https://github.com/github/gitignore#a-collection-of-gitignore-templates) empfohlen, welches von vielen Entwicklern regelmäßig gepflegt und mit sinnvollen zu ignorierenden Dateien ausgestattet ist. Anfangs sucht man sich für die globale Gitignore-Datei aus der [globalen Datei-Liste](https://github.com/github/gitignore/tree/master/Global) all diejenigen aus, welche für die eigene Arbeit relevant sind und kopiert sie alle in die globale Gitignore-Datei rein. Für die App-Entwicklung unter iOS wären dies typischerweise "OS X" und "Xcode", bei der Android-Entwicklung würde man den Inhalt von "Xcode" mit dem von "JetBrains" ersetzen (Android Studio ist von JetBrains). Dieser Schritt muss nur einmal pro Entwicklungsrechner erledigt werden. Fortan kopiert man als zweiten Schritt nach Erstellen eines neuen Git-Repos stets die passenden Gitignore-Empfehlungen aus der [Projekt-Dateiliste](https://github.com/github/gitignore). Bei der iOS-Entwicklung wäre dies etwa typischerweise die Datei mit dem Namen "Swift", bei der Android-Entwicklung "Android".


## Wie kann ich bereits commitete Dateien ignorieren?

Manchmal kommt es vor, dass man versehentlich eine Datei committet, die sich dann als eher störend für die Zusammenarbeit herausstellt, oder man bekommt ein Projekt übergeben, in dem noch keine (sinnvolle) Gitignore-Datei angelegt wurde. In diesen Fällen reicht es nicht einfach aus, eine neue Gitignore-Datei für das Projekt anzulegen und die anderen Mitarbeiter darum zu bitten, ihre globale Gitignore-Datei zu aktualisieren, denn Git berücksichtigt den Inhalt der Gitignore-Datei lediglich für neue, noch nicht commitete Dateien. Ist eine Datei jedoch bereits Teil eines Repositorys, so muss sie zunächst aus dem Repo entfernt werden. Dies erzielt man dadurch, dass man die entsprechende Datei löscht und diese Änderung (das Löschen der Datei) committet.

Es ist zu empfehlen diesen Schritt direkt mit der Änderung in der `.gitignore`-Datei gemeinsam zu commiten, dies ist aber nicht zwingend erforderlich. Bevor man die Datei löscht sollte man sie sicherheitshalber in ein Verzeichnis außerhalb des Repos (etwa auf den Desktop) kopieren, um die Datei nach dem erfolgreichen Entfernen & Ignorieren falls nötig wieder hinzufügen zu können. Hat man alles korrekt gemacht so sollte die Datei auch nicht mehr in SourceTree auftauchen.
