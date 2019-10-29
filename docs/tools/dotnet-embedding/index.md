---
title: Einbetten von .NET
description: .Net-Einbettung ermöglicht, dass Ihr vorhandener .NET-Code (C#, F#und andere) von Code verwendet werden können, der in anderen Programmiersprachen geschrieben wurde.
ms.prod: xamarin
ms.assetid: 617C38CA-B921-4A76-8DFC-B0A3DF90E48A
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: 5a0e7eeaee9b3189de63d0b82a3822cc68023505
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73006947"
---
# <a name="net-embedding"></a>Einbetten von .NET

![Vorschau](~/media/shared/preview.png)

.Net-Einbettung ermöglicht, dass Ihr vorhandener .NET-Code (C#, F#und andere) von anderen Programmiersprachen und in verschiedenen Umgebungen genutzt werden können.

Dies bedeutet, dass Sie, wenn Sie über eine .NET-Bibliothek verfügen, die Sie in Ihrer vorhandenen IOS-App verwenden möchten, dies tun können.   Oder wenn Sie es mit einer nativen C++ Bibliothek verknüpfen möchten, können Sie dies auch tun.   Oder verwenden Sie .NET-Code aus Java.

Die .net-Einbettung basiert auf dem Open Source [-Projekt embeddinator-4000](https://github.com/mono/Embeddinator-4000) .

## <a name="environments-and-languages"></a>Umgebungen und Sprachen

Das Tool kennt sowohl die Umgebung, die es verwendet, als auch die Sprache, in der es verwendet wird.   Beispielsweise lässt die IOS-Plattform keine Just-in-time (JIT)-Kompilierung zu, sodass der .NET-Code durch die .net-Einbettung statisch in systemeigenen Code kompiliert wird, der in ios verwendet werden kann.  In anderen Umgebungen ist die JIT-Kompilierung möglich, und in diesen Umgebungen wird JIT-Kompilierung durchzuführen.

Es unterstützt verschiedene sprachconsumer, sodass .NET-Code als idiomatischen Code in der Zielsprache angezeigt wird.   Dies ist die Liste der unterstützten Sprachen, die derzeit verfügbar sind:

- [**Ziel-c**](objective-c/index.md) – Zuordnung von .net zu idiomatischen Ziel-c-APIs
- [**Java**](android/index.md) – Mapping von .net zu idiomatischen Java-APIs
- [**C**](get-started/c.md) – Zuordnung von .net zu objektorientierten Objekten wie c-APIs

Weitere Sprachen werden später angezeigt.

## <a name="getting-started"></a>Erste Schritte

Überprüfen Sie zunächst eine unserer Handbücher für jede der derzeit unterstützten Sprachen:

- [**Ziel-C**](get-started/objective-c/index.md) – deckt macOS und IOS ab
- [**Java**](get-started/java/index.md) – deckt macOS und Android ab
- [**C**](get-started/c.md) – deckt die Programmiersprache c auf Desktop Plattformen ab

## <a name="related-links"></a>Verwandte Links

- [Embeddinator-4000 auf GitHub](https://github.com/mono/Embeddinator-4000)
