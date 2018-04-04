---
title: Ein einfaches RecyclerView-Beispiel
ms.prod: xamarin
ms.assetid: A50520D2-1214-40E1-9B27-B0891FE11584
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: d5be838dcb5530ece76c3701d8fce10403622e8d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="a-basic-recyclerview-example"></a>Ein einfaches RecyclerView-Beispiel


Um zu verstehen, wie `RecyclerView` funktioniert in einer typischen Anwendung, in diesem Thema wird erklärt, die [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) Beispiel-app, ein einfaches Codebeispiel, das verwendet `RecyclerView` eine umfangreiche Auflistung von Fotos anzeigen: 

[![Zwei Screenshots RecyclerView-App, die CardViews verwendet wird, um Fotos anzeigen](recyclerview-example-images/01-recyclerviewer-sml.png)](recyclerview-example-images/01-recyclerviewer.png#lightbox)

**RecyclerViewer** verwendet [CardView](~/android/user-interface/controls/card-view.md) zum Implementieren jedes Foto-Element in der `RecyclerView` Layout. Aufgrund der `RecyclerView`des Leistungsvorteile, diese Beispiel-app kann schnell eine umfangreiche Auflistung von Fotos problemlos und ohne merkliche Verzögerungen Bildlauf.


### <a name="an-example-data-source"></a>Eine Beispiel-Datenquelle

In diesem Beispiel-app, eine Datenquelle "Fotoalben" (dargestellt durch die `PhotoAlbum` Klasse) bereitstellt `RecyclerView` mit Elementinhalt.
`PhotoAlbum` ist eine Auflistung von Fotos mit Untertiteln an. Wenn Sie es instanziieren, erhalten Sie eine vorgefertigte Sammlung von 32 Fotos aus:

```csharp
PhotoAlbum mPhotoAlbum = new PhotoAlbum ();
```

Jede Instanz Foto in `PhotoAlbum` macht die Eigenschaften, die Ihnen ermöglichen, lesen Sie die Bild-Ressourcen-ID `PhotoID`, und die Beschriftungszeichenfolge `Caption`. Die Auflistung von Fotos wird so organisiert, dass jedes Foto über einen Indexer zugegriffen werden kann. Z. B. Zugriff auf die folgenden Codezeilen, die Bildressourcen-ID und die Beschriftung für das zehnte Foto in der Auflistung:

```csharp
int imageId = mPhotoAlbum[9].ImageId;
string caption = mPhotoAlbum[9].Caption;
```

`PhotoAlbum` bietet außerdem eine `RandomSwap` Methode, die Sie aufrufen können, um das erste Foto in der Auflistung mit einem zufällig ausgewählten Foto an anderer Stelle in der Auflistung austauschen:

```csharp
mPhotoAlbum.RandomSwap ();
```

Da die Implementierungsdetails der `PhotoAlbum` sind nicht mit der Funktionsweise relevanten `RecyclerView`, die `PhotoAlbum` Quellcode ist hier nicht dargestellt. Quellcode und `PhotoAlbum` finden Sie unter [PhotoAlbum.cs](https://github.com/xamarin/monodroid-samples/blob/master/android5.0/RecyclerViewer/RecyclerViewer/PhotoAlbum.cs) in der [RecyclerViewer](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer/) Beispiel-app.


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

Beachten Sie, dass Sie den vollqualifizierten Namen verwenden, müssen **android.support.v7.widget.RecyclerView** da `RecyclerView` wird in einer Unterstützungsbibliothek verpackt. Die `OnCreate` Methode `MainActivity` initialisiert dieses Layout, den Adapter instanziiert und die zugrunde liegenden Datenquelle vorbereitet:

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

2. Übergibt das Fotoalbum-Datenquelle an den Konstruktor des Adapters, `PhotoAlbumAdapter` (weiter unten in diesem Handbuch definiert). 
   Beachten Sie, dass es eine bewährte Methode, die die Datenquelle als Parameter an den Konstruktor des Adapters übergeben berücksichtigt wird. 

3. Ruft die `RecyclerView` aus dem Layout.

4. Anbieterbibliothek den Adapter in der `RecyclerView` Instanz durch Aufrufen der `RecyclerView` `SetAdapter` Methode wie oben gezeigt.

### <a name="layout-manager"></a>Layout-Manager

Jedes Element in der `RecyclerView` besteht aus einer `CardView` , enthält ein Foto-Image und ein Foto Beschriftung (Details finden Sie der [Inhaber der Ansicht](#view-holder) weiter unten im Abschnitt). Die vordefinierten `LinearLayoutManager` wird verwendet, um das Layout jedes `CardView` in vertikaler Ausrichtung an Durchführen eines Bildlaufs:

```csharp
mLayoutManager = new LinearLayoutManager (this);
mRecyclerView.SetLayoutManager (mLayoutManager);
```

Dieser Code befindet sich in der Hauptaktivität `OnCreate` Methode. Der Konstruktor für den Layout-Manager benötigt eine *Kontext*, sodass die `MainActivity` übergeben wird, mithilfe von `this` wie oben dargestellt.

Anstatt die der Predefind `LinearLayoutManager`, eingebunden werden können, in ein benutzerdefiniertes Layout-Manager, der zwei zeigt `CardView` Elemente Seite-an-Seite, einen Animationseffekt Geometriemodell zum Durchlaufen der Auflistung von Fotos implementieren. Weiter unten in diesem Handbuch sehen Sie ein Beispiel dafür, wie das Layout zu ändern, indem der Austausch in ein anderes Layout-Manager.

<a name="view-holder" />

### <a name="view-holder"></a>Inhaber der Ansicht

Der Inhaber der Ansichtsklasse wird aufgerufen, `PhotoViewHolder`. Jede `PhotoViewHolder` Instanz enthält Verweise auf die `ImageView` und `TextView` eines Elements zugeordnete Zeile, die im Layout eine `CardView` wie hier Routerplan:

[![Diagramm der mit einem ImageView und TextView CardView](recyclerview-example-images/02-cardview-layout-sml.png)](recyclerview-example-images/02-cardview-layout.png#lightbox)

`PhotoViewHolder` leitet sich von `RecyclerView.ViewHolder` und enthält Eigenschaften zum Speichern von Verweisen auf die `ImageView` und `TextView` im obigen Layout dargestellt.
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
In diesem Codebeispiel wird die `PhotoViewHolder` Konstruktor ist ein Verweis auf die Ansicht des übergeordneten Element übergeben (die `CardView`), `PhotoViewHolder` umschließt. Beachten Sie, dass Sie immer das übergeordnete Element des Basiskonstruktors Elementansicht weiterleiten. Die `PhotoViewHolder` Konstruktoraufrufe `FindViewById` auf der übergeordneten Elementansicht aller seiner untergeordneten Ansicht verweisen suchen `ImageView` und `TextView`, speichern, die in der `Image` und `Caption` Eigenschaften bzw. Der Adapter überträgt Ansicht Verweise später aus dieser Eigenschaften, wenn dadurch wird aktualisiert `CardView`des untergeordneten Ansichten mit neuen Daten.

Weitere Informationen zu `RecyclerView.ViewHolder`, finden Sie unter der [RecyclerView.ViewHolder-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.ViewHolder.html).


### <a name="adapter"></a>Adapter

Der Adapter lädt jedes `RecyclerView` Zeile mit Daten für ein bestimmtes Foto. Für einen bestimmten Foto an der Zeilenposition *P*, z. B. sucht der Adapter die zugeordneten Daten an der Position *P* innerhalb der Datenquelle und kopiert diese auf die Zeile Datenelement an Position *P* in der `RecyclerView` Auflistung. Der Adapter verwendet den Inhaber der Ansicht nachgeschlagen werden die Verweise für die `ImageView` und `TextView` an dieser Position, daher keine wiederholten Aufrufen müssen `FindViewById` für die Ansichten, da der Benutzer führt einen Bildlauf durch die Foto-Auflistung und Ansichten wiederverwendet.

In **RecyclerViewer**, eine Adapterklasse abgeleitet `RecyclerView.Adapter` erstellen `PhotoAlbumAdapter`:

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

Die `mPhotoAlbum` Member enthält, die Datenquelle (Albums), der an den Konstruktor übergeben wird; der Konstruktor Albums in diese Membervariable kopiert. Die folgenden erforderlichen `RecyclerView.Adapter` Methoden implementiert werden:

-   **`OnCreateViewHolder`** &ndash; Instanziiert den Inhaber für das Layout Element der Datei- und anzeigen.

-   **`OnBindViewHolder`** &ndash; Lädt die Daten der angegebenen Position in der Sichten, deren Verweise in den Inhaber der angegebenen Ansicht gespeichert sind.

-   **`ItemCount`** &ndash; Gibt die Anzahl der Elemente in der Datenquelle zurück.

Der Layout-Manager ruft diese Methoden beim Positionieren von Elementen in ist die `RecyclerView`. Die Implementierung dieser Methoden wird in den folgenden Abschnitten untersucht.


#### <a name="oncreateviewholder"></a>OnCreateViewHolder

Der Layout-Manager ruft `OnCreateViewHolder` bei der `RecyclerView` benötigt einen neue Ansicht Inhaber ein Elements darstellen. `OnCreateViewHolder` vergrößert die Elementansicht aus der Ansicht Layoutdatei und dient als Wrapper für die Ansicht in einem neuen `PhotoViewHolder` Instanz. Die `PhotoViewHolder` Konstruktor sucht und speichert Verweise auf untergeordnete Ansichten im Layout, wie im zuvor beschriebenen [Ansicht Inhaber](#view-holder).

Jedes Zeilenelement wird dargestellt, indem eine `CardView` , enthält eine `ImageView` (für das Foto) und eine `TextView` (für die Beschriftung). Dieses Layout befindet sich in der Datei **PhotoCardView.axml**:

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

Dieses Layout stellt einen einzeiligen Element in der `RecyclerView`. Die `OnBindViewHolder` Methode (siehe unten) kopiert Daten aus der Datenquelle in die `ImageView` und `TextView` dieses Layout.
`OnCreateViewHolder` Vergrößert dieses Layout für ein bestimmtes Foto Position in der `RecyclerView` und instanziiert einen neuen `PhotoViewHolder` Instanz (die sucht und speichert Verweise auf die `ImageView` und `TextView` untergeordnete Ansichten in der zugeordneten `CardView` Layout):

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

Die resultierende Ansicht Inhaber Instanz `vh`, zurück an den Aufrufer (der Layout-Manager) zurückgegeben.


#### <a name="onbindviewholder"></a>OnBindViewHolder

Wenn der Layout-Manager ist bereit zum Anzeigen einer bestimmten Ansicht in der `RecyclerView`des sichtbaren Bildschirmbereichs, ruft des Adapters `OnBindViewHolder` Methode, um das Element an der Position der angegebenen Zeile mit dem Inhalt der Datenquelle füllen. `OnBindViewHolder` Ruft die Foto-Informationen für die angegebene Zeilenposition (das Foto Bildressource und die Zeichenfolge für das Foto Beschriftung) ab und kopiert diese Daten zu den zugehörigen Sichten. Sichten befinden sich über Verweise in das Ansichtsobjekt für Inhaber gespeichert (die durch Übergeben der `holder` Parameter):

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

Das übergebene Inhaber Ansichtsobjekt zuerst in die abgeleitete Sicht Inhaber Typ umgewandelt werden muss (in diesem Fall `PhotoViewHolder`), bevor er verwendet wird.
Der Adapter lädt die Bildressource in der Sicht verwiesen wird, von des Sicht Inhabers `Image` -Eigenschaft, und kopiert den Beschriftungstext in der Sicht verwiesen wird, von des Sicht Inhabers `Caption` Eigenschaft. Dies *bindet* die zugeordnete Ansicht mit den Daten.

Beachten Sie, dass `OnBindViewHolder` ist der Code, der direkt mit der Struktur der Daten behandelt. In diesem Fall `OnBindViewHolder` versteht zum Zuordnen der `RecyclerView` Element Position der zugehörigen Datenelement in der Datenquelle. Die Zuordnung ist in diesem Fall einfach, da die Position ein Arrayindex in Albums verwendet werden kann; komplexere Datenquellen erfordern jedoch zusätzlichen Code, um eine solche Zuordnung herzustellen.


#### <a name="itemcount"></a>ItemCount

Die `ItemCount` Methode gibt die Anzahl der Elemente in die Daten sammelt. In dem Beispiel Foto-Viewer-Apps ist die Anzahl der Elemente die Anzahl von Fotos in Albums:

```csharp
public override int ItemCount
{
    get { return mPhotoAlbum.NumPhotos; }
}
```

Weitere Informationen zu `RecyclerView.Adapter`, finden Sie unter der [RecyclerView.Adapter-Klassenreferenz](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.Adapter.html).


### <a name="putting-it-all-together"></a>Platzieren sie alle zusammen

Das resultierende `RecyclerView` Implementierung für die Beispiel-Foto-app besteht aus `MainActivity` Code, der die Datenquelle, Layout-Manager und der Adapter erstellt. `MainActivity` erstellt die `mRecyclerView` Instanz, die Datenquelle und den Adapter instanziiert und wird in der Layout-Manager und der Adapter integriert:

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

Wenn dieser Code kompiliert und ausgeführt wird, erstellt es die grundlegende Foto-app anzeigen, wie in den folgenden Screenshots dargestellt:

[![Zwei Screenshots Fotos-app mit vertikale Bildläufe Fotokarten anzeigen](recyclerview-example-images/03-recyclerviewer-basic-sml.png)](recyclerview-example-images/03-recyclerviewer-basic.png#lightbox)

Diese grundlegenden app unterstützt nur das Durchsuchen des Albums. Reagiert nicht um Ereignisse Touch-Element, und ist es an den zugrunde liegenden Daten behandelt werden. Diese Funktionalität ist hinzugefügt [Erweitern des Beispiels RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md).


### <a name="changing-the-layoutmanager"></a>Ändern die LayoutManager

Aufgrund der `RecyclerView`die Flexibilität, es ist einfach, ändern die app, um ein anderes Layout-Manager verwendet. Im folgenden Beispiel wird diese modifiziert, um Albums mit einem Rasterlayout, die einen horizontalen Bildlauf durchführt, anstatt mit einem linearen vertikalen Layout anzuzeigen. Zu diesem Zweck die Instanziierung der Layout-Manager geändert, sodass verwendet die `GridLayoutManager` wie folgt:

```csharp
mLayoutManager = new GridLayoutManager(this, 2, GridLayoutManager.Horizontal, false);
```

Diese codeänderung ersetzt die vertikale `LinearLayoutManager` mit einem `GridLayoutManager` präsentiert ein Raster setzt sich aus zwei Zeilen, die in horizontaler Richtung zu scrollen. Wenn Sie kompilieren und die app erneut ausführen, sehen Sie sich, dass die Fotos in einem Raster angezeigt werden und Durchführen eines Bildlaufs horizontale und vertikale ist:

[![Beispiel-Screenshot der app mit horizontalen Bildlauf Fotos in einem Raster](recyclerview-example-images/04-gridlayoutmanager-sml.png)](recyclerview-example-images/04-gridlayoutmanager.png#lightbox)

Ändern Sie nur eine Codezeile, ist möglich, die Anzeigen von Fotos-app, um ein anderes Layout mit verschiedenen Verhalten verwenden ändern.
Beachten Sie, dass weder das Layout-XML als auch der Adaptercode geändert werden musste, um den Layoutstil zu ändern. 

Im nächsten Thema [Erweitern des Beispiels RecyclerView](~/android/user-interface/layouts/recycler-view/extending-the-example.md), diese grundlegenden Beispiel-app wird erweitert, um die Element-Click-Ereignisse behandeln, und aktualisieren Sie `RecyclerView` Wenn die Änderungen die zugrunde liegenden Datenquelle.



## <a name="related-links"></a>Verwandte Links

- [RecyclerViewer (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView Teile und Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Erweitern Sie im RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/extending-the-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
