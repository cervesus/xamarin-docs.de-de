---
title: Änderungen an den Tools von Android SDK
description: Änderungen an der Verwaltung der installierten API-Ebenen und AVDs von Android SDK.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/21/2018
ms.openlocfilehash: 4c559a76d7354fd957d065717ef14d91591d1be0
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73019495"
---
# <a name="changes-to-the-android-sdk-tooling"></a>Änderungen an den Tools von Android SDK

_Änderungen an der Verwaltung der installierten API-Ebenen und AVDs durch das Android SDK._

## <a name="changes-to-android-sdk-tooling"></a>Änderungen an Android SDK Tools

In den neueren Versionen von Android SDK Tools wurden die bestehenden AVD- (virtuelles Android-Gerät) und SDK-Manager durch neue CLI-Tools (Befehlszeilenschnittstelle) von Google ersetzt. Das **Android**-Programm wurde entfernt, und die Manager für die Google-GUI (grafische Benutzeroberfläche) in Visual Studio für Mac und älteren Versionen von Visual Studio-Tools für Xamarin werden ab Version 25.2.5 nicht mehr mit Android SDK Tools funktionieren. Wenn Sie beispielsweise versuchen, das **Android**-Programm über die Befehlszeile zu verwenden, wir eine Fehlermeldung ähnlich der folgenden angezeigt:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

In den folgenden Abschnitten wird die Verwaltung von Android SDK und virtuellen Android-Geräten mithilfe von Android SDK 25.3.0 und höher erläutert.

### <a name="ui-tools"></a>Benutzeroberflächentools

Visual Studio und Visual Studio für Mac bieten nun Xamarin-Ersetzungen für nicht mehr unterstützte Manager, die auf der Google-GUI basieren:

- Verwenden Sie den [Xamarin Android-SDK-Manager](~/android/get-started/installation/android-sdk.md) anstelle des alten Managers für das Google SDK, um Android SDK Tools, Plattformen und andere Komponenten herunterzuladen, die Sie zum Entwickeln von Xamarin.Android-Apps benötigen.

- Verwenden Sie den [Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md) anstelle des alten Google Emulator Managers, um virtuelle Android-Geräte zu erstellen und zu konfigurieren.

Die Funktionen dieser Tools entsprechen den ersetzten Managern, die auf der Google-GUI basieren.

### <a name="cli-tools"></a>CLI-Tools

Alternativ können Sie CLI-Tools verwenden, um Ihre Emulatoren und Android SDK zu verwalten und zu aktualisieren. Die folgenden Programme bilden nun die Befehlszeilenschnittstelle für Android SDK Tools:

#### <a name="sdkmanager"></a>sdkmanager

**Hinzugefügt in:** Android SDK Tools 25.2.3 und höher (November 2016)

Es gibt ein neues Programm namens **sdkmanager** im Ordner **tools/bin** von Android SDK. Dieses Tool dient zur Verwaltung von Android SDK über die Befehlszeile. Weitere Informationen über die Verwendung dieses Tools finden Sie unter [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Hinzugefügt in:** Android SDK Tools 25.3.0 und höher (März 2017)

Es gibt ein neues Programm namens **avdmanager** im Ordner **tools/bin** von Android SDK. Dieses Tool dient zur Verwaltung von AVDs für den Android-Emulator. Weitere Informationen über die Verwendung dieses Tools finden Sie unter [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Herabstufen

Sie können Ihre Version von **Android SDK Tools** herabstufen, indem Sie eine vorherige Version von Android SDK von der [Android Developer-Website](https://developer.android.com/studio/index.html) herunterladen und installieren.

### <a name="using-the-old-gui"></a>Verwenden der alten GUI

Sie können weiterhin die ursprüngliche GUI verwenden, indem Sie das **Android**-Programm in Ihrem **tools**-Ordner ausführen, solange Sie **Android SDK Tools**-Version **25.2.5** oder früher verwenden.

## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [Android-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md)
- [Verstehen von Android API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [Anmerkungen zu dieser Version von SDK Tools (Google)](https://developer.android.com/studio/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/avdmanager.html)
