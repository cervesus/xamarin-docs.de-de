---
title: Suche mit webmarkup in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie webbasierte Suchergebnisse erstellt werden, die mit einer xamarin. IOS-App verknüpft sind. Darin wird erläutert, wie Sie die Indizierung von Webanwendungen aktivieren, damit die Website Ihrer APP auffindbar ist, und zwar mit intelligenten App-Banner, universellen Links und mehr.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/20/2017
ms.openlocfilehash: 87037c8c3797c7c305ce2689172bda1babbc26bd
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291719"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Suche mit webmarkup in xamarin. IOS

Für apps, die den Zugriff auf Ihre Inhalte über eine Website ermöglichen (nicht nur innerhalb der APP), können Webinhalte mit speziellen Links gekennzeichnet werden, die von Apple durchforstet werden, und Deep Linking Ihrer APP auf dem IOS 9-Gerät des Benutzers bereitstellen.

Wenn Ihre IOS-App bereits Mobile Deep Linking unterstützt und Ihre Website Deep-Links zu Inhalten in Ihrer APP präsentiert, indiziert Apple _applebot_ Web Crawler diesen Inhalt und fügt ihn automatisch dem cloudindex hinzu:

[![](web-markup-images/webmarkup01.png "Übersicht über den cloudindex")](web-markup-images/webmarkup01.png#lightbox)

Apple zeigt diese Ergebnisse in Spotlight-Suche und Safari-Suchergebnissen an.
Wenn der Benutzer auf eines dieser Ergebnisse tippt (und die APP installiert ist), werden Sie an den Inhalt in Ihrer APP weitergeleitet:

[![](web-markup-images/webmarkup02.png "Deep Linking von einer Website in den Suchergebnissen")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Aktivieren der Webinhalts Indizierung

Es gibt vier Schritte, die erforderlich sind, um den Inhalt Ihrer App mithilfe von webmarkup durchsuchbar zu machen:

1. Stellen Sie sicher, dass Apple die Website Ihrer APP ermitteln und indizieren kann, indem Sie Sie als **Support** -oder **Marketing** Website in iTunes Connect definieren.
2. Stellen Sie sicher, dass die Website Ihrer APP das erforderliche Markup zum Implementieren mobiler Deep Linking enthält. Weitere Informationen finden Sie in den folgenden Abschnitten.
3. Aktivieren Sie die Verarbeitung von Deep-Links in ihrer IOS-app.
4. Fügen Sie für die strukturierten Daten, die von der Website Ihrer APP bereitgestellt werden, Markup hinzu, um dem Endbenutzer ein umfassendes und ansprechendes Ergebnis zu bieten. Obwohl dieser Schritt nicht zwingend erforderlich ist, wird er von Apple dringend empfohlen.

In den folgenden Abschnitten werden diese Schritte ausführlich erläutert.

## <a name="make-your-apps-website-discoverable"></a>Auffallen, dass die Website der APP erkennbar ist

Die einfachste Möglichkeit, Apple die Website Ihrer APP zu finden, ist die Verwendung als **Support** -oder **Marketing** Website, wenn Sie Ihre APP über iTunes Connect an Apple übermitteln.

## <a name="using-smart-app-banners"></a>Verwenden von intelligenten App-Banner

Stellen Sie ein intelligentes App-Banner auf Ihrer Website bereit, um einen klaren Link zu Ihrer APP zu präsentieren. Wenn die APP nicht bereits installiert ist, wird der Benutzer von Safari automatisch aufgefordert, Ihre APP zu installieren. Andernfalls kann die Verwendung auf den Link " **Ansicht** " tippen, um Ihre APP von der Website zu starten. Wenn Sie z. b. ein SmartApp-Banner erstellen möchten, können Sie den folgenden Code verwenden:

```html
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Weitere Informationen finden Sie in der Dokumentation zu den Apple [promoten apps mit intelligenten apps](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) .

## <a name="using-universal-links"></a>Verwenden universeller Verknüpfungen

Universelle Verknüpfungen für IOS 9 bieten eine bessere Alternative zu intelligenten App-transparenten oder vorhandenen benutzerdefinierten URL-Schemas, indem Sie Folgendes bereitstellen:

- **Eindeutig** – dieselbe URL kann nicht von mehr als einer Website beansprucht werden.
- **Sicher** – ein signiertes Zertifikat ist für die Website erforderlich, mit der sichergestellt wird, dass sich die Website im Besitz von Ihnen befindet und mit Ihrer APP ordnungsgemäß verknüpft ist.
- **Flexibel** – der Endbenutzer kann steuern, ob die URL die Website oder die app starten soll.
- **Universal** – die gleiche URL kann verwendet werden, um sowohl den Inhalt Ihrer Website als auch den Inhalt Ihrer APP zu definieren.

## <a name="using-twitter-cards"></a>Verwenden von Twitter-Karten

Sie können mithilfe einer Twitter-Karte Deep Links zum Inhalt Ihrer APP bereitstellen. Beispiel:

```html
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Weitere Informationen finden Sie in der Twitter-Dokumentation zum [Twitter-Karten Protokoll](http://dev.twitter.com/cards/mobile) von Twitter.

## <a name="using-facebook-app-links"></a>Verwenden von Facebook-App-Links

Sie können mit einem Facebook-App-Link Deep Links zum Inhalt Ihrer APP bereitstellen. Beispiel:

```html
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Weitere Informationen finden Sie in der Dokumentation der Facebook- [App-Links](http://applinks.org) .

## <a name="opening-deep-links"></a>Öffnen von Deep-Links

Sie müssen Unterstützung für das Öffnen und Anzeigen von Deep-Links in ihrer xamarin. IOS-app hinzufügen. Bearbeiten Sie die Datei **AppDelegate.cs** , und `OpenURL` überschreiben Sie die Methode, um das benutzerdefinierte URL-Format zu verarbeiten Beispiel:

```csharp
public override bool OpenUrl (UIApplication application, NSUrl url, string sourceApplication, NSObject annotation)
{

  // Handling a URL in the form http://company.com/appname/?123
  try {
    var components = new NSUrlComponents(url,true);
    var path = components.Path;
    var query = components.Query;

    // Is this a known format?
    if (path == "/appname") {
      // Display the view controller for the content
      // specified in query (123)
      return ContentViewController.LoadContent(query);
    }
  } catch {
    // Ignore issue for now
  }

  return false;
}
```

Im obigen Code suchen wir nach einer URL, die enthält `/appname` und den Wert von `query` (`123` in diesem Beispiel) an einen benutzerdefinierten Ansichts Controller in unserer App übergibt, um dem Benutzer den angeforderten Inhalt anzuzeigen.

## <a name="providing-rich-results-with-structured-data"></a>Bereitstellen umfangreicher Ergebnisse mit strukturierten Daten

Durch Einschließen von strukturiertem Daten Markup können Sie den Endbenutzern umfassende Suchergebnisse bereitstellen, die über einen Titel und eine Beschreibung hinausgehen. Einschließen von Bildern, App-spezifischen Daten (z. b. Bewertungen) und Aktionen, die mit strukturiertem Daten Markup erzielt werden.

Umfassende Ergebnisse sind ansprechender und können dazu beitragen, ihre Rangfolge im cloudbasierten Suchindex zu verbessern, indem mehr Benutzer zur Interaktion mit Ihnen verleitet werden.

Eine Möglichkeit zum Bereitstellen von strukturiertem Daten Markup ist die Verwendung von Open Graph. Beispiel:

```html
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Weitere Informationen finden Sie auf der [Open Graph](http://ogp.me) -Website.

Ein weiteres gängiges Format für strukturiertes Daten Markup ist das Format von Schema. org im Mikrodaten Format. Beispiel:

```html
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
  <span itemprop="ratingValue">4** stars -
  <span itemprop="reviewCount">255** reviews
```

Die gleichen Informationen können im JSON-LD-Format von Schema. org dargestellt werden:

```html
<script type="application/ld+json">
  "@content":"http://schema.org",
  "@type":"AggregateRating",
  "ratingValue":"4",
  "reviewCount":"255"
</script>
```

Im folgenden finden Sie ein Beispiel für Metadaten von Ihrer Website, die dem Endbenutzer umfangreiche Suchergebnisse bieten:

[![](web-markup-images/deeplink01.png "Umfassende Suchergebnisse über strukturiertes Daten Markup")](web-markup-images/deeplink01.png#lightbox)

Apple unterstützt derzeit die folgenden Schema Typen von Schema.org:

- AggregateRating
- Imageobject
- Interaktioncount
- Angebote
- Organization
- Pricerange
- Tions
- SearchAction

Weitere Informationen zu diesen Schema Typen finden Sie unter [Schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>Bereitstellen von Aktionen mit strukturierten Daten

Bestimmte Typen strukturierter Daten ermöglichen, dass ein Suchergebnis vom Endbenutzer verwendet werden kann. Derzeit werden die folgenden Aktionen unterstützt:

- Eine Telefonnummer wird unterschieden.
- Zuordnen von Zuordnungs Richtung zu einer bestimmten Adresse.
- Wiedergabe einer Audiodatei oder Videodatei.

Beispielsweise könnte die Definition einer Aktion, mit der eine Telefonnummer gewählt werden soll, wie folgt aussehen:

```html
<div itemscope itemtype="http://schema.org/Organization">
  <span itemprop="telephone">(408) 555-1212**
```

Wenn das Suchergebnis für den Endbenutzer angezeigt wird, wird im Ergebnis ein kleines Telefon Symbol angezeigt. Wenn der Benutzer auf das Symbol tippt, wird die angegebene Zahl aufgerufen.

Der folgende HTML-Code würde eine Aktion hinzufügen, um eine Audiodatei aus dem Suchergebnis wiederzugeben:

```html
<div itemscope itemtype="http://schema.org/AudioObject">
  <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**
```

Schließlich würde der folgende HTML-Code eine Aktion hinzufügen, um Anweisungen aus dem Suchergebnis zu erhalten:

```html
<div itemscope itemtype="http://schema.org/PostalAddress">
  <span itemprop="streetAddress">1 Infinite Loop**
  <span itemprop="addressLocality">Cupertino**
  <span itemprop="addressRegion">CA**
  <span itemprop="postalCode">95014**
```

Weitere Informationen finden Sie auf der [App Search-Entwickler Website](https://developer.apple.com/ios/search/)von Apple.



## <a name="related-links"></a>Verwandte Links

- [IOS 9-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS9)
- [IOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmier Handbuch für die APP-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
