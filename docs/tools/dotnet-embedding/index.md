---
title: Einbetten von .NET
description: .NET Einbetten von vorhandenem .NET Code (C#-, F#- usw.), die von anderen Programmiersprachen genutzt werden können
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: f6edf25faa00bc7c90a52b76a6e90168ccd85b32
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="net-embedding"></a>Einbetten von .NET

![Vorschau](~/media/shared/preview.png)

Ermöglicht das Einbetten von .NET vorhandenem .NET Code (C#-, F#- usw.), die von anderen Programmiersprachen und in verschiedenen Umgebungen verwendet werden.

Dies bedeutet, dass wenn Sie eine .NET Bibliothek, die Sie aus der vorhandenen iOS-app verwenden möchten verfügen, können Sie dies tun.   Oder wenn Sie ihn mit einem systemeigenen C++-Bibliothek verknüpfen möchten, ist auch möglich, die.   Oder .NET Code aus Java nutzen.

Einbetten von .NET basiert auf der [Embeddinator 4000](https://github.com/mono/Embeddinator-4000) open Source-Projekt.

## <a name="environments-and-languages"></a>Umgebungen und Sprachen

Das Tool ist sowohl Beachten Sie die Umgebung aus, die verwendet werden, sowie die Sprache, die sie nutzen.   Beispielsweise lässt die iOS-Plattform nicht Just-in-Time (JIT)-Kompilierung, damit .NET einbetten statisch .NET Code in systemeigenen Code kompiliert wird, die in iOS verwendet werden kann.  Diese Enviroments wir einen Abruf JIT kompiliert, und anderen Umgebungen erlauben JIT-Kompilierung.

Verschiedene Verbraucher Sprache, werden unterstützt, sodass er .NET Code als idiomatische Code in der Zielsprache bereitstellt.   Dies ist die Liste der unterstützten Sprachen derzeit:

- [**Objective-C** ](objective-c/index.md) – Zuordnen von .NET zu idiomatische Objective-C-APIs
- [**Java** ](android/index.md) – Zuordnen von .NET zu idiomatische Java-APIs
- [**C** ](get-started/c.md) – Zuordnen von .NET zu objektorientierte wie C-APIs

Weitere Sprachen werden später bereitgestellt.

## <a name="getting-started"></a>Erste Schritte

Überprüfen Sie eine der unsere Bereitstellungshandbücher für jede der derzeit unterstützten Sprachen Schritte aus, um zu beginnen:

- [**Objective-C** ](get-started/objective-c/index.md) – deckt MacOS und iOS
- [**Java** ](get-started/java/index.md) – deckt MacOS und Android
- [**C** ](get-started/c.md) – behandelt die Programmiersprache C auf Desktopplattformen auf

## <a name="related-links"></a>Verwandte Links

- [Embeddinator-4000 auf GitHub](https://github.com/mono/Embeddinator-4000)
