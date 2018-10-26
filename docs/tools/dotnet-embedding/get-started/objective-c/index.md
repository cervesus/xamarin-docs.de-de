---
title: Erste Schritte mit Objective-C
description: In diesem Dokument wird beschrieben, wie erste Schritte mit .NET einbetten, mit Objective-c Es erläutert die Anforderungen, die über NuGet und unterstützten Plattformen installieren Einbetten von .NET.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: lobrien
ms.author: laobri
ms.date: 11/14/2017
ms.openlocfilehash: c7bac0612679131383d3b89f24904c8083fa925b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103100"
---
# <a name="getting-started-with-objective-c"></a>Erste Schritte mit Objective-C

Dies ist die Seite "Erste Schritte", für Objective-C, die die Grundlagen für alle unterstützten Plattformen abdeckt.

## <a name="requirements"></a>Anforderungen

Zum Einbetten von .NET mit Objective-C verwenden, benötigen Sie einen Mac mit:

* MacOS 10.12 (Sierra) oder höher
* Xcode 8.3.2 oder höher
* [Mono 5.0](http://www.mono-project.com/download/)

Sie können installieren [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) bearbeiten und Kompilieren Ihrer C# Code.

> [!NOTE]
> * Frühere Versionen von MacOS, Xcode und Mono _möglicherweise_ arbeiten, sind jedoch nicht getestet und wird nicht unterstützt
> * Generierung von Code kann für Windows ausgeführt werden, aber es ist nur möglich, die er sich kompilieren auf einem Macintosh-Computer, auf dem Xcode installiert ist

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) installieren und konfigurieren Sie für Ihr Projekt Einbetten von .NET.

Eine Beispiel-Befehlsaufruf finden Sie der [MacOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) und [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) Leitfäden mit ersten Schritten.

## <a name="platforms"></a>Plattformen

Objective-C ist eine Sprache, die am häufigsten zum Schreiben von Anwendungen für MacOS, iOS, TvOS und WatchOS verwendet wird, Einbetten von .NET unterstützt alle Plattformen. Arbeiten mit jeder Plattform impliziert einige [wichtige Unterschiede, und diese werden hier erläutert](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Erstellen einer MacOS-Anwendung](~/tools/dotnet-embedding/get-started/objective-c/macos.md) ist am einfachsten, da es nicht so viele zusätzliche Schritte erläutert, wie das Einrichten von Identitäten, fort, Profile, Simulatoren und Geräten beinhaltet. Es wird empfohlen, mit dem MacOS-Dokument vor für iOS zu starten.

### <a name="ios--tvos"></a>iOS / TvOS

Stellen Sie sicher, dass Sie sind bereits eingerichtet, um iOS-Anwendungen zu entwickeln, bevor Sie versuchen, die zum Erstellen verwenden, Einbetten von .NET. Die [Anweisungen](~/tools/dotnet-embedding/get-started/objective-c/ios.md) wird davon ausgegangen, dass Sie bereits erfolgreich erstellt und eine iOS-Anwendung auf Ihrem Computer bereitstellen.

Unterstützung für TvOS ist analog zur Funktionsweise iOS einfach mit TvOS-Projekten in der IDEs (Visual Studio und Xcode) anstelle von iOS-Projekte.

> [!NOTE]
> Unterstützung für WatchOS in einer zukünftigen Version verfügbar und iOS/TvOS sehr ähnlich werden.

## <a name="further-reading"></a>Weiterführende Themen

* [Einbetten von .NET Funktionen, die speziell für Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Bewährte Methoden für Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [Einbetten von .NET-Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Mitwirkung an open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
* [Zielplattformen](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Verwandte Links

- [Wetter-Beispiel (iOS und MacOS)](https://github.com/jamesmontemagno/embeddinator-weather)
