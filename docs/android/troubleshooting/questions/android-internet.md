---
title: Warum herstellen kann nicht Mein Android Releasebuild mit dem Internet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: A6FE770B-A19A-4BF8-95E9-2CF880D4AFC5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/09/2018
ms.openlocfilehash: 7f956defd0243e1927746a53e6b3b1b05d98f8d2
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="why-cant-my-android-release-build-connect-to-the-internet"></a>Warum herstellen kann nicht Mein Android Releasebuild mit dem Internet?

## <a name="cause"></a>Ursache

Die häufigste Ursache für dieses Problem ist, die die **INTERNET** Berechtigung ist in einem Debugbuild automatisch enthalten, sondern muss manuell für einen Releasebuild festgelegt werden. Ursache hierfür ist die Internet-Berechtigung verwendet wird, um einen Debugger zum Anhängen an den Prozess zu ermöglichen, wie für "DebugSymbols" beschrieben [hier](~/android/deploy-test/building-apps/build-process.md).


## <a name="fix"></a>Problemlösung

Um das Problem zu beheben, können Sie die Internet-Berechtigung in der Android-Manifest benötigen. Dies kann entweder über ein manifest-Editor oder das Manifest Sourcecode erfolgen:

-   Beheben Sie in Editor: Wechseln Sie In Ihrer Android-Projekt zu **Eigenschaften -> AndroidManifest.xml Required Permissions ->** und überprüfen Sie **Internet**

-   Korrigieren in Sourcecode: Öffnen Sie die AndroidManifest in ein Quellcode-Editor, und fügen Sie das Tag Berechtigung innerhalb der `<Manifest>` Tags:

    ```xml
    <Manifest>
    ...
    <uses-permission android:name="android.permission.INTERNET" />
    </Manifest>
    ```
