---
title: Apple-Kontenverwaltung
ms.prod: xamarin
ms.assetid: 71388B83-699B-4E42-8CBF-8557A4A3CABF
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/05/2017
ms.openlocfilehash: 21af0ef09644f39f9be42788b3d8f4977a2143d3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="apple-account-management"></a>Apple-Kontenverwaltung

Die Schnittstelle für die Apple-Konto bietet eine Möglichkeit, alle Entwicklungsteams zugeordnet, einer Apple ID anzeigen Außerdem können Sie weitere Details zu jedem Team anzeigen, indem Sie eine Liste von _Signieren Identitäten_ und _Provisioning Profile_ , die auf dem Computer installiert sind.

Die Authentifizierung Ihrer Apple-ID erfolgt in der Befehlszeile mit [Fastlane](https://fastlane.tools/). FastLane muss auf dem Computer, für die Sie für die Authentifizierung erfolgreich installiert. Weitere Informationen zu Fastlane und zur Installation finden Sie unter der [Fastlane](~/ios/deploy-test/provisioning/fastlane/index.md) Handbüchern.

Das Dialogfeld "Apple-Konto" in Visual Studio für Mac können Sie folgende Aktionen ausführen:

* **Erstellen und Verwalten von Zertifikaten** 
* **Erstellen und Verwalten von Bereitstellungsprofilen** 

Informationen zu diesem Zweck wird in diesem Handbuch beschrieben.

IOS Bundle Signaturtools können auch folgende Aktionen ausführen:

* **Eine neue Signaturidentität einem vorhandenen Profil hinzufügen** 
* **Neue Bereitstellung von Geräten** 

Weitere Informationen zur Verwendung dieser Funktionen finden Sie in der [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Handbuch.
️
## <a name="requirements"></a>Anforderungen

Apple-Kontenverwaltung steht auf Visual Studio für Mac. Es ist zurzeit nicht verfügbar in Visual Studio für Windows.

Sie benötigen ein Apple Developer-Konto, um dieses Feature zu verwenden. Weitere Informationen zum Apple Developer-Konten finden Sie in der [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Handbuch.

- Stellen Sie sicher, dass Sie mit dem Internet verbunden sind. Dies liegt daran Fastlane direkt mit dem Apple-Entwicklerportal kommuniziert.
- Sicherzustellen, dass Sie über [Fastlane-Tools installiert](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).
- Sicherzustellen, dass Sie über die aktuellsten Fastlane Tools aus [ https://download.fastlane.tools ](https://download.fastlane.tools).
- Bevor Sie beginnen, stellen Sie sicher, akzeptieren Sie alle Benutzer Lizenzverträge in der [Entwicklerportal](https://developer.apple.com/account/).

## <a name="adding-an-apple-developer-account"></a>Hinzufügen eines Apple Developer-Kontos

1. So öffnen das Konto-Dialogfeld "Management" Gehe zu **Visual Studio > Einstellungen > Apple-Entwicklerkonto**:

    ![Apple Developer Kontooptionen](apple-account-management-images/image1.png)

2. Drücken Sie die **+** Schaltfläche, um die Anmeldung im Dialogfeld angezeigt, wie unten dargestellt: 

    ![FastLane-Dialogfeld.](apple-account-management-images/image2.png)

4. Geben Sie Ihre Apple-ID und Kennwort, und klicken Sie auf die **anmelden** Schaltfläche. Dadurch wird Ihre Anmeldeinformationen in einem sicheren Schlüssel auf diesem Computer gespeichert. [FastLane](~/ios/deploy-test/provisioning/fastlane/index.md) wird verwendet, um Ihre Anmeldeinformationen sicher zu behandeln, und übergeben Sie sie auf der Apple-Entwicklerportal.
 
5. Wählen Sie **immer zulassen** auf die Warnung zu Visual Studio, um Ihre Anmeldeinformationen zu verwenden:

    ![](apple-account-management-images/image4.png)

6. Sobald Ihr Konto erfolgreich hinzugefügt wurde, sehen Sie Ihre Apple-ID und alle Teams, denen Ihre Apple-ID gehört.

    ![](apple-account-management-images/image5.png)

7. Wählen Sie alle Teams, und drücken Sie die **"Details anzeigen"...** Schaltfläche. Daraufhin wird eine Liste aller Identitäten signieren und Bereitstellen von Profilen, die auf dem Computer installiert sind, angezeigt:

    ![](apple-account-management-images/image6.png)


<a name="managing" />


## <a name="managing-signing-identities-and-provisioning-profiles"></a>Verwalten von Identitäten signieren und Profile bereitstellen

Das Team-Dialogfeld "Details" zeigt eine Liste der Identitäten signieren, nach Typ geordnet. Die **Status** Spalte informiert Sie, wenn das Zertifikat ist: 

* **Gültige** – die Signaturidentität (das Zertifikat und den privaten Schlüssel) auf Ihrem Computer installiert ist und nicht abgelaufen.

* **Nicht im Schlüsselbund** – eine gültige Signaturidentität auf Apple Server vorhanden ist. Möchten Sie diesen auf dem Computer installieren, müssen sie von einem anderen Computer exportiert werden. Die Signaturidentität können nicht aus dem Apple-Entwicklerportal heruntergeladen werden, da er nicht den privaten Schlüssel enthält.

* **Private Schlüssel fehlt** – ein Zertifikat ohne privaten Schlüssel im Schlüsselbund installiert ist.

* **Abgelaufene** – das Zertifikat ist abgelaufen. Sie sollten dies aus Schlüsselbund entfernen.

  ![](apple-account-management-images/image7.png)

## <a name="create-a-signing-identities"></a>Eine Signierung Identitäten erstellen

Wählen Sie zum Erstellen einer neuen Signaturidentität der **neues Zertifikat erstellen** Dropdown-Schaltfläche und wählen Sie den Typ, die erforderlich sind. Wenn Sie die richtigen Berechtigungen eine neue Signatur haben wird die Identität nach wenigen Sekunden angezeigt.

Wenn eine Option in der Dropdownliste ist abgeblendet und nicht wird, ausgewählt wie unten gezeigt, bedeutet dies, dass Sie nicht das richtige Team zum Erstellen dieses Typs von Zertifikat berechtigt sind.

![](apple-account-management-images/image8.png)

## <a name="download-provisioning-profiles"></a>Provisioning Profiles herunterladen

Das Dialogfeld "Team-Details" zeigt außerdem eine Liste aller provisioning Profile, die mit Ihrem Entwicklerkonto verbunden. Sie können alle provisioning Profile auf den lokalen Computer herunterladen, durch Drücken der **Herunterladen von allen Profilen** Schaltfläche

![](apple-account-management-images/image9.png)

## <a name="ios-bundle-signing"></a>iOS Bundle Signing

Informationen zum Bereitstellen Ihrer app auf einem Gerät finden Sie unter der [gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md) Handbuch.

## <a name="troubleshooting"></a>Problembehandlung

### <a name="view-details-dialog-is-empty"></a>Dialogfeld "Details anzeigen" ist leer

Dies ist derzeit ein bekanntes Problem, das im Zusammenhang mit Fehler [#53906](https://bugzilla.xamarin.com/show_bug.cgi?id=53906). Stellen Sie sicher, dass Sie die neueste stabile Version von Visual Studio für Mac verwenden

### <a name="if-you-are-experiencing-issues-logging-in-your-account-please-try-the-following"></a>Wenn Sie Probleme beim Anmelden mit Ihrem Konto haben, versuchen Sie Folgendes:

* Öffnen Sie die Anwendung für die Schlüsselsammlung, und wählen Sie unter der Kategorie *Kennwörter*. Suchen Sie nach `deliver.`, und löschen Sie alle Einträge.

### <a name="error-adding-account-please-sign-in-with-an-app-specific-password"></a>"Fehler beim Hinzufügen des Kontos. Melden Sie sich mit einem app-spezifisches-Kennwort"

Dies ist da 2-Factor Authentication für Ihr Konto aktiviert ist. Stellen Sie sicher, dass Sie die neueste stabile Version von Visual Studio für Mac verwenden

### <a name="failed-to-create-new-certificate"></a>Fehler beim Erstellen des neuen Zertifikats
"Sie haben das Limit für Zertifikate dieses Typs reached"

![](apple-account-management-images/image10.png)

Die maximale Anzahl der zulässigen Zertifikate wurden generiert. Um dieses Problem zu beheben, navigieren Sie zu der [Apple Developer Center](https://developer.apple.com/account/ios/certificate/distribution) und Produktion Zertifikate widerrufen.

## <a name="known-issues"></a>Bekannte Probleme

* Manchmal dauert das Dialogfeld "Details anzeigen" eine übermäßige Zeit für die Identitäten und die Profile abzurufen.
* Häufig kann der Fokus nicht in Visual Studio für Mac zurück nach der Eingabe der Details Ihrer verursacht, dass Ihr Konto nicht hinzugefügt werden. Wenn dies der Fall ist, versuchen Sie erneut den Prozess aus.
* In Visual Studio für Mac erstellte Bereitstellungsprofile beachten keine Berechtigungen, die in Ihren Projekten ausgewählt wurden (entitlements.plist). Diese Funktion wird in zukünftigen Versionen der IDE hinzugefügt.
* Das Verteilen von Bereitstellungsprofilen verwendet standardmäßig den App Store als Ziel. In-House- oder Ad-hoc-Profile sollten manuell erstellt werden.
