---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 13271b50353d95ecd2db40e25d549788111530f7
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725003"
---
# <a name="xamarinandroid-gridlayout"></a>Xamarin. Android-GridLayout

Der `GridLayout` ist eine neue `ViewGroup` Unterklasse, die das Anordnen von Sichten in einem 2D-Raster unterstützt, ähnlich wie eine HTML-Tabelle, wie unten dargestellt:

 [![ausgeschnittenes GridLayout, das vier Zellen anzeigt](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout` funktioniert mit einer Hierarchie in flachen Sichten, in der untergeordnete Sichten ihre Positionen im Raster festlegen, indem Sie die Zeilen und Spalten angeben, in denen Sie sich befinden sollten. Auf diese Weise kann das *GridLayout* Sichten im Raster positionieren, ohne dass es erforderlich ist, dass zwischen Sichten eine Tabellenstruktur bereitstellen, z. b. in den Tabellenzeilen, die im tablelayout verwendet werden. Durch die Beibehaltung einer flachen Hierarchie kann *GridLayout* die untergeordneten Ansichten schneller ausrichten. Werfen wir einen Blick auf ein Beispiel, um zu veranschaulichen, wie dieses Konzept im Code tatsächlich gemeint ist.

## <a name="creating-a-grid-layout"></a>Erstellen eines Raster Layouts

Mit dem folgenden XML werden einem *GridLayout*mehrere `TextView`-Steuerelemente hinzugefügt.

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

Das Layout passt die Zeilen-und Spaltengrößen so an, dass die Zellen ihren Inhalt anpassen können, wie im folgenden Diagramm dargestellt:

 [![Diagramm des Layouts, das zwei Zellen auf der linken Seite kleiner als auf der rechten Seite anzeigt.](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

Dies führt zur folgenden Benutzeroberfläche, wenn Sie in einer Anwendung ausgeführt wird:

 [![Screenshot der gridlayoutdemo-APP, die vier Zellen anzeigt](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)

## <a name="specifying-orientation"></a>Angeben der Ausrichtung

Beachten Sie im obigen XML-Code, dass jeder `TextView` keine Zeile oder Spalte angibt. Wenn diese nicht angegeben sind, weist die `GridLayout` jede untergeordnete Ansicht basierend auf der Ausrichtung der Reihenfolge zu. Nehmen wir beispielsweise an, dass die Ausrichtung des GridLayout von der standardmäßigen, horizontalen und vertikalen Weise wie folgt geändert wird:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Nun positionieren die `GridLayout` die Zellen in jeder Spalte von oben nach unten, und nicht von links nach rechts, wie unten dargestellt:

 [![Diagramm, das veranschaulicht, wie Zellen in vertikaler Ausrichtung positioniert werden](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Dies führt zur Laufzeit zur folgenden Benutzeroberfläche:

 [![Screenshot von gridlayoutdemo mit in vertikaler Ausrichtung positionierten Zellen](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)

### <a name="specifying-explicit-position"></a>Angeben der expliziten Position

Wenn wir die Positionen der untergeordneten Ansichten im `GridLayout`explizit steuern möchten, können wir Ihre `layout_row`-und `layout_column` Attribute festlegen. Der folgende XML-Code führt z. b. zu dem Layout, das im ersten Screenshot angezeigt wird (siehe oben), unabhängig von der Ausrichtung.

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

### <a name="specifying-spacing"></a>Angeben von Abstände

Wir haben eine Reihe von Optionen, die den Abstand zwischen den untergeordneten Ansichten der `GridLayout`bereitstellen. Wir können das `layout_margin`-Attribut verwenden, um den Rand für jede untergeordnete Ansicht direkt festzulegen, wie unten gezeigt.

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Außerdem ist nun in Android 4 eine neue allgemeine Abstands Ansicht namens `Space` verfügbar. Um es zu verwenden, fügen Sie es einfach als untergeordnete Ansicht hinzu.
Der folgende XML-Code fügt z. b. eine zusätzliche Zeile zum `GridLayout` hinzu, indem er die `rowcount` auf 3 festlegt und eine `Space` Sicht hinzufügt, die Abstand zwischen dem `TextViews`bereitstellt.

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

Dieser XML-Code erstellt in der `GridLayout` Abstand, wie unten dargestellt:

 [![Screenshot von gridlayoutdemo, das größere Zellen mit Abstand illustriert](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

Der Vorteil der Verwendung der neuen `Space` Ansicht besteht darin, dass Sie Abstand zulässt und es nicht erforderlich ist, Attribute für jede untergeordnete Ansicht festzulegen.

### <a name="spanning-columns-and-rows"></a>Umspannen von Spalten und Zeilen

Der `GridLayout` unterstützt auch Zellen, die mehrere Spalten und Zeilen umfassen. Nehmen wir beispielsweise an, dass Sie dem `GridLayout` eine weitere Zeile mit einer Schaltfläche hinzufügen, wie unten gezeigt:

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

Dies führt dazu, dass die erste Spalte des `GridLayout` gestreckt wird, um die Größe der Schaltfläche anzupassen, wie hier zu sehen ist:

[![Screenshot von gridlayoutdemo mit Schaltfläche, die nur die erste Spalte umfasst](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Um die Streckung der ersten Spalte beizubehalten, können Sie die Schaltfläche auf zwei Spalten umspannen, indem Sie die zugehörige ColumnSpan wie folgt festlegen:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Dies führt zu einem Layout für das `TextViews`, das dem oben erwähnten Layout ähnelt, wobei die Schaltfläche am unteren Rand der `GridLayout` hinzugefügt wurde, wie unten dargestellt:

 [![Screenshot von gridlayoutdemo mit Schaltfläche, die beide Spalten umfasst](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)

## <a name="related-links"></a>Verwandte Themen

- [Gridlayoutdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/gridlayoutdemo)
