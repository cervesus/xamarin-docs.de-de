---
title: Wo kann ich meine Android SDK-Speicherorte festlegen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 6A9DE6E9-3E27-4DD2-87D2-34E95E5D401C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 11/16/2017
ms.openlocfilehash: c004fb7717f78750e7ac1e8dc1856a32ba808638
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61227673"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Wo kann ich meine Android SDK-Speicherorte festlegen?

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Wechseln Sie in Visual Studio zu **Tools > Optionen > Xamarin > Android-Einstellungen** anzeigen, und legen Sie den Android SDK-Speicherort:

[![Beispiel für Registerkarte "Speicherorte" in den Voreinstellungen](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Der Standardspeicherort für jeden Pfad lautet wie folgt aus:

- Java Development Kit-Speicherort: 

    **"C:"\\Programmdateien\\Java\\Jdk1.8.0_131**

- Android SDK-Speicherort: 

    **C:\\Programme (x86)\\Android\\android-sdk**

- Android NDK-Speicherort: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\android-ndk-r13b**

Beachten Sie, dass die Versionsnummer des das NDK variieren kann. Beispielsweise anstelle von **Android-Ndk-r13b**, möglicherweise eine frühere Version wie z. B. **Android-Ndk-r10e**.

Um den Android SDK-Speicherort festzulegen, geben Sie den vollständigen Pfad des Android SDK-Verzeichnis in der **Android SDK-Speicherort** Feld. Sie können zum Android SDK-Speicherort im Datei-Explorer navigieren, kopieren Sie den Pfad aus der Adressleiste und fügen Sie diesem Pfad in der **Android SDK-Speicherort** Feld.
Wenn Ihre Android SDK-Speicherort ist z. B. **"c:"\\Benutzer\\Benutzername\\AppData\\lokalen\\Android\\Sdk**, deaktivieren Sie den alten Pfad in der  **Android SDK-Speicherort** Feld, fügen Sie diesen Pfad aus, und klicken Sie auf **OK**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Wechseln Sie in Visual Studio für Mac zu **Voreinstellungen > Projekte > SDK-Speicherorte > Android**. In der **Android** klicken Sie auf die **Speicherorte** Registerkarte anzeigen und Festlegen der SDK-Speicherort:

[![Beispiel für Registerkarte "Speicherorte" in den Voreinstellungen](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Der Standardspeicherort für jeden Pfad lautet wie folgt aus:

- Android SDK-Speicherort: 

    **~/Library/Developer/Xamarin/android-sdk-macosx**

- Android NDK-Speicherort: 

    **~/Library/Developer/Xamarin/android-ndk/android-ndk-r14b**

- Der Speicherort der Java-SDK (JDK): 

    **/usr**

Beachten Sie, dass die Versionsnummer des das NDK variieren kann. Beispielsweise anstelle von **Android-Ndk-r14b**, möglicherweise eine frühere Version wie z. B. **Android-Ndk-r10e**.

Um den Android SDK-Speicherort festzulegen, geben Sie den vollständigen Pfad des Android SDK-Verzeichnis in der **Android SDK-Speicherort** Feld. Sie können den Android SDK-Ordner wählen Sie im Finder, drücken Sie **STRG +&#8984;+ I** um Ordner Informationen anzuzeigen, klicken Sie auf, und ziehen Sie den Pfad auf der rechten Seite des **, in denen:**, kopieren Sie aus, und fügen Sie ihn in die **Android SDK Speicherort** im Feld der **Speicherorte** Registerkarte. Wenn Ihre Android SDK-Speicherort ist z. B. **~/Library/Developer/Android/Sdk**, deaktivieren Sie den alten Pfad in der **Android SDK-Speicherort** Feld, fügen Sie diesen Pfad aus, und klicken Sie auf **OK**.

-----
