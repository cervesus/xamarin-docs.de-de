---
title: Praktisches Beispiel mithilfe von CocoaPods
description: Dieses Dokument veranschaulicht, wie Objective Sharpie zu verwenden, um die Bindungsdefinitionen C# -Code aus einem CocoaPod automatisch zu generieren.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: bac34f662e24c6b08a67cd8da1f41b37b43b3faf
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855207"
---
# <a name="real-world-example-using-cocoapods"></a>Praktisches Beispiel mithilfe von CocoaPods

> [!NOTE]
> Dieses Beispiel verwendet die [AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Neues in Version 3.0, Ziel Sharpie unterstützt, binden CocoaPods und enthält auch einen Befehl (`sharpie pod`), herunterladen, konfigurieren und erstellen CocoaPods sehr einfach. Sie sollten [machen Sie sich mit CocoaPods](https://cocoapods.org) verwenden Sie diese Funktion in der Regel vor.

## <a name="creating-a-binding-for-a-cocoapod"></a>Erstellen eine Bindung für eine CocoaPod

Die `sharpie pod` Befehl verfügt über eine globale Option und zwei Unterbefehle:

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

Die `init` Unterbefehl verfügt auch über einige nützliche Tipps:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Mehrere CocoaPod-Namen und subspec kann angegeben werden, um `init`.

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

Sobald Ihre CocoaPod eingerichtet wurde, können Sie jetzt die Bindung erstellen:

```bash
$ sharpie pod bind
```

Dies führt in der CocoaPod-Xcode-Projekt wird erstellt und dann ausgewertet und vom Ziel Sharpie analysiert. Ein Großteil der Ausgabe der Konsole generiert werden, aber Sie sollten dazu führen, in der Bindungsdefinition am Ende:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Nächste Schritte

Nach dem Generieren der **ApiDefinitions.cs** und **StructsAndEnums.cs** -Dateien, sehen Sie sich die folgende Dokumentation zum Generieren eine Assembly, die in Ihren apps verwenden:

- [Übersicht über die Datenbindung Objective-C](~/cross-platform/macios/binding/overview.md)
- [Binden von Objective-C-Bibliotheken](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Exemplarische Vorgehensweise: Binden einer iOS Objective-C-Bibliothek](~/ios/platform/binding-objective-c/walkthrough.md)
- [Xamarin University-Kurs: Erstellen einer Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen einer Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
