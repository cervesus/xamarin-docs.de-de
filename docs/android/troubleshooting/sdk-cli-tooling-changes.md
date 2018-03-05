---
title: "Änderungen an den Tools von Android SDK"
description: "Änderungen an wie das Android SDK installierten API-Ebenen und AVDs verwaltet."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5AC61C00-0FF6-4C2D-80E7-D67A3EE30A5A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/08/2018
ms.openlocfilehash: 69e9f08870a01c056951700978d07277af5edfa8
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="changes-to-the-android-sdk-tooling"></a>Änderungen an den Tools von Android SDK

_Änderungen an wie das Android SDK installierten API-Ebenen und AVDs verwaltet._

## <a name="changes-to--android-sdk-tooling"></a>Änderungen an den Android SDK-Tools

In modernen der SDK-Tools für Android-Versionen wurde Google entfernt, die vorhandenen AVD und SDK-Manager für neue _Befehlszeilenschnittstelle_ (CLI)-Tools. Die erste **android** Programm wurde entfernt und die grafische Benutzeroberfläche (Graphical User Interface)-Manager in Visual Studio für Mac und früheren Versionen von Xamarin für Visual Studio hinter Version Android SDK-Tools nicht mehr funktionieren.


![Android-IDE-Menü in Visual Studio](sdk-cli-tooling-changes-images/android-ide-menu.png)

Beim Verwenden der **android** Programm über die Befehlszeile führt dazu, eine Fehlermeldung ähnlich der folgenden:

```shell
The "android" command is deprecated.
For manual SDK, AVD, and project management, please use Android Studio.
For command-line tools, use tools\bin\sdkmanager.bat
and tools\bin\avdmanager.bat
```

Daher müssen Sie mithilfe der CLI-Tools zum Verwalten und aktualisieren Sie die Emulatoren und Android-SDK.

### <a name="cli-tools"></a>CLI-Tools

Die folgenden Programme besteht nun die Befehlszeilenschnittstelle für den Android SDK-Tools:

#### <a name="sdkmanager"></a>sdkmanager

**Hinzugefügt In:** Android SDK-Tools 25.2.3 (November 2016) und höher.

Es wird ein neues Programm aufgerufen **Sdkmanager** in der **Tools/Bin** Ordner Ihres Android SDK. Dieses Tool wird verwendet, um die Android-SDK in der Befehlszeile beizubehalten. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [Sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html).

#### <a name="avdmanager"></a>avdmanager

**Hinzugefügt In:** Android SDK-Tools 25.3.0 (März 2017) und höher.

Es wird ein neues Programm aufgerufen **Avdmanager** in der **Tools/Bin** Ordner Ihres Android SDK. Dieses Tool wird verwendet, um die AVD für Android-Emulators für Google beizubehalten. Weitere Informationen zur Verwendung dieses Tools finden Sie unter [Avdmanager](https://developer.android.com/studio/command-line/avdmanager.html).

### <a name="downgrading"></a>Ausführen eines Downgrades für

Können Sie ein downgrade Ihrer **Android SDK-Tools** Version durch die Installation von einer früheren Version von Android SDK aus der [Android Developer-Website](https://developer.android.com/studio/index.html).

### <a name="using-the-old-gui"></a>Mithilfe der alten GUI

Sie können weiterhin die ursprünglichen GUI verwenden, durch Ausführen der **android** Programmieren in Ihre **Tools** Ordner solange Sie sich befinden **Android SDK-Tools** Version **25.2.5**  oder niedriger.


## <a name="related-links"></a>Verwandte Links

- [Android SDK-Setup](~/android/get-started/installation/android-sdk.md)
- [Grundlegendes zu Android-API-Ebenen](~/android/app-fundamentals/android-api-levels.md)
- [Anmerkungen zu dieser Version (Google) SDK-Tools](https://developer.android.com/studiohttps://developer.xamarin.com/releases/sdk-tools.html)
- [sdkmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
- [avdmanager](https://developer.android.com/studio/command-line/sdkmanager.html)
