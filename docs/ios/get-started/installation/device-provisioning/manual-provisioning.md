---
title: Manuelle Bereitstellung für Xamarin.iOS
description: Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der manuellen Bereitstellung zum Einrichten von Entwicklungszertifikaten und -profilen.
ms.prod: xamarin
ms.assetid: E26ACC94-F4A5-4FF5-B7D4-BE596745A665
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/06/2020
ms.openlocfilehash: 9333750432395d008a5454e293648f4e594ae112
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571687"
---
# <a name="manual-provisioning-for-xamarinios"></a>Manuelle Bereitstellung für Xamarin.iOS

_Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der manuellen Bereitstellung zum Einrichten von Entwicklungszertifikaten und -profilen._

> [!NOTE]
> Die Anweisungen auf dieser Seite gelten für Entwickler, die das kostenpflichtige Apple Developer Program nutzen. Wenn Sie ein kostenloses Konto nutzen, erhalten Sie im Leitfaden zum [kostenlosen Bereitstellen](~/ios/get-started/installation/device-provisioning/free-provisioning.md) weitere Informationen zum Testen auf Geräten.

## <a name="create-a-development-certificate"></a>Erstellen eines Entwicklungszertifikats

Der erste Schritt beim Einrichten eines Entwicklungsgeräts besteht darin, ein Signaturzertifikat zu erstellen. Ein Signaturzertifikat besteht aus zwei Elementen:

- Ein Entwicklungszertifikat
- Ein privater Schlüssel

Entwicklungszertifikate und die zugehörigen [Schlüssel](#understanding-certificate-key-pairs) sind für einen iOS-Entwickler von entscheidender Bedeutung: Sie stellen Ihre Identität bei Apple fest und verknüpfen Sie mit einem bestimmten Gerät und Profil für die Entwicklung, ähnlich der digitalen Signatur auf Ihren Anwendungen. Apple überprüft Zertifikate, um den Zugriff auf die Geräte zu steuern, die für die Bereitstellung zugelassen sind.

Entwicklungsteams, Zertifikate und Profile können über den Abschnitt [Certificates, Identifiers & Profiles](https://developer.apple.com/account/resources/certificates/list) (Zertifikate, Bezeichner & Profile) im Apple Member Center verwaltet werden (Anmeldung erforderlich). Apple erfordert eine Signierungsidentität, damit Sie Ihren Code für das Gerät oder den Simulator erstellen können.  

> [!IMPORTANT]
> Beachten Sie, dass Sie jeweils nur über zwei iOS-Entwicklungszertifikate gleichzeitig verfügen können. Wenn Sie weitere erstellen möchten, müssen Sie ein vorhandenes widerrufen. Alle Computer, die ein gesperrtes Zertifikat verwenden, können ihre App nicht signieren.

Vergewissern Sie sich, dass Sie gemäß Anweisungen im Leitfaden [Apple-Kontoverwaltung](~/cross-platform/macios/apple-account-management.md) ein Apple Developer-Konto in Visual Studio hinzugefügt haben, bevor Sie den Prozess für die manuelle Bereitstellung starten. Führen Sie nach dem Hinzufügen Ihres Apple Developer-Kontos folgende Schritte aus, um ein Signaturzertifikat zu generieren:

1. Wechseln Sie in Visual Studio zum Fenster mit den Apple Developer-Konten.
    1. Mac: **Visual Studio > Einstellungen > Apple Developer-Konto**
    2. Windows: **Extras > Optionen > Xamarin > Apple-Konten**

2. Wählen Sie ein Team aus, und klicken Sie auf **Details anzeigen**.
3. Klicken Sie auf **Zertifikat erstellen**, und wählen Sie **Apple-Entwicklung** oder **iOS-Entwicklung** aus. Wenn Sie über die richtigen Berechtigungen verfügen, wird nach einigen Sekunden ein neues Signaturzertifikat angezeigt.

### <a name="understanding-certificate-key-pairs"></a>Grundlegendes zu Zertifikatschlüsselpaaren

Das Entwicklerprofil enthält Zertifikate, die zugeordneten Schlüssel und beliebige Bereitstellungsprofile, die dem Konto zugeordnet sind. Es gibt tatsächlich zwei Versionen des Entwicklerprofils – eine im Entwicklerportal, und die andere befindet sich auf einem lokalen Mac. Der Unterschied zwischen den beiden ist der Schlüsseltyp, den sie enthalten: _Das Portalprofil enthält alle öffentlichen Schlüssel, die Ihren Zertifikaten zugeordnet ist, während die Kopie auf Ihrem lokalen Mac alle privaten Schlüssel enthält_. Damit die Zertifikate gültig sind, müssen die Schlüsselpaare übereinstimmen.

> [!WARNING]
> Der Verlust des Zertifikats und der zugehörigen Schlüssel kann äußerst ungünstig sein, da vorhandene Zertifikate gesperrt und zugeordnete Geräte erneut bereitgestellt werden müssen, einschließlich der für die Ad-hoc-Bereitstellung registrierten Geräte. Nach dem erfolgreichen Einrichten der Entwicklungszertifikate exportieren Sie eine Sicherungskopie und speichern sie an einem sicheren Ort. Weitere Informationen hierzu finden Sie in Abschnitt „Exporting and Importing Certificates and Profiles“ (Exportieren und Importieren von Zertifikaten und Profilen) des Handbuchs [Maintaining Certificates (Verwalten von Zertifikaten)](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/MaintainingCertificates/MaintainingCertificates.html) in der Apple-Dokumentation.

<a name="provisioning"></a>

## <a name="provision-an-ios-device-for-development"></a>Bereitstellen eines iOS-Geräts für die Entwicklung

Nun, da Sie Ihre Identität bei Apple eingerichtet haben und über ein Entwicklungszertifikat verfügen, müssen Sie ein Bereitstellungsprofil und die erforderlichen Entitäten einrichten, um das Bereitstellen einer App auf einem Apple-Gerät zu ermöglichen. Das Gerät muss eine Version von iOS ausführen, die von Xcode unterstützt wird. Es kann erforderlich sein, das Gerät, Xcode oder beides zu aktualisieren.

<a name="adddevice"></a>

## <a name="add-a-device"></a>Hinzufügen eines Geräts

Wenn Sie ein Bereitstellungsprofil für die Entwicklung erstellen, müssen wir angeben, welche Geräte die Anwendung ausführen können. Um dies zu ermöglichen, können bis zu 100 Geräte pro Kalenderjahr zu unserem Developer Portal hinzugefügt werden, und von hier aus können wir die Geräte auswählen, die zu einem bestimmten Bereitstellungsprofil hinzugefügt werden. Führen Sie die nachfolgenden Schritte zum Hinzufügen eines Geräts zum Developer Portal auf Ihrem Mac aus:

1. Verbinden Sie das Gerät mit dem Mac mit dem bereitgestellten USB-Kabel.
2. Öffnen Sie Xcode, und wechseln Sie zu **Window > Devices and Simulators** (Fenster > Geräte und Simulatoren).
3. Wählen Sie unter der Registerkarte **Devices** (Geräte) im links angezeigten Menü das Gerät aus.
4. Markieren Sie die Zeichenfolge **Bezeichner**, und kopieren Sie sie in die Zwischenablage:

   ![Xcode-Fenster mit Geräten und Simulatoren und hervorgehobenem iOS-Bezeichner](manual-provisioning-images/xcode-devices.png)

5. Wechseln Sie in einem Webbrowser zum [Geräteabschnitt im Entwicklerportal](https://developer.apple.com/account/resources/devices/list), und klicken Sie auf die Schaltfläche **+** :

   ![Screenshot der Geräteseite auf der Apple Developer-Website mit hervorgehobener Schaltfläche](manual-provisioning-images/developer-portal-devices.png)

6. Legen Sie die richtige **Platform** (Plattform) fest, und geben Sie einen Namen für das neue Gerät an. Fügen Sie den zuvor kopierten Bezeichner in das Feld **Device ID** (Geräte-ID) ein:

    ![Screenshot der Seite zur Registrierung neuer Geräte mit ordnungsgemäß ausgefüllten Feldern](manual-provisioning-images/new-device-info.png)

7. Klicken Sie auf **Weiter**.
8. Überprüfen Sie die Informationen, und klicken Sie dann auf **Register** (Registrieren).

Wiederholen Sie die oben genannten Schritte für jedes iOS-Gerät, das zum Testen oder Debuggen einer Xamarin.iOS-Anwendung verwendet wird.

<a name="provisioningprofile"></a>

## <a name="create-a-development-provisioning-profile"></a>Erstellen eines Entwicklungsbereitstellungsprofils

Nach dem Hinzufügen des Geräts zum Entwicklerportal, ist es erforderlich, ein Bereitstellungsprofil zu erstellen, und das Gerät hinzuzufügen. 

Bevor Sie ein Bereitstellungsprofil erstellen, muss eine *App-ID* erstellt werden. Eine App-ID ist eine Zeichenfolge im Reverse-DNS-Stil, die eine Anwendung eindeutig identifiziert. Die folgenden Schritte veranschaulichen, wie Sie eine **Platzhalter-App-ID** erstellen, die zur Erstellung und Installation der meisten Anwendungen verwendet werden kann. **Explizite App-IDs** ermöglichen nur die Installation einer Anwendung (mit der entsprechenden Bundle-ID), und werden in der Regel für bestimmte iOS-Features, z.B. Apple Pay und HealthKit, verwendet. Informationen zum Erstellen expliziter App-IDs finden Sie in der Anleitung [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md).

### <a name="new-wildcard-app-id"></a>Neue Platzhalter-App-ID

1. Wechseln Sie in einem Webbrowser zum [Bezeichnerabschnitt im Entwicklerportal](https://developer.apple.com/account/resources/identifiers/list), und klicken Sie auf die Schaltfläche **+** .
2. Wählen Sie **App IDs** (App-IDs) aus, und klicken Sie auf **Continue** (Weiter).
3. Geben Sie eine **Beschreibung** ein. Legen Sie dann die **Bundle ID** (Bundle-ID) auf **Wildcard** (Platzhalter) fest, und geben Sie eine ID im Format `com.[DomainName].*` ein:

   ![Screenshot der Seite zur Registrierung neuer App-IDs mit ausgefüllten Pflichtfeldern](manual-provisioning-images/new-app-id.png)

4. Klicken Sie auf **Weiter**.
5. Überprüfen Sie die Informationen, und klicken Sie dann auf **Register** (Registrieren).

### <a name="new-provisioning-profile"></a>Neues Bereitstellungsprofil

Sobald die App-ID erstellt wurde, kann das Bereitstellungsprofile erstellt werden. Das Bereitstellungsprofil enthält Informationen dazu, *welcher* App (bzw. welchen Apps, wenn es sich um einen Platzhalter-App-ID handelt) dieses Profil zugeordnet ist, *wer* das Profil verwenden kann (abhängig von den hinzugefügten Entwicklerzertifikaten), und *welche* Geräte die App installieren können.

Führen Sie die folgenden Schritte aus, um ein Bereitstellungsprofil für die Entwicklung manuell zu erstellen:

1. Wechseln Sie in einem Webbrowser zum [Profilabschnitt im Entwicklerportal](https://developer.apple.com/account/resources/profiles/list), und klicken Sie auf die Schaltfläche **+** .

2. Wählen Sie unter **Development** (Entwicklung) die Option **iOS App Development** (iOS-App-Entwicklung) aus, und klicken Sie auf **Continue** (Weiter).

3. Wählen Sie im Dropdownmenü die zu verwendende App-ID aus, und klicken Sie auf **Continue** (Weiter).

4. Wählen Sie die Zertifikate aus, die im Bereitstellungsprofil enthalten sein sollen, und klicken Sie anschließend auf **Continue** (Weiter).

5. Wählen Sie alle Geräte aus, auf denen die App installiert wird, und klicken Sie auf **Continue** (Weiter).

6. Geben Sie einen **Provisioning Profile Name** (Bereitstellungsprofilnamen) an, und klicken Sie dann auf **Generate** (Generieren).

7. Sie können auf der nächsten Seite optional auf **Download** klicken, um das Bereitstellungsprofil auf den Mac herunterzuladen.

<a name="download"></a>

## <a name="download-provisioning-profiles-in-visual-studio"></a>Herunterladen von Bereitstellungsprofilen in Visual Studio

Nachdem Sie ein neues Bereitstellungsprofil im Apple Developer Portal erstellt haben, können Sie es mit Visual Studio herunterladen, damit es für die Bundlesignierung in Ihrer App zur Verfügung steht.

1. Wechseln Sie in Visual Studio zum Fenster mit den Apple Developer-Konten.
    1. Mac: **Visual Studio > Einstellungen > Apple Developer-Konto**
    2. Windows: **Extras > Optionen > Xamarin > Apple-Konten**

2. Wählen Sie das Team aus, und klicken Sie auf **Details anzeigen**.
3. Vergewissern Sie sich, dass das neue Profil in der Liste **Bereitstellungsprofile** angezeigt wird. Möglicherweise müssen Sie Visual Studio neu starten, um die Liste zu aktualisieren. 
4. Klicken Sie auf **Alle Profile herunterladen**.

Das neue Bereitstellungsprofil kann jetzt in Visual Studio verwendet werden.

## <a name="deploy-to-a-device"></a>Bereitstellen für ein Gerät

An dieser Stelle sollte die Bereitstellung abgeschlossen sein, und die App kann nun auf dem Gerät bereitgestellt werden. Führen Sie dazu folgende Schritte aus:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Verbinden Sie Ihr Gerät mit dem Mac.
2. Öffnen Sie die Datei **Info.plist**, und stellen Sie sicher, dass der Wert für die **Bundle-ID** der zuvor erstellten App-ID entspricht (es sei denn, die App-ID ist ein Platzhalter).
3. Wählen Sie im Abschnitt **Signieren** die Option **Manuelle Bereitstellung** als **Schema** aus:

    ![Screenshot der Datei „Info.plist“ in Visual Studio für Mac mit ausgewählter manueller Bereitstellung](manual-provisioning-images/vsm-info-plist.png)

4. Klicken Sie auf **Optionen für Bundlesignierung**.
5. Stellen Sie sicher, dass die Buildkonfiguration auf **Debug|iPhone** festgelegt ist. Öffnen Sie sowohl das Dropdownmenü **Signierungsidentität** als auch das Dropdownmenü **Bereitstellungsprofil**, um sicherzustellen, dass die richtigen Zertifikate und Bereitstellungsprofile aufgelistet werden: 

   ![Eigenschaftenseite zur iOS-Bundlesignierung mit geöffnetem Dropdownmenü „Bereitstellungsprofil“ und Auflistung aller verfügbaren Bereitstellungsprofile für die App](manual-provisioning-images/vsm-bundle-signing.png)

6. Wählen Sie eine bestimmte Identität und ein bestimmtes Profil zur Verwendung aus, oder behalten Sie die Einstellung **Automatisch** bei. Wenn die Einstellung auf **Automatisch** festgelegt ist, wählt Visual Studio für Mac die Identität und das Profil basierend auf der **Bundle-ID** in der Datei **Info.plist** aus. 
7. Klicken Sie auf **OK**.
8. Klicken Sie auf **Ausführen**, um die App auf Ihrem Gerät bereitzustellen.


# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Verbinden Sie Ihr Gerät mit dem Mac-Buildhost.
2. Öffnen Sie die Datei **Info.plist**, und stellen Sie sicher, dass der Wert für die **Bundle-ID** der zuvor erstellten App-ID entspricht (es sei denn, die App-ID ist ein Platzhalter).
3. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Namen des iOS-Projekts, wählen Sie **Eigenschaften** aus, und navigieren Sie dann zur Registerkarte **iOS-Bundlesignierung**.
4. Stellen Sie sicher, dass die Buildkonfiguration auf **Debug|iPhone** festgelegt ist. Wählen Sie im Abschnitt **Bundlesignierung** die Option **Manuelle Bereitstellung** als **Schema** aus:

    ![Screenshot der Datei „Info.plist“ in Visual Studio für Mac mit ausgewählter manueller Bereitstellung](manual-provisioning-images/vs-bundle-signing.png)

5. Öffnen Sie sowohl das Dropdownmenü **Signierungsidentität** als auch das Dropdownmenü **Bereitstellungsprofil**, um sicherzustellen, dass die richtigen Zertifikate und Bereitstellungsprofile aufgelistet werden.
6. Wählen Sie eine bestimmte Identität und ein bestimmtes Profil zur Verwendung aus, oder behalten Sie die Einstellung **Automatisch** bei. Wenn die Einstellung auf **Automatisch** festgelegt ist, wählt Visual Studio die Identität und das Profil basierend auf der **Bundle-ID** in der Datei **Info.plist** aus. 
7. Klicken Sie auf **Ausführen**, um die App auf Ihrem Gerät bereitzustellen.

-----

## <a name="provisioning-for-application-services"></a>Bereitstellung für Anwendungsdienste

Apple stellt eine Auswahl an speziellen Anwendungsdiensten, auch Funktionen genannt, bereit, die für eine Xamarin.iOS-Anwendung aktiviert werden können. Diese Anwendungsdienste müssen im iOS-Bereitstellungsportal beim Erstellen der **App-ID** und auch in der **Entitlements.plist**-Datei, die Teil des Xamarin.iOS-Anwendungsprojekts ist, konfiguriert werden. Informationen zum Hinzufügen von Anwendungsdiensten zu Ihrer App finden Sie in den Leitfäden [Introduction to Capabilities (Einführung in Funktionen)](~/ios/deploy-test/provisioning/capabilities/index.md) und [Working with Entitlements (Arbeiten mit Berechtigungen)](~/ios/deploy-test/provisioning/entitlements.md).

- Erstellen Sie eine App-ID mit den erforderlichen App-Diensten.
- Erstellen Sie ein neues [Bereitstellungsprofil](#provisioningprofile), das diese App-ID enthält.
- Legen Sie Berechtigungen im Xamarin.iOS-Projekt fest

## <a name="related-links"></a>Verwandte Links

- [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [App-Verteilung](~/ios/deploy-test/app-distribution/index.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
- [Apple: Leitfaden zur App-Verteilung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
