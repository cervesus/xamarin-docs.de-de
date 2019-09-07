---
title: Android 9 Pie
description: Erfahren Sie, wie Sie mit xamarin. Android mit der Entwicklung von Apps für Android 9-Kreis beginnen.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/21/2018
ms.openlocfilehash: 6475cd0f27e41321902b57dd28f59bfb250e0c8f
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757464"
---
# <a name="android-pie-features"></a>Android-Kreis Features

_Erfahren Sie, wie Sie mit xamarin. Android mit der Entwicklung von Apps für Android 9-Kreis beginnen._

Der [Android 9](https://developer.android.com/about/versions/pie/) -Kreis ist jetzt über Google verfügbar. In dieser Version werden eine Reihe neuer Features und APIs zur Verfügung gestellt, und viele davon sind erforderlich, um neue Hardwarefunktionen in den neuesten Android-Geräten nutzen zu können.

![Android-Kreis-Hero-Image](pie-images/01-android-p-logo.png)

Dieser Artikel ist für den Einstieg in die Entwicklung von xamarin. Android-Apps für Android-Kreis strukturiert. Darin wird erläutert, wie Sie die erforderlichen Updates installieren, das SDK konfigurieren und einen Emulator oder ein Gerät für Tests vorbereiten. Außerdem finden Sie hier einen Überblick über die neuen Features in Android-Kreis und einen Beispiel Quellcode, der veranschaulicht, wie einige der wichtigsten Android-Kreis Features verwendet werden.

Xamarin. Android 9,0 bietet Unterstützung für Android-Kreis. Weitere Informationen zur xamarin. Android-Unterstützung für Android-Kreis finden Sie in den Anmerkungen zu dieser Version von [Android P Developer Preview 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1) .

## <a name="requirements"></a>Anforderungen

Die folgende Liste ist erforderlich, um die Android-Kreis Funktionen in xamarin-basierten apps zu verwenden:

- **Visual Studio** &ndash; Visual Studio 2019 wird empfohlen.
    Wenn Sie Visual Studio 2017 verwenden, unter Windows Update auf Visual Studio 2017 Version 15,8 oder höher. Aktualisieren Sie unter macOS auf Visual Studio 2017 für Mac, Version 7,6 oder höher.

- **Xamarin. Android** &ndash; xamarin. Android 9.0.0.17 oder höher muss mit Visual Studio installiert werden (xamarin. Android wird automatisch als Teil der Mobile- **Entwicklung mit .net** -Arbeitsauslastung installiert).

- **Java Developer Kit** Die Entwicklung von xamarin Android 9,0 erfordert [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (oder Sie können die Vorschau der Microsoft-Distribution von [openjdk](~/android/get-started/installation/openjdk.md)testen). &ndash; JDK8 wird automatisch als Teil der Mobile- **Entwicklung mit .net** -Arbeitsauslastung installiert.

- **Android SDK** &ndash; Android SDK API 28 oder höher muss über den Android SDK-Manager installiert werden.

## <a name="getting-started"></a>Erste Schritte

Um mit der Entwicklung von Android-Kreis-apps mit xamarin. Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie Ihr erstes Android-kreisprojekt erstellen können:

1. Visual Studio 2019 wird empfohlen. Wenn Sie Visual Studio 2017 verwenden, aktualisieren Sie auf [Visual Studio 2017 Version 15,8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) oder höher. Wenn Sie Visual Studio für Mac verwenden, aktualisieren Sie auf [Visual Studio 2017 für Mac, Version 7,6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) oder höher.

2. Installieren Sie Pakete und Tools für Android-Kreis **(API 28)** über den SDK-Manager.

3. Erstellen Sie ein neues xamarin. Android-Projekt, das auf **Android 9,0**abzielt.

4. Konfigurieren Sie einen Emulator oder ein Gerät zum Testen von Android-Kreis-apps.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

### <a name="update-visual-studio"></a>Aktualisieren von Visual Studio 2017

Visual Studio 2019 wird zum Entwickeln von Android-Kreis-apps mit xamarin empfohlen.

Wenn Sie Visual Studio 2017 verwenden, aktualisieren Sie auf Visual Studio 2017 Version 15,8 oder höher (Anweisungen dazu finden Sie unter [Aktualisieren von Visual Studio 2017 auf die neueste Version](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). Aktualisieren Sie unter macOS auf Visual Studio 2017 für Mac 7,6 oder höher (Anweisungen finden Sie unter [Setup und Installation Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation)).

### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Zum Erstellen eines Projekts mit xamarin. Android 9,0 müssen Sie zunächst den Android SDK-Manager verwenden, um die SDK-Plattform für Android-Kreis **(API-Ebene 28)** oder höher zu installieren.

1. Starten Sie den SDK-Manager. Klicken Sie in Visual Studio auf Extras **> Android-> Android SDK-Manager**. Klicken Sie in Visual Studio für Mac auf Extras **> SDK-Manager**.

2. Klicken Sie in der unteren rechten Ecke auf das Zahnrad Symbol, und wählen Sie **Repository > Google (nicht unterstützt)** :

    [![Festlegen des Repository auf Google](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. Installieren Sie die **Android** -Paket-SDK-Pakete, die auf der Registerkarte **Plattformen** als **Android SDK Plattform 28** aufgeführt sind. (Weitere Informationen zur Verwendung des SDK-Managers finden Sie unter [Android SDK Setup](~/android/get-started/installation/android-sdk.md).):

    [![Installieren von Android-Kreis Paketen](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. Wenn Sie einen Emulator verwenden, erstellen Sie ein virtuelles Gerät, das **API-Ebene 28**unterstützt. Weitere Informationen zum Erstellen von virtuellen Geräten finden Sie unter [Verwalten von virtuellen Geräten mit dem Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).

### <a name="start-a-xamarinandroid-project"></a>Starten eines xamarin. Android-Projekts

Erstellen Sie ein neues xamarin. Android-Projekt. Wenn Sie mit der Android-Entwicklung mit xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/index.md) Weitere Informationen zum Erstellen von xamarin. Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versions Einstellungen so konfigurieren, dass Sie Android 9,0 oder höher als Ziel haben. Wenn Sie z. b. Ihr Projekt für Android-Kreis als Ziel festlegen möchten, müssen Sie die Android-API-Ziel Ebene Ihres Projekts auf **Android 9,0** (API 28) konfigurieren. Es wird empfohlen, dass Sie auch die zielframeworkebene auf API 28 oder höher festlegen. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie unter [Understanding Android API Levels](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-a-device-or-emulator"></a>Konfigurieren eines Geräts oder Emulators

Wenn Sie ein physisches Gerät, z. b. einen Nexus oder ein Pixel, verwenden, können Sie Ihr Gerät mit dem Android-Kreis aktualisieren, indem Sie die Anweisungen unter [Factory-Images für Nexus-und Pixel-Geräte](https://developers.google.com/android/images)befolgen.

Wenn Sie einen Emulator verwenden, erstellen Sie ein virtuelles Gerät für API-Ebene 28, und wählen Sie ein x86-basiertes Abbild aus. Informationen zur Verwendung der Android Device Manager zum Erstellen und Verwalten von virtuellen Geräten finden Sie unter [Verwalten von virtuellen Geräten mit dem Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Weitere Informationen zum Verwenden des Android-Emulators zum Testen und Debuggen finden Sie unter [Debugging auf dem Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="new-features"></a>Neue Funktionen

Mit dem Android-Kreis wird eine Reihe neuer Features eingeführt. Einige dieser neuen Features sind darauf ausgelegt, neue Hardwarefunktionen zu nutzen, die von den neuesten Android-Geräten angeboten werden, während andere die Benutzer Funktionalität von Android weiter verbessern können:

- **Unterstützung für Ausschneide Anzeige** Stellt APIs bereit, um den Speicherort und die Form des Ausschneide Fensters auf neueren Android-Geräten im oberen Bildschirmbereich zu finden. &ndash;

- **Benachrichtigungs Erweiterungen** Benachrichtigungs Meldungen können jetzt Bilder anzeigen, und eine `Person` neue Klasse wird verwendet, um Konversations Teilnehmer zu vereinfachen. &ndash;

- **Innen Positionierung** &ndash; Platt Form Unterstützung für das WLAN-Roundtrip-Zeitprotokoll, das es Apps ermöglicht, WiFi-Geräte für die Navigation in den Einstellungen von innen zu verwenden.

- **Unterstützung für mehrere Kameras** &ndash; Bietet die Möglichkeit, gleichzeitig von mehreren physischen Kameras aus auf Streams zuzugreifen (z. b. Dual-Front-und Dual-Back-Kameras).

In den folgenden Abschnitten werden diese Features hervorgehoben und kurze Codebeispiele bereitgestellt, die Ihnen bei der Verwendung in Ihrer APP helfen.

### <a name="display-cutout-support"></a>Unterstützung für Ausschneide Anzeige

Viele neuere Android-Geräte mit Edge-to-Edge-Bildschirmen verfügen am oberen Rand der Anzeige für Kamera und Redner über einen Bildschirm zum *anzeigen* .
Der folgende Screenshot zeigt ein emulatorbeispiel für einen Ausschnitt:

[![Android-Emulator, der einen Ausschnitt simuliert](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

Um zu verwalten, wie Ihr App-Fenster seinen Inhalt auf Geräten anzeigt, auf denen ein Ausschnitt angezeigt wird, hat Android-Kreis ein neues [layoutunbestrilaycutoutmode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) -Fenster Layout-Attribut hinzugefügt. Dieses Attribut kann auf einen der folgenden Werte festgelegt werden:

- [Layoutunbestrilaycutoutmumuever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER) &ndash; Das Fenster darf sich niemals mit dem Ausschneide Bereich überlappen.

- [Layoutunbestreitlaycutoutmodeshortedges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES) &ndash; Das Fenster darf nur auf den kurzen Rand des Bildschirms in den Ausschneide Bereich ausgedehnt werden. 

- [Layoutunbestreitlaycutoutmodedefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT) &ndash; Das Fenster kann in den Ausschneide Bereich erweitert werden, wenn das Ausschneide in einer Systemleiste enthalten ist.

Um beispielsweise zu verhindern, dass sich das Fenster der APP mit dem Ausschneide Bereich überschneidet, legen Sie den layoutausschneide Modus auf *nie*fest: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

In den folgenden Beispielen werden Beispiele für diese Ausschneide Modi bereitgestellt. Der erste Screenshot auf der linken Seite befindet sich in der APP im nicht Vollbildmodus. Im Mittelpunkt des Screenshots wird die APP Vollbildmodus, `LayoutInDisplayCutoutMode` wobei auf `LayoutInDisplayCutoutModeShortEdges`festgelegt ist. Beachten Sie, dass sich der weiße Hintergrund der APP auf den Bereich Anzeige Ausschnitt erstreckt:

[![Beispiel für das Anzeigen von Ausschneide Modi im Emulator](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

Im abschließenden Screenshot (oben rechts) wird auf `LayoutInDisplayCutoutMode` `LayoutInDisplayCutoutModeShortNever` festgelegt, bevor es in den Vollbildmodus wechselt.
Beachten Sie, dass der weiße Hintergrund der APP nicht in den Bereich für die Anzeige Ausschnitte erweitert werden darf.

Wenn Sie ausführlichere Informationen zum Ausschneide Bereich auf dem Gerät benötigen, können Sie die neue [displaycutout](https://developer.android.com/reference/android/view/DisplayCutout.html) -Klasse verwenden. `DisplayCutout`stellt den Bereich der Anzeige dar, der nicht zum Anzeigen von Inhalt verwendet werden kann. Sie können diese Informationen verwenden, um den Speicherort und die Form des Ausschneide Bereichs abzurufen, sodass Ihre APP nicht versucht, Inhalt in diesem nicht Funktionsbereich anzuzeigen.

Weitere Informationen zu den neuen Ausschneide Features in Android P finden Sie [unter Unterstützung der Ausschneide](https://developer.android.com/about/versions/pie/android-9.0#cutout)Funktion.

### <a name="notifications-enhancements"></a>Benachrichtigungs Erweiterungen

Der Android-Kreis bietet die folgenden Verbesserungen, um das Messaging zu verbessern:

- Benachrichtigungs Kanäle (eingeführt in [Android Oreo](~/android/platform/oreo.md)) unterstützen jetzt das Blockieren von Kanalgruppen.

- Das Benachrichtigungssystem verfügt über drei neue, nicht störende Kategorien (Priorisieren von Alarmen, Systemsounds und Medienquellen). Außerdem gibt es sieben neue do-not-stört-Modi, die verwendet werden können, um visuelle Unterbrechungen (z. b. Abzeichen, Benachrichtigungs Beleuchtung, Status leisten Auftritte und das Starten von voll Bild Aktivitäten) zu unterdrücken.

- Eine neue [Person](https://developer.android.com/reference/android/app/Person.html) -Klasse wurde hinzugefügt, um den Absender einer Nachricht darzustellen. Mit dieser Klasse können Sie das Rendering der einzelnen Benachrichtigungen optimieren, indem Sie die an einer Konversation beteiligten Personen (einschließlich ihrer Avatare und URIs) identifizieren.

- Benachrichtigungen können jetzt Bilder anzeigen. 

Im folgenden Beispiel wird veranschaulicht, wie die neuen APIs verwendet werden, um eine Benachrichtigung zu generieren, die ein Bild enthält. In den folgenden Screenshots wird eine Text Benachrichtigung gesendet, auf die eine Benachrichtigung mit einem eingebetteten Bild folgt. Wenn die Benachrichtigungen erweitert werden (wie auf der rechten Seite angezeigt), wird der Text der ersten Benachrichtigung angezeigt, und das in der zweiten Benachrichtigung eingebettete Bild wird vergrößert:

[![Beispiel Benachrichtigung mit Image](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

Im folgenden Beispiel wird veranschaulicht, wie Sie ein Bild in eine Android-Kreis Benachrichtigung einschließen und die Verwendung der neuen `Person` Klasse veranschaulichen:

1. Erstellen Sie `Person` ein-Objekt, das den Absender darstellt. Der Name und das Symbol des Absenders sind z. b `fromPerson`. in enthalten:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Erstellen Sie `Notification.MessagingStyle.Message` eine, die das zu sendende Bild enthält, und übergeben Sie das Bild an die neue [Notification. messagingstyle. Message. SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri) -Methode.
   Beispiel:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Fügen Sie die Nachricht einem `Notification.MessagingStyle` -Objekt hinzu. Beispiel:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Binden Sie diesen Stil in den Benachrichtigungs-Generator ein. Beispiel:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Veröffentlichen Sie die Benachrichtigung. Beispiel:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Weitere Informationen zum Erstellen von Benachrichtigungen finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

### <a name="indoor-positioning"></a>Innen Positionierung

Android-Kreis bietet Unterstützung für IEEE 802.11 MC (auch bekannt als _WiFi Round-Trip-Time_ oder _WiFi RTT_). Dadurch können Apps die Entfernung zu einem oder mehreren WLAN-Zugriffs Punkten erkennen. Mithilfe dieser Informationen ist es möglich, dass Ihre APP die *innen Positionierung* mit einer Genauigkeit von bis zu zwei Metern nutzt. Auf Android-Geräten, die Hardwareunterstützung für IEEE 801.11 MC bereitstellen, kann Ihre APP Navigations Features wie z. b. die standortbasierte Steuerung von intelligenten Geräten oder eine Reihe von Anweisungen in einem Store anbieten:

[![Beispiel für die Navigation im Innenbereich mithilfe von WiFi RTT](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

Die neue [wifirttmanager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager) -Klasse und mehrere Hilfsklassen bieten die Mittel zum Messen der Entfernung zu WLAN-Geräten. Weitere Informationen zu den in Android P eingeführten APIs für die innen Positionierung finden Sie unter [Android .net. WiFi. RTT](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).

### <a name="multi-camera-support"></a>Unterstützung für mehrere Kameras

Viele neuere Android-Geräte verfügen über Dual-Front-und/oder-Dual-Back-Kameras, die für Funktionen wie Stereo Vision, verbesserte visuelle Effekte und verbesserte Zoomfunktion nützlich sind. Android P führt eine neue [Multi-Kamera](https://developer.android.com/about/versions/pie/android-9.0#camera) -API ein, die es Ihrer APP ermöglicht, eine *logische Kamera* (oder eine *logische Multikamera*) zu verwenden, die von mindestens zwei physischen Kameras unterstützt wird.
Um festzustellen, ob das Gerät eine logische Multikamera unterstützt, können Sie die Funktionen der einzelnen Kameras auf dem Gerät überprüfen, um festzustellen, ob es [requestavailablecapabilitieslogicalmulticamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA)unterstützt.

Der Android-Kreis umfasst auch eine neue [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html) -Klasse, die verwendet werden kann, um Verzögerungen während der ersten Erfassung zu verringern und das Starten und Starten des Kameradaten Stroms zu vermeiden.

Weitere Informationen zur Unterstützung mehrerer Kameras in Android P finden Sie [unter Unterstützung mehrerer Kameras und Kamera Updates](https://developer.android.com/about/versions/pie/android-9.0#camera).

### <a name="other-features"></a>Weitere Funktionen

Außerdem unterstützt Android-Kreis eine Reihe weiterer neuer Features:

- Die neue [animatedimagedrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html) -Klasse, die zum Zeichnen und anzeigen animierter Bilder verwendet werden kann.

- Eine neue [imagedecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html) -Klasse, `BitmapFactory`die ersetzt. `ImageDecoder`kann verwendet werden, um eine `AnimatedImageDrawable`zu decodieren.

- Unterstützung für Video-und heif-Bilder (High Efficiency Image File Format) für HDR (Hochdynamisches Bereich).

- [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) wurde verbessert, um netzwerkbezogene Aufträge intelligenter verarbeiten zu können. Die neue [getnetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29) -Methode der [jobparameters](https://developer.android.com/reference/android/app/job/JobParameters) -Klasse gibt das beste Netzwerk zum Ausführen beliebiger Netzwerk Anforderungen für einen bestimmten Auftrag zurück.

Weitere Informationen zu den neuesten Android-Kreis Features finden Sie unter [Android 9 Features und APIs](https://developer.android.com/about/versions/pie/android-9.0).

## <a name="behavior-changes"></a>Verhaltensänderungen

Wenn die Android-Ziel Version auf API-Ebene 28 festgelegt ist, gibt es mehrere Platt Formänderungen, die sich auf das Verhalten Ihrer APP auswirken können, auch wenn Sie die oben beschriebenen neuen Features nicht implementieren. Die folgende Liste stellt eine kurze Zusammenfassung dieser Änderungen dar:

- Apps müssen jetzt die Vordergrund Berechtigung anfordern, bevor Sie Vordergrund Dienste verwenden.

- Wenn Ihre APP mehr als einen Prozess umfasst, kann Sie ein einzelnes [WebView](xref:Android.Webkit.WebView) -Datenverzeichnis nicht Prozess übergreifend freigeben.

- Der direkte Zugriff auf das Datenverzeichnis einer anderen APP über den Pfad ist nicht mehr zulässig.

Weitere Informationen zu Verhaltensänderungen für apps, die auf Android P abzielen, finden Sie unter [Verhaltensänderungen](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps).

## <a name="sample-code"></a>Beispielcode

[Androidpminidemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) ist eine xamarin. Android-Beispiel-App für Android, die veranschaulicht, wie die Anzeige Ausschneide Modi festgelegt werden, `Person` wie die neue Klasse verwendet wird und wie eine Benachrichtigung gesendet wird, die ein Bild enthält.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android-Kreis vorgestellt und erläutert, wie die neuesten Tools und Pakete für die xamarin. Android-Entwicklung mit dem Android-Kreis installiert und konfiguriert werden. Er bietet einen Überblick über die wichtigsten Features, die in Android-Kreis verfügbar sind, mit Beispiel Quellcode für einige dieser Features.
Es enthält Links zu API-Dokumentation und Android-Entwickler Themen, die Ihnen den Einstieg in das Erstellen von Apps für Android-Kreis erleichtern. Außerdem wurden die wichtigsten Änderungen am Android-Kreis Verhalten hervorgehoben, die sich auf vorhandene apps auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Android 9-Kreis](https://developer.android.com/about/versions/pie/)
