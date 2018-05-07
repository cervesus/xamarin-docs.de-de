---
title: Die globalen Formatvorlagen
description: Stile können verfügbar global erfolgen durch die Anwendung Ressourcenverzeichnis hinzugefügt. Dies hilft, um Formatvorlagen auf Seiten oder Steuerelemente Rechnung zu tragen.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: a505720e5fef8fe9e9ef82d03e53370210772f45
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="global-styles"></a>Die globalen Formatvorlagen

_Stile können verfügbar global erfolgen durch die Anwendung Ressourcenverzeichnis hinzugefügt. Dies hilft, um Formatvorlagen auf Seiten oder Steuerelemente Rechnung zu tragen._

## <a name="creating-a-global-style-in-xaml"></a>Erstellen einer globalen Formatvorlage in XAML

Standardmäßig verwenden alle Xamarin.Forms-Anwendungen, die von einer Vorlage erstellt die **App** Klasse zum Implementieren der [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) Unterklasse. Deklarieren einer [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) auf Anwendungsebene in der Anwendungsverzeichnis [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) mit XAML, der Standardwert **App** Klasse durch einen XAML-Code ersetzt werden muss. **App** Klasse und zugehörigen Code-Behind. Weitere Informationen finden Sie unter [arbeiten mit der App-Klasse](~/xamarin-forms/app-fundamentals/application-class.md).

Das folgende Codebeispiel zeigt eine [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) auf Anwendungsebene deklariert:

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

Dies [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) definiert ein einzelnes *explizite* Stil `buttonStyle`, dem wird verwendet, um die Darstellung des festlegen [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanzen. Globale Stile kann jedoch *explizite* oder *implizite*.

Das folgende Codebeispiel zeigt eine XAML-Seite anwenden der `buttonStyle` auf der Seite [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanzen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

[![](application-images/application-styles-1.png "Die globalen Formatvorlagen Beispiel")](application-images/application-styles-1-large.png#lightbox "globalen Formatvorlagen-Beispiel")

Weitere Informationen zum Erstellen von Formaten auf einer Seite [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), finden Sie unter [explizite Stile](~/xamarin-forms/user-interface/styles/explicit.md) und [impliziten Stilen](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="overriding-styles"></a>Überschreiben von Stilen

Stile in der Ansichtshierarchie haben Vorrang vor den definierten höher einrichten. Z. B. eine [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) festlegt [ `Button.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Button.TextColor/) auf `Red` Anwendungs-Ebene überschrieben werden wird, indem eine Seite Formatvorlage, der festlegt `Button.TextColor` auf `Green`. Auf ähnliche Weise wird eine Seite Formatvorlage der Formatvorlage für ein Steuerelement überschrieben werden. Darüber hinaus, wenn `Button.TextColor` direkt auf einer Steuerelementeigenschaft ist dies Vorrang vor alle Formatvorlagen festgelegt ist. Diese Priorität wird im folgenden Codebeispiel veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" Icon="xaml.png">
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

Die ursprüngliche `buttonStyle`, auf Anwendungsebene definiert ist, wird überschrieben, indem die `buttonStyle` Instanz auf Seitenebene definiert. Darüber hinaus wird die Formatvorlage der Seite durch die Steuerungsebene überschrieben `buttonStyle`. Aus diesem Grund die [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanzen sind mit blauem Text angezeigt, wie in den folgenden Screenshots dargestellt:

[![](application-images/application-styles-2.png "Überschreiben von Stilen Beispiel")](application-images/application-styles-2-large.png#lightbox "überschreiben Formatvorlagen-Beispiel")

## <a name="creating-a-global-style-in-c35"></a>Erstellen einer globalen Formatvorlage in C&#35;

[`Style`](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen der Anwendungsverzeichnis hinzugefügt werden können [ `Resources` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Resources/) Auflistung in c# durch Erstellen eines neuen [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), und klicken Sie dann durch Hinzufügen der `Style` um Instanzen der `ResourceDictionary`, als Im folgenden Codebeispiel gezeigt:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,   Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Der Konstruktor definiert ein einzelnes *explizite* Stil für die Anwendung auf [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanzen in der gesamten Anwendung. *Explizite* [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanzen werden hinzugefügt, um die [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) mithilfe der [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ResourceDictionary.Add/p/System.String/System.Object/) Methode, dabei werden ein `key`Zeichenfolge zum Verweisen auf die `Style` Instanz. Die `Style` Instanz klicken Sie dann auf alle Steuerelemente des richtigen Typs in der Anwendung angewendet werden kann. Globale Stile kann jedoch *explizite* oder *implizite*.

Das folgende Codebeispiel zeigt eine C#-Seite anwenden der `buttonStyle` auf der Seite [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanzen:

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

Die `buttonStyle` wird angewendet, um die [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) Instanzen durch Festlegen ihrer [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) Eigenschaften, und steuert die Darstellung einer der `Button` Instanzen.

## <a name="summary"></a>Zusammenfassung

Stile verfügbar gemacht werden global von der Anwendungsverzeichnis hinzugefügt [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Dies hilft, um Formatvorlagen auf Seiten oder Steuerelemente Rechnung zu tragen.



## <a name="related-links"></a>Verwandte Links

- [XAML-Markuperweiterungen](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Grundlegende Formate (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
- [Arbeiten mit Formatvorlagen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithStyles/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
- [Style](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/)
- [Setter](https://developer.xamarin.com/api/type/Xamarin.Forms.Setter/)
