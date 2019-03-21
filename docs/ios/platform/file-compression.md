---
title: Xamarin.iOS-dateikomprimierung
description: Dieses Dokument beschreibt, wie Sie mit der API Libcompression in Xamarin.iOS arbeiten. Erläutert Entleeren, jedoch ist, und die verschiedenen Algorithmen unterstützt.
ms.prod: xamarin
ms.assetid: 94D05DAB-01E8-4C62-9CEF-9D6417EEA8EB
ms.technology: xamarin-ios
author: mandel-macaque
ms.author: mandel
ms.date: 03/04/2019
ms.openlocfilehash: f7a1df65047fd8040dd40e9f7f057d6bfe6dea61
ms.sourcegitcommit: 4c97f5d73be7eb2da153a85183be4258b6b11ca6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/20/2019
ms.locfileid: "58290925"
---
# <a name="file-compression-in-xamarinios"></a>Xamarin.iOS-dateikomprimierung

Xamarin-apps für iOS 9.0 oder Mac OS 10.11 (und höher) können die _Komprimierung Framework_ komprimieren (Codierung) und Dekomprimieren von (decodieren) Daten. Xamarin.iOS enthält dieses Framework, befolgen die Stream-API. Die Komprimierung-Framework ermöglicht es Entwicklern, für die Interaktion mit dem Komprimieren und dekomprimiert Daten, als wären sie normale Streams ohne Rückrufe oder Delegaten verwendet werden müssen.

Die Komprimierung-Framework bietet Unterstützung für die folgenden Algorithmen:

* LZ4
* Unformatierte LZ4
* Lzfse
* Lzma
* Zlib

Mit dem Framework für die Komprimierung ermöglicht Entwicklern, die Komprimierung Vorgänge ohne Drittanbieter-Bibliotheken oder NuGet-Pakete ausführen. Dies reduziert die externe Abhängigkeiten und stellt sicher, dass die Komprimierung-Vorgänge (solange sie die Betriebssystem-Mindestanforderungen erfüllen) auf allen Plattformen unterstützt werden.

## <a name="general-file-decompression"></a>Allgemeine dekomprimiert.

Das Framework für die Komprimierung verwendet einen Stream-API in Xamarin.iOS- und Xamarin.Mac. Diese API bedeutet, um Daten zu komprimieren, die Entwickler, die normalen Muster, die in anderen e/a-APIs in .NET verwendet verwenden kann. Das folgende Beispiel zeigt, wie Sie Daten mit dem Framework Komprimierung Dekomprimieren handelt es sich ähnlich wie die API finden Sie in der `System.IO.Compression.DeflateStream` API:

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

Die `CompressionStream` implementiert die `IDisposable` -Schnittstelle, wie andere `System.IO.Streams`, sodass Entwickler sichergestellt wird, dass die Ressourcen freigegeben werden, sobald sie nicht mehr benötigt werden.

## <a name="general-file-compression"></a>Allgemeine dateikomprimierung

Die Komprimierung-API ermöglicht es auch Entwickler beim Komprimieren von Daten, die die gleiche API befolgen. Daten können komprimiert werden, mithilfe eines der bereitgestellten Algorithmen angegeben, der `CompressionAlgorithm` Enumerator.

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

Die `CompressionStream` alle asynchronen Vorgänge, die von Microsoft Intune unterstützt die `System.IO.DeflateStream`, was bedeutet, dass Entwickler das Schlüsselwort "Async" verwenden können, um die komprimieren und Dekomprimieren-Vorgänge auszuführen, ohne Blockierung im UI-Thread.