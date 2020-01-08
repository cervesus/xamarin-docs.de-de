---
title: Änderungen an WebKit und Safari in ios 11
description: In diesem Dokument werden Änderungen erläutert, die an WebKit und dem Safari Services-Framework in ios 11 vorgenommen wurden. Es wird beschrieben, wie Sie mit Formatierungs Updates in sfsafariviewcontroller und neuen Features in wkwebview arbeiten.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/12/2017
ms.openlocfilehash: c52ac3c0f06d58ab5fff8228ca3bdf722056b5b6
ms.sourcegitcommit: bad1ab3f78d7f94d48511666626b54f8ba155689
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663446"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>Änderungen an WebKit und Safari in ios 11

IOS 11 führt eine neue Version des Safari-Webbrowsers – Safari 11,0 – ein, die Änderungen an WebKit und safariservices umfasst. In dieser Anleitung werden diese Änderungen erläutert.

## <a name="safariservices"></a>Safariservices

`SFSafariViewController` wurde in ios 9 als Option zum Anzeigen von Webinhalten oder Authentifizieren von Benutzern in Ihrer APP eingeführt. Weitere Informationen zu den Funktionen finden Sie im Handbuch für [Webansichten](~/ios/user-interface/controls/webview.md#sfsafariviewcontroller) .

IOS 11 hat Stil Updates für den Safari-Ansichts Controller eingeführt, sodass Ihre Benutzer eine nahtlose Benutzer Darstellung zwischen einer APP und dem Web erhalten. Beispielsweise gibt das Entfernen der Adressleiste nun dem Safari-Ansichts Controller das Gefühl eines in-App-Browsers und nicht eines Mini Browsers. Sie können das Farbschema auch anpassen, um das Farbschema Ihrer APP anzupassen, indem Sie die Eigenschaften "`preferredBarTintColor`" und "`PreferredControlTintColor`" festlegen:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Der folgende Code Ausschnitt rendert die Balken in violett und weiß, wie in der folgenden Abbildung dargestellt:

![Sfsafariviewcontroller-Balken in violett und weiß gerendert](web-images/image1.png)

Die im Safari-Ansichts Controller dargestellte Schaltfläche verwerfen kann auch geändert werden, indem Sie die `DismissButtonStyle`-Eigenschaft entweder auf `Done`, `Close`oder `Cancel`festlegen:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Schaltflächen Text verwerfen geändert](web-images/image2.png)

Dieser Wert kann geändert werden, während `SFSafariViewController` angezeigt wird.

Abhängig von dem Inhalt, der in einem Safari-Ansichts Controller angezeigt wird, kann es erforderlich sein, sicherzustellen, dass die Menüleisten nicht reduziert werden, wenn der Benutzer einen Bildlauf ausführt. Dies wird durch Festlegen der neuen `BarCollapsedEnabled`-Eigenschaft auf `false`ermöglicht:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Die Leiste wird deaktiviert.](web-images/image3.png)

Außerdem hat Apple im Safari View Controller in ios 11 Updates für den Datenschutz durchgeführt. Das Durchsuchen von Daten, wie z. b. Cookies und lokaler Speicher, ist nun nur auf App-Basis und nicht in allen Instanzen von Safari View Controller vorhanden. Dadurch wird die Benutzer Browseraktivität innerhalb Ihrer APP privat.

Zusätzliche Funktionen wie Drag & Drop-Unterstützung für URLs und Unterstützung für `window.open()` wurden ebenfalls `SFSafariViewController` in ios 11 hinzugefügt. Weitere Informationen zu diesen neuen Features finden Sie in [der Dokumentation zu sfsafariviewcontroller von Apple](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).

## <a name="webkit"></a>WebKit

`WKWebView` wurde als Teil von WebKit in ios 8 eingeführt, um dem Benutzer Webinhalte anzuzeigen. Es ist viel anpassbarer als `SFSafariViewController`, sodass Sie eine eigene Navigations-und Benutzeroberfläche erstellen können.

Apple hat drei wesentliche Verbesserungen für `WKWebView` mit IOS 11 eingeführt: 

- Die Fähigkeit zum Verwalten von Cookies
- Inhalts Filterung
- Laden benutzerdefinierter Ressourcen

Die Cookieverwaltung erfolgt über die neue [`WKHttpCookieStore`](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) -Klasse, die es Ihnen ermöglicht, Cookies hinzuzufügen und zu löschen, alle in einer wkwebview gespeicherten Cookies zu erhalten und den Cookiespeicher für Änderungen zu beobachten.

Die Inhalts Filterung ermöglicht Ihnen die Verwaltung der Art von Inhalt, der dem Benutzer angezeigt wird, sodass Sie sicherstellen können, dass er sicher, familienfreundlich und ggf. nur für eine ausgewählte Benutzergruppe verfügbar ist. Dies wird durch die neue [`WKContentRuleList`](https://developer.apple.com/documentation/webkit/wkcontentrulelist) -Klasse implementiert, indem paar Trigger und Aktionen in JSON bereitgestellt werden. Weitere Informationen zu diesen Triggern und Aktionen finden Sie im Handbuch zu den [Inhalts Blockierungs Regeln](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) von Apple.

IOS 11 ermöglicht Ihnen jetzt das Anpassen `WKWebView` mit benutzerdefiniertem Laden von Ressourcen für Ihre Webinhalte. Dies wird über die `IWKUrlSchemeHandler`-Schnittstelle implementiert, mit der Sie URL-Schemas verarbeiten können, die nicht im Web-Kit einheitlich sind. Diese Schnittstelle verfügt über eine Start-und Stoppmethode, die implementiert werden muss:

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

Nachdem der Handler implementiert wurde, verwenden Sie ihn, um die `SetUrlSchemeHandler`-Eigenschaft für die `WKWebViewConfiguration`festzulegen. Laden Sie dann die URL eines etwas, das das benutzerdefinierte Schema verwendet:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```
