---
title: Auf Android WebView-Zoom
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie die Android-Plattform-spezifische nutzen, die auf eine Webansicht Zoom ermöglicht wird.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 0e180ca5019bd4e5d1351cfb2f7a8c28ade2afe2
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971642"
---
# <a name="webview-zoom-on-android"></a>Auf Android WebView-Zoom

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)

Dieses Android-Plattform-spezifische aktiviert Pinch-Finger-Zoom und einem Zoomsteuerelement auf eine [ `WebView` ](xref:Xamarin.Forms.WebView). Es ist in XAML verwendet, durch Festlegen der `WebView.EnableZoomControls` und `WebView.DisplayZoomControls` bindbare Eigenschaften `boolean` Werte:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

Die `WebView.EnableZoomControls` bindbare Eigenschaft steuert, ob die Pinch-Finger-Zoom auf aktiviert ist die [ `WebView` ](xref:Xamarin.Forms.WebView), und die `WebView.DisplayZoomControls` bindbare Eigenschaft steuert, ob die Zoom-Steuerelemente gelegt werden, auf die `WebView`.

Alternativ kann die plattformspezifischen genutzt werden, von C# mithilfe der fluent-API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

Die `WebView.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `WebView.EnableZoomControls` Methode in der [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um Kontrolle, ob Pinch-Finger-Zoom aktiviert ist, auf die [ `WebView` ](xref:Xamarin.Forms.WebView). Die `WebView.DisplayZoomControls` -Methode, im gleichen Namespace wird verwendet, um steuern, ob die Zoom-Steuerelemente gelegt werden, auf die `WebView`. Darüber hinaus die `WebView.ZoomControlsEnabled` und `WebView.ZoomControlsDisplayed` Methoden können verwendet werden, um zurück, ob die Pinch-Finger-Zoom und Zoom-Steuerelemente aktiviert sind.

Das Ergebnis ist dieser Pinch-Finger-Zoom kann aktiviert werden, auf eine [ `WebView` ](xref:Xamarin.Forms.WebView), und die Zoomsteuerelemente können überlagert werden, auf die `WebView`:

[![Screenshot der vergrößerten WebView unter Android](webview-zoom-controls-images/webview-zoom.png "vergrößert WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "WebView vergrößert")

> [!IMPORTANT]
> Zoom-Steuerelemente müssen werden sowohl aktivierte als auch angezeigt, über die entsprechenden bindbare Eigenschaften oder Methoden, um auf überlagernden eine [ `WebView` ](xref:Xamarin.Forms.WebView).

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
