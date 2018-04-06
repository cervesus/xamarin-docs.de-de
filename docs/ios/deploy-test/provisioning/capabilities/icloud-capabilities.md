---
title: iCloud-Funktionen
description: Das Hinzufügen von Funktionen zu einer Anwendung erfordert häufig zusätzliche Konfigurationsschritte für die Bereitstellung. In diesem Leitfaden werden die erforderlichen Einstellungen für die iCloud-Funktionen erläutert.
ms.prod: xamarin
ms.assetid: 3CBAC982-D8DE-48DD-97CD-32B551D9DB85
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: 7e18bff7f32303cb0c5016e4ade4b39723921534
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="icloud-capabilities"></a>iCloud-Funktionen

_Um Funktionen einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für die iCloud-Funktionen erläutert._

iCloud stellt iOS-Benutzern einen praktischen und einfachen Weg bereit, um ihre Inhalte zu speichern und diese zwischen Geräte freizugeben. Es gibt vier Möglichkeiten, wie Entwickler iCloud verwenden können, um für ihre Benutzer Speicher bereitzustellen: Schlüssel-Wert-Speicher, UIDocument-Speicher, CoreData und das direkte Verwenden von CloudKit für das Bereitstellen von Speicher für einzelne Dateien und Verzeichnisse. Weitere Informationen dazu finden Sie in diesem Leitfaden mit einer [Einführung in iCloud](~/ios/data-cloud/introduction-to-icloud.md).

Das Hinzufügen der iCloud-Funktion zu einer Anwendung ist wegen der _Container_ etwas schwieriger als bei anderen App-Diensten. Container werden in iCloud verwendet, um Informationen für eine App zu speichern und zu ermöglichen, dass alle Informationen, die in einem einzelnen iCloud-Konto enthalten sind, wie bei der Sandbox auf dem iOS-Gerät des Benutzers verteilt werden können. Weitere Informationen zu Containern finden Sie in diesem Leitfaden mit einer [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md).

<!--# Xcode

The steps in section [Adding App Services ](~/ios/deploy-test/provisioning/capabilities/index.md) above can be used to toggle the required iCloud capability. Depending on which means of storage is being used in your app, you may need to do additional setup. The image below shows the iCloud capabilities pane:

 ![iCloud Capability section](icloud-capabilities-images/image21.png)

First select the required service. If iCloud Documents or CloudKit have been selected, you must select a container. Selecting the default container will create a new container that is unique to the app. Alternatively, if you have created a container either via Xcode or the developer portal, select it here.
-->

<a name="icloud-developer-center" />

## <a name="developer-center"></a>Developer Center

Bei der Bereitstellung einer neuen Anwendung über das Developer Center müssen Sie drei Schritte unbedingt ausführen:

1.  Einen Container erstellen
2.  Eine App-ID mithilfe der iCloud-Funktion erstellen und sie dem Container hinzufügen
3. Ein Bereitstellungsprofil erstellen, das diese App-ID enthält

Die folgenden Schritte führen Sie durch diesen Vorgang:

1.  Navigieren Sie zum [Apple Developer Center](https://developer.apple.com/account/) und dann zum Abschnitt „Certificates, Identifier, and Profiles“ (Zertifikate, Bezeichner und Profile): 
    
     ![Startseite des Apple Developer Centers](icloud-capabilities-images/image22.png)

2.  Wählen Sie unter **Identifiers** (Bezeichner) **iCloud Containers** (iCloud-Container) aus, und klicken Sie dann auf **+**, um einen neuen Container zu erstellen:  
    
    ![Bildschirm „iCloud-Container“](icloud-capabilities-images/image23.png)

3.  Geben Sie unter **Description** eine Beschreibung und unter **Identifier** einen eindeutigen Bezeichner für den iCloud-Container ein: 
    
    ![Registrierungsbildschirm des iCloud-Containers](icloud-capabilities-images/image24.png)

4.  Klicken Sie auf **Continue** (Weiter), versichern Sie sich, dass alle Informationen richtig sind, und klicken Sie auf **Register** (Registrieren), um den iCloud-Container zu erstellen:  
    
    ![Registrierungsbildschirm des iCloud-Containers](icloud-capabilities-images/image25.png)

Führen Sie folgende Schritte aus, um eine neue App-ID zu erstellen und dieser einen Container hinzuzufügen:

1.  Klicken Sie im [Developer Center](https://developer.apple.com/account/) unter **Bezeichner** auf **App IDs** (App-IDs): 
    
    ![Abschnitt „Bezeichner“ im Developer Center](icloud-capabilities-images/image26.png)

2.  Klicken Sie zum Hinzufügen einer neuen App-ID auf die Schaltfläche **+**: 
    
    ![Hinzufügen einer neuen App-ID-Schaltfläche](icloud-capabilities-images/image27.png)

3.  Geben Sie unter **Name** einen Namen für die App-ID und unter **Explicit App ID** eine explizite App-ID an:
    
    ![Eingeben neuer App-ID-Details](icloud-capabilities-images/image28.png)

4.  Klicken Sie unter **App Services** (App-Dienste) auf **iCloud**, und wählen Sie **Include CloudKit support** (CloudKit-Unterstützung einschließen) aus:
    
    ![Auswählen von iCloud-App-Diensten](icloud-capabilities-images/image29.png)

5.  Klicken Sie auf **Continue** (Weiter) und dann auf **Registrieren**. Beachten Sie, dass iCloud auf dem Bestätigungsbildschirm mit einem gelben Punkt und aktivierter Option „Configurable“ (Konfigurierbar) angezeigt wird:   
    
    ![Bestätigungsbildschirm](icloud-capabilities-images/image30.png)

6.  Kehren Sie zur Liste der App-IDs zurück, und wählen Sie diejenige aus, die Sie gerade erstellt haben: 
    
    ![Bildschirm „App-ID auswählen“](icloud-capabilities-images/image31.png)

7.  Scrollen Sie zum unteren Rand dieses erweiterten Abschnitts, und klicken Sie auf **Edit** (Bearbeiten):
    
    ![Bearbeiten der App-ID](icloud-capabilities-images/image32.png)

8.  Scrollen Sie in der Liste nach unten zu iCloud, und klicken Sie auf die Schaltfläche **Bearbeiten**:  
    
    ![Bearbeiten der iCloud-App-ID](icloud-capabilities-images/image33.png)

9.  Wählen Sie den Container aus, den Sie mit dieser App-ID verwenden möchten:  
    
    ![Bildschirm „Container auswählen“](icloud-capabilities-images/image34.png)

10. Bestätigen Sie die Zuweisungen des Containers, und klicken Sie auf **Assign** (Zuweisen).
 
Diese App-ID kann jetzt zum Generieren oder erneuten Generieren eines neuen Bereitstellungsprofils verwendet werden (siehe die Erläuterung in der Anleitung [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md)). 

Weitere Informationen zur Verwendung von iCloud finden Sie im folgenden Leitfaden:

*   [Einführung in iCloud](~/ios/data-cloud/introduction-to-icloud.md)
*   [Einführung in CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)
*   [Einführung in die Dokumentauswahl](~/ios/platform/document-picker.md)

## <a name="next-steps"></a>Nächste Schritte
 
In der folgenden Liste werden mögliche weitere Schritte aufgeführt:

* Verwenden des Framework-Namespaces in Ihrer App
* Hinzufügen der erforderlichen Berechtigungen zu Ihrer App Informationen zu den erforderlichen Berechtigungen und wie sie hinzugefügt werden finden Sie im Leitfaden [Arbeiten mit Berechtigungen](~/ios/deploy-test/provisioning/entitlements.md).
* Stellen Sie im Bereich **iOS-Bündelsignierung** der App sicher, dass **Benutzerdefinierte Berechtigungen** auf **Entitlements.plist** festgelegt ist. Hierbei handelt es sich _nicht_ um die Standardeinstellung für Debug- und iOS-Simulatorbuilds.

Wenn Probleme mit App-Diensten auftreten, konsultieren Sie den Abschnitt [Problembehandlung](~/ios/deploy-test/provisioning/capabilities/index.md) in der Hauptanleitung.