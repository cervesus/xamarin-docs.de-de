---
title: Erste Schritte mit C
description: Dieses Dokument beschreibt, wie Sie Einbetten von .NET mit .NET Code in einer C-Anwendung einzubetten. Es wird erläutert, wie zum Einbetten von .NET in Visual Studio-2019 und Visual Studio für Mac verwenden.
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: lobrien
ms.author: laobri
ms.date: 04/19/2018
ms.openlocfilehash: 342ba2a6b51483983df7bd04034a4cef62fd57ff
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61213591"
---
# <a name="getting-started-with-c"></a>Erste Schritte mit C

## <a name="requirements"></a>Anforderungen

Zum Einbetten von .NET mit C verwenden, benötigen Sie ein Mac oder Windows-Computer ausgeführt wird:

### <a name="macos"></a>macOS

* MacOS 10.12 (Sierra) oder höher
* Xcode 8.3.2 oder höher
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 oder höher
* Visual Studio 2015 oder höher

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) installieren und konfigurieren Sie für Ihr Projekt Einbetten von .NET.

Der Befehlsaufruf, die, den Sie konfigurieren sollten, sieht so aus wie (möglicherweise mit einer anderen Versionsnummer erneut und Pfade):

### <a name="visual-studio-for-mac"></a>Visual Studio für Mac

```shell
mono {SolutionDir}/packages/Embeddinator-4000.0.4.0.0/tools/Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=macos --compile managed.dll
```

### <a name="visual-studio-2017"></a>Visual Studio 2017

```shell
$(SolutionDir)\packages\Embeddinator-4000.0.2.0.80\tools\Embeddinator-4000.exe --gen=c --outdir=managed_c --platform=windows --compile managed.dll
```

## <a name="generation"></a>Generierung

### <a name="output-files"></a>Ausgabedateien

Wenn alles gut geht, werden Sie mit der folgenden Ausgabe angezeigt:

```shell
Parsing assemblies...
    Parsed 'managed.dll'
Processing assemblies...
Generating binding code...
    Generated: managed.h
    Generated: managed.c
    Generated: mscorlib.h
    Generated: mscorlib.c
    Generated: embeddinator.h
    Generated: glib.c
    Generated: glib.h
    Generated: mono-support.h
    Generated: mono_embeddinator.c
    Generated: mono_embeddinator.h
```

Da die `--compile` Flag für das Tool übergeben wurde, Einbetten von .NET sollte auch kompiliert haben die Ausgabedateien in eine freigegebene Bibliothek, die sich neben die generierten Dateien, eine **libmanaged.dylib** Datei unter MacOS und **managed.dll** auf Windows.

Um die freigegebene Bibliothek nutzen zu können, zählen Sie die **managed.h** C-Headerdatei, bietet der C-Deklarationen, die auf die jeweils entsprechenden verwalteten Bibliotheks-APIs und Verknüpfung mit den oben genannten kompiliert, freigegebenen Bibliothek.

## <a name="further-reading"></a>Weiterführende Themen

* [Einbetten von .NET-Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Mitwirkung an open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
