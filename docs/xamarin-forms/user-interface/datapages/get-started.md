---
title: Erste Schritte mit DataPages
ms.topic: article
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 3b5346b6d556f6437d9c9fc17897180fd0b41b1e
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/15/2018
---
# <a name="getting-started-with-datapages"></a>Erste Schritte mit DataPages

![](~/media/shared/preview.png "Diese API ist derzeit als Vorschau verfügbar")

> [!IMPORTANT]
> DataPages erfordert eine [Xamarin.Forms Design](~/xamarin-forms/user-interface/themes/index.md) Verweis auf das Rendern.


Zunächst erstellen eine einfache Datenlaufwerk-Seite in der Vorschau DataPages, führen Sie die folgenden Schritte aus. Diese Demo verwendet ein hartcodierten-Stil ("Ereignisse") in der Vorschau Builds funktioniert nur mit bestimmten JSON-Format im Code.

[![](get-started-images/demo-sml.png "DataPages-Beispielanwendung")](get-started-images/demo.png#lightbox "DataPages-Beispielanwendung")

## <a name="1-add-nuget-packages"></a>1. Hinzufügen von NuGet-Paketen

Fügen Sie diese NuGet-Pakete auf Ihre Xamarin.Forms PCL und Anwendungsprojekte hinzu:

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Eine Implementierung Design Nuget (z. b. Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Design "Verweis hinzufügen

In der **App.xaml** Datei, eine benutzerdefinierte `xmlns:mytheme` für das Design und vergewissern Sie sich das Design der Anwendung Ressourcenverzeichnis zusammengeführt wird:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

**Wichtig:** befolgen Sie die Schritte zum auch [Design-Assemblys (siehe unten) laden](#loadtheme) durch Hinzufügen von einigen Standardcode für den iOS- `AppDelegate` und Android `MainActivity`. Dies wird in zukünftigen Preview-Version verbessert werden.


## <a name="3-add-a-xaml-page"></a>3. Fügen Sie eine XAML-Seite

Fügen Sie eine neue XAML-Seite für die Anwendung Xamarin.Forms und *ändern Sie die Basisklasse* aus `ContentPage` auf `Xamarin.Forms.Pages.ListDataPage`. Dies muss erfolgen, in der C#- und der XAML-Code:

**C# file**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**Verwendung von XAML-Datei**

Zusätzlich zum Ändern der Root-Element, um `<p:ListDataPage>` für den benutzerdefinierten Namespace `xmlns:p` muss hinzugefügt werden:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Application-Unterklasse.**

Ändern der `App` Klassenkonstruktor, damit die `MainPage` auf festgelegt ist ein `NavigationPage` mit der neuen `SessionDataPage`. Eine Seite "Navigation" *müssen* verwendet werden.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Fügen Sie die Datenquelle hinzu

Löschen der `Content` Element und ersetzen es durch eine `p:ListDataPage.DataSource` auf die Seite mit Daten aufzufüllen. Im Beispiel unten ein remote Json wird von einer URL-Datendatei geladen.

**Hinweis:** die Vorschau *erfordert* ein `StyleClass` Attribut Renderinghinweise für die Datenquelle zu übermitteln. Die `StyleClass="Events"` bezieht sich auf ein Layout, die in der Vorschau vordefiniert und Formatvorlagen enthält *hartcodiert* entsprechend die JSON-Datenquelle verwendet wird.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**JSON-Daten**

Ein Beispiel für die JSON-Daten aus der [Demo Quelle](http://demo3143189.mockable.io/sessions) wird unten gezeigt:

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. Führen Sie!

Die oben genannten Schritte sollten dazu führen, dass eine Datenseite arbeiten:

[![](get-started-images/demo-sml.png "DataPages-Beispielanwendung")](get-started-images/demo.png#lightbox "DataPages-Beispielanwendung")

Dies funktioniert, da die vorgefertigten Stil **"Ereignisse"** in das helle Design NuGet-Paket vorhanden und verfügt über definierte Formatvorlagen, die mit die Datenquelle (z. b. "title", "image", "presenter").

Die "Ereignisse" `StyleClass` wird erstellt, um die Anzeige der `ListDataPage` Steuerelement mit einem benutzerdefinierten `CardView` steuern, d. h. im definierten Xamarin.Forms.Pages. Die `CardView` Steuerelement verfügt über drei Eigenschaften: `ImageSource`, `Text`, und `Detail`. Das Design ist hartcodiert der Datasource drei Felder (aus der JSON-Datei) auf diese Eigenschaften für die Anzeige zu binden.

## <a name="5-customize"></a>5. Anpassen

Geerbte Stil kann eine Vorlage angeben und Verwenden von datenquellenbindungen überschrieben werden. Der folgende XAML-Code deklariert eine benutzerdefinierte Vorlage für jede Zeile mit dem neuen `ListItemControl` und `{p:DataSourceBinding}` in enthalten ist die Syntax der **Xamarin.Forms.Pages** Nuget:

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

Durch die Bereitstellung einer `DataTemplate` dieser Code überschreibt die `StyleClass` und verwendet stattdessen auf das Standardlayout für eine `ListItemControl`.

[![](get-started-images/custom-sml.png "DataPages-Beispielanwendung")](get-started-images/custom.png#lightbox "DataPages-Beispielanwendung")

Entwickler, die C#-XAML können Daten erstellen lieber Datenquelle Bindungen zu (Denken Sie daran, eine `using Xamarin.Forms.Pages;` Anweisung):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Es ist ein wenig mehr Arbeit Designs von Grund auf neu erstellen (finden Sie unter der [Designs Handbuch](~/xamarin-forms/user-interface/themes/index.md)) aber künftigen Vorschauversionen werden dies einfacher.


## <a name="troubleshooting"></a>Problembehandlung

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Datei oder Assembly 'Xamarin.Forms.Theme.Light' oder eine ihrer Abhängigkeiten konnte nicht geladen werden

In der Vorschauversion Designs zur Laufzeit laden möglicherweise nicht. Fügen Sie Code unten in der relevanten Projekte, um diesen Fehler zu beheben.

**iOS**

In der **AppDelegate.cs** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

In der **MainActivity.cs** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>Verwandte Links

- [DataPagesDemo-Beispiel](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
