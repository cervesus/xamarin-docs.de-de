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
ms.openlocfilehash: 2dcabe3c0067734250834c2927fd4cbb83906943
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128790"
---
# <a name="navigationpage-bar-height-on-android"></a>Höhe der Navigations Seitenleiste unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische legt die Höhe der Navigationsleiste auf fest [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Sie wird in XAML verwendet, indem die [`NavigationPage.BarHeight`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) bindbare Eigenschaft auf einen ganzzahligen Wert festgelegt wird:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

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

Die- `NavigationPage.On<Android>` Methode gibt an, dass diese plattformspezifische nur auf App-kompatikompatiblen Android-Apps ausgeführt wird. [ `NavigationPage.SetBarHeight` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. AppCompat. navigationpage. setbarheight ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Navigationpage}, System. Int32)) im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) Namespace wird verwendet, um die Höhe der Navigationsleiste auf einem festzulegen [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) . Außerdem ist [ `NavigationPage.GetBarHeight` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. AppCompat. navigationpage. getbarheight ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Navigationpage}))-Methode kann verwendet werden, um die Höhe der Navigationsleiste in der zurückzugeben `NavigationPage` .

Das Ergebnis ist, dass die Höhe der Navigationsleiste auf einem [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) festgelegt werden kann:

![](navigationpage-bar-height-images/navigationpage-barheight.png "NavigationPage navigation bar height")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
