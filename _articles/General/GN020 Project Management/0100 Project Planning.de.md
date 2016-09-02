---
section:    General
topicid:    GN020
refid:      GN020-0100
permalink:  /:language/articles/GN020-0100.html
title:      Projektplanung
date:       2016-01-01 00:00:00
author:     Cihat Gündüz
language:   de

prev_refid: GN010-0400
next_refid: GN020-0200
---

In diesem Artikel werden die Schritte erläutert, die bei der Planung eines Projekts zu beachten sind. Außerdem
werden weitere Tipps gegeben, um die einzelnen Schritte in der Praxis möglichst effizient durchzuführen.

## Überblick

Bei der Durchführung eines Projekts werden meist folgende Schritte durchlaufen:

- Auflistung der Funktionalen und nichtfunktionalen **Anforderungen** (Lastenheft)
- **Aufwandsschätzung** getrennt nach Anforderungen mit Pflichtenheft (optional)
- Festlegung **Umsetzungsreihenfolge** unter Berücksichtigung der Abhängigkeiten
- **Datenmodellierung** sowie Auflistung der **API**-Endpunkte
- Aufsetzen der **Projekte in GitLab** und Anlegen von **Issues** pro Projekt
- **Aufgabenverteilung** über Issue-Zuweisungen und **Zeitplan** via GitLab-Milestones

Die oben genannten Schritte sollen im Nachfolgenden im Detail erläutert werden. Beachte, dass die Projektplanung
lediglich einen erfolgreichen Start eines Projekts sicherstellen soll. Während der Umsetzung des Projekts fallen weitere
Management-Aufgaben an, wie etwa das Projektcontrolling ([GN020-0300](GN020-0300.html)).

## Anforderungen

Bei den Anforderungen geht es darum, im Kontakt mit dem Auftraggeber alle Funktionen, die die App am Ende unterstützen
soll aus Nutzersicht festzuhalten. Hierbei ist es meist hilfreich **Wireframes** für die wichtigsten Screens zu
entwerfen, um so die Interface-Elemente mit verschiedenen Funktionalitäten zu verbinden. Dabei sollte man sich gemeinsam
überlegen, wie die Software am Ende typischerweise eingesetzt wird und entsprechende **Szenarien** Screen für Screen
festhalten. Auch eine Untersuchung des Funktionsumfangs von **Konkurrenten und ähnlichen Apps** sorgt meist dafür, dass
man die Liste alle gewünschten Dinge abdeckt. Die Liste der Funktionen, die durch diese Ansätze entsteht, stellt dann im
Ergebnis die **funktionalen Anforderungen** dar.

Für die Feststellung der **nichtfunktionalen Anforderungen** können folgende Fragen helfen:

- Welche **Plattformen** sollen unterstützt werden? (Android / iOS / Web)
- Welche **Gerätetypen** sollen unterstützt werden? (Tablet / Phone / Watch / TV)
- Welche **Systemversionen** sollen unterstützt werden? (iOS 9+ / Android 4.1+)
- Welche **Screen-Modi** sollen unterstützt werden? (Portrait / Landscape)
- Gibt es besondere Anforderungen an die **Qualitätssicherung** der App? (Testabdeckung)
- Gibt es besondere Anforderungen an die **Intuitivität** der App? (Onboarding, Beta-Phase)
- Gibt es besondere Anforderungen an die **Geschwindigkeit** der App? (z.B. komplexe Datenverarbeitung)

## Aufwandsschätzung

Auf dieses Thema gehen wir im Artikel [GN020-0200](GN020-0200.html) im Detail ein. Hier sei noch ergänzt, dass
nicht immer eine Aufwandsschätzung vonnöten ist, da manchmal das Produkt Stück für Stück gebaut werden soll und der
Endpreis für den Auftraggeber weniger relevant ist, solange der Stundensatz bekannt ist.

## Umsetzungsreihenfolge

Bei diesem Schritt geht es zunächst darum, die Reihenfolge der umzusetzenden Funktionen so festzulegen, dass die
Anforderungen in möglichst kurzer Zeit erfüllt werden können. Die Funktionen sollten deshalb so in verschiedene
Meilensteine sortiert werden, dass die nachfolgend geschilderten Phasen durchlaufen werden können:

Zunächst ist es wichtig, dass die App **möglichst früh bedienbar** ist und man so gleich Feedback holen kann. Dabei
können im ersten Schritt alle Daten- und Zugriffsvalidierungen außer Acht gelassen und **die Hauptfunktionen** direkt
umgesetzt werden, um diese frühzeitig nutzbar zu machen. Auch Beispieldaten können in einer frühen Phase fest
einprogrammiert werden, damit der Fokus auf den nützlichen Funktionen bleibt und anfangs keine Energie auf Dinge wie die
Optimierung der Dateneingabe verwendet werden muss. Das Ergebnis dieser aller ersten Schritte sollte eine Art
Klick-Dummy-App sein – der **erste Meilenstein** ist definiert.

Im nächsten Schritt sollte die App tatsächlich **funktionstauglich** gemacht werden, indem man sich um die anderen
Kernaspekte kümmert, die im Klick-Dummy bislang außer Acht gelassen wurden. Hier können nun auch Validierungen und
Dateneingaben umgesetzt werden – das Ergebnis ist eine App, die ähnlich aussieht wie das Ergebnis aus dem ersten
Meilenstein – allerdings diesmal nicht mehr nur ein „Klick-Dummy“ ist: Der **zweite Meilenstein** ist fertig gestellt.
Der zweite Meilenstein sollte die Kernfeatures bereits enthalten.

Der **dritte Meilenstein** ist dazu da, alle relativ schnell umsetzbaren Neben-Funktionen fertig zu stellen. Das
Ergebnis ist eine App, der funktional nur noch einige wenige Dinge fehlen. Der Sinn hiervon ist, dass frühzeitig für die
gesamte App Feedback eingeholt werden soll, ohne auf die paar kleinen Funktionen zu warten, die noch fehlen aber
vermutlich noch eine Weile dauern werden. Dies trägt dazu bei, in der Beta-Phase weniger Iterationen zu brauchen, bis
der Feinschliff der App erfolgt ist. Wichtig ist, dass die noch fehlenden Funktionen klar als solche zu kennzeichnen
sind (sofern möglich).

Sobald auch alle Nebenfunktionen fertig gestellt sind und auch erste Feinschliff-Arbeiten erledigt sind **beginnt die
Beta-Phase**. Der **vierte Meilenstein** ist erreicht – nun sollten regelmäßig neue Beta-Versionen online gestellt und
Feedback für diese eingesammelt werden. Dabei sollte jede Beta-Version klar auflisten, was geändert wurde.

Wurde in mehreren Iterationen Feedback eingeholt, bis der Auftraggeber mit der Qualität zufrieden ist, dann erfolgt der
Release. Der **fünfte Meilenstein** ist erreicht. Es ist damit zu rechnen, dass am Tag der Veröffentlichung oder kurz
danach noch einige Randprobleme auftreten, die dann mit einer Hotfix-Version zu schließen sind. Sind die ersten
Wehwehchen behoben und die App mit einer oder mehreren Hotfix-Versionen längere Zeit ohne größere Probleme im Store, so
ist das Projekt **erfolgreich abgeschlossen**.

**In der Übersicht:**
- Meilenstein #1: Klick-Dummy mit wichtigsten Funktionen
- Meilenstein #2: Funktionale App mit wichtigsten Funktionen
- Meilenstein #3: App mit den meisten Funktionen
- Meilenstein #4: Beta-Phase mit Feedback
- Meilenstein #5: Release ggf. mit Hotfixes

Wichtig: Die Funktionen sollten lediglich auf die ersten drei Meilensteine verteilt werden. Meilensteine #4 und #5
ergeben sich dann mit der Zeit von selbst (siehe auch [GN020-0200](GN020-0200.html)). Der erste Schritt hierzu ist
die Auswahl an Kernfunktionen für die ersten beiden Meilensteine.

## Datenmodellierung & API-Entwurf

Die Modellierung der Daten findet in Form eines UML-Klassendiagramms statt. Hierzu gibt es viele Lösungen, die vermutlich einfachste und schnellste ist der Webdienst **[draw.io](https://www.draw.io/?libs=uml&lang=de)**, in dem man sich die UML-Elemente per Drag-and-Drop zusammen klickt und mit den nötigen Attributen und Abhängigkeiten ausstattet. Das Ergebnis kann dann in verschiedenen Formaten heruntergeladen werden, mitunter als PNG.

Bei der Datenmodellierung werden folgende Schritte durchlaufen:

- Anlegen einer **Liste aller zu speichernden Informationen**
	- hierbei sollten auch die Wireframes zu Rate gezogen werden
- **Gruppieren** der Einzelinformationen **zu Klassen**
- **Abbilden der Abhängigkeiten** zwischen den Klassen
- **Abstraktion gemeinsamer Daten** in Interfaces/Oberklassen
- **Definieren von Methoden** auf den Klassen zur Interaktion

Das Ergebnis der Datenmodellierung sieht etwa so aus:

TODO: Bild erstellen
![Datenmodell](../../../BestPractices/public/images/GN020/0100/example-data-model.png)
*Ein Datenmodell als UML-Klassendiagramm gestaltet auf draw.io.*

Ist die Datenmodellierung abgeschlossen, kann eine dazu passende API modelliert werden. Hierzu empfiehlt sich eine **Mind-Map**, die in der ersten Stufe alle **Ressourcen** auflistet und in der nächsten Stufe dann pro Ressource die benötigten **Endpoints** listet. Die Endpoints können dann wiederum als Auflistung die möglichen Parameter (bei POST, PUT, PATCH) oder die von der API zurückgegebenen Datenfelder (bei GET) enthalten. Für die Erstellung einer Mind-Map gibt es viele Programme, **[mindmeister.com](https://www.mindmeister.com)** ist ein besonders einfacher und schöner Web-Dienst mit einer kostenlosen Option.

Das Ergebnis der Mind-Map kann dann etwa so aussehen:

![API Endpoints](../../../BestPractices/public/images/GN020/0100/example-api-mindmap.png)
*Eine Mind-Map mit API Endpoints gestaltet auf mindmeister.com.*


## Projekte in GitLab & Issues

## Aufgabenverteilung & Zeitplan
