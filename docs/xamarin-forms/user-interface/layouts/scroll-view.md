---
title: Xamarin.Forms ScrollView
description: In diesem Artikel wird erläutert, wie Sie die Xamarin.Forms-ScrollView-Klasse verwenden, um Layouts zu präsentieren, die nicht auf einem Bildschirm passt, und welche Inhalte, die für die Tastatur Platz zu machen.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: f2bccc9140c4c1c9d5d543a4240178f9301852bb
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38997360"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[`ScrollView`](xref:Xamarin.Forms.ScrollView) Layouts enthält, und führen Sie einen Bildlauf außerhalb des Bildschirms können sie aus. `ScrollView` wird auch verwendet, können Sie Ansichten automatisch in den sichtbaren Bereich des Bildschirms verschieben, wenn die Tastatur angezeigt wird.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms-Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms-Layouts")

Dieser Artikel behandelt Folgendes:

- **[Zweck](#Purpose)**  &ndash; Zweck `ScrollView` und wann sie verwendet wird.
- **[Nutzung](#Usage)**  &ndash; mit `ScrollView` in der Praxis.
- **[Eigenschaften](#Properties)**  &ndash; öffentliche Eigenschaften, die gelesen und geändert werden können.
- **[Methoden](#Methods)**  &ndash; öffentliche Methoden, die aufgerufen werden können, um die Ansicht zu scrollen.
- **[Ereignisse](#Events)**  &ndash; Ereignisse, die zum Lauschen auf Änderungen in der Ansicht Zuständen verwendet werden können.

## <a name="purpose"></a>Zweck

`ScrollView` kann verwendet werden, um sicherzustellen, dass größere Ansichten auf kleinere Smartphones angezeigt. Beispielsweise kann ein Layout, das auf einem iPhone 6 s funktioniert auf einem iPhone 4 s abgeschnitten werden. Mit einem `ScrollView` die abgeschnittenen Teile des Layouts auf den kleineren Bildschirm angezeigt werden können.

## <a name="usage"></a>Verwendung

> [!NOTE]
> `ScrollView`s sollte nicht geschachtelt werden. Darüber hinaus `ScrollView`s sollte nicht geschachtelt werden, mit anderen Steuerelementen, die einen Bildlauf, wie z. B. bereitstellen `ListView` und `WebView`.

`ScrollView` Stellt eine `Content` Eigenschaft, die auf einer einzelnen Ansicht oder das Layout festgelegt werden kann. Betrachten Sie dieses Beispiel ein Layout mit einem sehr großen BoxView, gefolgt von einem `Entry`:

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

Vor dem Bildlauf nach unten, nur die `BoxView` wird angezeigt:

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Beachten Sie, dass, wenn der Benutzer beginnt, geben Sie Text in die `Entry`, die Ansicht einen Bildlauf durchführt, dass es auf dem Bildschirm sichtbar beibehalten:

![](scroll-view-images/scroll-end.png "Eintrag im ScrollView")

## <a name="properties"></a>Eigenschaften

`ScrollView` definiert die folgenden Eigenschaften an:

- [`ContentSize`](xref:Xamarin.Forms.ScrollView.ContentSizeProperty) Ruft eine [ `Size` ](xref:Xamarin.Forms.Size) Wert, der die Größe des Inhalts darstellt.
- [`Orientation`](xref:Xamarin.Forms.ScrollView.OrientationProperty) Ruft ab oder legt einen [ `ScrollOrientation` ](xref:Xamarin.Forms.ScrollOrientation) Enumerationswert, der die hauptscrollrichtung stehenden Richtung darstellt, der `ScrollView`.
- [`ScrollX`](xref:Xamarin.Forms.ScrollView.ScrollXProperty) Ruft eine `double` , das das aktuelle X Bildlaufposition darstellt.
- [`ScrollY`](xref:Xamarin.Forms.ScrollView.ScrollYProperty) Ruft eine `double` , das die aktuelle Y Bildlaufposition darstellt.
- [`HorizontalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.HorizontalScrollBarVisibilityProperty) Ruft ab oder legt einen [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) Wert, der darstellt, wenn die horizontale Schiebeleiste sichtbar ist.
- [`VerticalScrollBarVisibility`](xref:Xamarin.Forms.ScrollView.VerticalScrollBarVisibilityProperty) Ruft ab oder legt einen [ `ScrollBarVisibility` ](xref:Xamarin.Forms.ScrollBarVisibility) Wert, der darstellt, wenn die vertikale Schiebeleiste sichtbar ist.

## <a name="methods"></a>Methoden

`ScrollView` Stellt eine `ScrollToAsync` -Methode, die verwendet werden kann, um einen Bildlauf mit den Koordinaten oder durch Angeben einer bestimmten Sicht, die sichtbar gemacht werden soll.

Wenn Koordinaten zu verwenden, geben die `x` und `y` Koordinaten, zusammen mit der ein boolescher Wert, der angibt, ob der Bildlauf animiert werden soll:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Beim Scrollen an ein bestimmtes Element, das `ScrollToPosition` Enumeration gibt, in dem in der Ansicht wird das Element angezeigt:

- **Center** &ndash; führt einen Bildlauf durch das Element in der Mitte des sichtbaren Teil der Ansicht.
- **End** &ndash; führt einen Bildlauf durch das Element am Ende der sichtbare Teil der Ansicht.
- **MakeVisible** &ndash; führt einen Bildlauf durch das Element, damit es in der Ansicht angezeigt wird.
- **Starten Sie** &ndash; führt einen Bildlauf durch das Element am Anfang der sichtbare Teil der Ansicht.

Die `IsAnimated` Eigenschaft gibt an, wie die Ansicht ein Bildlauf durchgeführt wird. Bei Festlegung auf true festgelegt ist, eine flüssige Animation verwendet werden, wird anstatt sofort den Inhalt in die Ansicht verschoben.

## <a name="events"></a>Ereignisse

`ScrollView` nur ein Ereignis definiert `Scrolled`. `Scrolled` wird ausgelöst, wenn die Ansicht einen Bildlauf beendet wurde. Der Ereignishandler für `Scrolled` nimmt `ScrolledEventArgs`, die die `ScrollX` und `ScrollY` Eigenschaften. Das folgende Beispiel zeigt, wie Sie eine Bezeichnung mit der aktuellen Position des aktualisieren eine `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Beachten Sie, dass Bildlaufpositionen negativ ist, aufgrund der Bounce-Auswirkung, wenn am Ende einer Liste scrollen können.


## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
