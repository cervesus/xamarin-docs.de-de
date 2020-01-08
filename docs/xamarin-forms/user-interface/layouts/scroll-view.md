---
title: Xamarin.Forms ScrollView
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms-ScrollView-Klasse verwenden, um Layouts zu präsentieren, die nicht auf einem Bildschirm passt, und welche Inhalte, die für die Tastatur Platz zu machen.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/18/2019
ms.openlocfilehash: bb10cda7c9899f176861ceee712cc876984c56ef
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488269"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)

[`ScrollView`](xref:Xamarin.Forms.ScrollView) Layouts enthält, und führen Sie einen Bildlauf außerhalb des Bildschirms können sie aus. `ScrollView` wird auch verwendet, können Sie Ansichten automatisch in den sichtbaren Bereich des Bildschirms verschieben, wenn die Tastatur angezeigt wird.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

## <a name="purpose"></a>Zweck

[`ScrollView`](xref:Xamarin.Forms.ScrollView) kann verwendet werden, um sicherzustellen, dass größere Ansichten auf kleineren Telefonen gut angezeigt werden. Beispielsweise kann ein Layout, das auf einem iPhone 6 s funktioniert auf einem iPhone 4 s abgeschnitten werden. Mit einem `ScrollView` die abgeschnittenen Teile des Layouts auf den kleineren Bildschirm angezeigt werden können.

## <a name="usage"></a>Verwendungs-

> [!NOTE]
> [`ScrollView`](xref:Xamarin.Forms.ScrollView) Objekte dürfen nicht eingefügt werden. Darüber hinaus `ScrollView`s sollte nicht geschachtelt werden, mit anderen Steuerelementen, die einen Bildlauf, wie z. B. bereitstellen `ListView` und `WebView`.

[`ScrollView`](xref:Xamarin.Forms.ScrollView) macht eine `Content` Eigenschaft verfügbar, die auf eine einzelne Ansicht oder ein Layout festgelegt werden kann. Betrachten Sie dieses Beispiel ein Layout mit einem sehr großen BoxView, gefolgt von einem `Entry`:

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
public class ScrollingDemoCode : ContentPage
{
    public ScrollingDemoCode()
    {
        StackLayout stackLayout = new StackLayout();
        stackLayout.Children.Add(new BoxView { BackgroundColor = Color.Red, HeightRequest = 600, WidthRequest = 150 });
        stackLayout.Children.Add(new Entry());
        ScrollView scrollView = new ScrollView();
        scrollView.Content = stackLayout;
        Content = scrollView;
    }
}
```

Vor dem Bildlauf nach unten, nur die `BoxView` wird angezeigt:

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Beachten Sie, dass, wenn der Benutzer beginnt, geben Sie Text in die `Entry`, die Ansicht einen Bildlauf durchführt, dass es auf dem Bildschirm sichtbar beibehalten:

![](scroll-view-images/scroll-end.png "Entry in ScrollView")

## <a name="properties"></a>Eigenschaften

[`ScrollView`](xref:Xamarin.Forms.ScrollView) definiert die folgenden Eigenschaften:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) Ruft eine [ `Size` ](xref:Xamarin.Forms.Size) Wert, der die Größe des Inhalts darstellt.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Ruft ab oder legt einen [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) Enumerationswert, der die hauptscrollrichtung stehenden Richtung darstellt, der `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) Ruft eine `double` , das das aktuelle X Bildlaufposition darstellt.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) Ruft eine `double` , das die aktuelle Y Bildlaufposition darstellt.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) Ruft ab oder legt einen [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) Wert, der darstellt, wenn die horizontale Schiebeleiste sichtbar ist.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) Ruft ab oder legt einen [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) Wert, der darstellt, wenn die vertikale Schiebeleiste sichtbar ist.

> [!NOTE]
> Das Scrollen kann deaktiviert werden, indem Sie die [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) -Eigenschaft auf `Neither`festlegen.

## <a name="methods"></a>Methoden

[`ScrollView`](xref:Xamarin.Forms.ScrollView) stellt eine `ScrollToAsync` Methode bereit, die verwendet werden kann, um entweder mithilfe von Koordinaten oder durch Angeben einer bestimmten Sicht, die sichtbar gemacht werden soll, einen Bildlauf der Sicht durchführen zu können

Wenn Koordinaten zu verwenden, geben die `x` und `y` Koordinaten, zusammen mit der ein boolescher Wert, der angibt, ob der Bildlauf animiert werden soll:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

> [!IMPORTANT]
> Die `ScrollToAsync`-Methode führt nicht zu einem Bildlauf, wenn die [`ScrollView.Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) -Eigenschaft auf `Neither`festgelegt ist.

Beim Scrollen zu einem bestimmten Element gibt die `ScrollToPosition` Enumeration an, wo in der Ansicht das Element angezeigt wird:

- **Center** &ndash; führt einen Bildlauf durch das Element in der Mitte des sichtbaren Teil der Ansicht.
- **End** &ndash; führt einen Bildlauf durch das Element am Ende der sichtbare Teil der Ansicht.
- **MakeVisible** &ndash; führt einen Bildlauf durch das Element, damit es in der Ansicht angezeigt wird.
- **Starten Sie** &ndash; führt einen Bildlauf durch das Element am Anfang der sichtbare Teil der Ansicht.

Die `IsAnimated` Eigenschaft gibt an, wie die Ansicht ein Bildlauf durchgeführt wird. Wenn `true`festgelegt ist, wird eine Smooth Animation verwendet, anstatt den Inhalt sofort in die Ansicht zu verschieben.

## <a name="events"></a>Ereignisse

[`ScrollView`](xref:Xamarin.Forms.ScrollView) definiert nur ein Ereignis `Scrolled`. `Scrolled` wird ausgelöst, wenn die Ansicht einen Bildlauf beendet wurde. Der Ereignishandler für `Scrolled` nimmt `ScrolledEventArgs`, die die `ScrollX` und `ScrollY` Eigenschaften. Das folgende Beispiel zeigt, wie Sie eine Bezeichnung mit der aktuellen Position des aktualisieren eine `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Beachten Sie, dass Bildlaufpositionen negativ ist, aufgrund der Bounce-Auswirkung, wenn am Ende einer Liste scrollen können.

## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-layout)
- [BusinessTumble-Beispiel (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-businesstumble)
