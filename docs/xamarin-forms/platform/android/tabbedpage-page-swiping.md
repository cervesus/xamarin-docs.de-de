---
title: Tabbedpage Page schwenken unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element nutzen, das das Schwenken mit einer horizontalen Fingerbewegung zwischen Seiten in einer tabbedpage ermöglicht.
ms.prod: xamarin
ms.assetid: D1C09CCB-7246-41A4-8BD2-FA6FABCF1C72
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: 0043d90e631c19a55b766a877a9cd30316f14650
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997379"
---
# <a name="tabbedpage-page-swiping-on-android"></a>Tabbedpage Page schwenken unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische wird verwendet, um das Schwenken mit einer horizontalen Fingerbewegung zwischen Seiten in einem zu ermöglichen [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) . Sie wird in XAML verwendet, indem die [`TabbedPage.IsSwipePagingEnabled`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

Die- `TabbedPage.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. [ `TabbedPage.SetIsSwipePagingEnabled` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. * tisswipeer pagingenabled ( :::no-loc(Xamarin.Forms)::: . Die bindableobject, System. Boolean)-Methode im- [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um das Schwenken zwischen Seiten in einem zu aktivieren [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) . Außerdem verfügt die- `TabbedPage` Klasse im- `:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific` Namespace ebenfalls über ein [ `EnableSwipePaging` ] (Xref:) :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. enableswipepaging ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage}))-Methode, die diese plattformspezifische Methode aktiviert, und a [ `DisableSwipePaging` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. disableswipepaging ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage}))-Methode, die diese plattformspezifische deaktiviert. Die [`TabbedPage.OffscreenPageLimit`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty) angefügte-Eigenschaft und [ `SetOffscreenPageLimit` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. *-ffscreenpagelimit ( :::no-loc(Xamarin.Forms)::: . Bindableobject, System. Int32))-Methode verwendet wird, um die Anzahl der Seiten festzulegen, die auf beiden Seiten der aktuellen Seite im Leerlaufzustand aufbewahrt werden sollen.

Das Ergebnis ist, dass das Auslagern über die Seiten, die von einem angezeigt werden, [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) aktiviert ist:

![Schwenken von Paging über eine tabbedpage](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
