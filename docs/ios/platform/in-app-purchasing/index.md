---
title: "In-App-Käufe"
description: "iOS-Anwendungen können digitale Produkte und Dienste, die mithilfe der Store-Kit-APIs verkaufen. Produkte werden erstellt und im iTunes Connect-Portal verwaltet. Verwaltet die transaktionsverarbeitung von Apple und alle Produkte genehmigt, bevor sie verkauft werden können, und die Kosten gegen einer Gebühr für jede Transaktion (derzeit 30 %). Apple erfordert, dass Sie verwenden, die in-app, die für digitale Verkäufe in Ihrer app erwerben, aber nicht für den Verkauf von physischen Waren oder nicht Digital-Dienste verwenden. Apps, die alternative Zahlungsoptionen für digitale Produkte und Dienste zu bieten, werden wahrscheinlich abgelehnt werden. Dieses Dokument wird erläutert, wie Ihre Anwendung zur Verwendung von Store-Kit konfigurieren sowie Xamarin.iOS Beispiele für die häufigsten Kaufverhalten in app-Szenarien."
ms.topic: article
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: af8eb556215679bab2da8f54e8231f7d7d3ed418
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="in-app-purchasing"></a>In-App-Käufe

_iOS-Anwendungen können digitale Produkte und Dienste, die mithilfe der Store-Kit-APIs verkaufen. Produkte werden erstellt und im iTunes Connect-Portal verwaltet. Verwaltet die transaktionsverarbeitung von Apple und alle Produkte genehmigt, bevor sie verkauft werden können, und die Kosten gegen einer Gebühr für jede Transaktion (derzeit 30 %). Apple erfordert, dass Sie verwenden, die in-app, die für digitale Verkäufe in Ihrer app erwerben, aber nicht für den Verkauf von physischen Waren oder nicht Digital-Dienste verwenden. Apps, die alternative Zahlungsoptionen für digitale Produkte und Dienste zu bieten, werden wahrscheinlich abgelehnt werden. Dieses Dokument wird erläutert, wie Ihre Anwendung zur Verwendung von Store-Kit konfigurieren sowie Xamarin.iOS Beispiele für die häufigsten Kaufverhalten in app-Szenarien._


iOS-Anwendungen können verkaufen, digitale Produkte oder Dienste, die mit StoreKit – einen Satz von APIs, die von iOS, die Kommunikation mit der Apple-Servern bereitgestellt für die Durchführung von finanzielle Transaktionen mit dem Benutzer über ihre Apple-ID. Die StoreKit-APIs sind in erster Linie durch das Abrufen von Produktinformationen und Durchführen von Transaktionen betroffenen – es gibt keine Benutzeroberfläche-Komponente. Anwendungen, die in der app zu erwerben implementieren müssen ihre eigene Benutzeroberfläche erstellen, und verfolgen Sie gekauften mit benutzerdefiniertem Code für dem Benutzer der erforderlichen Produkte oder Dienste bereitstellen.

Entwicklung von Funktionen in app-Käufe erfordert eine Reihe von Schritten:

-  **Konfigurieren Ihre app** – Bereitstellungsprofil für die Anwendung muss ordnungsgemäß eingerichtet werden.
-  **Erstellen von Produkten** – produktbeschreibungen und Preisen in iTunes Connect Portal erstellt werden müssen.
-  **Implementieren von StoreKit** – die StoreKit-API muss gemäß der Typen von verkauften Produkte implementiert werden.
-  **Erstellen der Benutzeroberfläche und die Produkte selbst** – die Produkte müssen implementiert werden, einschließlich Mechanismen zum Nachverfolgen der jeweiligen Kauf und Sicherung/Wiederherstellung sie bei Bedarf.
-  **Überwachung von Sales und Empfangen von Betrag** – vom iTunes Connect bereitgestellten Informationen mit der Umsatzentwicklung überwachen und verfolgen Ihre Einkommen.


Dieses Dokument erläutert, welche Schritte Sie all diese Schritte, um zu gewährleisten, dass in-app-Käufe mithilfe von Xamarin.iOS.


## <a name="requirements"></a>Anforderungen

Zur Unterstützung von In-App zu erwerben müssen Sie in Xcode 7 und höher Xamarin.iOS 5.0 oder höher verwenden.

## <a name="contents"></a>Inhalt

 * [Grundlagen und Konfiguration der In-App-Käufe](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Store Kit-Übersicht und Abrufen von Produktinformationen](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Kaufen von Verbrauchsartikeln](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Kaufen von Nicht-Verbrauchsartikeln](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [Transaktionen und Überprüfung](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Abonnements und Berichte](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)


## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt wurde das Konzept der in der app zu erwerben, beschrieben, wie zum Konfigurieren Ihrer Anwendung zu nutzen und Beispiele zur Verwendung von Xamarin.iOS angezeigt. Es wurde behandelt:

-  **iOS-Bereitstellungsportal** – Richtlinien für das Aktivieren von in-app-Funktionalität zu erwerben.
-  **iTunes Connect** – Konfigurieren von Produkten in Ihrer app zu verkaufen.
-  **Speichern von Kit** – Erläuterung der Klassen verwendet, um Funktionen in app-Käufe erstellen.
-  **Codieren Ihre app für den Erwerb von** – Beispiele für in-app-Käufe in einem Xamarin.iOS-app zu erstellen.
-  **Reporting** – Übersicht über die Statistiken über iTunes Connect verfügbar.


## <a name="related-links"></a>Verwandte Links

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [In App-Käufe Programmierhandbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect-Entwicklerhandbuch](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Referenz für Store-Kit-Framework](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [In App-Käufe-Produkt-IDs Fragen und Antworten](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Technischer Hinweis in App-Käufe](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Die erste App Store-Übermittlung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [App Store Resource Center](https://developer.apple.com/appstore/index.html)
- [Tipps für App-Store-Übermittlung](https://developer.apple.com/appstore/resources/submission/tips.html)
- [App Store Review Guidelines](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Verwalten von Apps](https://developer.apple.com/appstore/resources/managing/index.html)
