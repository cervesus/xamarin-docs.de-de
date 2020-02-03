---
title: " Searchbar-Rechtschreibprüfung unter Windows"
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das Windows-plattformspezifische verwenden können, das es einer Suchleiste ermöglicht, mit dem Rechtschreib Prüfungs Modul zu interagieren.
ms.prod: xamarin
ms.assetid: 7974C91F-7502-4DB3-B0E9-C45E943DDA26
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: da75d42e0e0bcf38dd2ff50999b705125dc4e2b5
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76723612"
---
# <a name="searchbar-spell-check-on-windows"></a>Searchbar-Rechtschreibprüfung unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch ermöglicht einer [`SearchBar`](xref:Xamarin.Forms.SearchBar) die Interaktion mit der Rechtschreibprüfung. Sie wird in XAML verwendet, indem die [`SearchBar.IsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) angefügte-Eigenschaft auf einen `boolean` Wert festgelegt wird:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

Die `SearchBar.On<Windows>`-Methode gibt an, dass diese plattformspezifische nur auf der universelle Windows-Plattform ausgeführt wird. Die [`SearchBar.SetIsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) -Methode im [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace schaltet die Rechtschreibprüfung ein und aus. Außerdem kann die `SearchBar.SetIsSpellCheckEnabled`-Methode zum Umschalten der Rechtschreibprüfung verwendet werden, indem die [`SearchBar.GetIsSpellCheckEnabled`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) -Methode aufgerufen wird, um zurückzugeben, ob die Rechtschreibprüfung aktiviert ist:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Das Ergebnis ist, dass Text, der in die [`SearchBar`](xref:Xamarin.Forms.SearchBar) eingegeben werden kann, mit Rechtschreibprüfung versehen werden kann, wobei der Benutzer falsche Rechtschreib Werte anzeigt:

![Plattformspezifische Überprüfung der Suchleiste](searchbar-spell-check-images/searchbar-spellcheck.png "Plattformspezifische Überprüfung der Suchleiste")

> [!NOTE]
> Die `SearchBar`-Klasse im [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace verfügt auch über [`EnableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) und [`DisableSpellCheck`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) Methoden, die verwendet werden können, um die Rechtschreibprüfung für den `SearchBar`zu aktivieren bzw. zu deaktivieren.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
