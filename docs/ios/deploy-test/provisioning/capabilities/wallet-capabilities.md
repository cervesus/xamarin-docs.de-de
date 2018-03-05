---
title: Wallet-Funktionen
description: "Um Funktionen zu einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für die Wallet-Funktionen erläutert."
ms.topic: article
ms.prod: xamarin
ms.assetid: BD9475E6-F586-488C-93D4-8A2A1629B99B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 952fa7dfa93c1dcb45eafe4eec08c73d2a8571eb
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="wallet-capabilities"></a>Wallet-Funktionen

_Um Funktionen einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für die Wallet-Funktionen erläutert._

Wallet ist eine App zum Speichern und Anzeigen von Barcodes und anderen Inhalten, mit der Benutzer Tickets, Bordkarten und Coupons direkt auf ihrem Gerät anzeigen können. Diese Informationen werden in einem _Pass_ gespeichert. Eine Bordkarte oder ein einzelnes Ticket stellen beispielsweise einen Einzelpass dar. 

Entwickler haben mit Wallet eine Vielzahl von Möglichkeiten:

*   Eine Anwendung muss nicht erstellt werden, um einen Pass zu erstellen. Eine Passdatei ist ein ZIP-Archiv, das mehrere JSON-Dateien und optionale Metadatendateien enthält. Dafür sind eine [Passtyp-ID](~/ios/platform/passkit.md) und ein [Passzertifikat](~/ios/platform/passkit.md) erforderlich. Diese Informationen werden anschließend in einer JSON-Datei deklariert. Weitere Informationen zur Bereitstellung einer Passdatei finden Sie im Leitfaden [Introduction to PassKit](~/ios/platform/passkit.md) (Einführung in PassKit).

*   Für die Verteilung der Pässe werden Begleit-Apps verwendet. Diese verfügen auch über Funktionen zum Erstellen, Bearbeiten und Aktualisieren von Pässen, die dann der Wallet-App hinzugefügt werden. Ein Beispiel für eine Begleit-App ist etwa eine Kino-App: Wenn ein Benutzer über die Kino-App eine Eintrittskarte kauft, kann sie direkt aus der App zu Wallet hinzugefügt werden. Damit Sie Begleit-Apps verwenden können, muss Ihr Bereitstellungsprofil eine App-ID mit den Wallet-Funktionen enthalten, die Sie mithilfe der nachfolgenden Schritte festlegen können. Die App muss zudem über die erforderlichen Berechtigungen verfügen.

*   Kanal-Apps sind Apps, die Pässe nicht direkt bearbeiten. Sie empfangen den Pass und ermöglichen dem Benutzer, ihn Wallet hinzuzufügen. Darüber hinaus interagieren sie mit dem Pass nur minimal. Eine spezielle Bereitstellung oder Berechtigungen sind dafür nicht erforderlich. Kanal-Apps verwenden jedoch einige Methoden aus dem PassKit-Framework.

## <a name="developer-center"></a>Developer Center

Führen Sie folgende Schritte aus, um ein neues Bereitstellungsprofil zur Verwendung mit Wallet zu erstellen:

1.  Navigieren Sie im Apple Developer Portal zum Abschnitt [Zertifikate, Bezeichner und Profile](https://developer.apple.com/account/ios/certificate/).
2.  Navigieren Sie unter **Bezeichner** zu **App-IDs**: 
    
    ![Auswählen der App-ID](wallet-capabilities-images/image17.png)

3.  Klicken Sie auf das **+**-Symbol rechts oben auf der Seite.
4.  Geben Sie einen **Namen** und einen Bündelbezeichner ein, um eine neue App-ID zu registrieren. (Beachten Sie, dass der Bündelbezeichner mit dem Bündelbezeichner in Ihrem Projekt übereinstimmen muss):
   
    ![Hinzufügen von App-ID-Details](wallet-capabilities-images/image18.png)

5.  Wählen Sie aus der Liste der Dienste den App Service **Wallet**aus:
    
    ![Bildschirm „Dienst auswählen“](wallet-capabilities-images/image19.png)

6.  Wählen Sie **Weiter** und anschließend **Registrieren** aus, um die App-ID zu erstellen.

Vorhandene App-IDs können gegebenenfalls bearbeitet werden, um die Wallet-Funktion hinzuzufügen.

Diese App-ID kann jetzt zum Generieren oder erneuten Generieren eines neuen Bereitstellungsprofils verwendet werden (siehe die Erläuterung in der Anleitung [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md)):

![Verwenden der neu erstellten App-ID zum Erstellen eines Bereitstellungsprofils](wallet-capabilities-images/image20.png)


Weitere Informationen zur Verwendung von Wallet finden Sie im folgenden Leitfaden:

*   [Introduction to PassKit](~/ios/platform/passkit.md) (Einführung in PassKit)
 
## <a name="next-steps"></a>Nächste Schritte
 
In der folgenden Liste werden mögliche weitere Schritte aufgeführt:

* Verwenden des Framework-Namespaces in Ihrer App
* Hinzufügen der erforderlichen Berechtigungen zu Ihrer App Informationen zu den erforderlichen Berechtigungen und wie sie hinzugefügt werden finden Sie im Leitfaden [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md).
* Stellen Sie im Bereich **iOS-Bündelsignierung** der App sicher, dass **Benutzerdefinierte Berechtigungen** auf **Entitlements.plist** festgelegt ist. Hierbei handelt es sich _nicht_ um die Standardeinstellung für Debug- und iOS-Simulatorbuilds.

Wenn Probleme mit App-Diensten auftreten, konsultieren Sie den Abschnitt [Problembehandlung](~/ios/deploy-test/provisioning/capabilities/index.md) in der Hauptanleitung.