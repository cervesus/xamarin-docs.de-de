---
title: 'Installieren und Einrichten von Wear OS onxamarin. Android '
description: In diesem Artikel werden die Installationsschritte und Konfigurationsdetails erläutert, die zum Vorbereiten von Computern und Geräten für die Android-Wear-Entwicklung erforderlich sind. Am Ende dieses Artikels verfügen Sie über eine funktionierende xamarin. Android Wear-Installation, die in Visual Studio für Mac und/oder Microsoft Visual Studio integriert ist. Sie sind dann bereit, Ihre erste xamarin. Android Wear-Anwendung zu entwickeln.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: d85c199f6243fc49c1ca924bbd60cfef48b6d91f
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70120006"
---
# <a name="setup-and-installation"></a>Setup und Installation

_In diesem Artikel werden die Installationsschritte und Konfigurationsdetails erläutert, die zum Vorbereiten von Computern und Geräten für die Android-Wear-Entwicklung erforderlich sind. Am Ende dieses Artikels verfügen Sie über eine funktionierende xamarin. Android Wear-Installation, die in Visual Studio für Mac und/oder Microsoft Visual Studio integriert ist. Sie sind dann bereit, Ihre erste xamarin. Android Wear-Anwendung zu entwickeln._

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um xamarin-basierte Android Wear-apps zu erstellen:

- **Visual Studio oder Visual Studio für Mac** &ndash; Visual Studio 2017 Community oder höher ist erforderlich.

- **Xamarin. Android** &ndash; xamarin. Android 4,17 oder höher muss entweder mit Visual Studio oder mit Visual Studio für Mac installiert und konfiguriert werden.

- **Android SDK** -Android SDK 5.0.1 (API 21) oder höher muss über den Android SDK Manager installiert werden.

- **Java Developer Kit** Die xamarin Android-Entwicklung erfordert [JDK 1,8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) , wenn Sie für API-Ebene 24 oder höher entwickeln (JDK 1,8 unterstützt auch API-Ebenen vor 24). &ndash;

Sie können weiterhin [JDK 1,7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) verwenden, wenn Sie speziell für API-Ebene 23 oder früher entwickeln.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

## <a name="installation"></a>Installation

Nachdem Sie xamarin. Android installiert haben, führen Sie die folgenden Schritte aus, sodass Sie zum Erstellen und Testen von Android Wear-apps bereit sind: 

1. Installieren Sie die erforderlichen Android SDK und Tools.
2. Konfigurieren Sie ein Testgerät.
3. Erstellen Sie Ihre erste Android Wear-app.

Diese Schritte werden in den folgenden Abschnitten beschrieben.


### <a name="install-android-sdk-and-tools"></a>Installieren von Android SDK und Tools 

Starten Sie den **Android SDK-Manager**: 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Starten des Android SDK-Managers in Visual Studio](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Starten des Android SDK-Managers in Visual Studio für Mac](installation-images/xs/sdk-menu.png)

-----


Stellen Sie sicher, dass Sie die folgenden Android SDK und Tools installiert haben:

- Android SDK Tools v 24.0.0 oder höher und
- Android 4.4 w (API20) oder
- Android 5.0.1 (API21) oder höher.

Wenn Sie nicht über das aktuellste SDK und die neuesten Tools verfügen, laden Sie die erforderlichen SDK-Tools und die API *-* Bits herunter (möglicherweise müssen Sie &ndash; einen Bildlauf durchführen, um Sie zu finden. die API-Auswahl wird unten angezeigt): 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Beispiel-SDK-Manager-Screenshot der Aktivierung von Android 5.0.1-Komponenten](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Beispiel-SDK-Manager-Bildschirm Abbildung zum Aktivieren von Android 4,4 und 5.0.1](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>Konfiguration

Bevor Sie den Test Ihrer APP verwenden können, müssen Sie einen Android-Wear-Emulator oder ein tatsächliches Android Wear-Gerät konfigurieren. 


### <a name="android-wear-emulator"></a>Android-Wear-Emulator

Bevor Sie einen Android-Wear-Emulator verwenden können, müssen Sie ein Android-Gerät für Android Wear (AVD) mit dem **Google-Emulator-Manager**konfigurieren:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Starten des Android-Emulator-Managers in Visual Studio](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Starten des Android-Emulator-Managers über Visual Studio für Mac](installation-images/xs/emulator-menu.png)

-----

Weitere Informationen zum Einrichten eines Android-Wear-Emulators finden Sie unter [Debuggen von Android Wear in einem Emulator](~/android/wear/deploy-test/debug-on-emulator.md).


### <a name="android-wear-device"></a>Android Wear-Gerät

Wenn Sie über ein Android Wear-Gerät wie z. b. eine Android Wear-Smartwatch verfügen, können Sie die APP auf diesem Gerät Debuggen, anstatt einen Emulator zu verwenden. Informationen zum Entwickeln mit einem Wear-Gerät finden Sie unter [Debuggen auf einem Wear-Gerät](~/android/wear/deploy-test/debug-on-device.md).


## <a name="create-your-first-android-wear-app"></a>Erstellen Ihrer ersten Android Wear-App

Befolgen Sie die Anweisungen [Hello, Wear,](~/android/wear/get-started/hello-wear.md) um Ihre erste Watch-APP zu erstellen.


## <a name="packaging-your-app"></a>Verpacken der APP

Android Wear-Anwendungen werden immer mit einer begleitenden Android Phone-App verteilt. 

Wenn Sie Ihre Android Wear-Anwendung als Verweis auf Ihre Haupt-Android-Anwendung hinzufügen, wird davon ausgegangen, dass es sich um ein Android Wear-Projekt handelt, und es werden alle notwendigen XML-und Metadaten für Sie generiert. Außerdem wird überprüft, ob die Paket-und Versionsnummern stimmen, sodass Sie Ihre apps problemlos an Google Play senden können. 

Weitere Informationen zum Verpacken von Wear-apps finden Sie unter [Arbeiten mit der Paket](~/android/wear/deploy-test/packaging.md)Erstellung.


## <a name="related-links"></a>Verwandte Links

- [Skeletonwear (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-skeletonwear)
