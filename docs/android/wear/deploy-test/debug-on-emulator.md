---
title: Debuggen von Android Wear in einem Emulator
description: In diesen Artikeln wird erläutert, wie eine xamarin. Android Wear-Anwendung in einem Emulator debuggt wird.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: f085aaffbedb2965222b98a22cf6a4bb2393642b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70764039"
---
# <a name="debug-android-wear-on-an-emulator"></a>Debuggen von Android Wear in einem Emulator

_In diesen Artikeln wird erläutert, wie eine xamarin. Android Wear-Anwendung in einem Emulator debuggt wird._

## <a name="debug-wear-on-emulator-overview"></a>Übersicht über Debug Wear on Emulator

Die Entwicklung von Android Wear-Anwendungen erfordert die Ausführung der Anwendung, entweder auf physischer Hardware oder mithilfe eines Emulators oder Simulators. Die beste, aber nicht immer zweckmäßigste, Herangehensweise ist die Verwendung von Hardware. In vielen Fällen kann es einfacher und kostengünstiger sein, Android Wear-Hardware mithilfe eines Emulators zu simulieren bzw. zu emulieren, wie unten beschrieben. Wenn Sie noch nicht mit dem Prozess der Bereitstellung und Ausführung von Android Wear-apps vertraut sind, finden Sie weitere Informationen unter [Hello, Wear](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-android-emulator"></a>Konfigurieren des Android-Emulator

Um Ihre Wear-App auf einem Emulator auszuführen, müssen Sie die Android SDK Android-Emulator installieren und für Android Wear konfigurieren. Allgemeine Android SDK Emulator-Installations-und Konfigurationsinformationen finden Sie unter [Android-Emulator Setup](~/android/get-started/installation/android-emulator/index.md).

Wenn Sie ein virtuelles Wear-Gerät erstellen, wählen Sie ein Android Wear-Geräte Profil aus (z. b. **Android Wear Square**). Um die Leistung zu verbessern, verwenden Sie das Wear **x86** -CPU/ABI, wie in diesem Beispiel gezeigt:

[![Beispiel für die Konfiguration virtueller Geräte](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)

## <a name="launch-the-wear-virtual-device"></a>Virtuelles Wear-Gerät starten 

Nachdem Sie ein virtuelles Android Wear-Gerät erstellt haben, können Sie es aus dem Pulldownmenü des Geräts in der IDE auswählen, bevor Sie mit dem Debuggen beginnen. Wenn Ihr virtuelles Gerät nicht im pulldowngerät des Geräts verfügbar ist, überprüfen Sie, ob es sich bei dem Projekt um ein Android *Wear* -App-Projekt (kein Android-App-Projekt) handelt und ob die Ziel-API-Ebene auf dieselbe API-Ebene wie das virtuelle Gerät festgelegt ist. Beispiel:

[![Auswählen von "Wear AVD" im Visual Studio-Geräte Menü](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Nachdem der Android-Emulator gestartet wurde, stellt xamarin. Android die Wear-App im Emulator bereit. Der Emulator führt die App mit dem Image des konfigurierten virtuellen Geräts aus.

Wenn Sie diesen (oder einen anderen Bildschirm mit Interstitial) zuerst sehen, sind Sie nicht überrascht. Der Überwachungs Emulator kann einige Zeit in Anspruch nehmen: 

![Der Überwachungs Emulator zeigt nur eine Minute...](debug-on-emulator-images/please-wait.png)

Beim Ausführen der App muss der Emulator nicht jedes Mal beendet und anschließend wieder neu gestartet werden. Stattdessen kann dieser permanent ausgeführt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie Sie die Android-Emulator für die Wear-Entwicklung konfigurieren und ein virtuelles Wear-Gerät zum Debuggen starten.
