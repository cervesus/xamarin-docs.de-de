---
title: Xamarin und Problembehandlung für iOS-13
description: Dieser Abschnitt enthält Tipps zur Problembehandlung für Xamarin-Funktionen, die im Zusammenhang mit der iOS-13.
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: 99fd7e41e1521c6a9254d286bf989281658ccf24
ms.sourcegitcommit: 85c45dc28ab3625321c271804768d8e4fce62faf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "67039787"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>Tipps zur Problembehandlung für iOS 13 und Xamarin.iOS

![Feature der Vorschauversion](~/media/shared/preview.png)

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>Aktualisieren von Xcode 11 beendet den Simulator starten

Nach dem Update auf **Xcode 11 Beta 1** jedes Mal, wenn ein Simulator gestartet wird die folgende Ausnahme ausgelöst wird, und der Simulator nicht gestartet. Dies geschieht alle Simulatoren.

### <a name="exception"></a>Ausnahme

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>Problemumgehung

Bis es ist ein [beheben](https://github.com/xamarin/xamarin-macios/issues/6216), die folgenden Schritte können installieren die alte Gerätesimulator-Framework, damit Entwickler weiterhin können erneut erfasst werden:

> [!NOTE]
> Diese Schritte setzen voraus, dass Sie zwei Xcode-Anwendungen verfügen:
> - **Xcode11-beta1.app** – die Beta-Version, die nicht in Simulatoren und Visual Studio für Mac funktioniert
> - **Xcode102.app** – eine stabile Version von Xcode-10. Ihnen möglicherweise auch aufgerufen werden, **Xcode.app**.
>
> Ändern Sie die unten angegebenen Beispiele über die Befehlszeile, nach Bedarf für Ihre Konfiguration.

1. Stellen Sie sicher, dass Sie die Xcode-11, die über Xcode-Select ausgewählt haben:

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. Tools das Setup ausführen, falls erforderlich, zum ersten Mal.

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. Entfernen Sie die folgenden Frameworks:

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. Wechseln Sie zurück an die alte Version von Xcode

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. Führen Sie das erste Launch Tool für erneut aus der _alte_ Xcode-Version, die Sie soeben ausgewählt

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

Nach dem Ausführen dieser Schritte sollten Sie erneut mit dem iOS-Simulator arbeiten können.