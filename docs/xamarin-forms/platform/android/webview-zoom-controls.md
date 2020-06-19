---
title: WebView-Zoom unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Datei nutzen, die Zoom für eine WebView ermöglicht.
ms.prod: xamarin
ms.assetid: DC1A3762-6A42-4298-929C-445F416C3E60
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/09/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9264bdf4ab5644b1cdfa0c37f1c7cacd3ae4ed0a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128335"
---
# <a name="webview-zoom-on-android"></a>WebView-Zoom unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit dieser Android-plattformspezifischen Funktion können Sie einen ""-Zoom und ein Zoom Steuerelement für einen aktivieren [`WebView`](xref:Xamarin.Forms.WebView) . Sie wird in XAML verwendet, indem die `WebView.EnableZoomControls` Bindungs fähigen Eigenschaften und auf-Werte festgelegt werden `WebView.DisplayZoomControls` `boolean` :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView Source="https://www.xamarin.com"
             android:WebView.EnableZoomControls="true"
             android:WebView.DisplayZoomControls="true" />
</ContentPage>
```

Die `WebView.EnableZoomControls` bindbare Eigenschaft steuert, ob der Wert für "Verkleinern-zu-Zoom" in aktiviert ist [`WebView`](xref:Xamarin.Forms.WebView) , und die `WebView.DisplayZoomControls` bindbare Eigenschaft steuert, ob Zoom Steuerelemente auf der überlagert werden `WebView` .

Alternativ kann das plattformspezifische c#-c# mithilfe der flüssigen API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>()
    .EnableZoomControls(true)
    .DisplayZoomControls(true);
```

Die- `WebView.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die- `WebView.EnableZoomControls` Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um zu steuern, ob "Verkleinern-zu-Zoom" für aktiviert ist [`WebView`](xref:Xamarin.Forms.WebView) . Die- `WebView.DisplayZoomControls` Methode im gleichen Namespace wird verwendet, um zu steuern, ob Zoom Steuerelemente auf der überlagert werden `WebView` . Darüber hinaus können die `WebView.ZoomControlsEnabled` -Methode und die- `WebView.ZoomControlsDisplayed` Methode verwendet werden, um zurückzugeben, ob die-und-Zoom Steuerelemente aktiviert sind.

Das Ergebnis ist, dass die Option "verkleinern" in "Zoom" aktiviert werden kann [`WebView`](xref:Xamarin.Forms.WebView) , und Zoom Steuerelemente können überlagert werden `WebView` :

[![Screenshot der zoomten WebView unter Android](webview-zoom-controls-images/webview-zoom.png "Vergrößern der WebView")](webview-zoom-controls-images/webview-zoom-large.png#lightbox "Vergrößern der WebView")

> [!IMPORTANT]
> Zoom Steuerelemente müssen sowohl aktiviert als auch angezeigt werden, über die entsprechenden bindbaren Eigenschaften oder Methoden, die überlagert werden [`WebView`](xref:Xamarin.Forms.WebView) .

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
