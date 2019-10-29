---
title: Erweitern des recyclerview-Beispiels
description: Hinzufügen von Element-Click-Ereignis Handlern zur Beispiel-app "recyclerview".
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 07/13/2018
ms.openlocfilehash: d50971c1ef0064463e576a895729ad84577e1788
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73028879"
---
# <a name="extending-the-recyclerview-example"></a>Erweitern des recyclerview-Beispiels

Die Standard-APP, die in [einem grundlegenden recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) beschrieben wird, ist nicht sehr &ndash; Sie führt einfach einen Bildlauf durch und zeigt eine Liste mit einer Liste von Fotoelementen an In realen Anwendungen erwarten Benutzer, dass Sie mit der APP interagieren können, indem Sie auf Elemente in der Anzeige tippen. Außerdem kann sich die zugrunde liegende Datenquelle ändern (oder von der APP geändert werden), und der Inhalt der Anzeige muss mit diesen Änderungen konsistent bleiben. In den folgenden Abschnitten erfahren Sie, wie Sie Elemente mit Element Klick behandeln und `RecyclerView` aktualisieren, wenn sich die zugrunde liegende Datenquelle ändert.

### <a name="handling-item-click-events"></a>Behandeln von Element Klick Ereignissen

Wenn ein Benutzer ein Element im `RecyclerView`berührt, wird ein Element mit einem Element angezeigt, um die APP zu benachrichtigen, welches Element berührt wurde. Dieses Ereignis wird nicht durch `RecyclerView` &ndash; generiert, sondern durch die Element Ansicht (die in den Ansichts Halter umschließt wird) werden Berührungen und Berichte dieser Berührungen als Klick Ereignisse erkannt.

Die folgenden Schritte erläutern, wie Sie Elemente mit Element Klick behandeln, um zu veranschaulichen, wie die grundlegende Foto Anzeige-APP geändert wird, um zu melden, welche Fotos vom Benutzer berührt wurden. Wenn ein Element Click-Ereignis in der Beispiel-App auftritt, erfolgt die folgende Sequenz:

1. Die `CardView` des Fotos erkennt das Item-Click-Ereignis und benachrichtigt den Adapter.

2. Der Adapter leitet das Ereignis (mit Element Positionsinformationen) an den Element-Click-Handler der Aktivität weiter.

3. Der Element-Click-Handler der Aktivität antwortet auf das Element Click-Ereignis.

Zuerst wird ein ereignishandlermember mit dem Namen "`ItemClick`" der `PhotoAlbumAdapter`-Klassendefinition hinzugefügt:

```csharp
public event EventHandler<int> ItemClick;
```

Als nächstes wird `MainActivity`eine Element-Click-Ereignishandlermethode hinzugefügt.
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

In diesem einfachen Beispiel findet die Handlerregistrierung in der `OnCreate`-Methode der Hauptaktivität statt, aber eine Produktions-App kann den Handler in `OnResume` registrieren und die Registrierung in `OnPause` aufheben &ndash; Weitere Informationen finden Sie unter [Aktivitäts Lebenszyklus](~/android/app-fundamentals/activity-lifecycle/index.md) .

`PhotoAlbumAdapter` ruft nun `OnItemClick` auf, wenn ein Element Click-Ereignis empfangen wird. Der nächste Schritt ist das Erstellen eines Handlers im Adapter, der dieses `ItemClick` Ereignis auslöst. Die folgende Methode, `OnClick`, wird unmittelbar nach der `ItemCount`-Methode des Adapters hinzugefügt:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Diese `OnClick` Methode ist der *Listener* des Adapters für Element-Click-Ereignisse aus Element Ansichten. Bevor dieser Listener für eine Element Ansicht (über den Ansichts Halter der Element Ansicht) registriert werden kann, muss der `PhotoViewHolder`-Konstruktor geändert werden, um diese Methode als zusätzliches Argument zu akzeptieren, und `OnClick` bei der Element Ansicht `Click` Ereignis registrieren.
Dies ist der geänderte `PhotoViewHolder`-Konstruktor:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

Der `itemView`-Parameter enthält einen Verweis auf die `CardView`, die vom Benutzer berührt wurde. Beachten Sie, dass die Basisklasse des Ansichts Besitzers die Layoutposition des Elements (`CardView`) kennt, das es darstellt (über die `LayoutPosition`-Eigenschaft). diese Position wird an die `OnClick`-Methode des Adapters übermittelt, wenn ein Element-Click-Ereignis stattfindet. Die `OnCreateViewHolder`-Methode des Adapters wird so geändert, dass Sie die `OnClick`-Methode des Adapters an den Konstruktor des Ansichts Besitzers übergibt:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Wenn Sie nun die Beispiel-App zum Anzeigen von Fotos erstellen und ausführen, bewirkt das Tippen eines Fotos in der Anzeige, dass ein Toast angezeigt wird, der meldet, welches Foto berührt wurde:

[![Beispiel Toast, der angezeigt wird, wenn eine Foto Karte getippt wird](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Dieses Beispiel zeigt nur einen Ansatz für die Implementierung von Ereignis Handlern mit `RecyclerView`. Ein weiterer Ansatz, der hier verwendet werden könnte, ist das Platzieren von Ereignissen auf dem Ansichts Halter und das Abonnieren dieser Ereignisse durch den Adapter. Wenn die Beispiel Foto-APP eine Funktion zur Fotobearbeitung bereitgestellt hat, sind separate Ereignisse für die `ImageView` erforderlich, und die `TextView` in jedem `CardView`: Berührungen auf dem `TextView` würden ein `EditView` Dialogfeld starten, mit dem der Benutzer die Beschriftung bearbeiten kann. und berührt die `ImageView` würde ein Foto-TouchUp-Tool starten, mit dem der Benutzer das Foto anschneiden oder drehen kann. Abhängig von den Anforderungen Ihrer APP müssen Sie den besten Ansatz für die Behandlung von und die Reaktion auf Berührungs Ereignisse entwerfen.

Um zu veranschaulichen, wie `RecyclerView` aktualisiert werden kann, wenn das Dataset geändert wird, kann die Beispiel-App für die Foto Anzeige so geändert werden, dass ein Foto in der Datenquelle zufällig ausgewählt und mit dem ersten Foto ausgetauscht wird. Zuerst wird eine **zufällige Auswahl** Schaltfläche zum **Main. axml** -Layout der Beispiel Foto-app hinzugefügt:

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

Im nächsten Schritt wird der Code am Ende der `OnCreate`-Methode der Main-Aktivität hinzugefügt, um die Schaltfläche `Random Pick` im Layout zu suchen und einen Handler an ihn anzufügen:

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

Dieser Handler Ruft die `RandomSwap`-Methode des Foto-Albums auf, wenn die **zufällige Pick** -Taste getippt wird. Die `RandomSwap`-Methode tauscht ein Foto nach dem Zufallsprinzip mit dem ersten Foto in der Datenquelle aus und gibt dann den Index des zufällig ausgetauschten Fotos zurück. Wenn Sie die Beispiel-App mit diesem Code kompilieren und ausführen, führt das Tippen auf die **zufällige Auswahl** Schaltfläche nicht zu einer Änderung der Anzeige, da der `RecyclerView` die Änderung an der Datenquelle nicht erkennt.

Damit `RecyclerView` nach dem Ändern der Datenquelle aktualisiert wird, muss der **zufällige Pick** -Click-Handler so geändert werden, dass er die `NotifyItemChanged` Methode des Adapters für jedes Element in der Auflistung aufruft, das geändert wurde (in diesem Fall wurden zwei Elemente geändert: das erste Foto und das vertauscht Foto). Dies bewirkt, dass `RecyclerView` die Anzeige aktualisiert, sodass Sie mit dem neuen Status der Datenquelle konsistent ist:

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

Wenn nun die **zufällige Pick** -Schaltfläche angetippt ist, aktualisiert `RecyclerView` die Anzeige, um anzuzeigen, dass ein Foto weiter unten in der Auflistung durch das erste Foto in der Sammlung ausgetauscht wurde:

[![ersten Screenshot vor dem austauschen, zweiter Screenshot nach dem Austausch](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Natürlich konnten `NotifyDataSetChanged` aufgerufen werden, anstatt die beiden Aufrufe `NotifyItemChanged`vorzunehmen, aber dadurch würde `RecyclerView` die gesamte Auflistung aktualisieren, auch wenn sich nur zwei Elemente in der Auflistung geändert haben. Das Aufrufen von `NotifyItemChanged` ist wesentlich effizienter als das Aufrufen von `NotifyDataSetChanged`.

## <a name="related-links"></a>Verwandte Links

- [Recyclerviewer (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [Recyclerview-Teile und-Funktionalität](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Ein grundlegendes recyclerview-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
