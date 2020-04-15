---
title: Android 10-Features
description: Erfahren Sie, wie Sie Xamarin.Android verwenden, um Apps für Android 10 zu entwickeln.
ms.assetid: B3342772-FB88-4B7F-BC15-8BC78EED749E
author: JonDouglas
ms.author: jodou
ms.date: 09/17/2019
ms.openlocfilehash: c19c9e5bd279824ea2d3e4e9f88857388f786a2c
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73612268"
---
# <a name="android-10-with-xamarin"></a>Android 10 mit Xamarin

_Erfahren Sie, wie Sie Xamarin.Android verwenden, um Apps für Android 10 zu entwickeln._

Android 10 ist jetzt von Google verfügbar. In diesem Release ist eine Reihe neuer Features und APIs verfügbar, und viele davon sind erforderlich, um die neuen Hardwaremöglichkeiten in den neuesten Android-Geräten nutzen zu können.

![Android 10-Logo](~/android/platform/android-10-images/android10_black.png)

Dieser Artikel soll Ihnen den Einstieg in die Entwicklung von Xamarin.Android-Apps für Android 10 erleichtern. Es wird erläutert, wie Sie die notwendigen Updates installieren, das SDK konfigurieren und einen Emulator oder ein Gerät zum Testen vorbereiten. Außerdem finden Sie in diesem Artikel einen Überblick über die neuen Features in Android 10 und einen Beispielquellcode, in dem veranschaulicht wird, wie einige der wichtigsten Android 10-Features verwendet werden.

Xamarin. Android 10.0 bietet Unterstützung für Android 10. Weitere Informationen zur Xamarin.Android-Unterstützung für Android 10 finden Sie in [Versionshinweise zu Xamarin.Android 10](https://docs.microsoft.com/xamarin/android/release-notes/10/10.0).

## <a name="requirements"></a>Anforderungen

Um Android 10-Features in Xamarin-basierten Apps verwenden zu können, müssen folgende Voraussetzungen erfüllt sein:

- **Visual Studio**: Visual Studio 2019 wird empfohlen. Führen Sie unter Windows ein Update auf Visual Studio 2019, Version 16.3 oder höher, durch. Führen Sie unter macOS ein Update auf Visual Studio 2019 für Mac, Version 8.3 oder höher, durch.
- **Xamarin.Android**: Mit Visual Studio muss Xamarin.Android 10.0 oder höher installiert werden (Xamarin.Android wird unter Windows automatisch als Teil der **Mobile Entwicklung mit .NET**-Workload und als Teil des **Visual Studio für Mac-Installationsprogramms** installiert).
- **Java Developer Kit**: Für die Entwicklung mit Xamarin.Android 10.0 ist JDK 8 erforderlich. Die Microsoft-Distribution von OpenJDK wird automatisch als Teil von Visual Studio installiert.
- **Android SDK**: Android SDK API 29 muss über den Android-SDK-Manager installiert werden.

## <a name="get-started"></a>Erste Schritte

Um mit der Entwicklung von Android 10-Apps mit Xamarin.Android zu beginnen, müssen Sie die neuesten Tools und SDK-Pakete herunterladen und installieren, bevor Sie Ihr erstes Android 10-Projekt erstellen können:

1. **Visual Studio 2019 wird empfohlen.** Aktualisieren Sie auf Visual Studio 2019, Version 16.3 oder höher. Wenn Sie mit Visual Studio für Mac 2019 arbeiten, aktualisieren Sie auf Visual Studio 2019 für Mac, Version 8.3 oder höher.
2. Installieren Sie die **Android 10 (API 29)** -Pakete und -Tools über den SDK-Manager.
    - Android 10 (API 29) SDK-Plattform
    - Android 10 (API 29) System Abbild
    - Android SDK Build-Tools 29.0.0+
    - Android SDK Platform-Tools 29.0.0+
    - Android Emulator 29.0.0+
3. Erstellen Sie ein neues Xamarin.Android-Projekt für die Zielplattform Android 10.0.
4. Konfigurieren Sie einen Emulator oder ein Gerät zum Testen von Android 10-Apps.

Jeder dieser Schritte wird nachstehend erläutert:

### <a name="update-visual-studio"></a>Aktualisieren von Visual Studio 2017

Für das Erstellen von Android 10-Apps mit Xamarin wird Visual Studio 2019 empfohlen.

Wenn Sie mit Visual Studio 2019 arbeiten, aktualisieren Sie auf Visual Studio 2019, Version 16.3 oder höher (Anweisungen finden Sie unter [Aktualisieren von Visual Studio 2019 auf die neueste Version](https://docs.microsoft.com/visualstudio/install/update-visual-studio)). Unter macOS aktualisieren Sie auf Visual Studio 2019 für Mac 8.3 oder höher (Anweisungen finden Sie unter [Aktualisieren von Visual Studio für Mac](https://docs.microsoft.com/visualstudio/mac/update)).

### <a name="install-the-android-sdk"></a>Installieren des Android SDK

Um ein Projekt mit Xamarin.Android 10.0 zu erstellen, müssen Sie zunächst den Android-SDK-Manager verwenden, um die SDK-Plattform für **Android 10 (API-Ebene 29)** zu installieren.

1. Starten Sie den SDK-Manager. Klicken Sie in Visual Studio auf **Extras > Android > Android SDK-Manager**. Klicken Sie in Visual Studio für Mac auf **Extras > SDK-Manager**.
2. Klicken Sie in der unteren rechten Ecke auf das Zahnradsymbol, und wählen Sie **Repository > Google (nicht unterstützt)** aus:

    ![Repositoryauswahl für Android-SDK-Manager](~/android/platform/android-10-images/sdkrepository.png)

3. Installieren Sie die **Android 10 SDK-Plattform**-Pakete, die als **Android SDK-Plattform 29** auf der Registerkarte **Plattformen** aufgeführt sind (weitere Informationen zur Verwendung des SDK-Managers finden Sie unter [Einrichten des Android SDK für Xamarin.Android](https://docs.microsoft.com/xamarin/android/get-started/installation/android-sdk)):

    ![Registerkarte „Plattformen“ für Android-SDK-Manager](~/android/platform/android-10-images/sdkplatforms.png)

### <a name="create-a-xamarinandroid-project"></a>Erstellen eines Xamarin.Android-Projekts

Erstellen Sie ein neues Xamarin.Android-Projekt. Wenn Sie mit der Android-Entwicklung mit Xamarin noch nicht vertraut sind, finden Sie unter [Hello, Android (Hallo, Android)](https://docs.microsoft.com/xamarin/android/get-started/hello-android/index) Informationen zum Erstellen von Xamarin.Android-Projekten.

Wenn Sie ein Android-Projekt erstellen, müssen Sie die Versionseinstellungen für Android 10.0 oder höher konfigurieren. Wenn Sie Ihr Projekt beispielsweise auf Android 10 ausrichten, müssen Sie die Android-API-Zielebene Ihres Projekts auf **Android 10.0 (API 29)** festlegen. Dies schließt sowohl Ihre **Zielframeworkversion** als auch Ihre **Zielversion des Android SDK** auf API 29 oder höher ein. Weitere Informationen zum Konfigurieren von Android-API-Ebenen finden Sie unter [Grundlegendes zu Android-API-Ebenen](https://docs.microsoft.com/xamarin/android/app-fundamentals/android-api-levels).

![Xamarin. Android-Zielframework](~/android/platform/android-10-images/targetframework.png)

### <a name="configure-a-device-or-emulator"></a>Konfigurieren eines Geräts oder Emulators

Wenn Sie ein physisches Gerät, z. B. ein Pixel, verwenden, können Sie das Android 10-Update herunterladen, indem Sie in den Einstellungen Ihres Smartphones zu `System` > `System update` > `Check for update` navigieren. Wenn Sie Ihr Gerät flashen möchten, lesen Sie die Anweisungen, wie ein [Factory-Image](https://developers.google.com/android/images) oder [OTA-Image](https://developers.google.com/android/ota) auf Ihrem Gerät geflasht wird.

Wenn Sie einen Emulator verwenden, erstellen Sie ein virtuelles Gerät für API-Ebene 29, und wählen Sie ein x86-basiertes Image aus. Informationen zur Verwendung des Android-Geräte-Managers zum Erstellen und Verwalten virtueller Geräte finden Sie unter [Verwalten virtueller Geräte mit dem Android-Geräte-Manager](https://docs.microsoft.com/xamarin/android/get-started/installation/android-emulator/device-manager). Informationen zum Verwenden des Android-Emulators zum Testen und Debuggen finden Sie unter [Debuggen auf dem Android-Emulator](https://docs.microsoft.com/xamarin/android/deploy-test/debugging/debug-on-emulator).

## <a name="new-features"></a>Neue Funktionen

Mit Android 10 wird eine Reihe neuer Features (Funktionen) eingeführt. Einige dieser neuen Features sind darauf ausgelegt, neue Hardwarefunktionen zu nutzen, die in den neuesten Android-Geräten bereitgestellt werden, während andere dazu dienen, das Android-Benutzererlebnis weiter zu verbessern:

## <a name="enhance-your-app-with-android-10-features-and-apis"></a>Optimieren Ihrer App mit Android 10-Features und -APIs

Wenn Sie soweit sind, können Sie sich im nächsten Schritt mit Android 10 vertraut machen und mehr über die [neuen Features und APIs](https://developer.android.com/preview/api-overview.html)  erfahren, die Sie verwenden können. Im Folgenden finden Sie einige der wichtigsten Features, mit denen Sie beginnen können.

Diese Features werden für jede App empfohlen:

- **Dunkles Design:**  Stellen Sie ein einheitliches Aussehen für Benutzer sicher, die systemweit ein dunkles Design aktivieren. Fügen Sie dazu ein [Dunkles Design](https://developer.android.com/preview/features/darktheme) hinzu, oder aktivieren Sie [Force Dark](https://developer.android.com/preview/features/darktheme#force_dark).

![Dunkles Design](~/android/platform/android-10-images/darktheme.png)

- **Unterstützen Sie [die Navigation mithilfe von Gesten](https://developer.android.com/preview/features/gesturalnav)**  in Ihrer App, indem Sie von Kante-zu-Kante gehen und sicherstellen, dass Ihre benutzerdefinierten Gesten die Gesten für die Systemnavigation ergänzen.

![Gestennavigation](~/android/platform/android-10-images/gesturenavigation.png)

- **Optimieren für faltbare Geräte:**  Stellen Sie nahtlose Kante-zu-Kante-Bedienung auf den heutigen innovativen Geräten bereit, indem Sie [für faltbare Geräte optimieren](https://developer.android.com/preview/features/foldables).

![Faltbares Gerät](~/android/platform/android-10-images/foldable.png)

Diese Features werden empfohlen, wenn Sie für Ihre App relevant sind:

- **Interaktivere Benachrichtigungen:**  Wenn Ihre Benachrichtigungen Meldungen enthalten, aktivieren Sie [vorgeschlagene Antworten und Aktionen in Benachrichtigungen](https://developer.android.com/preview/features#smart-suggestions) , um Benutzer einzubeziehen und ihnen zu ermöglichen, sofort tätig zu werden.
- **Bessere Biometrie:**  Wenn Sie die biometrische Authentifizierung verwenden, wechseln Sie zu [BiometricPrompt](https://developer.android.com/reference/androidx/biometric/BiometricPrompt). Dies ist die bevorzugte Methode, um Fingerabdruckauthentifizierung auf modernen Geräten zu unterstützen.
- **Erweiterte Aufzeichnung:**  Um Untertitelung oder Gameplayaufzeichnung zu unterstützen, aktivieren Sie [Erfassung von Audiowiedergabe (AudioPlaybackCapture)](https://developer.android.com/preview/features/playback-capture). Dies ist eine großartige Möglichkeit, mehr Benutzer zu erreichen und Ihre App einfacher zugänglich zu machen.
- **Bessere Codecs:**  Für Medien-Apps sollten Sie [AV1](https://en.wikipedia.org/wiki/AV1) für Videostreaming und [HDR10 +](https://en.wikipedia.org/wiki/High-dynamic-range_video#HDR10+) für High Dynamic Range Video verwenden. Für Sprach- und Musikstreaming können Sie [Opus](http://opus-codec.org/)-Codierung verwenden, und für Musiker ist eine [native MIDI-API](https://developer.android.com/preview/features/midi) verfügbar.
- **Bessere Netzwerk-APIs:**  Wenn Ihre App IoT-Geräte über WLAN verwaltet, testen Sie die neuen [Netzwerkverbindungs-APIs](https://developer.android.com/preview/features#peer2peer) hinsichtlich Funktionen wie Konfigurieren, Herunterladen oder Drucken.

Dies sind nur einige der vielen neuen Features und APIs in Android 10. Wenn Sie Informationen zu allen wünschen, besuchen Sie die [Android 10-Website für Entwickler](https://developer.android.com/about/versions/10/highlights).

## <a name="behavior-changes"></a>Verhaltensänderungen

Wenn die Android-Zielversion auf API-Ebene 29 festgelegt ist, gibt es mehrere Plattformänderungen, die sich auf das Verhalten Ihrer App auswirken können, und zwar auch dann, wenn Sie die oben beschriebenen neuen Features nicht implementieren. Die folgende Liste stellt eine kurze Zusammenfassung dieser Änderungen dar:

- [Um App-Stabilität und -Kompatibilität sicherzustellen, schränkt die Android-Plattform nun Nicht-SDK-Schnittstellen ein, die Ihre App in Android 10](https://developer.android.com/about/versions/10/behavior-changes-10#non-sdk-restrictions)verwenden kann.
- [Shared Memory hat sich geändert](https://developer.android.com/about/versions/10/behavior-changes-10#shared-memory).
- [Android Runtime und AOT-Richtigkeit](https://developer.android.com/about/versions/10/behavior-changes-10#system-only-oat).
- [Berechtigungen für Vollbild-Intents müssen `USE_FULL_SCREEN_INTENT`](https://developer.android.com/about/versions/10/behavior-changes-10#full-screen-intents) anfordern.
- [Unterstützung für faltbare Geräte](https://developer.android.com/about/versions/10/behavior-changes-10#foldables).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde Android 10 vorgestellt und wurde erläutert, wie die neuesten Tools und Pakete für die Xamarin.Android-Entwicklung mit Android 10 installiert und konfiguriert werden. Es wird eine Übersicht über die wichtigsten Features von Android 10 bereitgestellt. Darüber hinaus werden in diesem Artikel Links zur API-Dokumentation und zu relevanten Themen für Android-Entwickler bereitgestellt, um Ihnen den Einstieg in die App-Entwicklung für Android 10 zu erleichtern. Ferner werden die wichtigsten Verhaltensänderungen von Android 10 beschrieben, die sich auf vorhandene Apps auswirken können.

## <a name="related-links"></a>Verwandte Links

- [Android 10](https://developer.android.com/about/versions/10)
