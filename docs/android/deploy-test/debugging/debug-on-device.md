---
title: Debugvorgang auf einem Gerät
description: In diesem Artikel wird das Debuggen einer Xamarin.Android-Anwendung auf einem physischen Android-Gerät erläutert.
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 3ca524e451a7a4eb838805c839b33c4b9dd6bddd
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "75556534"
---
# <a name="debug-on-an-android-device"></a>Auf einem Android-Gerät debuggen

_In diesem Artikel wird das Debuggen einer Xamarin.Android-Anwendung auf einem physischen Android-Gerät erläutert._

Es ist möglich, eine Xamarin.Android-Anwendung auf einem Android-Gerät mit Visual Studio für Mac oder Visual Studio zu debuggen. Bevor ein Gerät debuggt werden kann, muss es [für die Entwicklung vorbereitet](~/android/get-started/installation/set-up-device-for-development.md), und mit Ihrem PC oder Mac verbunden werden.

## <a name="debug-application"></a>Debuggen der Anwendung

Nachdem ein Gerät mit dem Computer verbunden ist, erfolgt auf die gleiche Weise wie bei anderen Xamarin-Produkten oder .NET-Anwendungen das Debuggen einer Xamarin.Android-Anwendung. Stellen Sie sicher, dass die **Debug**-Konfiguration und das externe Gerät in der IDE ausgewählt werden. Dadurch wird sichergestellt, dass die erforderlichen Debugsymbole verfügbar sind, und dass die IDE eine Verbindung mit der ausgeführten Anwendung herstellen kann: 

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

![Debugkonfiguration ausgewählt](debug-on-device-images/image1-vs.png)

Als nächstes wird ein Haltepunkt im Code festgelegt:

![Haltepunkt in der Codezeile festgelegt](debug-on-device-images/image2-vs.png)

Nachdem das Gerät ausgewählt wurde, wird Xamarin.Android eine Verbindung mit dem Gerät herstellen, die Anwendung bereitstellen und sie dann ausführen. Wenn der Haltepunkt erreicht wird, hält der Debugger die Anwendung an. Die Anwendung kann ähnlich wie andere C#-Anwendungen debuggt werden: 

![Haltepunkt erreicht](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

![Debugkonfiguration ausgewählt](debug-on-device-images/image1-xs.png)

Als nächstes wird ein Haltepunkt im Code festgelegt:

![Haltepunkt in der Codezeile festgelegt](debug-on-device-images/image2-xs.png)

Nachdem das Gerät ausgewählt wurde, wird Xamarin.Android eine Verbindung mit dem Gerät herstellen, die Anwendung bereitstellen und sie dann ausführen. Wenn der Haltepunkt erreicht wird, hält der Debugger die Anwendung an. Die Anwendung kann ähnlich wie andere C#-Anwendungen debuggt werden: 

![Haltepunkt erreicht](debug-on-device-images/image3-xs.png)

-----

## <a name="summary"></a>Zusammenfassung

In diesem Dokument wird erläutert, wie eine Xamarin.Android-Anwendung durch das Festlegen eines Haltepunkts und das Auswählen eines Zielgeräts debuggt wird.

## <a name="related-links"></a>Verwandte Links

- [Set Up Device for Development (Einrichten eines Geräts für die Entwicklung)](~/android/get-started/installation/set-up-device-for-development.md)
- [Festlegen des Debuggable-Attributs](~/android/deploy-test/debuggable-attribute.md)
