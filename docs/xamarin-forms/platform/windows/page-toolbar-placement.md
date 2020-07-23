---
title: Platzierung der Seiten Symbolleiste unter Windows
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die Windows-plattformspezifische verwenden, mit der die Platzierung einer Symbolleiste auf einer Seite geändert wird.
ms.prod: xamarin
ms.assetid: 99F29E95-0C36-4A3B-BDE8-7E9F119E844E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0c2bbc89f503cfbaad24d8f1ac5d0635c1c22e48
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937812"
---
# <a name="page-toolbar-placement-on-windows"></a>Platzierung der Seiten Symbolleiste unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch wird verwendet, um die Platzierung einer Symbolleiste in einem zu ändern [`Page`](xref:Xamarin.Forms.Page) , und wird in XAML verwendet, indem die [`Page.ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) :

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

Die- `Page.On<Windows>` Methode gibt an, dass diese plattformspezifische nur unter Windows ausgeführt wird. [ `Page.SetToolbarPlacement` ] (Xref: Xamarin.Forms . Platformconfiguration. windowsspecific. Page. settoolbarplacement ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Windows, Xamarin.Forms . Seite}, Xamarin.Forms . Platformconfiguration. windowsspecific. toolbarplacement))-Methode im- [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace wird zum Festlegen der Symbolleisten Platzierung verwendet, wobei die- [`ToolbarPlacement`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement) Enumeration drei Werte bereitstellt: [`Default`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default) , [`Top`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top) und [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom) .

Das Ergebnis ist, dass die angegebene Symbolleisten Platzierung auf die [`Page`](xref:Xamarin.Forms.Page) Instanz angewendet wird:

[![Plattformspezifische Symbolleisten Platzierung](page-toolbar-placement-images/toolbar-placement.png)](page-toolbar-placement-images/toolbar-placement-large.png#lightbox "Plattformspezifische Symbolleisten Platzierung")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
