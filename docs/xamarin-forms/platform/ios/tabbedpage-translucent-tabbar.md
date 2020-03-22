---
title: Tabbedpage-Schiebe leisten-Registerkarten Leiste unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die den Transaktionsmodus der Registerkarten Leiste auf einer tabbedpage festlegt.
ms.prod: xamarin
ms.assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: 55fda0be2e260c5aa4a34ab2dcc1ac3cac33b92a
ms.sourcegitcommit: 6c60914b380ff679bbffd7790edd4d5e18005d0a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80070294"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>Tabbedpage-Schiebe leisten-Registerkarten Leiste unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische wird verwendet, um den Transaktionsmodus der Registerkarten Leiste eines [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)festzulegen. Sie wird in XAML verwendet, indem die `TabbedPage.TranslucencyMode` bindbare-Eigenschaft auf einen `TranslucencyMode`-Enumerationswert festgelegt wird:

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

Die `TabbedPage.On<iOS>`-Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `TabbedPage.SetTranslucencyMode`-Methode im [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um den Transaktionsmodus der Registerkarten Leiste auf einem [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) festzulegen, indem Sie einen der folgenden `TranslucencyMode` Enumerationswerte angeben:

- `Default`, mit dem die Registerkarten Leiste auf den standardmäßigen Modus für die Transaktions Anzeige festgelegt wird. Dies ist der Standardwert der `TabbedPage.TranslucencyMode`-Eigenschaft.
- `Translucent`, mit dem die Registerkarten Leiste auf eine durchscheinend festgelegt wird.
- `Opaque`, wodurch die Registerkarten Leiste als nicht transparent festgelegt wird.

Außerdem kann die `GetTranslucencyMode`-Methode verwendet werden, um den aktuellen Wert der `TranslucencyMode` Enumeration abzurufen, die auf die [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)angewendet wird.

Das Ergebnis ist, dass der Transaktionsmodus der Registerkarten Leiste eines [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) festgelegt werden kann:

![Screenshot der durchlässigen und transparenten Registerkarten leisten unter IOS](tabbedpage-translucent-tabbar-images/translucencymodes.png "Durchlässiges und undurchsichtiges Tabstopp")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
