---
section:    Android
topic:      Working with Android Studio
refid:      AN010-0400
permalink:  /articles/AN010-0200.html
title:      Gute Navigationsstruktur
date:       2016-01-01 00:00:00
author:     Marina Meier
---


In diesem Artikel geht es um eine konsistente Navigationsstruktur. Für die Benutzer einer App ist es wichtig, dass sie schnell und ohne Überraschungen durch die App navigieren können. Um dieses zu gewährleisten, werden im Folgenden die Best Practises bei der Erstellung einer Navigationsstruktur betrachtet.


## BACK Navigation

![picture_back_button]()

Der Back-Button ist bei allen Android Geräten integriert und sollte nicht der UI hinzugefügt werden. Er wird benutzt um in umgekehrter chronologischer Reihenfolge durch die zuletzt besuchten Screens einer App zu navigieren. Des Weiteren kann das Klicken des Buttons dazu führen, dass man die App verlässt oder einen Dialog oder die Screen Tastatur schließt.
In den meisten Situationen pflegt das Android System selbst den Back Stack auf dem die einzelnen Screens abgelegt werden. In manchen Fällen muss das Back Verhalten jedoch manuell spezifiziert werden um die beste User Experience zu bieten.

Diese Situationen sind:

* Betreten einer Deep-Level Activity von einer Notification, eines App Widgets oder eines Navigation Drawers
* Navigation zwischen Fragmenten
* Navigation zwischen Webpages einer Webview**

#### Betreten einer Deep Level Activity

Eine Deep Level Activity bezeichnet eine Activity, die sich in der Screen Hierarchie an einer der unteren Stellen befindet, d.h. sie ist innerhalb der App nur über andere Aktivities zugänglich. Gelangt man nun über eine Notifikation oder ein App Widget zu so einem Screen, fehlt der vom System erzeugte Back Stack. Hier ist es sinnvoll den Benutzer über die komplette App Hierarchie bis hin zum ersten (“home”) Screen der App zu leiten, anstatt die App wieder zu schließen.
Dazu muss man mit Hilfe des `TaskStackBuilders` und dem `PendingIntent` alle Aktivities, die oberhalb dieser Activity in der Hierarchie liegen, dem Stack hinzugefügt werden.

#### Navigation zwischen Fragmenten

Fragmente müssen dem Back Stack auch manuell hinzugefügt werden. Wenn man die FragmentTransaktion zum Öffnen des Fragmentes aufruft, sollte man es daher mit `addToBackStack()` zum Stack hinzufügen. Wird nun der Back Button gedrückt, wird das zuletzt abgelegte Fragment oder die zuletzt abgelegte Activity aufgerufen.
Werden Fragmente innerhalb einer Hierarchie Ebene ausgetauscht, zum Beispiel bei horizontalen Transaktionen wie Tabs, müssen die Fragmente nicht zum Stack hinzugefügt werden.




## UP Navigation

![picture_up_button]()

Der Up Button befindet sich links oben in der Toolbar. Er sollte dazu benutzt werden um in einer App basierend auf hierarchischen Beziehungen zwischen Screens zu navigieren. Erreicht man den obersten Screen in der App Hierarchie, meist der Screen der gelaunched wird, gibt es keine Up Navigation mehr. Wichtig ist hierbei, dass der Benutzer auf jeden Fall in der eigenen App bleibt.

Jeder Screen, der nicht der “home” screen ist, sollte die Möglichkeit bieten zum übergeordneten (parent) Screen zu gelangen. Um diese Funktionalität zu erreichen muss man zuerst die Parent Activity im AndroidManifest spezifizieren. Dazu einfach ein `meta-data` oder `parentActivityName` Element mit der Parent Activity festlegen. Durch `setDisplayHomeAsUpEnabled(true)` wird in der `onCreate()` das Up Verhalten aktiviert.

Wenn eine Activity aus der eigenen App von anderen Apps aufgerufen werden kann, sollte ein neuer Task mit dem passenden Back Stack zu dieser Activity gestartet werden, um dem User die Up Navigation zu ermöglichen.

Auf einem Screen mit verschiedenen Einstiegspunkten, aus welchen die Parent Activity nicht ersichtlich ist, sollte sich die Up Navigation wie die Back Navigation verhalten. Ändern sich die Inhalte einer Activity, zum Beispiel durch swipen von Tabs, ändert das nichts an der Hierarchie und keine neue Navigation muss erstellt werden. Auch das Navigieren zwischen Screens in der gleichen Hierarchie Ebene (zum Beispiel zwischen Detailseiten)  ändert nichts an der Up Navigation.




## Navigation Drawer

Der Navigation Drawer beinhaltet die Haupt - Navigationspunkte und wird durch ein Panel am linken Rand des Screens dargestellt. Meistens ist er versteckt und kann durch swipen oder durch klicken auf das Icon in der Toolbar sichtbar gemacht werden. Das Icon, meist ein Hamburger Icon, zeigt an, das eine Navigation existiert.

Um einen Navigation Drawer in die App zu integrieren muss das Wurzelelement des Layouts der Main Activity das DrawerLayout sein. Darin befinden sich eine View, die den Inhalt des Screen darstellt, und eine andere View, die den Inhalt des Drawers beinhaltet. Das erste Element ist immer das FrameLayout, das den Inhalt des Screens beinhaltet und sollte daher die Breite und Höhe als match-parent definiert haben um den kompletten Screen abzudecken. Die Breite des Drawers sollte 320 dp nicht überschreiten, damit der Nutzer immer einen Teil des Inhalts sieht. Er besteht häufig aus einer ListView oder RecyclerView und einem Adapter. Was beim Klicken auf die einzelnen Listenelemente  passiert hängt von der gewählten App Struktur ab. Es wird zum Beispiel ein neues Fragment in den Screen Inhalt geladen.
