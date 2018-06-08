---
title: Impliziten Stilen
description: Ein impliziter Stil ist eine, die von allen Steuerelementen von der gleichen TargetType verwendet wird, ohne dass jedes Steuerelement auf den Stil zu verweisen.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 08f1edf807e0f76b2ca1ced023e7725b24122831
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848221"
---
# <a name="implicit-styles"></a>Impliziten Stilen

_Ein impliziter Stil ist eine, die von allen Steuerelementen von der gleichen TargetType verwendet wird, ohne dass jedes Steuerelement auf den Stil zu verweisen._

## <a name="creating-an-implicit-style-in-xaml"></a>Erstellen einen impliziten Stil in XAML

Deklarieren einer [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) auf Seitenebene, eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) muss hinzugefügt werden, um die Seite, und klicken Sie dann eine oder mehrere `Style` Deklarationen enthalten sein können, der `ResourceDictionary`. Ein `Style` erfolgt *implizite* durch Angabe einer `x:Key` Attribut. Das Format dann angewendet werden, um visuelle Elemente, die mit übereinstimmen der `TargetType` genau, aber nicht für Elemente, die abgeleitet sind die `TargetType` Wert.

Das folgende Codebeispiel zeigt eine *implizite* Stil auf einer Seite in XAML deklariert `ResourceDictionary`, und klicken Sie auf der Seite angewendet [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Instanzen:

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

Die [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiert ein einzelnes *implizite* Stil, die auf der Seite angewendete [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Instanzen. Die `Style` wird verwendet, um auf einen gelben Hintergrund blauen Text angezeigt, beim Festlegen von auch andere Optionen für die Darstellung. Die `Style` wird hinzugefügt, um der Seite [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ohne Angabe einer `x:Key` Attribut. Aus diesem Grund die `Style` gilt für alle der `Entry` implizit Instanzen, wie sie entsprechen der [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) Eigenschaft von der `Style` genau. Allerdings die `Style` gilt nicht für die `CustomEntry` -Instanz, die ein untergeordnetes ist `Entry`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](implicit-images/implicit-styles.png "Beispiel für impliziten Stilen")](implicit-images/implicit-styles-large.png#lightbox "impliziten Stilen-Beispiel")

Darüber hinaus die vierte [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) überschreibt die [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) und [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Entry.TextColor/) Eigenschaften des impliziten Formats, verschiedene `Color`Werte.

### <a name="creating-an-implicit-style-at-the-control-level"></a>Erstellen eines impliziten Stils für die Ebene an das Steuerelement

Zusätzlich zur Erstellung *implizite* Formatvorlagen auf Seitenebene, sie können auch erstellt werden auf der Steuerelementebene, wie im folgenden Codebeispiel gezeigt:

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

In diesem Beispiel wird die *implizite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) zugewiesen ist die [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Auflistung von der [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/)Steuerelement. Die *implizite* Stil kann dann auf das Steuerelement und seine untergeordneten Elemente angewendet werden.

Weitere Informationen zum Erstellen von Formaten in einer Anwendungsverzeichnis [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), finden Sie unter [globalen Formatvorlagen](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-implicit-style-in-c35"></a>Erstellen einen impliziten Stil in C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen können auf einer Seite hinzugefügt werden [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Auflistung in c# durch Erstellen eines neuen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), und klicken Sie dann durch Hinzufügen der `Style` auf Instanzen der `ResourceDictionary`, entsprechend der folgende Codebeispiel:

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

Der Konstruktor definiert ein einzelnes *implizite* Stil, die auf der Seite angewendete [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) Instanzen. Die `Style` wird verwendet, um auf einen gelben Hintergrund blauen Text angezeigt, beim Festlegen von auch andere Optionen für die Darstellung. Die `Style` wird hinzugefügt, um der Seite [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) ohne Angabe einer `key` Zeichenfolge. Aus diesem Grund die `Style` gilt für alle der `Entry` implizit Instanzen, wie sie entsprechen der [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) Eigenschaft von der `Style` genau. Allerdings die `Style` gilt nicht für die `CustomEntry` -Instanz, die ein untergeordnetes ist `Entry`.

## <a name="summary"></a>Zusammenfassung

Ein *implizite* Stil ist eine, die durch alle visuellen Elemente des gleichen verwendet wird [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), ohne dass jedes Steuerelement auf den Stil zu verweisen. Ein `Style` erfolgt *implizite* durch Angabe einer `x:Key` Attribut. Stattdessen die `x:Key` Attribut wird automatisch zum Wert von der [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/) Eigenschaft.



## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Grundlegende Formate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Formatvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Stil](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter-Methode](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
