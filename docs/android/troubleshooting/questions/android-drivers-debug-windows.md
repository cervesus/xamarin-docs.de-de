---
title: Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/22/2018
ms.openlocfilehash: 21fd8eff64d374e52e64194524a8c096cdf4d90e
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027044"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?

## <a name="finding-usb-drivers"></a>Suchen nach USB-Treibern

Zum Debuggen auf einem Android-Gerät bei der Entwicklung in Windows müssen Sie einen kompatiblen USB-Treiber installieren. Der Android-SDK-Manager enthält standardmäßig den „Google-USB-Treiber“, der Unterstützung für Nexus-Geräte bietet, wie hier beschrieben: [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html).

Für andere Geräte sind speziell vom jeweiligen Gerätehersteller veröffentlichte USB-Treiber erforderlich. In dem Handbuch unter dem folgenden Link finden Sie Links für die wichtigsten Hersteller: [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html).

## <a name="alternatives"></a>Alternativen

Je nach Hersteller kann es schwierig sein, den konkreten benötigten USB-Treiber zu finden. Alternativen für das Testen von unter Windows entwickelten Android-Apps sind z. B. das Verwenden eines Android-Emulators oder externer Testdienste. Hierzu zählen:

- [App Center-Test:](https://docs.microsoft.com/appcenter/test-cloud/) Testdienste in der Cloud, die auf hunderten von realen Android-Geräten ausgeführt werden.

- [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Debuggen auf dem Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)
