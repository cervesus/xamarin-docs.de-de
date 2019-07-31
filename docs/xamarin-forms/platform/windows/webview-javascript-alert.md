---
title: WebView-JavaScript-Warnungen unter Windows
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie das Windows-plattformspezifische verwenden, das es WebView ermöglicht, JavaScript-Warnungen in einem UWP-Meldungs Dialogfeld anzuzeigen
ms.prod: xamarin
ms.assetid: 95A153A1-72A0-4C0B-A452-ACE966BB12CB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 2bddf8ad314c49d8865863f6e67fc90a2bf80eb7
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656824"
---
# <a name="webview-javascript-alerts-on-windows"></a>WebView-JavaScript-Warnungen unter Windows

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Diese plattformspezifischen ermöglicht eine [ `WebView` ](xref:Xamarin.Forms.WebView) zum Anzeigen von JavaScript-Warnungen in einem Meldungsdialogfeld UWP. Es ist in XAML verwendet, durch Festlegen der [ `WebView.IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabledProperty) angefügten Eigenschaft, um eine `boolean` Wert:

```xaml
<ContentPage ...
             xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        <WebView ... windows:WebView.IsJavaScriptAlertEnabled="true" />
        ...
    </StackLayout>
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

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

Die `WebView.On<Windows>` Methode gibt an, dass diese plattformspezifischen nur für die universelle Windows-Plattform ausgeführt wird. Die [ `WebView.SetIsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.SetIsJavaScriptAlertEnabled(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Windows,Xamarin.Forms.WebView},System.Boolean)) Methode in der [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) -Namespace wird verwendet, um Kontrolle, ob JavaScript-Warnungen aktiviert sind. Darüber hinaus die `WebView.SetIsJavaScriptAlertEnabled` Methode kann verwendet werden, zum Umschalten der JavaScript-Warnungen durch Aufrufen der [ `IsJavaScriptAlertEnabled` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.WebView.IsJavaScriptAlertEnabled*) Methode zurückgeben soll, ob sie aktiviert sind:

```csharp
_webView.On<Windows>().SetIsJavaScriptAlertEnabled(!_webView.On<Windows>().IsJavaScriptAlertEnabled());
```

Das Ergebnis ist, dass JavaScript-Warnungen in einem UWP-Meldungsdialogfeld angezeigt werden können:

![WebView-JavaScript-Warnung plattformspezifische](webview-javascript-alert-images/webview-javascript-alert.png "WebView-JavaScript-Warnung plattformspezifische")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Windowsspecific-API](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
