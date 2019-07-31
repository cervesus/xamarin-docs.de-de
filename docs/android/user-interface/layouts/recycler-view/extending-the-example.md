---
title: Erweitern des recyclerview-Beispiels
description: Hinzufügen von Element-Click-Ereignis Handlern zur Beispiel-app "recyclerview".
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: fd813427836b0250b84941eca54d6bbe6219518e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645333"
---
# <a name="extending-the-recyclerview-example"></a>Erweitern des recyclerview-Beispiels


Die grundlegende APP, die in [einem grundlegenden recyclerview](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) -Beispiel &ndash; beschrieben wird, führt einfach nur einen Bildlauf durch und zeigt eine fixierte Liste von Fotoelementen an, um das Durchsuchen In realen Anwendungen erwarten Benutzer, dass Sie mit der APP interagieren können, indem Sie auf Elemente in der Anzeige tippen. Außerdem kann sich die zugrunde liegende Datenquelle ändern (oder von der APP geändert werden), und der Inhalt der Anzeige muss mit diesen Änderungen konsistent bleiben. In den folgenden Abschnitten erfahren Sie, wie Sie Elemente mit Klick Ereignissen behandeln und aktualisieren `RecyclerView` , wenn sich die zugrunde liegende Datenquelle ändert.


### <a name="handling-item-click-events"></a>Behandeln von Element Klick Ereignissen

Wenn ein Benutzer ein Element im `RecyclerView`berührt, wird ein Element mit einem Element angezeigt, um die APP zu benachrichtigen, welches Element berührt wurde. Dieses Ereignis wird nicht von `RecyclerView` &ndash; generiert. die Element Ansicht (die in den Ansichts Halter umschließt) erkennt Berührungen und meldet diese Berührungen als Click-Ereignisse.

Die folgenden Schritte erläutern, wie Sie Elemente mit Element Klick behandeln, um zu veranschaulichen, wie die grundlegende Foto Anzeige-APP geändert wird, um zu melden, welche Fotos vom Benutzer berührt wurden. Wenn ein Element Click-Ereignis in der Beispiel-App auftritt, erfolgt die folgende Sequenz:

1.  Das Element des `CardView` Fotos erkennt das Element-Click-Ereignis und benachrichtigt den Adapter.

2.  Der Adapter leitet das Ereignis (mit Element Positionsinformationen) an den Element-Click-Handler der Aktivität weiter.

3.  Der Element-Click-Handler der Aktivität antwortet auf das Element Click-Ereignis.

Zuerst wird ein ereignishandlermember aufgerufen `ItemClick` , der der `PhotoAlbumAdapter` Klassendefinition hinzugefügt wird:

```csharp
public event EventHandler<int> ItemClick;
```

Anschließend wird eine Element-Click-Ereignishandlermethode `MainActivity`hinzugefügt.
Dieser Handler zeigt kurz einen Toast an, der angibt, welches Foto Element berührt wurde:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Als nächstes ist eine Codezeile erforderlich, um den `OnItemClick` Handler bei `PhotoAlbumAdapter`zu registrieren. Ein guter Ausgangspunkt ist, nachdem `PhotoAlbumAdapter` erstellt wurde: 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

In diesem grundlegenden Beispiel findet die `OnCreate` Handlerregistrierung in der-Methode der Hauptaktivität statt, aber eine Produktions-App registriert den Handler möglicherweise in `OnPause` `OnResume` und die Registrierung in &ndash; dem [Aktivitäts Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md) , um weitere Informationen zu erhalten. Informationen.

`PhotoAlbumAdapter`Ruft `OnItemClick` jetzt auf, wenn ein Element Click-Ereignis empfangen wird. Der nächste Schritt ist das Erstellen eines Handlers im Adapter, der dieses `ItemClick` Ereignis auslöst. Die folgende Methode `OnClick`wird direkt nach der- `ItemCount` Methode des Adapters hinzugefügt:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Diese `OnClick` Methode ist der *Listener* des Adapters für Element-Click-Ereignisse aus Element Ansichten. Bevor dieser Listener für eine Element Ansicht (über den Ansichts Halter der Element Ansicht) registriert werden kann, `PhotoViewHolder` muss der Konstruktor geändert werden, um diese Methode als zusätzliches Argument zu akzeptieren, `OnClick` und sich beim Element `Click` Ansichts Ereignis registrieren.
Hier ist der geänderte `PhotoViewHolder` Konstruktor:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

Der `itemView` -Parameter enthält einen Verweis auf `CardView` den, der vom Benutzer berührt wurde. Beachten Sie, dass die Basisklasse des Ansichts Besitzers die Layoutposition`CardView`des Elements () kennt, das `LayoutPosition` es darstellt (über die-Eigenschaft). diese Position wird `OnClick` an die-Methode des Adapters übermittelt, wenn ein Item-Click-Ereignis stattfindet. Die- `OnCreateViewHolder` Methode des Adapters wird so geändert, dass Sie `OnClick` die-Methode des Adapters an den Konstruktor des Ansichts Besitzers übergibt:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Wenn Sie nun die Beispiel-App zum Anzeigen von Fotos erstellen und ausführen, bewirkt das Tippen eines Fotos in der Anzeige, dass ein Toast angezeigt wird, der meldet, welches Foto berührt wurde:

[![Beispiel Toast, der angezeigt wird, wenn eine Foto Karte getippt wird](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

In diesem Beispiel wird nur ein Ansatz für die Implementierung von Ereignis `RecyclerView`Handlern mit veranschaulicht. Ein weiterer Ansatz, der hier verwendet werden könnte, ist das Platzieren von Ereignissen auf dem Ansichts Halter und das Abonnieren dieser Ereignisse durch den Adapter. Wenn die Foto-Beispiel-App eine Funktion zur Fotobearbeitung bereitgestellt hat, sind separate `ImageView` Ereignisse für `TextView` die und `CardView`die innerhalb der `TextView` beiden Elemente `EditView` erforderlich mit der Beschriftung und dem `ImageView` -Wert wird ein Foto TouchUp-Tool gestartet, mit dem der Benutzer das Foto anschneiden oder drehen kann. Abhängig von den Anforderungen Ihrer APP müssen Sie den besten Ansatz für die Behandlung von und die Reaktion auf Berührungs Ereignisse entwerfen.

Um zu veranschaulichen `RecyclerView` , wie aktualisiert werden kann, wenn das Dataset geändert wird, kann die Beispiel-App für die Foto Anzeige so geändert werden, dass ein Foto in der Datenquelle nach dem Zufallsprinzip ausgewählt und mit dem ersten Foto ausgetauscht wird. Zuerst wird eine **zufällige Auswahl** Schaltfläche zum **Main. axml** -Layout der Beispiel Foto-app hinzugefügt:

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

Im nächsten Schritt wird Code am Ende der-Methode der Main- `OnCreate` Aktivität hinzugefügt, `Random Pick` um die Schaltfläche im Layout zu suchen und einen Handler an ihn anzufügen:

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

Dieser Handler Ruft die-Methode des `RandomSwap` Foto-Albums auf, wenn die **zufällige Pick** -Taste getippt wird. Die `RandomSwap` Methode tauscht ein Foto nach dem Zufallsprinzip mit dem ersten Foto in der Datenquelle aus und gibt dann den Index des zufällig vertauschten Fotos zurück. Wenn Sie die Beispiel-App mit diesem Code kompilieren und ausführen, führt das Tippen auf die **zufällige Auswahl** Schaltfläche nicht zu einer `RecyclerView` Änderung der Anzeige, da der die Änderung an der Datenquelle nicht kennt.

Um nach `RecyclerView` dem Ändern der Datenquelle weiterhin aktualisiert zu werden, muss der **zufällige Pick** -Click-Handler so geändert `NotifyItemChanged` werden, dass er die Methode des Adapters für jedes Element in der Auflistung aufruft, das geändert wurde (in diesem Fall wurden zwei Elemente geändert: das erste Foto und dem vertauschten Foto). Dies bewirkt `RecyclerView` , dass die Anzeige der Anzeige so aktualisiert wird, dass Sie mit dem neuen Status der Datenquelle konsistent ist:

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

Wenn nun die **zufällige Pick** -Taste getippt wird `RecyclerView` , aktualisiert die Anzeige, um anzuzeigen, dass ein Foto weiter unten in der Auflistung mit dem ersten Foto in der Auflistung ausgetauscht wurde:

[![Erster Screenshot vor dem austauschen, zweiter Screenshot nach dem Austausch](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Selbstverständlich `NotifyDataSetChanged` hätte aufgerufen werden können, statt die beiden `NotifyItemChanged`Aufrufe von vorzunehmen, `RecyclerView` aber dies würde dazu führen, dass die gesamte Auflistung aktualisiert wird, obwohl nur zwei Elemente in der Auflistung geändert wurden. Der `NotifyItemChanged` Aufruf von ist wesentlich effizienter als `NotifyDataSetChanged`das Aufrufen von.


## <a name="related-links"></a>Verwandte Links

- [Recyclerviewer (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Recyclerview-Teile und-Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Ein grundlegendes recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
