---
title: Swipeer View-Swipe-Übergangsmodus unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element verwenden, das den Übergang steuert, der beim Öffnen einer swipeer View verwendet wird.
ms.prod: xamarin
ms.assetid: 6B1F8213-9D62-4C40-9F04-881F1371B5AA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/11/2019
ms.openlocfilehash: 077d4a8a9530bf074fde710dd08c1fbea49668ef
ms.sourcegitcommit: d0e6436edbf7c52d760027d5e0ccaba2531d9fef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75490475"
---
# <a name="swipeview-swipe-transition-mode-on-android"></a>Swipeer View-Swipe-Übergangsmodus unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische steuert den Übergang, der beim Öffnen einer `SwipeView`verwendet wird. Sie wird in XAML verwendet, indem die `SwipeView.SwipeTransitionMode` bindbare-Eigenschaft auf einen Wert der `SwipeTransitionMode`-Enumeration festgelegt wird:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core" >
    <StackLayout>
        <SwipeView android:SwipeView.SwipeTransitionMode="Drag">
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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

Die `SwipeView.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `SwipeView.SetSwipeTransitionMode`-Methode im [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um den Übergang zu steuern, der beim Öffnen einer `SwipeView`verwendet wird. Die `SwipeTransitionMode`-Enumeration bietet zwei mögliche Werte:

- `Reveal` gibt an, dass die schwenkbaren Elemente angezeigt werden, wenn der `SwipeView` Inhalt über läuft, und ist der Standardwert der `SwipeView.SwipeTransitionMode`-Eigenschaft.
- `Drag` gibt an, dass die gezogenen Elemente in die Ansicht gezogen werden, wenn der `SwipeView` Inhalt gedrillt wird.

Außerdem kann die `SwipeView.GetSwipeTransitionMode`-Methode verwendet werden, um die `SwipeTransitionMode` zurückzugeben, die auf die `SwipeView`angewendet wird.

Das Ergebnis ist, dass ein angegebener `SwipeTransitionMode` Wert auf den `SwipeView`angewendet wird, der den Übergang steuert, der beim Öffnen des `SwipeView`verwendet wird:

[![Screenshot von swipeer View swipeer transitionmodes unter Android](swipeview-swipetransitionmode-images/swipetransitionmode.png "Swipeer transitionmodes unter Android")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Swipeer transitionmodes unter Android")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
