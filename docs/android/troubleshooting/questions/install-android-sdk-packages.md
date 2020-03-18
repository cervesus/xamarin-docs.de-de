---
title: Welche Android SDK-Pakete sollte ich installieren?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: F136AAE0-C6D2-4B0F-8F8C-7A6A94877266
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/30/2018
ms.openlocfilehash: 24c70c2e869f59091a1519af6d1165dbea9cc467
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "76725060"
---
# <a name="which-android-sdk-packages-should-i-install"></a>Welche Android SDK-Pakete sollte ich installieren?

Bei der Installation des Android SDK werden nicht automatisch alle Pakete installiert, die für die Entwicklung mindestens erforderlich sind. Wenngleich die Anforderungen der verschiedenen Entwickler variieren, werden in der Regel die folgenden Pakete für die Entwicklung mit Xamarin.Android benötigt:

## <a name="tools"></a>Tools

Installieren Sie die neuesten Tools aus dem Ordner „Tools“ im SDK-Manager:

- Android SDK Tools
- Android SDK – Plattformtools
- Android SDK – Buildtools

## <a name="android-platforms"></a>Android-Plattform(en)

Installieren Sie die SDK-Plattform für die Android-Versionen, die Sie als Mindest- und als Zielversionen festgelegt haben.

Beispiele:

- Ziel-API: 23
- Mindest-API: 23

Sie müssen nur die SDK-Plattform für API 23 installieren.

- Ziel-API: 23
- Mindest-API: 15

Sie müssen die SDK-Plattformen für die API 15 und 23 installieren. Beachten Sie, dass Sie keine API-Ebenen zwischen der Mindest- und der Zielversion installieren müssen (selbst wenn Sie zu diesen API-Ebenen zurückportieren).

## <a name="system-images"></a>Systemimages

Diese sind nur erforderlich, wenn Sie die Android-Standardemulatoren von Google verwenden möchten. Weitere Informationen finden Sie unter [Setup von Android-Emulator](~/android/get-started/installation/android-emulator/index.md).

## <a name="extras"></a>Extras
Die Android SDK-Extras werden in der Regel nicht benötigt; je nach Anwendungsfall können sie jedoch erforderlich sein.
