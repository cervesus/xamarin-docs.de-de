---
title: Explizite Stile in Xamarin.Forms
description: Expliziter Stil ist eine, die selektiv auf Steuerelemente angewendet wird, indem die Stileigenschaften. In diesem Artikel wird erläutert, wie explizite Stile in einer Xamarin.Forms-Anwendung genutzt wird.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7a149a41a6e50d3b18da166d9c7cb61e36f2d0e7
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65970458"
---
# <a name="explicit-styles-in-xamarinforms"></a>Explizite Stile in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Expliziter Stil ist eine, die selektiv auf Steuerelemente angewendet wird, indem die Stileigenschaften._

## <a name="create-an-explicit-style-in-xaml"></a>Erstellen Sie einen expliziten Stil in XAML

Deklariert eine [ `Style` ](xref:Xamarin.Forms.Style) auf Seitenebene, eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) muss hinzugefügt werden, um die Seite, und klicken Sie dann eine oder mehrere `Style` Deklarationen enthalten sein können, der `ResourceDictionary`. Ein `Style` erfolgt *explizite* erteilen Sie der Deklaration einer `x:Key` -Attribut, das sie einen beschreibenden Schlüssel, in erhalten der `ResourceDictionary`. *Explizite* Stile müssen dann an bestimmte visuelle Elemente angewendet werden, durch Festlegen ihrer [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften.

Das folgende Codebeispiel zeigt *explizite* Stile, die in XAML in einer Seite deklarierten `ResourceDictionary` , und klicken Sie auf der Seite angewendet [ `Label` ](xref:Xamarin.Forms.Label) Instanzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="labelRedStyle" TargetType="Label">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
                <Setter Property="FontSize" Value="Large" />
                <Setter Property="TextColor" Value="Red" />
            </Style>
            <Style x:Key="labelGreenStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Green" />
            </Style>
            <Style x:Key="labelBlueStyle" TargetType="Label">
                ...
                <Setter Property="TextColor" Value="Blue" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelRedStyle}" />
            <Label Text="are demonstrating"
                   Style="{StaticResource labelGreenStyle}" />
            <Label Text="explicit styles,"
                   Style="{StaticResource labelBlueStyle}" />
            <Label Text="and an explicit style override"
                   Style="{StaticResource labelBlueStyle}"
                   TextColor="Teal" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definiert drei *explizite* Stile, die auf der Seite angewendet werden [ `Label` ](xref:Xamarin.Forms.Label) Instanzen. Jede `Style` dient zum Anzeigen von Text in eine andere Farbe, beim Festlegen von Schriftart auch Optionen für Größe und horizontalen und vertikalen Layout. Jede `Style` wird angewendet auf einen anderen `Label` durch Festlegen seiner [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften mithilfe der `StaticResource` Markuperweiterung. Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

[![](explicit-images/explicit-styles.png "Beispiel für explizite Stile")](explicit-images/explicit-styles-large.png#lightbox "explizite Stile-Beispiel")

Darüber hinaus die endgültige [ `Label` ](xref:Xamarin.Forms.Label) verfügt über eine [ `Style` ](xref:Xamarin.Forms.Style) angewendet wird, aber überschreibt auch die [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) Eigenschaft, um einen anderen `Color`Wert.

### <a name="create-an-explicit-style-at-the-control-level"></a>Erstellen Sie einen expliziten Stil auf der Steuerelementebene

Zusätzlich zur Erstellung *explizite* Formatvorlagen auf Seitenebene, sie können auch erstellt werden auf der Steuerelementebene wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelRedStyle" TargetType="Label">
                      ...
                    </Style>
                    ...
                </ResourceDictionary>
            </StackLayout.Resources>
            <Label Text="These labels" Style="{StaticResource labelRedStyle}" />
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

In diesem Beispiel die *explizite* [ `Style` ](xref:Xamarin.Forms.Style) Instanzen werden zugewiesen, um die [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung von der [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) Steuerelement. Die Stile können Sie dann auf das Steuerelement und seine untergeordneten Elemente angewendet werden.

Informationen zum Erstellen von Stilen in einer Anwendung [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), finden Sie unter [globale Stile](~/xamarin-forms/user-interface/styles/application.md).

## <a name="create-an-explicit-style-in-c35"></a>Erstellen Sie einen expliziten Stil in C&#35;

[`Style`](xref:Xamarin.Forms.Style) Instanzen können einer Seite hinzugefügt werden [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung in C# durch Erstellen eines neuen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und klicken Sie dann durch Hinzufügen der `Style` auf Instanzen der `ResourceDictionary`Siehe die folgende Codebeispiel:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Red    }
            }
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Green }
            }
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Blue }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("labelRedStyle", labelRedStyle);
        Resources.Add ("labelGreenStyle", labelGreenStyle);
        Resources.Add ("labelBlueStyle", labelBlueStyle);
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels",
                            Style = (Style)Resources ["labelRedStyle"] },
                new Label { Text = "are demonstrating",
                            Style = (Style)Resources ["labelGreenStyle"] },
                new Label { Text = "explicit styles,",
                            Style = (Style)Resources ["labelBlueStyle"] },
                new Label {    Text = "and an explicit style override",
                            Style = (Style)Resources ["labelBlueStyle"], TextColor = Color.Teal }
            }
        };
    }
}
```

Der Konstruktor definiert drei *explizite* Stile, die auf der Seite angewendet werden [ `Label` ](xref:Xamarin.Forms.Label) Instanzen. Jedes *explizite* [ `Style` ](xref:Xamarin.Forms.Style) hinzugefügt wird die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) mithilfe der [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) -Methode angeben einer `key` Zeichenfolge zum Verweisen auf die `Style` Instanz. Jede `Style` wird angewendet auf einen anderen `Label` durch Festlegen ihrer [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften.

Es ist jedoch kein Vorteil der Verwendung einer [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) hier. Stattdessen [ `Style` ](xref:Xamarin.Forms.Style) Instanzen direkt zugewiesen werden können die [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften der erforderlichen visuellen Elemente, und die `ResourceDictionary` entfernt werden kann, wie im folgenden dargestellt Codebeispiel:

```csharp
public class ExplicitStylesPageCS : ContentPage
{
    public ExplicitStylesPageCS ()
    {
        var labelRedStyle = new Style (typeof(Label)) {
            ...
        };
        var labelGreenStyle = new Style (typeof(Label)) {
            ...
        };
        var labelBlueStyle = new Style (typeof(Label)) {
            ...
        };
        ...
        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelRedStyle },
                new Label { Text = "are demonstrating", Style = labelGreenStyle },
                new Label { Text = "explicit styles,", Style = labelBlueStyle },
                new Label { Text = "and an explicit style override", Style = labelBlueStyle,
                            TextColor = Color.Teal }
            }
        };
    }
}
```

Der Konstruktor definiert drei *explizite* Stile, die auf der Seite angewendet werden [ `Label` ](xref:Xamarin.Forms.Label) Instanzen. Jede `Style` dient zum Anzeigen von Text in eine andere Farbe, beim Festlegen von Schriftart auch Optionen für Größe und horizontalen und vertikalen Layout. Jede `Style` wird angewendet auf einen anderen `Label` durch Festlegen seiner [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften. Darüber hinaus die endgültige `Label` verfügt über eine `Style` angewendet wird, aber überschreibt auch die `TextColor` Eigenschaft auf einen anderen `Color` Wert.

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Einfache Stile (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Stilen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
