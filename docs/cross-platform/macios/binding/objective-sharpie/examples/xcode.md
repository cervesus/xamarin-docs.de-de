---
title: Real-World-Beispiel unter Verwendung eines Xcode-Projekts
description: In diesem Dokument wird beschrieben, wie die Verwendung ein Xcode-Projekts als eine direkte Eingabe, Ziel-Sharpie, vereinfacht den Prozess der Erstellung C# -Bindungen mit Objective-C-Code.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: ccfc2f1760d8971e2d824cf65344fa2a5e158c12
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64978371"
---
# <a name="real-world-example-using-an-xcode-project"></a>Real-World-Beispiel unter Verwendung eines Xcode-Projekts

**Dieses Beispiel verwendet die [POP-Bibliothek von Facebook](https://github.com/facebook/pop).**

In Version 3.0 unterstützt Objective Sharpie neue Xcode-Projekte als Eingabe. Diese Projekte Geben Sie den richtigen Headerdateien und Compilerflags erforderlich, um die native Bibliothek zu kompilieren, und daher erforderlich, um ihn zu binden. Objektive Sharpie wählt das erste _Ziel_ und seine Standardkonfiguration eines Projekts aus, wenn Sie keine nicht angewiesen.

Vor dem Ziel Sharpie versucht, die die Projekt und Header-Dateien zu analysieren, müssen sie es erstellt werden. Projekte haben häufig buildphasen, die ordnungsgemäß die Headerdateien für die externe Nutzung und Integration, Struktur werden daher wird empfohlen, immer das vollständige Projekt erstellen, bevor Sie versuchen, bindet, bindet es an.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

