---
title: Drahtlose Bereitstellung für Xamarin.iOS- und tvOS-Apps
description: In diesem Dokument wird erläutert, wie eine Xamarin.iOS-App drahtlos über Visual Studio für Mac oder Visual Studio 2019 auf einem iOS-Gerät bereitgestellt werden kann.
ms.prod: xamarin
ms.assetid: 5AB4C5A9-4FBB-4DCB-BD72-0022D5439E65
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.custom: video
ms.date: 01/22/2019
ms.openlocfilehash: afa87cf98a2f9caee49c510674fe774a46e81107
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70284355"
---
# <a name="wireless-deployment-for-xamarinios-and-tvos-apps"></a>Drahtlose Bereitstellung für Xamarin.iOS- und tvOS-Apps

Ein wichtiger Bestandteil des Entwicklerworkflows ist die Bereitstellung auf einem Gerät. In Xcode 9 wurde die Option zur Bereitstellung für ein iOS-Gerät oder Apple TV über ein Netzwerk eingeführt, sodass Sie die Geräte nicht mehr per Kabel anschließen müssen, wenn Sie Ihre App bereitstellen und debuggen möchten. Dieses Feature wurde in Visual Studio für Mac 7.4 und in Visual Studio 15.6 eingeführt.

Dieses Handbuch enthält ausführliche Informationen zum Koppeln und Bereitstellen für ein Gerät über das Netzwerk.

## <a name="requirements"></a>Requirements (Anforderungen)

Die drahtlose Bereitstellung steht als Feature sowohl in Visual Studio für Mac als auch in Visual Studio zur Verfügung.

Für die drahtlose Bereitstellung benötigen Sie Folgendes:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

- macOS 10.12.4
- Die aktuelle Version von Visual Studio für Mac
- Xcode 9.0 oder höher
- Ein Gerät mit iOS 11.0 oder tvOS 11.0 und höher

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

- Die aktuelle Version von Visual Studio
- Ein Gerät mit iOS 11.0 oder tvOS 11.0 und höher

Auf Ihrem Mac-Buildhost müssen die folgenden Komponenten installiert sein:

- macOS 10.12.4
- Visual Studio für Mac
- Xcode 9.0 oder höher

-----

## <a name="connecting-a-device"></a>Verbinden eines Geräts

Zum drahtlosen Bereitstellen und Debuggen auf Ihrem Gerät müssen Sie Ihr iOS-Gerät oder Apple TV mit Xcode auf dem Mac koppeln. Anschließend können Sie es aus der Liste der Zielgeräte in Visual Studio auswählen. 

Der folgende Kopplungsvorgang muss nur einmal pro Gerät durchgeführt werden. Xcode behält die Verbindungseinstellungen bei.

<a name="pair" />

### <a name="pairing-an-ios-device-with-xcode"></a>Koppeln eines iOS-Geräts mit Xcode

1. Öffnen Sie Xcode, und wechseln Sie zu **Window > Devices and Simulators** (Fenster > Geräte und Simulatoren).
2. Schließen Sie Ihr iOS-Gerät über ein Lightning-Kabel an Ihren Mac an. Möglicherweise müssen Sie auf Ihrem Gerät **Diesem Computer vertrauen** auswählen.
3. Wählen Sie Ihr Gerät aus, und aktivieren Sie dann das Kontrollkästchen **Connect via network** (Über Netzwerk verbinden), um Ihr Gerät zu koppeln:  ![Fenster mit Geräten und Simulatoren mit der Option zur Verbindungsherstellung über ein Netzwerk](wireless-deployment-images/image2.png)

### <a name="pairing-an-apple-tv-with-xcode"></a>Koppeln von Apple TV mit Xcode

1. Stellen Sie sicher, dass Ihr Mac und Apple TV mit demselben Netzwerk verbunden sind.

2. Öffnen Sie Xcode, und wechseln Sie zu **Window > Devices and Simulators** (Fenster > Geräte und Simulatoren).

3. Wechseln Sie in Apple TV zu **Einstellungen > Fernbedienungen und Geräte > Remote-App und Geräte**.

4. Wählen Sie Apple TV in Xcode im Bereich **Discovered** (Ermittelt) aus, und geben Sie den in Apple TV angezeigten Verifizierungscode ein.

5. Klicken Sie auf die Schaltfläche **Connect** (Verbinden). Nach erfolgreicher Kopplung wird neben Apple TV ein Netzwerkverbindungssymbol angezeigt.

## <a name="deploy-to-a-device"></a>Bereitstellen für ein Gerät

Wenn ein Gerät drahtlos verbunden und für die Bereitstellung bereit ist, wird es in der Liste der Zielgeräte angezeigt, als wäre das Gerät über USB verbunden.

Für Tests auf einem physischen Gerät muss das Gerät [bereitgestellt](~/ios/get-started/installation/device-provisioning/index.md) werden. Stellen Sie sicher, dass Sie die entsprechenden Schritte durchführen, bevor Sie die Bereitstellung auf einem Gerät starten. 

Zur Bereitstellung auf einem iOS- oder tvOS-Gerät gehen Sie folgendermaßen vor:

1. Stellen Sie sicher, dass Ihr Bereitstellungscomputer und das Zielgerät sich in dem gleichen drahtlosen Netzwerk befinden. 

2. Wählen Sie Ihr Gerät aus der Liste der Zielgeräte aus, und starten Sie die Anwendung.

3. Wenn das Gerät gesperrt ist, werden Sie aufgefordert, Ihr Gerät zu entsperren. Sobald das Gerät entsperrt ist, wird Ihre App auf dem Gerät bereitgestellt.

Drahtloses Debuggen wird automatisch nach der drahtlosen Bereitstellung aktiviert, sodass Sie zuvor festgelegte Haltepunkte verwenden und Ihren Debuggingworkflow wie gewohnt fortsetzen können.

## <a name="troubleshooting"></a>Problembehandlung

1. Stellen Sie immer sicher, dass Ihr iOS-Gerät oder Apple TV mit demselben Netzwerk verbunden sind wie Ihr Mac.

2. Wenn das Gerät nicht in Visual Studio angezeigt wird, überprüfen Sie das Xcode-Fenster **Devices and Simulators** (Geräte und Simulatoren). 

    - Wenn Xcode Ihr Gerät **nicht** als verbunden anzeigt, versuchen Sie erneut, das Gerät zu [koppeln](#pair).

    - Wenn Xcode das Gerät nicht als verbunden anzeigt, starten Sie Visual Studio und Ihr Gerät neu.

3. Falls Sie dies noch nicht getan haben, müssen Sie Ihr Gerät [bereitstellen](~/ios/get-started/installation/device-provisioning/index.md).

4. Wenn Probleme mit diesem Feature auftreten, die durch die aufgeführten Schritten nicht behoben werden können, melden Sie das Problem in der [Entwicklercommunity](https://developercommunity.visualstudio.com/spaces/41/index.html).

## <a name="related-links"></a>Verwandte Links

- [Koppeln eines drahtlosen Geräts mit Xcode](https://help.apple.com/xcode/mac/9.0/index.html?localePath=en.lproj#/devbc48d1bad)

## <a name="related-video"></a>Zugehörige Videos

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Debug-to-iOS-Devices-Over-Wi-Fi/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
