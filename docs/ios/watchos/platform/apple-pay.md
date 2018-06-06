---
title: Apple Pay auf WatchOS in Xamarin
description: Dieser Artikel behandelt die Erweiterungen Apple wurde gestellt Apple Pay WatchOS 3 und deren in Xamarin.iOS für Apple Watch-Implementierung.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 75d660ad0699b6fac3b1ae43046f322f380872b3
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791074"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Apple Pay auf WatchOS in Xamarin

Apple hat gestellt mehrere Erweiterungen Apple Pay WatchOS 3, die die Unterstützung für In-App-Zahlungen hinzugefügt. Dies ermöglicht es dem Benutzer für die sichere Zahlung bereitstellen und Kontaktinformationen für physischen waren und Dienstleistungen direkt aus dem Apple Watch-Zahlen.


## <a name="about-apple-pay-enhancements"></a>Welche Apple Pay-Erweiterungen

Als Stated oben verfügt über Apple verschiedene Verbesserungen an Apple Pay in WatchOS 3 vorgenommen, die für die sichere Zahlung zulassen und Kontaktinformationen für physischen waren und Dienstleistungen direkt aus dem Apple Watch-Zahlen. Diese Erweiterungen werden von Änderungen an der PassKit-Framework bereitgestellt.

Mit iOS 10 und WatchOS 3 wurden mehrere neue APIs hinzugefügt, arbeiten mit iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-testumgebung.

## <a name="passkit-framework-enhancements"></a>PassKit Framework-Erweiterungen

In iOS-10, wurde das PassKit-Framework zur Unterstützung von Apple Pay außerhalb von erweitert `UIKit` und -Aussteller an ihre Karten in ihren apps zu ermöglichen. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Unterstützung von Apple Pay außerhalb UIKit

Mithilfe von [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), eine app kann die gleiche Funktionalität von bereitgestellten unterstützen [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) ohne UIKit. Während die neue API für die Unterstützung von Apple Pay auf der Apple Watch (und in bestimmten Intents sowie) erforderlich ist, ist es in anderen Situationen (z. B. vorhandene apps) optional. Allerdings schlägt Apple Verschieben an die neue API so bald wie möglich mit einer einzelnen Codebasis umfassende Apple Pay-Unterstützung in allen von der Entwickler apps bereitstellen. Weitere Informationen zu Intents und Siri-Integration finden Sie in unserer [Einführung in SiriKit](~/ios/platform/sirikit/index.md) Dokumentation.

### <a name="presenting-issuer-cards-from-within-apps"></a>Präsentieren Aussteller Karten in Apps

Mit iOS 10 und WatchOS 3 wurden neue Funktionen hinzugefügt wurden, zu PassKit Framework, mit denen-Aussteller ihre Zahlungskarten in ihren eigenen apps bereitstellen können. Entwickler kann Hinzufügen einer `PKPaymentButtonTypeInStore` UIButton für die app-Benutzeroberfläche, die eine Schaltfläche Apple Pay für eine Karte angezeigt werden.

Die `PresentPaymentPass` Methode der [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) Klasse kann auch verwendet werden, um programmgesteuert auf die Karte anzuzeigen.

## <a name="new-payment-network-support"></a>Neue Zahlung Network-Support

Neue iOS 10 und WatchOS 3 eine app unterstützen automatisch ein neues Netzwerk für die Zahlung ab, wenn es zur Verfügung, ohne dass Entwickler steht zu ändern, neu kompilieren die app und erneut auf den App Store übermitteln.

Die neue [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) Methode der `PKPaymentNetwork` Klasse ermöglicht einer app, um die Netzwerke, die auf dem Gerät des Benutzers zur Laufzeit verfügbar zu ermitteln. Darüber hinaus die [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) Eigenschaft wurde erweitert, um die Zahlungsdienstanbieter Namen als Argument annehmen. Mit diesen Methoden kann eine app automatisch Netzwerkvorgänge unterstützen, die die Zahlungsdienstanbieter unterstützt.

Weitere Informationen finden Sie unter unsere [Apple bezahlen Konfiguration](~/ios/platform/apple-pay.md) und Apple [bezahlen Apple-Handbuch](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Neue Umgebung testen

Apple iOS 10 und WatchOS 3 eingeführt eine neue Umgebung testen, die den Entwickler Test Zahlungskarten direkt auf einem iOS-Gerät bereitstellen kann. Diese neue Umgebung testen gibt verschlüsselte Zahlungsdaten zur app zurück.

Führen Sie folgende Schritte aus, um die neue Umgebung für Tests zu aktivieren:

1. Erstellen Sie einen neuen Test iCloud-Konto in iTunes Connect.
2. Melden Sie sich das iOS-Gerät mit dem neuen Testen des Kontos.
3. So testen Sie die app in die gewünschte Region fest
4. Verwenden Sie einen Test Zahlung Karten aus dem [bezahlen Apple-Handbuch](https://developer.apple.com/apple-pay/) Zahlungen vornehmen.

> [!NOTE]
> Umschalten iCloud-Konten, schaltet das Gerät automatisch in die neue Umgebung testen. Apple jedoch weiterhin **erfordert** die app, die mit tatsächlichen getestet werden in einer produktiven Umgebung vor der Übermittlung im iTunes App Store-Karten.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt Apple wurde gestellt Apple Pay WatchOS 3 und wie diese in Xamarin.iOS implementiert.
