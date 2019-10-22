---
title: Xamarin. Forms-ScrollView
description: In diesem Artikel wird erläutert, wie Sie mithilfe der xamarin. Forms-ScrollView-Klasse Layouts darstellen, die nicht nur auf einen Bildschirm passen und über Inhalte verfügen, die Platz für die Tastatur machen.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 09/17/2019
ms.openlocfilehash: 8d523c6da6ca7feaf6894123822f789f37455865
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696851"
---
# <a name="xamarinforms-scrollview"></a>Xamarin. Forms-ScrollView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) enthält Layouts und ermöglicht den Bildlauf nach unten. `ScrollView` wird auch verwendet, um zuzulassen, dass Ansichten automatisch zum sichtbaren Teil des Bildschirms wechseln, wenn die Tastatur angezeigt wird.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Zweck

[`ScrollView`](xref:Xamarin.Forms.ScrollView) kann verwendet werden, um sicherzustellen, dass größere Ansichten auf kleineren Telefonen gut angezeigt werden. Beispielsweise kann ein Layout, das auf einem iPhone 6S funktioniert, auf ein iPhone 4S zugeschnitten werden. Durch die Verwendung einer `ScrollView` können die ausgeschnittenen Teile des Layouts auf dem kleineren Bildschirm angezeigt werden.

## <a name="usage"></a>Verwendung

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView) Objekte dürfen nicht eingefügt werden. Außerdem sollten `ScrollView`s nicht mit anderen Steuerelementen, die einen Bildlauf wie `ListView` und `WebView` bereitstellen, eingefügt werden.

[`ScrollView`](xref:Xamarin.Forms.ScrollView) macht eine `Content` Eigenschaft verfügbar, die auf eine einzelne Ansicht oder ein Layout festgelegt werden kann. Betrachten Sie dieses Beispiel für ein Layout mit einer sehr großen boxview, gefolgt von einem `Entry`:

```xaml
<ContentPage.Content>
    <ScrollView>
        <StackLayout>
            <BoxView BackgroundColor="Red" HeightRequest="600" WidthRequest="150" />
            <Entry />
        </StackLayout>
    </ScrollView>
</ContentPage.Content>
```

In C#:

```csharp
var scroll = new ScrollView();
Content = scroll;
var stack = new StackLayout();
stack.Children.Add(new BoxView { BackgroundColor = Color.Red,    HeightRequest = 600, WidthRequest = 600 });
stack.Children.Add(new Entry());
```

Bevor der Benutzer einen Bildlauf nach unten durchführt, wird nur der `BoxView` sichtbar:

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Beachten Sie Folgendes: Wenn der Benutzer mit der Eingabe von Text in der `Entry` beginnt, wird die Ansicht mit einem Bildlauf fortgeführt

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>Eigenschaften

[`ScrollView`](xref:Xamarin.Forms.ScrollView) definiert die folgenden Eigenschaften:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) Ruft einen [`Size`](xref:Xamarin.Forms.Size) Wert ab, der die Größe des Inhalts darstellt.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Ruft einen [`ScrollOrientation`](xref:Xamarin.Forms.ScrollOrientation) Enumerationswert ab, der die Scrollrichtung des `ScrollView` darstellt, oder legt ihn fest.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) Ruft eine `double` ab, die die aktuelle X-Scrollposition darstellt.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) Ruft ein `double` ab, das die aktuelle Y-Scrollposition darstellt.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) Ruft einen [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) Wert ab, der angibt, wann die horizontale Schiebe Leiste sichtbar ist, oder legt ihn fest.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) Ruft einen [`ScrollBarVisibility`](xref:Xamarin.Forms.ScrollBarVisibility) Wert ab, der angibt, wann die vertikale Schiebe Leiste sichtbar ist, oder legt ihn fest.

> [!NOTE]
> Das Scrollen kann deaktiviert werden, indem Sie die [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) -Eigenschaft auf `Neither` festlegen.

## <a name="methods"></a>Methoden

[`ScrollView`](xref:Xamarin.Forms.ScrollView) stellt eine `ScrollToAsync` Methode bereit, die verwendet werden kann, um entweder mithilfe von Koordinaten oder durch Angeben einer bestimmten Sicht, die sichtbar gemacht werden soll, einen Bildlauf der Sicht durchführen zu können

Wenn Sie Koordinaten verwenden, geben Sie die `x`-und `y` Koordinaten sowie einen booleschen Wert an, der angibt, ob der Bildlauf animiert werden soll:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> Die `ScrollToAsync`-Methode führt nicht zu einem Bildlauf, wenn die [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) -Eigenschaft auf `Neither` festgelegt ist.

Beim Scrollen zu einem bestimmten Element gibt die `ScrollToPosition` Enumeration an, wo in der Ansicht das Element angezeigt wird:

- **Zentri&ndash;** führt im-Element einen Bildlauf zur Mitte des sichtbaren Teils der Sicht durch.
- **End** &ndash; führt im-Element einen Bildlauf zum Ende des sichtbaren Teils der Sicht durch.
- **MakeVisible** &ndash; scrollt das-Element, sodass es in der Ansicht sichtbar ist.
- **Start** &ndash; führt im-Element einen Bildlauf zum Anfang des sichtbaren Teils der Sicht durch.

Die `IsAnimated`-Eigenschaft gibt an, wie ein Rollup der Sicht ausgeführt wird. Wenn `true` festgelegt ist, wird eine Smooth Animation verwendet, anstatt den Inhalt sofort in die Ansicht zu verschieben.

## <a name="events"></a>Ereignisse

[`ScrollView`](xref:Xamarin.Forms.ScrollView) definiert nur ein Ereignis `Scrolled`. `Scrolled` wird ausgelöst, wenn die Ansicht den Bildlauf abgeschlossen hat. Der Ereignishandler für `Scrolled` nimmt `ScrolledEventArgs` auf, der über die Eigenschaften `ScrollX` und `ScrollY` verfügt. Im folgenden wird veranschaulicht, wie eine Bezeichnung mit der aktuellen Bild Lauf Position eines `ScrollView` aktualisiert wird:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Beachten Sie, dass scrollpositionen aufgrund des Sprung Effekts beim Scrollen am Ende einer Liste negativ sein können.

## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [Businesstumm Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
