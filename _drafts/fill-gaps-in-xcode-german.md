---
layout: post
title:  "Fülle die Lücken in Xcode"
categories: ios
---

In diesem Artikel geht es darum gewisse Schwächen von Xcode mithilfe von Tools und Programmierlösungen auszugleichen. Hierzu wird für jedes Thema zunächst das Problem mit Xcode und deren mögliche Folgen in der Praxis erläutert, woraufhin ein Lösungsvorschlag gegeben wird.

## Fehlende Erkennung von neuen Übersetzungs-Keys

### Problembeschreibung

Bei der Entwicklung mit Xcode gibt es die Möglichkeit mithilfe des in der `Foundation` Library definierten Makros `NSLocalizedString(key,comment)` Texte programmatisch zu lokalisieren. Dies ermöglicht das Anbieten einer App in vielen verschiedenen Programmiersprachen, deren Übersetzungen typischerweise zusammengefasst sind in der Datei `Localizable.strings`, welche für jede Sprache eine eigene Version hat. Dabei wird im Quellcode lediglich ein Key verwendet, welcher in den `.strings`-Dateien die jeweiligen Texte in den jeweiligen Sprachen zugewiesen werden. Die App sucht sich dann auf dem Endgerät je nach Geräteeinstellung die passenden Texte automatisch heraus und ersetzt die Keys, die im Quellcode verwendet wurden. Leider beherrscht Xcode jedoch nicht die Möglichkeit neue Keys, die im Quellcode bei der Programmierung hinzugefügt wurden zu erkennen und diese automatisch in allen Sprachen den `.strings`-Dateien hinzuzufügen.

### Lösungsvorschlag

TODO: Am besten wäre eine Lösung, die vollständig in ein Open Source Tool integriert ist (BartyCrouch um Scan erweitern?)

Aktuell ist uns keine perfekte Lösung für dieses Problem bekannt. In der Praxis bewährt hat sich jedoch eine Kombination aus der nicht kostenfreien App [Linguan](http://linguanapp.com) und der automatischen Übersetzungs-Funktion vom Open-Source Tool [BartyCrouch](https://github.com/Flinesoft/BartyCrouch).

Mit Linguan durchsucht man dabei nach Hinzufügen von neuen Keys im Code das gesamte Projekt nach neuen Keys ab ("Scan sources") und trägt sogleich die korrekte Übersetzung in einer Quellsprache ab (z.B. Deutsch oder Englisch) – Linguan ergänzt jedoch nur `.strings`-Dateien mit den neuen Keys, für deren Sprache man eine Übersetzung anbietet, hier kommt BartyCrouch ins Spiel.

Ist BartyCrouch [richtig konfiguriert](https://github.com/Flinesoft/BartyCrouch#build-script) so braucht man nur noch das Projekt in Xcode öffnen und zu builden. Beim Build wird durch das konfigurierte BartyCrouch-Skript die Quellübersetzung gelesen, maschinell in alle definierten anderen Sprachen übersetzt und alle übersetzten `Localizable.strings`-Dateien mit den neuen Keys befüllt. Diese Vorgehensweise löst das Problem und hat auch den netten Nebeneffekt, dass die Übersetzungen später schneller gehen, da viele der kürzeren maschinellen Übersetzungen meist schon brauchbar sind.


## Unstatisch referenzierte Übersetzungen

Bei der Entwicklung mit Xcode gibt es die Möglichkeit mithilfe des in der `Foundation` Library definierten Makros `NSLocalizedString(key,comment)` Texte programmatisch zu lokalisieren. Dies ermöglicht das Anbieten einer App in vielen verschiedenen Programmiersprachen, deren Übersetzungen typischerweise zusammengefasst sind in der Datei `Localizable.strings`, welche für jede Sprache eine eigene Version hat.

## Unstatisch referenzierte Bilder

## Unstatisch referenzierte Interfaces

## Unstatisch referenzierte Farben

## Fehlende Code Conventions und TODO-Warnungen

## Manuelle Erstellung verschiedener Bilder-Auflösungen

## Fehlende Aktualisierung von Interface-Übersetzungen
