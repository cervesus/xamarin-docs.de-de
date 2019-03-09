---
title: Katalog
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/15/2018
ms.openlocfilehash: f9b73428531deeacc7bdea271cdc0c2872038e99
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57666958"
---
# <a name="gallery"></a>Katalog

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) ist ein Layout-Widget verwendet, um Elemente in einer Liste mit horizontalem Bildlauf anzuzeigen, und die aktuelle Auswahl in der Mitte der Ansicht positioniert.

> [!IMPORTANT]
> Dieses Widget wurde in Android 4.1 (API-Ebene 16) eingestellt. 

In diesem Tutorial haben Sie eine Sammlung von Fotos erstellen und dann eine eingeblendeten Nachricht jedes Mal, wenn ein Katalogelement ausgewählt ist.

Nach der `Main.axml` Layout für die Ansicht "Inhalt" festgelegt ist die `Gallery` wird erfasst, aus dem Layout mit [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
Die [`Adapter`](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/)
Eigenschaft wird dann zum Festlegen eines benutzerdefinierten Adapters ( `ImageAdapter`) als Quelle für alle Elemente in der Dallery angezeigt werden. Die `ImageAdapter` wird im nächsten Schritt erstellt.

Um etwas tun, wenn ein Element im Katalog geklickt wird, ist ein anonymer Delegat für abonniert die [`ItemClick`](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/)
Ereignis. Es zeigt eine [`Toast`](https://developer.xamarin.com/api/type/Android.Widget.Toast/)
der die Indexposition, die der ausgewählte Element (nullbasiert) anzeigt (in einem realen Szenario, die Position kann verwendet werden, die das Vergrößern für eine andere Aufgabe zu erhalten).

Einerseits bestehen einige Membervariablen, einschließlich der ein Array von IDs, die die Images gespeichert werden, in das Verzeichnis "drawable Resources" verweisen (**Ressourcen/drawable**).

Als Nächstes wird der Konstruktor der Klasse, wobei die [`Context`](https://developer.xamarin.com/api/type/Android.Content.Context/)
für eine `ImageAdapter` Instanz definiert und in ein lokales Feld gespeichert wird.
Dadurch wird als Nächstes einige erforderliche von geerbten Methoden implementiert [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).
Der Konstruktor und die [`Count`](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/)
Eigenschaft sind selbsterklärend. In der Regel [`GetItem(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/)
sollte das eigentliche Objekt an der angegebenen Position im Adapter zurückgeben, aber in diesem Beispiel wird ignoriert. Likewise, [`GetItemId(int)`](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/)
sollte die Zeilen-Id des Elements zurückgeben, aber es ist nicht erforderlich, hier.

Die Methode übernimmt die Aufgaben aus, um ein Bild, das Anwenden einer [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
eingebettet, die der [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
Bei dieser Methode das Element [`Context`](https://developer.xamarin.com/api/type/Android.Content.Context/)
Dient zum Erstellen eines neuen [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).
Die [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
wird vorbereitet, indem Sie das Anwenden eines Abbilds aus dem lokalen Array zeichenbare Ressourcen, die Einstellung der [`Gallery.LayoutParams`](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/)
Höhe und Breite des Bilds, das Festlegen der Skala Anpassen der [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)
Dimensionen und schließlich den Hintergrund zur Verwendung des Attributs vereinfacht abgerufen, die im Konstruktor festlegen.

Finden Sie unter [ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/) für andere Skalierungsoptionen Image.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Starten Sie ein neues Projekt namens *HelloGallery*.

[![Screenshot des neuen Android-Projekts im Dialogfeld "neue Projektmappe"](gallery-images/hellogallery1-sml.png)](gallery-images/hellogallery1.png#lightbox)

Finden Sie einige Fotos, die Sie verwenden möchten oder [Herunterladen dieser Beispielbilder](https://developer.android.com/shareables/sample_images.zip).
Hinzufügen der Bilddateien des Projekts **Ressourcen/Drawable** Verzeichnis. In der **Eigenschaften** legen die Build-Aktion für jedes **AndroidResource**.

Open **Resources/Layout/Main.axml** und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Open `MainActivity.cs` und fügen Sie den folgenden Code für die [`OnCreate()`](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/)
Methode:

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

Erstellen Sie eine neue Klasse namens `ImageAdapter` , Unterklassen [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/):

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

Führen Sie die Anwendung aus. Es sollte wie im folgenden Screenshot aussehen:

![Screenshot der HelloGallery Beispielbilder anzeigen](gallery-images/hellogallery3.png)



## <a name="references"></a>Verweise

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*Teile dieser Seite werden Änderungen, die basierend auf der Arbeit erstellt und freigegeben werden, indem Sie das Android Open Source-Projekt, und gemäß den Bedingungen, die in beschriebenen verwendet die*
[*Creative Commons 2.5 Attribution-Lizenz* ](http://creativecommons.org/licenses/by/2.5/).


