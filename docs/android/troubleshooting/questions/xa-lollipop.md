---
title: In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 9e36189c771ed0c91a6030fd0ab615ab9af4dd52
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73026716"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?

> [!NOTE]
> Dieser Leitfaden wurde ursprünglich für die Android L-Vorschau geschrieben.

- In [Xamarin.Android 4.17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md) wurde die Unterstützung für die Android L-Vorschau hinzugefügt.
- In [Xamarin.Android 4.20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md) wurde die Unterstützung für Android Lollipop hinzugefügt.

Xamarin unterstützt ausschließlich die aktuelle stabile Version der Xamarin-Tools. Die unten aufgeführten Informationen werden für ältere Toolversionen unverändert bereitgestellt. Die neuesten Informationen zu Xamarin-Releases finden Sie in den [Versionshinweisen](https://docs.microsoft.com/xamarin/whats-new/#product-release-notes).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>„Fehlende android.jar-Datei für die API-Ebene 21“ in der Android L-Vorschau

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Möglicherweise wird die folgende Fehlermeldung (oder eine ähnliche) angezeigt:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Diese Meldung bedeutet, dass die Android SDK-Plattform für die API-Ebene 21 nicht installiert ist. Installieren Sie sie im Android-SDK-Manager (**Tools > Android-SDK-Manager öffnen...** ), oder ändern Sie Ihr Xamarin.Android-Projekt, sodass die installierte API-Version angezielt wird.

Es gibt mehrere Lösungen für dieses Problem:

1. Bearbeiten Sie Ihr Projekt, sodass es für API 19 oder niedriger ausgelegt ist.

2. Benennen Sie Ihren android-21-Ordner um, sodass er „android-L“ und nicht mehr „android-21“ heißt. (Dies ist nur eine temporäre Lösung und funktioniert möglicherweise nicht sehr gut.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Führen Sie ein temporäres Downgrade zur „L“-Vorschau der Android-API-Ebene 21 durch [1]:

    1. Löschen Sie **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**. 
    2. Extrahieren Sie [1] in den Pfad **C:\\Users\\&lt;username&gt;\\AppData\\Local\\Android\\android-sdk\\platforms**, um einen **android-L**-Ordner zu erstellen.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Möglicherweise wird die folgende Fehlermeldung (oder eine ähnliche) angezeigt:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Das bedeutet, dass die Android SDK-Plattform für API-Ebene 21 nicht installiert ist. Installieren Sie sie im Android-SDK-Manager (Extras > SDK-Manager...), oder ändern Sie Ihr Xamarin.Android-Projekt so, dass es auf eine API-Version verweist, die installiert ist.

Es gibt mehrere Lösungen für dieses Problem:

1. Bearbeiten Sie Ihr Projekt, sodass es für API 19 oder niedriger ausgelegt ist.

2. Benennen Sie Ihren android-21-Ordner um, sodass er „android-L“ und nicht mehr „android-21“ heißt. (Dies ist nur eine temporäre Lösung und funktioniert möglicherweise nicht sehr gut.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Führen Sie ein temporäres Downgrade zur „L“-Vorschau der Android-API-Ebene 21 durch [1]:

    1. Löschen Sie **/Users/username/Library/Developer/Xamarin/android-sdk-macosx/android-21**.
    2. Extrahieren Sie [1] in **/Users/username/Library/Developer/Xamarin/android-sdk-macosx**, um einen **android-L**-Ordner zu erstellen.

-----

[1] – [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
