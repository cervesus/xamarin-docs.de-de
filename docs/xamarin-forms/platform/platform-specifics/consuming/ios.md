---
title: iOS Plattformeigenschaften
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel veranschaulicht, wie die Plattformeigenschaften iOS nutzen, die in Xamarin.Forms integriert sind.
ms.prod: xamarin
ms.assetid: C0837996-A1E8-47F9-B3A8-98EE43B4A675
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: be378a60a9d9a7b206b64f07ee70edb432cec8e3
ms.sourcegitcommit: 3e980fbf92c69c3dd737554e8c6d5b94cf69ee3a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2018
ms.locfileid: "37935653"
---
# <a name="ios-platform-specifics"></a>iOS Plattformeigenschaften

_Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel veranschaulicht, wie die Plattformeigenschaften iOS nutzen, die in Xamarin.Forms integriert sind._

Unter iOS enthält die Xamarin.Forms folgenden Besonderheiten die Plattform –:

- Blur-Unterstützung für eine [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/). Weitere Informationen finden Sie unter [Blur-anwenden](#blur).
- Steuern, ob es sich bei der Seitentitel als große Titel auf der Seite Navigationsleiste angezeigt wird. Weitere Informationen finden Sie unter [große Titel anzeigen](#large_title).
- Sicherstellen der Seite Inhalt auf einen Bereich des Bildschirms befindet sich, die für alle iOS-Geräte sicher ist. Weitere Informationen finden Sie unter [Aktivieren der sicheren Bereich Layoutführungslinie](#safe_area_layout).
- Eine lichtdurchlässiger Navigationsleiste. Weitere Informationen finden Sie unter [vornehmen, die Navigation Leiste lichtdurchlässiger](#translucent_navigation_bar).
- Steuern, ob die Farbe des Statusleiste angezeigte Texts auf einer [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wird angepasst, um die Helligkeit der Navigationsleiste zu entsprechen. Weitere Informationen finden Sie unter [Anpassen der Status Leiste Textmodus Farbe](#status_bar_color_mode).
- Um sicherzustellen, die eingegebenen Text passt in einen [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) durch den Schriftgrad anpassen. Weitere Informationen finden Sie unter [Anpassen der Schriftgrad eines Eintrags](#adjust_font_size).
- Steuern der bei Auswahl von Listenelementen in tritt auf, eine [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Weitere Informationen finden Sie unter [Auswahl Elementauswahl steuern](#picker_update_mode).
- Festlegen der Sichtbarkeit der Status für eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/). Weitere Informationen finden Sie unter [Festlegen der Sichtbarkeit der Status auf einer Seite](#set_status_bar_visibility).
- Steuern, ob eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Weitere Informationen finden Sie unter [verzögern Content Workflows in einer ScrollView](#delay_content_touches).
- Festlegen von den Trennzeichenstil für eine [ `ListView` ](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [Festlegen von den Trennzeichenstil für eine ListView](#listview-separatorstyle).
- Deaktivieren auf einer unterstützten legacy Farbmodus [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [deaktivieren Farbe Legacymodus](#legacy-color-mode).

<a name="blur" />

## <a name="applying-blur"></a>Anwenden von Blur

Diese plattformspezifischen wird verwendet, um den Inhalt, die darunter liegenden Ebenen blur und in XAML verwendet wird, durch Festlegen der [ `VisualElement.BlurEffect` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.BlurEffectProperty/) angefügte Eigenschaft auf den Wert der [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Enumeration:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

boxView.On<iOS>().UseBlurEffect(BlurEffectStyle.ExtraLight);
```

Die `BoxView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `VisualElement.UseBlurEffect` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.UseBlurEffect/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement}/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um mit Weichzeichnereffekts, gelten die [ `BlurEffectStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle) Enumeration, die Bereitstellung von vier Werte: [ `None` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.None), [ `ExtraLight` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.ExtraLight), [ `Light` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Light), und [ `Dark` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle.Dark).

Das Ergebnis ist, die einem angegebenen [ `BlurEffectStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.BlurEffectStyle/) gilt, an die [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) -Instanz, welche Weichzeichner der [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) darunter liegenden Ebenen:

![](ios-images/blur-effect.png "Blur-Effekt plattformspezifische")

<a name="large_title" />

## <a name="displaying-large-titles"></a>Große Titel anzeigen

Diese plattformspezifischen wird verwendet, um den Seitentitel als große Titel auf der Navigationsleiste für Geräte anzuzeigen, die iOS 11 oder höher verwenden. Ein große Titel wird links ausgerichtet und wird von einer größeren Schrift und geht in einen standard-Titel, wie der Benutzer beginnt, Scrollen von Inhalt, sodass Bildschirmfläche effizient verwendet wird. Allerdings wird im Querformat, der Titel in der Mitte des der Navigationsleiste Inhaltslayout optimieren zurück. Es ist in XAML verwendet, durch Festlegen der `NavigationPage.PrefersLargeTitles` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

Die `NavigationPage.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `NavigationPage.SetPrefersLargeTitle` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace steuert, ob große Titel aktiviert sind.

Vorausgesetzt, dass große Titel aktiviert sind, auf die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/), alle Seiten im Navigationsstapel werden große Titel angezeigt. Dieses Verhalten kann auf Seiten überschrieben werden, durch Festlegen der `Page.LargeTitleDisplay` angefügte Eigenschaft auf den Wert der `LargeTitleDisplayMode` Enumeration:

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

Die `Page.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Page.SetLargeTitleDisplay` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace, steuert das Verhalten der große Titel auf der [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), mit der `LargeTitleDisplayMode` Enumeration, die Bereitstellung von drei möglichen Werte:

- `Always` – erzwingen, dass die Navigationsleiste und die Schriftart Größe der große-Format verwendet.
- `Automatic` – Verwenden Sie das gleiche Format (Groß oder klein) als das vorherige Element im Navigationsstapel.
- `Never` – wird die Verwendung der regulären, kleine Format Navigationsleiste erzwungen.

Darüber hinaus die `SetLargeTitleDisplay` Methode kann verwendet werden, zum Umschalten der Enumerationswerte durch Aufrufen der `LargeTitleDisplay` Methode, die die aktuelle zurückgibt `LargeTitleDisplayMode`:

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

Das Ergebnis ist, die einem angegebenen `LargeTitleDisplayMode` gilt, an die [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), welche Steuerelemente das große Titel Verhalten:

![](ios-images/large-title.png "Blur-Effekt plattformspezifische")

<a name="safe_area_layout" />

## <a name="enabling-the-safe-area-layout-guide"></a>Aktivieren der sicheren Bereich Layoutführungslinie

Diese plattformspezifischen wird verwendet, um sicherzustellen, dass der Inhalt der Seite auf einen Bereich des Bildschirms befindet, die für alle Geräte sicher ist, die iOS 11 und höher verwenden. Insbesondere leichter sicherstellen, dass der Inhalt wird nicht gerundet Gerät Ecken, home Indikators oder der Sensor wohnkosten auf einem iPhone X abgeschnitten. Es ist in XAML verwendet, durch Festlegen der `Page.UseSafeArea` angefügten Eigenschaft, um eine `boolean` Wert:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetUseSafeArea(true);
```

Die `Page.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Page.SetUseSafeArea` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace steuert, ob der Bereich für geschützte Layoutführungslinie aktiviert ist.

Das Ergebnis ist, dass der Inhalt der Seite auf einen Bereich des Bildschirms positioniert werden kann, die für alle iPhones sicher ist:

[![](ios-images/safe-area-layout.png "Sicherheitsbereich Layoutführungslinie")](ios-images/safe-area-layout-large.png#lightbox "Sicherheitsbereich Layoutführungslinie")

> [!NOTE]
> Der sichere Bereich definiert, die von Apple dient in Xamarin.Forms zum Festlegen der [ `Page.Padding` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Page.Padding/) -Eigenschaft, und überschreibt alle vorherigen Werte dieser Eigenschaft, die festgelegt wurden.

Der sichere Bereich kann angepasst werden, durch Abrufen der [ `Thickness` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Thickness/) Wert mit einer der `Page.SafeAreaInsets` Methode aus der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) Namespace. Können dann als geändert werden erforderlich neu zugewiesen zu, und wählen Sie die `Padding` -Eigenschaft in der Page-Konstruktor oder [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) außer Kraft setzen:

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

## <a name="making-the-navigation-bar-translucent"></a>Machen die Navigationsleiste lichtdurchlässiger

Diese plattformspezifischen wird verwendet, um die Transparenz der Navigationsleiste zu ändern, und in XAML verwendet wird, durch Festlegen der [ `NavigationPage.IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucentProperty/) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<NavigationPage ...
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                BackgroundColor="Blue"
                ios:NavigationPage.IsNavigationBarTranslucent="true">
  ...
</NavigationPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

(App.Current.MainPage as Xamarin.Forms.NavigationPage).BackgroundColor = Color.Blue;
(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().EnableTranslucentNavigationBar();
```

Die `NavigationPage.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `NavigationPage.EnableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.EnableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um die Navigationsleiste transparent machen. Darüber hinaus die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage/) -Klasse in der `Xamarin.Forms.PlatformConfiguration.iOSSpecific` Namespace verfügt auch über eine [ `DisableTranslucentNavigationBar` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.DisableTranslucentNavigationBar/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) -Methode, die die Navigationsleiste auf den Standardzustand wiederhergestellt und ein [ `SetIsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetIsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/System.Boolean/) Methode, die verwendet werden kann, um die Navigation Leiste Transparenz durch Aufrufen von wechseln die [ `IsNavigationBarTranslucent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.IsNavigationBarTranslucent/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage}/) Methode:

```csharp
(App.Current.MainPage as Xamarin.Forms.NavigationPage)
  .On<iOS>()
  .SetIsNavigationBarTranslucent(!(App.Current.MainPage as Xamarin.Forms.NavigationPage).On<iOS>().IsNavigationBarTranslucent());
```

Das Ergebnis ist, dass die Transparenz der Navigationsleiste geändert werden kann:

![](ios-images/translucent-navigation-bar.png "Lichtdurchlässiger Navigationsleiste plattformspezifische")

<a name="status_bar_color_mode" />

## <a name="adjusting-the-status-bar-text-color-mode"></a>Die Statusleiste die Farbe im Textmodus anpassen

Diese plattformspezifischen steuert, ob die Farbe des Statusleiste angezeigte Texts auf einer [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wird angepasst, um die Helligkeit der Navigationsleiste zu entsprechen. Es ist in XAML verwendet, durch Festlegen der [ `NavigationPage.StatusBarTextColorMode` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.StatusBarTextColorModeProperty/) angefügte Eigenschaft auf den Wert der [ `StatusBarTextColorMode` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Enumeration:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

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

Die `NavigationPage.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `NavigationPage.SetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.SetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace, Steuerelemente, ob die Farbe, die des Texts der Statusleiste auf die [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) wird entsprechend angepasst der die Helligkeit der Navigationsleiste, mit der [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) Enumeration, die Bereitstellung von zwei möglicher Werten:

- [`DoNotAdjust`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.DoNotAdjust) – Gibt an, dass die Statusleiste Textfarbe nicht angepasst werden muss.
- [`MatchNavigationBarTextLuminosity`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode.MatchNavigationBarTextLuminosity) – Gibt an, dass die Statusleiste Textfarbe, die Helligkeit der Navigationsleiste entsprechen muss.

Darüber hinaus die [ `GetStatusBarTextColorMode` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.NavigationPage.GetStatusBarTextColorMode/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.NavigationPage%7D/) Methode kann verwendet werden, um das Abrufen des aktuellen Werts der [ `StatusBarTextColorMode` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.StatusBarTextColorMode) Enumeration, die auf die [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage).

Das Ergebnis ist, die Statusleiste die Textfarbe für eine [ `NavigationPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.NavigationPage/) die Helligkeit der Navigationsleiste entsprechend angepasst werden kann. In diesem Beispiel der Statusleiste an, Änderungen am Text von Farbe wie der Benutzer wechselt zwischen den [ `Master` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Master/) und [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) Seiten eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

![](ios-images/status-bar-text-color-mode.png "Statusleiste Text Color Modus plattformspezifische")

<a name="adjust_font_size" />

## <a name="adjusting-the-font-size-of-an-entry"></a>Anpassen der Schriftgrad eines Eintrags

Diese plattformspezifischen wird verwendet, um den Schriftgrad des Skalieren einer [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) um sicherzustellen, dass es sich bei der eingegebenen Text im Steuerelement geeignet ist. Es ist in XAML verwendet, durch Festlegen der [ `Entry.AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidthProperty/) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

entry.On<iOS>().EnableAdjustsFontSizeToFitWidth();
```

Die `Entry.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `Entry.EnableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.EnableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um skalieren die Größe des eingegebenen Texts um sicherzustellen, dass er stimmt mit der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/). Darüber hinaus die [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry/) -Klasse in der `Xamarin.Forms.PlatformConfiguration.iOSSpecific` Namespace verfügt auch über eine [ `DisableAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.DisableAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) -Methode, die diese plattformspezifisch, deaktiviert und ein [ `SetAdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.SetAdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/System.Boolean/) Methode, die verwendet werden können, zum Umschalten der Schriftgrad skalieren durch das Aufrufen der [ `AdjustsFontSizeToFitWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.iOSSpecific.Entry.AdjustsFontSizeToFitWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.Entry}/) Methode:

```csharp
entry.On<iOS>().SetAdjustsFontSizeToFitWidth(!entry.On<iOS>().AdjustsFontSizeToFitWidth());
```

Das Ergebnis ist, den Schriftgrad des der [ `Entry` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Entry/) wird skaliert, um sicherzustellen, dass es sich bei der eingegebenen Text in das Steuerelement passen:

![](ios-images/entry-font-size.png "Eintrag Font Size plattformspezifische anpassen")

<a name="picker_update_mode" />

## <a name="controlling-picker-item-selection"></a>Steuern der Auswahl-Elementauswahl

Diese plattformspezifischen steuert bei Auswahl von Listenelementen in tritt auf, eine [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), damit der Benutzer angeben, dass die Auswahl beim Durchsuchen der Elemente im Steuerelement oder nur einmal auftritt der **Fertig** Schaltfläche ist gedrückt. Es ist in XAML verwendet, durch Festlegen der `Picker.UpdateMode` angefügte Eigenschaft auf den Wert der `UpdateMode` Enumeration:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

picker.On<iOS>().SetUpdateMode(UpdateMode.WhenFinished);
```

Die `Picker.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Picker.SetUpdateMode` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um zu steuern Elementauswahl tritt auf, mit der `UpdateMode` Enumeration, die Bereitstellung von zwei möglicher Werten:

- `Immediately` – Auswahl von Listenelementen tritt auf, wie der Benutzer Elemente in durchsucht die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/). Dies ist das Standardverhalten in Xamarin.Forms.
- `WhenFinished` – Elementauswahl tritt nur auf, nachdem der Benutzer geklickt hat, hat die **Fertig** Schaltfläche der [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/).

Darüber hinaus die `SetUpdateMode` Methode kann verwendet werden, zum Umschalten der Enumerationswerte durch Aufrufen der `UpdateMode` Methode, die die aktuelle zurückgibt `UpdateMode`:

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

Das Ergebnis ist, die einem angegebenen `UpdateMode` gilt, an die [ `Picker` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Picker/), die steuert, wann Elementauswahl auftritt:

[![](ios-images/picker-updatemode.png "Auswahl UpdateMode plattformspezifische")](ios-images/picker-updatemode-large.png#lightbox "Picker UpdateMode Plaform-Specific")

<a name="set_status_bar_visibility" />

## <a name="setting-the-status-bar-visibility-on-a-page"></a>Festlegen der Statusleiste Sichtbarkeit auf einer Seite

Diese plattformspezifischen wird verwendet, um die Sichtbarkeit der Statusleiste für Festlegen einer [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), sowie die Möglichkeit zum Steuern, wie die Statusleiste eintritt oder dieses verlässt die `Page`. Er wird genutzt, in XAML durch Festlegen der `Page.PrefersStatusBarHidden` angefügte Eigenschaft auf einen Wert von der `StatusBarHiddenMode` -Enumeration und optional die `Page.PreferredStatusBarUpdateAnimation` angefügte Eigenschaft auf einen Wert von der `UIStatusBarAnimation` Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersStatusBarHidden="True"
             ios:Page.PreferredStatusBarUpdateAnimation="Fade">
  ...
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersStatusBarHidden(StatusBarHiddenMode.True)
         .SetPreferredStatusBarUpdateAnimation(UIStatusBarAnimation.Fade);
```

Die `Page.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `Page.SetPrefersStatusBarHidden` Methode in der `Xamarin.Forms.PlatformConfiguration.iOSSpecific` -Namespace wird verwendet, um die Sichtbarkeit der Statusleiste auf festzulegen eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) durch Angabe eines der `StatusBarHiddenMode` Enumerationswerte: `Default`, `True` , oder `False`. Die `StatusBarHiddenMode.True` und `StatusBarHiddenMode.False` Werte festzulegen, die Sichtbarkeit der Status unabhängig von der geräteausrichtung, und die `StatusBarHiddenMode.Default` Wert Blendet die Statusleiste in einer vertikal compact-Umgebung.

Das Ergebnis ist, die die Sichtbarkeit der Statusleiste auf eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) kann festgelegt werden:

![](ios-images/hide-status-bar.png "Statusleiste Sichtbarkeit plattformspezifische")

> [!NOTE]
> Auf einem [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/), dem angegebenen `StatusBarHiddenMode` Enumerationswert aktualisiert auch die Statusleiste auf alle untergeordneten Seiten. Auf allen anderen [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/)-abgeleiteten Typen, die den angegebenen `StatusBarHiddenMode` Enumerationswert wird nur die Statusleiste auf die aktuelle Seite aktualisieren.

Die `Page.SetPreferredStatusBarUpdateAnimation` Methode wird verwendet, um festzulegen, wie die Statusleiste eintritt oder dieses verlässt die [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) durch Angabe eines der `UIStatusBarAnimation` Enumerationswerte: `None`, `Fade`, oder `Slide`. Wenn die `Fade` oder `Slide` Enumerationswert angegeben wird, ein 0,25 zweites Animation ausgeführt wird, wie die Statusleiste eintritt oder dieses verlässt die `Page`.

<a name="delay_content_touches" />

## <a name="delaying-content-touches-in-a-scrollview"></a>Verzögern Content Workflows in einer ScrollView

Ein impliziter Timer wird immer dann ausgelöst, wenn eine Touch-Geste in beginnt eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) unter iOS und die `ScrollView` entscheidet, basierend auf die Aktion innerhalb der Spanne Timer, sollte die Aktion verarbeiten oder übergeben Sie es an seinen Inhalt. Standardmäßig wird der iOS `ScrollView` Verzögerungen bei der Content-Workflows, aber dadurch können Probleme auftreten, in einigen Fällen kann es bei der `ScrollView` Inhalt ausschlaggebende nicht die Aktion aus, wenn dies erforderlich ist. Aus diesem Grund diese plattformspezifischen steuert, ob eine `ScrollView` behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Es ist in XAML verwendet, durch Festlegen der `ScrollView.ShouldDelayContentTouches` angefügten Eigenschaft, um eine `boolean` Wert:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

Die `ScrollView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die `ScrollView.SetShouldDelayContentTouches` Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/) -Namespace wird verwendet, um Steuerelement gibt an, ob eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) behandelt eine Touch-Geste oder übergibt es an seinen Inhalt. Darüber hinaus die `SetShouldDelayContentTouches` Methode kann verwendet werden, um umzuschalten, verzögern Content berührungen durch Aufrufen der `ShouldDelayContentTouches` Methode zurückgeben soll, ob Inhalt Workflows verzögert werden:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

Das Ergebnis ist, eine [ `ScrollView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ScrollView/) können deaktivieren verzögern Content berührungen, also zu empfangen, die in diesem Szenario die [ `Slider` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Slider/) empfängt die Bewegung anstelle der [ `Detail` ](https://developer.xamarin.com/api/property/Xamarin.Forms.MasterDetailPage.Detail/) auf der Seite die [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/):

[![](ios-images/scrollview-delay-content-touches.png "ScrollView-Verzögerung Inhalt betrifft plattformspezifische")](ios-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView Delay Content Touches Plaform-Specific")

<a name="listview-separatorstyle" />

## <a name="setting-the-separator-style-on-a-listview"></a>Festlegen von den Trennzeichenstil für eine ListView

Diese plattformspezifischen steuert, ob das Trennzeichen zwischen den in Zellen ein [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die gesamte Breite des der `ListView`. Es ist in XAML verwendet, durch Festlegen der [ `ListView.SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SeparatorStyleProperty) angefügte Eigenschaft auf den Wert der [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) Enumeration:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        <ListView ... ios:ListView.SeparatorStyle="FullWidth">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

listView.On<iOS>().SetSeparatorStyle(SeparatorStyle.FullWidth);
```

Die `ListView.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `ListView.SetSeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.ListView.SetSeparatorStyle(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Kontrolle, ob in das Trennzeichen zwischen den Zellen der [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die vollständige Breite der `ListView`, mit der [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) Enumeration, die Bereitstellung von zwei möglicher Werten:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.Default) – Gibt das Standardverhalten für iOS-Trennzeichen. Dies ist das Standardverhalten in Xamarin.Forms.
- [`FullWidth`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle.FullWidth) – Gibt an, dass Trennzeichen von einer Kante gezeichnet werden soll, werden die `ListView` zum anderen.

Das Ergebnis ist, die einem angegebenen [ `SeparatorStyle` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.SeparatorStyle) Wert gilt für die [ `ListView` ](xref:Xamarin.Forms.ListView), die die Breite des Trennzeichens zwischen den Zellen steuert:

![](ios-images/listview-separatorstyle.png "ListView SeparatorStyle plattformspezifische")

> [!NOTE]
> Nachdem Sie der Trennzeichenstil festgelegt wurde, `FullWidth`, es kann nicht geändert werden, an `Default` zur Laufzeit.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Deaktivieren die Farbe der Legacymodus

Einige der Xamarin.Forms-Ansichten bieten eines ältere Farbmodus. In diesem Modus bei der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft der Ansicht nastaven NA hodnotu `false`, die Ansicht überschreibt die Farben, die vom Benutzer mit den systemeigenen Standardfarben für den deaktivierten Zustand festgelegt wird. Für die Abwärtskompatibilität dieser älteren Farbmodus-Kompatibilität für das Standardverhalten für die unterstützten Ansichten bleibt.

Diese plattformspezifischen deaktiviert dieser Modus ältere Farbe, damit Farben, die für eine Sicht vom Benutzer festgelegt bleiben, selbst wenn die Sicht deaktiviert ist. Es ist in XAML verwendet, durch Festlegen der [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) angefügte Eigenschaft zu `false`:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

Die `VisualElement.On<iOS>` Methode gibt an, dass diese plattformspezifischen nur unter iOS ausgeführt wird. Die [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) -Namespace wird verwendet, um Kontrolle, ob die älteren Farbmodus deaktiviert ist. Darüber hinaus die [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.iOS,Xamarin.Forms.VisualElement})) Methode kann verwendet werden, um zurück, ob es sich bei der älteren Farbmodus deaktiviert ist.

Das Ergebnis ist, dass die ältere Farbmodus deaktiviert werden kann, Farben, die für eine Sicht vom Benutzer festgelegt auch bleiben, wenn die Sicht deaktiviert ist:

![](ios-images/legacy-color-mode-disabled.png "Ältere Farbmodus deaktiviert")

> [!NOTE]
> Beim Festlegen einer [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) für eine Sicht, die ältere Farbmodus vollständig ignoriert wird. Weitere Informationen zu visuellen Status finden Sie unter [die Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie die Plattformeigenschaften iOS nutzen, die in Xamarin.Forms integriert sind. Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [iOSSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.iOSSpecific/)
