---
title: Suchen Sie App-Erweiterungen
description: Dieser Artikel behandelt die Erweiterungen Apple machte beim Suchen der App im iOS 10 und wie diese in Xamarin.iOS implementiert.
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 0df51429ea9655b0a72d9f4c1e413fa7e37410ac
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/26/2018
---
# <a name="app-search-enhancements"></a>Suchen Sie App-Erweiterungen

_Dieser Artikel behandelt die Erweiterungen Apple machte beim Suchen der App im iOS 10 und wie diese in Xamarin.iOS implementiert._

In iOS 10 machte Apple App suchen, z. B. Crowdsourced Deep verknüpfen, In der App suchen, Suche Fortsetzung Visualisierung Überprüfungsergebnisse verschiedene Verbesserungen. In diesem Artikel befasst sich diese Funktionen in einer app Xamarin.iOS implementieren.

## <a name="about-app-search-enhancements"></a>Zu den Verbesserungen der App suchen

Core Spotlight in iOS 10 bietet mehrere Erweiterungen wie z. B. App suchen:

- **Crowdsourced Deep-Link Beliebtheit (mit differenzielle Datenschutz)** -bietet eine Möglichkeit, die app mit Deep-Link-Inhalt in den Suchergebnissen angezeigter höher stufen.
- **Suchen in der App** -verwenden Sie die neue `CSSearchQuery` -Klasse, in der app Spotlight-Suche Möglichkeit ähnlich wie die e-Mail-, Nachrichten und Anmerkungen zu dieser apps arbeiten bereitzustellen.
- **Suchen Sie die Fortsetzung** – ermöglicht Benutzern das Starten einer Suche im Spotlight oder Safari, und öffnen Sie eine app und die Suche zu fortfahren.
- **Visualisierung von Überprüfungsergebnissen** -Apple [App Search-API-Überprüfungstools](https://search.developer.apple.com/appsearch-validation-tool) zeigt jetzt eine visuelle Darstellung von Markup und umfassende Verknüpfen einer Website aus, wenn Tests preforming.
- **Freigabe-App Image Message** -können Sie gängige in app-Images, die bereitgestellt werden, für die Freigabe in Nachrichten (über eine Nachrichten-App-Erweiterung) in der Spotlight-Suche angezeigt werden sollen.

In den folgenden Abschnitten werden die folgenden Themen ausführlicher behandelt.

## <a name="crowdsourced-deep-link-popularity"></a>Beliebtheit Crowdsourced Deep-Link

iOS 10 bietet einen Mechanismus zum zählen der Häufigkeit, beliebte Deep-Links in eine app vom Benutzer gefolgt sind, und verwendet diese Informationen, um die Rangfolge einer App zu verbessern den Inhalt in den Suchergebnissen, und schützen gleichzeitig weiterhin die Identität des Benutzers mit  *Differenzielle Datenschutz*.

Für die app, verwenden Sie die `NSUserActivity` Objekte sind Deep-Link-URLs und sein der `EligibleForPublicIndexing` -Eigenschaftensatz auf `true`, iOS 10 übermittelt eine Teilmenge der *differenzielle Datenschutz Hashes* auf Apple Server. Diese Informationen werden dann verwendet, beliebten in-app-Inhalte in den Suchergebnissen angezeigter höher stufen.

Weitere Informationen zum Implementieren von Deep Verknüpfungen in einer app Xamarin.iOS finden Sie unter unsere [Suche mit NSUserActivity](~/ios/platform/search/nsuseractivity.md) Dokumentation.

## <a name="in-app-searching"></a>Suchen in der App

Durch die Implementierung der neuen [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) Klasse, Spotlight Suche und übereinstimmende regeltechnologie zu Inhalt in sich selbst, ohne dass der Benutzer die app (ähnlich wie die e-Mail-, Nachrichten und Anmerkungen zu dieser app verlassen, kann eine app bereitstellen Arbeit).

In der Regel, die apps unterstützen `CSSearchQuery` müssen nicht ihre eigenen, separaten Suchindex zu verwalten. 

## <a name="search-continuation"></a>Fortsetzung der Suche

Apple iOS 9 eingeführt Such-APIs (z. B. Core Spotlight `NSUserActivity` und Web Markup) Deep-Stil von Inhalten innerhalb einer app, um Benutzern das Durchsuchen dieses Inhalts mithilfe der sowohl die Safari der Spotlight-Suche-Schnittstellen ermöglichen bereitstellen. Finden Sie in unserer [neue Such-APIs](~/ios/platform/search/index.md) Dokumentation.

In iOS baut 10 Apple auf dieses Feature, indem der Benutzer zum Starten einer Suche im Spotlight oder Safari, und klicken Sie dann die Suche fortsetzen, wenn sie eine app geöffnet. 

Um dieses Feature zu implementieren, bearbeiten Sie der app `Info.plist` hinzufügen. die `CoreSpotlightContinuation` vom Typ Key **booleschen** und legen Sie dessen Wert auf `YES`:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![](app-search-enhancements-images/search01.png "Bearbeiten von CoreSpotlightContinuation in der Datei "Info.plist"")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](app-search-enhancements-images/searchw01.png "Bearbeiten von CoreSpotlightContinuation in der Datei "Info.plist"")](app-search-enhancements-images/search01.png#lightbox)

-----

So reagieren Sie auf der Benutzer ein Suchergebnis zu fortfahren (`NSUserActivity`), bearbeiten Sie die `AppDelegate.cs` Datei, und überschreiben die `ContinueUserActivity` Methode. Beispiel:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    default:
        if (userActivity.ActivityType == CSSearchQuery.ContinuationActionType) {
            var search = userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString);
            // Continue user's search here...
        }
        break;
    }

    return true;
}
```

Dieser Code sucht den Abfrage-Fortsetzung-Aktionstyp (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), liest dann die aktuelle Abfrage des Benutzers aus der `NSUserActivity` Klasse Info Benutzerwörterbuch (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). Hier muss die app ergreifen, um die Fortsetzung des Suchvorgangs des Benutzers.

Weitere Informationen zum Arbeiten mit Suchvorgänge in einem Xamarin.iOS-app finden Sie unter unsere [Suche mit Core Spotlight](~/ios/platform/search/corespotlight.md) Dokumentation.

## <a name="visualization-of-validation-results"></a>Visualisierung von Überprüfungsergebnissen

Apple [App Search-API-Überprüfungstools](https://search.developer.apple.com/appsearch-validation-tool) zeigt jetzt eine visuelle Darstellung von Markup und umfassende Verknüpfen einer Website (einschließlich Markup wie z. B. auf definiert [Schema.org](http://schema.org/)) bei Tests preforming.

Mit dem Tool zur Clustervalidierung kann ein Entwickler finden Sie, dass die Information, dass der Webcrawler Applebot, für die Website z. B. Titel, Beschreibung, URL und alle anderen indiziert ist Elemente unterstützt.

Weitere Informationen zum Arbeiten mit Web Markup finden Sie unter unsere [suchen mit Web Markup](~/ios/platform/search/web-markup.md) Dokumentation.

## <a name="message-app-image-sharing"></a>Message-Freigabe-App-Images

Wenn eine Nachrichten-App-Erweiterung Bilder für die Freigabe in Nachrichten bereitstellt, kann die Erweiterung der Spotlight-Suchvorgänge für gängige Bilder aus in den Nachrichten ausführen, ohne die app verlassen zu müssen Benutzer kann konfiguriert werden.

Führen Sie folgende Schritte aus, um diese Funktion zu aktivieren:

1. Erstellen Sie eine Nachrichten-App-Erweiterung.
2. Hinzufügen der `com.apple.developer.associated-domains` der app-Berechtigungen und eine Liste der Webdomänen, die die Bilder zu hosten, die Nachrichten-App-Erweiterung ist die gemeinsame Verwendung, enthalten. Geben Sie für jede Domäne der `spotlight-image-search` Dienst.
3. Hinzufügen einer `apple-app-site-association` Datei auf der Website, die die Bilder hostet. Diese Datei enthält ein Wörterbuch für die `spotlight-image-search` service und enthält die ID der app, die das Team oder App-ID-Präfix, gefolgt von die Paket-ID ist Die Datei darf bis zu 500 Pfade und Muster, die von Spotlight indiziert und in gängigen Image Suchvorgänge eingeschlossen werden. Weitere Informationen finden Sie in der Apple- [erstellen und Hochladen der Datei Zuordnung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) Dokumentation.
4. Zulassen der Applebot Websites durchsucht. Finden Sie in der Apple- [zu Applebot](https://support.apple.com/HT204683) Dokumentation.

Finden Sie unter unsere [App Nachrichtenintegration](~/ios/platform/message-app-integration/index.md) Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt Apple machte beim Suchen der App im iOS 10 und wie diese in Xamarin.iOS implementiert.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
