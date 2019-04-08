---
title: Seite "TabbedPage" Übergangsanimationen unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die Android-Plattform-spezifische genutzt, die Übergangsanimationen deaktiviert, wenn durch die Seiten in einer "tabbedpage" navigieren.
ms.prod: xamarin
ms.assetid: 2DB4EA6D-9CED-4137-BAB2-B20A457B1CA3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 1e9c46fe9535c313581d6d0053559a28aa327887
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209145"
---
# <a name="tabbedpage-page-transition-animations-on-android"></a>Seite "TabbedPage" Übergangsanimationen unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Dieses Android-Plattform-spezifische wird verwendet, um Übergangsanimationen zu deaktivieren, wenn Sie über Seiten entweder programmgesteuert navigieren oder bei Verwendung der Registerkartenleiste in einem [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Es ist in XAML verwendet, durch Festlegen der `TabbedPage.IsSmoothScrollEnabled` bindbare Eigenschaft `false`:

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

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
