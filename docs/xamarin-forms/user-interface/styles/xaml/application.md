---
title: Globale Stile in Xamarin.Forms
description: Stile können verfügbar Global gemacht werden durch das Ressourcenverzeichnis der Anwendung hinzugefügt. Dadurch wird die um Duplizierung von Formatvorlagen für Seiten oder Steuerelemente zu vermeiden.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: 7b13a192f883ea667977f4d9ae3eea41d8c65e24
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971171"
---
# <a name="global-styles-in-xamarinforms"></a>Globale Stile in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Stile können verfügbar Global gemacht werden durch das Ressourcenverzeichnis der Anwendung hinzugefügt. Dadurch wird die um Duplizierung von Formatvorlagen für Seiten oder Steuerelemente zu vermeiden._

## <a name="create-a-global-style-in-xaml"></a>Erstellen Sie einen globalen Stil in XAML

Standardmäßig verwenden alle Xamarin.Forms-Anwendungen, die aus einer Vorlage erstellt werden, die Klasse **App**. Damit wird die Unterklasse [`Application`](xref:Xamarin.Forms.Application) implementiert. Deklariert eine [ `Style` ](xref:Xamarin.Forms.Style) auf Anwendungsebene, in der Anwendung [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) mit XAML, der Standardwert **App** Klasse mit einer XAML ersetzt werden muss **App** -Klasse und zugeordnete Code-Behind. Weitere Informationen finden Sie unter [arbeiten mit App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).

Das folgende Codebeispiel zeigt eine [ `Style` ](xref:Xamarin.Forms.Style) auf Anwendungsebene deklariert:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Dies [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) definiert ein einzelnes *explizite* Stil `buttonStyle`, dem wird verwendet, um die Darstellung der festlegen [ `Button` ](xref:Xamarin.Forms.Button) Instanzen. Globale Stile kann jedoch *explizite* oder *implizite*.

Das folgende Codebeispiel zeigt ein XAML-Seite anwenden der `buttonStyle` auf der Seite [ `Button` ](xref:Xamarin.Forms.Button) Instanzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

[![](application-images/application-styles-1.png "Globale Stile Beispiel")](application-images/application-styles-1-large.png#lightbox "globale Stile-Beispiel")

Informationen zum Erstellen von Stilen einer Seite [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), finden Sie unter [explizite Stile](~/xamarin-forms/user-interface/styles/explicit.md) und [impliziten Stilen](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="override-styles"></a>Überschreiben von Formatvorlagen

Formatvorlagen, die weiter unten in der Hierarchie von Inhaltsansichten haben Vorrang vor den definierten höher einrichten. Z. B. eine [ `Style` ](xref:Xamarin.Forms.Style) festlegt [ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor) zu `Red` in der Anwendung wird von einem Seite-Level-Stil, der festlegt Ebene überschrieben werden `Button.TextColor` zu `Green`. Auf ähnliche Weise wird ein Servicelevel-Seite-Style von einem Steuerelementstil Ebene überschrieben werden. Darüber hinaus, wenn `Button.TextColor` direkt auf einer Steuerelementeigenschaft ist dies Vorrang vor alle Formatvorlagen festgelegt ist. Diese Priorität wird im folgenden Codebeispiel veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Die ursprüngliche `buttonStyle`, auf Anwendungsebene definiert ist, wird überschrieben, indem die `buttonStyle` Instanz auf Seitenebene definiert. Darüber hinaus wird die Formatvorlage der Seite durch die Steuerungsebene überschrieben `buttonStyle`. Aus diesem Grund die [ `Button` ](xref:Xamarin.Forms.Button) Instanzen werden mit blauem Text angezeigt, wie in den folgenden Screenshots gezeigt:

[![](application-images/application-styles-2.png "Überschreiben die Stile Beispiel")](application-images/application-styles-2-large.png#lightbox "überschreiben die Stile-Beispiel")

## <a name="create-a-global-style-in-c35"></a>Erstellen Sie einen globalen Stil in C&#35;

[`Style`](xref:Xamarin.Forms.Style) Instanzen der Anwendung hinzugefügt werden können [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) Auflistung in C# durch Erstellen eines neuen [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und klicken Sie dann durch Hinzufügen der `Style` auf Instanzen der `ResourceDictionary`, als Im folgenden Codebeispiel gezeigt:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Der Konstruktor definiert ein einzelnes *explizite* Stil für die Anwendung auf [ `Button` ](xref:Xamarin.Forms.Button) Instanzen in der gesamten Anwendung. *Explizite* [ `Style` ](xref:Xamarin.Forms.Style) Instanzen werden hinzugefügt, um die [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) mithilfe der [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) Methode, geben Sie einen `key`Zeichenfolge zum Verweisen auf die `Style` Instanz. Die `Style` Instanz klicken Sie dann auf alle Steuerelemente des richtigen Typs in der Anwendung angewendet werden kann. Globale Stile kann jedoch *explizite* oder *implizite*.

Das folgende Codebeispiel veranschaulicht ein C#-Seite anwenden der `buttonStyle` auf der Seite [ `Button` ](xref:Xamarin.Forms.Button) Instanzen:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

Die `buttonStyle` gilt, an die [ `Button` ](xref:Xamarin.Forms.Button) Instanzen durch Festlegen ihrer [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) Eigenschaften und kontrolliert das Erscheinungsbild von der `Button` Instanzen.

## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Einfache Stile (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Stilen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Stil](xref:Xamarin.Forms.Style)
- [Set-Methode](xref:Xamarin.Forms.Setter)
