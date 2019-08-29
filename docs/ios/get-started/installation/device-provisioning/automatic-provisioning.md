---
title: Automatische Bereitstellung für Xamarin.iOS
description: Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der Option „Automatische Signatur“, um Entwicklungszertifikate und -profile anzufordern.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.custom: video
ms.date: 01/22/2019
ms.openlocfilehash: 4f5c28c4ad9b673ac50b404e7d34f718366bd11d
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121452"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Automatische Bereitstellung für Xamarin.iOS

_Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der Option „Automatische Signatur“, um Entwicklungszertifikate und -profile anzufordern._

## <a name="requirements"></a>Requirements (Anforderungen)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

- Visual Studio für Mac 7.3 oder höher
- Xcode 9 oder höher

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Visual Studio 2019
- Visual Studio 2017 Version 15.7 (oder höher)

Sie müssen außerdem mit einem Mac-Buildhost gekoppelt sein, der über folgende Eigenschaften verfügt:

- Xcode 10 oder höher

-----

## <a name="enabling-automatic-signing"></a>Aktivieren von „Automatische Signatur“

Bevor Sie den automatischen Signierprozess starten, vergewissern Sie sich, dass Sie, wie im Leitfaden [Apple Account Management](~/cross-platform/macios/apple-account-management.md) beschrieben, in Visual Studio eine Apple-ID hinzugefügt haben. Nachdem Sie eine Apple-ID hinzugefügt haben, können Sie alle zugeordneten _Teams_ nutzen. So lassen sich Zertifikate, Profile und andere IDs dem Team zuzuordnen. Die Team-ID wird auch zum Erstellen eines App-ID-Präfix verwendet, das im Bereitstellungsprofil mit eingeschlossen werden soll. Dies erlaubt Apple sicherzustellen, dass Sie sind, wer Sie vorgeben zu sein.

> [!IMPORTANT]
> Melden Sie sich zunächst entweder bei [iTunes Connect](https://itunesconnect.apple.com/) oder bei [appleid.apple.com](https://appleid.apple.com) an, um sicherzustellen, dass Sie die aktuellen Apple-Kontorichtlinien akzeptiert haben. Führen Sie bei Aufforderung die erforderlichen Schritte zum Akzeptieren neuer Kontovereinbarungen von Apple aus. Wenn Sie die Datenschutzbestimmungen von Mai 2018 nicht akzeptieren, wird beim Versuch der Gerätebereitstellung die folgende Warnung angezeigt:
>
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
> }
> ```
>
> oder
>
> ```
> Authentication Service Is Unavailable
> ```

Gehen Sie wie folgt vor, um Ihre App automatisch für die Bereitstellung auf einem iOS-Gerät zu signieren:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie in Visual Studio für Mac ein iOS-Projekt.

2. Öffnen Sie die Datei **Info.plist**.

3. Wählen Sie im Abschnitt **Signierung** die Option **Automatische Bereitstellung** aus:

    ![Dropdownliste zur Teamauswahl](automatic-provisioning-images/image2.png)

4. Wählen Sie Ihr Team aus der Dropdownliste **Team** aus.

5. Nach wenigen Sekunden werden ein Signaturzertifikat und ein Bereitstellungsprofil erstellt:

    ![Erfolgreich erstelltes Zertifikat und Profil](automatic-provisioning-images/image5.png)

    Wenn das automatische Signieren fehlschlägt, wird im Abschnitt **Automatische Signatur** die Ursache des Fehlers angezeigt.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Koppeln Sie Visual Studio 2019 mit einem Mac, wie im Leitfaden [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md) erläutert.

2. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie **Eigenschaften** aus. Navigieren Sie zur Registerkarte **iOS-Bundlesignierung**.

3. Wählen Sie das Schema **Automatische Bereitstellung** aus:

    ![Auswahl des Schemas „Automatisch“](automatic-provisioning-images/prov4.png)

4. Wählen Sie Ihr Team im Kombinationsfeld **Team** aus, um das automatische Signieren zu starten.

    ![Auswahl des Teams](automatic-provisioning-images/prov3.png)

5. Dadurch wird der automatische Signierprozess gestartet. Visual Studio versucht anschließend, eine App-ID, ein Bereitstellungsprofil und eine Signaturidentität zu generieren, um diese Artefakte für die Signatur zu verwenden. Sie können den Generierungsvorgang in der Buildausgabe sehen:

    ![Buildausgabe, die die Generierung von Artefakten zeigt](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Auslösen der automatischen Bereitstellung

Wenn das automatische Signieren aktiviert wurde, aktualisiert Visual Studio für Mac diese Artefakte bei Bedarf, sobald eines der folgenden Ereignisse eintritt:

- Ein iOS-Gerät wird an Ihren Mac angeschlossen
    - Hierbei wird automatisch geprüft, ob das Gerät im Apple Developer Portal registriert ist. Falls nicht, wird es hinzugefügt. Außerdem wird ein neues Bereitstellungsprofil generiert, das es enthält.
- Die Bündel-ID der App wird geändert.
    - Dadurch wird die App-ID aktualisiert. Ein neues Bereitstellungsprofil wird erstellt, das diese App-ID enthält.
- Eine unterstützte Funktion wird in der Datei „Entitlements.plist“ aktiviert.
    - Diese Funktion wird der App-ID hinzugefügt, und es wird ein neues Bereitstellungsprofil mit der aktualisierten App-ID erstellt.
    - Nicht alle Funktionen werden derzeit unterstützt. Weitere Informationen zu den unterstützten Funktionen finden Sie in der Anleitung zum [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md).

## <a name="wildcard-app-ids"></a>Platzhalter-App-IDs

Ab Visual Studio für Mac 7.6 wird bei der automatischen Bereitstellung standardmäßig versucht, eine Platzhalter-App-ID und Bereitstellungsprofile zu erstellen und zu verwenden, anstatt eine explizite App-ID basierend auf dem in der Datei **Info.plist** angegebenen **Bundlebezeichner** zu erstellen. Platzhalter-App-IDs verringern die Anzahl der Profile und IDs, die im Apple Developer Portal verwaltet werden müssen.

In einigen Fällen erfordern die Berechtigungen einer App eine explizite App-ID. Die folgenden Berechtigungen unterstützen keine Platzhalter-App-IDs:

- App-Gruppen
- Zugehörige Domänen
- Apple Pay
- Game Center
- HealthKit
- HomeKit
- Hotspot
- In-App-Käufe
- Multipfad
- NFC
- Persönliches VPN
- Pushbenachrichtigungen
- Konfiguration für drahtloses Zubehör

Wenn Ihre App eine dieser Berechtigungen verwendet, versucht Visual Studio für Mac, eine explizite App-ID (anstelle einer Platzhalter-App-ID) zu erstellen.

> [!NOTE]
> Die automatische Bereitstellung mit Platzhalter-App-IDs ist derzeit nur in Visual Studio für Mac verfügbar.

## <a name="related-links"></a>Verwandte Links

- [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [App-Verteilung](~/ios/deploy-test/app-distribution/index.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
- [Apple: Leitfaden zur App-Verteilung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Snack-Pack-Simplified-iOS-Provisioning-in-Visual-Studio-with-fastlane/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
