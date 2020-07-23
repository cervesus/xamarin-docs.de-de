---
title: Hinzufügen einer IOS-spezifischen Formatierung
description: In diesem Artikel wird erläutert, wie Sie eine IOS-spezifische Darstellung ohne einen Xamarin.Forms benutzerdefinierten Renderer festlegen.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4381b1fbe7b38c2695bce645903d599bdb792fc2
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937435"
---
# <a name="adding-ios-specific-formatting"></a>Hinzufügen einer IOS-spezifischen Formatierung

Eine Möglichkeit zum Festlegen einer IOS-spezifischen Formatierung besteht darin, einen [benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) für ein Steuerelement zu erstellen und plattformspezifische Stile und Farben für jede Plattform festzulegen.

Weitere Optionen zum Steuern der Darstellung Ihrer Xamarin.Forms IOS-APP sind:

- Konfigurieren von Anzeigeoptionen in " [ **Info. plist** "](#customizing-infoplist)
- Festlegen von Steuerelement Stilen über die [ `UIAppearance` API](#uiappearance-api)

Diese Alternativen werden im folgenden erläutert.

## <a name="customizing-infoplist"></a>Anpassen von "Info. plist"

In der **Info. plist** -Datei können Sie einige Aspekte des Renderings einer IOS-Anwendung konfigurieren, wie z. b. wie (und ob) die Statusleiste angezeigt wird.

Das [TODO-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo) verwendet beispielsweise den folgenden Code, um die Farbe und die Textfarbe der Navigationsleiste auf allen Plattformen festzulegen:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Das Ergebnis wird im folgenden Bildschirm Ausschnitt gezeigt. Beachten Sie, dass die Status leisten Elemente schwarz sind (Dies kann nicht innerhalb von festgelegt werden, Xamarin.Forms da es sich um ein plattformspezifisches Feature handelt).

![IOS-Themen](theme-images/status-default-sml.png)

Im Idealfall ist die Statusleiste auch weiß, was wir direkt im IOS-Projekt erreichen können. Fügen Sie der Datei " **Info. plist** " die folgenden Einträge hinzu, um zu erzwingen, dass die Statusleiste weiß ist:

![IOS Info. plist-Einträge](theme-images/info-plist.png)

oder bearbeiten Sie die entsprechende Datei " **Info. plist** " direkt, um Folgendes einzuschließen:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Wenn die App nun ausgeführt wird, ist die Navigationsleiste grün, und der Text ist weiß (aufgrund der Xamarin.Forms Formatierung), *und* der Status leisten Text ist dank der IOS-spezifischen Konfiguration ebenfalls weiß:

![IOS-Themen](theme-images/status-white-sml.png)

## <a name="uiappearance-api"></a>Uiappearance-API

Die [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) kann verwendet werden, um visuelle Eigenschaften für viele IOS-Steuerelemente festzulegen, *ohne* einen [benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)erstellen zu müssen.

Wenn Sie der **AppDelegate.cs** -Methode eine einzelne Codezeile hinzufügen, `FinishedLaunching` können Sie alle Steuerelemente eines bestimmten Typs mithilfe ihrer-Eigenschaft formatieren `Appearance` . Der folgende Code enthält zwei Beispiele: Global Formatieren der Registerkarten Leiste und des Switch-Steuer Elements:

**AppDelegate.cs** im IOS-Projekt

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>Uitabbar

Standardmäßig wird das Symbol für die ausgewählte Registerkarten Leiste in einem[`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
ist blau:

![Standardmäßiges IOS-Registerkarten Symbol in tabbedpage](theme-images/tabbar-default.png)

Um dieses Verhalten zu ändern, legen Sie die- `UITabBar.Appearance` Eigenschaft fest:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Dies bewirkt, dass die ausgewählte Registerkarte grün ist:

![Grünes IOS-Registerkarten Symbol in tabbedpage](theme-images/tabbar-custom.png)

Mit dieser API können Sie das Erscheinungsbild derXamarin.Forms
`TabbedPage`unter IOS mit wenig Code. Weitere Informationen zur Verwendung eines benutzerdefinierten Renderers zum Festlegen einer bestimmten Schriftart für die Registerkarte finden Sie in der Anleitung zum [Anpassen von Registerkarten](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) .

### <a name="uiswitch"></a>UISwitch

Das- `Switch` Steuerelement ist ein weiteres Beispiel, das leicht formatiert werden kann:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Diese beiden Bildschirmaufzeichnungen zeigen das Standard `UISwitch` Steuerelement auf der linken Seite und die angepasste Version (Einstellung `Appearance` ) auf der rechten Seite im [TODO-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)an:

![Uiswitch-Standardfarbe](theme-images/switch-default.png) ![Angepasste uiswitch-Farbe](theme-images/switch-custom.png)

### <a name="other-controls"></a>Weitere Steuermöglichkeiten

Viele IOS-Benutzeroberflächen-Steuerelemente können mithilfe der- [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)ihre Standardfarben und andere Attribute festlegen.

## <a name="related-links"></a>Ähnliche Themen

- [Uiappearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Anpassen von Registerkarten](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
