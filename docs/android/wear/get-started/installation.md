---
title: 'Installieren und Einrichten von Dach OS onXamarin.Android '
description: Dieser Artikel führt Sie durch die Installationsschritte und Konfigurationsdetails, die zum Vorbereiten der Computer und Geräte für Android Dach Entwicklung erforderlich. Am Ende dieses Artikels stehen Ihnen eine funktionierende Xamarin.Android Abnutzung-Installation in Visual Studio für Mac und/oder Microsoft Visual Studio integriert und verlaufen beim Erstellen Ihrer ersten Xamarin.Android Abnutzung-Anwendung bereit.
ms.prod: xamarin
ms.assetid: 3BB395FA-0545-4024-A18F-98CF5E9CA55F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: 14162663c518fdd1324f2b0340592fbae491d112
ms.sourcegitcommit: 4b0582a0f06598f3ff8ad5b817946459fed3c42a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/03/2018
ms.locfileid: "32436504"
---
# <a name="setup-and-installation"></a>Setup und Installation

_Dieser Artikel führt Sie durch die Installationsschritte und Konfigurationsdetails, die zum Vorbereiten der Computer und Geräte für Android Dach Entwicklung erforderlich. Am Ende dieses Artikels stehen Ihnen eine funktionierende Xamarin.Android Abnutzung-Installation in Visual Studio für Mac und/oder Microsoft Visual Studio integriert und verlaufen beim Erstellen Ihrer ersten Xamarin.Android Abnutzung-Anwendung bereit._

## <a name="requirements"></a>Anforderungen

Folgendes ist erforderlich, um Android Dach Xamarin-basierten apps zu erstellen:

-   **Visual Studio oder Visual Studio für Mac** &ndash; Sie Wenn Sie Visual verwenden, Visual Studio 2015 Professional Studio oder höher erforderlich ist.

-   **Xamarin.Android** &ndash; Xamarin.Android 4.17 or later must be installed and configured with either Visual Studio or Visual Studio for Mac.

-   **Android SDK** -Android-SDK 5.0.1 (API 21) oder höher muss installiert sein über den Android SDK Manager.

-   **Java Developer Kit** &ndash; Xamarin Android-Entwicklung erforderlich [JDK 1.8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) Domänenmodus für API-Ebene 24 entwickeln oder größer (JDK 1.8 unterstützt auch API-Ebenen älter als 24).

Sie können weiterhin [JDK 1.7](http://www.oracle.com/technetwork/java/javase/downloads/jdk7-downloads-1880260.html) Domänenmodus speziell für API-Ebene 23 entwickeln oder eine frühere Version.

> [!IMPORTANT]
> Xamarin.Android unterstützt JDK 9 nicht.

## <a name="installation"></a>Installation

Nach der Installation von Xamarin.Android führen Sie die folgenden Schritte aus, damit Sie soweit sind, erstellen und Testen von apps für Android Dach: 

1.  Installieren Sie die erforderlichen Android-SDK und Tools.
2.  Ein Testgerät zu konfigurieren.
3.  Erstellen Sie Ihrer erste app für Android Dach an.

Diese Schritte werden in den folgenden Abschnitten beschrieben.


### <a name="install-android-sdk-and-tools"></a>Android-SDK und-Tools installieren 

Starten Sie die **Android SDK Manager**: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Gewusst wie: Starten Sie den Android SDK-Manager in Visual Studio](installation-images/vs/sdk-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Gewusst wie: Starten Sie den Android SDK-Manager in Visual Studio für Mac](installation-images/xs/sdk-menu.png)

-----


Stellen Sie sicher, dass Sie die folgenden Android-SDK und Tools installiert haben:

* Android SDK-Tools V 24.0.0 oder höher, und
* Android 4.4W (API20), oder
* Android 5.0.1 (API21) oder höher.

Wenn Sie nicht die neuesten SDK und Tools installiert haben, laden Sie die erforderliche SDK-Tools *und* die API-Bits (möglicherweise müssen Sie ein wenig finden sie einen Bildlauf &ndash; die API-Auswahl wird unten gezeigt): 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Beispiel-SDK-Manager-Screenshot aktivieren Android 5.0.1 Komponenten](installation-images/vs/sdk-select.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Screenshot der Beispiel-SDK-Manager aktivieren Android 4.4 und 5.0.1 Komponenten](installation-images/xs/sdk-select.png)

-----


## <a name="configuration"></a>Konfiguration

Vor der Verwendung die app zu testen, müssen Sie einen Emulator Android Dach oder ein echtes Dach Android-Gerät konfigurieren. 


### <a name="android-wear-emulator"></a>Abnutzung Android-Emulator

Bevor Sie einen Emulator Android Dach verwenden können, müssen Sie konfigurieren, ein Android Dach Android Virtual Device (AVD) mithilfe der **Google-Emulator-Manager**:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Gewusst wie: Starten Sie den Android Emulator Manager aus Visual Studio](installation-images/vs/emulator-menu.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

![Gewusst wie: Starten Sie den Android-Emulator-Manager von Visual Studio für Mac](installation-images/xs/emulator-menu.png)

-----

Weitere Informationen zum Einrichten eines Dach Android-Emulators finden Sie unter [Debuggen Android Dach in einem Emulator](~/android/wear/deploy-test/debug-on-emulator.md).


### <a name="android-wear-device"></a>Abnutzung Android-Gerät

Wenn Sie ein Gerät Android Dach, z. B. ein Android Dach Smartwatch verfügen, können die app auf diesem Gerät statt mit einem Simulator zu debuggen. Informationen zum Entwickeln von mit einem Gerät Abnutzung finden Sie unter [Debuggen auf einem Gerät Dach](~/android/wear/deploy-test/debug-on-device.md).


## <a name="create-your-first-android-wear-app"></a>Erstellen Sie Ihrer ersten App für Android Abnutzung

Führen Sie die [Hallo, Abnutzung](~/android/wear/get-started/hello-wear.md) Anweisungen zum Erstellen Ihrer ersten Watch-app.


## <a name="packaging-your-app"></a>Verpacken der App

Android Abnutzung Anwendungen werden immer mit einer Begleit-Android-Telefon-app verteilt. 

Wenn Sie Ihre Anwendung Android Dach als Verweis auf die wichtigsten Android-Anwendung hinzufügen wird automatisch als angenommen ein Dach Android-Projekt und alle erforderlichen XML- und Metadaten für Sie generiert. Darüber hinaus überprüft er, dass das Paket und die Versionsnummern übereinstimmen, damit Sie Ihre apps leicht zu Google Play senden können. 

Weitere Informationen zum Verpacken Abnutzung-apps finden Sie unter [arbeiten mit Verpackung](~/android/wear/deploy-test/packaging.md).


## <a name="related-links"></a>Verwandte Links

- [SkeletonWear (Beispiel)](https://developer.xamarin.com/samples/SkeletonWear/)
