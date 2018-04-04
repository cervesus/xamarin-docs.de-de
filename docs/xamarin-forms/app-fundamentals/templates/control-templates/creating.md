---
title: Erstellen eines ControlTemplate-Objekts
description: Steuerelementvorlagen können auf Seitenebene oder Anwendungsebene definiert werden. In diesem Artikel wird das Erstellen und Nutzen von Steuerelementvorlagen veranschaulicht.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: e8a0695969609a4b0bbeb38896adae9a7c16ed07
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="creating-a-controltemplate"></a>Erstellen eines ControlTemplate-Objekts

_Steuerelementvorlagen können auf Seitenebene oder Anwendungsebene definiert werden. In diesem Artikel wird das Erstellen und Nutzen von Steuerelementvorlagen veranschaulicht._

## <a name="creating-a-controltemplate-in-xaml"></a>Erstellen einer ControlTemplate in XAML

Definieren einer [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) auf der Anwendungsebene eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) muss hinzugefügt werden die `App` Klasse. Standardmäßig verwenden alle Xamarin.Forms-Anwendungen, die von einer Vorlage erstellt die **App** Klasse zum Implementieren der [ `Application` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Application/) Unterklasse. Deklarieren einer `ControlTemplate` auf Anwendungsebene in der Anwendungsverzeichnis `ResourceDictionary` mit XAML, der Standardwert **App** Klasse durch einen XAML-Code ersetzt werden muss **App** Klasse und zugehörigen Code-Behind, als Im folgenden Codebeispiel gezeigt:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.App">
    <Application.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                <Grid>
                    ...
                    <BoxView ... />
                    <Label Text="Control Template Demo App"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                    <ContentPresenter ... />
                    <BoxView Color="Teal" ... />
                    <Label Text="(c) Xamarin 2016"
                           TextColor="White"
                           VerticalOptions="Center" ... />
                </Grid>
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Jede [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Instanz wird erstellt, als ein wiederverwendbares Objekt in einem [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/).  Dies erfolgt durch die Vergabe jede Deklaration eine eindeutige `x:Key` -Attribut, das mit einem beschreibenden Schlüssel in bietet die `ResourceDictionary`.

Das folgende Codebeispiel zeigt die zugeordnete `App` Code-Behind:

```csharp
public partial class App : Application
{
  public App ()
  {
    InitializeComponent ();
    MainPage = new HomePage ();
  }
}
```

Sowie die Einstellung der [ `MainPage` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Application.MainPage/) -Eigenschaft, die Code-Behind muss auch aufrufen, die `InitializeComponent` Methode zum Laden und analysieren den zugehörigen XAML-Code.

Das folgende Codebeispiel zeigt eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Anwenden der `TealTemplate` auf die [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0"
                 ControlTemplate="{StaticResource TealTemplate}">
        <StackLayout VerticalOptions="CenterAndExpand">
            <Label Text="Welcome to the app!" HorizontalOptions="Center" />
            <Button Text="Change Theme" Clicked="OnButtonClicked" />
        </StackLayout>
    </ContentView>
</ContentPage>
```

Die `TealTemplate` zugewiesen ist die [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) Eigenschaft mithilfe der `StaticResource` Markuperweiterung. Die [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) -Eigenschaftensatz auf eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , definiert den Inhalt auf die [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Dieser Inhalt wird angezeigt, durch die [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) enthalten sind, der `TealTemplate`. Daraus ergibt sich die Darstellung in den folgenden Screenshots dargestellt:

![](creating-images/teal-theme.png "Blaugrünen Steuerelementvorlage")

### <a name="re-theming-an-application-at-runtime"></a>RE-Designumgebung einer Anwendung zur Laufzeit

Klicken auf die **Design ändern** Schaltfläche führt die `OnButtonClicked` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Diese Methode ersetzt die aktive [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Instanz mit der alternativen `ControlTemplate` Instanz, was im folgenden Screenshot:

![](creating-images/aqua-theme.png "Aqua-Steuerelementvorlage")

> [!NOTE]
> Auf einem `ContentPage`, die `Content` Eigenschaft zugewiesen werden kann und die `ControlTemplate` Eigenschaft kann auch festgelegt werden. Wenn dies geschieht, wenn die `ControlTemplate` enthält eine `ContentPresenter` Instanz, den Inhalt zugewiesen der `Content` Eigenschaft vorgelegte der `ContentPresenter` innerhalb der `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Festlegen einer ControlTemplate mit einer Formatvorlage

Ein [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) kann auch angewendet werden, über eine [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) zum Design Möglichkeit noch weiter erweitern. Dies kann erreicht werden, indem Sie erstellen eine *implizite* oder *explizite* Stil für die Ziel-Ansicht in eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/), und die Einstellung der `ControlTemplate` Eigenschaft des Ziels Zeigen Sie in der [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanz. Das folgende Codebeispiel zeigt eine *implizite* Format, das auf der Ebene der hinzugefügten [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Da die [ `Style` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Style/) Instanz *implizite*, es gelten für alle `ContentView` Instanzen in der Anwendung. Daher ist es nicht mehr notwendig, legen Sie die [ `ContentView.ControlTemplate` ](https://developer.xamarin.com/api/property/Xamarin.Forms.TemplatedView.ControlTemplate/) Eigenschaft, wie im folgenden Codebeispiel wird veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Erstellen einer ControlTemplate auf Seitenebene

Zusätzlich zur Erstellung [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Instanzen auf Anwendungsebene, sie können auch erstellt werden auf Seitenebene, wie im folgenden Codebeispiel gezeigt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentPage.Resources>
        <ResourceDictionary>
            <ControlTemplate x:Key="TealTemplate">
                ...
            </ControlTemplate>
            <ControlTemplate x:Key="AquaTemplate">
                ...
            </ControlTemplate>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentView ... ControlTemplate="{StaticResource TealTemplate}">
        ...
    </ContentView>
</ContentPage>
```

Beim Hinzufügen einer [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) auf Seitenebene, eine [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) hinzugefügt wird die [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/), und klicken Sie dann die `ControlTemplate` Instanzen enthalten sind in der `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Erstellen einer ControlTemplate in C&#35;

Definieren einer [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) auf der Anwendungsebene eine `class` muss erstellt werden, die darstellt, die `ControlTemplate`. Die Klasse muss abgeleitet werden, aus der [Layout](~/xamarin-forms/user-interface/layouts/index.md) für die Vorlage verwendet wird, wie im folgenden Codebeispiel gezeigt:

```csharp
class TealTemplate : Grid
{
  public TealTemplate ()
  {
    ...
    var contentPresenter = new ContentPresenter ();
    Children.Add (contentPresenter, 0, 1);
    Grid.SetColumnSpan (contentPresenter, 2);
    ...
  }
}

class AquaTemplate : Grid
{
  ...
}
```

Die `AquaTemplate` Klasse ist identisch mit der `TealTemplate` Klasse, mit dem Unterschied, dass verschiedene Farben für verwendet werden die [ `BoxView.Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) und [ `Label.TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) Eigenschaften.

Das folgende Codebeispiel zeigt eine [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/) Anwenden der `TealTemplate` auf die [ `ContentView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/):

```csharp
public class HomePageCS : ContentPage
{
  ...
  ControlTemplate tealTemplate = new ControlTemplate (typeof(TealTemplate));
  ControlTemplate aquaTemplate = new ControlTemplate (typeof(AquaTemplate));

  public HomePageCS ()
  {
    var button = new Button { Text = "Change Theme" };
    var contentView = new ContentView {
      Padding = new Thickness (0, 20, 0, 0),
      Content = new StackLayout {
        VerticalOptions = LayoutOptions.CenterAndExpand,
        Children = {
          new Label { Text = "Welcome to the app!", HorizontalOptions = LayoutOptions.Center },
          button
        }
      },
      ControlTemplate = tealTemplate
    };
    ...
    Content = contentView;
  }
}
```

Die [ `ControlTemplate` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/) Instanzen werden erstellt, indem der Typ der Klassen, die in den Steuerelementvorlagen definieren die `ControlTemplate` Konstruktor.

Die [ `ContentView.Content` ](https://developer.xamarin.com/api/property/Xamarin.Forms.ContentView.Content/) -Eigenschaftensatz auf eine [ `StackLayout` ](https://developer.xamarin.com/api/type/Xamarin.Forms.StackLayout/) , definiert den Inhalt auf die [ `ContentPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPage/). Dieser Inhalt wird angezeigt, durch die [ `ContentPresenter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/) enthalten sind, der `TealTemplate`. Derselbe Mechanismus beschriebenen zuvor wird verwendet, um das Design zur Laufzeit ändern der `AquaTheme`.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie erstellen und Nutzen von Steuerelementvorlagen. Steuerelementvorlagen können auf Seitenebene oder Anwendungsebene definiert werden.


## <a name="related-links"></a>Verwandte Links

- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [Design "Simple" (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](https://developer.xamarin.com/api/type/Xamarin.Forms.ControlTemplate/)
- [ContentPresenter](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentPresenter/)
- [ContentView](https://developer.xamarin.com/api/type/Xamarin.Forms.ContentView/)
- [ResourceDictionary](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/)
