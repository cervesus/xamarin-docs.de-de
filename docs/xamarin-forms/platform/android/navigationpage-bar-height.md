---
title: Höhe der Befehlsleiste der NavigationPage unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die Android-Plattform-spezifische nutzen, die die Höhe der Navigationsleiste auf einer "NavigationPage" festlegt wird.
ms.prod: xamarin
ms.assetid: C8A73B64-FE70-408A-A72E-8AF147F0C52C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: e803f138c0be85517bbafbfa6e9d39f39abc7e1d
ms.sourcegitcommit: 395774577f7524b57035c5cca3c9034a4b636489
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/10/2019
ms.locfileid: "54209155"
---
# <a name="navigationpage-bar-height-on-android"></a>Höhe der Befehlsleiste der NavigationPage unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Dieses Android-Plattform-spezifische legt die Höhe der Navigationsleiste auf einen [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Es ist in XAML verwendet, durch Festlegen der [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) bindbare Eigenschaft in einen ganzzahligen Wert:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

public class AndroidNavigationPageCS : Xamarin.Forms.NavigationPage
{
    public AndroidNavigationPageCS()
    {
        On<Android>().SetBarHeight(450);
    }
}
```

Die `NavigationPage.On<Android>` Methode angibt, dass diese plattformspezifischen nur auf die Anwendungskompatibilität Android ausgeführt wird. Die [ `NavigationPage.SetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.SetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage},System.Int32)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) -Namespace wird verwendet, um die Höhe der Navigationsleiste auf Festlegen einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Darüber hinaus die [ `NavigationPage.GetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.GetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage})) Methode kann verwendet werden, um die Höhe der Navigationsleiste in zurückzugeben der `NavigationPage`.

Das Ergebnis ist, die die Höhe der Navigationsleiste auf einen [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) kann festgelegt werden:

![](navigationpage-bar-height-images/navigationpage-barheight.png "\"NavigationPage\" Navigation Balkenhöhe")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
