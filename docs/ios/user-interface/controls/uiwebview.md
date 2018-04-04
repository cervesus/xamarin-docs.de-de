---
title: Web-Ansichten
description: IOS-Web-Ansichtsoptionen eindeutig gemacht
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 787b5594476b3a1b5b3f6a0e8151a98c97443d00
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="web-views"></a>Web-Ansichten

Über die Lebensdauer des iOS veröffentlicht Apple vielfältige Weise für app-Entwickler Ansicht Webfunktionen in ihren apps integrieren. Die meisten Benutzer verwenden den integrierten Safari-Webbrowser auf seinem iOS-Gerät, und daher erwarten, dass Webansicht Funktionalität von anderen apps mit diesem-Erfahrung konsistent ist. Sie erwarten, dass die gleichen Gesten funktioniert, die Leistung auf Par und die Funktionen identisch sein.

In diesem Artikel wird untersucht jede der drei Webansichten von Apple bereitgestellten: `UIWebView`, `WKWebview`, und `SFSafariViewController`, die ähnlichkeiten und Unterschiede und deren Verwendung. 

iOS 11 eingeführten neue Änderungen sowohl `WKWebView` und `SFSafariViewController`. Weitere Informationen dazu finden Sie unter der [Web Änderungen in iOS 11 Handbuch](~/ios/platform/introduction-to-ios11/web.md) Handbuch.

## <a name="uiwebview"></a>UIWebView

`UIWebView` ist der Apple-ältere Möglichkeit Webinhalte in Ihrer app bereitstellen. Es iOS 2.0 veröffentlicht wurde und zum Zeitpunkt 8.0 ist veraltet.

Wenn Sie iOS-Versionen vor 8.0 unterstützen möchten, müssen Sie verwenden `UIWebView`. Auf der Tatsache, dass `UIWebView` ist weniger optimiert Leistung als die alternativen, es wird empfohlen, dass Sie die iOS-Version des Benutzers überprüfen soll. Wenn sie 8.0 oder höher, der Optionen erläutern unten erstellt eine bessere benutzererfahrung.
 
Um Ihre app Xamarin.iOS eine UIWebView hinzuzufügen, verwenden Sie den folgenden Code ein:
 
```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/webview.png "Die Auswirkung der ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Weitere Informationen zur Verwendung von `UIWebView`, finden Sie in der folgenden Rezepte:


- [Laden einer Webseite](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_a_web_page/)
- [Laden Sie lokalen Inhalt](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_local_content/)
- [Laden von nicht-Dokumente](https://developer.xamarin.com/recipes/ios/content_controls/web_view/load_non-web_documents/)

## <a name="wkwebview"></a>WKWebView

`WKWebView` iOS 8 Entwicklern die Möglichkeit app implementiert eine Schnittstelle ähnelt der mobilen Safari-Webbrowser eingeführt wurde. Dieses Problem wird, Teil der Fakt, `WKWebView` verwendet das Nitro Javascript-Modul, das gleiche Modul von mobilen Safari verwendet. `WKWebView` sollte immer verwendet werden über UIWebView wurden mögliche aufgrund der [eine höhere Leistung](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), integrierte benutzerfreundliche Gesten und die Einfachheit der Interaktion zwischen der Webseite und die app.
  
`WKWebView` können Ihre app in eine fast identische Möglichkeit, UIWebView, hinzugefügt werden aber als Entwickler Sie wesentlich mehr Kontrolle über die UI/UX und Funktionen haben. Erstellen und Anzeigen von Web-View-Objekts werden die angeforderte Seite angezeigt, jedoch Sie steuern können, wie der Benutzer navigieren kann, wie die Ansicht angezeigt wird und wie der Benutzer die Sicht beendet.  

Der folgende Code kann zum Starten einer `WKWebView` in Ihrer app Xamarin.iOS:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/wkwebview.png "Ein Beispiel-Webansicht ohne ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Es ist wichtig zu beachten, dass `WKWebView` befindet sich im Namespace WebKit, daher Sie die using-Direktive am Anfang der Klasse hinzufügen müssen.

`WKWebView` kann auch in Xamarin.Mac apps verwendet werden, und Sie aus diesem Grund sollten in Betracht ziehen verwenden, wenn Sie eine plattformübergreifende Mac/iOS-app erstellen.

Die [behandeln JavaScript Warnungen](https://developer.xamarin.com/recipes/ios/content_controls/web_view/handle_javascript_alerts/) Rezept enthält auch Informationen zur Verwendung von WKWebView mit Javascript

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>SFSafariViewController
 
 `SFSafariViewController` ist die aktuelle Methode zum Bereitstellen von Webinhalt aus Ihrer app, und wird in iOS 9 und höher verfügbar. Im Gegensatz zu `UIWebView` oder `WKWebView`, `SFSafariViewController` ist eine View-Controller und kann daher nicht mit anderen Ansichten verwendet werden. Sie sollten darstellen `SFSafariViewController` als eine neue View-Controller, auf die gleiche Weise stellen alle View-Controller.
 
 `SFSafariViewController` ist im Wesentlichen eine "Mini Safari", die in Ihrer app eingebettet werden kann. Wie WKWebView verwendet die gleichen Nitro Javascript-Engine, aber auch bietet eine Reihe von Safari Zusatzfunktionen wie AutoAusfüllen, Reader und die Möglichkeit, Cookies und Daten mit mobilen Safari freizugeben. Interaktion zwischen dem Benutzer und die `SFSafariViewController` ist für Ihre app nicht verfügbar. Ihre app wird nicht auf keines der Safari-Standardfeatures zugreifen.
 
Es implementiert auch, wird standardmäßig ein **Fertig** Schaltfläche, sodass Benutzer einfach zurückkehren, um Ihre app und weiterleiten, und sichern die Navigationsschaltflächen, sodass Ihre Benutzer zum Navigieren in eines Stapels von Webseiten. Darüber hinaus bietet es auch den Benutzer eine Adressleiste, die dafür sorgen, die sie auf der erwarteten Webseite befindlichen. Die-Adressleiste lässt keine den Benutzer die Url zu ändern. 

Diese Implementierung kann nicht geändert werden, damit `SFSafariViewController` eignet sich als Standardbrowser verwenden möchte, dass Ihre app eine Webseite ohne Anpassung zu präsentieren.

Der folgende Code kann zum Starten einer `SFSafariViewController` in Ihrer app Xamarin.iOS:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/sfsafariviewcontroller.png "Ein Beispiel Webansicht mit SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Es ist auch möglich, innerhalb der app, und die mobile Safari-app öffnen, mit dem folgenden Code:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Dies erzeugt die folgende Webansicht:

[![](uiwebview-images/safari.png "Eine Webseite in Safari präsentiert")](uiwebview-images/safari.png#lightbox)

Navigieren Benutzer von Ihrer app zu Safari sollte im Allgemeinen immer vermieden werden. Die meisten Benutzer nicht erwarten, dass Navigation außerhalb Ihrer Anwendung, wenn Sie die app verlassen, Benutzer nie, zurückkehren können im Wesentlichen Engagement Abbrechen.

iOS 9-Verbesserungen ermöglicht dem Benutzer problemlos an die app über eine Schaltfläche "zurück" zurückgegeben, die in der oberen linken Ecke der Seite Safari bereitgestellt wird.

## <a name="app-transport-security"></a>App-Transportsicherheit

App-Transportsicherheit oder *ATS* seit von Apple iOS 9, um sicherzustellen, dass die gesamte Internetkommunikation entsprechen, um die Verbindung zu bewährte Methoden sichern.

Weitere Informationen zu ATS, einschließlich Informationen zur Implementierung in Ihrer app finden Sie in der [App Transportsicherheit](~/ios/app-fundamentals/ats.md) Handbuch.

## <a name="related-links"></a>Verwandte Links

- [Webansichten (Beispiel)](https://developer.xamarin.com/samples/monotouch/WebView/)
