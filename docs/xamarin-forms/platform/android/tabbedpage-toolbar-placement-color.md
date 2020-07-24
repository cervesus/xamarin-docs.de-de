---
title: Tabbedpage-Symbolleisten Platzierung und-Farbe unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element nutzen, das die Platzierung und Farbe der Symbolleiste auf einer tabbedpage festlegt.
ms.prod: xamarin
ms.assetid: A5C68D6A-9A5F-42EE-845D-1E5B0CB1544E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- ':::no-loc(Xamarin.Forms):::'
- ':::no-loc(Xamarin.Essentials):::'
ms.openlocfilehash: e32c516c4a0a8b12bd8a76478557905149890c6a
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86997331"
---
# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>Tabbedpage-Symbolleisten Platzierung und-Farbe unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> Die Platt Form Besonderheiten, die die Farbe der Symbolleiste auf einem festlegen, [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) sind mittlerweile veraltet und wurden durch die [`SelectedTabColor`](xref::::no-loc(Xamarin.Forms):::.TabbedPage.SelectedTabColor) Eigenschaften und ersetzt [`UnselectedTabColor`](xref::::no-loc(Xamarin.Forms):::.TabbedPage.UnselectedTabColor) . Weitere Informationen finden Sie unter [Erstellen einer tabbedpage](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage).

Diese Platt Form Besonderheiten werden verwendet, um die Platzierung und die Farbe der Symbolleiste auf einem festzulegen [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) . Sie werden in XAML durch Festlegen der [`TabbedPage.ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) angefügten-Eigenschaft auf einen Wert der [`ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) -Enumeration sowie der [`TabbedPage.BarItemColor`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) angefügten-Eigenschaft und der-Eigenschaft [`TabbedPage.BarSelectedItemColor`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) an einen verwendet [`Color`](xref::::no-loc(Xamarin.Forms):::.Color) :

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;assembly=:::no-loc(Xamarin.Forms):::.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Alternativ können Sie mithilfe der fließend-API von c# genutzt werden:

```csharp
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration;
using :::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

Die `TabbedPage.On<Android>` -Methode gibt an, dass diese Platt Form Besonderheiten nur unter Android ausgeführt werden. [ `TabbedPage.SetToolbarPlacement` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. settoolbarplacement ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage}, :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. toolbarplacement))-Methode im- [`:::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um die Symbolleisten Platzierung für ein festzulegen [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) , wobei die- [`ToolbarPlacement`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) Enumeration die folgenden Werte bereitstellt:

- [`Default`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default)– Gibt an, dass die Symbolleiste am Standard Speicherort auf der Seite platziert wird. Dies ist der obere Teil der Seite auf Smartphones und der untere Teil der Seite auf der anderen Geräte Idiome.
- [`Top`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top)– Gibt an, dass sich die Symbolleiste oben auf der Seite befindet.
- [`Bottom`](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom)– Gibt an, dass sich die Symbolleiste am unteren Rand der Seite befindet.

Außerdem ist [ `TabbedPage.SetBarItemColor` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. setbaritemcolor ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage}, :::no-loc(Xamarin.Forms)::: . Color)) und [ `TabbedPage.SetBarSelectedItemColor` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. setbarselecteditemcolor ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage}, :::no-loc(Xamarin.Forms)::: . Color))-Methoden werden verwendet, um die Farbe der Symbolleisten Elemente bzw. der ausgewählten Symbolleisten Elemente festzulegen.

> [!NOTE]
> [ `GetToolbarPlacement` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. gettoolbarplacement ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage})), [ `GetBarItemColor` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. getbaritemcolor ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage})) und [ `GetBarSelectedItemColor` ] (Xref: :::no-loc(Xamarin.Forms)::: . Platformconfiguration. androidspecific. tabbedpage. getbarselecteditemcolor ( :::no-loc(Xamarin.Forms)::: . Iplatformelementconfiguration { :::no-loc(Xamarin.Forms)::: . Platformconfiguration. Android, :::no-loc(Xamarin.Forms)::: . Tabbedpage}))-Methoden können verwendet werden, um die Platzierung und die Farbe der [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) Symbolleiste abzurufen.

Das Ergebnis ist, dass die Symbolleisten Platzierung, die Farbe der Symbolleisten Elemente und die Farbe des ausgewählten Symbolleisten Elements für einen festgelegt werden können [`TabbedPage`](xref::::no-loc(Xamarin.Forms):::.TabbedPage) :

![Konfiguration der tabbedpage-Symbolleiste](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref::::no-loc(Xamarin.Forms):::.PlatformConfiguration.AndroidSpecific.AppCompat)
