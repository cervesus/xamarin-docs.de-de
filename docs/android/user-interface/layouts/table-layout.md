---
title: TableLayout
ms.topic: article
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 31713937f9423bacdee83620a7e852ea6f141ee6
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="tablelayout"></a>TableLayout

[`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) ist eine [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , anzeigt, dass untergeordnete [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) Elemente in Zeilen und Spalten.

Starten Sie ein neues Projekt mit dem Namen **HelloTableLayout**.

Öffnen der **Resources/Layout/Main.axml** Datei und fügen Sie Folgendes:

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

Beachten Sie, wie dies die Struktur einer HTML-Tabelle ähnelt. Die [ `TableLayout` ](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) Element ist, z. B. HTML `<table>` Element. [ `TableRow` ](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) entspricht einem `<tr>` -Element ist; für die Zellen, Sie können jedoch eine Art von [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) Element. In diesem Beispiel wird eine [ `TextView` ](https://developer.xamarin.com/api/type/Android.Widget.TextView/) wird für jede Zelle verwendet. Zwischen einigen Zeilen, es gibt auch eine grundlegende [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/), der verwendet wird, eine horizontale Linie zu zeichnen.

Stellen Sie sicher, dass Ihre **HelloTableLayout** Aktivität lädt dieses Layout in der [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) Methode:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Methode lädt die Layoutdatei für die [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), durch die Ressourcen-ID angegebene &mdash; `Resource.Layout.Main` bezieht sich auf die **Ressourcen/Layout / Main.axml** Layoutdatei.

Führen Sie die Anwendung aus. Sie sollten Folgendes angezeigt:

[![Beispiel-Screenshot TableLayout-App, die mehrere Zeilen der Tabelle anzeigen](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)



## <a name="references"></a>Verweise

-   [`TableLayout`](https://developer.xamarin.com/api/type/Android.Widget.TableLayout/) 

-   [`TableRow`](https://developer.xamarin.com/api/type/Android.Widget.TableRow/) 

-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
