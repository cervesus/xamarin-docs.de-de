---
title: Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/22/2018
ms.openlocfilehash: 85045967f5c63eb39c45f917b957d2a393a3a068
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "60945560"
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Welche USB-Treiber sind zum Debuggen von Android unter Windows erforderlich?

## <a name="finding-usb-drivers"></a>Suchen von USB-Treiber

Um auf Android-Geräten Debuggen, wenn in Windows zu entwickeln; Sie müssen einen kompatiblen USB-Treiber zu installieren. Android SDK-Manager umfasst "Google-USB-Treiber" in der Standardeinstellung Unterstützung für Nexus-Geräte fügt, wie hier beschrieben: [https://developer.android.com/sdk/win-usb.html](https://developer.android.com/sdk/win-usb.html)

Andere Geräte erfordern, USB-Treiber, die speziell vom Gerätehersteller veröffentlicht wird. Einige Links für die häufigsten Hersteller sind in diesem Handbuch enthalten: [https://developer.android.com/tools/extras/oem-usb.html](https://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternativen

Je nach den Manfacturer kann es schwierig, zum Ermitteln des genauen USB-Treibers erforderlich sein. Einige Alternativen für das Testen von Android-apps, die in Windows, einschließlich der mit einem Android-Emulator oder externe Testdienste entwickelt werden. Hierzu zählen:

- [App Center Test](https://docs.microsoft.com/appcenter/test-cloud/) – Cloud Testen von Diensten, die auf Hunderten von physischen Android-Geräten ausführen.

- [Visual Studio-Emulator für Android](https://visualstudio.microsoft.com/vs/msft-android-emulator/)

- [Debugging on the Android Emulator (Debuggen auf dem Android-Emulator)](~/android/deploy-test/debugging/debug-on-emulator.md)

