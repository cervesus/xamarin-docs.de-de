---
title: Barrierefreiheit-Skalierung für mit dem Namen Schriftgrade Ihren Bedürfnissen entsprechend unter iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die plattformspezifischen iOS nutzen, der Zugriff für benannte Schriftgrade Ihren Bedürfnissen entsprechend skalieren deaktiviert wird.
ms.prod: xamarin
ms.assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/28/2019
ms.openlocfilehash: 1fc6fefe1f9fe48fe2abb367b376a5a6f3484462
ms.sourcegitcommit: 0fd04ea3af7d6a6d6086525306523a5296eec0df
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67517949"
---
# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Barrierefreiheit-Skalierung für mit dem Namen Schriftgrade Ihren Bedürfnissen entsprechend unter iOS

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Dieses iOS-Plattform-spezifische deaktiviert die Barrierefreiheit für benannte Schriftgrade Ihren Bedürfnissen entsprechend skalieren. Es ist in XAML verwendet, durch Festlegen der `Application.EnableAccessibilityScalingForNamedFontSizes` bindbare Eigenschaft `false`:

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

Die `Application.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Application.SetEnableAccessibilityScalingForNamedFontSizes` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um benannte Schriftgrade Ihren Bedürfnissen entsprechend skaliert wird, durch die Einstellungen für iOS die Barrierefreiheit deaktivieren. Darüber hinaus die `Application.GetEnableAccessibilityScalingForNamedFontSizes` Methode kann verwendet werden, um zurück, ob die von iOS-Einstellungen für die Barrierefreiheit benannte Schriftgrade Ihren Bedürfnissen entsprechend skaliert werden.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
