---
title: Explizite Stile
description: Ein expliziter Stil ist durch Festlegen von deren Eigenschaften selektiv auf Steuerelemente angewendet wird.
ms.prod: xamarin
ms.assetid: C0DF9F8F-B431-4374-A574-325BC3C41A3B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 9f9e87ae0fd9d609cef56123e9052d85941bda51
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848186"
---
# <a name="explicit-styles"></a>Explizite Stile

_Ein expliziter Stil ist durch Festlegen von deren Eigenschaften selektiv auf Steuerelemente angewendet wird._

## <a name="creating-an-explicit-style-in-xaml"></a>Erstellen eine explizite Formatvorlage in XAML

Deklarieren einer [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) auf Seitenebene, eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) muss hinzugefügt werden, um die Seite, und klicken Sie dann eine oder mehrere `Style` Deklarationen enthalten sein können, der `ResourceDictionary`. Ein `Style` erfolgt *explizite* durch die Vergabe der Deklaration einer `x:Key` -Attribut, das sie einen beschreibenden Schlüssel, in bietet der `ResourceDictionary`. *Explizite* Stile müssen dann auf bestimmte visuelle Elemente angewendet werden, durch Festlegen von deren [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften.

Das folgende Codebeispiel zeigt *explizite* Formatvorlagen, die in XAML deklariert ist, auf einer Seite `ResourceDictionary` und auf die Seite angewendeten [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
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

Die [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiert drei *explizite* Formatvorlagen, die auf der Seite angewendet werden [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen. Jede `Style` dient zum Anzeigen von Text in einer anderen Farbe beim auch die Schriftart Größe und horizontalen und vertikalen Layoutoptionen festlegen. Jede `Style` wird angewendet auf einen anderen `Label` durch Festlegen seiner [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften mithilfe der `StaticResource` Markuperweiterung. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](explicit-images/explicit-styles.png "Beispiel für explizite Stile")](explicit-images/explicit-styles-large.png#lightbox "explizite Formatvorlagen-Beispiel")

Darüber hinaus die endgültige [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) verfügt über eine [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) angewendet, aber überschreibt auch die [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) -Eigenschaft auf einen anderen `Color`Wert.

### <a name="creating-an-explicit-style-at-the-control-level"></a>Erstellen expliziten Stil für die Ebene an das Steuerelement

Zusätzlich zur Erstellung *explizite* Formatvorlagen auf Seitenebene, sie können auch erstellt werden auf der Steuerelementebene, wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ExplicitStylesPage" Title="Explicit" Icon="xaml.png">
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

In diesem Beispiel wird die *explizite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen zugewiesen sind die [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Auflistung von der [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) Steuerelement. Die Stile können dann auf das Steuerelement und seine untergeordneten Elemente angewendet werden.

Weitere Informationen zum Erstellen von Formaten in einer Anwendungsverzeichnis [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), finden Sie unter [globalen Formatvorlagen](~/xamarin-forms/user-interface/styles/application.md).

## <a name="creating-an-explicit-style-in-c35"></a>Erstellen eine explizite Formatvorlage in C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen können auf einer Seite hinzugefügt werden [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Auflistung in c# durch Erstellen eines neuen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), und klicken Sie dann durch Hinzufügen der `Style` auf Instanzen der `ResourceDictionary`, entsprechend der folgende Codebeispiel:

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

Der Konstruktor definiert drei *explizite* Formatvorlagen, die auf der Seite angewendet werden [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen. Jede *explizite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) hinzugefügt wird der [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) mithilfe der [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) -Methode angeben einer `key` Zeichenfolge zum Verweisen auf die `Style` Instanz. Jede `Style` wird angewendet auf einen anderen `Label` durch Festlegen von deren [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften.

Es ist jedoch keinen Vorteil einer [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) hier. Stattdessen [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen direkt zugewiesen werden können die [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften der erforderlichen visueller Elemente und die `ResourceDictionary` entfernt werden kann, wie im folgenden dargestellt Codebeispiel:

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

Der Konstruktor definiert drei *explizite* Formatvorlagen, die auf der Seite angewendet werden [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen. Jede `Style` dient zum Anzeigen von Text in einer anderen Farbe beim auch die Schriftart Größe und horizontalen und vertikalen Layoutoptionen festlegen. Jede `Style` wird angewendet auf einen anderen `Label` durch Festlegen seiner [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften. Darüber hinaus die endgültige `Label` verfügt über eine `Style` angewendet, aber überschreibt auch die `TextColor` Eigenschaft zu einer anderen `Color` Wert.

## <a name="summary"></a>Zusammenfassung

Ein [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) erfolgt *explizite* durch die Vergabe der Deklaration einer `x:Key` Attribut, und klicken Sie dann selektiv anwenden auf Steuerelemente durch Festlegen ihrer [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften.



## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Grundlegende Formate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Formatvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Stil](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter-Methode](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
