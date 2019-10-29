---
title: Unter der Haube in xamarin. Mac
description: Dieses Dokument ist mit verschiedenen Anleitungen verknüpft, in denen die innere Funktionsweise von xamarin. Mac beschrieben wird. Verknüpfte Dokumente erörtern eine vorab Kompilierung, xamarin. Mac-Architektur und die xamarin. Mac-Registrierungsstelle.
ms.prod: xamarin
ms.assetid: 84974D75-0CCE-4455-AA38-00DE68AE33B6
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 11/10/2017
ms.openlocfilehash: c13f8db60d296b8fa684e1d6861ba5caf38a7680
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029938"
---
# <a name="under-the-hood-in-xamarinmac"></a>Unter der Haube in xamarin. Mac

## <a name="ahead-of-time-compilation-aotaotmd"></a>[Vorab Kompilierung (AOT)](aot.md)

Die AOT-Kompilierung (Ahead of Time) ist eine leistungsstarke Optimierungsmethode zum Verbessern der Startleistung. Dies wirkt sich jedoch auf die Buildzeit, die Anwendungs Größe und die Programmausführung auf tiefgreifende Weise aus, daher ist es sinnvoll, die Funktionsweise zu verstehen.

## <a name="mac-architecturearchitecturemd"></a>[Mac-Architektur](architecture.md)

Beziehung von xamarin. Mac zu Ziel-C, einschließlich Konzepten wie Kompilierung, Selektoren, Registrars, App-Start und Generator.

## <a name="xamarinmac-registrarregistrarmd"></a>[Xamarin. Mac-Registrierungsstelle](registrar.md)

Xamarin. Mac verbindet die Lücke zwischen der verwalteten Welt und der Cocoa-Laufzeit und ermöglicht verwalteten Klassen, nicht verwaltete Ziel-C-Klassen aufzurufen und bei Auftreten von Ereignissen zurückgerufen zu werden. Die Arbeit, die für diese "Magic" erforderlich ist, wird von der Registrierungsstelle behandelt, aber das Verständnis der "unter der Haube"-Funktion kann manchmal hilfreich sein.
