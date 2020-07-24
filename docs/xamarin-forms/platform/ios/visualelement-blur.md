---
title: Bildschirm Abbildung von visualelement unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die weich auf ein visualelement anwendet.
ms.prod: xamarin
ms.assetid: 2DE3B65E-B96E-4ECD-92DF-AA42D5205C44
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 44504ed47ca5ea94ed22c9569f8674ca0e63d89a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937864"
---
# <a name="visualelement-blur-on-ios"></a>Bildschirm Abbildung von visualelement unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische IOS-Datei wird verwendet, um den untergeordneten Inhalt zu verwischen, und kann auf beliebige angewendet werden [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Sie wird in XAML verwendet, indem die [`VisualElement.BlurEffect`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`BlurEffectStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
      <Image Source="monkeyface.png" />
      <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

Die- `BoxView.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `VisualElement.UseBlurEffect` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. useblureffect ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement}, Xamarin.Forms . Platformconfiguration. iosspecific. blureffectstyle))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den Weichzeichnereffekt anzuwenden, wobei die- [`BlurEffectStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) Enumeration vier Werte bereitstellt: [`None`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None) , [`ExtraLight`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight) , [`Light`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light) und [`Dark`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark) .

Das Ergebnis ist, dass ein [`BlurEffectStyle`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) angegebenes auf die-Instanz angewendet wird, die den untergeordneten-Wert [`BoxView`](xref:Xamarin.Forms.BoxView) blziert [`Image`](xref:Xamarin.Forms.Image) :

![Weichzeichnereffekt plattformspezifisch](applying-blur-images/blur-effect.png)

> [!NOTE]
> Beim Hinzufügen eines Weichzeichnereffekts zu einem werden [`VisualElement`](xref:Xamarin.Forms.VisualElement) Berührungs Ereignisse weiterhin von empfangen `VisualElement` .

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
