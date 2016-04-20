---
layout: post
title:  "Fülle die Lücken in Xcode"
categories: ios
---

In diesem Artikel geht es darum gewisse Schwächen von Xcode mithilfe von Tools und Programmierlösungen auszugleichen. Hierzu wird für jedes Thema zunächst das Problem mit Xcode und deren mögliche Folgen in der Praxis erläutert, woraufhin ein Lösungsvorschlag gegeben wird.

## Fehlende Erkennung von neuen Übersetzungs-Keys

### Problembeschreibung

Bei der Entwicklung mit Xcode gibt es die Möglichkeit mithilfe des in der `Foundation` Library definierten Makros `NSLocalizedString(key, comment)` Texte programmatisch zu lokalisieren. Dies ermöglicht das Anbieten einer App in vielen verschiedenen Programmiersprachen, deren Übersetzungen typischerweise zusammengefasst sind in der Datei `Localizable.strings`, welche für jede Sprache eine eigene Version hat. Dabei wird im Quellcode lediglich ein Key verwendet, welcher in den `.strings`-Dateien die jeweiligen Texte in den jeweiligen Sprachen zugewiesen werden. Die App sucht sich dann auf dem Endgerät je nach Geräteeinstellung die passenden Texte automatisch heraus und ersetzt die Keys, die im Quellcode verwendet wurden. Leider beherrscht Xcode jedoch nicht die Möglichkeit neue Keys, die im Quellcode bei der Programmierung hinzugefügt wurden zu erkennen und diese automatisch in allen Sprachen den `.strings`-Dateien hinzuzufügen.

### Lösungsvorschlag

TODO: Am besten wäre eine Lösung, die vollständig in ein Open Source Tool integriert ist (BartyCrouch um Scan erweitern?)

Aktuell ist uns keine perfekte Lösung für dieses Problem bekannt. In der Praxis bewährt hat sich jedoch eine Kombination aus der nicht kostenfreien App [Linguan](http://linguanapp.com) und der automatischen Übersetzungs-Funktion vom Open-Source Tool [BartyCrouch](https://github.com/Flinesoft/BartyCrouch).

Mit Linguan durchsucht man dabei nach Hinzufügen von neuen Keys im Code das gesamte Projekt nach neuen Keys ab ("Scan sources") und trägt sogleich die korrekte Übersetzung in einer Quellsprache ab (z.B. Deutsch oder Englisch) – Linguan ergänzt jedoch nur `.strings`-Dateien mit den neuen Keys, für deren Sprache man eine Übersetzung anbietet, weshalb für die Komplettlokalisierung in allen Sprachen BartyCrouch ins Spiel kommt.

Ist BartyCrouch [richtig konfiguriert](https://github.com/Flinesoft/BartyCrouch#build-script) (nutze die Translate-Funktion mit der `Localizable.strings`-Datei) so braucht man nur noch das Projekt in Xcode zu öffnen und zu builden. Beim Build wird durch das konfigurierte BartyCrouch-Skript die Quellübersetzung gelesen, maschinell in alle definierten anderen Sprachen übersetzt und alle übersetzten `Localizable.strings`-Dateien mit den neuen Keys befüllt. Diese Vorgehensweise löst das beschriebene Problem und hat auch den netten Nebeneffekt, dass die Übersetzungen später schneller gehen, da viele der kürzeren maschinellen Übersetzungen meist sogar brauchbar sind.


## Unstatisch referenzierte Übersetzungen

### Problembeschreibung

Wie weiter oben bereits erläutert bietet die `Foundation` Library das Makro `NSLocalizedString(key, comment)` um Texte in Xcode-Projekten zu übersetzen. Der `key` ist dabei vom Typ `String`, was den großen Nachteil hat, dass alle Vorteile von statischer Typisierung beim Builden von Code wegfallen. Das bedeutet konkret, dass Xcode nicht automatisiert überprüfen kann, ob zu den Werten, die als Keys im Code genutzt werden auch tatsächlich Übersetzungen existieren. So kann ein Vertipper, eine Änderung des Keys im Code oder fehlende Übersetzungen zu neuen Keys zu dem Problem führen, dass am Ende leere Texte in der App angezeigt werden und dies beim Entwickeln und builden gar nicht auffällt.

### Lösungsvorschlag

Wie für Code gibt es auch für Übersetzungen mithilfe von Tools die Möglichkeit eine Überprüfung ihrer Existens durchzuführen und ein Build fehlschlagen zu lassen, wenn ein Vertipper vorkommt oder eine Übersetzung komplett fehlt. Das Open Source Tool [Laurine](https://github.com/JiriTrecak/Laurine) untersucht nämlich automatisch alle `.strings`-Dateien auf ihre Keys und generiert aus diesen eine Code-Struktur namens `Localizations` worin sämtliche Keys per Dot-Syntax erreichbar sind und als Ergebnis die jeweilige Übersetzung liefern. Dies ermöglicht es alles Aufrufe von `NSLocalizedString(key, comment)` durch Aufrufe wie `Localizations.KeyName` zu ersetzen. Dies löst zum Einen das Problem, da Xcode bereits automatisch Code auf Existenz prüft, wenn er kompiliert wird und bietet als Nebeneffekt auch den Vorteil, dass man für sämtliche existierende Keys nun auch die Autovervollständigung nutzen kann, was bei reinen Strings nicht möglich war.

Zusätzlich zur Verwendung von Laurine sei an dieser Stelle noch eine Übersetzungs-Key-Struktur empfohlen, die die Autovervollständigungsfunktion maximal ausreizt, Übersetzungskommentare als Kontext-Informanten überflüssig macht und das Unterscheiden von Übersetzungs-Keys und tatsächlichen Übersetzungen vereinfacht. Die Struktur sollte folgende Regeln einhalten:

* Die Keys werden KOMPLETT IN GROSSBUCHSTABEN GESCHRIEBEN
* Die Keys werden hierarchisch.mit.Punkten.getrennt
* Trennungen zwischen Wörtern in den Keys werden mit_Unterschtrichen_umgesetzt

Nachfolgend seien ein paar Beispiel-Keys aufgelistet, die diese Vorgaben einhalten:

```swift
SETTINGS.TITLE
SETTINGS.FONT_SECTION.SIZE
SETTINGS.FONT_SECTION.COLOR
MODEL.ARTICLE.TITLE
MODEL.ARTICLE.RELEASE_DATE
```

In Laurine können sie dann folgendermaßen verwendet werden:

```swift
self.title = Localizations.Settings.Title
articleCell.titleLabel.text = Localizations.Model.Article.Title
```

## Unstatisch referenzierte Bilder

## Unstatisch referenzierte Interfaces

## Unstatisch referenzierte Farben

## Fehlende Code Conventions und TODO-Warnungen

## Manuelle Erstellung verschiedener Bilder-Auflösungen

## Fehlende Aktualisierung von Interface-Übersetzungen
