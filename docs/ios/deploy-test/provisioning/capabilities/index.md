---
title: Arbeiten mit Funktionen
description: "Um Funktionen zu einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für alle Funktionen erläutert."
ms.topic: article
ms.prod: xamarin
ms.assetid: 98A4676F-992B-4593-8D38-6EEB2EB0801C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/15/2017
ms.openlocfilehash: c038aba3989046e6df062e97ae7f777ae6238ade
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="working-with-capabilities"></a>Arbeiten mit Funktionen

_Um Funktionen zu einer Anwendung hinzuzufügen, ist oft eine zusätzliche Bereitstellungseinrichtung erforderlich. In diesem Leitfaden werden die erforderlichen Einstellungen für alle Funktionen erläutert._

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


Funktionen können entweder über Visual Studio für Mac oder manuell im Apple Developer Portal aktiviert werden. Bestimmte Funktionen, wie z.B. Wallet, Apple Pay und iCloud, erfordern eine zusätzliche Konfiguration der App-IDs.

In diesem Leitfaden wird erläutert, wie Sie jeden dieser App-Dienste in Ihrer Anwendung sowohl in Visual Studio für Mac als auch manuell im Developer Center aktivieren können. Außerdem werden alle möglicherweise erforderlichen zusätzlichen Einstellungen beschrieben. 

## <a name="adding-app-services"></a>Hinzufügen von App-Diensten

Damit die App die Funktionen verwenden kann, muss sie über ein gültiges Bereitstellungsprofil mit einer App-ID verfügen, in dem der richtige Dienst aktiviert ist. Sie können dieses Bereitstellungsprofil entweder automatisch in Visual Studio für Mac oder manuell im Apple Developer Center erstellen.

In diesem Abschnitt wird erläutert, wie Sie die automatische Bereitstellung von Visual Studio für Mac oder das Developer Center zur Aktivierung der meisten Funktionen verwenden können. Einige Funktionen, wie z.B. Wallet, iCloud, Apple Pay und App-Gruppen, erfordern zusätzliche Konfigurationsschritte. Diese werden in den benachbarten Handbüchern ausführlich erläutert.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

> [!IMPORTANT]
> **Hinweis:** Nicht alle Funktionen können in Visual Studio für Mac hinzugefügt und verwaltet werden. Die folgende Liste enthält die unterstützten Funktionen:
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


Funktionen werden der Datei **Entitlements.plist** in Visual Studio für Mac hinzugefügt. Führen Sie folgende Schritte aus, um Funktionen hinzuzufügen:

1. Öffnen Sie die Datei **Info.plist** Ihrer iOS-Anwendung, und stellen Sie sicher, dass **Signierung automatisch verwalten** ausgewählt ist. Falls Sie Hilfe benötigen, führen Sie die Schritte in der Anleitung [Automatische Bereitstellung](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) aus.

    ![Option „Signierung automatisch verwalten“](images/manage-signing.png)

2. Öffnen Sie die Datei **Entitlements.plist**, und wählen Sie die Funktion, die Sie hinzufügen möchten:

    ![Hinzufügen von Funktionen zur Datei „entitlements.plist“](images/image17.png)

    Das Auswählen einer Funktion bewirkt zwei Dinge:
    * Diese Funktion wird Ihrer App-ID hinzugefügt.
    * Die Schlüssel-Wert-Paar für Berechtigungen wird Ihrer Datei „entitlements.plist“ hinzugefügt.

    Visual Studio für Mac informiert Sie, wenn diese Aufgaben ausgeführt wurden, indem die folgende Erfolgsmeldung angezeigt wird:

    ![Hinzufügen von Funktionen zur Datei „entitlements.plist“](images/image18.png)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Da derzeit keine Unterstützung für die automatische Bereitstellung in Visual Studio 2017 vorhanden ist, müssen Sie das [Developer Center](#devcenter) verwenden, um eine App-ID mit dem korrekten Anwendungsdienst zu erstellen.

-----

<!--
<a name="xcode" />

## Xcode

Xamarin developers can also use Xcode to quickly create a provisioning profile with a suitable App ID. This process, described below, can be used for any app service in the list:

1.  Open Xcode and create a ‘dummy’ project. Give the dummy project the same name as your Xamarin.iOS project. The bundle identifier should be identical to the bundle identifier of your Xamarin.iOS project:

    ![Xcode Create Project](images/image1.png)

2.  Ensure **Automatically manage signing** is selected:

    ![Automatically manage signing selection](images/image2.png)

3.  Once the app has been created, go to the tab named **Capabilities**:

    ![Xcode Capabilities tab](images/image3.png)

4.  Browse to the capability that you wish to add, and move the switch to the **ON** position.
5.  This will create a provisioning profile with an App ID that contains the capability and adds the entitlement to the profile.
6.  In Visual Studio for Mac / Visual Studio, browse to **Project Options > Bundle Signing** and set the provisioning profile to the one that was just created in Xcode:

    ![Visual Studio for Mac Project Options](images/image4.png)
-->

<a name="devcenter" />

## <a name="developer-center"></a>Developer Center

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
8.  Bestätigen Sie Ihre App-ID. Jeder Dienst befindet sich wie unten gezeigt in einem der folgenden Zustände: **Aktiviert**, **Deaktiviert** oder **Konfigurierbar**. Ist der Dienst **Aktiviert**, können Sie ihn in einem Bereitstellungsprofil verwenden. Ist er **Konfigurierbar**, sind für diese Funktion weitere Konfigurationsschritte erforderlich. Die zusätzlichen Konfigurationsschritte werden in nachfolgenden Abschnitten ausführlicher beschrieben.

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

9. Wenn Sie Visual Studio für Mac verwenden, stellen Sie sicher, dass in der Datei **Info.plist** die Option **Signieren automatisch verwalten** deaktiviert ist.

10. Wechseln Sie in Visual Studio für Mac bzw. Visual Studio zu **Projektoptionen > Bündelsignierung**, und legen Sie das Bereitstellungsprofil auf das Projekt fest, das Sie gerade erstellt haben:

    ![Visual Studio für Mac – Projektoptionen](images/image16.png)

> [!IMPORTANT]
> Hinweis: Möglicherweise müssen Sie auch Berechtigungsschlüssel in der Datei „Entitlement.plist“ sowie Datenschutzschlüssel in der Datei „Info.plist“ festlegen. Weitere Informationen zu diesen Berechtigungen finden Sie im Leitfaden [Working with Entitlements](~/ios/deploy-test/provisioning/entitlements.md) (Arbeiten mit Berechtigungen).

<a name="nextsteps" />

## <a name="next-steps"></a>Nächste Schritte

Wenn eine Funktion auf der Serverseite aktiviert wurde, sind weitere Schritte erforderlich, damit Ihre App die Funktionen auch verwenden kann. In der folgenden Liste werden mögliche weitere Schritte aufgeführt:

*   Verwenden des Framework-Namespaces in Ihrer App
*   Hinzufügen der erforderlichen Berechtigungen zu Ihrer App Informationen zu den erforderlichen Berechtigungen und wie sie hinzugefügt werden finden Sie im Leitfaden [Introduction to Entitlements](~/ios/deploy-test/provisioning/entitlements.md) (Einführung in Berechtigungen).

<a name="troubleshooting" />

## <a name="troubleshooting-capabilities"></a>Funktionen zur Problembehandlung

In der folgenden Liste werden einige der häufigsten Probleme aufgeführt, die zu Hindernissen bei der Entwicklung einer App mit einem aktivierten App-Dienst führen können:

-   Stellen Sie sicher, dass die richtige ID ordnungsgemäß erstellt und im Abschnitt **Zertifikate, Bezeichner und Profile** des Apple Developer Portal registriert wurde.
-   Stellen Sie sicher, dass der Dienst der App-ID (oder der Erweiterungs-ID) hinzugefügt wurde und dass er für die Verwendung der zuvor erstellten App-Gruppe/Händler-ID/Container unter **Zertifikate, Bezeichner und Profile** des Apple Developer Portal konfiguriert wurde.
-   Stellen Sie sicher, dass die Bereitstellungsprofile und App-IDs installiert wurden und dass die Datei **Info.plist** der App (im Xamarin-Projekt) eine der zuvor konfigurierten App-IDs verwendet.
-   Stellen Sie sicher, dass für die Datei **Entitlements.plist** der App (im Xamarin-Projekt) der richtige Dienst aktiviert wurde.
-   Stellen Sie sicher, dass die Datenschutzschlüssel in der Datei „Info.plist“ entsprechend festgelegt sind.
-   Stellen Sie im Bereich **iOS-Bündelsignierung** der App sicher, dass **Benutzerdefinierte Berechtigungen** auf **Entitlements.plist** festgelegt ist. **Hinweis:** Hierbei handelt es sich _nicht_ um die Standardeinstellung für Debug- und iOS-Simulatorbuilds.

<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurden Funktionen, auch _App-Dienste_ genannt, sowie deren Aktivierung in Visual Studio und im Apple Developer Center beschrieben. Außerdem wird die Konfiguration von komplizierteren Diensten erläutert, wie z.B. Wallet, iCloud, Apple Pay und App-Gruppen. Abschließend werden weitere Konfigurationsschritte sowie einfache Optionen zur Problembehandlung aufgeführt.