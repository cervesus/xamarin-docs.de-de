---
title: Warum herstellen kann nicht Mein Android-Releasebuild eine Verbindung mit dem Internet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: cd27d5c884086cd0fade4364851039fd0cd915a0
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117905"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Warum herstellen kann nicht Mein Android-Releasebuild eine Verbindung mit dem Internet?

## <a name="cause"></a>Ursache

Die häufigste Ursache für dieses Problem ist, die die **INTERNET** Berechtigung ist in einem Debugbuild automatisch enthalten, sondern muss manuell für einen Releasebuild festgelegt werden. Dies ist, da die Internet-Berechtigung verwendet wird, um einen Debugger Anfügen an den Prozess zu ermöglichen, für "DebugSymbols" beschriebene [hier](~/android/deploy-test/building-apps/build-process.md).


## <a name="fix"></a>Korrektur

Um das Problem zu beheben, können Sie die Internet-Berechtigung im Android-Manifest anfordern. Dies kann entweder über den manifest-Editor oder des Manifests Quellcode durchgeführt werden:

-   Beheben Sie in Editor: Wechseln Sie In Ihrem Android-Projekt zu **Eigenschaften -> "androidmanifest.xml" -> erforderliche Berechtigungen** und überprüfen Sie **Internet**

-   Beheben Sie im Quellcode: Öffnen Sie die AndroidManifest in ein Quellcode-Editor, und fügen Sie das Tag Berechtigung in den `<Manifest>` Tags:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
