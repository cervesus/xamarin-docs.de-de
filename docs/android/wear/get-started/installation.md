---
title: 'Installieren und Einrichten von Wear OS onXamarin.Android '
description: Dieser Artikel führt durch die Schritte zur Installation und Konfigurationsdetails, die zum Vorbereiten der Computer und Geräte für Android Wear-Entwicklung erforderlich. Klicken Sie am Ende dieses Artikels müssen Sie eine funktionierende Xamarin.Android-Wear-Installation in Visual Studio für Mac bzw. oder Microsoft Visual Studio integriert, und Sie werden damit beginnen, Ihre erste Xamarin.Android-Wear-Anwendung erstellen.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/25/2018
ms.openlocfilehash: 0bae98a204ba3478834894d6c093259a8b2139b2
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827716"
---
# <a name="setup-and-installation"></a>Setup und Installation

_Dieser Artikel führt durch die Schritte zur Installation und Konfigurationsdetails, die zum Vorbereiten der Computer und Geräte für Android Wear-Entwicklung erforderlich. Klicken Sie am Ende dieses Artikels müssen Sie eine funktionierende Xamarin.Android-Wear-Installation in Visual Studio für Mac bzw. oder Microsoft Visual Studio integriert, und Sie werden damit beginnen, Ihre erste Xamarin.Android-Wear-Anwendung erstellen._

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um die Xamarin-basierte Android Wear-apps zu erstellen:

-   **Visual Studio oder Visual Studio für Mac** &ndash; Visual Studio 2017 Community oder höher ist erforderlich.

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-   **Android SDK** -Android SDK 5.0.1 (API 21) oder höher muss installiert sein über den Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin Android-Entwicklung erfordert [JDK 1.8](https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) Wenn Sie die Entwicklung für API-Ebene 24 oder höher sind (JDK 1.8 unterstützt auch API-Ebenen älter als 24).

Sie können weiterhin verwenden [JDK 1.7](https://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Wenn Sie die Entwicklung von speziell für die API-Ebene 23 oder früher sind.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

## <a name="installation"></a>Installation

Nachdem Sie Xamarin.Android installiert haben, führen Sie die folgenden Schritte aus, damit Sie zum Erstellen und Testen von apps für Android Wear bereit sind: 

1.  Installieren Sie die erforderlichen Android-SDK und Tools.
2.  Konfigurieren Sie ein Testgerät.
3.  Erstellen Sie Ihrer erste Android Wear-app.

Diese Schritte werden in den folgenden Abschnitten beschrieben.


### <a name="install-android-sdk-and-tools"></a>Installieren von Android SDK und tools 

Starten Sie den **Android SDK-Manager**: 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Gewusst wie: Starten des Android-SDK-Managers in Visual Studio](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Gewusst wie: Starten Sie den Android SDK-Manager in Visual Studio für Mac](installation-images/xs/sdk-menu.png)

-----


Stellen Sie sicher, dass Sie die folgenden Android SDK und Tools installiert haben:

* Android SDK Tools V 24.0.0 oder höher und
* Android 4.4W (API20), oder
* Android 5.0.1 (API21) oder höher.

Wenn Sie nicht die neuesten SDK und Tools installiert haben, laden Sie die erforderliche SDK-Tools *und* die API-Bits (müssen Sie möglicherweise ein wenig später wiederfinden Scrollen &ndash; die API-Auswahl wird unten gezeigt): 

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Screenshot von Beispiel-SDK-Manager der Aktivierung von Android 5.0.1 Komponenten](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Screenshot von Beispiel-SDK-Manager der Aktivierung von Android 4.4 und 5.0.1 Komponenten](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>Konfiguration

Vor der Verwendung Ihrer app testen, müssen Sie ein Android Wear-Emulator oder auf einem echten Android Wear-Gerät konfigurieren. 


### <a name="android-wear-emulator"></a>Emulator für Android Wear

Bevor Sie einen Emulator Android Wear verwenden können, müssen Sie konfigurieren, einer Android Wear Android Virtual Device (AVD) mithilfe der **Google Emulator Manager**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![Gewusst wie: Starten des Android-Emulator-Managers aus Visual Studio](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

![Gewusst wie: Starten Sie den Android-Emulator-Manager von Visual Studio für Mac](installation-images/xs/emulator-menu.png)

-----

Weitere Informationen zum Einrichten eines Android Wear-Emulators finden Sie unter [Android Wear in einem Emulator Debuggen](~/android/wear/deploy-test/debug-on-emulator.md).


### <a name="android-wear-device"></a>Android Wear-Gerät

Wenn Sie ein Android Wear-Gerät, z.B. einer Android Wear Smartwatch verfügen, können Sie die app auf diesem Gerät anstatt mit einem Simulator Debuggen. Weitere Informationen zur Entwicklung mit einem Wear-Gerät finden Sie unter [Debuggen auf einem Wear-Gerät](~/android/wear/deploy-test/debug-on-device.md).


## <a name="create-your-first-android-wear-app"></a>Erstellen Sie Ihrer ersten Android Wear-App

Führen Sie die [Hallo Wear](~/android/wear/get-started/hello-wear.md) Anweisungen, um Ihre erste Watch-app erstellen.


## <a name="packaging-your-app"></a>Verpacken der App

Android Wear-Anwendungen werden immer mit einer Begleit-Android-Telefon-app verteilt. 

Wenn Sie Ihre Android Wear-Anwendung als einen Verweis auf Ihre wichtigsten Android-Anwendung hinzufügen wird automatisch als ein Android Wear-Projekt und alle erforderlichen XML- und -Metadaten für Sie generiert. Darüber hinaus wird es überprüft, dass das Paket und die Versionsnummern übereinstimmen, damit Sie einfach Ihre apps in Google Play senden können. 

Weitere Informationen zum Verpacken von Wear-apps finden Sie unter [arbeiten mit der Paketerstellung](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Verwandte Links

- [SkeletonWear (Beispiel)](https://developer.xamarin.com/samples/monodroid/wear/SkeletonWear/)
