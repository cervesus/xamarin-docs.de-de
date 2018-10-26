---
title: Befehlszeilenemulator
ms.prod: xamarin
ms.assetid: E592AA32-5E83-B7E5-1753-12416551B23C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 513b06749d1616e9fd10f04d22259810c0b4d265
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120722"
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
