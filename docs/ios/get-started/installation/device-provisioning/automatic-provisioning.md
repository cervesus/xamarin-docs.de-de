---
title: Automatische Bereitstellung
description: Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der Option „Automatische Signatur“, um Entwicklungszertifikate und -profile anzufordern.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/22/2018
ms.openlocfilehash: d324e469ba392b14c635990d607bf04c949ad5db
ms.sourcegitcommit: 9f8e7393019791bbd6af4fefaa24a1602adabb4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/23/2018
---
# <a name="automatic-provisioning"></a>Automatische Bereitstellung

_Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der Option „Automatische Signatur“, um Entwicklungszertifikate und -profile anzufordern._

## <a name="requirements"></a>Anforderungen

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

- Visual Studio für Mac 7.3 oder höher
- Xcode 9 oder höher

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

- Visual Studio 2017 Version 15.7 (oder höher)

Sie müssen außerdem mit einem Mac-Buildhost gekoppelt sein, der über folgende Eigenschaften verfügt:

- Xcode 9 oder höher

-----

## <a name="enabling-automatic-signing"></a>Aktivieren von „Automatische Signatur“

Bevor Sie den automatischen Signierprozess starten, vergewissern Sie sich, dass Sie, wie im Leitfaden [Apple Account Management](~/cross-platform/macios/apple-account-management.md) beschrieben, in Visual Studio eine Apple-ID hinzugefügt haben. Nachdem Sie eine Apple-ID hinzugefügt haben, können Sie alle zugeordneten _Teams_ nutzen. So lassen sich Zertifikate, Profile und andere IDs dem Team zuzuordnen. Die Team-ID wird auch zum Erstellen eines App-ID-Präfix verwendet, das im Bereitstellungsprofil mit eingeschlossen werden soll. Dies erlaubt Apple sicherzustellen, dass Sie sind, wer Sie vorgeben zu sein.

> [!IMPORTANT]
> Melden Sie sich zunächst entweder bei [iTunes Connect](https://itunesconnect.apple.com/) oder bei [appleid.apple.com](https://appleid.apple.com) an, um sicherzustellen, dass Sie die aktuellen Apple-Kontorichtlinien akzeptiert haben. Führen Sie bei Aufforderung die erforderlichen Schritte zum Akzeptieren neuer Kontovereinbarungen von Apple aus. Wenn Sie die Datenschutzbestimmungen von Mai 2018 nicht akzeptieren, erhalten Sie beim Versuch der Gerätebereitstellung die folgende Warnung:
> ```
> Unexpected authentication failure. Reason: {
> "authType" : "sa"
>}
>```

Gehen Sie wie folgt vor, um Ihre App automatisch für die Bereitstellung auf einem iOS-Gerät zu signieren:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Öffnen Sie in Visual Studio für Mac ein iOS-Projekt.

2. Öffnen Sie die Datei **Info.plist**.

3. Wählen Sie im Abschnitt **Signierung** die Option **Automatische Bereitstellung**:

    ![Dropdownliste zur Teamauswahl](automatic-provisioning-images/image2.png)

4. Wählen Sie Ihr Team aus der Dropdownliste **Team** aus.

6. Nach wenigen Sekunden werden ein Signaturzertifikat und ein Bereitstellungsprofil erstellt:

    ![Erfolgreich erstelltes Zertifikat und Profil](automatic-provisioning-images/image5.png)

    Wenn das automatische Signieren fehlschlägt, wird im Abschnitt **Automatische Signatur** die Ursache des Fehlers angezeigt.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Koppeln Sie Visual Studio 2017 mit einem Mac, wie im Handbuch [Koppeln mit dem Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md) beschrieben.

2. Öffnen Sie die Bereitstellungsoptionen, indem Sie **Projekt > Bereitstellungseigenschaften...** auswählen.

3. Wählen Sie das Schema **Automatische Bereitstellung** aus:

    ![Auswahl des Schemas „Automatisch“](automatic-provisioning-images/prov4.png)

4. Wählen Sie Ihr Team im Kombinationsfeld **Team** aus, um das automatische Signieren zu starten.

    ![Auswahl des Teams](automatic-provisioning-images/prov3.png)

4. Dadurch wird der automatische Signierprozess gestartet. Visual Studio versucht anschließend, eine App-ID, ein Bereitstellungsprofil und eine Signaturidentität zu generieren, um diese Artefakte für die Signatur zu verwenden. Sie können den Generierungsvorgang in der Buildausgabe sehen:

    ![Buildausgabe, die die Generierung von Artefakten zeigt](automatic-provisioning-images/prov5.png)

-----

## <a name="triggering-automatic-provisioning"></a>Auslösen der automatischen Bereitstellung

Wenn das automatische Signieren aktiviert wurde, aktualisiert Visual Studio für Mac diese Artefakte bei Bedarf, sobald eines der folgenden Ereignisse eintritt:

* Ein iOS-Gerät wird an Ihren Mac angeschlossen
    - Hierbei wird automatisch geprüft, ob das Gerät im Apple Developer Portal registriert ist. Falls nicht, wird es hinzugefügt. Außerdem wird ein neues Bereitstellungsprofil generiert, das es enthält.
* Die Bündel-ID der App wird geändert.
    - Dadurch wird die App-ID aktualisiert. Ein neues Bereitstellungsprofil wird erstellt, das diese App-ID enthält.
* Eine unterstützte Funktion wird in der Datei „Entitlements.plist“ aktiviert.
    - Diese Funktion wird der App-ID hinzugefügt, und es wird ein neues Bereitstellungsprofil mit der aktualisierten App-ID erstellt.
    - Nicht alle Funktionen werden derzeit unterstützt. Weitere Informationen zu den unterstützten Funktionen finden Sie in der Anleitung zum [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md).


## <a name="related-links"></a>Verwandte Links

- [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [App-Verteilung](~/ios/deploy-test/app-distribution/index.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
- [Apple: Leitfaden zur App-Verteilung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
