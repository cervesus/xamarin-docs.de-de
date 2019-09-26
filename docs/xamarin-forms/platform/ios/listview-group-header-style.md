---
title: ListView-Gruppen Header Stil unter IOS
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das plattformspezifische IOS-Element nutzen, das steuert, ob ListView-Header Zellen beim Scrollen schweben.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f40737799f63c6e0c61fcc6f4f59584222a49d6d
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "68648324"
---
# <a name="listview-group-header-style-on-ios"></a>ListView-Gruppen Header Stil unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen [`ListView`](xref:Xamarin.Forms.ListView) Steuerelement wird gesteuert, ob Kopfzeilen Zellen beim Scrollen schweben Sie wird in XAML verwendet, indem die `ListView.GroupHeaderStyle` bindbare Eigenschaft auf einen Wert `GroupHeaderStyle` der-Enumeration festgelegt wird:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

Die `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `ListView.SetGroupHeaderStyle` -Methode [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) im-Namespace wird verwendet, um zu steuern [`ListView`](xref:Xamarin.Forms.ListView) , ob die Header Zellen beim Scrollen schweben. Die `GroupHeaderStyle` -Enumeration bietet zwei mögliche Werte:

- `Plain`– Gibt an, dass die Header Zellen [`ListView`](xref:Xamarin.Forms.ListView) beim scrollt (Standard) in den Gleit Komma Wert gesetzt werden.
- `Grouped`– Gibt an, dass Header Zellen beim [`ListView`](xref:Xamarin.Forms.ListView) Rollup von nicht float werden.

Außerdem kann die `ListView.GetGroupHeaderStyle` -Methode verwendet werden, um den `GroupHeaderStyle` zurückzugeben, der auf den [`ListView`](xref:Xamarin.Forms.ListView)angewendet wird.

Das Ergebnis ist, dass ein `GroupHeaderStyle` angegebener Wert auf [`ListView`](xref:Xamarin.Forms.ListView)den angewendet wird, der steuert, ob die Header Zellen beim Scrollen schweben:

[![Screenshot von Gleit Komma-und nicht unverankerten ListView-Header Zellen unter IOS](listview-group-header-style-images/group-header-styles.png "ListView mit Gleit Komma-und nicht-Gleit Komma Zellen")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView mit Gleit Komma-und nicht-Gleit Komma Zellen")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
