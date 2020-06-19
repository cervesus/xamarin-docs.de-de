---
title: Swipinview-Swipe-Übergangsmodus unter IOS
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische IOS-Anwendung nutzen, um den Übergang zu steuern, der beim Öffnen einer swipeer View verwendet wird.
ms.prod: xamarin
ms.assetid: C667F24C-BAD8-47E0-9285-D3546BEF703B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 4b2030461025c1cd647595a1ecc22c5589e99fef
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84137045"
---
# <a name="swipeview-swipe-transition-mode-on-ios"></a>Swipinview-Swipe-Übergangsmodus unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese IOS-plattformspezifische steuert den Übergang, der beim Öffnen eines verwendet wird `SwipeView` . Sie wird in XAML verwendet, indem die `SwipeView.SwipeTransitionMode` bindbare Eigenschaft auf einen Wert der- `SwipeTransitionMode` Enumeration festgelegt wird:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SwipeView ios:SwipeView.SwipeTransitionMode="Drag">
            <SwipeView.LeftItems>
                <SwipeItems>
                    <SwipeItem Text="Delete"
                               IconImageSource="delete.png"
                               BackgroundColor="LightPink"
                               Invoked="OnDeleteSwipeItemInvoked" />
                </SwipeItems>
            </SwipeView.LeftItems>
            <!-- Content -->
        </SwipeView>
    </StackLayout>
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<iOS>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

Die- `SwipeView.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. Die `SwipeView.SetSwipeTransitionMode` -Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den Übergang zu steuern, der beim Öffnen eines verwendet wird `SwipeView` . Die- `SwipeTransitionMode` Enumeration bietet zwei mögliche Werte:

- `Reveal`Gibt an, dass die schwingenden Elemente beim Schwenken des `SwipeView` Inhalts angezeigt werden, und ist der Standardwert der `SwipeView.SwipeTransitionMode` Eigenschaft.
- `Drag`Gibt an, dass die gezogenen Elemente in die Ansicht gezogen werden, wenn der Inhalt gedrillt `SwipeView` wird.

Außerdem `SwipeView.GetSwipeTransitionMode` kann die-Methode verwendet werden, um den zurückzugeben `SwipeTransitionMode` , der auf den angewendet wird `SwipeView` .

Das Ergebnis ist, dass ein `SwipeTransitionMode` angegebener Wert auf den angewendet wird `SwipeView` , der den Übergang steuert, der beim Öffnen von verwendet wird `SwipeView` :

[![Screenshot von swipeer View swipeer transitionmodes unter IOS](swipeview-swipetransitionmode-images/swipetransitionmode.png "Swipeer transitionmodes unter IOS")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Swipeer transitionmodes unter IOS")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
