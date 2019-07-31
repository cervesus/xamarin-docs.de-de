---
title: Erweiterungen der APP-Suche in xamarin. IOS
description: In diesem Artikel werden die Verbesserungen erläutert, die Apple bei der APP-Suche in ios 10 vorgenommen hat und wie Sie in xamarin. IOS implementiert werden.
ms.prod: xamarin
ms.assetid: 30124DB6-6A02-4F66-A2D9-BBC8008E6B48
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: 27eff717fd1390f54a177cc7636e7d107b69cd24
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656279"
---
# <a name="app-search-enhancements-in-xamarinios"></a>Erweiterungen der APP-Suche in xamarin. IOS

_In diesem Artikel werden die Verbesserungen erläutert, die Apple bei der APP-Suche in ios 10 vorgenommen hat und wie Sie in xamarin. IOS implementiert werden._

In ios 10 hat Apple eine Reihe von Verbesserungen an der APP-Suche vorgenommen, z. b. durch das Übermitteln von übergreifenden Verknüpfungen, das Suchen von Such Vorgängen und das Visualisieren von Validierungs Ergebnissen In diesem Artikel wird die Implementierung dieser Features in einer xamarin. IOS-App behandelt.

## <a name="about-app-search-enhancements"></a>Informationen zu Erweiterungen der APP-Suche

Kernspotlight in ios 10 bietet verschiedene Verbesserungen bei der APP-Suche, wie z.b.:

- **Crowdsource Deep-Link-Beliebtheit (mit differenziellen Datenschutz)** : bietet eine Möglichkeit zum herauf Stufen von Deep-verknüpften App-Inhalten in den Suchergebnissen.
- **In-App-Suche** : Verwenden Sie `CSSearchQuery` die neue-Klasse, um in-App-Spotlight-Suchfunktionen wie die Funktionsweise der Mail-, Nachrichten-und Notes-apps bereitzustellen
- **Such Fortsetzung** : ermöglicht einem Benutzer das Starten einer Suche in Spotlight oder Safari, das Öffnen einer APP und das Fortsetzen der Suche.
- **Visualisierung der Überprüfungs Ergebnisse** : das [Validierungs Tool der App Search-API](https://search.developer.apple.com/appsearch-validation-tool) von Apple zeigt nun eine visuelle Darstellung des Markups einer Website und die Deep-Verknüpfung bei der Vorbildung von Tests an.
- **Nachrichten-APP-Image Freigabe** : ermöglicht beliebte in-App-Images, die für die Freigabe in Nachrichten (über eine Nachrichten-APP-Erweiterung) bereitgestellt werden, um Sie in Spotlight

In den folgenden Abschnitten werden diese Themen ausführlicher behandelt.

## <a name="crowdsourced-deep-link-popularity"></a>Crowdsource-Beliebtheit für Deep-Link

IOS 10 bietet einen Mechanismus zum zählen der Häufigkeit, mit der beliebte Deep-Links zu einer APP vom Benutzer befolgt werden. diese Informationen werden verwendet, um die Rangfolge der Inhalte einer APP in den Suchergebnissen zu verbessern, während die Identität des Benutzers weiterhin mithilfe von differenziell geschützt wird.  *Datenschutz*.

Für apps, die- `NSUserActivity` Objekte verwenden, um Deep-Link-URLs bereit `EligibleForPublicIndexing` zustellen und die `true`-Eigenschaft auf festgelegt ist, übermittelt IOS 10 eine Teilmenge differenzieller *Datenschutz* Hashs an die Server von Apple. Diese Informationen werden dann verwendet, um beliebte in-App-Inhalte in den Suchergebnissen zu bewerben.

Weitere Informationen zum Implementieren von Deep-Linking in einer xamarin. IOS-App finden Sie in der Dokumentation zur [Suche mit nsuseractivity](~/ios/platform/search/nsuseractivity.md) .

## <a name="in-app-searching"></a>In-App-Suche

Durch Implementieren der neuen [cssearchquery](https://developer.apple.com/reference/corespotlight/cssearchquery) -Klasse kann eine APP Such-und abgleichsregeln für Spotlight bereitstellen, um Inhalte in sich selbst zu finden, ohne dass der Benutzer die app verlassen muss (ähnlich wie die Mail-, Nachrichten-und Notizen-App funktionieren).

Apps, die unterstützen `CSSearchQuery` , müssen in der Regel keinen eigenen, separaten Suchindex aufbewahren. 

## <a name="search-continuation"></a>Fortsetzung durchsuchen

In ios 9 wurden die Such-APIs (z. b. kernspotlight `NSUserActivity` und webmarkup) von Apple eingeführt, um den Inhalt in einer APP ausführlich zu nutzen, damit Benutzer mit den Such Schnittstellen Spotlight und Safari nach diesen Inhalten suchen können. Weitere Informationen finden Sie in der Dokumentation zu [neuen Such-APIs](~/ios/platform/search/index.md) .

In ios 10 baut Apple auf diesem Feature auf, indem es dem Benutzer ermöglicht, eine Suche in Spotlight oder Safari zu starten, und dann die Suche fortzusetzen, wenn eine APP geöffnet wird. 

Um dieses Feature zu implementieren, bearbeiten Sie die `Info.plist` Datei der APP, `CoreSpotlightContinuation` fügen Sie den Schlüssel des Typs " **Boolean** " `YES`hinzu, und legen Sie Ihren Wert auf fest:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![](app-search-enhancements-images/search01.png "Bearbeiten von corespotlightfortsetzung in der Datei \"Info. plist\"")](app-search-enhancements-images/search01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](app-search-enhancements-images/searchw01.png "Bearbeiten von corespotlightfortsetzung in der Datei \"Info. plist\"")](app-search-enhancements-images/search01.png#lightbox)

-----

Bearbeiten Sie die`NSUserActivity` `ContinueUserActivity` Datei, und überschreiben Sie die-Methode, um darauf zu reagieren, dass der Benutzer das Suchergebnis fortsetzt (). `AppDelegate.cs` Beispiel:

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

Dieser Code sucht nach dem Aktionstyp der Abfrage Fortsetzung (`userActivity.ActivityType == CSSearchQuery.ContinuationActionType`) und liest dann die aktuelle Abfrage des Benutzers aus dem Benutzer Informations Wörterbuch der `NSUserActivity` Klasse (`userActivity.UserInfo.KeyForValue(CSSearchQuery.QueryString)`). Von hier aus muss die app Maßnahmen ergreifen, um die Suche nach dem Benutzer fortzusetzen.

Weitere Informationen zum Arbeiten mit Such Vorgängen in einer xamarin. IOS-App finden Sie in der Dokumentation zur [Suche mit Core Spotlight](~/ios/platform/search/corespotlight.md) .

## <a name="visualization-of-validation-results"></a>Visualisierung von Validierungs Ergebnissen

Das [Validierungs Tool der App Search-API](https://search.developer.apple.com/appsearch-validation-tool) von Apple zeigt nun eine visuelle Darstellung des Markups einer Website an (einschließlich Markup, wie unter [Schema.org](http://schema.org/)definiert), wenn Tests vorgeformt werden.

Mit dem Validierungs Tool kann ein Entwickler die Informationen sehen, die der applebot-Webcrawler für die Website indiziert hat, z. b. Titel, Beschreibung, URL und alle anderen unterstützten Elemente.

Weitere Informationen zum Arbeiten mit webmarkup finden Sie in unserer Dokumentation zum Thema "" [mit webmarkup](~/ios/platform/search/web-markup.md) .

## <a name="message-app-image-sharing"></a>Nachrichten-APP-Image Freigabe

Wenn eine Nachrichten-APP-Erweiterung Bilder für die Freigabe in Nachrichten bereitstellt, kann die Erweiterung so konfiguriert werden, dass der Benutzer eine Spotlight-Suche nach gängigen Images aus den Nachrichten durchführen kann, ohne die app verlassen zu müssen.

Gehen Sie folgendermaßen vor, um dieses Feature zu aktivieren:

1. Erstellen Sie eine nachrichtenapp-Erweiterung.
2. Fügen Sie `com.apple.developer.associated-domains` den Berechtigungen der APP hinzu, und schließen Sie eine Liste von Webdomänen ein, die die Images hosten, die von der APP-Erweiterung für Nachrichten Geben Sie für jede Domäne den `spotlight-image-search` Dienst an.
3. Fügen Sie `apple-app-site-association` der Website, auf der die Images gehostet werden, eine Datei hinzu. Diese Datei enthält ein Wörterbuch für `spotlight-image-search` den Dienst und enthält die ID der APP, die die Team-ID oder das APP-ID-Präfix gefolgt von der Bündel-ID ist. Die Datei kann bis zu 500 Pfade und Muster enthalten, die von Spotlight indiziert und in Beliebte Bilder suchen eingeschlossen werden. Weitere Informationen finden Sie in der Dokumentation zum [Erstellen und Hochladen der](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/AppSearch/UniversalLinks.html#//apple_ref/doc/uid/TP40016308-CH12-SW4) Zuordnungs Datei durch Apple.
4. Ermöglicht dem applebot, die Websites zu durchforsten. Weitere Informationen finden Sie in der Apple-Dokumentation [zu applebot](https://support.apple.com/HT204683) .

Weitere Informationen finden Sie in unserer Dokumentation zur [Nachrichten-APP-Integration](~/ios/platform/message-app-integration/index.md) .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen erläutert, die Apple bei der APP-Suche in ios 10 vorgenommen hat und wie Sie in xamarin. IOS implementiert werden.



## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
