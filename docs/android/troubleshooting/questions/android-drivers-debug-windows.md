---
title: Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 8be3f1b8803aa7e052ebc89af51dad3b659f95f5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757328"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?

## <a name="finding-usb-drivers"></a>Suchen von USB-Treibern

So debuggen Sie auf einem Android-Gerät bei der Entwicklung in Windows Sie müssen einen kompatiblen USB-Treiber installieren. Der Android SDK-Manager enthält standardmäßig den "Google-USB-Treiber", der die Unterstützung für Nexus-Geräte hinzufügt, wie hier beschrieben:[https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

Für andere Geräte sind USB-Treiber erforderlich, die speziell vom Gerätehersteller veröffentlicht werden. Einige Links zu den gängigsten Herstellern sind in dieser Anleitung enthalten:[https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternativ

Abhängig von der manfakturer kann es schwierig sein, den genauen benötigten USB-Treiber zu finden. Einige Alternativen zum Testen von in Windows entwickelten Android-Apps, einschließlich der Verwendung eines Android-Emulators oder der Verwendung externer Testdienste. Hierzu zählen:

- [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/) -Cloud-Testdienste werden auf Hunderten von echten Android-Geräten ausgeführt.

- [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md)
