---
title: Setup von Android-Emulator
description: Der Android-Emulator kann mit verschiedenen Konfigurationen ausgeführt werden, wodurch unterschiedliche Geräte simuliert werden können. In diesem Leitfaden wird erläutert, wie Sie den Android-Emulator vorbereiten, um Ihre App zu testen.
ms.prod: xamarin
ms.assetid: 889963B7-F4DA-41D9-9B8D-B733BB71A329
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 08/27/2018
ms.openlocfilehash: 148afe5354d7995f15dc19c6257ed2a1567162ec
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027956"
---
# <a name="android-emulator-setup"></a>Setup von Android-Emulator

_In diesem Leitfaden wird erläutert, wie Sie den Android-Emulator vorbereiten, um Ihre App zu testen._

## <a name="overview"></a>Übersicht

Der Android-Emulator kann mit verschiedenen Konfigurationen ausgeführt werden, wodurch unterschiedliche Geräte simuliert werden können. Jede Konfiguration wird als _virtuelles Gerät_ bezeichnet. Wenn Sie die App im Emulator bereitstellen und testen, wählen Sie ein vorkonfiguriertes oder benutzerdefiniertes virtuelles Gerät aus, das ein physisches Android-Gerät simuliert, z.B. ein Nexus- oder Pixel-Smartphone.

In den folgenden Abschnitten wird beschrieben, wie sich der Android-Emulator für optimale Leistung beschleunigen lässt. Außerdem wird erläutert, wie Sie mit dem Android Device Manager virtuelle Geräte erstellen und anpassen und die Profileigenschaften eines virtuellen Geräts anpassen. Darüber hinaus werden im Abschnitt zur Problembehandlung häufige Emulatorprobleme und Schritte zur Umgehung dieser Probleme genannt.

## <a name="sections"></a>Abschnitte

### <a name="hardware-acceleration-for-emulator-performance"></a>[Hardwarebeschleunigung für verbesserte Leistung des Emulators](~/android/get-started/installation/android-emulator/hardware-acceleration.md)

In diesem Artikel wird beschrieben, wie Sie Ihren Computer für höchstmögliche Leistung des Android-Emulators mithilfe von entweder Hyper-V- oder HAXM-Virtualisierungstechnologie vorbereiten. Da der Android-Emulator ohne Hardwarebeschleunigung sehr langsam sein kann, wird empfohlen, dass Sie die Hardwarebeschleunigung auf Ihrem Computer aktivieren, bevor Sie den Emulator verwenden.

### <a name="managing-virtual-devices-with-the-android-device-manager"></a>[Verwalten virtueller Geräte mit Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md)

In diesem Artikel wird beschrieben, wie Sie Android Device Manager zum Erstellen und Anpassen von virtuellen Geräten verwenden.

### <a name="editing-android-virtual-device-properties"></a>[Bearbeiten der Eigenschaften von virtuellen Android-Geräten](~/android/get-started/installation/android-emulator/device-properties.md)

In diesem Artikel wird beschrieben, wie Sie den Android Device Manager zum Bearbeiten der Profileigenschaften eines virtuellen Geräts verwenden.

### <a name="android-emulator-troubleshooting"></a>[Android Emulator Troubleshooting (Android-Emulator-Problembehandlung](~/android/get-started/installation/android-emulator/troubleshooting.md)

In diesem Artikel werden die gängigsten Warnmeldungen und Probleme beschrieben, die beim Ausführen des Android-Emulators auftreten können. Außerdem finden Sie dort Tipps und Hinweise zum Umgehen dieser Probleme.

Nachdem Sie den Android-Emulator konfiguriert haben, finden Sie unter [Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md) weitere Informationen zum Starten des Emulators und dessen Einsatz zum Testen und Debuggen von Apps.

> [!NOTE]
> Mit der Veröffentlichung von Version **26.0.1** von Android SDK Tools hat Google die Unterstützung für bestehende AVD/SDK-Manager zugunsten seines neuen CLI-Tools (Befehlszeilenschnittstellentool) eingestellt. Aufgrund dieser Änderung werden jetzt Xamarin SDK- bzw. Device Manager anstelle von Google SDK- bzw. Device Managern für Android-Tools 26.0.1 und höher verwendet. Weitere Informationen zum Xamarin SDK Manager finden Sie unter [Einrichten des Android SDK für Xamarin.Android](~/android/get-started/installation/android-sdk.md).
