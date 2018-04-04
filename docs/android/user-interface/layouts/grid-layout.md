---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 53f2ba8ff14a4338310e02244acdbfd7fa9bc13c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="gridlayout"></a>GridLayout

Die `GridLayout` ist ein neues `ViewGroup` Unterklasse, die unterstützt Anordnung Ansichten 2D Datenblatt ähnlich einer HTML-Tabelle, wie unten dargestellt:

 [![Anzeigen von vier Zellen GridLayout zugeschnitten](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` funktioniert mit einer Flatfile-View-Hierarchie, in dem untergeordnete Ansichten ihre Positionen im Raster über festlegen die Zeilen und Spalten werden in befinden soll. Auf diese Weise die *GridLayout* können Ansichten im Raster zu positionieren, ohne dass intermediate Sichten eine Tabellenstruktur, geben Sie z. B. in den Zeilen der Tabelle, in der TableLayout verwendet angezeigt wird. Indem Sie eine flache Hierarchie verwalten *GridLayout* kann mehr zügig Layout seiner untergeordneten Ansichten. Werfen wir einen Blick auf ein Beispiel zur Veranschaulichung dieses Konzept tatsächlich im Code Bedeutung.


## <a name="creating-a-grid-layout"></a>Erstellen ein Gitternetzlayout

Die folgende XML-Code fügt mehrere `TextView` -Steuerelementen an eine *GridLayout*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

Anpassen des Layouts werden die Zeilen- und -Größen, damit die Zellen passt ihre Inhalte wie das folgende Diagramm veranschaulicht:

 [![Diagramm des Layouts, der zwei Zellen auf der linken Seite kleiner ist als auf der rechten Seite anzeigt](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

Daraus ergibt sich folgende Benutzer-Schnittstelle, wenn in einer Anwendung ausführen:

 [![Screenshot der GridLayoutDemo-app, die vier Zellen anzeigen](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>Angeben der Ausrichtung

Beachten Sie in der obigen XML jedes `TextView` nicht angegeben ist, eine Zeile oder Spalte. Wenn diese nicht angegeben werden, die `GridLayout` weist jede untergeordnete Ansicht in der Reihenfolge, die auf Grundlage der Ausrichtung. Z. B. wir die GridLayout Ausrichtung von der Standardeinstellung horizontal, zu vertikal wie folgt wird ändern:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Jetzt die `GridLayout` positioniert die Zellen von oben nach unten in jeder Spalte, anstelle von links nach rechts, wie unten dargestellt:

 [![Diagramm zur Veranschaulichung, wie die Zellen in vertikaler Ausrichtung positioniert werden](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Daraus ergibt sich die folgenden Benutzeroberfläche zur Laufzeit:

 [![Screenshot der GridLayoutDemo mit Zellen in vertikaler Ausrichtung positioniert](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>Angeben von expliziten Positionswerten

Wenn wir steuern, explizit die Positionen der untergeordneten Ansichten in möchten der `GridLayout`, legen wir ihre `layout_row` und `layout_column` Attribute. Beispielsweise führt die folgende XML-Code das Layout in der ersten Screenshot (siehe oben), unabhängig von der Ausrichtung angezeigt.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```



### <a name="specifying-spacing"></a>Abstand angibt.

Wir haben eine Reihe von Optionen, die Abstände zwischen den untergeordneten Ansichten Bereitstellen der `GridLayout`. Wir können die `layout_margin` Attribut, um den Rand für jede untergeordnete Ansicht direkt festgelegt werden, wie unten dargestellt

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Darüber hinaus in Android 4 eine neue allgemeine Abstand Ansicht mit der Bezeichnung `Space` ist jetzt verfügbar. Um es verwenden zu können, müssen fügen Sie es einfach als eine untergeordnete Ansicht hinzu.
Z. B. der folgenden XML-Code fügt eine zusätzliche Zeile zum der `GridLayout` durch Festlegen seiner `rowcount` und 3 an, und fügt eine `Space` Projektcenter Abstand zwischen den `TextViews`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

Dieser XML-Code erstellt einen Abstand in der `GridLayout` wie unten dargestellt:

 [![Screenshot der GridLayoutDemo zur Veranschaulichung größerer Zellen mit Abstand](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

Der Vorteil der Verwendung des neuen `Space` Sicht ist, können für den Abstand und erfordert keine uns Attribute für jede untergeordnete Ansicht festlegen.



### <a name="spanning-columns-and-rows"></a>Aufteilung von Spalten und Zeilen

Die `GridLayout` unterstützt auch die Zellen, die mehrere Spalten und Zeilen umfassen. Angenommen, wir eine weitere Zeile enthält eine Schaltfläche zum Hinzufügen der `GridLayout` wie unten dargestellt:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

Dies führt in der ersten Spalte von der `GridLayout` wird gestreckt, um die Größe der Schaltfläche Erweitert Siehe wir hier:

[![Screenshot der GridLayoutDemo mit umfasst nur die erste Spalte einer Schaltfläche](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Um zu verhindern, dass die erste Spalte Strecken, können wir die Schaltfläche, die zwei Spalten umfassen, durch Festlegen seiner Columnspan wie folgt festlegen:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Dies führt zu einem Layout für die `TextViews` , die ähnelt das Layout, wir zuvor mit der Schaltfläche hinzugefügt, die an das Ende mussten, der `GridLayout` wie unten dargestellt:

 [![Screenshot der GridLayoutDemo mit einer Schaltfläche der Aufteilung beide Spalten](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>Verwandte Links

- [GridLayoutDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Einführung in Eis Rustikal Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](http://developer.android.com/sdk/android-4.0.html)
