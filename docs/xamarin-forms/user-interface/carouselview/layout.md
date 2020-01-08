---
title: Xamarin. Forms carouselview-Layout
description: Standardmäßig zeigt eine carouselview die Elemente horizontal an. Es ist jedoch auch eine vertikale Ausrichtung möglich.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/14/2019
ms.openlocfilehash: 0149a66fedd98a94f1c9d96bf8e7e57715d1b90b
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75488256"
---
# <a name="xamarinforms-carouselview-layout"></a>Xamarin. Forms carouselview-Layout

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView) definiert die folgenden Eigenschaften, die das Layout Steuern:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)vom Typ `LinearItemsLayout`gibt das Layout an, das verwendet werden soll.
- `PeekAreaInsets`vom Typ [`Thickness`](xref:Xamarin.Forms.Thickness)gibt an, wie weit angrenzende Elemente teilweise durch sichtbar gemacht werden sollen.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Standardmäßig werden die Elemente in einem [`CarouselView`](xref:Xamarin.Forms.CarouselView) horizontal ausgerichtet angezeigt. Auf dem Bildschirm wird ein einzelnes Element angezeigt, das eine Schwenkbewegung und eine rückwärts Navigation durch die Auflistung von Elementen zur Folge hat. Es ist jedoch auch eine vertikale Ausrichtung möglich. Dies liegt daran, dass die [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) -Eigenschaft vom Typ `LinearItemsLayout`ist, der von der [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) -Klasse erbt. Die `ItemsLayout`-Klasse definiert die folgenden Eigenschaften:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)vom Typ [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)gibt die Richtung an, in der die [`CarouselView`](xref:Xamarin.Forms.CarouselView) erweitert werden, wenn Elemente hinzugefügt werden.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)vom Typ [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)gibt an, wie Andockpunkte an Elementen ausgerichtet werden.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)vom Typ [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)gibt das Verhalten der Andockpunkte beim Scrollen an.

Diese Eigenschaften werden durch [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekte gestützt, was bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können. Weitere Informationen zu Endpunkt Punkten finden Sie im Bild Lauf Handbuch zu [xamarin. Forms CollectionView](scrolling.md) unter [Snap Points](scrolling.md#snap-points) .

Die [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) -Enumeration definiert die folgenden Member:

- `Vertical` gibt an, dass der [`CarouselView`](xref:Xamarin.Forms.CarouselView) vertikal erweitert wird, wenn Elemente hinzugefügt werden.
- `Horizontal` gibt an, dass der [`CarouselView`](xref:Xamarin.Forms.CarouselView) horizontal erweitert wird, wenn Elemente hinzugefügt werden.

Die `LinearItemsLayout`-Klasse erbt von der [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) -Klasse und definiert eine `ItemSpacing`-Eigenschaft vom Typ `double`, die den leeren Leerraum um jedes Element darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer oder gleich 0 sein. Die `LinearItemsLayout`-Klasse definiert auch statische `Vertical` und `Horizontal` Member. Diese Member können zum Erstellen vertikaler oder horizontaler Listen verwendet werden. Alternativ kann ein `LinearItemsLayout` Objekt erstellt werden, das einen [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) Enumerationsmember als Argument angibt.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView) verwendet die systemeigenen layoutengines zum Durchführen des Layouts.

## <a name="horizontal-layout"></a>Horizontales Layout

Standardmäßig werden die Elemente [`CarouselView`](xref:Xamarin.Forms.CarouselView) horizontal angezeigt. Daher ist es nicht erforderlich, die [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) -Eigenschaft für die Verwendung dieses Layouts festzulegen:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Alternativ kann dieses Layout auch erreicht werden, indem die [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) -Eigenschaft auf ein `LinearItemsLayout` Objekt festgelegt wird. dabei wird der `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) Enumerationsmember als `Orientation`-Eigenschafts Wert angegeben:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

Dies führt zu einem Layout, das horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden.

## <a name="vertical-layout"></a>Vertikales Layout

[`CarouselView`](xref:Xamarin.Forms.CarouselView) können seine Elemente vertikal anzeigen, indem Sie die [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) -Eigenschaft auf ein `LinearItemsLayout` Objekt festlegen und dabei den `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) Enumerationsmember als `Orientation`-Eigenschafts Wert angeben:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CarouselView.ItemsLayout>
    <CarouselView.ItemTemplate>
        <DataTemplate>
            <StackLayout>
                <Frame HasShadow="True"
                       BorderColor="DarkGray"
                       CornerRadius="5"
                       Margin="20"
                       HeightRequest="300"
                       HorizontalOptions="Center"
                       VerticalOptions="CenterAndExpand">
                    <StackLayout>
                        <Label Text="{Binding Name}"
                               FontAttributes="Bold"
                               FontSize="Large"
                               HorizontalOptions="Center"
                               VerticalOptions="Center" />
                        <Image Source="{Binding ImageUrl}"
                               Aspect="AspectFill"
                               HeightRequest="150"
                               WidthRequest="150"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Location}"
                               HorizontalOptions="Center" />
                        <Label Text="{Binding Details}"
                               FontAttributes="Italic"
                               HorizontalOptions="Center"
                               MaxLines="5"
                               LineBreakMode="TailTruncation" />
                    </StackLayout>
                </Frame>
            </StackLayout>
        </DataTemplate>
    </CarouselView.ItemTemplate>
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

Dies führt zu einem Layout, das vertikal wächst, wenn neue Elemente hinzugefügt werden.

## <a name="partially-visible-adjacent-items"></a>Teilweise sichtbare angrenzende Elemente

Standardmäßig zeigt [`CarouselView`](xref:Xamarin.Forms.CarouselView) vollständige Elemente gleichzeitig an. Dieses Verhalten kann jedoch geändert werden, indem die `PeekAreaInsets`-Eigenschaft auf einen `Thickness` Wert festgelegt wird, der angibt, wie weit angrenzende Elemente partiell sichtbar gemacht werden sollen. Dies kann hilfreich sein, um den Benutzern mitzuteilen, dass zusätzliche Elemente angezeigt werden müssen. Der folgende XAML-Code zeigt ein Beispiel für das Festlegen dieser Eigenschaft:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}"
              PeekAreaInsets="100">
    ...
</CarouselView>
```

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    PeekAreaInsets = new Thickness(100)
};
```

Das Ergebnis ist, dass angrenzende Elemente teilweise auf dem Bildschirm verfügbar gemacht werden.

## <a name="item-spacing"></a>Element Abstand

Standardmäßig weist jedes Element in einer [`CarouselView`](xref:Xamarin.Forms.CarouselView) keinen leeren Leerraum auf. Dieses Verhalten kann geändert werden, indem Eigenschaften für das vom `CarouselView`verwendete Element Layout festgelegt werden.

Wenn eine [`CarouselView`](xref:Xamarin.Forms.CarouselView) die [`ItemsLayout`](xref:Xamarin.Forms.ItemsView.ItemsLayout) -Eigenschaft auf ein `LinearItemsLayout` Objekt festlegt, kann die `LinearItemsLayout.ItemSpacing`-Eigenschaft auf einen `double` Wert festgelegt werden, der den leeren Bereich um jedes Element darstellt:

```xaml
<CarouselView ItemsSource="{Binding Monkeys}">
    <CarouselView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CarouselView.ItemsLayout>
    ...
</CarouselView>
```

> [!NOTE]
> Für die `LinearItemsLayout.ItemSpacing`-Eigenschaft ist ein Validierungs Rückruf festgelegt, mit dem sichergestellt wird, dass der Wert der-Eigenschaft immer größer oder gleich 0 ist.

Der entsprechende C#-Code lautet:

```csharp
CarouselView carouselView = new CarouselView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Dieser Code ergibt ein vertikales Layout mit einem Abstand von 20 um jedes Element.

## <a name="dynamic-resizing-of-items"></a>Dynamische Größe von Elementen

Elemente in einer [`CarouselView`](xref:Xamarin.Forms.CarouselView) können zur Laufzeit dynamisch angepasst werden, indem layoutbezogene Eigenschaften von Elementen innerhalb der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)geändert werden. Im folgenden Codebeispiel werden z. b. die Eigenschaften [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) und [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) eines [`Image`](xref:Xamarin.Forms.Image) Objekts und die `HeightRequest`-Eigenschaft des übergeordneten [`Frame`](xref:Xamarin.Forms.Frame)geändert:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

Der `OnImageTapped` Ereignishandler wird als Reaktion auf ein [`Image`](xref:Xamarin.Forms.Image) Objekt ausgeführt, das abgetippt wird, und ändert die Abmessungen des Bilds (und des übergeordneten Frames), sodass es einfacher angezeigt wird.

## <a name="right-to-left-layout"></a>Layout von rechts nach links

[`CarouselView`](xref:Xamarin.Forms.CarouselView) können seinen Inhalt von rechts nach links durchlaufen, indem Sie dessen [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) -Eigenschaft auf [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)festlegen. Die `FlowDirection`-Eigenschaft sollte jedoch idealerweise auf einem Seiten-oder Stamm Layout festgelegt werden, was bewirkt, dass alle Elemente auf der Seite bzw. im Stamm Layout auf die Fluss Richtung Antworten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CarouselViewDemos.Views.HorizontalTemplateLayoutRTLPage"
             Title="Horizontal layout (RTL FlowDirection)"
             FlowDirection="RightToLeft">    
    <CarouselView ItemsSource="{Binding Monkeys}">
        ...
    </CarouselView>
</ContentPage>
```

Der Standard [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) für ein Element mit einem übergeordneten Element ist [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Daher erbt der [`CarouselView`](xref:Xamarin.Forms.CarouselView) den `FlowDirection`-Eigenschafts Wert vom [`ContentPage`](xref:Xamarin.Forms.ContentPage).

Weitere Informationen zur Fluss Richtung finden Sie unter von [rechts nach links lokalisierte Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Verwandte Links

- [Carouselview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Lokalisierung von rechts nach links](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin. Forms carouselview-Bildlauf](scrolling.md)
