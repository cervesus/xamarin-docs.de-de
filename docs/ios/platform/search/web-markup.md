---
title: Suche mit Markup im Web in Xamarin.iOS
description: In diesem Dokument wird beschrieben, wie webbasierte Suchergebnisse zu erstellen, die mit einer Xamarin.iOS-app verknüpft wird. Es wird erläutert, wie Webinhalte indizieren, wodurch Ihrer app-Website mithilfe von smart-app-Banner, universelle Links und mehr auffindbar zu ermöglichen.
ms.prod: xamarin
ms.assetid: 876315BA-2EF9-4275-AE33-A3A494BBF7FD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: 4ee07e4b47ed9e1bdca0efc814ad44e513f68e80
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61076579"
---
# <a name="search-with-web-markup-in-xamarinios"></a>Suche mit Markup im Web in Xamarin.iOS

Für apps, die Zugriff auf ihre Inhalte über eine Website bereitstellen (nicht nur von innerhalb der app), Webinhalte kann sich mit speziellen Links, die von Apple wird gecrawlt und deep links, um Ihre app auf dem Gerät des Benutzers iOS 9 markiert werden.

Wenn Ihre iOS-app bereits mobile deep-Links unterstützt und Ihrer Website, deep-Links zu Inhalten innerhalb Ihrer app, Apple angezeigt _Applebot_ Webcrawler werden diese Inhalte zu indizieren und fügen Sie diese in ihren Cloud-Index:

[![](web-markup-images/webmarkup01.png "Übersicht über die Cloud-Index")](web-markup-images/webmarkup01.png#lightbox)

Apple wird diese Ergebnisse in Spotlight-Suche, und suchen Sie Safari-Ergebnissen auftreten.
Wenn Sie auf der Benutzer tippt auf eines der folgenden Ergebnisse (und sie Ihre app installiert haben) und dann sie auf den Inhalt in Ihrer app ausgeführt werden:

[![](web-markup-images/webmarkup02.png "Tiefe auf einer Website in den Suchergebnissen verknüpfen")](web-markup-images/webmarkup02.png#lightbox)

## <a name="enabling-web-content-indexing"></a>Web Content Indizierung aktivieren

Es gibt vier Schritte erforderlich, damit Sie die app-Inhalte durchsuchbar mithilfe von Markup im Web:

1. Sicherstellen, dass Apple ermitteln und index-Ihrer-app-Website durch die Definition als kann die **Unterstützung** oder **Marketing** -Website in iTunes Connect.
2. Stellen Sie sicher, dass die Website von Ihrer app das erforderliche Markup zum Implementieren von mobile deep-Links enthält. Finden Sie unter den folgenden Abschnitten Weitere Informationen.
3. Aktivieren Sie deep-Link in Ihre iOS-app verarbeiten.
4. Fügen Sie Markup für die strukturierten Daten, die von Ihrer app-Website zu einem Ergebnis funktionsreicher und ansprechender für den Endbenutzer verfügbar gemacht. Dieser Schritt ist, zwar nicht zwingend erforderlich wird es von Apple empfohlen.

In den folgenden Abschnitten werden diese Schritte im Detail besprochen.

## <a name="make-your-apps-website-discoverable"></a>Stellen Sie Ihrer App-Website ermittelt

Die einfachste Möglichkeit, haben Apple-Ihrer-app-Website zu finden ist die für die Verwendung als die **Unterstützung** oder **Marketing** Website, wenn Sie Ihre app an Apple über iTunes Connect übermitteln.

## <a name="using-smart-app-banners"></a>Mithilfe von Smart-App-Banner

Geben Sie ein Smart-App-Banner auf Ihrer Website einen Link löschen in Ihrer app zu präsentieren. Wenn die app nicht bereits installiert ist, fordert der Safari automatisch den Benutzer auf Ihre app zu installieren. Andernfalls tippen auf die Verwendung der **Ansicht** Link zum Starten der app auf der Website. Zum Erstellen einer intelligenten App-Bannerbenachrichtigung können Sie z. B. den folgenden Code verwenden:

```xml
<meta name="AppName" content="app-id=123456, app-argument=http://company.com/AppName">
```

Weitere Informationen finden Sie unter Apple [Höherstufen von Apps mit App-Banner Smart](https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html) Dokumentation.

## <a name="using-universal-links"></a>Verwenden universelle Links

Neue IOS 9, universelle Links geben Sie eine bessere Alternative als intelligente App-Banner oder vorhandene benutzerdefinierte URL-Schemas durch die Bereitstellung der Folgendes:

- **Eindeutige** – kann nicht die gleiche URL von mehr als eine Website in Anspruch genommen werden.
- **Sichere** – ein signiertes Zertifikat ist erforderlich, für die Website, die sicherstellt, die Website gehört und ordnungsgemäß verknüpft die Ihrer app.
- **Flexible** – Benutzer können steuern, ob die URL der Website oder die app gestartet wird.
- **Universelle** – die gleiche URL kann verwendet werden, um sowohl Ihrer Website und Ihrer app-Inhalte zu definieren.

## <a name="using-twitter-cards"></a>Verwendung von Twitter-Karten

Sie können deep-Links zu Ihrer app-Inhalte, die mithilfe einer Twitter-Karte angeben. Zum Beispiel:

```xml
<meta name="twitter:app:name:iphone" content="AppName">
<meta name="twitter:app:id:iphone" content="AppNameID">
<meta name="twitter:app:url:iphone" content="AppNameURL">
```

Weitere Informationen finden Sie unter der Twitter [Twitter-Karte Protokoll](http://dev.twitter.com/cards/mobile) Dokumentation.

## <a name="using-facebook-app-links"></a>Mithilfe von Facebook-App-Links

Sie können deep-Links zu den Inhalt Ihrer app mit einer Facebook-App-Link bereitstellen. Zum Beispiel:

```xml
<meta property="al:ios:app_name" content="AppName">
<meta property="al:ios:app_store_id" content="AppNameID">
<meta property="al:ios:url" content="AppNameURL">
```

Weitere Informationen finden Sie unter Facebook [App Links](http://applinks.org) Dokumentation.

## <a name="opening-deep-links"></a>Öffnen Deep-Links

Sie müssen zum Hinzufügen der Unterstützung für das Öffnen und Anzeigen von Deep-Links in Ihrer Xamarin.iOS-app. Bearbeiten der **Datei "appdelegate.cs"** Datei, und überschreiben die `OpenURL` Methode, um das benutzerdefinierte URL-Format zu verarbeiten. Zum Beispiel:

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

Im obigen Code suchen wir eine URL mit `/appname` und übergeben den Wert der `query` (`123` in diesem Beispiel), einen benutzerdefinierten ansichtencontroller in unserer app, um den angeforderten Inhalt an den Benutzer anzuzeigen.

## <a name="providing-rich-results-with-structured-data"></a>Bereitstellen von Rich-Ergebnisse mit strukturierten Daten

Durch Einschließen von strukturierten Daten Markup können Sie umfassende Suchergebnisse bereitstellen, für den Endbenutzer, die einfach einen Titel und Beschreibung hinausgehen. Enthalten Sie Bilder, die bestimmte app-Daten (etwa Bewertungen) und die Aktionen, die Ergebnisse mithilfe von Markup für strukturierte Daten.

Umfassende Ergebnisse sind viel ansprechender und verbessert die Rangfolge in der Cloud anhand einer verlockenden mehr Benutzer zur Interaktion Search-Index.

Eine Option für die Bereitstellung von strukturierten Daten Markup ist die Verwendung von Open Graph. Zum Beispiel:

```xml
<meta property="og:image" content="http://company.com/appname/icon.jpg">
<meta property="og:audio" content="http://company.com/appname/theme.m4a">
<meta property="og:video" content="http://company.com/appname/tutorial.mp4">
```

Weitere Informationen finden Sie unter den [Open Graph](http://ogp.me) Website.

Ein anderes gängiges Format für strukturierte Daten Markup ist die Schema.org-Mikrodaten-Format. Zum Beispiel:

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

Der folgende Code zeigt ein Beispiel Metadaten auf Ihrer Website, die umfassende Suchergebnisse für den Endbenutzer bereitstellen:

[![](web-markup-images/deeplink01.png "Umfassende Suchergebnisse über strukturierte Daten Markup")](web-markup-images/deeplink01.png#lightbox)

Apple unterstützt derzeit die folgenden Schematypen schema.org:

 - AggregateRating
 - ImageObject
 - InteractionCount
 - Angebote
 - Organisation
 - PriceRange
 - Anleitung
 - SearchAction

Weitere Informationen zu diesen Schemas finden Sie unter [schema.org](http://schema.org).

## <a name="providing-actions-with-structured-data"></a>Bereitstellen von Aktionen mit strukturierten Daten

Bestimmte Typen von strukturierten Daten lässt sich auf ein Suchergebnis vom Endbenutzer reagiert werden. Derzeit werden die folgenden Aktionen unterstützt:

 - Wählen eine Telefonnummer ein.
 - Abrufen von Map-Richtung, in einer bestimmten Adresse.
 - Wiedergeben einer Audio- oder video-Datei an.

Definieren eine Aktion, um eine Telefonnummer zu wählen könnte beispielsweise wie folgt aussehen:

```xml
<div itemscope itemtype="http://schema.org/Organization">
    <span itemprop="telephone">(408) 555-1212**


```

Wenn dieses Suchergebnis für den Endbenutzer angezeigt wird, wird ein kleines Telefonsymbol im Resultset angezeigt. Wenn der Benutzer das Symbol tippt, wird die angegebene Zahl aufgerufen werden.

Eine Aktion, um die Wiedergabe einer Audiodatei im Suchergebnis würde folgende HTML-Code hinzugefügt werden:

```xml
<div itemscope itemtype="http://schema.org/AudioObject">
    <span itemprop="contentUrl">http://company.com/appname/greeting.m4a**


```

Folgende HTML-Code würde schließlich eine Aktion zum Abrufen von Anweisungen aus den Suchergebnissen hinzugefügt werden:

```xml
<div itemscope itemtype="http://schema.org/PostalAddress">
    <span itemprop="streetAddress">1 Infinite Loop**
    <span itemprop="addressLocality">Cupertino**
    <span itemprop="addressRegion">CA**
    <span itemprop="postalCode">95014**


```

Weitere Informationen finden Sie unter Apple [App-Suche-Entwicklerwebsite](https://developer.apple.com/ios/search/).



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
