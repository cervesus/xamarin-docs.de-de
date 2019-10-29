---
title: Xamarin. Android LinearLayout
ms.prod: xamarin
ms.assetid: B49D129C-AF24-3C5A-C833-5A34AFBB2442
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/07/2018
ms.openlocfilehash: b1cff01c66ae2581a68286e62bd8c8c5fb7f9d72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028957"
---
# <a name="xamarinandroid-linearlayout"></a>Xamarin. Android LinearLayout

[`LinearLayout`](xref:Android.Widget.LinearLayout) ist ein [`ViewGroup`](xref:Android.Views.ViewGroup)
, das untergeordnete [`View`](xref:Android.Views.View) anzeigt
Elemente in einer linearen Richtung, entweder vertikal oder horizontal.

Sie sollten vorsichtig sein, wenn Sie die [`LinearLayout`](xref:Android.Widget.LinearLayout)verwenden möchten.
Wenn Sie mit dem Schachteln mehrerer [`LinearLayout`](xref:Android.Widget.LinearLayout)s beginnen, sollten Sie die Verwendung eines [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
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

Prüfen Sie diese XML-Datei sorgfältig. Stamm [`LinearLayout`](xref:Android.Widget.LinearLayout)
, die die Ausrichtung so definiert, dass Sie vertikal &ndash; alle untergeordneten [`View`](xref:Android.Views.View)s (von denen zwei ist) vertikal gestapelt werden. Das erste untergeordnete Element ist ein weiteres [`LinearLayout`](xref:Android.Widget.LinearLayout)
, der eine horizontale Ausrichtung verwendet, und das zweite untergeordnete Element ist eine [`LinearLayout`](xref:Android.Widget.LinearLayout)
, der eine vertikale Ausrichtung verwendet. Jede dieser [`LinearLayout`](xref:Android.Widget.LinearLayout)s enthält mehrere [`TextView`](xref:Android.Widget.TextView)
Elemente, die in der durch ihre übergeordneten [`LinearLayout`](xref:Android.Widget.LinearLayout)definierten Weise aufeinander ausgerichtet sind.

Öffnen Sie jetzt **HelloLinearLayout.cs** , und stellen Sie sicher, dass das Layout **Ressourcen/Layout/Main. axml** im [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*))-Methode lädt die Layoutdatei für die [`Activity`](xref:Android.App.Activity), die durch die Ressourcen-ID angegeben ist &ndash; `Resources.Layout.Main` verweist auf die Layoutdatei **Resources/Layout/Main. axml** .

Führen Sie die Anwendung aus. Folgendes sollte angezeigt werden:

[![Screenshot des ersten horizontal angeordneten App-linearlayouts, Sekunde vertikal](linear-layout-images/helloviews1.png)](linear-layout-images/helloviews1.png#lightbox)

Beachten Sie, wie die XML-Attribute jedes Verhalten der Ansicht definieren. Experimentieren Sie mit verschiedenen Werten für `android:layout_weight`, um zu sehen, wie die Bildschirmfläche auf der Grundlage der Gewichtung der einzelnen Elemente verteilt wird. Im Dokument [Allgemeine Layoutobjekte](https://developer.android.com/guide/topics/ui/declaring-layout.html) finden Sie weitere Informationen zur Vorgehensweise [`LinearLayout`](xref:Android.Widget.LinearLayout)
behandelt das `android:layout_weight` Attribut.

## <a name="references"></a>Verweise

- [`LinearLayout`](xref:Android.Widget.LinearLayout)
- [`TextView`](xref:Android.Widget.TextView)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden._
