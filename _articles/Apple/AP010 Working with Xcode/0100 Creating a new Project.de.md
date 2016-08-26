---
section:    Apple
topic:      Working with Xcode
refid:      AP010-0100
permalink:  /articles/AP010-0100.html
title:      Erstellen eines neuen Projekts
date:       2016-01-01 00:00:00
author:     Simon Back
---

Dieser Artikel widmet sich dem Erstellen eines neuen Xcode Projekts. Dabei geht es vor allem um die richtige Konfiguration des Projekts und dem Verfügbar machen für andere Teammitglieder.

## Anlegen eines neuen Projekts

![Bild des Welcome Fensters](../../../BestPractices/public/images/AP010/0100/welcome-to-xcode.png)
*Unten links befindet sich die Option zum Anlegen eines neuen Projekts*

Nach dem Start von Xcode erwartet einen der Welcome Screen von Xcode. Zum Anlegen eines neuen Xcode Projekts wählt man einfach die Option "Create new Xcode Project". Falls man keinen Welcome Screen angezeigt bekommt, kann man auch einfach den Umweg über `File -> New -> Project...` nehmen.

![Bild des Schritts "Choose Template"](../../../BestPractices/public/images/AP010/0100/choose-template.png)
*Die empfohlene Option ist "Single-View Application"*

Anschließend wird man von Xcode aufgefordert eine Vorlage für das neue Projekt zu wählen. Hier stehen viele verschiedene Optionen zur Verfügung, wie beispielsweise "Master-Detail Application", "Page-Based Application" oder auch "Game". Anhand der Wahl legt Xcode dann selbstständig erste Views und passende View Controller an. In den meisten Fällen bietet sich Single-View Application an, da man nur einen einzelnen leeren View erhält und der Aufbau eines Storyboards vollkommen dem Nutzer überlassen wird. Speziell wenn man eine spezifische File Structure oder andere Best Practices anwenden möchte ist die Wahl von Single-View Application empfehlenswert da Xcode nicht unbedingt die besten Voreinstellungen trifft.

![Bild des Schritts "Choose Options"](../../../BestPractices/public/images/AP010/0100/choose-options.png)
*Die Optionen die wir für unser Beispielprojekt gewählt haben.*

Anschließend muss man noch einige Optionen festlegen. **Product Name** bezieht sich auf den Namen den die App später einmal tragen soll. Dabei sollte man beachten, dass dieser Case Sensitive ist. In unserem Beispielprojekt wählen wir hier "Jamit Challenge". **Organization Name** beschreibt den Namen des Unternehmens/Teams oder des Entwicklers (falls man alleine arbeitet). Im Feld **Organization Identifier** trägt man die eigene Website rückwärts ein. Ein Beispiel wäre "com.jamitlabs". Der eingetragene Identifier wird damit mit keiner echten Domain verknüpft, es handelt sich lediglich um die [von Apple empfohlene Notation](https://www.quora.com/Xcode-What-is-the-significance-of-a-projects-organization-identifier) um den Organization Identifier einzigartig zu machen. Man kann sich also auch ruhigen Gewissens für einen Domainnamen entscheiden der nicht im eigenen Besitz ist. Der **Bundle Identifier** wird anschließend automatisch angelegt und besteht aus dem Organization Identifier und dem Product Name. Bei **Language** handelt es sich um die Programmiersprache mit der man das Projekt anlegen möchte (Swift oder Objective-C). Nachträglich kann man das Projekt noch immer mit einer anderen Sprache erweitern, nur die ersten Dateien werden automatisch in der gewählten Sprache angelegt. Im Zweifelsfall sollte man alle Haken setzen, da die erstellten Dateien nicht stören. **Devices** legt fest für welche Geräte die App später einmal ausgelegt sein wird. Man kann sich zwischen den Optionen "iPhone", "iPad" und "Universal" entscheiden. Universal bedeutet dass die App sowohl auf dem iPad als auch auf dem iPhone lauffähig ist.

Anschließend folgen noch 3 weitere Optionen. **Use Core Data** legt den Grundstein für die Nutzung eines Core Data Models. Xcode legt dann für den Nutzer eine `.xcdatamodeld`-Datei an und legt weitere Voreinstellungen fest. Bei der Wahl von **Include Unit Tests / Include UI Tests** werden für den User bereits die entsprechenden Dateien für die unterschiedlichen Tests angelegt.

Die Optionen können auch alle noch nachträglich geändert werden. Das ist aber Teils mit etwas Aufwand verbunden weshalb es ratsam ist, sich vorher Gedanken über den gewünschten Umfang und die Funktionsweise der App zu machen.

![Bild der Speicherortwahl](../../../BestPractices/public/images/AP010/0100/choose-storage-location.png)
*Letzendlich muss noch der gewünschte Speicherort gewählt werden.*

Anschließend muss der Nutzer nur noch den gewünschten Speicherort wählen. Außerdem kann man sich hier entscheiden ob man bereits beim Erstellen des Projekts ein Git-Repository anlegen möchte. Mit einem Klick auf "Create" ist Erstellungsprozess abgeschlossen.

![Bild des Project Editors](../../../BestPractices/public/images/AP010/0100/project-editor.png)
*Der Project Editor nach dem Anlegen eines neuen Projekts.*

Anschließend öffnet sich der Project Editor. Falls man die Option zum Erstellen eines Repositorys angehakt hat wurden die ersten Dateien bereits mit der Message "Initial Commit" gemerged.


## Voreinstellungen die gemacht werden sollten

TODO: Auf Dateistruktur hinweisen

TODO: Sonstige empfohlene Tools und Settings
