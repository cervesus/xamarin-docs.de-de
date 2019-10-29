---
title: Apple Pay auf watchos in xamarin
description: In diesem Artikel werden die Verbesserungen erläutert, die Apple zum Apple Pay in watchos 3 vorgenommen hat, und wie diese in xamarin. IOS für Apple Watch implementiert werden.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: 372b034b7e14f3cfaadde8fe5a5370e368f161db
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73030121"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Apple Pay auf watchos in xamarin

Apple hat mehrere Verbesserungen an Apple Pay in watchos 3 vorgenommen, die Unterstützung für in-App-Zahlungen bietet. Dadurch kann der Benutzer auf sichere Weise Zahlungs-und Kontaktinformationen bereitstellen, um physische waren und Dienste direkt aus der Apple Watch zu bezahlen.

## <a name="about-apple-pay-enhancements"></a>Informationen zu Apple Pay Erweiterungen

Wie bereits erwähnt, hat Apple mehrere Verbesserungen an Apple Pay in watchos 3 vorgenommen, die eine sichere Zahlung und Kontaktinformationen ermöglichen, um die physischen waren und Dienste direkt vom Apple Watch zu bezahlen. Diese Verbesserungen werden durch Änderungen am passkit-Framework bereitgestellt.

Mit IOS 10 und watchos 3 wurden mehrere neue APIs hinzugefügt, die mit IOS und watchos funktionieren, um dynamische Zahlungs Netzwerke und eine neue Sandkasten Testumgebung zu unterstützen.

## <a name="passkit-framework-enhancements"></a>Passkit-Framework-Erweiterungen

In ios 10 wurde das passkit-Framework erweitert, um Apple Pay außerhalb `UIKit` zu unterstützen, und damit Kartenaussteller Ihre Karten innerhalb Ihrer Apps präsentieren können. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Unterstützung von Apple Pay außerhalb von UIKit

Durch die Verwendung von [pkpaymentauthorizationcontroller](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [pkpaymentautorisierungscontrollerdelegaten](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate)kann eine APP die gleiche Funktionalität unterstützen, die von [pkpaymentauthorizationviewcontroller](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) bereitgestellt wird, ohne dass UIKit verwendet wird. Diese neue API ist zwar für die Unterstützung von Apple Pay auf der Apple Watch erforderlich (und auch in bestimmten Intents), ist aber in anderen Situationen (z. b. vorhandenen apps) optional. Apple empfiehlt jedoch, so bald wie möglich auf die neue API zu wechseln, um eine umfassende Apple Pay Unterstützung für alle Entwickler Anwendungen mit einer einzigen Codebasis bereitzustellen. Weitere Informationen zur Intents-und Siri-Integration finden Sie [in der Einführung in die Sirikit](~/ios/platform/sirikit/index.md) -Dokumentation.

### <a name="presenting-issuer-cards-from-within-apps"></a>Präsentieren von Aussteller Karten innerhalb von apps

Mit IOS 10 und watchos 3 wurden neue Features zum passkit-Framework hinzugefügt, mit denen Kartenaussteller ihre Zahlungskarten in ihren eigenen apps präsentieren können. Der Entwickler kann der Benutzeroberfläche der App eine `PKPaymentButtonTypeInStore` UIButton hinzufügen, die eine Schaltfläche Apple Pay für eine Karte anzeigt.

Die `PresentPaymentPass`-Methode der [pkpasslibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) -Klasse kann auch verwendet werden, um die Karte Programm gesteuert anzuzeigen.

## <a name="new-payment-network-support"></a>Unterstützung für neue Zahlungs Netzwerke

Neu bei IOS 10 und watchos 3: eine APP kann automatisch ein neues Zahlungsnetzwerk unterstützen, wenn Sie verfügbar wird, ohne dass der Entwickler die APP ändern, neu kompilieren und erneut an den App Store übermitteln muss.

Mit der neuen [availablenetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) -Methode der `PKPaymentNetwork`-Klasse kann eine APP die Netzwerke ermitteln, die auf dem Gerät des Benutzers zur Laufzeit verfügbar sind. Außerdem wurde die [supportednetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) -Eigenschaft erweitert, um den Namen des Zahlungsanbieters als Argument zu verwenden. Mithilfe dieser Methoden kann eine APP automatisch jedes Netzwerk unterstützen, das der Zahlungsanbieter unterstützt.

Weitere Informationen finden Sie in unserer [Apple Pay-Konfiguration](~/ios/platform/apple-pay.md) und in der [Apple Pay Anleitung](https://developer.apple.com/apple-pay/)von Apple.

## <a name="new-testing-environment"></a>Neue Testumgebung

Mit IOS 10 und watchos 3 hat Apple eine neue Testumgebung eingeführt, die es dem Entwickler ermöglicht, Test Zahlungskarten direkt auf einem IOS-Gerät bereitzustellen. Diese neue Testumgebung gibt dann verschlüsselte Test Zahlungsdaten an die APP zurück.

Gehen Sie folgendermaßen vor, um die neue Testumgebung zu aktivieren:

1. Erstellen Sie ein neues icloud-Test Konto in iTunes Connect.
2. Melden Sie sich mit dem neuen Testkonto beim IOS-Gerät an.
3. Legen Sie die gewünschte Region fest, um die APP zu testen.
4. Verwenden Sie eine der Test Zahlungskarten aus dem [Apple Pay Handbuch](https://developer.apple.com/apple-pay/) , um Zahlungen zu tätigen.

> [!NOTE]
> Wenn Sie icloud-Konten wechseln, wechselt das Gerät automatisch zur neuen Testumgebung. Apple **verlangt** jedoch weiterhin, dass die APP mit echten Karten in einer Produktionsumgebung getestet wird, bevor Sie Sie an den iTunes App Store übermitteln.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen erläutert, die Apple zum Apple Pay in watchos 3 gemacht hat und wie Sie in xamarin. IOS implementiert werden.
