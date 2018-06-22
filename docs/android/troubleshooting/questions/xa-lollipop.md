---
title: Welche Version der Xamarin.Android Lollipop-Unterstützung hinzugefügt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 065c68a373f67bb352b59dc88ef89daec8b51ef8
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30764464"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>Welche Version der Xamarin.Android Lollipop-Unterstützung hinzugefügt?

**Hinweis:** dieses Handbuch wurde ursprünglich für die Vorschau Android L geschrieben.

-   [Xamarin.Android 4.17](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.17/) Android L-Vorschau-Unterstützung hinzugefügt.
-   [Xamarin.Android 4.20](https://developer.xamarin.com/releases/android/xamarin.android_4/xamarin.android_4.20/) Android Lollipop-Unterstützung hinzugefügt.

Xamarin unterstützt nur die aktuelle stabile Version der Xamarin-Tools. Die folgenden Informationen wird bereitgestellt "als-ist" für ältere Versionen der Tools. Überprüfen Sie die neuesten Informationen zu Xamarin Versionen [hier](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>"Fehlt android.jar für API-Ebene 21" in der Android-L-Vorschau

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Die folgende Fehlermeldung (oder ähnlich) angezeigt:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Diese Meldung bedeutet, dass die Android-SDK-Plattform für die API-Ebene 21 nicht installiert ist. Installieren sie Sie im Android SDK Manager (Extras > Öffnen Android SDK-Manager...), oder ändern Sie Ihr Projekt Xamarin.Android, um die API-Version als Ziel, die installiert ist.

Es gibt einige problemumgehungen für dieses Problem:

1. Ändern Sie Ihr Projekt, damit es als Ziel API-19 verwendet, oder verringern.

2. Benennen Sie Ihr Android-21-Ordner von Android-21 auf Android-L. (Bestenfalls, dies sollte nur als temporäre Korrektur verwendet werden, und er funktioniert möglicherweise nicht sehr gut überhaupt.)

   **%LocalAppData%\\Android\\Android-Sdk\\Plattformen\\Android-21**

3. Ein downgrade vorübergehend wieder auf die Android-API-Ebene 21 "L"-Preview [1]:

    1.  Löschen der **%LocalAppData%\\Android\\Android-Sdk\\Plattformen\\Android-21** 
    2.  [1] in extrahieren **"c:"\\Benutzer\\<username>\\AppData\\lokale\\Android\\Android-Sdk\\Plattformen** zum Erstellen einer **Android-l** Ordner.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Die folgende Fehlermeldung (oder ähnlich) angezeigt:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Dies bedeutet, dass die Android-SDK-Plattform für die API-Ebene 21 nicht installiert ist. Installieren sie Sie im Android SDK Manager (Extras > SDK-Manager...), oder ändern Sie Ihr Projekt Xamarin.Android, um die API-Version als Ziel, die installiert ist.

Es gibt einige problemumgehungen für dieses Problem:

1. Ändern Sie Ihr Projekt, damit es als Ziel API-19 verwendet, oder verringern.

2. Benennen Sie Ihr Android-21-Ordner von Android-21 auf Android-L. (Bestenfalls, dies sollte nur als temporäre Korrektur verwendet werden, und er funktioniert möglicherweise nicht sehr gut überhaupt.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Ein downgrade vorübergehend wieder auf die Android-API-Ebene 21 "L"-Preview [1]:

    1.  Löschen Sie **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**
    2.  [1] in extrahieren **/Users/username/Library/Developer/Xamarin/android-sdk-macosx** zum Erstellen einer **Android-l** Ordner.

-----


[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
