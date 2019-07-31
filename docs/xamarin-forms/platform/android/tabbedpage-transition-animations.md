---
title: Seiten Übergangs Animationen auf tabbedpage unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element nutzen, das beim Navigieren durch Seiten in einer tabbedpage Übergangs Animationen deaktiviert.
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 4f8d6ec2b06855364970bc9b672c3d3f7b9bfdfc
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649840"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Seiten Übergangs Animationen auf tabbedpage unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische dient zum Deaktivieren von Übergangs Animationen beim Navigieren durch Seiten, entweder Programm gesteuert oder bei Verwendung der Registerkarten Leiste in [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)einem. Es ist in XAML verwendet, durch Festlegen der `TabbedPage.IsSmoothScrollEnabled` bindbare Eigenschaft `false`:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.IsSmoothScrollEnabled="false">
    ...
</TabbedPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetIsSmoothScrollEnabled(false);
```

Die `TabbedPage.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `TabbedPage.SetIsSmoothScrollEnabled` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob Übergangsanimationen angezeigt werden, wenn die Seiten im Navigieren zwischen einem [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Darüber hinaus die `TabbedPage` -Klasse in der `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` Namespace verfügt auch über die folgenden Methoden:

- `IsSmoothScrollEnabled`, womit abrufen können, ob Übergangsanimationen angezeigt werden, wenn die Seiten im Navigieren zwischen einem `TabbedPage`.
- `EnableSmoothScroll`, womit Übergangsanimationen aktivieren Sie beim Navigieren zwischen Seiten in einem `TabbedPage`.
- `DisableSmoothScroll`, womit Übergangsanimationen deaktivieren Sie beim Navigieren zwischen Seiten in einem `TabbedPage`.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
