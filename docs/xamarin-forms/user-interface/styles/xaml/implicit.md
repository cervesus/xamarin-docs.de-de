---
title: Implizite Stile in Xamarin.Forms
description: Ein impliziter Stil ist, die durch alle Steuerelemente des gleichen TargetType, ohne dass jedes Steuerelement auf den Stil verwendet wird.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 277be51c242521f52e9b1e162226ae8137e7b133
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995513"
---
# <a name="implicit-styles-in-xamarinforms"></a>Implizite Stile in Xamarin.Forms

_Ein impliziter Stil ist, die durch alle Steuerelemente des gleichen TargetType, ohne dass jedes Steuerelement auf den Stil verwendet wird._

## <a name="creating-an-implicit-style-in-xaml"></a>Erstellen einen impliziten Stil in XAML

Deklariert eine [ `Style` ](xref:Xamarin.Forms.Style) auf Seitenebene, eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) muss hinzugefügt werden, um die Seite, und klicken Sie dann eine oder mehrere `Style` Deklarationen enthalten sein können, der `ResourceDictionary`. Ein `Style` erfolgt *implizite* nicht angegeben ein `x:Key` Attribut. Styl dann angewendet werden, um visuelle Elemente, die entsprechen der `TargetType` genau, jedoch nicht auf Elemente, die abgeleitet sind die `TargetType` Wert.

Das folgende Codebeispiel zeigt eine *implizite* Stil, die in XAML in einer Seite deklarierten `ResourceDictionary`, und klicken Sie auf der Seite angewendet [ `Entry` ](xref:Xamarin.Forms.Entry) Instanzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style TargetType="Entry">
                <Setter Property="HorizontalOptions" Value="Fill" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BackgroundColor" Value="Yellow" />
                <Setter Property="FontAttributes" Value="Italic" />
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Entry Text="These entries" />
            <Entry Text="are demonstrating" />
            <Entry Text="implicit styles," />
            <Entry Text="and an implicit style override" BackgroundColor="Lime" TextColor="Red" />
            <local:CustomEntry Text="Subclassed Entry is not receiving the style" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definiert ein einzelnes *implizite* Stil, die auf der Seite [ `Entry` ](xref:Xamarin.Forms.Entry) Instanzen. Die `Style` wird verwendet, um den blauen Text auf ein gelber Hintergrund angezeigt wird, beim Festlegen der auch andere Optionen für die Darstellung. Die `Style` hinzugefügt wird, klicken Sie auf der Seite [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) ohne Angabe einer `x:Key` Attribut. Aus diesem Grund der `Style` gilt für alle der `Entry` Instanzen implizit, wie sie entsprechen den [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaft der `Style` genau. Allerdings die `Style` gilt nicht für die `CustomEntry` -Instanz, die als eine Unterklasse definiert ist `Entry`. Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

[![](implicit-images/implicit-styles.png "Implizite Stile Beispiel")](implicit-images/implicit-styles-large.png#lightbox "impliziten Stilen-Beispiel")

Darüber hinaus die vierte [ `Entry` ](xref:Xamarin.Forms.Entry) überschreibt die [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) und [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) Eigenschaften des impliziten Formats, verschiedene `Color`Werte.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Erstellen einen impliziten Stil auf das Steuerelement auf

Zusätzlich zur Erstellung *implizite* Formatvorlagen auf Seitenebene, sie können auch erstellt werden auf der Steuerelementebene wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style TargetType="Entry">
                        <Setter Property="HorizontalOptions" Value="Fill" />
                        ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Entry Text="These entries" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

In diesem Beispiel die *implizite* [ `Style` ](xref:Xamarin.Forms.Style) zugewiesen wird die [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung von der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout)Steuerelement. Die *implizite* Stil klicken Sie dann auf das Steuerelement und seine untergeordneten Elemente angewendet werden kann.

Informationen zum Erstellen von Stilen in einer Anwendung [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), finden Sie unter [globale Stile](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Erstellen einen impliziten Stil in C&#35;

[`Style`](xref:Xamarin.Forms.Style) Instanzen können einer Seite hinzugefügt werden [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung in c# durch Erstellen eines neuen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und klicken Sie dann durch Hinzufügen der `Style` auf Instanzen der `ResourceDictionary`Siehe die folgende Codebeispiel:

```csharp
public class ImplicitStylesPageCS : ContentPage
{
    public ImplicitStylesPageCS ()
    {
        var entryStyle = new Style (typeof(Entry)) {
            Setters = {
                ...
                new Setter { Property = Entry.TextColorProperty, Value = Color.Blue }
            }
        };

        ...
        Resources = new ResourceDictionary ();
        Resources.Add (entryStyle);

        Content = new StackLayout {
            Children = {
                new Entry { Text = "These entries" },
                new Entry { Text = "are demonstrating" },
                new Entry { Text = "implicit styles," },
                new Entry { Text = "and an implicit style override", BackgroundColor = Color.Lime, TextColor = Color.Red },
                new CustomEntry  { Text = "Subclassed Entry is not receiving the style" }
            }
        };
    }
}
```

Der Konstruktor definiert ein einzelnes *implizite* Stil, die auf der Seite [ `Entry` ](xref:Xamarin.Forms.Entry) Instanzen. Die `Style` wird verwendet, um den blauen Text auf ein gelber Hintergrund angezeigt wird, beim Festlegen der auch andere Optionen für die Darstellung. Die `Style` hinzugefügt wird, klicken Sie auf der Seite [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) ohne Angabe einer `key` Zeichenfolge. Aus diesem Grund der `Style` gilt für alle der `Entry` Instanzen implizit, wie sie entsprechen den [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaft der `Style` genau. Allerdings die `Style` gilt nicht für die `CustomEntry` -Instanz, die als eine Unterklasse definiert ist `Entry`.

## <a name="summary"></a>Zusammenfassung

Ein *implizite* Stil ist eine, mit dem alle visuellen Elemente des gleichen [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType), ohne dass jedes Steuerelement auf die Formatvorlage verweisen. Ein `Style` erfolgt *implizite* nicht angegeben ein `x:Key` Attribut. Stattdessen die `x:Key` Attribut wird automatisch der Wert des der [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) Eigenschaft.



## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Einfache Stile (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Stilen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
