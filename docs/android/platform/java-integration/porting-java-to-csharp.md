---
title: Portieren von Java in c#
description: "Eine dritte Möglichkeit für die Verwendung von Java in einer Anwendung Xamarin.Android ist so portieren Sie Quellcode Java und c#."
ms.topic: article
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/05/2016
ms.openlocfilehash: bf881861eec0b28e59704253c3dab3f4e5dbae46
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="porting-java-to-c"></a>Portieren von Java in c#

_Eine dritte Möglichkeit für die Verwendung von Java in einer Anwendung Xamarin.Android ist so portieren Sie Quellcode Java und c#._

## <a name="overview"></a>Übersicht

Dieser Ansatz kann von Interesse sein, Organisationen, die:

-  **Technologie-Stapel einfügen aus Java zu C#.**
-  **C#- und eine Java-Version desselben Produkts müssen beibehalten werden.**
-  **Möchten Sie eine .NET Version einer gängigen Java-Bibliothek verfügen.**


Es gibt zwei Möglichkeiten, um Java-Code in c# portieren. Die erste Möglichkeit besteht darin den Code manuell zu portieren. Dies umfasst die erfahrenen Entwicklern .NET- und Java verstehen und mit der richtigen Idiome für jede Sprache vertraut sind. Dieser Ansatz stellt am sinnvollsten für kleine Mengen von Code oder für Organisationen, die vollständig von Java in c# verschieben möchten.

Die zweite Portieren Methodologie darin automatisieren Sie den Prozess, indem Sie einen Konverter Code wie [Schärfe](https://github.com/mono/sharpen). [Scharf](https://github.com/mono/sharpen) ist eine open-Source-Konverter aus, die ursprünglich verwendet wurde, um den Code für port Versant *db4o* von Java in c#. db4o ist eine objektorientierte-Datenbank, die Versant in Java entwickelt, und klicken Sie dann auf .NET portiert. Verwenden einen Konverter Code sinnvoll für Projekte, die muss vorhanden sein, beide Sprachen und erfordert einige Parität zwischen den beiden.

Wenn ein Konvertierungstool automatisierte Code sinnvoll ist ein Beispiel sehen Sie der [Ngit](https://github.com/mono/ngit) Projekt.
Ngit ist Bestandteil der Java-Projekt [Jgit](http://eclipse.org/).
Jgit selbst ist eine Java-Implementierung, der die [Git](http://git-scm.com/) Code Management Quellsystem. Zum Generieren von C#-Code aus Java verwenden die Programmierer Ngit ein benutzerdefiniertes automatisierten Systems zum Extrahieren von des Java-Codes aus Jgit, gelten einige Patches, um den Konvertierungsprozess zu ermöglichen, und führen Sie Schärfe, die den C#-Code generiert. Dadurch wird das Projekt Ngit die continuous, laufende Arbeiten profitieren, die in Jgit ausgeführt wird.

Es gibt häufig eine nicht triviale Menge an Arbeit bei der Platzierung bootstrapping ein Konvertierungstool automatisierte Code und Veranschaulichung einer Barrier-Klasse verwendet werden. In vielen Fällen kann es vereinfacht und leichter Port Java zu C#-per hand katalogisiert wird sein.



## <a name="related-links"></a>Verwandte Links

- [Scharf-Konvertierungstool](https://github.com/mono/sharpen)
