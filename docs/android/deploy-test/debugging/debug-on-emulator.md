---
title: Debuggen auf dem Android-Emulator
description: In diesem Leitfaden wird erläutert, wie Sie Apps mithilfe des Android-Emulators starten und debuggen.
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 2bc8f82db29ed3c07c67293a83e6874f0cc6acb2
ms.sourcegitcommit: 5821c9709bf5e06e6126233932f94f9cf3524577
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75556521"
---
# <a name="debug-on-the-android-emulator"></a>Debuggen auf dem Android-Emulator

_In diesem Leitfaden erfahren Sie, wie Sie ein virtuelles Gerät im Android-Emulator zum Debuggen und Testen Ihrer App starten._

Der Android-Emulator wird zusammen mit der Workload **Mobile-Entwicklung mit .NET** installiert und kann mit vielen verschiedenen Konfigurationen verwendet werden, wodurch sich unterschiedliche Android-Geräte simulieren lassen. Jede dieser Konfigurationen wird als _virtuelles Gerät_ erstellt. In diesem Leitfaden erfahren Sie, wie Sie den Emulator in Visual Studio starten und die App auf einem virtuellen Gerät ausführen. Informationen zum Konfigurieren des Android-Emulators und Erstellen von neuen virtuellen Geräten finden Sie unter [Android Emulator Setup (Einrichten des Android-Emulators)](~/android/get-started/installation/android-emulator/index.md).

## <a name="using-a-pre-configured-virtual-device"></a>Verwenden eines vorkonfigurierten virtuellen Geräts

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Visual Studio enthält vorkonfigurierte virtuelle Geräte, die im Dropdownmenü „Gerät“ angezeigt werden. Im folgenden Screenshot aus Visual Studio 2017 sind beispielsweise mehrere vorkonfigurierte virtuelle Geräte verfügbar:

- **VisualStudio\_android-23\_arm\_phone**

- **VisualStudio\_android-23\_arm\_tablet**

- **VisualStudio\_android-23\_x86\_phone** 

- **VisualStudio\_android-23\_x86\_tablet** 

[![Virtuelle Geräte](debug-on-emulator-images/win/01-virtual-devices-sml.png)](debug-on-emulator-images/win/01-virtual-devices.png#lightbox)

Üblicherweise wählen Sie das virtuelle Gerät **VisualStudio\_android-23\_x86\_phone** zum Testen und Debuggen einer Smartphone-App aus. Wenn eines dieser vorkonfigurierten virtuellen Geräte Ihre Anforderungen erfüllt (d.h. mit der API-Zielebene Ihrer App übereinstimmt), fahren Sie mit dem [Starten des Emulators](#launching) fort, um Ihre App im Emulator auszuführen. (Wenn Sie nicht noch nicht mit Android-API-Ebenen vertraut sind, finden Sie weitere Informationen unter [Understanding Android API Levels (Grundlegendes zu Android-API-Ebenen)](~/android/app-fundamentals/android-api-levels.md).)

Wenn Ihr Xamarin.Android-Projekt eine Zielframeworkebene verwendet, die mit den verfügbaren virtuellen Geräten nicht kompatibel ist, werden die nicht verwendbaren virtuellen Geräte in der Dropdownliste unter **Nicht unterstützte Geräte** aufgelistet. Für das folgende Projekt ist beispielsweise ein Zielframework auf **Android 7.1 Nougat (API 25)** festgelegt, das mit den virtuellen **Android 6.0**-Geräten inkompatibel ist, die in diesem Beispiel aufgeführt werden:

[![Inkompatibles virtuelles Gerät](debug-on-emulator-images/win/02-incompatible-level-sml.png)](debug-on-emulator-images/win/02-incompatible-level.png#lightbox)

Sie können auf **Android-Mindestziel wird geändert** klicken, um die mindestens erforderliche Android-Version des Projekts zu ändern, damit sie mit der API-Ebene der verfügbaren virtuellen Geräte übereinstimmt. Alternativ können Sie den [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md) verwenden, um neue virtuelle Geräte zu erstellen, die Ihre API-Zielebene unterstützen.
Bevor Sie virtuelle Geräte für eine neue API-Ebene konfigurieren können, müssen Sie zunächst die entsprechenden Systemimages für diese API-Ebene installieren (siehe [Einrichten des Android SDK für Xamarin.Android](~/android/get-started/installation/android-sdk.md)).

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Visual Studio für Mac enthält vorkonfigurierte virtuelle Geräte, die im Dropdownmenü „Gerät“ angezeigt werden. Im folgenden Screenshot sind beispielsweise zwei vorkonfigurierte virtuelle Geräte verfügbar:

- **Android\_Accelerated\_x86**

- **Android\_ARMv7a**

[![Virtuelle Geräte](debug-on-emulator-images/mac/01-virtual-devices-sml.png)](debug-on-emulator-images/mac/01-virtual-devices.png#lightbox)

Üblicherweise wählen Sie das virtuelle Gerät **Android\_Accelerated\_x86** zum Testen und Debuggen einer Smartphone-App aus. Wenn dieses vorkonfigurierte virtuelle Gerät Ihre Anforderungen erfüllt (d.h. mit der API-Zielebene Ihrer App übereinstimmt), fahren Sie mit dem [Starten des Emulators](#launching) fort, um Ihre App im Emulator auszuführen. (Wenn Sie nicht noch nicht mit Android-API-Ebenen vertraut sind, finden Sie weitere Informationen unter [Understanding Android API Levels (Grundlegendes zu Android-API-Ebenen)](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="editing-virtual-devices"></a>Bearbeiten virtueller Geräte

Sie müssen [Android Device Manager](~/android/get-started/installation/android-emulator/device-manager.md) verwenden, um virtuelle Geräte zu bearbeiten (oder neue zu erstellen).

<a name="launching" />

## <a name="launching-the-emulator"></a>Starten des Emulators

Im oberen Bereich von Visual Studio ist ein Dropdownmenü, mit dem der Modus **Debuggen** oder **Release** ausgewählt werden kann. Durch Auswählen von **Debuggen** wird der Debugger an den Anwendungsprozess angefügt, der innerhalb des Emulators ausgeführt wird, nachdem die App gestartet wurde. Die Auswahl des Modus **Release** deaktiviert den Debugger (Sie können die App jedoch weiterhin ausführen und Protokollanweisungen zum Debuggen verwenden). Nachdem Sie ein virtuelles Gerät aus dem Geräte-Dropdownmenü ausgewählt haben, wählen Sie entweder den Modus **Debuggen** oder **Release** aus, und klicken Sie anschließend auf die Schaltfläche „Wiedergeben“, um die Anwendung auszuführen:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![Modi „Debuggen“ und „Release“; Schaltfläche „Wiedergeben“](debug-on-emulator-images/win/17-debug-release-sml.png)](debug-on-emulator-images/win/17-debug-release.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

[![Modi „Debuggen“ und „Release“; Schaltfläche „Wiedergeben“](debug-on-emulator-images/mac/16-debug-release-sml.png)](debug-on-emulator-images/mac/16-debug-release.png#lightbox)

-----

Nach dem Start des Emulators stellt Xamarin.Android die App für den Emulator bereit. Der Emulator führt die App mit dem Image des konfigurierten virtuellen Geräts aus. Unten sehen Sie einen Beispielscreenshot des Android-Emulators. In diesem Beispiel führt der Emulator eine leere App namens **MyApp** aus:

![Emulator, der eine leere App ausführt](debug-on-emulator-images/emulator-running.png)

Beim Ausführen der App muss der Emulator nicht jedes Mal beendet und anschließend wieder neu gestartet werden. Stattdessen kann dieser permanent ausgeführt werden. Bei der erstmaligen Ausführung einer Xamarin.Android-App im Emulator wird zuerst die freigegebene Xamarin.Android-Laufzeit für die Ziel-API-Ebene und anschließend die Anwendung installiert. Die Installation der Laufzeit kann einige Zeit in Anspruch nehmen. Bitte haben Sie daher etwas Geduld. Die Laufzeit wird nur bei der ersten Bereitstellung der Xamarin.Android-App im Emulator installiert &ndash; nachfolgende Bereitstellungen sind schneller, da nur die App in den Emulator kopiert wird.

<a name="quick-boot" />

## <a name="quick-boot"></a>Quick Boot

Neuere Versionen des Android-Emulators enthalten ein Feature namens _Quick Boot_, mit dem der Emulator in nur wenigen Sekunden gestartet werden kann. Wenn Sie den Emulator schließen, wird eine Momentaufnahme des Zustands des virtuellen Geräts gespeichert, sodass dieser Zustand schnell wiederhergestellt werden kann, wenn der Emulator neugestartet wird.
Sie benötigen Folgendes, um auf dieses Feature zugreifen zu können:

- Android-Emulator-Version 27.0.2 oder höher
- Android SDK Tools-Version 26.1.1 oder höher

Wenn die oben aufgeführten Versionen des Emulators und von SDK Tools installiert sind, ist das „Quick Boot“-Feature standardmäßig aktiviert. 

Der erste Kaltstart des virtuellen Geräts findet ohne Verbesserungen der Geschwindigkeit statt, da noch keine Momentaufnahme erstellt wurde:

![Screenshot vom Kaltstart](debug-on-emulator-images/cold-boot.png)

Wenn Sie den Emulator beenden, speichert Quick Boot den Zustand des Emulators in einer Momentaufnahme:

![Speichern vom Zustand beim Herunterfahren](debug-on-emulator-images/saving-state.png)

Aufeinanderfolgende Startvorgänge von virtuellen Geräten sind deutlich schneller, da der Emulator einfach den Zustand des Emulators wiederherstellt, an dem Sie den Emulator geschlossen haben.

![Ladezustand beim Neustart](debug-on-emulator-images/loading-state.png)

## <a name="troubleshooting"></a>Problembehandlung

Tipps und Hinweise zur Umgehung häufiger Emulatorprobleme finden Sie unter [Android Emulator Troubleshooting (Android-Emulator-Problembehandlung](~/android/get-started/installation/android-emulator/troubleshooting.md).

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde das Konfigurieren des Android-Emulators zum Ausführen und Testen von Xamarin.Android-Apps erläutert. Außerdem wurden die Schritte zum Starten des Emulators mithilfe vorkonfigurierter virtueller Geräte beschrieben. Die Vorgehensweise zum Bereitstellen einer Anwendung für den Emulator von Visual Studio wurde ebenfalls veranschaulicht. 

Weitere Informationen zur Verwendung des Android-Emulators finden Sie in den folgenden Artikeln für Android-Entwickler:

- [Navigating on the Screen (Navigieren auf dem Bildschirm)](https://developer.android.com/studio/run/emulator.html#navigate)

- [Performing Basic Tasks in the Emulator (Ausführen von grundlegenden Aufgaben im Emulator)](https://developer.android.com/studio/run/emulator.html#tasks)

- [Working with Extended Controls, Settings, and Help (Arbeiten mit erweiterten Steuerelementen, Einstellungen und Hilfe)](https://developer.android.com/studio/run/emulator.html#extended)

- [Ausführen des Emulators mit Quick Boot](https://developer.android.com/studio/run/emulator#quickboot)
