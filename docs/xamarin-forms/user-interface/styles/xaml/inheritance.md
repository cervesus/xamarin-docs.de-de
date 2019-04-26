---
title: Stilvererbung in Xamarin.Forms
description: Stile können andere Stile Duplizierung zu reduzieren, und aktivieren die Wiederverwendung erben. In diesem Artikel wird erläutert, wie stilvererbung in einer Xamarin.Forms-Anwendung ausgeführt wird.
ms.prod: xamarin
ms.assetid: 67A3A39C-8CC0-446D-8162-FFA73582D3B8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: bef48db93ae76346802b6569080bb1e54e3e51b3
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61394011"
---
# <a name="style-inheritance-in-xamarinforms"></a>Stilvererbung in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Stile können andere Stile Duplizierung zu reduzieren, und aktivieren die Wiederverwendung erben._

## <a name="style-inheritance-in-xaml"></a>Stilvererbung in XAML

Stilvererbung wird ausgeführt, indem die [ `Style.BasedOn` ](xref:Xamarin.Forms.Style.BasedOn) Eigenschaft zu einem vorhandenen [ `Style` ](xref:Xamarin.Forms.Style). In XAML, erfolgt dies durch Festlegen der `BasedOn` Eigenschaft, um eine `StaticResource` Markuperweiterung, die ein zuvor erstelltes verweist `Style`. In C# geschrieben, erfolgt dies durch Festlegen der `BasedOn` Eigenschaft, um eine `Style` Instanz.

Formatvorlagen, die von einer grundlegenden Stil erben zählen [ `Setter` ](xref:Xamarin.Forms.Setter) Instanzen für die neuen Eigenschaften verwenden, um die Stile, die von den grundlegenden Stil zu überschreiben. Formatvorlagen, die von einer grundlegenden Stil erben müssen darüber hinaus verweisen, den gleichen Typ oder ein Typ, der vom Ziel von den grundlegenden Stil Typ abgeleitet ist. Angenommen, einem grundlegenden Stil für [ `View` ](xref:Xamarin.Forms.View) -Instanzen, Stile, die auf den grundlegenden Stil basieren können als Ziel `View` Instanzen oder von abgeleiteten Typen dem `View` Klasse, z. B. [ `Label` ](xref:Xamarin.Forms.Label) und [ `Button` ](xref:Xamarin.Forms.Button) Instanzen.

Der folgende Code veranschaulicht *explizite* formatieren Sie Vererbung in einer XAML-Seite:

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

Die `baseStyle` Ziele [ `View` ](xref:Xamarin.Forms.View) Instanzen aus, und legt sie fest der [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften. Die `baseStyle` nicht direkt für alle Steuerelemente festgelegt ist. Stattdessen `labelStyle` und `buttonStyle` erben, Festlegen von Werten für zusätzliche bindbare Eigenschaft. Die `labelStyle` und `buttonStyle` gelten dann für die [ `Label` ](xref:Xamarin.Forms.Label) Instanzen und [ `Button` ](xref:Xamarin.Forms.Button) Instanz durch Festlegen ihrer [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaften. Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

[![](inheritance-images/style-inheritance.png)](inheritance-images/style-inheritance-large.png#lightbox)

> [!NOTE]
> Ein impliziter Stil von expliziter Stil abgeleitet werden kann, aber ein expliziter Stil kann nicht in ein impliziter Stil abgeleitet werden.

### <a name="respecting-the-inheritance-chain"></a>Unter Beachtung der Vererbungskette

Ein Stil kann nur von Stilen auf derselben Ebene oder höher, erben in der Hierarchie anzeigen. Dies bedeutet Folgendes:

- Eine Ressource auf Ebene kann nur von anderen auf Anwendungsressourcen erben.
- Eine Seite-Ressource auf kann auf Ressourcen der Anwendung und andere Ebene Seitenressourcen erben.
- Eine Steuerelement-Ressource auf kann auf Anwendungsressourcen, Ebene Seitenressourcen und andere Ebene Steuerung von Ressourcen erben.

Diese Vererbungskette wird im folgenden Codebeispiel veranschaulicht:

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

In diesem Beispiel `labelStyle` und `buttonStyle` sind Ressourcen der Ebene, während `baseStyle` wird eine Seite-Ressource auf. Allerdings while `labelStyle` und `buttonStyle` erben `baseStyle`, es ist nicht möglich, dass `baseStyle` das erben `labelStyle` oder `buttonStyle`, da ihre jeweiligen Positionen in der Hierarchie von Inhaltsansichten.

## <a name="style-inheritance-in-c35"></a>Style-Vererbung in C&#35;

Der entsprechende C#-Seite, in denen [ `Style` ](xref:Xamarin.Forms.Style) Instanzen direkt zugewiesen sind die [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaften der Steuerelemente erforderlich, wird im folgenden Codebeispiel wird angezeigt:

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

Die `baseStyle` Ziele [ `View` ](xref:Xamarin.Forms.View) Instanzen aus, und legt sie fest der [ `HorizontalOptions` ](xref:Xamarin.Forms.View.HorizontalOptions) und [ `VerticalOptions` ](xref:Xamarin.Forms.View.VerticalOptions) Eigenschaften. Die `baseStyle` nicht direkt für alle Steuerelemente festgelegt ist. Stattdessen `labelStyle` und `buttonStyle` erben, Festlegen von Werten für zusätzliche bindbare Eigenschaft. Die `labelStyle` und `buttonStyle` gelten dann für die [ `Label` ](xref:Xamarin.Forms.Label) Instanzen und [ `Button` ](xref:Xamarin.Forms.Button) Instanz durch Festlegen ihrer [ `Style` ](xref:Xamarin.Forms.VisualElement.Style) Eigenschaften.

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Einfache Stile (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Stilen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
