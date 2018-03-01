---
title: Welche USB-Treiber ist zum Debuggen von Android unter Windows erforderlich?
ms.topic: article
ms.prod: xamarin
ms.assetid: 36EC7341-A2A4-409C-BD4F-330BAC505123
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/19/2017
ms.openlocfilehash: 97803c0496c7f5f6ea40e86023caad66c675037e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="what-usb-drivers-do-i-need-to-debug-android-on-windows"></a>Welche USB-Treiber ist zum Debuggen von Android unter Windows erforderlich?

## <a name="finding-usb-drivers"></a>Suchen von USB-Treiber

So debuggen Sie auf einem Android-Gerät bei der Entwicklung in Windows; Sie müssen einen kompatiblen USB-Treiber installieren. Android SDK Manager "Google-USB-Treiber" enthält, wird standardmäßig die Unterstützung für Nexus-Geräte hinzugefügt, wie hier beschrieben: [http://developer.android.com/sdk/win-usb.html](http://developer.android.com/sdk/win-usb.html)

Bei anderen Geräten müssen USB-Treiber, die vom Gerätehersteller speziell veröffentlicht. Einige Links für die häufigsten Hersteller sind in diesem Handbuch enthalten: [http://developer.android.com/tools/extras/oem-usb.html](http://developer.android.com/tools/extras/oem-usb.html)

## <a name="alternatives"></a>Alternativen

Je nach den Manfacturer kann es schwierig, Auffinden der genaue USB-Treiber erforderlich sein. Einige Alternativen für das Testen von Android-apps, die in Windows, einschließlich der Verwendung von Android-Emulator oder externe Tests Dienste entwickelt werden. Hierzu zählen:

- [Xamarin Test Cloud](https://xamarin.com/test-cloud) – Cloud Services ausführen auf Hunderten von real Android-Geräte zu testen.

- [Visual Studio-Emulator für Android](https://www.visualstudio.com/en-us/features/msft-android-emulator-vs.aspx)

- [Google Android SDK-Emulator](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

