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
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027012"
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Warum kann mein Android-Releasebuild keine Verbindung mit dem Internet herstellen?

## <a name="cause"></a>Ursache

Die häufigste Ursache für dieses Problem ist, dass die **Internet** Berechtigung automatisch in einem Debugbuild enthalten ist, aber für einen Releasebuild manuell festgelegt werden muss. Dies liegt daran, dass die Internet-Berechtigung verwendet wird, um einem Debugger das Anfügen an den Prozess zu gestatten, wie [hier](~/android/deploy-test/building-apps/build-process.md)für "DebugSymbols" beschrieben.

## <a name="fix"></a>Korrektur

Um das Problem zu beheben, können Sie die Internet-Berechtigung im Android-Manifest anfordern. Dies kann entweder über den Manifest-Editor oder den Sourcecode des Manifests erfolgen:

- Behebung im Editor: Wechseln Sie in Ihrem Android-Projekt zu **Properties-> androidmanifest. XML-> erforderliche Berechtigungen** , und überprüfen Sie **Internet** .

- Behebung in Sourcecode: Öffnen Sie das "androidmanifest" in einem Quellen-Editor, und fügen Sie das Berechtigungs Kennzeichen in den `<Manifest>`-Tags ein:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
