---
Title: "navigationpage Bar Text Color Mode on IOS" Description: "Platform-Besonderheiten ermöglicht Ihnen die Nutzung von Funktionen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische IOS-Element nutzen, das steuert, ob die Textfarbe der Statusleiste auf einer navigationpage mit der Helligkeit der Navigationsleiste übereinstimmt.
ms. Prod: xamarin ms. assetid: 03698a44-39f 1-4030-9af5-ar10a6713828a ms. Technology: xamarin-Forms Author: davidbritch ms. Author: dabritch ms. Date: 10/24/2018 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="navigationpage-bar-text-color-mode-on-ios"></a>Navigations Seitenleiste-textfarbmodus unter IOS

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifische steuert, ob die Textfarbe der Statusleiste für einen entsprechend [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) der Helligkeit der Navigationsleiste angepasst wird. Sie wird in XAML verwendet, indem die [`NavigationPage.StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) :

```xaml
<MasterDetailPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    x:Class="PlatformSpecifics.iOSStatusBarTextColorModePage">
    <MasterDetailPage.Master>
        <ContentPage Title="Master Page Title" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <NavigationPage BarBackgroundColor="Blue" BarTextColor="White"
                        ios:NavigationPage.StatusBarTextColorMode="MatchNavigationBarTextLuminosity">
            <x:Arguments>
                <ContentPage>
                    <Label Text="Slide the master page to see the status bar text color mode change." />
                </ContentPage>
            </x:Arguments>
        </NavigationPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>

```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

IsPresentedChanged += (sender, e) =>
{
    var mdp = sender as MasterDetailPage;
    if (mdp.IsPresented)
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.DoNotAdjust);
    else
        ((Xamarin.Forms.NavigationPage)mdp.Detail)
          .On<iOS>()
          .SetStatusBarTextColorMode(StatusBarTextColorMode.MatchNavigationBarTextLuminosity);
};
```

Die- `NavigationPage.On<iOS>` Methode gibt an, dass diese plattformspezifische nur unter IOS ausgeführt wird. [ `NavigationPage.SetStatusBarTextColorMode` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. navigationpage. SetStatus-bartextcolormode ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Navigationpage}, Xamarin.Forms . Platformconfiguration. iosspecific. statusbartextcolormode))-Methode im- [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) Namespace steuert, ob die Textfarbe der Statusleiste auf die [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) Helligkeit der Navigationsleiste angepasst wird, wobei die- [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) Enumeration zwei mögliche Werte bereitstellt:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust)– Gibt an, dass die Textfarbe der Statusleiste nicht angepasst werden soll.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity)– Gibt an, dass die Textfarbe der Statusleiste der Helligkeit der Navigationsleiste entsprechen soll.

Außerdem ist [ `GetStatusBarTextColorMode` ] (Xref: Xamarin.Forms . Platformconfiguration. iosspecific. navigationpage. GetStatus-bartextcolormode ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. IOS, Xamarin.Forms . Navigationpage}))-Methode kann verwendet werden, um den aktuellen Wert der [`StatusBarTextColorMode`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) -Enumeration abzurufen, die auf das angewendet wird [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) .

Das Ergebnis ist, dass die Textfarbe der Statusleiste auf einem [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) angepasst werden kann, um die Helligkeit der Navigationsleiste abzugleichen. In diesem Beispiel ändert sich die Textfarbe der Statusleiste, wenn der Benutzer zwischen [`Master`](xref:Xamarin.Forms.MasterDetailPage.Master) den [`Detail`](xref:Xamarin.Forms.MasterDetailPage.Detail) Seiten und eines wechselt [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) :

![](status-bar-text-color-images/status-bar-text-color-mode.png "Status Bar Text Color Mode Platform-Specific")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [iosspecific-API](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
