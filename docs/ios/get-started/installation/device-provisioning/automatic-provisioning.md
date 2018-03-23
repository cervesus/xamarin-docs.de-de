---
title: Automatische Bereitstellung
description: Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der Option „Automatische Signatur“ in Visual Studio für Mac, um Entwicklungszertifikate und -profile anzufordern.
ms.topic: article
ms.prod: xamarin
ms.assetid: 81FCB2ED-687C-40BC-ABF1-FB4303034D01
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 11/17/2017
ms.openlocfilehash: 271d9e3f7ae04f03a132ae2fd0ebf531fe52578c
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/22/2018
---
# <a name="automatic-provisioning"></a>Automatische Bereitstellung

_Sobald Xamarin.iOS erfolgreich installiert wurde, ist der nächste Schritt in der iOS-Entwicklung das Bereitstellen des iOS-Geräts. Dieses Handbuch beschreibt die Verwendung der Option „Automatische Signatur“ in Visual Studio für Mac, um Entwicklungszertifikate und -profile anzufordern._

## <a name="requirements"></a>Anforderungen

- Visual Studio für Mac 7.3 oder höher
- Xcode 9 oder höher

> [!IMPORTANT]
> Dieser Leitfaden veranschaulicht, wie Sie Visual Studio für Mac verwenden, um ein Apple-Gerät für die Bereitstellung einzurichten, und wie Sie eine Anwendung bereitstellen. Wie sie dies manuell oder mit Visual Studio unter Windows tun können, erfahren Sie in den detaillierten Schritte im Leitfaden für die [manuelle Bereitstellung](~/ios/get-started/installation/device-provisioning/manual-provisioning.md).

## <a name="enabling-automatic-signing"></a>Aktivieren von „Automatische Signatur“

Bevor Sie den automatischen Signierprozess starten, vergewissern Sie sich, dass Sie, wie im Leitfaden [Apple Account Management](~/cross-platform/macios/apple-account-management.md) beschrieben, in Visual Studio für Mac eine Apple-ID hinzugefügt haben. Nachdem Sie eine Apple-ID hinzugefügt haben, können Sie alle zugeordneten _Teams_ nutzen. So lassen sich Zertifikate, Profile und andere IDs dem Team zuzuordnen. Die Team-ID wird auch zum Erstellen eines App-ID-Präfix verwendet, das im Bereitstellungsprofil mit eingeschlossen werden soll. Dies erlaubt Apple sicherzustellen, dass Sie sind, wer Sie vorgeben zu sein.

Gehen Sie wie folgt vor, um Ihre App automatisch für die Bereitstellung auf einem iOS-Gerät zu signieren:

1. Öffnen Sie in Visual Studio für Mac ein iOS-Projekt.

2. Öffnen Sie die Datei **Info.plist**.

3. Wählen Sie im Abschnitt **Signierung** die Option **Automatische Bereitstellung**:

    ![Dropdownliste zur Teamauswahl](automatic-provisioning-images/image2.png)

4. Wählen Sie Ihr Team aus der Dropdownliste **Team** aus.

6. Nach wenigen Sekunden werden ein Signaturzertifikat und ein Bereitstellungsprofil erstellt:

    ![Erfolgreich erstelltes Zertifikat und Profil](automatic-provisioning-images/image5.png)

    Wenn das automatische Signieren fehlschlägt, wird im Abschnitt **Automatische Signatur** die Ursache des Fehlers angezeigt.

## <a name="triggering-automatic-provisioning"></a>Auslösen der automatischen Bereitstellung

Wenn das automatische Signieren aktiviert wurde, aktualisiert Visual Studio für Mac diese Artefakte bei Bedarf, sobald eines der folgenden Ereignisse eintritt:

* Ein iOS-Gerät wird an Ihren Mac angeschlossen.
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
