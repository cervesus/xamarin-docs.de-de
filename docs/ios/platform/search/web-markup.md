---
title: Suche mit Web-Markup
description: Hinzufügen von webbasierten Suchergebnisse, die zurück an Ihre app verknüpfen können.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/20/2017
ms.openlocfilehash: bc3446419ef0e469f7184d60fe8876cd2e5da520
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="search-with-web-markup"></a>Suche mit Web-Markup

Für apps, die Zugriff auf ihre Inhalte über eine Website bereitstellen (nicht nur von innerhalb der app), Webinhalte kann mit speziellen Links, die von Apple gecrawlt werden wird, und geben Sie deep links, um Ihre app für iOS 9-Gerät des Benutzers gekennzeichnet werden.

Wenn Ihre iOS-app bereits mobile deep Links unterstützt und Ihre Website deep Links zu Inhalten innerhalb Ihrer app, Apple präsentiert _Applebot_ Webcrawler indizieren dieser Inhalt und automatisch ihre Cloud-Index hinzugefügt werden:

[![](web-markup-images/webmarkup01.png "Übersicht über die Cloud-Index")](web-markup-images/webmarkup01.png#lightbox)

Apple werden die Ergebnisse im Spotlight-Suche und Safari Suchergebnissen Oberfläche.
Wenn auf der Benutzer tippt führt eine der folgenden (und die app installiert haben) und dann auf den Inhalt in der app ausgeführt werden:

[![](web-markup-images/webmarkup02.png "Verknüpfen von einer Website in den Suchergebnissen angezeigter Deep")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Aktivieren die Inhaltsindizierung Web

Es gibt vier Schritte erforderlich, damit Sie den Inhalt app durchsuchbar mit Web-Markup:

1. Sicherstellen, dass Apple ermittelt und indizieren die app-Website, indem Sie definieren ihn als das **Unterstützung** oder **Marketing** Website in iTunes Connect.
2. Stellen Sie sicher, dass Ihre app-Website die erforderlichen Markup zum Implementieren der mobilen deep Links enthält. Finden Sie unter den folgenden Abschnitten Weitere Informationen.
3. Deep-Link Behandlung in der iOS-app zu aktivieren.
4. Fügen Sie Markup für den strukturierten Daten, die Diagnoseinformationen werden von der app-Website, um eine umfassende und einbeziehen Ergebnis für den Endbenutzer bereitzustellen. Während dieser Schritt nicht zwingend erforderlich ist, wird es von Apple empfohlen.

In den folgenden Abschnitten werden diese Schritte im Detail besprochen.

## <a name="make-your-apps-website-discoverable"></a>Ihre App-Website erkennbar machen

Die einfachste Möglichkeit zum Apple Ihrer app-Website gefunden haben, ist für die Verwendung entweder als die **Unterstützung** oder **Marketing** Website, wenn Sie die app an Apple über iTunes Connect senden.

## <a name="using-smart-app-banners"></a>Mithilfe der intelligenten App-Banner

Geben Sie einen intelligenten App-Banner auf Ihrer Website in deutlichem in Ihre app darstellen. Wenn die app nicht bereits installiert ist, fordert Safari automatisch den Benutzer die app zu installieren. Andernfalls kann die Verwendung zunutze der **Ansicht** Link, um Ihre app von der Website zu starten. Zum Erstellen einer Smart-App-Banner können Sie beispielsweise den folgenden Code verwenden:

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Weitere Informationen finden Sie in der Apple- [Heraufstufen von Apps mit Smart-App-Banner](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) Dokumentation.

## <a name="using-universal-links"></a>Mithilfe von universellen Links

Neu für iOS 9, universelle Links erhalten Sie eine bessere Alternative Smart-App-Banner oder vorhandene benutzerdefinierte URL-Schemas durch Folgendes bereitstellen:

- **Eindeutige** – die gleiche URL kann nicht von mehr als eine Website angefordert werden.
- **Sichere** – ein signiertes Zertifikat ist erforderlich, für die Website, die wird sichergestellt, die Website gehört dass, von Ihnen und einen gültigen Wert verknüpft die Ihrer app.
- **Flexible** – die Endbenutzer können steuern, ob die URL der Website oder die app gestartet wird.
- **Universelle** – dieselbe URL kann verwendet werden, um Inhalte für Ihre Website und Ihre app definieren.

## <a name="using-twitter-cards"></a>Verwendung von Twitter-Karten

Sie können deep Links zu den Inhalt Ihrer app mit einer Karte Twitter bereitstellen. Zum Beispiel:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Weitere Informationen finden Sie unter der Twitter [Twitter-Karte Protokoll](http://dev.twitter.com/cards/mobile) Dokumentation.

## <a name="using-facebook-app-links"></a>Mithilfe von Facebook-App-Links

Sie können den Inhalt Ihrer app mit einer Facebook-App-Link deep Links bereitstellen. Zum Beispiel:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Weitere Informationen finden Sie unter der Facebook- [App Links](http://applinks.org) Dokumentation.

## <a name="opening-deep-links"></a>Öffnen Deep-Links

Sie müssen die Unterstützung für das Öffnen und Anzeigen von Deep-Links in der app Xamarin.iOS hinzuzufügen. Bearbeiten der **AppDelegate.cs** Datei, und überschreiben die `OpenURL` Methode, um die benutzerdefinierte URL-Format zu behandeln. Zum Beispiel:

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

Im obigen Code suchen wir eine URL mit `/appname` und übergeben den Wert der `query` (`123` in diesem Beispiel) eine benutzerdefinierte Ansicht Controller in dieser app auf den angeforderten Inhalt für den Benutzer anzuzeigen.

## <a name="providing-rich-results-with-structured-data"></a>Bereitstellen von Rich-Ergebnisse mit strukturierten Daten

Durch einschließlich Markup für strukturierte Daten können Sie umfangreiche Suchergebnisse bereitstellen, für den Endbenutzer, die einfach einen Titel und Beschreibung hinausgehen. Schließen Sie Bilder, die bestimmte app-Daten (etwa Bewertungen) und die Aktionen an, die Ergebnisse mithilfe von Markup für strukturierte Daten ein

Rich-Ergebnisse sind viel ansprechender und verbessert die Rangfolge in der Cloud basierte Suchindex verleiten weitere Benutzer mit ihnen interagieren.

Eine Möglichkeit zum Bereitstellen von strukturierten Daten Markup ist mithilfe der Open Graph. Zum Beispiel:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Weitere Informationen finden Sie unter der [Open Graph](http://ogp.me) Website.

Ein anderes gängiges Format für strukturierte Daten Markup ist Schema.org Mikrodaten Format. Zum Beispiel:

```xml
<div itemprop="aggregateRating" itemscope itemtype="http://schema.org/AggregateRating">
    <span itemprop="ratingValue">4** stars -
    <span itemprop="reviewCount">255** reviews


```

Die gleiche Informationen kann im Schema.org des JSON-LD-Format dargestellt werden:

```xml
<script type="application/ld+json">
    "@content":"http://schema.org",
    "@type":"AggregateRating",
    "ratingValue":"4",
    "reviewCount":"255"
</script>
```

Der folgende Code zeigt ein Beispiel Metadaten aus Ihrer Website, die umfangreiche Suchergebnisse für den Endbenutzer bereitstellen:

[![](web-markup-images/deeplink01.png "Rich-Suchergebnissen über strukturierte Daten Markup")](web-markup-images/deeplink01.png#lightbox)

Apple unterstützt derzeit die folgenden Schematypen von schema.org:

 - AggregateRating
 - ImageObject
 - InteractionCount
 - Angebote
 - Organisation
 - PriceRange
 - Anleitung
 - SearchAction

Weitere Informationen zu diesen Typen Schema finden Sie unter [schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>Bereitstellen von Aktionen mit strukturierten Daten

Bestimmte Typen von strukturierten Daten können ein Suchergebnis aussagefähige vom Endbenutzer sein. Derzeit werden die folgenden Aktionen unterstützt:

 - Wählen eine Telefonnummer ein.
 - Erste Zuordnung Richtung, in einer angegebenen Adresse an.
 - Wiedergeben einer audio oder video-Datei an.

Definieren eine Aktion aus, um eine Telefonnummer könnte beispielsweise wie folgt aussehen:

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

Wenn dieses Suchergebnis für den Endbenutzer dargestellt wird, wird ein kleines Phone-Symbol im Resultset angezeigt. Wenn der Benutzer das Symbol "tippt, wird die angegebene Anzahl aufgerufen werden.

Eine Aktion zum Wiedergeben einer Audiodatei im Suchergebnis würde der folgenden HTML-Code hinzugefügt werden:

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

Abschließend würden die folgende HTML-Code eine Aktion zum Richtungen entnommen werden. das Suchergebnis hinzufügen:

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

Weitere Informationen finden Sie in der Apple- [App suchen-Entwicklerwebsite](http://developer.apple.com/ios/search/).



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
