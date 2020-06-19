---
title: Ziehpunkt für den Schieberegler unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die die Eigenschaft "Slider. Value" durch Tippen auf den Schieberegler festlegen kann.
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 93b4dba3e8543bd2cc2a4f2187f617aae5daff77
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137071"
---
# <a name="slider-thumb-tap-on-ios"></a>Ziehpunkt für den Schieberegler unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische IOS-Anwendung ermöglicht die [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) Festlegung der-Eigenschaft, indem Sie auf eine Position auf der [`Slider`](xref:Xamarin.Forms.Slider) Leiste tippt, anstatt den Ziehpunkt ziehen zu müssen `Slider` . Sie wird in XAML verwendet, indem die [`Slider.UpdateOnTap`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) bindbare Eigenschaft auf festgelegt wird `true` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

Die- `Slider.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `Slider.SetUpdateOnTap` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. Slider. setupdateontap ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Slider}, System. Boolean))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace verwendet wird, um zu steuern, ob durch Tippen auf der `Slider` Leiste die-Eigenschaft festgelegt wird [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) . Außerdem ist [ `Slider.GetUpdateOnTap` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. Slider. getupdateontap ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Slider}))-Methode kann verwendet werden, um zurückzugeben, ob durch Tippen auf der `Slider` Leiste die-Eigenschaft festgelegt wird `Slider.Value` .

Das Ergebnis ist, dass durch Tippen auf der [`Slider`](xref:Xamarin.Forms.Slider) Leiste der Ziehpunkt verschoben `Slider` und die-Eigenschaft festgelegt werden kann [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) :

![](slider-thumb-images/slider-updateontap.png "Slider Update on Tap enabled")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
