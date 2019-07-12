---
title: In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: db0cb221ee18998fbb98449ae03c442c84bcfcd7
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67829705"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?

> [!NOTE]
> Dieses Handbuch wurde ursprünglich für die Vorschau für Android L geschrieben.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) Android L-Vorschau-Unterstützung hinzugefügt.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) Android Lollipop-Unterstützung hinzugefügt.

Xamarin unterstützt nur die aktuelle stabile Version der Xamarin-Tools. Die folgenden Informationen wird bereitgestellt "als-ist" für ältere Versionen der Tools. Überprüfen Sie die neuesten Informationen zu Xamarin-Versionen, [hier](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>"Fehlender android.jar für API-Ebene 21" in der Android-L-Vorschau

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Die folgende Fehlermeldung angezeigt (oder ähnlich) unter Umständen angezeigt:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Diese Meldung bedeutet, dass die Android SDK-Plattform für die API Level 21 nicht installiert ist. Installieren Sie sie in der Android SDK Manager (**Tools > Open Android SDK-Manager...** ), oder ändern Sie Ihr Xamarin.Android-Projekt, um eine API-Version als Ziel festzulegen, die installiert ist.

Es gibt einige problemumgehungen zur Behebung dieses Problems:

1. Ändern Sie Ihr Projekt so, dass das Ziel, dass API-19 oder zu verringern.

2. Benennen Sie Ihr Android-21-Ordner von Android-21 auf Android-L. (Im besten Fall Dies sollte nur als temporäre verwendet werden, und es möglicherweise überhaupt keine sehr gut.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Vorübergehend ein downgrade zurück auf die Vorschau des Android API Level 21 "L" [1]:

    1.  Löschen der **%LocalAppData%\\Android\\Android-Sdk\\Plattformen\\Android-21** 
    2.  Extrahieren Sie in [1] **C:\\Benutzer\\&lt;Benutzername&gt;\\AppData\\lokalen\\Android\\Android-Sdk\\Plattformen**  zum Erstellen einer **Android-l** Ordner.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Die folgende Fehlermeldung angezeigt (oder ähnlich) unter Umständen angezeigt:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Dies bedeutet, dass die Android SDK-Plattform für die API Level 21 nicht installiert ist. Installieren Sie sie in der Android SDK Manager (Extras > SDK-Manager...), oder ändern Sie Ihr Xamarin.Android-Projekt, um eine API-Version als Ziel festzulegen, die installiert ist.

Es gibt einige problemumgehungen zur Behebung dieses Problems:

1. Ändern Sie Ihr Projekt so, dass das Ziel, dass API-19 oder zu verringern.

2. Benennen Sie Ihr Android-21-Ordner von Android-21 auf Android-L. (Im besten Fall Dies sollte nur als temporäre verwendet werden, und es möglicherweise überhaupt keine sehr gut.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Vorübergehend ein downgrade zurück auf die Vorschau des Android API Level 21 "L" [1]:

    1.  Löschen Sie **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  Extrahieren Sie in [1] **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** zum Erstellen einer **Android-l** Ordner.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
