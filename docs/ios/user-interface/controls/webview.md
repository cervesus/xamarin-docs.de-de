---
title: Webansichten in Xamarin.iOS
description: In diesem Dokument werden die verschiedenen Möglichkeiten beschrieben, wie eine Xamarin.iOS-App Webinhalte anzeigen kann. Es werden WKWebView, SFSafariViewController, Safari und die Sicherheitssicherheit des App-Transports erläutert.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: cc6c87452bf5312d66e33b7c49d3dc216eceb7d6
ms.sourcegitcommit: b93754b220fca3d6e3d131341e3cfbe233d10f84
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/03/2020
ms.locfileid: "80628295"
---
# <a name="web-views-in-xamarinios"></a>Webansichten in Xamarin.iOS

Im Laufe der Lebensdauer von iOS hat Apple eine Reihe von Möglichkeiten für App-Entwickler veröffentlicht, Web-View-Funktionen in ihre Apps zu integrieren. Die meisten Benutzer nutzen den integrierten Safari-Webbrowser auf ihrem iOS-Gerät und erwarten daher, dass die Webansichtsfunktionalität anderer Apps mit dieser Erfahrung übereinstimmt. Sie erwarten, dass die gleichen Gesten funktionieren, die Leistung auf Augenhöhe ist und die Funktionalität gleich ist.

iOS 11 führte neue `WKWebView` `SFSafariViewController`Änderungen an beiden und ein. Weitere Informationen hierzu finden Sie im [Handbuch "Webänderungen" in iOS 11.](~/ios/platform/introduction-to-ios11/web.md)

## <a name="wkwebview"></a>WKWebView

`WKWebView`wurde in iOS 8 eingeführt, sodass App-Entwickler eine Web-Browsing-Schnittstelle ähnlich der mobilen Safari implementieren können. Dies ist zum Teil auf `WKWebView` die Tatsache zurückzuführen, dass die Nitro Javascript-Engine verwendet, die gleiche Engine von mobilen Safari verwendet. `WKWebView`sollte aufgrund der verbesserten Leistung, der integrierten benutzerfreundlichen Gesten und der einfachen Interaktion zwischen der Webseite und Ihrer App immer über UIWebView verwendet werden.

`WKWebView`kann Ihrer App auf nahezu identische Weise zu UIWebView hinzugefügt werden, als Entwickler haben Sie jedoch viel mehr Kontrolle über die Benutzeroberfläche/UX und Funktionalität. Beim Erstellen und Anzeigen des Webansichtsobjekts wird die angeforderte Seite angezeigt, Sie können jedoch steuern, wie die Ansicht dargestellt wird, wie der Benutzer navigieren kann und wie der Benutzer die Ansicht verlässt.  

Der folgende Code kann verwendet `WKWebView` werden, um eine in Ihrer Xamarin.iOS-App zu starten:

```csharp
WKWebView webView = new WKWebView(View.Frame, new WKWebViewConfiguration());
View.AddSubview(webView);

var url = new NSUrl("https://docs.microsoft.com");
var request = new NSUrlRequest(url);
webView.LoadRequest(request);
```

Es ist wichtig `WKWebView` zu beachten, dass sich der `WebKit` Namespace befindet, daher müssen Sie diese mit einer Direktive an die Spitze Ihrer Klasse hinzufügen.

`WKWebView`kann auch in Xamarin.Mac-Apps verwendet werden, und Sie sollten es verwenden, wenn Sie eine plattformübergreifende Mac/iOS-App erstellen.

Das [Handle JavaScript Alerts-Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/web_view/handle_javascript_alerts) enthält auch Informationen zur Verwendung von WKWebView mit Javascript.

## <a name="sfsafariviewcontroller"></a>SFSafariViewController

 `SFSafariViewController`ist die neueste Möglichkeit, Webinhalte aus Ihrer App bereitzustellen und ist in iOS 9 und höher verfügbar. Im `UIWebView` `WKWebView`Gegensatz `SFSafariViewController` zu oder ist ein View Controller und kann daher nicht mit anderen Ansichten verwendet werden. Sie sollten `SFSafariViewController` als neuer View Controller präsentieren, so wie Sie jeden View Controller präsentieren würden.

 `SFSafariViewController`ist im Wesentlichen eine "Mini-Safari", die in Ihre App eingebettet werden kann. Wie WKWebView verwendet es die gleiche Nitro Javascript Engine, bietet aber auch eine Reihe von zusätzlichen Safari-Funktionen wie AutoFill, Reader und die Möglichkeit, Cookies und Daten mit mobilem Safari zu teilen. Die Interaktion zwischen `SFSafariViewController` dem Benutzer und dem ist für Ihre App nicht zugänglich. Ihre App hat keinen Zugriff auf eine der standardmäßigen Safari-Funktionen.

Es implementiert standardmäßig auch eine **Schaltfläche "Fertig",** sodass der Benutzer problemlos zu Ihrer App zurückkehren und Navigationsschaltflächen vorwärts und zurückleiten kann, sodass der Benutzer durch einen Stapel von Webseiten navigieren kann. Darüber hinaus bietet es dem Benutzer auch eine Adressleiste, die ihm die Sicherheit gibt, dass er sich auf der erwarteten Webseite befindet. Die Adressleiste erlaubt es dem Benutzer nicht, die URL zu ändern.

Diese Implementierungen können nicht `SFSafariViewController` geändert werden, daher ist es ideal, als Standardbrowser zu verwenden, wenn Ihre App eine Webseite ohne Anpassung präsentieren möchte.

Der folgende Code kann verwendet `SFSafariViewController` werden, um eine in Ihrer Xamarin.iOS-App zu starten:

```csharp
var sfViewController = new SFSafariViewController(url);

PresentViewController(sfViewController, true, null);
```

Dadurch wird die folgende Webansicht erzeugt:

[![Eine Beispiel-Webansicht mit SFSafariViewController](webview-images/sfsafariviewcontroller.png)](webview-images/sfsafariviewcontroller.png#lightbox)

## <a name="safari"></a>Safari

Es ist auch möglich, die mobile Safari-App in Ihrer App zu öffnen, indem Sie den folgenden Code verwenden:

```csharp
var url = new NSUrl("https://docs.microsoft.com");

UIApplication.SharedApplication.OpenUrl(url);
```

Dadurch wird die folgende Webansicht erzeugt:

[![Eine in Safari vorgestellte Webseite](webview-images/safari.png)](webview-images/safari.png#lightbox)

Das Navigieren von Benutzern von Ihrer App zu Safari sollte in der Regel immer vermieden werden. Die meisten Benutzer erwarten keine Navigation außerhalb Ihrer Anwendung, wenn Sie also von Ihrer App weg navigieren, können Benutzer sie nie zurückgeben, was im Wesentlichen die Interaktion tötet.

IOS 9-Verbesserungen ermöglichen es dem Benutzer, einfach über eine Zurück-Schaltfläche zu Ihrer App zurückzukehren, die in der oberen linken Ecke der Safari-Seite bereitgestellt wird.

## <a name="app-transport-security"></a>App-Transportsicherheit

App Transport Security *(ATS)* wurde von Apple in iOS 9 eingeführt, um sicherzustellen, dass alle Internetkommunikationen den bewährten Methoden für sichere Verbindungen entsprechen.

Weitere Informationen zu ATS, einschließlich der Implementierung in Ihrer App, finden Sie im [Handbuch App Transport Security.](~/ios/app-fundamentals/ats.md)

## <a name="uiwebview-deprecation"></a>UIWebView-Veraltete

`UIWebView`ist Apples ältere Art, Webinhalte in Ihrer App bereitzustellen. Es wurde in iOS 2.0 veröffentlicht und ist seit 8.0 veraltet.

> [!IMPORTANT]
> `UIWebView` ist veraltet. Neue Apps, die dieses Steuerelement verwenden, [werden ab April 2020 nicht mehr in den App Store aufgenommen, und Apps-Updates mit diesem Steuerelement werden bis Dezember 2020 nicht akzeptiert.](https://developer.apple.com/news/?id=12232019b)
>
> [Apples `UIWebView` Dokumentation](https://developer.apple.com/documentation/uikit/uiwebview) schlägt vor, dass Apps stattdessen verwendet [`WKWebView`](#wkwebview) werden sollten.
>
> Wenn Sie Xamarin.Forms verwenden und nach Ressourcen zur `UIWebView`-Veraltungswarnung (ITMS-90809) suchen, finden Sie weitere Informationen in der [Dokumentation zu Xamarin.Forms-WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809).

Entwickler, die iOS-Anwendungen in den letzten sechs Monaten (oder so) `UIWebView` eingereicht haben, haben möglicherweise eine Warnung aus dem App Store erhalten, dass sie veraltet sind.

Veraltungen von APIs sind häufig. Xamarin.iOS verwendet benutzerdefinierte Attribute, um diese APIs (und vorschläge Ersatz, wenn verfügbar) an die Entwickler zu signalisieren. Was dieses Mal anders ist, und viel weniger häufig, ist, dass die Veraltung durch Apples App Store zur Einreichungszeit **erzwungen wird.**

Leider ist `UIWebView` das `Xamarin.iOS.dll` Entfernen des Typs aus einer [binären Bruchänderung](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility). Diese Änderung bricht vorhandene Bibliotheken von Drittanbietern, einschließlich einiger Bibliotheken, die möglicherweise nicht mehr unterstützt oder sogar neu kompilierbar sind (z. B. geschlossene Quelle). Dadurch entstehen nur zusätzliche Probleme für Entwickler. Daher entfernen wir den Typ *noch*nicht .

Ab [Xamarin.iOS 13.16](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16) stehen neue Erkennungs- und `UIWebView`Tools zur Verfügung, die Ihnen bei der Migration von helfen.

### <a name="detection"></a>Erkennung

Wenn Sie vor kurzem keine iOS-Anwendung im Apple App Store eingereicht haben, fragen Sie sich vielleicht, ob diese Situation auf Ihre(n) Anwendung/n zutrifft.

Um dies herauszufinden, `--warn-on-type-ref=UIKit.UIWebView` können Sie die **zusätzlichen Mtouch-Argumente** Ihres Projekts hinzufügen. Dadurch wird vor **jedem** Verweis auf die `UIWebView` veralteten in Ihrer Anwendung (und alle ihre Abhängigkeiten) warnt. Verschiedene Warnungen werden verwendet, um Typen **vor** und **nach** der Ausführung des verwalteten Linkers zu melden.

Die Warnungen können, wie andere `-warnaserror:`auch, mithilfe von in Fehler umgewandelt werden. Dies kann nützlich sein, wenn Sie `UIWebView` sicherstellen möchten, dass eine neue Abhängigkeit nach den Überprüfungen nicht hinzugefügt wird. Beispiel:

* `-warnaserror:1502`meldet Fehler, wenn Verweise in vorverknüpften Assemblys gefunden werden.
* `-warnaserror:1503`meldet Fehler, wenn Verweise in nachverknüpften Assemblys gefunden werden.

Sie können die Warnungen auch zum Schweigen bringen, wenn die Ergebnisse der Vorab-/Nachverknüpfung nicht nützlich sind. Beispiel:

* `-nowarn:1502`meldet **keine** Warnungen, wenn Verweise in vorverknüpften Assemblys gefunden werden.
* `-nowarn:1503`meldet **keine** Warnungen, wenn Verweise in postverknüpften Assemblys gefunden werden.

### <a name="removal"></a>Entfernen

Jede Anwendung ist einzigartig. Das `UIWebView` Entfernen aus der Anwendung kann je nach Verwendung s. Die häufigsten Szenarien sind wie folgt:

- Es gibt keine `UIWebView` Verwendung innerhalb Ihrer Anwendung. Alles ist in Ordnung. Sie sollten **keine** Warnungen haben, wenn Sie an den AppStore senden. Nichts anderes ist von Ihnen verlangt.
- Direkte Nutzung `UIWebView` durch Ihre Anwendung. Entfernen Sie zunächst `UIWebView`Ihre Verwendung von , ersetzen `WKWebView` Sie sie z. `SFSafariViewController` B. durch die typen neuer (iOS 8) oder (iOS 9). Sobald dies abgeschlossen ist, sollte der `UIWebView` verwaltete Linker keinen Verweis sehen, und die endgültige App-Binärdatei hat keine Spur davon.
- Indirekte Nutzung. `UIWebView`kann in einigen Bibliotheken von Drittanbietern vorhanden sein, entweder verwaltet oder systematonisch, die von Ihrer Anwendung verwendet werden. Aktualisieren Sie zunächst Ihre externen Abhängigkeiten auf die neuesten Versionen, da diese Situation möglicherweise bereits in einer neueren Version behoben ist. Wenn dies nicht der Fall ist, wenden Sie sich an die Betreuer der Bibliotheken, und fragen Sie nach ihren Aktualisierungsplänen.

Alternativ können Sie die folgenden Ansätze ausprobieren:

1. Wenn Sie **Xamarin.Forms**verwenden, lesen Sie diesen [Blogbeitrag](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/).
1. Aktivieren Sie den verwalteten Linker (für das gesamte `UIWebView`Projekt oder zumindest für die Abhängigkeit mit ), damit er entfernt werden *kann,* wenn nicht darauf verwiesen wird. Dadurch wird das Problem behoben, es kann jedoch zusätzliche Arbeit erforderlich sein, um den Codelinker sicher zu machen.
1. Wenn Sie die Einstellungen für verwaltete Linker nicht ändern können, lesen Sie die Sonderfälle unten.

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>Anwendungen können den Linker nicht verwenden (oder seine Einstellungen ändern)

Wenn Sie aus irgendeinem Grund den verwalteten Linker **nicht** verwenden (z. B. **nicht verknüpfen**), verbleibt das Symbol in der `UIWebView` Binär-App, die Sie an Apple übermitteln, und es kann abgelehnt werden.

Eine *zwingende* Lösung `--optimize=force-rejected-types-removal` besteht darin, die **zusätzlichen Mtouch-Argumente**Ihres Projekts hinzuzufügen. Dadurch werden Spuren `UIWebView` von aus der Anwendung entfernt. Code, der auf den Typ verweist, funktioniert jedoch **nicht** ordnungsgemäß (Ausnahmen oder Abstürze erwarten). Dieser Ansatz sollte nur verwendet werden, wenn Sie sicher sind, dass der Code zur Laufzeit nicht erreichbar ist (auch wenn er durch statische Analyse erreichbar war).

#### <a name="support-for-ios-7x-or-earlier"></a>Unterstützung für iOS 7.x (oder früher)

`UIWebView`ist seit v2.0 Teil von iOS. Die häufigsten Ersatzprodukte `WKWebView` sind (iOS `SFSafariViewController` 8) und (iOS 9). Wenn Ihre Anwendung immer noch ältere iOS-Versionen unterstützt, sollten Sie die folgenden Optionen in Betracht ziehen:

* Machen Sie iOS 8 zu Ihrer minimalen Zielversion (eine Buildzeitentscheidung).
* Verwenden `WKWebView` Sie diese Nur, wenn die App unter iOS 8+ ausgeführt wird (eine Laufzeitentscheidung).

#### <a name="applications-not-submitted-to-apple"></a>Anträge, die nicht bei Apple eingereicht wurden

Wenn Ihre Anwendung nicht an Apple gesendet wird, sollten Sie planen, sich von der veralteten API zu entfernen, da sie in zukünftigen iOS-Versionen entfernt werden kann. Sie können diesen Übergang jedoch mit Ihrem eigenen Zeitplan machen.

## <a name="related-links"></a>Verwandte Links

- [WebViews (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
