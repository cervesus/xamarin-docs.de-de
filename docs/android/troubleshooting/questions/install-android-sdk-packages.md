---
title: Welche Android SDK-Pakete sollte ich installieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: 04a07d5b7f37222515136e5592f31a4583b02fe3
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107522"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Welche Android SDK-Pakete sollte ich installieren?

Der Android SDK-Installation enthalten nicht automatisch alle mindestens erforderlichen Pakete für die Entwicklung von. Während der einzelner Entwickler unterscheiden muss, werden die folgenden Pakete in der Regel für die Entwicklung mit Xamarin.Android erforderlich:

## <a name="tools"></a>Tools

Installieren Sie die neuesten Tools, aus dem Ordner "Tools" in den SDK-Manager:

- Android SDK Tools
- Android SDK Platform-Tools
- Android SDK-Buildtools

## <a name="android-platforms"></a>Android-Plattformen

Installieren Sie "SDK-Plattform", für die Android-Versionen, die Sie als Minimum & Ziel festgelegt haben. 

Beispiele:

- Ziel-API 23
- Mindest-API-23

Nur SDK-Plattform für API-23 installieren müssen

- Ziel-API 23
- Mindest-API-15

SDK-Plattformen für API 15 und 23 installieren müssen. Beachten Sie, dass Sie nicht benötigen, die API-Ebenen zwischen dem Minimum und dem Ziel installieren (selbst wenn Sie Backporting auf diese API-Ebenen sind).

## <a name="system-images"></a>System-Images

Diese sind nur erforderlich, wenn Sie die Out-of-the-Box-Android-Emulatoren von Google verwenden möchten. Weitere Informationen finden Sie unter [Setup von Android-Emulator](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Extras
Der Android SDK-Extras sind in der Regel nicht erforderlich. Es ist jedoch hilfreich, aufmerksam zu werden, da sie je nach Ihren Anwendungsfall erforderlich sein können.

## <a name="further-reading"></a>Weiterführende Themen
Der folgende Leitfaden behandelt diese Optionen und bietet weitere Informationen zu den verschiedenen Paketen im SDK-Manager verfügt über verfügbare: [Android SDK-Manager-Installationshandbuch](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

