---
title: Oreo-Features
description: Beginnen Sie mit der Verwendung von xamarin. Android, um Apps für die neueste Version von Android zu entwickeln.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: conceptdev
ms.author: crdun
ms.date: 07/06/2018
ms.openlocfilehash: 29f7725e41e5163b8f990c827983fbd79bdd1b1e
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510483"
---
# <a name="oreo-features"></a>Oreo-Features

_Beginnen Sie mit der Verwendung von xamarin. Android, um Apps für die neueste Version von Android zu entwickeln._

[Android 8,0 Oreo](https://developer.android.com/index.html) ist die neueste Version von Android, die von Google verfügbar ist. Android Oreo bietet viele neue Features, die für xamarin. Android-Entwickler von Interesse sind. Zu diesen Features gehören Benachrichtigungs Kanäle, Benachrichtigungs Kennzeichen, benutzerdefinierte Schriftarten in XML, herunterladbare Schriftarten, automatische Füllung und Bild-in-Bild (PIP). Android Oreo enthält neue APIs für diese neuen Funktionen. diese APIs sind für xamarin. Android-Apps verfügbar, wenn Sie xamarin. Android 8,0 und höher verwenden.

[![Android Oreo Hero-Image](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

Dieser Artikel ist für den Einstieg in die Entwicklung von xamarin. Android-Apps für Android 8,0 Oreo strukturiert. Darin wird erläutert, wie die erforderlichen Updates installiert, das SDK konfiguriert und ein Emulator (oder ein Gerät) zum Testen erstellt wird. Außerdem finden Sie hier einen Überblick über die neuen Features in Android 8,0 Oreo mit Links zu Beispiel-apps, die die Verwendung von Android Oreo-Features in xamarin. Android-Apps veranschaulichen.


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um Android Oreo-Features in xamarin-basierten apps zu verwenden:

-   **Visual Studio** &ndash; Wenn Sie Windows verwenden, ist Version 15,5 oder höher von Visual Studio erforderlich.  Wenn Sie einen Mac verwenden, ist Visual Studio für Mac Version 7.2.0 erforderlich.

-   **Xamarin. Android** &ndash; xamarin. Android 8,0 oder höher muss mit Visual Studio installiert und konfiguriert werden.

-   **Android SDK** &ndash; Android SDK 8,0 (API 26) oder höher muss über den Android SDK-Manager installiert werden.



## <a name="getting-started"></a>Erste Schritte

Für den Einstieg in die Verwendung von Android Oreo mit xamarin. Android müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie ein Android Oreo-Projekt erstellen können:

1. Aktualisieren Sie auf die neueste Version von Visual Studio.

2. Installieren Sie die Pakete und Tools von **Android 8.0.0 (API 26)** oder höher über den SDK-Manager.

3. Erstellen Sie ein neues xamarin. Android-Projekt, das Android Oreo (API 26) als Ziel hat.

4. Konfigurieren Sie einen Emulator oder ein Gerät zum Testen von Android Oreo-apps.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualisieren von Visual Studio und xamarin. Android

Gehen Sie folgendermaßen vor, um die Unterstützung von Android Oreo zu Visual Studio hinzuzufügen:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Verwenden Sie für Visual Studio 2019 den [SDK-Manager](~/android/get-started/installation/android-sdk.md) , um API-Ebene 26,0 oder höher zu installieren.

- Wenn Sie Visual Studio 2017 verwenden:

    1. Aktualisieren Sie auf Visual Studio 2017 Version 15,7 oder höher (siehe [Aktualisieren von Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

    2. Verwenden Sie den [SDK-Manager](~/android/get-started/installation/android-sdk.md) , um API-Ebene 26,0 oder höher zu installieren.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

- Aktualisieren Sie auf die neueste stabile Version von Visual Studio für Mac wie unter [Aktualisieren von Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/update)erläutert.

-----

Weitere Informationen zur xamarin-Unterstützung für Android Oreo finden Sie in den Anmerkungen zu dieser [Version von xamarin. Android 8,0](https://docs.microsoft.com/xamarin/android/release-notes/8/8.0/).



### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Um ein Projekt mit xamarin. Android 8,0 zu erstellen, müssen Sie zunächst den xamarin Android SDK Manager verwenden, um die SDK-Plattform für **Android 8,0-Oreo** oder höher zu installieren. Sie müssen auch Android SDK Tools 26,0 oder höher installieren.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Starten Sie den SDK-Manager (Klicken Sie in Visual Studio auf Extras **> Android-> Android SDK-Manager**).

2. Installieren Sie die **Android 8,0-Oreo-** Pakete. Wenn Sie den Android SDK Emulator verwenden, stellen Sie sicher, dass Sie die benötigten **x86** -System Images einschließen:

    [![Auswählen von Android 8,0-Paketen im Android SDK-Manager](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher und **Android SDK Build-Tools 26.0.0** (oder höher):

    [![Auswählen von Android SDK Tools 26 im Android SDK Manager](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie den SDK-Manager (Klicken Sie in Visual Studio für Mac auf Extras **> SDK-Manager**).

2. Installieren Sie die **Android 8,0-Oreo SDK-** Pakete. Wenn Sie den Android SDK Emulator verwenden, stellen Sie sicher, dass Sie die benötigten **x86** -System Images einschließen:

    [![Auswählen von Android 8,0-Paketen im SDK-Manager](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher und **Android SDK Build-Tools 26.0.0** (oder höher):

    [![Auswählen von Android SDK Tools 26 im SDK-Manager](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Starten eines xamarin. Android-Projekts

Erstellen Sie ein neues xamarin. Android-Projekt. Wenn Sie mit der Android-Entwicklung mit xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Weitere Informationen zum Erstellen von xamarin. Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versions Einstellungen so konfigurieren, dass Sie Android 8,0 oder höher als Ziel haben. Wenn Sie z. b. Ihr Projekt für Android 8,0 als Ziel festlegen möchten, müssen Sie die Android-API-Ziel Ebene Ihres Projekts auf **Android 8,0 (API 26)** konfigurieren. Es wird empfohlen, dass Sie auch die zielframeworkebene auf API 26 oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie Untergrund Legendes zu [Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Konfigurieren eines Emulators oder Geräts

Wenn Sie versuchen, den standardmäßigen Google GUI-basierten AVD-Manager nach der Installation von Android SDK Tools 26,0 oder höher zu starten, erhalten Sie möglicherweise das folgende Fehler Dialogfeld, in dem Sie stattdessen das Befehlszeilen-AVD-Manager-Tool **avdmanager** verwenden können:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Dialog Android-Emulator-Manager-Warnung](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Dialog Android-Emulator-Manager-Warnung](oreo-images/mac/03-avd-warning.png)

-----

Diese Meldung wird angezeigt, da Google keinen eigenständigen GUI-AVD-Manager mehr bereitstellt, der API 26,0 und höher unterstützt. Für Android 8,0 Oreo müssen Sie entweder den xamarin Android-Emulator Manager oder das Befehlszeilen `avdmanager` Tool verwenden, um virtuelle Geräte für Android Oreo zu erstellen.

Informationen zum Verwenden des Android Device Manager zum Erstellen und Verwalten von virtuellen Geräten finden Sie unter [Verwalten von virtuellen Geräten mit dem Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Um virtuelle Geräte ohne die Android Device Manager zu erstellen, führen Sie die Schritte im nächsten Abschnitt aus.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Erstellen von virtuellen Geräten mithilfe von avdmanager

Gehen Sie folgendermaßen vor, um ein neues virtuelles Gerät mithilfe von **avdmanager** zu erstellen:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Öffnen Sie ein Eingabe Aufforderungs Fenster `JAVA_HOME` , und legen Sie auf den Speicherort des Java SDK auf Ihrem Computer fest. Für eine typische xamarin-Installation können Sie den folgenden Befehl verwenden:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Fügen Sie der den Speicherort `bin` des Ordners `PATH`Android SDK hinzu.
    Für eine typische xamarin-Installation können Sie den folgenden Befehl verwenden:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Schließen Sie das Eingabe Aufforderungs Fenster, und öffnen Sie ein neues Eingabe Aufforderungs Fenster. Erstellen Sie mit dem [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) -Befehl ein neues virtuelles Gerät. Um z. b. eine AVD mit dem Namen **AVD-Oreo-8,0** zu erstellen, indem Sie das x86-System Image für API-Ebene 26 verwenden, verwenden Sie den folgenden Befehl:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Wenn Sie dazu aufgefordert werden **, ein benutzerdefiniertes Hardwareprofil [No] zu erstellen,** können Sie **Nein** eingeben und das standardmäßige Hardwareprofil übernehmen. Wenn Sie " **Ja**" sagen, werden Sie von **avdmanager** aufgefordert, eine Liste mit Fragen zum Anpassen des Hardware Profils zu erhalten.

Nachdem Sie das virtuelle Gerät von **avdmanager** erstellt haben, wird es in das Pulldownmenü des Geräts eingefügt:

[![Neues AVD zum Gerät hinzugefügt (Pulldownmenü)](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Öffnen Sie ein **Terminal** Fenster, und wechseln Sie zum Speicherort des Verzeichnisses Android SDK Tools auf Ihrem Mac. Für eine typische xamarin-Installation können Sie den folgenden Befehl verwenden:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Erstellen Sie mit dem [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) -Befehl ein neues virtuelles Gerät. Um z. b. eine AVD mit dem Namen **AVD-Oreo-8,0** zu erstellen, indem Sie das x86-System Image für API-Ebene 26 verwenden, verwenden Sie den folgenden Befehl:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Wenn Sie dazu aufgefordert werden **, ein benutzerdefiniertes Hardwareprofil [No] zu erstellen,** können Sie **Nein** eingeben und das standardmäßige Hardwareprofil übernehmen. Wenn Sie " **Ja**" sagen, werden Sie von **avdmanager** aufgefordert, eine Liste mit Fragen zum Anpassen des Hardware Profils zu erhalten.

Nachdem Sie das virtuelle Gerät mithilfe von **avdmanager** erstellt haben, wird es in das Pulldownmenü des Geräts eingefügt:

[![Neues AVD zum Gerät hinzugefügt (Pulldownmenü)](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Weitere Informationen zum Konfigurieren eines Android-Emulators für das Testen und Debuggen finden Sie unter [Debugging auf dem Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

Wenn Sie ein physisches Gerät (z. b. einen Nexus oder ein Pixel) verwenden, können Sie Ihr Gerät entweder über die "Automatische über-Air"-Updates (OTA) aktualisieren oder ein System Abbild herunterladen und Ihr Gerät direkt blinken. Weitere Informationen zum manuellen Aktualisieren Ihres Geräts auf Android Oreo finden Sie unter [Factory-Images für Nexus-und Pixel-Geräte](https://developers.google.com/android/images).



## <a name="new-features"></a>Neue Funktionen

Android Oreo bietet eine Reihe von neuen Features und Funktionen, wie z. b. Benachrichtigungs Kanäle, Benachrichtigungs Kennzeichen, benutzerdefinierte Schriftarten in XML, herunterladbare Schriftarten, AutoFill und Bild-in-Bild. In den folgenden Abschnitten werden diese Features hervorgehoben, und es werden Links bereitgestellt, die Ihnen bei der Verwendung in Ihrer APP helfen.



### <a name="notification-channels"></a>Benachrichtigungs Kanäle

*Benachrichtigungs Kanäle* sind App-definierte Kategorien für Benachrichtigungen.
Sie können einen Benachrichtigungs Kanal für jeden Benachrichtigungstyp erstellen, den Sie senden müssen, und Sie können Benachrichtigungs Kanäle erstellen, um die von Benutzern Ihrer APP getroffenen Entscheidungen widerzuspiegeln. Mithilfe der neuen Benachrichtigungs Kanäle können Sie Benutzern eine präzisere Kontrolle über verschiedene Arten von Benachrichtigungen ermöglichen. Wenn Sie z. b. eine Messaging-App implementieren, können Sie separate Benachrichtigungs Kanäle für jede Konversations Gruppe erstellen, die von einem Benutzer erstellt wird.

[Benachrichtigungs Kanäle](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) erläutern, wie ein Benachrichtigungs Kanal erstellt und zum Veröffentlichen lokaler Benachrichtigungen verwendet wird. Ein Beispiel für einen realen Code finden Sie im [notificationchannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) -Beispiel. Diese Beispiel-App verwaltet zwei Kanäle und legt zusätzliche Benachrichtigungs Optionen fest.



### <a name="notification-badges"></a>Benachrichtigungs Abzeichen

Benachrichtigungs Ausweise sind kleine Punkte, die über APP-Symbole angezeigt werden, wie in diesem Screenshot gezeigt:

[![Beispiele für Benachrichtigungs Abzeichen für App-Symbole](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Diese Punkte geben an, dass neue Benachrichtigungen für mindestens einen Benachrichtigungs Kanal in der app vorhanden sind, die mit &ndash; dem App-Symbol verknüpft ist. Dies sind Benachrichtigungen, die der Benutzer noch nicht verworfen oder bearbeitet hat. Benutzer können mit einer langen Taste auf ein Symbol klicken, um auf die mit einem Benachrichtigungs Badge verknüpften Benachrichtigungen zu klicken und Benachrichtigungen aus dem Menü mit langen drücken zu verwerfen, das sich auf die Anwendung auswirkt.

Weitere Informationen zu Benachrichtigungs Abzeichen finden Sie im Thema Android-Entwickler [Benachrichtigungs Abzeichen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) .



### <a name="custom-fonts-in-xml"></a>Benutzerdefinierte Schriftarten in XML

Android Oreo führt *Schriftarten in XML*ein. Dadurch können Sie benutzerdefinierte Schriftarten als Ressourcen integrieren. OpenType-( **. OTF**) und TrueType-Schriftformate ( **. ttf**) werden unterstützt. Gehen Sie folgendermaßen vor, um Schriftarten als Ressourcen hinzuzufügen:

1. Erstellen Sie einen **Ressourcen-** /ordnerordner.

2. Kopieren Sie die Schriftart Dateien (z **. b. ttf** -und **OTF** -Dateien) in **Ressourcen/Schriftart**. 

3. Benennen Sie ggf. alle Schriftart Dateien so um, dass Sie den Benennungs Konventionen für Android-Dateien entsprechen (d. h., verwenden Sie nur Kleinbuchstaben *a-z*, *0-9*und Unterstriche in Dateinamen). Die Schriftart Datei `Pacifico-Regular.ttf` könnte beispielsweise in etwa wie `pacifico.ttf`folgt umbenannt werden.

4. Wenden Sie die benutzerdefinierte Schriftart mithilfe des `android:fontFamily` neuen Attributs in ihrer Layout-XML-Datei an. Beispielsweise wird in der `TextView` folgenden Deklaration die hinzugefügte " **Pacifico. ttf** "-Schrift Ressource verwendet:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Sie können auch eine XML-Datei der Schriftfamilie erstellen, die mehrere Schriftarten sowie Stil-und Gewichtungs Details beschreibt. Weitere Informationen finden Sie im Thema Android Developer [Fonts in XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) .


### <a name="downloadable-fonts"></a>Download Schriftarten

Ab Android Oreo können apps Schriftarten von einem Anbieter anfordern, anstatt Sie in das APK zu bündeln. Schriftarten werden nur bei Bedarf aus dem Netzwerk heruntergeladen. Diese Funktion reduziert die APK-Größe, spart den Telefon Arbeitsspeicher und die Nutzung von Mobil Funkdaten. Sie können diese Funktion auch unter Android-API-Versionen 14 und höher verwenden, indem Sie das Paket für die Android-Unterstützungs Bibliothek 26 installieren.

Wenn Ihre APP eine Schriftart benötigt, erstellen Sie ein `FontsRequest` -Objekt (indem Sie die herunter zuladbare Schriftart angeben), `FontsContract` und übergeben Sie Sie dann an eine Methode zum Herunterladen der Schriftart. In den folgenden Schritten wird der Schriftart Downloadvorgang ausführlicher beschrieben:

1.  Instanziieren Sie ein [fontrequest](https://developer.android.com/reference/android/provider/FontRequest.html) -Objekt. 

2.  Unterklasse und instanziieren Sie [fongscontract. fontrequestcallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implementieren Sie die Methode " [fontrequestcallback. ontypefaceretrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) ", die verwendet wird, um den Abschluss der Schriftart Anforderung zu behandeln.

4.  Implementieren Sie die Methode " [fontrequestcallback. ontypefacerequestfailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) ", die verwendet wird, um Ihre APP über Fehler zu informieren, die während des Schriftart Anforderungs Prozesses stattfinden.

5.  Rufen Sie die Methode " [fontcontract. requestfonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) " auf, um die Schriftart aus dem Schriftart Anbieter abzurufen. 

Wenn Sie die `RequestFonts` -Methode aufzurufen, wird zuerst überprüft, ob die Schriftart lokal zwischengespeichert ist (von einem vorherigen `RequestFont`-Aufrufvorgang). Wenn Sie nicht zwischengespeichert wird, wird der Schriftart Anbieter aufgerufen. die Schriftart wird asynchron abgerufen, und die Ergebnisse werden an Ihre APP zurückgeleitet, indem `OnTypeFaceRetrieved` die-Methode aufgerufen wird.

Das Beispiel zum Herunterladen von [Schriftarten](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) veranschaulicht, wie Sie das in Android Oreo eingeführte herunterladbare Schriftarten Feature verwenden 

Weitere Informationen zum Herunterladen von Schriftarten finden Sie im Thema Android Developer [Download Fonts](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) .



### <a name="autofill"></a>Auto Ausfüllen

Das neue _AutoFill_ -Framework in Android Oreo vereinfacht es Benutzern, sich wiederholende Aufgaben wie Anmelde Name, Kontoerstellung und Kreditkartentransaktionen zu verarbeiten. Benutzer verbringen weniger Zeit mit der erneuten Typisierung von Informationen (was zu Eingabefehlern führen kann). Bevor Ihre APP mit dem Auto Ausfüllen-Framework arbeiten kann, muss in den Systemeinstellungen ein Auto Ausfüllen-Dienst aktiviert werden (Benutzer können die automatische Füllung aktivieren oder deaktivieren).

Das [autofillframework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) -Beispiel veranschaulicht die Verwendung des AutoFill-Frameworks. Es umfasst Implementierungen von Client Aktivitäten mit Sichten, die automatisch ausgefüllt werden sollten, und einen Dienst, der Daten für das automatische Auffüllen an Client Aktivitäten bereitstellen kann.

Weitere Informationen über das neue Feature "AutoFill" und die Optimierung Ihrer APP für Autofill finden Sie im Thema Android Developer [AutoFill Framework](https://developer.android.com/guide/topics/text/autofill.html) .



### <a name="picture-in-picture-pip"></a>Bild im Bild (PIP)

Android Oreo ermöglicht das Starten einer Aktivität im PIP-Modus (Bild-in-Bild), wodurch der Bildschirm einer anderen Aktivität überlagert wird. Diese Funktion ist für die Videowiedergabe vorgesehen.

Um anzugeben, dass die Aktivität der APP den PIP-Modus verwenden kann, legen Sie im Android-Manifest das folgende Flag auf true fest:

```xml
android:supportsPictureInPicture
```

Wenn Sie angeben möchten, wie sich die Aktivität im PIP-Modus Verhalten soll, verwenden Sie das neue-Objekt " [pictureinpictureparams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) ". `PictureInPictureParams`stellt einen Satz von Parametern dar, die Sie verwenden, um eine Aktivität im PIP-Modus zu initialisieren und zu aktualisieren (z. b. das bevorzugte Seitenverhältnis der Aktivität). `Activity` In Android Oreo wurden die folgenden neuen PIP-Methoden hinzugefügt:

-   [Enterpictureinpicturemode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; versetzt die Aktivität in den PIP-Modus. Die Aktivität wird in der Ecke des Bildschirms platziert, und der restliche Bildschirm wird mit der vorherigen Aktivität aufgefüllt, die sich auf dem Bildschirm befand.

-   [Setpictureinpictuneu](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; Aktualisiert die PIP-Konfigurationseinstellungen der Aktivität (z. b. eine Änderung des Seitenverhältnisses).

Das Beispiel " [pictureinpicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) " veranschaulicht die grundlegende Verwendung des PIP-Modus (Bild-in-Bild) für in Oreo eingeführte Hand Held Geräte. Im Beispiel wird ein Video abgespielt, das beim Wechseln zwischen Anzeigemodi oder anderen Aktivitäten ununterbrochen fortgesetzt wird.



### <a name="other-features"></a>Weitere Features

Android Oreo enthält viele weitere neue Features, wie z. b. die Emoji-Unterstützungs Bibliothek, die Location-API, Hintergrund Limits, die Breite der Farbpalette für apps, neue Audiocodecs, WebView-Erweiterungen, verbesserte Unterstützung bei der Tastaturnavigation und eine neue aaudioapi (pro-Audiodatei) für leistungsstarke Audiodaten mit niedriger Latenz. Weitere Informationen zu diesen Features finden Sie im Thema Android-Entwickler [Android Oreo Features und APIs](https://developer.android.com/about/versions/oreo/android-8.0.html) .



## <a name="behavior-changes"></a>Verhaltensänderungen

Android Oreo umfasst eine Reihe von Änderungen am System-und API-Verhalten, die sich auf die Funktionalität vorhandener apps auswirken können. Diese Änderungen werden wie folgt beschrieben.


### <a name="background-execution-limits"></a>Einschränkungen bei der Hintergrund Ausführung

Um die Benutzererfahrung zu verbessern, erzwingt Android Oreo Einschränkungen, welche apps bei der Ausführung im Hintergrund ausgeführt werden können. Wenn der Benutzer z. b. ein Video ansehen oder ein Spiel spielt, könnte eine im Hintergrund ausgeführten APP die Leistung einer Video intensiven App beeinträchtigen, die im Vordergrund ausgeführt wird. Folglich werden von Android Oreo die folgenden Einschränkungen für apps, die nicht direkt mit dem Benutzer interagieren, platziert:

1.  **Einschränkungen für den Hintergrunddienst** &ndash; Wenn eine APP im Hintergrund ausgeführt wird, verfügt sie über ein Fenster von einigen Minuten, in dem Sie weiterhin Dienste erstellen und verwenden können. Am Ende dieses Fensters hält Android den Hintergrunddienst der APP an und behandelt ihn als im _Leerlauf_befindlich.

2.  **Übertragungs Einschränkungen** &ndash; Android 7,0 (API 25) hat Einschränkungen für Übertragungen festgelegt, die von einer APP für den Empfang registriert werden. Android Oreo macht diese Einschränkungen strenger. Android Oreo-Apps können z. b. Broadcast Empfänger nicht mehr für implizite Sendungen in ihren Manifesten registrieren.

Weitere Informationen zu den neuen Grenzwerten für die Hintergrund Ausführung finden Sie im Thema Android Developer [Background Execution Limits](https://developer.android.com/about/versions/oreo/background.html) .


### <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

Apps, die auf Android Oreo oder höher ausgerichtet sind, müssen Ihre apps ändern, damit die folgenden Änderungen ggf. unterstützt werden:

- Android Oreo ist nicht mehr in der Lage, die Priorität einzelner Benachrichtigungen festzulegen. Stattdessen legen Sie eine empfohlene Wichtigkeits Stufe fest, wenn Sie einen Benachrichtigungs Kanal erstellen. Die Wichtigkeits Stufe, die Sie einem Benachrichtigungs Kanal zuweisen, gilt für alle Benachrichtigungs Meldungen, die Sie an Sie senden.

- Für apps, die auf Android Oreo abzielen, funktioniert nicht aufgrund neuer Grenzwerte für Dienste, die im Hintergrund gestartet wurden `PendingIntent.GetService()` . Wenn Sie Android Oreo als Ziel verwenden, sollten Sie stattdessen " [pdingintent. getbroadcast](xref:Android.App.PendingIntent.GetBroadcast*) " verwenden.  


## <a name="sample-code"></a>Beispielcode

Es stehen mehrere xamarin. Android-Beispiele zur Verfügung, die Ihnen zeigen, wie Sie die Funktionen von Android Oreo nutzen können:

-   [Notificationschannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) zeigt die Verwendung des neuen Benachrichtigungs Kanalsystems, das in Android Oreo eingeführt wurde. In diesem Beispiel werden zwei Benachrichtigungs Kanäle verwaltet: eine mit der Standard Wichtigkeit und die andere mit hoher Wichtigkeit.

-   " [Pictureinpicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) " veranschaulicht die grundlegende Verwendung des PIP-Modus (Bild-in-Bild) für in Oreo eingeführte Handheld-Geräte. Im Beispiel wird ein Video abgespielt, das beim Wechseln zwischen Anzeigemodi oder anderen Aktivitäten ununterbrochen fortgesetzt wird.

-   [Autofillframework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) veranschaulicht die Verwendung des AutoFill-Frameworks. Es umfasst Implementierungen von Client Aktivitäten mit Sichten, die automatisch ausgefüllt werden sollten, und einen Dienst, der Daten für das automatische Auffüllen an Client Aktivitäten bereitstellen kann.

-   Zum Herunterladen von [Schriftarten](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) finden Sie ein Beispiel für die Verwendung des zuvor beschriebenen Features zum Herunterladen von Schriftarten

-   [Emojicompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) veranschaulicht die Verwendung der emojicompat-Unterstützungs Bibliothek. Sie können diese Bibliothek verwenden, um zu verhindern, dass Ihre APP fehlende Emoji-Zeichen als "Tofu"-Zeichen anzeigt.

-   [Location Updates Pending Intent](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) veranschaulicht die Verwendung der Location-API, um Updates zum Speicherort eines Geräts mithilfe `PendingIntent`von zu erhalten.

-   [Location Updates Vordergrund Dienst](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) veranschaulicht, wie die Location-API verwendet wird, um Updates zum Speicherort eines Geräts mithilfe eines gebundenen und gestarteten Vordergrund Dienstanbieter zu erhalten.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Android 8,0 Oreo-Entwicklung mitC#**


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android Oreo eingeführt und erläutert, wie die neuesten Tools und Pakete für die xamarin. Android-Entwicklung auf Android Oreo installiert und konfiguriert werden. Es wurde eine Übersicht über die wichtigsten Features von Android Oreo bereitgestellt, mit Links zu Beispiel Quellcode für verschiedene neue Features. Es enthält Links zu API-Dokumentation und Android-Entwickler Themen, die Ihnen den Einstieg in das Erstellen von Apps für Android Oreo erleichtern. Außerdem wurden die wichtigsten Änderungen am Android Oreo-Verhalten hervorgehoben, die sich auf vorhandene apps auswirken können.


## <a name="related-links"></a>Verwandte Links

- [Android 8.0 Oreo](https://developer.android.com/index.html)
