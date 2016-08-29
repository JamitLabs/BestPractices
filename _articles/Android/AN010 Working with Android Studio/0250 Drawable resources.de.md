---
section:    Android
topic:      Working with Android Studio
refid:      AN010-0250
permalink:  /articles/AN010-0250.html
title:      Drawable Ressourcen
date:       2016-01-01 00:00:00
author:     Marina Meier
---

Im Verzeichnis ***drawable*** werden verschiedene Arten von Graphiken abgelegt.

* **Bitmap File:** Graphik Datei (.png, .jpg, .gif)
* **Nine-Patch File:** .png Datei, die strechbare Regionen besitzt, welche es möglich
machen, dass das Bild je nach Inhalt an die Größe angepasst werden kann.
* **Layer List:** Array aus anderen Drawables.
* **State List:** XML Datei, welche verschiedene Graphiken für verschiedene Zustände defininert.
* **Level List:** XML Datei, welche ein Drawable definiert, welches eine Anzahl von Ersatz Drawables managed.
* **Transition Drawable:** XML Datei, welche ein Drawable, das zwischen zwei Drawables faden kann, defininert.
* **Inset Drawable:** XML Datei, welche ein Drawable, das ein anderes Drawable mit einem bestimmten Abstand einsetzt,
definiert.
* **Clip Drawable:** XML Datei, welche ein Drawable, das die ein anderes Drawables abschneidet, definiert.
* **Scale Drawable:** XML Datei, welche ein Drawable, das die Größe eines anderen Drawables verändert, definiert.
* **Shape Drawable:** XML Datei, welche eine geometrische Figur mit Farben und Verläufen definiert.

> Wichtig! Direkt im Drawables Verzeichniss sind nur XML files abzulegen. Bilder und Icons sollten sich in
auflösungsspezifischen Ordnern befinden. So ist es übersichtlicher und einfacher zu warten.

### Bilder für unterschiedliche Pixeldichten anlegen

Android läuft auf den unterschiedlichsten Geräten, deren Screengrößen und Pixeldichten stark variieren. Daher ist es
wichtig, dass die Auflösungen von Bildern oder Icons auf diese Geräte abgestimmt sind. Nur so kann man eine positive
Nutzererfahrung garantieren.

Android skaliert UI Layouts und Ressourcen zur Laufzeit. Um Bilder oder Icons in der richtigen Auflösung anzuzeigen,
sollten diese in verschiedenen Auflösungen im Projekt angelegt sein. So entscheidet sich Android, je nach Eigenschaften
des Gerätes, welches Bild oder Icon mit welcher Dichte angezeigt werden soll.

Diese sechs Pixeldichten können von Android unterstützt werden.

* ldpi (low) ~120dpi
* mdpi (medium) ~160dpi
* hdpi (high) ~240dpi
* xhdpi (extra-high) ~320dpi
* xxhdpi (extra-extra-high) ~480dpi
* xxxhdpi (extra-extra-extra-high) ~640dpi

![icon_density_example](../../../BestPractices/public/images/AN010/0250/icon_density_example.png)
*Beispiel für ein Icon in verschiedenen Auflösungen*

Beim Anlegen der Bilder in diesen Dichten muss die vorgegebene Ordnerstruktur eingehalten werden, damit das passende
Bild zur Laufzeit ausgewählt werden kann. Die Bennenung der Ordner, in denen die Bilder abgelegt werden müssen, ist wie
folgt: `drawable-hdpi, drawable-mdpi, drawable-xhdpi, ... `.

![density_exaxmple](../../../BestPractices/public/images/AN010/0250/density_example.png)
*Beispiel für die Ordnerstruktur*

Möchte man Bilder oder Icons bewusst nur in einer Auflösung anlegen, werden diese im Ordner `drawable-nodpi`abgelegt.

Stellt man ein Bild nicht in verschiedenen Auflösungen zur Verfügung, wird es von Android skaliert. Jedoch sollte man
sich nicht auf das Skalieren von Android verlassen, da die Bilder oft verzerrt, grisselig oder ausgefranst werden.

Um Icons in verschiedenen Auflösungen anzulegen ist [diese
Seite](https://romannurik.github.io/AndroidAssetStudio/icons-generic.html#source.space.trim=1&source.space.pad=0&size=99&padding=0&color=33b5e5%2C0&name=ic_dsc_6895)
zu empfehlen. Hier kann man das gewünschte Icon auswählen und die verschiedenen Auflösungen downloaden. Es können auch
weitere Einstellungen vorgenommen werden, wie zum Beispiel die Größe des Icons zu verändern.

Weitere Informationen: [Supporting Multiple
Screens](http://developer.android.com/guide/practices/screens_support.html#xxxhdpi-note)

### Vectorgrafiken

Es gibt auch die Möglichkeit Vectorgrafiken als Ressourcen in Android Studio anzulegen. Diese machen vor allem bei
einfachen Icons Sinn. Auch können sie die Größe der App reduzieren, da nicht Bilder in mehreren Auflösungen angelegt
werden müssen, sondern nur eine Datei, die sich der Auflösung entsprechend berechnet.

Um Vektorgrafiken zu erzeugen gibt es ein Tool in Android Studio, das [Vector Asset
Studio](http://developer.android.com/tools/help/vector-asset-studio.html).

Vectorgrafiken werden erst ab API Level 21 unterstützt. Alternativen für niedrigere Level sind einmal die Support
Library zu benutzten oder die Icons als Rastergrafiken von Vector Asset Studio generieren zu lassen.


### Nine-Patch Files

Ein Nine-Patch ist eine PNG Bild, welches sich je nach Größe des zur Verfügung stehenden Platzes anpasst. Ein Beispiel
wäre eine Box mit Schatten. Dabei soll sich die Größe der Box verändern können, der Schatten jedoch sollte nicht
verzogen werden. Bilder die sich sich für Nine Patch Dateien eignen, sollten aus streckbaren Flächen bestehen.

![nine_patch_example](../../../BestPractices/public/images/AN010/0250/nine_patch_example.png)

![nine_patch_example_two](../../../BestPractices/public/images/AN010/0250/nine_patch_example_two.png)

![nine_patch_example_three](../../../BestPractices/public/images/AN010/0250/nine_patch_example_three.png)
*Beispiel für eine Nine-Patch Datei*

Ändert sich nun der Platz, der dem Bild zur Verfügung steht, so wird die Datei diesem angepasst. Der grüne Inhalt wird
vergrößert oder verkleinert, der Schatten verändert sich jedoch nicht.

![nine_patch_bad_example](../../../BestPractices/public/images/AN010/0250/nine_patch_bad_example.png)
*Schlechtes Beispiel Nine Patch Datei*

![nine_patch_bad_example_two](../../../BestPractices/public/images/AN010/0250/nine_patch_bad_example_two.png)
*Schlechtes Beispiel Nine Patch Datei*

Nicht geeignet für Nine-Patch Dateien sind Photos oder Bilder mit Texten, da diese nicht streckbar sind und verzogen
werden.

Um Nine-Patch PNGs zu erzeugen ist [diese Seite](https://romannurik.github.io/AndroidAssetStudio/nine-patches.html) zu
empfehlen. Hier kann man in der *Interactive Preview* testen wie sich die Datei bei Veränderung der Größe verhält.
Außerdem bietet sie die Möglichkeit, die Dateien direkt herunterzuladen. Auch diese Dateien sollten in verschiedenen
Auflösungen integriert werden.

Zu erkennen sind Nine-Patch Dateien an der Endung: `.9.png`.

Weitere Informationen:
[Nine-Patch](http://developer.android.com/guide/topics/resources/drawable-resource.html#NinePatch)
