---
title: Erste Schritte mit DataPages
description: In diesem Artikel wird erläutert, wie für den Einstieg in die Erstellung einer einfachen datengesteuerten-Webseite mithilfe von Xamarin.Forms DataPages wird.
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: 1f7917784ea66c31979b87f43639a7d03756692c
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725595"
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

Fügen Sie in der Datei **app. XAML** eine benutzerdefinierte `xmlns:mytheme` für das Design hinzu, und stellen Sie sicher, dass das Design mit dem Ressourcen Wörterbuch der Anwendung zusammengeführt wird:

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

Fügen Sie der xamarin. Forms-Anwendung eine neue XAML-Seite hinzu, und *Ändern Sie die Basisklasse* von `ContentPage` in `Xamarin.Forms.Pages.ListDataPage`. Dies hat in der C#- und die XAML durchgeführt werden:

**C#Datei**

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

Zusätzlich zum Ändern des Stamm Elements in `<p:ListDataPage>` der benutzerdefinierte Namespace für `xmlns:p` ebenfalls hinzugefügt werden:

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

Ändern Sie den `App` Klassenkonstruktor, sodass der `MainPage` auf eine `NavigationPage` festgelegt ist, die die neue `SessionDataPage`enthält. Eine Navigationsseite *muss* verwendet werden.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Fügen Sie die Datenquelle hinzu.

Löschen Sie das `Content`-Element, und ersetzen Sie es durch ein `p:ListDataPage.DataSource`, um die Seite mit Daten aufzufüllen. Im Beispiel unten eine remote-Json ist-Datendatei aus einer URL geladen wird.

> [!NOTE]
> Die Vorschau *erfordert* ein `StyleClass` Attribut, um Renderinghinweise für die Datenquelle bereitzustellen. Der `StyleClass="Events"` verweist auf ein Layout, das in der Vorschau vordefiniert ist, und enthält Stile, die für die Verwendung der verwendeten JSON-Datenquelle *hart codiert* sind.

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

Die oben genannten Schritte sollten dazu führen, dass eine Datenseite arbeiten:

[![](get-started-images/demo-sml.png "DataPages Sample Application")](get-started-images/demo.png#lightbox "DataPages Sample Application")

Dies funktioniert, weil der vorgefertigte Stil **"Ereignisse"** im Design "lighttheme" im Design "Light" vorhanden ist und Stile definiert sind, die mit der Datenquelle (z. b. "Title", "Image", "Presenter").

Der `StyleClass` "Ereignisse" wird erstellt, um das `ListDataPage`-Steuerelement mit einem benutzerdefinierten `CardView`-Steuerelement anzuzeigen, das in xamarin. Forms. Pages definiert ist. Das `CardView`-Steuerelement verfügt über drei Eigenschaften: `ImageSource`, `Text`und `Detail`. Das Design ist hartcodiert, um das DataSource Element drei Felder (aus der JSON-Datei) auf diese Eigenschaften für die Anzeige zu binden.

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

Durch Bereitstellen eines `DataTemplate` dieser Code die `StyleClass` überschreibt und stattdessen das Standardlayout für eine `ListItemControl`verwendet.

[![](get-started-images/custom-sml.png "DataPages Sample Application")](get-started-images/custom.png#lightbox "DataPages Sample Application")

Entwickler, die C# XAML bevorzugen, können auch Datenquellen Bindungen erstellen (Beachten Sie, dass Sie eine `using Xamarin.Forms.Pages;`-Anweisung einschließen):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```

Es ist ein wenig mehr Arbeit, Designs von Grund auf neu zu erstellen, aber zukünftige Vorschau Versionen machen dies einfacher.

## <a name="troubleshooting"></a>Problembehandlung

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Datei oder Assembly 'Xamarin.Forms.Theme.Light' oder eine ihrer Abhängigkeiten konnte nicht geladen werden

In der Vorschauversion von Designs zur Laufzeit laden können möglicherweise nicht. Fügen Sie den folgenden Code in den relevanten Projekten, die diesen Fehler zu beheben.

**iOS**

Fügen Sie in **AppDelegate.cs** nach `LoadApplication` die folgenden Zeilen hinzu.

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

Fügen Sie in **MainActivity.cs** nach `LoadApplication` die folgenden Zeilen hinzu.

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```

## <a name="related-links"></a>Verwandte Links

- [Datapagesdemo-Beispiel](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
