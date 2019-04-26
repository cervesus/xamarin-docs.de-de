---
title: Schieberegler Thumb-Steuerelement zu tippen, unter iOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel erläutert das iOS-Plattform-spezifische nutzen, die mit der-Eigenschaft Slider.Value durch Tippen auf den Schieberegler festgelegt werden können.
ms.prod: xamarin
ms.assetid: D0915D37-9A59-4728-BB6A-FE094A661275
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 43cc87f9d319295ce65d55488e1be032ae00a697
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61082192"
---
# <a name="slider-thumb-tap-on-ios"></a>Schieberegler Thumb-Steuerelement zu tippen, unter iOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Diese iOS-Plattform-spezifische ermöglicht die [ `Slider.Value` ](xref:Xamarin.Forms.Slider.Value) festzulegende Eigenschaft durch Tippen auf eine Position auf der [ `Slider` ](xref:Xamarin.Forms.Slider) Balken-, anstatt dass ziehen die `Slider` Thumb-Steuerelement. Es ist in XAML verwendet, durch Festlegen der [ `Slider.UpdateOnTap` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Slider.UpdateOnTapProperty) bindbare Eigenschaft `true`:

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

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iOSSpecific API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
