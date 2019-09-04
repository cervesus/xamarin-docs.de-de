---
title: Einführung in xamarin. Forms-Stile
description: Mit Stilen können visuelle Elemente angepasst werden. Stile werden für einen bestimmten Typ definiert und enthalten Werte für die Eigenschaften, die für diesen Typ verfügbar sind.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 35f8dad3590c07ceb3c93aa735b8c02d75098498
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228174"
---
# <a name="introduction-to-xamarinforms-styles"></a>Einführung in xamarin. Forms-Stile

_Mit Stilen können visuelle Elemente angepasst werden. Stile werden für einen bestimmten Typ definiert und enthalten Werte für die Eigenschaften, die für diesen Typ verfügbar sind._

Xamarin. Forms-Anwendungen enthalten häufig mehrere Steuerelemente mit identischer Darstellung. Beispielsweise kann eine Anwendung über mehrere [`Label`](xref:Xamarin.Forms.Label) Instanzen verfügen, die die gleichen Schriftart Optionen und Layoutoptionen aufweisen, wie im folgenden XAML-Codebeispiel gezeigt:

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

Jede [`Label`](xref:Xamarin.Forms.Label) Instanz verfügt über identische Eigenschaftswerte zum Steuern der Darstellung des Texts, der `Label`von angezeigt wird. Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

[![Darstellung der Bezeichnung ohne Stile](introduction-images/no-styles.png)](introduction-images/no-styles-large.png#lightbox)

Das Festlegen der Darstellung der einzelnen Steuerelemente kann sich wiederholt und fehleranfällig sein. Stattdessen kann ein Stil erstellt werden, der die Darstellung definiert und dann auf die erforderlichen Steuerelemente angewendet wird.

## <a name="create-a-style"></a>Erstellen eines Stils

Die [`Style`](xref:Xamarin.Forms.Style) -Klasse gruppiert eine Auflistung von Eigenschafts Werten in ein Objekt, das dann auf mehrere visuelle Element Instanzen angewendet werden kann. Dadurch wird wiederholtes Markup reduziert, und die Darstellung einer Anwendung kann einfacher geändert werden.

Obwohl Stile in erster Linie für XAML-basierte Anwendungen entwickelt wurden, können Sie auch in C#erstellt werden:

- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die in XAML erstellt werden, werden [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) in der Regel in einer [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) definiert, die der-Auflistung eines Steuer Elements, [`Resources`](xref:Xamarin.Forms.Application.Resources) einer Seite oder der-Auflistung der Anwendung zugewiesen ist.
- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die C# in erstellt werden, werden in der Regel in der-Klasse der Seite oder in einer Klasse definiert, die Global aufgerufen werden kann.

Die Entscheidung, wo Sie eine [`Style`](xref:Xamarin.Forms.Style)-Klasse definieren, hat Einfluss darauf, wo Sie sie verwenden können:

- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die auf der Steuerelement Ebene definiert sind, können nur auf das Steuerelement und seine untergeordneten Elemente angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style)auf der Seitenebene definierte Instanzen können nur auf die Seite und ihre untergeordneten Elemente angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style)Instanzen, die auf Anwendungsebene definiert werden, können in der gesamten Anwendung angewendet werden.

Jede [`Style`](xref:Xamarin.Forms.Style) Instanz enthält eine Auflistung von einem oder mehreren [`Setter`](xref:Xamarin.Forms.Setter) -Objekten, wobei `Setter` jede eine [`Property`](xref:Xamarin.Forms.Setter.Property) -und [`Value`](xref:Xamarin.Forms.Setter.Value)eine-Instanz enthält. Der `Property` ist der Name der bindbaren Eigenschaft des Elements, auf das der Stil angewendet wird, `Value` und ist der Wert, der auf die Eigenschaft angewendet wird.

Jede [`Style`](xref:Xamarin.Forms.Style) Instanz kann *explizit*oder *implizit*sein:

- Eine *explizite* [`Style`](xref:Xamarin.Forms.Style) -Instanz wird definiert, indem [`TargetType`](xref:Xamarin.Forms.Style.TargetType) ein und `x:Key` ein-Wert angegeben werden, und durch Festlegen [`Style`](xref:Xamarin.Forms.NavigableElement.Style) der-Eigenschaft `x:Key` des Ziel Elements auf den Verweis. Weitere Informationen zu *expliziten* Stilen finden Sie unter [explizite Stile](~/xamarin-forms/user-interface/styles/explicit.md).
- Eine *implizite* [`Style`](xref:Xamarin.Forms.Style) -Instanz wird definiert, indem nur [`TargetType`](xref:Xamarin.Forms.Style.TargetType)ein angegeben wird. Die `Style` Instanz wird dann automatisch auf alle Elemente dieses Typs angewendet. Beachten Sie, dass Unterklassen `TargetType` von nicht automatisch `Style` auf angewendet werden. Weitere Informationen zu *impliziten* Stilen finden Sie unter [implizite Stile](~/xamarin-forms/user-interface/styles/implicit.md).

Beim Erstellen eines [`Style`](xref:Xamarin.Forms.Style)ist die [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschaft immer erforderlich. Das folgende Codebeispiel zeigt einen *expliziten* Stil (beachten `x:Key`Sie den), der in XAML erstellt wurde:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Zum Anwenden eines `Style`muss das Zielobjekt ein [`VisualElement`](xref:Xamarin.Forms.VisualElement) sein, das mit `Style`dem [`TargetType`](xref:Xamarin.Forms.Style.TargetType) Eigenschafts Wert von übereinstimmt, wie im folgenden XAML-Codebeispiel gezeigt:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Formatvorlagen, die weiter unten in der Hierarchie von Inhaltsansichten haben Vorrang vor den definierten höher einrichten. Z. B. eine [ `Style` ](xref:Xamarin.Forms.Style) festlegt [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) zu `Red` in der Anwendung wird von einem Seite-Level-Stil, der festlegt Ebene überschrieben werden `Label.TextColor` zu `Green`. Auf ähnliche Weise wird ein Servicelevel-Seite-Style von einem Steuerelementstil Ebene überschrieben werden. Wenn `Label.TextColor` außerdem direkt für eine Steuerelement Eigenschaft festgelegt wird, hat dies Vorrang vor beliebigen Stilen.

In den Artikeln in diesem Abschnitt wird erläutert, wie *explizite* und *implizite* Stile erstellt und angewendet werden, wie globale Stile erstellt werden, wie Stil Vererbung verwendet wird, wie auf Stiländerungen zur Laufzeit reagiert wird und wie die in enthaltenen integrierten Stile verwendet werden. Xamarin. Forms.

> [!NOTE]
> **Was ist styleId?**
>
> Vor xamarin. Forms 2,2 wurde die [`StyleId`](xref:Xamarin.Forms.Element.StyleId) -Eigenschaft verwendet, um einzelne Elemente in einer Anwendung zur Identifizierung beim Testen der Benutzeroberfläche und in Design Modulen wie "pixate" zu identifizieren. Xamarin. Forms 2,2 hat jedoch die [`AutomationId`](xref:Xamarin.Forms.Element.AutomationId) -Eigenschaft eingeführt, die die [`StyleId`](xref:Xamarin.Forms.Element.StyleId) -Eigenschaft abgelöst hat.

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
