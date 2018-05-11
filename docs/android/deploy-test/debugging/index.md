---
title: Debuggen von Xamarin.Android auf Geräten und in Emulatoren
description: Testen und Debuggen einer Xamarin.Android-App
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: a4ce36e28bd5b6dcf78d248b1f2ba951cad9b286
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="debugging"></a>Debuggen

In diesem Abschnitt wird beschrieben, wie das Debuggen einer Xamarin.Android-App auf Geräten oder in Emulatoren erfolgt.
## <a name="debugging-overview"></a>Übersicht über das Debugging

Die Entwicklung von Android-Anwendungen erfordert das Ausführen der Anwendung, entweder auf physischer Hardware oder in einem Emulator. Die beste, aber nicht immer zweckmäßigste, Herangehensweise ist die Verwendung von Hardware. In vielen Fällen kann es einfacher und kostengünstiger sein, Android-Hardware mithilfe einer der unten beschriebenen Emulatoren zu simulieren/emulieren.


### <a name="google-android-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Google Android-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

In diesen Artikeln wird erläutert, wie der Standardemulator verwendet wird, der mit Android SDK bereitgestellt wird. Dieser Emulator ist für Visual Studio für Windows und Visual Studio für Mac verfügbar.

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Android-Emulator für Visual Studio](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

In diesem Artikel wird das Debuggen und Testen einer Xamarin.Android-Anwendung mit dem Android-Emulator erläutert, der in Visual Studio 2015 integriert ist. Dieser Emulator ist eine gute Wahl, wenn Sie Visual Studio 2015 verwenden und keine benutzerdefinierten Geräteprofile nötig sind.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Debuggen auf einem Gerät](~/android/deploy-test/debugging/debug-on-device.md)

In diesem Artikel wird dargestellt, wie Sie ein physisches Android-Gerät so konfigurieren, dass die Xamarin.Android-Anwendung über Visual Studio oder Visual Studio für Mac direkt auf dem Gerät bereitgestellt werden kann.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Android-Debugprotokoll](~/android/deploy-test/debugging/android-debug-log.md)

Ein sehr gängiger Trick, den Entwickler verwenden, um ihre Anwendungen zu debuggen, ist die Verwendung von `Console.WriteLine`. Auf einer mobilen Plattform wie Android steht jedoch keine Konsole zur Verfügung. Android-Geräte stellen ein Protokoll bereit, das Sie beim Schreiben von Apps wahrscheinlich verwenden müssen. Dies wird aufgrund der abrufenden Befehle manchmal als **Logcat** bezeichnet. Dieser Artikel beschreibt die Verwendung von **Logcat**.

> [!WARNING]
> Beachten Sie, dass der **Xamarin Android Player** veraltet ist. Weitere Informationen finden Sie unter der [Ankündigung in diesem Blogbeitrag](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
