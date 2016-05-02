---
section:  General
topic:    Version Control
refid:    GN010-0300
title:    Abgewandeltes Git-Flow
date:     2016-01-01 00:00:00
author:   Cihat Gündüz
---

## Was ist Git-Flow?

Git-Flow ist in erster Linie eine Konvention zur Benennung und Benutzung von Branches in Git, das Vincent Driessen 2010 in [diesem Blogpost](http://nvie.com/posts/a-successful-git-branching-model/) vorgestellt hat und das seitdem immer größere Verbreitung findet. Das Ziel ist dabei eine einheitliche Arbeitsweise mit Branches in Git einzuführen, die alle üblichen Aufgaben bei der alltäglichen Programmierung und beim Deployment eines Projekts abdeckt. Zudem kommt Git-Flow auch mit einer Sammlung von Skripten, die bei diesen Aufgaben hilfreich zur Seite stehen.

Die Basis jeden Projekts bilden bei der Anwendung von Git-Flow zwei Branches, die auf Vorschlag des ursprünglichen Autoren **`master`** und **`develop`** genannt werden. Auf den `master` Branch wird grundsätzlich nie direkt commitet, stattdessen werden lediglich fertige Versionen, die außerdem bereits so veröffentlicht wurden von anderen Branches rein gemerged. Im Grunde ist der `master` Branch also eine Historie des bereits veröffentlichten Codes. Der `develop` Branch repräsentiert im Gegensatz zum `master` Branch den aktuellen Stand der Entwicklung. Obwohl das so klingen mag, als könne man Veränderungen direkt auf den `develop` Branch commiten und dies sogar trotz Anwendung von Git-Flow durchaus immer wieder passiert, sollte bis auf wenige Ausnahmefälle nie direkt im `develop` Branch commitet werden. `master` und `develop` sind also **Historien-Branches**, deren letzter Commit entweder den aktuellen Stand der Veröffentlichung (master) oder den aktuellsten stabilen Stand der Entwicklung (develop) repräsentieren.

![Bilderklärung des develop Branches](/public/images/flavored-git-flow/develop.png)
*Der develop-Branch entspringt dem master Branch.*

Die eigentliche Arbeit findet bei konsequenter Anwendung von Git-Flow ausschließlich in Arbeits-Branches statt. Diese lassen sich ganz einfach daran erkennen, dass sie alle einen Ordnernamen tragen.

Die reguläre Arbeit mit neuen Funktionen, Änderungen und Bug Fixes findet im Ordner **`feature/`** statt. ``feature/`` Branches haben dabei als Quelle stets den `develop` Branch, da sie eine Weiterarbeit am aktuellen Stand der Entwicklung sind. Beispielsweise erstellt man für eine neue Funktion, bei der es darum geht Push Notifications in eine App zu integrieren den Branch `feature/push_notifications` und commitet alles dort hinein. Erst bei Abschluss der Funktion als Ganzes wird der Branch dann in den `develop` Branch gemerged, wodurch der `develop` Branch erneut den aktuellsten stabilen Stand der Entwicklung wiedergibt. Diese Vorgehensweise hat den Vorteil, dass beliebig viele Funktionen (von einer Person oder mehreren) gleichzeitig entwickelt werden können, ohne dass ein kurzzeitig instabiler Zustand die Arbeit anderer beeinträchtigt.

![Bilderklärung des feature Branches](/public/images/flavored-git-flow/feature.png)
*Hier sieht man ein Projekt in dem parallel an 2 feature Branches gearbeitet wurde. Der eine wurde bereits in den develop Branch gemerged während sich der andere noch in Entwicklung befindet.*

Weitere Arbeits-Branches sind in den Ordnern `release/`, `hotfix/` und `support/` zu finden.

Der **`release/`** Ordner wird beim Deployment einer fertigen Version verwendet. Ein Beispiel wäre der Branch `release/3.1.0`, den man vor dem Release der Version 3.1.0 erstellt. In dem Branch können Deployment-spezifische Commits (typischerweise die Änderung von Zertifikaten oder anderen Projekteinstellungen) gemacht werden. Nach abgeschlossener Veröffentlichung der Version wird dann der `release/3.1.0` Branch in den `master` Branch **und** in den `develop` Branch gemerged. Außerdem findet eine Markierung mittels eines Git Tags statt, mithilfe dessen ein Commit mit der Version 3.1.0 markiert werden kann. Bei Nutzung der Git-Flow Skripte findet der Merge in beide Historien-Branches sowie die Markierung mit der Versionsnummer automatisch statt.

![Bilderklärung des release Branches](/public/images/flavored-git-flow/release.png)
*Da man Änderungen im develop Branch nie direkt in den master Branch merged erstellt man zum überbrücken einen release Branch. Änderungen an diesem werden automatisch in master und develop gemerged.*

Der **`hotfix/`** Ordner ist für das Beheben von zeitkritischen Fehlern bei bereits veröffentlichter Software gedacht. Betreibt man zum Beispiel aktuell Version 1.5 im Store und hat Version 2.0 derzeit in Entwicklung und stellt einen schwerwiegenden Fehler fest, den man schnell in der veröffentlichten Version (1.5) beheben aber auch in künftigen Versionen (2.x) ausschließen möchte, so führt man die Commits für den Fix etwa im Beispiel von abgelaufenen Zertifikaten im Branch `hotfix/expired-certificates` durch. Der Branch hat als Ursprung nicht (wie bei `release/` Branches) den `develop` Branch sondern den `master` Branch und wird nach Fertigstellung des Fixes (genauso wie bei `release/` Branches) sowohl in den `master` als auch in den `develop` Branch gemerged. Auch um diesen doppelten Merge kümmern sich die Git-Flow Skripte automatisch.

![Bilderklärung des hotfix Branch](/public/images/flavored-git-flow/hotfix.png)
*Hotfix Branches entspringen dem master Branch und werden bei Abschluss automatisch in den master sowie in den develop Branch gemerged.*

Der **`support/`** Ordner ist eine schon seit Jahren als Beta markierte Funktion von Git-Flow, deren Ziel es ist mehrere veröffentlichte Versionen einer Software mit Updates zu versorgen. Ohne die Nutzung von `support/` Branches kann immer nur eine aktuelle Version in einem Git-Projekt verwaltet werden (was im Falle von Apps für den Apple App Store und den Google Play Store auch ausreichend ist). Dennoch können `support/` Branches auch bei der Verwaltung von Apps nützlich sein, wenn ein Git-Projekt für mehrere Apps im Store benutzt wird, etwa um die gleiche App mehrmals für unterschiedliche Firmen gebrandet zu veröffentlichen. In diesem Fall wird das Branding z.B. für die Firmen Jamit Labs und Apple in den Branches `support/jamit_labs` und `support/apple` durchgeführt. Wird die App-Logik verändert können die Änderungen im `develop` Branch leicht in die verschiedenen `support/` Branches gemerged werden, wodurch man die Arbeit nicht doppelt (oder n-fach) durchführen muss, sondern nur einmal.

![Bilderklärung des support Branch](/public/images/flavored-git-flow/support.png)
*TODO!*

## Wie kann ich Git-Flow einfach nutzen?

Sowohl für die Erstellung als auch für das Fertigstellen von verschiedenen Arbeits-Branches in die Historien-Branches gibt es Skripte, die sich im [offiziellen GitHub Projekt](https://github.com/nvie/gitflow) finden und auch über andere Quellen nutzen lassen. Das Programm SourceTree (deren Einsatz wir bereits [hier](#) empfahlen) hat diese Skripte bereits standardmäßig mit an Board, sie sind am einfachsten mit der Tastenkombination Alt+Cmd+F oder über den [konfigurierbaren Git-Flow-Button](http://i.stack.imgur.com/KEQhG.gif) oben in der Menüleiste zugänglich.

Die Nutzung von Git-Flow in SourceTree sei nachfolgend anhand eines `feature/` Branches erläutert:

TODO: Screenshots mit Erklärungen zum Erstellen und Mergen eines feature Branches hinzufügen


## Was ist Jamit Labs Flavored Git-Flow?

![Bild das alten und neuen Flavor zeigt](/public/images/flavored-git-flow/jamit-flavored.png)
*Zum besseren Verständnis empfehlen wir eine Neubenennung der Git Flow Branches*

Die Benennung der verschiedenen Branches als `master`, `develop`, `feature/`, `hotfix/`, `release/` und `support/` läuft Gefahr zu Missverständnissen und Verwechslungen zu führen. Beispielsweise wird der `master` Branch traditionell für den stabilen aktuellen Arbeits-Branch verwendet, sodass einige Programmierer mit Vorerfahrungen gewillt sind, direkt auf den `master` Branch zu commiten. Andere wiederum verwenden `release/` Branches, um verschiedene veröffentlichte Versionen der Software zu verwalten (wozu es in Git-Flow `support/` gibt), anstatt ihn ausschließlich für das Deployment zu verwenden.

Aus diesem Grund erscheint es sinnvoll die Namen der Branches möglichst selbsterklärend und unmissverständlich zu ändern, wozu wir bei Jamit Labs folgende Umbenennungen vorgenommen haben. Es sei angemerkt, dass eine andere Benennung der Branches in Git-Flow explizit vorgesehen ist, was auch von SourceTree beim ersten Ausführen von Git-Flow pro Git-Projekt abgefragt wird, sodass die Umbenennung keinen großen Aufwand darstellt.
Es sei angemerkt, dass eine andere Benennung der Branches vom Nutzer selbst vorgenommen werden muss. Da man von SourceTree beim ersten Ausführen von Git-Flow innerhalb eines Projektes nach der gewünschten Benennung gefragt wird, stellt eine Umbenennung aber keinen großen Aufwand dar.

### master >> productive

Der `master` Branch hat zu viel Historie und ist damit für viele erfahrene Git-Anwender bereits mit bestimmten Aufgaben verbunden. Um mit diesen nicht vorbelastet zu sein und auch um die Tatsache heraus zu stellen, dass Code der in diesem Branch ist bereits veröffentlichter Code ist wäre eine Umbenennung in released nahelegend. Doch dies hätte den Nachteil, dass es mit dem Git-Flow `release/` Branch verwechselt wird, weshalb wir die Alternative aber weiterhin sprechende Benennung productive gewählt haben.

Es sei darauf verwiesen, dass der productive Branch als Historien-Branch ein Adjektiv als Namen hat und explizit kein Verb. Dies soll ein Hinweis darauf sein, dass hierin nicht direkt gearbeitet werden darf.

### develop >> stable

Der `develop` Branch klingt zu sehr nach Entwicklung und obwohl das auch durchaus passend ist ergibt sich aus den Erfahrungen in der Praxis die Notwendigkeit besser heraus zu stellen, dass der `develop` Branch den aktuellsten **stabilen** Entwicklungszustand repräsentiert. Deshalb heißt der `develop` Branch im Jamit Labs Flavor `stable`.

Es sei außerdem darauf verwiesen, dass der `stable Branch als Historien-Branch (wie der productiv`e branch) ein Adjektiv als Namen hat und explizit kein Verb. Dies soll ein Hinweis darauf sein, dass auch hierin nicht direkt gearbeitet werden darf.


### feature/ >> work/

Während der Name `feature/` in der Praxis keine echten Nachteile ergeben hat ist es doch in vielen Situationen etwas seltsam, Branch-Namen wie `feature/fix-memory-usage` zu lesen, da der Begriff "feature" durchaus bereits mit einer anderen Bedeutung besetzt ist. Da jedoch neben "features" auch Bug Fixes, Tests und alle möglichen anderen Arten von Änderungen in `feature/` Branches entwickelt werden ist eine Umbenennung durchaus sinnvoll. Wir verwenden daher den Ordnernamen `work/`. Dies soll zum einen alle verschiedenen Arten von Arbeiten sinnvoll ermöglichen, aber auch die Tatsache verdeutlichen, dass die meiste getane Arbeit typischerweise in `work/` Branches erledigt wird.

Es sei angemerkt, dass explizit ein Verb als Ordnernamen ausgewählt wurde, da es sich hierbei um Branches handelt, in denen Arbeit verrichtet werden darf und soll.


### release/ >> deploy/

Der `release` Branch ist ähnlich wie der `master` Branch für einige erfahrene Git-Anwender aus anderen Projekten bereits mit einer anderen Bedeutung besetzt (nämlich der Bedeutung der `support/` Branches nach Git-Flow). Um Verwechslungen zu vermeiden nehmen wir den klarer auf den Sinn anspielenden Namen `deploy/`. Unter Deployment versteht man schließlich den Release von fertiger Software – genau das, wofür der `deploy/` Branch verwendet wird.

Da auch hier wieder Arbeit in den Branches verrichtet werden soll, wurde wieder ein Verb als Ordnername gewählt.


### hotfix/ == hotfix/

`hotfix/` ist nicht nur ein Name, der bislang keine Verwechslungen hervorrief, sondern trifft auch genau den Sinn dieser Branches. Daher findet hier keine Umbenennung statt.

Wie bei `work/`und `deploy/` wurde wieder ein Verb als Ordnername gewählt.


### support/ >> support/, branding/, ...

`support/` trifft ähnlich wie bei `hotfix/` den Sinn der Branches bei Verwaltung mehrere veröffentlichter Software-Versionen sehr gut, weshalb wir auch hier keine Umbenennung empfehlen. Allerdings ist die Unterstützung von mehreren Software-Versionen nicht immer auf die Versionsnummer bezogen (also den Support von Altversionen) sondern dabei kann es sich zum Beispiel auch um verschiedene Brandings einer App handeln. Da `support/` in solchen Fällen keinen Sinn macht, schlagen wir vor für jede Art von verwalteten Software-Versionen einen eigenen Namen zu verwenden, den man vorher intern mit seinem Team abklären sollte. Für unterschiedliche Brandings könnte etwa `branding/` gewählt werden.



![Gesamtansicht mit allen Branches](/public/images/flavored-git-flow/overview.png)
*Abschließend noch einmal eine Gesamtansicht aller Branches im Zusammenhang (bereits mit neuer Benennung).*

### Weiterführende Links und Quellen

[Weitere anschauliche Erklärung zur Funktionsweise von Git Flow (leider wird hier die Konsole anstelle von SourceTree eingesetzt)](https://www.atlassian.com/pt/git/workflows#!workflow-gitflow)
