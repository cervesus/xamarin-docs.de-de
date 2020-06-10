---
Title: "tabbedpage Page swiping on Android" Description: "Platform-Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne benutzerdefinierte Renderer oder Effekte implementieren zu müssen. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Element nutzen, das das Schwenken mit einer horizontalen Fingerbewegung zwischen Seiten in einer tabbedpage ermöglicht. "
ms. Prod: xamarin ms. assetid: D1C09CCB-7246-41A4-8BD2-FA6FABCF1C72 ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 07/10/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="tabbedpage-page-swiping-on-android"></a>Tabbedpage Page schwenken unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese Android-plattformspezifische wird verwendet, um das Schwenken mit einer horizontalen Fingerbewegung zwischen Seiten in einem zu ermöglichen [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Sie wird in XAML verwendet, indem die [`TabbedPage.IsSwipePagingEnabled`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) angefügte-Eigenschaft auf einen Wert festgelegt wird `boolean` :

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

Die- `TabbedPage.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. [ `TabbedPage.SetIsSwipePagingEnabled` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. tabbedpage. * tisswipeer pagingenabled ( Xamarin.Forms . Die bindableobject, System. Boolean)-Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um das Schwenken zwischen Seiten in einem zu aktivieren [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) . Außerdem verfügt die- `TabbedPage` Klasse im- `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` Namespace ebenfalls über ein [ `EnableSwipePaging` ] (Xref:) Xamarin.Forms . Platformconfiguration. androidspecific. tabbedpage. enableswipepaging ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Tabbedpage}))-Methode, die diese plattformspezifische Methode aktiviert, und a [ `DisableSwipePaging` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. tabbedpage. disableswipepaging ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . Tabbedpage}))-Methode, die diese plattformspezifische deaktiviert. Die [`TabbedPage.OffscreenPageLimit`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty) angefügte-Eigenschaft und [ `SetOffscreenPageLimit` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. tabbedpage. *-ffscreenpagelimit ( Xamarin.Forms . Bindableobject, System. Int32))-Methode verwendet wird, um die Anzahl der Seiten festzulegen, die auf beiden Seiten der aktuellen Seite im Leerlaufzustand aufbewahrt werden sollen.

Das Ergebnis ist, dass das Auslagern über die Seiten, die von einem angezeigt werden, [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) aktiviert ist:

![](tabbedpage-page-swiping-images/tabbedpage-swipe.png)

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
