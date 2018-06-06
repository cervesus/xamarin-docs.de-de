---
title: Web-Änderungen in iOS 11
description: Dieses Dokument erläutert die Änderungen an WebKit und das dienstframework Safari unter iOS 11. Es wird beschrieben, wie mithilfe des Updates in SFSafariViewController und neue Features in WKWebView formatieren.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/12/2016
ms.openlocfilehash: f5876a9d201950ebac45e8b1f786b0e97452a7f1
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787447"
---
# <a name="web-changes-in-ios-11"></a>Web-Änderungen in iOS 11

iOS 11 wird eine neue Version des Safari-Webbrowser – Safari 11.0 – darunter Änderungen WebKit und SafariServices eingeführt. Dieses Handbuch untersucht diese Änderungen.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` wurde als eine Option zum Anzeigen von Webinhalten oder die Authentifizierung von Benutzern aus Ihrer app unter iOS 9 eingeführt. Weitere Informationen auf den zugehörigen Funktionen finden Sie der [Webansichten](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) Handbuch.

iOS 11 hat Stil Updates in Safari View-Controller, Vergabe von Benutzer mehr nahtlos zwischen einer app mit dem Web eingeführt. Beispielsweise das Entfernen der Adressleiste jetzt bietet den Safari-View-Controller das Verhalten eines in-app-Browsers, statt einen Mini-Browser. Sie können auch anpassen, das Farbschema passt sich mit Ihrer App das Farbschema durch Festlegen der `preferredBarTintColor` und `PreferredControlTintColor` Eigenschaften:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Der folgende Codeausschnitt rendert die Balken in Violett und weiß, wie in der folgenden Abbildung angezeigt:

![SFSafariViewController Balken in Violett und weiß dargestellt](web-images/image1.png)

Die Schaltfläche "Schließen", die im Safari-View-Controller dargestellt kann auch geändert werden, indem die `DismissButtonStyle` -Eigenschaft entweder `Done`, `Close`, oder `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Verwerfen von Schaltflächentext geändert](web-images/image2.png)

Dieser Wert kann geändert werden, während `SFSafariViewController` erhält.


Je nach den Inhalt, der in einem Safari-View-Controller angezeigt wird, ist es möglicherweise erforderlich, um sicherzustellen, dass die Menüleisten als der Benutzer einen Bildlauf zu reduzieren, nicht. Dies erfolgt durch Festlegen der neuen `BarCollapsedEnabled` Eigenschaft `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Leiste reduzieren, deaktiviert](web-images/image3.png)

Apple hat auch Updates zum Datenschutz in Safari-View-Controller in iOS-11. Durchsuchen von Daten jetzt, wie Cookies und lokalem Speicher nur auf Basis eines app-bezogenes und nicht für alle Instanzen des Safari-View-Controller vorhanden. Auf diese Weise browsing Benutzeraktivität privaten innerhalb Ihrer app.

Zusätzliche Funktionen wie z. B. ziehen und drop-Unterstützung für URLs und Unterstützung für `window.open()` wurden ebenfalls hinzugefügt `SFSafariViewController` in iOS-11. Weitere Informationen zu diesen neuen Features in finden Sie [Apple SFSafariViewController Dokumentation](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` im Rahmen des WebKit in iOS 8 als Mittel zum Anzeigen von Webinhalten zu Ihrem Benutzerkonto wurde eingeführt werden. Es ist wesentlich mehr als anpassbare `SFSafariViewController`, sodass Sie eigene Navigation und der Benutzeroberfläche zu erstellen.

Apple hat drei wichtigsten Verbesserungen für eingeführt `WKWebView` mit iOS 11: 

- Die Fähigkeit zum Verwalten von cookies
- Content-Filterung
- Benutzerdefinierte Ressource laden. 

Cookieverwaltung erfolgt über die neue [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) -Klasse, wodurch Sie zum Hinzufügen und Löschen von Cookies erhalten die Cookies, die in einem WKWebView gespeichert, und beobachten das Cookie für die Änderungen zu speichern.

Inhalte filtern können Sie den Typ des Inhalts zu verwalten, die Ihre Benutzer angezeigt wird, sodass Sie sicherstellen, dass es ist sicherer, Familie freundliche und, falls erforderlich, sind nur für eine ausgewählte Benutzergruppe verfügbar. Dies wird durch die neue implementiert [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) Klasse, indem Sie Paare von Triggern und Aktionen im JSON-Format. Weitere Informationen zu diesen Triggern und Aktionen finden Sie in der Apple- [blockieren Inhaltsregeln](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) Handbuch.

iOS 11 ermöglicht Ihnen die Anpassung jetzt `WKWebView` mit benutzerdefinierten Ressource, die für den Inhalt der Webseite zu laden. Dies wird durch implementiert die `IWKUrlSchemeHandler` -Schnittstelle, können Sie die URL-Schemas zu behandeln, die nicht vom einheitlichen Modus zum Web Kit sind. Diese Schnittstelle verfügt über eine Start- und Stop-Methode, die implementiert werden muss:

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

Sobald der Handler implementiert wurde, verwenden, um festzulegen der `SetUrlSchemeHandler` Eigenschaft auf die `WKWebViewConfiguration`. Laden Sie dann die URL von etwas, das das benutzerdefinierte Schema verwendet:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

