---
title: Erweitern Sie im RecyclerView-Beispiel
description: Hinzufügen von Element-Click-Ereignishandler zu der RecyclerView-Beispiel-app ein.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: eca0f58a470228ce8e6331defe88c1ef727cef57
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61036085"
---
# <a name="extending-the-recyclerview-example"></a>Erweitern Sie im RecyclerView-Beispiel


Die basic-app, die in beschriebenen [ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) eigentlich nicht viel &ndash; es einfach führt einen Bildlauf durch, und zeigt eine feste Liste der Foto-Elemente zum Browsen zu ermöglichen. In realen Anwendungen erwarten, dass Benutzer mit der app interagieren, indem Sie auf Elemente in der Anzeige können. Darüber hinaus wird die zugrunde liegenden Datenquelle ändern kann (oder von der app geändert werden), und den Inhalt der Anzeige müssen diese Änderungen konsistent bleiben. In den folgenden Abschnitten erfahren Sie, wie Sie die Element-Click-Ereignisse behandeln, und aktualisieren Sie `RecyclerView` Wenn die Änderungen die zugrunde liegenden Datenquelle.


### <a name="handling-item-click-events"></a>Behandeln von Element-Click-Ereignissen

Wenn ein Benutzer ein Element im berührt die `RecyclerView`, ein Element-Click-Ereignis wird generiert, um die app zu benachrichtigen, welches Element verwendet wurde. Dieses Ereignis wird nicht generiert, indem `RecyclerView` &ndash; stattdessen die Elementansicht (die in der Ansicht Inhaber umschlossen wird) erkennt, Workflows und meldet diese Workflows wie der click-Ereignisse.

Um das Element-Click-Ereignisse behandeln zu veranschaulichen, erklären Sie die folgenden Schritte aus erläutert, wie die grundlegenden Anzeigen von Fotos-app zum Bericht ändern die Foto vom Benutzer verwendete wurde, hatte. Wenn ein Element-Click-Ereignis in der Beispiel-app auftritt, erfolgt die folgende Sequenz:

1.  Des Bild des `CardView` erkennt das Element-Click-Ereignis und benachrichtigt den Adapter.

2.  Der Adapter leitet das Ereignis (mit Informationen über die Position Elemente) an die Element-Click-Ereignishandler der Aktivität.

3.  Aktivitäts Element-Click-Ereignishandler reagiert auf das Element-Click-Ereignis.

Ein Ereignismember-Handler zuerst aufgerufen `ItemClick` wird hinzugefügt, um die `PhotoAlbumAdapter` Definition der Klasse:

```csharp
public event EventHandler<int> ItemClick;
```

Anschließend wird eine Element-Click-Ereignishandlermethode hinzugefügt, um `MainActivity`.
Diesem Handler zeigt kurz als Popupbenachrichtigung an, der angibt, welches Foto-Element verwendet wurde:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Als Nächstes wird eine einzige Zeile Code für die Registrierung benötigt die `OnItemClick` Ereignishandler mit `PhotoAlbumAdapter`. Ein guter Ausgangspunkt zu diesem Zweck wird unmittelbar nach `PhotoAlbumAdapter` wird erstellt: 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

In diesem einfachen Beispiel Registrierung findet statt, in der Hauptaktivität `OnCreate` -Methode, aber eine Produktions-app registrieren Sie möglicherweise den Handler in `OnResume` und Aufheben der Registrierung in `OnPause` &ndash; finden Sie unter [Aktivitätslebenszyklus ](~/android/app-fundamentals/activity-lifecycle/index.md) für Weitere Informationen.

`PhotoAlbumAdapter` Jetzt rufen `OnItemClick` ein Element-Click-Ereignis empfängt. Der nächste Schritt ist die Erstellung einen Ereignishandler im Adapter, der dies auslöst `ItemClick` Ereignis. Die folgende Methode, `OnClick`, wird direkt danach des Adapters hinzugefügt `ItemCount` Methode:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Dies `OnClick` Methode ist der Adapter die *Listener* für Element-Click-Ereignisse aus Item Sichten. Bevor dieser Listener registriert werden kann, mit einem Element anzeigen (per der Elementansicht Ansicht Inhaber), die `PhotoViewHolder` Konstruktor muss geändert werden, um diese Methode als ein zusätzliches Argument akzeptieren, und registrieren Sie `OnClick` mit der Elementansicht `Click` Ereignis.
Hier ist die geänderte `PhotoViewHolder` Konstruktor:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

Die `itemView` Parameter enthält einen Verweis auf die `CardView` , die vom Benutzer verwendet wurde. Beachten Sie, dass die Basisklasse für die Ansicht Inhaber die Layoutposition des Elements weiß (`CardView`), den sie darstellt (über die `LayoutPosition` Eigenschaft), und diese Position des Adapters übergeben `OnClick` Methode, wenn ein Element-Click-Ereignis stattfindet. Des Adapters die `OnCreateViewHolder` Methode wird geändert, um das Übergeben des Adapters `OnClick` Methode, um die Ansicht des Karteninhabers-Konstruktor:

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Nun, wenn Sie erstellen und der beispielanwendung für das Anzeigen von Fotos ausführen, bewirkt durch Tippen auf ein Foto in der Anzeige, dass ein Popup angezeigt werden, dass die meldet, dass die Foto verwendet wurde:

[![Toast Beispiel, das wird angezeigt, wenn ein Foto Karte getippt wird](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

Dieses Beispiel zeigt nur einen Ansatz zum Implementieren von Ereignishandlern mit `RecyclerView`. Eine weitere Möglichkeit, die hier nicht verwendet werden kann, werden Ereignisse auf den Inhaber der Ansicht platzieren, und den Adapter diese Ereignisse abonniert haben ab. Sofern die Foto-Beispiel-app ein Foto Bearbeitungsfunktionen, separate Ereignisse wäre erforderlich, damit die `ImageView` und die `TextView` innerhalb der einzelnen `CardView`: erwähnt die `TextView` starten würde eine `EditView` Dialogfeld, in dem den Benutzer bearbeiten kann die Beschriftung und die Workflows für die `ImageView` würde ein Foto Touchup-Tool, mit dem den Benutzer Zuschneiden oder drehen das Foto zu starten. Je nach den Anforderungen Ihrer App müssen Sie am besten für die Behandlung von und Reaktion Berührungsereignisse entwerfen.

Zur Veranschaulichung wie `RecyclerView` kann aktualisiert werden, wenn die DataSet-Änderungen, die beispielanwendung für das Anzeigen von Fotos geändert werden können, um nach dem Zufallsprinzip ein Foto in der Datenquelle auszuwählen, und tauschen sie mit dem ersten Foto. Zunächst wird eine **zufällige auswählen** Schaltfläche wird hinzugefügt, um der Beispiel Foto-app **Main.axml** Layout:

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

Als Nächstes wird Code hinzugefügt, am Ende der Hauptaktivität `OnCreate` Methode zum Suchen der `Random Pick` im Layout Schaltfläche, und fügen Sie einen Handler zuzuweisen:

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

Dieser Ereignishandler ruft des Fotoalbums `RandomSwap` Methode bei der **zufällige auswählen** Schaltfläche getippt wird. Die `RandomSwap` Methode nach dem Zufallsprinzip tauscht ein Foto mit dem ersten Foto in der Datenquelle und gibt dann den Index des Fotos nach dem Zufallsprinzip ausgetauscht. Wenn Sie kompilieren und die Beispiel-app mit dem folgenden Code ausführen, tippen Sie auf die **zufällige auswählen** Schaltfläche führt zu einer Änderung der Anzeige nicht zu, da die `RecyclerView` sind nicht die Änderung an der Datenquelle bekannt.

Zu `RecyclerView` aktualisiert, nachdem die Datenquelle ändert, die **zufällige auswählen** klicken Sie auf Ereignishandler muss geändert werden, zum Aufrufen des Adapters `NotifyItemChanged` Methode für jedes Element in der Auflistung, die geändert wurde (in diesem Fall zwei Elemente vorhanden sind geändert: das erste Foto und das getauscht Foto). Dies bewirkt, dass `RecyclerView` um die Anzeige zu aktualisieren, damit sie mit den neuen Zustand der Datenquelle konsistent sind:

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

Nun, wenn die **zufällige auswählen** ist auf die Schaltfläche getippt, `RecyclerView` aktualisiert die Anzeige, um anzugeben, dass ein Foto weiter unten in der Auflistung mit dem ersten Foto in der Auflistung vertauscht wurden:

[![Erster Screenshot vor dem Austausch, zweiten Screenshot nach dem Austausch](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Natürlich `NotifyDataSetChanged` wurde möglicherweise aufgerufen anstatt die beiden Aufrufe von `NotifyItemChanged`, aber dies also gezwungen, `RecyclerView` die gesamte Auflistung zu aktualisieren, auch wenn nur zwei Elemente in der Auflistung geändert haben. Aufrufen von `NotifyItemChanged` ist erheblich effizienter als das Aufrufen `NotifyDataSetChanged`.


## <a name="related-links"></a>Verwandte Links

- [RecyclerViewer (Beispiel)](https://developer.xamarin.com/samples/monodroid/android5.0/RecyclerViewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView-Komponenten und Funktionen](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Ein einfaches RecyclerView-Beispiel](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
