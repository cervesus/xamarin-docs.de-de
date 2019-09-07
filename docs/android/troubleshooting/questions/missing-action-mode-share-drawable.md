---
title: "Android.Support.v7.AppCompat: Keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: attr 'android:actionModeShareDrawable'"
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 5814069C-FC43-41DE-B5A5-024D05E59929
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: b8961c9e58d4336a952649ce8181ca6ebdfe3165
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757222"
---
# <a name="androidsupportv7appcompat---no-resource-found-that-matches-the-given-name-attr-androidactionmodesharedrawable"></a>Android.Support.v7.AppCompat: Keine Ressource gefunden, die mit dem angegebenen Namen übereinstimmt: attr 'android:actionModeShareDrawable'

1. Stellen Sie sicher, dass Sie die neuesten Extras und das Android 5,0 (API 21) SDK über den Android SDK-Manager herunterladen.

2. Stellen Sie sicher, dass Sie die Anwendung kompilieren, bei der compilesdkversion auf 21 festgelegt ist. Optional können Sie auch targetdkversion auf 21 festlegen.

3. Wenn Sie eine frühere Version wie API 19 benötigen, laden Sie die entsprechende Version auf der nuget-Seite herunter:

[https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/](https://www.nuget.org/packages/Xamarin.Android.Support.v7.AppCompat/)

*Hinweis:* Wenn Sie diese manuell über die Paket-Manager-Konsole installieren, stellen Sie sicher, dass Sie auch dieselbe Version von xamarin. Android. Support. v4 installieren.

[https://www.nuget.org/packages/Xamarin.Android.Support.v4/](https://www.nuget.org/packages/Xamarin.Android.Support.v4/)

Stack Overflow Verweis:[https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro](https://stackoverflow.com/questions/26431676/appcompat-v721-0-0-no-resource-found-that-matches-the-given-name-attr-andro)

## <a name="see-also"></a>Siehe auch

- [Welche Android SDK-Pakete sollte ich installieren?](~/android/troubleshooting/questions/install-android-sdk-packages.md)
