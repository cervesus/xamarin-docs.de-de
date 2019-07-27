---
title: Native Typen für IOS und macOS
description: In diesem Dokument wird beschrieben, wie die xamarin-Unified API .NET-Typen nach Bedarf den systemeigenen 32-Bit-und 64-Bit-Typen zuordnet, basierend auf der Kompilierungs Zielarchitektur
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 9d43bbdb49fe4ab1ff909f709a37f979c360ceb9
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509589"
---
# <a name="native-types-for-ios-and-macos"></a>Native Typen für IOS und macOS

Mac-und IOS-APIs verwenden architekturspezifische Datentypen, die auf 32-Bit-Plattformen immer 32 Bits und 64 Bits auf 64-Bit-Plattformen sind.

Beispielsweise ordnet Ziel-C den `NSInteger` `int64_t` -Datentyp auf `int32_t` 32-Bit-Systemen und auf 64-Bit-Systemen zu.

Um dieses Verhalten zu erfüllen, ersetzen wir in unserer vereinheitlichten API die vorherigen Verwendungen `int` von (die in .net als immer wird `System.Int32`definiert definiert) auf einen neuen Datentyp `System.nint`:. Sie können sich "n" als "Native" vorstellen, also den nativen ganzzahligen Typ der Plattform.

Mit diesen neuen Datentypen wird der gleiche Quellcode für 32-Bit-und 64-Bit-Architekturen kompiliert, abhängig von den Kompilierungs-Flags.

## <a name="new-data-types"></a>Neue Datentypen

In der folgenden Tabelle sind die Änderungen der Datentypen aufgeführt, die mit dieser neuen 32/64-Bit-Welt in Einklang stehen:

|Nativer Typ|32-Bit-Unterstützungstyp|64-Bit-Unterstützungstyp|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

Wir haben diese Namen gewählt, damit C# Ihr Code mehr oder weniger auf die gleiche Weise wie heute aussehen kann.

### <a name="implicit-and-explicit-conversions"></a>Implizite und explizite Konvertierungen

Der Entwurf der neuen Datentypen soll es einer einzelnen C# Quelldatei ermöglichen, abhängig von der Host Plattform und den Kompilierungs Einstellungen auf natürliche Weise 32-oder 64-Bit-Speicher zu verwenden.

Dies erforderte das Entwerfen eines Satzes impliziter und expliziter Konvertierungen in und aus den plattformspezifischen Datentypen in die ganzzahligen und Gleit Komma Datentypen von .net.

Implizite Konvertierungs Operatoren werden bereitgestellt, wenn es keine Möglichkeit zum Verlust von Daten gibt (32-Bit-Werte werden in einem 64-Bit-Raum gespeichert).

Explizite Konvertierungs Operatoren werden bereitgestellt, wenn ein Datenverlust möglich ist (64-Bit-Wert wird an einem 32-oder potenziell 32-Speicherort gespeichert).

 `int`, `uint` und `float` sindimplizit`nfloat` in konvertierbar `nuint`,und da 32 Bits immer in 32 oder 64 Bits passen. `nint`

 `nint`, `nuint` `long`und `nfloat` sind implizit in konvertierbar. als 32-oder 64 `ulong` -Bit-Werte werden immer in den 64-Bit- `double` Speicher passen.

Sie müssen explizite Konvertierungen von `nint` `nuint` und `nfloat` in `int`verwenden, `uint` und `float` da die systemeigenen Typen möglicherweise 64 Bits Speicher enthalten.

Sie müssen explizite Konvertierungen von `long` `ulong` und `double` in `nint`verwenden, `nuint` und `nfloat` da die systemeigenen Typen möglicherweise nur 32 Bits Speicherplatz enthalten können.

## <a name="coregraphics-types"></a>CoreGraphics-Typen

Die für CoreGraphics verwendeten Punkt-, Größen-und Rechteck Datentypen verwenden je nach dem Gerät, auf dem Sie ausgeführt werden, 32 oder 64 Bits.  Als wir die IOS-und Mac-APIs ursprünglich gebunden haben, verwendeten wir vorhandene Datenstrukturen, die mit den Größen der Host Plattform (Datentypen `System.Drawing`in) verglichen wurden.

Wenn Sie zu **Unified**migrieren, müssen Sie die Instanzen von `System.Drawing` durch ihre `CoreGraphics` Entsprechungen ersetzen, wie in der folgenden Tabelle gezeigt:

|Alter Typ in System. Drawing|Neue Datentyp-CoreGraphics|Beschreibung|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Enthält Informationen zu Gleit Komma Rechtecks.|
|`SizeF`|`CGSize`|Enthält Informationen zur Gleit Komma Größe (Breite, Höhe).|
|`PointF`|`CGPoint`|Enthält einen Gleit Komma Wert, Punkt Informationen (X, Y)|

Die alten Datentypen, die verwendet werden, um die Elemente der Datenstrukturen zu speichern, während der `System.nfloat`neue Datentypen verwendet.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Unterschiede bei klassischem vs Unified API](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
