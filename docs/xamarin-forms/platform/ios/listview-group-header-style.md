---
title: ListView-Gruppen Header Stil unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische IOS-Element nutzen, das steuert, ob ListView-Header Zellen beim Scrollen schweben.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 46e8dec3d5644defdeb8a2265a73815adfde92d8
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136033"
---
# <a name="listview-group-header-style-on-ios"></a>ListView-Gruppen Header Stil unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen Steuerelement wird gesteuert, ob [`ListView`](xref:Xamarin.Forms.ListView) Kopfzeilen Zellen beim Scrollen schweben Sie wird in XAML verwendet, indem die `ListView.GroupHeaderStyle` bindbare Eigenschaft auf einen Wert der- `GroupHeaderStyle` Enumeration festgelegt wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.GroupHeaderStyle="Grouped">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

Die- `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `ListView.SetGroupHeaderStyle` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob die [`ListView`](xref:Xamarin.Forms.ListView) Header Zellen beim Scrollen schweben. Die- `GroupHeaderStyle` Enumeration bietet zwei mögliche Werte:

- `Plain`– Gibt an, dass die Header Zellen beim [`ListView`](xref:Xamarin.Forms.ListView) scrollt (Standard) in den Gleit Komma Wert gesetzt werden.
- `Grouped`– Gibt an, dass Header Zellen beim Rollup von nicht float werden [`ListView`](xref:Xamarin.Forms.ListView) .

Außerdem `ListView.GetGroupHeaderStyle` kann die-Methode verwendet werden, um den zurückzugeben `GroupHeaderStyle` , der auf den angewendet wird [`ListView`](xref:Xamarin.Forms.ListView) .

Das Ergebnis ist, dass ein `GroupHeaderStyle` angegebener Wert auf den angewendet wird [`ListView`](xref:Xamarin.Forms.ListView) , der steuert, ob die Header Zellen beim Scrollen schweben:

[![Screenshot von Gleit Komma-und nicht unverankerten ListView-Header Zellen unter IOS](listview-group-header-style-images/group-header-styles.png "ListView mit Gleit Komma-und nicht-Gleit Komma Zellen")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView mit Gleit Komma-und nicht-Gleit Komma Zellen")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
