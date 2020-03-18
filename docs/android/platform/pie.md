---
title: Android 9 Pie
description: Erfahren Sie, wie Sie Xamarin.Android verwenden, um Apps für Android 9 Pie zu entwickeln.
ms.prod: xamarin
ms.assetid: 6575DD32-9DC8-44E6-85EF-1F8BD07D3780
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/21/2018
ms.openlocfilehash: 0105b43116df697bc6688becb77298c236dfa601
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73019887"
---
# <a name="android-pie-features"></a>Features von Android Pie

_Erfahren Sie, wie Sie Xamarin.Android verwenden, um Apps für Android 9 Pie zu entwickeln._

[Android 9 Pie](https://developer.android.com/about/versions/pie/) ist jetzt von Google verfügbar. In diesem Release ist eine Reihe neuer Features und APIs verfügbar, und viele davon sind erforderlich, um die neuen Hardwaremöglichkeiten der neuesten Android-Geräte nutzen zu können.

![Herobild von Android Pie](pie-images/01-android-p-logo.png)

Dieser Artikel soll Ihnen den Einstieg in die Entwicklung von Xamarin.Android-Apps für Android Pie erleichtern. Es wird erläutert, wie Sie die notwendigen Updates installieren, das SDK konfigurieren und zum Testen einen Emulator oder ein Gerät vorbereiten. Außerdem finden Sie in diesem Artikel einen Überblick über die neuen Features in Android Pie und Beispielquellcode, in dem veranschaulicht wird, wie einige der wichtigsten Android Pie-Features verwendet werden.

Xamarin.Android 9.0 bietet Unterstützung für Android Pie. Weitere Informationen zur Xamarin.Android-Unterstützung für Android Pie finden Sie in [Versionshinweise zu Android P Developer Preview 3](https://docs.microsoft.com/xamarin/android/release-notes/9/9.0/#android-p-dp1).

## <a name="requirements"></a>Anforderungen

Um Android Pie-Features in Xamarin-basierten Apps zu verwenden, müssen folgende Voraussetzungen erfüllt sein:

- **Visual Studio**: Visual Studio 2019 wird empfohlen.
    Wenn Sie Visual Studio 2017 unter Windows verwenden, aktualisieren Sie auf Visual Studio 2017, Version 15.8 oder höher. Führen Sie unter macOS ein Update auf Visual Studio 2017 für Mac, Version 7.6 oder höher, durch.

- **Xamarin.Android**: Xamarin.Android 9.0.0.17 oder höher muss mit Visual Studio installiert sein (Xamarin.Android wird automatisch als Teil der Workload **Mobile-Entwicklung mit .NET** installiert).

- **Java Developer Kit**: Die Entwicklung für Xamarin Android 9.0 erfordert [JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (oder Sie können die Vorschauversion der Microsoft-Distribution von [OpenJDK](~/android/get-started/installation/openjdk.md) ausprobieren). JDK 8 wird als Teil der Workload **Mobile-Entwicklung mit .NET** automatisch installiert.

- **Android SDK**: Die Android SDK -API 28 oder höher muss über den Android-SDK-Manager installiert werden.

## <a name="getting-started"></a>Erste Schritte

Um mit der Entwicklung von Android Pie-Apps mit Xamarin.Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie Ihr erstes Android Pie-Projekt erstellen können:

1. Visual Studio 2019 wird empfohlen. Wenn Sie Visual Studio 2017 verwenden, aktualisieren Sie auf [Visual Studio 2017, Version 15.8](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) oder höher. Wenn Sie mit Visual Studio für Mac arbeiten, aktualisieren Sie auf [Visual Studio 2017 für Mac, Version 7.6](https://docs.microsoft.com/visualstudio/releasenotes/vs2017-relnotes) oder höher.

2. Installieren Sie die Pakete und Tools von **Android Pie (API 28)** über den SDK-Manager.

3. Erstellen Sie ein neues Xamarin.Android-Projekt für die Zielplattform **Android 9.0**.

4. Konfigurieren Sie einen Emulator oder ein Gerät zum Testen von Android Pie-Apps.

Jeder dieser Schritte wird in den folgenden Abschnitten erläutert:

### <a name="update-visual-studio"></a>Aktualisieren von Visual Studio 2017

Für das Erstellen von Android Pie-Apps mit Xamarin wird Visual Studio 2019 empfohlen.

Wenn Sie mit Visual Studio 2017 arbeiten, aktualisieren Sie auf Visual Studio 2017, Version 15.8 oder höher (Anweisungen finden Sie unter [Aktualisieren von Visual Studio 2017 auf die neueste Version](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). Unter macOS aktualisieren Sie auf Visual Studio 2017 für Mac 7.6 oder höher (Anweisungen finden Sie unter [Einrichten und Installieren von Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/installation)).

### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Um ein Projekt mit Xamarin.Android 9.0 zu erstellen, müssen Sie zunächst den Android-SDK-Manager verwenden, um die SDK-Plattform für **Android Pie (API-Ebene 28)** oder höher zu installieren.

1. Starten Sie den SDK-Manager. Klicken Sie in Visual Studio auf **Extras > Android > Android-SDK-Manager**. Klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**.

2. Klicken Sie rechts unten auf das Zahnradsymbol, und wählen Sie **Repository > Google (nicht unterstützt)** aus:

    [![Festlegen des Repositorys auf Google](pie-images/vs/set-repo-sml.png)](pie-images/vs/set-repo.png#lightbox)

3. Installieren Sie die **Android Pie** SDK-Pakete, die als **Android SDK-Plattform 28** auf der Registerkarte **Plattformen** aufgeführt sind (weitere Informationen zur Verwendung des SDK-Managers finden Sie unter [Einrichten des Android SDK für Xamarin.Android](~/android/get-started/installation/android-sdk.md)):

    [![Installieren von Android Pie-Paketen](pie-images/vs/sdk-manager-sml.png)](pie-images/vs/sdk-manager.png#lightbox)

4. Wenn Sie einen Emulator verwenden, erstellen Sie ein virtuelles Gerät mit Unterstützung der **API-Ebene 28**. Weitere Informationen zum Erstellen und Anpassen von virtuellen Geräten finden Sie unter [Verwalten virtueller Geräte mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).

### <a name="start-a-xamarinandroid-project"></a>Starten eines Xamarin.Android-Projekts

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie mit der Android-Entwicklung mit Xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android (Hallo, Android)](~/android/get-started/hello-android/index.md) Informationen zum Erstellen von Xamarin.Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versionseinstellungen für Android 9.0 oder höher konfigurieren. Wenn Sie Ihr Projekt beispielsweise auf Android Pie ausrichten, müssen Sie die Android-API-Zielebene Ihres Projekts auf **Android 9.0** (API 28) festlegen. Es wird empfohlen, außerdem die Ebene für das Zielframework auf API 28 oder höher festzulegen. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md).

### <a name="configure-a-device-or-emulator"></a>Konfigurieren eines Geräts oder Emulators

Wenn Sie ein physisches Gerät wie Nexus oder Pixel verwenden, können Sie Ihr Gerät auf Android Pie aktualisieren, indem Sie die Anweisungen unter [Factory Images for Nexus and Pixel Devices](https://developers.google.com/android/images) (Werkseitige Images für Nexus- und Pixel-Geräte) befolgen.

Wenn Sie einen Emulator verwenden, erstellen Sie ein virtuelles Gerät für API-Ebene 28, und wählen Sie ein x86-basiertes Image aus. Informationen zur Verwendung des Android-Geräte-Managers zum Erstellen und Verwalten virtueller Geräte finden Sie unter [Verwalten virtueller Geräte mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md).
Informationen zum Verwenden des Android-Emulators zum Testen und Debuggen finden Sie unter [Debuggen auf dem Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md).

## <a name="new-features"></a>Neue Funktionen

Mit Android Pie wird eine Reihe neuer Features eingeführt. Einige dieser neuen Features sind darauf ausgelegt, neue Hardwarefunktionen zu nutzen, die in den neuesten Android-Geräten bereitgestellt werden, während andere dazu dienen, das Android-Benutzererlebnis weiter zu verbessern:

- **Unterstützung von Displayaussparungen**: Bietet APIs zum Auffinden von Position und Form der _Aussparung_ am oberen Bildschirmrand bei neueren Android-Geräten.

- **Verbesserte Benachrichtigungen**: In Benachrichtigungen können nun Bilder angezeigt werden, und eine neue `Person`-Klasse wird verwendet, um die Teilnahme an Unterhaltungen zu vereinfachen.

- **Positionsbestimmung in Gebäuden**: Plattformunterstützung für das Protokoll WiFi Round-Trip-Time, das es Apps ermöglicht, WLAN-Geräte für die Navigation durch Gebäude zu verwenden.

- **Unterstützung mehrerer Kameras**: Es besteht die Möglichkeit, gleichzeitig auf Datenströme mehrerer physischer Kameras (z. B. zweier Front- und zweier Rückkameras) zuzugreifen.

Diese Features werden in den folgenden Abschnitten näher beleuchtet, und es werden kurze Codebeispiele für die ersten Schritte mit diesen Features in Ihrer App bereitgestellt.

### <a name="display-cutout-support"></a>Unterstützung von Displayaussparungen

Viele neuere Android-Geräte mit randlosen Bildschirmen haben eine *Displayaussparung* (oder „Kerbe“) am oberen Rand des Displays für Kamera und Lautsprecher.
Der folgende Screenshot zeigt ein Emulatorbeispiel einer Aussparung:

[![Android-Emulator zur Simulation einer Aussparung](pie-images/02-example-cutout-sml.png)](pie-images/02-example-cutout.png#lightbox)

Um zu verwalten, wie Ihr App-Fenster seinen Inhalt auf Geräten mit Displayaussparung anzeigt, wurde Android Pie das neue Fensterlayoutattribut [LayoutInDisplayCutoutMode](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#layoutInDisplayCutoutMode) hinzugefügt. Dieses Attribut kann auf einen der folgenden Werte festgelegt werden:

- [LayoutInDisplayCutoutModeNever](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_NEVER): Das Fenster darf nie den Aussparungsbereich überlappen.

- [LayoutInDisplayCutoutModeShortEdges](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_SHORT_EDGES): Das Fenster darf bis in den Aussparungsbereich hineinreichen, aber nur an den kurzen Bildschirmrändern. 

- [LayoutInDisplayCutoutModeDefault](https://developer.android.com/reference/android/view/WindowManager.LayoutParams.html#LAYOUT_IN_DISPLAY_CUTOUT_MODE_DEFAULT): Das Fenster darf bis in den Aussparungsbereich hineinreichen, wenn sich die Aussparung in einer Systemleiste befindet.

Um beispielsweise zu verhindern, dass das Anwendungsfenster den Aussparungsbereich überlappt, legen Sie den Layoutaussparungsmodus auf *never* fest: 

```csharp
Window.Attributes.LayoutInDisplayCutoutMode =
    Android.Views.LayoutInDisplayCutoutMode.Never;
```

Die folgenden Beispiele zeigen Beispiele für diese Aussparungsmodi. Der erste Screenshot auf der linken Seite zeigt die App im Nicht-Vollbildmodus. Im mittleren Screenshot wechselt die App in den Vollbildmodus, wobei `LayoutInDisplayCutoutMode` auf `LayoutInDisplayCutoutModeShortEdges` festgelegt ist. Beachten Sie, dass der weiße Hintergrund der App bis in den Aussparungsbereich des Displays reicht:

[![Beispiel für Displayaussparungsmodi im Emulator](pie-images/03-cutout-modes-sml.png)](pie-images/03-cutout-modes.png#lightbox)

Im letzten Screenshot (rechts oben) ist `LayoutInDisplayCutoutMode` auf `LayoutInDisplayCutoutModeShortNever` festgelegt, bevor der Wechsel zum Vollbildschirm erfolgt.
Beachten Sie, dass der weiße Hintergrund der App nicht in den Aussparungsbereich des Displays hineinreichen darf.

Wenn Sie detailliertere Informationen zum Aussparungsbereich auf dem Gerät benötigen, können Sie die neue Klasse [DisplayCutout](https://developer.android.com/reference/android/view/DisplayCutout.html) verwenden. `DisplayCutout` stellt den Bereich des Displays dar, der nicht zum Anzeigen von Inhalt verwendet werden darf. Sie können anhand dieser Informationen die Position und Form der Aussparung abrufen, damit Ihre App nicht versucht, Inhalte in diesem funktionslosen Bereich anzuzeigen.

Weitere Informationen über die neuen Aussparungsfeatures in Android Pie finden Sie unter [Display cutout support](https://developer.android.com/about/versions/pie/android-9.0#cutout) (Unterstützung von Displayaussparungen).

### <a name="notifications-enhancements"></a>Verbesserungen an Benachrichtigungen

Android Pie führt die folgenden Verbesserungen ein, um das Benachrichtigungserlebnis zu verbessern:

- In [Android Oreo](~/android/platform/oreo.md) eingeführte Benachrichtigungskanäle unterstützen nun die Blockierung von Kanalgruppen.

- Das Benachrichtigungssystem bietet drei neue Kategorien zur Vermeidung von Störungen (Priorisierung von Alarmen, Systemtönen und Medienquellen). Darüber hinaus gibt es sieben neue Modi zur Vermeidung von Störungen, die zur Unterdrückung visueller Unterbrechungen verwendet werden können (wie z. B. Badges, Benachrichtigungsleuchten, Anzeige der Statusleiste und Start von Aktivitäten im Vollbildmodus).

- Die neue Klasse [Person](https://developer.android.com/reference/android/app/Person.html) wurde hinzugefügt, um den Absender einer Nachricht darzustellen. Die Verwendung dieser Klasse trägt dazu bei, das Rendern jeder Benachrichtigung zu optimieren, indem die an einer Unterhaltung beteiligten Personen (einschließlich ihrer Avatare und URIs) identifiziert werden.

- In Benachrichtigungen können jetzt Bilder angezeigt werden. 

Das folgende Beispiel veranschaulicht, wie die neuen APIs verwendet werden, um eine Benachrichtigung zu generieren, die ein Bild enthält. In den folgenden Screenshots wird eine Textbenachrichtigung gepostet, gefolgt von einer Benachrichtigung mit einem eingebetteten Bild. Wenn die Benachrichtigungen erweitert werden (wie rechts zu sehen), wird der Text der ersten Benachrichtigung angezeigt und das in die zweite Benachrichtigung eingebettete Bild vergrößert:

[![Beispielbenachrichtigung mit Bild](pie-images/04-example-notifications-sml.png)](pie-images/04-example-notifications.png#lightbox)

Das folgende Beispiel veranschaulicht das Einbinden eines Bilds in eine Android Pie-Benachrichtigung und demonstriert die Verwendung der neuen `Person`-Klasse:

1. Erstellen Sie ein `Person`-Objekt, das den Absender darstellt. Beispielsweise sind der Name und das Symbol des Absenders in `fromPerson` enthalten:

    ```csharp
    Icon senderIcon = Icon.CreateWithResource(this, Resource.Drawable.sender_icon);
    Person fromPerson = new Person.Builder()
        .SetIcon(senderIcon)
        .SetName("Mark Sender")
        .Build();
    ```

2. Erstellen Sie eine `Notification.MessagingStyle.Message`, die das zu sendende Bild enthält, und übergeben Sie das Bild an die neue Methode [Notification.MessagingStyle.Message.SetData](https://developer.android.com/reference/android/app/Notification.MessagingStyle.Message.html#setData%28java.lang.String,%20android.net.Uri).
   Zum Beispiel:

    ```csharp
    Uri imageUri = Uri.Parse("android.resource://com.xamarin.pminidemo/drawable/example_image");
    Notification.MessagingStyle.Message message = new Notification.MessagingStyle
            .Message("Here's a picture of where I'm currently standing", 0, fromPerson)
            .SetData("image/", imageUri);
    ```

3. Fügen Sie die Nachricht einem `Notification.MessagingStyle`-Objekt hinzu. Zum Beispiel:

    ```csharp
    Notification.MessagingStyle style = new Notification.MessagingStyle(fromPerson)
            .AddMessage(message);
    ```

4. Binden Sie diesen Stil in den Benachrichtigungs-Generator ein. Zum Beispiel:

    ```csharp
    builder = new Notification.Builder(this, MY_CHANNEL)
        .SetContentTitle("Tour of the Colosseum")
        .SetContentText("I'm standing right here!")
        .SetSmallIcon(Resource.Mipmap.ic_notification)
        .SetStyle(style)
        .SetChannelId(MY_CHANNEL);
    ```

5. Veröffentlichen Sie die Benachrichtigung. Zum Beispiel:

    ```csharp
    const int notificationId = 1000;
    notificationManager.Notify(notificationId, builder.Build());
    ```

Weitere Informationen zum Erstellen von Benachrichtigungen finden Sie unter [Lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

### <a name="indoor-positioning"></a>Positionsbestimmung in Gebäuden

Android Pie bietet Unterstützung für IEEE 802.11mc (auch bekannt als _WiFi Round-Trip-Time_ oder _WiFi RTT_), was es Apps ermöglicht, die Entfernung zu einem oder mehreren WLAN-Zugangspunkten zu ermitteln. Anhand dieser Informationen ist es Ihrer App möglich, die *Positionsbestimmung in Gebäuden* mit einer Genauigkeit von ein bis zwei Metern zu nutzen. Auf Android-Geräten, die Hardwareunterstützung für IEEE 801.11mc bieten, kann Ihre App Navigationsfunktionen wie die standortbasierte Steuerung intelligenter Geräte oder Navigationsanweisungen für Geschäftsräume bieten:

[![Beispiel der Navigation in Gebäuden mithilfe von WiFi RTT](pie-images/05-wifi-rtt-sml.png)](pie-images/05-wifi-rtt.png#lightbox)

Die neue [WifiRttManager](https://developer.android.com/reference/android/net/wifi/rtt/WifiRttManager)-Klasse und mehrere Hilfsklassen bieten die Möglichkeit, die Entfernung zu WLAN-Geräten zu messen. Weitere Informationen über die in Android Pie eingeführten APIs zur Positionsbestimmung in Gebäuden finden Sie unter [Android.Net.Wifi.Rtt](https://developer.android.com/reference/android/net/wifi/rtt/package-summary).

### <a name="multi-camera-support"></a>Unterstützung mehrerer Kameras

Viele neuere Android-Geräte verfügen über zwei Front- und/oder zwei Rückkameras, die für Funktionen wie Stereosehen, erweiterte visuelle Effekte und verbesserte Zoomfunktionen nützlich sind. Android Pie führt eine neue [Multi-Camera](https://developer.android.com/about/versions/pie/android-9.0#camera)-API ein, die es Ihrer App ermöglicht, eine *logische Kamera* (oder *logische Multikamera*) zu verwenden, die von zwei oder mehr physischen Kameras unterstützt wird.
Um festzustellen, ob das Gerät eine logische Multikamera unterstützt, können Sie sich die Fähigkeiten der einzelnen Kameras auf dem Gerät ansehen, um festzustellen, ob sie [RequestAvailableCapabilitiesLogicalMultiCamera](https://developer.android.com/reference/android/hardware/camera2/CameraMetadata#REQUEST_AVAILABLE_CAPABILITIES_LOGICAL_MULTI_CAMERA) unterstützen.

Android Pie bietet auch die neue Klasse [SessionConfiguration](https://developer.android.com/reference/android/hardware/camera2/params/SessionConfiguration.html), mit der Verzögerungen bei der Erstaufnahme reduziert werden können und das Starten und Beenden des Kameradatenstroms überflüssig wird.

Weitere Informationen zur Unterstützung mehrerer Kameras in Android Pie finden Sie unter [Multi-camera support and camera updates](https://developer.android.com/about/versions/pie/android-9.0#camera) (Unterstützung von mehreren Kameras und Kameraaktualisierungen).

### <a name="other-features"></a>Weitere Funktionen

Außerdem unterstützt Android Pie eine Reihe weiterer neuer Features:

- Die neue [AnimatedImageDrawable](https://developer.android.com/reference/android/graphics/drawable/AnimatedImageDrawable.html)-Klasse, die zum Zeichnen und Anzeigen animierter Bilder verwendet werden kann.

- Eine neue [ImageDecoder](https://developer.android.com/reference/android/graphics/ImageDecoder.html)-Klasse, die `BitmapFactory` ersetzt. `ImageDecoder` kann verwendet werden, um `AnimatedImageDrawable` zu decodieren.

- Unterstützung für HDR-Video (High Dynamic Range) und HEIF-Bilder (High Efficiency Image File Format).

- [JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html) wurde dahingehend verbessert, dass netzwerkbezogene Aufträge intelligenter erledigt werden können. Die neue [GetNetwork](https://developer.android.com/reference/android/app/job/JobParameters#getNetwork%28%29)-Methode der [JobParameters](https://developer.android.com/reference/android/app/job/JobParameters)-Klasse gibt das beste Netzwerk für die Erfüllung beliebiger Netzwerkanforderungen für einen bestimmten Auftrag zurück.

Weitere Informationen über die neuesten Android Pie-Funktionen finden Sie unter [Android 9 features and APIs](https://developer.android.com/about/versions/pie/android-9.0) (Android 9-Features und -APIs).

## <a name="behavior-changes"></a>Verhaltensänderungen

Wenn die Android-Zielversion auf API-Ebene 28 festgelegt ist, gibt es mehrere Plattformänderungen, die sich auf das Verhalten Ihrer App auswirken können, und zwar auch dann, wenn Sie die oben beschriebenen neuen Features nicht implementieren. Die folgende Liste stellt eine kurze Zusammenfassung dieser Änderungen dar:

- Apps müssen nun vor der Nutzung von Diensten, die im Vordergrund stehen, eine Genehmigung für die Nutzung von Diensten im Vordergrund anfordern.

- Wenn Ihre App mehr als einen Prozess aufweist, kann sie nicht ein einzelnes [WebView](xref:Android.Webkit.WebView)-Datenverzeichnis prozessübergreifend gemeinsam nutzen lassen.

- Der direkte Zugriff auf das Datenverzeichnis einer anderen App per Pfad ist nicht mehr erlaubt.

Weitere Informationen zu Verhaltensänderungen bei Apps für Android Pie finden Sie unter [Behavior Changes](https://developer.android.com/about/versions/pie/android-9.0-changes-all#p-apps) (Verhaltensänderungen).

## <a name="sample-code"></a>Beispielcode

[AndroidPMiniDemo](https://github.com/xamarin/monodroid-samples/tree/master/android-p/AndroidPMiniDemo) ist eine Xamarin.Android-Beispiel-App für Android Pie, die zeigt, wie Displayaussparungsmodi festgelegt werden, die neue `Person`-Klasse verwendet wird und eine Benachrichtigung mit einem Bild gesendet wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android Pie vorgestellt und erläutert, wie die neuesten Tools und Pakete für die Xamarin.Android-Entwicklung mit Android Pie installiert und konfiguriert werden. Es wurde ein Überblick der wichtigsten Android Pie-Features mit Links zu Beispielquellcode für verschiedene dieser neuen Features gegeben.
Darüber hinaus wurden in diesem Artikel Links zur API-Dokumentation und zu relevanten Themen für Android-Entwickler bereitgestellt, um Ihnen den Einstieg in die App-Entwicklung für Android Pie zu erleichtern. Ferner wurden die wichtigsten Verhaltensänderungen von Android Pie beschrieben, die sich auf vorhandene Apps auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Android 9 Pie](https://developer.android.com/about/versions/pie/)
