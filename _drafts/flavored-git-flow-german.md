---
layout: post
title:  "Flavored Git-Flow"
categories: ios android
---

## Was ist Git-Flow?

Git-Flow ist in erster Linie eine Konvention zur Benennung und Benutzung von Branches in git, das Vincent Driessen 2010 in [diesem Blogpost](http://nvie.com/posts/a-successful-git-branching-model/) vorgestellt hat und das seitdem immer größere Verbreitung findet. Das Ziel ist dabei eine einheitliche Arbeitsweise mit Branches in Git einzuführen, die alle üblichen Aufgaben bei der alltäglichen Programmierung und beim Deployment eines Projekts abdeckt. Zudem kommt Git-Flow auch mit einer Sammlung von Skripts, die bei diesen Aufgaben hilfreich zur Seite stehen.

Die Basis jeden Projekts bilden bei der Anwendung von Git-Flow zwei Branches, die auf Vorschlag des usprünglichen Autoren master und develop genannt werden. Auf den master Branch wird grundsätzlich nie direkt commitet, stattdessen werden lediglich fertige Versionen, die außerdem bereits so veröffentlicht wurden von anderen Branches rein gemerged. Im Grunde ist der master Branch also eine Historie des bereits veröffentlichten Codes. Der develop Branch respräsentiert im Gegensatz zum master Branch den aktuellen Stand der Entwicklung. Obwohl das so klingen mag, als könne man Veränderungen direkt auf den develop Branch commiten und dies sogar trotz Anwendung von Git-Flow durchaus immer wieder passiert, sollte bis auf wenige Ausnahmefälle nie direkt im develop Branch commitet werden. master und develop sind also Historien-Branches, deren letzter Commit entweder den aktuellen Stand der Veröffentlichung (master) oder den aktuellsten stabilen Stand der Entwicklung (develop) repräsentieren.

Die eigentliche Arbeit findet bei konsequenter Anwendung von Git-Flow ausschließlich in Arbeits-Branches statt. Diese lassen sich ganz einfach daran erkennen, dass sie alle einen Ordnernamen tragen.

Die reguläre Arbeit mit neuen Funktionen, Änderungen und Bug Fixes findet im Ordner feature/ statt. feature/ Branches haben dabei als Quelle stets den develop Branch, da sie eine Weiterarbeit am aktuellen Stand der Entwicklung sind. Beispielsweise erstellt man für eine neue Funktion, bei der es darum geht Push Notifications in eine App zu integrieren den Branch feature/push_notifications und commitet alles dort hinein. Erst bei Abschluss der Funktion als Ganzes wird der Branch dann in den develop Branch gemerged, wodurch der develop Branch erneut den aktuellsten stabilen Stand der Entwicklung wiedergibt. Diese Vorgehensweise hat den Vorteil, dass beliebig viele Funktionen (von einer Person oder mehreren) gleichzeitig entwickelt werden können, ohne dass ein kurzzeitig instabiler Zustand die Arbeit anderer beeinträchtigt.

Weitere Arbeits-Branches sind in den Ordnern release/, hotfix/ und support/ zu finden.

Der release/ Ordner wird beim Deployment einer fertigen Version verwendet. Ein Beispiel wäre der Branch release/3.1.0, den man vor dem Release der Version 3.1.0 erstellt. In dem Branch können Deployment-spezifische Commits (typischerweise die Änderung von Zertifikaten oder anderen Projekteinstellungen) gemacht werden. Nach abgeschlossener Veröffentlichung der Version wird dann der release/3.1.0 Branch in den master Branch **und** in den develop Branch gemerged. Außerdem findet eine Markierung mittels eines Git Tags statt, mithilfe der ein Commit mit der Version 3.1.0 markiert werden kann. Bei Nutzung der Git-Flow Skripte findet der Merge in beide Historien-Branches sowie die Markierung mit der Versionsnummer automatisch statt.

Der hotfix/ Ordner ist für das Beheben von zeitkritischen Fehlern bei bereits veröffentlichter Software gedacht. Betreibt man zum Beispiel aktuell Version 1.5 im Store und hat Version 2.0 derzeit in Entwicklung und stellt einen schwerwiegenden Fehler fest, den man schnell in der veröffentlichten Version (1.5) beheben aber auch in künftigen Versionen (2.x) ausschließen möchte, so führt man die Commits für den Fix etwa im Beispiel von abgelaufenen Zertifikaten im Branch hotfix/expired-certificates durch. Der Branch hat als Ursprung nicht (wie bei release/ Branches) den develop Branch sondern den master Branch und wird aber nach Fertigstellung des Fixes (genauso wie bei release/ Branches) sowohl in den master als auch in den develop Branch gemerged. Auch um diesen doppelten Merge kümmern sich die Git-Flow Skripte automatisch.

Der support/ Ordner ist eine schon seit Jahren als Beta markierte Funktion von Git-Flow, deren Ziel es ist mehrere veröffentlichte Versionen einer Software mit Updates zu versorgen. Ohne die Nutzung von support/ Branches kann immer nur eine aktuelle Version in einem Git-Projekt verwaltet werden, was im Falle von Apps für den Apple App Store und den Google Play Store auch ausreichend ist. Dennoch können support/ Branches auch bei der Verwaltung von Apps nützlich sein, wenn ein Git-Projekt für mehrere Apps im Store benutzt wird, etwa um mehrere für unterschiedliche Firmen gebrandete Apps zu veröffentlichen. In diesem Fall wird das Branding z.B. für die Firmen Jamit Labs und Apple in den Branches support/jamit_labs und support/apple durchgeführt. Wird die App-Logik verändert können die Änderungen im develop Branch leicht in die verschiedenen support/ Branches gemerged werden, wodurch man die Arbeit nicht doppelt (oder n-fach) durchführen muss, sondern nur einmal.


## Wie kann ich Git-Flow einfach nutzen?





## Was ist Jamit Labs Flavored Git-Flow?
