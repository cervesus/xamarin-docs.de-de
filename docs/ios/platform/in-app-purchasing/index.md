---
title: In-App-Käufe in Xamarin.iOS
description: Dieses Dokument beschreibt die digitalen Produkten und Diensten, die mithilfe der StoreKit-APIs zu verkaufen. Es enthält links zu Anleitungen, die Konfiguration, verbrauchsartikeln, nicht-verbrauchsartikeln, Transaktionen, Abonnements und vieles mehr zu erläutern.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4b301c18ea0e69c818cf65b3b7df1cc8351350f5
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119331"
---
# <a name="in-app-purchasing-in-xamarinios"></a>In-App-Käufe in Xamarin.iOS

iOS-Anwendungen können verkaufen, digitalen Produkten oder Diensten, die mithilfe von StoreKit – eine Reihe von APIs unter iOS, die die Kommunikation mit Apple Server bereitgestellt, Finanztransaktionen, mit dem Benutzer über ihre Apple-ID durchzuführen. Die StoreKit-APIs sind in erster Linie mit Produktinformationen abrufen und Durchführen von Transaktionen befassen: Es gibt keine Benutzeroberfläche-Komponente. Anwendungen, die in-app-Käufe implementieren, müssen ihre eigene Benutzeroberfläche erstellen und verfolgen gekauften Artikel mit benutzerdefiniertem Code, dem Benutzer die erforderlichen Produkte oder Dienste anbieten.

Bereitstellen in app-Käufe Funktionalität erfordert eine Reihe von Schritten:

-  **Konfigurieren Ihrer app** – Bereitstellungsprofil für die Anwendung muss ordnungsgemäß eingerichtet werden.
-  **Erstellen von Produkten** – produktbeschreibungen und Preise in iTunes Connect Portal erstellt werden müssen.
-  **Implementieren von StoreKit** – die StoreKit-API muss entsprechend den von der angebotenen Produkte implementiert werden.
-  **Erstellen der Benutzeroberfläche und den Produkten selbst** – die Produkte müssen implementiert werden, einschließlich Mechanismen zum Nachverfolgen von jedem Kauf und Sicherung/Wiederherstellung sie bei Bedarf.
-  **Sales überwachen und Empfangen von Geldmittel** – verwenden Sie die Informationen von iTunes Connect Umsatztrends sowie überwachen Ihre Einnahmen.

Dieses Dokument erläutert, wie Sie alle diese Schritte zum Bereitstellen, dass in-app-Käufe unter Verwendung von Xamarin.iOS.

## <a name="requirements"></a>Anforderungen

Zur Unterstützung von In-App-Käufe müssen Sie Xamarin.iOS 5.0 oder höher mit Xcode 7 und höher verwenden.

## <a name="contents"></a>Inhalt

 * [Grundlagen und Konfiguration der In-App-Käufe](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

 * [Übersicht über die StoreKit und Abrufen von Produktinformationen](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

 * [Kaufen von Verbrauchsartikeln](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

 * [Kaufen von Nicht-Verbrauchsartikeln](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

 * [Transaktionen und Überprüfung](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

 * [Abonnements und Berichte](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat führte das Konzept der in-app-Käufe, beschrieben, wie Sie zum Konfigurieren Ihrer Anwendung zu nutzen und Beispiele zur Verwendung von Xamarin.iOS angezeigt. Es wurden behandelt:

-  **iOS-Bereitstellungsportal** – Richtlinien zum Aktivieren von in-app-Funktionalität zu erwerben.
-  **iTunes Connect** – Konfigurieren von Produkten in Ihrer app zu verkaufen.
-  **Store Kit** – Erklärung der Klassen verwendet, um in-app-Käufe Features zu erstellen.
-  **Codieren Ihre app für den Kauf von** – Beispiele für die Verwendung in app-Käufe in einer Xamarin.iOS-app zu erstellen.
-  **Reporting** – Überblick über die Statistiken über iTunes Connect zur Verfügung.


## <a name="related-links"></a>Verwandte Links

- [InAppPurchaseSample](https://developer.xamarin.com/samples/StoreKit/)
- [In App-Käufe-Programmierhandbuch](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect-Entwicklerleitfaden](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Store Kit Frameworkverweis](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Produkt-IDs in App-Käufe, häufig gestellte Fragen](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Technischer Hinweis in App-Käufe](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Ihre erste App Store-Übermittlung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [App Store-Ressourcencenter](https://developer.apple.com/appstore/index.html)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Die App-Verwaltung](https://developer.apple.com/appstore/resources/managing/index.html)
