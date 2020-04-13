---
title: Erkennen des dunklen Modus in Xamarin.Forms-Anwendungen
description: Der dunkle Modus kann in jeder Xamarin.Forms-Anwendung mithilfe einer Kombination aus ResourceDictionaries, DynamicResources und Plattformwissen unterstützt werden.
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 03/13/2020
ms.openlocfilehash: 7fe1a98e6a497a5791f26df2fc96d41781b71ef6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628316"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Erkennen des dunklen Modus in Xamarin.Forms-Anwendungen

Xamarin.Forms-Anwendungen können auf Änderungen des Betriebssystemdesigns reagieren, indem sie dieselbe Strategie verwenden, mit der Sie [die Themenbildung](theming.md)unterstützen können. Der Hauptunterschied besteht darin, wie der Themenwechsel ausgelöst wird. Der dunkle Modus bezieht sich auf einen breiteren Satz von Darstellungseinstellungen, die auf Betriebssystemebene festgelegt werden können und von denen erwartet wird, dass sie diese sofort respektieren und darauf reagieren.

Die Mindestanforderungen für die Verwendung des dunklen Modus in Ihren Xamarin.Forms-Anwendungen sind:

- iOS 13 oder höher.
- Android 9 oder höher (Android 9 unterstützt Darstellungsmodi, die Sie abfragen können).
- Android 10 oder höher (Android 10 benachrichtigt Apps über Modusänderungen).
- UWP v10.0.10240.0 oder höher.
- Xamarin.Forms (jede Version).

Der Prozess der Unterstützung der Darstellungsmodi, einschließlich hell und dunkel, ist wie folgt:

1. Definieren Sie Ressourcen, die [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary)von der gesamten Anwendung in einer freigegeben werden.
2. Definieren Sie für jeden Darstellungsmodus ein Ressourcenwörterbuchdesign, das über schrieben, welche Überschreibungen für jeden Stil, den Sie ändern möchten, spezifisch sind.
3. Legen Sie das Standardmäßige Darstellungsmodus-Design in der **App.xaml-Datei** Ihrer Anwendung fest.
4. Stylen Sie `DynamicResource` Ihre Anwendung mit der Markuperweiterung, in der Stile reagieren sollen, wenn sich die Darstellungsmodi ändern.
5. Fügen Sie Code hinzu, um auf Änderungen des Betriebssystemdesigns zu warten, und laden Sie das entsprechende Design Ihrer Anwendung.

Die folgenden Screenshots zeigen Themenseiten für den hellen und dunklen Modus:

[![Screenshot der Hauptseite einer Themen-App, auf iOS und Android](theming-images/main-page-both-themes.png "Hauptseite der Themen-App")](theming-images/main-page-both-themes-large.png#lightbox "Hauptseite der Themen-App")
[![Screenshot der Detailseite einer Thematischen App, auf iOS und Android](theming-images/detail-page-both-themes.png "Detailseite der Themen-App")](theming-images/detail-page-both-themes-large.png#lightbox "Detailseite der Themen-App")

## <a name="define-themes"></a>Definieren von Themen

Folgen Sie der [Themenanleitung](theming.md) für Schritt für Schritt Details über das Erstellen von dunklen und hellen Themen.

Sie können ganz einfach ein neues Design für Ihre Anwendung am entsprechenden Speicherort Ihres Plattformcodes festlegen. Um ein neues Design zu laden, ersetzen Sie einfach das aktuelle Ressourcenwörterbuch der Anwendung durch ein thematisches Ressourcenwörterbuch Ihrer Wahl:

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="detect-and-apply-theme"></a>Erkennen und Anwenden von Thema

Die Erkennung des aktuell ausgeführten Themas [`RequestedTheme`](~/essentials/app-theme.md) kann mit der Funktion [von Xamarin.Essentials](~/essentials/index.md) (Version 1.4.0 oder neuer) erreicht werden. Anschließend können Sie eine Hilfsmethode in einer `App` neuen Klasse oder in der Klasse erstellen, um das Design zu konfigurieren:

```csharp
public partial class App : Application
{
    public static void ApplyTheme()
    {
        if (AppInfo.RequestedTheme == AppTheme.Dark)
        {
            // Change to dark theme
            // e.g. App.Current.Resources = new YourDarkTheme();
        }
        else
        {
            // Change to light theme
            // e.g. App.Current.Resources = new YourLightTheme();
        }
    }
}
```

## <a name="react-to-appearance-mode-changes"></a>Reagieren auf Änderungen des Darstellungsmodus

Der Darstellungsmodus auf einem Gerät kann sich aus einer Vielzahl von Gründen ändern, je nachdem, wie der Benutzer seine Einstellungen konfiguriert hat, einschließlich der expliziten Auswahl eines Modus, einer Tageszeit oder Umgebungsfaktoren wie Lichtmangel. Sie müssen Plattformcode hinzufügen, um sicherzustellen, dass Ihre Anwendung auf diese Änderungen reagieren kann, und in den folgenden Abschnitten wird dies ausführlicher erläutert.

### <a name="android"></a>Android

Um den dunklen Modus in Ihrer App zu unterstützen, müssen Sie `Resources/values/styles.xml`das Design Ihrer `DayNight` App aktualisieren, das in zu finden ist, um von einem Design zu erben:

```xml
<style name="MainTheme.Base" parent="Theme.AppCompat.DayNight">
```

Wenn Sie ein Upgrade auf die [Materialkomponenten](https://www.nuget.org/packages/Xamarin.Google.Android.Material/) von AndroidX (1.1.0-rc2) oder neuer durchgeführt haben, können Sie:

```xml
<style name="MainTheme.Base" parent="Theme.MaterialComponents.DayNight">
```

Fügen Sie das **MainActivity.cs** `ConfigChanges.UiMode` Feld in der MainActivity.cs `ConfigurationChanges` Datei `Activity` Ihrer Anwendung der Eigenschaft im Attribut hinzu, damit Ihre App über Änderungen im UI-Modus benachrichtigt wird:

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

Überschreiben **MainActivity.cs** Sie in derselben `OnConfigurationChanged` MainActivity.cs Datei die Methode:

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);
    App.ApplyTheme();
}
```

### <a name="ios"></a>iOS

Unter iOS werden Änderungen des Darstellungsmodus auf dem UIViewController benachrichtigt, der in Xamarin.Forms die `ContentPage`ist. Um diese Methode zu überschreiben, erstellen Sie in Ihrem `PageRenderer.cs`iOS-Projekt einen benutzerdefinierten Renderer namens:

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

Durch das `TraitCollectionDidChange` Überschreiben der Methode `UserInterfaceStyle` können Sie auf eine Änderung reagieren.

### <a name="uwp"></a>UWP

Fügen Sie in UWP den folgenden Code zur **MainPage.xaml.cs** Datei Ihrer Anwendung hinzu:

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

- [Theming (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Ressourcenverzeichnisse](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamische Stile in Xamarin.Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Formatieren von Xamarin.Forms-Apps mithilfe von XAML-Formatvorlagen](~/xamarin-forms/user-interface/styles/xaml/index.md)
