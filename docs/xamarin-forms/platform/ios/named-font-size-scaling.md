---
title: Barrierefreiheits Skalierung für benannte Schriftgrößen unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die die Barrierefreiheits Skalierung für benannte Schriftgrößen deaktiviert.
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 13ca4945ed447ae811ee95e308238f04e58ac0bd
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70200084"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Barrierefreiheits Skalierung für benannte Schriftgrößen unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische Deaktivierung deaktiviert die Barrierefreiheits Skalierung für benannte Schriftgrößen. Es ist in XAML verwendet, durch Festlegen der `Application.EnableAccessibilityScalingForNamedFontSizes` bindbare Eigenschaft `false`:

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

Die `Application.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Application.SetEnableAccessibilityScalingForNamedFontSizes` -Methode [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) im-Namespace wird verwendet, um benannte Schriftgrößen zu deaktivieren, die von den IOS-Barrierefreiheits Einstellungen skaliert werden. Außerdem kann die `Application.GetEnableAccessibilityScalingForNamedFontSizes` -Methode verwendet werden, um zurückzugeben, ob benannte Schriftgrößen durch die IOS-Barrierefreiheits Einstellungen skaliert werden.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
