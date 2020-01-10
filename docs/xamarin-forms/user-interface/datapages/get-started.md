---
title: Erste Schritte mit DataPages
description: In diesem Artikel wird erläutert, wie für den Einstieg in die Erstellung einer einfachen datengesteuerten-Webseite mithilfe von Xamarin.Forms DataPages wird.
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 653d2420a9101203412b91a93cc7b6f681e2f5f2
ms.sourcegitcommit: 4691b48f14b166afcec69d1350b769ff5bf8c9f6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/08/2020
ms.locfileid: "75728303"
---
# <a name="getting-started-with-datapages"></a>Erste Schritte mit DataPages

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> Für DataPages ist ein xamarin. Forms-Design Verweis zum Rendering erforderlich. Dies umfasst das Installieren des [xamarin. Forms. Theme. Base](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) -nuget-Pakets in Ihrem Projekt, gefolgt von den nuget-Paketen [xamarin. Forms. Theme. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) oder [xamarin. Forms. Theme. Dark](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) .

Zunächst erstellen eine einfache datengesteuerte-Seite mithilfe der Vorschau DataPages führen Sie die folgenden Schritte aus. Diese Demo verwendet ein hartcodierten-Stil ("Ereignisse") in der Preview-Builds funktioniert nur mit bestimmten JSON-Format im Code.

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. Hinzufügen von nuget-Paketen

Fügen Sie die folgenden nuget-Pakete zu xamarin. Forms-.NET Standard Bibliothek-und Anwendungsprojekten hinzu:

- Xamarin.Forms.Pages
- Xamarin.Forms.Theme.Base
- Eine nuget-Design Implementierung (z. b. Xamarin. Forms. Theme. Light)

## <a name="2-add-theme-reference"></a>2. Design Verweis hinzufügen

In der **"App.xaml"** hinzufügen ein benutzerdefinierten `xmlns:mytheme` für das Design und stellen Sie sicher, das Design mit Ressourcenverzeichnis der Anwendung zusammengeführt wird:

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

> [!IMPORTANT]
> Sie sollten auch die Schritte zum Laden von Design-Assemblys [(unten)](#loadtheme) ausführen, indem Sie dem IOS-`AppDelegate` und Android-`MainActivity`Code hinzufügen. Dies wird in einer kommenden Vorschau-Version verbessert werden.

## <a name="3-add-a-xaml-page"></a>3. Hinzufügen einer XAML-Seite

Fügen Sie eine neue XAML-Seite, um die Xamarin.Forms-Anwendung und *ändern Sie die Basisklasse* aus `ContentPage` zu `Xamarin.Forms.Pages.ListDataPage`. Dies hat in der C#- und die XAML durchgeführt werden:

**C#-Datei**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML-Datei**

Zusätzlich zum Ändern der Stammelement `<p:ListDataPage>` für den benutzerdefinierten Namespace `xmlns:p` muss auch hinzugefügt werden:

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

Ändern der `App` Klassenkonstruktor, damit die `MainPage` nastaven NA hodnotu eine `NavigationPage` mit der neuen `SessionDataPage`. Eine Navigationsseite *müssen* verwendet werden.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Fügen Sie die Datenquelle hinzu.

Löschen der `Content` Element, und Ersetzen Sie ihn mit einer `p:ListDataPage.DataSource` auf die Seite mit Daten aufzufüllen. Im Beispiel unten eine remote-Json ist-Datendatei aus einer URL geladen wird.

> [!NOTE]
> Die Vorschau *erfordert* ein `StyleClass` Attribut, um Renderinghinweise für die Datenquelle bereitzustellen. Die `StyleClass="Events"` bezieht sich auf einem Layout, das ist in der Vorschau vordefiniert und enthält die Stile *hartcodiert* entsprechend die JSON-Datenquelle verwendet wird.

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

Ein Beispiel der JSON-Daten aus der [Demo Quelle](http://demo3143189.mockable.io/sessions) ist unten dargestellt:

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

## <a name="4-run"></a>4. führen Sie aus.

Die oben genannten Schritte sollten dazu führen, dass eine Datenseite arbeiten:

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

Dies funktioniert, weil der vorgefertigte Stil **"Ereignisse"** im Design "lighttheme" im Design "Light" vorhanden ist und Stile definiert sind, die mit der Datenquelle (z. b. "Title", "Image", "Presenter").

"Ereignisse" `StyleClass` wird erstellt, um die Anzeige der `ListDataPage` Steuerelement mit einem benutzerdefinierten `CardView` steuern, d. h. in definierten Xamarin.Forms.Pages. Die `CardView` Steuerelement verfügt über drei Eigenschaften: `ImageSource`, `Text`, und `Detail`. Das Design ist hartcodiert, um das DataSource Element drei Felder (aus der JSON-Datei) auf diese Eigenschaften für die Anzeige zu binden.

## <a name="5-customize"></a>5. anpassen

Das geerbte Format kann durch Angabe einer Vorlage aus, und Verwenden von datenquellenbindungen überschrieben werden. Der folgende XAML-Code deklariert eine benutzerdefinierte Vorlage für jede Zeile mithilfe der neuen `ListItemControl` und `{p:DataSourceBinding}` Syntax, die im **xamarin. Forms. Pages** -nuget enthalten ist:

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

Durch die Bereitstellung einer `DataTemplate` dieser Code überschreibt die `StyleClass` und verwendet stattdessen das Standardlayout für eine `ListItemControl`.

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

Entwickler, die es vorziehen, c#, XAML können Daten erstellen source Bindungen zu (Denken Sie daran, eine `using Xamarin.Forms.Pages;` Anweisung):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

Es ist ein wenig mehr Arbeit, Designs von Grund auf neu zu erstellen, aber zukünftige Vorschau Versionen machen dies einfacher.

## <a name="troubleshooting"></a>Problembehandlung

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Datei oder Assembly 'Xamarin.Forms.Theme.Light' oder eine ihrer Abhängigkeiten konnte nicht geladen werden

In der Vorschauversion von Designs zur Laufzeit laden können möglicherweise nicht. Fügen Sie den folgenden Code in den relevanten Projekten, die diesen Fehler zu beheben.

**iOS**

In der **Datei "appdelegate.cs"** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

In der **"mainactivity.cs"** fügen Sie die folgenden Zeilen nach `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>Verwandte Themen

- [DataPagesDemo-Beispiel](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
