---
title: Android 10-Features
description: Erfahren Sie mehr über das Entwickeln von Apps für Android 10 mit xamarin. Android.
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: df9fa43d2071d273104edafbe6b880a97afb3f96
ms.sourcegitcommit: e354aabfb39598e0ce11115db3e6bcebb9f68338
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2019
ms.locfileid: "72273136"
---
# <a name="android-10-with-xamarin"></a>Android 10 mit xamarin

_Erfahren Sie mehr über das Entwickeln von Apps für Android 10 mit xamarin. Android._

Android 10 ist jetzt über Google verfügbar. In dieser Version werden eine Reihe neuer Features und APIs zur Verfügung gestellt, und viele davon sind erforderlich, um neue Hardwarefunktionen in den neuesten Android-Geräten nutzen zu können.

![Android 10-Logo](~/android/platform/android-10-images/android10_black.png)

Dieser Artikel ist für den Einstieg in die Entwicklung von xamarin. Android-Apps für Android 10 strukturiert. Darin wird erläutert, wie Sie die erforderlichen Updates installieren, das SDK konfigurieren und einen Emulator oder ein Gerät für Tests vorbereiten. Außerdem finden Sie hier einen Überblick über die neuen Features in Android 10 und einen Beispiel Quell Code, der veranschaulicht, wie einige der wichtigsten Android 10-Features verwendet werden.

Xamarin. Android 10,0 bietet Unterstützung für Android 10. Weitere Informationen zur xamarin. Android-Unterstützung für Android 10 finden Sie in den Anmerkungen zu dieser [Version von xamarin. Android 10,0](https://docs.microsoft.com/xamarin/android/release-notes/10/10.0).

## <a name="requirements"></a>Anforderungen

Die folgende Liste ist erforderlich, um Android 10-Features in xamarin-basierten apps zu verwenden:

- **Visual Studio** : Visual Studio 2019 wird empfohlen. Bei Windows Update auf Visual Studio 2019, Version 16,3 oder höher. Aktualisieren Sie unter macOS auf Visual Studio 2019 für Mac, Version 8,3 oder höher.
- **Xamarin. Android** -xamarin. Android 10,0 oder höher muss mit Visual Studio installiert werden (xamarin. Android wird automatisch als Teil der Mobile- **Entwicklung mit .net** -Arbeitsauslastung unter Windows installiert und als Teil von **Visual Studio installiert. für Mac-Installer**)
- **Java Developer Kit** : die Entwicklung von xamarin. Android 10,0 erfordert JDK 8. Die Microsoft-Distribution von openjdk wird automatisch als Teil von Visual Studio installiert.
- **Android SDK** Android SDK API 29 muss über den Android SDK Manager installiert werden.

## <a name="get-started"></a>Erste Schritte

Um mit der Entwicklung von Android 10-apps mit xamarin. Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie Ihr erstes Android 10-Projekt erstellen können:

1. **Visual Studio 2019 wird empfohlen**. Aktualisieren Sie auf Visual Studio 2019, Version 16,3 oder höher. Wenn Sie Visual Studio für Mac 2019 verwenden, aktualisieren Sie auf Visual Studio 2019 für Mac, Version 8,3 oder höher.
2. Installieren Sie Pakete und Tools für **Android 10 (API 29)** über den SDK-Manager.
    - Android 10 (API 29) SDK-Plattform
    - Android 10 (API 29)-System Abbild
    - Android SDK Build-Tools 29.0.0 +
    - Android SDK Platform-Tools 29.0.0 +
    - Android-Emulator 29.0.0 +
3. Erstellen Sie ein neues xamarin. Android-Projekt, das auf Android 10,0 abzielt.
4. Konfigurieren Sie einen Emulator oder ein Gerät zum Testen von Android 10-apps.

Jeder dieser Schritte wird im folgenden erläutert:

### <a name="update-visual-studio"></a>Aktualisieren von Visual Studio 2017

Visual Studio 2019 wird zum Entwickeln von Android 10-apps mit xamarin empfohlen.

Wenn Sie Visual Studio 2019 verwenden, aktualisieren Sie auf Visual Studio 2019 Version 16,3 oder höher (Anweisungen dazu finden Sie unter [Aktualisieren von Visual Studio 2019 auf die neueste Version](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). Aktualisieren Sie unter macOS auf Visual Studio 2019 für Mac 8,3 oder höher (Anweisungen finden Sie unter [Aktualisieren von Visual Studio 2019 für Mac auf die neueste Version](https://docs.microsoft.com/en-us/visualstudio/mac/update)).

### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Zum Erstellen eines Projekts mit xamarin. Android 10,0 müssen Sie zunächst den Android SDK-Manager verwenden, um die SDK-Plattform für **Android 10 (API-Ebene 29)** zu installieren.

1. Starten Sie den SDK-Manager. Klicken Sie in Visual Studio auf Extras **> Android-> Android SDK-Manager.** Klicken Sie in Visual Studio für Mac auf Extras **> SDK-Manager.**
2. Klicken Sie in der unteren rechten Ecke auf das Zahnrad Symbol, und wählen Sie **Repository > Google (nicht unterstützt):**

    ![Android SDK Manager-Repository-Auswahl](~/android/platform/android-10-images/sdkrepository.png)

3. Installieren Sie die **Android 10 SDK Platform** -Pakete, die auf der Registerkarte **Plattformen** als **Android SDK Platform 29** aufgeführt sind. (Weitere Informationen zur Verwendung des SDK-Managers finden Sie unter [Android SDK Setup](https://docs.microsoft.com/en-us/xamarin/android/get-started/installation/android-sdk).):

    ![Registerkarte "Android SDK Manager"](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Erstellen eines xamarin. Android-Projekts

Erstellen Sie ein neues xamarin. Android-Projekt. Wenn Sie mit der Android-Entwicklung mit xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android](https://docs.microsoft.com/en-us/xamarin/android/get-started/hello-android/index) Weitere Informationen zum Erstellen von xamarin. Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versions Einstellungen so konfigurieren, dass Sie Android 10,0 oder höher als Ziel haben. Wenn Sie z. b. Ihr Projekt für Android 10 als Ziel festlegen möchten, müssen Sie die Android-API-Ziel Ebene Ihres Projekts auf **Android 10,0 (API 29)** konfigurieren. Dies schließt sowohl die **Zielframeworkversion** als auch die **Ziel Android SDK Version** zu API 29 oder höher ein. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie Untergrund Legendes zu [Android-API-Ebenen.](https://docs.microsoft.com/en-us/xamarin/android/app-fundamentals/android-api-levels)

![Xamarin. Android-Ziel Framework](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>Konfigurieren eines Geräts oder Emulators

Wenn Sie ein physisches Gerät, z. b. ein Pixel, verwenden, können Sie das Android 10-Update herunterladen, indem Sie in den Einstellungen Ihres Telefons zum `System` @ no__t-1 @ no__t-2 @ no__t-3 @ no__t-4 wechseln. Wenn Sie Ihr Gerät blinken möchten, finden Sie die Anweisungen unter blinken eines [factoryimages](https://developers.google.com/android/images) oder eines [OTA-Images](https://developers.google.com/android/ota) auf Ihrem Gerät.

Wenn Sie einen Emulator verwenden, erstellen Sie ein virtuelles Gerät für API-Ebene 29, und wählen Sie ein x86-basiertes Abbild aus. Informationen zur Verwendung der Android Device Manager zum Erstellen und Verwalten von virtuellen Geräten finden Sie unter [Verwalten von virtuellen Geräten mit dem Android Device Manager.](https://docs.microsoft.com/en-us/xamarin/android/get-started/installation/android-emulator/device-manager) Weitere Informationen zum Verwenden der Android-Emulator zum Testen und Debuggen finden Sie unter [Debugging auf der Android-Emulator.](https://docs.microsoft.com/en-us/xamarin/android/deploy-test/debugging/debug-on-emulator)

## <a name="new-features"></a>Neue Funktionen

Android 10 bietet eine Reihe von neuen Features. Einige dieser neuen Features sind darauf ausgelegt, neue Hardwarefunktionen zu nutzen, die von den neuesten Android-Geräten angeboten werden, während andere die Benutzer Funktionalität von Android weiter verbessern können:

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Optimieren Sie Ihre APP mit Android 10-Features und-APIs

Wenn Sie fertig sind, können Sie sich mit Android 10 vertraut machen und sich über die [neuen Features und APIs](https://developer.android.com/preview/api-overview.html) informieren, die Sie verwenden können. Im folgenden finden Sie einige der wichtigsten Features, mit denen Sie beginnen können.

Diese Features werden für jede APP empfohlen:

- **Dunkles Design:**  Sorgen Sie für Benutzer, die ein systemweites dunkles Design aktivieren, indem Sie ein [dunkles](https://developer.android.com/preview/features/darktheme)Design  or Aktivieren von " [dunkel](https://developer.android.com/preview/features/darktheme#force_dark)" aktivieren.

![Dunkles Design](~/android/platform/android-10-images/darktheme.png)

- **Unterstützen Sie die [Gestural-Navigation](https://developer.android.com/preview/features/gesturalnav)**  in Ihrer APP, indem Sie von Edge zu Edge wechseln und sicherstellen, dass Ihre benutzerdefinierten Gesten die System Navigations Gesten ergänzen.

![Gesten Navigation](~/android/platform/android-10-images/gesturenavigation.png)

- **Optimieren Sie für foldables:**  Sorgen Sie für eine nahtlose Edge-to-Edge-Umgebung auf heutigen innovativen Geräten, indem Sie die [Optimierung für foldables](https://developer.android.com/preview/features/foldables)durchführen.

![Reduzierbar](~/android/platform/android-10-images/foldable.png)

Diese Features werden empfohlen, wenn Sie für Ihre APP relevant sind:

- **Weitere interaktive Benachrichtigungen:**  Wenn Ihre Benachrichtigungen Meldungen enthalten, aktivieren Sie [vorgeschlagene Antworten und Aktionen in Benachrichtigungen](https://developer.android.com/preview/features#smart-suggestions) to einbinden von Benutzern, sodass Sie Sofortmaßnahmen ergreifen können.
- **Bessere Biometrik:**  Wenn Sie die biometrische Authentifizierung verwenden, wechseln Sie zu " [biometricprompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt)", die bevorzugte Methode zur Unterstützung der Fingerabdruckauthentifizierung auf modernen Geräten.
- Erweiterte **Aufzeichnung:**  Um Untertitel-oder Spiel Aufzeichnung zu unterstützen, aktivieren Sie die [Erfassung von Audiowiedergabe](https://developer.android.com/preview/features/playback-capture). Dies ist eine großartige Möglichkeit, mehr Benutzer zu erreichen und Ihre APP zugänglicher zu machen.
- **Bessere Codecs:**  Für Media apps, Try [AV1](https://en.wikipedia.org/wiki/AV1) For Video Streaming und [HDR10 +](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) for High Dynamic Range Video. Für Sprach-und Musikstreaming können Sie die [Opus](http://opus-codec.org/) -Codierung verwenden, und für Musiker ist eine native  - [API](https://developer.android.com/preview/features/midi)verfügbar.
- **Bessere Netzwerk-APIs:**  Wenn Ihre APP IOT-Geräte über Wi-Fi verwaltet, testen Sie die neuen [Netzwerkverbindungs-APIs](https://developer.android.com/preview/features#peer2peer) For-Funktionen wie konfigurieren, herunterladen oder drucken.

Dies sind nur einige der vielen neuen Features und APIs in Android 10. Um alle anzuzeigen, besuchen Sie die [Android 10-Website für Entwickler](https://developer.android.com/about/versions/10/highlights).

## <a name="behavior-changes"></a>Verhaltensänderungen

Wenn die Android-Ziel Version auf API-Ebene 29 festgelegt ist, gibt es mehrere Platt Formänderungen, die sich auf das Verhalten Ihrer APP auswirken, auch wenn Sie die oben beschriebenen neuen Features nicht implementieren. Die folgende Liste stellt eine kurze Zusammenfassung dieser Änderungen dar:

- [Um die APP-Stabilität und Kompatibilität zu gewährleisten, schränkt die Android-Plattform nun nicht-SDK-Schnittstellen ein, die Ihre APP in Android 10 verwenden kann](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions)
- [Shared Memory hat sich geändert](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory).
- [Android Runtime & AOT-Richtigkeit](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat).
- Die [Berechtigungen für Vollbild Intents müssen `USE_FULL_SCREEN_INTENT` anfordern](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents).
- [Unterstützung für foldables](https://developer.android.com/about/versions/10/behavior-changes-10#foldables).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android 10 eingeführt und erläutert, wie die neuesten Tools und Pakete für die xamarin. Android-Entwicklung mit Android 10 installiert und konfiguriert werden. Es wurde eine Übersicht über die wichtigsten Features von Android 10 bereitgestellt. Es enthält Links zu API-Dokumentation und Android-Entwickler Themen, die Ihnen den Einstieg in das Erstellen von Apps für Android 10 erleichtern. Außerdem wurden die wichtigsten Änderungen am Android 10-Verhalten hervorgehoben, die sich auf vorhandene apps auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Android 10](https://developer.android.com/about/versions/10)
