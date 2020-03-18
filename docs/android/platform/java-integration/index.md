---
title: Java-Integration in Xamarin.Android
description: Das Java-Ökosystem umfasst eine vielfältige und umfangreiche Sammlung von Komponenten. Viele dieser Komponenten können verwendet werden, um schneller neue Android-Anwendungen zu entwickeln. Dieses Dokument bietet eine allgemeine Übersicht über einige Möglichkeiten, wie Entwickler bei der Entwicklung von Xamarin.Android-Anwendungen von diesen Java-Komponenten profitieren können.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 01/18/2017
ms.openlocfilehash: ecaa02e036c74074b4fa922ea079355b72ff02e2
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020103"
---
# <a name="java-integration-with-xamarinandroid"></a>Java-Integration in Xamarin.Android

_Das Java-Ökosystem umfasst eine vielfältige und umfangreiche Sammlung von Komponenten. Viele dieser Komponenten können verwendet werden, um schneller neue Android-Anwendungen zu entwickeln. Dieses Dokument bietet eine allgemeine Übersicht über einige Möglichkeiten, wie Entwickler bei der Entwicklung von Xamarin.Android-Anwendungen von diesen Java-Komponenten profitieren können._

## <a name="overview"></a>Übersicht

Angesichts des Umfangs des Java-Ökosystems ist es sehr wahrscheinlich, dass eine für eine Xamarin.Android-Anwendung erforderliche Funktion bereits in Java codiert wurde. Daher bietet es sich an, beim Erstellen einer neuen Xamarin.Android-Anwendung diese bereits vorhandenen Bibliotheken zu testen und wiederzuverwenden.

Es gibt drei Möglichkeiten, Java-Bibliotheken in einer Xamarin.Android-Anwendung wiederzuverwenden: 

- **Erstellen Sie eine Java-Bindungsbibliothek:** Bei dieser Technik wird ein Xamarin.Android-Projekt verwendet, um C#-Wrapper für die Java-Typen zu erstellen. Dann kann die Xamarin.Android-Anwendung auf die von diesem Projekt erstellten C#-Wrapper verweisen und die `.jar`-Datei verwenden. 

- **Java Native Interface:** *Java Native* *Interface* (JNI) ist ein Framework, das es Code ermöglicht, bei dem es sich nicht um Java-Code handelt (z. B. C++ oder C#), Java-Code aufzurufen, der innerhalb des JNI-Frameworks ausgeführt wird, oder von diesem aufgerufen zu werden. 

- **Portieren Sie den Code:** Bei dieser Methode wird der Java-Quellcode in C# konvertiert. Dies kann manuell oder mithilfe eines automatisierten Tools wie Sharpen erfolgen. 

Bei den ersten beiden Techniken steht das*JNI*-Framework im Vordergrund. Mit diesem Framework können Anwendungen, die nicht in Java geschrieben sind, mit Java-Code interagieren, der in einer JVM-Instanz (Java Virtual Machine) ausgeführt wird. Xamarin.Android verwendet JNI, um *Bindungen* für C#-Code zu erstellen. 

Bei der ersten Technik wird ein stärker automatisierter, deklarativer Ansatz verwendet, um Java-Bibliotheken zu binden. Dies umfasst entweder die Verwendung von Visual Studio für Mac oder eines Visual Studio-Projekttyps, der von Xamarin.Android bereitgestellt wird &ndash; die Java-Bindungsbibliothek. Damit Sie diese Bindungen erstellen können, müssen Sie zwar ggf. noch einige manuelle Änderungen an der Java-Bindungsbibliothek vornehmen, aber bei einem reinen JNI-Ansatz wäre der Aufwand größer. Weitere Informationen zu Java-Bindungsbibliotheken finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md). 

Die zweite Technik, bei der JNI eingesetzt wird, setzt auf viel niedrigerer Ebene an, ermöglicht aber unter Umständen bessere Kontrolle und Java-Methoden, auf die Sie normalerweise nicht über eine Java-Bindungsbibliothek zugreifen können. 

Die dritte Technik unterscheidet sich grundlegend von den vorherigen beiden. Dabei geht es darum, Code von Java in C# zu portieren. Es kann sehr aufwendig sein, Code von einer Sprache in eine andere zu portieren, aber mithilfe des Tools *Sharpen* können Sie diesen Aufwand reduzieren. Sharpen ist ein Open-Source-Tool, mit dem Sie Java-Code in C# konvertieren können. 

## <a name="summary"></a>Zusammenfassung

In diesem Dokument habe Sie einen groben Überblick über einige Möglichkeiten erhalten, Java-Bibliotheken in einer Xamarin.Android-Anwendung wiederzuverwenden. Es wurden die Konzepte von Bindungen und verwalteten Callable Wrappers vorgestellt, und es wurden verschiedene Optionen zum Portieren von Java-Code in C# erläutert. 

## <a name="related-links"></a>Verwandte Links

- [Architektur](~/android/internals/architecture.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Java Native Interface](https://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
