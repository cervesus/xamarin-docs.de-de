---
title: Oreo-Features
description: Informationen zum Einstieg in Xamarin.Android zum Entwickeln von apps für die neueste Version von Android.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 07/06/2018
ms.openlocfilehash: ca9c4ed0871b91bed82f746ccb36af9fb32816c0
ms.sourcegitcommit: 5fc171a45697f7c610d65f74d1f3cebbac445de6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2018
ms.locfileid: "52171780"
---
# <a name="oreo-features"></a>Oreo-Features

_Informationen zum Einstieg in Xamarin.Android zum Entwickeln von apps für die neueste Version von Android._

[Android 8.0 Oreo](https://developer.android.com/index.html) die neueste Version von Android von Google verfügbar wird. Android Oreo bietet viele neue Funktionen für Xamarin.Android-Entwickler. Beispiele hierfür sind benachrichtigungskanäle, Benachrichtigung Badges, benutzerdefinierte Schriftarten in XML, herunterladbaren Schriftarten, AutoAusfüllen und Bild in Abbildung (PIP). Android Oreo enthält neue APIs für diese neuen Funktionen, und diese APIs sind für Xamarin.Android-apps, die bei der Verwendung von Xamarin.Android 8.0 und höher.

[![Android Oreo Hero-Bild](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

In diesem Artikel ist strukturiert, um Ihnen bei der Entwicklung von Xamarin.Android-apps für Android 8.0 Oreo beim Einstieg helfen. Es wird erläutert, wie die erforderlichen Updates zu installieren, konfigurieren Sie das SDK und erstellen einen Emulator (oder Gerät) zum Testen. Darüber hinaus eine Übersicht über die neuen Features in Android 8.0 Oreo mit Links zu Beispielanwendungen, die veranschaulichen, wie Sie Android Oreo-Features in Xamarin.Android-apps verwenden.


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die Android Oreo-Features in Xamarin-basierte apps verwenden:

-   **Visual Studio** &ndash; Version 15.5 oder höher von Visual Studio ist erforderlich, wenn es sich bei Verwendung von Windows.  Wenn Sie einen Mac verwenden, muss Visual Studio für Mac, Version 7.2.0.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 oder höher muss installiert und mit Visual Studio konfiguriert sein.

-   **Android SDK** &ndash; Android SDK 8.0 (API 26) oder höher muss installiert sein über den Android SDK Manager.



## <a name="getting-started"></a>Erste Schritte

Zum Einstieg in Android Oreo mit Xamarin.Android, müssen Sie herunterladen und die neuesten Tools und SDK-Pakete installieren, bevor Sie ein Android Oreo-Projekt erstellen können:

1. Aktualisieren Sie auf die neueste Version von Visual Studio.

2. Installieren Sie die **Android 8.0.0 (API 26)** oder spätere Pakete und Tools über den SDK-Manager.

3. Erstellen eines neuen Xamarin.Android-Projekts, das als Ziel Android Oreo (API 26).

4. Konfigurieren Sie einen Emulator oder Gerät zum Testen von apps für Android Oreo.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:



### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualisieren von Visual Studio- und Xamarin.Android

Um Visual Studio Android Oreo-Unterstützung hinzuzufügen, führen Sie folgende Schritte aus:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

-   Wenn Sie Visual Studio 2017 verwenden: 

    1. Update für Visual Studio 2017 Version 15.7 oder höher (finden Sie unter [Aktualisieren von Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. Verwenden der [SDK-Manager](~/android/get-started/installation/android-sdk.md) , API-Ebene 26.0 oder höher zu installieren.

-   Wenn Sie Visual Studio 2015 verwenden, empfehlen wir die Herabstufung SDK Tools auf 25 und die Verwendung des alten Google Emulator Managers GUI. SDK-Tools 25 können zusammen mit der API 26, 27 und neuere, weiterhin verwendet werden und keine Auswirkungen auf in die Entwicklung für neue Plattformen. Diese erhalten Schnittstelle Sie für die Verwaltung Ihres Android-SDK für ältere Versionen von Visual Studio.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

-   Aktualisieren Sie auf die neueste stabile Version von Visual Studio 2017 für Mac Siehe [Aktualisieren von Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/update).

-----

Weitere Informationen zu Xamarin-Unterstützung für Android Oreo, finden Sie unter den [Anmerkungen zur Version von Xamarin.Android 8.0](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Installieren Sie das Android SDK

Sie müssen zum Erstellen eines Projekts mit Xamarin.Android 8.0, zuerst den Xamarin Android SDK Manager verwenden, installieren Sie die SDK-Plattform für **Android 8.0 - Oreo** oder höher. Sie müssen auch vom Android SDK Tools 26.0 oder höher installieren.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Starten Sie den SDK-Manager (klicken Sie in Visual Studio auf **Tools > Android > Android SDK Manager**).

2. Installieren Sie die **Android 8.0 - Oreo** Pakete. Wenn Sie den Android SDK-Emulator verwenden, werden Sie sicher, dass die **X86** Systemimages, die Sie benötigen:

    [![Wählen Android 8.0-Pakete im Android SDK-Manager](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher und **Android SDK Build-Tools 26.0.0** (oder höher):

    [![Auswählen von Android SDK Tools 26 im Android SDK Manager](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie den SDK-Manager (klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**).

2. Installieren Sie die **Android 8.0 - Oreo** SDK-Pakete. Wenn Sie den Android SDK-Emulator verwenden, werden Sie sicher, dass die **X86** Systemimages, die Sie benötigen:

    [![Auswählen von Android 8.0-Paketen im SDK-Manager](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher und **Android SDK Build-Tools 26.0.0** (oder höher):

    [![Auswählen von Android SDK Tools 26 in den SDK-Manager](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Ein Xamarin.Android-Projekt starten

Erstellen eines neuen Xamarin.Android-Projekts an. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/index.md) um weitere Informationen zum Erstellen von Xamarin.Android-Projekte.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die versionseinstellungen in Android 8.0 und höher als Ziel konfigurieren. Z. B. zur Ausrichtung auf des Projekts für Android 8.0, müssen Sie konfigurieren die Android-API-Zielebene des Projekts in **Android 8.0 (API 26)**. Es wird empfohlen, dass Sie auch Ihr Framework Zielebene auf API 26 oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebene Level, finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Konfigurieren Sie einen Emulator oder Gerät

Wenn Sie versuchen, um die Standardeinstellung Google-GUI-basierte AVD-Manager zu starten, nach der Installation von Android SDK Tools 26.0 oder höher erhalten Sie möglicherweise Folgendes Fehlerdialogfeld, die Sie verwenden, das über die Befehlszeile AVD-Manager-Tool weist **Avdmanager** stattdessen :

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Android-Emulator-Manager ein Dialogfeld mit Warnung](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Android-Emulator-Manager ein Dialogfeld mit Warnung](oreo-images/mac/03-avd-warning.png)

-----

Diese Meldung wird angezeigt, da Google nicht mehr eine eigenständige grafische Benutzeroberfläche AVD-Manager enthält, die API 26.0 oder höher unterstützt. Für Android 8.0 Oreo, verwenden Sie den Xamarin Android-Emulator-Manager oder der Befehlszeile `avdmanager` Tool zum Erstellen von virtueller Geräten für Android Oreo.

Um die Android-Geräte-Manager zu erstellen und Verwalten von virtuellen Geräten verwenden, finden Sie unter [Verwalten von virtuellen Geräten mit Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Führen Sie die Schritte im nächsten Abschnitt, um virtuelle Geräte ohne die Android-Geräte-Manager zu erstellen.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Erstellen von virtuellen Geräten mithilfe von avdmanager

Verwendung von **Avdmanager** um ein neues virtuelles Gerät zu erstellen, gehen Sie folgendermaßen vor:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Öffnen Sie ein Eingabeaufforderungsfenster, und legen `JAVA_HOME` auf den Speicherort des Java-SDK auf Ihrem Computer. Für eine Standardinstallation von Xamarin können Sie den folgenden Befehl aus:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Fügen Sie den Speicherort von Android SDK `bin` Ordner, um Ihre `PATH`.
    Für eine Standardinstallation von Xamarin können Sie den folgenden Befehl aus:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Schließen Sie das Eingabeaufforderungsfenster, und öffnen Sie ein neues Eingabeaufforderungsfenster. Erstellen Sie ein neues virtuelles Gerät mithilfe der [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) Befehl. Z. B. So erstellen Sie ein AVD namens **AVD-Oreo-8.0** der X86 Systemabbild für API-Ebene 26, verwenden Sie den folgenden Befehl aus:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Wenn Sie gefragt werden mit **möchten Sie das Erstellen eines benutzerdefinierten Hardwareprofils [No]** gelangen **keine** und akzeptieren Sie das Standardprofil für die Hardware. Wenn Sie z. B. **Ja**, **Avdmanager** fordert Sie eine Liste mit Fragen zum Anpassen des Hardwareprofils.

Nachdem Sie **Avdmanager** um Ihr virtuelles Gerät zu erstellen, wird es in der Geräte-Pulldownmenü enthalten sein:

[![Neues AVD zum Geräte-Pulldownmenü hinzugefügt](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Öffnen einer **Terminal** und wechseln Sie zum Speicherort des Android SDK Tools-Verzeichnis auf Ihrem Mac. Für eine Standardinstallation von Xamarin können Sie den folgenden Befehl aus:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Erstellen Sie ein neues virtuelles Gerät mithilfe der [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) Befehl. Z. B. So erstellen Sie ein AVD namens **AVD-Oreo-8.0** der X86 Systemabbild für API-Ebene 26, verwenden Sie den folgenden Befehl aus:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Wenn Sie gefragt werden mit **möchten Sie das Erstellen eines benutzerdefinierten Hardwareprofils [No]** gelangen **keine** und akzeptieren Sie das Standardprofil für die Hardware. Wenn Sie z. B. **Ja**, **Avdmanager** fordert Sie eine Liste mit Fragen zum Anpassen des Hardwareprofils.

Nach der Verwendung von **Avdmanager** um Ihr virtuelles Gerät zu erstellen, wird es in der Geräte-Pulldownmenü enthalten sein:

[![Neues AVD zum Geräte-Pulldownmenü hinzugefügt](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Weitere Informationen zum Konfigurieren von Android-Emulator zum Testen und Debuggen, finden Sie unter [Debuggen auf Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

Wenn Sie z. B. eine Nexus oder eines Pixels ein physisches Gerät verwenden, können Sie entweder Aktualisieren Ihres Geräts über automatische über die Updates per Funk (OTA) oder Laden ein Systemabbild und flash Ihr Gerät direkt. Weitere Informationen zum manuellen Aktualisieren der Ihr Gerät auf Android Oreo finden Sie unter [Factory Images für Nexus und Pixel-Geräte](https://developers.google.com/android/images).



## <a name="new-features"></a>Neue Funktionen

Android Oreo führt zu einer Vielzahl neuer Features und Funktionen, z. B. benachrichtigungskanäle, Benachrichtigung Badges, benutzerdefinierte Schriftarten im XML-Format, herunterladbaren Schriftarten, AutoAusfüllen und Bild-im-Bild. In den folgenden Abschnitten veranschaulichen die folgenden Funktionen und Links können Sie beginnen, verwenden diese in Ihrer app bieten.



### <a name="notification-channels"></a>Benachrichtigungskanäle

*Benachrichtigungskanäle* app benutzerdefinierte Kategorien für Benachrichtigungen.
Sie können einen Benachrichtigungskanal für jede Art von Benachrichtigung, die Sie senden müssen erstellen, und erstellen benachrichtigungskanäle entsprechend der Auswahl von Benutzern Ihrer App vorgenommen. Das neue Kanäle Benachrichtigungsfeature ermöglicht es Sie Benutzern eine differenzierte Steuerung der verschiedenen Arten von Benachrichtigungen zu gewähren. Wenn Sie eine messaging-app implementieren, können Sie beispielsweise separate benachrichtigungskanäle für jede Konversationsgruppe erstellen, die von einem Benutzer erstellt wird.

[Benachrichtigungskanäle](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) wird erläutert, wie ein Benachrichtigungskanal für erstellen und verwenden sie lokale Benachrichtigungen senden. Eine reale-Codebeispiel finden Sie unter den [von Benachrichtigungskanälen](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Beispiel; diese Beispiel-app verwaltet zwei Kanäle und zusätzliche Benachrichtigungsoptionen legt sie fest.



### <a name="notification-badges"></a>Benachrichtigung Badges

Badges, die Benachrichtigung sind kleine Punkte, die über app-Symbole angezeigt werden, wie im folgenden Screenshot gezeigt:

[![Beispiel-Notification-Badges auf app-Symbole](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Diese Punkte anzugeben, dass es neue Benachrichtigungen für eine oder mehrere benachrichtigungskanäle in der app sind, app-Symbol zugeordnet ist &ndash; Hierbei handelt es sich um Benachrichtigungen, die der Benutzer noch nicht geschlossen oder eine Aktion ausgeführt hat. Benutzer können lange-auf ein Symbol, drücken Sie, um auf einen Badge Benachrichtigung zugeordneten Benachrichtigungen Blick geschlossen wird, oder dient Benachrichtigungen aus dem Menü drücken Sie Long-Wert-, Appeaars.

Weitere Informationen zu Notification Badges, finden Sie in der Android-Entwickler [Benachrichtigung Badges](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) Thema.



### <a name="custom-fonts-in-xml"></a>Benutzerdefinierte Schriftarten im XML-Format

Android Oreo führt *Schriftarten im XML-Format*, wodurch es möglich, dass Sie benutzerdefinierte Schriftarten als Ressourcen zu integrieren. OpenType (**.otf**) und TrueType-(**ttf**) Schriftartformate werden unterstützt. Um Schriftarten als Ressourcen hinzuzufügen, führen Sie folgende Schritte aus:

1. Erstellen Sie eine **Ressourcen/Schriftart** Ordner.

2. Kopieren Sie die Schriftartendateien (beispielsweise **ttf** und **.otf** Dateien), **Ressourcen/Schriftart**. 

3. Benennen Sie bei Bedarf jedes Schriftartdatei, damit sie den Android Dateinamenskonventionen entspricht (d. h. mit nur Kleinschreibung *a – Z*, *0-9*, und Unterstriche im Namen von Dateien). Z. B. die Schriftartdatei `Pacifico-Regular.ttf` konnte umbenannt werden, z. B. `pacifico.ttf`.

4. Die benutzerdefinierte Schriftart anwenden, indem Sie mit dem neuen `android:fontFamily` -Attribut in Ihrem Layout-XML. Beispielsweise die folgenden `TextView` Deklaration verwendet die hinzugefügte **pacifico.ttf** schriftartenressource:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Sie können auch eine Schriftart Familie XML-Datei erstellen, die mehrere Schriftarten sowie Details zu Stil und Gewichtung beschreibt. Weitere Informationen finden Sie in der Android-Entwickler [Schriftarten im XML-Format](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) Thema.


### <a name="downloadable-fonts"></a>Herunterladbare Schriftarten

Ab Android Oreo, können apps Schriftarten Bündeln dieser in das APK, sondern einen Anbieter aus anfordern. Schriftarten werden aus dem Netzwerk heruntergeladen werden, nur bei Bedarf. Diese Funktion verringert die APK-Größe, Arbeitsspeicher und Mobilfunk Daten Telefonverwendung sparen. Sie können dieses Feature auch durch Installieren des Pakets für Android Support-Bibliothek 26 auf Android-API-Versionen 14 und höher verwenden.

Wenn deine app braucht dringend eine Schriftart, erstellen Sie eine `FontsRequest` Objekt (das Angeben der Schriftart zum Herunterladen), und übergeben Sie sie an einer `FontsContract` Methode, um die Schriftart herunterzuladen. Die folgenden Schritte beschreiben den Downloadprozess der Schriftart im Detail:

1.  Instanziieren einer [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) Objekt. 

2.  Unterklasse und instanziieren Sie [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implementieren der [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) -Methode, die zur Behandlung der Abschluss der Anforderung Schriftart verwendet wird.

4.  Implementieren der [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) -Methode, die verwendet wird, um Ihre app über Fehler informiert werden, die während des Prozesses Schriftart ausgeführt werden.

5.  Rufen Sie die [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) Methode, um die Schriftart des Schriftart-Anbieters abzurufen. 

Beim Aufrufen der `RequestFonts` -Methode überprüft zunächst um festzustellen, ob die Schriftart lokal zwischengespeichert werden (aus einem vorherigen Aufruf von `RequestFont`). Wenn sie nicht zwischengespeichert wird, ruft der Anbieter für die Schriftart, ruft die Schriftart asynchron ab und übergibt dann die Ergebnisse zurück an Ihre app durch den Aufruf Ihrer `OnTypeFaceRetrieved` Methode.

Die [herunterladbare Schriftarten](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) Beispiel veranschaulicht, wie die herunterladbare Schriftarten-Funktion, die in Android Oreo eingeführt. 

Weitere Informationen zum Herunterladen von Schriftarten finden Sie in der Android-Entwickler [herunterladbare Schriftarten](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) Thema.



### <a name="autofill"></a>AutoAusfüllen:

Die neue _AutoAusfüllen_ -Framework in Android Oreo erleichtert es Benutzern, sich wiederholende Aufgaben wie z. B. Anmeldenamen, dem kontoerstellung und Kreditkartentransaktionen zu behandeln. Benutzer verbringen weniger Zeit mit dem erneuten eingeben von Informationen (was zur Eingabe von Fehlern führen kann). Bevor Ihre app mit dem Autofill Framework arbeiten kann, muss ein AutoAusfüllen-Dienst in den Systemeinstellungen aktiviert sein (Benutzer können aktivieren oder Deaktivieren von AutoAusfüllen).

Die [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) Beispiel veranschaulicht die Verwendung der AutoAusfüllen-Framework. Es enthält Implementierungen von Client-Aktivitäten mit Ansichten, die Autofilled und ein Dienst, der für den Client Aktivitäten AutoAusfüllen-Daten bereitstellen kann sollten.

Weitere Informationen über die neue AutoAusfüllen-Funktion und wie Sie Ihre app für das automatische Ausfüllen zu optimieren, finden Sie in der Android-Entwickler [Autofill Framework](https://developer.android.com/guide/topics/text/autofill.html) Thema.



### <a name="picture-in-picture-pip"></a>Picture in Picture (PIP)

Android Oreo ermöglicht für eine Aktivität zum Starten im Bild-im-Bild (PIP) Modus überlagern den Bildschirm eine andere Aktivität. Dieses Feature ist für die Videowiedergabe vorgesehen.

Um anzugeben, dass es sich bei Ihrer app-Aktivität PIP-Modus verwenden können, legen Sie das Flag auf "true", in der Android-Manifest ein:

```xml
android:supportsPictureInPicture
```

Um anzugeben, wie Ihre Aktivität sich Verhalten soll, wenn es im PIP-Modus befindet, verwenden Sie die neue [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) Objekt. `PictureInPictureParams` Stellt einen Satz von Parametern, mit denen Sie initialisiert werden, und aktualisieren eine Aktivität in der PIP-Modus (z. B. die Aktivität die bevorzugte Seitenverhältnis) dar. Die folgenden neuen PIP-Methoden wurden hinzugefügt, um `Activity` in Android Oreo:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; wird von der Aktivität in den PIP-versetzt. Die Aktivität befindet sich in der Ecke des Bildschirms, und der Rest des Bildschirms mit der vorherigen Aktivität, die auf dem Bildschirm gefüllt ist.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; Aktivitäts PIP-Konfigurationseinstellungen (z. B. eine Änderung im Seitenverhältnis) aktualisiert.

Die [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Beispiel veranschaulicht die grundlegende Verwendung des Bild-im-Bild (PiP) Modus für Handheldgeräte Oreo eingeführt. Das Beispiel spielt ein Video, das weiterhin ohne Unterbrechung beim zwischen Anzeigemodi oder andere Aktivitäten hin und her wechseln.



### <a name="other-features"></a>Andere Funktionen

Android Oreo enthält viele andere neue Features wie der Emoji Speicherort-API-Bibliothek-Hintergrund Unterstützungsgrenzen, Wide-Farbskala-Farbe für apps, neuen Codecs für Audioausgabe, WebView-Erweiterungen, verbesserte Navigation tastaturunterstützung und eine neue AAudio (pro Audio)-API für hohe Leistung mit geringer Latenz Audio, Weitere Informationen zu diesen Funktionen finden Sie in der Android-Entwickler [Android Oreo-Funktionen und APIs](https://developer.android.com/about/versions/oreo/android-8.0.html) Thema.



## <a name="behavior-changes"></a>Verhaltensänderungen

Android Oreo umfasst eine Vielzahl von System und API-verhaltensänderungen, die auf die Funktionalität vorhandener Apps auswirken können. Diese Änderungen werden wie folgt beschrieben werden.


### <a name="background-execution-limits"></a>Hintergrund-Execution-Grenzwerte

Um die benutzerfreundlichkeit zu verbessern, gelten Android Oreo Einschränkungen für apps Möglichkeiten während der Ausführung im Hintergrund. Z. B. wenn der Benutzer ein Video oder ein Spiel, könnten eine im Hintergrund ausgeführte app die Leistung von grafikintensiven Apps im Vordergrund ausgeführt beeinträchtigen. Daher wird Android Oreo die folgenden Einschränkungen für apps, die nicht direkt mit dem Benutzer interagieren:

1.  **Hintergrund-Diensteinschränkungen** &ndash; Wenn eine app im Hintergrund ausgeführt wird, verfügt es über ein Fenster von ein paar Minuten, in denen es dennoch zugelassen wird erstellen und Verwenden von Webdiensten. Am Ende des Fensters, Android, wird die app Hintergrund-Dienst beendet, und behandelt den Vorgang als _im Leerlauf_.

2.  **Übertragen Sie Einschränkungen** &ndash; Android 7.0 (API 25) Broadcasts, der eine app registriert wird, zum Empfangen von Einschränkungen auferlegt. Android Oreo wird diese Einschränkungen strengere. Beispielsweise können apps für Android Oreo übertragungsempfänger für implizite Broadcasts nicht mehr in ihren Manifesten registrieren.

Weitere Informationen zu den neuen Hintergrund Ausführung Grenzen, finden Sie in der Android-Entwickler [Hintergrund Ausführung Grenzwerte](https://developer.android.com/about/versions/oreo/background.html) Thema.


### <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

Apps, die Android Oreo als Ziel oder höher müssen ihre apps für die folgenden Änderungen vor, bei Bedarf geändert werden:

- Android Oreo ist die Möglichkeit, die Priorität der Benachrichtigung festzulegen. Stattdessen legen Sie eine empfohlene Wichtigkeitsstufe, wenn Sie einen Benachrichtigungskanal zu erstellen. Die Wichtigkeitsstufe, die Sie einen Benachrichtigungskanal zuweisen, gilt für alle benachrichtigungsmeldungen, die Sie bereitstellen, damit.

- Für apps für Android Oreo `PendingIntent.GetService()` funktioniert nicht aufgrund der neuen Grenzwerte für Dienste, die im Hintergrund gestartet platziert. Wenn Sie Android Oreo Anzielen, sollten Sie verwenden [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) stattdessen.  


## <a name="sample-code"></a>Beispielcode

Mehrere Xamarin.Android-Beispiele sind verfügbar, Sie veranschaulichen, wie Sie Android Oreo-Features nutzen:

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) veranschaulicht, wie das neue Benachrichtigungskanäle-System in Android Oreo eingeführt. In diesem Beispiel verwaltet zwei benachrichtigungskanäle: eine mit Standard-Wichtigkeit und der andere mit hoher Priorität.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) veranschaulicht die grundlegende Verwendung des Bild-im-Bild (PiP) Modus für Handheldgeräte Oreo eingeführt. Das Beispiel spielt ein Video, das weiterhin ohne Unterbrechung beim zwischen Anzeigemodi oder andere Aktivitäten hin und her wechseln.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) veranschaulicht die Verwendung der AutoAusfüllen-Framework. Es enthält Implementierungen von Client-Aktivitäten mit Ansichten, die Autofilled und ein Dienst, der für den Client Aktivitäten AutoAusfüllen-Daten bereitstellen kann sollten.

-   [Herunterladbare Schriftarten](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) enthält ein Beispiel zum Verwenden der herunterladbaren Schriftarten-Funktion, die weiter oben beschrieben.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) veranschaulicht die Verwendung des EmojiCompat Support-Bibliothek. Sie können diese Bibliothek verwenden, um zu verhindern, dass Ihre app mit fehlenden Emoji Zeichen als "Tofu" Zeichen.

-   [Speicherort-Updates Pendingintent](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) veranschaulicht die Verwendung der Standort-API zum Abrufen von Updates zum Suchen eines Geräts mit einer `PendingIntent`.

-   [Ortsdienst für Updates Vordergrund](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) veranschaulicht, wie der Standort-API zum Abrufen von Updates zum Suchen eines Geräts mit einem Dienst gebunden ist, und Schritte Vordergrund.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Android 8.0 Oreo-Entwicklung in c#**


## <a name="summary"></a>Zusammenfassung

In diesem Artikel Android Oreo eingeführt und wurde erläutert, wie zum Installieren und konfigurieren die neuesten Tools und Pakete für die Entwicklung von Xamarin.Android unter Android Oreo. Sie haben einen Überblick über die wichtigsten Features in Android Oreo mit Links zu Beispielcode über die Quelle für mehrere neue Features bereitgestellt. Sie enthalten Links zur Dokumentation der API und erste Schritte mit Android-Entwickler-Themen, die Sie unterstützen das Erstellen von apps für Android Oreo. Sie markiert auch die wichtigsten Android Oreo verhaltensänderungen, die vorhandene apps auswirken können.


## <a name="related-links"></a>Verwandte Links

- [Android 8.0 Oreo](https://developer.android.com/index.html)
