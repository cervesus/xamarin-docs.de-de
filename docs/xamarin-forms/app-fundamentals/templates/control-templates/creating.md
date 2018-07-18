---
title: Erstellen eines ControlTemplate-Objekts
description: Steuerelementvorlagen können auf Seiten- oder Anwendungsebene definiert werden. Dieser Artikel veranschaulicht das Erstellen und nutzen Vorlagen für Steuerelemente.
ms.prod: xamarin
ms.assetid: A9AEB052-FBF5-4589-9BD4-6D6F62BED7F1
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/08/2016
ms.openlocfilehash: b83668f6836b1d5d98f67592bf3e2b01e7319edc
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998186"
---
# <a name="creating-a-controltemplate"></a>Erstellen eines ControlTemplate-Objekts

_Steuerelementvorlagen können auf Seiten- oder Anwendungsebene definiert werden. Dieser Artikel veranschaulicht das Erstellen und nutzen Vorlagen für Steuerelemente._

## <a name="creating-a-controltemplate-in-xaml"></a>Erstellen einer ControlTemplate in XAML

Zum Definieren einer [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) auf Anwendungsebene, eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) muss hinzugefügt werden die `App` Klasse. Standardmäßig verwenden alle Xamarin.Forms-Anwendungen, die aus einer Vorlage erstellt die **App** Klasse zum Implementieren der [ `Application` ](xref:Xamarin.Forms.Application) Unterklasse. Deklariert eine `ControlTemplate` auf Anwendungsebene, in der Anwendung `ResourceDictionary` mit XAML, der Standardwert **App** Klasse ersetzt werden muss, mit einer XAML **App** -Klasse und den zugehörigen Code-Behind muss als Im folgenden Codebeispiel gezeigt:

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

Jede [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Instanz wird erstellt, als ein wieder verwendbares Objekt in einem [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary).  Dies wird erreicht, indem jeder Deklaration einen eindeutigen `x:Key` -Attribut, das darüber mit einem beschreibenden Schlüssel in der `ResourceDictionary`.

Im folgenden Codebeispiel wird veranschaulicht, die zugeordnete `App` CodeBehind:

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

Sowie die Einstellung der [ `MainPage` ](xref:Xamarin.Forms.Application.MainPage) -Eigenschaft, Code-Behind muss auch aufrufen, die `InitializeComponent` Methode zum Laden und analysieren die zugehörigen XAML.

Das folgende Codebeispiel zeigt eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Anwenden der `TealTemplate` auf die [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

Die `TealTemplate` zugewiesen wird die [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) -Eigenschaft mithilfe der `StaticResource` Markuperweiterung. Die [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) -Eigenschaftensatz auf eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , die den Inhalt auf anzuzeigende definiert die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Dieser Inhalt angezeigt, auf die [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) innerhalb der `TealTemplate`. Dadurch wird die Darstellung, die in den folgenden Screenshots gezeigt:

![](creating-images/teal-theme.png "Blaugrünen Steuerelementvorlage")

### <a name="re-theming-an-application-at-runtime"></a>RE-Design einer Anwendung zur Laufzeit

Klicken auf die **Ändern des Designs** Schaltfläche führt die `OnButtonClicked` -Methode, die im folgenden Codebeispiel gezeigt wird:

```csharp
void OnButtonClicked (object sender, EventArgs e)
{
  originalTemplate = !originalTemplate;
  contentView.ControlTemplate = (originalTemplate) ? tealTemplate : aquaTemplate;
}
```

Diese Methode ersetzt die aktive [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Instanz mit der alternativen `ControlTemplate` Instanz, was im folgenden Screenshot:

![](creating-images/aqua-theme.png "Aquamarin-Steuerelementvorlage")

> [!NOTE]
> Auf einem `ContentPage`, `Content` Eigenschaft zugewiesen werden kann und die `ControlTemplate` Eigenschaft kann auch festgelegt werden. Wenn dies auftritt, wenn die `ControlTemplate` enthält eine `ContentPresenter` -Instanz, den Inhalt, der zugeordnet der `Content` wird von der Eigenschaft angezeigt der `ContentPresenter` innerhalb der `ControlTemplate`.

### <a name="setting-a-controltemplate-with-a-style"></a>Festlegen einer "ControlTemplate" mit einem Stil

Ein [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) kann auch angewendet werden, über eine [ `Style` ](xref:Xamarin.Forms.Style) Design Möglichkeit weiter erweitern. Dies kann erreicht werden, indem Sie erstellen eine *implizite* oder *explizite* Stil für die Ansicht in eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), und legen die `ControlTemplate` Zieleigenschaft Zeigen Sie in der [ `Style` ](xref:Xamarin.Forms.Style) Instanz. Das folgende Codebeispiel zeigt eine *implizite* Stil, der auf Anwendungsebene hinzugefügt wurde, wird [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary):

```xaml
<Style TargetType="ContentView">
    <Setter Property="ControlTemplate" Value="{StaticResource TealTemplate}" />
</Style>
```

Da die [ `Style` ](xref:Xamarin.Forms.Style) Instanz *implizite*, es gelten für alle `ContentView` Instanzen in der Anwendung. Daher ist es nicht mehr notwendig, legen Sie die [ `ContentView.ControlTemplate` ](xref:Xamarin.Forms.TemplatedView.ControlTemplate) -Eigenschaft, wie im folgenden Codebeispiel wird veranschaulicht:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="SimpleTheme.HomePage">
    <ContentView x:Name="contentView" Padding="0,20,0,0">
      ...
    </ContentView>
</ContentPage>
```

Weitere Informationen zu Stilen finden Sie unter [Stile](~/xamarin-forms/user-interface/styles/index.md).

### <a name="creating-a-controltemplate-at-page-level"></a>Erstellen einer ControlTemplate auf Seitenebene

Zusätzlich zur Erstellung [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Instanzen auf Anwendungsebene, sie können auch erstellt werden auf Seitenebene, wie im folgenden Codebeispiel gezeigt:

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

Beim Hinzufügen einer [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) auf Seitenebene, eine [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) hinzugefügt wird die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage), und klicken Sie dann die `ControlTemplate` Instanzen enthalten sind in der `ResourceDictionary`.

## <a name="creating-a-controltemplate-in-c35"></a>Erstellen einer ControlTemplate in C&#35;

Zum Definieren einer [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) auf der Anwendungsebene eine `class` muss erstellt werden, die darstellt, die `ControlTemplate`. Sollte die Klasse ableiten der [Layout](~/xamarin-forms/user-interface/layouts/index.md) die Vorlage verwendet wird, wie im folgenden Codebeispiel gezeigt:

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

Die `AquaTemplate` Klasse ist identisch mit der `TealTemplate` Klasse, mit dem Unterschied, dass für unterschiedliche Farben verwendet werden die [ `BoxView.Color` ](xref:Xamarin.Forms.BoxView.Color) und [ `Label.TextColor` ](xref:Xamarin.Forms.Label.TextColor) Eigenschaften.

Das folgende Codebeispiel zeigt eine [ `ContentPage` ](xref:Xamarin.Forms.ContentPage) Anwenden der `TealTemplate` auf die [ `ContentView` ](xref:Xamarin.Forms.ContentView):

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

Die [ `ControlTemplate` ](xref:Xamarin.Forms.ControlTemplate) Instanzen werden erstellt, indem Sie den Typ der Klassen, die definieren, die Vorlagen, in der `ControlTemplate` Konstruktor.

Die [ `ContentView.Content` ](xref:Xamarin.Forms.ContentView.Content) -Eigenschaftensatz auf eine [ `StackLayout` ](xref:Xamarin.Forms.StackLayout) , die den Inhalt auf anzuzeigende definiert die [ `ContentPage` ](xref:Xamarin.Forms.ContentPage). Dieser Inhalt angezeigt, auf die [ `ContentPresenter` ](xref:Xamarin.Forms.ContentPresenter) innerhalb der `TealTemplate`. Derselbe Mechanismus beschrieben zuvor wird verwendet, so ändern Sie das Design zur Laufzeit, um die `AquaTheme`.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie das Erstellen und Nutzen von Steuerelementvorlagen wird. Steuerelementvorlagen können auf Seiten- oder Anwendungsebene definiert werden.


## <a name="related-links"></a>Verwandte Links

- [Stile](~/xamarin-forms/user-interface/styles/index.md)
- [Einfaches Thema (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/templates/controltemplates/simpletheme/)
- [ControlTemplate](xref:Xamarin.Forms.ControlTemplate)
- [ContentPresenter](xref:Xamarin.Forms.ContentPresenter)
- [ContentView](xref:Xamarin.Forms.ContentView)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
