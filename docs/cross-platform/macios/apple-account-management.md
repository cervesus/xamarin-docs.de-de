---
title: Apple-Kontoverwaltung
description: In diesem Dokument wird beschrieben, wie die Features der Apple-Kontoverwaltung in Visual Studio für Mac und Visual Studio 2019 verwendet werden.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: davidortinau
ms.author: daortin
ms.date: 05/06/2018
ms.openlocfilehash: 81f161442b33eee94f32c506947ed029fd40aadb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016345"
---
# <a name="apple-account-management"></a>Apple-Kontoverwaltung

Die Apple-Konto Verwaltungsschnittstelle bietet eine Möglichkeit, alle Entwicklungsteams anzuzeigen, die einer Apple-ID zugeordnet sind. Außerdem können Sie weitere Details zu den einzelnen Teams anzeigen, indem Sie eine Liste der _Signierungs Identitäten_ und _Bereitstellungs profile_ anzeigen, die auf Ihrem Computer installiert sind.

Die Authentifizierung Ihrer Apple-ID erfolgt in der Befehlszeile mit [Fastlane](https://fastlane.tools/). FastLane muss auf Ihrem Computer installiert sein, damit Sie erfolgreich authentifiziert werden können. Weitere Informationen zu Fastlane und zur Installation finden Sie in den [Fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) -Handbüchern.

Im Dialogfeld Apple-Konto können Sie folgende Aufgaben ausführen:

- **Erstellen und Verwalten von Zertifikaten**
- **Erstellen und Verwalten von Bereitstellungs Profilen**

Informationen zur Vorgehensweise finden Sie in diesem Handbuch.

> [!NOTE]
> Mit den Tools für die Apple-Kontoverwaltung von xamarin werden nur Informationen zu kostenpflichtigen Apple-Entwickler Konten angezeigt. Weitere Informationen zum Testen einer APP auf einem Gerät ohne kostenpflichtiges Apple-Entwicklerkonto finden Sie im Handbuch zur [kostenlosen Bereitstellung für xamarin. IOS-apps](~/ios/get-started/installation/device-provisioning/free-provisioning.md) .

Sie können auch die automatischen IOS-Bereitstellungs Tools verwenden, um Ihre Signierungs Identitäten, App-IDs und Bereitstellungs Profile automatisch zu erstellen und zu verwalten. Weitere Informationen zur Verwendung dieser Features finden Sie im Handbuch zur [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) .

## <a name="requirements"></a>Anforderungen

Die Apple-Kontoverwaltung ist auf Visual Studio für Mac, Visual Studio 2019 und Visual Studio 2017 (Version 15,7 und höher) verfügbar.

Sie müssen über ein Apple-Entwicklerkonto verfügen, um diese Funktion verwenden zu können. Weitere Informationen zu Apple-Entwickler Konten finden Sie im Handbuch zur [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) .

- Stellen Sie sicher, dass Sie mit dem Internet verbunden sind. Der Grund hierfür ist, dass Fastlane direkt mit dem Apple Developer Portal kommuniziert.
- Stellen Sie sicher, dass [Fastlane-Tools installiert](~/ios/deploy-test/provisioning/fastlane/index.md#Installation)sind.
- Stellen Sie sicher, dass Sie über [https://download.fastlane.tools](https://download.fastlane.tools)die neuesten Fastlane-Tools verfügen.
- Bevor Sie beginnen, stellen Sie sicher, dass Sie alle Benutzer Lizenzverträge im [Entwickler Portal](https://developer.apple.com/account/)akzeptieren.

## <a name="adding-an-apple-developer-account"></a>Hinzufügen eines Apple-Entwickler Kontos

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Wechseln Sie zum Öffnen des Dialog Felds "Kontoverwaltung" zu **Visual Studio > Einstellungen > Apple-Entwicklerkonto**:

    ![Apple Developer-Konto Optionen](apple-account-management-images/image1.png)

2. Klicken Sie auf die Schaltfläche **+** , um das Anmelde Dialogfeld anzuzeigen, wie unten dargestellt: 

    ![FastLane-Dialogfeld.](apple-account-management-images/image2.png)

3. Geben Sie Ihre Apple ID und Ihr Kennwort ein, und klicken Sie auf die Schaltfläche **Anmelden** Dadurch werden Ihre Anmelde Informationen in der sicheren Schlüsselkette auf diesem Computer gespeichert. [Fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) wird zur sicheren Verarbeitung Ihrer Anmelde Informationen verwendet und übergibt sie an das Apple-Entwickler Portal.

4. Wählen Sie im Warnungs Dialogfeld **immer zulassen** aus, damit Visual Studio Ihre Anmelde Informationen verwenden kann:

    ![Dialogfeld "Warnung immer zulassen"](apple-account-management-images/image4.png)

5. Nachdem Ihr Konto erfolgreich hinzugefügt wurde, sehen Sie Ihre Apple-ID und alle Teams, zu denen Ihre Apple-ID gehört.

    ![Dialogfeld "Apple Developer Account" mit hinzugefügten Konten](apple-account-management-images/image5.png)

6. Wählen Sie ein beliebiges Team aus, und klicken Sie **auf Details anzeigen.** Schaltfläche. Dadurch wird eine Liste aller Signierungs Identitäten und Bereitstellungs Profile angezeigt, die auf Ihrem Computer installiert sind:

    ![Anzeige der Detail Anzeige der Signierungs Identitäten und Bereitstellungs Profile auf Ihrem Computer](apple-account-management-images/image6.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Bevor Sie mit dem Hinzufügen Ihrer Apple-ID zu Visual Studio 2019 beginnen, sollten Sie sicherstellen, dass Ihre Entwicklungsumgebung [mit einem Mac-buildhost gekoppelt](~/ios/get-started/installation/windows/connecting-to-mac/index.md)ist.

1. Um das Fenster "Kontoverwaltung" zu öffnen, navigieren Sie zu Extras **> Optionen > xamarin > Apple-Konten**:

    ![Dialogfeld "Optionen" für Apple](apple-account-management-images/prov1.png)

1. Wählen Sie die Schaltfläche **Hinzufügen** aus, und geben Sie Ihre Apple-ID und Ihr

    ![Benutzername und Kennwort](apple-account-management-images/prov1a.png)

1. Nachdem Ihr Konto erfolgreich hinzugefügt wurde, sehen Sie Ihre Apple-ID und alle Teams, zu denen Ihre Apple-ID gehört.

1. Wählen Sie ein beliebiges Team aus, und klicken Sie **auf Details anzeigen.** Schaltfläche. Dadurch wird eine Liste aller Signierungs Identitäten und Bereitstellungs Profile angezeigt, die auf Ihrem Computer installiert sind:

    ![Benutzername und Kennwort](apple-account-management-images/prov2.png)

-----

## <a name="managing-signing-identities-and-provisioning-profiles"></a>Verwalten von Signierungs Identitäten und Bereitstellungs Profilen

Im Dialogfeld "Team Details" wird eine Liste der Signierungs Identitäten nach Typ angeordnet angezeigt. In der Spalte **Status** werden Sie darüber informiert, ob das Zertifikat: 

- **Gültig** – die Signierungs Identität (sowohl das Zertifikat als auch der private Schlüssel) wird auf dem Computer installiert und ist nicht abgelaufen.

- **Nicht in Keychain** – es gibt eine gültige Signatur Identität auf dem Server von Apple. Um diese auf Ihrem Computer zu installieren, muss Sie von einem anderen Computer exportiert werden. Die Signierungs Identität kann nicht aus dem Apple-Entwickler Portal heruntergeladen werden, da Sie nicht den privaten Schlüssel enthält.

- Der **private Schlüssel fehlt** – ein Zertifikat ohne privaten Schlüssel ist in der Keychain installiert.

- **Abgelaufen** – das Zertifikat ist abgelaufen. Entfernen Sie diese aus ihrer Keychain.

  ![Team Details-Dialog Informationen](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Erstellen von Signierungs Identitäten

Wählen Sie zum Erstellen einer neuen Signierungs Identität die Dropdown Schaltfläche **Zertifikat erstellen** aus, und wählen Sie den gewünschten Typ aus. Wenn Sie über die richtigen Berechtigungen verfügen, wird eine neue Signierungs Identität nach einigen Sekunden angezeigt.

Wenn eine Option in der Dropdown-Dropdown-Datei abgeblendet und deaktiviert ist, bedeutet dies, dass Sie nicht über die richtigen Team Berechtigungen verfügen, um diese Art von Zertifikat zu erstellen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Erstellen von Zertifikat Optionen](apple-account-management-images/image8.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Erstellen von Zertifikat Optionen](apple-account-management-images/prov3.png)

-----

## <a name="download-provisioning-profiles"></a>Herunterladen von Bereitstellungs Profilen

Im Dialogfeld "Team Details" wird auch eine Liste aller Bereitstellungs Profile angezeigt, die mit Ihrem Entwicklerkonto verbunden sind. Sie können alle Bereitstellungs Profile auf Ihren lokalen Computer herunterladen, indem Sie die Schaltfläche **alle Profile herunterladen** drücken.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Abschnitt "Bereitstellungs profile herunterladen"](apple-account-management-images/image9.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Abschnitt "Bereitstellungs profile herunterladen"](apple-account-management-images/prov4.png)

-----

## <a name="ios-bundle-signing"></a>IOS-Bündel Signierung

Informationen zum Bereitstellen der APP auf einem Gerät finden Sie im Handbuch zur [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) .

## <a name="troubleshooting"></a>Problembehandlung

### <a name="view-details-dialog-is-empty"></a>Dialogfeld ' Details anzeigen ' ist leer

Dies ist zurzeit ein bekanntes Problem im Zusammenhang mit Fehler [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Stellen Sie sicher, dass Sie die neueste stabile Version von verwenden Visual Studio für Mac

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Wenn bei der Protokollierung Ihres Kontos Probleme auftreten, versuchen Sie Folgendes:

- Öffnen Sie die Anwendung keychain, und wählen Sie Unterkategorie die Option Kenn *Wörter* Suchen Sie nach `deliver.`, und löschen Sie alle Einträge.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Fehler beim Hinzufügen des Kontos. Melden Sie sich mit einem App-spezifischen Kennwort an.

Dies liegt daran, dass die zweistufige Authentifizierung für Ihr Konto aktiviert ist. Stellen Sie sicher, dass Sie die neueste stabile Version von verwenden Visual Studio für Mac

### <a name="failed-to-create-new-certificate"></a>Fehler beim Erstellen des neuen Zertifikats.
"Sie haben das Limit für Zertifikate dieses Typs erreicht"

![Dialogfeld "Zertifikat Limit](apple-account-management-images/image10.png)

Die maximal zulässige Anzahl zulässiger Zertifikate wurde generiert. Um dieses Problem zu beheben, navigieren Sie zum [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) , und widerrufen Sie eines der Produktions Zertifikate.

## <a name="known-issues"></a>Bekannte Probleme

- Das Verteilen von Bereitstellungsprofilen verwendet standardmäßig den App Store als Ziel. In-House- oder Ad-hoc-Profile sollten manuell erstellt werden.
