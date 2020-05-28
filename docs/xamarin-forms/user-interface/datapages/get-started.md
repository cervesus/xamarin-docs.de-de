---
title: ''
description: In diesem Artikel wird erläutert, wie Sie mit dem Aufbau einer einfachen datengesteuerten Seite mithilfe von Xamarin.Forms DataPages beginnen.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 17cc67c7fcc89454ff8dcac9926617b4ed1f4b77
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84134393"
---
# <a name="getting-started-with-datapages"></a>Ersten Einstieg in DataPages

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)

![](~/media/shared/preview.png "This API is currently in preview")

> [!IMPORTANT]
> DataPages erfordert einen Design Xamarin.Forms Verweis zum Rendering. Dies umfasst die Installation von [ Xamarin.Forms . Design. basieren](https://www.nuget.org/packages/Xamarin.Forms.Theme.Base/) Sie auf das nuget-Paket in Ihrem Projekt, gefolgt von dem [ Xamarin.Forms . Design. Light](https://www.nuget.org/packages/Xamarin.Forms.Theme.Light/) oder [ Xamarin.Forms . Design. Dark](https://www.nuget.org/packages/Xamarin.Forms.Theme.Dark/) nuget-Pakete.

Führen Sie die folgenden Schritte aus, um mit dem Aufbau einer einfachen datengesteuerten Seite mithilfe der DataPages-Vorschau zu beginnen. In dieser Demo wird ein hart codierter Stil ("Ereignisse") in den Preview-Builds verwendet, der nur mit dem spezifischen JSON-Format im Code funktioniert.

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

## <a name="1-add-nuget-packages"></a>1. Hinzufügen von nuget-Paketen

Fügen Sie diese nuget-Pakete zu Ihrer Xamarin.Forms .NET Standard Bibliothek und ihren Anwendungsprojekten hinzu:

- Xamarin.Forms. Seiten
- Xamarin.Forms. Design. Base
- Eine nuget-Design Implementierung (z. b. Xamarin.Forms. Design. Light)

## <a name="2-add-theme-reference"></a>2. Design Verweis hinzufügen

Fügen Sie in der Datei **app. XAML** eine benutzerdefinierte für das Design hinzu, `xmlns:mytheme` und stellen Sie sicher, dass das Design in das Ressourcen Wörterbuch der Anwendung zusammengeführt wird:

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
> Sie sollten auch die Schritte zum Laden von Design-Assemblys [(unten)](#loadtheme) ausführen, indem Sie dem IOS `AppDelegate` -und Android-Code Code hinzufügen `MainActivity` . Dies wird in einer zukünftigen Vorschauversion verbessert.

## <a name="3-add-a-xaml-page"></a>3. Hinzufügen einer XAML-Seite

Fügen Sie der Anwendung eine neue XAML Xamarin.Forms -Seite hinzu, und *Ändern Sie die Basisklasse* von `ContentPage` in `Xamarin.Forms.Pages.ListDataPage` . Dies muss sowohl in c# als auch in XAML erfolgen:

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

Zusätzlich zum Ändern des Stamm Elements in `<p:ListDataPage>` den benutzerdefinierten Namespace für `xmlns:p` muss auch hinzugefügt werden:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Anwendungs Unterklasse**

Ändern `App` Sie den Klassenkonstruktor, sodass `MainPage` auf einen `NavigationPage` mit dem neuen festgelegt ist `SessionDataPage` . Eine Navigationsseite *muss* verwendet werden.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Fügen Sie die Datenquelle hinzu.

Löschen `Content` Sie das-Element, und ersetzen Sie es durch ein-Element `p:ListDataPage.DataSource` , um die Seite mit Daten aufzufüllen. Im folgenden Beispiel wird eine JSON-Remote Datendatei aus einer URL geladen.

> [!NOTE]
> Die Vorschau *erfordert* ein- `StyleClass` Attribut, um Renderinghinweise für die Datenquelle bereitzustellen. Der `StyleClass="Events"` verweist auf ein Layout, das in der Vorschau vordefiniert ist, und enthält Stile, die für die Verwendung der verwendeten JSON-Datenquelle *hart codiert* sind.

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

Ein Beispiel für die JSON-Daten aus der Demo Quelle finden Sie unten:

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

Die oben aufgeführten Schritte sollten zu einer Arbeitsdaten Seite führen:

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

Dies funktioniert, weil der vorgefertigte Stil **"Ereignisse"** im Design "lighttheme" im Design "Light" vorhanden ist und Stile definiert sind, die mit der Datenquelle (z. b. "Title", "Bild", "Presenter").

Die "Ereignisse" `StyleClass` wird erstellt, um das `ListDataPage` Steuerelement mit einem benutzerdefinierten Steuerelement anzuzeigen `CardView` , das in definiert ist Xamarin.Forms . Seiten. Das `CardView` -Steuerelement verfügt über drei Eigenschaften: `ImageSource` , `Text` und `Detail` . Das Design ist hart codiert, um die drei Felder der DataSource (aus der JSON-Datei) für die Anzeige an diese Eigenschaften zu binden.

## <a name="5-customize"></a>5. anpassen

Der geerbte Stil kann überschrieben werden, indem eine Vorlage angegeben und Datenquellen Bindungen verwendet werden. Der folgende XAML-Code deklariert eine benutzerdefinierte Vorlage für jede Zeile mithilfe der neuen `ListItemControl` -und- `{p:DataSourceBinding}` Syntax, die in enthalten ist ** Xamarin.Forms . **Nuget-Seiten:

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

Durch Bereitstellen von `DataTemplate` überschreibt dieser Code den `StyleClass` und verwendet stattdessen das Standardlayout für einen `ListItemControl` .

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

Entwickler, die c# in XAML bevorzugen, können auch Datenquellen Bindungen erstellen (Beachten Sie, dass Sie eine- `using Xamarin.Forms.Pages;` Anweisung einschließen):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

Es ist ein wenig mehr Arbeit, Designs von Grund auf neu zu erstellen, aber zukünftige Vorschau Versionen machen dies einfacher.

## <a name="troubleshooting"></a>Problembehandlung

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Die Datei oder Assembly konnte nicht geladen werden Xamarin.Forms . Theme. Light ' oder eine seiner Abhängigkeiten

In der Vorschauversion können Designs möglicherweise zur Laufzeit nicht geladen werden. Fügen Sie den unten gezeigten Code den relevanten Projekten hinzu, um diesen Fehler zu beheben.

**iOS**

Fügen Sie in **AppDelegate.cs** die folgenden Zeilen hinzu.`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

Fügen Sie in **MainActivity.cs** die folgenden Zeilen hinzu.`LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>Verwandte Links

- [Datapagesdemo-Beispiel](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
