---
title: Verwenden von relativelayout in xamarin. Android
description: Verwenden von relativelayout in einer xamarin. Android-Anwendung
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/29/2018
ms.openlocfilehash: 6cac771f46242cc0475be0a7ec0d475950f4b4e1
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028797"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin. Android relativelayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout) ist eine [`ViewGroup`](xref:Android.Views.ViewGroup) , die untergeordnete [`View`](xref:Android.Views.View) anzeigt.
Elemente in relativen Positionen. Die Position einer [`View`](xref:Android.Views.View) kann als relativ zu gleich geordneten Elementen (z. b. Links oder unterhalb eines angegebenen Elements) oder in Positionen relativ zum [`RelativeLayout`](xref:Android.Widget.RelativeLayout) angegeben werden.
Bereich (z. b. unten links neben zentriert).

Bei einem [`RelativeLayout`](xref:Android.Widget.RelativeLayout) handelt es sich um ein sehr leistungsfähiges Hilfsprogramm zum Entwerfen einer Benutzeroberfläche, da es sich um die [`ViewGroup`](xref:Android.Views.ViewGroup)s handelt. Wenn Sie sich mit mehreren [`LinearLayout`](xref:Android.Widget.LinearLayout)
Gruppen, Sie können Sie möglicherweise durch eine einzelne [`RelativeLayout`](xref:Android.Widget.RelativeLayout)ersetzen.

Starten Sie ein neues Projekt mit dem Namen **hellorelativelayout**.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und fügen Sie Folgendes ein:

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

Beachten Sie jedes der `android:layout_*` Attribute, z. b. `layout_below`, `layout_alignParentRight`und `layout_toLeftOf`.
Wenn Sie ein [`RelativeLayout`](xref:Android.Widget.RelativeLayout)verwenden, können Sie mithilfe dieser Attribute beschreiben, wie Sie die einzelnen [`View`](xref:Android.Views.View)positionieren möchten. Jedes dieser Attribute definiert eine andere Art von relativer Position. Einige Attribute verwenden die Ressourcen-ID eines gleich geordneten [`View`](xref:Android.Views.View) , um eine eigene relative Position zu definieren. Beispielsweise wird der letzte [`Button`](xref:Android.Widget.Button) so definiert, dass er Links von und bündig ausgerichtet ist, [`View`](xref:Android.Views.View) die von der ID `ok` (dem vorherigen [`Button`](xref:Android.Widget.Button)) identifiziert werden.

Alle verfügbaren Layoutattribute werden in [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)definiert.

Stellen Sie sicher, dass Sie dieses Layout in der [`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*) -Methode lädt die Layoutdatei für die [`Activity`](xref:Android.App.Activity), die durch die Ressourcen-ID angegeben ist &mdash; `Resource.Layout.Main` verweist auf die Layoutdatei **Resources/Layout/Main. axml** .

Führen Sie die Anwendung aus. Das folgende Layout sollte angezeigt werden:

[![Screenshot eines relativen Layouts mit den Schaltflächen Text View, EDITTEXT und Two](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>Ressourcen

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](https://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden._
