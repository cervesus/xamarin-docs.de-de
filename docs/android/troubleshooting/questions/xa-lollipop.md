---
title: In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 63B6E10C-098D-4C82-9253-07CA62EA85A5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: d5edb5f4e2ce1ca39ba27a1de1a51760ea167e8b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70757132"
---
# <a name="what-version-of-xamarinandroid-added-lollipop-support"></a>In welcher Version von Xamarin.Android wurde Lollipop-Unterstützung hinzugefügt?

> [!NOTE]
> Dieses Handbuch wurde ursprünglich für die Vorschauversion von Android L geschrieben.

- [Xamarin. Android 4,17](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.17/index.md) hat Unterstützung für Android L Preview hinzugefügt.
- [Xamarin. Android 4,20](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/android/xamarin.android_4/xamarin.android_4.20/index.md) hat Unterstützung für Android Lollipop hinzugefügt.

Xamarin unterstützt nur aktiv die aktuelle stabile Version der xamarin-Tools. Die unten aufgeführten Informationen werden für ältere Versionen der Tools "unverändert" bereitgestellt. Die neuesten Informationen zu xamarin-Releases finden Sie [hier](http://releases.xamarin.com/).

## <a name="missing-androidjar-for-api-level-21-in-android-l-preview"></a>"Fehlende Android. jar für API-Ebene 21" in Android L Preview

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Möglicherweise wird die folgende Fehlermeldung (oder ähnlich) angezeigt:

```cmd
Error 1 Could not find android.jar for API Level 21.
```

Diese Meldung bedeutet, dass die Android SDK Plattform für API-Ebene 21 nicht installiert ist. Installieren Sie Sie entweder im Android SDK-Manager (**Tools > Android SDK Manager öffnen...** ), oder ändern Sie Ihr xamarin. Android-Projekt so, dass es auf eine installierte API-Version ausgerichtet ist.

Für dieses Problem gibt es einige Problem Umgehungen:

1. Ändern Sie das Projekt so, dass es auf die API 19 oder niedriger abzielt.

2. Benennen Sie Ihren Android-21-Ordner von Android-21 in Android-L um. (Am besten sollte dies nur als temporäre Korrektur verwendet werden, und es kann überhaupt nicht sehr gut funktionieren.)

   **%LOCALAPPDATA%\\Android\\android-sdk\\platforms\\android-21**

3. Temporäres Herabstufen auf Android-API-Ebene 21 "L" Preview [1]:

    1. Löschen Sie die **Plattformen\\\\\\"\\% LocalAppData% Android Android-SDK" (Android-21** ). 
    2. Extrahieren Sie [1] in **C\\:\\Benutzer&gt;\\\\\\Benutzername\\APPDATAlokale\\Android Android-SDK-Plattformen zum Erstellen&lt;** ein **Android-L-** Ordner.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Möglicherweise wird die folgende Fehlermeldung (oder ähnlich) angezeigt:

```bash
/Library/Frameworks/Mono.framework/External/xbuild/Xamarin/Android/Xamarin.Android.Common.targets: 
Error: Could not find android.jar for API Level 21.**
```

Dies bedeutet, dass die Android SDK Plattform für API-Ebene 21 nicht installiert ist. Installieren Sie Sie entweder im Android SDK-Manager (Tools > SDK-Manager...), oder ändern Sie das xamarin. Android-Projekt, sodass es auf eine installierte API-Version ausgerichtet ist.

Für dieses Problem gibt es einige Problem Umgehungen:

1. Ändern Sie das Projekt so, dass es auf die API 19 oder niedriger abzielt.

2. Benennen Sie Ihren Android-21-Ordner von Android-21 in Android-L um. (Am besten sollte dies nur als temporäre Korrektur verwendet werden, und es kann überhaupt nicht sehr gut funktionieren.)

   **~/Library/Developer/Xamarin/android-sdk-macosx/android-21**

3. Temporäres Herabstufen auf Android-API-Ebene 21 "L" Preview [1]:

    1. **/Users/username/Library/Developer/Xamarin/Android-SDK-MacOSX/Android-21** löschen
    2. Extrahieren Sie [1] in **/Users/username/Library/Developer/Xamarin/Android-SDK-MacOSX** , um einen **Android-L-** Ordner zu erstellen.

-----

[1] - [https://dl-ssl.google.com/android/repository/android-L_r04.zip](https://dl-ssl.google.com/android/repository/android-L_r04.zip)
