---
section:  General
topic:    Version Control
refid:    "GN01#04"
title:    Gute Commit Messages
date:     2016-01-01 00:00:00
author:   Cihat Gündüz
---

In diesem Artikel werden Hilfestellungen für gute Commit Messages gegeben, indem zuerst der Nutzen von Commit Messages erklärt wird und danach Beispiele für schlechte und gute Commit Messages gegeben werden.

## Was ist das Ziel eines Commit Messages?

Das Ziel eines Commit Messages ist es Änderungen an Code auch im Nachhinein noch verständlich zu machen. Dabei reicht es nicht aus lediglich die gemachten technischen Änderungen zusammenzufassen, sondern viel mehr geht es darum in einem Commit Message zu klären, **warum** die Änderungen gemacht wurden. Nur so ist es möglich im Nachhinein nicht nur die Änderungen zu kennen (die man ja Dank Git-Diffs auch ohne Messages gut sehen kann) sondern auch entscheiden zu können, ob sie noch benötigt werden und somit ob man sie ändern oder verwerfen darf. Wenn eine echte Begründung im Commit Message nicht möglich ist, dann kann es helfen stattdessen den **Kontext der Änderung** zu benennen.

## Wie sollte ein Commit Message aussehen?

Commit Messages können eine beliebige Länge haben, was vielen gar nicht bekannt ist, da einzeilige Kommentare gängige Praxis sind. Auch wir empfehlen die Dinge in wenigen Wörtern auf den Punkt zu bringen, sofern nicht besonderer Erklärbedarf besteht. Da die meisten Git-Tools Commit Messages standardmäßig nach einer Zeile abschneiden und besonders Werkzeuge für die Kommandozeile die Breite stark beschränken, sind folgende Vorgaben für die Länge und Grundstruktur unbedingt einzuhalten:

* Die **erste Zeile** jedes Commit Messages **fasst die Begründung / den Kontext der Änderungen kurz zusammen**
* Die erste Zeile überschreitet nicht die **maximale Länge von etwa 50 Zeichen** zu sehr
* Die restlichen Zeilen (sofern vorhanden) überschreiten nicht eine **Länge von etwa 72 Zeichen** zu sehr
* Falls mehr als eine Zeile geschrieben wird, so **muss nach der ersten Zeile eine leere Zeile** folgen
* Der **restliche Text sollte in ganzen Sätzen** geschrieben und **in sinnvolle Abschnitte gegliedert** sein

Ein kurzer Commit Message sollte also maximal etwa diese Länge aufweisen (51 Zeichen):

```text
Lorem ipsum dolor sit amet consectetuer adipiscing
```

Ein langer Commit Message sollte etwa diese grobe Form aufweisen (Maximum 73 Zeichen):

```text
Lorem ipsum dolor sit amet consectetuer adipiscing

Elit sed diam nonummy nibh euismod tincidunt ut laoreet dolore magna
aliquam erat volutpat. Ut wisi enim ad minim veniam, quis nostrud exerci
tation ullamcorper suscipit lobortis nisl ut aliquip ex ea commodo
consequat.

Duis autem vel eum iriure dolor in hendrerit in vulputate velit esse
molestie consequat, vel illum dolore eu feugiat nulla facilisis at vero
eros et accumsan et iusto odio dignissim qui blandit praesent luptatum
zzril delenit augue duis dolore te feugait nulla facilisi.
```

In den meisten Fällen sollten wenige Worte (also ein einzeiliger Kommentar) reichen, um einen Commit auf den Punkt zu bringen. Falls mehrzeilige Commit Messages zur Regel werden, dann ist das als Zeichen dafür zu deuten, dass man zu selten Commits macht. Man sollte sich in dem Fall überlegen, ob man nicht schon früher commiten kann.

Während die oben genannte Form hauptsächlich zum Ziel hat in bestimmten Tools **überhaupt dargestellt werden zu können**, so fällt auch bei Einhaltung der groben Form in vielen Projekten ohne Standards auf, dass die Commits verschiedener (und manchmal sogar derselben Entwickler) völlig unterschiedliche Formate aufweisen. In nachfolgendem Beispiel ist zu sehen, dass sich meist Groß- und Kleinschreibung, Punkte am Ende der Titelzeile, benutzte Zeitform und sogar die Sprache völlig unterscheiden können:

```text
Projektdateien neu strukturiert.
updated dependencies + integrate SwiftyJSON
adding Swift linter for improved code conventions
ignore jazzy docs directory in Git
SwiftGen integriert für statische Ressourcen
Adds screen flow buttons to home screen.
Simplifies Coordinator delegation calls
Korrektes Speichern der Textgröße-Einstellung!
fixed wrong tracking key on home screen
```

Um eine gewisse Einheitlichkeit zu schaffen definieren wir deshalb zusätzlich folgende Regeln für Commit Messages:

* Der erste Buchstabe der ersten Zeile wird stets **groß geschrieben**
* Es darf ***kein*** **Punkt** oder ein anderes Satzzeichen am Ende der ersten Zeile folgen
* Die erste Zeile wird stets **im Imperativ formuliert** (so als würde man Befehle an Git senden)
* Schreibe alle Zeilen ausschließlich in **englischer Sprache**

```text
Restructure project files
Update dependencies + Integrate SwiftyJSON
Add Swift linter for improved code conventions
Ignore jazzy docs directory in Git
Integrate SwiftGen for statically typed resources
Add screen flow buttons to home screen
Simplify Coordinator delegation calls
Correctly save font size setting
Fix wrong tracking key on home screen
```

Diese Regeln orientieren sich an Community-Standards, die etwa in [diesem Blogpost](http://chris.beams.io/posts/git-commit/) zusammenfassend erläutert werden – genaue Begründungen sind dort nachzulesen.

## Häufig gemachte Fehler

Nachfolgend typische Fehler, die genau so bereits in der Praxis vorgekommen sind mit Verbesserungsvorschlägen.

### Zu generische Commit Messages

Auch wenn man sich in der ersten Zeile kurz fassen sollte, sollte dies nicht zu zu generischen Commits führen, die kaum Aussagekraft haben. Besonders Commits, die nur ein oder zwei Wörter lang sind laufen Gefahr nichts auszusagen. Auch Wörter wie "some" sind Anzeichen von zu generischen Commit Messages.

Schlecht:

```
- Changes
- Some Changes in Settings
- ScreenDrawerPrivate
- Umbrella Header
```

Besser:

```
- Fix BlankSecondScreen link & update constraints
- Change Info.plist path & macro in build settings
- Rename ScreenDrawer module to ScreenDrawerPrivate
- Add umbrella header with public ObjC classes
```

### Falsche Großschreibung im Englischen

Im Deutschen schreibt man alle Substantive groß. Während man im Englischen bei Titeln durchaus auch dazu neigt vieles groß zu schreiben, so sollten Commits dennoch wie normaler englischer Text behandelt werden. Das bedeutet: Die meisten Wörter (inkl. Substantive) werden **klein** geschrieben. Einzige Ausnahmen sind im Englischen Eigennamen wie von Personen, Marken oder Produkten.

Schlecht:

```
- Create Config for Mixed Language Framework
- Remove Some Files for Testing
```

Besser:

```
– Create config for mixed language framework
– Remove some files for testing
```

### Debugging-Commits

Manchmal kommt es vor, dass man ohne Commits eine bestimmte Änderung nicht testen kann, etwa wenn die Änderung an einem Framework stattfindet, welche mit einer Referenz zu einem Commit in einem anderen Projekt installiert wird. In solchen Fällen sollten die Commits dennoch einen sinnvollen Inhalt haben, die die Annahme zum Beheben eines Problems repräsentieren. So können die bereits ausprobierten Ansätze auch im Nachhinein noch eingesehen werden, wenn der Änderungsversuch falsch sein sollte.

Schlecht:

```
- Test Commit
- Tried to replace C-Code
```

Besser:

```
- [Attempt] Fix ObjC-Import by removing modulemap
- [Attempt] Fix ObjC-Import by replacing C-Code
```
