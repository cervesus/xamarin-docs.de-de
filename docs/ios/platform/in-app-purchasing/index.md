---
title: In-App-Xamarin.iOS Kaufverhalten
description: Dieses Dokument beschreibt, wie digitale Produkte und Dienste, die mithilfe der StoreKit-APIs zu verkaufen. Es links zu Anleitungen, die Konfiguration, nutzbar Produkte, die Verwendbarkeit nicht Produkte, Transaktionen, Abonnements und mehr zu behandeln.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 8a41ed44a331c91a333b95c1d62136244a6945dd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787340"
---
# <a name="in-app-purchasing-in-xamarinios"></a>In-App-Xamarin.iOS Kaufverhalten

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

 * [Übersicht über die StoreKit und Abrufen von Produktinformationen](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

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
- [iTunes Connect-Entwicklerleitfaden](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Referenz für Store-Kit-Framework](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [In App-Käufe-Produkt-IDs Fragen und Antworten](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Technischer Hinweis in App-Käufe](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Die erste App Store-Übermittlung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [App Store-Ressourcencenter](https://developer.apple.com/appstore/index.html)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Verwalten von Apps](https://developer.apple.com/appstore/resources/managing/index.html)
