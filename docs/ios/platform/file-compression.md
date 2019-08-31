---
title: Dateikomprimierung in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie mit der libcompression-API in xamarin. IOS arbeiten. Er erläutert das deflating, das erhöhen und die unterstützten Algorithmen.
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: bcc63aa4e1926f5502d571bf47c83b0c8ea7e429
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199522"
---
# <a name="file-compression-in-xamarinios"></a>Dateikomprimierung in xamarin. IOS

Xamarin-apps, die IOS 9,0 oder macOS 10,11 (und höher) als Ziel verwenden, können das _Komprimierungs Framework_ zum Komprimieren (Codieren) und Dekomprimieren (decodieren) von Daten verwenden. Xamarin. IOS stellt dieses Framework nach der Stream-API bereit. Das Komprimierungs Framework ermöglicht es Entwicklern, mit den komprimierten und entkomprimierten Daten zu interagieren, als ob es sich um normale Streams handelt, ohne Rückrufe oder Delegaten verwenden zu müssen.

Das Komprimierungs Framework bietet Unterstützung für die folgenden Algorithmen:

* LZ4
* LZ4 RAW
* Lzfse
* Lzma
* Zlib

Mithilfe des Komprimierungs Frameworks können Entwickler Komprimierungs Vorgänge ausführen, ohne dass Bibliotheken von Drittanbietern oder nugets verwendet werden. Dadurch werden externe Abhängigkeiten reduziert, und es wird sichergestellt, dass die Komprimierungs Vorgänge auf allen Plattformen unterstützt werden (vorausgesetzt, Sie erfüllen die Mindestanforderungen an das Betriebssystem).

## <a name="general-file-decompression"></a>Allgemeine Datei Dekomprimierung

Das Komprimierungs Framework verwendet eine Stream-API in xamarin. IOS und xamarin. Mac. Diese API bedeutet, dass der Entwickler zum Komprimieren von Daten die normalen Muster verwenden kann, die in anderen IO-APIs in .NET verwendet werden. Im folgenden Beispiel wird gezeigt, wie Daten mit dem Komprimierungs Framework, das der API in der `System.IO.Compression.DeflateStream` -API ähnelt, deaktiviert werden:

```csharp
// sample zlib data
static byte [] compressed_data = { 0xf3, 0x48, 0xcd, 0xc9, 0xc9, 0xe7, 0x02, 0x00 };

using (var backing = new MemoryStream (compressed_data)) // compress data to read
using (var decompressing = new CompressionStream (backing, CompressionMode.Decompress, CompressionAlgorithm.Zlib)) // create decompression stream with the correct algorithm
using (var reader = new StreamReader (decompressing))
{
    // perform the required stream operations
    Console.WriteLine (reader.ReadLine ());
}
```

Implementiert die `IDisposable` -Schnittstelle wie andere `System.IO.Streams`, damit Entwickler sicherstellen sollten, dass Ressourcen freigegeben werden, sobald Sie nicht mehr benötigt werden. `CompressionStream`

## <a name="general-file-compression"></a>Allgemeine Dateikomprimierung

Die Komprimierungs-API ermöglicht es Entwicklern auch, Daten nach derselben API zu komprimieren. Daten können mit einem der im `CompressionAlgorithm` -Enumerator angegebenen Algorithmen komprimiert werden.

```csharp
// sample method that copies the data from the source stream to the destination stream
static void CopyStream (Stream src, Stream dest)
{
    byte[] array = new byte[1024];
    int bytes_read;
    bytes_read = src.Read (array, 0, 1024);
    while (bytes_read != 0) {
        dest.Write (array, 0, bytes_read);
        bytes_read = src.Read (array, 0, 1024);
    }
}

static void CompressExample ()
{
    // create some sample data to compress
    byte [] data = new byte[100000];
    for (int i = 0; i < 100000; i++) {
        data[i] = (byte) i;
    }

    using (var dataStream = new MemoryStream (data)) // stream that contains the data to compress
    using (var backing = new MemoryStream ()) // stream in which the compress data will be written
    using (var compressing = new CompressionStream (backing, CompressionMode.Compress, CompressionAlgorithm.Zlib, true))
    {
        // copy the data to the compressing stream
        CopyStream (dataStream, compressing);
        // at this point, the CompressionStream contains all the data from the dataStream but compressed
        // using the zlib algorithm
    }
```

## <a name="async-support"></a>Asynchrone Unterstützung

Unterstützt alle asynchronen Vorgänge `System.IO.DeflateStream`, die von unterstützt werden. Dies bedeutet, dass Entwickler das Async-Schlüsselwort verwenden können, um die Vorgänge zum Komprimieren und Dekomprimieren auszuführen, ohne den UI-Thread zu blockieren. `CompressionStream`
