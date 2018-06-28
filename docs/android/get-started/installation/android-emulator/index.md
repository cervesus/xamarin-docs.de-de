---
title: Setup des Google Android-Emulators
description: Der Google Android-Emulator kann in verschiedenen Konfigurationen ausgeführt werden, um unterschiedliche Geräte zu simulieren. In diesem Leitfaden wird erläutert, wie Sie den Android-Emulator vorbereiten, um Ihre App zu testen.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/01/2018
ms.openlocfilehash: e5ba2cc23ea9751ca60644d3eb5b7e3f31bbb6bb
ms.sourcegitcommit: a7febc19102209b21e0696256c324f366faa444e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/04/2018
ms.locfileid: "34732530"
---
# <a name="google-android-emulator-setup"></a>Setup des Google Android-Emulators

_In diesem Leitfaden wird erläutert, wie Sie den Google Android-Emulator vorbereiten, um Ihre App zu testen._


## <a name="overview"></a>Übersicht

Der Google Android-Emulator kann in verschiedenen Konfigurationen ausgeführt werden, um unterschiedliche Geräte zu simulieren. Jede Konfiguration wird als _virtuelles Gerät_ bezeichnet. Wenn Sie die App im Emulator bereitstellen und testen, wählen Sie ein vorkonfiguriertes oder benutzerdefiniertes virtuelles Gerät aus, das ein physisches Android-Gerät simuliert, z.B. ein Nexus- oder Pixel-Smartphone.

Die im Folgenden aufgeführten Abschnitte beschreiben das Beschleunigen des Google Android-Emulators für optimale Leistung, das Verwenden von Android Device Manager zum Erstellen und Anpassen virtueller Geräte sowie das Anpassen der Profileigenschaften eines virtuellen Geräts. Zusätzlich werden im Abschnitt zur Problembehandlung häufige Probleme beim Setup und Problemumgehung beschrieben.

## <a name="sections"></a>Abschnitte

### <a name="hardware-acceleration-for-emulator-performanceandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Hardwarebeschleunigung für verbesserte Leistung des Emulators](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

In diesem Artikel wird beschrieben, wie Sie Ihren Computer auf die optimale Leistung des Android Emulators vorbereiten.
Da der Google Android-Emulator ohne Hardwarebeschleunigung sehr langsam sein kann, wird empfohlen, dass Sie die Hardwarebeschleunigung auf Ihrem Computer aktivieren, bevor Sie diesen Emulator verwenden.

### <a name="managing-virtual-devices-with-the-android-device-managerandroidget-startedinstallationandroid-emulatordevice-managermd"></a>[Verwalten virtueller Geräte mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)

In diesem Artikel wird beschrieben, wie Sie Android Device Manager zum Erstellen und Anpassen von virtuellen Geräten verwenden.

### <a name="editing-android-virtual-device-propertiesandroidget-startedinstallationandroid-emulatordevice-propertiesmd"></a>[Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md)

In diesem Artikel wird beschrieben, wie Sie Android Device Manager zum Bearbeiten der Profileigenschaften eines virtuellen Android-Geräts verwenden.

### <a name="troubleshooting-emulator-setup-problemsandroidget-startedinstallationandroid-emulatortroubleshootingmd"></a>[Behandlung von Problemen beim Setup des Emulators](~/android/get-started/installation/android-emulator/troubleshooting.md)

In diesem Artikel wird das Diagnostizieren und Beheben von Problemen mit Android Device Manager beschrieben, die beim Setup des Android-Emulators auftreten können.


Nachdem Sie den Android-Emulator konfiguriert haben, finden Sie unter [Debuggen mit dem Google Android-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md) weitere Informationen zum Starten des Emulators und dessen Einsatz zum Testen und Debuggen von Apps.


> [!NOTE]
> Mit der Veröffentlichung von Version **26.0.1** von Android SDK Tools hat Google die Unterstützung für bestehende AVD/SDK-Manager zugunsten seines neuen CLI-Tools (Befehlszeilenschnittstellentool) eingestellt. Aufgrund dieser Änderung werden jetzt Xamarin SDK- bzw. Device Manager anstelle von Google SDK- bzw. Device Managern für Android-Tools 26.0.1 und höher verwendet. Weitere Informationen zum Xamarin SDK-Manager finden Sie unter [Android SDK Setup (Setup von Android SDK)](~/android/get-started/installation/android-sdk.md).

