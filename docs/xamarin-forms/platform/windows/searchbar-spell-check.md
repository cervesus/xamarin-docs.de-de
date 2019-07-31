---
title: " Searchbar-Rechtschreibprüfung unter Windows"
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das Windows-plattformspezifische verwenden können, das es einer Suchleiste ermöglicht, mit dem Rechtschreib Prüfungs Modul zu interagieren.
ms.prod: xamarin
ms.assetid: 7974C91F-7502-4DB3-B0E9-C45E943DDA26
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 863748a7af057d2ea3e53719c332cd555ff3b698
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656859"
---
# <a name="searchbar-spell-check-on-windows"></a>Searchbar-Rechtschreibprüfung unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese universelle Windows-Plattform plattformspezifisch ermöglicht einem [`SearchBar`](xref:Xamarin.Forms.SearchBar) die Interaktion mit der Rechtschreibprüfung. Es ist in XAML verwendet, durch Festlegen der [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

Die `SearchBar.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird die Rechtschreibprüfung aktiviert und deaktiviert. Darüber hinaus die `SearchBar.SetIsSpellCheckEnabled` Methode kann verwendet werden, um die Rechtschreibprüfung zu wechseln, durch den Aufruf der [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) Methode zurückgeben soll, ob die Rechtschreibprüfung aktiviert ist:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Das Ergebnis ist, den in eingegebenen Text die [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) kann Rechtschreibung überprüft werden, mit der falsche Schreibweisen für den Benutzer angegeben wird:

![SearchBar Rechtschreibung prüfen plattformspezifische](searchbar-spell-check-images/searchbar-spellcheck.png "SearchBar Rechtschreibung prüfen plattformspezifische")

> [!NOTE]
> Die `SearchBar` -Klasse in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace verfügt auch über [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) und [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) Methoden, die zum Aktivieren und deaktivieren verwendet werden können die Rechtschreibprüfung für die `SearchBar`bzw.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
