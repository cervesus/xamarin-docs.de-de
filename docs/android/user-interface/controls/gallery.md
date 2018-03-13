---
title: Katalog
ms.topic: article
ms.prod: xamarin
ms.assetid: 3112E68A-7853-B147-90A6-6295CA2C4CB5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: da6815a073d93379c8564f3ff91023deb20b0d55
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="gallery"></a>Katalog

[`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) Layout Widgets zum Anzeigen von Elementen in einer horizontal bildlauffähigen Liste verwendet wird, und die aktuelle Auswahl in der Mitte der Ansicht positioniert.

> [!IMPORTANT]
> Dieses Widget wurde in Android 4.1 (API Level 16) veraltet. 

In diesem Lernprogramm Sie eine Sammlung von Fotos erstellen und dann eine Toast-Meldung jedes Mal, wenn eines Katalogelements angezeigt.

Nach der `Main.axml` Layout festgelegt ist, bei der Inhaltsansicht der `Gallery` aufgezeichnet wird, aus dem Layout mit [ `FindViewById` ](https://developer.xamarin.com/api/member/Android.App.Activity.FindViewById/p/System.Int32/).
Die [ `Adapter` ](https://developer.xamarin.com/api/property/Android.Widget.AdapterView.RawAdapter/) Eigenschaft wird dann verwendet, um einen benutzerdefinierten Adapter festgelegt ( `ImageAdapter`) als Quelle für alle Elemente in der Dallery angezeigt werden. Die `ImageAdapter` wird im nächsten Schritt erstellt.

Um etwas tun, wenn ein Element im Katalog geklickt wird, ein anonymes Delegaten abonniert ist die [ `ItemClick` ](https://developer.xamarin.com/api/event/Android.Widget.AdapterView.ItemClick/) Ereignis. Es zeigt eine [ `Toast` ](https://developer.xamarin.com/api/type/Android.Widget.Toast/) , der die Indexposition, die der ausgewählte Element (nullbasiert) anzeigt (in einem realen Szenario müssen die Position kann verwendet werden vollständige Größe für eine andere Aufgabe abrufen).

Einerseits bestehen einige Membervariablen, z. B. ein Array von IDs, die die Bilder im Ressourcenverzeichnis zeichenbaren verweisen (**Ressourcen und Ausgaben möglich**).

Als Nächstes wird der Konstruktor der Klasse, in denen die [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) für eine `ImageAdapter` Instanz definiert und in ein lokales Feld gespeichert wird.
Als Nächstes einige erforderliche von geerbten Methoden implementiert [ `BaseAdapter` ](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/).
Der Konstruktor und die [ `Count` ](https://developer.xamarin.com/api/property/Android.Widget.BaseAdapter.Count/) Eigenschaft sind selbsterklärend. In der Regel [ `GetItem(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItem/p/System.Int32/) sollte das eigentliche Objekt an der angegebenen Position im Adapter zurückgeben, aber in diesem Beispiel wird ignoriert. Ebenso [ `GetItemId(int)` ](https://developer.xamarin.com/api/member/Android.Widget.BaseAdapter.GetItemId/p/System.Int32/) sollte die Zeilen-Id des Elements zurückgeben, aber es ist nicht erforderlich, hier.

Die Methode führt die Schritte, um ein Bild zum Anwenden einer [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) eingebettet, die der [ `Gallery` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery/) bei dieser Methode das Element [ `Context` ](https://developer.xamarin.com/api/type/Android.Content.Context/) ist zur Erstellung einer neuen [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/).
Die [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) wird vorbereitet, indem Sie ein Bild aus dem lokalen Array zeichenbaren Ressourcen festlegen Anwenden der [ `Gallery.LayoutParams` ](https://developer.xamarin.com/api/type/Android.Widget.Gallery+LayoutParams/) Höhe und Breite für das Bild, das Festlegen der Skala an der [ `ImageView` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView/) Dimensionen sowie das anschließende schließlich den Hintergrund des styleable Attributs abgerufen, die im Konstruktor festlegen.

Finden Sie unter [ `ImageView.ScaleType` ](https://developer.xamarin.com/api/type/Android.Widget.ImageView+ScaleType/) für andere Skalierungsoptionen Bild.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Starten Sie ein neues Projekt mit dem Namen *HelloGallery*.

![Screenshot des neuen Android-Projekt im Dialogfeld "neue Projektmappe"](gallery-images/hellogallery1.png)

Suchen Sie einige Fotos, die Sie verwenden möchten oder [herunterladen diese Beispielbilder](http://developer.android.com/shareables/sample_images.zip).
Hinzufügen der Bilddateien dem Projekt **Ressourcen/Drawable** Verzeichnis. In der **Eigenschaften** legen den Buildvorgang für jedes Element **AndroidResource**.

Open **Resources/Layout/Main.axml** , und fügen Sie Folgendes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Gallery xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/gallery"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content"
/>
```

Open `MainActivity.cs` , und fügen Sie den folgenden Code für die [ `OnCreate()` ](https://developer.xamarin.com/api/member/Android.App.Activity.OnCreate/p/Android.OS.Bundle/) Methode:

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

Führen Sie die Anwendung aus. Es sollte wie der folgende Screenshot aussehen:

![Screenshot der HelloGallery Beispielbilder anzeigen](gallery-images/hellogallery3.png)



## <a name="references"></a>Verweise

-   [`BaseAdapter`](https://developer.xamarin.com/api/type/Android.Widget.BaseAdapter/)
-   [`Gallery`](https://developer.xamarin.com/api/type/Android.Widget.Gallery/)
-   [`ImageView`](https://developer.xamarin.com/api/type/Android.Widget.ImageView/)

*Teile dieser Seite werden basierend auf der Arbeit erstellt und von Android Open Source-Projekt gemeinsam genutzt und verwendet entsprechend Begriffe, die in beschriebenen Änderungen der*
[*Creative Commons 2.5 Namensnennung Lizenz* ](http://creativecommons.org/licenses/by/2.5/).


