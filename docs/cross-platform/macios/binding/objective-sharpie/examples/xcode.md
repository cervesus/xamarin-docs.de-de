---
title: Real-World-Beispiel unter Verwendung eines Xcode-Projekts
description: In diesem Dokument wird beschrieben, wie die Verwendung ein Xcode-Projekts als eine direkte Eingabe, Ziel-Sharpie, die das Erstellen von C#-Bindungen in Objective-C-Code vereinfacht wird.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855246"
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

## <a name="related-links"></a>Verwandte Links

- [Xamarin University-Kurs: Erstellen einer Bibliothek für Objective-C-Bindungen](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Xamarin University-Kurs: Erstellen einer Bibliothek Objective-C-Bindungen mit objektive Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)