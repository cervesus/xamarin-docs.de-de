---
title: Debuggen von Android Abnutzung in einem Emulator
description: Dieser Artikel wird erläutert, wie Debuggen einer Anwendung Xamarin.Android Abnutzung in einem Emulator.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 3dda1445815f747f484458388cd856c2f9705603
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="debug-android-wear-on-an-emulator"></a>Debuggen von Android Abnutzung in einem Emulator

_Dieser Artikel wird erläutert, wie Debuggen einer Anwendung Xamarin.Android Abnutzung in einem Emulator._

## <a name="debug-wear-on-emulator-overview"></a>Debuggen von Abnutzung auf Emulator (Übersicht)

Anwendungsentwicklung Android Dach erfordert die Anwendung ausführen, entweder auf physischer Hardware oder einem Emulator oder Simulator verwenden. Die beste, aber nicht immer zweckmäßigste, Herangehensweise ist die Verwendung von Hardware. In vielen Fällen kann es einfacher und kostengünstiger simulieren/Android Dach Hardware verwenden einen Emulator aus, wie unten beschrieben zu emulieren. Wenn Sie noch nicht vertraut sind, mit der Prozess der Bereitstellung und Ausführung von Dach Android-apps finden Sie unter [Hallo, Abnutzung](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-google-android-emulator"></a>Konfigurieren des Google Android-Emulators

Um Ihre Abnutzung-app in einem Emulator auszuführen, müssen Sie installieren den Android SDK-Android-Emulator und für Android Dach zu konfigurieren. Allgemeine Android SDK-Emulator Installations- und Konfigurationsschritte Informationen finden Sie unter [Google Android-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md).

Wenn Sie ein Abnutzung virtuelles Gerät erstellen, wählen Sie ein Android Dach Geräteprofil (z. B. **Android Abnutzung Quadrat**). Verwenden Sie zum Verbessern der Leistung der Abnutzung **X86** CPU/ABI wie in diesem Beispiel dargestellt:

[![Beispielkonfiguration für Abnutzung virtuelles Gerät](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Starten Sie den virtuellen Abnutzung-Gerät 

Nachdem Sie ein virtuelles Gerät mit Android Dach erstellt haben, können Sie es aus dem Pulldownmenü Gerät in der IDE auswählen, bevor Sie das Debuggen starten. Wenn Ihr virtuelle Gerät nicht in der Pulldownliste Gerät verfügbar ist, stellen Sie sicher, dass das Projekt ein Android ist *Dach* app-Projekt (nicht mit einem Android-app-Projekt) und dass ihr Ziel API-Ebene, auf die gleiche API festgelegt ist-Ebene als das virtuelle Gerät. Zum Beispiel:

[![Auswählen einer Dach AVD in Visual Studio-Gerät-Menü](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Nachdem die Android-Emulator gestartet wurde, wird Xamarin.Android Abnutzung-app auf dem Emulator bereitzustellen. Der Emulator führt die App mit dem Image des konfigurierten virtuellen Geräts aus.

Nicht überrascht, wenn dies sein (oder einem anderen interstitial Bildschirm) auf den ersten. Der Emulator überwachen kann dauern gestartet wird: 

![Sehen Sie sich, dass der Emulator nur eine Minute zeigt...](debug-on-emulator-images/please-wait.png)

Beim Ausführen der App muss der Emulator nicht jedes Mal beendet und anschließend wieder neu gestartet werden. Stattdessen kann dieser permanent ausgeführt werden.

 
## <a name="summary"></a>Zusammenfassung
 
Diese Anleitung wird erläutert, wie zum Konfigurieren von Google Android-Emulator für die Entwicklung von Abnutzung und ein Abnutzung virtuelles Gerät für das Debuggen zu starten.
