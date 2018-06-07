---
title: Windows-Plattform-Besonderheiten
description: Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/30/2018
ms.openlocfilehash: 7299de658a3491928e9bbeaa4dd192a8e95c435e
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732800"
---
# <a name="windows-platform-specifics"></a>Windows-Plattform-Besonderheiten

_Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden._

Auf die universelle Windows Plattform (UWP) enthält Xamarin.Forms folgenden Plattform-Angaben:

- Festlegen von Platzierungsoptionen für die Symbolleiste. Weitere Informationen finden Sie unter [ändern die Symbolleiste Platzierung](#toolbar_placement).
- Reduzieren der [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Navigationsleiste. Weitere Informationen finden Sie unter [reduzieren eine Navigationsleiste MasterDetailPage](#collapsable_navigation_bar).
- Aktivieren einer [ `WebView` ](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem uwp-Nachricht. Weitere Informationen finden Sie unter [JavaScript-Warnungen anzeigen](#webview-javascript-alert).
- Aktivieren einer [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) für die Interaktion mit dem Check-Modul zur Rechtschreibprüfung verwendet. Weitere Informationen finden Sie unter [SearchBar Rechtschreibprüfung aktivieren](#searchbar-spellcheck).
- Erkennen von Lesefolge von Textinhalt in [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen. Weitere Informationen finden Sie unter [Lesefolge aus Inhalt erkennen](#inputview-readingorder).
- Deaktivieren Sie ältere Farbe-Modus auf einem unterstützten [ `VisualElement` ](xref:Xamarin.Forms.VisualElement). Weitere Informationen finden Sie unter [deaktivieren Farbe Legacymodus](#legacy-color-mode).
- Aktivieren der Unterstützung für Tap Geste in einer [ `ListView` ](xref:Xamarin.Forms.ListView). Weitere Informationen finden Sie unter [aktivieren tippen Geste Unterstützung in einem ListView](#listview-selectionmode).

<a name="toolbar_placement" />

## <a name="changing-the-toolbar-placement"></a>Ändern die Symbolleiste-Platzierung

Diese plattformspezifischen wird verwendet, um die Platzierung einer Symbolleiste zu ändern eine [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/), und in XAML verwendet wird, durch Festlegen der [ `Page.ToolbarPlacement` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.ToolbarPlacementProperty/) -Eigenschaft auf einen Wert von der [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Enumeration:

```xaml
<TabbedPage ...
            xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
            windows:Page.ToolbarPlacement="Bottom">
  ...
</TabbedPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetToolbarPlacement(ToolbarPlacement.Bottom);
```

Die `Page.On<Windows>` Methode gibt an, dass diese plattformspezifische nur unter Windows ausgeführt wird. Die [ `Page.SetToolbarPlacement` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Page.SetToolbarPlacement/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.Page}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) -Namespace wird verwendet, um die Platzierung Symbolleiste mit Festlegen der [ `ToolbarPlacement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement/) Enumeration bereitstellen drei Werte: [ `Default` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Default/), [ `Top` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Top/), und [ `Bottom` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ToolbarPlacement.Bottom/).

Das Ergebnis ist, dass die angegebene Symbolleiste Platzierung angewendet wird, auf die [ `Page` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Page/) Instanz:

[![](windows-images/toolbar-placement.png "Symbolleiste Platzierung plattformspezifischen")](windows-images/toolbar-placement-large.png#lightbox "Symbolleiste Platzierung plattformspezifischen")

<a name="collapsable_navigation_bar" />

## <a name="collapsing-a-masterdetailpage-navigation-bar"></a>Eine Navigationsleiste MasterDetailPage reduzieren

Diese plattformspezifischen dient zum Ausblenden der Navigationsleiste auf eine [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/), und in XAML verwendet wird, durch Festlegen der [ `MasterDetailPage.CollapseStyle` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty/) und [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty/)angefügte Eigenschaften:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

Die `MasterDetailPage.On<Windows>` Methode gibt an, dass diese plattformspezifische nur unter Windows ausgeführt wird. Die [ `Page.SetCollapseStyle` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.SetCollapseStyle/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/) -Namespace wird verwendet, um anzugeben, den Stil reduzieren die [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) -Enumeration bietet zwei Werte: [ `Full` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full/) und [ `Partial` ](https://developer.xamarin.com/api/field/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial/). Die [ `MasterDetailPage.CollapsedPaneWidth` ](https://developer.xamarin.com/api/member/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidth/p/Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.MasterDetailPage}/System.Double/) Methode wird verwendet, um die Breite einer teilweise reduzierten Navigationsleiste angeben.

Das Ergebnis ist, die ein angegebenes [ `CollapseStyle` ](https://developer.xamarin.com/api/type/Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle/) wird angewendet, um die [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) -Instanz, mit die Breite, die ebenfalls angegeben wird:

[![](windows-images/collapsed-navigation-bar.png "Reduziert die Navigationsleiste plattformspezifischen")](windows-images/collapsed-navigation-bar-large.png#lightbox "reduzierten Navigationsleiste plattformspezifischen")

<a name="webview-javascript-alert" />

## <a name="displaying-javascript-alerts"></a>Anzeigen von JavaScript-Warnungen

Diese plattformspezifischen ermöglicht eine [ `WebView` ](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem uwp-Nachricht. Sie wird in XAML verwendet, durch Festlegen der [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

var webView = new Xamarin.Forms.WebView
{
  Source = new HtmlWebViewSource
  {
    Html = @"<html><body><button onclick=""window.alert('Hello World from JavaScript');"">Click Me</button></body></html>"
  }
};
webView.On<Windows>().SetIsJavaScriptAlertEnabled(true);
```

Die `WebView.On<Windows>` Methode gibt an, dass dieser plattformspezifische nur auf die universelle Windows-Plattform ausgeführt wird. Die [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Kontrolle, ob JavaScript-Warnungen aktiviert sind. Darüber hinaus die `WebView.SetIsJavaScriptAlertEnabled` Methode kann verwendet werden, um umzuschalten JavaScript Warnungen durch Aufrufen der [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) Methode zurückgeben soll, ob sie aktiviert sind:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Das Ergebnis ist, dass JavaScript-Warnungen in einem uwp-Nachricht angezeigt werden können:

![WebView JavaScript Warnung plattformspezifischen](windows-images/webview-javascript-alert.png "WebView JavaScript Warnung plattformspezifischen")

<a name="searchbar-spellcheck" />

## <a name="enabling-searchbar-spell-check"></a>SearchBar Rechtschreibprüfung aktivieren

Diese plattformspezifischen ermöglicht eine [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) für die Interaktion mit dem Check-Modul zur Rechtschreibprüfung verwendet. Sie wird in XAML verwendet, durch Festlegen der [ `SearchBar.IsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.IsSpellCheckEnabledProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <SearchBar ... windows:SearchBar.IsSpellCheckEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

searchBar.On<Windows>().SetIsSpellCheckEnabled(true);
```

Die `SearchBar.On<Windows>` Methode gibt an, dass dieser plattformspezifische nur auf die universelle Windows-Plattform ausgeführt wird. Die [ `SearchBar.SetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.SetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace, wird die Rechtschreibprüfung aktiviert und deaktiviert. Darüber hinaus die `SearchBar.SetIsSpellCheckEnabled` Methode kann verwendet werden, um die Rechtschreibprüfung durch Aufrufen von Umschalten der [ `SearchBar.GetIsSpellCheckEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.GetIsSpellCheckEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.SearchBar})) Methode zurück, ob die Rechtschreibprüfung aktiviert ist:

```csharp
searchBar.On<Windows>().SetIsSpellCheckEnabled(!searchBar.On<Windows>().GetIsSpellCheckEnabled());
```

Das Ergebnis ist, in die Text eingegeben werden, in der [ `SearchBar` ](xref:Xamarin.Forms.SearchBar) Rechtschreibprüfung überprüft werden, mit der falschen Schreibweisen, die dem Benutzer angezeigt wird:

![SearchBar Rechtschreibprüfung Kontrollkästchen plattformspezifischen](windows-images/searchbar-spellcheck.png "SearchBar Rechtschreibprüfung Kontrollkästchen plattformspezifischen")

> [!NOTE]
> Die `SearchBar` -Klasse in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) Namespace verfügt auch über [ `EnableSpellCheck` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.EnableSpellCheck*) und [ `DisableSpellCheck` ](xre:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.SearchBar.DisableSpellCheck*) Methoden, die zum Aktivieren und deaktivieren verwendet werden können die Rechtschreibprüfung für die `SearchBar`bzw.

<a name="inputview-readingorder" />

## <a name="detecting-reading-order-from-content"></a>Erkennen von Lesefolge von Inhalt

Diese plattformspezifischen ermöglicht die leserichtung (links nach rechts oder rechts-nach-links) der bidirektionalen Text in [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen dynamisch erkannt werden. Sie wird in XAML verwendet, durch Festlegen der [ `InputView.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.DetectReadingOrderFromContentProperty) (für `Entry` und `Editor` Instanzen) oder [ `Label.DetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.Label.DetectReadingOrderFromContentProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <Editor ... windows:InputView.DetectReadingOrderFromContent="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

editor.On<Windows>().SetDetectReadingOrderFromContent(true);
```

Die `Editor.On<Windows>` Methode gibt an, dass dieser plattformspezifische nur auf die universelle Windows-Plattform ausgeführt wird. Die [ `InputView.SetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.SetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet um zu steuern, ob die Lesefolge aus dem Inhalt erkannt wird die [ `InputView` ](xref:Xamarin.Forms.InputView). Darüber hinaus die `InputView.SetDetectReadingOrderFromContent` Methode kann verwendet werden, um umzuschalten, ob die Lesefolge aus dem Inhalt, durch Aufrufen erkannt wird der [ `InputView.GetDetectReadingOrderFromContent` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.InputView.GetDetectReadingOrderFromContent(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.InputView})) Methode, um den aktuellen Wert zurückzugeben:

```csharp
editor.On<Windows>().SetDetectReadingOrderFromContent(!editor.On<Windows>().GetDetectReadingOrderFromContent());
```

Das Ergebnis ist, [ `Entry` ](xref:Xamarin.Forms.Entry), [ `Editor` ](xref:Xamarin.Forms.Editor), und [ `Label` ](xref:Xamarin.Forms.Label) Instanzen können die Lesefolge von ihren Inhalt dynamisch ermittelt haben:

[![Erkennen von Lesefolge aus Inhalt plattformspezifischen InputView](windows-images/editor-readingorder.png "InputView Lesefolge aus Inhalt plattformspezifischen erkennen")](windows-images/editor-readingorder-large.png#lightbox "InputView erkennen die Lesefolge von Inhalt plattformspezifischen")

> [!NOTE]
> Im Gegensatz zur Einstellung der [ `FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) -Eigenschaft, die Logik für Sichten, die die Lesefolge von Textinhalt wirkt sich nicht auf die Ausrichtung des Texts innerhalb der Ansicht zu erkennen. Stattdessen wird die Reihenfolge, in der bidirektionalen Textblöcke angelegt werden, angepasst.

<a name="legacy-color-mode" />

## <a name="disabling-legacy-color-mode"></a>Deaktivieren die Vorgängerversion Färbung aktiviert

Einige der Xamarin.Forms Ansichten an eine ältere Färbung aktiviert Funktionen. In diesem Modus kann bei der [ `IsEnabled` ](xref:Xamarin.Forms.VisualElement.IsEnabled) -Eigenschaft der Ansicht auf festgelegt ist `false`, die Sicht werden die Farben, die vom Benutzer mit der systemeigenen Standardfarben für den deaktivierten Zustand überschrieben. Für die Abwärtskompatibilität Kompatibilität diese ältere Färbung aktiviert das Standardverhalten für die unterstützten Ansichten bleibt.

Diese plattformspezifischen wird dieser älteren Färbung aktiviert, deaktiviert, sodass Farben für eine Sicht vom Benutzer festgelegt bleiben, auch wenn die Sicht deaktiviert ist. Sie wird in XAML verwendet, durch Festlegen der [ `VisualElement.IsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.IsLegacyColorModeEnabledProperty) -Eigenschaft auf `false`:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Editor Text="Enter text here"
                TextColor="Blue"
                BackgroundColor="Bisque"
                windows:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

_legacyColorModeDisabledEditor.On<Windows>().SetIsLegacyColorModeEnabled(false);
```

Die `VisualElement.On<Windows>` Methode gibt an, dass diese plattformspezifische nur unter Windows ausgeführt wird. Die [ `VisualElement.SetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.SetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Kontrolle, ob die legacy-Farben-Modus deaktiviert ist. Darüber hinaus die [ `VisualElement.GetIsLegacyColorModeEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.VisualElement.GetIsLegacyColorModeEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.VisualElement})) Methode kann verwendet werden, um zurück, ob die ältere Farbe Modus deaktiviert ist.

Das Ergebnis ist, dass die ältere Färbung aktiviert deaktiviert werden kann, Farben, die vom Benutzer für eine Sicht festgelegt selbst bleiben, wenn die Sicht deaktiviert ist:

![](windows-images/legacy-color-mode-disabled.png "Ältere Farbe Modus deaktiviert")

> [!NOTE]
> Beim Festlegen einer [ `VisualStateGroup` ](xref:Xamarin.Forms.VisualStateGroup) für eine Sicht, die ältere Färbung aktiviert vollständig ignoriert. Weitere Informationen zu visuelle Zustände, finden Sie unter [der Xamarin.Forms visuellen Status-Manager](~/xamarin-forms/user-interface/visual-state-manager.md).

<a name="listview-selectionmode" />

## <a name="enabling-tap-gesture-support-in-a-listview"></a>Aktivieren der Unterstützung für Tap Geste in einer ListView

Klicken Sie auf die universelle Windows-Plattform, standardmäßig die Xamarin.Forms [ `ListView` ](xref:Xamarin.Forms.ListView) verwendet die systemeigenen `ItemClick` Ereignis so reagieren Sie auf die Aktivität, anstatt das systemeigene `Tapped` Ereignis. Dies ermöglicht die Barrierefreiheitsfunktionen, sodass die Windows-Sprachausgabe und Tastatur interagieren können die `ListView`. Allerdings rendert es auch eine Tap-Gesten innerhalb der `ListView` nicht mehr funktionsfähig.

Diese plattformspezifischen steuert, ob Elemente in einer [ `ListView` ](xref:Xamarin.Forms.ListView) reagieren kann, um Gesten, tippen Sie auf und daher, ob das systemeigene `ListView` ausgelöst wird die `ItemClick` oder `Tapped` Ereignis. Sie wird in XAML verwendet, durch Festlegen der [ `ListView.SelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SelectionModeProperty) -Eigenschaft auf den Wert der [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) Enumeration:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <ListView ... windows:ListView.SelectionMode="Inaccessible">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Alternativ können sie in c# mithilfe der fluent-API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

listView.On<Windows>().SetSelectionMode(ListViewSelectionMode.Inaccessible);
```

Die `ListView.On<Windows>` Methode gibt an, dass dieser plattformspezifische nur auf die universelle Windows-Plattform ausgeführt wird. Die [ `ListView.SetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.SetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView},Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Steuerelement gibt an, ob Elemente in eine [ `ListView` ](xref:Xamarin.Forms.ListView) können Antworten mit Gesten, tippen auf der [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) -Enumeration bietet zwei mögliche Werte:

- [`Accessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Accessible) – Gibt an, dass die `ListView` immer dann ausgelöst, die systemeigene `ItemClick` Ereignis, um die Interaktion zu behandeln, und daher Zugriff bereit. Aus diesem Grund die Sprachausgabe Windows und der Tastatur können interagieren mit der `ListView`. Jedoch Elemente in der `ListView` nicht auf das Gesten abgerufen werden sollen. Dies ist das Standardverhalten für `ListView` Instanzen auf die universelle Windows-Plattform.
- [`Inaccessible`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode.Inaccessible) – Gibt an, dass die `ListView` immer dann ausgelöst, die systemeigene `Tapped` Ereignis Interaktion zu behandeln. Daher Elemente in der `ListView` um Gesten tippen reagieren kann. Allerdings ist keine Eingabehilfen-Funktionalität und daher die Sprachausgabe Windows und der Tastatur können nicht interagieren mit der `ListView`.

> [!NOTE]
> Die `Accessible` und `Inaccessible` Modi schließen sich gegenseitig, und Sie müssen zur Wahl zwischen einem zugegriffen werden kann [ `ListView` ](xref:Xamarin.Forms.ListView) oder eine `ListView` , die auf Gesten tippen reagieren kann.

Darüber hinaus die [ `GetSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListView.GetSelectionMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.ListView})) Methode kann verwendet werden, um das aktuelle [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode).

Das Ergebnis ist, die ein angegebenes [ `ListViewSelectionMode` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.ListViewSelectionMode) auf angewendet wird die [ `ListView` ](xref:Xamarin.Forms.ListView), welche steuert, ob Elemente in der `ListView` reagieren kann, um Gesten, tippen Sie auf und daher, ob das systemeigene `ListView` ausgelöst wird die `ItemClick` oder `Tapped` Ereignis.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden. Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
