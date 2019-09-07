---
title: Welche Android SDK-Pakete sollte ich installieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/30/2018
ms.openlocfilehash: bd0f2a7704e5d666f6b32d4ccc489e069ec6ade6
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757254"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Welche Android SDK-Pakete sollte ich installieren?

Die Installation des Android SDK umfasst nicht automatisch alle Pakete, die für die Entwicklung erforderlich sind. Die folgenden Pakete sind in der Regel für die Entwicklung mit xamarin. Android erforderlich, während die einzelnen Entwickler benötigt werden:

## <a name="tools"></a>Tools

Installieren Sie die neuesten Tools aus dem Ordner Tools im SDK-Manager:

- Android SDK Tools
- Android SDK Platform-Tools
- Android SDK Build-Tools

## <a name="android-platforms"></a>Android-Plattform (en)

Installieren Sie die "SDK-Plattform" für die Android-Versionen, die Sie als minimal & Ziel festgelegt haben. 

Beispiele:

- Ziel-API 23
- Minimale API 23

Nur die SDK-Plattform für API 23 muss installiert werden.

- Ziel-API 23
- Minimale API 15

SDK-Plattformen für API 15 und 23 müssen installiert werden. Beachten Sie, dass Sie die API-Ebenen nicht zwischen dem minimal-und dem Ziel installieren müssen (selbst wenn Sie eine backportierung auf diese API-Ebenen durchführen).

## <a name="system-images"></a>System Abbilder

Diese sind nur erforderlich, wenn Sie die Standard-Android-Emulatoren von Google verwenden möchten. Weitere Informationen finden Sie unter [Android-Emulator Setup](~/android/get-started/installation/android-emulator/index.md) .

## <a name="extras"></a>Extras
Die Android SDK Extras sind in der Regel nicht erforderlich. Es ist jedoch hilfreich, diese zu beachten, da Sie abhängig von Ihrem Anwendungsfall möglicherweise erforderlich sind.

## <a name="further-reading"></a>Weiterführende Themen
Im folgenden Leitfaden werden diese Optionen behandelt und die verschiedenen Pakete, die der SDK-Manager bietet, ausführlicher erläutert: [Android SDK Manager-Setup Handbuch](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)
