---
title: Sichtbarkeit von Start Indikatoren unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die die Sichtbarkeit des Start Indikators auf einer Seite festlegt.
ms.prod: xamarin
ms.assetid: F81022E0-3C6C-49C0-A000-FAF6574D3FB7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e76684ffb293380c283153c35c907acc50e40aab
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128075"
---
# <a name="home-indicator-visibility-on-ios"></a>Sichtbarkeit von Start Indikatoren unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen wird die Sichtbarkeit des Start Indikators für einen festgelegt [`Page`](xref:Xamarin.Forms.Page) . Sie wird in XAML verwendet, indem die [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) bindbare Eigenschaft auf festgelegt wird `boolean` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

Die- `Page.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `Page.SetPrefersHomeIndicatorAutoHidden` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. Page. setprefershomeindiaseorautohidden ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Page}, System. Boolean))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace steuert die Sichtbarkeit des Start Indikators. Außerdem ist [ `Page.PrefersHomeIndicatorAutoHidden` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. Page. prefershomeindiaseorautohidden ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Page}))-Methode kann verwendet werden, um die Sichtbarkeit des Start Indikators abzurufen.

Das Ergebnis ist, dass die Sichtbarkeit des Start Indikators auf einem [`Page`](xref:Xamarin.Forms.Page) gesteuert werden kann:

![Screenshot der Sichtbarkeit von Start Indikatoren auf einer IOS-Seite](page-home-indicator-images/home-indicator-visibility.png "Seite zur Sichtbarkeit des Homebalkens")

> [!NOTE]
> Diese plattformspezifische kann auf-, [`ContentPage`](xref:Xamarin.Forms.ContentPage) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) -, [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) -und-Objekte angewendet werden [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
