---
title: ListView-Gruppen Header Stil unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische IOS-Element nutzen, das steuert, ob ListView-Header Zellen beim Scrollen schweben.
ms.prod: xamarin
ms.assetid: 099B2C7F-727F-4BCF-903B-87E728108C24
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: f40737799f63c6e0c61fcc6f4f59584222a49d6d
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68648324"
---
# <a name="listview-group-header-style-on-ios"></a>ListView-Gruppen Header Stil unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen Steuerelement wird gesteuert, ob beim Scrollen [`ListView`](xref:Xamarin.Forms.ListView) Header Zellen schweben. Sie wird in XAML verwendet, indem die `ListView.GroupHeaderStyle` bindbare-Eigenschaft auf einen Wert der `GroupHeaderStyle`-Enumeration festgelegt wird:

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

Alternativ kann Sie von C# der Verwendung der flüssigen API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetGroupHeaderStyle(GroupHeaderStyle.Grouped);
```

Die `ListView.On<iOS>`-Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `ListView.SetGroupHeaderStyle`-Methode im [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um zu steuern, ob beim Scrollen [`ListView`](xref:Xamarin.Forms.ListView) Header Zellen schweben. Die `GroupHeaderStyle`-Enumeration bietet zwei mögliche Werte:

- `Plain` – gibt an, dass die Header Zellen beim Rollup der [`ListView`](xref:Xamarin.Forms.ListView) (Standardeinstellung) float werden.
- `Grouped` – gibt an, dass Header Zellen bei einem Rollup des [`ListView`](xref:Xamarin.Forms.ListView) nicht float werden.

Außerdem kann die `ListView.GetGroupHeaderStyle`-Methode verwendet werden, um die `GroupHeaderStyle` zurückzugeben, die auf die [`ListView`](xref:Xamarin.Forms.ListView)angewendet wird.

Das Ergebnis ist, dass ein angegebener `GroupHeaderStyle` Wert auf den [`ListView`](xref:Xamarin.Forms.ListView)angewendet wird, der steuert, ob die Header Zellen beim Scrollen schweben:

[![Screenshot von Gleit Komma-und nicht unverankerten ListView-Header Zellen unter IOS](listview-group-header-style-images/group-header-styles.png "ListView mit Gleit Komma-und nicht-Gleit Komma Zellen")](listview-group-header-style-images/group-header-styles-large.png#lightbox "ListView mit Gleit Komma-und nicht-Gleit Komma Zellen")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
