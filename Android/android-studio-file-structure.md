---
layout: post
title: Android Studio Projekt Struktur
categories: android
---

In diesem Artikel wird ein sinnvoller Aufbau für ein Android Studio Projekt
dargestellt. Dabei geht es um den von Android vorgebenen Aufbau der ```res``` - Dateien
und ein Beispiel für die frei wählbare Struktur der ```java``` - Dateien.

## Anlegen der ```res ```(Ressourcen) - Ordner

Im Ressourcen - Verzeichniss werden Inhalte wie zum Beispiel Texte, Bilder, Layouts oder Animationen
angelegt. Die Plazierung außerhalb des Codes ermöglicht es die Daten unabhängig davon
bearbeiten zu können und aufwändiges Durchsuchen des Codes nach Inhalten zu vermeiden.

![start_structure]()

In diesem Beispiel kann man die Unterverzeichnisse sehen, die für ein einfaches Project
benötigt werden. Die Struktur und Benennung der Verzeichnisse ist von Android vorgegeben.
Innerhalb des ```res``` Ordners, dürfen sich nur Unterverzeichnisse befinden und keine
einfachen Dateien. Auch die Benennung der Unterverzeichnisse muss genauso übernommen werden,
da Android sich zur Laufzeit genau die Dateien aus den Ordnern heraus greift, welche
am Besten zu den Einstellungen des jeweiligen Geräts passen.

 Im Folgenden eine Auflistung der Default Ressourcen. Auf deren genauen Inhalte wird
 später eingegangen.

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

### Spezialisierungen

 Zusätzlich zu den Default Ressourcen gibt es Spezialisierungen, welche auf genauere
 Geräteeinstellungen eingehen. Zum Beispiel könnte man veschiedene Layouts für verschiedene Screen Größen anlegen oder man möchte, dass ein anderes Layout verwendet wird, wenn der Benutzer der App in den Landscape Modus wechselt. Für diese Spezialisierungen müssen neue Verzeichnisse im ```res```Ordner
 angelegt werden.

 ![]()

> Dabei ist zu Beachten, dass die Verzeichnisse nicht geschachtelt werden dürfen.
>
>
>```
> res/
>    drawable/
>        drawable-hdpi/
>    layout/
>    ```
>
> Die Spezialisierungen müssen sich im ```res ``` - Ordner befinden.
>
> ```
> res/
>    drawable/
>    drawable-hdpi/
>    layout/
> ```

Bei der Bennenung gilt folgende Regel:

```
<resources_name>-<qualifier>
```        

* **resources_name** ist der Name der Default - Ressource auf die sich die Spezialisierungen bezieht.
* **qualifier** ist der Name der Spezialisierung für welche die Ressource benutzt werden soll.

Die Reihenfolge in der die Spezialisierung angelegt werden spielt ein große Rolle, da
falls sie falsch gewählt wurde, Dateien ignoriert werden.

```
res/
    layout/
    layout-en/
    layout-fr/
    layout-en-rUS/
    layout-fr-fFR/
```
Das ist die richtige Reihenfolge.

```
res/
    layout/
    layout-en-rUS/
    layout-fr/
    layout-en/
    layout-fr-fFR/

```
Das wäre nicht korrekt, da Android






* ausgelagert können sie durch Resourcen IDs angesprochen werden. In R Klasse gespeichert.


* diese Beispiele sind die default resourcen, für spezielle Fälle wie größere Screengrößen
andere Sprachen sollten weitere Unterverzeichnisse angelegt werden.

* Aufbau < resource_name > - < config_qualifier >
* auch mehrere können angehängt werden, immer mit - getrennt, ABER: Reihenfolge beachten! da sonst ignoriert
* Die Dateien im speziellen Ordner müssen genauso benannt werden wie im default ordner
* Reihenfolge ist wichtig!!!
* qualifiers die erst von neueren versionen verwendet werden, denen wird automatisch
das version -v qualifiers angehängt

* mehrere qualifiers müssen auch in bestimmter Reihenfolge geordnet werden.
* Unterverzeichnisse können keine weiteren Unterverzeichnisse beinhalten
* jeder resource kann jeweils nur ein Typ von einem Qualifier zugeordnet werden.
nicht drawable-rES-rFR
* soll in beiden Ordner die gleiche Datei sein, kann man sie außerhalb definieren.
* Android finds the best matching resource
* if no match --> default resourcen

* Wichtig default resourcen anzubieten sonst stürzt die Applikation ab. Es ist immernoch besser im portrait angezeigt zu bekommen anstatt im landscape, als abzustürzen.
auch wenn qualifiers verwendet werden die zu neu für eine system version ist, kann die app abstürzen.
* Ausnahme: wenn min SDK = 4 braucht man keine default drawable  resource. android findet das passende in den screen density ordnern drawable - hdpi zum Beispiel. Jedoch sollte man
alle drei typen von densities angeben.


### Unterschied von Animationen im ```animator``` - Verzeichnis und im ```anim ```- Verzeichniss

In das Verzeichnis ```anim``` gehören **tween animations**.


Bei diesen handelt es sich um die **Standard Animation** und kann nur auf Views angewendet werden. Möglich ist dabei nur die Veränderung der Position (translation), die Veränderung der Größe (scale), die Rotation (rotation) und die Durchsichtigkeit (alpha) der View. Die Implementierung und der Aufbau ist relativ einfach. Bei der Animation wird jedoch nicht das tatsächliche Objekt
angepasst, sondern nur der Aubau der View. Das kann zum Problem werden, wenn die View klickbar sein soll, da der klickbare Bereich während der Animation nicht aktualisiert wird.

Weitere Informationen: [Android View Animations](http://developer.android.com/guide/topics/graphics/view-animation.html#tween-animation)


In das Verzeichnis ```animator``` gehören **property animations**.


Bei diesen Animationen werden die Eigenschaften eines Objects über einen bestimmten Zeitraum verändert. In diesem Fall werden jedoch
die Objekte direkt modifiziert und der klickbare Bereich würde mitwandern, da die Eigenschaften des Zielobjects in Echtzeit aktualisiert werden. Diese Art von Animation kann für alle Arten von Objecten verwendet werden und nicht nur für Views und sind insgesamt generischer und flexibler.

Weitere Informationen: [Android Property Animation](http://developer.android.com/guide/topics/graphics/prop-animation.html)

## Verzeichnis ```color```

Im Verzeichnis ***color*** werden xml-Dateien angelegt, die eine **Color State List**
beinhalten. Diese ist ein Objekt, welches man wie eine **color** verwenden kann.
Hier kann man eine Liste von verschiedenen Farben für verschiedene Zustände definieren,
wie zum Beispiel *gedrückt*, *im Fokus*, *normal*.
Wird diese Liste benutzt, werden diese Farben je nach Zustand eingesetzt.

Weitere Informationen: [Color State List Resource](http://developer.android.com/guide/topics/resources/color-list-resource.html)


## Verzeichnis ```drawable```

Im Verzeichnis ***drawable*** werden verschiedene Arten von Graphiken abgelegt.

* Bitmap File: Graphik Dateien (.png, .jpg, .gif)
* Nine-Patch File: .png Datei, die strechbare Regionen besitzt, welche es möglich machen, dass das Bild je nach Inhalt an die Größe angepasst werden kann.  
* Layer List: Array aus anderen Drawables.
* State List: XML Datei, welche verschiedene Graphiken für verschiedene Zustände defininert.
* Level List: XML Datei, welche ein Drawable definiert, welches eine Anzahl von Ersatz Drawables managed.
* Transition Drawable: XML Datei, welche ein Drawable, das zwischen zwei Drawables faden kann, defininert.
* Inset Drawable: XML Datei, welche ein Drawable, das ein anderes Drawable mit einem bestimmten Abstand einsetzt, definiert.
* Clip Drawable: XML Datei, welche ein Drawable, das die ein anderes Drawables abschneidet, definiert.
* Scale Drawable: XML Datei, welche ein Drawable, das die Größe eines anderen Drawables verändert, definiert.
* Shape Drawable: XML Datei, welche eine geometrische Figur mit Farben und Verläufen definiert.


### Nine-Patch Files

Ein Nine-Patch ist eine PNG Bild, welches sich je nach Größe des zur Verfügung stehenden Platzes anpasst.
Typischerweise verwendet man so ein Bild als Hintergrund, wenn die Höhe oder die Breite des Bildes an die Bildschirmgröße
angpassen soll. Dazu sollte das Bild jedoch strechbare Flächen besitzen, die sich anpassen können.

![nine_patch_example]()

In diesem Beispiel möchte man als Hintergrund einen roten Rahmen mit weißem Inhalt verwenden. Der Text ist nicht Teil des Bildes, sondern soll den Inhalt der über dem Hintergrund liegt veranschaulichen.  Dazu legt man eine PNG Datei an, die genanntes beinhaltet. Die weiße Fläche im Inneren und auch der Rahmen sind strechbare Flächen.


![]()

Ändert sich nun der Platz, der dem Bild zur Verfügung steht, so wird die Datei diesem angepasst. Der innere weiße Inhalt wird vergrößert oder verkleinert und auch der Rahmen passt sich den Verhältnissen an.


![nine_patch_bad_example]()

Nicht geeignet für Nine-Patch Dateien sind Photos oder Bilder mit Texten, da diese nicht strechbar sind und verzogen werden.

Um Nine-Patch PNGs zu erzeugen ist [diese Seite](https://romannurik.github.io/AndroidAssetStudio/nine-patches.html) zu empfehlen.
Hier kann man in der *Interactive Preview* testen wie sich die Datei bei Veränderung der Größe verhält. Außerdem bietet sie die Möglichkeit, die Dateien direkt herunterzuladen.

### Bilder für unterschiedliche Pixeldichten anlegen.

// TODO


## Verzeichnis ```mipmap```

Im Verzeichnis *mipmap* werden die Icons abgelegt, welche von Launcher Apps (Homescreen Replacements) verwendet werden.
//TODO was genau bedeutet das?


## Verzeichnis ```layout```

//TODO
## Verzeichnis ```menu```

Im Verzeichnis *menu* werden die Optionsmenüs, Kontextmenüs und das Untermenüs angelegt.

//TODO

## Verzeichnis ```raw```

//TODO

## Verzeichnis ```values```

Im Verzeichnis *values* werden Dateien angelegt, welche einen einfachen Wert besitzen, wie zum Beispiel Strings, Integers oder Farben. Diese Dateien enthalten nicht nur eine Ressouce, wie die anderen XML Dateien in den anderen Verzeichnissen, sondern können beliebig viele Ressourcen enthalten.
Die Festlegung von Farben, Strings oder Dimensionen in diesen Dateien ist sinnvoll, da sie so unabhängig vom Code geändert werden können. Möchte man zum Beispiel eine Accent Farbe in einer App benutzen und diese wird fünfzig mal verwendet, so muss man sie, falls man sich für eine andere Farbe entscheidet, nicht fünzig mal ändern, sondern nur ein einziges mal in der colors.xml.
Die Bennenug der Dateien folgt diesen Namenskonventionen:

#### arrays.xml
#### colors.xml

Ein **Color** Wert definiert eine Farbe und besteht aus einem RGB Wert und einem Alphakanal. Die Color-Ressource kann überall dort verwendet werden, wo ein hexadezimal Wert akzeptiert wird.
Diese Format Möglichkeiten gibt es: #RGB, #ARGB, #RRGGBB, #AARRGGBB

```
<color name="color_name">#000000</color>
```

#### dimens.xml

Ein **Dimen** Wert definiert eine Dimension und besteht aus einer Nummer und einer Einheit. Benutz werden können Dimen-Werte zum Beispiel für verschiedene Abstände in Layouts oder Textgrößen.

Die folgenden Einheiten werden akzeptiert:

* **dp** (Density-independent Pixels) sind eine abstrakte Einheit, die auf der Pixeldichte des Screens beruht. Auf einem 160 dpi screen ist 1 dp ungefähr gleich 1px. Ist die Pixeldichte höher oder niedriger wird die Anzahl der Pixel, die einem dp entsprechen, im Verhältnis angepasst.

* **sp** (Scale-independent Pixel) funktionieren gleich wie *dp*, kann jedoch aber durch die Schriftgrößen Präfernezen des Benutzers angepasst werden. Daher ist es sinnvoll diese Einheit für Schriftgrößen einzusetzten.

* **pt** (Points) 1/72 of an inch, basierend auf der physikalischen Größe des Screens.

* **px** (Pixel) ist relativ zu den Pixeln eines Screen. Dies ist nicht zu empfehlen, da jedes Gerät eine andere Auflösung hat und die Darstellung somit sehr kompliziert wird.

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

Ein **Style** Wert definiert das Format und das Aussehen für eine View oder eine komplette Activity oder Applikation.






## Verzeichnis ```xml```

Im Verzeichniss *xml* werden beliebige XML Dateien abgelegt.
