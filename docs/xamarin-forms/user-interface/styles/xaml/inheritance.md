---
title: 'Stil Vererbung in:::no-loc(Xamarin.Forms):::'
description: 'Stile können von anderen Formaten erben, um Duplizierungen zu verringern und die Wiederverwendung zu ermöglichen. In diesem Artikel wird erläutert, wie Sie die Stil Vererbung in einer- :::no-loc(Xamarin.Forms)::: Anwendung ausführen.'
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 9b374987ce7741c82c433b2e35261c3a23ef778f
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996746"
---
# <a name="style-inheritance-in-no-locxamarinforms"></a>Stil Vererbung in:::no-loc(Xamarin.Forms):::

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Stile können von anderen Formaten erben, um Duplizierungen zu verringern und die Wiederverwendung zu ermöglichen._

## <a name="style-inheritance-in-xaml"></a>Stil Vererbung in XAML

Die Stil Vererbung erfolgt durch Festlegen der- [`Style.BasedOn`](xref::::no-loc(Xamarin.Forms):::.Style.BasedOn) Eigenschaft auf eine vorhandene [`Style`](xref::::no-loc(Xamarin.Forms):::.Style) . In XAML wird dies erreicht, indem die- `BasedOn` Eigenschaft auf eine `StaticResource` Markup Erweiterung festgelegt wird, die auf eine zuvor erstellte verweist `Style` . In c# wird dies erreicht, indem die- `BasedOn` Eigenschaft auf eine-Instanz festgelegt wird `Style` .

Stile, die von einem Basistyp erben [`Setter`](xref::::no-loc(Xamarin.Forms):::.Setter) , können Instanzen für neue Eigenschaften einschließen oder Sie verwenden, um Stile aus dem Basisstil zu überschreiben. Außerdem müssen Stile, die von einem Basistyp erben, auf denselben Typ abzielen, oder auf einen Typ, der von dem Typ abgeleitet ist, der auf den Basistyp abzielt. Wenn z. b. ein Basistyp [`View`](xref::::no-loc(Xamarin.Forms):::.View) auf Instanzen abzielt, können Stile, die auf dem Basistyp basieren, auf `View` Instanzen oder Typen abzielen, die von der `View` -Klasse abgeleitet werden, z [`Label`](xref::::no-loc(Xamarin.Forms):::.Label) . b.-und- [`Button`](xref::::no-loc(Xamarin.Forms):::.Button) Instanzen.

Der folgende Code veranschaulicht die *explizite* Stil Vererbung in einer XAML-Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
                <Setter Property="HorizontalOptions"
                        Value="Center" />
                <Setter Property="VerticalOptions"
                        Value="CenterAndExpand" />
            </Style>
            <Style x:Key="labelStyle" TargetType="Label"
                   BasedOn="{StaticResource baseStyle}">
                ...
                <Setter Property="TextColor" Value="Teal" />
            </Style>
            <Style x:Key="buttonStyle" TargetType="Button"
                   BasedOn="{StaticResource baseStyle}">
                <Setter Property="BorderColor" Value="Lime" />
                ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Label Text="These labels"
                   Style="{StaticResource labelStyle}" />
            ...
            <Button Text="So is the button"
                    Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die `baseStyle` Ziel [`View`](xref::::no-loc(Xamarin.Forms):::.View) Instanzen und legt die-Eigenschaft und die-Eigenschaft fest [`HorizontalOptions`](xref::::no-loc(Xamarin.Forms):::.View.HorizontalOptions) [`VerticalOptions`](xref::::no-loc(Xamarin.Forms):::.View.VerticalOptions) . Der `baseStyle` ist nicht direkt auf Steuerelemente festgelegt. Stattdessen `labelStyle` und `buttonStyle` erben Sie, indem Sie zusätzliche bindbare Eigenschaftswerte festlegen. Der `labelStyle` und `buttonStyle` werden dann auf die [`Label`](xref::::no-loc(Xamarin.Forms):::.Label) Instanzen und die [`Button`](xref::::no-loc(Xamarin.Forms):::.Button) Instanz angewendet, indem die entsprechenden Eigenschaften festgelegt werden [`Style`](xref::::no-loc(Xamarin.Forms):::.NavigableElement.Style) . Dies ergibt die in den folgenden Screenshots gezeigte Darstellung:

[![Screenshot der Stil Vererbung](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Ein impliziter Stil kann von einem expliziten Stil abgeleitet werden, ein expliziter Stil kann jedoch nicht von einem impliziten Stil abgeleitet werden.

### <a name="respecting-the-inheritance-chain"></a>Berücksichtigen der Vererbungs Kette

Ein Stil kann nur von Stilen auf derselben Ebene oder höher in der Hierarchie der Ansicht geerbt werden. Dies bedeutet Folgendes:

- Eine Ressource auf Anwendungsebene kann nur von anderen Ressourcen auf Anwendungsebene erben.
- Eine Ressource auf Seitenebene kann von Ressourcen auf Anwendungsebene und anderen Ressourcen auf Seitenebene erben.
- Eine Ressource auf Steuerungsebene kann von Ressourcen auf Anwendungsebene, Ressourcen auf Seitenebene und anderen Ressourcen auf Steuerungsebene erben.

Diese Vererbungs Kette wird im folgenden Codebeispiel veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="baseStyle" TargetType="View">
              ...
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="labelStyle" TargetType="Label" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                    <Style x:Key="buttonStyle" TargetType="Button" BasedOn="{StaticResource baseStyle}">
                      ...
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            ...
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

In diesem Beispiel `labelStyle` sind und `buttonStyle` Ressourcen auf Steuerungsebene, während `baseStyle` eine Ressource auf Seitenebene ist. Obwohl `labelStyle` und `buttonStyle` von Erben `baseStyle` , ist es nicht möglich, `baseStyle` von oder zu erben `labelStyle` `buttonStyle` , da die jeweiligen Standorte in der Ansichts Hierarchie angezeigt werden.

## <a name="style-inheritance-in-c35"></a>Stil Vererbung in C-&#35;

Die entsprechende c#-Seite, bei [`Style`](xref::::no-loc(Xamarin.Forms):::.Style) der-Instanzen direkt den [`Style`](xref::::no-loc(Xamarin.Forms):::.NavigableElement.Style) Eigenschaften der erforderlichen Steuerelemente zugewiesen werden, wird im folgenden Codebeispiel gezeigt:

```csharp
public class StyleInheritancePageCS : ContentPage
{
    public StyleInheritancePageCS ()
    {
        var baseStyle = new Style (typeof(View)) {
            Setters = {
                new Setter {
                    Property = View.HorizontalOptionsProperty, Value = LayoutOptions.Center    },
                ...
            }
        };

        var labelStyle = new Style (typeof(Label)) {
            BasedOn = baseStyle,
            Setters = {
                ...
                new Setter { Property = Label.TextColorProperty, Value = Color.Teal    }
            }
        };

        var buttonStyle = new Style (typeof(Button)) {
            BasedOn = baseStyle,
            Setters = {
                new Setter { Property = Button.BorderColorProperty, Value =    Color.Lime },
                ...
            }
        };
        ...

        Content = new StackLayout {
            Children = {
                new Label { Text = "These labels", Style = labelStyle },
                ...
                new Button { Text = "So is the button", Style = buttonStyle }
            }
        };
    }
}
```

Die `baseStyle` Ziel [`View`](xref::::no-loc(Xamarin.Forms):::.View) Instanzen und legt die-Eigenschaft und die-Eigenschaft fest [`HorizontalOptions`](xref::::no-loc(Xamarin.Forms):::.View.HorizontalOptions) [`VerticalOptions`](xref::::no-loc(Xamarin.Forms):::.View.VerticalOptions) . Der `baseStyle` ist nicht direkt auf Steuerelemente festgelegt. Stattdessen `labelStyle` und `buttonStyle` erben Sie, indem Sie zusätzliche bindbare Eigenschaftswerte festlegen. Der `labelStyle` und `buttonStyle` werden dann auf die [`Label`](xref::::no-loc(Xamarin.Forms):::.Label) Instanzen und die [`Button`](xref::::no-loc(Xamarin.Forms):::.Button) Instanz angewendet, indem die entsprechenden Eigenschaften festgelegt werden [`Style`](xref::::no-loc(Xamarin.Forms):::.NavigableElement.Style) .

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Grundlegende Stile (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Arbeiten mit Stilen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref::::no-loc(Xamarin.Forms):::.ResourceDictionary)
- [style](xref::::no-loc(Xamarin.Forms):::.Style)
- [Trend](xref::::no-loc(Xamarin.Forms):::.Setter)
