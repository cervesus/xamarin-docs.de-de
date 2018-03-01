---
title: Erweiterte Konzepte und Besonderheiten
description: "Zugrunde liegende Architektur hinter Xamarin.Android und den zugehörigen API-Entwurf."
ms.topic: article
ms.prod: xamarin
ms.assetid: CC6A0D52-E9FA-4270-B3FA-84660621D6D5
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/13/2017
ms.openlocfilehash: d120398d4c59e51cee8da5e8ed2fbe0994ceca76
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="advanced-concepts-and-internals"></a>Erweiterte Konzepte und Besonderheiten


##  <a name="architectureandroidinternalsarchitecturemd"></a>[Architektur](~/android/internals/architecture.md)

Dieser Artikel beschreibt die zugrunde liegende Architektur hinter einer Xamarin.Android-Anwendung. Es wird erläutert, wie Xamarin.Android Anwendungen innerhalb einer ausführungsumgebung Mono / zusammen mit den mit der Android-Laufzeit virtuellen Computer ausgeführt und werden als Android Callable Wrapper und Aufrufwrappern verwaltet solche wichtige Konzepte erläutert. 



##  <a name="api-designandroidinternalsapi-designmd"></a>[API-Entwurf](~/android/internals/api-design.md)

Bindungen für verschiedene Android-APIs ermöglichen Entwicklern das Erstellen von systemeigener Android-Anwendungen mit Mono Lieferumfang Xamarin.Android, zusätzlich zu den Kern des Basis-Klassenbibliotheken, die Teil der Mono sind.

Den Kern der Xamarin.Android es ist ein Interop-Modul, Bridges die C#-Welt mit der Außenwelt Java und bietet Entwicklern den Zugriff an die Java-APIs von C#- oder andere .NET-Sprachen:.



##  <a name="assembliescross-platforminternalsavailable-assembliesmd"></a>[Assemblys](~/cross-platform/internals/available-assemblies.md)

Im Lieferumfang von Xamarin.Android sind verschiedene Assemblys. Ebenso wie Silverlight eine erweiterte Teilmenge der desktop .NET-Assemblys ist, ist Xamarin.Android auch eine erweiterte Teilmenge von mehreren Silverlight und desktop .NET-Assemblys. 

