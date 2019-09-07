---
title: Xamarin. Android tablelayout
ms.prod: xamarin
ms.assetid: 0C7B9C95-5E5F-A069-BA37-984E49F7DCAD
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 0e09bf2364df9b672a9612829eaa7a8ba343b0e9
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70758396"
---
# <a name="xamarinandroid-tablelayout"></a>Xamarin. Android tablelayout

[`TableLayout`](xref:Android.Widget.TableLayout)ist eine[`ViewGroup`](xref:Android.Views.ViewGroup)
das untergeordnete Element anzeigt[`View`](xref:Android.Views.View)
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

Beachten Sie, dass dies der Struktur einer HTML-Tabelle ähnelt. Die[`TableLayout`](xref:Android.Widget.TableLayout)
das Element ist wie das `<table>` HTML-Element.[`TableRow`](xref:Android.Widget.TableRow)
ist wie ein `<tr>` -Element, aber für die Zellen können Sie beliebige Arten von [`View`](xref:Android.Views.View) Elementen verwenden. In diesem Beispiel wird ein[`TextView`](xref:Android.Widget.TextView)
wird für jede Zelle verwendet. Zwischen einigen Zeilen gibt es auch eine einfache [`View`](xref:Android.Views.View), die zum Zeichnen einer horizontalen Linie verwendet wird.

Stellen Sie sicher, dass die **hellotablelayout** -Aktivität dieses Layout lädt.[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)-Methode lädt die Layoutdatei für [`Activity`](xref:Android.App.Activity)das, das durch die Ressourcen &mdash; -ID `Resource.Layout.Main` angegeben wird, bezieht sich auf die Layoutdatei **Resources/Layout/Main. axml** .

Führen Sie die Anwendung aus. Folgendes sollte angezeigt werden:

[![Screenshot der tablelayout-APP, die mehrere Tabellenzeilen anzeigt](table-layout-images/helloviews3.png)](table-layout-images/helloviews3.png#lightbox)

## <a name="references"></a>Verweise

- [`TableLayout`](xref:Android.Widget.TableLayout)
- [`TableRow`](xref:Android.Widget.TableRow)
- [`TextView`](xref:Android.Widget.TextView)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden._
