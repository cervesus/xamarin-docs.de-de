---
title: Gemischter Inhalt unter Android WebView
description: Plattformeigenschaften können Sie Funktionen zu nutzen, die nur auf einer bestimmten Plattform verfügbar ist ohne die Implementierung der benutzerdefinierten Renderern und Effekte. In diesem Artikel wird erläutert, nutzen Sie die Android-Plattform-spezifische, die gemischte Inhalte in einer WebView in Anwendungen angezeigt, Ziel API 21 oder höher.
ms.prod: xamarin
ms.assetid: 68F908F3-04C5-4B91-B6E5-B7E8357B4154
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 67ab68ceda69f9000bb160d1e443fd82ad7b61b3
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/20/2019
ms.locfileid: "65924569"
---
# <a name="webview-mixed-content-on-android"></a>Gemischter Inhalt unter Android WebView

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Dieses Android-Plattform-spezifische steuert, ob eine [ `WebView` ](xref:Xamarin.Forms.WebView) können gemischten Inhalt anzeigen in Anwendungen, die auf API 21 oder höher abzielen. Gemischte Inhalte sind Inhalte, die anfänglich über eine HTTPS-Verbindung geladen, aber die Ressourcen (z. B. Bilder, Audio, Video, Stylesheets, Skripts) über eine HTTP-Verbindung lädt. Es ist in XAML verwendet, durch Festlegen der [ `WebView.MixedContentMode` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.WebView.MixedContentModeProperty) angefügte Eigenschaft auf den Wert der [ `MixedContentHandling` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.MixedContentHandling) Enumeration:

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

- [PlatformSpecifics (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Erstellen von Plattformeigenschaften](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
