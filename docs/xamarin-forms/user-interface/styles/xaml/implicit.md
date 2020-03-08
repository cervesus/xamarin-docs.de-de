---
title: Implizite Stile in Xamarin.Forms
description: Ein impliziter Stil ist, die durch alle Steuerelemente des gleichen TargetType, ohne dass jedes Steuerelement auf den Stil verwendet wird.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: d58ba81596cccf470b7246514d71f35968599880
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78918133"
---
# <a name="implicit-styles-in-xamarinforms"></a>Implizite Stile in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Ein impliziter Stil ist ein Wert, der von allen Steuerelementen desselben TargetType verwendet wird, ohne dass jedes Steuerelement auf den Stil verweisen muss._

## <a name="create-an-implicit-style-in-xaml"></a>Erstellen eines impliziten Stils in XAML

Um eine [`Style`](xref:Xamarin.Forms.Style) auf Seitenebene zu deklarieren, muss der Seite ein [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) hinzugefügt werden, und anschließend kann mindestens eine `Style` Deklaration in die `ResourceDictionary`eingeschlossen werden. Eine `Style` wird *implizit* durch Angabe eines `x:Key` Attributs gemacht. Der Stil wird dann auf visuelle Elemente angewendet, die mit dem `TargetType` genau übereinstimmen, jedoch nicht mit Elementen, die vom `TargetType` Wert abgeleitet werden.

Das folgende Codebeispiel zeigt einen *impliziten* Stil, der in XAML in der `ResourceDictionary`einer Seite deklariert und auf die [`Entry`](xref:Xamarin.Forms.Entry) Instanzen der Seite angewendet wurde:

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

Der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) definiert einen einzelnen *impliziten* Stil, der auf die [`Entry`](xref:Xamarin.Forms.Entry) Instanzen der Seite angewendet wird. Der `Style` wird verwendet, um blauen Text auf einem gelben Hintergrund anzuzeigen, während auch andere Darstellungs Optionen festgelegt werden. Der `Style` wird der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) der Seite hinzugefügt, ohne ein `x:Key` Attribut anzugeben. Aus diesem Grund wird die `Style` implizit auf alle `Entry` Instanzen angewendet, da Sie mit der [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschaft der `Style` exakt übereinstimmen. Der `Style` wird jedoch nicht auf die `CustomEntry` Instanz angewendet, die eine untergeordnete `Entry`ist. Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[Beispiel für ![implizite Stile](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

Außerdem überschreibt der vierte [`Entry`](xref:Xamarin.Forms.Entry) die [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) und [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) Eigenschaften des impliziten Stils auf andere `Color` Werte.

### <a name="create-an-implicit-style-at-the-control-level"></a>Erstellen eines impliziten Stils auf der Steuerelement Ebene

Neben dem Erstellen *impliziter* Stile auf Seitenebene können diese auch auf der Steuerelement Ebene erstellt werden, wie im folgenden Codebeispiel gezeigt:

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

In diesem Beispiel wird die *implizite* [`Style`](xref:Xamarin.Forms.Style) der [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) -Auflistung des [`StackLayout`](xref:Xamarin.Forms.StackLayout) -Steuer Elements zugewiesen. Der *implizite* Stil kann dann auf das Steuerelement und seine untergeordneten Elemente angewendet werden.

Weitere Informationen zum Erstellen von Stilen in der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)einer Anwendung finden Sie unter [globale Stile](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-implicit-style-in-c35"></a>Erstellen eines impliziten Stils in C&#35;

[`Style`](xref:Xamarin.Forms.Style) Instanzen können der [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) Auflistung einer Seite in C# hinzugefügt werden, indem ein neuer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)erstellt und dann die `Style` Instanzen dem `ResourceDictionary`hinzugefügt werden, wie im folgenden Codebeispiel gezeigt:

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

Der-Konstruktor definiert einen einzelnen *impliziten* Stil, der auf die [`Entry`](xref:Xamarin.Forms.Entry) Instanzen der Seite angewendet wird. Der `Style` wird verwendet, um blauen Text auf einem gelben Hintergrund anzuzeigen, während auch andere Darstellungs Optionen festgelegt werden. Der `Style` wird der [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) der Seite hinzugefügt, ohne dass eine `key` Zeichenfolge angegeben wird. Aus diesem Grund wird die `Style` implizit auf alle `Entry` Instanzen angewendet, da Sie mit der [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschaft der `Style` exakt übereinstimmen. Der `Style` wird jedoch nicht auf die `CustomEntry` Instanz angewendet, die eine untergeordnete `Entry`ist.

## <a name="apply-a-style-to-derived-types"></a>Einen Stil auf abgeleitete Typen anwenden

Die [`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) -Eigenschaft ermöglicht das Anwenden eines Stils auf Steuerelemente, die vom Basistyp abgeleitet werden, auf den die [`TargetType`](xref:Xamarin.Forms.Style.TargetType) -Eigenschaft verweist. Wenn Sie diese Eigenschaft auf `true` festlegen, kann ein einzelner Stil auf mehrere Typen abzielen, vorausgesetzt, dass die Typen von dem Basistyp abgeleitet werden, der in der `TargetType`-Eigenschaft angegeben ist.

Im folgenden Beispiel wird ein impliziter Stil gezeigt, der die Hintergrundfarbe der [`Button`](xref:Xamarin.Forms.Button) -Instanzen auf rot festlegt:

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

Das Platzieren dieses Formats in einer [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) auf Seitenebene führt dazu, dass es auf alle [`Button`](xref:Xamarin.Forms.Button) Instanzen auf der Seite angewendet wird, und auch auf alle Steuerelemente, die von `Button`abgeleitet werden. Wenn die [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) -Eigenschaft jedoch nicht festgelegt wurde, wird der Stil nur auf `Button`-Instanzen angewendet.

Der entsprechende C#-Code lautet:

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
- [Grundlegende Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Arbeiten mit Stilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [style](xref:Xamarin.Forms.Style)
- [Trend](xref:Xamarin.Forms.Setter)
