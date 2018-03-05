---
title: iOS-Plattform-Besonderheiten
description: "Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die iOS-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/16/2017
ms.openlocfilehash: a95b49fa3f090339773233dada46a14e69c8bb43
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="ios-platform-specifics"></a>iOS-Plattform-Besonderheiten

_Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die iOS-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden._

Bei iOS kann enthält Xamarin.Forms folgenden Plattform-Angaben:

- Weichzeichnen Unterstützung für eine [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Weitere Informationen finden Sie unter [Weichzeichnen anwenden](#blur).
- Steuern, ob der Seitenname als große Titel auf der Seite "-Navigationsleiste angezeigt wird. Weitere Informationen finden Sie unter [große Titel anzeigen](#large_title).
- Diese Seite Inhalt sichergestellt ist auf einen Bereich des Bildschirms positioniert, der für alle iOS-Geräte sicher ist. Weitere Informationen finden Sie unter [aktivieren im abgesicherten Bereich Layout Handbuch](#safe_area_layout).
- Eine transparente Navigationsleiste. Weitere Informationen finden Sie unter [der Navigation Leiste transparent machen](#translucent_navigation_bar).
- Steuern, ob der Text in der Statusleiste auf Farbe ein [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) so angepasst, dass die Helligkeit der Navigationsleiste übereinstimmen. Weitere Informationen finden Sie unter [den Status Leiste Textmodus Farbe Anpassung](#status_bar_color_mode).
- Sicherstellen, die eingegebenen Text passt in eine [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) durch Anpassen der Größe der Schriftart. Weitere Informationen finden Sie unter [Anpassen der Größe der Schriftart eines Eintrags](#adjust_font_size).
- Steuern der tritt Elementauswahl in einem [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Weitere Informationen finden Sie unter [Steuern der Auswahl einer Elementauswahl](#picker_update_mode).
- Festlegen der Sichtbarkeit der Status für eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Weitere Informationen finden Sie unter [Festlegen der Sichtbarkeit der Status auf einer Seite](#set_status_bar_visibility).
- Steuern, ob eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Weitere Informationen finden Sie unter [verzögern Content Fingereingaben in einem ScrollView](#delay_content_touches).

<a name="blur" />

## <a name="applying-blur"></a>Anwenden von Blur

Diese plattformspezifischen wird verwendet, um den Inhalt in den Ebenen darunter Weichzeichnen und in XAML durch Festlegen von belegt die [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) -Eigenschaft auf den Wert der [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
  ...
  <AbsoluteLayout HorizontalOptions="Center">
    <Image Source="monkeyface.png" />
    <BoxView x:Name="boxView" ios:VisualElement.BlurEffect="ExtraLight" HeightRequest="300" WidthRequest="300" />
  </AbsoluteLayout>
  ...
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

Die `BoxView.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um die Weichzeichnereffekt gelten die [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Enumeration vier bereitstellen Werte: [ `None` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None/), [ `ExtraLight` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight/), [ `Light` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light/), und [ `Dark` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark/).

Das Ergebnis ist, die ein angegebenes [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) wird angewendet, um die [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) Instanz fest, welche verwischt die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) darunter liegenden Ebenen:

![](ios-images/blur-effect.png "Weichzeichnen Sie Effekt plattformspezifischen")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Große Titel anzeigen

Diese plattformspezifischen wird verwendet, um Seitentitel als große Titel auf der Navigationsleiste für Geräte anzuzeigen, die iOS-11 oder höher verwenden. Großer Titel links ausgerichtet wird und eine größere Schrift verwendet und geht in einen standardmäßigen Titel, wie der Benutzer beginnt, Durchführen eines Bildlaufs Inhalte, damit die Bildschirmfläche effizient verwendet wird. Allerdings wird im Querformat, der Titel in die Mitte der Navigationsleiste Inhaltslayout optimieren zurück. Sie wird in XAML verwendet, durch Festlegen der `NavigationPage.PrefersLargeTitles` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

Die `NavigationPage.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die `NavigationPage.SetPrefersLargeTitle` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace befindet, steuert, ob große Titel aktiviert sind.

Vorausgesetzt, dass große Titel aktiviert sind, auf die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), alle Seiten im Navigationsstapel große Titel angezeigt werden. Dieses Verhalten kann auf Seiten überschrieben werden, durch Festlegen der `Page.LargeTitleDisplay` -Eigenschaft auf einen Wert von der `LargeTitleDisplayMode` Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Alternativ kann das Seitenverhalten in c# mithilfe der fluent-API überschrieben werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

Die `Page.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die `Page.SetLargeTitleDisplay` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace befindet, steuert das Verhalten große Titel auf der [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), mit der `LargeTitleDisplayMode` Enumeration, die drei möglichen bereitstellen Werte:

- `Always` – Erzwingt die Navigationsleiste und Schriftart Größe der große-Format verwendet.
- `Automatic` – Verwenden Sie das gleiche Format (Groß oder klein) wie das vorherige Element im Navigationsstapel.
- `Never` – wird die Verwendung der Navigationsleiste regulärer, kleine-Format.

Darüber hinaus die `SetLargeTitleDisplay` Methode kann verwendet werden, um umzuschalten Enumerationswerte durch Aufrufen der `LargeTitleDisplay` Methode, die das aktuelle zurückgibt `LargeTitleDisplayMode`:

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

Das Ergebnis ist, die ein angegebenes `LargeTitleDisplayMode` wird angewendet, um die [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), die die große Titel Verhalten steuert:

![](ios-images/large-title.png "Weichzeichnen Sie Effekt plattformspezifischen")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Aktivieren im Bereich für geschützte Layout-Handbuch

Diese plattformspezifischen wird verwendet, um sicherzustellen, dass der Inhalt der Seite auf einen Bereich des Bildschirms positioniert ist, die für alle Geräte sicher ist, iOS, 11 und höher zu verwenden. Insbesondere hilft sicherstellen, dass der Inhalt wird nicht vom Gerät abgerundeten Ecken, home Indikator oder der Sensor-Gehäuse auf einem iPhone X abgeschnitten. Sie wird in XAML verwendet, durch Festlegen der `Page.UseSafeArea` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Safe Area"
             ios:Page.UseSafeArea="true">
    <StackLayout>
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

Die `Page.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die `Page.SetUseSafeArea` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace befindet, steuert, ob im abgesicherten Bereich Layout Handbuch aktiviert ist.

Das Ergebnis ist, dass der Inhalt der Seite auf einen Bereich des Bildschirms positioniert werden kann, die für alle iPhones sicher ist:

[![](ios-images/safe-area-layout.png "Abgesicherten Bereich Layout Handbuch")](ios-images/safe-area-layout-large.png "abgesicherten Bereich Layout-Handbuch")

> [!NOTE]
> **Hinweis**: abgesicherte Bereich definiert, die von Apple dient in Xamarin.Forms Festlegen der [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) -Eigenschaft, und überschreibt alle vorherigen Werte dieser Eigenschaft, die festgelegt wurden.

Abgesicherte Bereich kann angepasst werden, durch das Abrufen von dessen [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) Wert mit der `Page.SafeAreaInsets` Methode aus der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace. Können dann als geändert werden erforderlich neu zugewiesen, und wählen Sie die `Padding` Eigenschaft in der Page-Konstruktor oder [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) außer Kraft setzen:

```csharp
protected override void OnAppearing()
{
    base.OnAppearing();

    var safeInsets = On<iOS>().SafeAreaInsets();
    safeInsets.Left = 20;
    Padding = safeInsets;
}
```

<a name="translucent_navigation_bar" />

## <a name="making-the-navigation-bar-translucent"></a>Die Navigationsleiste machen transparent

Diese plattformspezifischen wird verwendet, um die Transparenz der Navigationsleiste zu ändern, und in XAML durch Festlegen von belegt die [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

Die `NavigationPage.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um die Navigationsleiste transparent machen. Darüber hinaus die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) -Klasse in der `Xamarin.Forms.PlatformConfiguration.iOSSpecific` Namespace verfügt auch über eine [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) -Methode, die die Navigationsleiste auf den Standardzustand wiederhergestellt und ein [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) Methode, die verwendet werden kann, wechseln Sie die Navigation Leiste Transparenz durch Aufrufen der [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Methode:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Das Ergebnis ist, dass die Transparenz der Navigationsleiste geändert werden kann:

![](ios-images/translucent-navigation-bar.png "Transparentes Navigationsleiste plattformspezifischen")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Anpassen der Statusleiste Farbe Textmodus

Diese plattformspezifischen steuert, ob der Text in der Statusleiste auf Farbe ein [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) so angepasst, dass die Helligkeit der Navigationsleiste übereinstimmen. Sie wird in XAML verwendet, durch Festlegen der [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) -Eigenschaft auf den Wert der [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Enumeration:

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

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

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

Die `NavigationPage.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace, Steuerelemente, ob die Farbe von Text in der Statusleiste auf die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wird entsprechend angepasst der die Helligkeit der Navigationsleiste, mit der [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) -Enumeration bietet zwei mögliche Werte:

- [`DoNotAdjust`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust/) – Gibt an, dass die Statusleiste Textfarbe nicht angepasst werden muss.
- [`MatchNavigationBarTextLuminosity`](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity/) – Gibt an, dass die Statusleiste Textfarbe die Helligkeit der Navigationsleiste entsprechen soll.

Darüber hinaus die [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) Methode kann verwendet werden, um das Abrufen des aktuellen Werts der [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) -Enumeration, der angewendet wird die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/).

Das Ergebnis ist die Statusleiste die Textfarbe für eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) die Helligkeit der Navigationsleiste entsprechend angepasst werden kann. In diesem Beispiel wird die Statusleiste Text Farbe ändert sich, als der Benutzer wechselt zwischen den [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) und [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Seiten von einer [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Statusleiste Text Farbe Modus plattformspezifischen")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Anpassen der Schriftartgröße eines Eintrags

Diese plattformspezifischen wird verwendet, um den Schriftgrad des Skalieren einer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) um sicherzustellen, dass der eingegebenen Text im Steuerelement passt. Sie wird in XAML verwendet, durch Festlegen der [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
    <StackLayout Margin="20">
        <Entry x:Name="entry"
               Placeholder="Enter text here to see the font size change"
               FontSize="22"
               ios:Entry.AdjustsFontSizeToFitWidth="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

Die `Entry.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace dient zum Skalieren des Schriftgrad des eingegebenen Texts um sicherzustellen, dass er stimmt mit der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Darüber hinaus die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) -Klasse in der `Xamarin.Forms.PlatformConfiguration.iOSSpecific` Namespace verfügt auch über eine [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) -Methode, die diese plattformspezifische deaktiviert und ein [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) Methode, die verwendet werden kann, Schriftgrad, die durch den Aufruf Skalierung aktivieren bzw. Deaktivieren der [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Methode:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Das Ergebnis ist, die den Schriftgrad des der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) wird skaliert, um sicherzustellen, dass der eingegebenen Text im Steuerelement passt:

![](ios-images/entry-font-size.png "Eintrag Schriftart Größe plattformspezifischen anpassen")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Steuern der Auswahl einer Elementauswahl

Diese plattformspezifischen steuert tritt Elementauswahl in einer [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), dadurch kann der Benutzer angeben, dass die Elementauswahl beim Durchsuchen von Elementen im Steuerelement, oder nur einmal auftritt der **Fertig** gedrückt wird. Sie wird in XAML verwendet, durch Festlegen der `Picker.UpdateMode` -Eigenschaft auf den Wert der `UpdateMode` Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <Picker ... Title="Select a monkey" ios:Picker.UpdateMode="WhenFinished">
          ...
        </Picker>
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die `Picker.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die `Picker.SetUpdateMode` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um die steuern, wann die Elementauswahl auftritt, mit der `UpdateMode` -Enumeration bietet zwei mögliche Werte:

- `Immediately` – Elementauswahl tritt auf, wie der Benutzer Elemente im durchsucht die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Dies ist das Standardverhalten in Xamarin.Forms.
- `WhenFinished` – Elementauswahl tritt nur auf, sobald der Benutzer geklickt hat, hat die **Fertig** Schaltfläche der [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Darüber hinaus die `SetUpdateMode` Methode kann verwendet werden, um umzuschalten Enumerationswerte durch Aufrufen der `UpdateMode` Methode, die das aktuelle zurückgibt `UpdateMode`:

```csharp
switch (picker.On<iOS>().UpdateMode())
{
    case UpdateMode.Immediately:
        picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
        break;
    case UpdateMode.WhenFinished:
        picker.On<iOS>().SetUpdateMode(UpdateMode.Immediately);
        break;
}
```

Das Ergebnis ist, die ein angegebenes `UpdateMode` wird angewendet, um die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), die steuert, wann Elementauswahl auftritt:

[![](ios-images/picker-updatemode.png "Auswahl einer UpdateMode plattformspezifischen")](ios-images/picker-updatemode-large.png "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Festlegen der Statusleiste Sichtbarkeit auf einer Seite

Diese plattformspezifischen wird verwendet, um die Sichtbarkeit der Statusleiste auf festzulegen eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), sowie die Möglichkeit, die steuern, wie die Statusleiste betritt oder verlässt den `Page`. Sie wird in XAML verwendet, durch Festlegen der `Page.PrefersStatusBarHidden` -Eigenschaft auf den Wert der `StatusBarHiddenMode` -Enumeration, und optional die `Page.PreferredStatusBarUpdateAnimation` -Eigenschaft auf einen Wert von der `UIStatusBarAnimation` Enumeration:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

Die `Page.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die `Page.SetPrefersStatusBarHidden` Methode in der `Xamarin.Forms.PlatformConfiguration.iOSSpecific` -Namespace wird verwendet, um die Sichtbarkeit der Statusleiste auf festzulegen eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) durch Angabe einer der `StatusBarHiddenMode` Enumerationswerte: `Default`, `True` , oder `False`. Die `StatusBarHiddenMode.True` und `StatusBarHiddenMode.False` Werte festlegen, die Sichtbarkeit der Status unabhängig von der geräteausrichtung, und die `StatusBarHiddenMode.Default` Wert Blendet die Statusleiste in einer vertikal compact-Umgebung.

Das Ergebnis ist, die die Sichtbarkeit der Statusleiste auf eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) kann festgelegt werden:

![](ios-images/hide-status-bar.png "Statusleiste Sichtbarkeit plattformspezifischen")

> [!NOTE]
> Auf einem [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), dem angegebenen `StatusBarHiddenMode` Enumerationswert werden auch aktualisiert die Statusleiste auf alle untergeordneten Seiten. Auf allen anderen [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-abgeleitete Typen, die den angegebenen `StatusBarHiddenMode` Enumerationswert wird nur die Statusleiste auf die aktuelle Seite aktualisieren.

Die `Page.SetPreferredStatusBarUpdateAnimation` Methode wird verwendet, um festzulegen, wie die Statusleiste betritt oder verlässt den [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) durch Angabe einer der `UIStatusBarAnimation` Enumerationswerte: `None`, `Fade`, oder `Slide`. Wenn die `Fade` oder `Slide` Enumerationswert angegeben wird, ein 0,25 zweites Animation ausgeführt wird, wie die Statusleiste betritt oder verlässt den `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Verzögern Content Fingereingaben in einem ScrollView

Ein implizite Zeitgeber wird ausgelöst, wenn eine Touch-Geste in beginnt eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) unter iOS und den `ScrollView` entscheidet basierend auf der Benutzeraktion innerhalb der Timer, ob diese Bewegung behandeln oder übergeben Sie ihn an seinen Inhalt sollte. Standardmäßig iOS `ScrollView` Verzögerungen Content Fingereingaben, aber dies kann Probleme verursachen, in einigen Fällen kann es bei der `ScrollView` Inhalt nicht ausschlaggebende Bewegung, wenn er sollte. Aus diesem Grund diese plattformspezifische steuert, ob ein `ScrollView` behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Sie wird in XAML verwendet, durch Festlegen der `ScrollView.ShouldDelayContentTouches` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

Die `ScrollView.On<iOS>` Methode gibt an, dass dieser plattformspezifische nur auf iOS ausgeführt wird. Die `ScrollView.SetShouldDelayContentTouches` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um Steuerelement gibt an, ob eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Darüber hinaus die `SetShouldDelayContentTouches` Methode kann verwendet werden, um umzuschalten, verzögern Content Fingereingaben durch Aufrufen der `ShouldDelayContentTouches` -Methode zurückgegeben wird, ob Inhalt Fingereingaben verzögert werden:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Das Ergebnis ist, eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) können deaktivieren verzögern Content Fingereingaben daher empfangen, die in diesem Szenario die [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) empfängt die Bewegung statt über das [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) auf der Seite der [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "ScrollView Verzögerung Inhalt berührt plattformspezifischen")](ios-images/scrollview-delay-content-touches-large.png "ScrollView Delay Content Touches Plaform-Specific")

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie die iOS-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden. Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
