---
section:  Android
topic:    Working with Android Studio
refid:    AN010-0200
title:    Android Studio Projektstruktur
date:     2016-01-01 00:00:00
author:   Marina Meier
---

In diesem Artikel wird ein sinnvoller Aufbau für ein Android Studio Projekt
dargestellt. Dabei geht es um den von Android vorgebenen Aufbau der ```res``` - Dateien
und ein Beispiel für die frei wählbare Struktur der ```java``` - Dateien.

## Anlegen der `res`(Ressourcen) - Ordner

Im Ressourcen - Verzeichniss werden Inhalte wie zum Beispiel Texte, Bilder, Layouts oder Animationen
angelegt. Die Plazierung außerhalb des Codes ermöglicht es die Daten unabhängig davon
bearbeiten zu können.

![start_structure](../../../public/images/AN010/0200/start_structure.png)

In diesem Beispiel kann man die Unterverzeichnisse sehen, die für ein einfaches Projekt
benötigt werden. Die Struktur und Benennung der Verzeichnisse ist von Android vorgegeben.
Innerhalb des ```res``` -Ordners dürfen sich nur Unterverzeichnisse befinden und keine
einfachen Dateien. Auch die Benennung der Unterverzeichnisse ist fest vorgegeben,
da Android sich zur Laufzeit genau die Dateien aus den Ordnern heraus greift, welche
am Besten zu den Eigenschaften des jeweiligen Gerätes passen.

Im Folgenden eine Auflistung der Default Ressourcen.

 * animator
 * anim
 * color
 * drawable
 * mipmap
 * layout
 * menu
 * raw
 * values
 * xml

*Auf den Inhalt der einzelnen Ordner wird später eingegangen.*

### Spezialisierungen

 Zusätzlich zu den Default Ressourcen gibt es Spezialisierungen, welche auf genauere
 Geräteeigenschaften eingehen. Zum Beispiel könnte man veschiedene Layouts für verschiedene
 Screengrößen anlegen oder man möchte, dass ein anderes Layout verwendet wird, wenn der
 Benutzer der App in den Landscape Modus wechselt. Für diese Spezialisierungen müssen
 neue Verzeichnisse im ```res``` -Ordner angelegt werden.

 ![Beispiel für ein res Verzwichniss mit Spezialisierungsordnern]()

Folgende Regeln sind dabei zu Beachten:

* **Die Verzeichnisse dürfen nicht geschachtelt werden**

  Richtig:

  ```
   res/
      drawable/
      drawable-hdpi/
      layout/
  ```
  Falsch:  

  ```
  res/
     drawable/
         drawable-hdpi/
     layout/
  ```


* **Die Benennung folgt diesem Schema**

  ```
  <resources_name>-<qualifier>
  ```
  * **resources_name** ist der Name der Default - Ressource auf die sich die Spezialisierungen bezieht.
  * **qualifier** ist der Name der Spezialisierung für welche die Ressource benutzt werden soll.


* **Beim Anlegen der Verzeichnisse muss eine bestimmte Reihenfolge eingehalten werden**

  Da Android sich die zu den Geräteeigenschaften passenden Dateien heraussucht, würden manche Dateien
  bei falscher Anordnung ignoriert werden.

  Richtig:

  ```
  res/
      layout/
      layout-en/
      layout-fr/
      layout-en-rUS/
      layout-fr-fFR/
  ```
  Falsch:

  ```
  res/
      layout/
      layout-en-rUS/
      layout-fr/
      layout-en/
      layout-fr-fFR/

  ```
* **Beim Anhängen von mehreren Spezialisierungen muss eine bestimmte Reihenfolge eingehalten werden.**

 Auf [dieser Seite](http://developer.android.com/guide/topics/resources/providing-resources.html)
 in Tabelle 2 sind die Reihenfolge der Verzeichnisse und aller möglichen Spezialisierungen angegeben.

* **Ein Verzeichnis kann nur eine Spezialisierung eines Typs bekommen**

  Richtig:

  ```
  drawable-rES/
  drawable-rFR/
  ```

  Falsch:

  ```
  drawable-rES-rFR/
  ```

  Möchte man jedoch die gleiche Datei in beiden Ordnern ablegen, kann sie auch außerhalb definiert werden.


Android sucht nach der am Besten zu den Geräteeigenschaften passenden Spezialisierung.
Falls keine passende gefunden werden kann, wird auf die Default Werte zugegriffen.
Deshalb ist es wichtig Default Ressourcen anzubieten, sonst stürzt die App ab.
Genauso ist es wichtig, dass die Dateien in allen Spezialisierungen, sowie dem
Default Ordner, gleich benannt sind, da die Ressourcen im Code durch IDs angesprochen
werden.


### Unterschied von Animationen im ```animator``` - Verzeichnis und im ```anim ```- Verzeichniss

In das Verzeichnis ```anim``` gehören **tween animations**.


Bei diesen handelt es sich um die **Standard Animationen** und kann nur auf Views
angewendet werden. Möglich ist dabei nur die Veränderung der Position (translation),
die Veränderung der Größe (scale), der Rotation (rotation) und der Durchsichtigkeit (alpha)
der View. Die Implementierung und der Aufbau ist relativ einfach. Bei der Animation
wird jedoch nicht das tatsächliche Objekt angepasst, sondern nur der Aubau der View.
Das kann zum Problem werden, wenn die View klickbar sein soll, da der klickbare
Bereich während der Animation nicht aktualisiert wird.

Weitere Informationen: [Android View Animations](http://developer.android.com/guide/topics/graphics/view-animation.html#tween-animation)


In das Verzeichnis ```animator``` gehören **property animations**.


Bei diesen Animationen werden die Eigenschaften eines Objects über einen bestimmten
Zeitraum verändert. In diesem Fall werden die Objekte direkt modifiziert und auch der
klickbare Bereich würde am Objekt haften, da die Eigenschaften des Zielobjects in Echtzeit
aktualisiert werden. Diese Art von Animation kann für alle Arten von Objekten verwendet werden,
nicht nur für Views, und sind insgesamt generischer und flexibler.

Weitere Informationen: [Android Property Animation](http://developer.android.com/guide/topics/graphics/prop-animation.html)

### Verzeichnis ```color```

Im Verzeichnis ***color*** werden xml-Dateien angelegt, die eine **Color State List**
beinhalten. Diese ist ein Objekt, welches man wie eine **color** verwenden kann.
Hier kann man eine Liste von verschiedenen Farben für verschiedene Zustände definieren,
wie zum Beispiel *gedrückt*, *im Fokus*, *normal*.
Wird diese Liste benutzt, werden diese Farben je nach Zustand eingesetzt.

Weitere Informationen: [Color State List Resource](http://developer.android.com/guide/topics/resources/color-list-resource.html)


### Verzeichnis ```drawable```

Im Verzeichnis ***drawable*** werden verschiedene Arten von Graphiken abgelegt.

* **Bitmap File:** Graphik Datei (.png, .jpg, .gif)
* **Nine-Patch File:** .png Datei, die strechbare Regionen besitzt, welche es möglich
machen, dass das Bild je nach Inhalt an die Größe angepasst werden kann.  
* **Layer List:** Array aus anderen Drawables.
* **State List:** XML Datei, welche verschiedene Graphiken für verschiedene Zustände defininert.
* **Level List:** XML Datei, welche ein Drawable definiert, welches eine Anzahl von Ersatz Drawables managed.
* **Transition Drawable:** XML Datei, welche ein Drawable, das zwischen zwei Drawables faden kann, defininert.
* **Inset Drawable:** XML Datei, welche ein Drawable, das ein anderes Drawable mit einem bestimmten Abstand einsetzt, definiert.
* **Clip Drawable:** XML Datei, welche ein Drawable, das die ein anderes Drawables abschneidet, definiert.
* **Scale Drawable:** XML Datei, welche ein Drawable, das die Größe eines anderen Drawables verändert, definiert.
* **Shape Drawable:** XML Datei, welche eine geometrische Figur mit Farben und Verläufen definiert.

> Wichtig! Direkt im Drawables Verzeichniss sind nur XML files abzulegen. Bilder und Icons sollten sich in auflösungsspezifischen Ordnern befinden.

#### Bilder für unterschiedliche Pixeldichten anlegen.

Da Android auf vielen unterschiedlichen Geräten mit verschiedenen Pixeldichten läuft,
ist es wichtig, dass die Auflösung von Bildern oder Icons auf diese Größe abgestimmt ist.
Nur so kann man eine positive Nutzererfahrung garantieren.

Es gibt sechs allgmeine Pixeldichten. Sind Bilder oder Icons in unterschiedlichen Dichten angelegt,
entscheidet Android je nach Eigenschaften des Gerätes welches Bild oder Icon mit welcher
Dichte angezeigt werden soll.

* ldpi (low) ~120dpi
* mdpi (medium) ~160dpi
* hdpi (high) ~240dpi
* xhdpi (extra-high) ~320dpi
* xxhdpi (extra-extra-high) ~480dpi
* xxxhdpi (extra-extra-extra-high) ~640dpi

Damit Android das richtige Bild auswählen kann, muss die vorgegebene Ordnerstruktur stimmen.
Zusätzlich zu dem Default Drawable Ordner müssen sich mindestens drei weitere Ordner
in dem ```res```-Verzeichnis befinden. Die Bennenung ist wie folgt: ```drawable-hdpi, drawable-mdpi, ... ```.
Diese Ordner werden in Android Studio erstmal nicht angezeigt. Erst wenn man Bilder
oder Icons zu den einzelnen Ordnern hinzufügt, erscheint *unten gezeigte* Struktur.
Jedes Bild oder Icon sollte dabei in allen Ordnern den gleichen Namen tragen.

![density_exaxmple](../../../public/images/AN010/0200/density_example.png)

Die verschiedenen Auflösungen berechnen sich wie folgt basierend auf der **medium density (mdpi)** :

* ldpi (0,75x)
* mdpi (1,0x baseline)
* hdpiv (1,5x)
* xhdpi (2,0x)
* xxhdpi (3,0x)
* xxxhdpi (4,0x)

Man kann sich die Bilder in verschiedenen Auflösungen auch auf [dieser Seite](https://romannurik.github.io/AndroidAssetStudio/icons-generic.html#source.space.trim=1&source.space.pad=0&size=99&padding=0&color=33b5e5%2C0&name=ic_dsc_6895)
berechnen lassen. Nach dem Auswählen eines Bildes oder Icons, können noch weitere
Einstellungen getroffen werden, wie zum Beispiel Anpassungen der Größe oder des Paddings.
Hier kann man sich auch die generierten Dateien herunterladen. Diese befinden sich
in der oben geschriebenen Ordnerstruktur, müssen jedoch in das Android Studio Projekt kopiert werden.


Möchte man Bilder oder Icons einsetzten, die nur in einer Auflösung vorkommen sollen,
werden diese im Ordner ```drawable-nodpi``` abgelegt.

Weitere Informationen: [Supporting Multiple Screens](http://developer.android.com/guide/practices/screens_support.html#xxxhdpi-note)


#### Nine-Patch Files

Ein Nine-Patch ist eine PNG Bild, welches sich je nach Größe des zur Verfügung
stehenden Platzes anpasst. Typischerweise verwendet man so ein Bild als Hintergrund,
wenn die Höhe oder die Breite des Bildes an die Bildschirmgröße angepasst werden soll.
Dazu sollte das Bild jedoch strechbare Flächen besitzen, die erweitert werden können.

![nine_patch_example](../../../public/images/AN010/0200/nine_patch_example.png)
*Gutes Beispiel Nine Patch Datei*

In diesem Beispiel möchte man als Hintergrund einen roten Rahmen mit weißem Inhalt verwenden.
Der Text ist nicht Teil des Bildes, sondern soll den Inhalt der über dem Hintergrund liegt veranschaulichen.
Die weiße Fläche im Inneren und auch der Rahmen sind strechbare Flächen.


![nine_patch_example_two](../../../public/images/AN010/0200/nine_patch_example_two.png)
*Weiteres Beispiel*

![nine_patch_example_three](../../../public/images/AN010/0200/nine_patch_example_three.png)
*Weiteres Beispiel*

Ändert sich nun der Platz, der dem Bild zur Verfügung steht, so wird die Datei diesem angepasst.
Der innere weiße Inhalt wird vergrößert oder verkleinert und auch der Rahmen passt sich den Verhältnissen an.

![nine_patch_bad_example](../../../public/images/AN010/0200/nine_patch_bad_example.png)
*Schlechtes Beispiel Nine Patch Datei*

![nine_patch_bad_example_two](../../../public/images/AN010/0200/nine_patch_bad_example_two.png)
*Schlechtes Beispiel Nine Patch Datei*

Nicht geeignet für Nine-Patch Dateien sind Photos oder Bilder mit Texten, da diese
nicht strechbar sind und verzogen werden.

Um Nine-Patch PNGs zu erzeugen ist [diese Seite](https://romannurik.github.io/AndroidAssetStudio/nine-patches.html)
zu empfehlen. Hier kann man in der *Interactive Preview* testen wie sich die
Datei bei Veränderung der Größe verhält. Außerdem bietet sie die Möglichkeit,
die Dateien direkt herunterzuladen. Auch diese Datien sollten in verschiedenen
Auflösungen integriert werden.

Weitere Informationen: [Nine-Patch](http://developer.android.com/guide/topics/resources/drawable-resource.html#NinePatch)



### Verzeichnis ```mipmap```

Im Verzeichnis *mipmap* werden die Icons abgelegt, welche von Launcher Apps (Homescreen Replacements)
verwendet werden.

//TODO was genau bedeutet das?


### Verzeichnis ```layout```

Im Verzeichnis *layout* wird das Aussehen der Benutzeroberflächen festgelegt. Es gibt
zwei Möglichkeiten ein Layout zu definieren:

* **In XML Dateien**
* **Zur Laufzeit**

Man kann jede Variante für sich oder beide zusammen verwenden.
Zu empfehlen ist, das Grundlayout in den XML Dateien zu definieren. Diese können von
Android Studio visuell dargestellt werden und erleichtern das Erstellen und Bearbeiten.
Auch ist der Aufbau der Oberfläche klar von der Funktionsweise getrennt. Späteres modifizieren
ist somit einfacher und es besteht die Möglichkeit Layouts für unterschiedliche Screengrößen oder Ausrichtungen anzupassen.
Zur Laufzeit sollten dann Veränderungen der in den XML Dateien definerten Layouts erfolgen und Custom Views erstellt werden.



### Verzeichnis ```menu```

Im Verzeichnis *menu* werden die Optionsmenüs, Kontextmenüs und Untermenüs angelegt.

//TODO

### Verzeichnis ```raw```

//TODO

### Verzeichnis ```values```

Im Verzeichnis *values* werden Dateien angelegt, welche einen einfachen Wert besitzen,
wie zum Beispiel Strings, Integers oder Farben. Diese Dateien enthalten nicht nur eine Ressouce,
wie die anderen XML Dateien in den anderen Verzeichnissen, sondern können beliebig
viele Ressourcen enthalten.
Die Festlegung von Farben, Strings oder Dimensionen in diesen Dateien ist sinnvoll,
da sie so unabhängig vom Code geändert werden können. Möchte man zum Beispiel eine Accent
Farbe in einer App benutzen und diese wird fünfzig mal verwendet, so muss man sie,
falls man sich für eine andere Farbe entscheidet, nicht fünzig mal ändern, sondern
nur ein einziges Mal in der colors.xml.
Die Bennenug der Dateien folgt diesen Namenskonventionen:

#### arrays.xml
#### colors.xml

Ein **Color** Wert definiert eine Farbe und besteht aus einem RGB Wert und einem
Alphakanal. Die Color-Ressource kann überall dort verwendet werden, wo ein hexadezimal Wert akzeptiert wird.
Diese Format Möglichkeiten gibt es: #RGB, #ARGB, #RRGGBB, #AARRGGBB

```
<color name="color_name">#000000</color>
```

#### dimens.xml

Ein **Dimen** Wert definiert eine Dimension und besteht aus einer Nummer und
einer Einheit. Benutz werden können Dimen-Werte zum Beispiel für verschiedene
Abstände in Layouts oder Textgrößen.

Die folgenden Einheiten werden akzeptiert:

* **dp** (Density-independent Pixels) sind eine abstrakte Einheit, die auf der
Pixeldichte des Screens beruht. Auf einem 160 dpi screen ist 1 dp ungefähr gleich 1px.
Ist die Pixeldichte höher oder niedriger wird die Anzahl der Pixel, die einem dp
entsprechen, im Verhältnis angepasst.

* **sp** (Scale-independent Pixel) funktionieren gleich wie *dp*, kann jedoch aber
durch die Schriftgrößen Präferenzen des Benutzers angepasst werden. Daher ist es
sinnvoll diese Einheit für Schriftgrößen einzusetzten.

* **pt** (Points) 1/72 of an inch, basierend auf der physikalischen Größe des Screens.

* **px** (Pixel) ist relativ zu den Pixeln eines Screen. Dies ist nicht zu empfehlen,
da jedes Gerät eine andere Auflösung hat und die Darstellung somit sehr kompliziert wird.

* **mm** (Milimeter) ist relativ zur physikalischen Größe des Screens.
* **in** (Inches) ist relativ zru physikalischen Größe des Screens.

```
<dimen name="dimen_name">12sp</dimen>
```

#### strings.xml

Ein **String** Wert defininert ein Wort oder einen Text.

```
<string name="string_name">Hallo Welt!</string>
```

Es ist auch möglich String-Arrays zu definieren.

```
<string-array name="string_array_name">
    <item>Erstes Wort</item>
    <item>Zweites Wort</item>
    <item>Drittes Wort</item>
</string-array>
```

#### styles.xml

Ein **Style** Wert definiert das Format und das Aussehen für eine View oder eine
komplette Activity oder Applikation.






### Verzeichnis ```xml```

Im Verzeichniss *xml* werden beliebige XML Dateien abgelegt.
