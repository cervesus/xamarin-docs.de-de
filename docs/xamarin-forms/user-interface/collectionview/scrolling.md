---
title: Xamarin. Forms CollectionView-Bildlauf
description: Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem definiert CollectionView zwei ScrollTo-Methoden, die Elemente Programm gesteuert in die Ansicht scrollen.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/13/2019
ms.openlocfilehash: 303266f44664f7f57aeaf36869a3a06c8eb91870
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69888647"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin. Forms CollectionView-Bildlauf

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente in der Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt. Beide über Ladungen verfügen über zusätzliche Argumente, die angegeben werden können, um die genaue Position des Elements nach dem Abschluss des Bildlaufs anzugeben, und ob der Bild Lauf animiert werden soll.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert ein [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) -Ereignis, das ausgelöst wird, wenn [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) eine der-Methoden aufgerufen wird. Das [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) Objekt, das das `ScrollToRequested` Ereignis begleitet, verfügt über viele `IsAnimated`Eigenschaften, `Item`einschließlich, `ScrollToPosition` `Index`, und. Diese Eigenschaften werden von den Argumenten festgelegt, die `ScrollTo` in den Methoden aufrufen angegeben werden.

Außerdem definiert ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) `Scrolled` -Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das `ItemsViewScrolledEventArgs` Objekt, das das `Scrolled` Ereignis begleitet, verfügt über zahlreiche Eigenschaften. Weitere Informationen finden Sie unter [erkennen](#detect-scrolling)von scrollläufen.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert auch eine `ItemsUpdatingScrollMode` -Eigenschaft, die das Scrollverhalten von `CollectionView` darstellt, wenn der neue Elemente hinzugefügt werden. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Steuern der Scrollposition, wenn neue Elemente hinzugefügt werden](#control-scroll-position-when-new-items-are-added).

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Diese Funktion wird als Andocken bezeichnet, da Elemente an der Position ausgerichtet werden, wenn der Bildlauf beendet wird. Weitere Informationen finden Sie unter Andock [Punkte](#snap-points).

[`CollectionView`](xref:Xamarin.Forms.CollectionView)kann Daten auch inkrementell laden, wenn der Benutzer einen Bildlauf ausführt Weitere Informationen finden Sie unter [inkrementelles Laden von Daten](populate-data.md#load-data-incrementally).

## <a name="detect-scrolling"></a>Scrollen erkennen

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert ein `Scrolled` Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das folgende XAML-Beispiel zeigt `CollectionView` , wie ein Ereignishandler für das `Scrolled` -Ereignis festgelegt wird:

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

In diesem Codebeispiel wird der `OnCollectionViewScrolled` Ereignishandler ausgeführt, wenn das `Scrolled` Ereignis ausgelöst wird:

```csharp
void OnCollectionViewScrolled(object sender, ItemsViewScrolledEventArgs e)
{
    Debug.WriteLine("HorizontalDelta: " + e.HorizontalDelta);
    Debug.WriteLine("VerticalDelta: " + e.VerticalDelta);
    Debug.WriteLine("HorizontalOffset: " + e.HorizontalOffset);
    Debug.WriteLine("VerticalOffset: " + e.VerticalOffset);
    Debug.WriteLine("FirstVisibleItemIndex: " + e.FirstVisibleItemIndex);
    Debug.WriteLine("CenterItemIndex: " + e.CenterItemIndex);
    Debug.WriteLine("LastVisibleItemIndex: " + e.LastVisibleItemIndex);
}
```

Der `OnCollectionViewScrolled` Ereignishandler gibt die Werte `ItemsViewScrolledEventArgs` des Objekts aus, das das Ereignis begleitet.

> [!IMPORTANT]
> Das `Scrolled` Ereignis wird für benutzergesteuerte scrollläufe und für programmatische scrollläufe ausgelöst.

## <a name="scroll-an-item-at-an-index-into-view"></a>Scrollen eines Elements an einem Index in der Ansicht

Die erste [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden Überladung führt einen Bildlauf für das Element am angegebenen Index durch. Bei einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) -Objekt `collectionView`mit dem Namen wird im folgenden Beispiel veranschaulicht, wie das Element am Index 12 in der Ansicht angezeigt wird:

```csharp
collectionView.ScrollTo(12);
```

> [!NOTE]
> Das [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) -Ereignis wird ausgelöst, [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) wenn die-Methode aufgerufen wird.

## <a name="scroll-an-item-into-view"></a>Bildlauf für ein Element in der Ansicht

Die zweite [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden Überladung scrollt das angegebene Element in die Ansicht. Im folgenden [`CollectionView`](xref:Xamarin.Forms.CollectionView) Beispiel wird `collectionView`ein-Objekt mit dem Namen veranschaulicht, wie das angegebene Element in der Ansicht angezeigt wird:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

> [!NOTE]
> Das [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) -Ereignis wird ausgelöst, [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) wenn die-Methode aufgerufen wird.

## <a name="scroll-bar-visibility"></a>Sichtbarkeit der Scrollleiste

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert `HorizontalScrollBarVisibility` die `VerticalScrollBarVisibility` Eigenschaften und, die durch bindbare Eigenschaften unterstützt werden. Diese Eigenschaften erhalten einen [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) -Enumerationswert, der angibt, wann die horizontale oder vertikale Bild Lauf Leiste sichtbar ist, oder legt diesen fest. Die `ScrollBarVisibility`-Enumeration definiert die folgenden Member:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt das Standardverhalten der Bild Lauf Leiste für die Plattform an, und ist der Standard `HorizontalScrollBarVisibility` Wert `VerticalScrollBarVisibility` für die-Eigenschaft und die-Eigenschaft.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt an, dass Scrollleisten sichtbar sind, auch wenn der Inhalt in die Ansicht passt.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt an, dass Scrollleisten nicht sichtbar sind, auch wenn der Inhalt nicht in die Ansicht passt.

## <a name="control-scroll-position"></a>Scrollposition des Steuer Elements

Beim Scrollen eines Elements in die Ansicht kann die genaue Position des Elements nach Abschluss des Bildlaufs mit dem `position` -Argument [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) der-Methode angegeben werden. Dieses Argument akzeptiert einen [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) -Enumerationsmember.

### <a name="makevisible"></a>MakeVisible

Der [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass ein Rollup des Elements ausgeführt werden soll, bis es in der Ansicht sichtbar ist:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

Dieser Beispielcode führt zu einem minimalen Bildlauf, der erforderlich ist, um das Element in die Ansicht zu scrollen:

[ ![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht angezeigt wird, in der vertikalen Liste von IOS und Android](scrolling-images/scrolltoposition-makevisible.png "CollectionView mit") einem] Auflistungs Element (scrolling-images/scrolltoposition-makevisible-large.png#lightbox "Vertikale Auflistung von CollectionView mit") Rollup-Element

> [!NOTE]
> Der [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) -Member wird standardmäßig verwendet, wenn `position` das-Argument beim Aufrufen der `ScrollTo` -Methode nicht angegeben wird.

### <a name="start"></a>Beginn

Der [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass für das Element ein Rollup zum Anfang der Ansicht ausgeführt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Dieser Beispielcode führt dazu, dass das Element zum Anfang der Ansicht gescrollt wird:

[ ![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht angezeigt wird, in der vertikalen Liste von IOS und Android](scrolling-images/scrolltoposition-start.png "CollectionView mit") einem] Auflistungs Element (scrolling-images/scrolltoposition-start-large.png#lightbox "Vertikale Auflistung von CollectionView mit") Rollup-Element

### <a name="center"></a>Center

Der [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass das Element in den Mittelpunkt der Ansicht gescrollt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Dieser Beispielcode führt dazu, dass das Element in den Mittelpunkt der Ansicht gescrollt wird:

[ ![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht angezeigt wird, in der vertikalen Liste von IOS und Android](scrolling-images/scrolltoposition-center.png "CollectionView mit") einem] Auflistungs Element (scrolling-images/scrolltoposition-center-large.png#lightbox "Vertikale Auflistung von CollectionView mit") Rollup-Element

### <a name="end"></a>Ende

Der [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass das Element an das Ende der Ansicht gescrollt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Dieser Beispielcode führt dazu, dass das Element zum Ende der Ansicht gescrollt wird:

[ ![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht angezeigt wird, in der vertikalen Liste von IOS und Android](scrolling-images/scrolltoposition-end.png "CollectionView mit") einem] Auflistungs Element (scrolling-images/scrolltoposition-end-large.png#lightbox "Vertikale Auflistung von CollectionView mit") Rollup-Element

## <a name="disable-scroll-animation"></a>Scrollanimation deaktivieren

Eine Bild Lauf Animation wird angezeigt, wenn Sie ein Element in die Ansicht scrollen. Diese Animation kann jedoch deaktiviert werden, indem das `animate` -Argument `ScrollTo` der-Methode auf `false`festgelegt wird:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position-when-new-items-are-added"></a>Scrollposition beim Hinzufügen neuer Elemente steuern

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert eine `ItemsUpdatingScrollMode` Eigenschaft, die durch eine bindbare Eigenschaft unterstützt wird. Diese Eigenschaft ruft einen `ItemsUpdatingScrollMode` -Enumerationswert ab, der das Scrollverhalten `CollectionView` von darstellt, wenn ihm neue Elemente hinzugefügt werden, oder legt diesen fest. Die `ItemsUpdatingScrollMode`-Enumeration definiert die folgenden Member:

- `KeepItemsInView`passt den scrolloffset an, damit das erste sichtbare Element angezeigt wird, wenn neue Elemente hinzugefügt werden.
- `KeepScrollOffset`behält den scrolloffset relativ zum Anfang der Liste bei, wenn neue Elemente hinzugefügt werden.
- `KeepLastItemInView`passt den scrolloffset an, um das letzte Element sichtbar zu machen, wenn neue Elemente hinzugefügt werden.

Der Standardwert `ItemsUpdatingScrollMode` der-Eigenschaft ist `KeepItemsInView`. Wenn also neue Elemente zu einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) hinzugefügt werden, wird das erste sichtbare Element in der Liste angezeigt. Um sicherzustellen, dass neu hinzugefügte Elemente immer am Ende der Liste sichtbar sind `ItemsUpdatingScrollMode` , sollte die-Eigenschaft `KeepLastItemInView`auf festgelegt werden:

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="snap-points"></a>Andockpunkte

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Diese Funktion wird als Andocken bezeichnet, da Elemente an der Position andocken, wenn der Bildlauf beendet wird. Sie [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) wird durch die folgenden Eigenschaften der-Klasse gesteuert:

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Gibt beim Scrollen [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)das Verhalten der Andockpunkte an, vom Typ.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)Gibt an [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment), wie Andockpunkte mit Elementen ausgerichtet werden.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> Beim Ausrichten erfolgt dies in der Richtung, in der die geringste Menge an Bewegung erzeugt wird.

### <a name="snap-points-type"></a>Typ der Andockpunkte

Die [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) -Enumeration definiert die folgenden Member:

- `None`Gibt an, dass der Bildlauf nicht an Elemente angedostet.
- `Mandatory`Gibt an, dass der Inhalt immer an den nächstgelegenen Punkt ausgerichtet wird, an dem der Bildlauf durchgeführt werden würde, entlang der Richtung der Trägheit.
- `MandatorySingle`Gibt das gleiche Verhalten wie `Mandatory`an, führt jedoch nur ein Element gleichzeitig aus.

Standardmäßig ist die [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) -Eigenschaft auf `SnapPointsType.None`festgelegt, wodurch sichergestellt wird, dass der Bildlauf Elemente nicht ausstellt, wie in den folgenden Screenshots gezeigt:

[ ![Screenshot einer Auflistungs Ansicht (vertikal) ohne Andockpunkte in IOS und Android](scrolling-images/snappoints-none.png "CollectionView vertikale Liste ohne") ] Andockpunkte (scrolling-images/snappoints-none-large.png#lightbox "Auflistungs Ansicht (vertikal) ohne") Andockpunkte

### <a name="snap-points-alignment"></a>Ausrichtung der Andockpunkte

Die [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) -Enumeration `Start`definiert `Center`die Member `End` , und.

> [!IMPORTANT]
> Der Wert [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) der-Eigenschaft wird nur berücksichtigt, wenn [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) die-Eigenschaft auf `Mandatory`oder `MandatorySingle`festgelegt ist.

#### <a name="start"></a>Beginn

Der `SnapPointsAlignment.Start` -Member gibt an, dass die Ausrichtungs Punkte am führenden Rand der Elemente ausgerichtet werden.

Standardmäßig ist die [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) -Eigenschaft auf `SnapPointsAlignment.Start`festgelegt. Aus Gründen der Vollständigkeit wird jedoch im folgenden XAML-Beispiel gezeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Start">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Wenn ein Benutzer einen Bildlauf initiiert, wird das oberste Element am oberen Rand der Ansicht ausgerichtet:

[ ![Screenshot einer vertikalen CollectionView-Liste mit Start-Snap Points, on IOS und Android](scrolling-images/snappoints-start.png "CollectionView Vertical list with Start Snap Points") ] (scrolling-images/snappoints-start-large.png#lightbox "Vertikale CollectionView-Liste mit Start-Snap Points")

#### <a name="center"></a>Center

Der `SnapPointsAlignment.Center` Member gibt an, dass die Punkte am Mittelpunkt der Elemente ausgerichtet werden. Das folgende XAML-Beispiel zeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="Center">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Wenn ein Benutzer einen Bildlauf initiiert, wird das oberste Element am oberen Rand der Ansicht zentriert ausgerichtet:

[ ![Screenshot einer vertikalen CollectionView-Liste mit Center-Snap-Points in IOS und Android](scrolling-images/snappoints-center.png "CollectionView vertikale Liste mit Center-Snap-Points") ] (scrolling-images/snappoints-center-large.png#lightbox "Vertikale CollectionView-Liste mit Center-Snap-Points")

#### <a name="end"></a>Ende

Der `SnapPointsAlignment.End` Member gibt an, dass die Ausrichtungs Punkte am nachfolgenden Rand der Elemente ausgerichtet werden. Das folgende XAML-Beispiel zeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CollectionView x:Name="collectionView"
                ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <ListItemsLayout SnapPointsType="MandatorySingle"
                         SnapPointsAlignment="End">
            <x:Arguments>
                <ItemsLayoutOrientation>Vertical</ItemsLayoutOrientation>
            </x:Arguments>
        </ListItemsLayout>
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            ...
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Der entsprechende C#-Code ist:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new ListItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    ItemTemplate = new DataTemplate(() =>
    {
        return null;
    })
};
```

Wenn ein Benutzer einen Bildlauf initiiert, wird das untere Element am unteren Rand der Ansicht ausgerichtet:

[ ![Screenshot einer vertikalen CollectionView-Liste mit endausrichtungs Punkten in IOS und Android](scrolling-images/snappoints-end.png "CollectionView vertikal Liste mit Endpunkten") ] (scrolling-images/snappoints-end-large.png#lightbox "Vertikale Auflistung von CollectionView mit Endpunkten")

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
