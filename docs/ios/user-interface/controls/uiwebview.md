---
title: Web-Ansichten in Xamarin.iOS
description: Dieses Dokument beschreibt die verschiedenen Möglichkeiten, die eine Xamarin.iOS-app, Web-Inhalte anzeigen kann. Es wird erläutert, UIWebView, WKWebView, SFSafariViewController, Safari und app-transportsicherheit.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 56b0f0e910cc95ca50d1e5e460ce71a1c8f669a2
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242039"
---
# <a name="web-views-in-xamarinios"></a>Web-Ansichten in Xamarin.iOS

Apple hat eine Reihe von Möglichkeiten für app-Entwickler Webvorschau in ihre apps integrieren veröffentlicht, während der Lebensdauer der e/a. Die meisten Benutzer verwenden den integrierten Safari-Webbrowser auf ihrem iOS-Gerät, und daher erwarten, dass die Webansicht-Funktionalität von anderen apps mit diesem-Erfahrung konsistent ist. Sie erwarten, dass die gleichen Gesten funktionieren, die Leistung auf Par und die Funktionen identisch sein.

In diesem Artikel wird untersucht, jede der drei Webansicht, die von Apple bereitgestellten: `UIWebView`, `WKWebview`, und `SFSafariViewController`, die ähnlichkeiten und Unterschiede und wie sie verwendet werden können. 

iOS 11 eingeführte neue Änderungen sowohl `WKWebView` und `SFSafariViewController`. Weitere Informationen dazu finden Sie unter den [Web-Änderungen in iOS 11-Handbuch](~/ios/platform/introduction-to-ios11/web.md) Guide.

## <a name="uiwebview"></a>UIWebView

`UIWebView` wird von Apple die herkömmliche Weise für die Bereitstellung von Web-Inhalte in Ihrer app. Es wurde im iOS 2.0 veröffentlicht, und ist seit 8.0 veraltet.

Wenn Sie iOS-Versionen vor 8.0 unterstützen möchten, müssen mit `UIWebView`. Auf der Tatsache, dass `UIWebView` weniger optimiert ist für Leistung als die alternativen, es wird empfohlen, dass Sie iOS-Version des Benutzers überprüfen soll. Wenn sie 8.0 oder höher verwenden eine der Optionen erläutern unten wird eine bessere benutzererfahrung zu erstellen.
 
Um eine UIWebView Ihrer Xamarin.iOS-app hinzuzufügen, verwenden Sie den folgenden Code:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/webview.png "Die Auswirkungen der ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Weitere Informationen zur Verwendung von `UIWebView`, finden Sie in der folgenden Anleitungen:


- [Laden einer Webseite](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Laden von lokalen Inhalten](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Laden von nicht-Dokumenten](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>WKWebView

`WKWebView` wurde in iOS 8 und ermöglicht app-Entwickler eine Benutzeroberfläche ähnlich, die von mobilen Safari-Webbrowser implementieren eingeführt. Dies ist, teilweise auf die Tatsache zurückzuführen, `WKWebView` verwendet die Nitro Javascript-Engine, die gleiche Engine, die von mobilen Safari verwendet. `WKWebView` sollte stets verwendet werden UIWebView wurden aufgrund von möglichen über die [eine höhere Leistung](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), benutzerfreundlichen Gesten, und die einfache Interaktion zwischen der Webseite und Ihre app integriert.
  
`WKWebView` können zu Ihrer app in eine fast identische Möglichkeit zum UIWebView, hinzugefügt werden, aber als Entwickler deutlich mehr Kontrolle über die Benutzeroberfläche/UX und Funktionen haben. Erstellen und Anzeigen des Objekts der Web-Ansicht zeigt die angeforderte Seite, jedoch Sie steuern können, wie die Ansicht dargestellt werden, wie der Benutzer navigieren kann und wie sich der Benutzer auf die Sicht beendet.  

Der folgende Code kann zum Starten einer `WKWebView` in Ihrer Xamarin.iOS-app:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/wkwebview.png "Eine Beispiel-Web-Ansicht ohne ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Es ist wichtig zu beachten, dass `WKWebView` befindet sich in den WebKit-Namespace, sodass Sie dadurch die using-Direktive am Anfang der Klasse hinzugefügt werden.

`WKWebView` kann auch in einer Xamarin.Mac-apps verwendet werden, und Sie aus diesem Grund sollten in Betracht, wenn Sie eine plattformübergreifende Mac/iOS-app erstellen.

Die [JavaScript Warnungen behandeln](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) Anleitung enthält auch Informationen zur Verwendung von WKWebView mit Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` ist das aktuelle Verfahren zum Bereitstellen von Webinhalte aus Ihrer app und in iOS 9 und höher verfügbar. Im Gegensatz zu `UIWebView` oder `WKWebView`, `SFSafariViewController` ist ein View-Controller und kann daher nicht mit anderen Ansichten verwendet werden. Sie sollten zur Verfügung stellen `SFSafariViewController` als eine neue View-Controller, auf die gleiche Weise präsentieren alle View-Controller.
 
 `SFSafariViewController` ist im Wesentlichen einem "Mini Safari", die in Ihre app eingebettet werden kann. Wie WKWebView verwendet die gleichen Nitro Javascript-Engine, sondern bietet auch eine Reihe von zusätzlichen Safari-Features wie AutoAusfüllen, Reader und die Möglichkeit zur Freigabe von Cookies und Daten mit mobilen Safari. Interaktion zwischen dem Benutzer und dem `SFSafariViewController` ist für Ihre app nicht verfügbar. Ihre app wird nicht auf keines der standardmäßigen Safari-Features zugreifen.
 
Es implementiert auch, wird standardmäßig ein **Fertig** Dadurch können Benutzer einfach zu Ihrer app zurückkehren und weiterzuleiten, wobei Navigationsschaltflächen, sodass Ihre Benutzer durch einen Stapel von Webseiten zu navigieren. Darüber hinaus bietet es auch den Benutzer eine Adressleiste, sodass sie die Sicherheit, die sie auf der erwarteten Webseite sind. Die-Adressleiste lässt sich nicht auf den Benutzer, ändern die Url aus. 

Diese Implementierungen kann nicht geändert werden, also `SFSafariViewController` ist ideal als Standardbrowser verwendet wird, wenn möchte, dass Ihre app eine Webseite ohne Anpassung zu präsentieren.

Der folgende Code kann zum Starten einer `SFSafariViewController` in Ihrer Xamarin.iOS-app:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/sfsafariviewcontroller.png "Eine Beispiel-Web-Ansicht mit SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Es ist auch möglich, zu die mobile Safari-app in der app zu öffnen, mit dem folgenden Code:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/safari.png "Eine Webseite angezeigt, die in Safari")](uiwebview-images/safari.png#lightbox)

Navigieren durch Benutzer weg von Ihrer app in Safari sollten im Allgemeinen immer vermieden werden. Die meisten Benutzer nicht erwarten, dass die Navigation außerhalb Ihrer Anwendung, sodass, wenn Sie Ihre app verlassen, Benutzer nie, zurückgegeben können Engagement im Wesentlichen zu beenden.

iOS 9-Verbesserungen können Benutzer einfach an Ihre app über eine Schaltfläche "zurück" zurückgegeben, die in der oberen linken Ecke der Seite Safari bereitgestellt wird.

## <a name="app-transport-security"></a>App-Transportsicherheit

App-Transportsicherheit: oder *ATS* seit von Apple iOS 9, um sicherzustellen, dass die gesamte Internetkommunikation entsprechen, um die Verbindung zu bewährte Methoden sichern.

Weitere Informationen zu ATS, einschließlich der Implementierung in Ihrer app finden Sie in der [App Transport Security](~/ios/app-fundamentals/ats.md) Guide.

## <a name="related-links"></a>Verwandte Links

- [WebViews (Beispiel)](https://developer.xamarin.com/samples/monotouch/WebView/)
