---
title: Windows-Plattform-Besonderheiten
description: Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden.
ms.prod: xamarin
ms.assetid: 22B403C0-FE6D-498A-AE53-095E6C4B527C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/23/2018
ms.openlocfilehash: d4ddb662bf167a0c80561cce097104a7f5fc8096
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2018
---
# <a name="windows-platform-specifics"></a>Windows-Plattform-Besonderheiten

_Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen. Dieser Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden._

Auf die universelle Windows Plattform (UWP) enthält Xamarin.Forms folgenden Plattform-Angaben:

- Festlegen von Platzierungsoptionen für die Symbolleiste. Weitere Informationen finden Sie unter [ändern die Symbolleiste Platzierung](#toolbar_placement).
- Reduzieren der [ `MasterDetailPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.MasterDetailPage/) Navigationsleiste. Weitere Informationen finden Sie unter [reduzieren eine Navigationsleiste MasterDetailPage](#collapsable_navigation_bar).
- Aktivieren einer [ `WebView` ](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem uwp-Nachricht. Weitere Informationen finden Sie unter [JavaScript-Warnungen anzeigen](#webview-javascript-alert).

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

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie die Windows-Plattform-Einzelheiten zu nutzen, die in Xamarin.Forms integriert werden. Plattform Einzelheiten können Sie Funktionen zu nutzen, die nur auf eine bestimmte Plattform verfügbar ist ohne Implementierung benutzerdefinierter Renderer oder Auswirkungen.

## <a name="related-links"></a>Verwandte Links

- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [WindowsSpecific](https://developer.xamarin.com/api/namespace/Xamarin.Forms.PlatformConfiguration.WindowsSpecific/)
