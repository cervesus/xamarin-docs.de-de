---
title: Apple Pay in Xamarin.iOS
description: Dieses Handbuch beschreibt das Einrichten der Xamarin.iOS-Umgebung für die Verwendung mit der Apple Pay zum bezahlen physischer Güter, z. B. Essen, Unterhaltung und Mitgliedschaften über Ihre app. Sie enthält Informationen zu den erforderlichen Bezeichner, die Zertifikate und die Berechtigungen.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/05/2017
ms.openlocfilehash: b971029ff3b2b1e8f5e63233d1d754c44b0e3309
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61346971"
---
# <a name="apple-pay-in-xamarinios"></a>Apple Pay in Xamarin.iOS

_Dieses Handbuch beschreibt das Einrichten der Xamarin.iOS-Umgebung für die Verwendung mit der Apple Pay zum bezahlen physischer Güter, z. B. Essen, Unterhaltung und Mitgliedschaften über Ihre app. Sie enthält Informationen zu den erforderlichen Bezeichner, die Zertifikate und die Berechtigungen._

Apple Pay, wurde zusammen mit iOS 8 eingeführt, Benutzern das bezahlen physischer Güter wie Futter, Unterhaltung und Mitgliedschaften über ihre iOS-Geräte aktivieren. Es ist verfügbar, auf dem iPhone 6 und iPhone 6 Plus, und Sie können auch mit der Apple Watch-Store-Einkäufe kombiniert werden. Wenn auf einem iPhone verwendet wird, verwendet er Touch ID als eine Möglichkeit, um zu bestätigen und Transaktionen eines Benutzers Kredit- oder Debitkarte zu autorisieren.

## <a name="requirements"></a>Anforderungen

Apple Pay ist nur verfügbar in iOS 8 und höher, und aus diesem Grund erfordert mindestens Xcode 6.

Die folgenden Elemente sind auch erforderlich, um Apple Pay in Ihrer app zu integrieren:

 - Zahlungsplattform-Prozessor
 - Händler-ID
 - Ein Apple Pay-Zertifikat
 - Apple Pay-Berechtigung

In diesem Dokument werden diese Elemente ausführlicher erläutert.

## <a name="differences-between-apple-pay-and-iap"></a>Unterschiede zwischen Apple Pay und IAP

Der Hauptunterschied zwischen Apple Pay und *In-App-Käufe* (IAP), bezieht sich auf die die Produkte, die sie verkaufen. *Physische* waren über Apple Pay verkauft werden, Essen, Unterbringung und physische Unterhaltung (z. B. Kino-Tickets) sind Beispiele für dieses. Im Gegensatz dazu IAP verkauft *virtuellen* waren, z. B. Premium oder Inhalte und -Abonnements – stellen Sie sich weitere Monate einen streamingdienst oder zusätzliche befindet sich in einem Spiel.

Die Frameworks verwendet, sind auch ein wichtiger Unterschied; [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) ist zwar für Apple Pay verwendet, [StoreKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) bietet das Framework-API für IAP.

Mit der Apple Pay, Apple [Zustände](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , dass es "nicht von Benutzern, Händlern oder Entwickler, mit Apple Pay für Zahlungen in Rechnung gestellt []". Im Gegensatz dazu weist IAP 30 % Gebühr für jede Transaktion an. Darüber hinaus mit der Apple Pay, die Transaktion geht nicht über Apple überhaupt übertrage, gelangt Sie stattdessen über eine zahlungsplattform.

## <a name="using-a-payment-processor-platform"></a>Verwenden Sie eine Zahlung-Prozessor-Plattform

Eine der grundlegenden Bestandteile von Apple Pay ist die Verarbeitung von Zahlungen. Es ist, zwar möglich, dies selbst übernehmen erfordert es wichtige Kenntnisse der Kryptografie
- finden Sie im Apple [Zahlungsabwicklung Handbuch](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html).
Zahlung Verarbeitung Plattformen verarbeiten auf der anderen Seite dieser Vorgänge für Sie, sodass Sie sich auf die Entwicklung Ihrer app konzentrieren.

Zwei Optionen umfassen:

- **Stripe** -melden Sie sich bei [Stripe.com](https://stripe.com/) auf ihre APIs zugreifen.

- **JudoPay** -sehen Sie sich ihre [Xamarin-Beispielcode auf Github](https://github.com/Judopay/Xamarin-Sample-App), und registrieren Sie sich auf [JudoPay.com](https://www.judopay.com/).

## <a name="provisioning-for-apple-pay"></a>Bereitstellung für Apple Pay

Zum Konfigurieren des einer app für Apple Pay verwenden, müssen Setup im Apple Developer Portal und in Ihrer app. Es gibt eine Reihe von Schritten, die befolgt werden sollten, um Ihre app für Apple Pay, erfolgreich bereitzustellen:

1. Erstellen Sie einen Händler-ID:
    - Führen Sie die Schritte [hier](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid)
2. Erstellen Sie eine App-ID mit der Apply Pay-Funktion, und fügen Sie den Händler hinzu:
    - Führen Sie die Schritte [hier](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid)
3. Generieren Sie ein Zertifikat für die Händler-ID:
    - Führen Sie die Schritte [hier](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate)
4. Generieren Sie ein Bereitstellungsprofil mit dem neu erstellten App-ID:
    - Führen Sie die Schritte [hier](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning)
5. Apple Pay Berechtigungen hinzufügen:
    - Wählen Sie die Apple Pay-Berechtigung wie [hier](~/ios/deploy-test/provisioning/entitlements.md), oder fügen Sie das Schlüssel/Wert-Paar manuell in die Datei aus [hier](~/ios/deploy-test/provisioning/entitlements.md)

## <a name="working-with-apple-pay"></a>Arbeiten mit Apple Pay

Apple hat mehrere Erweiterungen zu Apple Pay in iOS 10 vorgenommen, die der Benutzer auf sichere Zahlungen von Websites und durch die Interaktion mit Siri und Zuordnungen haben.

Mit iOS 10 wurden mehrere neue APIs mit iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-Test-Umgebung hinzugefügt.

### <a name="apple-pay-website-integration"></a>Apple Pay-Website-Integration

Neue IOS 10, Entwickler kann integrieren, Apple Pay direkt in ihre Websites mit **ApplePay JS**. Überprüfen die Transaktion auf dem iPhone oder Apple Watch können Benutzer Durchsuchen der Website mit Safari unter iOS oder MacOS Zahlungen mit Apple Pay vornehmen. Weitere Informationen finden Sie unter Apple [ApplePay JP Frameworkverweis](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>PassKit-Framework-Erweiterungen

In iOS 10, das PassKit-Framework wurde erweitert, um Unterstützung von Apple Pay außerhalb von `UIKit` und belastungsrisiken präsentieren Sie ihre eigenen Karten in ihren apps zu ermöglichen.


#### <a name="supporting-apple-pay-outside-of-uikit"></a>Unterstützung von Apple Pay außerhalb von UIKit

Mithilfe von [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), eine app kann die gleiche Funktionalität von bereitgestellten unterstützen [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) ohne Verwendung von UIKit. Während dieser neuen API für die Unterstützung von Apple Pay auf der Apple Watch (und in bestimmten Intents sowie) erforderlich ist, ist es in anderen Situationen (z. B. vorhandener apps) optional. Allerdings empfiehlt Apple verschieben so bald wie möglich auf die neue API für eine Breite Apple Pay Unterstützung in allen von der Entwickler-apps mit einer einzigen Codebasis. Weitere Informationen zu Intents und Siri-Integration, informieren Sie sich unsere [Einführung in SiriKit](~/ios/platform/sirikit/index.md) Dokumentation.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Darstellen von Aussteller Karten in Apps

Mit iOS 10 haben neue Features hinzugefügt wurden, auf dem PassKit-Framework, die belastungsrisiken präsentieren Sie ihre Karten in ihren eigenen apps zu ermöglichen. Entwickler kann Hinzufügen einer `PKPaymentButtonTypeInStore` UIButton an der app-Benutzeroberfläche, die eine Schaltfläche "Apple Pay" für eine Karte angezeigt werden.

Die `PresentPaymentPass` Methode der [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) Klasse kann auch verwendet werden, um programmgesteuert auf die Karte angezeigt.

### <a name="new-payment-network-support"></a>Neue Zahlungsmethode Netzwerkunterstützung

Neue IOS 10, eine app unterstützen automatisch ein neues Netzwerk für die Zahlung ab, wenn es ohne dass der Entwickler zu ändern, kompilieren Sie die app neu und erneut auf den App Store verfügbar wird.

Die neue [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) Methode der `PKPaymentNetwork` Klasse ermöglicht einer app aus, um die Netzwerke, die auf dem Gerät des Benutzers zur Laufzeit zu ermitteln. Darüber hinaus die [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) Eigenschaft wurde erweitert, um die Zahlung des Anbieters Namen als Argument annehmen. Mit diesen Methoden kann eine app automatisch jedes Netzwerk unterstützen, die die Zahlungsdienstanbieter unterstützt.

Weitere Informationen finden Sie unter unserem [Apple Zahlen Konfiguration](~/ios/platform/apple-pay.md) und Apple [Zahlen Apple-Handbuch](https://developer.apple.com/apple-pay/).

### <a name="new-testing-environment"></a>Neue Testumgebung

Mit iOS 10 eine neue testumgebung, die dem Entwickler erlaubt, Test-Zahlungskarten direkt auf einem iOS-Gerät bereitstellen, Apple eingeführt. Diese neue testumgebung gibt verschlüsselten-Testdaten für die Zahlung für die app zurück.

Führen Sie folgende Schritte aus, um die neue testumgebung zu aktivieren:

1. Erstellen Sie einen neuen Test iCloud-Konto in iTunes Connect.
2. Melden Sie sich das iOS-Gerät mit dem neuen Konto für Tests.
3. Legen Sie zum Testen der app in die gewünschte Region ein.
4. Verwenden Sie eine der der Test-Zahlungskarten über die [Zahlen Apple-Handbuch](https://developer.apple.com/apple-pay/) tätigen von Zahlungen.

> [!IMPORTANT]
> Durch den Wechsel von iCloud-Konten, wechselt das Gerät automatisch zur neuen Umgebung testen. Allerdings weiterhin von Apple **erfordert** die app mit echten getestet werden, können in einer produktionsumgebung vor der Übermittlung im iTunes App Store.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel vorgestellt, die die verschiedenen Elemente erforderlich, um Apple Pay in Ihrer app verwenden. Erläutert, wie Sie eine Händler-ID zu erstellen und deren Verwendung innerhalb der **"Entitlements.plist"**, dies muss manuell geändert werden.

## <a name="related-links"></a>Verwandte Links

- [In-App-Käufe](~/ios/platform/in-app-purchasing/index.md)
- [Einführung in PassKit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (Beispiel)](https://developer.xamarin.com/samples/monotouch/ios9/Emporium/)
