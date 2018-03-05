---
title: "Ausführen des Android SDK-Emulators"
description: So debuggen Sie eine App mit dem Android SDK-Emulator
ms.topic: article
ms.prod: xamarin
ms.assetid: AEA165A4-D81A-411B-91DF-2DED2EED27B5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 2995d9126617a767013ed1f5cb808f22ce0fd2da
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="running-the-android-sdk-emulator"></a>Ausführen des Android SDK-Emulators

In diesem Leitfaden erfahren Sie, wie Sie ein virtuelles Gerät im Android SDK-Emulator zum Debuggen und Testen Ihrer App starten.

## <a name="using-a-pre-configured-virtual-device"></a>Verwenden eines vorkonfigurierten virtuellen Geräts

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Visual Studio enthält vorkonfigurierte virtuelle Geräte, die im Dropdownmenü „Gerät“ angezeigt werden. Im folgenden Screenshot aus Visual Studio 2017 sind beispielsweise mehrere vorkonfigurierte virtuelle Geräte verfügbar:

-   **VisualStudio\_android-23\_arm\_phone**

-   **VisualStudio\_android-23\_arm\_tablet**

-   **VisualStudio\_android-23\_x86\_phone** 

-   **VisualStudio\_android-23\_x86\_tablet** 

[ ![Virtuelle Geräte](running-the-emulator-images/win/01-virtual-devices-sml.png)](running-the-emulator-images/win/01-virtual-devices.png)

Üblicherweise wählen Sie das virtuelle Gerät **VisualStudio\_android-23\_x86\_phone** zum Testen und Debuggen einer Smartphone-App aus. Wenn eines dieser vorkonfigurierten virtuellen Geräte Ihre Anforderungen erfüllt (d.h. mit der API-Zielebene Ihrer App übereinstimmt), fahren Sie mit dem [Starten des Emulators](#launching) fort, um Ihre App im Emulator auszuführen. (Wenn Sie nicht noch nicht mit Android-API-Ebenen vertraut sind, finden Sie weitere Informationen unter [Understanding Android API Levels (Grundlegendes zu Android-API-Ebenen)](~/android/app-fundamentals/android-api-levels.md).)

Wenn Ihr Xamarin.Android-Projekt eine Zielframeworkebene verwendet, die mit den verfügbaren virtuellen Geräten nicht kompatibel ist, werden die nicht verwendbaren virtuellen Geräte in der Dropdownliste unter **Nicht unterstützte Geräte** aufgelistet. Für das folgende Projekt ist beispielsweise ein Zielframework auf **Android 7.1 Nougat (API 25)** festgelegt, das mit den virtuellen **Android 6.0**-Geräten inkompatibel ist, die standardmäßig bereitgestellt sind:

[ ![Inkompatibles virtuelles Gerät](running-the-emulator-images/win/02-incompatible-level-sml.png)](running-the-emulator-images/win/02-incompatible-level.png)

Sie können auf **Android-Mindestziel wird geändert** klicken, um die mindestens erforderliche Android-Version des Projekts zu ändern, damit sie mit der API-Ebene der verfügbaren virtuellen Geräte übereinstimmt. Alternativ können Sie den **Android-Emulator-Manager** verwenden, um neue virtuelle Geräte zu erstellen, die wie weiter unten unter [Konfigurieren von virtuellen Geräten](#virtualdevice) erklärt Ihre API-Zielebene unterstützen. Bevor Sie virtuelle Geräte für eine neue API-Ebene konfigurieren können, müssen Sie zunächst die entsprechenden Systemimages für diese API-Ebene installieren &ndash; dies wird im nächsten Abschnitt erklärt.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Visual Studio für Mac enthält vorkonfigurierte virtuelle Geräte, die im Dropdownmenü „Gerät“ angezeigt werden. Im folgenden Screenshot sind beispielsweise zwei vorkonfigurierte virtuelle Geräte verfügbar:

-   **Android\_Accelerated\_x86**

-   **Android\_ARMv7a**

[ ![Virtuelle Geräte](running-the-emulator-images/mac/01-virtual-devices-sml.png)](running-the-emulator-images/mac/01-virtual-devices.png)

Üblicherweise wählen Sie das virtuelle Gerät **Android\_Accelerated\_x86** zum Testen und Debuggen einer Smartphone-App aus. Wenn dieses vorkonfigurierte virtuelle Gerät Ihre Anforderungen erfüllt (d.h. mit der API-Zielebene Ihrer App übereinstimmt), fahren Sie mit dem [Starten des Emulators](#launching) fort, um Ihre App im Emulator auszuführen. (Wenn Sie nicht noch nicht mit Android-API-Ebenen vertraut sind, finden Sie weitere Informationen unter [Understanding Android API Levels (Grundlegendes zu Android-API-Ebenen)](~/android/app-fundamentals/android-api-levels.md).)

-----

## <a name="creating-custom-virtual-devices"></a>Erstellen von benutzerdefinierten virtuellen Geräten

Sie müssen entweder den Xamarin Android-Geräte-Manager oder den älteren Google Emulator Manager verwenden, der Teil von Android SDK ist, um benutzerdefinierte virtuelle Geräte zu erstellen. Weitere Informationen zum Erstellen und Anpassen von virtuellen Geräten finden Sie unter [Xamarin Android Geräte-Manager](~/android/get-started/installation/android-emulator/xamarin-device-manager.md).
Wenn Sie lieber den älteren Google Emulator Manager verwenden möchten, finden Sie unter [Google Emulator Manager](~/android/get-started/installation/android-emulator/google-emulator-manager.md) weitere Informationen.

Beachten Sie, dass Sie den Xamarin Android Geräte-Manager verwenden müssen, wenn Sie für Android 8.0 Oreo entwickeln.

<a name="launching" />

## <a name="launching-the-emulator"></a>Starten des Emulators

Im oberen Bereich der IDE ist ein Dropdownmenü, mit dem der Modus **Debuggen** oder **Release** ausgewählt werden kann. Durch Auswählen von **Debuggen** wird der Debugger an den Anwendungsprozess angefügt, der innerhalb des Emulators ausgeführt wird. 

Nachdem Sie ein virtuelles Gerät aus dem Geräte-Dropdownmenü ausgewählt haben, wählen Sie entweder den Modus **Debuggen** oder **Release** aus, und klicken Sie anschließend auf die Schaltfläche **Wiedergeben**, um die Anwendung auszuführen:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![Modi „Debuggen“ und „Release“; Schaltfläche „Wiedergeben“](running-the-emulator-images/win/17-debug-release-sml.png)](running-the-emulator-images/win/17-debug-release.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

[![Modi „Debuggen“ und „Release“; Schaltfläche „Wiedergeben“](running-the-emulator-images/mac/16-debug-release-sml.png)](running-the-emulator-images/mac/16-debug-release.png)

-----

Nach dem Start des Android-Emulators stellt Xamarin.Android die App im Emulator bereit. Der Emulator führt die App mit dem Image des konfigurierten virtuellen Geräts aus. Ein Beispielscreenshot des Android SDK Emulators ist unten zu sehen. Der Emulator wird in einer leeren App mit dem Namen **MyApp** ausgeführt:

![Emulator, der eine leere App ausführt](running-the-emulator-images/emulator-running.png)

Beim Ausführen der App muss der Emulator nicht jedes Mal beendet und anschließend wieder neu gestartet werden. Stattdessen kann dieser permanent ausgeführt werden. Bei der erstmaligen Ausführung einer Xamarin.Android-App im Emulator wird zuerst die freigegebene Xamarin.Android-Laufzeit für die Ziel-API-Ebene und anschließend die Anwendung installiert. Die Installation der Laufzeit kann einige Zeit in Anspruch nehmen. Bitte haben Sie daher etwas Geduld. Die Laufzeit wird nur bei der ersten Bereitstellung der Xamarin.Android-App im Emulator installiert &ndash; nachfolgende Bereitstellungen sind schneller, da nur die App in den Emulator kopiert wird.

Weitere Informationen zur Verwendung des Android SDK Emulators finden Sie in den folgenden Themen für Android-Entwickler:

-   [Navigating on the Screen (Navigieren auf dem Bildschirm)](https://developer.android.com/studio/run/emulator.html#navigate)

-   [Performing Basic Tasks in the Emulator (Ausführen von grundlegenden Aufgaben im Emulator)](https://developer.android.com/studio/run/emulator.html#tasks)

-   [Working with Extended Controls, Settings, and Help (Arbeiten mit erweiterten Steuerelementen, Einstellungen und Hilfe)](https://developer.android.com/studio/run/emulator.html#extended)

