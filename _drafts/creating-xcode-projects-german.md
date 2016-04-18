---
layout: post
title:  "Erstellen eines Xcode Projekts"
categories: ios
---

## Wie lege ich ein neues Xcode Projekt an?

Dieser Artikel widmet sich dem Erstellen eines neuen Xcode Projekts. Dabei geht es vor allem um die richtige Konfiguration des Projekts und dem Verfügbar machen für andere Teammitglieder.

## Anlegen eines neuen Projekts
Nach dem Start von Xcode erwartet einen der Welcome Screen von Xcode. Zum Anlegen eines neuen Xcode Projekts wählt man einfach die Option "Create new Xcode Project". Falls man keinen Welcome Screen angezeigt bekommt kann man auch einfach den Umweg über `File -> New -> Project...` nehmen.

![Bild des Welcome Fensters](public/images/creating-xcode-projects/welcome-to-xcode.png)
*Unten links befindet sich die Option zum anlegen eines neuen Projekts*

Anschließend wird man von Xcode aufgefordert eine Vorlage für das neue Projekt zu wählen. Hier stehen Optionen viele verschiedene Optionen wie beispielsweise "Master-Detail Application", "Page-Based Application" oder auch "Game" zur Verfügung. Anhand der Wahl legt Xcode dann selbstständig erste Views und passende View Controller an. In den meisten Fällen bietet sich Single-View Application an, da man nur einen einzelnen leeren View erhält und der Aufbau eines Storyboards vollkommen dem Nutzer überlassen wird. Speziell wenn man eine spezifische File Structure oder andere Best Practices anwenden möchte ist die Wahl von Single-View Application empfehlenswert da Xcode nicht unbedingt die besten Voreinstellungen trifft.

![Bild des Schritts "Choose Template"](public/images/creating-xcode-projects/choose-template.png)
*Die empfohlene Option ist "Single-View Application"*

Anschließend muss man noch einige Optionen festlegen. **Product Name** bezieht sich auf den Namen den die App später einmal tragen soll. Dabei sollte man beachten, dass dieser Case Sensitive ist. In unserem Beispielprojekt wählen wir hier "Jamit Challenge". **Organization Name** beschreibt den Namen des Entwicklers. Da wir hier bei Jamit Labs nur selten eigene Projekte umsetzen, wählen wir meist den Namen des Auftraggebers. Im Feld **Organization Identifier** trägt man die Website des Unternehmens oder Entwicklers rückwärts ein. Ein Beispiel wäre "com.jamitlabs". Der eingetragene Identifier wird damit mit keiner echten Domain verknüpft, es handelt sich lediglich um die [von Apple empfohlene Notation](https://www.quora.com/Xcode-What-is-the-significance-of-a-projects-organization-identifier) um den Organization Identifier einzigartig zu machen. Der **Bundle Identifier** wird anschließend automatisch angelegt und besteht aus dem Organization Identifier und dem Product Name. Bei **Language** handelt es sich um die Programmiersprache mit der man das Projekt anlegen möchte (Swift oder Objective-C). Nachträglich kann man das Projekt noch immer mit einer anderen Sprache erweitern, nur die ersten Files werden automatisch in der gewählten Sprache angelegt. **Devices** legt fest für welche Geräte die App später einmal ausgelegt sein wird.

Anschließend folgen noch 3 weitere Optionen. **Use Core Data** legt den Grundstein für die Nutzung eines Core Data Models. Xcode legt dann für den Nutzer eine `.xcdatamodeld` file an und legt weitere Voreinstellungen fest. Bei der Wahl von **Include Unit Tests/Include UI Tests** werden für den User bereits die entsprechenden Files für die unterschiedlichen Tests angelegt.

Die Optionen können auch alle noch nachträglich geändert werden. Das ist aber Teils mit etwas Aufwand verbunden weshalb es ratsam ist, sich vorher Gedanken über den gewünschten Umfang und die Funktionsweise der App zu machen.

![Bild des Schritts "Choose Options"](public/images/creating-xcode-projects/choose-options.png)
*Die Optionen die wir für unser Beispielprojekt gewählt haben.*

Anschließend muss der Nutzer nur noch den gewünschten Speicherort wählen und der Project Editor für das neu erstellte Projekt öffnet sich. Das Projekt sollte man nun einmal commiten ([hier](version-control-german) mehr zu commits und Version Control). Als commit Message bietet sich `Initial commit` an.

![Bild des Project Editors](public/images/creating-xcode-projects/project-editor.png)
*Der Project Editor nach dem Anlegen eines neuen Projekts.*

## Voreinstellungen die gemacht werden sollten

Im Project Navigator findet man nun alle Files die bereits von Xcode angelegt wurden. Diese haben aber keinerlei Struktur, deshalb ist es ratsam erst einmal Ordnung im Project Navigator zu schaffen. Dazu sollte man sich an [diesen Artikel](xcode-file-structure-german)) halten. Anschließend sollte man den Fortschritt erneut commiten (Message: `Use Jamit flavored file structure`)
