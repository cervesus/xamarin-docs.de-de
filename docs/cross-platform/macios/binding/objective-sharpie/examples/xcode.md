---
title: Beispiel für eine reale Verwendung eines Xcode-Projekts
description: In diesem Dokument wird beschrieben, wie Sie ein Xcode-Projekt als direkte Eingabe für den Ziel-Sharpie verwenden, C# wodurch der Prozess zum Erstellen von Bindungen zum Ziel-C-Code vereinfacht wird.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: 16191b7ee5bae353a438340d6099d39a296bffd0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016232"
---
# <a name="real-world-example-using-an-xcode-project"></a>Beispiel für eine reale Verwendung eines Xcode-Projekts

**In diesem Beispiel wird die [Pop-Bibliothek von Facebook](https://github.com/facebook/pop)verwendet.**

Neu in Version 3,0 unterstützt das Ziel "Sharpie" Xcode-Projekte als Eingabe. Diese Projekte geben die richtigen Header Dateien und Compilerflags an, die zum Kompilieren der systemeigenen Bibliothek erforderlich sind, und müssen daher ebenfalls gebunden werden. Ziel-Sharpie wählt das erste _Ziel_ und seine Standardkonfiguration für ein Projekt aus, wenn es nicht zur anderen Anweisung aufgefordert wird.

Bevor das Ziel "Sharpie" versucht, die Projekt-und Header Dateien zu analysieren, muss es erstellt werden. Projekte verfügen häufig über buildphasen, die Header Dateien für die externe Nutzung und Integration ordnungsgemäß strukturieren. Daher empfiehlt es sich, immer das vollständige Projekt zu erstellen, bevor versucht wird, es zu binden.

```
$ git clone https://github.com/facebook/pop.git
Cloning into 'pop'...
   (more git clone output)

$ cd pop
$ sharpie bind pop.xcodeproj -sdk iphoneos9.0
```
