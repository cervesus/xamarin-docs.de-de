---
title: Debuggen von Android Wear in einem Emulator
description: In diesen Artikeln wird das Debuggen einer Xamarin.Android-Wear-Anwendung in einem Emulator erläutert.
ms.prod: xamarin
ms.assetid: 225684B2-3122-4E3B-A028-A3A400976D31
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/21/2018
ms.openlocfilehash: 699fb3cc3a5730e8ab2c677feb7cdfbdcf106aeb
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61308273"
---
# <a name="debug-android-wear-on-an-emulator"></a>Debuggen von Android Wear in einem Emulator

_In diesen Artikeln wird das Debuggen einer Xamarin.Android-Wear-Anwendung in einem Emulator erläutert._

## <a name="debug-wear-on-emulator-overview"></a>Debuggen von Wear auf Emulator-Übersicht

Entwickeln von Anwendungen für Android Wear erfordert das Ausführen der Anwendung, entweder auf physischer Hardware oder einem Emulator oder Simulator. Die beste, aber nicht immer zweckmäßigste, Herangehensweise ist die Verwendung von Hardware. In vielen Fällen kann es einfacher und kostengünstiger zu simulieren/emulieren von Android Wear-Hardware, die einen Emulator zu verwenden, wie unten beschrieben sein. Wenn Sie noch nicht vertraut sind, mit dem Bereitstellen und Ausführen von apps für Android Wear, finden Sie unter [Hallo Wear](~/android/wear/get-started/hello-wear.md).

## <a name="configure-the-android-emulator"></a>Konfigurieren des Android-Emulators

Um Ihre Wear-app in einem Emulator ausführen möchten, müssen Sie den Android SDK-Android-Emulator zu installieren und konfigurieren sie für Android Wear. Allgemeine Android SDK-Emulator Informationen zur Installation und Konfiguration, finden Sie unter [Setup von Android-Emulator](~/android/get-started/installation/android-emulator/index.md).

Wenn Sie ein virtuelles Wear-Gerät erstellen, wählen Sie ein Profil für Android Wear-Gerät (z. B. **Android Wear Quadrat**). Verwenden Sie zur Verbesserung der Leistung der Verschleiß **X86** CPU/ABI, wie in diesem Beispiel gezeigt:

[![Beispielkonfiguration für Wear virtuelles Gerät](debug-on-emulator-images/01-wear-avd-example-sml.png)](debug-on-emulator-images/01-wear-avd-example.png#lightbox)


## <a name="launch-the-wear-virtual-device"></a>Starten Sie das virtuelle Wear-Gerät 

Nachdem Sie ein virtuelles Android Wear-Gerät erstellt haben, können Sie es im Gerät Pulldown-Menü in der IDE auswählen, bevor Sie das Debuggen starten. Wenn Ihr virtuelle Gerät nicht im Geräte-Pulldownmenü verfügbar ist, überprüfen Sie, ob das Projekt Android *Wear* app-Projekt (nicht mit einem Android-app-Projekt) und die API-Zielebene festgelegt ist, auf die gleiche API-Ebene wie das virtuelle Gerät. Zum Beispiel:

[![Ein AVD Wear auswählen in Visual Studio im Menü](debug-on-emulator-images/vs/choose-wear-sim.png)](debug-on-emulator-images/vs/choose-wear-sim.png#lightbox)

Nach dem Starten des Android-Emulators stellt Xamarin.Android die Wear-app mit dem Emulator bereit. Der Emulator führt die App mit dem Image des konfigurierten virtuellen Geräts aus.

Seien Sie nicht überrascht, wenn dies angezeigt wird (oder eines anderen interstitial Bildschirms) auf den ersten. Die Watch-Emulator kann gestartet werden länger dauern: 

![Sehen Sie sich, dass der Emulator nur eine Minute zeigt...](debug-on-emulator-images/please-wait.png)

Beim Ausführen der App muss der Emulator nicht jedes Mal beendet und anschließend wieder neu gestartet werden. Stattdessen kann dieser permanent ausgeführt werden.

 
## <a name="summary"></a>Zusammenfassung
 
Dieses Handbuch wurde erklärt, wie Android-Emulator für die Entwicklung von Wear konfigurieren und ein virtuelles Wear-Gerät für das Debuggen zu starten.
