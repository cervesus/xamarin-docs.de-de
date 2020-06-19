---
title: Tabbedpage-Schiebe leisten-Registerkarten Leiste unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die den Transaktionsmodus der Registerkarten Leiste auf einer tabbedpage festlegt.
ms.prod: xamarin
ms.assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8127191aab80d81fc2e532e3d5e14931b834eeae
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137032"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>Tabbedpage-Schiebe leisten-Registerkarten Leiste unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische wird verwendet, um den Transaktionsmodus der Registerkarten Leiste eines festzulegen [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Sie wird in XAML verwendet, indem die `TabbedPage.TranslucencyMode` bindbare Eigenschaft auf einen- `TranslucencyMode` Enumerationswert festgelegt wird:

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

Die- `TabbedPage.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `TabbedPage.SetTranslucencyMode` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den Transaktionsmodus der Registerkarten Leiste eines festzulegen, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) indem Sie einen der folgenden `TranslucencyMode` Enumerationswerte angeben:

- `Default`, wodurch die Registerkarten Leiste auf ihren standardmäßigen Transaktionsmodus festgelegt wird. Dies ist der Standardwert der `TabbedPage.TranslucencyMode`-Eigenschaft.
- `Translucent`, bei dem die Registerkarten Leiste als durchscheinend festgelegt wird.
- `Opaque`, wodurch die Registerkarten Leiste als nicht transparent festgelegt wird.

Außerdem kann die- `GetTranslucencyMode` Methode zum Abrufen des aktuellen Werts der-Enumeration verwendet werden, die `TranslucencyMode` auf den angewendet wird [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

Das Ergebnis ist, dass der Transaktionsmodus der Registerkarten Leiste eines [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) festgelegt werden kann:

![Screenshot der durchlässigen und transparenten Registerkarten leisten unter IOS](tabbedpage-translucent-tabbar-images/translucencymodes.png "Durchlässiges und undurchsichtiges Tabstopp")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
