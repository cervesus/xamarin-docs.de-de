---
title: Xamarin.FormsStackLayout
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f624674cc6d4ba1bdc34a42fb52fb63ff8a7135a
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137968"
---
# <a name="xamarinforms-stacklayout"></a>Xamarin.FormsStackLayout

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)

[![Xamarin.FormsStackLayout](stacklayout-images/layouts.png "[! Schel. No-Loc (xamarin. Forms)] Stacklayout")](stacklayout-images/layouts-large.png#lightbox "[! Schel. No-Loc (xamarin. Forms)] Stacklayout")

Ein [`StackLayout`](xref:Xamarin.Forms.StackLayout) organisiert untergeordnete Sichten in einem eindimensionalen Stapel, entweder horizontal oder vertikal. Standardmäßig wird ein `StackLayout` vertikal ausgerichtet. Außerdem `StackLayout` kann ein als übergeordnetes Layout verwendet werden, das andere untergeordnete Layouts enthält.

Die- [`StackLayout`](xref:Xamarin.Forms.StackLayout) Klasse definiert die folgenden Eigenschaften:

- [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation)stellt vom Typ [`StackOrientation`](xref:Xamarin.Forms.StackOrientation) die Richtung dar, in der untergeordnete Ansichten positioniert werden. Der Standardwert dieser Eigenschaft ist `Vertical`.
- [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing)`double`gibt den Umfang des Speicherplatzes zwischen jeder untergeordneten Ansicht des Typs an. Der Standardwert dieser Eigenschaft ist sechs geräteunabhängige Einheiten.

Diese Eigenschaften werden von- [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) Objekten unterstützt. Dies bedeutet, dass die Eigenschaften Ziele von Daten Bindungen und formatierten sein können.

Die- [`StackLayout`](xref:Xamarin.Forms.StackLayout) Klasse wird von der- `Layout<T>` Klasse abgeleitet, die eine `Children` Eigenschaft vom Typ definiert `IList<T>` . Die `Children` -Eigenschaft ist der der `ContentProperty` `Layout<T>` -Klasse und muss daher nicht explizit aus XAML festgelegt werden.

> [!TIP]
> Um die bestmögliche layoutleistung zu erzielen, befolgen Sie die Richtlinien unter [Optimieren der layoutleistung](~/xamarin-forms/deploy-test/performance.md#optimize-layout-performance).

## <a name="vertical-orientation"></a>Vertikale Ausrichtung

Der folgende XAML-Code zeigt, wie ein vertikal orientierter erstellt wird [`StackLayout`](xref:Xamarin.Forms.StackLayout) , der verschiedene untergeordnete Sichten enthält:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.VerticalStackLayoutPage"
             Title="Vertical StackLayout demo">
    <StackLayout Margin="20">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel werden ein vertikales [`StackLayout`](xref:Xamarin.Forms.StackLayout) mit [`Label`](xref:Xamarin.Forms.Label) -und- [`BoxView`](xref:Xamarin.Forms.BoxView) Objekten erstellt Standardmäßig gibt es sechs geräteunabhängige Speichereinheiten zwischen den untergeordneten Ansichten:

[![Screenshot eines vertikal ausgerichteten Stacklayout](stacklayout-images/vertical.png "Vertikaler orientierter Stacklayout")](stacklayout-images/vertical-large.png#lightbox "Vertikaler orientierter Stacklayout")

Der entsprechende C#-Code lautet:

```csharp
public class VerticalStackLayoutPageCS : ContentPage
{
    public VerticalStackLayoutPageCS()
    {
        Title = "Vertical StackLayout demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

> [!NOTE]
> Der Wert der- [`Margin`](xref:Xamarin.Forms.View.Margin) Eigenschaft stellt den Abstand zwischen einem Element und seinen angrenzenden Elementen dar. Weitere Informationen finden Sie unter [Ränder und Abstände](margin-and-padding.md).

## <a name="horizontal-orientation"></a>Horizontale Ausrichtung

Der folgende XAML-Code zeigt, wie Sie einen horizontal ausgerichteten erstellen [`StackLayout`](xref:Xamarin.Forms.StackLayout) , indem Sie seine- [`Orientation`](xref:Xamarin.Forms.StackLayout.Orientation) Eigenschaft auf festlegen `Horizontal` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.HorizontalStackLayoutPage"
             Title="Horizontal StackLayout demo">
    <StackLayout Margin="20"
                 Orientation="Horizontal"
                 HorizontalOptions="Center">
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel wird ein horizontales [`StackLayout`](xref:Xamarin.Forms.StackLayout) enthaltende- [`BoxView`](xref:Xamarin.Forms.BoxView) Objekt mit sechs geräteunabhängigen Speichereinheiten zwischen den untergeordneten Sichten erstellt:

[![Screenshot eines horizontal ausgerichteten Stacklayout](stacklayout-images/horizontal.png "Horizontal orientiertes Stacklayout")](stacklayout-images/horizontal-large.png#lightbox "Horizontal orientiertes Stacklayout")

Der entsprechende C#-Code lautet:

```csharp
public HorizontalStackLayoutPageCS()
{
    Title = "Horizontal StackLayout demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Orientation = StackOrientation.Horizontal,
        HorizontalOptions = LayoutOptions.Center,
        Children =
        {
            new BoxView { Color = Color.Red },
            new BoxView { Color = Color.Yellow },
            new BoxView { Color = Color.Blue },
            new BoxView { Color = Color.Green },
            new BoxView { Color = Color.Orange },
            new BoxView { Color = Color.Purple }
        }
    };
}
```

## <a name="space-between-child-views"></a>Leerraum zwischen untergeordneten Ansichten

Der Abstand zwischen untergeordneten Sichten in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout) kann geändert werden, indem die- [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) Eigenschaft auf einen Wert festgelegt wird `double` :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.StackLayoutSpacingPage"
             Title="StackLayout Spacing demo">
    <StackLayout Margin="20"
                 Spacing="0">
        <Label Text="Primary colors" />
        <BoxView Color="Red" />
        <BoxView Color="Yellow" />
        <BoxView Color="Blue" />
        <Label Text="Secondary colors" />
        <BoxView Color="Green" />
        <BoxView Color="Orange" />
        <BoxView Color="Purple" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel werden eine vertikale [`StackLayout`](xref:Xamarin.Forms.StackLayout) mit [`Label`](xref:Xamarin.Forms.Label) -und- [`BoxView`](xref:Xamarin.Forms.BoxView) Objekten erstellt, die keinen Abstand zwischen Ihnen aufweisen:

[![Screenshot eines Stacklayout ohne Abstände](stacklayout-images/spacing.png "Stacklayout ohne Abstände")](stacklayout-images/spacing-large.png#lightbox "Stacklayout ohne Abstände")

> [!TIP]
> Die- [`Spacing`](xref:Xamarin.Forms.StackLayout.Spacing) Eigenschaft kann auf negative Werte festgelegt werden, um die untergeordneten Sichten zu überlappen.

Der entsprechende C#-Code lautet:

```csharp
public class StackLayoutSpacingPageCS : ContentPage
{
    public StackLayoutSpacingPageCS()
    {
        Title = "StackLayout Spacing demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Spacing = 0,
            Children =
            {
                new Label { Text = "Primary colors" },
                new BoxView { Color = Color.Red },
                new BoxView { Color = Color.Yellow },
                new BoxView { Color = Color.Blue },
                new Label { Text = "Secondary colors" },
                new BoxView { Color = Color.Green },
                new BoxView { Color = Color.Orange },
                new BoxView { Color = Color.Purple }
            }
        };
    }
}
```

## <a name="position-and-size-of-child-views"></a>Position und Größe von untergeordneten Ansichten

Die Größe und Position von untergeordneten Sichten innerhalb von [`StackLayout`](xref:Xamarin.Forms.StackLayout) hängt von den Werten der Eigenschaften und der untergeordneten Sichten [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) und den Werten ihrer [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) -und-Eigenschaften ab [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) . In vertikaler [`StackLayout`](xref:Xamarin.Forms.StackLayout) Sicht werden untergeordnete Sichten erweitert, um die verfügbare Breite auszufüllen, wenn ihre Größe nicht explizit festgelegt ist. Entsprechend wird in horizontalen `StackLayout` untergeordneten Sichten erweitert, um die verfügbare Höhe auszufüllen, wenn ihre Größe nicht explizit festgelegt ist.

Die [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) -Eigenschaft und die-Eigenschaft [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) von [`StackLayout`](xref:Xamarin.Forms.StackLayout) und deren untergeordneten Ansichten können auf Felder aus der-Struktur festgelegt werden [`LayoutOptions`](xref:Xamarin.Forms.LayoutOptions) , die zwei Layouteinstellungen kapselt:

- Die *Ausrichtung* bestimmt die Position und Größe einer untergeordneten Ansicht innerhalb des übergeordneten Layouts.
- *Erweiterung* gibt an, ob die untergeordnete Ansicht zusätzlichen Speicherplatz verwenden soll, wenn Sie verfügbar ist.

> [!TIP]
> Legen Sie die [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions) -Eigenschaft und die-Eigenschaft [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions) von nur [`StackLayout`](xref:Xamarin.Forms.StackLayout) dann fest, wenn dies erforderlich ist Die Standardwerte von `LayoutOptions.Fill` und `LayoutOptions.FillAndExpand` ermöglichen die beste Layoutoptimierung. Das Ändern dieser Eigenschaften hat Kosten und beansprucht Speicherplatz, auch wenn Sie auf die Standardwerte zurückgesetzt werden.

### <a name="alignment"></a>Ausrichtung

Im folgenden XAML-Beispiel werden Ausrichtungs Einstellungen für jede untergeordnete Ansicht in der festgelegt [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.AlignmentPage"
             Title="Alignment demo">
    <StackLayout Margin="20">
        <Label Text="Start"
               BackgroundColor="Gray"
               HorizontalOptions="Start" />
        <Label Text="Center"
               BackgroundColor="Gray"
               HorizontalOptions="Center" />
        <Label Text="End"
               BackgroundColor="Gray"
               HorizontalOptions="End" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               HorizontalOptions="Fill" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel werden Ausrichtungs Einstellungen für die-Objekte festgelegt, [`Label`](xref:Xamarin.Forms.Label) um deren Position im zu steuern [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Die [`Start`](xref:Xamarin.Forms.LayoutOptions.Start) [`Center`](xref:Xamarin.Forms.LayoutOptions.Center) Felder,, [`End`](xref:Xamarin.Forms.LayoutOptions.End) und [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill) dienen zum Definieren der Ausrichtung der [`Label`](xref:Xamarin.Forms.Label) Objekte innerhalb des übergeordneten Elements `StackLayout` :

[![Screenshot eines Stacklayout mit fest gelegbarer Ausrichtungsoptionen](stacklayout-images/alignment.png "Stacklayout mit Ausrichtungsoptionen")](stacklayout-images/alignment-large.png#lightbox "Stacklayout mit Ausrichtungsoptionen")

Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse respektiert ausschließlich die Ausrichtungseinstellungen für Ansichten untergeordneter Elemente, die sich in entgegengesetzter Richtung zur `StackLayout`-Ausrichtung befinden. Deshalb legen die Ansichten der untergeordneten [`Label`](xref:Xamarin.Forms.Label)-Klasse innerhalb einer vertikal ausgerichteten `StackLayout`-Klasse ihre [`HorizontalOptions`](xref:Xamarin.Forms.View.HorizontalOptions)-Eigenschaften auf eines der Ausrichtungsfelder fest:

- [`Start`](xref:Xamarin.Forms.LayoutOptions.Start), die den [`Label`](xref:Xamarin.Forms.Label) auf der linken Seite des positioniert [`StackLayout`](xref:Xamarin.Forms.StackLayout) .
- Feld [`Center`](xref:Xamarin.Forms.LayoutOptions.Center), das die [`Label`](xref:Xamarin.Forms.Label)-Klasse in der Mitte der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse positioniert
- [`End`](xref:Xamarin.Forms.LayoutOptions.End), die den [`Label`](xref:Xamarin.Forms.Label) auf der rechten Seite des positioniert [`StackLayout`](xref:Xamarin.Forms.StackLayout) .
- Feld [`Fill`](xref:Xamarin.Forms.LayoutOptions.Fill), das sicherstellt, dass die [`Label`](xref:Xamarin.Forms.Label)-Klasse die gesamte Breite der [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse ausfüllt

Der entsprechende C#-Code lautet:

```csharp
public class AlignmentPageCS : ContentPage
{
    public AlignmentPageCS()
    {
        Title = "Alignment demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Start", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Start },
                new Label { Text = "Center", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Center },
                new Label { Text = "End", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.End },
                new Label { Text = "Fill", BackgroundColor = Color.Gray, HorizontalOptions = LayoutOptions.Fill }
            }
        };
    }
}
```

### <a name="expansion"></a>Ausdehnung

Im folgenden XAML-Beispiel werden die Erweiterungs Einstellungen für jedes [`Label`](xref:Xamarin.Forms.Label) in der festgelegt [`StackLayout`](xref:Xamarin.Forms.StackLayout) :

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.ExpansionPage"
             Title="Expansion demo">
    <StackLayout Margin="20">
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Start"
               BackgroundColor="Gray"
               VerticalOptions="StartAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Center"
               BackgroundColor="Gray"
               VerticalOptions="CenterAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="End"
               BackgroundColor="Gray"
               VerticalOptions="EndAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
        <Label Text="Fill"
               BackgroundColor="Gray"
               VerticalOptions="FillAndExpand" />
        <BoxView BackgroundColor="Red"
                 HeightRequest="1" />
    </StackLayout>
</ContentPage>
```

In diesem Beispiel werden Erweiterungs Einstellungen für die-Objekte festgelegt, [`Label`](xref:Xamarin.Forms.Label) um deren Größe in zu steuern [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Die [`StartAndExpand`](xref:Xamarin.Forms.LayoutOptions.StartAndExpand) [`CenterAndExpand`](xref:Xamarin.Forms.LayoutOptions.CenterAndExpand) Felder,, [`EndAndExpand`](xref:Xamarin.Forms.LayoutOptions.EndAndExpand) und [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) werden zum Definieren der Ausrichtungs Einstellung verwendet, und es wird festgelegt, ob der `Label` mehr Platz einnimmt, wenn er im übergeordneten Element verfügbar ist `StackLayout` :

[![Screenshot eines Stacklayout mit fest gelegbarer Erweiterungsoptionen](stacklayout-images/expansion.png "Stacklayout mit Erweiterungsoptionen")](stacklayout-images/expansion-large.png#lightbox "Stacklayout mit Erweiterungsoptionen")

Eine [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse kann Ansichten untergeordneter Elemente nur in die Orientierungsrichtung erweitern. Deshalb kann die vertikal ausgerichtete `StackLayout`-Klasse die Ansichten untergeordneter [`Label`](xref:Xamarin.Forms.Label)-Klassen erweitern, die ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)-Eigenschaften auf eines der Erweiterungsfelder festgelegt haben. Das bedeutet, dass jede `Label`-Klasse für die vertikale Ausrichtung den gleichen Platz innerhalb der `StackLayout`-Klasse belegt. Allerdings hat nur die letzte `Label`-Klasse, die ihre [`VerticalOptions`](xref:Xamarin.Forms.View.VerticalOptions)-Eigenschaft auf [`FillAndExpand`](xref:Xamarin.Forms.LayoutOptions.FillAndExpand) festlegt, eine andere Größe.

> [!TIP]
> Wenn Sie einen verwenden [`StackLayout`](xref:Xamarin.Forms.StackLayout) , stellen Sie sicher, dass nur eine untergeordnete Ansicht auf festgelegt ist [`LayoutOptions.Expands`](xref:Xamarin.Forms.LayoutOptions.Expands) . Mit dieser Eigenschaft wird sichergestellt, dass das angegebene untergeordnete Element den größten Bereich belegt, der im `StackLayout` verfügbar ist. Zudem ist es Vergeudung, diese Berechnungen mehrmals durchzuführen.

Der entsprechende C#-Code lautet:

```csharp
public ExpansionPageCS()
{
    Title = "Expansion demo";
    Content = new StackLayout
    {
        Margin = new Thickness(20),
        Children =
        {
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "StartAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.StartAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "CenterAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.CenterAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "EndAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.EndAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 },
            new Label { Text = "FillAndExpand", BackgroundColor = Color.Gray, VerticalOptions = LayoutOptions.FillAndExpand },
            new BoxView { BackgroundColor = Color.Red, HeightRequest = 1 }
        }
    };
}
```

> [!IMPORTANT]
> Wenn der gesamte Platz in einer [`StackLayout`](xref:Xamarin.Forms.StackLayout)-Klasse belegt ist, haben Einstellungen für die Erweiterung keine Auswirkungen.

Weitere Informationen zur Ausrichtung und Erweiterung finden Sie unter [Layoutoptionen Xamarin.Forms in ](layout-options.md).

## <a name="nested-stacklayout-objects"></a>Ein-oder-Objekt

Ein- [`StackLayout`](xref:Xamarin.Forms.StackLayout) Objekt kann als übergeordnetes Layout verwendet werden, das `StackLayout` untergeordnete Objekte oder andere untergeordnete Layouts enthält.

Der folgende XAML-Code zeigt ein Beispiel für die Schachtelung von [`StackLayout`](xref:Xamarin.Forms.StackLayout) Objekten:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StackLayoutDemos.Views.CombinedStackLayoutPage"
             Title="Combined StackLayouts demo">
    <StackLayout Margin="20">
        ...
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Red" />
                <Label Text="Red"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Yellow" />
                <Label Text="Yellow"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        <Frame BorderColor="Black"
               Padding="5">
            <StackLayout Orientation="Horizontal"
                         Spacing="15">
                <BoxView Color="Blue" />
                <Label Text="Blue"
                       FontSize="Large"
                       VerticalOptions="Center" />
            </StackLayout>
        </Frame>
        ...
    </StackLayout>
</ContentPage>
```

In diesem Beispiel enthält das übergeordnete Objekt geschachtelte Objekte in- [`StackLayout`](xref:Xamarin.Forms.StackLayout) `StackLayout` [`Frame`](xref:Xamarin.Forms.Frame) Objekten. Das übergeordnete Element `StackLayout` ist vertikal ausgerichtet, während die untergeordneten `StackLayout` Objekte horizontal ausgerichtet werden:

[![Screenshot der in der Struktur abgebildeten Stacklayout-Objekte](stacklayout-images/combined.png "Netsted stacklayouts")](stacklayout-images/combined-large.png#lightbox "Netsted stacklayouts")

> [!IMPORTANT]
> Wenn Sie [`StackLayout`](xref:Xamarin.Forms.StackLayout) Objekte und andere Layouts verschachteln, desto stärker wirkt sich das geschachtelte Layout auf die Leistung aus. Weitere Informationen finden Sie unter [auswählen des richtigen Layouts](~/xamarin-forms/deploy-test/performance.md#choose-the-correct-layout).

Der entsprechende C#-Code lautet:

```csharp
public class CombinedStackLayoutPageCS : ContentPage
{
    public CombinedStackLayoutPageCS()
    {
        Title = "Combined StackLayouts demo";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children =
            {
                new Label { Text = "Primary colors" },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Red },
                            new Label { Text = "Red", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Yellow },
                            new Label { Text = "Yellow", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                new Frame
                {
                    BorderColor = Color.Black,
                    Padding = new Thickness(5),
                    Content = new StackLayout
                    {
                        Orientation = StackOrientation.Horizontal,
                        Spacing = 15,
                        Children =
                        {
                            new BoxView { Color = Color.Blue },
                            new Label { Text = "Blue", FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)), VerticalOptions = LayoutOptions.Center }
                        }
                    }
                },
                // ...
            }
        };
    }
}
```

## <a name="related-links"></a>Verwandte Links

- [Stacklayout-Demos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-stacklayoutdemos)
- [Layoutoptionen inXamarin.Forms](layout-options.md)
- [Layout auswählen Xamarin.Forms](choose-layout.md)
- [Optimieren der Xamarin.Forms App-Leistung](~/xamarin-forms/deploy-test/performance.md)
