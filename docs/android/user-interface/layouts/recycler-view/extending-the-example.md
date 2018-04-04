---
title: Erweitern Sie im RecyclerView-Beispiel
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 83147261a2d5458272f7e2bc105154da4308f4b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="extending-the-recyclerview-example"></a>Erweitern Sie im RecyclerView-Beispiel


Die basic-app, die in beschriebenen [ein einfaches RecyclerView Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) tatsächlich nicht viele Funktionen &ndash; einfach einen Bildlauf durchführt und zeigt eine feste Liste von Foto durchsuchen zu erleichtern. Erwarten Benutzer in echten Anwendungen um mit der app interagieren, indem Sie auf Elemente in der Anzeige zu können. Darüber hinaus die zugrunde liegenden Datenquelle ändern kann (oder von der Anwendung geändert werden), und den Inhalt der Anzeige müssen mit diesen Änderungen konsistent bleiben. In den folgenden Abschnitten erfahren Sie, wie Element-Click-Ereignisse behandeln, und aktualisieren Sie `RecyclerView` Wenn die Änderungen die zugrunde liegenden Datenquelle.


### <a name="handling-item-click-events"></a>Behandlung von Element-Click-Ereignisse

Wenn ein Benutzer ein Element in berührt die `RecyclerView`, ein Element-Click-Ereignis wird generiert, um die app zu benachrichtigen, welche Artikel verwendet wurde. Dieses Ereignis wird nicht generiert, indem `RecyclerView` &ndash; stattdessen die Elementansicht (die in der Ansicht Inhaber umschlossen wird) Fingereingaben erkennt und meldet diese Fingereingaben wie click-Ereignisse.

Zum Veranschaulichen der Element-Click-Ereignisse behandeln erläutern die folgenden Schritte aus, wie die grundlegenden Anzeigen von Fotos-app Bericht geändert wird, welche Foto wurde vom Benutzer berührt werden mussten. Wenn ein Element-Click-Ereignis in der Beispiel-app auftritt, findet die folgende Sequenz:

1.  Des Fotos `CardView` erkennt die Element-Click-Ereignis und benachrichtigt den Adapter.

2.  Der Adapter leitet das Ereignis (mit Informationen zur Position des Elements) die Aktivität Element-Click-Ereignishandler.

3.  Die Element-Click-Ereignishandler der Aktivität antwortet auf die Element-Click-Ereignis.

Zunächst ein Ereignismember-Handler aufgerufen `ItemClick` wird hinzugefügt, um die `PhotoAlbumAdapter` Klassendefinition:

```csharp
public event EventHandler<int> ItemClick;
```

Als Nächstes eine Element-Click-Ereignishandlermethode hinzugefügt `MainActivity`.
Dieser Ereignishandler zeigt kurz einen Toast, der angibt, welches Element Foto berührt wurde:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Als Nächstes wird eine Codezeile benötigt, um das Registrieren der `OnItemClick` Handler mit `PhotoAlbumAdapter`. Eine gute hierfür ist, sofort nach dem `PhotoAlbumAdapter` erstellt wird (in der Hauptaktivität `OnCreate` Methode):

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

`PhotoAlbumAdapter` jetzt angerufen `OnItemClick` eine Element-Click-Ereignis empfängt. Der nächste Schritt besteht, um einen Handler im Adapter zu erstellen, der dies auslöst `ItemClick` Ereignis. Die folgende Methode `OnClick`, sofort nach des Adapters wird hinzugefügt `ItemCount` Methode:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Dies `OnClick` Methode ist des Adapters *Listener* für Element-Click-Ereignisse aus Item Sichten. Vor diesem Listener mit einem Elementansicht (über die Elementansicht Ansicht Inhaber), registriert werden, kann die `PhotoViewHolder` Konstruktor muss geändert werden, um diese Methode als ein zusätzliches Argument akzeptiert, und registrieren Sie `OnClick` mit der Elementansicht `Click` Ereignis.
Hier wird das geänderte `PhotoViewHolder` Konstruktor:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

Die `itemView` Parameter enthält einen Verweis auf die `CardView` , wurde vom Benutzer berührt werden. Beachten Sie, dass die Basisklasse für die Sicht den Inhaber die Layoutposition des Elements weiß (`CardView`), den sie darstellt (über die `LayoutPosition` Eigenschaft), und diese Position wird an des Adapters übergeben `OnClick` Methode, wenn ein Element-Click-Ereignis stattfindet. Des Adapters `OnCreateViewHolder` Methode wurde geändert, um des Adapters übergeben `OnClick` Methode, um die Ansicht des Inhabers-Konstruktor:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Jetzt beim Erstellen und die Beispiel-anzeigen von Fotos-app ausführen, tippen Sie auf ein Foto in der Anzeige bewirkt, dass ein Popups angezeigt, die besagt, dass die Foto berührt wurde:

[![Beispiel für Popupbenachrichtigungen der angezeigt, wenn ein Foto Karte wird abgerufen.](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Dieses Beispiel zeigt nur eine Vorgehensweise zum Implementieren von Ereignishandlern mit `RecyclerView`. Ein anderer Ansatz, der hier verwendet werden konnte besteht darin zu platzieren Sie Ereignisse auf den Inhaber der Ansicht und des Adapters diese Ereignisse abonnieren. Sofern die Beispiel-app Fotos ein Foto Bearbeitungsfunktionen, separate Ereignisse würde werden muss, um die `ImageView` und die `TextView` innerhalb jedes `CardView`: berührt auf der `TextView` starten würden eine `EditView` Dialogfeld, in dem den Benutzer bearbeiten kann die Beschriftung und Fingereingaben auf die `ImageView` würde ein Foto Retusche-Tool, mit dem den Benutzer Zuschneiden oder drehen das Foto starten. Je nach den Anforderungen der app müssen Sie die beste Methode für die Behandlung von und reagieren auf Ereignisse berühren entwerfen.

Zur Veranschaulichung der Funktion wie `RecyclerView` können aktualisiert werden, wenn die DataSet-Änderungen, die Beispiel-app Anzeigen von Fotos geändert werden können, um nach dem Zufallsprinzip ein Foto in der Datenquelle auswählen, und tauschen es mit dem ersten Foto. Zuerst wird eine **zufällige auswählen** Schaltfläche wird das Beispiel Foto-app hinzugefügt **Main.axml** Layout:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Als Nächstes wird Code hinzugefügt, am Ende der Hauptaktivität `OnCreate` Methode zum Suchen der `Random Pick` im Layout Schaltfläche und einen Handler zuordnen:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

Dieser Handler Ruft die Albums `RandomSwap` Methode bei der **zufällige Pick** Schaltfläche abgerufen wird. Die `RandomSwap` Methode nach dem Zufallsprinzip vertauscht ein Foto mit dem ersten Foto in der Datenquelle, und gibt den Index des Fotos nach dem Zufallsprinzip ausgetauscht. Wenn Sie kompilieren und die Beispiel-app mit dem folgenden Code ausführen, tippen Sie auf die **zufällige Pick** Schaltfläche führt zu einer Änderung der Anzeige nicht zu, da die `RecyclerView` ist nicht bekannt ist, der die Änderung an der Datenquelle.

Zu `RecyclerView` aktualisiert, nachdem die Datenquelle ändert, die **zufällige Pick** klicken Sie auf Ereignishandler muss geändert werden, um die vom Adapter aufgerufen `NotifyItemChanged` Methode für jedes Element in der Auflistung, die geändert wurden (in diesem Fall zwei Elemente vorhanden sind geändert: das erste Foto und getauscht Foto). Dies bewirkt, dass `RecyclerView` auf seine Anzeige zu aktualisieren, sodass er mit dem neuen Status der Datenquelle übereinstimmt:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

Nun, dass bei der **zufällige auswählen** Schaltfläche wird abgerufen, `RecyclerView` aktualisiert die Anzeige, dass ein Foto weiter unten in der Auflistung mit dem ersten Foto in der Auflistung vertauscht wurden:

[![Erste Screenshot vor dem Austausch zweite Screenshot nach einem Austausch](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Natürlich `NotifyDataSetChanged` konnte aufgerufen wurden, anstatt die beiden Aufrufe von `NotifyItemChanged`, aber dies daher würde erzwingen `RecyclerView` um die gesamte Auflistung zu aktualisieren, obwohl nur zwei Elemente in der Auflistung geändert wurde. Aufrufen von `NotifyItemChanged` ist erheblich effizienter als das Aufrufen `NotifyDataSetChanged`.


## <a name="related-links"></a>Verwandte Links

- [RecyclerViewer (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView Teile und Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
