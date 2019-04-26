---
title: Verwenden die RelativeLayout in Xamarin.Android
description: Verwenden von RelativeLayout in einer Xamarin.Android-Anwendung
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: af2972ecc92435836a75013e6203ba47c2c04627
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61303629"
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) ist eine [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , untergeordneten anzeigt [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
Elemente in die relativen Positionen. Die Position des ein [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) kann relativ zum gleichgeordneten Elementen (z. B. hinsichtlich der linken Seite des oder unterhalb eines angegebenen Elements) angegeben werden, oder in positioniert relativ zu den [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
Bereich (z. B. links unten ausgerichtet angezeigt Center).

Ein [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) ist ein sehr leistungsfähiges Dienstprogramm, bei geschachtelten Entwerfen einer Benutzeroberfläche, da es entfernen kann [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Wenn Sie sich über mehrere geschachtelte [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
Gruppen, möglicherweise können sie mit einem einzelnen ersetzen [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Starten Sie ein neues Projekt namens **HelloRelativeLayout**.

Öffnen der **Resources/Layout/Main.axml** Datei, und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="@android:drawable/editbox_background"
        android:layout_below="@id/label"/>
    <Button
        android:id="@+id/ok"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_below="@id/entry"
        android:layout_alignParentRight="true"
        android:layout_marginLeft="10dip"
        android:text="OK" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_toLeftOf="@id/ok"
        android:layout_alignTop="@id/ok"
        android:text="Cancel" />
</RelativeLayout>
```

Beachten Sie, dass jede der `android:layout_*` Attribute, z. B. `layout_below`, `layout_alignParentRight`, und `layout_toLeftOf`.
Bei Verwendung einer [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), können Sie diese Attribute beschrieben, wie Sie jede positionieren möchten [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Jedes dieser Attribute definieren eine andere Art von relativen Position. Einige Attribute verwenden, die Ressourcen-ID ein gleichgeordnetes Element [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) seine eigene relative Position zu definieren. Z. B. das letzte [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) wird definiert, um auf die links von und das ausgerichtet-mit-the-Top-of liegen die [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) durch die ID identifizierten `ok` (d.h., dass die vorherige [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Alle verfügbaren Attribute sind in definiert [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Stellen Sie sicher, dass Sie dieses Layout im Laden der [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
Methode:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Methode lädt die Layoutdatei für den [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), angegeben durch die Ressourcen-ID &mdash; `Resource.Layout.Main` bezieht sich auf die **Ressourcen/Layout / Main.axml** Layoutdatei.

Führen Sie die Anwendung aus. Daraufhin sollte das folgende Layout:

[![Screenshot eines relativen Layouts mit einer TextView EditText und zwei Schaltflächen](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Ressourcen

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
