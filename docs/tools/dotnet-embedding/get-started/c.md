---
title: Erste Schritte mit C
ms.prod: xamarin
ms.assetid: 2A27BE0F-95FB-4C3A-8A43-72540179AA85
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 8dff45de6de7c9492b199f323656778ac5c34d57
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-c"></a>Erste Schritte mit C


## <a name="requirements"></a>Anforderungen

Verwendung von .NET Einbetten von C benötigen Sie einen Mac oder Windows-Computer mit:

* MacOS 10.12 (Sierra) oder höher
* Xcode 8.3.2 oder höher

* Windows 7, 8, 10 oder höher
* Visual Studio 2015 oder höher

* [Mono](http://www.mono-project.com/download/)


## <a name="installation"></a>Installation

Der nächste Schritt ist zum Herunterladen und installieren die .NET Einbetten von Tools auf Ihrem Computer.

Binäre Builds für C# und Java-Generatoren sind noch nicht verfügbar, jedoch sind in Kürze verfügbar.

Alternativ ist es möglich, erstellen sie über unsere Git-Repository finden Sie unter der [beitragen](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md) Dokument Anweisungen.


## <a name="generation"></a>Generierung

Rufen Sie das Übergeben der richtigen Flags für die die Programmiersprache C als Ziel .NET einbetten-Tool, um C#-Code zu generieren:

### <a name="windows"></a>Windows:

```csharp
$ build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=windows --compile managed.dll
```

Achten Sie darauf, die aus einer Visual Studio-Befehlsshell bestimmte auf die Visual Studio-Version Sie aufrufen können die Anwendung.

### <a name="macos"></a>macOS

```csharp
$ mono build/lib/Debug/Embeddinator-4000.exe --gen=c --output=managed_c --platform=macos --compile managed.dll
```

### <a name="output-files"></a>Ausgabedateien

Wenn alles gut geht, wird die folgende Ausgabe angezeigt:

```csharp
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

Da die `--compile` Flag an das Tool übergeben wurde, .NET einbetten sollte auch haben kompiliert die Ausgabedateien in eine freigegebene Bibliothek, die sich neben die generierten Dateien, eine `libmanaged.dylib` Datei auf MacOS, und `managed.dll` unter Windows.

Um die freigegebene Bibliothek nutzen Sie zählen die `managed.h` C-Header-Datei, die die C-Deklarationen, die für die jeweilige bereitstellt verwalteten Bibliotheks-APIs und Verknüpfung mit der oben erwähnten kompiliert freigegebene Bibliothek.

## <a name="further-reading"></a>Weiterführende Themen

* [Embeddinator Einschränkungen](~/tools/dotnet-embedding/limitations.md)
* [Das open-Source-Projekt verwendet werden sollen](https://github.com/mono/Embeddinator-4000/blob/master/docs/Contributing.md)
* [Fehlercodes und Beschreibungen](~/tools/dotnet-embedding/errors.md)
