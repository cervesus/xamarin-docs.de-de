---
title: Apple Pay auf WatchOS in Xamarin
description: Dieser Artikel behandelt die Verbesserungen Apple hat zu Apple Pay in WatchOS 3 und deren Implementierung in Xamarin.iOS für Apple Watch.
ms.prod: xamarin
ms.assetid: 32FF5D21-C252-485D-83AC-A7E592237962
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 354e03ee1e07ba99fcdeb05617bc65ed89f0e8c2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103087"
---
# <a name="apple-pay-on-watchos-in-xamarin"></a>Apple Pay auf WatchOS in Xamarin

Apple hat mehrere Erweiterungen zu Apple Pay in WatchOS 3, das Unterstützung für In-App-Zahlungen. Dadurch kann der Benutzer für die sichere stellen Zahlungsgatewaydienste bereit und wenden Sie sich an Informationen zum physischen waren und Dienstleistungen direkt von der Apple Watch-bezahlen.


## <a name="about-apple-pay-enhancements"></a>Zu den Verbesserungen für Apple Pay

Als Stated oben hat Apple mehrere Erweiterungen zu Apple Pay in WatchOS 3 vorgenommen, die Kontaktinformationen an für physische waren und Dienstleistungen direkt von der Apple Watch und sichere Bezahlung ermöglicht. Diese Verbesserungen werden durch Änderungen an dem PassKit-Framework bereitgestellt.

Mit iOS 10 und WatchOS 3 wurden verschiedene neue APIs mit iOS und WatchOS zur Unterstützung von dynamischen Zahlung Netzwerke und eine neue Sandkasten-Test-Umgebung hinzugefügt.

## <a name="passkit-framework-enhancements"></a>PassKit-Framework-Erweiterungen

In iOS 10, das PassKit-Framework wurde erweitert, um Unterstützung von Apple Pay außerhalb von `UIKit` und belastungsrisiken präsentieren Sie ihre Karten in ihren apps zu ermöglichen. 

### <a name="supporting-apple-pay-outside-of-uikit"></a>Unterstützung von Apple Pay außerhalb von UIKit

Mithilfe von [PKPaymentAuthorizationController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontroller) und [PKPaymentAuthorixationControllerDelegate](https://developer.apple.com/reference/passkit/pkpaymentauthorizationcontrollerdelegate), eine app kann die gleiche Funktionalität von bereitgestellten unterstützen [ PKPaymentAuthorizationViewController](https://developer.apple.com/reference/passkit/pkpaymentauthorizationviewcontroller) ohne Verwendung von UIKit. Während dieser neuen API für die Unterstützung von Apple Pay auf der Apple Watch (und in bestimmten Intents sowie) erforderlich ist, ist es in anderen Situationen (z. B. vorhandener apps) optional. Allerdings empfiehlt Apple verschieben so bald wie möglich auf die neue API für eine Breite Apple Pay Unterstützung in allen von der Entwickler-apps mit einer einzigen Codebasis. Weitere Informationen zu Intents und Siri-Integration, informieren Sie sich unsere [Einführung in SiriKit](~/ios/platform/sirikit/index.md) Dokumentation.

### <a name="presenting-issuer-cards-from-within-apps"></a>Darstellen von Aussteller Karten in Apps

Mit iOS 10 und WatchOS 3 wurden neue Features hinzugefügt, dem PassKit-Framework, die belastungsrisiken präsentieren Sie ihre Zahlungskarten in ihren eigenen apps zu ermöglichen. Entwickler kann Hinzufügen einer `PKPaymentButtonTypeInStore` UIButton an der app-Benutzeroberfläche, die eine Schaltfläche "Apple Pay" für eine Karte angezeigt werden.

Die `PresentPaymentPass` Methode der [PKPassLibrary](https://developer.apple.com/reference/passkit/pkpasslibrary) Klasse kann auch verwendet werden, um programmgesteuert auf die Karte angezeigt.

## <a name="new-payment-network-support"></a>Neue Zahlungsmethode Netzwerkunterstützung

Neue iOS 10 und WatchOS 3, eine app unterstützen automatisch ein neues Netzwerk für die Zahlung ab, wenn es ohne dass der Entwickler zu ändern, kompilieren Sie die app neu und erneut auf den App Store verfügbar wird.

Die neue [AvailableNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1833288-availablenetworks) Methode der `PKPaymentNetwork` Klasse ermöglicht einer app aus, um die Netzwerke, die auf dem Gerät des Benutzers zur Laufzeit zu ermitteln. Darüber hinaus die [SupportedNetworks](https://developer.apple.com/reference/passkit/pkpaymentrequest/1619329-supportednetworks) Eigenschaft wurde erweitert, um die Zahlung des Anbieters Namen als Argument annehmen. Mit diesen Methoden kann eine app automatisch jedes Netzwerk unterstützen, die die Zahlungsdienstanbieter unterstützt.

Weitere Informationen finden Sie unter unserem [Apple Zahlen Konfiguration](~/ios/platform/apple-pay.md) und Apple [Zahlen Apple-Handbuch](https://developer.apple.com/apple-pay/).

## <a name="new-testing-environment"></a>Neue Testumgebung

Mit iOS 10 und WatchOS 3 eingeführt Apple eine neue testumgebung, die dem Entwickler erlaubt, Test-Zahlungskarten direkt auf einem iOS-Gerät bereitstellen. Diese neue testumgebung gibt verschlüsselten-Testdaten für die Zahlung für die app zurück.

Führen Sie folgende Schritte aus, um die neue testumgebung zu aktivieren:

1. Erstellen Sie einen neuen Test iCloud-Konto in iTunes Connect.
2. Melden Sie sich das iOS-Gerät mit dem neuen Konto für Tests.
3. Legen Sie zum Testen der app in die gewünschte Region ein.
4. Verwenden Sie eine der der Test-Zahlungskarten über die [Zahlen Apple-Handbuch](https://developer.apple.com/apple-pay/) tätigen von Zahlungen.

> [!NOTE]
> Durch den Wechsel von iCloud-Konten, wechselt das Gerät automatisch zur neuen Umgebung testen. Allerdings weiterhin von Apple **erfordert** die app mit echten getestet werden, können in einer produktionsumgebung vor der Übermittlung im iTunes App Store.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt Apple hat zu Apple Pay in WatchOS 3 und wie Sie sie in Xamarin.iOS zu implementieren.
