---
title: Ein einfaches RecyclerView-Beispiel
description: Eine Beispiel-app, die veranschaulicht, wie Sie mit der RecyclerView.
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/30/2018
ms.openlocfilehash: d71c4f0f3221d06c22876329a5933273d8d6f92d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61184689"
---
# <a name="a-basic-recyclerview-example"></a>Ein einfaches RecyclerView-Beispiel

Um zu verstehen wie `RecyclerView` funktioniert in einer typischen Anwendung, um dieses Thema erläutert die [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) Beispiel-app, die ein einfaches Codebeispiel, das verwendet `RecyclerView` um eine umfangreiche Sammlung von Fotos anzuzeigen: 

[![Die beiden Screenshots einer RecyclerView-App, die CardViews verwendet wird, zum Anzeigen von Fotos](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** verwendet [CardView](~/android/user-interface/controls/card-view.md) zum Implementieren von Textelementen Foto in die `RecyclerView` Layout. Aufgrund der `RecyclerView`die Leistungsvorteile, diese Beispiel-app kann schnell eine große Sammlung von Fotos nahtlos und ohne nennenswerte Verzögerungen scrollen.


### <a name="an-example-data-source"></a>Eine Beispiel-Datenquelle

In dieser Beispiel-app, eine Datenquelle "Fotoalbum" (dargestellt durch die `PhotoAlbum` Klasse) stellt `RecyclerView` mit Inhalt des Elements.
`PhotoAlbum` ist eine Sammlung von Fotos mit Beschriftungen an. Wenn Sie es instanziieren, erhalten Sie eine vorgefertigte Sammlung von 32 Fotos:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Jede Instanz Foto in `PhotoAlbum` macht die Eigenschaften, die Ihnen ermöglichen, lesen Sie die Image-Ressourcen-ID, `PhotoID`, und die Beschriftungszeichenfolge `Caption`. Die Sammlung von Fotos ist so organisiert, dass jedes Foto von einem Indexer zugegriffen werden kann. Z. B. Zugriff auf die folgenden Codezeilen, die imageressourcen-ID und die Beschriftung für das zehnte Foto in der Auflistung:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` bietet außerdem eine `RandomSwap` -Methode, die Sie aufrufen können, um das erste Foto in der Auflistung mit einem zufällig ausgewählten Foto an anderer Stelle in der Auflistung zu wechseln:

```csharp
mPhotoAlbum.RandomSwap ();
```

Da die Implementierungsdetails der `PhotoAlbum` sind nicht relevant für ein Verständnis `RecyclerView`, `PhotoAlbum` Quellcode ist hier nicht angezeigt. Der Quellcode `PhotoAlbum` finden Sie unter [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) in die [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) Beispiel-app.


### <a name="layout-and-initialization"></a>Layout und Initialisierung

Die Layoutdatei **Main.axml**, besteht aus einer `RecyclerView` innerhalb einer `LinearLayout`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Beachten Sie, dass Sie den vollqualifizierten Namen verwenden, müssen **android.support.v7.widget.RecyclerView** da `RecyclerView` wird in einer Support-Bibliothek verpackt. Die `OnCreate` -Methode der `MainActivity` dieses Layout initialisiert den Adapter instanziiert und Vorbereitung der zugrunde liegenden Datenquelle:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        // Prepare the data source:
        mPhotoAlbum = new PhotoAlbum ();

        // Instantiate the adapter and pass in its data source:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);

        // Set our view from the "main" layout resource:
        SetContentView (Resource.Layout.Main);

        // Get our RecyclerView layout:
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug the adapter into the RecyclerView:
        mRecyclerView.SetAdapter (mAdapter);
```

Dieser Code führt Folgendes aus:

1. Instanziiert die `PhotoAlbum` -Datenquelle.

2. Übergibt das Fotoalbum die Datenquelle an den Konstruktor des Adapters, `PhotoAlbumAdapter` (der später in diesem Handbuch definiert ist). 
   Beachten Sie, dass es eine bewährte Methode, die die Datenquelle als Parameter an den Konstruktor des Adapters übergeben berücksichtigt wird. 

3. Ruft die `RecyclerView` aus dem Layout.

4. Bindet den Adapter in der `RecyclerView` -Instanz durch Aufrufen der `RecyclerView` `SetAdapter` Methode wie oben gezeigt.

### <a name="layout-manager"></a>Layout-Manager

Jedes Element in der `RecyclerView` setzt sich aus einer `CardView` , eine Foto-Image und das Foto Beschriftung enthält (Details finden Sie unter der [Ansicht Inhaber](#view-holder) weiter unten im Abschnitt). Die vordefinierten `LinearLayoutManager` wird verwendet, um das Layout jedes `CardView` in vertikaler Ausrichtung an scrollen:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Dieser Code befindet sich in der Hauptaktivität `OnCreate` Methode. Der Konstruktor, der Layout-Manager erfordert eine *Kontext*, sodass die `MainActivity` übergeben wird, mithilfe von `this` wie oben gezeigt.

Anstatt die vordefinierten `LinearLayoutManager`, können Sie ein benutzerdefiniertes Layout-Manager, der zwei zeigt einbinden `CardView` Elemente Seite-an-Seite, einen Animationseffekt Geometriemodell durchlaufen Sie die Sammlung von Fotos zu implementieren. Weiter unten in diesem Leitfaden wird verdeutlicht, wie Sie das Layout zu ändern, indem der Austausch in ein anderes Layout-Manager angezeigt.

<a name="view-holder" />

### <a name="view-holder"></a>Inhaber der Ansicht

Die Ansicht Holder-Klasse wird aufgerufen, `PhotoViewHolder`. Jede `PhotoViewHolder` Instanz enthält Verweise auf die `ImageView` und `TextView` eines Elements zugeordnete Zeile, die im Layout eine `CardView` wie hier abgebildet:

[![Diagramm der mit einem ImageView und TextView CardView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` leitet sich von `RecyclerView.ViewHolder` und enthält Eigenschaften zum Speichern von Verweisen auf die `ImageView` und `TextView` im oben genannten Layout dargestellt.
`PhotoViewHolder` besteht aus zwei Eigenschaften und einen Konstruktor:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```
In diesem Codebeispiel wird die `PhotoViewHolder` Konstruktor wird einen Verweis auf die Ansicht des übergeordneten Element übergeben (die `CardView`), `PhotoViewHolder` umschließt. Beachten Sie, dass Sie immer das übergeordnete Element Elementansicht des Basiskonstruktors weiterleiten. Die `PhotoViewHolder` Konstruktoraufrufe `FindViewById` auf die Ansicht des übergeordneten Element einzelnen von seinen untergeordneten Ansicht verweisen `ImageView` und `TextView`, speichern die Ergebnisse in die `Image` und `Caption` Eigenschaften, bzw. Der Adapter übernimmt Referenzen anzeigen später aus dieser Eigenschaften, wenn dies aktualisiert `CardView`des untergeordneten Ansichten mit neuen Daten.

Weitere Informationen zu `RecyclerView.ViewHolder`, finden Sie unter den [RecyclerView.ViewHolder Klassenverweis](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adapter

Der Adapter lädt jede `RecyclerView` Zeile mit Daten für ein bestimmtes Foto. Für ein bestimmtes Foto an der Zeilenposition *P*, z. B. der Adapter auch die zugeordneten Daten an der Position *P* innerhalb der Datenquelle und kopiert diese auf die Zeile Datenelement an Position *P* in die `RecyclerView` Auflistung. Der Adapter verwendet den Inhaber der Ansicht, um die Verweise für Suchen der `ImageView` und `TextView` an dieser Position aus, sodass es nicht wiederholten Aufrufen `FindViewById` für die Ansichten, da der Benutzer scrollt durch die Foto-Sammlung und Ansichten wiederverwendet.

In **RecyclerViewer**, ergibt sich eine Klasse des Adapters aus `RecyclerView.Adapter` erstellen `PhotoAlbumAdapter`:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;

    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }
    ...
}
```

Die `mPhotoAlbum` Member enthält, die Datenquelle (das Fotoalbum), die an den Konstruktor übergeben wird, der Konstruktor kopiert das Fotoalbum in diese Membervariable. Die folgenden erforderlichen `RecyclerView.Adapter` Methoden implementiert werden:

-   **`OnCreateViewHolder`** &ndash; Instanziiert die Datei und Ansicht Inhaber der Element-Layout.

-   **`OnBindViewHolder`** &ndash; Lädt die Daten an der angegebenen Position in den Ansichten, deren Verweise in den Inhaber der angegebenen Ansicht gespeichert sind.

-   **`ItemCount`** &ndash; Gibt die Anzahl der Elemente in der Datenquelle zurück.

Diese Methoden werden von der Layout-Manager aufgerufen, während sie Elemente in Positionierung ist die `RecyclerView`. Die Implementierung dieser Methoden wird in den folgenden Abschnitten untersucht.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Der Layout-Manager ruft `OnCreateViewHolder` bei der `RecyclerView` benötigt einen neue Ansicht Inhaber ein Element darstellen. `OnCreateViewHolder` vergrößert die Ansicht "Element" aus der Ansicht Layoutdatei und dient als Wrapper für die Ansicht in einem neuen `PhotoViewHolder` Instanz. Die `PhotoViewHolder` Konstruktor sucht und speichert Sie Verweise auf untergeordnete Ansichten in das Layout, wie zuvor beschrieben in [Ansicht Inhaber](#view-holder).

Jedes Zeilenelement wird dargestellt, indem eine `CardView` , enthält ein `ImageView` (für das Foto) und ein `TextView` (für die Beschriftung). Dieses Layout befindet sich in der Datei **PhotoCardView.axml**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:card_view="http://schemas.android.com/apk/res-auto"
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="fill_parent"
    android:layout_height="wrap_content">
    <android.support.v7.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        card_view:cardElevation="4dp"
        card_view:cardUseCompatPadding="true"
        card_view:cardCornerRadius="5dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="vertical"
            android:padding="8dp">
            <ImageView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:id="@+id/imageView"
                android:scaleType="centerCrop" />
            <TextView
                android:layout_width="match_parent"
                android:layout_height="wrap_content"
                android:textAppearance="?android:attr/textAppearanceMedium"
                android:textColor="#333333"
                android:text="Caption"
                android:id="@+id/textView"
                android:layout_gravity="center_horizontal"
                android:layout_marginLeft="4dp" />
        </LinearLayout>
    </android.support.v7.widget.CardView>
</FrameLayout>
```

Dieses Layout stellt eine einzelne Zeile-Element in der `RecyclerView`. Die `OnBindViewHolder` Methode (siehe unten) kopiert Daten aus der Datenquelle in die `ImageView` und `TextView` dieses Layout.
`OnCreateViewHolder` Vergrößert dieses Layout für ein bestimmtes Foto Position in der `RecyclerView` und instanziiert ein neues `PhotoViewHolder` Instanz (die sucht und speichert Verweise auf die `ImageView` und `TextView` untergeordnete Ansichten in der zugeordneten `CardView` Layout):

```csharp
public override RecyclerView.ViewHolder
    OnCreateViewHolder (ViewGroup parent, int viewType)
{
    // Inflate the CardView for the photo:
    View itemView = LayoutInflater.From (parent.Context).
                Inflate (Resource.Layout.PhotoCardView, parent, false);

    // Create a ViewHolder to hold view references inside the CardView:
    PhotoViewHolder vh = new PhotoViewHolder (itemView);
    return vh;
}

```

Die resultierende Ansicht Inhaber Instanz `vh`, an den Aufrufer (der Layout-Manager) zurückgegeben.


#### <a name="onbindviewholder"></a>OnBindViewHolder

Kann sich bei der Layout-Manager zum Anzeigen einer bestimmten Ansicht in der `RecyclerView`des sichtbaren Bildschirmbereichs, ruft des Adapters die `OnBindViewHolder` Methode, um das Element an der angegebenen Position mit Inhalt aus der Datenquelle füllen. `OnBindViewHolder` Ruft die Foto-Informationen für die angegebene Zeilenposition (das Foto des Image-Ressource und die Zeichenfolge für das Foto des Beschriftung), und kopiert diese Daten in der zugehörigen Ansichten. Ansichten sind über Verweise in das Ansichtsobjekt für die Inhaber gespeichert (die durch Übergeben der `holder` Parameter):

```csharp
public override void
    OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
{
    PhotoViewHolder vh = holder as PhotoViewHolder;

    // Load the photo image resource from the photo album:
    vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);

    // Load the photo caption from the photo album:
    vh.Caption.Text = mPhotoAlbum[position].Caption;
}
```

Das übergebene Inhaber Ansichtsobjekt muss zuerst in die abgeleitete Sicht Inhaber Typ umgewandelt werden (in diesem Fall `PhotoViewHolder`) bevor sie verwendet wird.
Der Adapter lädt die imageressource in die Sicht, auf die Ansicht des Karteninhabers `Image` -Eigenschaft, und kopiert Sie den Beschriftungstext in die Sicht, auf die Ansicht des Karteninhabers `Caption` Eigenschaft. Dies *bindet* die zugeordnete Ansicht mit den Daten.

Beachten Sie, dass `OnBindViewHolder` ist der Code, der direkt mit der Struktur der Daten behandelt. In diesem Fall `OnBindViewHolder` versteht das Zuordnen der `RecyclerView` Element Position in der Datenquelle des zugeordneten Datenelements. Die Zuordnung ist in diesem Fall einfach, da die Position als einen Arrayindex in das Fotoalbum verwendet werden kann; komplexere Datenquellen erfordern jedoch möglicherweise zusätzlichen Code zu, um eine solche Zuordnung herzustellen.


#### <a name="itemcount"></a>ItemCount

Die `ItemCount` Methode gibt die Anzahl der Elemente in der Sammlung zurück. In der Beispiel-Foto-Viewer-app ist die Anzahl der Elemente die Anzahl der Fotos im Fotoalbum:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Weitere Informationen zu `RecyclerView.Adapter`, finden Sie unter den [RecyclerView.Adapter Klassenverweis](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Letzte Schritte alle

Die resultierende `RecyclerView` Implementierung für die Beispiel-Foto-app besteht aus `MainActivity` Code, der die Datenquelle, die Layout-Manager und den Adapter erstellt. `MainActivity` erstellt die `mRecyclerView` Instanz, der Datenquelle und den Adapter instanziiert und in der Layout-Manager und der Adapter integriert wird:

```csharp
public class MainActivity : Activity
{
    RecyclerView mRecyclerView;
    RecyclerView.LayoutManager mLayoutManager;
    PhotoAlbumAdapter mAdapter;
    PhotoAlbum mPhotoAlbum;

    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);
        mPhotoAlbum = new PhotoAlbum();
        SetContentView (Resource.Layout.Main);
        mRecyclerView = FindViewById<RecyclerView> (Resource.Id.recyclerView);

        // Plug in the linear layout manager:
        mLayoutManager = new LinearLayoutManager (this);
        mRecyclerView.SetLayoutManager (mLayoutManager);

        // Plug in my adapter:
        mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
        mRecyclerView.SetAdapter (mAdapter);
    }
}

```

`PhotoViewHolder` Sucht und speichert die Verweise anzeigen:

```csharp
public class PhotoViewHolder : RecyclerView.ViewHolder
{
    public ImageView Image { get; private set; }
    public TextView Caption { get; private set; }

    public PhotoViewHolder (View itemView) : base (itemView)
    {
        // Locate and cache view references:
        Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
        Caption = itemView.FindViewById<TextView> (Resource.Id.textView);
    }
}
```

`PhotoAlbumAdapter` implementiert die drei erforderliche Methode überschreibt:

```csharp
public class PhotoAlbumAdapter : RecyclerView.Adapter
{
    public PhotoAlbum mPhotoAlbum;
    public PhotoAlbumAdapter (PhotoAlbum photoAlbum)
    {
        mPhotoAlbum = photoAlbum;
    }

    public override RecyclerView.ViewHolder
        OnCreateViewHolder (ViewGroup parent, int viewType)
    {
        View itemView = LayoutInflater.From (parent.Context).
                    Inflate (Resource.Layout.PhotoCardView, parent, false);
        PhotoViewHolder vh = new PhotoViewHolder (itemView);
        return vh;
    }

    public override void
        OnBindViewHolder (RecyclerView.ViewHolder holder, int position)
    {
        PhotoViewHolder vh = holder as PhotoViewHolder;
        vh.Image.SetImageResource (mPhotoAlbum[position].PhotoID);
        vh.Caption.Text = mPhotoAlbum[position].Caption;
    }

    public override int ItemCount
    {
        get { return mPhotoAlbum.NumPhotos; }
    }
}
```

Wenn dieser Code kompiliert und ausgeführt wird, wird das grundlegende Foto-app anzeigen, wie in den folgenden Screenshots gezeigt erstellt:

[![Die beiden Screenshots des Fotos-app mit der Region mit vertikalem Bildlauf Fotokarten anzeigen](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Wenn Schatten nicht gezeichnet werden (wie im Screenshot oben dargestellt), bearbeiten Sie **Properties/Androidmanifest.XML** und fügen Sie die folgenden attributeinstellung, die `<application>` Element:

```xml
android:hardwareAccelerated="true"
```

Diese einfache app unterstützt nur das Fotoalbum durchsuchen. Er reagiert nicht Element Touch-Ereignissen, noch werden Änderungen an den zugrunde liegenden Daten behandelt. Diese Funktionalität wurde [Erweitern des Beispiels RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).




### <a name="changing-the-layoutmanager"></a>Ändern die LayoutManager

Aufgrund der `RecyclerView`die Flexibilität, es ist einfach so ändern Sie die app, um ein anderes Layout-Manager verwenden. Im folgenden Beispiel wird es geändert, um das Fotoalbum mit einem Rasterlayout an, die einen horizontalen Bildlauf durchführt, anstatt mit einem linearen vertikalen Layout anzuzeigen. Zu diesem Zweck die Instanziierung der Layout-Manager wird so geändert, verwenden Sie die `GridLayoutManager` wie folgt:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Diese codeänderung ersetzt den vertikalen `LinearLayoutManager` mit einem `GridLayoutManager` bereitstellt, die ein Raster besteht aus zwei Zeilen, die in horizontaler Richtung zu scrollen. Wenn Sie kompilieren und führen die app erneut aus, sehen Sie sich, dass die Fotos in einem Raster angezeigt werden und Durchführen eines Bildlaufs horizontale und vertikale ist:

[![Beispielhafter Screenshot der app mit horizontalen Bildlauf von Fotos in einem Raster](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Ändern Sie nur eine Codezeile erforderlich, ist es möglich, Sie ändern die Anzeigen von Fotos-app, um ein anderes Layout mit unterschiedlichem Verhalten verwenden.
Beachten Sie, dass weder die Layout-XML als auch den Adaptercode geändert werden, um den Layoutstil ändern mussten. 

Im nächsten Thema [Erweitern des Beispiels RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), dieses einfache Beispiel-app wird erweitert, um die Element-Click-Ereignisse behandeln, und aktualisieren Sie `RecyclerView` Wenn die Änderungen die zugrunde liegenden Datenquelle.



## <a name="related-links"></a>Verwandte Links

- [RecyclerViewer (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView-Komponenten und Funktionen](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Erweitern Sie im RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
