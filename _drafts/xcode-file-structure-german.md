---
layout: post
title:  "Dateistrukturen in Xcode"
categories: ios
---

## Wie finde ich meine Dateien in schnell in Xcode?

Während Xcode über seine mehr als 13 Jahre Lebenszeit immer mehr nützliche Funktionen erhalten hat, die das Arbeiten damit immer weiter vereinfachten (wie Tabs, Assistant-Editor, Quick-Open mit `Cmd+Umschalt+o`) so bleibt es in der Praxis dabei, dass der Project-Navigator meist der Dreh- und Angelpunkt der Arbeit in Xcode darstellt. Dort können nicht nur neue Dateien gefunden, geöffnet, erstellt und umbenannt werden, sondern auch das Sortieren von verschiedenen zusammengehörigen Dateien verschiedenster Art in sogenannten Groups ist möglich. Diese Gruppen sind von der Dateistruktur im Finder komplett unabhängig, sodass es möglich ist (wie in vielen älteren Projekten auch üblich) sämtliche Dateien innerhalb des Hauptprojektverzeichnisses zu belassen und die Strukturierung und Gruppierung der Dateien komplett in Xcode vorzunehmen. Eine neue Gruppe erstellt man mit einem Rechtsklick im Projekt-Navigator und dem Eintrag "New Group".

Das hilfreichste dabei, die richtige Datei schnell in einem Projekt wieder zu finden – sowohl bei häufigem Wechsel zwischen Projekten, die mitunter von anderen angefangen oder geändert wurden, als auch bei älteren eigenen Projekten – ist es den Aufbau der Dateigruppen zu kennen. Diese Struktur ist im Allgemeinen bei jedem Entwickler unterschiedlich, ein Problem, das nicht nur bei der iOS-Entwicklung auftritt. Andere Plattformen wie z.B. Ruby on Rails lösen dieses Problem mit gemeinsamen Konventionen, die klar festlegen, wo welche Datei hin gehört – das dahinter stehende Prinzip wird "Convention over Configuration" – kurz "CoC" genannt. Beim Erstellen eines Rails-Projektes wird dabei etwa automatisch eine ganze Projektverzeichnisstruktur geschaffen, in die man seine Dateien nur noch korrekt einfügen muss. Und selbst dafür gibt es hilfreiche Kommandos.

Während wir *noch* nicht soweit sind ein Projekt automatisch generieren und die Dateien an die richtigen Stellen einfügen zu lassen, so lohnt es sich aber durchaus schon eine Projektstruktur mit semantischer Bedeutung vorzugeben, die einerseits das Auffinden aber andererseits auch das Entscheiden vereinfacht, wo sich eine Datei befinden wird. Im besten Falle sollte eine solche Projektstruktur zugleich eine sinnvolle Trennung von Programmlogik in Bestandteile mit klaren Verantwortlichkeiten begünstigen – ganz nach den Prinzipien gut entwickelter Software (siehe [hier](http://www.oodesign.com/design-principles.html), [hier](https://msdn.microsoft.com/en-us/library/ee658124.aspx) oder [hier](http://code.tutsplus.com/tutorials/3-key-software-principles-you-must-understand--net-25161)).


## Wie sollten meine Dateien innerhalb/außerhalb von Xcode strukturiert sein?

## Empfohlene Dateistruktur

## Wie kann ich diese Struktur auf bestehende Projekte anwenden?
