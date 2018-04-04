---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 82c82de912fd253d45e6343e2dd1c50e389c6371
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="gridview"></a>GridView

[`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) ist eine [ `ViewGroup` ](https://developer.xamarin.com/api/type/Android.Views.ViewGroup/) , die Elemente in einem zweidimensionalen, bildlauffähigen Datenblatt anzeigt. Der Rasterelemente werden automatisch eingefügt, um das Layout mithilfe einer [ `ListAdapter` ](https://developer.xamarin.com/api/property/Android.App.ListActivity.ListAdapter/).

In diesem Lernprogramm erstellen Sie ein Raster mit Bildminiaturansichten. Wenn ein Element ausgewählt ist, wird eine Meldung Toast die Position des Bilds angezeigt.

Starten Sie ein neues Projekt mit dem Namen **HelloGridView**.

Suchen Sie einige Fotos, die Sie verwenden möchten oder [herunterladen diese Beispielbilder](http://developer.android.com/shareables/sample_images.zip). Hinzufügen der Bilddateien dem Projekt **Ressourcen/Drawable** Verzeichnis. In der **Eigenschaften** legen den Buildvorgang für jedes Element **AndroidResource**.

Öffnen der **Resources/Layout/Main.axml** Datei und fügen Sie Folgendes:

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

Dies [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) füllt den gesamten Bildschirm. Attribute sind eher selbsterklärend. Weitere Informationen zu gültigen Attributen finden Sie unter der [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) Verweis.

Open `HelloGridView.cs` , und fügen Sie den folgenden Code für die [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) Methode:

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

Nach der **Main.axml** Layout festgelegt ist, bei der Inhaltsansicht der [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) aufgezeichnet wird, aus dem Layout mit [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/). Die [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Eigenschaft wird dann verwendet, um einen benutzerdefinierten Adapter festgelegt (`ImageAdapter`) als Quelle für alle Elemente im Raster angezeigt werden. Die `ImageAdapter` wird im nächsten Schritt erstellt.

Um etwas tun, wenn ein Element im Raster geklickt wird, ein anonymes Delegaten abonniert ist die [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) Ereignis.
Es zeigt eine [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) , der die Indexposition (nullbasiert) des ausgewählten Elements anzeigt (in einem realen Szenario müssen die Position kann verwendet werden vollständige Größe für eine andere Aufgabe abrufen). Beachten Sie, dass Java-Formatklassen Listener statt .NET Ereignisse verwendet werden können.

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

Zunächst einige erforderliche von geerbten Methoden implementiert [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/). Der Konstruktor und die [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) Eigenschaft sind selbsterklärend. In der Regel [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/) sollte das eigentliche Objekt an der angegebenen Position im Adapter zurückgeben, aber in diesem Beispiel wird ignoriert. Ebenso [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/) sollte die Zeilen-Id des Elements zurückgeben, aber es ist nicht erforderlich, hier.

Ist die erste Methode erforderlichen [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/).
Diese Methode erstellt ein neues [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) für jedes Bild hinzugefügt, um die `ImageAdapter`. Wenn dies aufgerufen wird, eine [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) übergeben wird, ist dem normalerweise ein wiederverwendet Objekt (mindestens Nachdem dies einmal aufgerufen wurde), daher ein Kontrollkästchen besteht, um festzustellen, ob das Objekt null ist. Wenn sie *ist* null, ein [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) instanziiert und mit den gewünschten Eigenschaften für die Image-Präsentation konfiguriert:

- [`LayoutParams`](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) Legt die Höhe und Breite für die Ansicht&mdash;Dadurch wird sichergestellt, dass jedes Image ist unabhängig von der Größe der zeichenbaren, angepasst und zugeschnitten in diesen Dimensionen nach Bedarf.

- [`SetScaleType()`](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetScaleType/) deklariert, dass Bilder (falls erforderlich) zur Mitte zugeschnitten werden soll.

- [`SetPadding(int, int, int, int)`](https://developer.xamarin.com/api/member/Android.Views.View.SetPadding/) definiert die Auffüllung für alle Seiten. (Beachten Sie, dass, wenn die Images unterschiedliche Seitenverhältnissen haben, klicken Sie dann weniger Auffüllung verursacht für weitere Zuschneiden des Bilds, wenn sie nicht über die Dimensionen übergeben, um die ImageView entspricht.)

Wenn die [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) übergeben [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) ist *nicht* Null, und klicken Sie dann auf der lokalen [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) wird initialisiert, indem die Wiederverwendung [ `View` ](https://developer.xamarin.com/api/type/Android.Views.View/) Objekt.

Am Ende der [ `GetView()` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetView/) -Methode, die `position` ganze Zahl, die an die Methode übergeben wird verwendet, um Wählen Sie ein Bild aus der `thumbIds` -Array, das als die Bildressource für festgelegt ist die [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).

Wird lediglich zum Definieren der `thumbIds` Array von zeichenbaren Ressourcen.

Führen Sie die Anwendung aus. Ihre Rasterlayout sollte etwa wie folgt aussehen:

[![Beispiel-Screenshot des GridView 15 Bilder anzeigen](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

Experimentieren Sie mit dem Verhalten von der [ `GridView` ](https://developer.xamarin.com/api/type/Android.Widget.GridView/) und [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) Elemente durch Anpassen ihrer Eigenschaften. Z. B. statt [ `LayoutParams` ](https://developer.xamarin.com/api/property/Android.Views.View.LayoutParameters/) versuchen Sie es mit [ `SetAdjustViewBounds()` ](https://developer.xamarin.com/api/member/Android.Widget.ImageView.SetAdjustViewBounds/).


## <a name="references"></a>Verweise

-   [`GridView`](https://developer.xamarin.com/api/type/Android.Widget.GridView/) 
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).

*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).
