---
title: Weiterführende Konzepte und Interna
description: Zugrunde liegende Architektur hinter Xamarin.Android und den zugehörigen API-Design.
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/21/2018
ms.openlocfilehash: f5844dd4340afa0596219a33ed1e479a0dbcfa76
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50114170"
---
# <a name="advanced-concepts-and-internals"></a>Weiterführende Konzepte und Interna

_Dieser Abschnitt enthält Themen, in denen die Architektur, API-Entwurf und Einschränkungen von Xamarin.Android erläutert. Darüber hinaus enthält es Themen, in denen die Garbage Collection-Implementierung und die Assemblys, die in Xamarin.Android verfügbar sind. Da Xamarin.Android ist [Open-Source-](https://github.com/xamarin/xamarin-android), es ist auch möglich, die interne Funktionsweise von Xamarin.Android, die durch Untersuchen des Quellcodes zu verstehen._


##  <a name="architectureandroidinternalsarchitecturemd"></a>[Architektur](~/android/internals/architecture.md)

Dieser Artikel beschreibt die zugrunde liegende Architektur hinter einer Xamarin.Android-Anwendung. Es wird erläutert, wie Xamarin.Android-Anwendungen in einer ausführungsumgebung Mono parallel mit der Android-Runtime-VM ausführen und erläutert wichtigsten Konzepten wie Android Callable Wrapper und Callable Wrapper verwaltet. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API-Entwurf](~/android/internals/api-design.md)

Zusätzlich zu den Basisklassenbibliotheken, die Teil von Mono, Lieferumfang von Xamarin.Android-Bindungen für verschiedene Android-APIs, um Entwicklern das Erstellen von nativen Android-Anwendungen mit Mono zu ermöglichen.

Das Herzstück von Xamarin.Android gibt es ist eine interop-Engine diese Bridges die C#-Welt mit der Java-Welt und bietet Entwicklern Zugriff auf die Java-APIs von C#- oder andere .NET-Sprachen:.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Assemblys](~/cross-platform/internals/available-assemblies.md)

Im Lieferumfang von Xamarin.Android sind mehrere Assemblys. Genau wie Silverlight eine erweiterte Teilmenge der desktop .NET Assemblys ist, wird Xamarin.Android auch eine erweiterte Teilmenge von mehreren Silverlight und desktop .NET-Assemblys. 

