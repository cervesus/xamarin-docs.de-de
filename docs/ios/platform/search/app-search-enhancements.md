---
title: Verbesserungen bei der App-Suche in Xamarin.iOS
description: Dieser Artikel behandelt die Verbesserungen Apple App-Suche auf iOS 10 und implementieren Sie diese in Xamarin.iOS gestellt hat.
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: e61cb20f12f6373ef57beb759b933d3dd9b6e76e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50110582"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Verbesserungen bei der App-Suche in Xamarin.iOS

_Dieser Artikel behandelt die Verbesserungen Apple App-Suche auf iOS 10 und implementieren Sie diese in Xamarin.iOS gestellt hat._

In iOS 10 hat Apple verschiedene Verbesserungen an App-Suche, z. B. per Crowdsourcing gesammelter Kommentare Deep-Linking, In-App zu suchen, Fortsetzung der Suche und Visualisierung von Überprüfungsergebnissen vorgenommen. Dieser Artikel behandelt diese Features in einer Xamarin.iOS-app implementieren.

## <a name="about-app-search-enhancements"></a>Informationen zu Verbesserungen bei der App-Suche

Core Spotlight IOS 10 bietet mehrere Erweiterungen wie z. B. zum App-Suche an:

- **Per Crowdsourcing gesammelter Kommentare Deep-Link Beliebtheit (mit differenziellen Privacy)** -bietet eine Möglichkeit zur Förderung der Deep-Link-app-Inhalte in den Suchergebnissen angezeigt.
- **In-App-Suche** -verwenden Sie die neue `CSSearchQuery` Klasse zu in-app-Spotlight-Suche Möglichkeit, die ähnlich wie die E-Mail, Nachrichten und Anmerkungen zu dieser apps arbeiten.
- **Suchen Sie die Fortsetzung** – ermöglicht Benutzern das Starten einer Suche im Blickpunkt oder Safari, und öffnen Sie eine app, und diese Suche fortsetzen.
- **Visualisierung von Überprüfungsergebnissen** -Apple [App-Suche-API-Überprüfungstool](https://search.developer.apple.com/appsearch-validation-tool) zeigt nun eine visuelle Darstellung von Markup und Deep linking einer Website aus, bei der Herstellung von Tests.
- **Nachrichten-App Teilen von Bildern** -beliebten in-app-Images bereitgestellt werden, für die Freigabe in Nachrichten (über eine Nachrichten-App-Erweiterung), bei der Spotlight-Suche angezeigt werden können.

In den folgenden Abschnitten werden diese Themen ausführlicher behandelt.

## <a name="crowdsourced-deep-link-popularity"></a>Per Crowdsourcing gesammelter Kommentare Deep-Link Beliebtheit

iOS 10 bietet einen Mechanismus zum zählen der Häufigkeit, beliebte Deep-Links in eine app von dem Benutzer gefolgt sind und verwendet diese Informationen, um die Rangfolge einer App verbessern den Inhalt in den Suchergebnissen, und gleichzeitig die Identität des Benutzers mit  *Differenzielle Datenschutz*.

Für die app, verwenden Sie die `NSUserActivity` Objekte Deep-Link-URLs und weisen die `EligibleForPublicIndexing` -Eigenschaftensatz auf `true`, iOS 10 übermittelt eine Teilmenge der *differenzielle Datenschutz Hashes* auf Apple Servern. Diese Informationen werden dann verwendet, Förderung von beliebten in-app-Inhalte in den Suchergebnissen angezeigt.

Weitere Informationen zur Implementierung von Deep-linking in einer Xamarin.iOS-app finden Sie unserem [Suche mit NSUserActivity](~/ios/platform/search/nsuseractivity.md) Dokumentation.

## <a name="in-app-searching"></a>Suchen in-App

Durch die Implementierung der neuen [CSSearchQuery](https://developer.apple.com/reference/corespotlight/cssearchquery) -Klasse, Spotlight Suche und entsprechende regeltechnologie Inhalte innerhalb von selbst, ohne dass der Benutzer die app (ähnlich wie die E-Mail, Nachrichten und Anmerkungen zu dieser app zu verlassen, kann eine app bereitstellen Arbeit).

In der Regel, die apps unterstützen `CSSearchQuery` müssen sich nicht um ihre eigenen, separaten Search-Index zu verwalten. 

## <a name="search-continuation"></a>Fortsetzung der Suche

In iOS 9 Apple führte den Such-APIs (z. B. Core Spotlight `NSUserActivity` und webmarkup) Bereitstellen von Deep-wünschen, des Inhalts in einer app, dass Benutzer für diesen Inhalt mithilfe der Spotlight und die Safari-Suche-Schnittstellen suchen können. Finden Sie unter unserem [-Suche-APIs neue](~/ios/platform/search/index.md) Dokumentation.

In iOS baut 10 Apple auf dieses Feature, indem der Benutzer zum Starten einer Suche im Blickpunkt oder Safari, und klicken Sie dann die Suche fortsetzen, wenn sie eine app öffnen. 

Um dieses Feature zu implementieren, bearbeiten Sie der app `Info.plist` hinzufügen. die `CoreSpotlightContinuation` vom Typ Key **booleschen** und setzen ihren Wert auf `YES`:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](app-search-enhancements-images/search01.png "Bearbeiten von CoreSpotlightContinuation in der Datei \"Info.plist\"")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](app-search-enhancements-images/searchw01.png "Bearbeiten von CoreSpotlightContinuation in der Datei \"Info.plist\"")](app-search-enhancements-images/search01.png#lightbox)

-----

Für den Benutzer, die Sie den Vorgang fortsetzen eines Suchergebnisses zu reagieren (`NSUserActivity`), bearbeiten die `AppDelegate.cs` Datei, und überschreiben die `ContinueUserActivity` Methode. Zum Beispiel:

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

Dieser Code sucht nach Typ der Abfrage Fortsetzung (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`), liest dann die aktuelle Abfrage des Benutzers aus der `NSUserActivity` -Klasse Informationen Benutzerwörterbuch (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). Hier muss die app Maßnahmen ergreifen, um die Fortsetzung des Suchvorgangs des Benutzers.

Weitere Informationen zum Arbeiten mit Suchvorgänge in einer Xamarin.iOS-app finden Sie unserem [Suche mit Core Spotlight](~/ios/platform/search/corespotlight.md) Dokumentation.

## <a name="visualization-of-validation-results"></a>Visualisierung der Ergebnisse

Apple [App-Suche-API-Überprüfungstool](https://search.developer.apple.com/appsearch-validation-tool) zeigt nun eine visuelle Darstellung von Markup und Deep linking Website (einschließlich Markup wie z. B. auf definiert [Schema.org](http://schema.org/)) bei der Herstellung von Tests.

Das Tool zur Clustervalidierung erkennen, ein Entwickler, dass die Information, dass der Webcrawler Applebot für die Website z. B. Titel, Beschreibung, URL und alle anderen indizierten Elemente unterstützt.

Weitere Informationen zum Arbeiten mit Markup im Web finden Sie unsere [suchen mit Webmarkup](~/ios/platform/search/web-markup.md) Dokumentation.

## <a name="message-app-image-sharing"></a>Das Image des Nachrichten-App freigeben

Wenn eine Nachrichten-App-Erweiterung Images für die Freigabe in Nachrichten bereitstellt, kann die Erweiterung für dem Benutzer ermöglichen, führen Sie die Spotlight-Suche nach beliebten Images aus, in den Nachrichten, ohne die app verlassen zu müssen konfiguriert werden.

Um dieses Feature zu aktivieren, führen Sie folgende Schritte aus:

1. Erstellen Sie eine Nachrichten-App-Erweiterung.
2. Hinzufügen der `com.apple.developer.associated-domains` der app-Berechtigungen und eine Liste der Webdomänen, die die Images hostet, die Nachrichten-App-Erweiterung ist die gemeinsame Verwendung, enthalten. Geben Sie für jede Domäne, die `spotlight-image-search` Service.
3. Hinzufügen einer `apple-app-site-association` Datei auf der Website, die die Images hostet. Diese Datei enthält ein Wörterbuch für die `spotlight-image-search` Dienst, und die app ID, die das Team-ID oder App-ID-Präfix, gefolgt von der Bundle-ID enthält Die Datei darf bis zu 500 Pfade und Muster, die von Spotlight indiziert und in gängigen bildersuchen eingeschlossen werden. Weitere Informationen finden Sie unter Apple [erstellen und Hochladen der Datei Zuordnung](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) Dokumentation.
4. Ermöglichen der Applebot, um die Websites zu durchforsten. Informieren Sie sich von Apple [zu Applebot](https://support.apple.com/HT204683) Dokumentation.

Finden Sie unter unseren [App Nachrichtenintegration](~/ios/platform/message-app-integration/index.md) Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt Apple App-Suche auf iOS 10 und implementieren Sie diese in Xamarin.iOS gestellt hat.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
