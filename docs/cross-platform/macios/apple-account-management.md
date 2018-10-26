---
title: Apple-Kontoverwaltung
description: Dieses Dokument beschreibt, wie Sie die Apple-Konto-Verwaltungsfunktionen in Visual Studio für Mac und Visual Studio 2017 verwenden.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 1e353aceaf0e2c0525b82c0ccb7e7bcb73df3075
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50106064"
---
# <a name="apple-account-management"></a>Apple-Kontoverwaltung

Die Verwaltungsschnittstelle des Apple-Konto bietet eine Möglichkeit, alle Entwicklungsteams, die mit einer Apple ID verknüpften anzeigen Außerdem können Sie weitere Informationen über jedes Team anzeigen, indem Sie mit einer Liste der _Signierungsidentitäten_ und _Bereitstellungsprofile_ , die auf Ihrem Computer installiert sind.

Die Authentifizierung der Apple-ID erfolgt in der Befehlszeile mit [Fastlane](https://fastlane.tools/). FastLane muss installiert sein, auf dem Computer für Sie erfolgreich authentifiziert werden. Weitere Informationen zu Fastlane und zur Installation finden Sie der [Fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) Anleitungen.

Das Dialogfeld "Apple-Konto" können Sie die folgenden Schritte ausführen:

* **Erstellen und Verwalten von Zertifikaten**
* **Erstellen und Verwalten von Bereitstellungsprofilen**

Informationen zu diesem Zweck wird in diesem Handbuch beschrieben.

> [!NOTE]
> Xamarin Tools für die Verwaltung für Apple-Konten werden nur Informationen über kostenpflichtige Apple-entwicklerkonten angezeigt. Informationen zum Testen einer app auf einem Gerät ohne einen kostenpflichtigen Apple-Entwicklerkonto, informieren Sie sich die [für Xamarin.iOS-apps die freie Bereitstellung](~/ios/get-started/installation/device-provisioning/free-provisioning.md) Guide.

Sie können auch die automatische Bereitstellung von Tools für die iOS automatisch erstellen und verwalten Ihre Signierungsidentitäten, App-IDs und Bereitstellungsprofile. Weitere Informationen zur Verwendung dieser Funktionen finden Sie in der [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Guide.

## <a name="requirements"></a>Anforderungen

Apple-kontoverwaltung finden Sie in Visual Studio für Mac und Visual Studio 2017 (Version 15.7 und höher)

Sie benötigen ein Apple Developer-Konto, um dieses Feature zu verwenden. Weitere Informationen zum Apple-entwicklerkonten steht in der [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Guide.

- Stellen Sie sicher, dass Sie mit dem Internet verbunden sind. Dies ist da Fastlane direkt mit der Apple-Entwicklerportal kommuniziert.
- Stellen Sie sicher, Sie haben [Fastlane-Tools installiert](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Stellen Sie sicher, Sie haben die neuesten Fastlane-Tools von [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Bevor Sie beginnen, stellen Sie sicher, akzeptieren Sie alle Benutzer Lizenzverträge in die [Entwicklerportal](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Hinzufügen einer Apple-Entwicklerkonto

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Dialogfeld für die kontoverwaltung öffnen wechseln Sie zu **Visual Studio > Einstellungen > Apple-Entwicklerkonto**:

    ![Optionen für die Apple-Entwicklerkonto](apple-account-management-images/image1.png)

2. Drücken Sie die **+** Schaltfläche, um die Anmeldung im Dialogfeld angezeigt, wie unten dargestellt: 

    ![FastLane-Dialogfeld.](apple-account-management-images/image2.png)

4. Geben Sie Ihre Apple-ID und Kennwort ein, und klicken Sie auf die **Anmeldung** Schaltfläche. Dadurch werden Ihre Anmeldeinformationen in der sicheren Keychain auf diesem Computer speichern. [FastLane](~/ios/deploy-test/provisioning/fastlane/index.md) wird verwendet, um Ihre Anmeldeinformationen sicher verarbeitet und an Apple Developer Portal übergeben werden.
 
5. Wählen Sie **immer zulassen** auf das Dialogfeld "Warnung" zu Visual Studio, um die Anmeldeinformationen zu verwenden:

    ![Dialogfeld "Warnung" immer zulassen](apple-account-management-images/image4.png)

6. Sobald Ihr Konto erfolgreich hinzugefügt wurde, sehen Sie sich Ihre Apple-ID und alle Teams, denen Ihre Apple-ID gehört.

    ![Apple Developer-Konto-Dialogfeld mit Konten hinzugefügt](apple-account-management-images/image5.png)

7. Wählen Sie alle Teams, und drücken Sie die **Details anzeigen...** Bilderstellungsschaltfläche (...). Dies zeigt eine Liste aller von Signierungsidentitäten und Bereitstellungsprofile, die auf Ihrem Computer installiert sind:

    ![Ansicht Details Bildschirm, mit dem codesignieridentitäten und bereitstellungsprofile auf Ihrem Computer](apple-account-management-images/image6.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Bevor Sie beginnen, Ihre Apple-ID für Visual Studio 2017 hinzufügen, stellen Sie sicher, dass Ihre Entwicklungsumgebung [gekoppelt mit einem Mac-buildhost](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Um die Konto-Management-Fenster öffnen, wechseln Sie zu **Tools > Optionen > Xamarin > Apple-Konten**:

    ![Optionsbildschirm von Apple-Konten](apple-account-management-images/prov1.png)

1. Wählen Sie die **hinzufügen** und geben Sie Ihre Apple-ID und Kennwort:

    ![Dialogfeld "UserName" und "Kennwort"](apple-account-management-images/prov1a.png)

1. Sobald Ihr Konto erfolgreich hinzugefügt wurde, sehen Sie sich Ihre Apple-ID und alle Teams, denen Ihre Apple-ID gehört.
 
1. Wählen Sie alle Teams, und drücken Sie die **Details anzeigen...** Bilderstellungsschaltfläche (...). Dies zeigt eine Liste aller von Signierungsidentitäten und Bereitstellungsprofile, die auf Ihrem Computer installiert sind:

    ![Dialogfeld "UserName" und "Kennwort"](apple-account-management-images/prov2.png)

-----


## <a name="managing-signing-identities-and-provisioning-profiles"></a>Verwalten von Signierungsidentitäten und Bereitstellungsprofile

Das Team-Dialogfeld "Details" zeigt eine Liste von Signierungsidentitäten, nach Typ strukturiert. Die **Status** Spalte werden Sie darüber informiert, wenn das Zertifikat ist: 

* **Gültige** – die signierungsidentität (das Zertifikat und den privaten Schlüssel) auf Ihrem Computer installiert ist und nicht abgelaufen.

* **Nicht in Keychain** – eine gültige signierungsidentität auf Apple-Server vorhanden ist. Um dies auf Ihrem Computer zu installieren, müssen sie von einem anderen Computer exportiert werden. Sie können nicht die signierungsidentität aus dem Apple-Entwicklerportal herunterladen, da sie nicht den privaten Schlüssel enthält.

* **Privater Schlüssel fehlt** – ein Zertifikat ohne privaten Schlüssel im Schlüsselbund installiert ist.

* **Abgelaufene** – das Zertifikat ist abgelaufen. Sie sollten dies aus Ihrer Schlüsselsammlung entfernen.

  ![Teaminformationen Details-Dialogfeld](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Erstellen Sie eine Signierungsidentitäten

Um eine neue signierungsidentität erstellen möchten, wählen die **Create Certificate** Dropdown-Schaltfläche und wählen Sie den Typ an, die erforderlich sind. Wenn Sie die richtigen Berechtigungen ein neues Signaturzertifikat haben wird die Identität nach wenigen Sekunden angezeigt.

Wenn eine Option in der Dropdownliste abgeblendet und deaktiviert, bedeutet dies, dass Sie nicht das richtige Team zum Erstellen dieser Art des Zertifikats berechtigt sind.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Create Certificate-Optionen](apple-account-management-images/image8.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Create Certificate-Optionen](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Herunterladen von Bereitstellungsprofilen

Das Team-Dialogfeld "Details" zeigt außerdem eine Liste aller provisioning Profile, die mit Ihrem Developer-Konto verbunden. Sie können alle bereitstellungsprofile auf Ihrem lokalen Computer herunterladen, durch Drücken der **alle Profile herunterladen** Schaltfläche

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Abschnitt der provisioning Profile herunterladen](apple-account-management-images/image9.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Abschnitt der provisioning Profile herunterladen](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>iOS Bundle Signing

Informationen zum Bereitstellen Ihrer app auf einem Gerät, finden Sie in der [gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Guide.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="view-details-dialog-is-empty"></a>Dialogfeld "Details anzeigen" ist leer

Dies ist derzeit ein bekanntes Problem, das im Zusammenhang mit Fehler [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Stellen Sie sicher, dass Sie die neueste stabile Version von Visual Studio für Mac verwenden

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Wenn Sie in Ihrem Konto anmelden Probleme auftreten, versuchen Sie Folgendes:

* Öffnen Sie die Keychain-Anwendung, und wählen Sie unter der Kategorie *Kennwörter*. Suchen Sie nach `deliver.`, und löschen Sie alle Einträge.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Fehler beim Hinzufügen des Kontos. Melden Sie sich mit einem app-spezifische-Kennwort"

Dies ist da 2-Factor Authentication für Ihr Konto aktiviert ist. Stellen Sie sicher, dass Sie die neueste stabile Version von Visual Studio für Mac verwenden

### <a name="failed-to-create-new-certificate"></a>Fehler beim Erstellen des neuen Zertifikats
"Sie haben das Limit für Zertifikate dieses Typs erreicht."

![Dialogfeld "Zertifikat-Limit"](apple-account-management-images/image10.png)

Die maximale Anzahl von zulässigen Zertifikate wurden generiert. Um dieses Problem zu beheben, navigieren Sie zu der [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) und widerrufen Sie eines der Zertifikate für die Produktion.

## <a name="known-issues"></a>Bekannte Probleme

* Das Verteilen von Bereitstellungsprofilen verwendet standardmäßig den App Store als Ziel. In-House- oder Ad-hoc-Profile sollten manuell erstellt werden.
