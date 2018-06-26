---
title: Änderungen an den Tools von Android SDK
description: Änderungen an wie das Android SDK installierten API-Ebenen und AVDs verwaltet.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/21/2018
ms.openlocfilehash: 4e808736fd92fa40ecbf0c24938c0fedd7afcff9
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935450"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Änderungen an den Tools von Android SDK

_Änderungen an wie das Android SDK installierten API-Ebenen und AVDs verwaltet._

## <a name="changes-to-android-sdk-tooling"></a>Änderungen an den Android SDK-Tools

In den neuesten Versionen der SDK-Tools für Android ist Google die vorhandenen AVD und SDK-Manager zugunsten von neuen CLI (Command Line Interface)-Tools entfernt. Die **android** Programm wurde entfernt und die Google-Benutzeroberfläche (Graphical User Interface)-Manager in Visual Studio für Mac und früheren Versionen von Visual Studio-Tools für Xamarin früheren Version 25.2.5 Android SDK-Tools nicht mehr funktionieren. Beispielsweise möchten, verwenden Sie die **android** Programm über die Befehlszeile führt dazu, eine Fehlermeldung wie folgt:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

In den folgenden Abschnitten wird erläutert, wie die Android-SDK und Android virtuellen Geräten mit Android SDK 25.3.0 zu verwalten und höher.

### <a name="ui-tools"></a>UI-Tools

Visual Studio und Visual Studio für Mac bieten jetzt Xamarin Ersatz für die nicht mehr unterstützte Google-GUI-basierte-Manager:

-   Verwenden Sie zum Herunterladen von Android SDK-Tools, Plattformen und anderen Komponenten, die Sie benötigen für die Entwicklung von apps Xamarin.Android, die [Xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) anstelle der älteren Google SDK Manager.

-   Verwenden Sie zum Erstellen und Konfigurieren von virtuellen Android-Geräte, die [Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md) anstelle der älteren Google Emulator-Manager.

Diese Tools sind funktionell gleichwertig mit der Google-GUI-basierte Managern, die sie ersetzen.

### <a name="cli-tools"></a>CLI-Tools

Alternativ können Sie CLI-Tools verwenden, verwalten und-Emulatoren und Android-SDK zu aktualisieren. Die folgenden Programme besteht nun die Befehlszeilenschnittstelle für den Android SDK-Tools:

#### <a name="sdkmanager"></a>sdkmanager

**Hinzugefügt In:** Android SDK-Tools 25.2.3 (November 2016) und höher.

Es wird ein neues Programm aufgerufen **Sdkmanager** in der **Tools/Bin** Ordner Ihres Android SDK. Dieses Tool wird verwendet, um die Android-SDK in der Befehlszeile beizubehalten. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [Sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Hinzugefügt In:** Android SDK-Tools 25.3.0 (März 2017) und höher.

Es wird ein neues Programm aufgerufen **Avdmanager** in der **Tools/Bin** Ordner Ihres Android SDK. Dieses Tool wird verwendet, um die AVDs für den Android-Emulator beizubehalten. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Ausführen eines Downgrades für

Können Sie ein downgrade Ihrer **Android SDK-Tools** Version durch die Installation von einer früheren Version von Android SDK aus der [Android Developer-Website](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Mithilfe der alten GUI

Sie können weiterhin die ursprünglichen GUI verwenden, durch Ausführen der **android** Programmieren in Ihre **Tools** Ordner solange Sie sich befinden **Android SDK-Tools** Version **25.2.5**  oder niedriger.


## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Verstehen von Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
