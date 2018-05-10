---
title: Erste Schritte mit C
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: topgenorth
ms.author: toopge
ms.date: 04/19/2018
ms.openlocfilehash: f3c238dc9805dafa922f8e32fb4b1935a3fa152c
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="getting-started-with-c"></a>Erste Schritte mit C

## <a name="requirements"></a>Anforderungen

Um .NET Einbetten von mit C# zu verwenden, benötigen Sie einen Mac oder Windows-Computer mit:

### <a name="macos"></a>macOS

* MacOS 10.12 (Sierra) oder höher
* Xcode 8.3.2 oder höher
* [Mono](http://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 oder höher
* Visual Studio 2015 oder höher

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .NET Einbetten von NuGet

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) installieren und Konfigurieren von .NET einbetten für das Projekt.

Der Befehl aufrufen, die Sie konfigurieren, sollten sieht wie (möglicherweise mit anderen Versionsnummer erneut und Pfade):

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

Wenn alles gut geht, wird die folgende Ausgabe angezeigt:

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

Da die `--compile` Flag an das Tool übergeben wurde, .NET einbetten sollte auch haben kompiliert die Ausgabedateien in eine freigegebene Bibliothek, die sich neben die generierten Dateien, eine **libmanaged.dylib** auf MacOS und Datei**managed.dll** unter Windows.

Um die freigegebene Bibliothek nutzen zu können, zählen die **managed.h** C-Header-Datei, die die C-Deklarationen, die für die jeweilige bereitstellt verwalteten Bibliotheks-APIs und Verknüpfung mit der oben erwähnten kompiliert freigegebene Bibliothek.

## <a name="further-reading"></a>Weiterführende Themen

* [.NET Einbetten von Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Das open-Source-Projekt verwendet werden sollen](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
