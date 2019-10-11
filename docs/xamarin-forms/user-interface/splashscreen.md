---
title: Xamarin. Forms-Begrüßungsbildschirm
description: In diesem Artikel wird erläutert, wie Sie einen Begrüßungsbildschirm für eine xamarin. Forms-Anwendung erstellen.
ms.prod: xamarin
ms.assetId: 338C8F60-90F2-4B3D-BB47-7F846AE8DC6C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/1/2019
ms.openlocfilehash: 2624c3f35a9cfac122a0b36ea8c1684d30f57824
ms.sourcegitcommit: 5110d1279809a2af58d3d66cd14c78113bb51436
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2019
ms.locfileid: "72035773"
---
# <a name="xamarinforms-splash-screen"></a>Xamarin. Forms-Begrüßungsbildschirm

Anwendungen haben häufig eine Startverzögerung, während die Anwendung den Initialisierungs Prozess abschließt. Entwickler können ein Branding anbieten, das in der Regel als Begrüßungsbildschirm bezeichnet wird, während die Anwendung gestartet wird. In diesem Artikel wird erläutert, wie Sie Begrüßungs Bildschirme für xamarin. Forms-Anwendungen erstellen.

Xamarin. Forms wird auf jeder Plattform initialisiert, nachdem die systemeigene Startsequenz abgeschlossen wurde. Xamarin. Forms ist initialisiert:

- In der `OnCreate`-Methode der `MainActivity`-Klasse unter Android.
- In der `FinishedLaunching`-Methode der `AppDelegate`-Klasse unter IOS.
- In der `OnLaunched`-Methode der `App`-Klasse für die UWP.

Der Begrüßungsbildschirm sollte so schnell wie möglich angezeigt werden, wenn die Anwendung gestartet wird, xamarin. Forms wird jedoch erst nach dem Ende der Startsequenz initialisiert, was bedeutet, dass der Begrüßungsbildschirm außerhalb von xamarin. Forms auf jeder Plattform implementiert werden muss. In den folgenden Abschnitten wird erläutert, wie Sie auf jeder Plattform einen Begrüßungsbildschirm erstellen.

## <a name="xamarinforms-android-splash-screen"></a>Xamarin. Forms Android-Begrüßungsbildschirm

Das Erstellen eines Begrüßungs Bildschirms unter Android erfordert das Erstellen eines Begrüßungs `Activity` als `MainLauncher` mit einem speziellen Design. Sobald die Begrüßungs `Activity` gestartet ist, wird die Haupt `Activity` mit dem normalen Anwendungsdesign gestartet.

Weitere Informationen zu Begrüßungs Bildschirmen in xamarin. Android finden Sie unter [xamarin. Android Splash Screen](~/android/user-interface/splash-screen.md).

## <a name="xamarinforms-ios-splash-screen"></a>Xamarin. Forms-IOS-Begrüßungsbildschirm

Ein Begrüßungsbildschirm unter IOS wird als Startbildschirm bezeichnet. Zum Erstellen eines Startbildschirms unter IOS müssen Sie ein Storyboard erstellen, das die Benutzeroberfläche des Startbildschirms definiert, und dann das Storyboard als Startbildschirm in der **Info. plist**-Datei festlegen.

Weitere Informationen zu Start Bildschirmen in xamarin. IOS finden Sie unter [xamarin. IOS-Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md).

## <a name="xamarinforms-uwp-splash-screen"></a>Xamarin. Forms UWP-Begrüßungsbildschirm

Bei UWP enthält die Datei " **Package. appxmanifest** " eine Registerkarte **visueller Objekte** mit einem unter Menü des Begrüßungs **Bildschirms** . Die Begrüßungsbildschirm Grafiken können in diesem Menü angegeben werden:

[![Einstellung Begrüßungsbildschirm auf UWP](splashscreen-images/uwp-splashscreen-cropped.png)](splashscreen-images/uwp-splashscreen.png#lightbox)

## <a name="related-links"></a>Verwandte Links

- [Xamarin. Android-Begrüßungsbildschirm](~/android/user-interface/splash-screen.md)
- [Xamarin. IOS-Startbildschirm](~/ios/app-fundamentals/images-icons/launch-screens.md)
