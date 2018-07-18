---
title: Übersicht über die Java-Integration
description: Die Java-Ökosystem umfasst verschiedene und enorme Auflistung von Komponenten. Viele dieser Komponenten können zum Erstellen eine Android-Anwendung entwickeln benötigte Zeit reduzieren, verwendet werden. Dieses Dokument wird eingeführt und bieten eine allgemeine Übersicht über einige der Methoden, die Entwicklern diese vorhandenen Java-Komponenten verwenden können, um ihre Xamarin.Android Anwendungsentwicklung zu verbessern.
ms.prod: xamarin
ms.assetid: 7B5B8695-1C49-19BF-AE99-948CDCBD2A20
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/18/2017
ms.openlocfilehash: dbaf17479ae077fced425df5ac31bdbbc4e06b64
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
ms.locfileid: "30766945"
---
# <a name="java-integration-overview"></a>Übersicht über die Java-Integration

_Die Java-Ökosystem umfasst verschiedene und enorme Auflistung von Komponenten. Viele dieser Komponenten können zum Erstellen eine Android-Anwendung entwickeln benötigte Zeit reduzieren, verwendet werden. Dieses Dokument wird eingeführt und bieten eine allgemeine Übersicht über einige der Methoden, die Entwicklern diese vorhandenen Java-Komponenten verwenden können, um ihre Xamarin.Android Anwendungsentwicklung zu verbessern._


## <a name="overview"></a>Übersicht

Wenn Sie den Umfang der Java-Ökosystem, ist es sehr wahrscheinlich, dass die angegebenen Funktionen erforderlich sind, für eine Anwendung Xamarin.Android in Java bereits codiert wurde. Aus diesem Grund ist es zu versuchen und diese vorhandenen Bibliotheken wiederverwenden, beim Erstellen einer Anwendung Xamarin.Android ansprechender. 

Es gibt drei Möglichkeiten, Java-Bibliotheken in einer Anwendung Xamarin.Android wiederverwenden: 

-   **Erstellen einer Java-Bindungen Bibliothek** &ndash; mit diesem Verfahren wird ein Projekt Xamarin.Android verwendet, um C#-Wrapper für die Java-Typen zu erstellen. Eine Anwendung Xamarin.Android verweisen Sie auf die c#-Wrapper, die von diesem Projekt erstellt und dann verwenden, kann die `.jar` Datei. 

-   **Java Native Interface** &ndash; der *Java Native* *Schnittstelle* (JNI) ist ein Framework, die nicht-Java-Code (z. B. C++ und c#) ermöglicht das Aufrufen oder aufgerufen werden, von der Java-Code innerhalb einer JVM ausgeführt wird . 

-   **Der Code portieren** &ndash; diese Methode umfasst die Java-Quellcode, und konvertieren es in c#. Dies kann manuell oder mithilfe eines automatisierten Tools wie z. B. Schärfe erfolgen. 

Den Kern der ersten beiden Methoden ist die *Java Native Interface* (JNI). JNI ist ein Framework, mit der Anwendungen, die nicht in Java geschrieben sind, für die Interaktion mit Java-Code in einer Java Virtual Machine ausgeführt, werden. Xamarin.Android verwendet JNI erstellen *Bindungen* für C#-Code. 

Das erste Verfahren ist ein automatisierter, deklarativer Ansatz zum Binden von Java-Bibliotheken. Sie müssen Sie mit dem entweder Visual Studio für Mac oder ein Visual Studio-Projekt-Typ, der von Xamarin.Android bereitgestellten &ndash; der Java-Bindungen-Bibliothek. Um diese Bindungen erfolgreich zu erstellen, eine Bibliothek der Java-Bindungen weiterhin sind möglicherweise einige manuellen Änderungen, jedoch nicht so viele wie einen reinen JNI-Ansatz würde. Finden Sie unter [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md) für Weitere Informationen zum Binden von Java-Bibliotheken. 

Das zweite Verfahren, mit JNI, funktioniert mit viel geringerer, aber ermöglichen für eine genauere Steuerung und der Zugriff auf die Java-Methoden, die normalerweise nicht über eine Bibliothek für die Java-Bindung verfügbar wäre. 

Das dritte Verfahren unterscheidet sich grundlegend von den vorherigen beiden: Portieren von Code von Java in c#. Portieren von Code von einer Sprache in eine andere kann ein sehr arbeitsaufwendig Vorgang sein, aber es ist möglich, zu reduzieren, dass Aufwand mithilfe eines Tools namens *Schärfe*. Scharf ist ein open Source-Tool, das eine Java-zu-Konverter c#. 



## <a name="summary"></a>Zusammenfassung

Dieses Dokument bereitgestellt, eine allgemeine Übersicht über einige der verschieden,-Bibliotheken von Java in einer Anwendung Xamarin.Android wiederverwendet werden können. Es die Konzepte von Bindungen eingeführt verwaltet callable Wrapper und Optionen für das Portieren von Java-Code in c# erläutert. 


## <a name="related-links"></a>Verwandte Links

- [Architektur](~/android/internals/architecture.md)
- [Binden einer Java-Bibliothek](~/android/platform/binding-java-library/index.md)
- [Arbeiten mit JNI](~/android/platform/java-integration/working-with-jni.md)
- [Sharpen](https://github.com/slluis/sharpen)
- [Java Native Interface](http://docs.oracle.com/javase/7/docs/technotes~/jni/index.html)
