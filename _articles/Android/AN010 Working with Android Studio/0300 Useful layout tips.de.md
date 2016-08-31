---
section:    Android
topicid:    AN010
refid:      AN010-0300
permalink:  /:language/articles/AN010-0300.html
title:      Tipps zum Erstellen von Layouts
date:       2016-01-01 00:00:00
author:     Marina Meier
language:   de

prev_refid: AN010-0250
next_refid: AN010-0400
---

## Android Layout

### Unterschied layout_gravity und gravity


* **gravity** (*inside gravity*) beschreibt die Position des Inhalts innerhalb eines View Elements. Zum Beispiel die
Anordnung eines Textes in einer View (left, right, center). Dieses Attribut kommt nicht zum Tragen wenn für layout_width
oder layout_height *wrap_content* verwendet wurde, da so kein Raum entsteht in welchem das Element positioniert werden
könnte.

* **layout_gravity** (*outside gravity*) beschreibt die Position eines 'child'-View in seiner 'parent'-View. Wie ist das
Element relativ zu dem Element angeordnet in dem es definiert ist.

![Bild zur
Verdeutlichung(layout_gravity-gravity-example)](../../../BestPractices/public/images/AN010/0300/layout_gravity-gravity-example.png)
*Veranschaulichung von gravity und layout_gravity*


### Unterschied include und merge

`<include/>` - Tags werden dazu verwendet um einmal defnierte Layout Teile in andere Layouts einzufügen. Vorteile sind
zum Einen, dass die XML Dateien übersichtlicher werden und zum Anderen, dass bei mehrfachem verwenden des gleichen
Layouts (zum Beispiel einer Toolbar) Änderungen nur an einer Stelle gemacht werden müssen.

![include_layout.xml](../../../BestPractices/public/images/AN010/0300/include_layout_xml.png)
*Layout, das in ein anderes eingefügt werden soll.*

![main_layout_with_include](../../../BestPractices/public/images/AN010/0300/main_layout_with_include.png)
*Das Layout wird durch das `<include/>` - Tag eingefügt und enthält somit das neue Layout.*

Möchte man mehrere Elemente außerhalb des main_layouts definieren, müssen diese in ein Root-Element gepackt werden. In
diesem Beispiel ist das in der include_layout.xml Datei das vertical LinearLayout. Möchte man diese Elemente in einer
Datei in ein vertical LinearLayout einfügen kommt es zu Redundanzen. Das `<merge/>` - Tag hilft dabei redundante View
Groups in der View - Hierachie zu entfernen.

![main_layout_include_bad_example](../../../BestPractices/public/images/AN010/0300/main_layout_include_bad_example.png)
*In diesem Beispiel ist das Root-Element in beiden Layouts ein **vertical LinearLayout**.*

Durch diese Schachtelung wird die Performance der UI verlangsamt. Um dies zu vermeiden kann man statt einer View Group
als Root-Element, ein `<merge/>`- Element verwenden.

![merge_example](../../../BestPractices/public/images/AN010/0300/merge_example.png)
*In diesem Beispiel wurde anstatt einem vertical LinearLayout als Root-Element das `<merge/>`- Element verwendet. Das
System ignoriert die `<merge/>`- Root und plaziert beide Elemente direkt im Layout.*

Weitere Informationen: [Re-using Layouts with
include](http://developer.android.com/training/improving-layouts/reusing-layouts.html)

### CoordinatorLayout

Google hat im Jahr 2015 eine neue **Android Design Support Library** vorgestellt, welche es Entwicklern einfach machen
soll, das Material Design von Google umzusetzen. Dabei geht es darum Abhängigkeiten zwischen *child-views* zu
koordinieren.
Mit wenigen Zeilen Code kann man ein Layout mit **Floating Action Buttons**, **Snackbars** oder **Collapsing Toolbars**
erstellen. In diesem Abschnitt möchte ich kurz auf die Möglichkeiten eingehen, die diese Library bietet und eine Seite
verlinken, die ausführlich beschreibt wie das Ganze zu implementieren ist.

#### Floating Action Button und Snackbar

![video fab]()

Bei einem Klick auf den FAB, wird dieser nach oben geschoben und die Snackbar wird sichtbar.
Die Snackbar kann man mit einem Toast vergleichen. Sie ist aber ein Teil des Layouts und liegt nicht darüber.

Zwei Interessante Attribute von **FAB** dabei sind layout_anchor und layout_anchorGravity.

* **layout_anchor** beschreibt die View, an welchem der Button ausgerichtet wird. Ist der Button an einem Element einer
Liste festgemacht, und dieses Element verschwindet beim Scollen nach unten  aus dem Screen, positioniert sich der Button
am unteren Rand des Screens und schwebt mit.
`layout_anchor="@id/example"`
* **layout_anchorGravity** beschreibt an welchem Rand der View der Button positioniert sein soll.
`layout_anchorGravity="right|bottom" `

#### CollapsingToolbar

![video toolbar]()

Die Library unterstützt auch verschiedene Scroll - Techniken. Dabei geht es um das Verhalten der Toolbar, wenn bestimmte
Inhalte gescrollt werden. Mithilfe von Scrollflags kann festgelegt werden, wie Elemente beim Scrollen den Screen
verlassen oder betreten.

#### Implementierungsbeispiel

[Tutorial zum Benutzen der Android Design Support
Library](https://inthecheesefactory.com/blog/android-design-support-library-codelab/en)
