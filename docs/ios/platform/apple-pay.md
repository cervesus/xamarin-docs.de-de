---
title: Apple Pay in Xamarin.iOS
description: Dieses Handbuch untersucht das Einrichten der Umgebung Xamarin.iOS für die Verwendung mit Apple Pay zum Bezahlen der physischer waren, z. B. Nahrungsmittel Kunst, Unterhaltung und Mitgliedschaften über Ihre app. Es enthält Informationen zu den erforderlichen Bezeichner, Zertifikate und Berechtigungen.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 7033373cddb2503e5912eb17b1e72ece759cc3ad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786690"
---
# <a name="apple-pay-in-xamarinios"></a>Apple Pay in Xamarin.iOS

_Dieses Handbuch untersucht das Einrichten der Umgebung Xamarin.iOS für die Verwendung mit Apple Pay zum Bezahlen der physischer waren, z. B. Nahrungsmittel Kunst, Unterhaltung und Mitgliedschaften über Ihre app. Es enthält Informationen zu den erforderlichen Bezeichner, Zertifikate und Berechtigungen._

Apple Pay wurde zusammen mit iOS 8 eingeführt, sodass Benutzer zum Bezahlen der physischer waren, z. B. Nahrungsmittel Kunst, Unterhaltung und Mitgliedschaften über ihre iOS-Geräte. Er ist für iPhone 6 und iPhone 6 Plus, und Sie können auch mit der Apple Watch für Store-Einkäufen gekoppelt werden. Bei Verwendung auf einem iPhone wird verwendet Touch ID eine Möglichkeit, um zu bestätigen, und Autorisieren eines Benutzers Kreditkarte oder Debitkarte zur Transaktionen.

## <a name="requirements"></a>Anforderungen

Apple Pay ist nur verfügbar im iOS 8 und höher und erfordert daher mindestens Xcode 6.

Die folgenden Elemente sind auch erforderlich, um Apple Pay in Ihrer app zu integrieren:

 - Zahlungsplattform-Prozessor
 - Anbieter-ID
 - Ein Apple Pay-Zertifikat
 - Apple Pay-Berechtigung

Dieses Dokument werden diese Elemente ausführlicher betrachten.

## <a name="differences-between-apple-pay-and-iap"></a>Unterschiede zwischen dem Apple und IAP

Der Hauptunterschied zwischen Apple Pay und *Erwerb von In-App* (IAP), bezieht sich auf die Produkte, die sie verkaufen. *Physische* waren über Apple Pay verkauft werden; Nahrungsmittel, Unterkunft und physischen Unterhaltung (z. B. im Kino Tickets) sind Beispiele für dieses. Im Gegensatz dazu IAP verkauft *virtuellen* waren, z. B. Premium oder zusätzlichen Inhalte und Abonnements – Reaktionszeiten zusätzliche Monate einen streamingdienst oder zusätzliche befinden sich in einem Spiel.

Die Frameworks verwendet werden auch einen wesentlichen Unterschied; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) ist für die Apple Pay verwendet, während er sich [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) IAP Framework-API bereit.

Mit Apple Pay, Apple [Zustände](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , dass er "fallen keine Benutzer, Händler oder Entwickler mithilfe von Apple Pay für Zahlungen Kosten []". Im Gegensatz dazu hat IAP 30 % sind für jede Transaktion kostenpflichtig. Darüber hinaus mit Apple Pay, die Transaktion geht nicht über Apple überhaupt, stattdessen eine zahlungsplattform durchlaufen.

## <a name="using-a-payment-processor-platform"></a>Verwenden eine Zahlung Prozessorplattform

Eine der grundlegenden Bestandteile von Apple Pay ist die Verarbeitung von Zahlungen. Es ist, zwar möglich, dies selbst tun erfordert sie beträchtliche Kenntnisse der Kryptografie
- finden Sie in der Apple- [Zahlung verarbeiten Handbuch](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Zahlung Verarbeitung Plattformen behandeln andererseits, diese Vorgänge für Sie, sodass Sie sich auf das Erstellen Ihrer app konzentrieren.

Zwei Optionen:

- **Stripe** -melden Sie sich bei [Stripe.com](https://stripe.com/) dieser APIs den Zugriff auf.

- **JudoPay** -sehen Sie sich ihre [Xamarin-Beispielcode auf Github](https://github.com/Judopay/Xamarin-Sample-App), und melden Sie sich bei [JudoPay.com](https://www.judopay.com/).

## <a name="provisioning-for-apple-pay"></a>Für Apple Pay-Bereitstellung

Eine app so konfigurieren, verwenden Sie das Apple Pay, fordert Setup auf der Apple-Entwicklerportal und in Ihrer app. Es gibt eine Reihe von Schritten, die ausgeführt werden sollen, um Ihre app für Apple Pay erfolgreich bereitzustellen:

1. Erstellen Sie eine Händler-ID:
    - Führen Sie die Schritte [hier](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Erstellen einer App-ID mithilfe der Funktion für Zahlen angewendet, und fügen Sie den Anbieter hinzu:
    - Führen Sie die Schritte [hier](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Generieren Sie ein Zertifikat für die Anbieter-ID:
    - Führen Sie die Schritte [hier](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Generieren Sie ein Bereitstellungsprofil mit der neu erstellte App-ID:
    - Führen Sie die Schritte [hier](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay Berechtigungen hinzufügen:
    - Wählen Sie die Apple Pay Ansprüche als ausführliche [hier](~/ios/deploy-test/provisioning/entitlements.md), oder fügen Sie manuell das Schlüssel/Wert-Paar in die Datei aus [hier](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Arbeiten mit Apple Pay

Apple hat mehrere Erweiterungen Apple Pay in iOS 10 vorgenommen, mit denen Benutzer sicheren Zahlungen von Websites und über die Interaktion mit Siri und Zuordnungen vornehmen können.

Mit iOS-10 wurden mehrere neue APIs hinzugefügt, arbeiten mit iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-testumgebung.

### <a name="apple-pay-website-integration"></a>Apple Pay-Website-Integration

Neu für iOS 10, der Entwickler kann integrieren, Apple Pay direkt in ihren Websites mit **ApplePay JS**. Suchen Sie die Website mit Safari in iOS- oder Mac OS können Zahlungen mit Apple Pay durch Überprüfen der Transaktions auf dem iPhone oder Apple Watch vornehmen. Weitere Informationen finden Sie in der Apple- [ApplePay JP Frameworkverweis](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>PassKit Framework-Erweiterungen

In iOS-10, wurde das PassKit-Framework zur Unterstützung von Apple Pay außerhalb von erweitert `UIKit` und -Aussteller an ihre eigenen Karten in ihren apps zu ermöglichen.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Unterstützung von Apple Pay außerhalb UIKit

Mithilfe von [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), eine app kann die gleiche Funktionalität von bereitgestellten unterstützen [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) ohne UIKit. Während die neue API für die Unterstützung von Apple Pay auf der Apple Watch (und in bestimmten Intents sowie) erforderlich ist, ist es in anderen Situationen (z. B. vorhandene apps) optional. Allerdings schlägt Apple Verschieben an die neue API so bald wie möglich mit einer einzelnen Codebasis umfassende Apple Pay-Unterstützung in allen von der Entwickler apps bereitstellen. Weitere Informationen zu Intents und Siri-Integration finden Sie in unserer [Einführung in SiriKit](~/ios/platform/sirikit/index.md) Dokumentation.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Präsentieren Aussteller Karten in Apps

Mit iOS-10 haben neue Funktionen hinzugefügt, um PassKit-Framework, mit denen-Aussteller ihre Karten in ihren eigenen apps bereitstellen können. Entwickler kann Hinzufügen einer `PKPaymentButtonTypeInStore` UIButton für die app-Benutzeroberfläche, die eine Schaltfläche Apple Pay für eine Karte angezeigt werden.

Die `PresentPaymentPass` Methode der [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) Klasse kann auch verwendet werden, um programmgesteuert auf die Karte anzuzeigen.

### <a name="new-payment-network-support"></a>Neue Zahlung Network-Support

Neu für iOS 10, eine app unterstützen automatisch ein neues Netzwerk für die Zahlung ab, sobald diese ohne dass Entwickler zu ändern, neu kompilieren die app und erneut auf den App Store verfügbar ist.

Die neue [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) Methode der `PKPaymentNetwork` Klasse ermöglicht einer app, um die Netzwerke, die auf dem Gerät des Benutzers zur Laufzeit verfügbar zu ermitteln. Darüber hinaus die [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) Eigenschaft wurde erweitert, um die Zahlungsdienstanbieter Namen als Argument annehmen. Mit diesen Methoden kann eine app automatisch Netzwerkvorgänge unterstützen, die die Zahlungsdienstanbieter unterstützt.

Weitere Informationen finden Sie unter unsere [Apple bezahlen Konfiguration](~/ios/platform/apple-pay.md) und Apple [bezahlen Apple-Handbuch](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Neue Umgebung testen

Apple eingeführt mit iOS-10 eine neue Umgebung testen, die den Entwickler Test Zahlungskarten direkt auf einem iOS-Gerät bereitstellen kann. Diese neue Umgebung testen gibt verschlüsselte Zahlungsdaten zur app zurück.

Führen Sie folgende Schritte aus, um die neue Umgebung für Tests zu aktivieren:

1. Erstellen Sie einen neuen Test iCloud-Konto in iTunes Connect.
2. Melden Sie sich das iOS-Gerät mit dem neuen Testen des Kontos.
3. So testen Sie die app in die gewünschte Region fest
4. Verwenden Sie einen Test Zahlung Karten aus dem [bezahlen Apple-Handbuch](https://developer.apple.com/apple-pay/) Zahlungen vornehmen.

> [!IMPORTANT]
> Umschalten iCloud-Konten, schaltet das Gerät automatisch in die neue Umgebung testen. Apple jedoch weiterhin **erfordert** die app, die mit tatsächlichen getestet werden in einer produktiven Umgebung vor der Übermittlung im iTunes App Store-Karten.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel untersucht es die verschiedenen Elemente erforderlich, um Apple Pay innerhalb Ihrer app verwenden. Erläutert, wie eine Händler-ID zu erstellen und deren Verwendung in der **Entitlements.plist**, welche manuell geändert werden muss.

## <a name="related-links"></a>Verwandte Links

- [In-App-Käufe](~/ios/platform/in-app-purchasing/index.md)
- [Einführung in PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
