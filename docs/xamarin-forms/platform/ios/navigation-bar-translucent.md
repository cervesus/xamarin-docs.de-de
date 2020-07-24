---
title: Translustialisierung der Navigations Seitenleiste unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die die Transparenz der Navigationsleiste auf einer Navigationsseite ändert.
ms.prod: xamarin
ms.assetid: 1150941B-56DB-4781-BE36-A4C4F9F2C500
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d30e1f86e2ef22250b9e25e2352501b6f2c05ade
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939138"
---
# <a name="navigationpage-bar-translucency-on-ios"></a>Translustialisierung der Navigations Seitenleiste unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische dient zum Ändern der Transparenz der Navigationsleiste eines [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) und wird in XAML verwendet, indem die [`NavigationPage.IsNavigationBarTranslucent`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty) angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

Die- `NavigationPage.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `NavigationPage.EnableTranslucentNavigationBar` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. navigationpage. enabletranslucentnavigationbar ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Navigationpage}))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um die Navigationsleiste durchlässig zu gestalten. Außerdem verfügt die- [`NavigationPage`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage) Klasse im- `Xamarin.Forms.PlatformConfiguration.iOSSpecific` Namespace ebenfalls über ein [ `DisableTranslucentNavigationBar` ] (Xref:) Xamarin.Forms . Platformconfiguration. iosspecific. navigationpage. disabletranslucentnavigationbar ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Navigationpage}))-Methode, die die Navigationsleiste auf ihren Standardzustand und einen [ `SetIsNavigationBarTranslucent` ] (Xref:) wiederherstellt Xamarin.Forms . Platformconfiguration. iosspecific. navigationpage. seetisnavigationbar. Xamarin.Forms Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Navigationpage}, System. Boolean)-Methode, die verwendet werden kann, um die Navigation in der Navigationsleiste durch Aufrufen von [ `IsNavigationBarTranslucent` ] (Xref:) zu wechseln Xamarin.Forms . Platformconfiguration. iosspecific. navigationpage. isnavigationbarmit ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Navigationpage}))-Methode:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Das Ergebnis ist, dass die Transparenz der Navigationsleiste geändert werden kann:

![Plattformspezifischer Navigationsleiste](navigation-bar-translucent-images/translucent-navigation-bar.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
