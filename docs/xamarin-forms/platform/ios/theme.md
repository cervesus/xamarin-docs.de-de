---
title: Hinzufügen von iOS-spezifische Formatierung
description: In diesem Artikel wird erläutert, wie iOS-spezifische Darstellung ohne Verwendung eines benutzerdefinierten Xamarin.Forms-Renderers festgelegt wird.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 3b8a440617dedfbe23f869e865b3cedae21d6c5b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241376"
---
# <a name="adding-ios-specific-formatting"></a>Hinzufügen von iOS-spezifische Formatierung

Eine Möglichkeit zum Festlegen der iOS-spezifische Formatierung ist die Erstellung einer [benutzerdefinierter Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) für ein Steuerelement und die plattformspezifische Stile festgelegt und Farben für jede Plattform.

Andere Optionen zu steuern, wie Ihre Xamarin.Forms iOS-app-Darstellung enthalten:

* Konfigurieren von Anzeigeoptionen in [ **"Info.plist"**](#info-plist)
* Festlegen der Stile von Listensteuerelementen über die [ `UIAppearance` API](#uiappearance)

Diese alternativen werden nachfolgend beschrieben.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Anpassen der Datei "Info.plist"

Die **"Info.plist"** Datei können Sie die Konfiguration einiger Aspekte der Renderering einer iOS-Anwendung, z. B. wie (und gibt an, ob) auf die Statusleiste angezeigt wird.

Z. B. die [Beispiel "Todo"](https://developer.xamarin.com/samples/xamarin-forms/Todo/) der folgende Code verwendet, um Farbe und Textfarbe auf der Navigationsleiste auf allen Plattformen festzulegen:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Das Ergebnis wird im folgenden Codeausschnitt Bildschirm angezeigt. Beachten Sie, dass die Leiste Statuselemente Schwarz sind (Dies kann nicht festgelegt werden in Xamarin.Forms, da es sich um eine bestimmte Funktion ist).

![](theme-images/status-default-sml.png "iOS-Design")

Im Idealfall die Statusleiste wird auch die Whitelist gesetzt werden: Es können verschiedene Aufgaben direkt in das iOS-Projekt. Fügen Sie die folgenden Einträge, die **"Info.plist"** zu erzwingen, dass die Statusleiste weiß sein:

![](theme-images/info-plist.png "iOS-Einträge der Datei \"Info.plist\"")

oder bearbeiten Sie die entsprechende **"Info.plist"** Datei direkt in enthalten:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Wenn die app ausgeführt wird, die Navigationsleiste ist Grün und der Text ist (aufgrund von Xamarin.Forms Formatierung) weiß *und* der Text der Statusleiste wird auch weißen Dank der iOS-spezifischen Konfiguration:

![](theme-images/status-white-sml.png "iOS-Design")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

Der [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) können verwendet werden, um die visuelle Eigenschaften für viele iOS-Steuerelemente festlegen *ohne* erstellen eine [benutzerdefinierter Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Eine einzige Zeile Code zum Hinzufügen der **Datei "appdelegate.cs"** `FinishedLaunching` Methode kann alle Steuerelemente eines bestimmten Typs mit formatieren ihrer `Appearance` Eigenschaft. Der folgende Code enthält zwei Beispiele – Global formatieren die Registerkarte Balken- und Steuerelement wechseln:

**Datei "appdelegate.cs"** in der iOS-Projekt

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

### <a name="uitabbar"></a>UITabBar

Standardmäßig wird das ausgewählte Registerkarte Balken-Symbol in einer [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) blaue wäre:

![](theme-images/tabbar-default.png "Standard-iOS-Leiste Registerkartensymbol in \"tabbedpage\"")

Um dieses Verhalten zu ändern, legen die `UITabBar.Appearance` Eigenschaft:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Dies bewirkt, dass die ausgewählte Registerkarte grün sein:

![](theme-images/tabbar-custom.png "Grüne iOS Symbol Registerkarte Balken \"tabbedpage\"")

Mit dieser API können Sie die Gestaltung der Xamarin.Forms `TabbedPage` unter iOS mit nur wenigen Codezeilen. Finden Sie in der [anpassen Registerkarten Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) für Weitere Informationen zur Verwendung eines benutzerdefinierten Renderers eine bestimmte Schriftart für die Registerkarte fest.

### <a name="uiswitch"></a>UISwitch

Die `Switch` Steuerelement ist ein weiteres Beispiel, das ganz einfach formatiert werden kann:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Diese beiden Screenshots zeigen die Standardeinstellung `UISwitch` -Steuerelement auf der linken Seite und die benutzerdefinierte Version (Einstellung `Appearance`) auf der rechten Seite der [Beispiel "Todo"](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Standardfarbe UISwitch") ![](theme-images/switch-custom.png "UISwitch Farbe angepasst")

### <a name="other-controls"></a>Andere Steuerelemente

Viele Steuerelemente der iOS-Benutzeroberfläche haben, die standardmäßigen Farben und andere Attribute, die festlegen, mit der [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Verwandte Links

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Anpassen von Registerkarten](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
