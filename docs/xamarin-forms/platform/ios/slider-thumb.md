---
title: Ziehpunkt für den Schieberegler unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung verwenden, die die Eigenschaft "Slider. Value" durch Tippen auf den Schieberegler festlegen kann.
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 573b68097724c976ce73b51e3b7ba21b52f7a776
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651781"
---
# <a name="slider-thumb-tap-on-ios"></a>Ziehpunkt für den Schieberegler unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische IOS-Anwendung [`Slider.Value`](xref:Xamarin.Forms.Slider.Value) ermöglicht die Festlegung der-Eigenschaft, indem Sie auf [`Slider`](xref:Xamarin.Forms.Slider) eine Position auf der Leiste tippt, anstatt den `Slider` Ziehpunkt ziehen zu müssen. Es ist in XAML verwendet, durch Festlegen der [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) bindbare Eigenschaft `true`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Slider ... ios:Slider.UpdateOnTap="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var slider = new Xamarin.Forms.Slider();
slider.On<iOS>().SetUpdateOnTap(true);
```

Die `Slider.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `Slider.SetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.SetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Kontrolle, ob ein Tippen auf die `Slider` Balken wird festgelegt. die [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) Diese Eigenschaft. Darüber hinaus die [ `Slider.GetUpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.GetUpdateOnTap(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Slider})) Methode kann verwendet werden, um zurück, ob ein Tippen auf die `Slider` Balken wird festgelegt. die `Slider.Value` Eigenschaft.

Das Ergebnis ist, tippen auf die [ `Slider` ](xref:Xamarin.Forms.Slider) Balken verschieben kann die `Slider` Ziehpunkt, und legen Sie die [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) Eigenschaft:

![](slider-thumb-images/slider-updateontap.png "Update der Schieberegler auf Tap aktiviert")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
