---
title: Weiterführende Konzepte und Interna
description: Zugrunde liegende Architektur hinter xamarin. Android und dem API-Design von IT-Experten.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/21/2018
ms.openlocfilehash: 97382243ac5f767d94a782b895401c1f2f8ae554
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027859"
---
# <a name="advanced-concepts-and-internals"></a>Weiterführende Konzepte und Interna

_Dieser Abschnitt enthält Themen, in denen die Architektur, der API-Entwurf und die Einschränkungen von xamarin. Android erläutert werden. Außerdem enthält es Themen, in denen die Garbage Collection Implementierung und die in xamarin. Android verfügbaren Assemblys erläutert werden. Da xamarin. Android [Open Source](https://github.com/xamarin/xamarin-android)ist, ist es auch möglich, die inneren Funktionsweise von xamarin. Android zu verstehen, indem der zugehörige Quellcode untersucht wird._

## <a name="architectureandroidinternalsarchitecturemd"></a>[Architektur](~/android/internals/architecture.md)

In diesem Artikel wird die zugrunde liegende Architektur hinter einer xamarin. Android-Anwendung erläutert. Es wird erläutert, wie xamarin. Android-Anwendungen in einer Mono-Ausführungsumgebung zusammen mit dem virtuellen Android-Lauf Zeit Computer ausgeführt werden, und es werden wichtige Konzepte als von Android Callable Wrapper und verwaltete Callable Wrapper erläutert. 

## <a name="api-designandroidinternalsapi-designmd"></a>[API-Entwurf](~/android/internals/api-design.md)

Zusätzlich zu den Basisklassen Bibliotheken, die Teil von Mono sind, werden in xamarin. Android Bindungen für verschiedene Android-APIs geliefert, damit Entwickler Native Android-Anwendungen mit Mono erstellen können.

Im Kern von xamarin. Android gibt es eine Interop-Engine, die die C# Welt mit der Java-Welt verbindet und Entwicklern den Zugriff auf die Java- C# APIs von oder anderen .NET-Sprachen ermöglicht.

## <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Assemblys](~/cross-platform/internals/available-assemblies.md)

Xamarin. Android ist mit mehreren Assemblys ausgeliefert. Ebenso wie Silverlight eine erweiterte Teilmenge der Desktop-.NET-Assemblys ist, ist xamarin. Android auch eine erweiterte Teilmenge von mehreren Silverlight-und Desktop-.NET-Assemblys. 
