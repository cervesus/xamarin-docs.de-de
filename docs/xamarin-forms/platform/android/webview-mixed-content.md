---
title: Gemischter WebView-Inhalt unter Android
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, wie Sie die plattformspezifische Android-Datei nutzen, die gemischte Inhalte in einer WebView in Anwendungen mit API 21 oder höher anzeigte.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 286a7dceead327d727110d4ebbcecbc2341345b3
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656070"
---
# <a name="webview-mixed-content-on-android"></a>Gemischter WebView-Inhalt unter Android

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Mit diesem Android-plattformspezifischen Steuer [`WebView`](xref:Xamarin.Forms.WebView) Element wird gesteuert, ob ein gemischter Inhalt in Anwendungen mit API 21 oder höher angezeigt werden kann. Gemischte Inhalte sind Inhalte, die anfänglich über eine HTTPS-Verbindung geladen, aber die Ressourcen (z. B. Bilder, Audio, Video, Stylesheets, Skripts) über eine HTTP-Verbindung lädt. Es ist in XAML verwendet, durch Festlegen der [ `WebView.MixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) angefügte Eigenschaft auf den Wert der [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Enumeration:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <WebView ... android:WebView.MixedContentMode="AlwaysAllow" />
</ContentPage>
```

Alternativ können sie aus C# mithilfe der fluent-API verwendet werden:

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

[![WebView gemischten plattformspezifische für die Verarbeitung von Inhalten](webview-mixed-content-images/webview-mixedcontent.png "WebView gemischten plattformspezifische für die Verarbeitung von Inhalten")](webview-mixed-content-images/webview-mixedcontent-large.png#lightbox "WebView gemischten plattformspezifische für die Verarbeitung von Inhalten")

## <a name="related-links"></a>Verwandte Links

- [PlatformSpecifics (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [Androidspecific-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [Androidspecific. AppCompat-API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
