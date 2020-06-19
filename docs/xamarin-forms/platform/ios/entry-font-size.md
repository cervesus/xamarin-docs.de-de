---
title: Eingabe Schriftgröße unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, die den Schrift Grad eines Eintrags skaliert.
ms.prod: xamarin
ms.assetid: E8881D4E-902B-4397-A43E-916B2885EC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 57498811d8789d8ef9ef775f8f39f141b77659c8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84138527"
---
# <a name="entry-font-size-on-ios"></a>Eingabe Schriftgröße unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische IOS-Datei wird verwendet, um den Schrift Grad eines zu skalieren [`Entry`](xref:Xamarin.Forms.Entry) , um sicherzustellen, dass der eingeputzte Text in das Steuerelement passt. Sie wird in XAML verwendet, indem die [`Entry.AdjustsFontSizeToFitWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty) angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

Die- `Entry.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `Entry.EnableAdjustsFontSizeToFitWidth` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. entry. enableder sfontsizedeftwidth ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den Schrift Grad des eingegebenen Texts zu skalieren, um sicherzustellen, dass er in passt [`Entry`](xref:Xamarin.Forms.Entry) . Außerdem verfügt die- [`Entry`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry) Klasse im- `Xamarin.Forms.PlatformConfiguration.iOSSpecific` Namespace ebenfalls über ein [ `DisableAdjustsFontSizeToFitWidth` ] (Xref:) Xamarin.Forms . Platformconfiguration. iosspecific. entry. disableder sfontsizedeftwidth ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}))-Methode, die diese plattformspezifische und eine [ `SetAdjustsFontSizeToFitWidth` ] (Xref:) deaktiviert Xamarin.Forms . Platformconfiguration. iosspecific. entry. setanpassungen sfontsizedeftwidth ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}, System. Boolean)-Methode, die verwendet werden kann, um die Skalierung der Schriftgröße durch Aufrufen von [ `AdjustsFontSizeToFitWidth` ] (Xref:) zu umschalten Xamarin.Forms . Platformconfiguration. iosspecific. entry. ". Xamarin.Forms ". Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Entry}))-Methode:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Das Ergebnis ist, dass der Schrift Grad der [`Entry`](xref:Xamarin.Forms.Entry) skaliert wird, um sicherzustellen, dass der eingeputzte Text in das Steuerelement passt:

![](entry-font-size-images/entry-font-size.png "Adjust Entry Font Size Platform-Specific")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
