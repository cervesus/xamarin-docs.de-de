---
title: LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
ms.openlocfilehash: f3d0394f6b2388918f728bd5a25e9e809a832ca6
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670973"
---
# <a name="linearlayout"></a>LinearLayout

[`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) is a [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
die untergeordnete anzeigt [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
Elemente in einer linearen Richtung, entweder vertikal oder horizontal.

Sollte mit Bedacht zu verwenden. die [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).
Beginnen Sie mit Schachtelung mehrerer [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s, Sie sollten erwägen, stattdessen ein [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
Stattdessen verwenden.

Starten Sie ein neues Projekt namens **HelloLinearLayout**.

Open **Resources/Layout/Main.axml** und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"    >

  <LinearLayout
      android:orientation=    "horizontal"
      android:layout_width=    "match_parent"
      android:layout_height=    "match_parent"
      android:layout_weight=    "1"    >
      <TextView
          android:text=    "red"
          android:gravity=    "center_horizontal"
          android:background=    "#aa0000"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "green"
          android:gravity=    "center_horizontal"
          android:background=    "#00aa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "blue"
          android:gravity=    "center_horizontal"
          android:background=    "#0000aa"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
      <TextView
          android:text=    "yellow"
          android:gravity=    "center_horizontal"
          android:background=    "#aaaa00"
          android:layout_width=    "wrap_content"
          android:layout_height=    "match_parent"
          android:layout_weight=    "1"    />
  </LinearLayout>
        
  <LinearLayout
    android:orientation=    "vertical"
    android:layout_width=    "match_parent"
    android:layout_height=    "match_parent"
    android:layout_weight=    "1"    >
    <TextView
        android:text=    "row one"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row two"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row three"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
        android:layout_weight=    "1"    />
    <TextView
        android:text=    "row four"
        android:textSize=    "15pt"
        android:layout_width=    "match_parent"
        android:layout_height=    "wrap_content"
       android:layout_weight=    "1"    />
  </LinearLayout>

</LinearLayout>
```

Prüfen Sie sorgfältig, diesen XML-Code. Es ist ein Stamm [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
definiert die Ausrichtung auf vertikal &ndash; alle untergeordneten [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/)s (über die IT-Abteilung zwei hat) werden vertikal gestapelt werden. Das erste untergeordnete Element ist eine weitere [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
verwendet eine horizontale Ausrichtung und das zweite untergeordnete Element ist ein [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
die eine vertikale Ausrichtung verwendet. Jede dieser geschachtelten [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)s enthalten mehrere [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
Elemente, die miteinander in die Art und Weise definiert, die von ihren übergeordneten ausgerichtet sind [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/).

Öffnen Sie nun **HelloLinearLayout.cs** und stellen Sie sicher, lädt er die **Resources/Layout/Main.axml** Layout in der [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
Methode:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/(System.Int32)) Methode lädt die Layoutdatei für den [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), angegeben durch die Ressourcen-ID &ndash; `Resources.Layout.Main` bezieht sich auf die **Ressourcen/Layout / Main.axml** Layoutdatei.

Führen Sie die Anwendung aus. Sie sollte Folgendes angezeigt werden:

[![Screenshot der app, die erste LinearLayout horizontal angeordnet, die zweite vertikal](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

Beachten Sie, wie die XML-Attribute jeder Ansicht Verhalten definieren. Experimentieren mit verschiedenen Werten für `android:layout_weight` basierend auf die Gewichtung der einzelnen Elemente finden Sie unter wie Bildschirmfläche verteilt wird. Finden Sie unter den [allgemeine Layoutobjekte](https://developer.android.com/guide/topics/ui/declaring-layout.html) Dokument Weitere Informationen [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/)
verarbeitet die `android:layout_weight` Attribut.


## <a name="references"></a>Verweise

-   [`LinearLayout`](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) 
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/) 

*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/).

