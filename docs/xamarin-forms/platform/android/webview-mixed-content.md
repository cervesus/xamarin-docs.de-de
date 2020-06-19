---
title: Gemischter WebView-Inhalt unter Android
description: Platt Form Besonderheiten ermöglichen es Ihnen, Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar sind, ohne dass benutzerdefinierte Renderer oder Effekte implementiert werden. In diesem Artikel wird erläutert, wie Sie das plattformspezifische Android-Gerät verwenden, das gemischte Inhalte in einer WebView in Anwendungen anzeigt, die API 21 oder höher als Ziel haben.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7269b0617be7199c365f350fc26ecd42256e28f3
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84128452"
---
# <a name="webview-mixed-content-on-android"></a>Gemischter WebView-Inhalt unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem Android-plattformspezifischen Steuerelement wird gesteuert, ob ein [`WebView`](xref:Xamarin.Forms.WebView) gemischter Inhalt in Anwendungen mit API 21 oder höher angezeigt werden kann. Gemischter Inhalt ist Inhalt, der anfänglich über eine HTTPS-Verbindung geladen wird, aber Ressourcen (z. b. Bilder, Audiodaten, Videos, Stylesheets, Skripts) über eine HTTP-Verbindung lädt. Sie wird in XAML verwendet, indem die [`WebView.MixedContentMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) angefügte-Eigenschaft auf einen Wert der-Enumeration festgelegt wird [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) :

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternativ kann Sie mithilfe der flüssigen API von c# genutzt werden:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

webView.On<Android>().SetMixedContentMode(MixedContentHandling.AlwaysAllow);
```

Die- `WebView.On<Android>` Methode gibt an, dass diese plattformspezifische nur unter Android ausgeführt wird. [ `WebView.SetMixedContentMode` ] (Xref: Xamarin.Forms . Platformconfiguration. androidspecific. WebView. setmixedcontentmode ( Xamarin.Forms . Iplatformelementconfiguration { Xamarin.Forms . Platformconfiguration. Android, Xamarin.Forms . WebView}, Xamarin.Forms . Platformconfiguration. androidspecific. mixedcontenthandling))-Methode im- [`Xamarin.Forms.PlatformConfiguration.AndroidSpecific`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) Namespace wird verwendet, um zu steuern, ob gemischter Inhalt angezeigt werden kann, wobei die- [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Enumeration drei mögliche Werte bereitstellt:

- [`AlwaysAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.AlwaysAllow)– Gibt an, dass der [`WebView`](xref:Xamarin.Forms.WebView) einem HTTPS-Ursprung ermöglicht, Inhalte aus einem http-Ursprung zu laden.
- [`NeverAllow`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.NeverAllow)– Gibt an, dass der [`WebView`](xref:Xamarin.Forms.WebView) keinen HTTPS-Ursprung zum Laden von Inhalt aus einem http-Ursprung zulässt.
- [`CompatibilityMode`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling.CompatibilityMode)– Gibt an, dass [`WebView`](xref:Xamarin.Forms.WebView) versucht, mit dem Ansatz des neuesten Geräte Webbrowsers kompatibel zu sein. Einige HTTP-Inhalte dürfen von einem HTTPS-Ursprung geladen werden, und andere Inhaltstypen werden blockiert. Die Arten von Inhalten, die blockiert oder zugelassen werden, können sich bei jeder Betriebssystemversion ändern.

Das Ergebnis ist, dass ein [`MixedContentHandling`](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) angegebener Wert auf den angewendet wird [`WebView`](xref:Xamarin.Forms.WebView) , der steuert, ob gemischter Inhalt angezeigt werden kann:

[![WebView-Verarbeitung gemischter Inhalte plattformspezifisch](webview-mixed-content-images/webview-mixedcontent.png "WebView-Verarbeitung gemischter Inhalte plattformspezifisch")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView-Verarbeitung gemischter Inhalte plattformspezifisch")

## <a name="related-links"></a>Verwandte Links

- [Platformbesonderheiten (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
