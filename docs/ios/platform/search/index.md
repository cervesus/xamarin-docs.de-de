---
title: In Xamarin.iOS-Suche-APIs
description: Dieser Artikel befasst sich mit den neuen App-Such-APIs unter iOS 9 bereitgestellt, dass Benutzer nach Informationen und Funktionen in Ihrer Xamarin.iOS-apps suchen können.
ms.prod: xamarin
ms.assetid: 7323EB3D-A78F-4BF0-9990-3160C7E83CF0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/20/2017
ms.openlocfilehash: b2968399279fe3e9d160471bbcae08ae091be93e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61075653"
---
# <a name="search-apis-in-xamarinios"></a>In Xamarin.iOS-Suche-APIs

_Dieser Artikel befasst sich mit den App-Suche-APIs unter iOS 9 bereitgestellt, dass Benutzer nach Informationen und Funktionen in Ihrer Xamarin.iOS-apps suchen können._

Suche wurde erweitert, unter iOS 9, um hervorragende neue Möglichkeiten zum Zugriff auf Informationen und Funktionen in einer Xamarin.iOS-app bereitzustellen. Verwenden den neuen App-Suche-APIs, app-Inhalte durch Spotlight und Safari Suchergebnissen Übergabeanimationen und Siri-Erinnerungen und Vorschläge durchsuchbaren erfolgt. Dadurch können Benutzer schnell die Aktivitäten und Informationen, die Tiefe in Ihrer app zugreifen.

Darüber hinaus erleichtern den neuen Such-APIs auf die Suche in Ihrer app ohne die vorherige Implementierung Suchfunktionen integrieren. Aus diesem Grund Ansprüche Apple an, dass in der Regel ein paar Stunden dauert nach einer iOS 9-app-Inhalte global gesucht werden mithilfe von App-Suche.

[![](images/intro01.png "Ein Beispiel für iOS 9-app-Inhalte global durchsuchbaren mithilfe von App-Suche")](images/intro01.png#lightbox)

App-Suche besteht aus drei verschiedenen APIs:

1. [**NSUserActivity** ](nsuseractivity.md) – Dies ist eine Erweiterung der Übergabe-API, die Apple iOS 8 veröffentlicht. Es wird zum app-Interaktion Verlauf durchsuchbar machen sowohl öffentlich und privat) durch den Benutzer.

2. [**Core-Spotlight** ](corespotlight.md) -ermöglicht einer app zur Indizierung seines Inhalts in den Suchergebnissen angezeigt werden. Es funktioniert wie eine Datenbank-API, in dem Elemente hinzugefügt und entfernt werden können und es ist die beste Möglichkeit, private Indizieren von Inhalten in einer app.

3. [**WebMarkup** ](web-markup.md) : für apps, die Zugriff auf ihre Inhalte über eine Webschnittstelle bereitstellen (nicht nur von innerhalb der app). Webinhalte kann mit speziellen Links gekennzeichnet werden, die von Apple wird gecrawlt und deep Links zu Ihrer app auf iOS 9-Gerät des Benutzers.

## <a name="selecting-an-app-search-approach"></a>Wählen eine App-Suche-Methode

Entscheiden, welche der folgenden Methoden implementieren, hängt davon ab die Typen der Interaktion, die von Ihrer app bereitgestellt wird und der Typ des Inhalts, die es bereitstellt.

Verwenden Sie die folgenden Richtlinien:

- [**NSUserActivity** ](nsuseractivity.md) – verwenden Sie dieses Framework durchsuchbarkeit verbessert sowohl die öffentlichen und privaten Inhalte als auch Auffindbarkeit Navigation Punkten in Ihrer app bereit.

- [**Core-Spotlight** ](corespotlight.md) – verwenden Sie dieses Framework, privater Daten, die auf dem Gerät gespeicherten Metadatenfeldern bereit.

- [**Web-Markup** ](web-markup.md) – verwenden Sie dieses Framework, um die Auffindbarkeit für apps bereit, mit denen ihre Inhalte nicht nur von innerhalb der app, sondern auch der app-Website darstellen.

Die für die App-Suche verfügbar sind, unterscheiden sich und können dazu verwendet einzeln jedoch Apple diese Zusammenarbeit konzipiert. Wenn mehr als ein Ansatz zum Indizieren eines bestimmten Elements, stellen Sie sicher, dass Sie die gleiche verwenden **Element-ID** auf jedem Ansatz, also die Person Arbeitsaufgaben miteinander verknüpft.

Mit mehr als ein Ansatz stellt nicht nur sicher, dass Ihre Inhalte vom Endbenutzer gefunden, aber auch wird die Rangfolge des Elements aus in Search verbessert.

Während der Prozess der Rangfolge in wiegt für den Entwickler größtenteils transparent Benutzerinteraktion mit einem bestimmten Element stark nach dieser Rang (z. B. der Benutzer Tippen auf einen Link).
Durch die Bereitstellung umfassender, informative Elemente, können Sie sicherstellen, dass ein Benutzer begeistert sein werden für die Interaktion mit Ihrer Inhalte, daher auslösen sein Rang.

## <a name="what-content-to-index"></a>Welche Inhalte Index

Apple bietet die folgenden Vorschläge, welche Inhalte und Aktionen zu Search-Indizes für die in Ihrer app:

 - Keine Inhalte angezeigt, erstellt oder zusammengestellten durch den Benutzer in Ihrer app.
 - Navigationspunkte und Funktionen in der app.
 - Dinge wie neue Nachrichten, Inhalt oder andere Arten von Elementen, die von Ihrer app angezeigt, die vor kurzem an das Gerät heruntergeladen wurden.

## <a name="app-search-enhancements"></a>Verbesserungen bei der App-Suche

Core Spotlight IOS 10 bietet mehrere Erweiterungen wie z. B. zum App-Suche an:

- **Per Crowdsourcing gesammelter Kommentare Deep-Link Beliebtheit (mit differenziellen Privacy)** -bietet eine Möglichkeit zur Förderung der Deep-Link-app-Inhalte in den Suchergebnissen angezeigt.
- **In-App-Suche** -verwenden Sie die neue `CSSearchQuery` Klasse zu in-app-Spotlight-Suche Möglichkeit, die ähnlich wie die E-Mail, Nachrichten und Anmerkungen zu dieser apps arbeiten.
- **Suchen Sie die Fortsetzung** – ermöglicht Benutzern das Starten einer Suche im Blickpunkt oder Safari, und öffnen Sie eine app, und diese Suche fortsetzen.
- **Visualisierung von Überprüfungsergebnissen** -Apple [App-Suche-API-Überprüfungstool](https://search.developer.apple.com/appsearch-validation-tool) zeigt nun eine visuelle Darstellung von Markup und Deep linking einer Website aus, bei der Herstellung von Tests.
- **Nachrichten-App Teilen von Bildern** -beliebten in-app-Images bereitgestellt werden, für die Freigabe in Nachrichten (über eine Nachrichten-App-Erweiterung), bei der Spotlight-Suche angezeigt werden können.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [Verbesserungen bei der Suche von App](~/ios/platform/search/app-search-enhancements.md) Handbuch.

### <a name="proactive-suggestions"></a>Proaktive Vorschläge

iOS 10 stellt neue Methoden für die treibende Engagement zu einer app durch den Wechsel des Systems proaktiv hilfreiche Informationen automatisch an den Benutzer zur richtigen Zeit jeweils vorhanden. Wie iOS bereitgestellten 9 die Möglichkeit zum Hinzufügen von detaillierten Suche der App mit Spotlight, Übergabeanimationen und Vorschläge für Siri mit iOS 10-app Funktionalität verfügbar gemacht werden können, die für dem Benutzer durch das System in den folgenden Speicherorten angezeigt werden kann:

- Der App-Switcher
- Der Sperrbildschirm
- CarPlay
- Karten
- Siri-Interaktionen
- QuickType Vorschläge 

Eine app macht diese Funktion in das System für eine Sammlung von Technologien wie z. B. über [NSUserActivity](xref:Foundation.NSUserActivity), Markup im Web, Core Spotlight, MapKit, Media Player und UIKit.

Wenn Sie mehr erfahren möchten, informieren Sie sich unsere [proaktive Vorschläge](~/ios/platform/search/proactive-suggestions.md) Guide.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt die neuen Funktionen von Such-API, iOS 9 bietet für Xamarin.iOS-apps. Äquivalent [NSUserActivity](nsuseractivity.md), [Core Spotlight](corespotlight.md) und [Webmarkup](web-markup.md) Methoden zum Indizieren des Inhalts. Abgeschlossen mit einer kurzen Erläuterung der wann ein Ansatz für die Suche verwendet werden sollten und welche Arten von Inhalten werden sollte indiziert.



## <a name="related-links"></a>Verwandte Links

- [iOS 9-Beispiele](https://developer.xamarin.com/samples/ios/iOS9/)
- [iOS 9 für Entwickler](https://developer.apple.com/ios/pre-release/)
- [iOS 9.0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Programmierhandbuch für App-Suche](https://developer.apple.com/library/prerelease/ios/documentation/General/Conceptual/AppSearch/index.html#//apple_ref/doc/uid/TP40016308)
