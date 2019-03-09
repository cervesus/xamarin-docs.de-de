---
title: Einführung in Xamarin.Forms-Stile
description: Stile ermöglichen die Darstellung der visuellen Elemente angepasst werden. Stile werden für einen bestimmten Typ definiert und enthalten Werte für die Eigenschaften für diesen Typ verfügbar.
ms.prod: xamarin
ms.assetid: 3FF899C0-6CFB-4C1D-837D-9E9E10181967
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 4048ec78d48b810b39d46fbcb7708860c478cce3
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57667203"
---
# <a name="introduction-to-xamarinforms-styles"></a>Einführung in Xamarin.Forms-Stile

_Stile ermöglichen die Darstellung der visuellen Elemente angepasst werden. Stile werden für einen bestimmten Typ definiert und enthalten Werte für die Eigenschaften für diesen Typ verfügbar._

Xamarin.Forms-Anwendungen enthalten häufig mehrere Steuerelemente, die eine identische Darstellung. Z. B. eine Anwendung möglicherweise mehrere [ `Label` ](xref:Xamarin.Forms.Label) Instanzen, in denen die Schriftartoptionen für den gleichen Optionen und das Layout, wie in den folgenden XAML-Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="Styles.NoStylesPage"
    Title="No Styles"
    Icon="xaml.png">
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
        Icon = "csharp.png";
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

Jede [ `Label` ](xref:Xamarin.Forms.Label) Instanz verfügt über identische Eigenschaftswerte für die Steuerung der Darstellung des Texts angezeigt werden, indem die `Label`. Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![](introduction-images/no-styles.png "Bezeichnung Darstellung ohne Stile")](introduction-images/no-styles-large.png#lightbox "Bezeichnung Darstellung ohne Stile")

Die Darstellung der einzelnen Steuerelemente festlegen kann sein, sich wiederholende und fehleranfällig. Stattdessen kann ein Stil erstellt werden, die definiert die Darstellung, und klicken Sie dann auf die erforderlichen Steuerelemente angewendet.

## <a name="create-a-style"></a>Erstellen eines Stils

Die [ `Style` ](xref:Xamarin.Forms.Style) Klasse gruppiert eine Auflistung von Eigenschaftswerten in ein Objekt, das Sie dann auf mehrere Instanzen für visuelles Element angewendet werden kann. Dies hilft beim Reduzieren von repetitivem Markup und eine Darstellung Anwendungen leichter geändert werden kann.

Obwohl Formate in erster Linie für XAML-basierte Anwendungen entwickelt wurden, können sie auch in c# erstellt werden:

- [`Style`](xref:Xamarin.Forms.Style) in XAML erstellte Instanzen werden in der Regel definiert, einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) , zugewiesen ist die [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung eines Steuerelements, Seite oder die [ `Resources` ](xref:Xamarin.Forms.Application.Resources) Auflistung von der Anwendung.
- [`Style`](xref:Xamarin.Forms.Style) Instanzen, die in c# erstellt wurde, werden in der Regel definiert, in der Klasse von der Seite oder in eine Klasse, die Global zugegriffen werden kann.

Die Entscheidung, wo Sie eine [`Style`](xref:Xamarin.Forms.Style)-Klasse definieren, hat Einfluss darauf, wo Sie sie verwenden können:

- [`Style`](xref:Xamarin.Forms.Style) Instanzen, die auf der Steuerelementebene definiert können nur auf das Steuerelement und seinen untergeordneten Elementen angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style) auf Seitenebene definierte Instanzen können nur auf der Seite und seinen untergeordneten Elementen angewendet werden.
- [`Style`](xref:Xamarin.Forms.Style) auf Anwendungsebene definierte Instanzen können in der gesamten Anwendung angewendet werden.

Jede [ `Style` ](xref:Xamarin.Forms.Style) Instanz enthält eine Auflistung einer oder mehreren [ `Setter` ](xref:Xamarin.Forms.Setter) Objekte mit jeweils `Setter` müssen eine [ `Property` ](xref:Xamarin.Forms.Setter.Property) und ein [`Value`](xref:Xamarin.Forms.Setter.Value). Die `Property` ist der Name der bindbare Eigenschaft des Elements, der Stil angewendet wird, und die `Value` ist der Wert, der auf die Eigenschaft angewendet wird.

Jede [ `Style` ](xref:Xamarin.Forms.Style) Instanz möglich *explizite*, oder *implizite*:

- Ein *explizite* [ `Style` ](xref:Xamarin.Forms.Style) -Instanz wird durch die Angabe definiert eine [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) und ein `x:Key` Wert ein, und durch Festlegen des Target-Elements [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaft, um die `x:Key` Verweis. Weitere Informationen zu *explizite* Stilen finden Sie unter [explizite Stile](~/xamarin-forms/user-interface/styles/explicit.md).
- Ein *implizite* [ `Style` ](xref:Xamarin.Forms.Style) -Instanz wird definiert, durch Angeben von nur einem [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType). Die `Style` Instanz dann automatisch angewendet werden, um alle Elemente dieses Typs. Beachten Sie diese Unterklassen von der `TargetType` automatisch müssen nicht die `Style` angewendet. Weitere Informationen zu *implizite* Stilen finden Sie unter [impliziten Stilen](~/xamarin-forms/user-interface/styles/implicit.md).

Beim Erstellen einer [ `Style` ](xref:Xamarin.Forms.Style), [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaft ist immer erforderlich. Das folgende Codebeispiel zeigt eine *explizite* Stil (Beachten Sie die `x:Key`) in XAML erstellt:

```xaml
<Style x:Key="labelStyle" TargetType="Label">
    <Setter Property="HorizontalOptions" Value="Center" />
    <Setter Property="VerticalOptions" Value="CenterAndExpand" />
    <Setter Property="FontSize" Value="Large" />
</Style>
```

Anzuwendende eine `Style`, das Zielobjekt muss ein [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) , entspricht die [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaftswert, der die `Style`, wie in den folgenden XAML-Codebeispiel gezeigt:

```xaml
<Label Text="Demonstrating an explicit style" Style="{StaticResource labelStyle}" />
```

Formatvorlagen, die weiter unten in der Hierarchie von Inhaltsansichten haben Vorrang vor den definierten höher einrichten. Z. B. eine [ `Style` ](xref:Xamarin.Forms.Style) festlegt [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) zu `Red` in der Anwendung wird von einem Seite-Level-Stil, der festlegt Ebene überschrieben werden `Label.TextColor` zu `Green`. Auf ähnliche Weise wird ein Servicelevel-Seite-Style von einem Steuerelementstil Ebene überschrieben werden. Darüber hinaus, wenn `Label.TextColor` direkt auf eine Steuerelementeigenschaft, dies hat Vorrang vor alle Formatvorlagen festgelegt ist.

Die Artikel in diesem Abschnitt veranschaulichen und erläutern, wie zum Erstellen und anwenden *explizite* und *implizite* Stile, wie Sie globale Stile erstellen, formatieren die Vererbung, zum Reagieren auf Änderungen an Formatvorlagen zur Laufzeit und wie Sie mit der integrierten Formatvorlagen in Xamarin.Forms enthalten.

> [!NOTE]
> **Was ist StyleId?**
>
> Vor dem Xamarin.Forms 2.2 die [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) Eigenschaft zum Identifizieren der einzelnen Elemente in einer Anwendung für die Identifikation in UI-Tests und Design-Engines wie z.B. Pixate verwendet wurde. Xamarin.Forms 2.2 führte jedoch die [ `AutomationId` ](xref:Xamarin.Forms.Element.AutomationId) -Eigenschaft, die ersetzt wird, hat die [ `StyleId` ](xref:Xamarin.Forms.Element.StyleId) Eigenschaft.

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
