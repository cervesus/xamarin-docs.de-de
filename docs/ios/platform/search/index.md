---
title: Neue Such-APIs
description: "Dieser Artikel behandelt die neuen App-Such-APIs bereitgestellt, die von iOS 9 verwenden, um Benutzern das Suchen nach Informationen und Funktionen in Ihren apps Xamarin.iOS ermöglichen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 6ec8cb9b6fdb391afcb8f9baaa641da5aec38f6d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="new-search-apis"></a>Neue Such-APIs

_Dieser Artikel behandelt die neuen App-Such-APIs bereitgestellt, die von iOS 9 verwenden, um Benutzern das Suchen nach Informationen und Funktionen in Ihren apps Xamarin.iOS ermöglichen._

Suche wurde in iOS 9 hervorragende neue Möglichkeiten, um den Zugriff auf Informationen und Funktionen in einem Xamarin.iOS-app bereitgestellt erweitert. Verwenden die neuen App-Such-APIs, ist app-Inhalte über die Spotlight und Safari Suchergebnissen Übergabe und Siri Erinnerungen und Vorschläge als durchsuchbar festgelegt. Dadurch können Benutzer, Aktivitäten und innerhalb Ihrer app umfassende Informationen rasch zuzugreifen.

Darüber hinaus erleichtern die neue Such-APIs Suche in Ihrer app ohne die vorherige Implementierung Suchabfrage zu integrieren. Aus diesem Grund Ansprüche Apple an, dass es sich normalerweise um ein paar Stunden zu einer iOS 9-app-Inhalte universell mithilfe der App suchen zu lassen dauert.

[![](images/intro01.png "Ein Beispiel für iOS 9-app-Inhalte universell durchsuchbaren mithilfe der App-Suche")](images/intro01.png#lightbox)

App-Suche besteht aus drei separaten-APIs:

1. [**NSUserActivity** ](nsuseractivity.md) -Dies ist eine Erweiterung der Übergabe-API, die Apple iOS 8 veröffentlicht. Dient zur Verwendungsverlauf der app-Interaktion durchsuchbaren stellen sowohl öffentlich und privat) durch den Benutzer.

2. [**Core Spotlight** ](corespotlight.md) -ermöglicht einer app zur Indizierung seines Inhalts in den Suchergebnissen angezeigt werden. Es funktioniert wie eine Datenbank-API, in denen Elemente hinzugefügt oder entfernt werden können und es ist die beste Methode zum Index privaten Inhalt innerhalb einer app.

3. [**WebMarkup** ](web-markup.md) – für apps, die Zugriff auf ihre Inhalte über eine Weboberfläche bereitstellen (nicht nur von innerhalb der app). Webinhalte kann mit speziellen Links gekennzeichnet werden, die von Apple gecrawlt werden und bieten umfassende Verknüpfen mit Ihrer app auf iOS 9-Gerät des Benutzers.

## <a name="selecting-an-app-search-approach"></a>Auswählen eines Ansatzes für die App-Suche

Entscheiden, welche der folgenden Methoden zu implementieren, hängt davon ab die Typen von Interaktion, die von Ihrer Anwendung bereitgestellt und der Typ des Inhalts, wenn es vorlegt.

Verwenden Sie die folgenden Richtlinien:

- [**NSUserActivity** ](nsuseractivity.md) – verwenden Sie dieses Framework, um Suchvorgänge für die öffentlichen und privaten Inhalt und auch Suchvorgänge von Points für die Navigation innerhalb der app bereitzustellen.

- [**Core Spotlight** ](corespotlight.md) – verwenden Sie dieses Framework private Daten, die auf dem Gerät gespeicherten Suchvorgänge bereit.

- [**Web-Markup** ](web-markup.md) – verwenden Sie dieses Framework, um Suchvorgänge für apps bereitzustellen, die ihre Inhalte nicht nur von innerhalb der app, sondern auch für die app-Website darstellen.

Jeder der Ansätze unterscheiden und können die App-Suche verwendet einzeln jedoch Apple diese Zusammenarbeit konzipiert. Wenn mehr als ein Ansatz verwenden, um ein bestimmtes Element zu indizieren, stellen Sie sicher, dass Sie die gleiche verwenden **Element-ID** auf jedem Ansatz, sodass diese Person Arbeit miteinander verknüpft.

Mit mehr als ein Ansatz wird sichergestellt, dass nicht nur, dass Ihre Inhalte vom Endbenutzer gefunden werden wird, sondern trägt auch dazu bei, um die Rangfolge des Elements aus Suche zu verbessern.

Während des Prozesses Rangfolge in wiegt für den Entwickler überwiegend transparenter Interaktionen der Benutzer mit einem bestimmten Element stark nach diesen Rang (z. B. die Benutzer Tippen Sie auf einen Link).
Bereitstellen reichhaltiger, informative Elemente an, stellen Sie sicher, dass ein Benutzer begeistert sein wird für die Interaktion mit Ihrem Inhalt daher sein Rang auslösen.

## <a name="what-content-to-index"></a>Welche Inhalte an Index

Apple bietet die folgenden Vorschläge, welche Inhalte und Aktionen Suchindizes für in Ihrer app bereitgestellt:

 - Alle Inhalte angezeigt, erstellt oder curated vom Benutzer innerhalb Ihrer app.
 - Points für die Navigation und Funktionen innerhalb der app.
 - Elemente wie neue Nachrichten, die Inhalt oder andere Typen von Elementen, die von Ihrer Anwendung angezeigt, die kürzlich auf dem Gerät heruntergeladen wurden.

## <a name="app-search-enhancements"></a>Suchen Sie App-Erweiterungen

Core Spotlight in iOS 10 bietet mehrere Erweiterungen wie z. B. App suchen:

- **Crowdsourced Deep-Link Beliebtheit (mit differenzielle Datenschutz)** -bietet eine Möglichkeit, die app mit Deep-Link-Inhalt in den Suchergebnissen angezeigter höher stufen.
- **Suchen in der App** -verwenden Sie die neue `CSSearchQuery` -Klasse, in der app Spotlight-Suche Möglichkeit ähnlich wie die e-Mail-, Nachrichten und Anmerkungen zu dieser apps arbeiten bereitzustellen.
- **Suchen Sie die Fortsetzung** – ermöglicht Benutzern das Starten einer Suche im Spotlight oder Safari, und öffnen Sie eine app und die Suche zu fortfahren.
- **Visualisierung von Überprüfungsergebnissen** -Apple [App Search-API-Überprüfungstools](https://search.developer.apple.com/appsearch-validation-tool) zeigt jetzt eine visuelle Darstellung von Markup und umfassende Verknüpfen einer Website aus, wenn Tests preforming.
- **Freigabe-App Image Message** -können Sie gängige in app-Images, die bereitgestellt werden, für die Freigabe in Nachrichten (über eine Nachrichten-App-Erweiterung) in der Spotlight-Suche angezeigt werden sollen.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [App Suche Erweiterungen](~/ios/platform/search/app-search-enhancements.md) Handbuch.

### <a name="proactive-suggestions"></a>Proaktive Vorschläge

iOS 10 präsentiert neue Methoden für die steuernde Engagement an eine app nach Wechsel des Systems proaktiv hilfreiche Informationen automatisch für den Benutzer zur richtigen Zeit jeweils anzuzeigen. Ebenso wie iOS bereitgestellt 9 die Möglichkeit zum Hinzufügen von umfassenden Suche der App mit Spotlight, Übergabe und Vorschläge zur Siri mit iOS 10, die eine app Funktionen verfügbar gemacht werden können, die vom System in den folgenden Speicherorten aus, die dem Benutzer angezeigt werden können:

- Der App Switcher
- Dem Sperrbildschirm
- CarPlay
- Karten
- Siri Interaktionen
- QuickType-Vorschläge 

Eine app macht diese Funktion in das System für eine Sammlung von Technologien wie z. B. über [NSUserActivity](https://developer.xamarin.com/api/type/Foundation.NSUserActivity/), Web-Markup, Core Spotlight, MapKit und UIKit Media Player.

Wenn Sie mehr erfahren möchten, finden Sie unter unsere [proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md) Handbuch.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat die neue abgedeckt-Suchdienst-API-Funktionen, iOS 9 zum Xamarin.iOS apps bereitstellt. Äquivalent [NSUserActivity](nsuseractivity.md), [Core Spotlight](corespotlight.md) und [Web Markup](web-markup.md) Methoden zum Indizieren von Inhalt. Mit der eine kurze Erläuterung der bei ein angegebenen Suchmuster Ansatz verwendet werden soll und welche Arten von Inhalten sollte abgeschlossen indiziert.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
