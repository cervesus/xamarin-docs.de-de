---
title: Searchbar-Stil unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische IOS-Element nutzen, das steuert, ob eine Suchleiste einen Hintergrund hat.
ms.prod: xamarin
ms.assetid: 3D512DD6-078E-4BC6-926E-62BA6F4DE640
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/05/2020
ms.openlocfilehash: 7d95a90e96f868b6d8368054659f978555bc28ac
ms.sourcegitcommit: 8d13d2262d02468c99c4e18207d50cd82275d233
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "82532952"
---
# <a name="searchbar-style-on-ios"></a>Searchbar-Stil unter IOS

[![Beispiel](~/media/shared/download.png) herunterladen herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen Steuer [`SearchBar`](xref:Xamarin.Forms.SearchBar) Element wird gesteuert, ob ein einen Hintergrund hat. Sie wird in XAML verwendet, indem die `SearchBar.SearchBarStyle` bindbare Eigenschaft auf einen Wert der- `UISearchBarStyle` Enumeration festgelegt wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ios:SearchBar.SearchBarStyle="Minimal"
                   Placeholder="Enter search term" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SearchBar searchBar = new SearchBar { Placeholder = "Enter search term" };
searchBar.On<iOS>().SetSearchBarStyle(UISearchBarStyle.Minimal);
```

Die `SearchBar.On<iOS>` -Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `SearchBar.SetSearchBarStyle` -Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, [`SearchBar`](xref:Xamarin.Forms.SearchBar) ob die über einen Hintergrund verfügt. Die `UISearchBarStyle` -Enumeration bietet drei mögliche Werte:

- `Default`Gibt an, [`SearchBar`](xref:Xamarin.Forms.SearchBar) dass die den Standardstil hat. Dies ist der Standardwert der `SearchBar.SearchBarStyle` bindbare Eigenschaft.
- `Prominent`Gibt an, [`SearchBar`](xref:Xamarin.Forms.SearchBar) dass das-Feld über einen durchlässigen Hintergrund verfügt und das Suchfeld nicht transparent ist.
- `Minimal`Gibt an, [`SearchBar`](xref:Xamarin.Forms.SearchBar) dass das-Feld keinen Hintergrund hat und das Suchfeld durchlässig ist.

Außerdem kann die `SearchBar.GetSearchBarStyle` -Methode verwendet werden, um den `UISearchBarStyle` zurückzugeben, der auf den `SearchBar`angewendet wird.

Das Ergebnis ist, dass ein `UISearchBarStyle` angegebenes Element auf [`SearchBar`](xref:Xamarin.Forms.SearchBar)einen angewendet wird, der `SearchBar` steuert, ob die über einen Hintergrund verfügt:

![Screenshot der Suchleisten Stile unter IOS](searchbar-style-images/searchbar-styles.png "Searchbar-Stile unter IOS")

Die folgenden Screenshots zeigen die `UISearchBarStyle` Elemente, die [`SearchBar`](xref:Xamarin.Forms.SearchBar) auf Objekte angewendet werden `BackgroundColor` , deren-Eigenschaft festgelegt ist:

![Screenshot der Searchbar-Stile mit Hintergrundfarbe unter IOS](searchbar-style-images/searchbar-background-styles.png "Searchbar-Stile mit Hintergrundfarbe unter IOS")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
