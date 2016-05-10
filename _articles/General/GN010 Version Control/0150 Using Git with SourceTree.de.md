---
section:  General
topic:    Version Control
refid:    GN010-0150
title:    "Arbeiten mit Git in SourceTree"
date:     2016-01-01 00:00:00
author:   Cihat Gündüz
---

In diesem Artikel sollen alltägliche Aufgaben in Git mithilfe des Programms [SourceTree]([SourceTree](https://www.sourcetreeapp.com)) erklärt werden.

## Ersteinrichtung

TODO

## Arbeiten im Repository Browser

Der Repository Browser von SourceTree zeigt standardmäßig eine **Übersicht aller Repositories**, die man **lokal** auf dem Rechner über SourceTree eingerichtet hat. Öffnen kann man ihn am schnellsten mit der Tastenkombination `Cmd+B`. Er besitzt auch eine **"Remote"-Ansicht**, in der alle Repositories von Hosted Services aufgelistet sind, auf die man Zugriff hat und in denen man sich über die Repository Browser-Einstellungen (Settings-Icon rechts oben) angemeldet hat. Beim nachfolgend beschriebenen Arbeitsstil braucht man die Remote-Ansicht jedoch nicht, weshalb man sie komplett ignorieren kann.

Der Repository Browser erlaubt es **Repositories beliebig zu gruppieren**, ohne dass davon die tatsächlichen Pfade der Projekte geändert werden. Nützlich ist dies etwa, um mehrere Code-Projekte zu einem zusammengehörenden Produkt zusammen zu fassen.

### Erstellen eines neuen Respositories

Verzeichnisse, die von Git versioniert werden nennt man **Repositories**. Möchte man Git in einem neuen Projekt verwenden, so muss man aus dem Projektverzeichnis ein Respository machen. Dies übernimmt meist schon die Entwicklungsumgebung (z.B. Xcode oder Android Studio) biem Anlegen eines Projektes, aber nachfolgend sei kurz die manuelle Einrichtung in SourceTree geschildert:

Klicke im Repository Browser auf den Button "+ Neues Repository" und wähle "Lokales Repository erstellen". Trage nun als Zielpfad das Verzeichnis ein, in welchem das Repository angelegt werden soll, vergebe einen Namen und klicke auf "Erstellen". Der Haken ist bei "Erstelle auch ein Remote Repository" standardmäßig nicht gesetzt und der Typ steht standardmäßig auf "Git", was beides so bleiben sollte. Schon ist das neue Repository angelegt.

![Dialog zum Erstellen eines neuen Repositorys](../../../public/images/GN010/0150/create-new-repo-dialog.png)
*Im Dialog zum Erstellen eines neuen Repositorys trägt man Zielpfad und Name ein.*

### Ein bestehendes Respository verwenden

Soll man mit einem bestehenden Respository arbeiten, so erhält man in der Regel eine Einladung für den Zugriff auf das Projekt auf einem Hosted Service (z.B. github.com, gitlab.com). Diese Services bieten zwar die Möglichkeit an, den Code etwa als ZIP-Datei herunter zu laden, jedoch ist dies nicht zu empfehlen. Stattdessen sollte man Ausschau halten nach dem "SSH" bzw. "HTTPS" Pfad.

![Hosted Service](../../../public/images/GN010/0150/hosted-service.png)
*Möglichkeiten ein Repository von einem Hosted Service auszuschechen.*

Nun markiert und kopiert man entweder den SSH oder den HTTPS Pfad mittels `Cmd+C`, öffnet SourceTree, bringt den Respository Browser mit `Cmd+B` in den Vordergrund und wählt oben unter "+ Neues Repository" den Eintrag "Von URL klonen".

![Von URL klonen](../../../public/images/GN010/0150/clone-from-url.png)
*Im Repository Browser können neue Repositories hinzugefügt werden.*

Es erscheint ein Dialog, in dem man den kopierten Pfad mittels `Cmd+V` einfügt, den Zielpfad auswählt, in den die Projektdateien des Projekts heruntergeladen werden sollen und definiert zusätzlich einen Anzeigenamen innerhalb des Repository Browsers.

![Klondialog](../../../public/images/GN010/0150/clone-dialog.png)
*Beim Klonen kann der Zielpfad und der Anzeigename im Repository Browser festgelegt werden.*

Klickt man nun auf den Button "Klone" beginnt der Prozess, der den aktuellen Stand des Repositories vom Hosted Service herunterlädt – kurz das Repository wird **ausgecheckt***.

#### SSH oder HTTPS?

Grundsätzlich ist es einfacher per HTTPS auszuchecken, da Open Source Repositories dann gänzlich ohne Einrichtung funktionieren und für private Repositories lediglich die Login-Daten abgefragt werden. Daher sei für den Anfang HTTPS empfohlen.

Langfristig kann es jedoch auch Sinn machen auf SSH umzusteigen, da hier die Authentifizierung über ein Public-Key-Verfahren geschieht. Hierzu muss lokal auf dem Rechner eine Kombination aus Private- und Public-Keys generiert werden (falls nicht schon vorhanden), der Public-Key muss anschließend auf dem Hosted Service eingetragen werden, wodurch der Rechner für alle künftigen Projekte (durch den Private-Key) automatisch authentifiziert ist. Zur Einrichtung gibt es viele Anleitungen, zum Beispiel hier jeweils eine für [GitHub](https://help.github.com/articles/generating-an-ssh-key/) und eine für [GitLab](http://doc.gitlab.com/ce/ssh/README.html).

## Arbeiten in der Projektansicht

Mit einem Doppelklick auf ein Repository im Repository Browser öffnet man die Projektansicht eines einzelnen Repository. In der Projektansicht werden alle nötigen Aktionen innerhalb eines Git Repositories durchgeführt. Außerdem kann man in ihr die bestehenden Commits und Branches begutachten und somit die Änderungshistorie des Projekts verfolgen.

![History-Ansicht eines fortgeschrittenen Projekts](../../../public/images/GN010/0150/project-history-view-advanced.png)
*Ein typisches Respository mit mehreren Branches, Merges, Remotes und Commits in der Projektansicht.*

Da die Projektansicht eine aus vielen Bestandteilen kombinierte Ansicht ist, kann sie im ersten Moment etwas erschlagend und überfüllt wirken. Daher seien nachfolgend die für die tägliche Arbeit wichtigen Teile kurz erklärt, wobei wir in den Beispielbildern ein neues Demo-Repository benutzen.

### Toolbar

![Toolbar in der Projektansicht](../../../public/images/GN010/0150/project-view-highlight-toolbar.png)
*Wichtig sind in der konfigurierbaren Toolbar vor allem Pull und Push – die anderen Buttons werden viel seltener gebraucht und können deshalb auf Wunsch alle entfernt werden.*

Die Toolbar besteht standardmäßig aus 10 Buttons, von denen wir nur wenige im Alltag brauchen. Deshalb sei darauf hingewiesen, dass die Toolbar per Rechtsklick und "Symbolleiste anpassen ..." vollständig an die eigenen Bedürfnisse anpassbar ist. Die so gemachten Änderungen bleiben projektübergreifend erhalten. Man kann beispielsweise die kaum genutzten Buttons "Commit", "Anfordern", "Branch", "Merge", "Stash" und "Im Finder anzeigen" aus der Toolbar entfernen. So bleiben nur noch die folgenden übrig:

- **Pull**: Lädt den aktuellsten Stand vom Hosted Service ("remote") des aktuellen Branches ab und merged ihn automatisch mit dem lokalen Stand, sofern auf beiden Änderungen statt fanden.
- **Push**: Lädt den aktuellsten Stand des aktuellen Branches zum Hosted Service hoch, damit andere ihn "pullen" können.
- **Eingabeaufforderung**: Öffnet das Terminal im Projektverzeichnis um dort Command Line Tools.
- **Einstellungen**: Öffnet die projektspezifischen Einstellungen, wo man z.B. die Remotes verwalten kann.


### Sidebar-Eintrag Arbeitskopie

![Arbeitskopie-Eintrag in der Seitenleiste des Projekts](../../../public/images/GN010/0150/project-view-highlight-working-copy.png)
*In "Dateistatus" werden neue Commits gemacht, während "Verlauf" dazu dient die letzten Änderungen (Commits) zu durchstöbern. "Suchen" wird wesentlich seltener benutzt.*

Über den Eintrag "Arbeitskopie" in der Seitenleiste der Projektansicht lässen sich unterschiedliche Ziele erfüllen:

- **Dateistatus**: Hier werden die lokalen Änderungen in dem Repository angezeigt, wobei es möglich ist Dateien oder einzelne Zeilen als Änderungen für den nächsten Commit einzutragen. Auch die Commit Message lässt sich hier definieren und der Commit schließlich auch durchführen.
- **Verlauf**: Hierbei handelt es sich um die History-Ansicht, die sich auch standardmäßig öffnet. Man kann hier die zuletzt gemachten Änderungen, die sogenannte "Git-Historie" sehen. Dies geschieht in Form von nach Datum absteigend sortierten Commits, wobei bei Anklicken eines Commits die zu dem Commit zugehörigen Änderungen im unteren Bereich angezeigt werden.
- **Suchen**: Hier kann alle Commit Messages der gesamte Git-Historie gezielt durchsuchen. Die anderen beiden Ansichten werden wesentlich öfter gebraucht, da in ihnen die Hauptarbeit geleistet wird: "Dateistatus" zum Committen und "Verlauf" zum Durchstöbern der letzten Änderungen.

### Sidebar-Eintrag Branches

![Branches-Eintrag in der Seitenleiste des Projekts](../../../public/images/GN010/0150/project-view-highlight-branches.png)
*TODO*

TODO


![History-Ansicht eines neuen Projekts](../../../public/images/GN010/0150/project-history-view.png)
*Die History-Ansicht eines Projekts zeigt im Hauptbereich die Liste der vergangenen Commits, sowie die aktuellen noch nicht committeten Änderungen an.*

![Änderungsansicht eines Projekts](../../../public/images/GN010/0150/file-change-view.png)
*Die Änderungen-Ansicht zeigt die aktuellen Dateiänderungen im Projekt an und erlaubt sowohl das Vormerken von Dateien als auch das Eintragen einer Commit Message.*


![](../../../public/images/GN010/0150/project-view-highlight-file-state.png)

![](../../../public/images/GN010/0150/project-view-highlight-history.png)
