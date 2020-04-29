---
title: Xamarin. Forms carouselview-Layout
description: Standardmäßig zeigt eine carouselview die Elemente horizontal an. Es ist jedoch auch eine vertikale Ausrichtung möglich.
ms.prod: xamarin
ms.assetid: fede0382-c972-4023-a4ea-fe5cadec91a6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/28/2020
ms.openlocfilehash: 3242148fa97e6b3795b57b2fed86f3643a5ecdf6
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82517422"
---
# <a name="xamarinforms-carouselview-layout"></a>Xamarin. Forms carouselview-Layout

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)

[`CarouselView`](xref:Xamarin.Forms.CarouselView)definiert die folgenden Eigenschaften, die das Layout Steuern:

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)Gibt das zu `LinearItemsLayout`verwendende Layout des Typs an.
- `PeekAreaInsets`Gibt an [`Thickness`](xref:Xamarin.Forms.Thickness), wie weit angrenzende Elemente teilweise von sichtbar gemacht werden sollen.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können.

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) werden die Elemente in einer horizontalen Ausrichtung angezeigt. Auf dem Bildschirm wird ein einzelnes Element angezeigt, das eine Schwenkbewegung und eine rückwärts Navigation durch die Auflistung von Elementen zur Folge hat. Es ist jedoch auch eine vertikale Ausrichtung möglich. Dies liegt daran, [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) dass die-Eigenschaft `LinearItemsLayout`vom Typ ist, der von [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) der-Klasse erbt. Die `ItemsLayout` -Klasse definiert die folgenden Eigenschaften:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)Gibt die Richtung [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)des Typs an, in der die [`CarouselView`](xref:Xamarin.Forms.CarouselView) erweitert werden, wenn Elemente hinzugefügt werden.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)Gibt an [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment), wie Andockpunkte mit Elementen ausgerichtet werden.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)Gibt beim Scrollen [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)das Verhalten der Andockpunkte an, vom Typ.

Diese Eigenschaften werden von [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen sein können. Weitere Informationen zu Endpunkt Punkten finden Sie im Bild Lauf Handbuch zu [xamarin. Forms CollectionView](scrolling.md) unter [Snap Points](scrolling.md#snap-points) .

Die [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) -Enumeration definiert die folgenden Member:

- `Vertical`Gibt an, [`CarouselView`](xref:Xamarin.Forms.CarouselView) dass das vertikal erweitert wird, wenn Elemente hinzugefügt werden.
- `Horizontal`Gibt an, [`CarouselView`](xref:Xamarin.Forms.CarouselView) dass der horizontal erweitert wird, wenn Elemente hinzugefügt werden.

Die `LinearItemsLayout` -Klasse erbt von [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) der-Klasse und definiert `ItemSpacing` eine-Eigenschaft vom `double`Typ, die den leeren Leerraum um jedes Element darstellt. Der Standardwert dieser Eigenschaft ist 0, und der Wert muss immer größer oder gleich 0 sein. Die `LinearItemsLayout` -Klasse definiert auch `Vertical` statische `Horizontal` Member und. Diese Member können zum Erstellen vertikaler oder horizontaler Listen verwendet werden. Alternativ kann ein `LinearItemsLayout` -Objekt erstellt werden, wobei ein [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) Enumerationsmember als Argument angegeben wird.

> [!NOTE]
> [`CarouselView`](xref:Xamarin.Forms.CarouselView)verwendet die systemeigenen layoutengines, um das Layout auszuführen.

## <a name="horizontal-layout"></a>Horizontales Layout

Standardmäßig [`CarouselView`](xref:Xamarin.Forms.CarouselView) zeigt seine Elemente horizontal an. Daher ist es nicht erforderlich, die-Eigenschaft [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) für die Verwendung dieses Layouts festzulegen:

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

Alternativ kann dieses Layout auch erreicht werden, [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) indem die-Eigenschaft auf ein `LinearItemsLayout` -Objekt festgelegt `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) wird. dabei wird der `Orientation` -Enumerationsmember als Eigenschafts Wert angegeben:

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

Dies führt zu einem Layout, das horizontal vergrößert wird, wenn neue Elemente hinzugefügt werden:

[![Screenshot eines horizontalen caretzeitenlayouts unter IOS und Android](layout-images/horizontal.png "Karouselview horizontales Layout")](layout-images/horizontal-large.png#lightbox "Karouselview horizontales Layout")

## <a name="vertical-layout"></a>Vertikales Layout

[`CarouselView`](xref:Xamarin.Forms.CarouselView)kann die Elemente vertikal anzeigen, indem die [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) -Eigenschaft auf `LinearItemsLayout` ein-Objekt fest `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) gelegt wird. dabei wird `Orientation` der-Enumerationsmember als Eigenschafts Wert angegeben:

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

Dies führt zu einem Layout, das vertikal wächst, wenn neue Elemente hinzugefügt werden:

[![Screenshot eines vertikalen karouselview-Layouts unter IOS und Android](layout-images/vertical.png "Karouselview vertikal Layout")](layout-images/vertical-large.png#lightbox "Karouselview vertikal Layout")

## <a name="partially-visible-adjacent-items"></a>Teilweise sichtbare angrenzende Elemente

Standardmäßig werden [`CarouselView`](xref:Xamarin.Forms.CarouselView) vollständige Elemente gleichzeitig angezeigt. Dieses Verhalten kann jedoch geändert werden, indem die- `PeekAreaInsets` Eigenschaft auf einen `Thickness` Wert festgelegt wird, der angibt, wie weit angrenzende Elemente partiell sichtbar gemacht werden sollen. Dies kann hilfreich sein, um den Benutzern mitzuteilen, dass zusätzliche Elemente angezeigt werden müssen. Der folgende XAML-Code zeigt ein Beispiel für das Festlegen dieser Eigenschaft:

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

Das Ergebnis ist, dass angrenzende Elemente teilweise auf dem Bildschirm verfügbar gemacht werden:

[![Screenshot einer CollectionView mit teilweise sichtbaren angrenzenden Elementen unter IOS und Android](layout-images/peek-items.png "Carouselview Peek Area insets")](layout-images/peek-items-large.png#lightbox "Eingangsbereich-Insets für carouselview")

## <a name="item-spacing"></a>Element Abstand

Standardmäßig gibt es kein Leerzeichen zwischen jedem Element in einem [`CarouselView`](xref:Xamarin.Forms.CarouselView). Dieses Verhalten kann geändert werden, indem die `ItemSpacing` -Eigenschaft für das von verwendete Element Layout `CarouselView`festgelegt wird.

Wenn eine [`CarouselView`](xref:Xamarin.Forms.CarouselView) seine [`ItemsLayout`](xref:Xamarin.Forms.CarouselView.ItemsLayout) -Eigenschaft auf ein `LinearItemsLayout` -Objekt festlegt `LinearItemsLayout.ItemSpacing` , kann die-Eigenschaft auf `double` einen Wert festgelegt werden, der den Leerraum zwischen Elementen darstellt:

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
> Die `LinearItemsLayout.ItemSpacing` -Eigenschaft verfügt über einen Satz von Validierungs Rückrufen, mit dem sichergestellt wird, dass der Wert der-Eigenschaft immer größer oder gleich 0 ist.

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

Dieser Code ergibt ein vertikales Layout, das einen Abstand von 20 zwischen Elementen aufweist.

## <a name="dynamic-resizing-of-items"></a>Dynamische Größe von Elementen

Elemente in einer [`CarouselView`](xref:Xamarin.Forms.CarouselView) können zur Laufzeit dynamisch angepasst werden, indem layoutbezogene Eigenschaften von Elementen in der [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)geändert werden. Im folgenden Codebeispiel werden z. b. [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) die [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) -Eigenschaft und [`Image`](xref:Xamarin.Forms.Image) die-Eigenschaft eines `HeightRequest` -Objekts und die [`Frame`](xref:Xamarin.Forms.Frame)-Eigenschaft des übergeordneten Elements geändert:

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(150) ? 200 : 150;
    Frame frame = ((Frame)image.Parent.Parent);
    frame.HeightRequest = frame.HeightRequest.Equals(300) ? 350 : 300;
}
```

Der `OnImageTapped` Ereignishandler wird als Reaktion auf ein [`Image`](xref:Xamarin.Forms.Image) angezeigter Objekt ausgeführt und ändert die Abmessungen des Bilds (und des über `Frame`geordneten Elements), sodass es leichter angezeigt werden kann:

[![Screenshot einer "carouselview" mit dynamischer Elementgröße unter IOS und Android](layout-images/runtime-resizing.png "Dynamische Elementgröße von carouselview")](layout-images/runtime-resizing-large.png#lightbox "Dynamische Elementgröße von carouselview")

## <a name="right-to-left-layout"></a>Layout von rechts nach links

[`CarouselView`](xref:Xamarin.Forms.CarouselView)kann seinen Inhalt in einer Richtung von rechts nach links durch Festlegen der- [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Eigenschaft auf [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft)festlegen. Allerdings sollte `FlowDirection` die-Eigenschaft idealerweise auf einem Seiten-oder Stamm Layout festgelegt werden, was bewirkt, dass alle Elemente auf der Seite bzw. im Stamm Layout auf die Fluss Richtung Antworten:

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

Der Standard [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) Wert für ein Element mit einem über [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)geordneten Element ist. Daher erbt [`CarouselView`](xref:Xamarin.Forms.CarouselView) der den `FlowDirection` -Eigenschafts Wert von [`ContentPage`](xref:Xamarin.Forms.ContentPage).

Weitere Informationen zur Fluss Richtung finden Sie unter von [rechts nach links lokalisierte Lokalisierung](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Verwandte Links

- [Carouselview (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-carouselviewdemos/)
- [Lokalisierung von rechts nach links](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Xamarin. Forms carouselview-Bildlauf](scrolling.md)
