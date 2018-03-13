---
title: Android-Plattform-Besonderheiten
description: "Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden."
ms.topic: article
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/17/2017
ms.openlocfilehash: 739ee4ebeb3176d23ab1eb911baaab31a26252c4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="android-platform-specifics"></a>Android-Plattform-Besonderheiten

_Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden._

Auf Android-Geräten enthält Xamarin.Forms folgenden Plattform-Angaben:

- Festlegen des Betriebsmodus einer Bildschirmtastatur. Weitere Informationen finden Sie unter [durch Festlegen des Weiche Tastatur Eingabemodus](#soft_input_mode).
- Aktivieren schnelle Bildläufe in einem [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) Weitere Informationen finden Sie unter [aktivieren schnelle Bildläufe in einem ListView](#fastscroll).
- Aktivieren der Streifen zwischen Seiten in einem [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/). Weitere Informationen finden Sie unter [aktivieren Streifen zwischen Seiten in einem TabbedPage](#enable_swipe_paging).
- Steuern der Z-Reihenfolge visueller Elemente zum Bestimmen der Zeichnungsreihenfolge. Weitere Informationen finden Sie unter [steuern die Erhöhung der visuellen Elemente](#elevation).
- Das Deaktivieren der [ `Disappearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) und [ `Appearing` ](https://developer.xamarin.com/api/event/Xamarin.Forms.Page.Appearing/) Seite Lebenszyklusereignisse auf Anhalten und fortsetzen, für Anwendungen, AppCompat verwenden. Weitere Informationen finden Sie unter [Deaktivieren der Disappearing und angezeigten Seite Lebenszyklusereignisse](#disable_lifecycle_events).

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

Diese plattformspezifischen wird verwendet, um die Erhöhung der Rechte oder Z-Reihenfolge der visuellen Elemente auf Anwendungen steuern dieses Ziel API 21 oder größer. Die Erhöhung von ein visuelles Element bestimmt die Zeichnungsreihenfolge mit visuellen Elementen mit höheren Z-Werten occluding visuelle Elemente mit niedrigeren Z-Werten. Sie wird in XAML verwendet, durch Festlegen der `Elevation.Elevation` angefügten Eigenschaft, um eine `boolean` Wert:

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
            <Button Text="Button Above BoxView - Click Me" android:Elevation.Elevation="10"/>
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

Die `Button.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die `Elevation.SetElevation` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/) -Namespace wird verwendet, um die Höhe des visuellen Elements auf einen NULL-Werte zulassen festgelegt `float`. Darüber hinaus die `Elevation.GetElevation` Methode kann verwendet werden, um die Erhöhung der Rechte Wert ein visuelles Element abzurufen.

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

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden. Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen.


## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific/)
- [AndroidSpecific.AppCompat](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat/)
