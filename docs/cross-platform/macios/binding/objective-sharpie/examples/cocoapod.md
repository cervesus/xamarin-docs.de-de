---
title: Beispiel für die Verwendung von cocoapods in der Praxis
description: In diesem Dokument wird veranschaulicht, wie Sie mit dem Ziel-Sharpie die C# Bindungs Definitionen automatisch aus einer cocoapod generieren.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: conceptdev
ms.author: crdun
ms.date: 03/28/2018
ms.openlocfilehash: 0f730b1c0a0deacdb84c198cfe4af47308a268cc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290028"
---
# <a name="real-world-example-using-cocoapods"></a>Beispiel für die Verwendung von cocoapods in der Praxis

> [!NOTE]
> In diesem Beispiel wird die [afnetworking-cocoapod](https://cocoapods.org/pods/AFNetworking)verwendet.

Neu in Version 3,0, Ziel-Sharpie unterstützt die Bindung von cocoapods und umfasst sogar`sharpie pod`einen Befehl (), um das herunterladen, konfigurieren und Erstellen von cocoapods sehr einfach zu gestalten. Bevor Sie dieses Feature verwenden, sollten Sie [sich mit den cocoapods im allgemeinen vertraut machen](https://cocoapods.org) .

## <a name="creating-a-binding-for-a-cocoapod"></a>Erstellen einer Bindung für eine cocoapod

Der `sharpie pod` Befehl verfügt über eine globale Option und zwei Unterbefehle:

```bash
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

Der `init` Unterbefehl bietet auch eine hilfreiche Hilfe:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Es können mehrere cocoapod-Namen und untergeordnete Spezifikations `init`Namen für bereitgestellt werden.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

Nachdem Ihre cocoapod eingerichtet wurde, können Sie jetzt die Bindung erstellen:

```bash
$ sharpie pod bind
```

Dies führt dazu, dass das cocoapod-Xcode-Projekt erstellt und dann vom Ziel-Sharpie ausgewertet und analysiert wird. Es wird eine große Anzahl von Konsolen Ausgaben generiert, sollte jedoch zu der Bindungs Definition am Ende führen:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie die Dateien **ApiDefinitions.cs** und **StructsAndEnums.cs** erstellt haben, sehen Sie sich die folgende Dokumentation an, um eine Assembly zu generieren, die in ihren apps verwendet werden kann:

- [Bindungs Ziel-C (Übersicht)](~/cross-platform/macios/binding/overview.md)
- [Binden von Ziel-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Exemplarische Vorgehensweise: Binden einer IOS-Ziel-C-Bibliothek](~/ios/platform/binding-objective-c/walkthrough.md)
