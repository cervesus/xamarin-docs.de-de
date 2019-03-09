---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: 63164d90419f3a49d9eb52a52d02e05fbee43dbf
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667619"
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) is a [`ViewGroup`](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/)
die Elemente in einem zweidimensionalen, bildlauffähigen Datenblatt anzeigt. Die Rasterelemente werden automatisch eingefügt, mit dem Layout ein [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).

In diesem Tutorial erstellen Sie ein Raster mit Miniaturansichten. Wenn ein Element ausgewählt ist, wird eine eingeblendeten Nachricht, die Position des Bilds angezeigt.

Starten Sie ein neues Projekt namens **HelloGridView**.

Finden Sie einige Fotos, die Sie verwenden möchten oder [Herunterladen dieser Beispielbilder](https://developer.android.com/shareables/sample_images.zip). Hinzufügen der Bilddateien des Projekts **Ressourcen/Drawable** Verzeichnis. In der **Eigenschaften** legen die Build-Aktion für jedes **AndroidResource**.

Öffnen der **Resources/Layout/Main.axml** Datei, und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gridview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:columnWidth="90dp"
    android:numColumns="auto_fit"
    android:verticalSpacing="10dp"
    android:horizontalSpacing="10dp"
    android:stretchMode="columnWidth"
    android:gravity="center"
/>
```

Dies [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) füllt den gesamten Bildschirm. Die Attribute sind recht selbsterklärend. Weitere Informationen zu gültigen Attribute finden Sie unter den [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) Verweis.

Open `HelloGridView.cs` und fügen Sie den folgenden Code für die [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
Methode:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    SetContentView (Resource.Layout.Main);

    var gridview = FindViewById<GridView> (Resource.Id.gridview);
    gridview.Adapter = new ImageAdapter (this);

    gridview.ItemClick += delegate (object sender, AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Nach der **Main.axml** Layout für die Ansicht "Inhalt" festgelegt ist die [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) wird erfasst, aus dem Layout mit [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). Die [`Adapter`](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)
Eigenschaft wird dann zum Festlegen eines benutzerdefinierten Adapters (`ImageAdapter`) als Quelle für alle Elemente, die im Raster angezeigt werden. Die `ImageAdapter` wird im nächsten Schritt erstellt.

Um etwas tun, wenn ein Element im Raster geklickt wird, ist ein anonymer Delegat für abonniert die [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) Ereignis.
Es zeigt eine [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) , der die Indexposition (nullbasiert) des ausgewählten Elements anzeigt (in einem realen Szenario, die Position kann verwendet werden, die das Vergrößern für eine andere Aufgabe zu erhalten). Beachten Sie, dass Java-Stil Listenerklassen anstelle von Ereignissen für .NET verwendet werden können.

Erstellen Sie eine neue Klasse namens `ImageAdapter` , Unterklassen [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
        context = c;
    }

    public override int Count {
        get { return thumbIds.Length; }
    }

    public override Java.Lang.Object GetItem (int position)
    {
        return null;
    }

    public override long GetItemId (int position)
    {
        return 0;
    }

    // create a new ImageView for each item referenced by the Adapter
    public override View GetView (int position, View convertView, ViewGroup parent)
    {
        ImageView imageView;

        if (convertView == null) {  // if it's not recycled, initialize some attributes
            imageView = new ImageView (context);
            imageView.LayoutParameters = new GridView.LayoutParams (85, 85);
            imageView.SetScaleType (ImageView.ScaleType.CenterCrop);
            imageView.SetPadding (8, 8, 8, 8);
        } else {
            imageView = (ImageView)convertView;
        }

        imageView.SetImageResource (thumbIds[position]);
        return imageView;
    }

    // references to our images
    int[] thumbIds = {
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7,
        Resource.Drawable.sample_0, Resource.Drawable.sample_1,
        Resource.Drawable.sample_2, Resource.Drawable.sample_3,
        Resource.Drawable.sample_4, Resource.Drawable.sample_5,
        Resource.Drawable.sample_6, Resource.Drawable.sample_7
    };
}
```

Dadurch wird zunächst einige erforderliche von geerbten Methoden implementiert [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/). Der Konstruktor und die [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) Eigenschaft sind selbsterklärend. In der Regel [`GetItem(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/)
sollte das eigentliche Objekt an der angegebenen Position im Adapter zurückgeben, aber in diesem Beispiel wird ignoriert. Likewise, [`GetItemId(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/)
sollte die Zeilen-Id des Elements zurückgeben, aber es ist nicht erforderlich, hier.

Die erste Methode, die erforderlich ist [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/).
Diese Methode erstellt ein neues [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
für jedes Bild hinzugefügt, die `ImageAdapter`. Wenn Sie aufgerufen wird, eine [`View`](https://developer.xamarin.com/api/type/Android.Views.View/)
übergeben wird im, dies ist normalerweise ein Objekt wiederverwendet (zumindest nachdem dies einmal aufgerufen wurde), sodass es überprüft wird, um festzustellen, ob das Objekt null ist. Wenn sie *ist* null ist, ein [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
Instanziierung und Konfiguration mit gewünschten Eigenschaften für die Image-Präsentation:

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) Legt die Höhe und Breite für die Ansicht&mdash;Dadurch wird sichergestellt, dass unabhängig von der Größe des der drawable, jedes Image wird geändert, und entsprechend in diesen Dimensionen nach Bedarf zugeschnitten.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) deklariert, dass die Bilder in Richtung der Mitte zugeschnitten werden soll (falls erforderlich).

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) definiert die Auffüllung für alle Seiten. (Beachten Sie, dass, wenn die Images unterschiedliche Seitenverhältnisse haben, klicken Sie dann weniger Auffüllung verursacht für weitere Zuschneiden des Bildes, wenn sie nicht mit die Dimensionen, die ImageView übereinstimmt.)

Wenn die [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) übergeben [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) ist *nicht* Null, und klicken Sie dann auf der lokalen [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
Initialisiert mit dem wiederverwendet [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) Objekt.

Am Ende der [`GetView()`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/)
Methode, die `position` ganze Zahl, die an die Methode übergeben wird, wählen Sie ein Image aus verwendet die `thumbIds` Array, das die Bildressource für festgelegt ist die [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

Nur noch besteht darin, definieren die `thumbIds` zeichenbare Ressourcen.

Führen Sie die Anwendung aus. Das Rasterlayout sollte etwa wie folgt aussehen:

[![Beispielhafter Screenshot der GridView, die 15-Images anzeigen](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

Experimentieren Sie mit dem Verhalten von der [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) und [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
Elemente, indem Sie deren Eigenschaften anpassen. Beispielsweise anstelle von [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) versuchen Sie es mit [ `SetAdjustViewBounds()` ](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/).


## <a name="references"></a>Verweise

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
