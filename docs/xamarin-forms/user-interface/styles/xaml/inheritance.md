---
title: Stil-Vererbung in Xamarin.Forms
description: Stile können von anderen Formatvorlagen Duplizierung zu reduzieren, und aktivieren die Wiederverwendung erben. In diesem Artikel wird erläutert, wie Stil Vererbung in einer Xamarin.Forms-Anwendung ausgeführt wird.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: aff47769fad065e03de4c62af1be1d67b903eb0a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245093"
---
# <a name="style-inheritance-in-xamarinforms"></a>Stil-Vererbung in Xamarin.Forms

_Stile können von anderen Formatvorlagen Duplizierung zu reduzieren, und aktivieren die Wiederverwendung erben._

## <a name="style-inheritance-in-xaml"></a>Stil-Vererbung in XAML

Stil-Vererbung wird ausgeführt, indem die [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) Eigenschaft zu einem vorhandenen [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/). In XAML, dies erfolgt durch Festlegen der `BasedOn` Eigenschaft, um eine `StaticResource` Markuperweiterung, die ein zuvor erstelltes verweist auf `Style`. In c# ist dies erfolgt durch Festlegen der `BasedOn` Eigenschaft, um eine `Style` Instanz.

Formatvorlagen, die von einer Basis-Formatvorlage erben zählen [ `Setter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/) Instanzen für neue Eigenschaften oder verwenden, um Formatvorlagen aus dem Basis-Stil zu überschreiben. Darüber hinaus müssen Formatvorlagen, die von einer Basis-Formatvorlage erben Ziel verwenden denselben Typ oder ein Typ, der den Typ, der das Ziel des grundlegenden Format abgeleitet. Z. B. wenn ein Basis-Formatvorlage [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Instanzen, Stile, die auf der Basis-Formatvorlage basieren paketaktualisierungen `View` Instanzen oder Typen, die Ableitung der `View` Klasse, z. B. [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) und [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanzen.

Der folgende Code zeigt *explizite* formatieren Vererbung in einer XAML-Seite:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
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

Die `baseStyle` Ziele [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Instanzen, und legt die [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaften. Die `baseStyle` nicht direkt für alle Steuerelemente festgelegt ist. Stattdessen `labelStyle` und `buttonStyle` erben, zusätzliche bindbare Eigenschaftswerte festlegen. Die `labelStyle` und `buttonStyle` gelten dann für die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen und [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanz durch Festlegen ihrer [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Eine implizite Formatvorlage aus einem expliziten Stil abgeleitet werden, aber ein expliziter Stil kann nicht in einem impliziten Stil abgeleitet werden.

### <a name="respecting-the-inheritance-chain"></a>Die Vererbungskette ressourcenbezogene

Ein Format kann nur von Stilen auf derselben Ebene oder höher, erben in der Hierarchie anzeigen. Dies bedeutet Folgendes:

- Eine Anwendungsressource Ebene kann nur von anderen Ebene Anwendungsressourcen erben.
- Ebene Anwendungsressourcen und andere Ebene Page-Ressourcen kann eine Ebene Seitenressource erben.
- Eine Steuerelement-Level-Ressource, die von Ebene Anwendungsressourcen, Ebene Page-Ressourcen und andere Ressourcen der Steuerelement-Ebenen erben kann.

Diese Vererbungskette wird in das folgende Codebeispiel veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.StyleInheritancePage" Title="Inheritance" Icon="xaml.png">
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

In diesem Beispiel `labelStyle` und `buttonStyle` Ebene Ressourcen sind zwar `baseStyle` ist eine Seite-Level-Ressource. Allerdings while `labelStyle` und `buttonStyle` Vererben `baseStyle`, es ist nicht möglich, dass `baseStyle` zu vererben `labelStyle` oder `buttonStyle`, da ihre jeweiligen Positionen in der Hierarchie anzeigen.

## <a name="style-inheritance-in-c35"></a>Stil-Vererbung in C&#35;

Der entsprechende C#-Seite, in dem [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen wird direkt zugewiesen der [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften der Steuerelemente erforderlich, wird im folgenden Codebeispiel gezeigt:

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

Die `baseStyle` Ziele [ `View` ](https://developer.xamarin.com/api/type/Xamarin.Forms.View/) Instanzen, und legt die [ `HorizontalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.HorizontalOptions/) und [ `VerticalOptions` ](https://developer.xamarin.com/api/property/Xamarin.Forms.View.VerticalOptions/) Eigenschaften. Die `baseStyle` nicht direkt für alle Steuerelemente festgelegt ist. Stattdessen `labelStyle` und `buttonStyle` erben, zusätzliche bindbare Eigenschaftswerte festlegen. Die `labelStyle` und `buttonStyle` gelten dann für die [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) Instanzen und [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanz durch Festlegen ihrer [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften.

## <a name="summary"></a>Zusammenfassung

Stile können von anderen Formatvorlagen Duplizierung zu reduzieren, und aktivieren die Wiederverwendung erben. Stil-Vererbung wird ausgeführt, indem die [ `Style.BasedOn` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.BasedOn/) Eigenschaft zu einem vorhandenen [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/).


## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Grundlegende Formate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Formatvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Stil](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter-Methode](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
