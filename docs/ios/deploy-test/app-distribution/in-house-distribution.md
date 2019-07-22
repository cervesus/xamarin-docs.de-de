---
title: Interne Verteilung für Xamarin.iOS-Apps
description: Dieses Dokument bietet einen kurzen Überblick über die interne Verteilung von Anwendungen als Mitglied des Enterprise Developer Program von Apple.
ms.prod: xamarin
ms.assetid: 9466E51E-303E-466E-85D7-D0525E16BB37
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: a2dc974fba7beb70f1caecf8198181193d32e4c2
ms.sourcegitcommit: 7ccc7a9223cd1d3c42cd03ddfc28050a8ea776c2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/13/2019
ms.locfileid: "67865531"
---
# <a name="in-house-distribution-for-xamarinios-apps"></a>Interne Verteilung für Xamarin.iOS-Apps

_Dieses Dokument bietet einen kurzen Überblick über die interne Verteilung von Anwendungen als Mitglied des Enterprise Developer Program von Apple._

Nachdem Ihre Xamarin.iOS-Anwendung entwickelt wurde, ist der nächste Schritt im Lebenszyklus der Softwareentwicklung die Verteilung Ihrer Anwendung an Benutzer. Proprietäre Anwendungen können *intern* (früher als „Enterprise“ bezeichnet) verteilt werden. Sie wurden über das **Apple Developer Enterprise Program** verteilt, welches folgende Vorteile bietet:

- Die Anwendung muss nicht zur Überprüfung durch Apple übermittelt werden.
- Es gibt keine Beschränkungen für die Anzahl an Geräten, auf denen Sie eine Anwendung bereitstellen können
    - Beachten Sie, dass Apple deutlich macht, dass interne Anwendungen nur für die interne Verwendung bestimmt sind.

Es ist auch wichtig zu beachten, dass die Enterprise-Anwendung

- keinen Zugriff auf iTunes Connect für Verteilungs- oder Testvorgänge (einschließlich TestFlight) bietet.
- 299 $ pro Abonnement pro Jahr kostet.

Alle Anwendungen müssen von Apple signiert sein.

<a name="testing" />

## <a name="testing-your-application"></a>Testen der Anwendung

Das Testen der Anwendung erfolgt mithilfe der Ad-hoc-Verteilung. Weitere Informationen zum Testen finden Sie im Leitfaden [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md). Denken Sie daran, dass Sie nur maximal auf 100 Geräten testen können.

<a name="setup" />

## <a name="getting-set-up-for-distribution"></a>Einrichten für die Verteilung

Wie bei anderen Apple Developer-Programmen können unter dem Developer Enterprise Program von Apple nur Teamadministratoren und Agents Verteilungszertifikate und Bereitstellungsprofile erstellen.

Zertifikate von Apple Developer Enterprise Program sind drei Jahre gültig, und Bereitstellungsprofile laufen nach einem Jahr ab.

Es ist wichtig, zu beachten, dass abgelaufene Zertifikate nicht erneuert werden können, und Sie stattdessen – wie [unten](#certificate) ausführlich beschrieben – das abgelaufene Zertifikat durch ein neues ersetzen müssen.

<a name="certificate" />

## <a name="creating-a-distribution-certificate"></a>Erstellen eines Verteilungszertifikats

1. Navigieren Sie zum Abschnitt *Certificates, Identifiers & Profiles* (Zertifikate, Bezeichner & Profile) im Developer Member Center von Apple.
2. Wählen Sie unter *Certificates* (Zertifikate) die Option **Production** (Produktion) aus.
3. Klicken Sie auf die Schaltfläche **+** , um ein neues Zertifikat zu erstellen.
4. Wählen Sie unter der Überschrift *Produktion* **Intern und Ad-hoc** aus:

   [![](in-house-distribution-images/createcertmanually01.png "„Intern“ und „Ad-hoc“ auswählen")](in-house-distribution-images/createcertmanually01.png#lightbox)

5. Klicken Sie auf „Continue“ (Weiter), und befolgen Sie die Anweisungen zur Erstellung einer Zertifikatsignieranforderung (CSR) mithilfe des Keychain-Zugriffs:

   [![](in-house-distribution-images/createcertmanually02.png "Eine Zertifikatsignieranforderung über Keychain-Zugriff erstellen")](in-house-distribution-images/createcertmanually02.png#lightbox)

6. Sobald Sie Ihre CSR wie beschrieben erstellt haben, klicken Sie auf „Weiter“, und laden Sie Ihre CSR in das Member Center hoch:

   [![](in-house-distribution-images/createcertmanually03.png "Zertifikatsignieranforderung in das Member Center hochladen")](in-house-distribution-images/createcertmanually03.png#lightbox)

7. Klicken Sie auf „Generieren“, um Ihr Zertifikat zu erstellen.
8. Laden Sie das abgeschlossene Zertifikat herunter, und doppelklicken Sie auf die Datei, um sie zu installieren.
9. Zu diesem Zeitpunkt sollte das Zertifikat auf dem Computer installiert werden, aber Sie müssen möglicherweise Ihre Profile aktualisieren, um sicherzustellen, dass sie in Xcode sichtbar sind.

Alternativ ist es möglich, ein Zertifikat über das Dialogfeld „Einstellungen“ in Xcode anzufordern. Führen Sie dazu folgende Schritte aus:

1. Wählen Sie das Team aus, und klicken Sie auf *Details anzeigen*:

   [![](in-house-distribution-images/selectteam.png "Ihr Team auswählen")](in-house-distribution-images/selectteam.png#lightbox)

2. Klicken Sie anschließend auf die Schaltfläche **Create** (Erstellen) neben **iOS Distribution Certificate** (iOS-Verteilungszertifikat):

   [![](in-house-distribution-images/selectcert.png "Das iOS-Verteilungszertifikat erstellen")](in-house-distribution-images/selectcert.png#lightbox)

3. Klicken Sie anschließend auf die Schaltfläche **plus (+)** , und wählen Sie **iOS App Store** aus:

   [![](in-house-distribution-images/selectcert.png "„iOS App Store“ auswählen")](in-house-distribution-images/selectcert.png#lightbox)

<a name="profile" />

## <a name="creating-a-distribution-provisioning-profile"></a>Erstellen eines Verteilungsbereitstellungsprofils

<a name="appid" />

### <a name="creating-an-app-id"></a>Erstellen einer App-ID

Wie bei anderen erstellten Bereitstellungsprofilen ist eine App-ID erforderlich, um die Anwendung zu identifizieren, die Sie an das Gerät des Benutzers verteilen. Wenn Sie diese noch nicht erstellt haben, führen Sie die folgenden Schritte aus:


1. Navigieren Sie im [Apple Developer Center](https://developer.apple.com/account/overview.action) zum Abschnitt *Certificate, Identifiers and Profiles* (Zertifikate, Bezeichner und Profile). Wählen Sie unter **Identifiers** (Bezeichner) **App IDs** (App-IDs) aus.
2. Klicken Sie auf die Schaltfläche **+** , und geben Sie einen **Namen** ein, der Sie im Portal identifiziert.
3. Das App-Präfix sollte bereits als Ihre Team-ID festgelegt sein und kann nicht geändert werden. Wählen Sie entweder eine explizite oder eine Platzhalter-App-ID aus, und geben Sie eine Bundle-ID in einem umgekehrten DNS-Format ein, wie beispielsweise: **Explizit**: com.[Domaenenname].[App-Name] **Platzhalter**:com.[Domaenenname].*
4. Wählen Sie die [App Services](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services) (App-Dienste) aus, die für Ihre Anwendung erforderlich sind.
5. Klicken Sie auf die Schaltfläche **Weiter**, und folgen Sie der Anleitung auf dem Bildschirm, um die neue App-ID zu erstellen.

Wenn Sie die erforderlichen Komponenten zum Erstellen eines Verteilungsprofils haben, führen Sie die folgenden Schritte aus, um es zu erstellen:

1. Navigieren Sie zurück zum Apple-Bereitstellungsportal, und wählen Sie **Provisioning** (Bereitstellung) > **Distribution** (Verteilung) aus:

   [![](in-house-distribution-images/distribute01.png "Klicken Sie auf „Bereitstellung > Verteilung“")](in-house-distribution-images/distribute01.png#lightbox)

2. Klicken Sie auf die **+** -Schaltfläche, und wählen Sie den Typ des Verteilungsprofils aus, das Sie **intern** erstellen möchten:

   [ ![](in-house-distribution-images/distribute02.png "Erstellen Sie ein internes Verteilungsprofil")](in-house-distribution-images/distribute02.png#lightbox)

3. Klicken Sie auf die Schaltfläche **Continue** (Weiter), und wählen Sie aus der Dropdownliste die App-ID aus, für die Sie ein Verteilungsprofil erstellen möchten:

   [![](in-house-distribution-images/distribute03.png "Wählen Sie in der Dropdownliste „App-ID“ aus")](in-house-distribution-images/distribute03.png#lightbox)

4. Klicken Sie auf die Schaltfläche **Weiter**, und wählen Sie ein Verteilungszertifikat zum Signieren der Anwendung aus:

   [ ![](in-house-distribution-images/distribute04.png "Wählen Sie ein zum Signieren der Anwendung erforderliches Verteilungszertifikat aus")](in-house-distribution-images/distribute04.png#lightbox)

5. Klicken Sie auf die Schaltfläche **Weiter**, und geben Sie einen **Namen** für das neue Verteilungsprofil ein:

   [![](in-house-distribution-images/distribute06.png "Geben Sie einen Namen für das neue Verteilungsprofil ein")](in-house-distribution-images/distribute06.png#lightbox)

6. Klicken Sie auf die Schaltfläche **Generieren**, um das neue Profil zu erstellen und den Prozess abzuschließen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 Möglicherweise müssen Sie Visual Studio für Mac beenden und die Liste der verfügbaren Signierungsidentitäten und Bereitstellungsprofile in Xcode aktualisieren (anhand der Anweisungen im Abschnitt [Anfordern von Signierungsidentitäten](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), bevor ein neues Verteilungsprofil in Visual Studio für Mac verfügbar ist.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Möglicherweise müssen Sie Visual Studio beenden und durch Xcode (auf dem Mac des Buildhosts) die Listen der verfügbaren Signierungsidentitäten und Bereitstellungsprofile aktualisieren (anhand der Anweisungen im Abschnitt [Anfordern von Signierungsidentitäten](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), bevor ein neues Verteilungsprofil in Visual Studio verfügbar wird.

-----

<a name="inhouse" />

## <a name="distributing-your-app-in-house"></a>Verteilen Ihrer Anwendung intern

Mit dem Developer Enterprise Program von Apple ist der Lizenznehmer verantwortlich für das Verteilen der Anwendung und für die Einhaltung der von Apple festgelegten [Richtlinien](http://adcdownload.apple.com/Documentation/License_Agreements__Apple_Developer_Enterprise_Program/Apple_Developer_Program_Enterprise_Agreement_20150608.pdf).

Ihre Anwendung kann mithilfe einer Vielzahl verschiedener Methoden sicher verteilt werden, z.B.:

- Lokal über iTunes
- MDM-Server
- Ein interner, sicherer Webserver
- E-Mail

Um Ihre Anwendung auf eine der folgenden Weisen zu verteilen, müssen Sie, wie im nächsten Abschnitt erläutert wird, zuerst eine IPA-Datei erstellen.


### <a name="creating-an-ipa-for-in-house-deployment"></a>Erstellen einer IPA für die interne Bereitstellung

Nach der Bereitstellung können Anwendungen in eine Datei namens *IPA* verpackt werden. Dies ist eine ZIP-Datei, die die Anwendung zusammen mit zusätzlichen Metadaten und Symbolen enthält. IPA wird verwendet, um eine Anwendung lokal in iTunes hinzuzufügen, sodass sie direkt auf einem Gerät synchronisiert werden kann, das im Bereitstellungsprofil enthalten ist.

Weitere Informationen zum Erstellen einer IPA finden Sie im Leitfaden [IPA-Support](~/ios/deploy-test/app-distribution/ipa-support.md).


## <a name="summary"></a>Zusammenfassung

Dieser Artikel gibt einen kurzen Überblick über die interne Verteilung von Xamarin.iOS-Anwendungen.

## <a name="related-links"></a>Verwandte Links

- [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Die Datei „iTunesMetadata.plist“](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA-Unterstützung](~/ios/deploy-test/app-distribution/ipa-support.md)
