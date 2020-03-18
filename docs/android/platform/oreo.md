---
title: Features von Oreo
description: Dieser Artikel bietet Unterstützung beim Einstieg in die Verwendung von Xamarin.Android zur Entwicklung von Apps für die neueste Android-Version.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: davidortinau
ms.author: daortin
ms.date: 07/06/2018
ms.openlocfilehash: 56430f8c4988c16a31f9806b0ffb8b6355d6340b
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019999"
---
# <a name="oreo-features"></a>Features von Oreo

_Dieser Artikel bietet Unterstützung beim Einstieg in die Verwendung von Xamarin.Android zur Entwicklung von Apps für die neueste Android-Version._

[Android 8.0 Oreo](https://developer.android.com/index.html) ist die neueste Version von Android, die in Google erhältlich ist. Android Oreo bietet zahlreiche neue Features, die für Xamarin.Android-Entwickler interessant sind. Zu diesen Features gehören Benachrichtigungskanäle, Infobadges, benutzerdefinierte Schriftarten in XML, herunterladbare Schriftarten, AutoAusfüllen und ein Bild-in-Bild-Feature (Picture In Picture, PIP). Android Oreo umfasst neue APIs für diese neuen Funktionalitäten, und diese APIs stehen für Xamarin.Android-Apps zur Verfügung, wenn Sie Xamarin.Android 8.0 verwenden.

[![Herobild von Android Oreo](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Dieser Artikel soll Ihnen den Einstieg in die Entwicklung von Xamarin.Android-Apps für Android 8.0 Oreo erleichtern. Es wird erläutert, wie Sie die notwendigen Updates installieren, das SDK konfigurieren und einen Emulator (oder ein Gerät) zum Testen erstellen. Darüber hinaus erhalten Sie einen Überblick über die neuen Features in Android 8.0 Oreo, mit Links zu Beispiel-Apps, die die Verwendung von Android Oreo-Features in Xamarin.Android-Apps veranschaulichen.

## <a name="requirements"></a>Anforderungen

Um Android Oreo-Features in Xamarin-basierten Apps zu verwenden, müssen folgende Voraussetzungen erfüllt sein:

- **Visual Studio** &ndash; Wenn Sie Windows verwenden, ist Version 15.5 oder höher von Visual Studio erforderlich.  Bei Verwendung eines Mac wird Visual Studio für Mac 7.2.0 benötigt.

- **Xamarin.Android** &ndash; Xamarin.Android 8.0 oder höher muss mit Visual Studio installiert und konfiguriert sein.

- **Android SDK** &ndash; Android SDK 8.0 (API 26) oder höher muss über den Android-SDK-Manager installiert werden.

## <a name="getting-started"></a>Erste Schritte

Um mit der Nutzung von Android Oreo mit Xamarin.Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie ein Android Oreo-Projekt erstellen können:

1. Installieren Sie die aktuelle Version von Visual Studio.

2. Installieren Sie **Android 8.0.0 (API 26)** oder höhere Pakete und Tools über den SDK-Manager.

3. Erstellen Sie ein neues Xamarin.Android-Projekt für die Zielplattform Android Oreo (API-Ebene 26).

4. Konfigurieren Sie einen Emulator oder ein Gerät zum Testen von Android Oreo-Apps.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualisieren von Visual Studio und Xamarin.Android

Führen Sie zum Hinzufügen von Android Oreo-Unterstützung zu Visual Studio die folgenden Schritte aus:

<!-- markdownlint-disable MD001 -->

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

- Verwenden Sie für Visual Studio 2019 den [SDK-Manager](~/android/get-started/installation/android-sdk.md), um API-Ebene 26.0 oder höher zu installieren.

- Bei Verwendung von Visual Studio 2017:

    1. Führen Sie ein Update auf Visual Studio 2017, Version 15.7 oder höher durch (siehe [Aktualisieren von Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. Verwenden Sie den [SDK-Manager](~/android/get-started/installation/android-sdk.md), um API-Ebene 26.0 oder höher zu installieren.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

- Aktualisieren Sie auf die neueste stabile Version von Visual Studio für Mac, wie unter [Aktualisieren von Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/update) beschrieben.

-----

Weitere Informationen zur Xamarin-Unterstützung für Android Oreo finden Sie in den [Versionshinweise zu Xamarin.Android 8.0](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/).

### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Um ein Projekt mit Xamarin.Android 8.0 zu erstellen, müssen Sie zunächst den Android-SDK-Manager für Xamarin verwenden, um die SDK-Plattform für **Android 8.0 – Oreo** oder höher zu installieren. Sie müssen außerdem Version 26.0 oder höher der Android SDK Tools installieren.

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Starten Sie den SDK-Manager in Visual Studio (klicken Sie auf **Extras > Android > Android SDK-Manager**).

2. Installieren Sie die Pakete für **Android 8.0 – Oreo**. Wenn Sie den Android SDK-Emulator verwenden, achten Sie darauf, die benötigten **x86**-Systemimages einzuschließen:

    [![Auswählen der Android 8.0-Pakete im Android-SDK-Manager](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK Tools 26.0.** 2 oder höher, **Android SDK Platform-Tools 26.0.0** oder höher und **Android SDK Build-Tools 26.0.0** (oder höher):

    [![Auswählen der Android SDK Tools 26 im Android-SDK-Manager](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie den SDK-Manager (klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**).

2. Installieren Sie die SDK-Pakete für **Android 8.0 – Oreo**. Wenn Sie den Android SDK-Emulator verwenden, achten Sie darauf, die benötigten **x86**-Systemimages einzuschließen:

    [![Auswählen der Android 8.0-Pakete im Android-SDK-Manager](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher und **Android SDK Build-Tools 26.0.0** (oder höher):

    [![Auswählen der Android SDK Tools 26 im SDK-Manager](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----

### <a name="start-a-xamarinandroid-project"></a>Starten eines Xamarin.Android-Projekts

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie mit der Android-Entwicklung mit Xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android (Hallo, Android)](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Xamarin.Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versionseinstellungen für Android 8.0 oder höher konfigurieren. Wenn Sie Ihr Projekt beispielsweise auf Android 8.0 ausrichten, müssen Sie die Android-API-Zielebene Ihres Projekts auf **Android 8.0 (API 26)** festlegen. Es wird empfohlen, außerdem die Ebene für das Zielframework auf API 26 oder höher festzulegen. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-an-emulator-or-device"></a>Konfigurieren eines Emulators oder Geräts

Wenn Sie nach der Installation der Android SDK Tools 26.0 oder höher versuchen, den standardmäßigen GUI-basierten AVD-Manager von Google zu starten, wird möglicherweise die folgende Fehlermeldung angezeigt. Darin werden Sie angewiesen, stattdessen das befehlszeilenbasierte AVD-Verwaltungstool **avdmanager** zu verwenden:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Warnungsdialogfeld zum Android-Emulator-Manager](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Warnungsdialogfeld zum Android-Emulator-Manager](oreo-images/mac/03-avd-warning.png)

-----

Diese Meldung wird angezeigt, weil Google keinen eigenständigen GUI-basierten AVD-Manager mehr bereitstellt, der API 26.0 und höher unterstützt. Für Android 8.0 Oreo müssen Sie entweder den Android-Emulator-Manager von Xamarin oder das Befehlszeilentool `avdmanager` verwenden, um virtuelle Geräte für Android Oreo zu erstellen.

Informationen zur Verwendung des Android-Geräte-Managers zum Erstellen und Verwalten virtueller Geräte finden Sie unter [Verwalten virtueller Geräte mit dem Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Folgen Sie den Schritten im nächsten Abschnitt, um virtuelle Geräte ohne den Android-Geräte-Manager zu erstellen.

#### <a name="creating-virtual-devices-using-avdmanager"></a>Erstellen virtueller Geräte mit „avdmanager“

Führen Sie die folgenden Schritte aus, um mit **avdmanager** ein neues virtuelles Gerät zu erstellen:

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

1. Öffnen Sie ein Eingabeaufforderungsfenster, und legen Sie `JAVA_HOME` auf den Speicherort des Java SDK auf Ihrem Computer fest. Für eine typische Xamarin-Installation können Sie den folgenden Befehl verwenden:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2. Fügen Sie `PATH` den Speicherort des Android SDK-Ordners `bin` hinzu.
    Für eine typische Xamarin-Installation können Sie den folgenden Befehl verwenden:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3. Schließen Sie das Eingabeaufforderungsfenster, und öffnen Sie ein neues Eingabeaufforderungsfenster. Erstellen Sie mit dem Befehl [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) ein neues virtuelles Gerät. Um beispielsweise ein virtuelles Android-Gerät namens **AVD-Oreo-8.0** mit dem x86-Systemimage für API-Ebene 26 zu erstellen, verwenden Sie den folgenden Befehl:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4. Bei Anzeige der Frage **Do you wish to create a custom hardware profile [no]** (Möchten Sie ein benutzerdefiniertes Hardwareprofil erstellen?) können Sie **no** eingeben und das Standardhardwareprofil akzeptieren. Wenn Sie **yes** eingeben, zeigt **avdmanager** verschiedene Fragen in Bezug auf die Anpassung des Hardwareprofils an.

Nachdem Sie Ihr virtuelles Gerät über **avdmanager** erstellt haben, wird es in das Pulldownmenü für Geräte eingeschlossen:

[![Neues virtuelles Android-Gerät zum Pulldownmenü für Geräte hinzugefügt](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

1. Öffnen Sie ein **Terminal**-Fenster, und ändern Sie den Speicherort des Android SDK Tools-Ordners auf Ihrem Mac. Für eine typische Xamarin-Installation können Sie den folgenden Befehl verwenden:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2. Erstellen Sie mit dem Befehl [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) ein neues virtuelles Gerät. Um beispielsweise ein virtuelles Android-Gerät namens **AVD-Oreo-8.0** mit dem x86-Systemimage für API-Ebene 26 zu erstellen, verwenden Sie den folgenden Befehl:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3. Bei Anzeige der Frage **Do you wish to create a custom hardware profile [no]** (Möchten Sie ein benutzerdefiniertes Hardwareprofil erstellen?) können Sie **no** eingeben und das Standardhardwareprofil akzeptieren. Wenn Sie **yes** eingeben, zeigt **avdmanager** verschiedene Fragen in Bezug auf die Anpassung des Hardwareprofils an.

Nachdem Sie Ihr virtuelles Gerät über **avdmanager** erstellt haben, wird es in das Pulldownmenü für Geräte eingeschlossen:

[![Neues virtuelles Android-Gerät zum Pulldownmenü für Geräte hinzugefügt](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Weitere Informationen zum Konfigurieren eines Android-Emulators zum Testen und Debuggen finden Sie unter [Debuggen im Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

Wenn Sie ein physisches Gerät wie Nexus oder Pixel verwenden, können Sie Ihr Gerät entweder automatisch mithilfe von OTA-Updates (Over the Air) aktualisieren oder ein Systemimage herunterladen und Ihr Gerät direkt flashen. Weitere Informationen zum manuellen Aktualisieren Ihres Geräts auf Android Oreo finden Sie unter [Factory Images for Nexus and Pixel Devices](https://developers.google.com/android/images) (Werkseitige Images für Nexus- und Pixel-Geräte).

## <a name="new-features"></a>Neue Funktionen

In Android Oreo werden verschiedene neue Features eingeführt, darunter Benachrichtigungskanäle, Infobadges, benutzerdefinierte Schriftarten in XML, herunterladbare Schriftarten, AutoAusfüllen und ein Bild-in-Bild-Feature (Picture In Picture, PIP). Diese Features werden in den folgenden Abschnitten näher beleuchtet, und es werden Links mit weiteren Informationen zur Nutzung dieser Features in Ihrer App bereitgestellt.

### <a name="notification-channels"></a>Benachrichtigungskanäle

*Benachrichtigungskanäle* sind App-definierte Kategorien für Benachrichtigungen.
Sie können für jede Art von Benachrichtigung, die Sie senden müssen, einen Benachrichtigungskanal erstellen, und Sie können Benachrichtigungskanäle erstellen, um die von den Benutzern Ihrer Apps getroffenen Entscheidungen widerzuspiegeln. Mit dem neuen Feature für Benachrichtigungskanäle wird den Benutzern eine differenzierte Steuerung der verschiedene Arten von Benachrichtigungen ermöglicht. Wenn Sie beispielsweise eine Messaging-App implementieren, können Sie für jede Konversationsgruppe, die von einem Benutzer angelegt wird, separate Benachrichtigungskanäle anlegen.

Unter [Benachrichtigungskanäle](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) wird erklärt, wie ein Benachrichtigungskanal erstellt und für die Veröffentlichung lokaler Benachrichtigungen verwendet wird. Ein Codebeispiel aus der Praxis finden Sie im Beispiel [NotificationChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels). Diese Beispiel-App verwaltet zwei Kanäle und legt zusätzliche Benachrichtigungsoptionen fest.

### <a name="notification-badges"></a>Infobadges

Infobadges sind kleine Punkte, die auf App-Symbolen angezeigt werden, wie in diesem Screenshot dargestellt:

[![Beispiele für Infobadges auf App-Symbolen ](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Diese Punkte zeigen an, dass neue Benachrichtigungen für mindestens einen Benachrichtigungskanal in der mit diesem App-Symbol verknüpften App vorliegen &ndash; Benachrichtigungen, die der Benutzer noch nicht geschlossen oder auf die er noch nicht reagiert hat. Benutzer können durch langes Drücken auf ein Symbol die Benachrichtigung anzeigen, sie schließen oder eine Option im Menü auswählen, das durch langes Drücken auf ein Symbol geöffnet wird.

Weitere Informationen zu Infobadges finden Sie im Android-Entwicklerthema [Übersicht zu Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges).

### <a name="custom-fonts-in-xml"></a>Benutzerdefinierte Schriftarten in XML

Android Oreo führt *Schriftarten in XML* ein, wodurch Sie benutzerdefinierte Schriftarten als Ressourcen einbetten können. Es werden OpenType- (**OTF**) und TrueType-Schriftartformate (**TTF**) unterstützt. Gehen Sie zum Hinzufügen von Schriftarten als Ressourcen folgendermaßen vor:

1. Erstellen Sie einen Ordner **Resources/font**.

2. Kopieren Sie Ihre Schriftartdateien (z. B. **TTF**- und **OTF**-Dateien) nach **Resources/font**. 

3. Falls erforderlich, benennen Sie jede Schriftdatei so um, dass sie den Android-Dateinamenskonventionen entspricht (d. h. verwenden Sie nur Kleinbuchstaben *a–z*, *0–9* und Unterstriche in Dateinamen). Beispielsweise könnten die Schriftartdatei `Pacifico-Regular.ttf` in `pacifico.ttf` umbenannt werden.

4. Wenden Sie die benutzerdefinierte Schriftart mit dem neuen Attribut `android:fontFamily` in Ihrer Layout-XML an. Beispielsweise verwendet die folgende `TextView`-Deklaration die hinzugefügte Schriftartressource **pacifico.ttf**:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Sie können auch eine XML-Datei mit Schriftfamilien erstellen, die mehrere Schriftarten sowie Details zu Schriftschnitt und -breite enthalten. Weitere Informationen finden Sie im Android-Entwicklerthema [Fonts in XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) (Schriftarten in XML).

### <a name="downloadable-fonts"></a>Herunterladen von Schriftarten

Beginnend mit Android Oreo können Apps Schriftarten von einem Anbieter anfordern, anstatt sie in der APK zu bündeln. Schriftarten werden nur bei Bedarf aus dem Netzwerk heruntergeladen. Dieses Feature verringert die APK-Größe, wodurch der Telefonspeicher geschont wird und weniger mobile Daten verbraucht werden. Sie können dieses Feature auch auf den Android-API-Versionen 14 und höher verwenden, indem Sie das Paket „Android Support Library 26“ installieren.

Wenn Ihre App eine Schriftart benötigt, erstellen Sie ein `FontsRequest`-Objekt (das angibt, welche Schriftart heruntergeladen werden soll) und übergeben dieses an eine `FontsContract`-Methode, um die Schriftart herunterzuladen. In den folgenden Schritten wird der Vorgang zum Herunterladen von Schriftarten ausführlicher beschrieben:

1. Instanziieren Sie ein [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html)-Objekt. 

2. Erstellen Sie eine Unterklasse, und instanziieren Sie [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3. Implementieren Sie die [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29)-Methode, die zum Ausführen der Schriftartanforderung verwendet wird.

4. Implementieren Sie die Methode [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29), mit der Ihre App über Fehler informiert wird, die während der Schriftartanforderung möglicherweise auftreten.

5. Rufen Sie die [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback))-Methode auf, um die Schriftart aus dem Schriftartanbieter abzurufen. 

Wenn Sie die `RequestFonts`-Methode aufrufen, überprüft diese zunächst, ob die Schriftart lokal zwischengespeichert wurde (durch einen vorherigen Aufruf von `RequestFont`). Ist die Schriftart nicht zwischengespeichert, wird der Schriftartanbieter aufgerufen und die Schriftart asynchron abgerufen. Anschließend werden die Ergebnisse durch einen Aufruf Ihrer `OnTypeFaceRetrieved`-Methode an Ihre App zurückgegeben.

Das Beispiel [DownloadableFonts](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts) veranschaulicht die Verwendung des in Android Oreo eingeführten Features zum Herunterladen von Schriftarten. 

Weitere Informationen zum Herunterladen von Schriftarten finden Sie im Android-Entwicklerthema [Downloadable Fonts](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) (Herunterladbare Schriftarten).

### <a name="autofill"></a>AutoAusfüllen

Das neue _AutoAusfüllen_-Framework in Android Oreo erleichtert den Benutzern wiederholt ausgeführte Aufgaben wie Anmeldung, Kontoerstellung und Kreditkartentransaktionen. Benutzer müssen weniger Zeit für das erneute Eingeben von Informationen aufwenden (was zu Eingabefehlern führen kann). Damit Ihre Anwendung mit dem AutoAusfüllen-Framework arbeiten kann, muss ein AutoAusfüllen-Dienst in den Systemeinstellungen aktiviert werden (Benutzer können AutoAusfüllen-Feature aktivieren oder deaktivieren).

Das Beispiel [AutofillFramework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework) veranschaulicht die Verwendung des AutoAusfüllen-Frameworks. Es umfasst Implementierungen von Clientaktivitäten mit Ansichten, die automatisch ausgefüllt werden sollen, und einen Dienst, der Clientaktivitäten automatisch ausgefüllte Daten zur Verfügung stellen kann.

Weitere Informationen zum neuen AutoAusfüllen-Feature und zur Optimierung Ihrer App für diese Funktion finden Sie im Android-Entwicklerthema [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html) (AutoAusfüllen-Framework).

### <a name="picture-in-picture-pip"></a>Bild-in-Bild (Picture in Picture, PIP)

Android Oreo ermöglicht es, eine Aktivität im Bild-in-Bild-Modus (PIP) zu starten und den Bildschirm einer anderen Aktivität zu überlagern. Dieses Feature ist für die Wiedergabe von Videos vorgesehen.

Um anzugeben, dass die Aktivität Ihrer Anwendung den PIP-Modus verwenden kann, legen Sie das folgende Flag im Android-Manifest auf TRUE fest:

```xml
android:supportsPictureInPicture
```

Um anzugeben, wie sich Ihre Aktivität im PIP-Modus verhalten soll, verwenden Sie das neue [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html)-Objekt. `PictureInPictureParams` repräsentiert einen Satz an Parametern, die Sie zum Initialisieren und Aktualisieren einer Aktivität im PIP-Modus verwenden können (z. B. das bevorzugte Seitenverhältnis der Aktivität). In Android Oreo wurden `Activity` die folgenden neuen PIP-Methoden hinzugefügt:

- [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; versetzt die Aktivität in den PIP-Modus. Die Aktivität wird in der Ecke des Bildschirms platziert, und der Rest des Bildschirms wird mit der zuvor auf dem Bildschirm vorhandenen Aktivität gefüllt.

- [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; aktualisiert die PIP-Konfigurationseinstellungen der Aktivität (z. B. eine Änderung des Seitenverhältnisses).

Das Beispiel [PictureInPicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture) veranschaulicht die grundlegende Verwendung des in Oreo eingeführten PIP-Modus für Handheldgeräte. Im Beispiel wird ein Video ohne Unterbrechung weiter wiedergegeben, während zwischen den Anzeigemodi oder anderen Aktivitäten hin- und hergeschaltet wird.

### <a name="other-features"></a>Andere Funktionen

Android Oreo bietet viele weitere neue Features wie etwa die Emoji-Unterstützungsbibliothek, eine Standort-API, Begrenzungen für die Hintergrundausführung, eine breite Farbskala für Apps, neue Audiocodecs, WebView-Verbesserungen, eine verbesserte Tastatur-Navigationsunterstützung und eine neue AAudio-API (Pro-Audio) für eine hochleistungsfähige Audiowiedergabe mit niedriger Latenz. Weitere Informationen zu diesen Features finden Sie im Android-Entwicklerthema [Android Oreo Features and APIs](https://developer.android.com/about/versions/oreo/android-8.0.html) (Android Oreo-Features und -APIs).

## <a name="behavior-changes"></a>Verhaltensänderungen

Android Oreo umfasst eine Vielzahl von System- und API-Verhaltensänderungen, die sich auf die Funktionalität vorhandener Apps auswirken können. Diese Änderungen werden nachfolgend beschrieben.

### <a name="background-execution-limits"></a>Begrenzungen für die Hintergrundausführung

Zur Verbesserung des Benutzererlebnisses gelten in Android Oreo Beschränkungen für Apps, während sie im Hintergrund ausgeführt werden. Wenn der Benutzer beispielsweise ein Video anschaut oder ein Spiel spielt, kann eine im Hintergrund ausgeführte App die Leistung einer videointensiven App beeinträchtigen, die im Vordergrund ausgeführt wird. Deshalb gelten in Android Oreo die folgenden Einschränkungen für Apps, die nicht direkt mit dem Benutzer interagieren:

1. **Einschränkung von Hintergrunddiensten** &ndash; Wenn eine App im Hintergrund ausgeführt wird, erhält sie ein Zeitfenster von einigen Minuten, in dem sie weiterhin Dienste erstellen und nutzen darf. Nach Ablauf des Zeitfensters beendet Android den Hintergrunddienst der App und betrachtet sie als _im Leerlauf_.

2. **Übertragungseinschränkungen** &ndash; Android 7.0 (API 25) legt Beschränkungen für Übertragungen fest, für deren Empfang sich eine App registriert. Android Oreo verschärft diese Beschränkungen. Beispielsweise ist es nicht mehr möglich, dass Android Oreo-Apps Broadcast Receiver für implizite Übertragungen in ihren Manifesten registrieren.

Weitere Informationen zu den neuen Beschränkungen für die Hintergrundausführung finden Sie im Android-Entwicklerthema [Background Execution Limits](https://developer.android.com/about/versions/oreo/background.html) (Begrenzungen für die Hintergrundausführung).

### <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

Apps mit Android Oreo als Zielplattform müssen so geändert werden, dass sie die folgenden Änderungen unterstützen (sofern erforderlich):

- Android Oreo ermöglicht es nicht mehr, die Priorität einzelner Benachrichtigungen festzulegen. Stattdessen wird beim Erstellen eines Benachrichtigungskanals eine empfohlene Relevanzebene festgelegt. Die einem Benachrichtigungskanal zugewiesene Relevanzebene gilt für alle Benachrichtigungen, die darin gepostet werden.

- Für Apps mit Android Oreo als Zielplattform funktioniert `PendingIntent.GetService()` aufgrund neuer Beschränkungen für im Hintergrund gestartete Dienste nicht. Wenn Ihre App für Android Oreo vorgesehen ist, verwenden Sie stattdessen [PendingIntent.GetBroadcast](xref:Android.App.PendingIntent.GetBroadcast*).  

## <a name="sample-code"></a>Beispielcode

Es stehen verschiedene Xamarin.Android-Beispiele zur Verfügung, die veranschaulichen, wie Sie die Vorteile der Android Oreo-Features nutzen:

- [NotificationsChannels](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-notificationchannels) zeigt, wie das in Android Oreo eingeführte neue System der Benachrichtigungskanäle genutzt werden kann. Dieses Beispiel verwaltet zwei Benachrichtigungskanäle: einen mit Standardrelevanz und einen mit hoher Relevanz.

- [PictureInPicture](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-pictureinpicture) veranschaulicht die grundlegende Verwendung des in Oreo eingeführten PIP-Modus für Handheldgeräte. Im Beispiel wird ein Video ohne Unterbrechung weiter wiedergegeben, während zwischen den Anzeigemodi oder anderen Aktivitäten hin- und hergeschaltet wird.

- [AutofillFramework](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-autofillframework) zeigt die Verwendung des AutoAusfüllen-Frameworks. Es umfasst Implementierungen von Clientaktivitäten mit Ansichten, die automatisch ausgefüllt werden sollen, und einen Dienst, der Clientaktivitäten automatisch ausgefüllte Daten zur Verfügung stellen kann.

- [DownloadableFonts](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-downloadablefonts) stellt ein Beispiel für die Verwendung der zuvor beschriebenen Funktion für herunterladbare Schriften bereit.

- [EmojiCompat](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-emojicompat) veranschaulicht die Verwendung der EmojiCompat-Unterstützungsbibliothek. Sie können Ihre Apps mithilfe dieser Bibliothek daran hindern, fehlende Emojis als „Tofu-Zeichen“ anzuzeigen.

- [LocationUpdatesPendingIntent](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdpendintent) veranschaulicht die Verwendung der Standort-API zum Abrufen von Updates zum Gerätestandort über `PendingIntent`.

- [LocationUpdatesForegroundService ](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-o-androidplaylocation-locupdfgservice) zeigt, wie mit der Standort-API über einen gebundenen und gestarteten Vordergrunddienst Updates zum Gerätestandort abgerufen werden können.

## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Android 8.0 Oreo-Entwicklung mit C#**

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android Oreo vorgestellt und erläutert, wie die neuesten Tools und Pakete zur Xamarin.Android-Entwicklung für Android Oreo installiert und konfiguriert werden. Es wurde ein Überblick der wichtigsten Android Oreo-Features mit Links zu Codebeispielen für verschiedene neue Features bereitgestellt. Darüber hinaus wurden in diesem Artikel Links zur API-Dokumentation und zu relevanten Themen für Android-Entwickler bereitgestellt, um Ihnen den Einstieg in die App-Entwicklung für Android Oreo zu erleichtern. Ferner wurden die wichtigsten Verhaltensänderungen von Android Oreo beschrieben, die sich auf vorhandene Apps auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Android 8.0 Oreo](https://developer.android.com/index.html)
