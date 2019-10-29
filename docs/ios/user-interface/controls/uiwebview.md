---
title: Webansichten in xamarin. IOS
description: In diesem Dokument werden die verschiedenen Möglichkeiten beschrieben, wie eine xamarin. IOS-App Webinhalte anzeigen kann. Es erläutert UIWebView, wkwebview, sfsafariviewcontroller, Safari und App-Transportsicherheit.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 210e59239957d3963a3d3275315a0eac14748ff8
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021783"
---
# <a name="web-views-in-xamarinios"></a>Webansichten in xamarin. IOS

Die Lebensdauer von IOS Apple hat eine Reihe von Möglichkeiten für App-Entwickler veröffentlicht, webansichts Funktionen in Ihre apps einzubinden. Die meisten Benutzer verwenden den integrierten Safari-Webbrowser auf Ihrem IOS-Gerät und erwarten daher, dass die webansichtenfunktionalität von anderen apps mit dieser Funktionalität konsistent ist. Sie erwarten, dass die gleichen Gesten funktionieren, die Leistung auf dem gleichen Niveau und die Funktionalität identisch ist.

In diesem Artikel untersuchen wir die drei von Apple bereitgestellten Webansichten: `UIWebView`, `WKWebview`und `SFSafariViewController`, ihre Ähnlichkeiten und Unterschiede sowie die Art und Weise, wie Sie verwendet werden können. 

IOS 11 hat neue Änderungen an `WKWebView` und `SFSafariViewController`eingeführt. Weitere Informationen hierzu finden Sie im Leitfaden für [webänderungen in ios 11](~/ios/platform/introduction-to-ios11/web.md) .

## <a name="uiwebview"></a>UIWebView

`UIWebView` ist die ältere Methode von Apple, Webinhalte in Ihrer APP bereitzustellen. Sie wurde in ios 2,0 veröffentlicht und ist seit 8,0 als veraltet markiert.

Wenn Sie beabsichtigen, IOS-Versionen vor 8,0 zu unterstützen, müssen Sie `UIWebView`verwenden. Aufgrund der Tatsache, dass `UIWebView` weniger leistungsoptimiert ist als die Alternativen, empfiehlt es sich, die IOS-Version des Benutzers zu überprüfen. Wenn es 8,0 oder höher ist, wird durch die Verwendung einer der unten erläuternden Optionen eine bessere Benutzer Leistung erzielt.

Um ihrer xamarin. IOS-App eine UIWebView hinzuzufügen, verwenden Sie den folgenden Code:

```
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://xamarin.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Dadurch wird die folgende Webansicht erzeugt:

[![](uiwebview-images/webview.png "The effect of ScalesPagesToFit")](uiwebview-images/webview.png#lightbox)

Weitere Informationen zur Verwendung von `UIWebView`finden Sie in den folgenden Rezepte:

- [Laden einer Webseite](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_a_web_page)
- [Lokalen Inhalt laden](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_local_content)
- [Nicht-Webdokumente laden](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/load_non-web_documents)

## <a name="wkwebview"></a>Wkwebview

`WKWebView` wurde in ios 8 eingeführt, sodass App-Entwickler eine Webbrowserschnittstelle implementieren können, die der von Mobile Safari ähnelt. Dies liegt teilweise an der Tatsache, dass `WKWebView` die Nitro-Javascript-Engine verwendet, die gleiche Engine, die von Mobile Safari verwendet wird. `WKWebView` sollten immer über UIWebView verwendet werden. Dies ist aufgrund der [verbesserten Leistung](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview), der integrierten benutzerfreundlichen Gesten und der einfachen Interaktion zwischen der Webseite und Ihrer APP möglich.
  
`WKWebView` können Ihrer APP nahezu identisch mit UIWebView hinzugefügt werden. als Entwickler haben Sie jedoch viel mehr Kontrolle über die Benutzeroberfläche, die Benutzeroberfläche und die Funktionalität. Beim Erstellen und Anzeigen des webansichts Objekts wird die angeforderte Seite angezeigt, Sie können jedoch steuern, wie die Ansicht dargestellt wird, wie der Benutzer navigieren kann und wie der Benutzer die Ansicht verlässt.  

Der folgende Code kann verwendet werden, um eine `WKWebView` in ihrer xamarin. IOS-APP zu starten:

```csharp
    WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
    View.AddSubview(webView);

    var url = new NSUrl("https://xamarin.com");
    var request = new NSUrlRequest(url);
    webView.LoadRequest(request);
```

Dadurch wird die folgende Webansicht erzeugt:

[![](uiwebview-images/wkwebview.png "An example web view without ScalesPagesToFit")](uiwebview-images/wkwebview.png#lightbox)

Beachten Sie, dass `WKWebView` im WebKit-Namespace vorhanden ist. Daher müssen Sie diese using-Direktive am Anfang der Klasse hinzufügen.

`WKWebView` können auch in xamarin. Mac-Apps verwendet werden, und Sie sollten es ggf. verwenden, wenn Sie eine plattformübergreifende Mac/IOS-app erstellen.

Das Rezept zum [Behandeln von JavaScript-Warnungen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) bietet auch Informationen zur Verwendung von wkwebview mit JavaScript.

<a name="safariviewcontroller" />

## <a name="sfsafariviewcontroller"></a>Sfsafariviewcontroller

 `SFSafariViewController` ist die neueste Methode, um Webinhalte von Ihrer APP bereitzustellen, und ist in ios 9 und höher verfügbar. Im Gegensatz zu `UIWebView` oder `WKWebView`ist `SFSafariViewController` ein Ansichts Controller und kann daher nicht mit anderen Sichten verwendet werden. Sie sollten `SFSafariViewController` wie einen beliebigen Ansichts Controller als neuen Ansichts Controller präsentieren.

 `SFSafariViewController` ist im Wesentlichen eine "Mini Safari", die in Ihre APP eingebettet werden kann. Wie bei wkwebview wird dieselbe Nitro-Javascript-Engine verwendet, aber es bietet auch eine Reihe zusätzlicher Safari-Features, wie z. b. AutoFill, Reader und die Möglichkeit, Cookies und Daten mit mobile Safari gemeinsam zu nutzen. Die Interaktion zwischen dem Benutzer und dem `SFSafariViewController` ist für Ihre APP nicht zugänglich. Ihre APP hat keinen Zugriff auf die standardmäßigen Safari-Features.

Außerdem implementiert Sie standardmäßig eine Schaltfläche " **done** ", die es Benutzern ermöglicht, problemlos zu Ihrer APP zurückzukehren und Navigations Schaltflächen Vorwärts und rückwärts zu navigieren, sodass der Benutzer durch einen Stapel von Webseiten navigieren kann. Außerdem bietet Sie dem Benutzer eine Adressleiste, die dem Benutzer die Meinung bietet, dass Sie sich auf der erwarteten Webseite befinden. In der Adressleiste ist es dem Benutzer nicht möglich, die URL zu ändern. 

Diese Implementierungen können nicht geändert werden. Daher ist `SFSafariViewController` ideal, um als Standardbrowser zu verwenden, wenn Ihre APP eine Webseite ohne Anpassungen präsentieren möchte.

Der folgende Code kann verwendet werden, um eine `SFSafariViewController` in ihrer xamarin. IOS-APP zu starten:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Dadurch wird die folgende Webansicht erzeugt:

[![](uiwebview-images/sfsafariviewcontroller.png "An example web view with SFSafariViewController")](uiwebview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Es ist auch möglich, die mobile Safari-App aus der APP heraus zu öffnen, indem Sie den folgenden Code verwenden:

```csharp
var url = new NSUrl("https://xamarin.com");

UIApplication.SharedApplication.OpenUrl(url);

```

Dadurch wird die folgende Webansicht erzeugt:

[![](uiwebview-images/safari.png "A web page presented in Safari")](uiwebview-images/safari.png#lightbox)

Die Navigation von Benutzern zu Safari sollte in der Regel immer vermieden werden. Die meisten Benutzer erwarten keine Navigation außerhalb Ihrer Anwendung. Wenn Sie also von ihrer app Weg navigieren, können Benutzer diese nicht mehr zurückgeben.

mit den Verbesserungen für IOS 9 kann der Benutzer problemlos über eine Schaltfläche "zurück", die in der linken oberen Ecke der Safari-Seite angezeigt wird, zu Ihrer APP zurückkehren.

## <a name="app-transport-security"></a>App-Transportsicherheit

App-Transport Sicherheit oder *ATS* wurde von Apple in ios 9 eingeführt, um sicherzustellen, dass die gesamte Internetkommunikation den bewährten Methoden der sicheren Verbindung entspricht.

Weitere Informationen zu den Vorgängen, einschließlich der Implementierung in der APP, finden Sie im Leitfaden zur [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md) .

## <a name="related-links"></a>Verwandte Links

- [Webviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
