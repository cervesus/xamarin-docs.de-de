---
title: Einbetten von .NET
description: Einbetten von .NET können Ihre vorhandenen .NET Code (c#, f# und andere) von in anderen Programmiersprachen geschriebenen Code genutzt werden soll.
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: 23233ea8b06e0db580ba99edf2705e3dae5b931f
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50107143"
---
# <a name="net-embedding"></a>Einbetten von .NET

![Vorschau](~/media/shared/preview.png)

Einbetten von .NET können Ihre vorhandenen .NET Code (c#, f# und andere) von anderen Programmiersprachen und in verschiedenen Umgebungen verwendet werden.

Dies bedeutet, dass wenn Sie eine Bibliothek für .NET, die Sie aus Ihrer vorhandenen iOS-app verwenden möchten verfügen, können Sie dies tun.   Oder wenn Sie es mit einem systemeigenen C++-Bibliothek verknüpfen möchten, ist auch möglich, die.   Oder nutzen Sie .NET Code von Java.

Einbetten von .NET basiert auf der [Embeddinator-4000](https://github.com/mono/Embeddinator-4000) open Source-Projekt.

## <a name="environments-and-languages"></a>Umgebungen und Sprachen

Das Tool ist sowohl über die Umgebung, die dazu verwendet werden, sowie die Sprache, die sie nutzen.   Beispielsweise ist die iOS-Plattform nicht just-in-Time (JIT)-Kompilierung, sodass Einbetten von .NET statisch .NET Code in systemeigenen Code kompiliert wird, die in iOS verwendet werden kann.  In diesen Umgebungen, entscheiden wir uns auf JIT kompiliert, und andere Umgebungen erlauben JIT-Kompilierung.

Damit Sie ihn als idiomatischen Code in der Zielsprache .NET Code zeigt unterstützt verschiedene Verbraucher für Sprache.   Dies ist die Liste der unterstützten Sprachen, derzeit:

- [**Objective-C-** ](objective-c/index.md) : Zuordnen von .NET zu idiomatische Objective-C-APIs
- [**Java** ](android/index.md) : Zuordnen von .NET zu idiomatische Java-APIs
- [**C** ](get-started/c.md) : Zuordnen von .NET zu objektorientierten wie C-APIs

Weitere Sprachen werden später bereitgestellt.

## <a name="getting-started"></a>Erste Schritte

Überprüfen Sie unsere Leitfäden für jede der derzeit unterstützten Sprachen finden Sie Informationen zum Einstieg:

- [**Objective-C-** ](get-started/objective-c/index.md) – behandelt, MacOS und iOS
- [**Java** ](get-started/java/index.md) – behandelt, MacOS und Android
- [**C** ](get-started/c.md) – behandelt die Programmiersprache C auf Desktopplattformen

## <a name="related-links"></a>Verwandte Links

- [Embeddinator-4000 auf GitHub](https://github.com/mono/Embeddinator-4000)
