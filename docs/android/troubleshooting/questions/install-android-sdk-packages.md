---
title: Welche Android SDK-Paketen sollte ich installiere?
ms.topic: article
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/02/2018
ms.openlocfilehash: e2b0736a9ccc4dde5c1dcdf2d99f527247040560
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="which-android-sdk-packages-should-i-install"></a>Welche Android SDK-Paketen sollte ich installiere?

Installation des Android SDK hinzugefügt nicht alle die mindestens erforderlichen Pakete für die Entwicklung von automatisch. Während der einzelnen Entwickler unterschiedlich sein muss, werden die folgenden Pakete in der Regel für die Entwicklung mit Xamarin.Android erforderlich:

## <a name="tools"></a>Tools

Installieren Sie die neuesten Tools im Ordner "Extras" in den SDK-Manager:

- Android SDK-Tools
- Android SDK Platform-Tools
- Android SDK-Buildtools

## <a name="android-platforms"></a>Android-Plattformen

Installieren Sie die "SDK-Plattform" für die Android-Versionen, die Sie als Minimum & Ziel festgelegt haben. 

Beispiele:

- Ziel-API 23
- Minimale API 23

Nur für das SDK-Plattform für API 23 installieren müssen.

- Ziel-API 23
- Minimale API 15

Müssen Sie für die API 15 und 23 SDK-Plattformen zu installieren. Beachten Sie, dass Sie nicht benötigen, die API-Ebenen zwischen dem Minimum und dem Ziel installiert (auch wenn Sie Backporting auf diesen API-Ebenen befinden).

## <a name="system-images"></a>Von Betriebssystemabbildern
Diese sind nur erforderlich, wenn Sie die Out-of-Box-Android-Emulatoren von Google verwenden möchten. 

- [Den standardemulator konfigurieren](~/android/get-started/installation/android-emulator/index.md)

- [Gewusst wie: beschleunigen Sie die standardemulator](~/android/get-started/installation/android-emulator/index.md)

## <a name="extras"></a>Extras
Android SDK Extras sind in der Regel nicht erforderlich. Es ist jedoch hilfreich zu bekannt sein, da sie je nach Ihrer Anwendungsfall erforderlich sein können.

## <a name="further-reading"></a>Weiterführende Themen
Die folgenden Schritte werden diese Optionen beschrieben und wird ausführlicher Informationen zu den verschiedenen Paketen das SDK-Manager wurde verfügbar: [Android SDK Manager-Setup-Handbuch](http://www.themethodology.net/2015/02/android-sdk-manager-setup-for.html?m=1)

