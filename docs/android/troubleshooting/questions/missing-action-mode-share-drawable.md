---
title: 'Android.Support.v7.AppCompat - keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: Attr "Android: ActionModeShareDrawable"'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 07655587642c3e1aa94d035e76f6f6758340546d
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat - keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: Attr "Android: ActionModeShareDrawable"

1. Stellen Sie sicher, dass Sie die neuesten Extras als auch für die Android 5.0.x (API 21) über den Android SDK Manager-SDK herunterladen.

2. Stellen Sie sicher, dass Sie beim Kompilieren der Anwendungsstatus mit CompileSdkVersion auf 21 festgelegt. Sie können optional die TargetSdkVersion auf 21 ebenfalls festgelegt.

3. Wenn Sie eine frühere Version, z. B. API-19 benötigen, laden Sie die jeweilige Version finden Sie auf der Seite "Nuget" herunter:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Hinweis*: Wenn Sie manuell dies über die Paket-Manager-Konsole installieren, stellen Sie sicher, dass Sie auch die gleiche Version von Xamarin.Android.Support.v4 installieren

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow-Referenz: [http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](http://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Siehe auch

- [Welche Android SDK-Pakete sollte ich installieren?](~/android/troubleshooting/questions/install-android-sdk-packages.md)

