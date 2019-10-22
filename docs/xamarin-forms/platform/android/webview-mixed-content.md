---
title: Gemischter WebView-Inhalt unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Gerät verwenden, das gemischte Inhalte in einer WebView in Anwendungen anzeigt, die API 21 oder höher als Ziel haben.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: f9b4a12d3049cea37235cc04805a7411a9bdb236
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72696486"
---
# <a name="webview-mixed-content-on-android"></a>Gemischter WebView-Inhalt unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit dieser Android-plattformspezifischen Steuerung wird gesteuert, ob eine [`WebView`](xref:Xamarin.Forms.WebView) gemischte Inhalte in Anwendungen anzeigen kann, die API 21 oder höher als Ziel haben. Gemischter Inhalt ist Inhalt, der anfänglich über eine HTTPS-Verbindung geladen wird, aber Ressourcen (z. b. Bilder, Audiodaten, Videos, Stylesheets, Skripts) über eine HTTP-Verbindung lädt. Sie wird in XAML verwendet, indem die [`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) angefügte-Eigenschaft auf einen Wert der [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) -Enumeration festgelegt wird:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternativ kann Sie von C# der Verwendung der flüssigen API genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

Die `WebView.On<Android>`-Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. Die [`WebView.SetMixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.SetMixedContentMode(Xamarin.Forms.IPlatformElementConfiguration{Xamarin.Forms.PlatformConfiguration.Android,Xamarin.Forms.WebView},Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling)) -Methode im [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) -Namespace wird verwendet, um zu steuern, ob gemischter Inhalt angezeigt werden kann, wobei die [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) -Enumeration drei mögliche Werte bereitstellt:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow) – gibt an, dass die [`WebView`](xref:Xamarin.Forms.WebView) einem HTTPS-Ursprung das Laden von Inhalten aus einem http-Ursprung gestattet.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow) – gibt an, dass die [`WebView`](xref:Xamarin.Forms.WebView) keinen HTTPS-Ursprung zum Laden von Inhalt aus einem http-Ursprung zulässt.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode) – gibt an, dass die [`WebView`](xref:Xamarin.Forms.WebView) mit dem Ansatz des neuesten Geräte Webbrowsers kompatibel sein wird. Einige HTTP-Inhalte dürfen von einem HTTPS-Ursprung geladen werden, und andere Inhaltstypen werden blockiert. Die Arten von Inhalten, die blockiert oder zugelassen werden, können sich bei jeder Betriebssystemversion ändern.

Das Ergebnis ist, dass ein angegebener [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Wert auf den [`WebView`](xref:Xamarin.Forms.WebView)angewendet wird, der steuert, ob gemischter Inhalt angezeigt werden kann:

[![WebView-Verarbeitung gemischter Inhalte plattformspezifisch](webview-mixed-content-images/webview-mixedcontent.png "WebView-Verarbeitung gemischter Inhalte plattformspezifisch")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView-Verarbeitung gemischter Inhalte plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
