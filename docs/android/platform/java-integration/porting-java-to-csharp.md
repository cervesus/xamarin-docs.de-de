---
title: Portieren von Java C# auf für xamarin. Android
description: Eine dritte Option zur Verwendung von Java in einer xamarin. Android-Anwendung ist das Portieren des Java C#-Quellcodes in.
ms.prod: xamarin
ms.assetid: 39E528BD-010F-47FC-BE48-8E7848E30454
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 04/05/2016
ms.openlocfilehash: c6627f585326848c5221729ca94071b00651c59e
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511170"
---
# <a name="porting-java-to-c-for-xamarinandroid"></a>Portieren von Java C# auf für xamarin. Android

Diese Vorgehensweise ist möglicherweise für Organisationen von Interesse, die Folgendes beachten:

- **Wechselt technologische Stapel von Java zu C#.**
- **Muss eine C# und eine Java-Version desselben Produkts verwalten.**
- **Sie möchten eine .NET-Version einer beliebten Java-Bibliothek haben.**

Es gibt zwei Möglichkeiten, Java-Code in C#zu portieren. Die erste Möglichkeit besteht darin, den Code manuell zu portieren. Dies betrifft erfahrene Entwickler, die .net und Java verstehen und mit den richtigen Ausdrücke für jede Sprache vertraut sind. Diese Vorgehensweise ist für kleine Mengen von Code oder für Organisationen, die vollständig von Java zu C#wechseln möchten, am sinnvollsten.

Die zweite Methode zum Portieren besteht darin, den Prozess mithilfe eines Code Konverters, wie z. b. " [Sharpen](https://github.com/mono/sharpen)", zu automatisieren. [Sharpen](https://github.com/mono/sharpen) ist ein Open-Source-Konverter von Versant, der ursprünglich verwendet wurde, um  den Code für db4o C#von Java auf zu portieren. db4o ist eine objektorientierte Datenbank, die in Java entwickelt und anschließend in .NET portiert wurde. Die Verwendung eines Code Konverters kann für Projekte sinnvoll sein, die in beiden Sprachen vorhanden sein müssen und die Parität zwischen den beiden erfordern.

Ein Beispiel für den Fall, dass ein automatisiertes Code Konvertierungs Tool sinnvoll ist, kann im [ngit](https://github.com/mono/ngit) -Projekt angezeigt werden.
Ngit ist ein Port des Java-Projekts [jgit](http://eclipse.org/).
Jgit selbst ist eine Java-Implementierung des [git](http://git-scm.com/) -Quell Code Verwaltungssystems. Zum Generieren C# von Code aus Java verwenden die ngit-Programmierer ein benutzerdefiniertes automatisiertes System zum Extrahieren des Java-Codes aus jgit, zum Anwenden einiger Patches auf den Konvertierungsprozess und zum anschließenden Ausführen C# von Sharpen, wodurch der Code generiert wird. Dadurch kann das ngit-Projekt von der kontinuierlichen, laufenden Arbeit profitieren, die auf jgit ausgeführt wird.

Es gibt häufig eine nicht triviale Menge an Arbeit, die mit dem Bootstrapping eines automatisierten Code Konvertierungs Tools verbunden ist, und dies kann eine Barriere für die Verwendung darstellen. In vielen Fällen ist es möglicherweise einfacher und einfacher, Java von Hand C# zu portieren.

## <a name="related-links"></a>Verwandte Links

- [Tool zum Erweitern der Konvertierung](https://github.com/mono/sharpen)
