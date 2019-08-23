---
title: In-App-Käufe in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie digitale Produkte und Dienste mithilfe der storekit-APIs verkauft werden. Hier finden Sie Links zu Leitfäden, in denen die Konfiguration, nutzbare Produkte, nicht nutzbare Produkte, Transaktionen, Abonnements und vieles mehr erörtert werden.
ms.prod: xamarin
ms.assetid: B41929D8-47E4-466D-1F09-6CC3C09C83B2
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 2445645012e54b1818b1ec72116a85d8b985ead3
ms.sourcegitcommit: 1341f2950b775a4daa7d0548a51fdef759afd6e3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/22/2019
ms.locfileid: "69976454"
---
# <a name="in-app-purchasing-in-xamarinios"></a>In-App-Käufe in xamarin. IOS

IOS-Anwendungen können digitale Produkte oder Dienste mit storekit verkaufen – eine Reihe von APIs, die von IOS bereitgestellt werden, die mit Apple-Servern kommunizieren, um Finanztransaktionen mit dem Benutzer über Ihre Apple-ID auszuführen. Die storekit-APIs betreffen in erster Linie das Abrufen von Produktinformationen und das Ausführen von Transaktionen – es ist keine Benutzeroberflächen Komponente vorhanden. Anwendungen, die in-App-Käufe implementieren, müssen eine eigene Benutzeroberfläche erstellen und erworbene Elemente mit benutzerdefiniertem Code nachverfolgen, um dem Benutzer die erforderlichen Produkte oder Dienste bereitzustellen.

Zum Bereitstellen von in-App-Kauf Funktionen sind einige Schritte erforderlich:

- **Konfigurieren Ihrer APP** – das Bereitstellungs Profil der Anwendung muss ordnungsgemäß eingerichtet werden.
- **Erstellen von Produkten** – Produktbeschreibungen und Preise müssen im iTunes Connect-Portal erstellt werden.
- **Implementieren von storekit** – die storekit-API muss gemäß den verkauften Produkttypen implementiert werden.
- **Erstellen der Benutzeroberfläche und der Produkte selbst** – die Produkte müssen implementiert werden, einschließlich Mechanismen zum Nachverfolgen der einzelnen Käufe und sichern/wiederherstellen, falls erforderlich.
- Über **Wachen von Verkäufen und Kauf Geldern** – verwenden Sie die von iTunes Connect bereitgestellten Informationen, um Umsatz Trends zu überwachen und Ihr Einkommen zu verfolgen

In diesem Dokument wird erläutert, wie Sie alle diese Schritte ausführen, um in-App-Einkäufe mithilfe von xamarin. IOS bereitzustellen.

## <a name="requirements"></a>Anforderungen

Um in-App-Käufe zu unterstützen, müssen Sie xamarin. IOS 5,0 oder höher mit Xcode 7 und höher verwenden.

## <a name="contents"></a>Inhalt

* [Grundlagen und Konfiguration der In-App-Käufe](~/ios/platform/in-app-purchasing/in-app-purchase-basics-and-configuration.md)

* [Übersicht über storekit und Abrufen von Produktinformationen](~/ios/platform/in-app-purchasing/store-kit-overview-and-retreiving-product-information.md)

* [Kaufen von Verbrauchsartikeln](~/ios/platform/in-app-purchasing/purchasing-consumable-products.md)

* [Kaufen von Nicht-Verbrauchsartikeln](~/ios/platform/in-app-purchasing/purchasing-non-consumable-products.md)

* [Transaktionen und Überprüfung](~/ios/platform/in-app-purchasing/transactions-and-verification.md)

* [Abonnements und Berichte](~/ios/platform/in-app-purchasing/subscriptions-and-reporting.md)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Konzept des in-App-Einkaufs eingeführt, und es wurde beschrieben, wie Sie Ihre Anwendung so konfigurieren, dass Sie von der APP genutzt und Beispiele mithilfe von xamarin. IOS präsentiert werden. Es hat Folgendes abgedeckt:

- **IOS-Bereitstellungs Portal** – Richtlinien zum Aktivieren der in-App-Kauf Funktionalität.
- **iTunes Connect** – konfigurieren Sie die Produkte, die in Ihrer APP verkauft werden.
- **Store Kit** – Erläuterung der Klassen, die zum Erstellen von in-App-Kauf Features verwendet werden.
- **Codieren Ihrer APP für den Einkauf** – Beispiele für das Erstellen von in-App-Käufen in einer xamarin. IOS-app.
- **Bericht** Erstellung – Übersicht über die über iTunes Connect verfügbaren Statistiken.


## <a name="related-links"></a>Verwandte Links

- [InAppPurchaseSample](https://docs.microsoft.com/samples/xamarin/ios-samples/storekit/)
- [Im Leitfaden zur APP-Kauf Programmierung](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Introduction.html)
- [iTunes Connect-Entwicklerleitfaden](https://developer.apple.com/library/ios/documentation/LanguagesUtilities/Conceptual/iTunesConnect_Guide/iTunesConnect_Guide.pdf)
- [Store Kit-frameworkverweis](https://developer.apple.com/library/ios/documentation/StoreKit/Reference/StoreKit_Collection/StoreKit_Collection.pdf)
- [Produkt Bezeichner für in-App-Käufe Q & A](https://developer.apple.com/library/ios/#qa/qa1329/_index.html)
- [Technische Notiz zum in-App-Erwerb](https://developer.apple.com/library/ios/#technotes/tn2259/_index.html)
- [Ihre erste App Store-Übermittlung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
- [App Store-Ressourcen Center](https://developer.apple.com/appstore/index.html)
- [Tipps für die Übermittlung an den App Store](https://developer.apple.com/appstore/resources/submission/tips.html)
- [Richtlinien für die Überprüfung im App Store](https://developer.apple.com/appstore/resources/approval/guidelines.html)
- [Verwalten von apps](https://developer.apple.com/appstore/resources/managing/index.html)
