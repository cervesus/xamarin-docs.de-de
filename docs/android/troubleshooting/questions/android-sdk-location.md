---
title: Wo kann ich meine Android SDK-Speicherorte festlegen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 11/16/2017
ms.openlocfilehash: 8685be4bb1cc45ff04dc8d9f7d8e64e7b1483b60
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027023"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Wo kann ich meine Android SDK-Speicherorte festlegen?

# <a name="visual-studio"></a>[Visual Studio](#tab/windows)

Navigieren Sie in Visual Studio zu **Extras > Optionen > Xamarin > Android-Einstellungen**, um den Speicherort des Android SDK anzuzeigen und festzulegen:

[![Beispiel für Registerkarte „Speicherorte“ in den Einstellungen](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Der Standardspeicherort für die einzelnen Pfade lautet wie folgt:

- Speicherort für das Java Development Kit: 

    **C:\\Programme\\Java\\jdk1.8.0_131**

- Speicherort für das Android SDK: 

    **C:\\Programme (x86)\\Android\\android-sdk**

- Speicherort für das Android NDK: 

    **C:\\Programme\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

Beachten Sie, dass die Versionsnummer des NDK abweichen kann. Beispielsweise könnte es sich statt um **android-ndk-r13b** um eine frühere Version wie **android-ndk-r10e** handeln.

Um den Speicherort für das Android SDK festzulegen, geben Sie im Feld **Android SDK-Speicherort** den vollständigen Pfad des Android SDK-Verzeichnisses ein. Sie können im Datei-Explorer zum Speicherort des Android SDK navigieren, den Pfad aus der Adressleiste kopieren und diesen Pfad in das Feld **Android SDK-Speicherort** einfügen.
Wenn Ihr Android SDK beispielsweise unter **C:\\Benutzer\\Benutzername\\AppData\\Local\\Android\\Sdk** gespeichert ist, löschen Sie den bisherigen Pfad im Feld **Android SDK-Speicherort**, fügen Sie den neuen Pfad ein, und klicken Sie auf **OK**.

# <a name="visual-studio-for-mac"></a>[Visual Studio für Mac](#tab/macos)

Navigieren Sie in Visual Studio für Mac zu **Einstellungen > Projekte > SDK-Speicherorte > Android**. Klicken Sie auf der Seite **Android** auf die Registerkarte **Speicherorte**, und legen Sie den Speicherort für das SDK fest:

[![Beispiel für Registerkarte „Speicherorte“ in den Einstellungen](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Der Standardspeicherort für die einzelnen Pfade lautet wie folgt:

- Speicherort für das Android SDK: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Speicherort für das Android NDK: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Speicherort für das Java SDK (JDK): 

    **/usr**

Beachten Sie, dass die Versionsnummer des NDK abweichen kann. Beispielsweise könnte es sich statt um **android-ndk-r14b** um eine frühere Version wie **android-ndk-r10e** handeln.

Um den Speicherort für das Android SDK festzulegen, geben Sie im Feld **Android SDK-Speicherort** den vollständigen Pfad des Android SDK-Verzeichnisses ein. Sie können den Android SDK-Ordner im Finder auswählen, zum Anzeigen von Ordnerinformationen **CTRL+&#8984;+I** drücken, auf den Pfad klicken und ihn rechts neben **Wo:** ziehen, den Pfad kopieren und ihn dann in das Feld **Android SDK-Speicherort** auf der Registerkarte **Speicherorte** einfügen. Wenn Ihr Android SDK beispielsweise unter **~/Library/Developer/Android/Sdk** gespeichert ist, löschen Sie den bisherigen Pfad im Feld **Android SDK-Speicherort**, fügen Sie den neuen Pfad ein, und klicken Sie auf **OK**.

-----
