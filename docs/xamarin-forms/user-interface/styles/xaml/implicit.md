---
title: Implizite Stile in Xamarin.Forms
description: Ein impliziter Stil ist, die durch alle Steuerelemente des gleichen TargetType, ohne dass jedes Steuerelement auf den Stil verwendet wird.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: cdbfaafdac8f965adaf4b840b568154e40ef7e10
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228186"
---
# <a name="implicit-styles-in-xamarinforms"></a>Implizite Stile in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Ein impliziter Stil ist, die durch alle Steuerelemente des gleichen TargetType, ohne dass jedes Steuerelement auf den Stil verwendet wird._

## <a name="create-an-implicit-style-in-xaml"></a>Erstellen eines impliziten Stils in XAML

Deklariert eine [ `Style` ](xref:Xamarin.Forms.Style) auf Seitenebene, eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) muss hinzugefügt werden, um die Seite, und klicken Sie dann eine oder mehrere `Style` Deklarationen enthalten sein können, der `ResourceDictionary`. Ein `Style` erfolgt *implizite* nicht angegeben ein `x:Key` Attribut. Styl dann angewendet werden, um visuelle Elemente, die entsprechen der `TargetType` genau, jedoch nicht auf Elemente, die abgeleitet sind die `TargetType` Wert.

Das folgende Codebeispiel zeigt eine *implizite* Stil, die in XAML in einer Seite deklarierten `ResourceDictionary`, und klicken Sie auf der Seite angewendet [ `Entry` ](xref:Xamarin.Forms.Entry) Instanzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
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

[![Beispiel für implizite Stile](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

Darüber hinaus die vierte [ `Entry` ](xref:Xamarin.Forms.Entry) überschreibt die [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) und [ `TextColor` ](xref:Xamarin.Forms.Entry.TextColor) Eigenschaften des impliziten Formats, verschiedene `Color`Werte.

### <a name="create-an-implicit-style-at-the-control-level"></a>Erstellen eines impliziten Stils auf der Steuerelement Ebene

Zusätzlich zur Erstellung *implizite* Formatvorlagen auf Seitenebene, sie können auch erstellt werden auf der Steuerelementebene wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" xmlns:local="clr-namespace:Styles;assembly=Styles" x:Class="Styles.ImplicitStylesPage" Title="Implicit" IconImageSource="xaml.png">
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

## <a name="create-an-implicit-style-in-c35"></a>Erstellen eines impliziten Stils in C&#35;

[`Style`](xref:Xamarin.Forms.Style) Instanzen können einer Seite hinzugefügt werden [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung in C# durch Erstellen eines neuen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und klicken Sie dann durch Hinzufügen der `Style` auf Instanzen der `ResourceDictionary`Siehe die folgende Codebeispiel:

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

## <a name="apply-a-style-to-derived-types"></a>Einen Stil auf abgeleitete Typen anwenden

Die [`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) -Eigenschaft ermöglicht das Anwenden eines Stils auf Steuerelemente, die vom Basistyp abgeleitet werden, auf [`TargetType`](xref:Xamarin.Forms.Style.TargetType) den die-Eigenschaft verweist. Wenn Sie diese Eigenschaft auf `true` festlegen, kann ein einzelner Stil auf mehrere Typen abzielen, vorausgesetzt, dass die Typen von dem Basistyp abgeleitet sind, der in der `TargetType` -Eigenschaft angegeben ist.

Das folgende Beispiel zeigt einen impliziten Stil, der die Hintergrundfarbe der [`Button`](xref:Xamarin.Forms.Button) -Instanzen auf rot festlegt:

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

Das Platzieren dieses Stils auf Seitenebene [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) führt dazu, dass es auf alle Instanzen auf der Seite und auch auf alle [`Button`](xref:Xamarin.Forms.Button) Steuerelemente angewendet wird, die von `Button`abgeleitet werden. Wenn die [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) Eigenschaft jedoch nicht festgelegt wurde, wird der Stil nur auf `Button` Instanzen angewendet.

Der entsprechende C#-Code ist:

```csharp
var buttonStyle = new Style(typeof(Button))
{
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.Red
        }
    }
};

Resources = new ResourceDictionary { buttonStyle };
```

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Einfache Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Arbeiten mit Stilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
