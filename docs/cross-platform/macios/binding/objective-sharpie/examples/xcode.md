---
title: Real-World-Beispiel für die Verwendung von Xcode-Projekt
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: a90d2fdee353057126fe804a4c20032dad1a3057
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="real-world-example-using-an-xcode-project"></a>Real-World-Beispiel für die Verwendung von Xcode-Projekt


**Dieses Beispiel verwendet die [POP-Bibliothek von Facebook](https://github.com/facebook/pop).**

In Version 3.0 unterstützt Ziel Sharpie neue Xcode-Projekte als Eingabe. Diese Projekte Geben Sie den richtigen Header-Dateien und Compilerflags erforderlich, um die systemeigene Bibliothek zu kompilieren und somit erforderlich, um es zu binden. Objektive Sharpie wählt das erste _Ziel_ und die Standardkonfiguration eines Projekts, wenn Sie keine nicht angewiesen.

Vor dem Ziel Sharpie versucht, die Projekt- und Header-Dateien zu analysieren, muss es erstellt werden. Projekte haben häufig Build-Phasen, die ordnungsgemäß Headerdateien für externe Ressourcenverbrauch und Integration, Struktur werden daher ist es am besten, immer das vollständige Projekt zu erstellen, bevor Sie versuchen, ihn zu binden.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

