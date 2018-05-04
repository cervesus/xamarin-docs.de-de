---
title: Verwenden die RelativeLayout in Xamarin.Android
ms.prod: xamarin
ms.assetid: AFD9C849-02C3-E728-BC78-77A563612BC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: cd2d7537036978e30c97b5776155e429178b6dac
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
---
# <a name="relativelayout"></a>RelativeLayout

[`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) ist eine [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , anzeigt, dass untergeordnete [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) Elemente im relativen Positionen. Die Position des eine [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) kann relativ zum nebengeordneten Elementen (z. B. auf der linken Seite des oder unterhalb eines bestimmten Elements) angegeben werden oder in relativ zur positioniert die [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) Bereich (z. B. orientiert sich am unteren, links von der Mitte).

Ein [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/) ist ein sehr leistungsfähiges Hilfsprogramm für die Schachtelung Entwerfen einer Benutzeroberfläche, da es entfernen, kann [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)s. Wenn Sie sich unter Verwendung mehrerer geschachtelt [ `LinearLayout` ](https://developer.xamarin.com/api/type/Android.Widget.LinearLayout/) Gruppen möglicherweise ersetzen Sie sie mit einem einzelnen [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/).

Starten Sie ein neues Projekt mit dem Namen **HelloRelativeLayout**.

Öffnen der **Resources/Layout/Main.axml** Datei und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <TextView
        android:id="@+id/label"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:text="Type here:"/>
    <EditText
        android:id="@+id/entry"
        android:layout_width="fill_parent"
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

Beachten Sie jede der `android:layout_*` Attribute – beispielsweise `layout_below`, `layout_alignParentRight`, und `layout_toLeftOf`.
Bei Verwendung einer [ `RelativeLayout` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/), können Sie diese Attribute beschrieben, wie Sie jede positionieren möchten [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/). Jedes dieser Attribute definieren eine andere Art von relativen Position. Einige Attribute verwenden, die Ressourcen-ID, der ein gleichgeordnetes Element [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) seiner eigenen relative Position zu definieren. Z. B. das letzte [ `Button` ](https://developer.xamarin.com/api/type/Android.Widget.Button/) definiert ist, auf die links von und das ausgerichtet-mit-des-Top-of liegen die [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) durch die ID identifizierte `ok` (also in der vorherigen [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)).

Alle verfügbaren Attribute definiert sind, [ `RelativeLayout.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/).

Stellen Sie sicher, dass Sie dieses Layout in laden die [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) Methode:

```csharp
protected override void OnCreate (Bundle savedInstanceState)
{
    base.OnCreate (savedInstanceState);
    SetContentView (Resource.Layout.Main);
}
```

Die [ `SetContentView(int)` ](https://developer.xamarin.com/api/member/Android.App.Activity.SetContentView/p/System.Int32/) Methode lädt die Layoutdatei für die [ `Activity` ](https://developer.xamarin.com/api/type/Android.App.Activity/), durch die Ressourcen-ID angegebene &mdash; `Resource.Layout.Main` bezieht sich auf die **Ressourcen/Layout / Main.axml** Layoutdatei.

Führen Sie die Anwendung aus. Das folgende Layout sollte angezeigt werden:

[![Screenshot eines relativen Layouts mit einem TextView EditText- und zwei Schaltflächen](relative-layout-images/helloviews2.png)](relative-layout-images/helloviews2.png#lightbox)


## <a name="resources"></a>Ressourcen

-   [`RelativeLayout`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout/)
-   [`RelativeLayout.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.RelativeLayout+LayoutParams/)
-   [`TextView`](https://developer.xamarin.com/api/type/Android.Widget.TextView/)
-   [`EditText`](https://developer.xamarin.com/api/type/Android.Widget.EditText/)
-   [`Button`](https://developer.xamarin.com/api/type/Android.Widget.Button/)


*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
