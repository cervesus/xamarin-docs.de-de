---
title: Xamarin.Forms-CollectionView Durchführen eines Bildlaufs
description: Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, kann die Endposition des Bildlaufs gesteuert werden, damit Elemente vollständig angezeigt werden. CollectionView definiert außerdem zwei ScrollTo-Methoden, die Elemente programmgesteuert in der Ansicht zu scrollen.
ms.prod: xamarin
ms.assetid: 2ED719AF-33D2-434D-949A-B70B479C9BA5
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 888b4f369f67a7d8566640927ba2ae3a395d68d8
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/06/2019
ms.locfileid: "65048165"
---
# <a name="xamarinforms-collectionview-scrolling"></a>Xamarin.Forms-CollectionView Durchführen eines Bildlaufs

![](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)

`CollectionView` definiert zwei `ScrollTo` Methoden, die Elemente in der Ansicht zu scrollen. Eine der Überladungen führt einen Bildlauf durch das Element am angegebenen Index in der Ansicht, während die andere das angegebene Element in der Ansicht einen Bildlauf durchführt. Beide Überladungen haben zusätzliche Argumente, die angegeben werden können, um die genaue Position des Elements anzugeben, nach dem Abschluss des Bildlaufs und angibt, ob das Scrollen zu animieren.

`CollectionView` definiert eine `ScrollToRequested` Ereignis, wenn ausgelöst wird, eines der `ScrollTo` Methoden aufgerufen wird. Die `ScrollToRequestedEventArgs` Objekt an, die mit der `ScrollToRequested` Ereignis verfügt über zahlreiche Eigenschaften wie `IsAnimated`, `Index`, `Item`, und `ScrollToPosition`. Diese Eigenschaften werden festgelegt, aus den Argumenten angegeben werden, der `ScrollTo` Methodenaufrufe.

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, kann die Endposition des Bildlaufs gesteuert werden, damit Elemente vollständig angezeigt werden. Dieses Feature wird als ausrichten, bezeichnet, da Elemente ausrichten, um beim Scrollen von beendet zu positionieren. Weitere Informationen finden Sie unter [Snap-Points](#snap-points).

## <a name="scroll-an-item-at-an-index-into-view"></a>Führen Sie ein Element an einen Index in der Ansicht einen Bildlauf

Die erste `ScrollTo` -methodenüberladung, verschiebt das Element am angegebenen Index in der Ansicht. Erhält eine `CollectionView` Objekt mit dem Namen `collectionView`, das folgende Beispiel zeigt, wie Sie das Element am Index 12 in der Ansicht einen Bildlauf durchführen:

```csharp
collectionView.ScrollTo(12);
```

## <a name="scroll-an-item-into-view"></a>Führen Sie ein Element in der Ansicht einen Bildlauf

Die zweite `ScrollTo` methodenüberladung führt einen Bildlauf durch das angegebene Element in der Ansicht. Erhält eine `CollectionView` Objekt mit dem Namen `collectionView`, das folgende Beispiel zeigt, wie das angegebene Element in der Ansicht einen Bildlauf durchführen:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
collectionView.ScrollTo(monkey);
```

## <a name="control-scroll-position"></a>Bildlaufposition des Steuerelements

Wenn ein Element in der Ansicht zu scrollen, kann die genaue Position des Elements nach Abschluss der Bildlauf angegeben werden, mit der `position` Argument des der `ScrollTo` Methoden. Dieses Argument akzeptiert eine [ `ScrollToPosition` ](xref:Xamarin.Forms.ScrollToPosition) -Enumerationsmember.

### <a name="makevisible"></a>MakeVisible

Die [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) Element gibt an, dass das Element ein Bildlauf durchgeführt werden soll, bis es in der Ansicht angezeigt wird:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

Dieser Beispielcode führt der minimale Bildlauf erforderlich, um das Element in der Ansicht einen Bildlauf durchführen:

[![Screenshot von einer vertikalen Liste der CollectionView mit einem Element ein Bildlauf durchgeführt, in der Sicht, für iOS und Android](scrolling-images/scrolltoposition-makevisible.png "CollectionView vertikale Liste mit Bildlauf Element")](scrolling-images/scrolltoposition-makevisible-large.png#lightbox "CollectionView vertikale Liste mit Bildlauf Element")

> [!NOTE]
> Die [ `ScrollToPosition.MakeVisible` ](xref:Xamarin.Forms.ScrollToPosition) Member wird standardmäßig verwendet, wenn die `position` Argument nicht angegeben beim Aufrufen der `ScrollTo` Methode.

### <a name="start"></a>Starten

Die [ `ScrollToPosition.Start` ](xref:Xamarin.Forms.ScrollToPosition) Element gibt an, dass das Element am Anfang der Ansicht gescrollt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Dieser Beispielcode führt das Element am Anfang der Ansicht gescrollt werden:

[![Screenshot von einer vertikalen Liste der CollectionView mit einem Element ein Bildlauf durchgeführt, in der Sicht, für iOS und Android](scrolling-images/scrolltoposition-start.png "CollectionView vertikale Liste mit Bildlauf Element")](scrolling-images/scrolltoposition-start-large.png#lightbox "CollectionView vertikale Liste mit Bildlauf Element")

### <a name="center"></a>Center

Die [ `ScrollToPosition.Center` ](xref:Xamarin.Forms.ScrollToPosition) Element gibt an, dass das Element in die Mitte der Ansicht gescrollt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Dieser Beispielcode führt das Element in die Mitte der Ansicht gescrollt werden:

[![Screenshot von einer vertikalen Liste der CollectionView mit einem Element ein Bildlauf durchgeführt, in der Sicht, für iOS und Android](scrolling-images/scrolltoposition-center.png "CollectionView vertikale Liste mit Bildlauf Element")](scrolling-images/scrolltoposition-center-large.png#lightbox "CollectionView vertikale Liste mit Bildlauf Element")

### <a name="end"></a>Ende

Die [ `ScrollToPosition.End` ](xref:Xamarin.Forms.ScrollToPosition) Element gibt an, dass das Element am Ende der Ansicht gescrollt werden soll:

```csharp
collectionView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Dieser Beispielcode führt das Element am Ende der Ansicht gescrollt werden:

[![Screenshot von einer vertikalen Liste der CollectionView mit einem Element ein Bildlauf durchgeführt, in der Sicht, für iOS und Android](scrolling-images/scrolltoposition-end.png "CollectionView vertikale Liste mit Bildlauf Element")](scrolling-images/scrolltoposition-end-large.png#lightbox "CollectionView vertikale Liste mit Bildlauf Element")

## <a name="disable-scroll-animation"></a>Führen Sie einen Bildlauf Animation deaktivieren

Eine Bildlauf Animation wird angezeigt, wenn ein Element in der Ansicht zu scrollen. Allerdings kann dieser Animation deaktiviert werden, indem der `animate` Argument der `ScrollTo` Methode, um `false`:

```csharp
collectionView.ScrollTo(monkey, animate: false);
```

## <a name="snap-points"></a>Andockpunkte

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, kann die Endposition des Bildlaufs gesteuert werden, damit Elemente vollständig angezeigt werden. Dieses Feature wird als ausrichten, bezeichnet, da Elemente ausrichten, um beim Durchführen eines Bildlaufs beendet und, indem Sie die folgenden Eigenschaften gesteuert wird zu positionieren der `ItemsLayout` Klasse:

- `SnapPointsType`, des Typs `SnapPointsType`, gibt das Verhalten der Ausrichtungspunkte an, beim Durchführen eines Bildlaufs.
- `SnapPointsAlignment`, des Typs `SnapPointsAlignment`, gibt an, wie Elemente Ausrichtungspunkte ausgerichtet sind.

Diese Eigenschaften verfügen über [ `BindableProperty` ](xref:Xamarin.Forms.BindableProperty) Objekte, was bedeutet, dass die Eigenschaften, Ziele von datenbindungen werden können.

> [!NOTE]
> Tritt das Andocken, findet sie in der Richtung statt, die die geringste Menge an Motion erzeugt.

### <a name="snap-points-type"></a>Geben Sie andockpunkte

Die `SnapPointsType` Enumeration definiert die folgenden Elemente:

- `None` Gibt an, dass es sich bei Bildlauf an Elemente nicht ausgerichtet ist.
- `Mandatory` Gibt an, dass der Inhalt wird an den nächstgelegenen Snap ausgerichtet stets zu zeigen, zu dem Durchführen eines Bildlaufs auf natürliche Weise, die Richtung der Trägheit ausgeführt wird.
- `MandatorySingle` Gibt an, das gleiche Verhalten wie `Mandatory`, aber nur ein Element zu einem Zeitpunkt verschiebt.

In der Standardeinstellung die `SnapPointsType` -Eigenschaftensatz auf `SnapPointsType.None`, die sicherstellt, Durchführen eines Bildlaufs wird Elemente nicht ausgerichtet, wie in den folgenden Screenshots gezeigt:

[![Screenshot von einer vertikalen Liste der CollectionView ohne Ausrichtungspunkte, unter iOS und Android](scrolling-images/snappoints-none.png "CollectionView vertikalen Liste ohne Ausrichtungspunkte")](scrolling-images/snappoints-none-large.png#lightbox "CollectionView vertikalen Liste ohne Snap Punkte")

### <a name="snap-points-alignment"></a>Andockpunkte-Ausrichtung

Die `SnapPointsAlignment` -Enumeration definiert `Start`, `Center`, und `End` Member.

> [!IMPORTANT]
> Der Wert des der `SnapPointsAlignment` Eigenschaft ist nur berücksichtigt, wenn die `SnapPointsType` -Eigenschaftensatz auf `Mandatory`, oder `MandatorySingle`.

#### <a name="start"></a>Starten

Die `SnapPointsAlignment.Start` Element gibt an, dass die Ausrichtungspunkte an den führenden Rand des Elemente ausgerichtet sind.

In der Standardeinstellung ist die `SnapPointsAlignment`-Eigenschaft auf `SnapPointsAlignment.Start` festgelegt. Aus Gründen der Vollständigkeit zeigt das folgende XAML-Beispiel allerdings wie die Member dieser Enumeration festgelegt:

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

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, wird das oberste Element am oberen Rand der Ansicht ausgerichtet werden:

[![Screenshot von einer vertikalen Liste der CollectionView mit Start-Ausrichtungspunkte für iOS und Android](scrolling-images/snappoints-start.png "CollectionView vertikale Liste mit Start Ausrichtungspunkte")](scrolling-images/snappoints-start-large.png#lightbox "CollectionView vertikale Liste mit Start Ausrichten von Punkten")

#### <a name="center"></a>Center

Die `SnapPointsAlignment.Center` Element gibt an, dass die Ausrichtungspunkte an der Mitte der Elemente ausgerichtet sind. Im folgende XAML-Beispiel zeigt, wie die Member dieser Enumeration festgelegt wird:

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

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, wird das oberste Element Center, die am oberen Rand der Ansicht sein:

[![Screenshot von einer vertikalen Liste der CollectionView mit Snap von Mittelpunkten unter iOS und Android](scrolling-images/snappoints-center.png "CollectionView vertikale Liste mit Snap Mittelpunkten")](scrolling-images/snappoints-center-large.png#lightbox "CollectionView vertikale Liste mit Snap Mittelpunkten")

#### <a name="end"></a>Ende

Die `SnapPointsAlignment.End` Element gibt an, mit nachgestellten Rands des Elemente Ausrichtungspunkte ausgerichtet sind. Im folgende XAML-Beispiel zeigt, wie die Member dieser Enumeration festgelegt wird:

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

Wenn ein Benutzer Kundenkarte ein Scrollen zu initiieren, wird das Element nach unten am unteren Rand der Ansicht ausgerichtet werden:

[![Screenshot von einer vertikalen Liste der CollectionView mit End-Ausrichtungspunkte für iOS und Android](scrolling-images/snappoints-end.png "CollectionView vertikale Liste mit End-Snap-Points")](scrolling-images/snappoints-end-large.png#lightbox "CollectionView vertikale Liste mit End-Snap Punkte")

## <a name="related-links"></a>Verwandte Links

- [CollectionView (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/CollectionViewDemos/)
