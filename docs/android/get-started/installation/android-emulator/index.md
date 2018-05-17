---
title: Setup von Android-Emulator
description: In diesem Abschnitt wird beschrieben, wie Sie den Google Android-Emulator vorbereiten, um Ihre App zu testen. Es wird erläutert, wie Sie den Emulator auf Höchstleistung beschleunigen, und es wird dargestellt, wie Sie einen Emulator-Manager verwenden, um virtuelle Geräte zu erstellen und anzupassen.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/25/2018
ms.openlocfilehash: 215e298068b7a3a23b2e469e923f172c8303bbcb
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="android-emulator-setup"></a>Setup von Android-Emulator

_In diesem Abschnitt wird beschrieben, wie Sie den Google Android-Emulator vorbereiten, um Ihre App zu testen. Es wird erläutert, wie Sie den Emulator auf Höchstleistung beschleunigen, und es wird dargestellt, wie Sie einen Emulator-Manager verwenden, um virtuelle Geräte zu erstellen und anzupassen._


## <a name="overview"></a>Übersicht

Der Google Android SDK-Emulator kann in verschiedenen Konfigurationen ausgeführt werden, um unterschiedliche Geräte zu simulieren. Jede dieser Konfigurationen wird als _virtuelles Gerät_ erstellt. In diesem Handbuch erfahren Sie, wie Sie die Leistung des Android-Emulators beschleunigen, und wie Sie entweder den Xamarin Android-Emulator-Manager oder den älteren Google Emulator Manager verwenden, um virtuelle Geräte zu erstellen.


> [!NOTE]
> Mit der Veröffentlichung von Version **26.0.1** von Android SDK Tools hat Google die Unterstützung für bestehende AVD/SDK-Manager zugunsten seines neuen CLI-Tools (Befehlszeilenschnittstellentool) eingestellt. Aufgrund dieser Änderung werden jetzt Xamarin SDK- bzw. Geräte-Manager anstelle von Google SDK- bzw. Emulator Manager für Android-Tools 26.0.1 und höher verwendet. (Weitere Informationen zum Xamarin SDK-Manager finden Sie unter [Android SDK Setup (Android SDK-Setup)](~/android/get-started/installation/android-sdk.md)).


## <a name="sections"></a>Abschnitte

### <a name="hardware-accelerationandroidget-startedinstallationandroid-emulatorhardware-accelerationmd"></a>[Hardwarebeschleunigung](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

So bereiten Sie Ihren Computer auf die höchstmögliche Leistung des Google Android-Emulators vor. Da der Google Android-Emulator ohne Hardwarebeschleunigung sehr langsam sein kann, wird empfohlen, dass Sie die Hardwarebeschleunigung auf Ihrem Computer aktivieren, bevor Sie den Google Android-Emulator verwenden.

### <a name="xamarin-android-device-managerandroidget-startedinstallationandroid-emulatorxamarin-device-managermd"></a>[Xamarin Android-Geräte-Manager](~/android/get-started/installation/android-emulator/xamarin-device-manager.md)

So verwenden Sie den Xamarin Android-Geräte-Manager zum Erstellen und Anpassen von virtuellen Geräten für den Google Android-Emulator. Der **Xamarin Android-Geräte-Manager**, der sich derzeit in der Vorschauversion befindet, soll den älteren Google Emulator Manager ersetzen. Wenn Sie Android Oreo 8.0 oder höher anzielen, müssen Sie den Xamarin Android-Geräte-Manager anstelle des Google Emulator Managers verwenden.

### <a name="google-emulator-managerandroidget-startedinstallationandroid-emulatorgoogle-emulator-managermd"></a>[Google Emulator Manager](~/android/get-started/installation/android-emulator/google-emulator-manager.md)

So verwenden Sie den älteren Google Emulator Manager zum Erstellen und Anpassen von virtuellen Geräten für den Google Android-Emulator. Sie können den Google Android Emulator zusammen mit dem ursprünglichen Google Emulator Manager weiterhin ausführen, wenn Sie bei Version 25.2.5 oder einer früheren Version von Android SDK Tools bleiben.

Nachdem Sie den Android SDK-Emulator konfiguriert haben, lesen Sie [Google Android-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md), um mehr über das Starten des Emulators und dessen Einsatz zum Testen und Debuggen von Apps zu erfahren.
