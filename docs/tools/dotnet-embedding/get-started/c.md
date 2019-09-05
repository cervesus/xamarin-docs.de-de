---
title: Erste Schritte mit C
description: In diesem Dokument wird beschrieben, wie Sie .net-Einbettungen mithilfe von .net embed in eine C-Anwendung einbetten. Darin wird erläutert, wie .net-Einbettungen sowohl in Visual Studio 2019 als auch Visual Studio für Mac verwendet werden.
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
author: conceptdev
ms.author: crdun
ms.date: 04/19/2018
ms.openlocfilehash: 1dc68a709f8e1f864961bbe87af112b648b0dd2a
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70278736"
---
# <a name="getting-started-with-c"></a>Erste Schritte mit C

## <a name="requirements"></a>Anforderungen

Um die .net-Einbettung mit C zu verwenden, benötigen Sie einen Mac-oder Windows-Computer unter:

### <a name="macos"></a>macOS

* macOS 10,12 (Sierra) oder höher
* Xcode 8.3.2 oder höher
* [Mono](https://www.mono-project.com/download/)

### <a name="windows"></a>Windows

* Windows 7, 8, 10 oder höher
* Visual Studio 2015 oder höher

## <a name="installing-net-embedding-from-nuget"></a>Installieren von .net-Einbettungen über nuget

Befolgen Sie diese [Anweisungen](~/tools/dotnet-embedding/get-started/install/install.md) , um .net-Einbettungen für Ihr Projekt zu installieren und zu konfigurieren.

Der Befehls Aufruf, den Sie konfigurieren sollten, sieht wie folgt aus (möglicherweise mit verschiedenen Versionsnummern und Pfaden):

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

Wenn alles gut funktioniert, wird die folgende Ausgabe angezeigt:

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

Da das `--compile` Flag an das Tool weitergegeben wurde, sollte die .net-Einbettung auch die Ausgabedateien in eine freigegebene Bibliothek kompiliert haben, die Sie neben den generierten Dateien finden können, eine **libmanaged. dylib** -Datei unter macOS und **Managed. dll** unter Windows.

Um die freigegebene Bibliothek zu nutzen, können Sie die **verwaltete h** -c-Header Datei einschließen, die die c-Deklarationen bereitstellt, die den entsprechenden verwalteten Bibliotheks-APIs entsprechen, und die Verknüpfung mit der zuvor erwähnten kompilierten freigegebenen Bibliothek.

## <a name="further-reading"></a>Weiterführende Themen

* [.Net-Einbettungs Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Mitwirken am Open Source-Projekt](https://github.com/mono/Embeddinator-4000/blob/master/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
