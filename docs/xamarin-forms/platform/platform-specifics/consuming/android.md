---
title: Android-Plattform-Besonderheiten
description: Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 705c3037505b61bfb364772187e0ce8ead14b27f
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847770"
---
# <a name="android-platform-specifics"></a>Android-Plattform-Besonderheiten

_Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden._

Auf Android-Geräten enthält Xamarin.Forms folgenden Plattform-Angaben:

- Festlegen des Betriebsmodus einer Bildschirmtastatur. Weitere Informationen finden Sie unter [durch Festlegen des Weiche Tastatur Eingabemodus](#soft_input_mode).
- Aktivieren schnelle Bildläufe in einem [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Weitere Informationen finden Sie unter [aktivieren schnelle Bildläufe in einem ListView](#fastscroll).
- Aktivieren der Streifen zwischen Seiten in einem [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Weitere Informationen finden Sie unter [aktivieren Streifen zwischen Seiten in einem TabbedPage](#enable_swipe_paging).
- Steuern der Z-Reihenfolge visueller Elemente zum Bestimmen der Zeichnungsreihenfolge. Weitere Informationen finden Sie unter [steuern die Erhöhung der visuellen Elemente](#elevation).
- Das Deaktivieren der [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) und [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) Seite Lebenszyklusereignisse auf Anhalten und fortsetzen, für Anwendungen, AppCompat verwenden. Weitere Informationen finden Sie unter [Deaktivieren der Disappearing und angezeigten Seite Lebenszyklusereignisse](#disable_lifecycle_events).
- Steuern, ob eine [ `WebView` ](xref:Xamarin.Forms.WebView) gemischte Inhalte anzeigen können. Weitere Informationen finden Sie unter [aktivieren gemischten Inhalt in einem WebView](#webview-mixed-content).
- Festlegen der Eingabemethoden-Editor-Optionen für die Bildschirmtastatur für eine [ `Entry` ](xref:Xamarin.Forms.Entry). Weitere Informationen finden Sie unter [Einstellung Eintrag Eingabemethoden-Editor-Optionen](#entry-imeoptions).
- Deaktivieren Sie ältere Farbe-Modus auf einem unterstützten [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [deaktivieren Farbe Legacymodus](#legacy-color-mode).
- Verwenden der standardauffüllung und Schattenwerte von Android-Schaltflächen. Weitere Informationen finden Sie unter [mithilfe von Android-Schaltflächen](#button-padding-shadow).

<a name="soft_input_mode" />

## <a name="setting-the-soft-keyboard-input-mode"></a>Den Eingabemodus Bildschirmtastatur festlegen

Diese plattformspezifischen wird verwendet, um den Betriebsmodus für einen Bereich zur Eingabe von Bildschirmtastatur festgelegt und in XAML durch Festlegen von belegt die [ `Application.WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty/) -Eigenschaft auf den Wert der [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Enumeration:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

Die `Application.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [ `Application.UseWindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) -Namespace wird verwendet, um die Bildschirmtastatur Eingabebereich Betriebsmodus mit Festlegen der [ `WindowSoftInputModeAdjust` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust/) Enumeration, die Bereitstellung von zwei Werten: [ `Pan` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan/) und [ `Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/). Die `Pan` Wert verwendet die [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) Anpassung-Option, die Größe des Fensters nicht während ein Eingabesteuerelements den Fokus besitzt. Stattdessen werden der Inhalt des Fensters geschwenkt so, dass der aktuelle Fokus die Bildschirmtastatur verdeckt. Die `Resize` Wert verwendet die [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) Anpassung-Option, die Größe des Fensters ändern, während ein Eingabesteuerelements den Fokus, um Platz für die Bildschirmtastatur besitzt.

Das Ergebnis ist, dass die Bildschirmtastatur Bereich Betriebsmodus kann festgelegt werden Eingabe, wenn eine Eingabe Steuerelement den Fokus hat:

[![](android-images/pan-resize.png "Bildschirmtastatur funktioniert Modus plattformspezifischen")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="fastscroll" />

## <a name="enabling-fast-scrolling-in-a-listview"></a>Aktivieren schnelle Bildläufe in einem ListView

Diese plattformspezifischen wird verwendet, um schnelle Durchführen eines Bildlaufs durch die Daten im Aktivieren einer [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Sie wird in XAML verwendet, durch Festlegen der `ListView.IsFastScrollEnabled` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

Die `ListView.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die `ListView.SetIsFastScrollEnabled` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) -Namespace wird verwendet, um schnelle Durchführen eines Bildlaufs durch die Daten im Aktivieren einer [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/). Darüber hinaus die `SetIsFastScrollEnabled` Methode kann verwendet werden, um umzuschalten, schnelle Bildläufe durch Aufrufen der `IsFastScrollEnabled` -Methode zurückgegeben wird, ob schnelle Bildlauf aktiviert ist:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Das Ergebnis ist, schnell einen Bildlauf durch die Daten in einem [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) kann aktiviert werden, die ändert der Größe des Ziehpunkts Scroll:

[![](android-images/fastscroll.png "ListView FastScroll plattformspezifischen")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="enable_swipe_paging" />

## <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Streifen zwischen Seiten in einem TabbedPage aktivieren

Diese plattformspezifischen wird verwendet, um aktivieren Streifen mit einem horizontalen Finger Bewegung zwischen Seiten in einem [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Sie wird in XAML verwendet, durch Festlegen der [ `TabbedPage.IsSwipePagingEnabled` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty/) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

Die `TabbedPage.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [ `TabbedPage.SetIsSwipePagingEnabled` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled/p/Xamarin.Forms.BindableObject/System.Boolean/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) -Namespace wird verwendet, um zwischen Seiten in ein Lesegerät Aktivieren einer [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Darüber hinaus die `TabbedPage` -Klasse in der `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` Namespace verfügt auch über eine [ `EnableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) Methode, die diese plattformspezifische ermöglicht und eine [ `DisableSwipePaging` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging/p/Xamarin.Forms.IPlatformElementConfiguration%7BXamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage%7D/) -Methode, die deaktiviert Diese plattformspezifischen. Die [ `TabbedPage.OffscreenPageLimit` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty/) angefügten Eigenschaft, und [ `SetOffscreenPageLimit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit/p/Xamarin.Forms.BindableObject/System.Int32/) -Methode werden verwendet, um die Anzahl der Seiten festzulegen, die im Leerlauf auf beiden Seiten der aktuellen Seite beibehalten werden sollen.

Das Ergebnis ist, Wischen Paging durch die Seiten angezeigt werden, indem eine [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) aktiviert ist:

![](android-images/tabbedpage-swipe.png)

<a name="elevation" />

## <a name="controlling-the-elevation-of-visual-elements"></a>Steuern die Erhöhung von visuellen Elementen

Diese plattformspezifischen wird verwendet, um die Erhöhung der Rechte oder Z-Reihenfolge der visuellen Elemente auf Anwendungen steuern dieses Ziel API 21 oder größer. Die Erhöhung von ein visuelles Element bestimmt die Zeichnungsreihenfolge mit visuellen Elementen mit höheren Z-Werten occluding visuelle Elemente mit niedrigeren Z-Werten. Sie wird in XAML verwendet, durch Festlegen der `VisualElement.Elevation` angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             Title="Elevation">
    <StackLayout>
        <Grid>
            <Button Text="Button Beneath BoxView" />
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>        
        <Grid Margin="0,20,0,0">
            <Button Text="Button Above BoxView - Click Me" android:VisualElement.Elevation="10"/>
            <BoxView Color="Red" Opacity="0.2" HeightRequest="50" />
        </Grid>
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

public class AndroidElevationPageCS : ContentPage
{
    public AndroidElevationPageCS()
    {
        ...
        var aboveButton = new Button { Text = "Button Above BoxView - Click Me" };
        aboveButton.On<Android>().SetElevation(10);

        Content = new StackLayout
        {
            Children =
            {
                new Grid
                {
                    Children =
                    {
                        new Button { Text = "Button Beneath BoxView" },
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                },
                new Grid
                {
                    Margin = new Thickness(0,20,0,0),
                    Children =
                    {
                        aboveButton,
                        new BoxView { Color = Color.Red, Opacity = 0.2, HeightRequest = 50 }
                    }
                }
            }
        };
    }
}
```

Die `Button.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die `VisualElement.SetElevation` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) -Namespace wird verwendet, um die Höhe des visuellen Elements auf einen NULL-Werte zulassen festgelegt `float`. Darüber hinaus die `VisualElement.GetElevation` Methode kann verwendet werden, um die Erhöhung der Rechte Wert ein visuelles Element abzurufen.

Das Ergebnis ist, dass die Erhöhung der visuellen Elemente gesteuert werden kann, sodass visuelle Elemente mit höheren Z-Werten visuelle Elemente mit niedrigeren Z-Werten verdeckt. Aus diesem Grund in diesem Beispiel wird die zweite [ `Button` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Button/) oben gerendert wird die [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/) da sie einen höheren Wert für die Erhöhung der Rechte verfügt:

![](android-images/elevation.png)

<a name="disable_lifecycle_events" />

## <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Deaktivieren die Lebenszyklusereignisse des verschwinden und angezeigten Seite

Diese plattformspezifischen dient zum Deaktivieren der [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) und [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) Page-Ereignisse für die Anwendung anhalten und Fortsetzen der jeweils für Anwendungen, AppCompat verwenden. Darüber hinaus enthält es die Möglichkeit zu steuern, ob die Bildschirmtastatur auf fortsetzen, angezeigt wird, wenn er auf Pause "," angezeigt wurde, vorausgesetzt, dass der Betriebsmodus der Bildschirmtastatur auf festgelegt ist [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

> [!NOTE]
> Beachten Sie, dass diese Ereignisse standardmäßig aktiviert sind, bereits bestehendem Verhalten für Anwendungen beibehalten, die die Ereignisse abhängig. Das Deaktivieren dieser Ereignisse macht AppCompat dieses Ereigniszyklus vor AppCompat dieses Ereigniszyklus übereinstimmen.

Diese plattformspezifischen in XAML verwendet werden kann, durch Festlegen der [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty/), [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty/), und [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty/) angefügte Eigenschaften zu `boolean` Werte:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"             xmlns:androidAppCompat="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize"
             androidAppCompat:Application.SendDisappearingEventOnPause="false"
             androidAppCompat:Application.SendAppearingEventOnResume="false"
             androidAppCompat:Application.ShouldPreserveKeyboardOnResume="true">
  ...
</Application>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;
...

Xamarin.Forms.Application.Current.On<Android>()
     .UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize)
     .SendDisappearingEventOnPause(false)
     .SendAppearingEventOnResume(false)
     .ShouldPreserveKeyboardOnResume(true);
```

Die `Application.Current.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [ `Application.SendDisappearingEventOnPause` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/) -Namespace dient zum Aktivieren oder Deaktivieren von Auslösen von Ereignissen der [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) Page-Ereignisklasse, wenn die Anwendung Wechselt in den Hintergrund. Die [ `Application.SendAppearingEventOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Methode dient zum Aktivieren oder Deaktivieren von Auslösen von Ereignissen der [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) Page-Ereignisklasse, wenn die Anwendung aus dem Hintergrund zurückkehrt. Die [ `Application.ShouldPreserveKeyboardOnResume` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application}/System.Boolean/) Methode wird verwendet, Steuerelement, ob der Bildschirmtastatur auf fortsetzen, angezeigt wird, wenn er auf Pause "," angezeigt wurde bereitgestellt, dass der Betriebsmodus der Bildschirmtastatur festgelegt ist, um [ `WindowSoftInputModeAdjust.Resize` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize/).

Das Ergebnis ist, die [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) und [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) Page-Ereignisse wird nicht ausgelöst werden, auf die Anwendung anhalten und fortsetzen bzw. und, auch wenn die Bildschirmtastatur wurde angezeigt wird, wenn die Anwendung wurde angehalten, es wird auch angezeigt, wenn die Anwendung wird fortgesetzt:

[![](android-images/keyboard-on-resume.png "Lebenszyklus Ereignisse plattformspezifischen")](android-images/keyboard-on-resume-large.png#lightbox "Lebenszyklus Ereignisse plattformspezifischen")

<a name="webview-mixed-content" />

## <a name="enabling-mixed-content-in-a-webview"></a>Gemischten Inhalt in einem WebView aktivieren

Diese plattformspezifischen steuert, ob eine [ `WebView` ](xref:Xamarin.Forms.WebView) können gemischten Inhalt anzeigen in Anwendungen, die auf die API 21 oder höher abzielen. Gemischter Inhalt ist, das über eine HTTPS-Verbindung anfänglich geladen wird, aber die Ressourcen (z. B. Bildern, Audio, Video, Stylesheets, Skripts) über eine HTTP-Verbindung lädt. Sie wird in XAML verwendet, durch Festlegen der [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) -Eigenschaft auf den Wert der [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Enumeration:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

Die `WebView.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob gemischte Inhalte angezeigt werden kann, mit der [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Enumeration, die Bereitstellung von drei möglicher Werten:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – Gibt an, dass die [ `WebView` ](xref:Xamarin.Forms.WebView) lässt HTTPS Ursprung aus einem Ursprung der HTTP-Inhalt geladen.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – Gibt an, dass die [ `WebView` ](xref:Xamarin.Forms.WebView) lässt sich nicht auf HTTPS Ursprung aus einem Ursprung der HTTP-Inhalt geladen.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – Gibt an, dass die [ `WebView` ](xref:Xamarin.Forms.WebView) wird versucht, mit dem Ansatz, der die aktuelle Webbrowser des Geräts kompatibel sein. Einige HTTP-Inhalt kann von einer HTTPS-Ursprung geladen werden darf, und anderen Typen von Inhalt werden blockiert. Die Typen von Inhalten, die zugelassen oder blockiert werden, können mit jeder Version des Betriebssystems ändern.

Das Ergebnis ist, die ein angegebenes [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Wert angewendet wird, um die [ `WebView` ](xref:Xamarin.Forms.WebView), die steuert, ob gemischte Inhalte angezeigt werden kann:

[![WebView gemischten Inhalt Behandlung plattformspezifischen](android-images/webview-mixedcontent.png "WebView gemischten Inhalt Behandlung plattformspezifischen")](android-images/webview-mixedcontent-large.png#lightbox "WebView gemischten Inhalt Behandlung plattformspezifischen")

<a name="entry-imeoptions" />

## <a name="setting-entry-input-method-editor-options"></a>Einstellung Eintrag Eingabemethoden-Editor-Optionen

Diese plattformspezifischen legt die Eingabemethode-Editor (IME)-Optionen für die Bildschirmtastatur für eine [ `Entry` ](xref:Xamarin.Forms.Entry). Dies schließt die Aktionsschaltfläche Benutzer festlegen, in der unteren Ecke der Bildschirmtastatur und die Interaktionen mit der `Entry`. Sie wird in XAML verwendet, durch Festlegen der [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) -Eigenschaft auf den Wert der [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Enumeration:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

Die `Entry.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Eingabemethode Aktion Option für die Bildschirmtastatur für die [ `Entry` ](xref:Xamarin.Forms.Entry), mit der [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Enumeration, die die folgenden Werte:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – Gibt an, dass keine bestimmte aktionsschlüssel ist erforderlich, dass das zugrunde liegende Steuerelement selbst, wenn erzeugt werden können. Diese werden entweder `Next` oder `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – Gibt an, dass kein aktionsschlüssel zur Verfügung gestellt werden.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – Zeigt an, dass der aktionsschlüssel führt einen Vorgang "go" eingegebener benötigt des Benutzers an das Ziel des Texts.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Gibt an, dass der aktionsschlüssel einen "Suchen"-Vorgang ausführt, benötigt des Benutzers die Ergebnisse der Suche nach dem sie eingegeben haben.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Gibt an, dass der aktionsschlüssel eine "Senden" an, der den Text an ihr Ziel übermittelt ausgeführt werden.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Gibt an, dass der aktionsschlüssel führt eine "Weiter" an, wobei des Benutzers in das nächste Feld, von denen Text akzeptiert.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Gibt an, dass der aktionsschlüssel eine "fertige" an, und schließen die Bildschirmtastatur ausgeführt werden.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Gibt an, dass der aktionsschlüssel führt eine "frühere" an, wobei des Benutzers zum vorherigen Feld, von denen Text akzeptiert.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – die Maske Aktionsoptionen auswählen.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – Gibt an, dass die Rechtschreibprüfung wird weder vom Benutzer Informationen noch Korrekturvorschläge basierend auf was der Benutzer zuvor eingegeben hat.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – Gibt an, dass die Benutzeroberfläche nicht Fullscreen eingefügt werden sollen.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – Gibt an, dass keine Benutzeroberfläche für die extrahierten Text angezeigt wird.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – Gibt an, dass keine Benutzeroberfläche für benutzerdefinierte Aktionen angezeigt werden.

Das Ergebnis ist, die ein angegebenes [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Wert angewendet wird, um die Bildschirmtastatur für die [ `Entry` ](xref:Xamarin.Forms.Entry), wodurch die Eingabemethoden-Editor-Optionen:

[![Eintrag Eingabe Methode Editor plattformspezifischen](android-images/entry-imeoptions.png "Eintrag Eingabe Methode Editor plattformspezifischen")](android-images/entry-imeoptions-large.png#lightbox "Eintrag Eingabe Methode Editor plattformspezifischen")

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Deaktivieren die Vorgängerversion Färbung aktiviert

Einige der Xamarin.Forms Ansichten an eine ältere Färbung aktiviert Funktionen. In diesem Modus kann bei der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft der Ansicht auf festgelegt ist `false`, die Sicht werden die Farben, die vom Benutzer mit der systemeigenen Standardfarben für den deaktivierten Zustand überschrieben. Für die Abwärtskompatibilität Kompatibilität diese ältere Färbung aktiviert das Standardverhalten für die unterstützten Ansichten bleibt.

Diese plattformspezifischen wird dieser älteren Färbung aktiviert, deaktiviert, sodass Farben für eine Sicht vom Benutzer festgelegt bleiben, auch wenn die Sicht deaktiviert ist. Sie wird in XAML verwendet, durch Festlegen der [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) -Eigenschaft auf `false`:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                android:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

Die `VisualElement.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob die legacy-Farben-Modus deaktiviert ist. Darüber hinaus die [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) Methode kann verwendet werden, um zurück, ob die ältere Farbe Modus deaktiviert ist.

Das Ergebnis ist, dass die ältere Färbung aktiviert deaktiviert werden kann, Farben, die vom Benutzer für eine Sicht festgelegt selbst bleiben, wenn die Sicht deaktiviert ist:

![](android-images/legacy-color-mode-disabled.png "Ältere Farbe Modus deaktiviert")

> [!NOTE]
> Beim Festlegen einer [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) für eine Sicht, die ältere Färbung aktiviert vollständig ignoriert. Weitere Informationen zu visuelle Zustände, finden Sie unter [der Xamarin.Forms visuellen Status-Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="button-padding-shadow" />

## <a name="using-android-buttons"></a>Verwenden von Android-Schaltflächen

Diese plattformspezifischen steuert, ob es sich bei Xamarin.Forms Schaltflächen die standardauffüllung und den Volumeschattenkopie-Werte von Android Schaltflächen verwenden. Sie wird in XAML verwendet, durch Festlegen der [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) und [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) angefügten Eigenschaften `boolean` Werte:

```xaml
<ContentPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button ...
                android:Button.UseDefaultPadding="true"
                android:Button.UseDefaultShadow="true" />         
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

Die `Button.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) und[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) Methoden in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace dienen zum Steuerelement, ob standardmäßig die Schaltflächen der Xamarin.Forms verwenden Auffüllung und Schattenwerte Android Schaltflächen. Darüber hinaus die [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) und [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) Methoden können verwendet werden, um zurück, ob eine Schaltfläche padding-Wert und der Standardwert für Schattenkopien, jeweils standardmäßig verwendet.

Das Ergebnis ist, dass Xamarin.Forms Schaltflächen die standardauffüllung und den Volumeschattenkopie-Werte von Android Schaltflächen verwenden können:

![](android-images/button-padding-and-shadow.png "Ältere Farbe Modus deaktiviert")

Beachten Sie, dass im Screenshot oben jedes [ `Button` ](xref:Xamarin.Forms.Button) verfügt über identische Definitionen, außer dass die rechten `Button` verwendet die standardauffüllung und Schattenwerte von Android-Schaltflächen.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden. Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
