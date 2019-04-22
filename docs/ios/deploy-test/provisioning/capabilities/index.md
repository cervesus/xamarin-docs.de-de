---
title: Arbeiten mit Funktionen in Xamarin.iOS
description: Um Funktionen zu einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für alle Funktionen erläutert.
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 05/06/2018
ms.openlocfilehash: 7049cc36f5f661152e027beb53180d793078beff
ms.sourcegitcommit: 3489c281c9eb5ada2cddf32d73370943342a1082
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/18/2019
ms.locfileid: "58855028"
---
# <a name="working-with-capabilities-in-xamarinios"></a>Arbeiten mit Funktionen in Xamarin.iOS

_Um Funktionen einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für alle Funktionen erläutert._

Apple stellt Entwicklern _Funktionen_, häufig auch _App-Dienste_ genannt, als Werkzeug zur Erweiterung der Funktionen und des Verwendungsspielraums von iOS-Apps zur Verfügung. Mit diesen Funktionen können Entwickler ihrer Anwendung eine umfassendere Integration von Plattformfunktionen hinzufügen, wie z.B. das Einleiten finanzieller Transaktionen in der App, weitere Gerätedienste wie Siri und viele mehr.
Diese Funktionen können mit Xamarin.iOS-Projekten verwendet werden. Die vollständige Liste der Dienste lautet wie folgt:

* App-Gruppen
* Zugehörige Bereiche
* Datenschutz
* Game Center
* HealthKit
* HomeKit
* Konfiguration für drahtloses Zubehör
* iCloud
* In-App-Käufe
* Inter-App-Audio
* Apple Pay
* Wallet
* Pushbenachrichtigung
* Persönliches VPN
* Siri
* Karten
* Hintergrundmodi
* Keychain-Freigabe
* Netzwerkerweiterungen
* Hotspotkonfiguration
* Multipfad
* Lesen von NFC-Tags

Funktionen können entweder über Visual Studio für Mac und Visual Studio 2019 oder manuell im Apple Developer Portal aktiviert werden. Bestimmte Funktionen, wie z.B. Wallet, Apple Pay und iCloud, erfordern eine zusätzliche Konfiguration der App-IDs.

In diesem Leitfaden wird erläutert, wie Sie jeden dieser App-Dienste in Ihrer Anwendung automatisch in Visual Studio und manuell im Developer Center aktivieren können. Außerdem werden alle möglicherweise erforderlichen zusätzlichen Einstellungen beschrieben. 

## <a name="adding-app-services"></a>Hinzufügen von App-Diensten

Damit die App die Funktionen verwenden kann, muss sie über ein gültiges Bereitstellungsprofil mit einer App-ID verfügen, in dem der richtige Dienst aktiviert ist. Sie können dieses Bereitstellungsprofil entweder automatisch in Visual Studio für Mac und Visual Studio 2019 oder manuell im Apple Developer Center erstellen.

In diesem Abschnitt wird erläutert, wie Sie die automatische Bereitstellung von Visual Studio oder das Developer Center zur Aktivierung der meisten Funktionen verwenden können. Einige Funktionen, wie z.B. Wallet, iCloud, Apple Pay und App-Gruppen, erfordern zusätzliche Konfigurationsschritte. Diese werden in den benachbarten Handbüchern ausführlich erläutert.

> [!IMPORTANT]
> Nicht alle Funktionen können mit der automatischen Bereitstellung hinzugefügt und verwaltet werden. Die folgende Liste enthält die unterstützten Funktionen:
>
>* HealthKit 
>* HomeKit 
>* Persönliches VPN 
>* Konfiguration für drahtloses Zubehör 
>* Inter-App-Audio 
>* SiriKit 
>* Hotspot 
>* Netzwerkerweiterungen 
>* Lesen von NFC-Tags
>* Multipfad 
>
>Funktionen für Pushbenachrichtigungen, Game Center, In App-Käufe, Zuordnungen, Keychain-Freigabe, verknüpfte Domänen und Datenschutz werden derzeit nicht unterstützt. Um diese Funktionen hinzuzufügen, verwenden Sie die manuelle Bereitstellung, und führen Sie die Schritte im Abschnitt [Developer Center](#devcenter) aus.

## <a name="using-the-ide"></a>Verwenden von IDE

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Funktionen werden der Datei **Entitlements.plist** in Visual Studio für Mac hinzugefügt. Um Funktionen hinzuzufügen, führen Sie die folgenden Schritte aus:

1. Öffnen Sie die Datei **Info.plist** Ihrer iOS-Anwendung, und wählen Sie das Schema **Automatische Bereitstellung** und Ihr **Team** aus dem Kombinationsfeld aus. Falls Sie Hilfe benötigen, führen Sie die Schritte in der Anleitung [Automatische Bereitstellung](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) aus.

    ![Option „Signierung automatisch verwalten“](images/manage-signing.png)

2. Öffnen Sie die Datei **Entitlements.plist**, und wählen Sie die Funktion, die Sie hinzufügen möchten:

    ![Hinzufügen von Funktionen zur Datei „entitlements.plist“](images/image17.png)

    Das Auswählen einer Funktion bewirkt zwei Dinge:
    * Diese Funktion wird Ihrer App-ID hinzugefügt.
    * Die Schlüssel-Wert-Paar für Berechtigungen wird Ihrer Datei „entitlements.plist“ hinzugefügt.

    Visual Studio für Mac informiert Sie, wenn diese Aufgaben ausgeführt wurden, indem die folgende Erfolgsmeldung angezeigt wird:

    ![Hinzufügen von Funktionen zur Datei „entitlements.plist“](images/image18.png)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Funktionen werden der Datei **Entitlements.plist** hinzugefügt. Wenn Sie Funktionen in Visual Studio 2019 hinzufügen möchten, führen Sie die folgenden Schritte aus:

1. Koppeln Sie Visual Studio 2019 mit einem Mac, wie im Leitfaden [Durchführen einer Kopplung mit einem Mac für die Xamarin.iOS-Entwicklung](~/ios/get-started/installation/windows/connecting-to-mac/index.md) erläutert.

2. Öffnen Sie die Bereitstellungsoptionen, indem Sie **Projekt > Bereitstellungseigenschaften...** auswählen.

3. Wählen Sie das Schema **Automatische Bereitstellung** und Ihr **Team** aus dem Kombinationsfeld aus. Falls Sie Hilfe benötigen, führen Sie die Schritte in der Anleitung [Automatische Bereitstellung](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) aus.

    ![Option „Signierung automatisch verwalten“](images/manage-signing-vs.png)

4. Öffnen Sie die Datei **Entitlements.plist**, und wählen Sie die Funktion aus, die Sie hinzufügen möchten. Speichern Sie die Datei.

    Durch Speichern von **Entitlement.plist** geschehen zwei Dinge:

    * Diese Funktion wird Ihrer App-ID hinzugefügt.
    * Die Schlüssel-Wert-Paar für Berechtigungen wird Ihrer Datei „entitlements.plist“ hinzugefügt.

-----


<a name="devcenter" />

## <a name="using-the-developer-center"></a>Verwenden von Office Developer Center

Die Verwendung des Developer Centers besteht aus zwei Schritten: Erstellen einer App-ID und Verwenden der App-ID zum Erstellen eines Bereitstellungsprofils. Diese Schritte sind weiter unten ausführlich beschrieben.

### <a name="creating-an-app-id-with-an-app-service"></a>Erstellen einer App-ID mit einem App-Dienst

1.  Wechseln Sie auf einem Mac (auf dem Buildhost-Mac, wenn Sie einen Windows-Computer verwenden) zum [Apple Developer Center](https://developer.apple.com/account), und melden Sie sich an.
2.  Wählen Sie **Zertifikate, Bezeichner und Profile** aus:

    ![Apple Developer Center](images/image5.png)

3.  Wählen Sie unter **Bezeichner** die Option **App-IDs** aus:

    ![Auswahl „App-IDs“ im Developer Center](images/image6.png)

4.  Klicken Sie zum Erstellen einer neuen App-ID auf die **+**-Schaltfläche in der oberen rechten Ecke.
5.  Geben Sie eine Beschreibung der App-ID ein, wählen Sie „Eindeutige App-ID“ aus, und geben Sie eine Bündel-ID im Format `com.domain.appname` ein. Diese Bündel-ID sollte mit der Bündel-ID in Ihrem Projekt übereinstimmen:

    ![Hinzufügen von App-ID-Details](images/image7.png)

6.  Wählen Sie unter **App-Dienste** den Dienst oder die Dienste aus, die in Ihrer App erfordert werden:

    ![Auswahl der App-Dienste](images/image8.png)

7.  Klicken Sie auf **Weiter**.
8.  Bestätigen Sie Ihre App-ID. Jeder Dienst kann einen der folgenden Status aufweisen: **Aktiviert**, **Deaktiviert** oder **Konfigurierbar**, wie oben angezeigt. Ist der Dienst **Aktiviert**, können Sie ihn in einem Bereitstellungsprofil verwenden. Ist er **Konfigurierbar**, sind für diese Funktion weitere Konfigurationsschritte erforderlich. Die zusätzlichen Konfigurationsschritte werden in nachfolgenden Abschnitten ausführlicher beschrieben.

    ![Bestätigung der App-ID](images/image9.png)

9.  Klicken Sie auf **Registrieren** und anschließend auf **Fertig**. Die neu erstellte App-ID sollte nun in der Liste der iOS-App-IDs angezeigt werden.


<a name="provisioningprofile" />

### <a name="creating-a-provisioning-profile"></a>Erstellen eines Bereitstellungsprofils

Erstellen Sie nun ein Bereitstellungsprofil, das diese App-ID enthält. Führen Sie die nachfolgenden Schritte aus:

1.  Wechseln Sie im Apple Developer Center zu **Bereitstellungsprofile > Alle**:

    ![Abschnitt „Bereitstellungsprofil“](images/image10.png)

2.  Klicken Sie zum Erstellen eines neuen Bereitstellungsprofils auf die **+**-Schaltfläche in der oberen rechten Ecke.
3.  Wählen Sie den Bereitstellungsprofiltyp aus, den Sie benötigen, und klicken Sie auf **Weiter**:

    ![Auswahl des Bereitstellungsprofils](images/image11.png)

4.  Wählen Sie aus der Dropdownliste die App-ID aus, die in den vorhergehenden Schritten erstellt wurde, und klicken Sie auf **Weiter**:

    ![Auswahl der App-ID](images/image12.png)

5.  Wählen Sie die Zertifikate zum Signieren der App aus, und klicken Sie auf **Weiter**:

    ![Auswahl der Zertifikate](images/image13.png)

6.  Wählen Sie die Geräte aus, die dieses Profil enthalten soll, und klicken Sie auf **Weiter**:

    ![Auswahl der Geräte für das Bereitstellungsprofil](images/image14.png)

7.  Geben Sie dem Profil zur Identifizierung einen Namen, und klicken Sie zum Generieren des Profils auf **Weiter**:

    ![Name des Bereitstellungsprofils](images/image15.png)

8.  Klicken Sie zum Herunterladen des Profils auf die Schaltfläche **Herunterladen**. Doppelklicken Sie im Finder auf die Datei, um das Bereitstellungsprofil zu installieren.

9. Stellen Sie bei Verwendung von Visual Studio sicher, dass die Option **Manuelle Bereitstellung** ausgewählt ist.

10. Wechseln Sie in Visual Studio für Mac bzw. Visual Studio zu **Projektoptionen > Bündelsignierung**, und legen Sie das Bereitstellungsprofil auf das Projekt fest, das Sie gerade erstellt haben:

    ![Visual Studio für Mac – Projektoptionen](images/image16.png)

> [!IMPORTANT]
> Möglicherweise müssen Sie auch Berechtigungsschlüssel in der Datei „Entitlement.plist“ sowie Datenschutzschlüssel in der Datei „Info.plist“ festlegen. Weitere Informationen zu diesen Berechtigungen finden Sie im Leitfaden [Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md) (Arbeiten mit Berechtigungen).

<a name="nextsteps" />

## <a name="next-steps"></a>Nächste Schritte

Wenn eine Funktion auf der Serverseite aktiviert wurde, sind weitere Schritte erforderlich, damit Ihre App die Funktionen auch verwenden kann. In der folgenden Liste werden mögliche weitere Schritte aufgeführt:

*   Verwenden des Framework-Namespaces in Ihrer App
*   Hinzufügen der erforderlichen Berechtigungen zu Ihrer App Informationen zu den erforderlichen Berechtigungen und wie sie hinzugefügt werden finden Sie im Leitfaden [Introduction to Entitlements](~/ios/deploy-test/provisioning/entitlements.md) (Einführung in Berechtigungen).

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>Funktionen zur Problembehandlung

In der folgenden Liste werden einige der häufigsten Probleme aufgeführt, die zu Hindernissen bei der Entwicklung einer App mit einem aktivierten App-Dienst führen können:

-   Stellen Sie sicher, dass die richtige ID ordnungsgemäß erstellt und im Abschnitt  **Certificates, IDs & Profiles** (Zertifikate, Bezeichner und Profile) im Apple Developer Portal registriert wurde.
-   Stellen Sie sicher, dass der Dienst der App-ID (oder der Erweiterungs-ID) hinzugefügt wurde und dass er für die Verwendung der zuvor erstellten App-Gruppe/Händler-ID/Container unter  **Certificates, IDs & Profiles**  (Zertifikate, Bezeichner und Profile) im Apple Developer Portal konfiguriert wurde.
-   Stellen Sie sicher, dass die Bereitstellungsprofile und App-IDs installiert wurden und dass die Datei  **Info.plist**  der App (im Xamarin-Projekt) eine der zuvor konfigurierten App-IDs verwendet.
-   Stellen Sie sicher, dass für die Datei  **Entitlements.plist**  der App (im Xamarin-Projekt) der richtige Dienst aktiviert wurde.
-   Stellen Sie sicher, dass die Datenschutzschlüssel in der Datei „Info.plist“ entsprechend festgelegt sind.
-   Stellen Sie im Bereich  **iOS-Bundle-Signierung** der App sicher, dass  **Benutzerdefinierte Berechtigungen** auf **Entitlements.plist** festgelegt ist. Hierbei handelt es sich  _nicht_  um die Standardeinstellung für Debug- und iOS-Simulatorbuilds.

<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurden Funktionen, auch _App-Dienste_ genannt, sowie deren Aktivierung in Visual Studio und im Apple Developer Center beschrieben. Außerdem wird die Konfiguration von komplizierteren Diensten erläutert, wie z.B. Wallet, iCloud, Apple Pay und App-Gruppen. Abschließend werden weitere Konfigurationsschritte sowie einfache Optionen zur Problembehandlung aufgeführt.