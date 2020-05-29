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
ms.openlocfilehash: d3ae03ec6cbc3469422e6a2d57f186254e87f40c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140015"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Seiten Übergangs Animationen auf tabbedpage unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische dient zum Deaktivieren von Übergangs Animationen beim Navigieren durch Seiten, entweder Programm gesteuert oder bei Verwendung der Registerkarten Leiste in einem [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Sie wird in XAML verwendet, indem die `TabbedPage.IsSmoothScrollEnabled` bindbare Eigenschaft auf festgelegt wird `false` :

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

Die- `TabbedPage.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die- `TabbedPage.SetIsSmoothScrollEnabled` Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um zu steuern, ob Übergangs Animationen angezeigt werden, wenn Sie zwischen Seiten in einem navigieren [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Außerdem verfügt die- `TabbedPage` Klasse im- `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` Namespace auch über die folgenden Methoden:

- `IsSmoothScrollEnabled`, der verwendet wird, um abzurufen, ob Übergangs Animationen angezeigt werden, wenn Sie zwischen Seiten in einem navigieren `TabbedPage` .
- `EnableSmoothScroll`, der verwendet wird, um Übergangs Animationen beim Navigieren zwischen Seiten in einem zu aktivieren `TabbedPage` .
- `DisableSmoothScroll`, das verwendet wird, um Übergangs Animationen beim Navigieren zwischen Seiten in einem zu deaktivieren `TabbedPage` .

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
