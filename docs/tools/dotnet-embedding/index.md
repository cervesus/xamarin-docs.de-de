---
title: Einbetten von .NET
description: .NET Einbetten von vorhandenem .NET Code (C#-, F#- usw.), die von anderen Programmiersprachen genutzt werden können
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f5e778ef9ba31c1a9e880b9fc66c2e48ddb2420c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="net-embedding"></a>Einbetten von .NET

![Vorschau](~/media/shared/preview.png)

Ermöglicht das Einbetten von .NET vorhandenem .NET Code (C#-, F#- usw.), die von anderen Programmiersprachen und in verschiedenen Umgebungen verwendet werden.

Dies bedeutet, dass wenn Sie eine .NET Bibliothek, die Sie aus der vorhandenen iOS-app verwenden möchten verfügen, können Sie dies tun.   Oder wenn Sie ihn mit einem systemeigenen C++-Bibliothek verknüpfen möchten, ist auch möglich, die.   Oder .NET Code aus Java nutzen.

## <a name="environments-and-languages"></a>Umgebungen und Sprachen

Das Tool ist sowohl Beachten Sie die Umgebung aus, die verwendet werden, sowie die Sprache, die sie nutzen.   Beispielsweise lässt die iOS-Plattform nicht Just-in-Time (JIT)-Kompilierung, damit die Embeddinator statisch .NET Code in systemeigenen Code kompiliert wird, die in iOS verwendet werden kann.  Diese Enviroments wir einen Abruf JIT kompiliert, und anderen Umgebungen erlauben JIT-Kompilierung.

Verschiedene Verbraucher Sprache, werden unterstützt, sodass er .NET Code als idiomatische Code in der Zielsprache bereitstellt.   Dies ist die Liste der unterstützten Sprachen derzeit:

- [**Objective-C** ](objective-c/index.md) – Zuordnen von .NET zu idiomatische Objective-C-APIs.
- [**Java** ](android/index.md) – Zuordnen von .NET zu idiomatische Java-APIs.
- **C**: Zuordnen von .NET zu einer objektorientierten wie C-APIs.

Weitere Sprachen werden später bereitgestellt.

## <a name="getting-started"></a>Erste Schritte

Überprüfen Sie eine der unsere Bereitstellungshandbücher für jede der derzeit unterstützten Sprachen Schritte aus, um zu beginnen:

- [**Objective-C** ](get-started/objective-c/index.md) – MacOS und iOS behandelt.
- [**Java** ](get-started/java/index.md) – deckt MacOS und Android.
- [**C** ](get-started/c.md) – behandelt die Programmiersprache C auf Desktopplattformen auf.


## <a name="related-links"></a>Verwandte Links

- [Embeddinator-4000 auf GitHub](https://github.com/mono/Embeddinator-4000)
