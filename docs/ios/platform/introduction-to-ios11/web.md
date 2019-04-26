---
title: Webänderungen in iOS 11
description: Dieses Dokument erläutert die Änderungen an WebKit und die Safari-Services-Framework in iOS 11. Es wird beschrieben, wie mit Formatierung SFSafariViewController Updates und neuen Funktionen in WKWebView arbeiten.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/12/2017
ms.openlocfilehash: ba691a6605dcf7e86a76ed13d4c8ef5f0984ff6e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61399728"
---
# <a name="web-changes-in-ios-11"></a>Webänderungen in iOS 11

iOS 11 führt eine neue Version des Safari-Webbrowser – Safari 11.0 – Änderungen an WebKit und SafariServices enthält. Dieses Handbuch beschreibt diese Änderungen.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` wurde als Option für die Anzeige von Webinhalten oder zum Authentifizieren von Benutzern aus Ihrer app in iOS 9 eingeführt. Weitere Informationen zu Features finden Sie der [Webansichten](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) Guide.

iOS 11 wurde Style-Updates auf den Safari-Ansichtscontroller, erteilen Ihren Benutzern mehr nahtlos zwischen einer app und das Web eingeführt. Z. B. das Entfernen der Adressleiste zeigt Ihnen nun die Safari-View-Controller das Verhalten einer in-app-Browser, statt einen Minibrowser. Sie können auch anpassen, das Farbschema passen sich des Farbschemas für Ihre app durch Festlegen der `preferredBarTintColor` und `PreferredControlTintColor` Eigenschaften:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

Der folgende Codeausschnitt rendert die Balken in Violett und weiß, wie in der folgenden Abbildung angezeigt:

![SFSafariViewController Balken in Violett und weiß gerendert](web-images/image1.png)

Die Schaltfläche "Schließen" angezeigt, in der Safari-View-Controller kann auch geändert werden, durch Festlegen der `DismissButtonStyle` Eigenschaft entweder `Done`, `Close`, oder `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Schließen der Text der Schaltfläche geändert](web-images/image2.png)

Dieser Wert kann geändert werden zwar `SFSafariViewController` wird angezeigt.


Je nach Inhalt, der in einem Safari-View-Controller angezeigt wird, ist es möglicherweise erforderlich, um sicherzustellen, dass die Menüleisten als der Benutzer einen Bildlauf zu reduzieren, nicht. Dies wird aktiviert, indem die neue `BarCollapsedEnabled` Eigenschaft `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Leiste reduzieren, deaktiviert](web-images/image3.png)

Apple hat auch Aktualisierungen zum Datenschutz in der Safari-View-Controller in iOS 11 vorgenommen. Durchsuchen von Daten, z. B. Cookies und lokaler Speicher nur auf einer pro-app-Basis und nicht für alle Instanzen des Safari-ansichtscontroller vorhanden. Auf diese Weise die Suchaktivitäten privat innerhalb Ihrer app.

Zusätzliche Features wie z. B. ziehen und drop-Unterstützung für URLs und Unterstützung für `window.open()` wurden ebenfalls hinzugefügt `SFSafariViewController` in iOS 11. Weitere Informationen zu diesen neuen Features in finden Sie [Apple SFSafariViewController Dokumentation](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` wurde als Teil des WebKit IOS 8 als Mittel zum Anzeigen von Webinhalten auf den Benutzer eingeführt. Es ist viel besser angepasst werden als `SFSafariViewController`, sodass Sie Ihre eigenen Navigation und der Benutzeroberfläche zu erstellen.

Apple wurden die drei wichtigsten Verbesserungen für `WKWebView` IOS 11: 

- Die Möglichkeit zum Verwalten von cookies
- Inhaltsfilterung
- Benutzerdefinierte Ressource geladen. 

Cookieverwaltung erfolgt über die neue [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) -Klasse, die Ihnen die Möglichkeit zum Hinzufügen und Löschen von Cookies, um alle in einem WKWebView gespeicherten Cookies zu erhalten, und beobachten das Cookie für die Änderungen zu speichern.

Inhalt filtern können Sie den Typ des Inhalts zu verwalten, die Ihre Benutzer angezeigt wird, können Sie sicherstellen, dass es ist sicherer, Familie geeignet, und, falls erforderlich, nur verfügbar für eine ausgewählte Gruppe von Benutzern. Dies wird implementiert, über die neue [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) -Klasse, indem Sie Paare von Triggern und Aktionen im JSON-Format. Weitere Informationen zu diesen Trigger und Aktionen finden Sie im Apple [Inhaltsregeln blockieren](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) Guide.

iOS 11 können Sie anpassen, jetzt `WKWebView` mit benutzerdefinierten Ressource, die für Ihre Webinhalte werden geladen. Dies wird implementiert, über die `IWKUrlSchemeHandler` -Schnittstelle, die können Sie die URL-Schemas zu behandeln, die nicht im einheitlichen Modus zum Web Kit enthalten sind. Diese Schnittstelle verfügt über eine Start- und Stop-Methode, die implementiert werden muss:

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

Nachdem der Handler implementiert wurde, verwenden Sie diesen zum Festlegen der `SetUrlSchemeHandler` Eigenschaft für die `WKWebViewConfiguration`. Laden Sie dann die URL von etwas, das das benutzerdefinierte Schema verwendet:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

