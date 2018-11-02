---
title: Apple Pay-Funktionen in Xamarin.iOS
description: Um Funktionen zu einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für die Apple Pay-Funktionen erläutert.
ms.prod: xamarin
ms.assetid: 735CC916-16A4-471B-87F7-0535E24288D7
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/15/2017
ms.openlocfilehash: c7a2d347970d4edfe713edab264647fb644ff74a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112343"
---
# <a name="apple-pay-capabilities-in-xamarinios"></a>Apple Pay-Funktionen in Xamarin.iOS

_Um Funktionen einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für die Apple Pay-Funktionen erläutert._

Apple Pay ermöglicht Benutzern das Bezahlen physischer Güter über das iOS-Gerät. Dieser Abschnitt beschreibt, wie alle erforderlichen Komponenten für Apple Pay in Apple Developer Center erstellt werden.

Bei der Bereitstellung einer neuen Anwendung über das Developer Center gibt es drei Schritte, die ausgeführt werden müssen:

1.  Erstellen Sie eine Händler-ID.
2.  Erstellen Sie eine App-ID mithilfe der Apply Pay-Funktion, und fügen Sie den Händler hinzu.
3.  Generieren Sie ein Zertifikat für die Händler-ID.

Die folgenden Schritte führen Sie durch die Erstellung der oben genannten Elemente:

<a name="merchantid" />

## <a name="create-merchant-id"></a>Erstellen der Händler-ID

Eine Händler-ID wird verwendet, um Apple Pay wissen zu lassen, dass Sie Zahlungen akzeptieren können. Sie wird an PassKits `PaymentRequest`-Methode übergeben und in der Apple Pay-Berechtigung verwendet:

1.  Navigieren Sie in das [Apple Developer Center](https://developer.apple.com/account/), und navigieren Sie zum Abschnitt „Zertifikate“, „Bezeichner“ und „Profile“: 
 
    ![Auswahl der Händler-ID im Developer Center](apple-pay-capabilities-images/image57.png)

2.  Wählen Sie unter **Bezeichner** die **Händler-IDs** aus, und wählen Sie dann **+** aus, um eine neue Händler-ID zu erstellen:  

3.  Füllen Sie das Formular, wie unten angegeben, mit einer neuen Beschreibung und einem Bezeichner aus. Durch die Beschreibung kann die ID von Ihnen identifiziert und später geändert werden. Der Bezeichner muss für Sie eindeutig sein, und er muss mit der Zeichenfolge  `merchant` beginnen. Apple empfiehlt, dass der Bezeichner im folgenden Format dargestellt wird: `merchant.com.[Your-App-Name]`:
   
    ![Details der neuen Händler-ID](apple-pay-capabilities-images/image58.png)

4.  Bestätigen Sie die Details, und  **Registrieren**  Sie Ihre ID: 
    
    ![Bestätigung der Händler-ID](apple-pay-capabilities-images/image59.png)

<a name="appid" />

## <a name="create-an-app-id-with-the-apple-pay-capability-that-includes-the-merchant-id"></a>Erstellen einer App-ID mit der Apple Pay-Funktion, die die Händler-ID enthält

1.  Klicken Sie im [Developer Center](https://developer.apple.com/account/) auf die **App-IDs** unter **Bezeichner**: 
    
    ![Auswählen der App-ID im Developer Center](apple-pay-capabilities-images/image6.png)

2.  Wählen Sie die **+**-Schaltfläche zum Hinzufügen einer neuen App-ID aus: 
   
    ![Hinzufügen einer neuen App-ID-Schaltfläche](apple-pay-capabilities-images/image27.png)

3.  Geben Sie einen Namen für die App-ID und eine explizite App-ID an:    
   
    ![Bildschirm „App-ID-Details“ ](apple-pay-capabilities-images/image35.png)

4.  Wählen Sie unter App-Dienste Apple Pay aus:    
  
    ![App-Dienste Apple Pay](apple-pay-capabilities-images/image36.png)

5.  Wählen Sie **Continue** (Weiter) und dann **Register** (Registrieren) aus. Beachten Sie, dass Apple Pay auf dem Bestätigungsbildschirm als konfigurierbar aktiviert ist und ein gelbes Symbol angezeigt wird: 
   
    ![Bestätigungsbildschirm für Apple Pay](apple-pay-capabilities-images/image37.png)

6.  Wechseln Sie zurück zur Liste der App-IDs, und wählen Sie diejenige aus, die Sie gerade erstellt haben:  
   
    ![Bearbeiten der App-ID](apple-pay-capabilities-images/image38.png)

7.  Führen Sie einen Bildlauf zum unteren Rand dieses erweiterten Abschnitts aus, und klicken Sie auf **Bearbeiten**.
8.  Scrollen Sie nach unten zu Apple Pay, und klicken Sie auf die Schaltfläche **Bearbeiten**:  
    
    ![Bearbeiten der Details für Apple Pay-App-ID](apple-pay-capabilities-images/image39.png)

9.  Wählen Sie die Händler-ID aus, die mit dieser App-ID verwendet wird, und klicken Sie auf **Fortfahren**:  
    
    ![Auswählen der Händler-ID für die App-ID](apple-pay-capabilities-images/image40.png)

10. Bestätigen Sie die Zuweisungen der Händler-ID, und klicken Sie auf **Zuweisen**:  
    
    ![Bestätigungsbildschirm](apple-pay-capabilities-images/image41.png)

Diese App-ID kann jetzt zum Generieren oder erneuten Generieren eines neuen Bereitstellungsprofils verwendet werden (siehe die Erläuterung in der Anleitung [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md)). 

<a name="certificate" />

## <a name="create-a-certificate-for-your-merchant-id"></a>Erstellen eines Zertifikats für die Händler-ID

Apple erfordert ein Zertifikat zum Verschlüsseln vertraulicher Daten, die der Transaktion zugeordnet sind. Jede erstellte Händler-ID muss über ein eigenes Zertifikat verfügen. 

Führen Sie die folgenden Schritte aus, um ein Zertifikat zu erstellen:

1.  Wählen Sie die Händler-ID aus, die oben erstellt wurde, und drücken Sie **Bearbeiten**: 
    
    ![Bearbeiten des Händler-ID-Dialogfelds](apple-pay-capabilities-images/image42.png)

2.  Klicken Sie auf dem iOS-Bildschirm der Einstellungen der Händler-ID auf **Zertifikat erstellen**: 
   
    ![Erstellen des Zahlungsverarbeitungszertifikats](apple-pay-capabilities-images/image43.png)

3.  Beantworten Sie die folgende Frage: 

    ![Angabe, ob Zahlungen ausschließlich in China verarbeitet werden](apple-pay-capabilities-images/image44.png)

4.  An diesem Punkt werden Sie aufgefordert, eine _Zertifikatsignieranforderung_ zu erstellen: 

    ![Erstellen einer Zertifikatsignieranforderung (CSR)](apple-pay-capabilities-images/image45.png)
    
    > [!IMPORTANT]
    > Bei Verwendung eines Zahlungsanbieters für Apple Pay, wie JudoPay oder Stripe, können Sie eine ordnungsgemäß formatierte CSR anfordern, die Sie an diesem Punkt verwenden können. Informationen zu dieser Anforderung befinden sich auf den Websites [JudoPay](https://www.judopay.com/docs/version-52/apple-pay/getting-started/#create-an-apple-pay-certificate) und [Stripe](https://stripe.com/docs/apple-pay/apps#csr). Führen Sie die Schritte 5 bis 8 unten aus, um Ihre eigene CSR zu erstellen. Sobald Sie über eine CSR verfügen, fahren Sie mit Schritt 9 fort.

5.  Öffnen Sie die Anwendung „Keychain-Zugriff“, und navigieren Sie zu **Keychain-Zugriff > Certificate Assistant (Zertifikatassistent) > Request a Certificate from a Certificate Authority (Ein Zertifikat von einer Zertifizierungsstelle anfordern):** 

     ![Erstellen einer Zertifikatsignieranforderung mithilfe einer Keychain auf einem Mac](apple-pay-capabilities-images/image46.png)

6.  Geben Sie Ihre E-Mail-Adresse und einen Namen für den privaten Schlüssel ein, lassen Sie die CA-E-Mail-Adresse leer, wählen Sie die Option **Auf Datenträger speichern** aus, und wählen Sie **Let me specify key pair information** (Schlüsselpaarinformationen angeben) aus:

     ![Dialogfeld „Zertifikatinformationen“](apple-pay-capabilities-images/image47.png)

7.  Speichern Sie die CSR an einem geeigneten Speicherort: 

     ![Speichern der CSR auf einem lokalen Computer](apple-pay-capabilities-images/image48.png)

8.  Legen Sie im Informationsbildschirm „Schlüsselpaar“ die **Schlüsselgröße** auf **256 Bits** und den **Algorithmus** auf **ECC** fest, und klicken Sie auf **Fortfahren**:

     ![Dialogfeld „Schlüsselpaarinformationen angeben“](apple-pay-capabilities-images/image49.png)

9.  Klicken Sie im Developer Center auf **Fortfahren**, um die CSR hochzuladen: 

     ![Hochladen von CSR in Developer Center](apple-pay-capabilities-images/image50.png)

10. Klicken Sie auf **Datei auswählen...**, um die CSR auszuwählen, und auf **Fortfahren**, um sie in das Entwicklerportal hochzuladen: 

     ![Hochladen der CSR in Developer Center](apple-pay-capabilities-images/image51.png)

11. Sobald das Zertifikat generiert wurde, laden Sie es herunter. Klicken Sie zweimal darauf, um es auf Ihrer Keychain zu installieren.

Weitere Informationen zur Verwendung von Apple Pay finden Sie im folgenden Handbuch:

*   [Introduction to Apple Pay (Einführung in Apple Pay)](~/ios/platform/apple-pay.md)

## <a name="next-steps"></a>Nächste Schritte
 
In der folgenden Liste werden mögliche weitere Schritte aufgeführt:

* Verwenden des Framework-Namespaces in Ihrer App
* Hinzufügen der erforderlichen Berechtigungen zu Ihrer App Informationen zu den erforderlichen Berechtigungen und wie sie hinzugefügt werden finden Sie im Leitfaden [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md).
* Stellen Sie im Bereich  **iOS-Bundle-Signierung** der App sicher, dass  **Benutzerdefinierte Berechtigungen** auf **Entitlements.plist** festgelegt ist. Hierbei handelt es sich  _nicht_  um die Standardeinstellung für Debug- und iOS-Simulatorbuilds.

Wenn Probleme mit App-Diensten auftreten, konsultieren Sie den Abschnitt [Problembehandlung](~/ios/deploy-test/provisioning/capabilities/index.md) in der Hauptanleitung.