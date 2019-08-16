---
title: GridView
ms.prod: xamarin
ms.assetid: 6992C4FF-ECBB-3493-AEE6-3E063C1A8C54
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: f71c275dd2beee6aedf41ecd19c8a4a39ab5a36f
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69522642"
---
# <a name="xamarinandroid-gridview"></a>Xamarin. Android-GridView

[`GridView`](xref:Android.Widget.GridView)ist eine[`ViewGroup`](xref:Android.Views.ViewGroup)
, das Elemente in einem zweidimensionalen, scrollbaren Raster anzeigt. Die Raster Elemente werden mithilfe [`ListAdapter`](xref:Android.App.ListActivity.ListAdapter)von automatisch in das Layout eingefügt.

In diesem Tutorial erstellen Sie ein Raster mit Bild Miniaturansichten. Wenn ein Element ausgewählt wird, wird in einer Popup Meldung die Position des Bilds angezeigt.

Starten Sie ein neues Projekt mit dem Namen **hellogridview**.

Suchen Sie nach einigen Fotos, die Sie verwenden möchten, oder [Laden Sie diese Beispiel Images herunter](https://developer.android.com/shareables/sample_images.zip). Fügen Sie die Bilddateien dem **Ressourcen** -/Draw-Verzeichnis des Projekts hinzu. Legen Sie im Fenster **Eigenschaften** die Buildaktion für jede auf **androidresource**fest.

Öffnen Sie die Datei **Resources/Layout/Main. axml** , und fügen Sie Folgendes ein:

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

Dadurch [`GridView`](xref:Android.Widget.GridView) wird der gesamte Bildschirm ausgefüllt. Die Attribute sind vielmehr selbsterklärend. Weitere Informationen zu gültigen Attributen finden Sie in der [`GridView`](xref:Android.Widget.GridView) Referenz.

Öffnen `HelloGridView.cs` Sie, und fügen Sie den folgenden Code für das[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

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

Nachdem das **Main. axml** -Layout für die Inhaltsansicht festgelegt wurde [`GridView`](xref:Android.Widget.GridView) , wird das aus dem Layout [`FindViewById`](xref:Android.App.Activity.FindViewById*)mit aufgezeichnet. Die[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
die Eigenschaft wird dann verwendet, um einen benutzerdefinierten`ImageAdapter`Adapter () als Quelle für alle Elemente festzulegen, die im Raster angezeigt werden sollen. Die `ImageAdapter` wird im nächsten Schritt erstellt.

Wenn Sie auf ein Element im Raster klicken, wird ein anonymer Delegat für das [`ItemClick`](xref:Android.Widget.AdapterView.ItemClick) Ereignis abonniert.
Es zeigt einen [`Toast`](xref:Android.Widget.Toast) , der die Indexposition (null basiert) des ausgewählten Elements anzeigt (in einem realen Szenario kann die Position verwendet werden, um das Bild mit voller Größe für eine andere Aufgabe zu erhalten). Beachten Sie, dass Listenerklassen im Java-Stil anstelle von .NET-Ereignissen verwendet werden können.

Erstellen Sie eine neue Klasse `ImageAdapter` namens Unterklassen [`BaseAdapter`](xref:Android.Widget.BaseAdapter):

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

Zuerst werden einige erforderliche, von [`BaseAdapter`](xref:Android.Widget.BaseAdapter)geerbte Methoden implementiert. Der Konstruktor und die [`Count`](xref:Android.Widget.BaseAdapter.Count) -Eigenschaft sind selbsterklärend. Gewohnt[`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
sollte das tatsächliche-Objekt an der angegebenen Position im Adapter zurückgeben, wird jedoch in diesem Beispiel ignoriert. Dasselbe[`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
sollte die Zeilen-ID des Elements zurückgeben, aber hier nicht erforderlich.

Die erste erforderliche Methode ist [`GetView()`](xref:Android.Widget.BaseAdapter.GetView*).
Diese Methode erstellt eine neue[`View`](xref:Android.Views.View)
für jedes Abbild, das `ImageAdapter`der hinzugefügt wird. Wenn dies aufgerufen wird, wird ein[`View`](xref:Android.Views.View)
wird an das-Objekt übergebenen, bei dem es sich normalerweise um ein wiederverwendetes Objekt handelt (zumindest nachdem dies einmal aufgerufen wurde). Daher muss überprüft werden, ob das Objekt NULL ist. Wenn er NULL *ist* , wird ein[`ImageView`](xref:Android.Widget.ImageView)
wird instanziiert und mit den gewünschten Eigenschaften für die Bildpräsentation konfiguriert:

- [`LayoutParams`](xref:Android.Views.View.LayoutParameters)legt die Höhe und die Breite für die&mdash;Ansicht fest. Dadurch wird sichergestellt, dass jedes Bild, unabhängig von der Größe des drawable, geändert und entsprechend in diese Dimensionen angepasst wird.

- [`SetScaleType()`](xref:Android.Widget.ImageView.SetScaleType*)deklariert, dass Bilder in den Mittelpunkt zugeschnitten werden sollen (falls erforderlich).

- [`SetPadding(int, int, int, int)`](xref:Android.Views.View.SetPadding*)definiert den Leerraum für alle Seiten. (Beachten Sie Folgendes: Wenn die Bilder unterschiedliche Seitenverhältnisse aufweisen, bewirkt das Auffüllen des Bilds, dass weniger Auffüll Zeichen vorhanden sind, wenn es nicht mit den Dimensionen der ImageView identisch ist.)

Wenn die [`View`](xref:Android.Views.View) an die [`GetView()`](xref:Android.Widget.BaseAdapter.GetView*) Übergabe von *nicht* NULL ist, dann wird die lokale[`ImageView`](xref:Android.Widget.ImageView)
wird mit dem [`View`](xref:Android.Views.View) wiederverwendeten Objekt initialisiert.

Am Ende der[`GetView()`](xref:Android.Widget.BaseAdapter.GetView*)
-Methode, `position` wird die in die-Methode eingeführte Ganzzahl zum Auswählen eines `thumbIds` Bilds aus dem Array verwendet, das als Bildressource [`ImageView`](xref:Android.Widget.ImageView)für das festgelegt wird.

Sie müssen nur noch das `thumbIds` Array der drawable-Ressourcen definieren.

Führen Sie die Anwendung aus. Ihr Raster Layout sollte in etwa wie folgt aussehen:

[![Beispiel-Screenshot von GridView, das 15 Bilder anzeigt](grid-view-images/helloviews4.png)](grid-view-images/helloviews4.png#lightbox)

Experimentieren Sie mit den Verhalten von [`GridView`](xref:Android.Widget.GridView) und[`ImageView`](xref:Android.Widget.ImageView)
Elemente durch Anpassen ihrer Eigenschaften. Verwenden [`LayoutParams`](xref:Android.Views.View.LayoutParameters) Sie beispielsweise anstelle von [`SetAdjustViewBounds()`](xref:Android.Widget.ImageView.SetAdjustViewBounds*)Try.

## <a name="references"></a>Verweise

- [`GridView`](xref:Android.Widget.GridView)
- [`ImageView`](xref:Android.Widget.ImageView)
- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)

_Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der [Creative Commons 2,5-Zuweisungs Lizenz](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden._
