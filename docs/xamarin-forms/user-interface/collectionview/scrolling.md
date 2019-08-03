---
title: Xamarin. Forms CollectionView-Bildlauf
description: Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem definiert CollectionView zwei ScrollTo-Methoden, die Elemente Programm gesteuert in die Ansicht scrollen.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 89bbe402f056b875a7dadd96527364847ad470e8
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738924"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin. Forms CollectionView-Bildlauf

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente in der Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt. Beide über Ladungen verfügen über zusätzliche Argumente, die angegeben werden können, um die genaue Position des Elements nach dem Abschluss des Bildlaufs anzugeben, und ob der Bild Lauf animiert werden soll.

[`CollectionView`](xref:Xamarin.Forms.CollectionView)definiert ein [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) -Ereignis, das ausgelöst wird, wenn [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) eine der-Methoden aufgerufen wird. Das [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) Objekt, das das `ScrollToRequested` Ereignis begleitet, verfügt über viele `IsAnimated`Eigenschaften, `Item`einschließlich, `ScrollToPosition` `Index`, und. Diese Eigenschaften werden von den Argumenten festgelegt, die `ScrollTo` in den Methoden aufrufen angegeben werden.

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Diese Funktion wird als Andocken bezeichnet, da Elemente an der Position ausgerichtet werden, wenn der Bildlauf beendet wird. Weitere Informationen finden Sie unter Andock [Punkte](#snap-points).

## <a name="scroll-an-item-at-an-index-into-view"></a>Scrollen eines Elements an einem Index in der Ansicht

Die erste [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden Überladung führt einen Bildlauf für das Element am angegebenen Index durch. Bei einem [`CollectionView`](xref:Xamarin.Forms.CollectionView) -Objekt `collectionView`mit dem Namen wird im folgenden Beispiel veranschaulicht, wie das Element am Index 12 in der Ansicht angezeigt wird:

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>Bildlauf für ein Element in der Ansicht

Die zweite [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden Überladung scrollt das angegebene Element in die Ansicht. Im folgenden [`CollectionView`](xref:Xamarin.Forms.CollectionView) Beispiel wird `collectionView`ein-Objekt mit dem Namen veranschaulicht, wie das angegebene Element in der Ansicht angezeigt wird:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

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

### <a name="start"></a>Starten

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

#### <a name="start"></a>Starten

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
