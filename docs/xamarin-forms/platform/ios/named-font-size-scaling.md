---
Title: "Barrierefreiheits Skalierung für benannte Schriftgrößen unter IOS" Description: "Platform-Besonderheiten" ermöglicht Ihnen die Nutzung von Funktionen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die die Barrierefreiheits Skalierung für benannte Schriftgrößen deaktiviert. "
ms. Prod: xamarin ms. assetid: B443BAF6-E6F6-4A0E-80B5-CAACE6B550EF ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 06/28/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="accessibility-scaling-for-named-font-sizes-on-ios"></a>Barrierefreiheits Skalierung für benannte Schriftgrößen unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische Deaktivierung deaktiviert die Barrierefreiheits Skalierung für benannte Schriftgrößen. Sie wird in XAML verwendet, indem die `Application.EnableAccessibilityScalingForNamedFontSizes` bindbare Eigenschaft auf festgelegt wird `false` :

```xaml
<Application ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Application.EnableAccessibilityScalingForNamedFontSizes="false">
    ...
</Application>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

Xamarin.Forms.Application.Current.On<iOS>().SetEnableAccessibilityScalingForNamedFontSizes(false);
```

Die- `Application.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `Application.SetEnableAccessibilityScalingForNamedFontSizes` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um benannte Schriftgrößen zu deaktivieren, die von den IOS-Barrierefreiheits Einstellungen skaliert werden. Außerdem kann die- `Application.GetEnableAccessibilityScalingForNamedFontSizes` Methode verwendet werden, um zurückzugeben, ob benannte Schriftgrößen durch die IOS-Barrierefreiheits Einstellungen skaliert werden.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
