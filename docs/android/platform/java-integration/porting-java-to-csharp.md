---
title: Portieren von Java inC#
description: Eine dritte option für mithilfe von Java in einer Xamarin.Android-Anwendung besteht darin, die Java-Quellcode auf port C#.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/05/2016
ms.openlocfilehash: 9beb6d59c9376a404c06af7f0cd1efd985929843
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104270"
---
# <a name="porting-java-to-c"></a>Portieren von Java inC#

_Eine dritte option für mithilfe von Java in einer Xamarin.Android-Anwendung besteht darin, die Java-Quellcode auf port C#._

## <a name="overview"></a>Übersicht

Dieser Ansatz kann für Organisationen interessant sein, die:

-  **Einfügen von Java zum Technologiestapel C#.**
-  **Müssen eine C# und eine Java-Version desselben Produkts.**
-  **Möchten Sie eine .NET Version einer beliebte Java-Bibliothek verfügen.**


Es gibt zwei Möglichkeiten zum Portieren von Java-Code zum C#. Die erste Möglichkeit besteht, den Code manuell zu portieren. Dies umfasst die erfahrene Entwickler, die zu verstehen, .NET- und Java, und klicken Sie mit der richtigen Sprachen für jede Sprache vertraut sind. Dieser Ansatz ist am sinnvollsten für kleine Mengen von Code oder für Organisationen, die vollständig von Java zum verschieben möchten C#.

Bei der zweite Portieren-Methode ist, versuchen Sie es aus, und automatisieren den Prozess mithilfe eines Konverters Code wie z. B. [Scharfzeichnen](https://github.com/mono/sharpen). [Erweitern Sie](https://github.com/mono/sharpen) ist ein open-Source-Konverter aus Versant, die ursprünglich verwendet wurde, um den Code für port *db4o* von Java zum C#. db4o ist eine objektorientierte-Datenbank, die Versant in Java entwickelt, und klicken Sie dann auf .NET portiert. Mit einem Code-Konverter sinnvoll für Projekte, die muss in beiden Sprachen vorhanden sein und erfordern einige Parität zwischen den beiden.

Wenn eine automatisierte Tool für die Konvertierung sinnvoll ist ein Beispiel finden Sie in der [Ngit](https://github.com/mono/ngit) Projekt.
Ngit ist Bestandteil des Java-Projekts [Jgit](http://eclipse.org/).
Jgit selbst ist eine Java-Implementierung, der die [Git](http://git-scm.com/) source Code Management-System. Zum generieren C# Code von Java, die Ngit Programmierer Verwendung ein benutzerdefinierten System zum Extrahieren von des Java-Codes aus Jgit Automated, einige Patches, die zur Aufnahme des Konvertierungsvorgangs anwenden, und führen Sie die Scharfzeichnen, die generiert die C# Code. Dadurch wird das Projekt Ngit fortlaufend, laufende Arbeit profitieren, die auf einem Jgit erfolgt.

Es ist häufig eine nicht triviale Menge an Arbeit bei der bootstrapping ein Konvertierungstool von automatisierter Code, und dies kann sich als Barriere verwendet werden. In vielen Fällen kann es sein, einfacher und leichter zu Port von Java zum C# manuell.



## <a name="related-links"></a>Verwandte Links

- [Schärfen-Konvertierungstool](https://github.com/mono/sharpen)
