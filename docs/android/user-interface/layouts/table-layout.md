---
title: Xamarin. Android tablelayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/06/2018
ms.openlocfilehash: 4175b1fa62b2bc0e4209d13934c2bdbdd1e2a085
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028750"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin. Android tablelayout

[`TableLayout`](xref:Android.Widget.TableLayout) ist ein [`ViewGroup`](xref:Android.Views.ViewGroup)
, das untergeordnete [`View`](xref:Android.Views.View) anzeigt
Elemente in Zeilen und Spalten.

Starten Sie ein neues Projekt mit dem Namen **hellotablelayout**.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und fügen Sie Folgendes ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Open..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-O"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Save As..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-Shift-S"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Import..."
            android:padding="3dip"/>
    </TableRow>

    <TableRow>
        <TextView
            android:text="X"
            android:padding="3dip"/>
        <TextView
            android:text="Export..."
            android:padding="3dip"/>
        <TextView
            android:text="Ctrl-E"
            android:gravity="right"
            android:padding="3dip"/>
    </TableRow>

    <View
        android:layout_height="2dip"
        android:background="#FF909090"/>

    <TableRow>
        <TextView
            android:layout_column="1"
            android:text="Quit"
            android:padding="3dip"/>
    </TableRow>
</TableLayout>
```

Beachten Sie, dass dies der Struktur einer HTML-Tabelle ähnelt. Der [`TableLayout`](xref:Android.Widget.TableLayout)
das Element ist wie das HTML-`<table>` Element. [`TableRow`](xref:Android.Widget.TableRow)
ist wie ein `<tr>` Element; Allerdings können Sie für die Zellen eine beliebige Art [`View`](xref:Android.Views.View) Elements verwenden. In diesem Beispiel wird ein [`TextView`](xref:Android.Widget.TextView)
wird für jede Zelle verwendet. Zwischen einigen Zeilen gibt es auch eine einfache [`View`](xref:Android.Views.View), die verwendet wird, um eine horizontale Linie zu zeichnen.

Stellen Sie sicher, dass die **hellotablelayout** -Aktivität dieses Layout in der [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*))-Methode lädt die Layoutdatei für die [`Activity`](xref:Android.App.Activity), die durch die Ressourcen-ID angegeben ist &mdash; `Resource.Layout.Main` verweist auf die Layoutdatei **Resources/Layout/Main. axml** .

Führen Sie die Anwendung aus. Folgendes sollte angezeigt werden:

[![Beispiel-Screenshot der tablelayout-APP, die mehrere Tabellenzeilen anzeigt](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)

## <a name="references"></a>Verweise

- [`TableLayout`](xref:Android.Widget.TableLayout)
- [`TableRow`](xref:Android.Widget.TableRow)
- [`TextView`](xref:Android.Widget.TextView)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden._
