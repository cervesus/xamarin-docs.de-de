---
Title: " Xamarin.Forms carouselview Scroll" Description: "Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Scrollvorgangs gesteuert werden, sodass Elemente vollständig angezeigt werden. Außerdem definiert carouselview zwei ScrollTo-Methoden, die Elemente Programm gesteuert in die Ansicht scrollen. "
ms. Prod: xamarin ms. assetid: 92 d7b618-07fa-4343-9d0f -212525e92c39 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 01/28/2020 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="xamarinforms-carouselview-scrolling"></a>Xamarin.FormsCarouselview-Bildlauf

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert die folgenden scrollbezogenen Eigenschaften:

- `HorizontalScrollBarVisibility`vom Typ `ScrollBarVisibility` , der angibt, wann die horizontale Schiebe Leiste sichtbar ist.
- `IsDragging`vom Typ `bool` , der angibt, ob der Bildlauf durch `CarouselView` führt. Dies ist eine schreibgeschützte Eigenschaft, deren Standardwert ist `false` .
- `IsScrollAnimated`vom Typ `bool` , der angibt, ob beim Scrollen von eine Animation ausgeführt wird `CarouselView` . Standardwert: `true`.
- `ItemsUpdatingScrollMode`vom Typ `ItemsUpdatingScrollMode` , der das Scrollverhalten von darstellt, `CarouselView` Wenn diesem neue Elemente hinzugefügt werden.
- `VerticalScrollBarVisibility`vom Typ `ScrollBarVisibility` , der angibt, wann die vertikale Schiebe Leiste sichtbar ist.

Alle diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen sein können.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert auch zwei [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden, mit denen Elemente in der Ansicht angezeigt werden. Eine der-über Ladungen führt einen Bildlauf für das Element am angegebenen Index durch, während der andere das angegebene Element in die Ansicht verschiebt. Beide über Ladungen verfügen über zusätzliche Argumente, die angegeben werden können, um die genaue Position des Elements nach dem Abschluss des Bildlaufs anzugeben, und ob der Bild Lauf animiert werden soll.

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert ein- [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) Ereignis, das ausgelöst wird, wenn eine der- [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden aufgerufen wird. Das [`ScrollToRequestedEventArgs`](xref:Xamarin.Forms.ScrollToRequestedEventArgs) Objekt, das das `ScrollToRequested` Ereignis begleitet, verfügt über viele Eigenschaften, einschließlich `IsAnimated` , `Index` , `Item` und `ScrollToPosition` . Diese Eigenschaften werden von den Argumenten festgelegt, die in den `ScrollTo` Methoden aufrufen angegeben werden.

Außerdem [`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert ein- `Scrolled` Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Das `ItemsViewScrolledEventArgs` Objekt, das das `Scrolled` Ereignis begleitet, verfügt über zahlreiche Eigenschaften. Weitere Informationen finden Sie unter [Erkennen von scrollläufen](#detect-scrolling).

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Diese Funktion wird als Andocken bezeichnet, da Elemente an der Position ausgerichtet werden, wenn der Bildlauf beendet wird. Weitere Informationen finden Sie unter Andock [Punkte](#snap-points).

[`CarouselView`](xref:Xamarin.Forms.CarouselView)kann Daten auch inkrementell laden, wenn der Benutzer einen Bildlauf ausführt Weitere Informationen finden Sie unter [inkrementelles Laden von Daten](populate-data.md#load-data-incrementally).

## <a name="detect-scrolling"></a>Scrollen erkennen

Die- `IsDragging` Eigenschaft kann untersucht werden, um zu bestimmen, ob der [`CarouselView`](xref:Xamarin.Forms.CarouselView) gerade durch Elemente durchsucht.

Außerdem [`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert ein- `Scrolled` Ereignis, das ausgelöst wird, um anzugeben, dass der Bildlauf ausgeführt wurde. Dieses Ereignis sollte verwendet werden, wenn Daten über den scrollvorgang erforderlich sind.

Das folgende XAML-Beispiel zeigt `CarouselView` , wie ein Ereignishandler für das-Ereignis festgelegt wird `Scrolled` :

```xaml
<CarouselView Scrolled="OnCollectionViewScrolled">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView();
carouselView.Scrolled += OnCarouselViewScrolled;
```

In diesem Codebeispiel wird der `OnCarouselViewScrolled` Ereignishandler ausgeführt, wenn das `Scrolled` Ereignis ausgelöst wird:

```csharp
void OnCarouselViewScrolled(object sender, ItemsViewScrolledEventArgs e)
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

In diesem Beispiel gibt der- `OnCarouselViewScrolled` Ereignishandler die Werte des- `ItemsViewScrolledEventArgs` Objekts aus, das das-Ereignis begleitet.

> [!IMPORTANT]
> Das `Scrolled` Ereignis wird für benutzergesteuerte scrollläufe und für programmatische scrollläufe ausgelöst.

## <a name="scroll-an-item-at-an-index-into-view"></a>Scrollen eines Elements an einem Index in der Ansicht

Die erste Methoden Überladung führt einen Bildlauf für [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) das Element am angegebenen Index durch. Bei einem- [`CarouselView`](xref:Xamarin.Forms.CarouselView) Objekt `carouselView` mit dem Namen wird im folgenden Beispiel veranschaulicht, wie das Element am Index 6 in der Ansicht angezeigt wird:

```csharp
carouselView.ScrollTo(6);
```

> [!NOTE]
> Das- [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) Ereignis wird ausgelöst, wenn die- [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methode aufgerufen wird.

## <a name="scroll-an-item-into-view"></a>Bildlauf für ein Element in der Ansicht

Die zweite [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methoden Überladung scrollt das angegebene Element in die Ansicht. Bei einem- [`CarouselView`](xref:Xamarin.Forms.CarouselView) Objekt `carouselView` mit dem Namen wird im folgenden Beispiel veranschaulicht, wie das Element "Proboscis Monkey" in der Ansicht angezeigt wird:

```csharp
MonkeysViewModel viewModel = BindingContext as MonkeysViewModel;
Monkey monkey = viewModel.Monkeys.FirstOrDefault(m => m.Name == "Proboscis Monkey");
carouselView.ScrollTo(monkey);
```

> [!NOTE]
> Das- [`ScrollToRequested`](xref:Xamarin.Forms.ItemsView.ScrollToRequested) Ereignis wird ausgelöst, wenn die- [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) Methode aufgerufen wird.

## <a name="disable-scroll-animation"></a>Scrollanimation deaktivieren

Eine Bild Lauf Animation wird angezeigt, wenn Sie zwischen Elementen in einem wechseln [`CarouselView`](xref:Xamarin.Forms.CarouselView) . Diese Animation findet sowohl für vom Benutzer initiierte scrollt als auch für programmgesteuerte scrollläufe. `IsScrollAnimated`Wenn Sie die-Eigenschaft auf festlegen, `false` wird die Animation für beide scrollkategorien deaktiviert.

Alternativ kann das- `animate` Argument der- `ScrollTo` Methode auf festgelegt werden, `false` um die scrollanimation bei programmatischen scrollt zu deaktivieren:

```csharp
carouselView.ScrollTo(monkey, animate: false);
```

## <a name="control-scroll-position"></a>Scrollposition des Steuer Elements

Beim Scrollen eines Elements in die Ansicht kann die genaue Position des Elements nach Abschluss des Bildlaufs mit dem- `position` Argument der-Methode angegeben werden [`ScrollTo`](xref:Xamarin.Forms.ItemsView.ScrollTo*) . Dieses Argument akzeptiert einen- [`ScrollToPosition`](xref:Xamarin.Forms.ScrollToPosition) Enumerationsmember.

### <a name="makevisible"></a>MakeVisible

Der [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass ein Rollup des Elements ausgeführt werden soll, bis es in der Ansicht sichtbar ist:

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.MakeVisible);
```

Dieser Beispielcode führt zu einem minimalen Bildlauf, der erforderlich ist, um das Element in die Ansicht zu scrollen.

> [!NOTE]
> Der- [`ScrollToPosition.MakeVisible`](xref:Xamarin.Forms.ScrollToPosition) Member wird standardmäßig verwendet, wenn das- `position` Argument beim Aufrufen der-Methode nicht angegeben wird `ScrollTo` .

### <a name="start"></a>Start

Der [`ScrollToPosition.Start`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass für das Element ein Rollup zum Anfang der Ansicht ausgeführt werden soll:

```csharp
carouselView.ScrollTo(monkey, position: ScrollToPosition.Start);
```

Dieser Beispielcode führt dazu, dass das Element zum Anfang der Ansicht gescrollt wird.

### <a name="center"></a>Zentrum

Der [`ScrollToPosition.Center`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass das Element in den Mittelpunkt der Ansicht gescrollt werden soll:

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.Center);
```

Dieser Beispielcode führt dazu, dass das Element in den Mittelpunkt der Ansicht gescrollt wird.

### <a name="end"></a>Ende

Der [`ScrollToPosition.End`](xref:Xamarin.Forms.ScrollToPosition) Member gibt an, dass das Element an das Ende der Ansicht gescrollt werden soll:

```csharp
carouselViewView.ScrollTo(monkey, position: ScrollToPosition.End);
```

Dieser Beispielcode führt dazu, dass das Element an das Ende der Ansicht gescrollt wird.

## <a name="control-scroll-position-when-new-items-are-added"></a>Scrollposition beim Hinzufügen neuer Elemente steuern

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert eine `ItemsUpdatingScrollMode` Eigenschaft, die durch eine bindbare Eigenschaft unterstützt wird. Diese Eigenschaft ruft einen- `ItemsUpdatingScrollMode` Enumerationswert ab, der das Scrollverhalten von darstellt, `CarouselView` wenn ihm neue Elemente hinzugefügt werden, oder legt diesen fest. Die `ItemsUpdatingScrollMode`-Enumeration definiert die folgenden Member:

- `KeepItemsInView`passt den scrolloffset an, damit das erste sichtbare Element angezeigt wird, wenn neue Elemente hinzugefügt werden.
- `KeepScrollOffset`behält den scrolloffset relativ zum Anfang der Liste bei, wenn neue Elemente hinzugefügt werden.
- `KeepLastItemInView`passt den scrolloffset an, um das letzte Element sichtbar zu machen, wenn neue Elemente hinzugefügt werden.

Der Standardwert der- `ItemsUpdatingScrollMode` Eigenschaft ist `KeepItemsInView` . Wenn also neue Elemente zu einem hinzugefügt werden, [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird das erste sichtbare Element in der Liste angezeigt. Um sicherzustellen, dass neu hinzugefügte Elemente immer am Ende der Liste sichtbar sind, sollte die- `ItemsUpdatingScrollMode` Eigenschaft auf festgelegt werden `KeepLastItemInView` :

```xaml
<CarouselView ItemsUpdatingScrollMode="KeepLastItemInView">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsUpdatingScrollMode = ItemsUpdatingScrollMode.KeepLastItemInView
};
```

## <a name="scroll-bar-visibility"></a>Sichtbarkeit der Scrollleiste

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert `HorizontalScrollBarVisibility` `VerticalScrollBarVisibility` die Eigenschaften und, die durch bindbare Eigenschaften unterstützt werden. Diese Eigenschaften erhalten einen- [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) Enumerationswert, der angibt, wann die horizontale oder vertikale Bild Lauf Leiste sichtbar ist, oder legt diesen fest. Die `ScrollBarVisibility`-Enumeration definiert die folgenden Member:

- [`Default`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt das Standardverhalten der Bild Lauf Leiste für die Plattform an, und ist der Standardwert für die `HorizontalScrollBarVisibility` -Eigenschaft und die-Eigenschaft `VerticalScrollBarVisibility` .
- [`Always`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt an, dass Scrollleisten sichtbar sind, auch wenn der Inhalt in die Ansicht passt.
- [`Never`](xref:Xamarin.Forms.ScrollBarVisibility)Gibt an, dass Scrollleisten nicht sichtbar sind, auch wenn der Inhalt nicht in die Ansicht passt.

## <a name="snap-points"></a>Andockpunkte

Wenn ein Benutzer einen Bildlauf initiiert, kann die Endposition des Bildlaufs gesteuert werden, sodass Elemente vollständig angezeigt werden. Diese Funktion wird als Andocken bezeichnet, da Elemente an der Position andocken, wenn der Bildlauf beendet wird. Sie wird durch die folgenden Eigenschaften der- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) Klasse gesteuert:

- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Gibt beim Scrollen das Verhalten der Andockpunkte an, vom Typ [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) .
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)Gibt an, [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) wie Andockpunkte mit Elementen ausgerichtet werden.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

> [!NOTE]
> Beim Ausrichten erfolgt dies in der Richtung, in der die geringste Menge an Bewegung erzeugt wird.

### <a name="snap-points-type"></a>Typ der Andockpunkte

Die- [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType) Enumeration definiert die folgenden Member:

- `None`Gibt an, dass der Bildlauf nicht an Elemente angedostet.
- `Mandatory`Gibt an, dass der Inhalt immer an den nächstgelegenen Punkt ausgerichtet wird, an dem der Bildlauf durchgeführt werden würde, entlang der Richtung der Trägheit.
- `MandatorySingle`Gibt das gleiche Verhalten wie `Mandatory` an, führt jedoch nur ein Element gleichzeitig aus.

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) wird die- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) Eigenschaft auf festgelegt `SnapPointsType.MandatorySingle` , wodurch sichergestellt wird, dass bei einem Bildlauf nur ein Element gleichzeitig durchsucht wird.

Die folgenden Screenshots zeigen eine, bei der das ausrichten deaktiviert [`CarouselView`](xref:Xamarin.Forms.CarouselView) wurde:

[![Screenshot einer carouselview ohne Ausrichtungs Punkte unter IOS und Android](scrolling-images/snappoints-none.png "Carouselview ohne Andockpunkte")](scrolling-images/snappoints-none-large.png#lightbox "Carouselview ohne Andockpunkte")

### <a name="snap-points-alignment"></a>Ausrichtung der Andockpunkte

Die [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment) -Enumeration definiert die Member `Start` , `Center` und `End` .

> [!IMPORTANT]
> Der Wert der- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) Eigenschaft wird nur berücksichtigt, wenn die- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType) Eigenschaft auf oder festgelegt ist `Mandatory` `MandatorySingle` .

#### <a name="start"></a>Start

Der- `SnapPointsAlignment.Start` Member gibt an, dass die Ausrichtungs Punkte am führenden Rand der Elemente ausgerichtet werden. Das folgende XAML-Beispiel zeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Start" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Start
    },
    // ...
};
```

Wenn ein Benutzer einen Bildlauf in einem horizontalen Bildlauf initiiert [`CarouselView`](xref:Xamarin.Forms.CarouselView) , wird das linke Element am linken Rand der Ansicht ausgerichtet:

[![Screenshot einer carouselview mit Start-Snap-Points unter IOS und Android](scrolling-images/snappoints-start.png "Carouselview mit Start-Snap Points")](scrolling-images/snappoints-start-large.png#lightbox "Carouselview mit Start-Snap Points")

#### <a name="center"></a>Zentrum

Der `SnapPointsAlignment.Center` Member gibt an, dass die Punkte am Mittelpunkt der Elemente ausgerichtet werden.

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) ist die- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment) Eigenschaft auf festgelegt `Center` . Aus Gründen der Vollständigkeit wird jedoch im folgenden XAML-Beispiel gezeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="Center" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.Center
    },
    // ...
};
```

Wenn ein Benutzer einen Bildlauf in einem horizontalen Bildlauf initiiert [`CarouselView`](xref:Xamarin.Forms.CarouselView) , wird das Mittel Element an der Mitte der Ansicht ausgerichtet:

[![Screenshot einer carouselview mit Center-Snap-Points unter IOS und Android](scrolling-images/snappoints-center.png "Carouselview mit Center-Snap-Points")](scrolling-images/snappoints-center-large.png#lightbox "Carouselview mit Center-Snap-Points")

#### <a name="end"></a>Ende

Der `SnapPointsAlignment.End` Member gibt an, dass die Ausrichtungs Punkte am nachfolgenden Rand der Elemente ausgerichtet werden. Das folgende XAML-Beispiel zeigt, wie dieser Enumerationsmember festgelegt wird:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal"
                           SnapPointsType="MandatorySingle"
                           SnapPointsAlignment="End" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Horizontal)
    {
        SnapPointsType = SnapPointsType.MandatorySingle,
        SnapPointsAlignment = SnapPointsAlignment.End
    },
    // ...
};
```

Wenn ein Benutzer einen Bildlauf in einem horizontalen Bildlauf initiiert [`CarouselView`](xref:Xamarin.Forms.CarouselView) , wird das Rechte Element am rechten Rand der Sicht ausgerichtet.

[![Screenshot einer carouselview mit endandock Punkten unter IOS und Android](scrolling-images/snappoints-end.png "Carouselview mit endandock Punkten")](scrolling-images/snappoints-end-large.png#lightbox "Carouselview mit endandock Punkten")

## <a name="related-links"></a>Verwandte Links

- [Carouselview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
