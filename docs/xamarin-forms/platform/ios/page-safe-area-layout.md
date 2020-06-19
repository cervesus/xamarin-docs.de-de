---
title: Handbuch zum Safe Area Layout unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, um sicherzustellen, dass Seiteninhalte in einem Bereich des Bildschirms positioniert sind, der für alle Geräte mit IOS 11 und höher sicher ist.
ms.prod: xamarin
ms.assetid: 2B6789C1-39B4-4C16-ADE1-3ED3378EAC63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5ca30481fbc0e5631ff75000c688dd805793e670
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128059"
---
# <a name="safe-area-layout-guide-on-ios"></a>Handbuch zum Safe Area Layout unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische wird verwendet, um sicherzustellen, dass der Seiten Inhalt in einem Bereich des Bildschirms positioniert ist, der für alle Geräte, die IOS 11 und höher verwenden, sicher ist. Insbesondere wird sichergestellt, dass der Inhalt nicht von abgerundeten Geräte Ecken, dem Start Indikator oder dem Sensorgehäuse auf einem iPhone X abgeschnitten wird. Sie wird in XAML verwendet, indem die `Page.UseSafeArea` angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

Die- `Page.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Mit der- `Page.SetUseSafeArea` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird gesteuert, ob das Handbuch für den sicheren Bereichs Layout aktiviert ist.

Das Ergebnis ist, dass der Seiten Inhalt in einem Bereich des Bildschirms positioniert werden kann, der für alle iPhones sicher ist:

[![](page-safe-area-images/safe-area-layout.png "Safe Area Layout Guide")](page-safe-area-images/safe-area-layout-large.png#lightbox "Safe Area Layout Guide")

> [!NOTE]
> Der von Apple definierte sichere Bereich wird in verwendet Xamarin.Forms , um die [`Page.Padding`](xref:Xamarin.Forms.Page.Padding) -Eigenschaft festzulegen, und überschreibt alle vorherigen Werte dieser Eigenschaft, die festgelegt wurden.

Der sichere Bereich kann angepasst werden, indem der [`Thickness`](xref:Xamarin.Forms.Thickness) Wert mit der- `Page.SafeAreaInsets` Methode aus dem- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace abgerufen wird. Sie kann dann nach Bedarf geändert und der- `Padding` Eigenschaft im Seitenkonstruktor erneut zugewiesen werden oder [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing) außer Kraft gesetzt werden:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
