---
title: Erste Schritte mit Objective-C
description: Dieses Dokument beschreibt, wie erste Schritte mit .NET einbetten, mit Objective-C. Es erläutert die Anforderungen, die Installation von .NET Einbetten von NuGet und unterstützte Plattformen.
ms.prod: xamarin
ms.assetid: 4ABC0247-B608-42D4-89CB-D2E598097142
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 02d79103825d150b6e6f5bec7ed3ee1788078312
ms.sourcegitcommit: 3f2737f8abf9b855edf060474aa222e973abda3f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/28/2018
ms.locfileid: "37066626"
---
# <a name="getting-started-with-objective-c"></a>Erste Schritte mit Objective-C

Dies ist das Abrufen gestarteten Seite für Objective-C, dem behandelt die Grundlagen für alle unterstützten Plattformen.

## <a name="requirements"></a>Anforderungen

Um .NET einbetten mit Objective-C verwenden, benötigen Sie einen Mac ausgeführt wird:

* MacOS 10.12 (Sierra) oder höher
* Xcode 8.3.2 oder höher
* [Mono 5.0](http://www.mono-project.com/download/)

Sie können [Visual Studio für Mac](https://visualstudio.microsoft.com/vs/mac/) bearbeiten und Kompilieren von C#-Code.

> [!NOTE]
> * Frühere Versionen von Mac OS, Xcode und Mono _möglicherweise_ arbeiten, sind aber nicht getestet und wird nicht unterstützt
> * Generierung von Code kann auf Windows vorgenommen werden, aber es ist nur möglich, Kompilieren auf einem Macintosh-Computer, auf dem Xcode installiert ist

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) installieren und Konfigurieren von .NET einbetten für das Projekt.

Eine Beispiel-Befehlsaufruf abgelesen werden die [MacOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) und [iOS](~/tools/dotnet-embedding/get-started/objective-c/ios.md) Handbücher mit ersten Schritten.

## <a name="platforms"></a>Plattformen

Objective-C ist eine Programmiersprache, die am häufigsten verwendet wird, um das Schreiben von Anwendungen für Mac OS, iOS und WatchOS tvos. außerdem wurden; .NET einbetten unterstützt alle von diesen Plattformen. Arbeiten mit jeder Plattform impliziert einige [Hauptunterschiede und diese hier erläuterten](~/tools/dotnet-embedding/objective-c/platforms.md).

### <a name="macos"></a>macOS

[Erstellen einer Anwendung MacOS](~/tools/dotnet-embedding/get-started/objective-c/macos.md) ist am einfachsten, da es keine, wie viele zusätzliche Schritte erläutert beinhaltet, wie die Identität, Provisining Profile-Simulatoren und Geräte einrichten. Es wird empfohlen, vor dem Projekt für iOS mit dem Dokument MacOS zu starten.

### <a name="ios--tvos"></a>iOS / tvos. außerdem wurden

Stellen Sie sicher, dass Sie bereits so eingerichtet sind iOS-Anwendungen entwickeln, bevor Sie versuchen, erstellen Sie eine mit .NET einbetten. Die [Anweisungen](~/tools/dotnet-embedding/get-started/objective-c/ios.md) wird davon ausgegangen, dass Sie bereits erfolgreich erstellt und eine iOS-Anwendung auf Ihrem Computer bereitstellen.

Unterstützung für tvos. außerdem wurden erfolgt analog zur Funktionsweise iOS mithilfe tvos. außerdem wurden Projekte nur in der IDEs (Visual Studio und Xcode) anstelle von iOS-Projekten.

> [!NOTE]
> Unterstützung für WatchOS in einer zukünftigen Version verfügbar und iOS/tvos. außerdem wurden sehr ähnlich werden.

## <a name="further-reading"></a>Weiterführende Themen

* [.NET Einbetten von Funktionen, die spezifisch für Objective-C](~/tools/dotnet-embedding/objective-c/index.md)
* [Bewährte Methoden für Objective-C](~/tools/dotnet-embedding/objective-c/best-practices.md)
* [.NET Einbetten von Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Das open-Source-Projekt verwendet werden sollen](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
* [Zielplattformen](~/tools/dotnet-embedding/objective-c/platforms.md)

## <a name="related-links"></a>Verwandte Links

- [Wetter-Beispiel (iOS und Mac OS)](https://github.com/jamesmontemagno/embeddinator-weather)
