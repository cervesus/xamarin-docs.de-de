---
title: App Store-Verteilung
description: In diesem Artikel wird beschrieben, wie Sie eine Xamarin.iOS-Anwendung im App Store veröffentlichen. Außerdem wird das Erstellen eines Verteilungszertifikat und eines Verteilungsbereitstellungsprofils erläutert, außerdem wird erklärt, wie Sie iTunes Connect konfigurieren und die App übermitteln.
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/23/2017
ms.openlocfilehash: 14cf2910767e9c205e5ddc8f580020505f54ef46
ms.sourcegitcommit: e7f27ba75cae5099ef053b819b84132a77d4f9e7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/16/2019
ms.locfileid: "55832027"
---
# <a name="app-store-distribution"></a>App Store-Verteilung

Nachdem eine Xamarin.iOS-Anwendung entwickelt wurde, ist der nächste Schritt im Lebenszyklus der Softwareentwicklung die Verteilung der Anwendung an Benutzer mithilfe des iTunes App Stores. Dies ist die gängigste Methode zum Verteilen von Anwendungen. Indem eine Anwendung im App Store von Apple veröffentlicht wird, kann Sie weltweit für Verbraucher verfügbar gemacht werden.

> [!IMPORTANT]
> Apple [hat mitgeteilt](https://developer.apple.com/ios/submit/), dass ab März 2019 alle Apps und Updates, die an den App Store gesendet werden, mit dem iOS 12.1 SDK oder höher, das in Xcode 10.1 oder höher enthalten ist, erstellt worden sein müssen.
> Apps müssen ebenso die Bildschirmgrößen des iPhone XS und des iPad Pro in 12,9" unterstützen.

Das Verteilen einer Anwendung erfordert genau wie das Entwickeln einer Anwendung, dass Anwendungen mit dem entsprechenden *Bereitstellungsprofil* bereitgestellt werden. Bereitstellungsprofile sind Dateien, die Informationen zur Codesignierung sowie die Identität der Anwendung und den beabsichtigten Verteilungsmechanismus enthalten. Sie enthalten auch Informationen darüber, für welche Geräte eine App bereitgestellt wird, wenn die Verteilung nicht über den App Store stattfindet.

> [!IMPORTANT]
> **Beachten Sie**, dass Sie an einem Apple-Entwicklerprogramm für Einzelpersonen oder Organisationen teilnehmen **müssen**, um iTunes Connect verwenden und somit eine App im App Store veröffentlichen zu können. Wenn Sie Mitglied eines Apple Developer **Enterprise** Programs sind, können Sie die Schritte auf dieser Seite nicht ausführen.

<a name="provisioning" />

## <a name="provisioning-an-app-for-app-store-distribution"></a>Bereitstellen einer App für die Verteilung über den App Store

Unabhängig davon, wie Sie eine Xamarin.iOS-Anwendung veröffentlichen möchten, müssen Sie ein spezielles *Verteilungsbereitstellungsprofil* dafür erstellen. Dieses Profil ermöglicht das digitale Signieren der Anwendung für die Veröffentlichung, sodass sie auf einem iOS-Gerät installiert werden kann. Ähnlich wie ein Entwicklungsbereitstellungsprofil enthält ein Verteilungsprofil Folgendes:

- Eine App-ID
- Ein Verteilungszertifikat

Sie können dieselbe **App-ID** und dieselben **Geräte** wie für das Entwicklungsbereitstellungsprofil verwenden. Wenn Sie bereits eines besitzen, müssen Sie ein Verteilungszertifikat erstellen, um Ihre Organisation zu identifizieren, wenn Sie die App an den App Store übermitteln. Die Schritte zum Erstellen eines Verteilungszertifikats werden im folgenden Abschnitt beschrieben.

> [!NOTE]
> Nur Team-Agents und Administratoren können Verteilungszertifikate und Bereitstellungsprofile erstellen.

<a name="creatingcertificate" />

## <a name="creating-a-distribution-certificate"></a>Erstellen eines Verteilungszertifikats

1. Navigieren Sie zum Abschnitt *Certificates, Identifiers & Profiles* (Zertifikate, Bezeichner & Profile) im Developer Member Center von Apple.
2. Wählen Sie unter *Certificates* (Zertifikate) die Option **Production** (Produktion) aus.
3. Klicken Sie auf die Schaltfläche **+**, um ein neues Zertifikat zu erstellen.
4. Wählen Sie unter der Überschrift *Produktion* die Option **App Store and Ad Hoc** (App Store und Ad-hoc) aus:

    [![](images/createcertmanually01.png "Auswählen von „App Store“ und „Ad-hoc“")](images/createcertmanually01.png#lightbox)
5. Klicken Sie auf **Continue** (Weiter), und befolgen Sie die Anweisungen zum Erstellen einer Zertifikatsignieranforderung mithilfe des Keychain-Zugriffs:

    [![](images/createcertmanually02.png "Eine Zertifikatsignieranforderung über Keychain-Zugriff erstellen")](images/createcertmanually02.png#lightbox)
6. Sobald Sie die CSR wie beschrieben erstellt haben, klicken Sie auf **Weiter**, und laden Sie die CSR in das Member Center hoch:

    [![](images/createcertmanually03.png "Zertifikatsignieranforderung in das Member Center hochladen")](images/createcertmanually03.png#lightbox)

7. Klicken Sie auf **Generate** (Generieren), um das Zertifikat zu erstellen.
8. Klicken Sie für das abgeschlossene Zertifikat auf **Download** (Herunterladen), und doppelklicken Sie auf die Datei, um sie zu installieren.
9. Zu diesem Zeitpunkt sollte das Zertifikat auf dem Computer installiert sein, aber Sie müssen möglicherweise [Ihre Profile aktualisieren](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download), um sicherzustellen, dass sie in Xcode sichtbar sind.

Alternativ ist es möglich, ein Zertifikat über das Dialogfeld „Preferences“ (Einstellungen) in Xcode anzufordern. Führen Sie dazu folgende Schritte aus:

1.   Wählen Sie das Team aus, und klicken Sie auf **Manage Certificates…** (Zertifikate verwalten...): [![](images/selectteam.png "Auswählen des Teams und Klick auf „Details anzeigen“")](images/selectteam.png#lightbox)

2.   Klicken Sie anschließend auf die Schaltfläche **Create** (Erstellen) neben **iOS Distribution Certificate** (iOS-Verteilungszertifikat): [![](images/selectcert.png "Erstellen eines iOS-Verteilungszertifikats")](images/selectcert.png#lightbox)

3.   Die Signierungsidentität wird wie unten dargestellt abhängig von Ihren Teamprivilegien generiert, oder Sie müssen warten, bis ein Team-Agent oder Administrator diese genehmigt: [![](images/generated.png "Die Signieridentität wird generiert und ein Dialogfeld angezeigt")](images/generated.png#lightbox)


<a name="creatingprofile" />

## <a name="creating-a-distribution-profile"></a>Erstellen eines Verteilungsprofils

<a name="creatingappid" />

### <a name="creating-an-app-id"></a>Erstellen einer App-ID

Wie bei anderen erstellten Bereitstellungsprofilen ist eine App-ID erforderlich, um die Anwendung zu identifizieren, die Sie an das Gerät des Benutzers verteilen. Wenn Sie die App-ID noch nicht erstellt haben, führen Sie die folgenden Schritte aus:


1. Navigieren Sie im [Apple Developer Center](https://developer.apple.com/account/overview.action) zum Abschnitt *Certificate, Identifiers and Profiles* (Zertifikate, Bezeichner und Profile). Wählen Sie unter **Identifiers** (Bezeichner) **App IDs** (App-IDs) aus.
2. Klicken Sie auf die Schaltfläche **+**, und geben Sie einen **Namen** ein, der Sie im Portal identifiziert.
3. Das App-Präfix sollte bereits als Ihre Team-ID festgelegt sein und kann nicht geändert werden. Wählen Sie entweder eine explizite oder eine Platzhalter-App-ID aus, und geben Sie eine Bundle-ID in einem umgekehrten DNS-Format ein, wie beispielsweise:
    - **Explizit**: com.[Domänenname].[AppName]
    - **Platzhalter**: com.[Domänenname].*
4. Wählen Sie die [App-Dienste](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) aus, die die Anwendung benötigt.
5. Klicken Sie auf die Schaltfläche **Weiter**, und folgen Sie der Anleitung auf dem Bildschirm, um die neue App-ID zu erstellen.


### <a name="creating-a-provisioning-profile"></a>Erstellen eines Bereitstellungsprofils

Wenn Sie die erforderlichen Komponenten zum Erstellen eines Verteilungsprofils haben, führen Sie die folgenden Schritte aus, um es zu erstellen:

1. Navigieren Sie zurück zum Apple-Bereitstellungsportal, und wählen Sie **Provisioning** (Bereitstellung) > **Distribution** (Verteilung) aus:

    [![](images/distribute01.png "Auswählen von „Bereitstellung > Verteilung“")](images/distribute01.png#lightbox)

2. Klicken Sie auf die Schaltfläche **+**, und wählen Sie als Typ des Verteilungsprofils, das Sie erstellen möchten, **App Store** aus:

    [![](images/distribute02.png "App Store-Verteilungsprofil erstellen")](images/distribute02.png#lightbox)

3. Klicken Sie auf die Schaltfläche **Continue** (Weiter), und wählen Sie aus der Dropdownliste die App-ID aus, für die Sie ein Verteilungsprofil erstellen möchten:

    [![](images/distribute03.png "Wählen Sie in der Dropdownliste „App-ID“ aus")](images/distribute03.png#lightbox)

4. Klicken Sie auf die Schaltfläche **Weiter**, und wählen Sie das Zertifikat zum Signieren der Anwendung aus:

    [![](images/distribute04.png "Auswählen des zum Signieren der Anwendung erforderlichen Verteilungszertifikats")](images/distribute04.png#lightbox)

5. Klicken Sie auf die Schaltfläche **Weiter**, und wählen Sie das iOS-Gerät aus, auf dem die Xamarin.iOS-Anwendung ausgeführt werden soll:

    [![](images/distribute05.png "Auswählen des iOS-Geräts, auf dem die App ausgeführt werden darf")](images/distribute05.png#lightbox)

6. Klicken Sie auf die Schaltfläche **Weiter**, und geben Sie einen **Namen** für das neue Verteilungsprofil ein:

    [![](images/distribute06.png "Geben Sie einen Namen für das neue Verteilungsprofil ein")](images/distribute06.png#lightbox)

7. Klicken Sie auf die Schaltfläche **Generieren**, um das neue Profil zu erstellen und den Prozess abzuschließen.


# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 Möglicherweise müssen Sie Visual Studio für Mac beenden und die Liste der verfügbaren Signierungsidentitäten und Bereitstellungsprofile in Xcode aktualisieren (anhand der Anweisungen im Abschnitt [Anfordern von Signierungsidentitäten](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), bevor ein neues Verteilungsprofil in Visual Studio für Mac verfügbar ist.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 Möglicherweise müssen Sie Visual Studio beenden und durch Xcode (auf dem Mac des Buildhosts) die Liste der verfügbaren Signierungsidentitäten und Bereitstellungsprofile aktualisieren (anhand der Anweisungen im Abschnitt [Anfordern von Signierungsidentitäten](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), bevor ein neues Verteilungsprofil in Visual Studio verfügbar ist.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Auswählen eines Verteilungsprofils in einem Xamarin.iOS-Projekt

Wenn Sie bereit sind, den endgültigen Build einer Xamarin.iOS-Anwendung für den Verkauf im iTunes App Store bereitzustellen, wählen Sie das zuvor erstellte Verteilungsprofil aus.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

 Gehen Sie in Visual Studio für Mac folgendermaßen vor:

1. Doppelklicken Sie auf den Projektnamen im **Projektmappen-Explorer**, um das Projekt für die Bearbeitung zu öffnen.
2. Wählen Sie aus der Dropdownliste **Konfiguration** die Optionen **iOS-Bundle-Signierung** und **Release | iPhone** aus:

    ![](images/releasexs01.png "Wählen Sie „Release | iPhone“ in der Dropdownliste „Konfiguration“ aus")
3. In den meisten Fällen kann für die **Signierungsidentität** und das **Bereitstellungsprofil** der Standardwert **Automatisch** beibehalten werden. Visual Studio für Mac wählt basierend auf dem Bündelbezeichner in der Datei „Info.plist“ das richtige Profil aus:

    ![](images/releasexs02.png "Die Signieridentität und das Bereitstellungsprofil sind auf den Standardwert „Automatisch“ festgelegt")
4. Wählen Sie bei Bedarf die Signierungsidentität und das (zuvor erstellte) Verteilungsprofil aus den Dropdownlisten aus:

    ![](images/releasexs03.png "Wählen Sie Signieridentität und Verteilungsprofil aus")
5. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 Führen Sie in Visual Studio folgende Schritte aus:

1. Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Projektnamen, und wählen Sie **Eigenschaften** aus, um das Projekt für die Bearbeitung zu öffnen.
2. Wählen Sie aus der Dropdownliste **Konfiguration** die Optionen **iOS-Bundle-Signierung** und **Release | iPhone** aus:

    ![](images/releasevs01.png "Wählen Sie „Release | iPhone“ in der Dropdownliste „Konfiguration“ aus")
3. In den meisten Fällen kann für die **Signierungsidentität** und das **Bereitstellungsprofil** der Standardwert **Automatisch** beibehalten werden. Visual Studio wählt basierend auf dem Bündelbezeichner in der Datei „Info.plist“ das richtige Profil aus:

    ![](images/releasevs02.png "Die Signieridentität und das Bereitstellungsprofil sind auf den Standardwert „Automatisch“ festgelegt")
4. Wählen Sie bei Bedarf die Signierungsidentität und das (zuvor erstellte) Verteilungsprofil aus den Dropdownlisten aus:

    ![](images/releasevs03.png "Wählen Sie Signieridentität und Verteilungsprofil aus")
5. Speichern Sie die Änderungen an den Projekteigenschaften.

-----

<a name="itunesconnect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Konfigurieren der Anwendung in iTunes Connect

Sobald die Anwendung erfolgreich bereitgestellt wurde, ist der nächste Schritt das Konfigurieren der App in [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa). Dabei handelt es sich um eine Sammlung von webbasierten Tools, die unter anderem für das Verwalten von iOS-Anwendungen im App Store verwendet werden.

Ihre Xamarin.iOS-Anwendung muss ordnungsgemäß in iTunes Connect eingerichtet und konfiguriert werden, bevor sie zur Überprüfung an Apple gesendet und schließlich für den Verkauf oder als kostenlose App im App Store freigegeben werden kann.

Weitere Informationen finden Sie in der Dokumentation unter [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).

<a name="submitting" />

## <a name="submitting-an-app-to-itunes-connect"></a>Übermitteln einer App an iTunes Connect

Sobald die Anwendung mithilfe des Verteilungsbereitstellungsprofils signiert und in iTunes Connect erstellt wurde, werden die Binärdateien der Anwendung zur Überprüfung durch Apple hochgeladen. Nach einer erfolgreichen Überprüfung durch Apple wird diese im App Store verfügbar.

Weitere Informationen zum Veröffentlichen von Anwendungen im App Store finden Sie unter [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

<a name="windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Automatisches Zurückkopieren von App Bundles nach Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die wichtigsten Komponenten für das Vorbereiten einer Xamarin.iOS-Anwendung für die Verteilung im App Store behandelt.

## <a name="related-links"></a>Verwandte Links

- [Konfigurieren einer App in iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Veröffentlichen im App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Interne Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Ad-hoc-Verteilung](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Die Datei „iTunesMetadata.plist“](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA-Unterstützung](~/ios/deploy-test/app-distribution/ipa-support.md)
