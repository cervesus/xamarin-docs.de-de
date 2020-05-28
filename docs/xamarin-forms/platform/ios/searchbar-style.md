---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e02ef600af761915d05c912b586e409dd6f46b85
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137084"
---
# <a name="searchbar-style-on-ios"></a>Searchbar-Stil unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem IOS-plattformspezifischen Steuerelement wird gesteuert, ob ein einen [`SearchBar`](xref:Xamarin.Forms.SearchBar) Hintergrund hat. Sie wird in XAML verwendet, indem die `SearchBar.SearchBarStyle` bindbare Eigenschaft auf einen Wert der- `UISearchBarStyle` Enumeration festgelegt wird:

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

Die- `SearchBar.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die- `SearchBar.SetSearchBarStyle` Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um zu steuern, ob die [`SearchBar`](xref:Xamarin.Forms.SearchBar) über einen Hintergrund verfügt. Die- `UISearchBarStyle` Enumeration bietet drei mögliche Werte:

- `Default`Gibt an, dass die [`SearchBar`](xref:Xamarin.Forms.SearchBar) den Standardstil hat. Dies ist der Standardwert der `SearchBar.SearchBarStyle` bindbare Eigenschaft.
- `Prominent`Gibt an, dass das [`SearchBar`](xref:Xamarin.Forms.SearchBar) -Feld über einen durchlässigen Hintergrund verfügt und das Suchfeld nicht transparent ist.
- `Minimal`Gibt an, dass das [`SearchBar`](xref:Xamarin.Forms.SearchBar) -Feld keinen Hintergrund hat und das Suchfeld durchlässig ist.

Außerdem `SearchBar.GetSearchBarStyle` kann die-Methode verwendet werden, um den zurückzugeben `UISearchBarStyle` , der auf den angewendet wird `SearchBar` .

Das Ergebnis ist, dass ein angegebenes Element `UISearchBarStyle` auf einen angewendet wird [`SearchBar`](xref:Xamarin.Forms.SearchBar) , der steuert, ob die `SearchBar` über einen Hintergrund verfügt:

![Screenshot der Suchleisten Stile unter IOS](searchbar-style-images/searchbar-styles.png "Searchbar-Stile unter IOS")

Die folgenden Screenshots zeigen die Elemente, die `UISearchBarStyle` auf Objekte angewendet werden [`SearchBar`](xref:Xamarin.Forms.SearchBar) , deren- `BackgroundColor` Eigenschaft festgelegt ist:

![Screenshot der Searchbar-Stile mit Hintergrundfarbe unter IOS](searchbar-style-images/searchbar-background-styles.png "Searchbar-Stile mit Hintergrundfarbe unter IOS")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
