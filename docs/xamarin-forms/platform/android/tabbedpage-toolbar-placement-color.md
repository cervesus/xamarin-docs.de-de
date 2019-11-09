---
title: Tabbedpage-Symbolleisten Platzierung und-Farbe unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element nutzen, das die Platzierung und Farbe der Symbolleiste auf einer tabbedpage festlegt.
ms.prod: xamarin
ms.assetid: A5C68D6A-9A5F-42EE-845D-1E5B0CB1544E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: b95b73759d44631da0525fce16218b8a87ca0507
ms.sourcegitcommit: efbc69acf4ea484d8815311b058114379c9db8a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73842840"
---
# <a name="tabbedpage-toolbar-placement-and-color-on-android"></a>Tabbedpage-Symbolleisten Platzierung und-Farbe unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

> [!IMPORTANT]
> Die Platt Form Besonderheiten, die die Farbe der Symbolleiste auf einem [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) festlegen, sind mittlerweile veraltet und wurden durch die Eigenschaften [`SelectedTabColor`](xref:Xamarin.Forms.TabbedPage.SelectedTabColor) und [`UnselectedTabColor`](xref:Xamarin.Forms.TabbedPage.UnselectedTabColor) ersetzt. Weitere Informationen finden Sie unter [Erstellen einer tabbedpage](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md#create-a-tabbedpage).

Diese Platt Form Besonderheiten werden verwendet, um die Platzierung und Farbe der Symbolleiste auf einem [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)festzulegen. Sie werden in XAML verwendet, indem die [`TabbedPage.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) angefügte-Eigenschaft auf einen Wert der [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) -Enumeration festgelegt wird, und die [`TabbedPage.BarItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) und [`TabbedPage.BarSelectedItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) angefügten Eigenschaften einer [`Color`](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Alternativ können Sie von C# der Verwendung der fließenden API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

Die `TabbedPage.On<Android>`-Methode gibt an, dass diese Platt Form Besonderheiten nur unter Android ausgeführt werden. Die [`TabbedPage.SetToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)) -Methode im [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Symbolleisten Platzierung auf einem [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)festzulegen, wobei die [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) -Enumeration die folgenden Werte bereitstellt:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default) – gibt an, dass die Symbolleiste am Standard Speicherort auf der Seite platziert wird. Dies ist der obere Teil der Seite auf Smartphones und der untere Teil der Seite auf der anderen Geräte Idiome.
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top) – gibt an, dass sich die Symbolleiste oben auf der Seite befindet.
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom) – gibt an, dass sich die Symbolleiste am unteren Rand der Seite befindet.

Außerdem werden die Methoden [`TabbedPage.SetBarItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) und [`TabbedPage.SetBarSelectedItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) verwendet, um die Farbe der Symbolleisten Elemente bzw. der ausgewählten Symbolleisten Elemente festzulegen.

> [!NOTE]
> Die Methoden [`GetToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), [`GetBarItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage}))und [`GetBarSelectedItemColor`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) können verwendet werden, um die Platzierung und Farbe der [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) Symbolleiste abzurufen.

Das Ergebnis ist, dass die Symbolleisten Platzierung, die Farbe der Symbolleisten Elemente und die Farbe des ausgewählten Symbolleisten Elements für eine [`TabbedPage`](xref:Xamarin.Forms.TabbedPage)festgelegt werden können:

![](tabbedpage-toolbar-placement-color-images/tabbedpage-toolbar-placement.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
