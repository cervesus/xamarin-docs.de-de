---
title: Xamarin.Forms ScrollView
description: In diesem Artikel wird erläutert, wie die Klasse Xamarin.Forms ScrollView zum Präsentieren von Layouts, die nicht auf einem Bildschirm passt und für die Inhalte, die für die Tastatur Platz zu machen.
ms.prod: xamarin
ms.assetid: 7B542872-B3D1-49B3-B15E-0E98F53C1F6E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/22/2016
ms.openlocfilehash: 72897013842d464ff9d46825e2b111efbaeb79b8
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245233"
---
# <a name="xamarinforms-scrollview"></a>Xamarin.Forms ScrollView

[`ScrollView`](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) enthält Layouts und können sie einen Bildlauf außerhalb des Bildschirms. `ScrollView` wird auch verwendet, um erlauben Ansichten automatisch in der sichtbare Teil des Bildschirms verschoben werden, wenn die Tastatur angezeigt wird.

[![](scroll-view-images/layouts-sml.png "Xamarin.Forms Layouts")](scroll-view-images/layouts.png#lightbox "Xamarin.Forms Layouts")

Dieser Artikel behandelt Folgendes:

- **[Zweck](#Purpose)**  &ndash; Zweck `ScrollView` und wann sie verwendet wird.
- **[Verwendung](#Usage)**  &ndash; wie `ScrollView` in der Praxis.
- **[Eigenschaften](#Properties)**  &ndash; öffentlichen Eigenschaften, die gelesen und geändert werden können.
- **[Methoden](#Methods)**  &ndash; öffentliche Methoden, die aufgerufen werden können, um die Ansicht zu scrollen.
- **[Ereignisse](#Events)**  &ndash; Ereignisse, die zum Lauschen auf Änderungen in der Ansicht Zustände verwendet werden können.

## <a name="purpose"></a>Zweck

`ScrollView` kann verwendet werden, um sicherzustellen, dass größere Sichten auch für kleinere Telefone anzuzeigen. Beispielsweise kann ein Layout, das auf einem iPhone 6 s funktioniert auf einem iPhone 4 s zugeschnitten werden. Mit einem `ScrollView` entschied abgeschnittenen Teile des Layouts, die auf den kleineren Bildschirm angezeigt werden.

## <a name="usage"></a>Verwendung

> [!NOTE]
> `ScrollView`s sollte nicht geschachtelt werden. Darüber hinaus `ScrollView`s muss nicht mit anderen Steuerelementen, die einen Bildlauf durchzuführen, z. B. bereitstellen verschachtelt sein `ListView` und `WebView`.

`ScrollView` macht eine `Content` Eigenschaft, die mit einer einzigen Ansicht oder dem Layout festgelegt werden kann. Betrachten Sie dieses Beispiel ein Layout mit einem sehr großen BoxView, gefolgt von einem `Entry`:

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

Bevor Sie den Bildlauf nach unten, nur die `BoxView` wird angezeigt:

![](scroll-view-images/scroll-start.png "BoxView in ScrollView")

Beachten Sie, dass, wenn der Benutzer beginnt, geben Sie Text in die `Entry`, per Bildlauf die Ansicht auf dem Bildschirm sichtbar bleiben:

![](scroll-view-images/scroll-end.png "Eintrag im ScrollView")

## <a name="properties"></a>Eigenschaften

ScrollView hat die folgenden Eigenschaften:

- **Inhalt** &ndash; Ruft ab oder legt ihn fest in anzuzeigenden Ansicht an die `ScrollView`.
- **[ContentSize](https://developer.xamarin.com/api/type/Xamarin.Forms.Size/)**  &ndash; schreibgeschützt ist, ruft die Größe des Inhalts, besitzt eine Breite und Höhe Komponente ab. Dies ist eine bindbare Eigenschaft
- **[Ausrichtung](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/)**  &ndash; Dies ist eine [ `ScrollOrientation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollOrientation/), eine Enumeration, die festgelegt werden können, um `Horizontal`, `Vertical`, oder `Both`.
- **ScrollX** &ndash; schreibgeschützt ist, ruft die aktuelle Bildlaufposition in der X-Dimension ab.
- **ScrollY** &ndash; schreibgeschützt ist, ruft die aktuelle Bildlaufposition in der Y-Dimension ab.

## <a name="methods"></a>Methoden

`ScrollView` Stellt eine `ScrollToAsync` -Methode, die verwendet werden kann, um einen Bildlauf Koordinaten zu verwenden oder durch Angeben einer bestimmten Sicht, die sichtbar gemacht werden sollen.

Wenn Koordinaten zu verwenden, geben die `x` und `y` Koordinaten, zusammen mit der ein boolescher Wert, der angibt, ob das Durchführen eines Bildlaufs animiert werden soll:

```csharp
scroll.ScrollToAsync(0, 150, true); //scrolls so that the position at 150px from the top is visible

scroll.ScrollToAsync(label, ScrollToPosition.Start, true); //scrolls so that the label is at the start of the list
```

Bildläufe an ein bestimmtes Element der `ScrollToPosition` Enumeration gibt an, in dem in der Ansicht wird das Element angezeigt:

- **Center** &ndash; verschiebt das Element in die Mitte der den sichtbaren Teil der Ansicht.
- **End** &ndash; verschiebt das Element am Ende den sichtbaren Teil der Ansicht.
- **MakeVisible** &ndash; verschiebt das Element, damit es in der Ansicht angezeigt wird.
- **Starten Sie** &ndash; verschiebt das Element am Anfang der sichtbare Teil der Ansicht.

Die `IsAnimated` Eigenschaft gibt an, wie die Sicht ein Bildlauf durchgeführt wird. Bei festlegen auf "true", die eine reibungslose Animation verwendet werden, wird anstatt sofort genommen wird der Inhalt in der Ansicht.

## <a name="events"></a>Ereignisse

`ScrollView` Stellt nur ein Ereignis `Scrolled`. `Scrolled` wird ausgelöst, wenn die Sicht Durchführen eines Bildlaufs abgeschlossen ist. Der Ereignishandler für `Scrolled` akzeptiert `ScrolledEventArgs`, verfügt über die `ScrollX` und `ScrollY` Eigenschaften. Das folgende Beispiel zeigt, wie eine Bezeichnung mit der aktuellen Position des aktualisiert eine `ScrollView`:

```csharp
Label label = new Label { Text = "Position: " };
ScrollView scroll = new ScrollView();
scroll.Scrolled += (object sender, ScrolledEventArgs e) => {
    label.Text = "Position: " + e.ScrollX + " x " + e.ScrollY;
};
```

Beachten Sie, dass Bildlaufpositionen negativ ist, aufgrund der Bounce Auswirkungen, wenn Sie am Ende einer Liste blättern können.


## <a name="related-links"></a>Verwandte Links

- [Layout (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [BusinessTumble-Beispiel (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
