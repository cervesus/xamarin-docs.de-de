---
title: WebView-Zoom unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Datei nutzen, die Zoom für eine WebView ermöglicht.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
ms.openlocfilehash: 2142882add91d613263d11fa4c1e6d7ad142c7c7
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656002"
---
# <a name="webview-zoom-on-android"></a>WebView-Zoom unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit dieser Android-plattformspezifischen Funktion können Sie einen ""-Zoom und ein [`WebView`](xref:Xamarin.Forms.WebView)Zoom Steuerelement für einen aktivieren. Sie wird in XAML verwendet, indem die `WebView.EnableZoomControls` Bindungs `WebView.DisplayZoomControls` fähigen Eigenschaften und auf `boolean` -Werte festgelegt werden:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

Die `WebView.EnableZoomControls` bindbare Eigenschaft steuert [`WebView`](xref:Xamarin.Forms.WebView), ob der Wert für "Verkleinern-zu-Zoom" in `WebView.DisplayZoomControls` aktiviert ist, und die bindbare Eigenschaft steuert, ob Zoom `WebView`Steuerelemente auf der überlagert werden.

Alternativ kann die plattformspezifische API C# verwendet werden, um die fließende API zu verwenden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

Die `WebView.On<Android>` Methode gibt an, dass diese plattformspezifischen nur unter Android ausgeführt wird. Die `WebView.EnableZoomControls` -Methode [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) im-Namespace wird verwendet, um zu steuern, ob "Verkleinern-zu- [`WebView`](xref:Xamarin.Forms.WebView)Zoom" für aktiviert ist. Die `WebView.DisplayZoomControls` -Methode im gleichen Namespace wird verwendet, um zu steuern, ob Zoom Steuerelemente auf der `WebView`überlagert werden. Darüber hinaus können die `WebView.ZoomControlsEnabled` - `WebView.ZoomControlsDisplayed` Methode und die-Methode verwendet werden, um zurückzugeben, ob die-und-Zoom Steuerelemente aktiviert sind.

Das Ergebnis ist, dass die Option "verkleinern" in "Zoom" [`WebView`](xref:Xamarin.Forms.WebView)aktiviert werden kann, und Zoom Steuerelemente können `WebView`überlagert werden:

[ ![Screenshot der zoomten WebView unter Android](webview-zoom-controls-images/webview-zoom.png "Zoom WebView") ] Vergrößern der (webview-zoom-controls-images/webview-zoom-large.png#lightbox "WebView")

> [!IMPORTANT]
> Zoom Steuerelemente müssen sowohl aktiviert als auch angezeigt werden, über die entsprechenden bindbaren Eigenschaften oder Methoden, die überlagert [`WebView`](xref:Xamarin.Forms.WebView)werden.

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
