---
title: Android Plattformeigenschaften
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert sind.
ms.prod: xamarin
ms.assetid: C5D4AA65-9BAA-4008-8A1E-36CDB78A435D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/03/2018
ms.openlocfilehash: c422b9ac5af9417523f349537fda1bb0c01aa7bc
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "39175176"
---
# <a name="android-platform-specifics"></a>Android Plattformeigenschaften

_Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert sind._

## <a name="visualelements"></a>VisualElements

Unter Android werden die folgende plattformspezifische Funktionalität für Xamarin.Forms-Ansichten, Seiten und Layouts bereitgestellt:

- Steuern der Z-Reihenfolge der visuellen Elemente um Zeichnungsreihenfolge zu bestimmen. Weitere Informationen finden Sie unter [steuern die Erhöhung der visuellen Elemente](#elevation).
- Deaktivieren auf einer unterstützten legacy Farbmodus [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [deaktivieren Farbe Legacymodus](#legacy-color-mode).

<a name="elevation" />

### <a name="controlling-the-elevation-of-visual-elements"></a>Steuern die Erhöhung von visuellen Elementen

Diese plattformspezifischen wird verwendet, um die Erhöhung der Rechte oder Z-Reihenfolge der visuellen Elemente auf Anwendungen steuern, die auf API 21 oder höher. Die Höhe ein visuelles Element bestimmt die Zeichnungsreihenfolge mit visuellen Elementen mit höheren Z-Werten, die occluding visuelle Elemente mit niedrigeren Z-Werte. Es ist in XAML verwendet, durch Festlegen der `VisualElement.Elevation` angefügten Eigenschaft, um eine `boolean` Wert:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

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

Die `Button.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `VisualElement.SetElevation` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Höhe des visuellen Elements auf eine auf NULL festlegbare festgelegt `float`. Darüber hinaus die `VisualElement.GetElevation` Methode kann verwendet werden, um die Erhöhung der Rechte Wert ein visuelles Element abgerufen werden soll.

Das Ergebnis ist, dass die Höhe der visuellen Elemente so, dass visuelle Elemente mit höheren Z-Werten visuelle Elemente mit niedrigeren Z-Werten verdeckt gesteuert werden kann. Aus diesem Grund in diesem Beispiel wird die zweite [ `Button` ](xref:Xamarin.Forms.Button) gerendert wird, über die [ `BoxView` ](xref:Xamarin.Forms.BoxView) da sie einen höheren Wert für die Erhöhung der Rechte verfügt:

![](android-images/elevation.png)

<a name="legacy-color-mode" />

### <a name="disabling-legacy-color-mode"></a>Deaktivieren die Farbe der Legacymodus

Einige der Xamarin.Forms-Ansichten bieten eines ältere Farbmodus. In diesem Modus bei der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft der Ansicht nastaven NA hodnotu `false`, die Ansicht überschreibt die Farben, die vom Benutzer mit den systemeigenen Standardfarben für den deaktivierten Zustand festgelegt wird. Für die Abwärtskompatibilität dieser älteren Farbmodus-Kompatibilität für das Standardverhalten für die unterstützten Ansichten bleibt.

Diese plattformspezifischen deaktiviert dieser Modus ältere Farbe, damit Farben, die für eine Sicht vom Benutzer festgelegt bleiben, selbst wenn die Sicht deaktiviert ist. Es ist in XAML verwendet, durch Festlegen der [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.IsLegacyColorModeEnabledProperty) angefügte Eigenschaft zu `false`:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

_legacyColorModeDisabledButton.On<Android>().SetIsLegacyColorModeEnabled(false);
```

Die `VisualElement.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob die älteren Farbmodus deaktiviert ist. Darüber hinaus die [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.VisualElement})) Methode kann verwendet werden, um zurück, ob es sich bei der älteren Farbmodus deaktiviert ist.

Das Ergebnis ist, dass die ältere Farbmodus deaktiviert werden kann, Farben, die für eine Sicht vom Benutzer festgelegt auch bleiben, wenn die Sicht deaktiviert ist:

![](android-images/legacy-color-mode-disabled.png "Ältere Farbmodus deaktiviert")

> [!NOTE]
> Beim Festlegen einer [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) für eine Sicht, die ältere Farbmodus vollständig ignoriert wird. Weitere Informationen zu visuellen Status finden Sie unter [die Xamarin.Forms Visual State Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="views"></a>Ansichten

Unter Android werden die folgende plattformspezifische Funktionalität für Xamarin.Forms-Ansichten bereitgestellt:

- Verwenden die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen. Weitere Informationen finden Sie unter [Using Android Buttons](#button-padding-shadow).
- Festlegen der Eingabemethoden-Editor-Optionen für die Bildschirmtastatur für eine [ `Entry` ](xref:Xamarin.Forms.Entry). Weitere Informationen finden Sie unter [Einstellung Eintrag Eingabemethoden-Editor-Optionen](#entry-imeoptions).
- Aktivieren schnelle Bildläufe eine [ `ListView` ](xref:Xamarin.Forms.ListView) Weitere Informationen finden Sie unter [ermöglichen schnelle Bildlauf in einer ListView](#fastscroll).
- Steuern, ob eine [ `WebView` ](xref:Xamarin.Forms.WebView) gemischte Inhalte anzeigen können. Weitere Informationen finden Sie unter [aktivieren gemischter Inhalt in einer WebView](#webview-mixed-content).

<a name="button-padding-shadow" />

### <a name="using-android-buttons"></a>Verwenden von Android-Schaltflächen

Diese plattformspezifischen steuert, ob es sich bei Xamarin.Forms-Schaltflächen die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen verwenden. Es ist in XAML verwendet, durch Festlegen der [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPaddingProperty) und [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadowProperty) angefügte Eigenschaften zu `boolean` Werte:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

button.On<Android>().SetUseDefaultPadding(true).SetUseDefaultShadow(true);
```

Die `Button.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `Button.SetUseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) und[ `Button.SetUseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.SetUseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button},System.Boolean)) Methoden in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace werden verwendet, um Kontrolle, ob Schaltflächen von Xamarin.Forms die Standardeinstellung verwenden Abstand und Schattenwerte von Android-Schaltflächen. Darüber hinaus die [ `Button.UseDefaultPadding` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultPadding(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) und [ `Button.UseDefaultShadow` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Button.UseDefaultShadow(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Button})) Methoden können verwendet werden, um zurück, ob eine Schaltfläche bzw. padding-Wert und Standardwert Schattenkopien zugewiesen wurde, verwendet.

Das Ergebnis ist, dass Xamarin.Forms-Schaltflächen die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen verwenden können:

![](android-images/button-padding-and-shadow.png "Ältere Farbmodus deaktiviert")

Beachten Sie, dass im Screenshot oben jedes [ `Button` ](xref:Xamarin.Forms.Button) verfügt über identische Definitionen, außer dass die rechten `Button` verwendet die standardauffüllung und den Volumeschattenkopie-Werte, der Android-Schaltflächen.

<a name="entry-imeoptions" />

### <a name="setting-entry-input-method-editor-options"></a>Einstellung Eintrag Eingabemethoden-Editor-Optionen

Diese plattformspezifischen legt die Eingabemethode-Editor (IME) Optionen für die Bildschirmtastatur für eine [ `Entry` ](xref:Xamarin.Forms.Entry). Dies beinhaltet das Festlegen der Schaltfläche "Benutzer-Aktion" in der unteren Ecke die Bildschirmtastatur und die Interaktionen mit der `Entry`. Es ist in XAML verwendet, durch Festlegen der [ `Entry.ImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.ImeOptionsProperty) angefügte Eigenschaft auf den Wert der [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Enumeration:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout ...>
        <Entry ... android:Entry.ImeOptions="Send" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

entry.On<Android>().SetImeOptions(ImeFlags.Send);
```

Die `Entry.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `Entry.SetImeOptions` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Entry.SetImeOptions(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Entry},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Eingabemethode Action-Option für die Bildschirmtastatur für Festlegen der [ `Entry` ](xref:Xamarin.Forms.Entry), mit der [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Enumeration, die die folgenden Werte:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Default) – Gibt an, dass keine bestimmte Aktion-Schlüssel ist erforderlich, dass das zugrunde liegende Steuerelement selbst, wenn erzeugt werden können. Entweder `Next` oder `Done`.
- [`None`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.None) – Gibt an, dass kein aktionsschlüssel zur Verfügung gestellt wird.
- [`Go`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Go) – Gibt an, dass der aktionsschlüssel führt einen Vorgang "go", um den Benutzer an das Ziel des Texts Eingabe.
- [`Search`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Search) – Gibt an, dass der aktionsschlüssel wird eine Operation mit "Suche", um den Benutzer auf die Ergebnisse der Suche nach dem sie eingegeben haben.
- [`Send`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Send) – Gibt an, dass der aktionsschlüssel einen "Senden"-Vorgang, auf der Bereitstellung von den Text an das Ziel ausgeführt wird.
- [`Next`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Next) – Gibt an, dass der aktionsschlüssel wird einen "Weiter" Vorgang ausführen, um den Benutzer zum nächsten Feld, das Text akzeptiert.
- [`Done`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Done) – Gibt an, dass der aktionsschlüssel einen "erledigt"-Vorgang schließen die Bildschirmtastatur ausgeführt werden.
- [`Previous`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.Previous) – Gibt an, dass der aktionsschlüssel wird einen "früheren" Vorgang ausführen, um den Benutzer zum vorherigen Feld, das Text akzeptiert.
- [`ImeMaskAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.ImeMaskAction) – der Maske zur Aktionsoptionen auswählen.
- [`NoPersonalizedLearning`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoPersonalizedLearning) – Gibt an, dass die Rechtschreibprüfung wird weder lernen Sie von der Benutzer noch basierte auf was der Benutzer zuvor eingegeben hat Korrekturen vorzuschlagen.
- [`NoFullscreen`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoFullscreen) – Gibt an, dass die Benutzeroberfläche nicht im Vollbildmodus gespeichert werden sollen.
- [`NoExtractUi`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoExtractUi) – Gibt an, dass keine Benutzeroberfläche für den extrahierten Text angezeigt wird.
- [`NoAccessoryAction`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags.NoAccessoryAction) – Gibt an, dass keine Benutzeroberfläche für benutzerdefinierte Aktionen angezeigt wird.

Das Ergebnis ist, die einem angegebenen [ `ImeFlags` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ImeFlags) Wert angewendet wird, um die Bildschirmtastatur für die [ `Entry` ](xref:Xamarin.Forms.Entry), die den Eingabemethoden-Editor-Optionen festlegt:

[![Eintrag input Methode Editor plattformspezifische](android-images/entry-imeoptions.png "Eintrag input Methode Editor plattformspezifische")](android-images/entry-imeoptions-large.png#lightbox "Eintrag input Methode Editor plattformspezifische")

<a name="fastscroll" />

### <a name="enabling-fast-scrolling-in-a-listview"></a>Aktivieren schnelle Bildläufe in einem ListView

Diese plattformspezifischen wird verwendet, um schnell einen Bildlauf durch die Daten im ermöglichen eine [ `ListView` ](xref:Xamarin.Forms.ListView). Es ist in XAML verwendet, durch Festlegen der `ListView.IsFastScrollEnabled` angefügten Eigenschaft, um eine `boolean` Wert:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

Die `ListView.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `ListView.SetIsFastScrollEnabled` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um schnell einen Bildlauf durch die Daten im ermöglichen eine [ `ListView` ](xref:Xamarin.Forms.ListView). Darüber hinaus die `SetIsFastScrollEnabled` Methode kann verwendet werden, um umzuschalten, schnelle Bildläufe durch Aufrufen der `IsFastScrollEnabled` Methode zurückgeben soll, ob schnelle Bildlauf aktiviert ist:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Das Ergebnis ist, schnell einen Bildlauf durch die Daten in einem [ `ListView` ](xref:Xamarin.Forms.ListView) kann aktiviert werden, welche Änderungen es sich um die Größe des bildlaufziehpunkts:

[![](android-images/fastscroll.png "ListView FastScroll plattformspezifische")](android-images/fastscroll-large.png#lightbox "ListView FastScroll Plaform-Specific")

<a name="webview-mixed-content" />

### <a name="enabling-mixed-content-in-a-webview"></a>Gemischten Inhalt in einer WebView aktivieren

Diese plattformspezifischen steuert, ob eine [ `WebView` ](xref:Xamarin.Forms.WebView) können gemischten Inhalt anzeigen in Anwendungen, die auf API 21 oder höher abzielen. Gemischte Inhalte sind Inhalte, die anfänglich über eine HTTPS-Verbindung geladen, aber die Ressourcen (z. B. Bilder, Audio, Video, Stylesheets, Skripts) über eine HTTP-Verbindung lädt. Es ist in XAML verwendet, durch Festlegen der [ `WebView.MixedContentMode` ](x:ref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) angefügte Eigenschaft auf den Wert der [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Enumeration:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

Die `WebView.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `WebView.SetMixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob die gemischte Inhalte angezeigt werden kann, mit der [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Enumeration, die Bereitstellung von drei möglicher Werten:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – Gibt an, dass die [ `WebView` ](xref:Xamarin.Forms.WebView) können einen HTTPS-Ursprung zum Laden von Inhalten aus einer HTTP-Ursprung.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – Gibt an, dass die [ `WebView` ](xref:Xamarin.Forms.WebView) lässt sich nicht auf einen HTTPS-Ursprung zum Laden von Inhalten aus einer HTTP-Ursprung.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – Gibt an, dass die [ `WebView` ](xref:Xamarin.Forms.WebView) wird versucht, mit dem die aktuelle Webbrowser-Ansatz kompatibel sein. Einige HTTP-Inhalt berechtigt, auf die von einer HTTPS-Ursprung geladen werden, und andere Arten von Inhalten blockiert werden. Die Typen von Inhalten, die blockiert oder zugelassen werden, können mit jeder Version des Betriebssystems ändern.

Das Ergebnis ist, die einem angegebenen [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Wert gilt für die [ `WebView` ](xref:Xamarin.Forms.WebView), die steuert, ob gemischte Inhalte angezeigt werden kann:

[![WebView gemischten plattformspezifische für die Verarbeitung von Inhalten](android-images/webview-mixedcontent.png "WebView gemischten plattformspezifische für die Verarbeitung von Inhalten")](android-images/webview-mixedcontent-large.png#lightbox "WebView gemischten plattformspezifische für die Verarbeitung von Inhalten")

## <a name="pages"></a>Seiten

Unter Android werden die folgende plattformspezifische Funktionalität für Xamarin.Forms-Seiten bereitgestellt:

- Festlegen der Höhe der Navigationsleiste auf einen [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Weitere Informationen finden Sie unter [Festlegen der Höhe der Leiste Navigation auf einer "NavigationPage"](#navigationpage-barheight).
- Aktivieren, Wischen zwischen den Seiten einer [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter [aktivieren Streifen zwischen Seiten in einer "tabbedpage"](#enable_swipe_paging).
- Festlegen der Platzierung der Symbolleiste und die Farbe für eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Weitere Informationen finden Sie unter [Einstellung "tabbedpage" Symbolleiste Platzierung und die Farbe](#tabbedpage-toolbar).

<a name="navigationpage-barheight" />

### <a name="setting-the-navigation-bar-height-on-a-navigationpage"></a>Festlegen der Navigationsleiste auf die Höhe auf einer "NavigationPage"

Diese plattformspezifischen legt die Höhe der Navigationsleiste auf einen [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Es ist in XAML verwendet, durch Festlegen der [ `NavigationPage.BarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.BarHeightProperty) bindbare Eigenschaft in einen ganzzahligen Wert:

```xaml
<NavigationPage ...
                xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat;assembly=Xamarin.Forms.Core"
                android:NavigationPage.BarHeight="450">
    ...
</NavigationPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

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

Die `NavigationPage.On<Android>` Methode angibt, dass diese plattformspezifischen nur auf die Anwendungskompatibilität Android ausgeführt wird. Die [ `NavigationPage.SetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.SetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage},System.Int32)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) -Namespace wird verwendet, um die Höhe der Navigationsleiste auf Festlegen einer [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage). Darüber hinaus die [ `NavigationPage.GetBarHeight` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.NavigationPage.GetBarHeight(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.NavigationPage})) Methode kann verwendet werden, um die Höhe der Navigationsleiste in zurückzugeben der `NavigationPage`.

Das Ergebnis ist, die die Höhe der Navigationsleiste auf einen [ `NavigationPage` ](xref:Xamarin.Forms.NavigationPage) kann festgelegt werden:

![](android-images/navigationpage-barheight.png "\"NavigationPage\" Navigation Balkenhöhe")

<a name="enable_swipe_paging" />

### <a name="enabling-swiping-between-pages-in-a-tabbedpage"></a>Aktivieren das Streifen zwischen Seiten in einer "tabbedpage"

Diese plattformspezifischen wird verwendet, um ermöglichen ein Lesegerät mit einer horizontalen Finger Bewegung zwischen Seiten in einem [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Es ist in XAML verwendet, durch Festlegen der [ `TabbedPage.IsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.IsSwipePagingEnabledProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.OffscreenPageLimit="2"
            android:TabbedPage.IsSwipePagingEnabled="true">
    ...
</TabbedPage>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetOffscreenPageLimit(2)
             .SetIsSwipePagingEnabled(true);
```

Die `TabbedPage.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `TabbedPage.SetIsSwipePagingEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetIsSwipePagingEnabled(Xamarin.Forms.BindableObject,System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um zwischen Seiten in ein Lesegerät zu aktivieren einer [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Darüber hinaus die `TabbedPage` -Klasse in der `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` Namespace verfügt auch über eine [ `EnableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.EnableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) Methode, die diese plattformspezifischen ermöglicht und eine [ `DisableSwipePaging` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.DisableSwipePaging(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) -Methode, die deaktiviert Diese plattformspezifischen. Die [ `TabbedPage.OffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.OffscreenPageLimitProperty) angefügten Eigenschaft, und [ `SetOffscreenPageLimit` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetOffscreenPageLimit(Xamarin.Forms.BindableObject,System.Int32)) -Methode werden verwendet, um die Anzahl der Seiten festlegen, die im Leerlauf auf beiden Seiten der aktuellen Seite beibehalten werden sollen.

Das Ergebnis ist, wischbewegungen Paging durch die Seiten angezeigt werden, indem eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) aktiviert ist:

![](android-images/tabbedpage-swipe.png)

<a name="tabbedpage-toolbar" />

### <a name="setting-tabbedpage-toolbar-placement-and-color"></a>Festlegen von "tabbedpage" Symbolleiste Platzierung und Farbe

Diese Plattformeigenschaften werden verwendet, um die Position und die Farbe der Symbolleiste für Festlegen einer [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage). Durch Festlegen von in XAML verwendet werden die [ `TabbedPage.ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.ToolbarPlacementProperty) angefügte Eigenschaft auf den Wert der [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) Enumeration, und die [ `TabbedPage.BarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarItemColorProperty) und [ `TabbedPage.BarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.BarSelectedItemColorProperty) angefügte Eigenschaften zu einem [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<TabbedPage ...
            xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
            android:TabbedPage.ToolbarPlacement="Bottom"
            android:TabbedPage.BarItemColor="Black"
            android:TabbedPage.BarSelectedItemColor="Red">
    ...
</TabbedPage>
```

Alternativ können sie aus c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

On<Android>().SetToolbarPlacement(ToolbarPlacement.Bottom)
             .SetBarItemColor(Color.Black)
             .SetBarSelectedItemColor(Color.Red);
```

Die `TabbedPage.On<Android>` Methode gibt an, dass diese Plattformeigenschaften nur unter Android ausgeführt werden. Die [ `TabbedPage.SetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Symbolleiste-Position festgelegt werden, auf eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage), mit der [ `ToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement) Enumeration, die die folgenden Werte:

- [`Default`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Default) – Gibt an, dass die Symbolleiste am Standardspeicherort auf der Seite platziert ist. Dies ist die oberen Rand der Seite auf Smartphones und dem unteren Rand der Seite auf andere Idiome Gerät.
- [`Top`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Top) – Gibt an, dass die Symbolleiste am oberen Rand der Seite platziert ist.
- [`Bottom`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.ToolbarPlacement.Bottom) – Gibt an, dass die Symbolleiste am unteren Rand der Seite platziert ist.

Darüber hinaus die [ `TabbedPage.SetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) und [ `TabbedPage.SetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.SetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage},Xamarin.Forms.Color)) Methoden werden verwendet, um die Farbe der Symbolleistenelemente und ausgewählte Symbolleistenelemente, bzw. festzulegen.

> [!NOTE]
> Die [ `GetToolbarPlacement` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetToolbarPlacement(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), [ `GetBarItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})), und [ `GetBarSelectedItemColor` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.TabbedPage.GetBarSelectedItemColor(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.TabbedPage})) Methoden können verwendet werden, um die Position und die Farbe des Abrufen der [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage) Symbolleiste.

Das Ergebnis ist, dass die Platzierung der Symbolleiste, die Farbe der Elemente der Symbolleiste und die Farbe des ausgewählten Symbolleistenelements für festgelegt werden, können eine [ `TabbedPage` ](xref:Xamarin.Forms.TabbedPage):

![](android-images/tabbedpage-toolbar-placement.png)

## <a name="application"></a>Application

Unter Android wird die folgende plattformspezifische Funktionalität bereitgestellt, für die Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application) Klasse:

- Festlegen des Betriebsmodus einer Bildschirmtastatur. Weitere Informationen finden Sie unter [Festlegen des Modus der weiche Tastatur Eingabe](#soft_input_mode).
- Deaktivieren der [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seite Lebenszyklusereignisse für anhalten und fortsetzen, für Anwendungen, die AppCompat verwenden. Weitere Informationen finden Sie unter [Deaktivieren der Disappearing und Seitenlebenszyklus-Ereignisse angezeigt werden](#disable_lifecycle_events).

<a name="soft_input_mode" />

### <a name="setting-the-soft-keyboard-input-mode"></a>Der Bildschirmtastatur Eingabemodus

Diese plattformspezifischen wird verwendet, um den Betriebsmodus für eine Bildschirmtastatur Eingabebereich festgelegt, und in XAML verwendet wird, durch Festlegen der [ `Application.WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.WindowSoftInputModeAdjustProperty) angefügte Eigenschaft auf den Wert der [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) Enumeration:

```xaml
<Application ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core"
             android:Application.WindowSoftInputModeAdjust="Resize">
  ...
</Application>
```

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

App.Current.On<Android>().UseWindowSoftInputModeAdjust(WindowSoftInputModeAdjust.Resize);
```

Die `Application.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `Application.UseWindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.Application.UseWindowSoftInputModeAdjust(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um die Bildschirmtastatur Eingabebereich Betriebsmodus mit Festlegen der [ `WindowSoftInputModeAdjust` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust) Enumeration, die Bereitstellung von zwei Werten: [ `Pan` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Pan) und [ `Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize). Die `Pan` Wert verwendet die [ `AdjustPan` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustPan/) Anpassung-Option, die Größe des Fensters ändern nicht, wenn ein Steuerelement den Fokus besitzt. Stattdessen werden der Inhalt des Fensters verschoben, damit an, dass der aktuelle Fokus die Bildschirmtastatur verdeckt. Die `Resize` Wert verwendet die [ `AdjustResize` ](https://developer.xamarin.com/api/field/Android.Views.SoftInput.AdjustResize/) Anpassung-Option, mit der die Fenstergröße wird, wenn ein Eingabesteuerelements, um Platz für die Bildschirmtastatur fokussierte.

Das Ergebnis ist, dass die Bildschirmtastatur Bereich Betriebsmodus kann festgelegt werden Eingabe, wenn ein Steuerelement den Fokus besitzt:

[![](android-images/pan-resize.png "Bildschirmtastatur-Betrieb-Modus plattformspezifische")](android-images/pan-resize-large.png#lightbox "Soft Keyboard Operating Mode Plaform-Specific")

<a name="disable_lifecycle_events" />

### <a name="disabling-the-disappearing-and-appearing-page-lifecycle-events"></a>Deaktivieren die verschwinden und angezeigten Seitenlebenszyklus-Ereignisse

Diese plattformspezifischen wird zum Deaktivieren der [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seitenereignisse Anwendung anhalten und Fortsetzen der jeweils für Anwendungen, die AppCompat verwenden. Darüber hinaus enthält es die Möglichkeit, um zu steuern, ob die Bildschirmtastatur bei Reaktivierung angezeigt wird, wenn es auf Pause angezeigt wurde, vorausgesetzt, dass der Betriebsmodus, der die Bildschirmtastatur, um festgelegt ist [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

> [!NOTE]
> Beachten Sie, dass diese Ereignisse standardmäßig aktiviert sind, vorhandenes Verhalten für Anwendungen beibehalten, die auf den Ereignissen basieren. Deaktivieren diese Ereignisse kann der AppCompat-Ereigniszyklus der Pre-AppCompat-Ereigniszyklus übereinstimmen.

Diese plattformspezifischen kann in XAML verwendet werden, durch Festlegen der [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPauseProperty), [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResumeProperty), und [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResumeProperty) angefügte Eigenschaften zu `boolean` Werte:

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

Alternativ können sie aus c# mithilfe der fluent-API verwendet werden:

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

Die `Application.Current.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die [ `Application.SendDisappearingEventOnPause` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendDisappearingEventOnPause(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) -Namespace wird verwendet, aktiviert oder deaktiviert das Auslösen der [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) -Ereignis der Seite, wenn die Anwendung Wechselt in den Hintergrund. Die [ `Application.SendAppearingEventOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.SendAppearingEventOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Methode dient zum Aktivieren oder deaktivieren das Auslösen der [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seitenereignis, wenn die Anwendung im Hintergrund fortgesetzt wird. Die [ `Application.ShouldPreserveKeyboardOnResume` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat.Application.ShouldPreserveKeyboardOnResume(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.Application},System.Boolean)) Methode wird verwendet, Kontrolle, ob die Bildschirmtastatur bei Reaktivierung angezeigt wird, wenn es auf Pause angezeigt wurde bereitgestellt, der Betriebsmodus, der die Bildschirmtastatur eingerichtet wird, um [ `WindowSoftInputModeAdjust.Resize` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WindowSoftInputModeAdjust.Resize).

Das Ergebnis ist, die [ `Disappearing` ](xref:Xamarin.Forms.Page.Appearing) und [ `Appearing` ](xref:Xamarin.Forms.Page.Appearing) Seitenereignisse wird nicht ausgelöst werden, auf die Anwendung anhalten und fortsetzen bzw. und, auch wenn die Bildschirmtastatur wurde angezeigt wird, wenn die Anwendung wurde angehalten, es wird auch angezeigt, wenn die Anwendung fortgesetzt wird:

[![](android-images/keyboard-on-resume.png "Lebenszyklus Ereignisse Clientplattform-spezifische")](android-images/keyboard-on-resume-large.png#lightbox "Lebenszyklus Ereignisse Clientplattform-spezifische")

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie die Android-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert sind. Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [AndroidSpecific](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
