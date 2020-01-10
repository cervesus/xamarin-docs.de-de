---
title: Webansichten in xamarin. IOS
description: In diesem Dokument werden die verschiedenen Möglichkeiten beschrieben, wie eine xamarin. IOS-App Webinhalte anzeigen kann. Darin werden wkwebview, sfsafariviewcontroller, Safari und App-Transportsicherheit erläutert.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 1ae3a2af436a4ad8860ab27df550a1d74d5084a6
ms.sourcegitcommit: 0ffef1721f28717d46c8168ec96a45b6fe96b623
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2020
ms.locfileid: "75718765"
---
# <a name="web-views-in-xamarinios"></a>Webansichten in xamarin. IOS

Die Lebensdauer von IOS Apple hat eine Reihe von Möglichkeiten für App-Entwickler veröffentlicht, webansichts Funktionen in Ihre apps einzubinden. Die meisten Benutzer verwenden den integrierten Safari-Webbrowser auf Ihrem IOS-Gerät und erwarten daher, dass die webansichtenfunktionalität von anderen apps mit dieser Funktionalität konsistent ist. Sie erwarten, dass die gleichen Gesten funktionieren, die Leistung auf dem gleichen Niveau und die Funktionalität identisch ist.

IOS 11 hat neue Änderungen an `WKWebView` und `SFSafariViewController`eingeführt. Weitere Informationen hierzu finden Sie im Leitfaden für [webänderungen in ios 11](~/ios/platform/introduction-to-ios11/web.md) .

## <a name="wkwebview"></a>WKWebView

`WKWebView` wurde in ios 8 eingeführt, sodass App-Entwickler eine Webbrowserschnittstelle implementieren können, die der von Mobile Safari ähnelt. Dies liegt teilweise an der Tatsache, dass `WKWebView` die Nitro-Javascript-Engine verwendet, die gleiche Engine, die von Mobile Safari verwendet wird. `WKWebView` should always be used over UIWebView where possible due to the [increased performance](http://blog.initlabs.com/post/100113463211/wkwebview-vs-uiwebview),built in user-friendly gestures, and the ease of interaction between the web page and your app.
  
`WKWebView` can be added to your app in an almost identical way to UIWebView, however as the developer you have much more control over the UI/UX and functionality. Creating and displaying the web view object will display the requested page, however you can control how the view is presented, how the user can navigate, and how the user exits the view.  

The code below can be used to launch a `WKWebView` in your Xamarin.iOS app:

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

It is important to note that `WKWebView` is in the `WebKit` namespace, so you will have to add this using directive to the top of your class.

`WKWebView` can also be used in Xamarin.Mac apps, and you should use it if you are creating a cross-platform Mac/iOS app.

The [Handle JavaScript Alerts](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) recipe also provides information on using WKWebView with Javascript.

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController` is the latest way to provide web content from your app and is available in iOS 9 and later. Unlike `UIWebView` or `WKWebView`, `SFSafariViewController` is a View Controller and so cannot be used with other views. You should present `SFSafariViewController` as a new View Controller, in the same way you would present any View Controller.

 `SFSafariViewController` is essentially a 'mini safari' that can be embedded into your app. Like WKWebView it uses the same Nitro Javascript Engine, but also provides a range of additional Safari features such as AutoFill, Reader, and the ability to share cookies and data with mobile Safari. Interaction between the user and the `SFSafariViewController` is not accessible to your app. Your app will not have access to any of the default Safari features.

It also, by default, implements a **Done** button, allowing to user to easily return to your app, and forward and back navigation buttons, allowing your user to navigate through a stack of web pages. In addition, it also provides the user with an address bar giving them the peace of mind that they are on the expected web page. The address bar does not allow the user to change the url. 

These implementations cannot be changed, so `SFSafariViewController` is ideal to use as the default browser if your app wants to present a webpage without any customization.

The code below can be used to launch a `SFSafariViewController` in your Xamarin.iOS app:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Dadurch wird die folgende Webansicht erzeugt:

[![An example web view with SFSafariViewController](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

It is also possible to open the mobile Safari app from within your app, by using the code below:

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

Dadurch wird die folgende Webansicht erzeugt:

[![A web page presented in Safari](webview-images/safari.png)](webview-images/safari.png#lightbox)

Navigating users away from your app to Safari should generally always be avoided. Most users will not expect navigation outside of your application, so if you navigate away from your app, users may never return it, essentially killing engagement.

iOS 9 improvements allow the user to easily return to your app through a back button that is provided in the top left corner of the Safari page.

## <a name="app-transport-security"></a>App-Transportsicherheit

App Transport Security, or *ATS* was introduced by Apple in iOS 9 to ensure that all internet communications conform to secure connection best practices.

For more information on ATS, including how to implement it in your app, refer to the [App Transport Security](~/ios/app-fundamentals/ats.md) guide.

## <a name="uiwebview-deprecated"></a>UIWebView (deprecated)

> [!IMPORTANT]
> `UIWebView` ist veraltet. Apps using this control will [not be accepted into the App Store as of April 2020, and existing apps need to remove it by December 2020](https://developer.apple.com/news/?id=12232019b).
> 
> [Apple's `UIWebView` documentation](https://developer.apple.com/documentation/uikit/uiwebview) suggests apps should use [`WKWebView`](#wkwebview) instead.

`UIWebView` is Apple's legacy way of providing web content in your app. It was released in iOS 2.0, and has been deprecated as of 8.0.

Um ihrer xamarin. IOS-App eine UIWebView hinzuzufügen, verwenden Sie den folgenden Code:

```csharp
webView = new UIWebView (View.Bounds);
View.AddSubview(webView);

var url = "https://docs.microsoft.com"; // NOTE: https secure request
webView.LoadRequest(new NSUrlRequest(new NSUrl(url)));
```

Dadurch wird die folgende Webansicht erzeugt:

[![der Auswirkung von scalespagestofit](webview-images/webview.png)](webview-images/webview.png#lightbox)

## <a name="related-links"></a>Verwandte Themen

- [Webviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
