---
Title: "swipeview Swipe Transition Mode on Android" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element nutzen können, das den Übergang steuert, der beim Öffnen einer swipeer View verwendet wird. "
ms. Prod: xamarin ms. assetid: 6b1f 8213-9d62-4c40-9F 04-881f 1371b5aa ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 12/11/2019 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="swipeview-swipe-transition-mode-on-android"></a>Swipeer View-Swipe-Übergangsmodus unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische steuert den Übergang, der beim Öffnen eines verwendet wird `SwipeView` . Sie wird in XAML verwendet, indem die `SwipeView.SwipeTransitionMode` bindbare Eigenschaft auf einen Wert der- `SwipeTransitionMode` Enumeration festgelegt wird:

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

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

SwipeView swipeView = new Xamarin.Forms.SwipeView();
swipeView.On<Android>().SetSwipeTransitionMode(SwipeTransitionMode.Drag);
// ...
```

Die- `SwipeView.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die `SwipeView.SetSwipeTransitionMode` -Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace wird verwendet, um den Übergang zu steuern, der beim Öffnen eines verwendet wird `SwipeView` . Die- `SwipeTransitionMode` Enumeration bietet zwei mögliche Werte:

- `Reveal`Gibt an, dass die schwingenden Elemente beim Schwenken des `SwipeView` Inhalts angezeigt werden, und ist der Standardwert der `SwipeView.SwipeTransitionMode` Eigenschaft.
- `Drag`Gibt an, dass die gezogenen Elemente in die Ansicht gezogen werden, wenn der Inhalt gedrillt `SwipeView` wird.

Außerdem `SwipeView.GetSwipeTransitionMode` kann die-Methode verwendet werden, um den zurückzugeben `SwipeTransitionMode` , der auf den angewendet wird `SwipeView` .

Das Ergebnis ist, dass ein `SwipeTransitionMode` angegebener Wert auf den angewendet wird `SwipeView` , der den Übergang steuert, der beim Öffnen von verwendet wird `SwipeView` :

[![Screenshot von swipeer View swipeer transitionmodes unter Android](swipeview-swipetransitionmode-images/swipetransitionmode.png "Swipeer transitionmodes unter Android")](swipeview-swipetransitionmode-images/swipetransitionmode-large.png#lightbox "Swipeer transitionmodes unter Android")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
