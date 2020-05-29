---
title: Einführung in Xamarin.Forms Stile
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
ms.openlocfilehash: 5766af7da3a0cf550a2ccb3a926dad25fd7962eb
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84138819"
---
# <a name="introduction-to-xamarinforms-styles"></a>Einführung in Xamarin.Forms Stile

_Mit Stilen können visuelle Elemente angepasst werden. Stile werden für einen bestimmten Typ definiert und enthalten Werte für die Eigenschaften, die für diesen Typ verfügbar sind._

Xamarin.FormsAnwendungen enthalten häufig mehrere Steuerelemente mit identischer Darstellung. Beispielsweise kann eine Anwendung über mehrere Instanzen verfügen, die [`Label`](xref:Xamarin.Forms.Label) die gleichen Schriftart Optionen und Layoutoptionen aufweisen, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="are not"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
            <Label Text="using styles"
                   HorizontalOptions="Center"
                   VerticalOptions="CenterAndExpand"
                   FontSize="Large" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Im folgenden Codebeispiel wird die Erstellung der entsprechenden Seite in C# dargestellt:

```csharp
public class NoStylesPageCS : ContentPage
{
    public NoStylesPageCS ()
    {
        Title = "No Styles";
        IconImageSource = "csharp.png";
        Padding = new Thickness (0, 20, 0, 0);

        Content = new StackLayout {
            Children = {
                new Label {
                    Text = "These labels",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "are not",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                },
                new Label {
                    Text = "using styles",
                    HorizontalOptions = LayoutOptions.Center,
                    VerticalOptions = LayoutOptions.CenterAndExpand,
                    FontSize = Device.GetNamedSize (NamedSize.Large, typeof(Label))
                }
            }
        };
    }
}
```

Jede [`Label`](xref:Xamarin.Forms.Label) Instanz verfügt über identische Eigenschaftswerte zum Steuern der Darstellung des Texts, der von angezeigt wird `Label` . Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![Darstellung der Bezeichnung ohne Stile](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

Das Festlegen der Darstellung der einzelnen Steuerelemente kann sich wiederholt und fehleranfällig sein. Stattdessen kann ein Stil erstellt werden, der die Darstellung definiert und dann auf die erforderlichen Steuerelemente angewendet wird.

## <a name="create-a-style"></a>Erstellen eines Stils

Die- [`Style`](xref:Xamarin.Forms.Style) Klasse gruppiert eine Auflistung von Eigenschafts Werten in ein Objekt, das dann auf mehrere visuelle Element Instanzen angewendet werden kann. Dadurch wird wiederholtes Markup reduziert, und die Darstellung einer Anwendung kann einfacher geändert werden.

Obwohl Stile in erster Linie für XAML-basierte Anwendungen entwickelt wurden, können Sie auch in c# erstellt werden:

- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die in XAML erstellt werden, werden in der Regel in einer definiert, die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) der-Auflistung eines [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) Steuer Elements, einer Seite oder der-Auflistung [`Resources`](xref:Xamarin.Forms.Application.Resources) der Anwendung zugewiesen ist.
- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die in c# erstellt werden, werden in der Regel in der-Klasse der Seite oder in einer Klasse definiert, die Global aufgerufen werden kann.

Die Auswahl des Speicher Orts für die Verwendung von wirkt sich auf die Verwendung von aus [`Style`](xref:Xamarin.Forms.Style) :

- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die auf der Steuerelement Ebene definiert sind, können nur auf das Steuerelement und seine untergeordneten Elemente angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style)auf der Seitenebene definierte Instanzen können nur auf die Seite und ihre untergeordneten Elemente angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die auf Anwendungsebene definiert werden, können in der gesamten Anwendung angewendet werden.

Jede [`Style`](xref:Xamarin.Forms.Style) Instanz enthält eine Auflistung von einem oder mehreren [`Setter`](xref:Xamarin.Forms.Setter) -Objekten, wobei jede `Setter` eine [`Property`](xref:Xamarin.Forms.Setter.Property) -und eine-Instanz enthält [`Value`](xref:Xamarin.Forms.Setter.Value) . `Property` ist der Name der bindbaren Eigenschaft des Elements, auf das die Formatvorlage angewendet wird, und `Value` ist der Wert, der auf die Eigenschaft angewendet wird.

Jede [`Style`](xref:Xamarin.Forms.Style) Instanz kann *explizit*oder *implizit*sein:

- Eine *explizite* [`Style`](xref:Xamarin.Forms.Style) -Instanz wird definiert, indem ein [`TargetType`](xref:Xamarin.Forms.Style.TargetType) und ein- `x:Key` Wert angegeben werden, und durch Festlegen der-Eigenschaft des Ziel Elements [`Style`](xref:Xamarin.Forms.NavigableElement.Style) auf den `x:Key` Verweis. Weitere Informationen zu *expliziten* Stilen finden Sie unter [explizite Stile](~/xamarin-forms/user-interface/styles/explicit.md).
- Eine *implizite* - [`Style`](xref:Xamarin.Forms.Style) Instanz wird definiert, indem nur ein angegeben wird [`TargetType`](xref:Xamarin.Forms.Style.TargetType) . Die `Style` Instanz wird dann automatisch auf alle Elemente dieses Typs angewendet. Beachten Sie, dass Unterklassen von `TargetType` nicht automatisch auf `Style` angewendet werden. Weitere Informationen zu *impliziten* Stilen finden Sie unter [implizite Stile](~/xamarin-forms/user-interface/styles/implicit.md).

Beim Erstellen eines [`Style`](xref:Xamarin.Forms.Style) ist die- [`TargetType`](xref:Xamarin.Forms.Style.TargetType) Eigenschaft immer erforderlich. Das folgende Codebeispiel zeigt einen *expliziten* Stil (Beachten Sie den), der `x:Key` in XAML erstellt wurde:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Zum Anwenden eines `Style` muss das Zielobjekt ein sein, [`VisualElement`](xref:Xamarin.Forms.VisualElement) das mit dem [`TargetType`](xref:Xamarin.Forms.Style.TargetType) Eigenschafts Wert von übereinstimmt `Style` , wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Stile niedriger in der Ansichts Hierarchie haben Vorrang vor der höheren Definition. Beispielsweise wird festgelegt, dass ein auf [`Style`](xref:Xamarin.Forms.Style) [`Label.TextColor`](xref:Xamarin.Forms.Label.TextColor) `Red` der Anwendungsebene auf festgelegt wird, indem auf Seitenebene auf festgelegt wird `Label.TextColor` `Green` . Ebenso wird ein Stil auf Seitenebene durch einen Stil auf Steuerelement Ebene überschrieben. Wenn außerdem `Label.TextColor` direkt für eine Steuerelement Eigenschaft festgelegt wird, hat dies Vorrang vor beliebigen Stilen.

In den Artikeln in diesem Abschnitt wird erläutert, wie *explizite* und *implizite* Stile erstellt und angewendet werden, wie globale Stile erstellt werden, wie Stil Vererbung verwendet wird, wie auf Stiländerungen zur Laufzeit reagiert wird und wie die in enthaltenen integrierten Stile verwendet werden Xamarin.Forms .

> [!NOTE]
> **Was ist styleId?**
>
> Vor Xamarin.Forms 2,2 [`StyleId`](xref:Xamarin.Forms.Element.StyleId) wurde die-Eigenschaft verwendet, um einzelne Elemente in einer Anwendung zur Identifizierung beim Testen der Benutzeroberfläche und in Design Modulen wie "pixate" zu identifizieren. Xamarin.Forms2,2 hat jedoch die- [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) Eigenschaft eingeführt, die die-Eigenschaft abgelöst hat [`StyleId`](xref:Xamarin.Forms.Element.StyleId) .

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Stil](xref:Xamarin.Forms.Style)
- [Trend](xref:Xamarin.Forms.Setter)
