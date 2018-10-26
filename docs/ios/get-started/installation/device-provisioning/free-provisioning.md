---
title: Kostenlose Bereitstellung für Xamarin.iOS-Apps
description: In diesem Artikel wird beschrieben, wie Xamarin.iOS-Entwickler Ihre App auf einem physischen Gerät testen können, ohne dass sie sich beim kostenpflichtigen Developer Program von Apple anmelden müssen.
ms.prod: xamarin
ms.assetid: A5CE2ECF-8057-49ED-8393-EB0C5977FE4C
ms.technology: xamarin-ios
author: asb3993
ms.author: amburns
ms.date: 07/16/2018
ms.openlocfilehash: 503dae8253b3c0bb82038dd54b5d97ff632b439b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115034"
---
# <a name="free-provisioning-for-xamarinios-apps"></a>Kostenlose Bereitstellung für Xamarin.iOS-Apps

Mit der kostenlosen Bereitstellung können Xamarin.iOS-Entwickler ihre Anwendungen auf ihrem iOS-Gerät bereitstellen und testen, **ohne** am **Apple-Entwicklerprogramm** teilzunehmen.
Das Testen in Simulatoren ist wichtig und praktisch, aber es ist dennoch essentiell, Apps auf physischen iOS-Geräten zu testen, um sicherzustellen, dass sie auch unter realen Bedingungen mit Einschränkungen des Arbeitsspeichers, des Speichers und der Netzwerkkonnektivität funktionieren.

So verwenden Sie die kostenlose Bereitstellung zum Bereitstellen einer App auf einem Gerät:

- Verwenden Sie Xcode zum Erstellen der erforderlichen *Signierungsidentität* (Entwicklerzertifikat und privater Schlüssel) und des erforderlichen *Bereitstellungsprofils* (das eine explizite App-ID und die UDID des verbundenen iOS-Geräts enthält).
- Verwenden Sie die Signierungsidentät und das Bereitstellungsprofil, die von Xcode erstellt wurden, in Visual Studio für Mac oder Visual Studio 2017 zum Bereitstellen von Xamarin.iOS-Anwendungen.

> [!IMPORTANT]
> Durch die [automatische Bereitstellung](~/ios/get-started/installation/device-provisioning/automatic-provisioning.md) kann Visual Studio für Mac oder Visual Studio 2017 automatisch ein Gerät zum Testen für den Entwickler einrichten. Die automatische Bereitstellung ist bei der kostenlosen Bereitstellung nicht möglich. Um die automatische Bereitstellung nutzen zu können, müssen Sie über ein kostenpflichtiges Apple Developer Program-Konto verfügen.

## <a name="requirements"></a>Anforderungen

So stellen Sie Ihre Xamarin.iOS-Anwendungen auf einem Gerät mit kostenloser Bereitstellung bereit:

- Die verwendete Apple-ID darf nicht mit dem Apple Developer Program verknüpft sein.
- Ihre Xamarin.iOS-App muss eine eindeutige App-ID verwenden und keine Platzhalter-ID.
- Die in Ihrer App verwendete Bündel-ID muss in Ihrer Xamarin.iOS-App eindeutig sein und darf nicht schon in einer anderen App verwendet worden sein. Eine mit der kostenlosen Bereitstellung verwendete Bündel-ID **darf nicht** erneut verwendet werden.
- Wenn Sie eine App bereits verteilt haben, können Sie diese App nicht mehr mit der kostenlosen Bereitstellung bereitstellen.
- Wenn Ihre App App Services verwendet, müssen Sie ein Bereitstellungsprofil erstellen. Eine Anleitung hierzu finden Sie im Leitfaden zur [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services). 

Sehen Sie sich den Abschnitt [Einschränkungen](#limitations) in diesem Artikel an, um mehr Informationen zu den Einschränkungen der kostenlosen Bereitstellung zu erhalten. In den [Leitfäden zur App-Verteilung](~/ios/deploy-test/app-distribution/index.md) finden Sie mehr Informationen zur Verteilung von iOS-Anwendungen.

## <a name="testing-on-device-with-free-provisioning"></a>Testen auf einem Gerät mit kostenloser Bereitstellung

Führen Sie die untenstehenden Schritte durch, um Ihre Xamarin.iOS-App mit kostenloser Bereitstellung zu testen.

### <a name="use-xcode-to-create-a-signing-identity-and-provisioning-profile"></a>Erstellen einer Signierungsidentität und eines Bereitstellungsprofil mit Xcode

1. Wenn Sie noch keine Apple-ID besitzen, [erstellen Sie eine Apple-ID](https://appleid.apple.com).
2. Öffnen Sie Xcode, und navigieren Sie zu **Xcode > Einstellungen**.
3. Klicken Sie unter **Konten** auf die **+**-Schaltfläche, um Ihre vorhandene Apple-ID hinzuzufügen. Dies sollte ungefähr wie folgt aussehen:

    ![Xcode-Einstellungen > Konten](free-provisioning-images/launchapp1.png "Xcode Preferences – Accounts")

4. Schließen Sie die Xcode-Einstellungen.
5. Schließen Sie das iOS-Gerät an, auf dem Sie Ihre App bereitstellen möchten.
6. Erstellen Sie ein neues Projekt in Xcode. Navigieren Sie zu **Datei > Neu > Projekt**, und wählen Sie **Einzelansicht-App** aus.
7. Legen Sie im Dialogfeld für ein neues Projekt **Team** auf die gerade hinzugefügte Apple-ID fest. In der Dropdownliste sollte die Option in etwa **Ihr Name (Persönliches Team)** lauten:

    ![Neue App erstellen](free-provisioning-images/launchapp2.png "Create a new app")

8. Wenn das neue Projekt erstellt wurde, wählen Sie ein Xcode-Buildschema aus, das Ihr iOS-Gerät (statt eines Simulators) als Ziel verwendet.

    ![Xcode-Buildschema auswählen](free-provisioning-images/xcodescheme.png "Select an Xcode build scheme")

9. Öffnen Sie die Projekteinstellungen Ihrer App. Wählen Sie dazu den obersten Knoten im **Projektnavigator** in Xcode aus.
10. Achten Sie darauf, dass die **Bündle-ID** unter **Allgemein > Identität** _genau_ mit der Bündel-ID Ihrer Xamarin.iOS-App übereinstimmt.

    ![Bündel-ID festlegen](free-provisioning-images/launchapp5.png "Set a bundle identifier")

    > [!IMPORTANT]
    > Xcode erstellt nur ein Bereitstellungsprofil für eine eindeutige App-ID, die mit der App-ID Ihrer Xamarin.iOS-App übereinstimmen muss.
    > Wenn sie nicht übereinstimmen, können Sie Ihre Xamarin.iOS-App nicht mit der kostenlosen Bereitstellung bereitstellen.

11. Achten Sie darauf, dass das Bereitstellungsziel unter **Bereitstellungsinformationen** mit der iOS-Version des verbundenen iOS-Geräts übereinstimmt oder niedriger ist.
12. Wählen Sie unter **Signing** (Signieren) die Option **Automatically manage signing** (Signierung automatisch verwalten) und Ihr Team aus der Dropdownliste aus.

    ![Signierung automatisch verwalten](free-provisioning-images/launchapp6.png "Automatically manage signing")

    Xcode erstellt automatisch ein Bereitstellungsprofil und eine Signierungsidentität für Sie. Diese werden durch Klicken auf das Informationssymbol neben dem Bereitstellungsprofil angezeigt:

    ![Bereitstellungsprofil anzeigen](free-provisioning-images/launchapp7.png "View the provisioning profile")

    > [!TIP]
    > Wenn es einen Fehler beim Erstellen des Bereitstellungsprofils gibt, stellen Sie sicher, dass das aktuell ausgewählte Buildschema von Xcode das verbundene iOS-Gerät und keinen Simulator als Ziel verwendet.

13. Stellen Sie die leere Anwendung durch Klicken auf die Schaltfläche „Ausführen“ auf Ihrem Gerät bereit, um in Xcode zu testen.

### <a name="deploy-your-xamarinios-app"></a>Bereitstellen Ihrer Xamarin.iOS-App

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Verbinden Sie Ihr iOS-Gerät via USB oder [drahtlos](~/ios/deploy-test/wireless-deployment.md) mit dem Mac-Buildhost.
2. Doppelklicken Sie im **Lösungspad** von Visual Studio für Mac auf **Info.plist**.
3. Klicken Sie unter **Signieren** auf **Manuelle Bereitstellung**.
4. Klicken Sie auf die **iOS-Bündelsignierung**- Schaltfläche.
5. Wählen Sie **Debug** als **Konfiguration**.
6. Wählen Sie **iPhone** als **Plattform**.
7. Wählen Sie die von Xcode erstellte **Signierungsidentität**.
8. Wählen Sie das von Xcode erstellte **Bereitstellungsprofil**.

    ![Signierungsidentität und Bereitstellungsprofil festlegen](free-provisioning-images/launchapp8.png "Set the signing identity and provisioning profile")

    > [!TIP]
    > Wird Ihre Signierungsidentität oder das korrekte Bereitstellungsprofil nicht angezeigt, müssen Sie Visual Studio für Mac möglicherweise neu starten.

9. Klicken Sie auf **OK**, um die **Projektoptionen** zu speichern und zu schließen.
10. Wählen Sie Ihr iOS-Gerät, und führen Sie die App aus.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Stellen Sie sicher, dass Visual Studio 2017 [mit einem Mac-Buildhost gekoppelt wurde](~/ios/get-started/installation/windows/connecting-to-mac/index.md).
2. Verbinden Sie Ihr iOS-Gerät via USB oder [drahtlos](~/ios/deploy-test/wireless-deployment.md) mit dem Mac-Buildhost.
3. Klicken Sie im Visual Studio 2017-**Projektmappen-Explorer** mit der rechten Maustaste auf das Xamarin.iOS-Projekt, und wählen Sie dann **Eigenschaften** aus.
4. Navigieren Sie zu **iOS-Bündelsignierung**.
5. Wählen Sie **Debug** als **Konfiguration**.
6. Wählen Sie **iPhone** als **Plattform**.
7. Klicken Sie auf **Manuelle Bereitstellung**.
8. Wählen Sie die von Xcode erstellte **Signierungsidentität**.
9. Wählen Sie das von Xcode erstellte **Bereitstellungsprofil**.
    
    ![Signierungsidentität und Bereitstellungsprofil festlegen](free-provisioning-images/setprofile-w157.png "Set the signing identity and provisioning profile")

    > [!TIP]
    > Xcode hat eine Signierungsidentität und ein Bereitstellungsprofil erstellt und diese auf Ihrem Mac-Buildhost gespeichert. Visual Studio 2017 kann darauf zugreifen, weil es mit dem Mac-Buildhost [gekoppelt](~/ios/get-started/installation/windows/connecting-to-mac/index.md) wurde. Wenn sie nicht angezeigt werden, müssen Sie Visual Studio 2017 vermutlich neu starten.

10. Speichern und schließen Sie die Projekteigenschaften.
11. Wählen Sie Ihr iOS-Gerät, und führen Sie die App aus.

-----

## <a name="limitations"></a>Einschränkungen

Es existieren einige Einschränkungen, wann und wie Sie die kostenlose Bereitstellung verwenden können, um Ihre Anwendung auf einem iOS-Gerät auszuführen. Dadurch will Apple sicherstellen, dass die Bereitstellung nur auf *Ihrem* Gerät erfolgt:

- Der Zugriff auf iTunes Connect ist eingeschränkt, wodurch Dienste wie etwa das Veröffentlichen im App Store und TestFlight für Entwickler, die für ihre Anwendungen die kostenlose Bereitstellung verwenden, nicht verfügbar sind. Für die Verteilung über Ad-hoc- und interne Mittel ist ein Apple-Entwicklerkonto erforderlich (Enterprise- oder persönliches Konto).
- Bereitstellungsprofile, die mit der kostenlosen Bereitstellung erstellt wurden, laufen nach einer Woche ab, Signierungsidentitäten nach einem Jahr. 
- Xcode erstellt Bereitstellungsprofile nur mit einer eindeutigen App-ID, weshalb Sie für jede App, die Sie installieren möchten, die [oben](#testing-on-device-with-free-provisioning) aufgeführten Anweisungen befolgen müssen.
- Die meisten Anwendungsdienste können nicht mit der kostenlosen Bereitstellung bereitgestellt werden. Dazu zählen Apple Pay, Game Center, iCloud, In-App-Käufe, Pushbenachrichtigungen und Wallet. Im Leitfaden [Supported capabilities (Unterstützte Funktionen) (iOS)](https://help.apple.com/developer-account/#/dev21218dfd6) finden Sie eine vollständige Liste der Funktionen. Wenn Sie Ihre App für die Verwendung mit Anwendungsdiensten bereitstellen möchten, finden Sie Informationen dazu in den Leitfäden zum [Arbeiten mit Funktionen](~/ios/deploy-test/provisioning/capabilities/index.md).

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurden die Vorteile und Einschränkungen der kostenlosen Bereitstellung bei der Installation von Anwendungen auf einem iOS-Gerät beschrieben. Es wurde veranschaulicht, wie Sie die kostenlose Bereitstellung zum Installieren von Xamarin.iOS-Apps verwenden können.

## <a name="related-links"></a>Verwandte Links

- [Gerätebereitstellung](~/ios/get-started/installation/device-provisioning/index.md)
- [Bereitstellung für Anwendungsdienste](~/ios/get-started/installation/device-provisioning/index.md#provisioning-for-application-services)
