---
title: Erkennen des dunklen Modus in xamarin. Forms-Anwendungen
description: Der dunkle Modus kann in jeder xamarin. Forms-Anwendung unterstützt werden, die eine Kombination aus resourcewörterbüchern, dynamikresources und Platt Form Kenntnissen verwendet.
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 104237155797ca90c52ad385e8349480f9666c4c
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79303501"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Erkennen des dunklen Modus in xamarin. Forms-Anwendungen

Xamarin. Forms-Anwendungen können auf Änderungen des Betriebssystem Designs reagieren, indem Sie die gleiche Strategie verwenden, die es Ihnen ermöglicht, Designs [zu unterstützen](theming.md). Der Hauptunterschied besteht darin, wie die Änderung des Designs ausgelöst wird. Der dunkle Modus bezieht sich auf einen umfassenderen Satz von Darstellungs Einstellungen, die auf Betriebssystemebene festgelegt werden können, und auf welche Anwendungen sofort geachtet werden soll.

Die Mindestanforderungen für die Verwendung des dunklen Modus in ihren xamarin. Forms-Anwendungen lauten:

- IOS 13 oder höher.
- Android 9 oder höher (Android 9 unterstützt Darstellungsmodi, die Sie Abfragen können).
- Android 10 oder höher (Android 10 benachrichtigt Apps über Änderungen im Modus).
- UWP v 10.0.10240.0 oder höher.
- Xamarin. Forms (beliebige Version).

Der Prozess der Unterstützung von Darstellungsmodi, einschließlich hell und dunkel, sieht wie folgt aus:

1. Definieren von Ressourcen, die von der gesamten Anwendung in einem [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)freigegeben werden.
2. Definieren Sie ein Ressourcenverzeichnis Design für jeden Darstellungs Modus, der außer Kraft setzungen für jedes Format bereitstellt, das Sie ändern möchten.
3. Legen Sie in der Datei **app. XAML** der Anwendung das standardmäßige Darstellungs Modus-Design fest.
4. Formatieren Sie Ihre Anwendung mit der `DynamicResource` Markup Erweiterung, in der Stile bei Änderung der Darstellungsmodi reagieren sollen.
5. Fügen Sie Code hinzu, um auf Betriebssystem-Designänderungen zu lauschen, und laden Sie das entsprechende Design Ihrer Anwendung.

Die folgenden Screenshots zeigen Design Seiten für den hellen und den dunklen Modus:

[![Screenshot der Hauptseite einer Designs-app unter IOS und Android](theming-images/main-page-both-themes.png "Hauptseite der App Thema")](theming-images/main-page-both-themes-large.png#lightbox "Hauptseite der App Thema")
[ ![Screenshot der Detailseite einer Designs-app unter IOS und Android](theming-images/detail-page-both-themes.png "Detail Seite der APP mit Design")](theming-images/detail-page-both-themes-large.png#lightbox "Detail Seite der APP mit Design")

## <a name="define-themes"></a>Definieren von Designs

Befolgen Sie das Design [Handbuch](theming.md) , um ausführliche Informationen zum Erstellen von dunklen und hellen Designs zu erhalten. 

Sie können ein neues Design für Ihre Anwendung ganz einfach an der entsprechenden Stelle des Platt Form Codes festlegen. Zum Laden eines neuen Designs ersetzen Sie einfach das aktuelle Ressourcen Wörterbuch der Anwendung durch ein Ressourcen Wörterbuch mit einem Design Ihrer Wahl:

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>Design erkennen und anwenden

Das aktuelle Design kann mithilfe des [`RequestedTheme`](~/essentials/app-theme.md) Features von [xamarin. Essentials](~/essentials/index.md) (Version 1.4.0 oder neuer) erkannt werden. Anschließend können Sie eine Hilfsmethode in einer neuen Klasse oder in der `App`-Klasse erstellen, um das Design zu konfigurieren:

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
        else
        {
            // change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
    }
}
```

## <a name="react-to-appearance-mode-changes"></a>Reagieren auf Änderungen im Darstellungs Modus

Der Darstellungs Modus auf einem Gerät kann sich aus verschiedenen Gründen ändern, je nachdem, wie der Benutzer seine Einstellungen konfiguriert hat, einschließlich der expliziten Auswahl eines Modus, der Tageszeit oder der Umgebungsfaktoren (z. b. niedriger Beleuchtung). Sie müssen Platt Form Code hinzufügen, um sicherzustellen, dass Ihre Anwendung auf diese Änderungen reagieren kann. in den folgenden Abschnitten wird dies ausführlicher erläutert.

### <a name="android"></a>Android

Um den dunklen Modus in Ihrer APP zu unterstützen, müssen Sie das Design Ihrer APP aktualisieren, das in `Resources/values/styles.xml`gefunden werden kann, um von einem `DayNight` Design zu erben:

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

Wenn Sie ein Upgrade auf die [Material Komponenten](https://www.nuget.org/packages/Xamarin.Google.Android.Material/) von androidx (1.1.0-rc2) oder neuer durchgeführt haben, können Sie Folgendes verwenden:

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

Fügen Sie in der **MainActivity.cs** -Datei Ihrer Anwendung der Eigenschaft `ConfigurationChanges` im `Activity`-Attribut das `ConfigChanges.UiMode`-Feld hinzu, damit Ihre APP über Änderungen im Benutzeroberflächen Modus benachrichtigt wird:

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

Überschreiben Sie in derselben Datei **MainActivity.cs** die `OnConfigurationChanged`-Methode:

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### <a name="ios"></a>iOS

Unter IOS werden Änderungen im Darstellungs Modus über den UIViewController benachrichtigt, der in xamarin. Forms das `ContentPage`ist. Um diese Methode zu überschreiben, erstellen Sie im IOS-Projekt einen benutzerdefinierten Renderer namens `PageRenderer.cs`:

```csharp
using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourApp.iOS.Renderers.PageRenderer))]
namespace YourApp.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                App.ApplyTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            App.ApplyTheme();
        }
    }
}
```

Wenn Sie die `TraitCollectionDidChange`-Methode überschreiben, können Sie auf eine `UserInterfaceStyle` Änderung reagieren.

### <a name="uwp"></a>UWP

Fügen Sie auf der UWP der **MainPage.XAML.cs** -Datei Ihrer Anwendung den folgenden Code hinzu:

```csharp
public sealed partial class MainPage
{
    UISettings uiSettings;

    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new YourApp.App());

        uiSettings = new UISettings();
        uiSettings.ColorValuesChanged += ColorValuesChanged;
    }

    private void ColorValuesChanged(UISettings sender, object args)
    {
        Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
        {
            App.ApplyTheme();
        });
    }
}
```

## <a name="related-links"></a>Verwandte Links

- [Design (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile in xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
