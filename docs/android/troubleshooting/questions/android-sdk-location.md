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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027023"
---
# <a name="where-can-i-set-my-android-sdk-locations"></a>Wo kann ich meine Android SDK-Speicherorte festlegen?

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Navigieren Sie in Visual Studio zu Extras **> Optionen > xamarin > Android-Einstellungen** , um den Android SDK Speicherort anzuzeigen und festzulegen:

[![Registerkarte "Speicherorte" in den Einstellungen](android-sdk-location-images/win/01-locations-sml.png)](android-sdk-location-images/win/01-locations.png#lightbox)

Der Standard Speicherort für jeden Pfad lautet wie folgt:

- Java Development Kit-Speicherort: 

    **C:\\-Programmdateien\\Java\\JDK 1.8.0 _131**

- Android SDK Speicherort: 

    **C:\\Programme (x86)\\Android\\android-sdk**

- Android NDK-Speicherort: 

    **C:\\ProgramData\\Microsoft\\AndroidNDK64\\Android-NDK-r13b**

Beachten Sie, dass die Versionsnummer des NDK variieren kann. Anstelle von **Android-NDK-r13b**könnte es beispielsweise eine frühere Version sein, z. b. **Android-NDK-r10e**.

Um den Android SDK Speicherort festzulegen, geben Sie den vollständigen Pfad des Android SDK Verzeichnisses in das Feld **Android SDK Speicherort** ein. Sie können zum Android SDK Speicherort im Datei-Explorer navigieren, den Pfad aus der Adressleiste kopieren und diesen Pfad in das Feld **Android SDK Speicherort** einfügen.
Wenn Ihr Android SDK Speicherort z. b. " **C:\\Benutzer\\username\\APPDATA\\local\\Android\\SDK**lautet, löschen Sie den alten Pfad im Feld **Android SDK Speicherort** , und fügen Sie den Pfad ein. , und klicken Sie auf **OK**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Navigieren Sie in Visual Studio für Mac zu **Einstellungen > Projekten > SDK-Speicherorte > Android**. Klicken Sie auf der Seite **Android** auf die Registerkarte Speicher **Orte** , um den SDK-Speicherort anzuzeigen und festzulegen:

[![Registerkarte "Speicherorte" in den Einstellungen](android-sdk-location-images/mac/01-locations-sml.png)](android-sdk-location-images/mac/01-locations.png#lightbox)

Der Standard Speicherort für jeden Pfad lautet wie folgt:

- Android SDK Speicherort: 

    **~/Library/Developer/Xamarin/Android-SDK-MacOSX**

- Android NDK-Speicherort: 

    **~/Library/Developer/Xamarin/Android-NDK/Android-NDK-r14b**

- Java SDK (JDK)-Speicherort: 

    **/usr**

Beachten Sie, dass die Versionsnummer des NDK variieren kann. Anstelle von **Android-NDK-r14b**könnte es beispielsweise eine frühere Version sein, z. b. **Android-NDK-r10e**.

Um den Android SDK Speicherort festzulegen, geben Sie den vollständigen Pfad des Android SDK Verzeichnisses in das Feld **Android SDK Speicherort** ein. Sie können den Ordner Android SDK im Finder auswählen, **&#8984;STRG + + I** zum Anzeigen der Ordner Informationen drücken, auf den Pfad klicken und den Pfad rechts von **Where:** kopieren und dann auf der Registerkarte Speicher **Orte** in das Feld **Android SDK Speicherort** einfügen. Wenn Ihr Android SDK Speicherort z. b. den Wert **~/Library/Developer/Android/SDK**hat, löschen Sie den alten Pfad im Feld **Android SDK Speicherort** , fügen Sie diesen Pfad ein, und klicken Sie auf **OK**.

-----
