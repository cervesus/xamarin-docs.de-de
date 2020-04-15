---
title: Warum kann mein Android-Releasebuild keine Verbindung mit dem Internet herstellen?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/09/2018
ms.openlocfilehash: 5996cfa3c0a18fc186ea862a2b3d7910594e1281
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027012"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Warum kann mein Android-Releasebuild keine Verbindung mit dem Internet herstellen?

## <a name="cause"></a>Ursache

Die häufigste Ursache für dieses Problem besteht darin, dass die **INTERNET**-Berechtigung automatisch in einem Debugbuild enthalten ist, für einen Releasebuild aber manuell festgelegt werden muss. Dies liegt daran, dass die INTERNET-Berechtigung verwendet wird, um einem Debugger das Anfügen an den Prozess zu gestatten, wie [hier](~/android/deploy-test/building-apps/build-process.md) für „DebugSymbols“ beschrieben.

## <a name="fix"></a>Korrektur

Sie können die INTERNET-Berechtigung im Android-Manifest anfordern, um das Problem zu lösen. Dies kann entweder über den Manifest-Editor oder den Quellcode des Manifests erfolgen:

- In Editor beheben: Navigieren Sie in Ihrem Android-Projekt zu **Eigenschaften > AndroidManifest.xml > Erforderliche Berechtigungen**, und aktivieren Sie das Kontrollkästchen **Internet**.

- Im Quellcode beheben: Öffnen Sie das AndroidManifest in einem Quell-Editor, und fügen Sie das Berechtigungstag innerhalb der `<Manifest>`-Tags hinzu:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
