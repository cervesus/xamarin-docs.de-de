---
title: Unterstützung von Programmiersprachen in xamarin
description: In diesem Dokument werden verschiedene von xamarin Unterstützte Programmiersprachen beschrieben. Darin werden C#, F#, Portable Visual Basic.net-und Razor-Vorlagen erläutert.
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: davidortinau
ms.author: daortin
ms.date: 02/18/2018
ms.openlocfilehash: db963a38322e809d1aa82c02fbb9ae5cc4a650fc
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73014633"
---
# <a name="programming-language-support-in-xamarin"></a>Unterstützung von Programmiersprachen in xamarin

## <a name="c"></a>C\#

### <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Async Support Overview (Übersicht über die asynchrone Unterstützung)](~/cross-platform/platform/async.md)

In Version 5 C# von wurden zwei neue Schlüsselwörter eingeführt, um asynchrone Vorgänge auszudrücken: Async und warten. Mithilfe dieser Schlüsselwörter können Sie einfachen Code schreiben, der die Task Parallel Library verwendet, um Vorgänge mit langer Ausführungsdauer (z. b. Netzwerk Zugriff) in einem anderen Thread auszuführen und problemlos auf die Ergebnisse beim Abschluss zuzugreifen. Die neuesten Versionen von xamarin. IOS und xamarin. Android unterstützen Async und warten: Dieses Dokument enthält Erläuterungen und ein Beispiel für die Verwendung der neuen Syntax mit xamarin.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[C# 6-Sprachfunktionen](~/cross-platform/platform/csharp-six.md)

Mit der neuesten Version der C# Sprache – Version 6 – wird die Sprache weiterentwickelt, um weniger Bausteine, bessere Übersichtlichkeit und mehr Konsistenz zu erhalten. Eine saubere Initialisierungs Syntax, die Möglichkeit, `await` in `catch/finally` Blöcken zu verwenden, und der NULL bedingte `?`-Operator sind besonders nützlich.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

Entwickeln mobiler apps mit F# und xamarin.

## <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Portable Visual Basic.net](~/cross-platform/platform/visual-basic/index.md)

Visual Studio unterstützt die Erstellung von portablen Klassenbibliotheken mithilfe von Visual Basic.net, die dann in xamarin-Anwendungen integriert werden können. In diesem Artikel wird gezeigt, wie Sie eine neue Visual Basic PCL erstellen und diese dann in einer xamarin. IOS-, xamarin. Android-und Windows Phone-Beispielanwendung verwenden.

## <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Erstellen von HTML-Ansichten mithilfe von Razor-Vorlagen](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin ermöglicht Entwicklern die Verwendung der Razor-Vorlagen-Engine, die ursprünglich mit ASP.NET MVC eingeführt wurde C# , sowie das einfache kombinieren von Daten mit HTML, JavaScript und CSS, ohne dass Sie manuell HTML-Zeichen folgen im Code erstellen müssen.
In diesem Artikel wird veranschaulicht, wie Razor-Vorlagen mit xamarin für Android und IOS verwendet werden.
