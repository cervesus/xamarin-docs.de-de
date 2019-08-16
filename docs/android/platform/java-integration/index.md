---
title: Java-Integration mit xamarin. Android
description: Das Java-Ökosystem umfasst eine vielfältige und immense Sammlung von Komponenten. Viele dieser Komponenten können verwendet werden, um die Zeit zu verkürzen, die für die Entwicklung einer Android-Anwendung benötigt wird. Dieses Dokument bietet eine allgemeine Übersicht über einige Möglichkeiten, wie Entwickler diese vorhandenen Java-Komponenten verwenden können, um Ihre xamarin. Android-Anwendungsentwicklung zu verbessern.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: 22c3217eec1b2e531ad4534fc1cb35a701a06e34
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69524088"
---
# <a name="java-integration-with-xamarinandroid"></a>Java-Integration mit xamarin. Android

_Das Java-Ökosystem umfasst eine vielfältige und immense Sammlung von Komponenten. Viele dieser Komponenten können verwendet werden, um die Zeit zu verkürzen, die für die Entwicklung einer Android-Anwendung benötigt wird. Dieses Dokument bietet eine allgemeine Übersicht über einige Möglichkeiten, wie Entwickler diese vorhandenen Java-Komponenten verwenden können, um Ihre xamarin. Android-Anwendungsentwicklung zu verbessern._

## <a name="overview"></a>Übersicht

Angesichts des Umfangs des Java-Ökosystems ist es sehr wahrscheinlich, dass eine für eine xamarin. Android-Anwendung erforderliche Funktionalität bereits in Java codiert wurde. Daher ist es attraktiv, diese vorhandenen Bibliotheken beim Erstellen einer xamarin. Android-Anwendung wiederzuverwenden.

Es gibt drei Möglichkeiten zur Wiederverwendung von Java-Bibliotheken in einer xamarin. Android-Anwendung: 

- **Erstellen einer Bibliothek für Java-Bindungen** Bei dieser Technik wird ein xamarin. Android-Projekt verwendet, um Wrapper für die Java-Typen zu erstellen C# &ndash; Eine xamarin. Android-Anwendung kann dann auf C# die Wrapper verweisen, die von diesem Projekt erstellt wurden, `.jar` und dann die Datei verwenden. 

- **Native Java-Schnittstelle** C++ C# Die Java Native Interface (JNI) ist ein Framework, das es ermöglicht, dass nicht-Java-Code (z. b. oder) von Java-Code aufgerufen oder aufgerufen wird, der in einer JVM ausgeführt wird. &ndash; 

- **Portieren des Codes** Bei dieser Methode wird der Java-Quellcode übernommen und anschließend in C#umgerechnet. &ndash; Dies kann manuell oder mithilfe eines automatisierten Tools wie z. b. "schärfen" erfolgen. 

Die ersten beiden Techniken sind die *Java Native Interface* (JNI). Jni ist ein Framework, mit dem Anwendungen, die nicht in Java geschrieben sind, mit Java-Code interagieren können, der in einem Java Virtual Machine ausgeführt wird Xamarin. Android verwendet JNI, um *Bindungen* für C# Code zu erstellen. 

Das erste Verfahren ist ein automatisierender, deklarativer Ansatz für die Bindung von Java-Bibliotheken. Dabei wird entweder Visual Studio für Mac oder ein Visual Studio-Projekttyp verwendet, der von xamarin &ndash; . Android der Java-Bindungs Bibliothek bereitgestellt wird. Um diese Bindungen erfolgreich zu erstellen, sind möglicherweise trotzdem einige manuelle Änderungen erforderlich, aber nicht so viele wie ein reiner jni-Ansatz. Weitere Informationen zu Java-Bindungs Bibliotheken finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md) . 

Die zweite Methode, die jni verwendet, funktioniert auf sehr niedriger Ebene, bietet aber eine präzisere Steuerung und den Zugriff auf Java-Methoden, auf die normalerweise nicht über eine Java-Bindungs Bibliothek zugegriffen werden kann. 

Das dritte Verfahren unterscheidet sich grundlegend von den vorherigen beiden: Portieren des Codes von C#Java auf. Das Portieren von Code von einer Sprache in eine andere kann ein sehr aufwändiger Prozess sein, aber es ist möglich, diesen Aufwand mit der Hilfe eines Tools namens " *Sharpen*" zu verringern. Sharpen ist ein Open Source-Tool, das ein Java-toC# -Converter-Tool ist. 



## <a name="summary"></a>Zusammenfassung

Dieses Dokument bietet einen Überblick über die verschiedenen Methoden, mit denen Bibliotheken aus Java in einer xamarin. Android-Anwendung wieder verwendet werden können. Es wurden die Konzepte von Bindungen und verwalteten Callable Wrapper vorgestellt, und es wurden die Optionen zum Portieren von C#Java-Code in erläutert. 


## <a name="related-links"></a>Verwandte Links

- [Architektur](~/android/internals/architecture.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [Arbeiten mit jni](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Native Java-Schnittstelle](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
