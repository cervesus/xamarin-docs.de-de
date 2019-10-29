---
title: Die ersten Schritte mit Ziel-C
description: In diesem Dokument wird beschrieben, wie Sie mit dem Einbetten von .net mit Ziel-C beginnen. Es werden die Anforderungen erläutert, die .net-Einbettung von nuget und die unterstützten Plattformen werden installiert.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: davidortinau
ms.author: daortin
ms.date: 11/14/2017
ms.openlocfilehash: bf2f832056c164e199cfb779766f298b2b3817de
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73007087"
---
# <a name="getting-started-with-objective-c"></a>Die ersten Schritte mit Ziel-C

Dies ist die Seite "Getting Started" für "Ziel-C", die die Grundlagen für alle unterstützten Plattformen behandelt.

## <a name="requirements"></a>Anforderungen

Um die .net-Einbettung mit Ziel-C zu verwenden, benötigen Sie einen Mac, auf dem Folgendes ausgeführt wird:

- macOS 10,12 (Sierra) oder höher
- Xcode 8.3.2 oder höher
- [Mono 5,0](https://www.mono-project.com/download/)

Sie können [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) installieren, um den C# Code zu bearbeiten und zu kompilieren.

> [!NOTE]
>
> - Frühere Versionen von macOS, Xcode und Mono funktionieren _möglicherweise_ , sind jedoch nicht getestet und werden nicht unterstützt.
> - Die Code Generierung kann unter Windows ausgeführt werden, es ist jedoch nur möglich, Sie auf einem Mac-Computer zu kompilieren, auf dem Xcode installiert ist.

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .net-Einbettungen über nuget

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) , um .net-Einbettungen für Ihr Projekt zu installieren und zu konfigurieren.

Ein Beispiel für einen Befehls Aufruf ist in den Handbüchern zu den ersten Schritten mit [macOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) und [IOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) aufgeführt.

## <a name="platforms"></a>Plattformen

Ziel-C ist eine Sprache, die am häufigsten zum Schreiben von Anwendungen für macOS, Ios, tvos und watchos verwendet wird. die .net-Einbettung unterstützt alle diese Plattformen. Das Arbeiten mit jeder Plattform impliziert einige [wesentliche Unterschiede, die hier erläutert werden](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

Das [Erstellen einer macOS-Anwendung](~/tools/dotnet-embedding/get-started/objective-c/macos.md) ist am einfachsten, da Sie nicht so viele zusätzliche Schritte umfasst, z. b. das Einrichten der Identität, die Bereitstellung von Profilen, Simulatoren und Geräte. Es wird empfohlen, mit dem macOS-Dokument vor dem für IOS zu beginnen.

### <a name="ios--tvos"></a>IOS/tvos

Stellen Sie sicher, dass Sie bereits für die Entwicklung von IOS-Anwendungen eingerichtet sind, bevor Sie versuchen, eine mit .net-Einbettung zu erstellen. Die [folgenden Anweisungen](~/tools/dotnet-embedding/get-started/objective-c/ios.md) setzen voraus, dass Sie bereits erfolgreich eine IOS-Anwendung von Ihrem Computer erstellt und bereitgestellt haben.

Die Unterstützung für tvos ist analog zur Funktionsweise von IOS, indem nur tvos-Projekte in den IDES (Visual Studio und Xcode) anstelle von IOS-Projekten verwendet werden.

> [!NOTE]
> Die Unterstützung für watchos wird in einer zukünftigen Version verfügbar sein und ist sehr ähnlich wie IOS/tvos.

## <a name="further-reading"></a>Weiterführende Themen

- [.Net-Einbettungs Funktionen speziell für "Ziel-C"](~/tools/dotnet-embedding/objective-c/index.md)
- [Bewährte Methoden für "Ziel-C"](~/tools/dotnet-embedding/objective-c/best-practices.md)
- [.Net-Einbettungs Einschränkungen](~/tools/dotnet-embedding/limitations.md)
- [Mitwirken am Open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
- [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
- [Zielplattformen](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Verwandte Links

- [Wetter Beispiel (IOS & macOS)](https://github.com/jamesmontemagno/embeddinator-weather)
