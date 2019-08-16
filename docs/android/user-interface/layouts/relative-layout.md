---
title: Verwenden von relativelayout in xamarin. Android
description: Verwenden von relativelayout in einer xamarin. Android-Anwendung
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/29/2018
ms.openlocfilehash: af74ae3c7c87f501bff519bcfa361264205ca3f1
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522375"
---
# <a name="xamarinandroid-relativelayout"></a>Xamarin. Android relativelayout

[`RelativeLayout`](xref:Android.Widget.RelativeLayout)ist eine [`ViewGroup`](xref:Android.Views.ViewGroup) , die untergeordnete Elemente anzeigt[`View`](xref:Android.Views.View)
Elemente in relativen Positionen. Die Position eines [`View`](xref:Android.Views.View) kann als relativ zu gleich geordneten Elementen (z. b. Links oder unterhalb eines angegebenen Elements) oder in Positionen relativ zum[`RelativeLayout`](xref:Android.Widget.RelativeLayout)
Bereich (z. b. unten links neben zentriert).

Ein [`RelativeLayout`](xref:Android.Widget.RelativeLayout) ist ein sehr leistungsfähiges Dienstprogramm für das Entwerfen einer Benutzeroberfläche, da es [`ViewGroup`](xref:Android.Views.ViewGroup)sich um das Entfernen von Netzwerken handelt. Wenn Sie sich mithilfe mehrerer der folgenden Zeichen befinden,[`LinearLayout`](xref:Android.Widget.LinearLayout)
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

Beachten Sie jedes `android:layout_*` der Attribute, `layout_below`z. b `layout_alignParentRight`., `layout_toLeftOf`und.
Wenn Sie [`View`](xref:Android.Views.View)verwenden [,könnenSiedieseAttributeverwenden,umzubeschreiben,wieSiedieeinzelnen`RelativeLayout`](xref:Android.Widget.RelativeLayout)Eigenschaften positionieren möchten. Jedes dieser Attribute definiert eine andere Art von relativer Position. Einige Attribute verwenden die Ressourcen-ID eines [`View`](xref:Android.Views.View) gleich geordneten Elements, um seine eigene relative Position zu definieren. Beispielsweise wird der letzte [`Button`](xref:Android.Widget.Button) so definiert, dass er Links von und bündig ausgerichtet ist, die [`View`](xref:Android.Views.View) durch die ID `ok` (die vorherige [`Button`](xref:Android.Widget.Button)) identifiziert wird.

Alle verfügbaren Layoutattribute werden in [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)definiert.

Stellen Sie sicher, dass Sie dieses Layout in der[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [`SetContentView(int)`](xref:Android.App.Activity.SetContentView*) -Methode lädt die Layoutdatei [`Activity`](xref:Android.App.Activity)für den, der durch die &mdash; Ressourcen-ID `Resource.Layout.Main` angegeben wird, bezieht sich auf die Layoutdatei **Resources/Layout/Main. axml** .

Führen Sie die Anwendung aus. Das folgende Layout sollte angezeigt werden:

[![Screenshot eines relativen Layouts mit der Schaltfläche "TextView", "EDITTEXT" und zwei Schaltflächen](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)

## <a name="resources"></a>Ressourcen

- [`RelativeLayout`](xref:Android.Widget.RelativeLayout)
- [`RelativeLayout.LayoutParams`](xref:Android.Widget.RelativeLayout.LayoutParams)
- [`TextView`](xref:Android.Widget.TextView)
- [`EditText`](xref:Android.Widget.EditText)
- [`Button`](xref:Android.Widget.Button)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden._
