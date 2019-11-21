---
title: Manuelle Bereitstellung für Xamarin.iOS
description: Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der manuellen Bereitstellung zum Einrichten von Entwicklungszertifikaten und -profilen.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/15/2017
ms.openlocfilehash: 50ba4a46e9d9f7cbf5337844025790ab51e309dd
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022680"
---
# <a name="manual-provisioning-for-xamarinios"></a>Manuelle Bereitstellung für Xamarin.iOS

_Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der manuellen Bereitstellung zum Einrichten von Entwicklungszertifikaten und -profilen._

> [!NOTE]
> Die Anweisungen auf dieser Seite gelten für Entwickler, die das kostenpflichtige Apple Developer Program nutzen. Wenn Sie ein kostenloses Konto nutzen, erhalten Sie im Leitfaden zum [kostenlosen Bereitstellen](~/ios/get-started/installation/device-provisioning/free-provisioning.md) weitere Informationen zum Testen auf Geräten.

## <a name="creating-a-signing-identity"></a>Erstellen einer Signierungsidentität

Der erste Schritt beim Einrichten eines Entwicklungsgeräts besteht darin, eine Signierungsidentität zu erstellen. Eine Signierungsidentität besteht aus zwei Elementen:

- Ein Entwicklungszertifikat
- Ein privater Schlüssel

Entwicklungszertifikate und die zugehörigen [Schlüssel](#understanding-certificate-key-pairs) sind für einen iOS-Entwickler von entscheidender Bedeutung: Sie stellen Ihre Identität bei Apple fest und verknüpfen Sie mit einem bestimmten Gerät und Profil für die Entwicklung, ähnlich der digitalen Signatur auf Ihren Anwendungen. Apple überprüft Zertifikate, um den Zugriff auf die Geräte zu steuern, die für die Bereitstellung zugelassen sind.

Entwicklungsteams, Zertifikate und Profile können über den Abschnitt [Certificates, Identifiers & Profiles](https://developer.apple.com/account/overview.action) (Zertifikate, Bezeichner & Profile) im Apple Member Center verwaltet werden (Anmeldung erforderlich). Apple erfordert eine Signierungsidentität, damit Sie Ihren Code für das Gerät oder den Simulator erstellen können.  

> [!IMPORTANT]
> Beachten Sie, dass Sie jeweils nur über zwei iOS-Entwicklungszertifikate gleichzeitig verfügen können. Wenn Sie weitere erstellen möchten, müssen Sie ein vorhandenes widerrufen. Alle Computer, die ein gesperrtes Zertifikat verwenden, können ihre App nicht signieren.

Führen Sie folgende Schritte aus, um eine Signieridentität zu generieren:

1. Melden Sie sich im Abschnitt [Certificates, Identifiers, and Profiles (Zertifikate, Bezeichner und Profile) des Developer Portal an](https://developer.apple.com/account/overview.action), und wählen Sie den Abschnitt **Certificates** (Zertifikate) aus der **iOS-Apps**-Spalte aus. Klicken Sie dann auf die Schaltfläche **+** , um ein neues Zertifikat zu erstellen:

    [![](manual-provisioning-images/cert-plus.png "Click the + to create a new certificate")](manual-provisioning-images/cert-plus.png#lightbox)

2. Wählen Sie die Option **iOS App Development** (iOS-App-Entwicklung) für den Zertifikattyp aus, und klicken Sie auf **Continue** (Weiter). Dieser Bildschirm kann je nach Ihren Kontoberechtigungen variieren:

    [![](manual-provisioning-images/cert-first.png "Select the iOS App Development option for the certificate type")](manual-provisioning-images/cert-first.png#lightbox)

3. Fordern Sie eine Zertifikatsignieranforderung an, die hochgeladen wird, um ein Zertifikat manuell zu generieren. Starten Sie zu diesem Zweck den **Keychain Access** (Keychain-Zugriff) auf einem Macintosh-Computer. Navigieren Sie zum Hauptmenü, und wählen Sie **Certificate Assistant** (Zertifikatassistent) und **Request a Certificate from a Certificate Authority...** (Ein Zertifikat von einer Zertifizierungsstelle anfordern...) wie unten gezeigt aus:

      [![](manual-provisioning-images/key-first.png "Request a Certificate Signing Request")](manual-provisioning-images/key-first.png#lightbox)

4. Geben Sie Ihre Informationen ein, und wählen Sie die Option **Auf Datenträger speichern** aus:

    [![](manual-provisioning-images/key-second.png "Fill in your information")](manual-provisioning-images/key-second.png#lightbox)

5. Speichern Sie die Zertifikatsignieranforderung an einem Ort, wo sie leicht gefunden werden kann:

    [![](manual-provisioning-images/cert-third.png "Save the CSR")](manual-provisioning-images/cert-third.png#lightbox)

6. Kehren Sie zum Bereitstellungsportal zurück, laden Sie das Zertifikat in das Portal hoch, und übermitteln Sie es:

    [![](manual-provisioning-images/cert-second.png "Upload the Certificate to the portal")](manual-provisioning-images/cert-second.png#lightbox)

    Wenn Sie nicht über Administratorrechte verfügen, muss das Zertifikat von einem Administrator oder einem Team-Agent genehmigt werden.

7. Laden Sie das Zertifikat nach der Genehmigung aus dem Bereitstellungsportal herunter:

    [![](manual-provisioning-images/status-dev.png "Download the Certificate from the Provisioning Portal")](manual-provisioning-images/status-dev.png#lightbox)

8. Doppelklicken Sie auf das heruntergeladene Zertifikat, um den Keychain-Zugriff zu starten, und öffnen Sie den Bereich **My Certificates** (Meine Zertifikate), der das oder die neuen Zertifikate und den zugehörigen privaten Schlüssel anzeigt:

    [![](manual-provisioning-images/keychain.png "The Certificate in Keychain Access")](manual-provisioning-images/keychain.png#lightbox)

### <a name="understanding-certificate-key-pairs"></a>Grundlegendes zu Zertifikatschlüsselpaaren

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Das Entwicklerprofil enthält Zertifikate, die zugeordneten Schlüssel und beliebige Bereitstellungsprofile, die dem Konto zugeordnet sind. Es gibt tatsächlich zwei Versionen des Entwicklerprofils – eine im Entwicklerportal, und die andere befindet sich auf einem lokalen Mac. Der Unterschied zwischen den beiden ist der Schlüsseltyp, den sie enthalten: _Das Portalprofil enthält alle öffentlichen Schlüssel, die Ihren Zertifikaten zugeordnet ist, während die Kopie auf Ihrem lokalen Mac alle privaten Schlüssel enthält_. Damit die Zertifikate gültig sind, müssen die Schlüsselpaare übereinstimmen. Behalten Sie eine Sicherung des Entwicklerprofils auf dem lokalen Mac. Sollte der private Schlüssel verloren gehen, müssen alle Zertifikate und Bereitstellungsprofile erneut generiert werden.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Das Entwicklerprofil enthält Zertifikate, die zugeordneten Schlüssel und beliebige Bereitstellungsprofile, die dem Konto zugeordnet sind. Es gibt tatsächlich zwei Versionen eines Entwicklerprofils – eine im Entwicklerportal, und die andere befindet sich auf einem Macintosh-Computer. Der Unterschied zwischen den beiden ist der Schlüsseltyp, den sie enthalten: _Das Portalprofil enthält alle öffentlichen Schlüssel, die Ihren Zertifikaten zugeordnet ist, während die Kopie auf Ihrem Mac alle privaten Schlüssel enthält_. Damit die Zertifikate gültig sind, müssen die Schlüsselpaare übereinstimmen. Behalten Sie eine Sicherung des Entwicklerprofils auf dem Mac des Buildhosts von Xamarin. Sollte der private Schlüssel verloren gehen, müssen alle Zertifikate und Bereitstellungsprofile erneut generiert werden.

-----

> [!WARNING]
> Der Verlust des Zertifikats und der zugehörigen Schlüssel kann äußerst ungünstig sein, da vorhandene Zertifikate gesperrt und zugeordnete Geräte erneut bereitgestellt werden müssen, einschließlich der für die Ad-hoc-Bereitstellung registrierten Geräte. Nach dem erfolgreichen Einrichten der Entwicklungszertifikate exportieren Sie eine Sicherungskopie und speichern sie an einem sicheren Ort. Weitere Informationen hierzu finden Sie in Abschnitt „Exporting and Importing Certificates and Profiles“ (Exportieren und Importieren von Zertifikaten und Profilen) des Handbuchs [Maintaining Certificates (Verwalten von Zertifikaten)](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) in der Apple-Dokumentation.

<a name="provisioning" />

## <a name="provisioning-an-ios-device-for-development"></a>Bereitstellen eines iOS-Geräts für die Entwicklung

Nachdem Sie Ihre Identität mit Apple hergestellt haben und ein Entwicklungszertifikat besitzen, müssen Sie ein Bereitstellungsprofil und die erforderlichen Entitäten einrichten, um das Bereitstellen einer App auf einem Apple-Gerät zu ermöglichen. Das Gerät muss eine Version von iOS ausführen, die von Xcode unterstützt wird. Es kann erforderlich sein, das Gerät, Xcode oder beides zu aktualisieren.

<a name="adddevice" />

## <a name="add-a-device"></a>Hinzufügen eines Geräts

Wenn Sie ein Bereitstellungsprofil für die Entwicklung erstellen, müssen wir angeben, welche Geräte die Anwendung ausführen können. Um dies zu ermöglichen, können bis zu 100 Geräte pro Kalenderjahr zu unserem Developer Portal hinzugefügt werden, und von hier aus können wir die Geräte auswählen, die zu einem bestimmten Bereitstellungsprofil hinzugefügt werden. Führen Sie die nachfolgenden Schritte zum Hinzufügen eines Geräts zum Developer Portal auf Ihrem Mac aus:

1. Starten Sie Xcode.
2. Verbinden Sie das Gerät mit dem Mac mit dem bereitgestellten USB-Kabel.
3. Wählen Sie im **Windows**-Menü die Option **Geräte** aus:

   [![](manual-provisioning-images/add01.png "From the Windows menu select Devices")](manual-provisioning-images/add01.png#lightbox)

4. Wählen Sie das gewünschte iOS-Gerät aus der **GERÄTE**-Liste auf der linken Seite des Gerätefensters aus.
5. Markieren Sie die Zeichenfolge **Bezeichner**, und kopieren Sie sie in die Zwischenablage:

   [![](manual-provisioning-images/add02.png "Highlight the Identifier string")](manual-provisioning-images/add02.png#lightbox)

6. Navigieren Sie in Safari zum [Apple Developer Center](https://developer.apple.com/membercenter/index.action), und melden Sie sich an.
7. Klicken Sie auf den Link **Certificates, Identifiers & Profiles (Zertifikate, Bezeichner & Profile)** :

   [![](manual-provisioning-images/add03.png "Click the Certificates, Identifiers  Profiles link")](manual-provisioning-images/add03.png#lightbox)

8. Klicken Sie auf den Link **Geräte**:

   [![](manual-provisioning-images/add04.png "Click on the Devices link")](manual-provisioning-images/add04.png#lightbox)

9. Klicken Sie auf die **+** -Schaltfläche:

   [![](manual-provisioning-images/add05.png "Click the + button")](manual-provisioning-images/add05.png#lightbox)

10. Geben Sie einen Namen für das neue Gerät an, und fügen Sie das Gerät **Bezeichner**, das wir oben kopiert haben, im Feld **UUID** ein:

    [![](manual-provisioning-images/add06.png "Provide a name for the new device and the device Identifier")](manual-provisioning-images/add06.png#lightbox)

11. Klicken Sie auf die Schaltfläche **Continue** (Weiter).
12. Überprüfen Sie abschließend die Informationen, und klicken Sie auf die **Registrieren**-Schaltfläche:

    [![](manual-provisioning-images/add07.png "Review the information")](manual-provisioning-images/add07.png#lightbox)

Wiederholen Sie die oben genannten Schritte für jedes iOS-Gerät, das zum Testen oder Debuggen einer Xamarin.iOS-Anwendung verwendet wird.

Nach dem Hinzufügen des Geräts zum Entwicklerportal, ist es erforderlich, ein Bereitstellungsprofil zu erstellen, und das Gerät hinzuzufügen.

<a name="provisioningprofile" />

## <a name="creating-a-development-provisioning-profile"></a>Erstellen eines Entwicklungsbereitstellungsprofils

Wie beim Entwicklungszertifikat können Bereitstellungsprofile manuell über den Abschnitt [Certificates, Identifiers & Profiles](https://developer.apple.com/account/overview.action) im Apple Members Center erstellt werden.

Bevor Sie ein Bereitstellungsprofil erstellen, muss eine *App-ID* erstellt werden. Eine App-ID ist eine Zeichenfolge im Reverse-DNS-Stil, die eine Anwendung eindeutig identifiziert. Die folgenden Schritte veranschaulichen, wie Sie eine **Platzhalter-App-ID** erstellen, die zur Erstellung und Installation der meisten Anwendungen verwendet werden kann. **Explizite App-IDs** ermöglichen nur die Installation einer Anwendung (mit der entsprechenden Bundle-ID), und werden in der Regel für bestimmte iOS-Features, z.B. Apple Pay und HealthKit, verwendet. Informationen zum Erstellen expliziter App-IDs finden Sie in der Anleitung [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md).

### <a name="app-id"></a>App-ID

1. Navigieren Sie im [Entwicklerportal](https://developer.apple.com/account/overview.action) zum Abschnitt *Zertifikate, Bezeichner und Profile* des Apple Developer Center. Wählen Sie unter **Bezeichner** **App-IDs** aus.
2. Klicken Sie auf die **+** -Schaltfläche aus, und geben Sie einen **Namen** an:

    [![](manual-provisioning-images/appid05a.png "Provide a Name")](manual-provisioning-images/appid05a.png#lightbox)
3. Das App-Präfix sollte voreingestellt werden. Wählen Sie eine **Platzhalter-App-ID** für das App-Suffix. Geben Sie eine Bundle-ID im Format `com.[DomainName].*` ein :

   [![](manual-provisioning-images/appid05b.png "Enter a Bundle ID")](manual-provisioning-images/appid05b.png#lightbox)

4. Klicken Sie auf die Schaltfläche **Weiter**, und folgen Sie der Anleitung auf dem Bildschirm, um die neue App-ID zu erstellen.

### <a name="provisioning-profile"></a>Bereitstellungsprofil

Sobald die App-ID erstellt wurde, können die Bereitstellungsprofile erstellt werden. Dieses Bereitstellungsprofil enthält Informationen dazu, *welcher* App (oder Apps, wenn es sich um einen Platzhalter-App-ID handelt) dieses Profil zugeordnet ist, *wer* das Profil verwenden kann (je nachdem, welche Entwicklerzertifikate hinzugefügt wurden), und *welche* Geräte diese App installieren können.

Führen Sie diese Schritte aus, um ein Bereitstellungsprofil für die Entwicklung manuell zu erstellen:

1. Verwenden Sie Safari, um zum [Apple Developers Member Center](https://developer.apple.com/membercenter/index.action) zu navigieren, und wählen Sie im Abschnitt *Zertifikate, Bezeichner & Profile* Bereitstellungsprofile aus.
2. Klicken Sie zum Erstellen eines neuen Profils auf die **+** -Schaltfläche in der oberen rechten Ecke.
3. Wählen Sie im Abschnitt **Entwicklung** das Optionsfeld neben der **iOS-App-Entwicklung**, und klicken Sie auf **Weiter**:

    [![](manual-provisioning-images/provisioning-profile01.png "Select the type of profile to create")](manual-provisioning-images/provisioning-profile01.png#lightbox)
4. Wählen Sie die zu verwendende App-ID aus dem Dropdownmenü aus:

    [![](manual-provisioning-images/provisioning-profile02.png "Select the App ID that to use")](manual-provisioning-images/provisioning-profile02.png#lightbox)
5. Wählen Sie die Zertifikate aus, die im Bereitstellungsprofil enthalten sein sollen, und klicken Sie anschließend auf **Continue** (Weiter):

    [![](manual-provisioning-images/provisioning-profile03.png "Select the Certificates to include in the provisioning profile")](manual-provisioning-images/provisioning-profile03.png#lightbox)
6. Wählen Sie alle Geräte aus, auf denen die App installiert wird.

    [![](manual-provisioning-images/provisioning-profile04.png "Select all the devices that the app will be installed on")](manual-provisioning-images/provisioning-profile04.png#lightbox)
7. Geben Sie dem Bereitstellungsprofil einen identifizierbaren Namen, und klicken Sie auf **Weiter**, um das Profil zu erstellen:

    [![](manual-provisioning-images/provisioning-profile05.png "Provide the Provisioning Profile with an identifiable a name")](manual-provisioning-images/provisioning-profile05.png#lightbox)
8. Drücken Sie auf **Herunterladen**, um das Bereitstellungsprofil auf einem Mac herunterzuladen:

    [![](manual-provisioning-images/provisioning-profile06.png "Download the provisioning profile")](manual-provisioning-images/provisioning-profile06.png#lightbox)
9. Doppelklicken Sie auf die Datei, auf der das Bereitstellungsprofil in Xcode installiert werden soll. Beachten Sie, dass Xcode möglicherweise keine optischen Hinweise zur Profilinstallation anzeigen wird, mit Ausnahme des Profilstarts. Dies kann unter **Xcode > Einstellungen > Konten** überprüft werden. Wählen Sie Ihre Apple-ID aus, und klicken Sie auf **Details anzeigen...** . Ihr neues Bereitstellungsprofil sollte wie unten gezeigt aufgeführt werden:

      [![](manual-provisioning-images/provisioning-profile07.png "Viewing the profile in Xcode")](manual-provisioning-images/provisioning-profile07.png#lightbox)

Nachdem das Bereitstellungsprofil erfolgreich erstellt wurde, kann es erforderlich sein, Xcode zu aktualisieren, sodass alle Entwicklungszertifikate in Visual Studio für Mac und Visual Studio zur Verfügung stehen.

<a name="download" />

## <a name="downloading-profiles-and-certificates-in-xcode"></a>Herunterladen von Profilen und Zertifikaten in Xcode

Zertifikate und Bereitstellungsprofile, die im Apple-Entwicklerportal erstellt wurden, werden möglicherweise nicht automatisch in Xcode angezeigt. Aus diesem Grund kann es erforderlich sein, sie herunterzuladen, damit Visual Studio für Mac und Visual Studio darauf zugreifen können. Führen Sie die folgenden Schritte aus, um alle im Apple Developer Portal erstellten Zertifikate zu aktualisieren und herunterzuladen:

1. Beenden Sie Visual Studio für Mac bzw. Visual Studio.
2. Starten Sie Xcode.
3. Wählen Sie **Xcode-Menü > Einstellungen...** aus
4. Klicken Sie auf die **Konten**-Registerkarte.
5. Wählen Sie ein Team aus, und klicken Sie auf die Schaltfläche **Download Manual Profiles** (Manuelle Profile herunterladen):  [![](manual-provisioning-images/selectteam1.png "Herunterladen von manuellen Profilen")](manual-provisioning-images/selectteam1.png#lightbox).

6. Beenden Sie Xcode.
7. Starten Sie Visual Studio für Mac bzw. Visual Studio.

Die neuen Zertifikate oder Bereitstellungsprofile sind in Visual Studio für Mac oder Visual Studio verfügbar und einsatzbereit.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

> [!IMPORTANT]
> Möglicherweise müssen Sie Visual Studio für Mac beenden und neu starten, bevor neue oder geänderte Zertifikate oder von Xcode aktualisierte Profile angezeigt werden.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

> [!IMPORTANT]
> Möglicherweise müssen Sie Visual Studio beenden und neu starten, bevor neue oder geänderte Zertifikate oder von Xcode aktualisierte Profile angezeigt werden.

-----

<a name="appservices" />

## <a name="provisioning-for-application-services"></a>Bereitstellung für Anwendungsdienste

Apple stellt eine Auswahl an speziellen Anwendungsdiensten, auch Funktionen genannt, bereit, die für eine Xamarin.iOS-Anwendung aktiviert werden können. Diese Anwendungsdienste müssen im iOS-Bereitstellungsportal beim Erstellen der **App-ID** und auch in der **Entitlements.plist**-Datei, die Teil des Xamarin.iOS-Anwendungsprojekts ist, konfiguriert werden. Informationen zum Hinzufügen von Anwendungsdiensten zu Ihrer App finden Sie in den Leitfäden [Introduction to Capabilities (Einführung in Funktionen)](~/ios/deploy-test/provisioning/capabilities/index.md) und [Working with Entitlements (Arbeiten mit Berechtigungen)](~/ios/deploy-test/provisioning/entitlements.md).

- Erstellen Sie eine App-ID mit den erforderlichen App-Diensten.
- Erstellen Sie ein neues [Bereitstellungsprofil](#provisioningprofile), das diese App-ID enthält.
- Legen Sie Berechtigungen im Xamarin.iOS-Projekt fest

## <a name="deploying-to-a-device"></a>Bereitstellen auf einem Gerät

An dieser Stelle sollte die Bereitstellung abgeschlossen sein, und die App kann nun auf dem Gerät bereitgestellt werden. Führen Sie dazu folgende Schritte aus:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

> [!IMPORTANT]
> Bevor Sie beginnen, stellen Sie sicher, dass Sie in der **Info.plist** die Option **Manuelle Bereitstellung** ausgewählt haben.

1. Schließen Sie das Gerät an einem Macintosh-Computer an.
2. Stellen Sie in der **Info.plist** des Projekts sicher, dass die Bundle-ID der App-ID entspricht (es sei denn, die App-ID ist ein Platzhalter):

   ![](manual-provisioning-images/deploydevice01xs.png "Entering an Identifier")

3. Klicken Sie mit der rechten Maustaste auf das Projekt, um das Dialogfeld Projektoptionen anzuzeigen, und navigieren Sie zu **Erstellen > iOS-Bundle-Signierung**. Überprüfen Sie in der Dropdownliste neben der **Signierungsidentität** und dem **Bereitstellungsprofil**, ob Visual Studio für Mac die richtigen Profile anzeigt, und wählen Sie eine bestimmte Identität und ein bestimmtes Profil aus:

   ![](manual-provisioning-images/deploydevice02xs.png "Select a specific identity & profile")

   Wenn die Einstellung auf **Automatisch** festgelegt ist, wählt Visual Studio für Mac die Identität und das Profil basierend auf der Bundle-ID aus, die in Schritt 2 festgelegt wurde.

4. Stellen Sie sicher, dass die Buildkonfiguration auf **iPhone** / **iPad** anstatt auf Simulator festgelegt wurde.
5. Klicken Sie in Visual Studio für Mac auf **Ausführen**, und beobachten Sie die Ausführung der App auf dem Gerät.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

> [!IMPORTANT]
> Bevor Sie beginnen, stellen Sie sicher, dass Sie in der unter **Projekt > Bereitstellungseigenschaften…** die Option **Manuelle Bereitstellung** ausgewählt haben.

1. Schließen Sie das Gerät an den Mac-Buildhost an.
2. Stellen Sie in der **Info.plist** des Projekts sicher, dass die Bundle-ID der App-ID entspricht:

   ![](manual-provisioning-images/servicevs01.png "Entering an Identifier")

3. Klicken Sie mit der rechten Maustaste auf das Projekt, um das Dialogfeld Projektoptionen anzuzeigen, und navigieren Sie zu **Erstellen > iOS Bundle Signing**. Überprüfen Sie in der Dropdownliste neben der **Signierungsidentität** und dem **Bereitstellungsprofil**, dass Xamarin Studio die richtigen Profile anzeigt, und wählen Sie eine bestimmte Identität bzw. ein bestimmtes Profil aus:

    Wenn dies auf **Automatisch** festgelegt ist, wird Visual Studio die Identität und das Profil auf der Grundlage der Bundle-ID auswählen, die in Schritt #2 festgelegt wurde.

4. Stellen Sie sicher, dass die Buildkonfiguration auf **iPhone** oder **iPad** anstatt auf Simulator festgelegt wurde.
5. Klicken Sie auf **Ausführen** in Visual Studio, und führen Sie die App auf dem Gerät aus.

-----

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch behandelt die erforderlichen Schritte zum Einrichten der Entwicklungsumgebung für Xamarin.iOS. Es wurde untersucht, wie eine Anwendung mit Informationen zum Entwickler, dem Team, den Geräten, auf denen eine App ausgeführt werden kann, und der individuellen App-ID codesigniert ist.

## <a name="related-links"></a>Verwandte Links

- [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [App-Verteilung](~/ios/deploy-test/app-distribution/index.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
- [Apple: Leitfaden zur App-Verteilung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
