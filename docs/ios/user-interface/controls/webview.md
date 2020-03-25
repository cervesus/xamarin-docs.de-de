---
title: Webansichten in xamarin. IOS
description: In diesem Dokument werden die verschiedenen Möglichkeiten beschrieben, wie eine xamarin. IOS-App Webinhalte anzeigen kann. Darin werden wkwebview, sfsafariviewcontroller, Safari und App-Transportsicherheit erläutert.
ms.prod: xamarin
ms.assetid: 84886CF4-2B2B-4540-AD92-7F0B791952D1
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 7c469a011a70840cfe94a7f87ed77f03968a3525
ms.sourcegitcommit: ec112800a76089ab1db66fe24b8bbcc510e067b4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80159820"
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

## <a name="uiwebview-deprecation"></a>UIWebView-Veraltung

`UIWebView` ist die ältere Methode von Apple, Webinhalte in Ihrer APP bereitzustellen. Sie wurde in ios 2,0 veröffentlicht und ist seit 8,0 als veraltet markiert.

> [!IMPORTANT]
> `UIWebView` ist veraltet. Neue apps, die dieses Steuerelement verwenden [, werden vom April 2020 nicht in den App Store übernommen, und apps, die dieses Steuerelement verwenden, werden bis zum 2020. Dezember nicht akzeptiert](https://developer.apple.com/news/?id=12232019b).
>
> In [der `UIWebView` Dokumentation von Apple](https://developer.apple.com/documentation/uikit/uiwebview) wird vorgeschlagen, dass apps stattdessen [`WKWebView`](#wkwebview) verwenden sollten.
>
> Wenn Sie Xamarin.Forms verwenden und nach Ressourcen zur `UIWebView`-Veraltungswarnung (ITMS-90809) suchen, finden Sie weitere Informationen in der [Dokumentation zu Xamarin.Forms-WebView](~/xamarin-forms/user-interface/webview.md#uiwebview-deprecation-and-app-store-rejection-itms-90809).

Entwickler, die IOS-Anwendungen in den letzten sechs Monaten (oder so) übermittelt haben, haben möglicherweise eine Warnung aus dem App-Store erhalten, wenn `UIWebView` veraltet ist.

Die APIs sind häufig veraltet. Xamarin. IOS verwendet benutzerdefinierte Attribute zum signalisieren dieser APIs (und zum vorschlagen von Ersetzungen, falls verfügbar) an die Entwickler. Das unterscheidet sich diesmal und ist weitaus weniger häufig, besteht darin, dass die Veraltung vom App Store von Apple zum Zeitpunkt der Übermittlung **erzwungen wird** .

Leider ist das Entfernen des `UIWebView` Typs aus `Xamarin.iOS.dll` eine [binäre Breaking Change](https://docs.microsoft.com/dotnet/core/compatibility/categories#binary-compatibility). Durch diese Änderung werden vorhandene Bibliotheken von Drittanbietern getrennt, einschließlich einiger, die möglicherweise nicht mehr unterstützt oder sogar erneut kompiliert werden können (z. b. geschlossene Quelle). Dadurch werden nur weitere Probleme für Entwickler entstehen. Daher wird der Typ *noch*nicht entfernt.

Ab [xamarin. IOS 13,16](https://docs.microsoft.com/xamarin/ios/release-notes/13/13.16) sind neue Erkennungen und Tools verfügbar, um Sie bei der Migration von `UIWebView`zu unterstützen.

### <a name="detection"></a>Erkennung

Wenn Sie vor kurzem eine IOS-Anwendung an den Apple App Store übermittelt haben, Fragen Sie sich vielleicht, ob diese Situation auf Ihre Anwendung (en) zutrifft.

Um dies zu ermitteln, können Sie den **zusätzlichen mberührungs-Argumenten** Ihres Projekts `--warn-on-type-ref=UIKit.UIWebView` hinzufügen. Dadurch wird **ein beliebiger** Verweis auf den veralteten `UIWebView` in der Anwendung (und alle zugehörigen Abhängigkeiten) gewarnt. **Vor** und **nach** der Ausführung des verwalteten Linker werden verschiedene Warnungen verwendet, um Typen zu melden.

Die Warnungen, wie andere, können mithilfe von `-warnaserror:`in Fehler umgewandelt werden. Dies kann hilfreich sein, wenn Sie sicherstellen möchten, dass nach Ihren Überprüfungen eine neue Abhängigkeit zu `UIWebView` nicht hinzugefügt wird. Beispiel:

* `-warnaserror:1502` meldet Fehler, wenn Verweise in vorverknüpften Assemblys gefunden werden.
* `-warnaserror:1503` meldet Fehler, wenn Verweise in nach Links verknüpften Assemblys gefunden werden.

Sie können die Warnungen auch dann über Schweigen, wenn die Prä-/postverknüpfungs Ergebnisse nicht hilfreich sind. Beispiel:

* `-nowarn:1502` meldet **keine** Warnungen, wenn Verweise in vorverknüpften Assemblys gefunden werden.
* `-nowarn:1503` meldet **keine** Warnungen, wenn Verweise in nach Links verknüpften Assemblys gefunden werden.

### <a name="removal"></a>Entfernen

Jede Anwendung ist eindeutig. Wenn Sie `UIWebView` aus der Anwendung entfernen, können je nachdem, wie und wo Sie verwendet werden, unterschiedliche Schritte erforderlich sein. Die häufigsten Szenarien lauten wie folgt:

- In Ihrer Anwendung werden keine `UIWebView` verwendet. Alles ist in Ordnung. Beim Senden an den AppStore sollten **keine** Warnungen vorliegen. Es ist nichts anderes erforderlich.
- Direkte Verwendung von `UIWebView` durch Ihre Anwendung. Beginnen Sie, indem Sie die Verwendung von `UIWebView`entfernen, z. b. durch die neueren `WKWebView` (IOS 8) oder `SFSafariViewController` (IOS 9)-Typen ersetzen. Nachdem dieser Vorgang abgeschlossen ist, sollte der verwaltete Linker keinen Verweis auf `UIWebView` sehen, und die endgültige App-Binärdatei hat keine Ablauf Verfolgung.
- Indirekte Verwendung. `UIWebView` können in einigen Drittanbieterbibliotheken vorhanden sein, die von der Anwendung verwaltet oder System eigen verwendet werden. Beginnen Sie, indem Sie Ihre externen Abhängigkeiten auf ihre neuesten Versionen aktualisieren, da diese Situation möglicherweise bereits in einer neueren Version gelöst wurde. Wenn dies nicht der gibt, wenden Sie sich an den Maintainer (e) der Bibliotheken, und Fragen Sie nach den Update Plänen.

Alternativ können Sie die folgenden Ansätze ausprobieren:

1. Wenn Sie **xamarin. Forms**verwenden, lesen Sie diesen [Blogbeitrag](https://devblogs.microsoft.com/xamarin/uiwebview-deprecation-xamarin-forms/).
1. Aktivieren Sie den verwalteten Linker (für das gesamte Projekt oder zumindest für die Abhängigkeit mit `UIWebView`), damit er *möglicherweise* entfernt wird, wenn er nicht referenziert wird. Dadurch wird das Problem gelöst, aber möglicherweise sind zusätzliche Schritte erforderlich, um den Code Linker sicher zu machen.
1. Wenn Sie die Einstellungen für verwaltete Linker nicht ändern können, finden Sie weitere Informationen in den folgenden Sonderfällen.

#### <a name="applications-cannot-use-the-linker-or-change-its-settings"></a>Anwendungen können den Linker nicht verwenden (oder seine Einstellungen ändern)

Wenn Sie den verwalteten Linker aus irgendeinem Grund **nicht** verwenden (z. b. **nicht verknüpfen), verbleibt**das `UIWebView` Symbol in der binären APP, die Sie an Apple senden, und es wird möglicherweise abgelehnt.

Eine *energische* Lösung besteht darin, den **zusätzlichen mberührungs-Argumenten**Ihres Projekts `--optimization=force-rejected-types-removal` hinzuzufügen. Hierdurch werden Ablauf Verfolgungen `UIWebView` aus der Anwendung entfernt. Jeglicher Code, der auf den Typ verweist, funktioniert jedoch **nicht** ordnungsgemäß (Ausnahmen oder Abstürze werden erwartet). Diese Vorgehensweise sollte nur verwendet werden, wenn Sie sicher sind, dass der Code zur Laufzeit nicht erreichbar ist (auch wenn er über die statische Analyse erreichbar war).

#### <a name="support-for-ios-7x-or-earlier"></a>Unterstützung für IOS 7. x (oder früher)

`UIWebView` ist seit v 2.0 Teil von IOS. Die häufigsten Ersetzungen sind `WKWebView` (IOS 8) und `SFSafariViewController` (IOS 9). Wenn Ihre Anwendung weiterhin ältere IOS-Versionen unterstützt, sollten Sie die folgenden Optionen berücksichtigen:

* Legen Sie IOS 8 als minimale Zielversion (Buildzeit-Entscheidung) ab.
* Verwenden Sie nur `WKWebView`, wenn die APP unter IOS 8 + (eine Lauf Zeit Entscheidung) ausgeführt wird.

#### <a name="applications-not-submitted-to-apple"></a>Nicht an Apple gesendete Anwendungen

Wenn Ihre Anwendung nicht an Apple übermittelt wird, sollten Sie planen, von der veralteten API zu wechseln, da Sie in zukünftigen IOS-Releases entfernt werden kann. Allerdings können Sie diese Umstellung mithilfe Ihres eigenen Zeitplans durchführen.

## <a name="related-links"></a>Verwandte Links

- [Webviews (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/webview)
