---
title: Android 9 Pie
description: Informationen zum Entwickeln von apps für Android mithilfe von Xamarin.Android 9-Kreis.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: fa41affc57714254a12623f79da3dc1396ecd009
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670137"
---
# <a name="android-pie-features"></a>Android Kreis-Funktionen

_Informationen zum Entwickeln von apps für Android mithilfe von Xamarin.Android 9-Kreis._

[Android 9 Kreis](https://developer.android.com/about/versions/pie/) ist jetzt von Google verfügbar. Eine Reihe von neuen Funktionen und APIs in dieser Version vorgenommen werden, und viele davon sind erforderlich, in der neuesten Android-Geräte der neuen Hardwarefunktionen nutzen.

![Android Kreis Hero-Bild](pie-images/01-android-p-logo.png)

In diesem Artikel ist strukturiert, um Ihnen bei der Entwicklung von Xamarin.Android-apps für Android Kreis beim Einstieg helfen. Es wird erläutert, wie die erforderlichen Updates zu installieren, konfigurieren Sie das SDK und bereiten Sie vor einem Emulator oder Gerät zu Testzwecken. Außerdem bietet einen Überblick über die neuen Features in Android Kreis- und enthält Beispiel-Quellcode, das veranschaulicht, wie Sie einige der wichtigsten Features Android Kreis verwenden.

Xamarin.Android 9.0 bietet Unterstützung für Android Kreis. Weitere Informationen zu Xamarin.Android-Unterstützung für Android Kreis-, finden Sie unter den [Android P Developer Preview 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1) Anmerkungen zu dieser Version.

## <a name="requirements"></a>Anforderungen

In der folgende Liste ist erforderlich, Android Kreis-Features in Xamarin-basierte apps verwenden:

-   **Visual Studio** &ndash; bei Verwendung von Windows update für Visual Studio 2017 Version 15,8 oder höher. Wenn Sie einen Mac verwenden, aktualisieren Sie zu Visual Studio 2017 für Mac 7.6 oder höher.

-   **Xamarin.Android** &ndash; Xamarin.Android 9.0.0.17 oder höher muss installiert sein, mit Visual Studio (Xamarin.Android wird automatisch installiert, als Teil der **Mobile Entwicklung mit .NET** Workload).

-   **Java Developer Kit** &ndash; Entwicklung mit Xamarin Android 9.0 erfordert [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (oder Sie können versuchen, die Vorschau von Microsoft Verteilung der [OpenJDK](~/android/get-started/installation/openjdk.md)). JDK8 wird automatisch installiert, als Teil der **Mobile Entwicklung mit .NET** arbeitsauslastung.

-   **Android SDK** &ndash; Android SDK-API-28 oder höher muss über den Android SDK Manager installiert werden.

## <a name="getting-started"></a>Erste Schritte

Informationen zum Einstieg mit Xamarin.Android Android Pie-apps entwickeln, müssen Sie herunterladen und die neuesten Tools und SDK-Pakete installieren, bevor Sie Ihr erste Android Kreis-Projekt erstellen können:

1. Aktualisieren auf [Visual Studio 2017 Version 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) oder höher. Wenn Sie Visual Studio für Mac verwenden, aktualisieren Sie auf [Visual Studio 2017 für Mac, Version 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) oder höher.

2. Installieren Sie **Android Pie (API-28)** -Pakete und Tools über den SDK-Manager.

3. Erstellen eines neuen Xamarin.Android-Projekts, das als Ziel **Android 9.0**.

4. Konfigurieren Sie einen Emulator oder Gerät zum Testen von Android Pie-apps.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:


### <a name="update-visual-studio"></a>Aktualisieren von Visual Studio 2017

Aktualisieren Sie zum Hinzufügen der Unterstützung für Android Kreis zu Visual Studio auf Visual Studio 2017 Version 15,8 oder höher (Anweisungen hierzu finden Sie unter [aktualisieren Visual Studio 2017 auf die neueste Version](https://docs.microsoft.com/visualstudio/install/update-visual-studio)).

Um Visual Studio für Mac Android Kreis-Unterstützung hinzugefügt haben, aktualisieren Sie zu Visual Studio 2017 für Mac 7.6 oder höher (Anweisungen hierzu finden Sie unter [Setup und installieren Sie Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation)).


### <a name="install-the-android-sdk"></a>Installieren Sie das Android SDK

Zum Erstellen eines Projekts mit Xamarin.Android 9.0, müssen Sie zuerst den Android SDK Manager verwenden, installieren Sie die SDK-Plattform für **Android Pie (API-Ebene 28)** oder höher.

1. Starten Sie den SDK-Manager. Klicken Sie in Visual Studio auf **Tools > Android > Android SDK Manager**. Klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**.

2. Klicken Sie in der unteren rechten Ecke, klicken Sie auf das Zahnradsymbol, und wählen Sie **Repository > Google (nicht unterstützt)**:

    [![Das Repository festlegen an Google](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. Installieren der **Android Kreis** SDK-Pakete, die als aufgelistet sind **Android SDK Platform-28** in die **Plattformen** Registerkarte (Weitere Informationen zur Verwendung des SDK-Managers finden Sie unter [ Android SDK-Installation](~/android/get-started/installation/android-sdk.md)):

    [![Installieren von Paketen für Android Kreis](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. Wenn Sie einen Emulator verwenden, erstellen Sie ein virtuelles Gerät, unterstützt **-API-Ebene 28**. Weitere Informationen zum Erstellen von virtuellen Geräten finden Sie unter [Verwalten von virtuellen Geräten mit Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).



### <a name="start-a-xamarinandroid-project"></a>Ein Xamarin.Android-Projekt starten

Erstellen eines neuen Xamarin.Android-Projekts an. Wenn Sie die Android-Entwicklung mit Xamarin vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/index.md) um weitere Informationen zum Erstellen von Xamarin.Android-Projekte.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die versionseinstellungen Ziel Android 9.0 oder höher konfigurieren. Z. B. um das Projekt für Android Kreis als Ziel festzulegen, müssen Sie konfigurieren die Android-API-Zielebene des Projekts in **Android 9.0** (28-API). Es wird empfohlen, dass Sie auch Ihr Zielframework Level-API-28 oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie unter [Understanding Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md).


### <a name="configure-a-device-or-emulator"></a>Konfigurieren von einem Gerät oder emulator

Wenn Sie z. B. eine Nexus oder eines Pixels ein physisches Gerät verwenden, können Sie Ihr Gerät zum Android Kreis aktualisieren, mithilfe der Anweisungen in [Factory Images für Nexus und Pixel-Geräte](https://developers.google.com/android/images).

Wenn Sie einen Emulator verwenden, Erstellen eines virtuellen Geräts für API-Ebene 28, und wählen Sie ein Image X86-basierten. Weitere Informationen zur Verwendung von Android-Geräte-Manager zum Erstellen und Verwalten von virtuellen Geräten finden Sie unter [Verwalten von virtuellen Geräten mit Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Weitere Informationen zur Verwendung von Android-Emulator zum Testen und Debuggen, finden Sie unter [Debuggen auf Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).



## <a name="new-features"></a>Neue Funktionen

Android Kreis wird eine Vielzahl neuer Features eingeführt. Einige dieser neuen Features sollen nutzen, neue Hardwarefunktionen, die von der neuesten Android-Geräte bereitgestellt werden, während andere entworfen werden, um die Android-benutzerfreundlichkeit weiter zu verbessern:

-   **Anzeigeunterstützung Ausschnitt** &ndash; bietet APIs, um den Speicherort und die Form des finden die _Ausschnitt_ am oberen Rand des Bildschirms auf neueren Android-Geräten.

-   **Verbesserungen der Benachrichtigung** &ndash; können jetzt Benachrichtigungen anzeigen, Bilder und ein neues `Person` Klasse wird verwendet, um die Teilnehmer der Konversation zu vereinfachen.

-   **Ruhiges Positionierung** &ndash; Neuheit Unterstützung für das WLAN-Round-Trip-Zeit-Protokoll, das apps über WiFi-Geräte für die Navigation in ruhiges Einstellungen ermöglicht.

-   **Unterstützung für Multi-Kamera** &ndash; gleichzeitig bietet die Möglichkeit in Streams für den Zugriff von mehreren physischen Kameras (z. B. Dual-Front "und" Dual-Back-Kameras).


In den folgenden Abschnitten markieren diese Funktionen und angeben, dass die Nutzung in Ihrer app kurze Codebeispiele können Sie beginnen.

### <a name="display-cutout-support"></a>Ausschnitt anzeigeunterstützung

Viele neuere Android-Geräte mit Edge-to-Edge-Bildschirme sind eine *Ausschnitt anzeigen* (oder "Kerbe") am oberen Rand der Anzeige der Kamera und hält Vorträge.
Der folgende Screenshot enthält ein Beispiel für Emulator einen Ausschnitt:

[![Android-Emulator simuliert einen Ausschnitt](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

Um zu verwalten wie Ihre app-Fenster seinen Inhalt auf Geräten mit einem Display-Ausschnitt zeigt, Android Kreis wurde hinzugefügt, ein neues [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) Fenster Layout-Attribut. Dieses Attribut kann auf einen der folgenden Werte festgelegt werden:

-   [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; Fenster ist nie zulässig Ausschnitt Bereich überschneiden.

-   [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; Fenster in den Bereich der Ausschnitt jedoch nur an der kurzen Kante des Bildschirms erweitern können. 

-   [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; Fenster kann in den Bereich der Ausschnitt erweitert werden soll, wenn der Ausschnitt in einer Systemleiste enthalten ist.

Um zu verhindern, dass das app-Fenster überschneidet sich mit den Ausschnitt-Bereich, legen Sie beispielsweise den Ausschnitt Layoutmodus auf *nie*: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

Die folgenden Beispiele enthalten Beispiele für diesen Ausschnitt-Modi. Der erste Screenshot auf der linken Seite ist für die app im nicht-Vollbildmodus. Im Screenshot Center ist die app wechselt Vollbild-mit `LayoutInDisplayCutoutMode` festgelegt `LayoutInDisplayCutoutModeShortEdges`. Beachten Sie, dass es sich bei der app weißer Hintergrund in den Anzeigebereich der Ausschnitt erweitert:

[![Beispiel Anzeigemodi Ausschnitt im emulator](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

Im letzten Screenshot (oben auf der rechten Seite), `LayoutInDisplayCutoutMode` nastaven NA hodnotu `LayoutInDisplayCutoutModeShortNever` vor zum Vollbildmodus gewechselt wird.
Beachten Sie, dass der app weißer Hintergrund darf nicht auf den Anzeigebereich der Ausschnitt erweitert.

Wenn Sie ausführlichere Informationen zu den Ausschnitt-Bereich auf dem Gerät benötigen, können Sie die neue [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) Klasse. `DisplayCutout` Stellt den Bereich der Anzeige, die verwendet werden kann, um Inhalt anzuzeigen. Sie können diese Informationen verwenden, um die Position und Form der Ausschnitt abzurufen, damit Ihre app nicht versucht, Inhalt in diesem Bereich nicht funktionsbezogenen angezeigt.

Weitere Informationen zu den neuen Ausschnitt-Features in Android P, finden Sie unter [Ausschnitt Anzeigeunterstützung](https://developer.android.com/about/versions/pie/android-9.0#cutout).



### <a name="notifications-enhancements"></a>Verbesserungen für Benachrichtigungen

Android Kreis führt die folgenden Verbesserungen messaging verbessern:

-   Benachrichtigungskanäle (eingeführt in [Android Oreo](~/android/platform/oreo.md)) unterstützt jetzt die Blockierung des Channel-Gruppen.

-   Das Benachrichtigungssystem verfügt über drei neue-Not-stören Kategorien (Priorisieren von Alarmen, Systemsounds und Medien). Darüber hinaus stehen sieben neue-Not-stören Modi, die mit dem visual Unterbrechungen (z. B. Badges, Benachrichtigung Beleuchtung, Status Leiste Darstellungen und Vollbild-Aktivitäten starten) unterdrückt werden können.

-   Ein neues [Person](https://developer.android.com/reference/android/app/Person.html) -Klasse wurde hinzugefügt, die den Absender einer Nachricht darstellt. Diese Klasse hilft, die um das Rendern von jede Benachrichtigung zu optimieren, indem Sie in einer Konversation (einschließlich ihrer Avatare und URIs) beteiligten Personen zu identifizieren.

-   Benachrichtigungen können Images jetzt anzeigen. 

Das folgende Beispiel veranschaulicht, wie Sie die neuen APIs verwenden, um eine Benachrichtigung generieren, die ein Bild enthält. In den folgenden Screenshots eine SMS-Benachrichtigung gesendet wird und eine Benachrichtigung mit der ein eingebettetes Bild folgt. Die zweite Benachrichtigung wird vergrößert, wenn die Benachrichtigungen erweitert werden, (wie auf der rechten Seite dargestellt), wird der Text von der ersten Benachrichtigung angezeigt, und das Bild in eingebetteten:

[![Beispiel-Benachrichtigung mit Bild](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

Das folgende Beispiel veranschaulicht, wie Sie ein Bild in einer Benachrichtigung Android Kreis enthalten, und es veranschaulicht die Verwendung der neuen `Person` Klasse:

1. Erstellen Sie eine `Person` -Objekt, den Absender darstellt. Z. B. Name und das Symbol des Senders befinden sich im `fromPerson`:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Erstellen Sie eine `Notification.MessagingStyle.Message` , enthält das Bild, das senden möchten, übergeben das Bild, das die neue [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) Methode.
   Zum Beispiel:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Fügen Sie die Nachricht an eine `Notification.MessagingStyle` Objekt. Zum Beispiel:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Schließen Sie dieses Format, in der Benachrichtigung-Generator. Zum Beispiel:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Veröffentlichen Sie die Benachrichtigung ein. Zum Beispiel:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Weitere Informationen zum Erstellen von Benachrichtigungen finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).


### <a name="indoor-positioning"></a>Ruhiges Positionierung

Android Kreis bietet Unterstützung für IEEE 802.11mc (auch bekannt als _WiFi Round-Trip-Zeit_ oder _WiFi RTT_), wodurch es möglich, für apps, um die Entfernung zu einem zu erkennen oder weitere Wi-Fi-Zugriffspunkte. Mithilfe dieser Informationen kann für Ihre app nutzen *ruhiges Positionierung* mit einer Genauigkeit von ein bis zwei Verbrauchseinheiten. Auf Android-Geräten, die für IEEE 801.11mc Hardware unterstützen, kann Ihre app Navigationsfunktionen, z. B. standortbasierte-Steuerelement von smarten Geräten über autonomes oder Turn-von-Turn-Anweisungen über eine Store anbieten:

[![Beispiel für ruhiges Navigation, die über WiFi RTT](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

Die neue [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) Klasse und mehrere Hilfsklassen, bietet die Möglichkeit zum Messen der Entfernung für Wi-Fi-Geräte. Weitere Informationen zu den ruhiges positionierungs-APIs, die in Android P eingeführt wurden, finden Sie unter [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).


### <a name="multi-camera-support"></a>Multi-Kamera-Unterstützung

Viele neuere Android-Geräte haben Dual-Front "und/oder" Dual-Back-Kameras, die nützlich für Features wie Stereo Bildanalyse, verbesserte visuelle Effekte und verbesserte Zoom-Funktion sind. Android P wird ein neuer [Multi-Kamera](https://developer.android.com/about/versions/pie/android-9.0#camera) -API, die Ihre app ermöglicht ein *logische Kamera* (oder *logische Multi-Kamera*), basiert auf zwei oder mehr physische Kameras.
Um zu bestimmen, ob das Gerät eine logische Multi-Kamera unterstützt, sehen Sie sich die Funktionen von jede Kamera auf dem Gerät, um festzustellen, ob es unterstützt [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA).

Android Kreis enthält auch eine neue [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) -Klasse, die zum Reduzieren Verzögerungen bei der ersten Erfassung und ist somit überflüssig zu starten, und starten die Kamera-Datenstrom verwendet werden kann.

Weitere Informationen zu Multi-Kamera-Unterstützung in Android P, finden Sie unter [Multi-Kamera-Unterstützung und Kamera Updates](https://developer.android.com/about/versions/pie/android-9.0#camera).


### <a name="other-features"></a>Weitere Funktionen

Darüber hinaus unterstützt Android Kreis-, mehrere neue Features:

-   Die neue [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) -Klasse, die zum Zeichnen und Anzeigen von animierten Bildern verwendet werden kann.

-   Ein neues [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) -Klasse, die ersetzt `BitmapFactory`. `ImageDecoder` kann verwendet werden, um die Decodierung einer `AnimatedImageDrawable`.

-   Images für HDR (hoch dynamischen Bereich)-Video und HEIF (hohe Effizienz Image File Format) unterstützt.

-   Die [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) wurde verbessert, um intelligentere netzwerkbezogener Aufträge zu verarbeiten. Die neue [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) Methode der [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters) -Klasse gibt das beste Netzwerk für die Durchführung jeder netzwerkanforderungen für einen bestimmten Auftrag zurück.

Weitere Informationen über die neuesten Android Kreis-Features finden Sie unter [Android 9-Funktionen und APIs](https://developer.android.com/about/versions/pie/android-9.0).


## <a name="behavior-changes"></a>Verhaltensänderungen

Wenn die Android-Zielversion auf API-Ebene 28 festgelegt ist, sind mehrere plattformänderungen, die das Verhalten Ihrer app auswirken können, selbst wenn Sie nicht die neuen Features, die oben beschriebenen implementieren. Die folgende Liste enthält eine kurze Zusammenfassung dieser Änderungen:

-  Apps müssen die Vordergrund-Berechtigung vor der Verwendung von vordergrunddienste jetzt anfordern.

-  Wenn Ihre Anwendung mehr als einem Prozess verfügt, kann nicht es Freigeben eines einzelnen [WebView](https://developer.xamarin.com/api/type/Android.Webkit.WebView/) Datenverzeichnis zwischen Prozessen.

-  Direkten Zugriff auf eine andere app-Datenverzeichnis anhand des Pfads ist nicht mehr zulässig.

Weitere Informationen zu verhaltensänderungen für apps für Android-P, finden Sie unter [Verhaltensänderungen](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps).


## <a name="sample-code"></a>Beispielcode

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) ist eine Xamarin.Android-Beispiel-app für Android Kreis, die zeigt, wie Ausschnitt Anzeigemodi festgelegt wie für die Verwendung der neuen `Person` -Klasse, und wie Sie eine Benachrichtigung zu senden, die ein Bild enthält.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel eingeführt Android Kreis- und erläutert, wie zum Installieren und konfigurieren die neuesten Tools und Pakete für Xamarin.Android-Entwicklung mit Android Kreis. Sie haben einen Überblick über die wichtigsten Features in Android Kreis-und mit Beispiel-Quellcode für mehrere dieser Features bereitgestellt.
Sie enthalten Links zur Dokumentation der API und erste Schritte mit Android Developer-Themen, die Sie unterstützen das Erstellen von apps für Android Kreis. Sie markiert auch die wichtigsten Android Kreis verhaltensänderungen, die vorhandene apps auswirken können.


## <a name="related-links"></a>Verwandte Links

- [Android 9 Kreis](https://developer.android.com/about/versions/pie/)
