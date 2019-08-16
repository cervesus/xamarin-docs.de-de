---
title: Xamarin. Android LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/07/2018
ms.openlocfilehash: 3171a89678e88a924198c3921d197c0f0378d29b
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522627"
---
# <a name="xamarinandroid-linearlayout"></a>Xamarin. Android LinearLayout

[`LinearLayout`](xref:Android.Widget.LinearLayout)ist eine[`ViewGroup`](xref:Android.Views.ViewGroup)
das untergeordnete Element anzeigt[`View`](xref:Android.Views.View)
Elemente in einer linearen Richtung, entweder vertikal oder horizontal.

Sie sollten vorsichtig sein, wenn Sie die [`LinearLayout`](xref:Android.Widget.LinearLayout)verwenden.
Wenn Sie mit der Schachtelung mehrerer [`LinearLayout`](xref:Android.Widget.LinearLayout)s beginnen, sollten Sie die Verwendung eines[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
Stattdessen.

Starten Sie ein neues Projekt mit dem Namen **hellolinearlayout**.

Öffnen Sie **Resources/Layout/Main. axml** , und fügen Sie Folgendes ein:

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

Prüfen Sie diese XML-Datei sorgfältig. Es ist ein Stammverzeichnis vorhanden.[`LinearLayout`](xref:Android.Widget.LinearLayout)
, die die Ausrichtung so definiert, &ndash; dass [`View`](xref:Android.Views.View)alle untergeordneten s (von denen zwei ist) vertikal gestapelt werden. Das erste untergeordnete Element ist ein weiteres[`LinearLayout`](xref:Android.Widget.LinearLayout)
, der eine horizontale Ausrichtung verwendet, und das zweite untergeordnete Element ein[`LinearLayout`](xref:Android.Widget.LinearLayout)
, der eine vertikale Ausrichtung verwendet. Jede dieser nten [`LinearLayout`](xref:Android.Widget.LinearLayout)s enthält mehrere[`TextView`](xref:Android.Widget.TextView)
Elemente, die in der durch ihr übergeordneten [`LinearLayout`](xref:Android.Widget.LinearLayout)Element definierten Weise ausgerichtet sind.

Öffnen Sie jetzt **HelloLinearLayout.cs** , und stellen Sie sicher, dass das Layout " **Resources/Layout/Main. axml** " im[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*)-Methode lädt die Layoutdatei für [`Activity`](xref:Android.App.Activity)das, das durch die Ressourcen &ndash; -ID `Resources.Layout.Main` angegeben wird, bezieht sich auf die Layoutdatei **Resources/Layout/Main. axml** .

Führen Sie die Anwendung aus. Folgendes sollte angezeigt werden:

[![Screenshot des ersten horizontal angeordneten App-linearlayouts, zweites vertikal](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

Beachten Sie, wie die XML-Attribute jedes Verhalten der Ansicht definieren. Experimentieren Sie mit unterschiedlichen Werten `android:layout_weight` für, um zu sehen, wie die Bildschirm-Real Anlage basierend auf der Gewichtung der einzelnen Elemente verteilt wird. Weitere Informationen zur Vorgehensweise finden Sie im Dokument [Allgemeine Layoutobjekte](https://developer.android.com/guide/topics/ui/declaring-layout.html) .[`LinearLayout`](xref:Android.Widget.LinearLayout)
behandelt das `android:layout_weight` -Attribut.


## <a name="references"></a>Verweise

- [`LinearLayout`](xref:Android.Widget.LinearLayout)
- [`TextView`](xref:Android.Widget.TextView)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden._
