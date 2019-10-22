---
title: Xamarin. Forms CollectionView-Bildlauf
description: Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem definiert CollectionView zwei ScrollTo-Methoden, die Elemente Programm gesteuert in die Ansicht scrollen.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 7aef14cbb854d89a2088a450353b943402f76a86
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697217"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin. Forms CollectionView-Bildlauf

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, die Elemente in der Ansicht scrollen. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt. Beide über Ladungen verfügen über zusätzliche Argumente, die angegeben werden können, um die Gruppe anzugeben, zu der das Element gehört, die genaue Position des Elements, nachdem der Bildlauf abgeschlossen ist, und ob der Bild Lauf animiert werden soll.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert ein [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) Ereignis, das ausgelöst wird, wenn eine der [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden aufgerufen wird. Das [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) Objekt, das das `ScrollToRequested` Ereignis begleitet, verfügt über viele Eigenschaften, einschließlich `IsAnimated`, `Index`, `Item` und `ScrollToPosition`. Diese Eigenschaften werden von den Argumenten festgelegt, die in den `ScrollTo` Methoden aufrufen angegeben werden.

Außerdem definiert [`CollectionView`](xref:Xamarin.Forms.CollectionView) ein `Scrolled` Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das `ItemsViewScrolledEventArgs` Objekt, das das `Scrolled` Ereignis begleitet, verfügt über zahlreiche Eigenschaften. Weitere Informationen finden Sie unter [Erkennen von scrollläufen](#detect-scrolling).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert auch eine `ItemsUpdatingScrollMode` Eigenschaft, die das Scrollverhalten der `CollectionView` darstellt, wenn ihr neue Elemente hinzugefügt werden. Weitere Informationen zu dieser Eigenschaft finden Sie unter [Steuern der Scrollposition, wenn neue Elemente hinzugefügt werden](#control-scroll-position-when-new-items-are-added).

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Diese Funktion wird als Andocken bezeichnet, da Elemente an der Position ausgerichtet werden, wenn der Bildlauf beendet wird. Weitere Informationen finden Sie unter Andock [Punkte](#snap-points).

[`CollectionView`](xref:Xamarin.Forms.CollectionView) können Daten auch inkrementell laden, wenn der Benutzer einen Bildlauf ausführt. Weitere Informationen finden Sie unter [inkrementelles Laden von Daten](populate-data.md#load-data-incrementally).

## <a name="detect-scrolling"></a>Scrollen erkennen

[`CollectionView`](xref:Xamarin.Forms.CollectionView) definiert ein `Scrolled` Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das folgende XAML-Beispiel zeigt eine `CollectionView`, die einen Ereignishandler für das `Scrolled`-Ereignis festlegt:

```xaml
<CollectionView Scrolled="OnCollectionViewScrolled">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView();
collectionView.Scrolled += OnCollectionViewScrolled;
```

In diesem Codebeispiel wird der `OnCollectionViewScrolled` Ereignishandler ausgeführt, wenn das `Scrolled`-Ereignis ausgelöst wird:

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

In diesem Beispiel gibt der `OnCollectionViewScrolled` Ereignishandler die Werte des `ItemsViewScrolledEventArgs` Objekts aus, das das Ereignis begleitet.

> [!IMPORTANT]
> Das `Scrolled` Ereignis wird für vom Benutzer initiierte scrollläufe und für programmatische scrollläufe ausgelöst.

## <a name="scroll-an-item-at-an-index-into-view"></a>Scrollen eines Elements an einem Index in der Ansicht

Die erste [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) -Methoden Überladung führt einen Bildlauf für das Element am angegebenen Index durch. Im folgenden Beispiel wird ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) Objekt mit dem Namen `collectionView` dargestellt, das zeigt, wie das Element am Index 12 in der Ansicht angezeigt wird:

```csharp
collectionView.ScrollTo(12);
```

Alternativ kann ein Element in gruppierten Daten durch die Angabe der Element-und Gruppen Indizes in die Ansicht gescrollt werden. Im folgenden Beispiel wird gezeigt, wie das dritte Element in der zweiten Gruppe in die Ansicht scrollen kann:

```csharp
// Items and groups are indexed from zero.
collectionView.ScrollTo(2, 1);
```

> [!NOTE]
> Das [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) -Ereignis wird ausgelöst, wenn die [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) -Methode aufgerufen wird.

## <a name="scroll-an-item-into-view"></a>Bildlauf für ein Element in der Ansicht

Die zweite [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) -Methoden Überladung führt einen Bildlauf für das angegebene Element durch. Im folgenden Beispiel wird gezeigt, wie ein [`CollectionView`](xref:Xamarin.Forms.CollectionView) Objekt mit dem Namen "`collectionView`" angezeigt wird:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

Alternativ kann ein Element in gruppierten Daten durch das Angeben des Elements und der Gruppe in die Ansicht gescrollt werden. Im folgenden Beispiel wird gezeigt, wie das Element "Proboscis Monkey" in der Gruppe "Affen" in "View" Scrollen kann:

```csharp
GroupedAnimalsViewModel viewModel = BindingContext as GroupedAnimalsViewModel;
AnimalGroup group = viewModel.Animals.FirstOrDefault(a => a.Name == "Monkeys");
Animal monkey = group.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey, group);
```

> [!NOTE]
> Das [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) -Ereignis wird ausgelöst, wenn die [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) -Methode aufgerufen wird.

## <a name="disable-scroll-animation"></a>Scrollanimation deaktivieren

Eine Bild Lauf Animation wird angezeigt, wenn Sie ein Element in die Ansicht scrollen. Diese Animation kann jedoch deaktiviert werden, indem das `animate`-Argument der `ScrollTo`-Methode auf `false` festgelegt wird:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>Scrollposition des Steuer Elements

Beim Scrollen eines Elements in die Ansicht kann die genaue Position des Elements nach dem Abschluss des Bildlaufs mit dem `position`-Argument der [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden angegeben werden. Dieses Argument akzeptiert einen [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) Enumerationsmember.

### <a name="makevisible"></a>MakeVisible

Das [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) -Element gibt an, dass ein Rollup des Elements ausgeführt werden soll, bis es in der Ansicht sichtbar ist:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

Dieser Beispielcode führt zu einem minimalen Bildlauf, der erforderlich ist, um das Element in die Ansicht zu scrollen:

[![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht unter IOS und Android angezeigt wird](scrolling-images/scrolltoposition-makevisible.png "Vertikale Auflistung von CollectionView mit Rollup-Element")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "Vertikale Auflistung von CollectionView mit Rollup-Element")

> [!NOTE]
> Der [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) Member wird standardmäßig verwendet, wenn das `position`-Argument beim Aufrufen der `ScrollTo`-Methode nicht angegeben wird.

### <a name="start"></a>Starten

Der [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass für das Element ein Rollup zum Anfang der Ansicht ausgeführt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Dieser Beispielcode führt dazu, dass das Element zum Anfang der Ansicht gescrollt wird:

[![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht unter IOS und Android angezeigt wird](scrolling-images/scrolltoposition-start.png "Vertikale Auflistung von CollectionView mit Rollup-Element")](scrolling-images/scrolltoposition-start-large.png#lightbox "Vertikale Auflistung von CollectionView mit Rollup-Element")

### <a name="center"></a>Center

Der [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass das Element in den Mittelpunkt der Ansicht gescrollt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Dieser Beispielcode führt dazu, dass das Element in den Mittelpunkt der Ansicht gescrollt wird:

[![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht unter IOS und Android angezeigt wird](scrolling-images/scrolltoposition-center.png "Vertikale Auflistung von CollectionView mit Rollup-Element")](scrolling-images/scrolltoposition-center-large.png#lightbox "Vertikale Auflistung von CollectionView mit Rollup-Element")

### <a name="end"></a>Ende

Der [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass das Element an das Ende der Ansicht gescrollt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Dieser Beispielcode führt dazu, dass das Element zum Ende der Ansicht gescrollt wird:

[![Screenshot einer vertikalen CollectionView-Liste mit einem Element, das in der Ansicht unter IOS und Android angezeigt wird](scrolling-images/scrolltoposition-end.png "Vertikale Auflistung von CollectionView mit Rollup-Element")](scrolling-images/scrolltoposition-end-large.png#lightbox "Vertikale Auflistung von CollectionView mit Rollup-Element")

## <a name="control-scroll-position-when-new-items-are-added"></a>Scrollposition beim Hinzufügen neuer Elemente steuern

[`CollectionView`](xref:Xamarin.Forms.CollectionView) eine `ItemsUpdatingScrollMode` Eigenschaft definiert, die durch eine bindbare Eigenschaft unterstützt wird. Diese Eigenschaft ruft einen `ItemsUpdatingScrollMode` Enumerationswert ab, der das Bild Laufverhalten der `CollectionView` darstellt, wenn diesem neue Elemente hinzugefügt werden, oder legt diesen fest. Die `ItemsUpdatingScrollMode`-Enumeration definiert die folgenden Member:

- `KeepItemsInView` passt den scrolloffset an, damit das erste sichtbare Element angezeigt wird, wenn neue Elemente hinzugefügt werden.
- `KeepScrollOffset` behält den scrolloffset relativ zum Anfang der Liste bei, wenn neue Elemente hinzugefügt werden.
- `KeepLastItemInView` passt den scrolloffset an, damit das letzte Element sichtbar bleibt, wenn neue Elemente hinzugefügt werden.

Der Standardwert der `ItemsUpdatingScrollMode`-Eigenschaft ist `KeepItemsInView`. Wenn also neue Elemente zu einem hinzugefügt werden [`CollectionView`](xref:Xamarin.Forms.CollectionView) wird das erste sichtbare Element in der Liste angezeigt. Um sicherzustellen, dass neu hinzugefügte Elemente immer am Ende der Liste sichtbar sind, sollte die `ItemsUpdatingScrollMode`-Eigenschaft auf `KeepLastItemInView` festgelegt werden:

```xaml
<CollectionView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>Sichtbarkeit der Scrollleiste

[`CollectionView`](xref:Xamarin.Forms.CollectionView) `HorizontalScrollBarVisibility`-und `VerticalScrollBarVisibility` Eigenschaften definiert, die von bindbaren Eigenschaften unterstützt werden. Diese Eigenschaften erhalten einen [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) Enumerationswert, der angibt, wann die horizontale oder vertikale Schiebe Leiste sichtbar ist, oder legt diesen fest. Die `ScrollBarVisibility`-Enumeration definiert die folgenden Member:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility) gibt das Standardverhalten der Bild Lauf Leiste für die Plattform an, und ist der Standardwert für die Eigenschaften `HorizontalScrollBarVisibility` und `VerticalScrollBarVisibility`.
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility) gibt an, dass Scrollleisten sichtbar sind, auch wenn der Inhalt in die Ansicht passt.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility) gibt an, dass Scrollleisten nicht sichtbar sind, auch wenn der Inhalt nicht in die Ansicht passt.

## <a name="snap-points"></a>Andockpunkte

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Diese Funktion wird als Andocken bezeichnet, da Elemente an der Position ausgerichtet werden, wenn der Bildlauf beendet wird. Sie wird durch die folgenden Eigenschaften der [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) -Klasse gesteuert:

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)vom Typ [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)gibt das Verhalten der Andockpunkte beim Scrollen an.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)vom Typ [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)gibt an, wie Andockpunkte an Elementen ausgerichtet werden.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> Beim Ausrichten erfolgt dies in der Richtung, in der die geringste Menge an Bewegung erzeugt wird.

### <a name="snap-points-type"></a>Typ der Andockpunkte

Die [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) -Enumeration definiert die folgenden Member:

- `None` gibt an, dass der Bildlauf nicht an Elemente andostet.
- `Mandatory` gibt an, dass der Inhalt immer an den nächstgelegenen Punkt ausgerichtet wird, an dem der Bildlauf auf natürliche Weise an der Richtung der Trägheit angehalten wird.
- `MandatorySingle` gibt das gleiche Verhalten wie `Mandatory` an, führt jedoch nur ein Element gleichzeitig aus.

Standardmäßig wird die [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) -Eigenschaft auf `SnapPointsType.None` festgelegt, wodurch sichergestellt wird, dass Elemente nicht durch Scrollen wie in den folgenden Screenshots angezeigt werden:

[![Screenshot einer Auflistungs Ansicht (vertikal) ohne Andockpunkte unter IOS und Android](scrolling-images/snappoints-none.png "Auflistungs Ansicht (vertikal) ohne Andockpunkte")](scrolling-images/snappoints-none-large.png#lightbox "Auflistungs Ansicht (vertikal) ohne Andockpunkte")

### <a name="snap-points-alignment"></a>Ausrichtung der Andockpunkte

Die [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) -Enumeration definiert `Start`-, `Center`-und `End`-Member.

> [!IMPORTANT]
> Der Wert der [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) -Eigenschaft wird nur berücksichtigt, wenn die [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) -Eigenschaft auf `Mandatory` oder `MandatorySingle` festgelegt ist.

#### <a name="start"></a>Starten

Der `SnapPointsAlignment.Start` Member gibt an, dass die Ausrichtungs Punkte am führenden Rand von Elementen ausgerichtet sind.

Standardmäßig wird die [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) -Eigenschaft auf `SnapPointsAlignment.Start` festgelegt. Aus Gründen der Vollständigkeit wird jedoch im folgenden XAML-Beispiel gezeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

Wenn ein Benutzer einen Bildlauf initiiert, wird das oberste Element am oberen Rand der Ansicht ausgerichtet:

[![Screenshot einer vertikalen CollectionView-Liste mit Start-Snap Points unter IOS und Android](scrolling-images/snappoints-start.png "Vertikale CollectionView-Liste mit Start-Snap Points")](scrolling-images/snappoints-start-large.png#lightbox "Vertikale CollectionView-Liste mit Start-Snap Points")

#### <a name="center"></a>Center

Der `SnapPointsAlignment.Center` Member gibt an, dass die Punkt Punkte an der Mitte der Elemente ausgerichtet sind. Das folgende XAML-Beispiel zeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

Wenn ein Benutzer einen Bildlauf initiiert, wird das oberste Element am oberen Rand der Ansicht zentriert ausgerichtet:

[![Screenshot einer vertikalen CollectionView-Liste mit Center-Snap-Points unter IOS und Android](scrolling-images/snappoints-center.png "Vertikale CollectionView-Liste mit Center-Snap-Points")](scrolling-images/snappoints-center-large.png#lightbox "Vertikale CollectionView-Liste mit Center-Snap-Points")

#### <a name="end"></a>Ende

Das `SnapPointsAlignment.End`-Element gibt an, dass die Andockpunkte am nachfolgenden Rand von Elementen ausgerichtet sind. Das folgende XAML-Beispiel zeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Der entsprechende C#-Code lautet:

```csharp
CollectionView collectionView = new CollectionView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

Wenn ein Benutzer einen Bildlauf initiiert, wird das untere Element am unteren Rand der Ansicht ausgerichtet:

[![Screenshot einer vertikalen CollectionView-Liste mit endausrichtungs Punkten unter IOS und Android](scrolling-images/snappoints-end.png "Vertikale Auflistung von CollectionView mit Endpunkten")](scrolling-images/snappoints-end-large.png#lightbox "Vertikale Auflistung von CollectionView mit Endpunkten")

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
