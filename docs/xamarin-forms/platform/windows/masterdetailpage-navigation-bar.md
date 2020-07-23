---
title: Masterdetailpage-Navigationsleiste unter Windows
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die Windows-plattformspezifische verwenden, die die Navigationsleiste auf einer masterdetailpage reduziert.
ms.prod: xamarin
ms.assetid: 0E7436C9-FA3E-40CD-801C-3F7ED95C412D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 876e5449e17276efc3b368ab7d2c9e57365f948f
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937825"
---
# <a name="masterdetailpage-navigation-bar-on-windows"></a>Masterdetailpage-Navigationsleiste unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch wird verwendet, um die Navigationsleiste eines zu reduzieren [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) , und wird in XAML verwendet, indem [`MasterDetailPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) die [`MasterDetailPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty) angefügten Eigenschaften und festgelegt werden:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

Die- `MasterDetailPage.On<Windows>` Methode gibt an, dass diese plattformspezifische nur unter Windows ausgeführt wird. [ `Page.SetCollapseStyle` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. masterdetailpage. setredusestyle ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Masterdetailpage}, Xamarin.Forms . Platformconfiguration. windowsspecific. redulstyle))-Methode im- [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace wird verwendet, um das reduzierungsformat anzugeben, wobei die [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) -Enumeration zwei Werte bereitstellt: [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) und [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial) . [ `MasterDetailPage.CollapsedPaneWidth` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. masterdetailpage. redusedpanewidth ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Masterdetailpage}, System. Double))-Methode wird verwendet, um die Breite einer teilweise reduzierten Navigationsleiste anzugeben.

Das Ergebnis ist, dass eine angegebene [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) auf die Instanz angewendet wird [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) , wobei die Breite ebenfalls angegeben wird:

[![Plattformspezifisch reduzierte Navigationsleiste](masterdetailpage-navigation-bar-images/collapsed-navigation-bar.png)](masterdetailpage-navigation-bar-images/collapsed-navigation-bar-large.png#lightbox "Plattformspezifisch reduzierte Navigationsleiste")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
