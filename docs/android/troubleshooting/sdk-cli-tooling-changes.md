---
title: Änderungen an den Tools von Android SDK
description: Änderungen an, wie das Android SDK installierten-API-Ebenen und AVDs verwaltet.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: dbd3287e7c646be7fd969699eab685906a1c6c1a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112805"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Änderungen an den Tools von Android SDK

_Änderungen an, wie das Android SDK installierten-API-Ebenen und AVDs verwaltet._

## <a name="changes-to-android-sdk-tooling"></a>Änderungen an den Android SDK-Tools

In früheren Versionen von der SDK Tools für Android hat Google die bestehende AVD und SDK-Manager zugunsten von neuen CLI (Command Line Interface)-Tools entfernt. Die **android** Programm wurde entfernt und die Google-GUI (Graphical User Interface)-Manager in Visual Studio für Mac und früheren Versionen von Visual Studio-Tools für Xamarin funktioniert nicht mehr nach der Version Android SDK Tools 25.2.5. Beispielsweise möchten, verwenden Sie die **android** Programm über die Befehlszeile führt zu einer Fehlermeldung wie folgt:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

In den folgenden Abschnitten wird erläutert, wie das Android SDK und das virtuelle Android-Geräte mit Android SDK 25.3.0 verwalten und höher.

### <a name="ui-tools"></a>UI-Tools

Visual Studio und Visual Studio für Mac bieten jetzt die Xamarin-Ersatz für die nicht mehr unterstützte Google-GUI-basierte-Manager:

-   Verwenden Sie zum Herunterladen der Android SDK Tools, Plattformen und andere Komponenten, die Sie für die Entwicklung von Xamarin.Android-apps müssen die [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) anstelle von den älteren Google SDK Manager.

-   Verwenden Sie zum Erstellen und Konfigurieren von virtuellen Android-Geräte, die [Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md) anstelle von den älteren Google Emulator Manager.

Diese Tools sind funktionell gleichwertig mit der Google-GUI-basierte Managern, die sie ersetzen.

### <a name="cli-tools"></a>CLI-Tools

Alternativ können Sie CLI-Tools zum Verwalten und aktualisieren Sie die Emulatoren und Android-SDK verwenden. Die folgenden Programme besteht nun die Befehlszeilenschnittstelle für die Android SDK-Tools:

#### <a name="sdkmanager"></a>sdkmanager

**Hinzugefügt In:** Android SDK Tools 25.2.3 (November 2016) und höher.

Es gibt ein neues Programm namens **Sdkmanager** in die **Tools/Bin** Ordner Ihres Android-SDK. Dieses Tool wird verwendet, um das Android-SDK in der Befehlszeile zu verwalten. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [Sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Hinzugefügt In:** Android SDK Tools 25.3.0 (März 2017) und höher.

Es gibt ein neues Programm namens **Avdmanager** in die **Tools/Bin** Ordner Ihres Android-SDK. Dieses Tool wird verwendet, um die AVDs für Android-Emulator zu gewährleisten. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Ein Downgrade

Können Sie ein downgrade Ihrer **Android SDK Tools** Version durch die Installation von einer früheren Version von Android SDK aus der [Android-Entwicklerwebsite](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Verwenden die alte GUI

Sie können weiterhin die ursprüngliche grafische Benutzeroberfläche verwenden, mit der **android** programmieren Sie in Ihrer **Tools** so lange, wie Sie auf Ordner **Android SDK Tools** Version **25.2.5**  oder niedriger.


## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Verstehen von Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
