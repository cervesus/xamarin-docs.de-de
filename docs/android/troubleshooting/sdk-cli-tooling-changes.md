---
title: Änderungen an den Tools von Android SDK
description: Änderungen an der Art und Weise, wie die Android SDK die installierten API-Ebenen und AVDS verwaltet.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 897daef3aba1166018a0ac796e9c7956c5f0c711
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70761889"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Änderungen an den Tools von Android SDK

_Änderungen an der Art und Weise, wie die Android SDK die installierten API-Ebenen und AVDS verwaltet._

## <a name="changes-to-android-sdk-tooling"></a>Änderungen an Android SDK Tools

In den letzten Versionen der SDK Tools für Android hat Google die vorhandenen AVD-und SDK-Manager entfernt, um neue CLI-Tools (Befehlszeilenschnittstelle) zu verwenden. Das **Android** -Programm wurde entfernt, und die Google GUI-Manager (grafische Benutzeroberfläche) in Visual Studio für Mac und älteren Versionen von Visual Studio-Tools für xamarin funktionieren nicht mehr in der früheren Version 25.2.5 of Android SDK Tools. Wenn Sie z. b. versuchen, das **Android** -Programm über die Befehlszeile zu verwenden, wird eine Fehlermeldung wie die folgende angezeigt:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

In den folgenden Abschnitten wird erläutert, wie Sie die virtuellen Android SDK-und Android-Geräte mit Android SDK 25.3.0 und höher verwalten.

### <a name="ui-tools"></a>UI-Tools

Visual Studio und Visual Studio für Mac bieten nun xamarin-Ersetzungen für die nicht mehr unterstützten Google GUI-basierten Manager:

- Um Android SDK Tools, Plattformen und andere Komponenten herunterzuladen, die Sie zum Entwickeln von xamarin. Android-Apps benötigen, verwenden Sie den [xamarin Android SDK Manager](~/android/get-started/installation/android-sdk.md) anstelle der älteren Google SDK-Manager.

- Um virtuelle Android-Geräte zu erstellen und zu konfigurieren, verwenden Sie den [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md) anstelle des Legacy-Managers für Google-Emulator.

Diese Tools sind funktionell gleichwertig mit den von Ihnen ersetzten Google GUI-basierten Managern.

### <a name="cli-tools"></a>CLI-Tools

Alternativ können Sie die CLI-Tools verwenden, um die Emulatoren und die Android SDK zu verwalten und zu aktualisieren. Die folgenden Programme bilden nun die Befehlszeilenschnittstelle für die Android SDK Tools:

#### <a name="sdkmanager"></a>sdkmanager

**Hinzugefügt in:** Android SDK Tools 25.2.3 (November, 2016) und höher.

Es gibt ein neues Programm namens **sdkmanager** im Ordner Extras **/bin** Ihrer Android SDK. Dieses Tool wird verwendet, um die Android SDK in der Befehlszeile zu verwalten. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Hinzugefügt in:** Android SDK Tools 25.3.0 (März, 2017) und höher.

Es gibt ein neues Programm mit dem Namen **avdmanager** im Ordner Extras **/bin** Ihrer Android SDK. Mit diesem Tool werden die AVDS für die Android-Emulator verwaltet. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Downgrade

Sie können Ihre **Android SDK Tools** Version herabstufen, indem Sie eine frühere Version des Android SDK von der [Android Developer-Website](https://developer.android.com/studio/index.html)installieren.

### <a name="using-the-old-gui"></a>Verwenden der alten GUI

Sie können die ursprüngliche GUI weiterhin verwenden, indem Sie das **Android** -Programm in Ihrem **Tool** Ordner ausführen, sofern Sie **Android SDK Tools** Version **25.2.5** oder niedriger verwenden.

## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Verstehen von Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
