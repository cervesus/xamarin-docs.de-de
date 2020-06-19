---
title: Implizite Stile inXamarin.Forms
description: Ein impliziter Stil ist ein Wert, der von allen Steuerelementen desselben TargetType verwendet wird, ohne dass jedes Steuerelement auf den Stil verweisen muss.
ms.prod: xamarin
ms.assetid: 02A75F3B-4389-49D4-A2F4-AFD473A4A161
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 3fb6ea40ced93103ec9cc92fa707f68c674d7826
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84139008"
---
# <a name="implicit-styles-in-xamarinforms"></a>Implizite Stile inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Ein impliziter Stil ist ein Wert, der von allen Steuerelementen desselben TargetType verwendet wird, ohne dass jedes Steuerelement auf den Stil verweisen muss._

## <a name="create-an-implicit-style-in-xaml"></a>Erstellen eines impliziten Stils in XAML

Um einen [`Style`](xref:Xamarin.Forms.Style) auf Seitenebene zu deklarieren, [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) muss der Seite ein hinzugefügt werden, und anschließend kann eine oder mehrere `Style` Deklarationen in den eingefügt werden `ResourceDictionary` . Eine `Style` wird *implizit* durch Angabe eines `x:Key` Attributs erstellt. Der Stil wird dann auf visuelle Elemente angewendet, die exakt mit übereinstimmen `TargetType` , jedoch nicht mit Elementen, die vom- `TargetType` Wert abgeleitet werden.

Das folgende Codebeispiel zeigt einen *impliziten* Stil, der in XAML in einer Seite deklariert `ResourceDictionary` und auf die Instanzen der Seite angewendet wurde [`Entry`](xref:Xamarin.Forms.Entry) :

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

[`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)Definiert einen einzelnen *impliziten* Stil, der auf die Instanzen der Seite angewendet wird [`Entry`](xref:Xamarin.Forms.Entry) . `Style`Wird verwendet, um blauen Text auf einem gelben Hintergrund anzuzeigen, während auch andere Darstellungs Optionen festgelegt werden. Der `Style` wird der der Seite [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ohne Angabe eines `x:Key` Attributs hinzugefügt. Daher wird der `Style` auf alle- `Entry` Instanzen implizit angewendet, wenn Sie mit der- [`TargetType`](xref:Xamarin.Forms.Style.TargetType) Eigenschaft des `Style` exakt übereinstimmen. Der wird jedoch `Style` nicht auf die Instanz angewendet `CustomEntry` , die eine Unterklasse ist `Entry` . Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![Beispiel für implizite Stile](implicit-images/implicit-styles.png)](implicit-images/implicit-styles-large.png#lightbox)

Außerdem überschreibt der vierte [`Entry`](xref:Xamarin.Forms.Entry) die-Eigenschaft [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) und die-Eigenschaft [`TextColor`](xref:Xamarin.Forms.InputView.TextColor) des impliziten Formats auf verschiedene `Color` Werte.

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

In diesem Beispiel wird der *implizite* der-Auflistung [`Style`](xref:Xamarin.Forms.Style) des-Steuer Elements zugewiesen [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) [`StackLayout`](xref:Xamarin.Forms.StackLayout) . Der *implizite* Stil kann dann auf das Steuerelement und seine untergeordneten Elemente angewendet werden.

Weitere Informationen zum Erstellen von Stilen in einer Anwendung [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) finden Sie unter [globale Stile](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-implicit-style-in-c35"></a>Erstellen Sie einen impliziten Stil in C-&#35;

[`Style`](xref:Xamarin.Forms.Style)der Auflistung einer Seite in c# können-Instanzen hinzugefügt werden [`Resources`](xref:Xamarin.Forms.VisualElement.Resources) , indem ein neuer erstellt [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) und dann die-Instanzen hinzugefügt werden `Style` `ResourceDictionary` , wie im folgenden Codebeispiel gezeigt:

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

Der Konstruktor definiert einen einzelnen *impliziten* Stil, der auf die Instanzen der Seite angewendet wird [`Entry`](xref:Xamarin.Forms.Entry) . `Style`Wird verwendet, um blauen Text auf einem gelben Hintergrund anzuzeigen, während auch andere Darstellungs Optionen festgelegt werden. Der `Style` wird der der Seite [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) ohne Angabe einer `key` Zeichenfolge hinzugefügt. Daher wird der `Style` auf alle- `Entry` Instanzen implizit angewendet, wenn Sie mit der- [`TargetType`](xref:Xamarin.Forms.Style.TargetType) Eigenschaft des `Style` exakt übereinstimmen. Der wird jedoch `Style` nicht auf die Instanz angewendet `CustomEntry` , die eine Unterklasse ist `Entry` .

## <a name="apply-a-style-to-derived-types"></a>Einen Stil auf abgeleitete Typen anwenden

Die- [`Style.ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) Eigenschaft ermöglicht das Anwenden eines Stils auf Steuerelemente, die vom Basistyp abgeleitet werden, auf den die- [`TargetType`](xref:Xamarin.Forms.Style.TargetType) Eigenschaft verweist. Wenn Sie diese Eigenschaft auf festlegen, `true` kann ein einzelner Stil auf mehrere Typen abzielen, vorausgesetzt, dass die Typen von dem Basistyp abgeleitet sind, der in der-Eigenschaft angegeben ist `TargetType` .

Das folgende Beispiel zeigt einen impliziten Stil, der die Hintergrundfarbe der- [`Button`](xref:Xamarin.Forms.Button) Instanzen auf rot festlegt:

```xaml
<Style TargetType="Button"
       ApplyToDerivedTypes="True">
    <Setter Property="BackgroundColor"
            Value="Red" />
</Style>
```

Das Platzieren dieses Stils auf Seitenebene [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) führt dazu, dass es auf alle [`Button`](xref:Xamarin.Forms.Button) Instanzen auf der Seite und auch auf alle Steuerelemente angewendet wird, die von abgeleitet werden `Button` . Wenn die Eigenschaft jedoch [`ApplyToDerivedTypes`](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) nicht festgelegt wurde, wird der Stil nur auf Instanzen angewendet `Button` .

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
