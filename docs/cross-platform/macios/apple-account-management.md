---
title: Apple-Kontoverwaltung
description: In diesem Dokument wird beschrieben, wie die Features der Apple-Kontoverwaltung in Visual Studio für Mac und Visual Studio 2019 verwendet werden.
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
author: davidortinau
ms.author: daortin
ms.date: 03/05/2020
ms.openlocfilehash: 17607e09a141fd29cd81cde93d812b20e62a9af8
ms.sourcegitcommit: 60d2243809d8e980fca90b9f771e72f8c0e64d71
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "78946253"
---
# <a name="apple-account-management"></a>Apple-Kontoverwaltung

Die Apple-Kontoverwaltungs-Schnittstelle in Visual Studio bietet eine Möglichkeit, Informationen für Entwicklungsteams anzuzeigen, die einer Apple-ID zugeordnet sind. Sie können folgende Aktionen ausführen:

- **Hinzufügen von Apple-Entwickler Konten**
- **Anzeigen von Signatur Zertifikaten und Bereitstellungs Profilen**
- **Neue Signatur Zertifikate erstellen**
- **Vorhandene Bereitstellungs profile herunterladen**

> [!IMPORTANT]
> Mit den Tools für die Apple-Kontoverwaltung von xamarin werden nur Informationen zu kostenpflichtigen Apple-Entwickler Konten angezeigt. Weitere Informationen zum Testen einer APP auf einem Gerät ohne kostenpflichtiges Apple-Entwicklerkonto finden Sie im Handbuch zur [kostenlosen Bereitstellung für xamarin. IOS-apps](~/ios/get-started/installation/device-provisioning/free-provisioning.md) .

## <a name="requirements"></a>Requirements (Anforderungen)

Die Apple-Kontoverwaltung ist auf Visual Studio für Mac, Visual Studio 2019 und Visual Studio 2017 (Version 15,7 und höher) verfügbar. Sie müssen auch über ein kostenpflichtiges Apple-Entwicklerkonto verfügen, um dieses Feature zu verwenden. Weitere Informationen zu Apple-Entwickler Konten finden Sie im Handbuch zur [Geräte Bereitstellung](~/ios/get-started/installation/device-provisioning/index.md) .

> [!NOTE]
> Bevor Sie beginnen, müssen Sie zunächst alle Benutzer Lizenzverträge im [Apple Developer Portal](https://developer.apple.com/account/)akzeptieren.

## <a name="add-an-apple-developer-account"></a>Hinzufügen eines Apple-Entwickler Kontos

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Wechseln Sie zu **Visual Studio > Einstellungen > Apple Developer Account** , und klicken Sie auf die Schaltfläche **+** , um das Anmelde Dialogfeld zu öffnen:

    ![Askreershot der Seite mit den Apple-Entwickler Konten in Visual Studio für Mac Einstellungen.](apple-account-management-images/add-account-vsm.png)

2. Geben **Sie Ihre**Apple-ID und Ihr Kennwort ein Dadurch werden Ihre Anmelde Informationen in der sicheren Schlüsselkette auf diesem Computer gespeichert.

3. Wählen Sie im Warnungs Dialogfeld **immer zulassen** aus, damit Visual Studio Ihre Anmelde Informationen verwenden kann:

    ![Dialogfeld "Warnung immer zulassen"](apple-account-management-images/image4.png)

4. Nachdem Ihr Konto erfolgreich hinzugefügt wurde, werden Ihre Apple-ID und alle Teams angezeigt, zu denen Ihre Apple-ID gehört:

    ![Dialogfeld "Apple Developer Account" mit hinzugefügten Konten](apple-account-management-images/image5.png)

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

> [!NOTE]
> Wenn Sie Visual Studio 2017 oder Visual Studio 2019 (Version 16,4 und älter) verwenden, müssen Sie [mit einem Mac-buildhost gekoppelt](~/ios/get-started/installation/windows/connecting-to-mac/index.md) werden, bevor Sie fortfahren.

1. Wechseln Sie zu Extras **> Optionen > xamarin > Apple-Konten** , und klicken Sie auf **Hinzufügen**:

    ![Screenshot der Seite mit den Apple-Konten in den Visual Studio-Optionen.](apple-account-management-images/add-account-vsw.png)

2. Geben Sie Ihre Apple-ID und Ihr Kennwort ein, **und klicken Sie**

3. Nachdem Ihr Konto erfolgreich hinzugefügt wurde, sehen Sie Ihre Apple-ID und alle Teams, zu denen Ihre Apple-ID gehört:

    ![Screenshot der Seite "Entwicklerkonto" mit hinzugefügten Konten](apple-account-management-images/accounts-vsw.png)

-----

## <a name="view-signing-certificates-and-provisioning-profiles"></a>Anzeigen von Signatur Zertifikaten und Bereitstellungs Profilen

Wählen Sie ein Team aus, und klicken Sie auf **Details anzeigen...** zum Öffnen eines Dialog Felds, in dem eine Liste der Signierungs Identitäten und Bereitstellungs Profile angezeigt wird, die auf Ihrem Computer installiert sind.

Im Dialogfeld "Team Details" wird eine Liste der Signierungs Identitäten nach Typ angeordnet angezeigt. In der Spalte **Status** werden Sie darüber informiert, ob das Zertifikat: 

- **Gültig** – die Signierungs Identität (sowohl das Zertifikat als auch der private Schlüssel) wird auf dem Computer installiert und ist nicht abgelaufen.

- **Nicht in Keychain** – es gibt eine gültige Signatur Identität auf dem Server von Apple. Um diese auf Ihrem Computer zu installieren, muss Sie von einem anderen Computer exportiert werden. Die Signierungs Identität kann nicht aus dem Apple-Entwickler Portal heruntergeladen werden, da Sie nicht den privaten Schlüssel enthält.

- Der **private Schlüssel fehlt** – ein Zertifikat ohne privaten Schlüssel ist in der Keychain installiert.

- **Abgelaufen** – das Zertifikat ist abgelaufen. Entfernen Sie diese aus ihrer Keychain.

  ![Team Details-Dialog Informationen](apple-account-management-images/image7.png)

## <a name="create-a-signing-certificate"></a>Erstellen eines Signatur Zertifikats

Um eine neue Signierungs Identität zu erstellen, klicken Sie auf **Zertifikat erstellen** , um das Dropdown Menü zu öffnen, und wählen Sie den [Zertifikattyp](https://help.apple.com/xcode/mac/current/#/dev80c6204ec) aus, den Sie erstellen möchten. Wenn Sie über die richtigen Berechtigungen verfügen, wird eine neue Signierungs Identität nach einigen Sekunden angezeigt.

Wenn eine Option in der Dropdown-Dropdown-Datei abgeblendet und deaktiviert ist, bedeutet dies, dass Sie nicht über die richtigen Team Berechtigungen verfügen, um diese Art von Zertifikat zu erstellen.

## <a name="download-provisioning-profiles"></a>Herunterladen von Bereitstellungs Profilen

Im Dialogfeld "Team Details" wird auch eine Liste aller Bereitstellungs Profile angezeigt, die mit Ihrem Entwicklerkonto verbunden sind. Sie können alle Bereitstellungs Profile auf Ihren lokalen Computer herunterladen, indem Sie auf **alle Profile herunterladen**klicken.


## <a name="troubleshoot"></a>Problembehandlung

- Es kann mehrere Stunden dauern, bis ein neues Apple-Entwicklerkonto genehmigt wird. Sie können die automatische Bereitstellung erst aktivieren, wenn das Konto genehmigt wurde.

- Wenn das Hinzufügen eines Apple-Entwickler Kontos mit der Meldung `Authentication Error: Xcode 7.3 or later is required to continue developing with your Apple ID.`fehlschlägt, stellen Sie sicher, dass die von Ihnen verwendete Apple-ID über eine aktive, kostenpflichtige Mitgliedschaft beim Apple Developer Program verfügt. Wenn Sie ein kostenpflichtiges Apple-Entwicklerkonto verwenden möchten, lesen Sie den Leitfaden zur [kostenlosen Bereitstellung für xamarin. IOS-apps](~/ios/get-started/installation/device-provisioning/free-provisioning.md) .

- Wenn der Versuch, ein neues Signaturzertifikat zu erstellen, mit dem Fehler `You have reached the limit for certificates of this type`fehlschlägt, wurde die maximal zulässige Anzahl zulässiger Zertifikate generiert. Um dieses Problem zu beheben, navigieren Sie zum [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) , und widerrufen Sie eines der Produktions Zertifikate.

- Wenn bei der Protokollierung Ihres Kontos auf Visual Studio für Mac Probleme auftreten, besteht die Möglichkeit, die Keychain-Anwendung zu öffnen, und wählen Sie unter **Kategorie** die Option Kenn **Wörter**aus. Suchen Sie nach `deliver.`, und löschen Sie alle gefundenen Einträge.

## <a name="known-issues"></a>Bekannte Probleme

- Das Verteilen von Bereitstellungsprofilen verwendet standardmäßig den App Store als Ziel. In-House- oder Ad-hoc-Profile sollten manuell erstellt werden.
