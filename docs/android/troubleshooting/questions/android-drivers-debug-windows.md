---
title: Welche USB-Treiber ist zum Debuggen von Android unter Windows erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/22/2018
ms.openlocfilehash: ee3f2b1e1ff6a3ac1bec2d73d4af6e740979aa04
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066870"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Welche USB-Treiber ist zum Debuggen von Android unter Windows erforderlich?

## <a name="finding-usb-drivers"></a>Suchen von USB-Treiber

So debuggen Sie auf einem Android-Gerät bei der Entwicklung in Windows; Sie müssen einen kompatiblen USB-Treiber installieren. Android SDK Manager umfasst "Google-USB-Treiber" wird standardmäßig die fügt Unterstützung für Nexus-Geräte, wie hier beschrieben: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Bei anderen Geräten müssen USB-Treiber, die vom Gerätehersteller speziell veröffentlicht. In diesem Handbuch werden einige Links für die häufigsten Hersteller enthalten: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternativen

Je nach den Manfacturer kann es schwierig, Auffinden der genaue USB-Treiber erforderlich sein. Einige Alternativen für das Testen von Android-apps, die in Windows, einschließlich der Verwendung von Android-Emulator oder externe Tests Dienste entwickelt werden. Hierzu zählen:

- [Testen der App Mitte](https://docs.microsoft.com/appcenter/test-cloud/) – Cloud Services ausführen auf Hunderten von real Android-Geräte zu testen.

- [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Debuggen auf Android-Emulator](~/android/deploy-test/debugging/debug-on-emulator.md)

