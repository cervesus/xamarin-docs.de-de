---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 45e625b28bbdf0009b5cfcf661b00ce17638771d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61311258"
---
# <a name="gridlayout"></a>GridLayout

Die `GridLayout` ist ein neues `ViewGroup` Unterklasse, die unterstützt Anordnung von Ansichten in einem 2D-Raster ähnelt einer HTML-Tabelle, wie unten dargestellt:

 [![Anzeigen von vier Zellen GridLayout zugeschnitten](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` funktioniert mit einer Flatfile-View-Hierarchie, in dem untergeordnete Ansichten ihre Positionen im Raster über festlegen die Zeilen und Spalten, die, denen Sie auf sein soll. Auf diese Weise die *GridLayout* positionieren Ansichten im Raster, ohne dass fortgeschrittenen Sichten Geben Sie eine Tabellenstruktur, so wie in den Zeilen der Tabelle, die in der TableLayout verwendet werden kann. Indem Sie eine flache Hierarchie, verwalten *GridLayout* kann mehr geht Layout seine untergeordneten Ansichten. Werfen wir einen Blick auf ein Beispiel zur Veranschaulichung, wie dieses Konzept im Code eigentlich bedeutet.


## <a name="creating-a-grid-layout"></a>Erstellen eines Grid-Layouts

Das folgende XML fügt mehrere `TextView` -Steuerelementen an eine *GridLayout*.

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

Anpassen des Layouts werden die Zeilen- und -Größen, damit die Zellen ihrem Inhalt verwendet werden können wie im folgenden Diagramm dargestellt:

 [![Diagramm des Layouts, der zwei Zellen auf der linken Seite kleiner ist als auf der rechten Seite anzeigt](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

Dadurch wird in der folgenden-Benutzeroberfläche bei der Ausführung in einer Anwendung:

 [![Screenshot der GridLayoutDemo-app, die vier Zellen anzeigen](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>Angeben der Ausrichtung

Beachten Sie, dass im obigen XML jedes `TextView` gibt nicht an einer Zeile oder Spalte. Wenn diese nicht angegeben werden, die `GridLayout` weist jeder untergeordneten Ansicht in der Reihenfolge, basierend auf die Ausrichtung. Z. B. Ändern der GridLayouts Ausrichtung von der Standardeinstellung handelt es sich horizontal, zu vertikal wie folgt:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Jetzt die `GridLayout` positioniert die Zellen von oben nach unten in jeder Spalte anstelle von links nach rechts, wie unten dargestellt:

 [![Diagramm zur Veranschaulichung, wie Zellen in vertikale Ausrichtung positioniert werden](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Dadurch wird in der folgenden-Benutzeroberfläche zur Laufzeit:

 [![Screenshot der GridLayoutDemo mit Zellen, die vertikale Ausrichtung positioniert](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>Angeben von expliziten Positionswerten

Wenn Sie möchten explizit zu steuern, die Positionen der untergeordneten Ansichten in der `GridLayout`, wir können festlegen, ihre `layout_row` und `layout_column` Attribute. Beispielsweise führt die folgende XML-Code das Layout, die im ersten Screenshot (siehe oben), unabhängig von die Ausrichtung angezeigt.

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



### <a name="specifying-spacing"></a>Angeben von Leerzeichen

Wir haben eine Reihe von Optionen, die Abstände zwischen den untergeordneten Ansichten Bereitstellen der `GridLayout`. Wir können die `layout_margin` Attributs auf den Rand zu den einzelnen untergeordneten Ansichten direkt festgelegt werden, wie unten dargestellt

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Darüber hinaus in Android 4 eine neue allgemeine Abstand Ansicht mit der `Space` ist jetzt verfügbar. Um es zu verwenden, müssen fügen Sie es einfach als eine untergeordnete Ansicht hinzu.
Das folgende XML fügt z.B. eine zusätzliche Zeile zum die `GridLayout` durch Festlegen der `rowcount` und 3 an, und fügt eine `Space` anzeigen, das Abstand zwischen der `TextViews`.

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

Der Vorteil der Verwendung der neuen `Space` Ansicht ist, können für die Analyse und erfordern keine Attribute für jede untergeordnete Ansicht festgelegt.



### <a name="spanning-columns-and-rows"></a>Aufteilung von Spalten und Zeilen

Die `GridLayout` unterstützt auch Zellen, die mehreren Spalten und Zeilen erstrecken. Angenommen, wir eine andere Zeile mit einer Schaltfläche zum Hinzufügen der `GridLayout` wie unten dargestellt:

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

Dadurch treten bei der ersten Spalte der `GridLayout` wird gestreckt, um die Größe der Schaltfläche, wie wir hier sehen:

[![Screenshot der GridLayoutDemo mit der Schaltfläche, umfassen nur die erste Spalte](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Um zu verhindern, dass die erste Spalte Strecken, können wir die Schaltfläche, um zwei Spalten umfassen, durch Festlegen seiner Columnspan wie folgt festlegen:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Dies führt zu ein Layout für die `TextViews` ähnelt das Layout, die wir zuvor mit der Schaltfläche hinzugefügt, die an das Ende der `GridLayout` wie unten dargestellt:

 [![Screenshot der GridLayoutDemo mit der Schaltfläche, die beide Spalten umfasst](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>Verwandte Links

- [GridLayoutDemo (sample)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Einführung in die Ice Cream Sandwich](http://www.android.com/about/ice-cream-sandwich/)
- [Android 4.0 Platform](https://developer.android.com/sdk/android-4.0.html)
