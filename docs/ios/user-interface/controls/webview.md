---
title: Webansichten in xamarin. IOS
description: In diesem Dokument werden die verschiedenen Möglichkeiten beschrieben, wie eine xamarin. IOS-App Webinhalte anzeigen kann. Darin werden wkwebview, sfsafariviewcontroller, Safari und App-Transportsicherheit erläutert.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 8640800717a88e800503e93c339eeb080707374e
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725431"
---
# <a name="web-views-in-xamarinios"></a>Webansichten in xamarin. IOS

Die Lebensdauer von IOS Apple hat eine Reihe von Möglichkeiten für App-Entwickler veröffentlicht, webansichts Funktionen in Ihre apps einzubinden. Die meisten Benutzer verwenden den integrierten Safari-Webbrowser auf Ihrem IOS-Gerät und erwarten daher, dass die webansichtenfunktionalität von anderen apps mit dieser Funktionalität konsistent ist. Sie erwarten, dass die gleichen Gesten funktionieren, die Leistung auf dem gleichen Niveau und die Funktionalität identisch ist.

IOS 11 hat neue Änderungen an `WKWebView` und `SFSafariViewController`eingeführt. Weitere Informationen hierzu finden Sie im Leitfaden für [webänderungen in ios 11](~/ios/platform/introduction-to-ios11/web.md) .

## <a name="wkwebview"></a>WKWebView

`WKWebView` wurde in ios 8 eingeführt, sodass App-Entwickler eine Webbrowserschnittstelle implementieren können, die der von Mobile Safari ähnelt. Dies liegt teilweise an der Tatsache, dass `WKWebView` die Nitro-Javascript-Engine verwendet, die gleiche Engine, die von Mobile Safari verwendet wird. `WKWebView` sollten immer über UIWebView verwendet werden, wenn möglich, aufgrund der verbesserten Leistung, der integrierten benutzerfreundlichen Gesten und der einfachen Interaktion zwischen der Webseite und ihrer app.

`WKWebView` können Ihrer APP nahezu identisch mit UIWebView hinzugefügt werden. als Entwickler haben Sie jedoch viel mehr Kontrolle über die Benutzeroberfläche, die Benutzeroberfläche und die Funktionalität. Beim Erstellen und Anzeigen des webansichts Objekts wird die angeforderte Seite angezeigt, Sie können jedoch steuern, wie die Ansicht dargestellt wird, wie der Benutzer navigieren kann und wie der Benutzer die Ansicht verlässt.  

Der folgende Code kann verwendet werden, um eine `WKWebView` in ihrer xamarin. IOS-APP zu starten:

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

Beachten Sie, dass sich `WKWebView` im `WebKit`-Namespace befindet, sodass Sie diese using-Direktive am Anfang der Klasse hinzufügen müssen.

`WKWebView` können auch in xamarin. Mac-Apps verwendet werden, und Sie sollten es verwenden, wenn Sie eine plattformübergreifende Mac/IOS-app erstellen.

Das Rezept zum [Behandeln von JavaScript-Warnungen](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) bietet auch Informationen zur Verwendung von wkwebview mit JavaScript.

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

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

[![eine Beispiel Webansicht mit sfsafariviewcontroller](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Es ist auch möglich, die mobile Safari-App aus der APP heraus zu öffnen, indem Sie den folgenden Code verwenden:

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

Dadurch wird die folgende Webansicht erzeugt:

[![einer in Safari dargestellten Webseite](webview-images/safari.png)](webview-images/safari.png#lightbox)

Die Navigation von Benutzern zu Safari sollte in der Regel immer vermieden werden. Die meisten Benutzer erwarten keine Navigation außerhalb Ihrer Anwendung. Wenn Sie also von ihrer app Weg navigieren, können Benutzer diese nicht mehr zurückgeben.

mit den Verbesserungen für IOS 9 kann der Benutzer problemlos über eine Schaltfläche "zurück", die in der linken oberen Ecke der Safari-Seite angezeigt wird, zu Ihrer APP zurückkehren.

## <a name="app-transport-security"></a>App-Transportsicherheit

App-Transport Sicherheit oder *ATS* wurde von Apple in ios 9 eingeführt, um sicherzustellen, dass die gesamte Internetkommunikation den bewährten Methoden der sicheren Verbindung entspricht.

Weitere Informationen zu den Vorgängen, einschließlich der Implementierung in der APP, finden Sie im Leitfaden zur [App-Transport Sicherheit](~/ios/app-fundamentals/ats.md) .

## <a name="uiwebview-deprecated"></a>UIWebView (veraltet)

> [!IMPORTANT]
> `UIWebView` ist veraltet. Apps, die dieses Steuerelement verwenden [, werden vom April 2020 nicht in den App Store übernommen, und vorhandene apps müssen es bis zum 2020. Dezember entfernen](https://developer.apple.com/news/?id=12232019b).
>
> In [der `UIWebView` Dokumentation von Apple](https://developer.apple.com/documentation/uikit/uiwebview) wird vorgeschlagen, dass apps stattdessen [`WKWebView`](#wkwebview) verwenden sollten.

> [!IMPORTANT]
> Wenn Sie Xamarin.Forms verwenden und nach Ressourcen zur `UIWebView`-Veraltungswarnung (ITMS-90809) suchen, finden Sie weitere Informationen in der [Dokumentation zu Xamarin.Forms-WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809).

`UIWebView` ist die ältere Methode von Apple, Webinhalte in Ihrer APP bereitzustellen. Sie wurde in ios 2,0 veröffentlicht und ist seit 8,0 als veraltet markiert.

Um ihrer xamarin. IOS-App eine UIWebView hinzuzufügen, verwenden Sie den folgenden Code:

```csharp
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://docs.microsoft.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Dadurch wird die folgende Webansicht erzeugt:

[![der Auswirkung von scalespagestofit](webview-images/webview.png)](webview-images/webview.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Webviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
