---
title: Automatische Bereitstellung für Xamarin.iOS
description: Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der Option „Automatische Signatur“, um Entwicklungszertifikate und -profile anzufordern.
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.custom: video
ms.date: 03/05/2020
ms.openlocfilehash: 069c40b74876bea1d3a0c8fca23b3d90c4b91635
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "79510678"
---
# <a name="automatic-provisioning-for-xamarinios"></a>Automatische Bereitstellung für Xamarin.iOS

_Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. In diesem Leitfaden wird die Verwendung der automatischen Bereitstellung zum Anfordern von Entwicklungszertifikaten und -profilen erläutert._

## <a name="requirements"></a>Anforderungen

Die automatische Bereitstellung steht in Visual Studio für Mac, Visual Studio 2019 und Visual Studio 2017 (Version 15.7 und höher) zur Verfügung. 

> [!NOTE]
> Sie müssen außerdem über ein kostenpflichtiges Apple Developer-Konto verfügen, um dieses Feature nutzen zu können. Weitere Informationen zu Apple Developer-Konten finden Sie im Leitfaden [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md).
> Wenn Sie über kein kostenpflichtiges Apple-Entwicklerkonto verfügen, finden Sie weitere Informationen im Leitfaden zur [kostenlosen Bereitstellung für Xamarin.iOS](~/ios/get-started/installation/device-provisioning/free-provisioning.md).

> [!NOTE]
> Bevor Sie beginnen, müssen Sie zunächst alle Lizenzvereinbarungen im [Apple Developer Portal](https://developer.apple.com/account/) oder in [App Store Connect](https://appstoreconnect.apple.com/) akzeptieren.


## <a name="enable-automatic-provisioning"></a>Aktivieren der automatischen Bereitstellung

Vergewissern Sie sich vor dem Start des Prozesses zum automatischen Signieren, dass Sie gemäß Anweisungen im Leitfaden [Apple-Kontoverwaltung](~/cross-platform/macios/apple-account-management.md) eine Apple-ID in Visual Studio hinzugefügt haben. 

Nachdem Sie eine Apple-ID hinzugefügt haben, können Sie alle zugeordneten _Teams_ nutzen. So lassen sich Zertifikate, Profile und andere IDs dem Team zuzuordnen. Die Team-ID wird auch verwendet, um ein Präfix für eine App-ID zu erstellen, die in das Bereitstellungsprofil aufgenommen wird. Dies erlaubt Apple sicherzustellen, dass Sie sind, wer Sie vorgeben zu sein.

Gehen Sie wie folgt vor, um Ihre App automatisch für die Bereitstellung auf einem iOS-Gerät zu signieren:

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie in Visual Studio für Mac ein iOS-Projekt.

2. Öffnen Sie die Datei **Info.plist**.

3. Wählen Sie im Abschnitt **Signierung** die Option **Automatische Bereitstellung** aus:

    ![Dropdownliste zur Teamauswahl](automatic-provisioning-images/image2.png)

4. Wählen Sie Ihr Team aus der Dropdownliste **Team** aus.

5. Nach wenigen Sekunden werden ein Signaturzertifikat und ein Bereitstellungsprofil erstellt:

    ![Erfolgreich erstelltes Zertifikat und Profil](automatic-provisioning-images/image5.png)

    Wenn das automatische Signieren fehlschlägt, wird im Abschnitt **Automatische Signatur** die Ursache des Fehlers angezeigt.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Wenn Sie Visual Studio 2017 oder Visual Studio 2019 (Version 16.4 und früher) verwenden, muss eine [Kopplung mit einem Mac-Buildhost](~/ios/get-started/installation/windows/connecting-to-mac/index.md) durchgeführt werden, bevor Sie den Vorgang fortsetzen.

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den iOS-Projektnamen, und wählen Sie **Eigenschaften** aus. Navigieren Sie zur Registerkarte **iOS-Bundlesignierung**:

    ![Screenshot der Seite zur Bundlesignierung in den iOS-Eigenschaften](automatic-provisioning-images/bundle-signing-win.png)

2. Wählen Sie das Schema **Automatische Bereitstellung** aus.

3. Wählen Sie Ihr im Dropdownmenü **Team** Ihr Team aus, um das automatische Signieren zu starten. Visual Studio zeigt an, ob der Prozess erfolgreich abgeschlossen wurde:

    ![Screenshot der Seite zur Bundlesignierung mit Hervorhebung der Meldung „Die automatische Bereitstellung wurde erfolgreich abgeschlossen“](automatic-provisioning-images/signing-success-win.png)

-----

## <a name="run-automatic-provisioning"></a>Ausführen der automatischen Bereitstellung

Wenn die automatische Bereitstellung aktiviert ist, führt Visual Studio den Prozess bei Bedarf erneut aus, wenn einer der folgenden Fälle eintritt:

- Ein iOS-Gerät wird an Ihren Mac angeschlossen
  - Hierbei wird automatisch geprüft, ob das Gerät im Apple Developer Portal registriert ist. Liegt keine Registrierung vor, wird das Gerät hinzugefügt. Außerdem wird ein neues Bereitstellungsprofil generiert, das das Gerät enthält.
- Die Bündel-ID der App wird geändert.
  - Dadurch wird die App-ID aktualisiert. Ein neues Bereitstellungsprofil wird erstellt, das diese App-ID enthält.
- Eine unterstützte Funktion wird in der Datei „Entitlements.plist“ aktiviert.
  - Diese Funktion wird der App-ID hinzugefügt, und es wird ein neues Bereitstellungsprofil mit der aktualisierten App-ID erstellt.
  - Nicht alle Funktionen werden derzeit unterstützt. Weitere Informationen zu den unterstützten Funktionen finden Sie in der Anleitung zum [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md).

## <a name="wildcard-app-ids"></a>Platzhalter-App-IDs

In Visual Studio für Mac und Visual Studio 2019 (Version 16.5 oder höher) wird bei der automatischen Bereitstellung standardmäßig versucht, eine Platzhalter-App-ID und Bereitstellungsprofile zu erstellen und zu verwenden, anstatt eine explizite App-ID basierend auf dem in der Datei **Info.plist** angegebenen **Bundlebezeichner** zu erstellen. Platzhalter-App-IDs verringern die Anzahl der Profile und IDs, die im Apple Developer Portal verwaltet werden müssen.

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

Wenn Ihre App eine dieser Berechtigungen verwendet, versucht Visual Studio, eine explizite App-ID (anstelle einer Platzhalter-App-ID) zu erstellen.

## <a name="troubleshoot"></a>Problembehandlung 

- Es kann mehrere Stunden dauern, bis ein neues Apple Developer-Konto genehmigt wird. Sie können die automatische Bereitstellung erst aktivieren, nachdem Ihr Konto genehmigt wurde.
- Wenn die automatische Bereitstellung nicht erfolgreich ist und die Fehlermeldung `Authentication Service Is Unavailable` angezeigt wird, melden Sie sich entweder bei [App Store Connect](https://appstoreconnect.apple.com/) oder [appleid.apple.com](https://appleid.apple.com) an, um zu überprüfen, ob Sie die neuesten Dienstvereinbarungen akzeptiert haben.
- Falls die Fehlermeldung `Authentication Error: Xcode 7.3 or later is required to continue developing with your Apple ID.` angezeigt wird, müssen Sie sicherstellen, dass das ausgewählte Team über eine aktive kostenpflichtige Mitgliedschaft beim Apple Developer Program verfügt. Informationen zur Verwendung eines kostenpflichtigen Apple Developer-Kontos finden Sie im Leitfaden [Kostenlose Bereitstellung für Xamarin.iOS-Apps](~/ios/get-started/installation/device-provisioning/free-provisioning.md).

## <a name="related-links"></a>Verwandte Links

- [Free Provisioning (Kostenlose Bereitstellung)](~/ios/get-started/installation/device-provisioning/free-provisioning.md)
- [App-Verteilung](~/ios/deploy-test/app-distribution/index.md)
- [Problembehandlung](~/ios/deploy-test/troubleshooting.md)
- [Apple: Leitfaden zur App-Verteilung](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html)
