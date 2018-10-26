---
title: Welche USB-Treiber muss ich zum Debuggen von Android auf Windows?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 8b996a1cc89acedc47c7169ec579dfb99dae788f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114534"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Welche USB-Treiber muss ich zum Debuggen von Android auf Windows?

## <a name="finding-usb-drivers"></a>Suchen von USB-Treiber

Um auf Android-Geräten Debuggen, wenn in Windows zu entwickeln; Sie müssen einen kompatiblen USB-Treiber zu installieren. Android SDK-Manager umfasst "Google-USB-Treiber" in der Standardeinstellung Unterstützung für Nexus-Geräte fügt, wie hier beschrieben: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Andere Geräte erfordern, USB-Treiber, die speziell vom Gerätehersteller veröffentlicht wird. Einige Links für die häufigsten Hersteller sind in diesem Handbuch enthalten: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternativen

Je nach den Manfacturer kann es schwierig, zum Ermitteln des genauen USB-Treibers erforderlich sein. Einige Alternativen für das Testen von Android-apps, die in Windows, einschließlich der mit einem Android-Emulator oder externe Testdienste entwickelt werden. Hierzu zählen:

- [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/) – Cloud Testen von Diensten, die auf Hunderten von physischen Android-Geräten ausführen.

- [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md)

