---
title: Problembehandlung bei xamarin und IOS 13
description: Dieser Abschnitt enthält Tipps zur Problembehandlung für xamarin-Funktionen im Zusammenhang mit IOS 13.
ms.prod: xamarin
ms.assetid: 00DE8C33-1407-45C0-A0C7-32AF1E490034
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 06/12/2019
ms.openlocfilehash: 9a12163b0300a8d523632ad38e1b66a8c44b7aaf
ms.sourcegitcommit: 09bc69d7119a04684c9e804c5cb113b8b1bb7dfc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2019
ms.locfileid: "71206319"
---
# <a name="troubleshooting-tips-for-ios-13-and-xamarinios"></a>Tipps zur Problembehandlung für IOS 13 und xamarin. IOS

## <a name="updating-to-xcode-11-stops-the-simulator-from-launching"></a>Beim Aktualisieren auf Xcode 11 wird der Simulator beendet.

Nach dem Aktualisieren auf **Xcode 11 Beta 1** bei jedem Start eines Simulators wird die folgende Ausnahme ausgelöst, und der Simulator wird nicht gestartet. Dies geschieht bei allen Simulatoren.

### <a name="exception"></a>Ausnahme

`Foundation.ObjCException: NSInvalidArgumentException: -[SimDevice registerNotificationHandler:]: unrecognized selector sent to instance 0x7ffbf5d1e110`

### <a name="workaround"></a>Problemumgehung

Bis eine [Korrektur](https://github.com/xamarin/xamarin-macios/issues/6216)vorliegt, können die folgenden Schritte ausgeführt werden, um das alte simulatorframework neu zu installieren, damit Entwickler weiterhin arbeiten können:

> [!NOTE]
> Diese Schritte setzen voraus, dass Sie über zwei Xcode-Anwendungen verfügen:
>
> - **Xcode11-Beta1. app** – die Beta Version, die nicht mit Simulatoren und Visual Studio für Mac funktioniert.
> - **Xcode102. app** – eine stabile Version von Xcode 10. Sie können auch " **Xcode. app**" genannt werden.
>
> Ändern Sie die folgenden Befehlszeilen Beispiele entsprechend Ihrer Konfiguration.

1. Stellen Sie sicher, dass Xcode 11 über Xcode-Select ausgewählt ist:

   `sudo xcode-select -s /Applications/Xcode11-beta1.app/Contents/Developer/`

2. Führen Sie bei Bedarf die Setup Tools zum ersten Mal aus.

    `/Applications/Xcode11-beta1.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

3. Entfernen Sie das folgende Framework:

    `sudo rm -Rf  /Library/Developer/PrivateFrameworks/CoreSimulator.framework/Versions/*`

4. Wechseln Sie zurück zur alten Xcode-Version.

   `sudo xcode-select -s /Applications/Xcode102.app/Contents/Developer/`

5. Führen Sie das erste Start Tool für die _alte_ Xcode-Version erneut aus, die Sie soeben ausgewählt haben.

   `/Applications/Xcode102.app/Contents/Developer/usr/bin/xcodebuild -runFirstLaunch`

Nachdem Sie diese Schritte ausgeführt haben, sollten Sie in der Lage sein, erneut mit dem IOS-Simulator zu arbeiten.
