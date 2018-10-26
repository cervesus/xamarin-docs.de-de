---
title: Übersicht über die Java-Integration
description: Die Java-Ökosystem umfasst eine verschiedenartige und enorme Sammlung von Komponenten. Viele dieser Komponenten können verwendet werden, um Zeit zu verkürzen, um eine Android-Anwendung zu entwickeln. In diesem Dokument wird eingeführt und bieten eine allgemeine Übersicht über einige der Methoden, die Entwickler dieser vorhandenen Java-Komponenten verwenden können, um ihre Entwicklungsumgebung für Xamarin.Android-Anwendung zu verbessern.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 01/18/2017
ms.openlocfilehash: 3ab31fb7cac97fbae3315f51daf3dd4b1edbcc1d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112391"
---
# <a name="java-integration-overview"></a>Übersicht über die Java-Integration

_Die Java-Ökosystem umfasst eine verschiedenartige und enorme Sammlung von Komponenten. Viele dieser Komponenten können verwendet werden, um Zeit zu verkürzen, um eine Android-Anwendung zu entwickeln. In diesem Dokument wird eingeführt und bieten eine allgemeine Übersicht über einige der Methoden, die Entwickler dieser vorhandenen Java-Komponenten verwenden können, um ihre Entwicklungsumgebung für Xamarin.Android-Anwendung zu verbessern._


## <a name="overview"></a>Übersicht

Wenn das Ausmaß der Java-Umgebung, ist es sehr wahrscheinlich, dass alle angegebenen Funktionen erforderlich, die für eine Xamarin.Android-Anwendung in Java bereits codiert wurde. Aus diesem Grund ist es attraktiv, testen und Wiederverwenden von dieser vorhandenen Bibliotheken beim Erstellen einer Xamarin.Android-Anwendung. 

Es gibt drei Möglichkeiten, die Java-Bibliotheken in einer Xamarin.Android-Anwendung wiederverwenden: 

-   **Erstellen einer Java Bindungsbibliothek** &ndash; mit dieser Technik dient zum Erstellen einer Xamarin.Android-Projekt C# Wrapper für die Java-Typen. Eine Xamarin.Android-Anwendung kann dann referenzieren, die C# Wrapper von diesem Projekt erstellt, und klicken Sie dann verwenden die `.jar` Datei. 

-   **Java Native Interface** &ndash; der *Java Native* *Schnittstelle* (JNI) ist ein Framework, mit nicht-Java-Code (wie z. B. C++ oder C#) aufrufen, oder durch Ausführen der Java-Code aufgerufen werden innerhalb einer JVM. 

-   **Der Code portieren** &ndash; diese Methode erfordert das Erstellen der Java-Quellcode, und klicken Sie dann durch die Konvertierung in C#. Dies kann manuell oder mithilfe eines automatisierten Tools wie z. B. Scharfzeichnen erfolgen. 

Das Herzstück der ersten beiden Methoden ist die *Java Native Interface* (JNI). JNI ist ein Framework, mit der Anwendungen, die nicht in Java geschrieben sind, für die Interaktion mit Java-Code in einer Java-Computer ausgeführt wird. Xamarin.Android verwendet die JNI erstellen *Bindungen* für C# Code. 

Die erste Technik ist, einen automatisierten, deklarativen Ansatz zum Binden von Java-Bibliotheken. Mit Visual Studio für Mac oder Visual Studio-Projekt ein, die von Xamarin.Android bereitgestellt wird hierbei &ndash; der Java-Bindungen-Bibliothek. Um diese Bindungen erfolgreich zu erstellen, kann einer Java Bindungsbibliothek muss weiterhin, dass einige manuelle Änderungen, aber nicht über so viele wie einen reinen JNI-Ansatz würde. Finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md) für Weitere Informationen zum Binden von Java-Bibliotheken. 

Das zweite Verfahren, mit JNI, funktioniert auf einer viel niedrigeren Ebene, können jedoch für eine präzisere Kontrolle bereitzustellen und Zugriff auf Java-Methoden, die normalerweise nicht über eine Java-Bibliothek binden zugänglich wäre. 

Die dritte Technik unterscheidet sich grundlegend von den vorherigen beiden: das Portieren von Code von Java zum C#. Portieren von Code von einer Sprache in eine andere kann ein sehr arbeitsaufwendig Prozess sein, aber es ist möglich, zu verringern, dass der Aufwand mithilfe eines Tools namens *Scharfzeichnen*. Schärfen ist ein open-Source-Tool, das ist eine Java-zu-C# Konverter. 



## <a name="summary"></a>Zusammenfassung

Dieses Dokument bereitgestellt, eine allgemeine Übersicht der verschiedenen Methoden, die Bibliotheken aus Java in einer Xamarin.Android-Anwendung wiederverwendet werden können. Es enthält die Konzepte von Bindungen und callable Wrapper verwaltet und erläutert die Optionen für das Portieren von Java-Code zum C#. 


## <a name="related-links"></a>Verwandte Links

- [Architektur](~/android/internals/architecture.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Java Native Interface](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
