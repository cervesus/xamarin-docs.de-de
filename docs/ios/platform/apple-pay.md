---
title: Apple Pay in xamarin. IOS
description: In dieser Anleitung wird das Einrichten der xamarin. IOS-Umgebung für die Verwendung mit Apple Pay erläutert, um für physische waren, wie z. b. Nahrungsmittel, Unterhaltung und Mitgliedschaften, über Ihre APP zu bezahlen. Sie enthält Informationen zu den erforderlichen bezeichgern, Zertifikaten und Berechtigungen.
ms.prod: xamarin
ms.assetid: A25AE660-B145-465F-9CCE-8D82BFD614C6
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/05/2017
ms.openlocfilehash: 1d9a65ab34cb0c02368f53679d38f1d07ec1f257
ms.sourcegitcommit: 76f930ce63b193ca3f7f85f768b031e59cb342ec
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/23/2019
ms.locfileid: "71198556"
---
# <a name="apple-pay-in-xamarinios"></a>Apple Pay in xamarin. IOS

_In dieser Anleitung wird das Einrichten der xamarin. IOS-Umgebung für die Verwendung mit Apple Pay erläutert, um für physische waren, wie z. b. Nahrungsmittel, Unterhaltung und Mitgliedschaften, über Ihre APP zu bezahlen. Sie enthält Informationen zu den erforderlichen bezeichgern, Zertifikaten und Berechtigungen._

Apple Pay wurde zusammen mit IOS 8 eingeführt, sodass Benutzer physische waren wie Nahrungsmittel, Unterhaltung und Mitgliedschaften über Ihre IOS-Geräte bezahlen können. Es ist auf iPhone 6 und iPhone 6 Plus verfügbar und kann auch mit dem Apple Watch für Einkäufe im Store gekoppelt werden. Bei Verwendung auf einem iPhone wird die Berührungs-ID verwendet, um Transaktionen für die Kredit-oder Debitkarte eines Benutzers zu bestätigen und zu autorisieren.

## <a name="requirements"></a>Anforderungen

Apple Pay ist nur in ios 8 und höher verfügbar und erfordert daher mindestens Xcode 6.

Die folgenden Elemente sind ebenfalls erforderlich, um Apple Pay in Ihre APP zu integrieren:

- Plattform für den Zahlungsprozessor
- Händler Bezeichner
- Ein Apple Pay Zertifikat
- Apple Pay Berechtigung

In diesem Dokument werden diese Elemente ausführlicher erläutert.

## <a name="differences-between-apple-pay-and-iap"></a>Unterschiede zwischen Apple Pay und IAP

Der primäre Unterschied zwischen Apple Pay und *in-App-Einkauf* (IAP) bezieht sich auf die Produkte, die Sie verkaufen. *Physische* waren werden über Apple Pay verkauft. Essen, Unterhaltung und physische Unterhaltung (z. b. Kinotickets) sind Beispiele hierfür. Im Gegensatz dazu werden von IAP *virtuelle* waren (z. b. Premium-oder zusätzliche Inhalte) und Abonnements verkauft – weitere Monate eines Streamingdiensts oder ein zusätzliches Leben in einem Spiel.

Die verwendeten Frameworks sind auch ein wichtiger Unterschied. [Passkit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) wird für Apple Pay verwendet, während [storekit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/) die Framework-API für IAP bereitstellt.

Mit Apple Pay gibt Apple an, dass [es den Benutzern](https://developer.apple.com/apple-pay/Getting-Started-with-Apple-Pay.pdf) , Händlern oder Entwicklern nicht berechnet, Apple Pay für Zahlungen zu verwenden. Im Vergleich dazu hat IAP für jede Transaktion eine Abrechnung von 30%. Außerdem wird bei Apple Pay die Transaktion überhaupt nicht durch Apple durchlaufen, sondern eine Zahlungsplattform.

## <a name="using-a-payment-processor-platform"></a>Verwenden einer Payment Processor-Plattform

Einer der grundlegenden Bestandteile von Apple Pay ist die Verarbeitung von Zahlungen. Obwohl es möglich ist, dies selbst zu tun, erfordert es bedeutende Kenntnisse der Kryptografie, wie im Leitfaden zur [Zahlungs Verarbeitung](https://developer.apple.com/library/ios/ApplePay_Guide/ProcessPayment.html)von Apple ausführlich beschrieben.
Zahlungs Verarbeitungs Plattformen hingegen verarbeiten diese Vorgänge für Sie, sodass Sie sich auf das Entwickeln Ihrer APP konzentrieren können.

Es gibt zwei Optionen:

- **Stripe** : Registrieren Sie sich bei [Stripe.com](https://stripe.com/) für den Zugriff auf Ihre APIs.

- **Judopay** : sehen Sie sich Ihren [xamarin-Beispielcode auf GitHub an](https://github.com/Judopay/Xamarin-Sample-App), und registrieren Sie sich unter [JudoPay.com](https://www.judopay.com/).

## <a name="provisioning-for-apple-pay"></a>Bereitstellung für Apple Pay

Zum Konfigurieren einer APP für die Verwendung von Apple Pay ist das Setup im Apple Developer Portal und in Ihrer APP erforderlich. Es gibt eine Reihe von Schritten, die zum erfolgreichen Bereitstellen Ihrer APP für Apple Pay befolgt werden sollten:

1. Erstellen Sie eine Händler-ID:
    - Führen Sie die [folgenden Schritte aus](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#merchantid) .
2. Erstellen Sie eine APP-ID mit der Apply Pay-Funktion, und fügen Sie den Händler hinzu:
    - Führen Sie die [folgenden Schritte aus](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#appid) .
3. Generieren Sie ein Zertifikat für die Händler-ID:
    - Führen Sie die [folgenden Schritte aus](~/ios/deploy-test/provisioning/capabilities/apple-pay-capabilities.md#certificate) .
4. Generieren Sie ein Bereitstellungs Profil mit der neu erstellten APP-ID:
    - Führen Sie die [folgenden Schritte aus](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning) .
5. Apple Pay Berechtigungen hinzufügen:
    - Wählen Sie die Apple Pay-Berechtigung wie [hier](~/ios/deploy-test/provisioning/entitlements.md) [beschrieben aus,](~/ios/deploy-test/provisioning/entitlements.md) oder fügen Sie das Schlüssel-Wert-Paar manuell der Datei hinzu.

## <a name="working-with-apple-pay"></a>Arbeiten mit Apple Pay

Apple hat mehrere Verbesserungen an Apple Pay in ios 10 vorgenommen, mit denen der Benutzer sichere Zahlungen von Websites und durch Interaktion mit Siri und Maps durchführen kann.

Mit IOS 10 wurden mehrere neue APIs hinzugefügt, die mit IOS und watchos funktionieren, um dynamische Zahlungs Netzwerke und eine neue Sandkasten Testumgebung zu unterstützen.

### <a name="apple-pay-website-integration"></a>Integration der Apple Pay-Website

Neu bei IOS 10, kann der Entwickler Apple Pay mithilfe von **applepay js**direkt in seine Websites integrieren. Benutzer, die die Website mit Safari in ios oder macOS durchsuchen, können Apple Pay durch Überprüfen der Transaktion auf ihren iPhones oder Apple Watch bezahlen. Weitere Informationen finden Sie in der Apple- [Referenz für den applepay-JP-Framework](https://developer.apple.com/reference/applepayjs).

### <a name="passkit-framework-enhancements"></a>Passkit-Framework-Erweiterungen

In ios 10 wurde das passkit-Framework erweitert, um Apple Pay außerhalb von `UIKit` und zu unterstützen, damit Kartenaussteller ihre eigenen Karten innerhalb Ihrer Apps präsentieren können.

#### <a name="supporting-apple-pay-outside-of-uikit"></a>Unterstützung von Apple Pay außerhalb von UIKit

Durch die Verwendung von [pkpaymentauthorizationcontroller](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [pkpaymentautorisierungscontrollerdelegaten](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)kann eine APP die gleiche Funktionalität unterstützen, die von [pkpaymentauthorizationviewcontroller](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bereitgestellt wird, ohne dass UIKit verwendet wird. Diese neue API ist zwar für die Unterstützung von Apple Pay auf der Apple Watch erforderlich (und auch in bestimmten Intents), ist aber in anderen Situationen (z. b. vorhandenen apps) optional. Apple empfiehlt jedoch, so bald wie möglich auf die neue API zu wechseln, um eine umfassende Apple Pay Unterstützung für alle Entwickler Anwendungen mit einer einzigen Codebasis bereitzustellen. Weitere Informationen zur Intents-und Siri-Integration finden Sie [in der Einführung in die Sirikit](~/ios/platform/sirikit/index.md) -Dokumentation.

#### <a name="presenting-issuer-cards-from-within-apps"></a>Präsentieren von Aussteller Karten innerhalb von apps

Mit IOS 10 wurden neue Features zum passkit-Framework hinzugefügt, mit denen Kartenaussteller Ihre Karten in ihren eigenen apps präsentieren können. Der Entwickler kann der Benutzer `PKPaymentButtonTypeInStore` Oberfläche der APP einen UIButton hinzufügen, der eine Apple Pay Schaltfläche für eine Karte anzeigt.

Die `PresentPaymentPass` -Methode der [pkpasslibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) -Klasse kann auch verwendet werden, um die Karte Programm gesteuert anzuzeigen.

### <a name="new-payment-network-support"></a>Unterstützung für neue Zahlungs Netzwerke

Neu bei IOS 10. eine APP kann automatisch ein neues Zahlungsnetzwerk unterstützen, wenn Sie verfügbar wird, ohne dass der Entwickler die APP ändern, neu kompilieren und erneut an den App Store übermitteln muss.

Mit der neuen [availablenetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) -Methode `PKPaymentNetwork` der-Klasse kann eine APP die Netzwerke ermitteln, die auf dem Gerät des Benutzers zur Laufzeit verfügbar sind. Außerdem wurde die [supportednetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) -Eigenschaft erweitert, um den Namen des Zahlungsanbieters als Argument zu verwenden. Mithilfe dieser Methoden kann eine APP automatisch jedes Netzwerk unterstützen, das der Zahlungsanbieter unterstützt.

Weitere Informationen finden Sie in unserer [Apple Pay-Konfiguration](~/ios/platform/apple-pay.md) und in der [Apple Pay Anleitung](https://developer.apple.com/apple-pay/)von Apple.

### <a name="new-testing-environment"></a>Neue Testumgebung

Mit IOS 10 hat Apple eine neue Testumgebung eingeführt, mit der Entwickler Test Zahlungskarten direkt auf einem IOS-Gerät bereitstellen können. Diese neue Testumgebung gibt dann verschlüsselte Test Zahlungsdaten an die APP zurück.

Gehen Sie folgendermaßen vor, um die neue Testumgebung zu aktivieren:

1. Erstellen Sie ein neues icloud-Test Konto in iTunes Connect.
2. Melden Sie sich mit dem neuen Testkonto beim IOS-Gerät an.
3. Legen Sie die gewünschte Region fest, um die APP zu testen.
4. Verwenden Sie eine der Test Zahlungskarten aus dem [Apple Pay Handbuch](https://developer.apple.com/apple-pay/) , um Zahlungen zu tätigen.

> [!IMPORTANT]
> Wenn Sie icloud-Konten wechseln, wechselt das Gerät automatisch zur neuen Testumgebung. Apple **verlangt** jedoch weiterhin, dass die APP mit echten Karten in einer Produktionsumgebung getestet wird, bevor Sie Sie an den iTunes App Store übermitteln.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die verschiedenen Elemente untersucht, die für die Verwendung von Apple Pay in Ihrer APP erforderlich sind. Wir haben uns mit dem Erstellen einer Händler-ID und der Verwendung in der " **Berechtigungen. plist**" beschäftigt, die manuell geändert werden muss.

## <a name="related-links"></a>Verwandte Links

- [In-App-Käufe](~/ios/platform/in-app-purchasing/index.md)
- [Einführung in passkit](~/ios/platform/passkit.md)
- [PassKit](https://developer.apple.com/library/ios/documentation/PassKit/Reference/PKPaymentAuthorizationViewController_Ref/)
- [Apple Pay](https://developer.apple.com/apple-pay/)
- [Emporium (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-emporium)
