---
title: Debuggen von Xamarin.Android auf Geräten und in Emulatoren
description: Testen und Debuggen einer Xamarin.Android-App
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: 1ed6ec57365c5d3a861dd3fd947a2ad195ce5357
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935385"
---
# <a name="debugging"></a>Debuggen

In diesem Abschnitt wird beschrieben, wie das Debuggen einer Xamarin.Android-App auf Geräten oder in Emulatoren erfolgt.

## <a name="debugging-overview"></a>Übersicht über das Debugging

Die Entwicklung von Android-Anwendungen erfordert das Ausführen der Anwendung, entweder auf physischer Hardware oder in einem Emulator. Die beste, aber nicht immer zweckmäßigste, Herangehensweise ist die Verwendung von Hardware. In vielen Fällen kann es einfacher und kostengünstiger sein, Android-Hardware mithilfe einer der unten beschriebenen Emulatoren zu simulieren/emulieren.

### <a name="debugging-on-the-android-emulatorandroiddeploy-testdebuggingdebug-on-emulatormd"></a>[Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md)

In diesem Artikel wird erläutert, wie Sie den Android-Emulator in Visual Studio starten und die App auf einem virtuellen Gerät ausführen.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Debuggen auf einem Gerät](~/android/deploy-test/debugging/debug-on-device.md)

In diesem Artikel wird dargestellt, wie Sie ein physisches Android-Gerät so konfigurieren, dass die Xamarin.Android-Anwendung über Visual Studio oder Visual Studio für Mac direkt auf dem Gerät bereitgestellt werden kann.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md)

Ein sehr gängiger Trick, den Entwickler verwenden, um ihre Anwendungen zu debuggen, ist die Verwendung von `Console.WriteLine`. Auf einer mobilen Plattform wie Android steht jedoch keine Konsole zur Verfügung. Android-Geräte stellen ein Protokoll bereit, das Sie beim Schreiben von Apps wahrscheinlich verwenden müssen. Dies wird aufgrund der abrufenden Befehle manchmal als **Logcat** bezeichnet. Dieser Artikel beschreibt die Verwendung von **Logcat**.

> [!WARNING]
> Beachten Sie, dass der **Xamarin Android Player** veraltet ist. Weitere Informationen finden Sie unter der [Ankündigung in diesem Blogbeitrag](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/). Darüber hinaus ist der **Visual Studio-Android-Emulator** seit Visual Studio 2017 als veraltet gekennzeichnet.
