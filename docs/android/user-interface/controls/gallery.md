---
title: Android Gallery-Steuerelement
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: 6fe6b5a11473827eb716b0adf0fb0f3ae28a3538
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510282"
---
# <a name="xamarinandroid-gallery-control"></a>Xamarin. Android-Katalog-Steuerelement

[`Gallery`](xref:Android.Widget.Gallery)ein layoutwidget, das zum Anzeigen von Elementen in einer horizontalen Bildlauf Liste verwendet wird und die aktuelle Auswahl in der Mitte der Ansicht positioniert.

> [!IMPORTANT]
> Dieses Widget wurde in Android 4,1 (API-Ebene 16) als veraltet markiert. 

In diesem Tutorial erstellen Sie einen Katalog mit Fotos und zeigen dann jedes Mal eine Popup Meldung an, wenn ein Galerie Element ausgewählt wird.

Nachdem das `Main.axml` Layout für die Inhaltsansicht festgelegt wurde, `Gallery` wird das aus dem Layout mit [`FindViewById`](xref:Android.App.Activity.FindViewById*)aufgezeichnet.
Die[`Adapter`](xref:Android.Widget.AdapterView.RawAdapter)
die Eigenschaft wird dann verwendet, um einen benutzerdefinierten `ImageAdapter`Adapter () als Quelle für alle Elemente festzulegen, die in der dallery angezeigt werden sollen. Die `ImageAdapter` wird im nächsten Schritt erstellt.

Um etwas zu tun, wenn auf ein Element im Katalog geklickt wird, wird ein anonymer Delegat für das[`ItemClick`](xref:Android.Widget.AdapterView.ItemClick)
Ereignis. Sie zeigt ein[`Toast`](xref:Android.Widget.Toast)
Dadurch wird die Indexposition (null basiert) des ausgewählten Elements angezeigt (in einem realen Szenario könnte die Position verwendet werden, um für eine andere Aufgabe das Bild mit voller Größe zu erhalten).

Erstens gibt es einige Member-Variablen, einschließlich eines Arrays von IDs, die auf die im drawable Resources-Verzeichnis gespeicherten Bilder (**Ressourcen/drawable**) verweisen.

Der nächste Schritt ist der Klassenkonstruktor, in dem der[`Context`](xref:Android.Content.Context)
für eine `ImageAdapter` -Instanz wird definiert und in einem lokalen Feld gespeichert.
Anschließend werden einige der von [`BaseAdapter`](xref:Android.Widget.BaseAdapter)geerbten erforderlichen Methoden implementiert.
Der Konstruktor und das[`Count`](xref:Android.Widget.BaseAdapter.Count)
die Eigenschaft ist selbsterklärend. Gewohnt[`GetItem(int)`](xref:Android.Widget.BaseAdapter.GetItem*)
sollte das tatsächliche-Objekt an der angegebenen Position im Adapter zurückgeben, wird jedoch in diesem Beispiel ignoriert. Dasselbe[`GetItemId(int)`](xref:Android.Widget.BaseAdapter.GetItemId*)
sollte die Zeilen-ID des Elements zurückgeben, aber hier nicht erforderlich.

Die-Methode übernimmt das Anwenden eines Bilds auf ein[`ImageView`](xref:Android.Widget.ImageView)
Diese werden in das[`Gallery`](xref:Android.Widget.Gallery)
In dieser Methode ist der Member[`Context`](xref:Android.Content.Context)
wird verwendet, um einen neuen [`ImageView`](xref:Android.Widget.ImageView)zu erstellen.
Die[`ImageView`](xref:Android.Widget.ImageView)
wird vorbereitet, indem ein Image aus dem lokalen Array von drawable-Ressourcen angewendet wird, wobei das[`Gallery.LayoutParams`](xref:Android.Widget.Gallery.LayoutParams)
Höhe und Breite des Bilds, Festlegen der Skalierung auf das[`ImageView`](xref:Android.Widget.ImageView)
Dimensionen und schließlich das Festlegen des Hintergrunds für die Verwendung des im Konstruktor erworbenen anpassbaren-Attributs.

Weitere [`ImageView.ScaleType`](xref:Android.Widget.ImageView.ScaleType) Bild Skalierungs Optionen finden Sie unter.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Starten Sie ein neues Projekt mit dem Namen *hellogallery*.

[![Screenshot des neuen Android-Projekts im Dialogfeld "neue Projekt Mappe"](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

Suchen Sie nach einigen Fotos, die Sie verwenden möchten, oder [Laden Sie diese Beispiel Images herunter](https://developer.android.com/shareables/sample_images.zip).
Fügen Sie die Bilddateien dem **Ressourcen** -/Draw-Verzeichnis des Projekts hinzu. Legen Sie im Fenster **Eigenschaften** die Buildaktion für jede auf **androidresource**fest.

Öffnen Sie **Resources/Layout/Main. axml** , und fügen Sie Folgendes ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Öffnen `MainActivity.cs` Sie, und fügen Sie den folgenden Code für das[`OnCreate()`](xref:Android.App.Activity.OnCreate*)
anzuwenden

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    // Set our view from the "main" layout resource
    SetContentView (Resource.Layout.Main);

    Gallery gallery = (Gallery) FindViewById<Gallery>(Resource.Id.gallery);

    gallery.Adapter = new ImageAdapter (this);

    gallery.ItemClick += delegate (object sender, Android.Widget.AdapterView.ItemClickEventArgs args) {
        Toast.MakeText (this, args.Position.ToString (), ToastLength.Short).Show ();
    };
}
```

Erstellen Sie eine neue Klasse `ImageAdapter` namens Unterklassen [`BaseAdapter`](xref:Android.Widget.BaseAdapter):

```csharp
public class ImageAdapter : BaseAdapter
{
    Context context;

    public ImageAdapter (Context c)
    {
          context = c;
    }

    public override int Count { get { return thumbIds.Length; } }

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
          ImageView i = new ImageView (context);

          i.SetImageResource (thumbIds[position]);
          i.LayoutParameters = new Gallery.LayoutParams (150, 100);
          i.SetScaleType (ImageView.ScaleType.FitXy);

          return i;
    }

    // references to our images
    int[] thumbIds = {
            Resource.Drawable.sample_1,
            Resource.Drawable.sample_2,
            Resource.Drawable.sample_3,
            Resource.Drawable.sample_4,
            Resource.Drawable.sample_5,
            Resource.Drawable.sample_6,
            Resource.Drawable.sample_7
     };
}

```

Führen Sie die Anwendung aus. Dies sollte wie im folgenden Screenshot aussehen:

![Bildschirm Abbildung von "hellogallery" mit Beispielbildern](gallery-images/hellogallery3.png)

## <a name="references"></a>Verweise

- [`BaseAdapter`](xref:Android.Widget.BaseAdapter)
- [`Gallery`](xref:Android.Widget.Gallery)
- [`ImageView`](xref:Android.Widget.ImageView)

*Teile dieser Seite sind Änderungen, die auf der vom Android Open Source-Projekt erstellten und freigegebenen Arbeit basieren und gemäß den in der*
[*Creative Commons 2,5-Zuweisungs Lizenz*](http://creativecommons.org/licenses/by/2.5/)beschriebenen Begriffen verwendet werden.
