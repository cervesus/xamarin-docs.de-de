---
title: Hinzufügen von iOS-spezifische Formatierung
description: In diesem Artikel wird erläutert, wie iOS-spezifische Darstellung ohne Verwendung eines benutzerdefinierten Renderers für Xamarin.Forms festgelegt werden.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: 74a3cdc340cb09e8adf15ed0dd09315c985d18b5
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243530"
---
# <a name="adding-ios-specific-formatting"></a>Hinzufügen von iOS-spezifische Formatierung

Eine Möglichkeit zum Festlegen von iOS-spezifische Formatierung ist die Erstellung einer [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) für ein Steuerelement und Set plattformspezifischen Formatvorlagen und Farben für jede Plattform.

Weitere Optionen für das Steuern der Darstellung der Xamarin.Forms iOS-app einschließen:

* Konfigurieren Sie die Anzeigeoptionen in [ **"Info.plist"**](#info-plist)
* Festlegen der Stile von Listensteuerelementen über die [ `UIAppearance` API](#uiappearance)

Diese alternativen werden unten erläutert.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Anpassen der Datei "Info.plist"

Die **"Info.plist"** der Datei können Sie die Konfiguration einiger Aspekte der Renderering für ein iOS-Anwendung, z. B. wie (und ob) auf die Statusleiste angezeigt wird.

Z. B. die [TODO-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/Todo/) den folgenden Code verwendet, um Farbe und der Textfarbe auf der Navigationsleiste auf allen Plattformen zu festzulegen:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Das Ergebnis wird der Bildschirm Codeausschnitt unten dargestellt. Beachten Sie, dass die Leiste Statuselemente Schwarz sind (Dies kann nicht festgelegt werden in Xamarin.Forms, da es sich um eine plattformspezifische-Funktion ist).

![](theme-images/status-default-sml.png "iOS Designumgebung")

Im Idealfall die Statusleiste auch wäre weißen - etwas wir erreichen direkt in das iOS-Projekt. Fügen Sie die folgenden Einträge, die **"Info.plist"** zum Erzwingen der Statusleiste weiß sein:

![](theme-images/info-plist.png "iOS Einträge der Datei \"Info.plist\"")

oder bearbeiten Sie die entsprechenden **"Info.plist"** Datei direkt einzuschließen:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Nun, wenn die Anwendung ausgeführt wird, die Navigationsleiste grün ist und der Text weiß (aufgrund von Xamarin.Forms Formatierung ist) *und* Text in der Statusleiste wird auch weißen Dank iOS-spezifischen Konfigurationsaufgaben:

![](theme-images/status-white-sml.png "iOS Designumgebung")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>UIAppearance API

Die [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) können verwendet werden, um die visuelle Eigenschaften für viele iOS-Steuerelemente festlegen *ohne* erstellen eine [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/index.md).

Eine einzige Codezeile zum Hinzufügen der **AppDelegate.cs** `FinishedLaunching` Methode kann alle Steuerelemente eines bestimmten Typs unter Verwendung formatieren ihrer `Appearance` Eigenschaft. Der folgende Code enthält zwei Beispiele – Global Formatieren der Registerkarte "Balken- und Steuerelement wechseln:

**AppDelegate.cs** in der iOS-Projekt

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

Standardmäßig wird das Symbol der ausgewählten Registerkarte Balken in einer [ `TabbedPage` ](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md) wäre Blau:

![](theme-images/tabbar-default.png "Standard-iOS-Leiste Registerkartensymbol in TabbedPage")

Um dieses Verhalten zu ändern, legen die `UITabBar.Appearance` Eigenschaft:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Dies bewirkt, dass die ausgewählte Registerkarte grün sein:

![](theme-images/tabbar-custom.png "IOS-Leiste Registerkartensymbol in TabbedPage Grün")

Diese API verwenden, können Sie anpassen, die Darstellung der Xamarin.Forms `TabbedPage` für iOS mit nur wenigen Codezeilen. Finden Sie in der [anpassen Registerkarten Rezept](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/) für ausführlichere Informationen zum Verwenden eines benutzerdefinierten Renderers eine bestimmte Schriftart für die Registerkarte "festgelegt.

### <a name="uiswitch"></a>UISwitch

Die `Switch` Steuerelement ist ein weiteres Beispiel, das einfache Weise formatiert werden kann:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Diese zwei Bildschirmaufnahmen (Screenshots) anzeigen, die Standardeinstellung `UISwitch` Steuerelement auf der linken Seite und die angepasste Version (Einstellung `Appearance`) auf der rechten Seite in der [TODO-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/Todo/):

![](theme-images/switch-default.png "Standardfarbe UISwitch") ![ ] (theme-images/switch-custom.png "UISwitch Farbe angepasst")

### <a name="other-controls"></a>Andere Steuerelemente

Viele iOS Benutzeroberflächen-Steuerelemente können ihre Standardfarben und anderen Attribute festgelegt, mit der [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Verwandte Links

- [UIAppearance](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Anpassen von Registerkarten](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/ios/customize-tabs/)
