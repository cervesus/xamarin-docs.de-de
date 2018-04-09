---
title: Befehlszeilenemulator
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: b1ca1c2b441a9c9ca26de5668f318312bea156a3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="command-line-emulator"></a>Befehlszeilenemulator


## <a name="running-the-android-emulator-from-the-command-line"></a>Ausführen des Android-Emulators über die Befehlszeile

Um das Ausführen des Android-Emulators über die Befehlszeile zu ermöglichen, können Sie das Emulator-Tool verwenden, das vom Android SDK bereitgestellt wird. Dieses Tool kann verwendet werden, um den Emulator über „Terminal“ unter OS X oder über „Eingabeaufforderung“ auf einem Windows-Computer auszuführen.

Um einen bestimmten Android-Emulator zu starten, führen Sie folgenden Befehl aus dem Toolverzeichnis des Android SDK-Speicherorts aus (zum Beispiel C:\android-sdk-windows\tools):

So gehen Sie unter Windows vor:

```cmd
emulator.exe -avd NameOfYourEmulator -partition-size 512
```

Und so unter macOS:

```bash
./emulator -avd NameOfYourEmulator -partition-size 512
```

Damit der Emulator genügend Speicher für die Installation der Xamarin.Android-Plattform zur Verfügung stellen kann, ist die Partitionsgröße notwendig, da die Größe des Emulators in der Regel recht klein ausfällt.

Weitere Informationen zu zusätzlichen Parametern finden Sie auf dieser Android-Website: [http://developer.android.com/guide/developing/tools/emulator.html](http://developer.android.com/guide/developing/tools/emulator.html)
