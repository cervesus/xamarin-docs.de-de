---
title: Oreo-Funktionen
description: Wie die ersten Schritte mit Xamarin.Android für die neueste Version von Android-apps entwickeln.
ms.prod: xamarin
ms.assetid: EAEF7341-7A00-4439-9FAF-43882637BEF8
ms.technology: xamarin-android
ms.custom: video
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: a23072427a74119bfa339fea8a695cd13b775685
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935102"
---
# <a name="oreo-features"></a>Oreo-Funktionen

_Wie die ersten Schritte mit Xamarin.Android für die neueste Version von Android-apps entwickeln._

[Android 8.0 Oreo](https://developer.android.com/index.html) ist die neueste Version von Android aus Google verfügbar. Android Oreo bietet viele neue Funktionen von Interesse für Xamarin.Android-Entwickler. Zu diesen Funktionen gehören unter einem Benachrichtigungskanal versteht, Benachrichtigung Signale, benutzerdefinierte Schriftarten in XML, herunterladbare Schriftarten AutoAusfüllen und Bild im Bild (PIP). Android Oreo enthält neue APIs für diese neuen Funktionen und diese APIs sind für Xamarin.Android-apps, die bei der Verwendung von Xamarin.Android 8.0 verfügbar und höher.

[![Android Oreo Hero-Bild](oreo-images/01-android-o-logo-sml.png)](oreo-images/01-android-o-logo.png#lightbox)

In diesem Artikel wird strukturiert, um Ihnen beim Einstieg in die Entwicklung von Xamarin.Android apps für Android 8.0 Oreo helfen. Es wird erläutert, wie die erforderlichen Updates zu installieren, konfigurieren das SDK und erstellen einen Emulator (oder Geräte) zum Testen. Darüber hinaus einen Überblick über die neuen Funktionen in Android 8.0 Oreo mit Links zu beispielapps, die veranschaulichen, wie Android Oreo Funktionen in Xamarin.Android apps verwenden.


## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um Android Oreo Funktionen in Xamarin-basierten apps zu verwenden:

-   **Visual Studio** &ndash; 15.5 oder höher von Visual Studio bei Verwendung von Windows erforderlich ist.  Wenn Sie mit einen Mac arbeiten, ist Visual Studio für Mac Version 7.2.0 erforderlich.

-   **Xamarin.Android** &ndash; Xamarin.Android 8.0 oder höher muss installiert und mit Visual Studio konfiguriert werden.

-   **Android SDK** &ndash; Android SDK 8.0 (26-API) oder höher muss installiert sein über den Android SDK Manager.



## <a name="getting-started"></a>Erste Schritte

Zum Einstieg Android Oreo mit Xamarin.Android verwenden, müssen Sie herunterladen und installieren die neuesten Tools und SDK-Paketen, vor dem Erstellen eines Projekts für Android Oreo:

1. Aktualisieren Sie auf die neueste Version von Visual Studio.

2. Installieren der **Android 8.0.0 (26-API)** oder spätere Pakete und Tools über den SDK-Manager.

3. Erstellen Sie ein neues Xamarin.Android-Projekt, dessen Ziel Android Oreo (26-API).

4. Konfigurieren Sie einen Emulator oder Gerät zu Testzwecken Oreo Android-apps.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:



### <a name="update-visual-studio-and-xamarinandroid"></a>Aktualisieren von Visual Studio und Xamarin.Android

Um Visual Studio Android Oreo Unterstützung hinzuzufügen, führen Sie folgende Schritte aus:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

-   Wenn Sie Visual Studio 2017 verwenden: 

    1. Update für Visual Studio 2017 15.5 oder höher (siehe [Update Visual Studio 2017](https://docs.microsoft.com/en-us/visualstudio/install/update-visual-studio)).

    2. Verwenden Sie den SDK-Manager [installationsanweisungen](~/android/get-started/installation/android-sdk.md#installation) Xamarin-SDK-Manager installieren.
       Die Xamarin-SDK-Manager muss installiert sein, da Google keine Standlone GUI-SDK-Manager bietet, die API 26,0 und höher unterstützt.

-   Wenn Sie Visual Studio 2015, wir verwenden, empfohlen werden Downgrades von SDK-Tools auf 25 und mithilfe, der ALTER-Emulators für Google-Manager GUI. SDK-Tools 25 können weiterhin zusammen mit der API-26, 27 und neuere, verwendet werden und wird nicht für neue Plattformen Entwicklung beeinträchtigen. Eine Schnittstelle, erhalten Sie für die Verwaltung Ihres Android-SDK für ältere Versionen von Visual Studio.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

-   Aktualisieren auf die neueste stabile Version des Visual Studio-2017 für Mac, wie in beschrieben [Aktualisieren von Visual Studio für Mac](https://docs.microsoft.com/en-us/visualstudio/mac/update).

-----

Weitere Informationen zu Xamarin-Unterstützung für Android Oreo, finden Sie unter der [Xamarin.Android 8.0 Versionshinweise](https://developer.xamarin.com/releases/android/xamarin.android_8/xamarin.android_8.0/).



### <a name="install-the-android-sdk"></a>Android SDK installieren

Um ein Projekt mit Xamarin.Android 8.0 erstellen zu können, müssen Sie zuerst den Xamarin Android SDK-Manager verwenden, so installieren Sie die SDK-Plattform für **Android 8.0 - Oreo** oder höher. Sie müssen auch Android SDK-Tools 26,0 oder höher installieren.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Starten Sie den SDK-Manager (klicken Sie in Visual Studio auf **Tools > Android > Android SDK Manager**).

2. Installieren der **Android 8.0 - Oreo** Pakete. Wenn Sie den Android SDK-Emulator verwenden, werden Sie sicherstellen, dass die **X86** von Betriebssystemabbildern, die Sie benötigen:

    [![Auswählen von Android 8.0 Pakete im Android SDK Manager](oreo-images/win/01-android-o-packages.png)](oreo-images/win/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK-Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher, und **Android SDK-Buildtools 26.0.0** (oder höher):

    [![Android SDK-Tools 26 auswählen in den Android SDK-Manager](oreo-images/win/02-sdk-tools.png)](oreo-images/win/02-sdk-tools.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1. Starten Sie den SDK-Manager (klicken Sie in Visual Studio für Mac auf **Tools > SDK-Manager**).

2. Installieren der **Android 8.0 - Oreo** SDK-Paketen. Wenn Sie den Android SDK-Emulator verwenden, werden Sie sicherstellen, dass die **X86** von Betriebssystemabbildern, die Sie benötigen:

    [![Auswählen von Android 8.0-Paketen in den SDK-Manager](oreo-images/mac/01-android-o-packages.png)](oreo-images/mac/01-android-o-packages.png#lightbox)

3. Installieren Sie **Android SDK-Tools 26.0.2** oder höher, **Android SDK Platform-Tools 26.0.0** oder höher, und **Android SDK-Buildtools 26.0.0** (oder höher):

    [![Android SDK-Tools 26 auswählen in den SDK-Manager](oreo-images/mac/02-sdk-tools.png)](oreo-images/mac/02-sdk-tools.png#lightbox)

-----



### <a name="start-a-xamarinandroid-project"></a>Starten eines Projekts Xamarin.Android

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Projekten Xamarin.Android.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die versionseinstellungen Ziel Android 8.0 oder höher konfigurieren. Z. B. um das Projekt für Android 8.0 abzielen möchten, müssen, konfigurieren Sie die Zielebene Android-API des Projekts in **Android 8.0 (26-API)**. Es wird empfohlen, dass Sie auch die Ziel-Frameworkebene und 26-API oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebene Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-an-emulator-or-device"></a>Konfigurieren Sie einen Emulator oder Gerät

Wenn Sie versuchen, die Standardeinstellung Google-GUI-basierte AVD-Manager starten nach der Installation von Android SDK Tools 26.0 oder höher, erhalten Sie möglicherweise folgende Fehlerdialogfeld, den Sie den AVD-Manager-Befehlszeilentool verwenden angewiesen wird, **Avdmanager** stattdessen :

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Android Emulator Manager ein Dialogfeld mit Warnung](oreo-images/win/03-avd-warning.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Android Emulator Manager ein Dialogfeld mit Warnung](oreo-images/mac/03-avd-warning.png)

-----

Diese Meldung wird angezeigt, da Google nicht mehr eine Standlone GUI AVD-Manager enthält, die API 26,0 und höher unterstützt. Für Android 8.0 Oreo müssen Sie verwenden den Xamarin Android-Emulator-Manager oder der Befehlszeile `avdmanager` mit dem Tool für Android Oreo virtuelle Geräte zu erstellen.

Um die Android-Geräte-Manager zu erstellen und Verwalten von virtuellen Geräten durchgeführt werden, finden Sie unter [Verwalten von virtuellen Geräten mit Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Führen Sie die Schritte im nächsten Abschnitt, um virtuelle Geräte ohne Android-Geräte-Manager zu erstellen.


#### <a name="creating-virtual-devices-using-avdmanager"></a>Erstellen von virtuellen Geräte mithilfe von avdmanager

Mit **Avdmanager** um ein neues virtuelles Gerät erstellen, gehen Sie folgendermaßen vor:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1.  Öffnen Sie ein Eingabeaufforderungsfenster, und legen Sie `JAVA_HOME` auf den Speicherort des Java SDK auf Ihrem Computer. Für eine Standardinstallation von Xamarin können Sie den folgenden Befehl aus:

    ```cmd
    setx JAVA_HOME "C:\Program Files\Java\jdk1.8.0_131"
    ```

2.  Fügen Sie den Speicherort von Android SDK `bin` Ordner auf Ihrem `PATH`.
    Für eine Standardinstallation von Xamarin können Sie den folgenden Befehl aus:

    ```cmd
    setx PATH "%PATH%;C:\Program Files (x86)\Android\android-sdk\tools\bin"
    ```

3.  Schließen Sie das Eingabeaufforderungsfenster, und öffnen Sie ein Eingabeaufforderungsfenster. Erstellen Sie ein neues virtuelles Gerät mithilfe der [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) Befehl. Beispielsweise so erstellen Sie ein AVD namens **AVD-Oreo-8.0** der X86 Systemabbild für API-Ebene 26, verwenden Sie den folgenden Befehl:

    ```cmd
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

4.  Wenn Sie aufgefordert werden mit **möchten Sie das Erstellen eines benutzerdefinierten Hardwareprofils [No]** gelangen **keine** und akzeptieren Sie die Standard-Hardwareprofil. Wenn Sie z. B. **Ja**, **Avdmanager** werden Sie eine Liste mit Fragen zum Anpassen des Hardwareprofils aufgefordert.

Nachdem Sie **Avdmanager** um Ihr virtuelles Gerät erstellen, wird es in das Pulldownmenü Gerät enthalten sein:

[![Neue AVD Pulldownmenü Gerät hinzugefügt](oreo-images/win/04-android-o-avd-sml.png)](oreo-images/win/04-android-o-avd.png#lightbox)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

1.  Öffnen einer **Terminaldienste** und wechseln Sie zum Speicherort des Android SDK-Tools-Verzeichnis auf Ihrem Mac. Für eine Standardinstallation von Xamarin können Sie den folgenden Befehl aus:

    ```bash
    cd ~/Library/Developer/Xamarin/android-sdk-macosx/tools/bin
    ```

2.  Erstellen Sie ein neues virtuelles Gerät mithilfe der [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html) Befehl. Beispielsweise so erstellen Sie ein AVD namens **AVD-Oreo-8.0** der X86 Systemabbild für API-Ebene 26, verwenden Sie den folgenden Befehl:

    ```bash
    avdmanager create avd -n AVD-Oreo-8.0 -k "system-images;android-26;google_apis;x86"
    ```

3.  Wenn Sie aufgefordert werden mit **möchten Sie das Erstellen eines benutzerdefinierten Hardwareprofils [No]** gelangen **keine** und akzeptieren Sie die Standard-Hardwareprofil. Wenn Sie z. B. **Ja**, **Avdmanager** fordert Sie eine Liste mit Fragen zum Anpassen der das Hardwareprofil ein.

Nach der Verwendung von **Avdmanager** um Ihr virtuelles Gerät erstellen, wird es in das Pulldownmenü Gerät enthalten sein:

[![Neue AVD Pulldownmenü Gerät hinzugefügt](oreo-images/mac/04-android-o-avd-sml.png)](oreo-images/mac/04-android-o-avd.png#lightbox)

-----

Weitere Informationen zum Konfigurieren von Android-Emulator zum Testen und Debuggen, finden Sie unter [Debuggen auf Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

Wenn Sie z. B. eine Nexus oder ein Pixel ein physisches Gerät verwenden, können entweder Aktualisieren Ihres Geräts über automatisch über die Updates per Funk (OTA) oder Herunterladen ein Abbilds und flash Ihr Gerät direkt. Weitere Informationen zum manuellen Aktualisieren Ihres Geräts auf Android Oreo finden Sie unter [Factory Images für Nexus und Pixel-Geräte](https://developers.google.com/android/images).



## <a name="new-features"></a>Neue Funktionen

Android Oreo führt zu einer Vielzahl von neuen Features und Funktionen, z. B. unter einem Benachrichtigungskanal versteht, Benachrichtigung Signale, benutzerdefinierte Schriftarten in XML, herunterladbare Schriftarten, AutoAusfüllen und im Bild. In den folgenden Abschnitten veranschaulichen die diese Funktionen und Links helfen Ihnen beim Einstieg diese in Ihrer app bieten.



### <a name="notification-channels"></a>Unter einem Benachrichtigungskanal versteht

*Unter einem Benachrichtigungskanal versteht* app definierte Kategorien für Benachrichtigungen.
Erstellen eines benachrichtigungskanals für jeden Typ der Benachrichtigung, die Sie senden müssen, und Sie können unter einem Benachrichtigungskanal versteht entsprechend der Auswahl von Benutzern der Anwendung vorgenommen erstellen. Das neue Kanäle Benachrichtigungsfeature ermöglicht Sie Benutzern eine präzisere Kontrolle über verschiedene Arten von Benachrichtigungen zu gewähren. Wenn Sie eine app mit messaging implementieren, können Sie z. B. separate benachrichtigungskanäle für jede Konversationsgruppe erstellen, die von einem Benutzer erstellt wird.

[Unter einem Benachrichtigungskanal versteht](~/android/app-fundamentals/notifications/local-notifications.md#notif-chan) wird erläutert, wie einen Benachrichtigungskanal erstellen und es zum Bereitstellen von lokaler Benachrichtigungen verwenden. Eine praktische Codebeispiel finden Sie unter der [NotificationChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) Sample; diese Beispiel-app verwaltet zwei Kanäle und legt Weitere Benachrichtigungsoptionen.



### <a name="notification-badges"></a>Benachrichtigung Signale

Benachrichtigung Signale sind kleine Punkte, die über app-Symbole angezeigt werden, wie in diesem Screenshot gezeigt:

[![Beispiel-Benachrichtigung Signale auf app-Symbole](oreo-images/02-badges-sml.png)](oreo-images/02-badges.png#lightbox)

Diese Punkte anzugeben, dass es neue Benachrichtigungen für eine oder mehrere benachrichtigungskanäle in der app zugeordnet, Symbol "app sind" &ndash; Hierbei handelt es sich um Benachrichtigungen, die der Benutzer noch nicht geschlossen oder Reaktionen bewirkten. Benutzer können lang-auf ein Symbol, drücken Sie, um auf ein Signal Benachrichtigung zugeordneten Benachrichtigungen Blick verworfen wurde oder auf Benachrichtigungen aus dem Menü lang drücken, Appeaars fungiert.

Weitere Informationen zu der Benachrichtigung Signale, finden Sie unter der Android-Entwickler [Benachrichtigung Signale](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#Badges) Thema.



### <a name="custom-fonts-in-xml"></a>Benutzerdefinierte Schriftarten in XML

Android Oreo führt *Schriftarten in XML*, wodurch es möglich, benutzerdefinierte Schriftarten als Ressourcen zu integrieren. OpenType (**.OTF bei**) und TrueType (**.ttf**) Schriftartformate werden unterstützt. Führen Sie folgende Schritte aus, um die Schriftarten als Ressourcen hinzuzufügen:

1. Erstellen einer **Ressourcen/Schriftart** Ordner.

2. Kopieren Sie die Schriftartendateien (z. B. **.ttf** und **.OTF bei** Dateien), **Ressourcen/Schriftart**. 

3. Bei Bedarf umbenennen jedes Schriftartdatei, damit es den Benennungskonventionen für Android-Datei entspricht (d. h. mit nur Kleinschreibung *a-Z*, *0-9*, und Unterstriche im Namen). Z. B. die Schriftartdatei `Pacifico-Regular.ttf` konnte umbenannt werden, z. B. `pacifico.ttf`.

4. Die benutzerdefinierte Schriftart anwenden, indem Sie mit der neuen `android:fontFamily` -Attribut in der Layout-XML. Beispielsweise die folgenden `TextView` Deklaration verwendet die hinzugefügte **pacifico.ttf** Schriftartressource:

   ```xml
   <TextView
     android:text="Example Text in Pacifico Regular"
     android:layout_width="wrap_content"
     android:layout_height="wrap_content"
     android:fontFamily="@font/pacifico" />
   ```

Sie können auch eine Schriftart Familie XML-Datei erstellen, die mehrere Schriftarten sowie Details zur Art und Gewichtung beschreibt. Weitere Informationen finden Sie in der Android-Entwickler [Schriftarten in XML](https://developer.android.com/guide/topics/ui/look-and-feel/fonts-in-xml.html) Thema.


### <a name="downloadable-fonts"></a>Herunterladbare Schriftarten

Beginnen mit dem Android Oreo, können apps Schriftarten aus einen Anbieter, anstatt sie in der APK Bündelung anfordern. Schriftarten werden aus dem Netzwerk heruntergeladen werden, nur bei Bedarf. Diese Funktion verringert APK, Telefon-Speicher und Mobilfunk Datennutzung sparen. Sie können diese Funktion auch auf Android-API-Version 14 oder höher verwenden, durch die Installation des Android-Unterstützung Bibliothek 26-Pakets.

Wenn Ihre app eine Schriftart benötigt, erstellen Sie eine `FontsRequest` Objekt (das Angeben der Schriftart zum Herunterladen) und übergeben Sie sie an eine `FontsContract` Methode, um die Schriftart heruntergeladen. Die folgenden Schritte beschreiben den Downloadvorgang Schriftart im Detail:

1.  Instanziieren einer [FontRequest](https://developer.android.com/reference/android/provider/FontRequest.html) Objekt. 

2.  Unterklasse und instanziieren Sie [FontsContract.FontRequestCallback](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html).

3.  Implementieren der [FontRequestCallback.OnTypeFaceRetrieved](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRetrieved%28android.graphics.Typeface%29) -Methode, die verwendet wird, um den Abschluss der Anforderung Schriftart zu behandeln.

4.  Implementieren der [FontRequestCallback.OnTypeFaceRequestFailed](https://developer.android.com/reference/android/provider/FontsContract.FontRequestCallback.html#onTypefaceRequestFailed%28int%29) -Methode, die verwendet wird, um Ihre app über Fehler informiert werden, die während des Prozesses Schriftart ausgeführt werden.

5.  Rufen Sie die [FontsContract.RequestFonts](https://developer.android.com/reference/android/provider/FontsContract.html#requestFonts(android.content.Context,%20android.provider.FontRequest,%20android.os.Handler,%20android.os.CancellationSignal,%20android.provider.FontsContract.FontRequestCallback)) Methode, um die Schriftart des Schriftart-Anbieters abzurufen. 

Beim Aufrufen der `RequestFonts` Methode prüft zunächst festzustellen, ob die Schriftart lokal zwischengespeichert werden (von einem vorherigen Aufruf von `RequestFont`). Wenn diese nicht zwischengespeichert wird, ruft der Anbieter Schriftart, ruft die Schriftart asynchron ab und übergibt dann die Ergebnisse zurück an Ihre app durch den Aufruf der `OnTypeFaceRetrieved` Methode.

Die [herunterladbare Schriftarten](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) Beispiel veranschaulicht, wie die herunterladbare Schriftarten-Funktion in Android Oreo eingeführt. 

Weitere Informationen zum Herunterladen von Schriftarten, finden Sie in der Android-Entwickler [herunterladbare Schriftarten](https://developer.android.com/guide/topics/ui/look-and-feel/downloadable-fonts.html) Thema.



### <a name="autofill"></a>AutoAusfüllen

Die neue _AutoAusfüllen_ Framework in Android Oreo erleichtert es Benutzern, sich wiederholende Aufgaben wie z. B. Anmeldenamen, die kontoerstellung und Kreditkartentransaktionen zu behandeln. Benutzer Zeit weniger erneut eingeben von Informationen (was zur Eingabe von Fehlern führen kann). Bevor Ihre app mit dem AutoAusfüllen-Framework ausgeführt werden kann, muss ein AutoAusfüllen-Dienst in den Systemeinstellungen aktiviert sein (Benutzer aktivieren oder deaktivieren AutoAusfüllen).

Die [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework/) Beispiel veranschaulicht die Verwendung der AutoAusfüllen-Framework. Es enthält die Implementierungen eines Clients Aktivitäten mit Sichten, die Autofilled und ein Dienst, der AutoAusfüllen-Daten an Client Aktivitäten bereitstellen kann sollten.

Weitere Informationen zu der neuen AutoAusfüllen-Funktion und zum Optimieren Ihrer app für das automatische ausfüllen, finden Sie in der Android-Entwickler [AutoAusfüllen-Framework](https://developer.android.com/guide/topics/text/autofill.html) Thema.



### <a name="picture-in-picture-pip"></a>Bild im Bild (PIP)

Android Oreo ermöglicht es für eine Aktivität zum Starten im Bild-im-Bild (PIP) Modus überlagern der Bildschirm von einer anderen Aktivität. Diese Funktion dient zur Wiedergabe von Videos.

Um anzugeben, dass Ihre app Aktivität PIP-Modus verwenden kann, legen Sie das folgende Flag auf "true" in der Android-Manifest aus:

```xml
android:supportsPictureInPicture
```

Um anzugeben, wie die Aktivität Verhalten soll, wenn er im PIP-Modus befindet, verwenden Sie die neue [PictureInPictureParams](https://developer.android.com/reference/android/app/PictureInPictureParams.html) Objekt. `PictureInPictureParams` Stellt eine Reihe von Parametern, mit denen Sie initialisiert werden, und aktualisieren eine Aktivität im PIP-Modus (z. B. die Aktivität bevorzugte Seitenverhältnis). Die folgenden neuen PIP-Methoden wurden hinzugefügt, um `Activity` in Android Oreo:

-   [EnterPictureInPictureMode](https://developer.android.com/reference/android/app/Activity.html#enterPictureInPictureMode%28android.app.PictureInPictureParams%29) &ndash; versetzt die Aktivität im PIP-Modus. Die Aktivität befindet sich in der Ecke des Bildschirms, und der Rest des Bildschirms wird mit der vorherigen Aktivität, die auf dem Bildschirm wurde gefüllt.

-   [SetPictureInPictureParams](https://developer.android.com/reference/android/app/Activity.html#setPictureInPictureParams%28android.app.PictureInPictureParams%29) &ndash; aktualisiert die Konfigurationseinstellungen für die Aktivität PIP (z. B. eine Änderung im Seitenverhältnis).

Die [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) Beispiel veranschaulicht die grundlegende Verwendung des Modus für Handheldgeräte eingeführten Oreo Bild im Bild (PiP). Im Beispiel wird ein Video der weiterhin ohne Unterbrechung hin und her wechseln zwischen Anzeigemodi oder andere Aktivitäten beim wiedergegeben.



### <a name="other-features"></a>Andere Funktionen

Android Oreo enthält viele neue Features wie die Emoji Unterstützung Speicherort-API-Bibliothek im Hintergrund Grenzwerte, Breite Farbskala Farbe für apps, neue Audiocodecs WebView-Erweiterungen, verbesserte Navigation Tastaturfunktionen und eine neue AAudio (pro Audio)-API für hohe Leistung mit niedriger Latenz Audio für Weitere Informationen zu diesen Funktionen finden Sie in der Android-Entwickler [Android Oreo-Funktionen und APIs](https://developer.android.com/about/versions/oreo/android-8.0.html) Thema.



## <a name="behavior-changes"></a>Verändertes Programmverhalten

Android Oreo umfasst eine Vielzahl von System- und API-verhaltensänderungen, die auf die Funktionalität vorhandener Apps auswirken können. Diese Änderungen werden wie folgt beschrieben werden.


### <a name="background-execution-limits"></a>Grenzwerte für die Ausführung im Hintergrund

Um die benutzerfreundlichkeit zu verbessern, Android Oreo erzwingt Einschränkungen für apps Möglichkeiten beim im Hintergrund ausgeführt. Z. B. wenn der Benutzer ist ein Video ansehen oder ein Spiel, könnten eine im Hintergrund ausgeführte app die Leistung von grafikintensiven Apps im Vordergrund ausgeführt beeinträchtigen. Daher setzt Android Oreo die folgenden Einschränkungen für apps, die nicht direkt mit dem Benutzer interagieren:

1.  **Im Hintergrund Diensteinschränkungen** &ndash; Wenn eine app im Hintergrund ausgeführt wird, ist es ein paar Minuten dauern, in denen es immer noch zulässig, erstellen und Verwenden von Webdiensten Fenster. Am Ende dieses Fensters Android wird die app-Hintergrund-Dienst beendet, und behandelt sie als _im Leerlauf_.

2.  **Broadcast Einschränkungen** &ndash; Android 7.0 (API-25) platziert Einschränkungen auf Broadcasts, der eine app Empfang von registriert. Android Oreo macht diese Einschränkungen strengere. Beispielsweise können apps für Android Oreo nicht mehr broadcast Empfänger für implizite Broadcasts in ihren Manifesten registrieren.

Weitere Informationen zu den neuen Hintergrund Ausführung Grenzen finden Sie unter der Android-Entwickler [Grenzwerte für die Ausführung im Hintergrund](https://developer.android.com/about/versions/oreo/background.html) Thema.


### <a name="breaking-changes"></a>Die Lauffähigkeit der Anwendung beeinträchtigende Änderungen

Apps, die als Ziel Android Oreo oder höher müssen ihre apps zur Unterstützung von der folgenden Änderungen ggf. ändern:

- Android Oreo ist die Möglichkeit, die Priorität der einzelnen Benachrichtigungen festlegen. Stattdessen wird eine empfohlene Wichtigkeitsstufe festlegen, für die Erstellung ein benachrichtigungskanals. Die Wichtigkeit, die Sie einen Benachrichtigungskanal zuweisen gilt für alle Notification-Nachrichten, die Sie bereitstellen, die darauf.

- Für apps für Android Oreo `PendingIntent.GetService()` funktioniert nicht aufgrund der neuen Grenzwerte, die auf Dienste, die im Hintergrund gestartet platziert. Wenn Sie Android Oreo abzielen, verwenden Sie [PendingIntent.GetBroadcast](https://developer.xamarin.com/api/member/Android.App.PendingIntent.GetBroadcast/p/Android.Content.Context/System.Int32/Android.Content.Intent/Android.App.PendingIntentFlags/) stattdessen.  


## <a name="sample-code"></a>Beispielcode

Mehrere Beispiele für die Xamarin.Android stehen zur Veranschaulichung der Vorgehensweise Android Oreo Funktionen nutzen:

-   [NotificationsChannels](https://developer.xamarin.com/samples/monodroid/android-o/NotificationChannels) veranschaulicht, wie das neue Benachrichtigungskanäle-System in Android Oreo eingeführt. In diesem Beispiel verwaltet zwei benachrichtigungskanäle: eines mit Standard-Wichtigkeit und der andere mit hoher Wichtigkeit.

-   [PictureInPicture](https://developer.xamarin.com/samples/monodroid/android-o/PictureInPicture) veranschaulicht die grundlegende Verwendung des Modus Bild im Bild (PiP) für Handheldgeräte in Oreo eingeführt. Im Beispiel wird ein Video der weiterhin ohne Unterbrechung hin und her wechseln zwischen Anzeigemodi oder andere Aktivitäten beim wiedergegeben.

-   [AutofillFramework](https://developer.xamarin.com/samples/monodroid/android-o/AutoFillFramework) veranschaulicht die Verwendung der AutoAusfüllen-Framework. Es enthält die Implementierungen eines Clients Aktivitäten mit Sichten, die Autofilled und ein Dienst, der AutoAusfüllen-Daten an Client Aktivitäten bereitstellen kann sollten.

-   [Herunterladbare Schriftarten](https://developer.xamarin.com/samples/monodroid/android-o/DownloadableFonts) enthält ein Beispiel dafür, wie die zuvor beschriebenen herunterladbare Schriftarten-Funktion verwenden.

-   [EmojiCompat](https://developer.xamarin.com/samples/monodroid/android-o/EmojiCompat) veranschaulicht die Verwendung von EmojiCompat Unterstützungsbibliothek. Diese Bibliothek können Sie um Ihre app mit fehlenden Emoji Zeichen als "Tofu"-Zeichen zu vermeiden.

-   [Speicherort Updates ausstehende Absicht](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdPendIntent) veranschaulicht die Verwendung der Standort-API zum Abrufen von Updates über Standort eines Geräts mithilfe einer `PendingIntent`.

-   [Speicherort Vordergrund Updatedienst](https://developer.xamarin.com/samples/monodroid/android-o/AndroidPlayLocation/LocUpdFgService) veranschaulicht, wie der Standort-API zum Abrufen von Updates über eine mit einem Dienst gebunden ist, und Schritte Vordergrund Gerätestandort.


## <a name="video"></a>Video

> [!VIDEO https://youtube.com/embed/OuvEcaMO-Ho]

**Android 8.0 Oreo-Entwicklung mit c#**


## <a name="summary"></a>Zusammenfassung

In diesem Artikel Android Oreo eingeführt, und es wird erläutert, wie zum Installieren und konfigurieren die neuesten Tools und Pakete für die Entwicklung von Xamarin.Android auf Android Oreo. Es liefert einen Überblick über die wichtigsten Funktionen in Android Oreo verfügbar, mit Links zu Beispielcode über die Quelle für mehrere neue Funktionen. Sie enthalten Links zu API-Dokumentation und Android-Entwickler Themen helfen Ihnen beim Einstieg in das Erstellen von apps für Android Oreo. Außerdem wird die wichtigsten Android Oreo verhaltensänderungen, die vorhandene apps auswirken hervorgehoben.


## <a name="related-links"></a>Verwandte Links

- [Android 8.0 Oreo](https://developer.android.com/index.html)
