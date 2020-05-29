---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7410386e10f605fdeed452fe37755c1e48e6b9b9
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136993"
---
# <a name="visualelement-drop-shadows-on-ios"></a>Visualelement-Ablage Schatten auf IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische dient zum Aktivieren eines Schlag Schattens für einen [`VisualElement`](xref:Xamarin.Forms.VisualElement) . Sie wird in XAML verwendet, indem die [`VisualElement.IsShadowEnabled`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsShadowEnabledProperty) angefügte-Eigenschaft auf festgelegt `true` wird, sowie eine Reihe zusätzlicher optionaler angefügter Eigenschaften, mit denen der Ablage Schatten gesteuert wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <BoxView ...
                 ios:VisualElement.IsShadowEnabled="true"
                 ios:VisualElement.ShadowColor="Purple"
                 ios:VisualElement.ShadowOpacity="0.7"
                 ios:VisualElement.ShadowRadius="12">
            <ios:VisualElement.ShadowOffset>
                <Size>
                    <x:Arguments>
                        <x:Double>10</x:Double>
                        <x:Double>10</x:Double>
                    </x:Arguments>
                </Size>
            </ios:VisualElement.ShadowOffset>
         </BoxView>
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var boxView = new BoxView { Color = Color.Aqua, WidthRequest = 100, HeightRequest = 100 };
boxView.On<iOS>()
       .SetIsShadowEnabled(true)
       .SetShadowColor(Color.Purple)
       .SetShadowOffset(new Size(10,10))
       .SetShadowOpacity(0.7)
       .SetShadowRadius(12);
```

Die- `VisualElement.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `VisualElement.SetIsShadowEnabled` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. * tisshadowenabled ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement}, System. Boolean))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob ein Schlag Schatten für aktiviert ist `VisualElement` . Außerdem können die folgenden Methoden aufgerufen werden, um den Schlag Schatten zu steuern:

- [ `SetShadowColor` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. setShadowColor ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement}, Xamarin.Forms . Color)) – legt die Farbe des Schlag Schattens fest. Die Standardfarbe ist [`Color.Default`](xref:Xamarin.Forms.Color.Default*) .
- [ `SetShadowOffset` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. setshadowoffset ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement}, Xamarin.Forms . Größe)) – legt den Offset des Schlag Schattens fest. Der Offset ändert die Richtung, in die der Schatten umgewandelt wird, und wird als [`Size`](xref:Xamarin.Forms.Size) Wert angegeben. Die `Size` Struktur Werte werden in geräteunabhängigen Einheiten ausgedrückt, wobei der erste Wert die Distanz nach links (negativer Wert) oder rechts (positiver Wert) und der zweite Wert die Distanz oberhalb (negativer Wert) oder niedriger (positiver Wert) ist. Der Standardwert dieser Eigenschaft ist (0,0, 0,0), was dazu führt, dass der Schatten um jede Seite von umbrochen wird `VisualElement` .
- [ `SetShadowOpacity` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. setshadowopacity ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement}, System. Double)) – legt die Deckkraft des Schlag Schattens fest. der Wert liegt im Bereich von 0,0 (transparent) bis 1,0 (nicht transparent). Der Standardwert für die Deckkraft ist 0,5.
- [ `SetShadowRadius` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. setshadowradius ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement}, System. Double)) – legt den weichzeichenradius fest, der zum Rendering des Schlag Schattens verwendet wird. Der Standard RADIUS-Wert ist 10,0.

> [!NOTE]
> Der Zustand eines Ablage Schattens kann durch Aufrufen von [ `GetIsShadowEnabled` ] (Xref:) abgefragt werden Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. getisshadowenabled ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement})), [ `GetShadowColor` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. getShadowColor ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement})), [ `GetShadowOffset` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. getshadowoffset ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement})), [ `GetShadowOpacity` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. getshadowopacity ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement})) und [ `GetShadowRadius` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. visualelement. getshadowradius ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Visualelement}))-Methoden.

Das Ergebnis ist, dass ein Ablage Schatten für eine aktiviert werden kann [`VisualElement`](xref:Xamarin.Forms.VisualElement) :

![](drop-shadow-images/drop-shadow.png "Drop shadow enabled")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
