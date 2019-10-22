---
title: WebView-Zoom unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Datei nutzen, die Zoom für eine WebView ermöglicht.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 2142882add91d613263d11fa4c1e6d7ad142c7c7
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "68656002"
---
# <a name="webview-zoom-on-android"></a>WebView-Zoom unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit dieser Android-plattformspezifischen Funktion können Sie ein-und Zoom-Steuerelement auf einem [`WebView`](xref:Xamarin.Forms.WebView). Sie wird in XAML verwendet, indem die `WebView.EnableZoomControls` und `WebView.DisplayZoomControls` bindbaren Eigenschaften auf `boolean` Werte festgelegt werden:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

Die `WebView.EnableZoomControls` bindbare Eigenschaft steuert, ob für die [`WebView`](xref:Xamarin.Forms.WebView)die Option zum Verkleinern von "verkleinern" aktiviert ist, und die Eigenschaft "`WebView.DisplayZoomControls` bindbare" steuert, ob Zoom Steuerelemente auf dem `WebView` überlagert werden.

Alternativ kann die plattformspezifische API C# verwendet werden, um die fließende API zu verwenden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

Die `WebView.On<Android>`-Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die `WebView.EnableZoomControls`-Methode im [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um zu steuern, ob die Option zum Verkleinern von "verkleinern" auf dem [`WebView`](xref:Xamarin.Forms.WebView)aktiviert ist. Die `WebView.DisplayZoomControls`-Methode im gleichen Namespace wird verwendet, um zu steuern, ob Zoom Steuerelemente auf dem `WebView` überlagert werden. Darüber hinaus können die Methoden `WebView.ZoomControlsEnabled` und `WebView.ZoomControlsDisplayed` verwendet werden, um zurückzugeben, ob die Option zum Verkleinern und Zoomen bzw. Zoom Steuerelemente aktiviert ist.

Das Ergebnis ist, dass die Option "verkleinern" in " [`WebView`](xref:Xamarin.Forms.WebView)" aktiviert werden kann, und die Zoom Steuerelemente können auf dem `WebView` überlagert werden:

[![Screenshot der zoomten WebView unter Android](webview-zoom-controls-images/webview-zoom.png "Vergrößern der WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "Vergrößern der WebView")

> [!IMPORTANT]
> Zoom Steuerelemente müssen sowohl aktiviert als auch angezeigt werden, und zwar über die entsprechenden bindbaren Eigenschaften oder Methoden, die auf einer [`WebView`](xref:Xamarin.Forms.WebView)überlagert werden sollen.

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
