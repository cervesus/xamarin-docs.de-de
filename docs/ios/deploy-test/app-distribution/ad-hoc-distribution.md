---
title: Ad-hoc-Verteilung
description: Dieser Artikel bietet einen Überblick über Ad-hoc-Verteilungstechniken, die in erster Linie zum Testen einer Xamarin.iOS-Anwendung mit einer großen Gruppe von Personen verwendet werden.
ms.topic: article
ms.prod: xamarin
ms.assetid: 3B621CAD-103C-478A-97C3-829015F48D1A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: e0db9df11436cf1613ac5eacdf293245f99b8855
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="ad-hoc-distribution"></a>Ad-hoc-Verteilung

_Dieser Artikel bietet einen Überblick über Ad-hoc-Verteilungstechniken, die in erster Linie zum Testen einer Xamarin.iOS-Anwendung mit einer großen Gruppe von Personen verwendet werden._

Nach der Entwicklung Ihrer Xamarin.iOS-App ist der nächste Schritt im Lebenszyklus der Softwareentwicklung die Verteilung Ihrer App an Benutzer zu Testzwecken.

iTunes Connect ist eine Möglichkeit für das Testen von Apps und wird in der Anleitung zu [TestFlight](~/ios/deploy-test/testflight.md) genauer beschrieben. Da allerdings Mitglieder des Apple Developer Enterprise Program keinen Zugriff auf iTunes Connect haben, ist die *Ad-hoc*-Verteilung die beste Methode zum Testen dieser Apps.

Xamarin.iOS-Anwendungen können über die *Ad-hoc*-Verteilung durch Benutzer getestet werden. Mit dieser Verteilungsart, die sowohl im Apple Developer Program als auch im Apple Developer Enterprise Program verfügbar ist, können bis zu 100 iOS-Geräte getestet werden.

Die Ad-hoc-Verteilung bietet als Vorteile, dass keine App Store-Genehmigung erforderlich ist und die Installation drahtlos über einen Webserver oder über iTunes erfolgen kann. Die Entwicklung und Verteilung sind jedoch auf **100** Geräte pro Mitgliedschaftsjahr beschränkt. Außerdem müssen diese Geräte manuell im Member Center über die jeweilige UDID hinzugefügt werden. Weitere Informationen zum Hinzufügen von Geräten finden Sie in der Anleitung [Device Provisioning (Bereitstellung von Geräten)](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#adddevice).

Bei der Ad-hoc-Verteilung müssen Anwendungen mit einem *Ad-hoc-Bereitstellungsprofil* bereitgestellt werden. Dieses enthält Informationen zur Codesignierung, die Identität der Anwendung und die Geräte, auf denen die Anwendung installiert werden kann.

In dieser Anleitung erhalten Sie Informationen zur Bereitstellung für die Ad-hoc-Verteilung und zur Verteilung einer Xamarin.iOS-App.

<a name="setup" />

## <a name="setting-up-for-distribution"></a>Einrichten der Verteilung

Selbst wenn Sie beabsichtigen, eine Xamarin.iOS-Anwendung für die interne Bereitstellung zu Testzwecken freizugeben, müssen Sie ein Bereitstellungsprofil erstellen, das auf die Ad-hoc-Verteilung abgestimmt ist. Dieses Profil ermöglicht das digitale Signieren der Anwendung für das Release, sodass sie auf einem iOS-Gerät installiert werden kann.

Im nächste Abschnitt wird die Erstellung eines Verteilungszertifikats und eines Bereitstellungsprofils für die Verteilung beschrieben.

> [!NOTE]
> Nur Team-Agents und Administratoren können Verteilungszertifikate und Bereitstellungsprofile erstellen.

<a name="createcertificate" />

## <a name="create-a-distribution-certificate"></a>Erstellen eines Verteilungszertifikats


1. Navigieren Sie zum Abschnitt *Certificates, Identifiers & Profiles* (Zertifikate, Bezeichner & Profile) im Developer Member Center von Apple.
2. Wählen Sie unter *Certificates* (Zertifikate) die Option **Production** (Produktion) aus.
3. Klicken Sie auf die Schaltfläche **+**, um ein neues Zertifikat zu erstellen.
4. Wählen Sie unter der Überschrift *Production* (Produktion) je nach Mitgliedschaft in einem Programm entweder **In-House and Ad Hoc** (interne Verteilung und Ad-hoc-Verteilung) oder **App Store- und Ad-hoc-Verteilung** aus:

  [![](ad-hoc-distribution-images/cert-first-small.png "Wählen „Intern und Ad-hoc“ oder „App-Store und Ad-hoc“")](ad-hoc-distribution-images/cert-first-large.png#lightbox)

5. Klicken Sie auf „Continue“ (Weiter), und befolgen Sie die Anweisungen zur Erstellung einer Zertifikatsignieranforderung (CSR) mithilfe des Keychain-Zugriffs:

  [![](ad-hoc-distribution-images/createcertmanually02.png "Eine Zertifikatsignieranforderung über Keychain-Zugriff erstellen")](ad-hoc-distribution-images/createcertmanually02.png#lightbox)

6. Sobald Sie die CSR wie beschrieben erstellt haben, klicken Sie auf „Continue“ (Weiter), und laden Sie die CSR in das Member Center hoch:

  [![](ad-hoc-distribution-images/createcertmanually03.png "Zertifikatsignieranforderung in das Member Center hochladen")](ad-hoc-distribution-images/createcertmanually03.png#lightbox)

7. Klicken Sie auf „Generate“ (Generieren), um das Zertifikat zu erstellen.
8. Laden Sie das abgeschlossene Zertifikat herunter, und doppelklicken Sie auf die Datei, um sie zu installieren.
9. Das Zertifikat sollte nun auf dem Computer installiert sein. Sie müssen aber möglicherweise auf die Schaltfläche [Refresh your profiles](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download) (Ihre Profile aktualisieren) klicken, um sicherzustellen, dass die Profile in Xcode sichtbar sind.

Alternativ ist es möglich, ein Zertifikat über das Dialogfeld „Preferences“ (Einstellungen) in Xcode anzufordern. Führen Sie dazu folgende Schritte aus:

1.   Wählen Sie das Team aus, und klicken Sie auf **Manage Certificates…** (Zertifikate verwalten...): [![](ad-hoc-distribution-images/selectteam.png "Auswählen des Teams")](ad-hoc-distribution-images/selectteam.png#lightbox)

2.   Klicken Sie anschließend auf die **Plus**-Schaltfläche (+), und wählen Sie **iOS App Store** aus: [![](ad-hoc-distribution-images/selectcert.png "Auswählen von iOS App Store")](ad-hoc-distribution-images/selectcert.png#lightbox)

<a name="createprofile" />

## <a name="create-a-distribution-provisioning-profile"></a>Erstellen eines Verteilungsbereitstellungsprofils

<a name="createappid" />

### <a name="create-an-app-id"></a>Erstellen einer App-ID
Wie bei anderen erstellten Bereitstellungsprofilen ist eine App-ID erforderlich, um die Anwendung zu identifizieren, die an das Gerät des Benutzers verteilt wird. Wenn Sie die App-ID noch nicht erstellt haben, führen Sie die folgenden Schritte aus:


1. Navigieren Sie im [Apple Developer Center](https://developer.apple.com/account/overview.action) zum Abschnitt *Certificate, Identifiers and Profiles* (Zertifikate, Bezeichner und Profile). Wählen Sie unter **Identifiers** (Bezeichner) **App IDs** (App-IDs) aus.
2. Klicken Sie auf die Schaltfläche **+**, und geben Sie einen **Namen** ein, der Sie im Portal identifiziert.
3. Das App-Präfix sollte bereits als Ihre Team-ID festgelegt sein und kann nicht geändert werden. Wählen Sie entweder „Explicit ID“ (explizite ID) oder „ Wildcard App ID“ (Platzhalter-App-ID) aus, und geben Sie unter „Bundle ID“ eine Bundle-ID in einem der folgenden umgekehrten DNS-Formate ein:
    - **Explizit**: `com.[DomainName].[AppName]`
    - **Platzhalter**: `com.[DomainName].*`
4. Wählen Sie die [App Services](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#appservices) (App-Dienste) aus, die für Ihre Anwendung erforderlich sind.
5. Klicken Sie auf die Schaltfläche **Continue** (Weiter), und folgen Sie den Anweisungen auf dem Bildschirm, um die neue App-ID zu erstellen.

Wenn Sie die erforderlichen Komponenten zum Erstellen eines Verteilungsprofils haben, führen Sie die folgenden Schritte aus, um es zu erstellen:

1. Navigieren Sie zurück zum Apple-Bereitstellungsportal, und klicken Sie auf **Bereitstellung > Verteilung**: [![](ad-hoc-distribution-images/distribute01.png "Auswahl: Bereitstellung > Verteilung")](ad-hoc-distribution-images/distribute01.png#lightbox)

2. Klicken Sie auf die Schaltfläche **+**, und wählen Sie den Verteilungsprofiltyp aus, den Sie **ad hoc** erstellen möchten:

    [![](ad-hoc-distribution-images/distribute02.png "Erstellen des Verteilungstyps „Ad-hoc“")](ad-hoc-distribution-images/distribute02.png#lightbox)

3. Klicken Sie auf die Schaltfläche **Continue** (Weiter), und wählen Sie aus der Dropdownliste die App-ID aus, für die Sie ein Verteilungsprofil erstellen möchten:

    [![](ad-hoc-distribution-images/distribute03.png "Wählen Sie in der Dropdownliste „App-ID“ aus")](ad-hoc-distribution-images/distribute03.png#lightbox)

4. Klicken Sie auf die Schaltfläche **Weiter**, und wählen Sie ein Verteilungszertifikat zum Signieren der Anwendung aus:

    [ ![](ad-hoc-distribution-images/distribute04.png "Wählen Sie ein zum Signieren der Anwendung erforderliches Verteilungszertifikat aus")](ad-hoc-distribution-images/distribute04.png#lightbox)

6. Klicken Sie auf die Schaltfläche **Weiter**, und geben Sie einen **Namen** für das neue Verteilungsprofil ein:

    [![](ad-hoc-distribution-images/distribute06.png "Geben Sie einen Namen für das neue Verteilungsprofil ein")](ad-hoc-distribution-images/distribute06.png#lightbox)

7. Klicken Sie auf die Schaltfläche **Generieren**, um das neue Profil zu erstellen und den Prozess abzuschließen.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Möglicherweise müssen Sie Visual Studio für Mac beenden und die Liste der verfügbaren Signierungsidentitäten und Bereitstellungsprofile in Xcode aktualisieren (anhand der Anweisungen im Abschnitt [Herunterladen von Profilen und Zertifikaten in Xcode](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), bevor ein neues Verteilungsprofil in Visual Studio für Mac verfügbar ist.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Möglicherweise müssen Sie Visual Studio beenden und durch Xcode (auf dem Mac des Buildhosts) die Listen der verfügbaren Signierungsidentitäten und Bereitstellungsprofile aktualisieren (anhand der Anweisungen im Abschnitt [Downloading Profiles and Certificates in Xcode (Herunterladen von Profilen und Zertifikaten in Xcode)](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), bevor ein neues Verteilungsprofil in Visual Studio verfügbar ist.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Auswählen eines Verteilungsprofils in einem Xamarin.iOS-Projekt

Wenn Sie den endgültigen Build einer Xamarin.iOS-Anwendung erstellen möchten, wählen Sie das zuvor erstellte Verteilungsprofil aus.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

 Gehen Sie in Visual Studio für Mac folgendermaßen vor:

1. Doppelklicken Sie auf den Projektnamen im **Projektmappen-Explorer**, um das Projekt für die Bearbeitung zu öffnen.
2. Wählen Sie aus der Dropdownliste **Konfiguration** **iOS-Bundle-Signierung** und den Buildtyp aus:

    ![](ad-hoc-distribution-images/releasexs01.png "Wählen Sie in der Dropdownliste „Konfiguration“ den Buildtyp aus")
3. In den meisten Fällen kann für die **Signierungsidentität** und das **Bereitstellungsprofil** der Standardwert **Automatisch** beibehalten werden. Visual Studio für Mac wählt basierend auf dem Bündelbezeichner in der Datei „Info.plist“ das richtige Profil aus:

    ![](ad-hoc-distribution-images/releasexs02.png "Die Signieridentität und das Bereitstellungsprofil sind auf den Standardwert „Automatisch“ festgelegt")
4. Wählen Sie bei Bedarf die Signierungsidentität und das (zuvor erstellte) Verteilungsprofil aus den Dropdownlisten aus:

    ![](ad-hoc-distribution-images/releasexs03.png "Wählen Sie Signieridentität und Verteilungsprofil aus")
5. Klicken Sie auf die Schaltfläche **OK**, um die Änderungen zu speichern.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
 Führen Sie in Visual Studio folgende Schritte aus:

1. Klicken Sie mit der rechten Maustaste im **Projektmappen-Explorer** auf den Projektnamen, und wählen Sie **Eigenschaften** aus, um das Projekt für die Bearbeitung zu öffnen.
2. Wählen Sie aus der Dropdownliste **Konfiguration** **iOS-Bundle-Signierung** und den Buildtyp aus:

    ![](ad-hoc-distribution-images/releasevs01.png "Wählen Sie in der Dropdownliste „Konfiguration“ den Buildtyp aus")
3. In den meisten Fällen kann für die **Signierungsidentität** und das **Bereitstellungsprofil** der Standardwert **Automatisch** beibehalten werden. Visual Studio wählt basierend auf dem Bündelbezeichner in der Datei „Info.plist“ das richtige Profil aus:

    ![](ad-hoc-distribution-images/releasevs02.png "Die Signieridentität und das Bereitstellungsprofil sind auf den Standardwert „Automatisch“ festgelegt")
4. Wählen Sie bei Bedarf die Signierungsidentität und das (zuvor erstellte) Verteilungsprofil aus den Dropdownlisten aus:

    ![](ad-hoc-distribution-images/releasevs03.png "Wählen Sie Signieridentität und Verteilungsprofil aus")
5. Speichern Sie die Änderungen an den Projekteigenschaften.

-----

<a name="adhoc" />

## <a name="ad-hoc-distribution"></a>Ad-hoc-Verteilung

[TestFlight](~/ios/deploy-test/testflight.md) ist zwar eine häufig verwendete App für Betatests und Verteilungen, gleichzeitig aber Bestandteil von iTunes Connect. Sie ist daher nicht für Mitglieder des **Apple Developer Enterprise Program** verfügbar.

Durch die Ad-hoc-Verteilung können Entwickler auf vielen unterschiedlichen Geräten Betatests für Apps durchführen, wenn iTunes Connect keine Option ist. Die Ad-hoc-Verteilung funktioniert ähnlich wie die interne Verteilung und erfordert die Erstellung einer IPA-Datei, die anschließend entweder drahtlos oder manuell über iTunes verteilt werden kann.

<a name="IPA_Creation" />

### <a name="ipa-support-for-ad-hoc-deployment"></a>IPA-Unterstützung für Ad-hoc-Bereitstellung

Nach der Bereitstellung können Anwendungen in einer *IPA*-Datei verpackt werden. Dies ist eine ZIP-Datei, die die Anwendung zusammen mit zusätzlichen Metadaten und Symbolen enthält. Die IPA-Datei wird verwendet, um eine Anwendung lokal in iTunes hinzuzufügen, sodass sie direkt auf einem Gerät synchronisiert werden kann, das im Bereitstellungsprofil enthalten ist.

Weitere Informationen zum Erstellen einer IPA-Datei finden Sie im Leitfaden [IPA Support](~/ios/deploy-test/app-distribution/ipa-support.md) (IPA-Unterstützung).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Ad-hoc-Verteilungsmechanismen erläutert, die für das Testen von Xamarin.iOS-Anwendungen erforderlich sind.


## <a name="related-links"></a>Verwandte Links

- [App Store-Verteilung](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Interne Verteilung](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Die Datei „iTunesMetadata.plist“](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [IPA-Unterstützung](~/ios/deploy-test/app-distribution/ipa-support.md)
