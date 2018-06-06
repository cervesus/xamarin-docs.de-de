---
title: Systemeigene Typen für IOS- und Mac OS
description: Dieses Dokument beschreibt basierend auf der Zielarchitektur Kompilierung wie die Xamarin einheitliche API .NET-oder Schematypen 32 Bit und 64-Bit-systemeigene Typen nach Bedarf zugeordnet.
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fc2b91a9265fcf09e4f58d5de27a1fdef9350b2d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781103"
---
# <a name="native-types-for-ios-and-macos"></a>Systemeigene Typen für IOS- und Mac OS

Mac- und iOS-APIs verwenden architekturspezifischer Datentypen, die immer 32 Bits auf 32-Bit-Plattformen und 64 Bit unter 64-Bit-Plattformen sind.

Objective-C beispielsweise ordnet die `NSInteger` Datentyp `int32_t` auf 32-Bit-Systeme und zu `int64_t` auf 64-Bit-Systemen.

Ersetzen wir dieses Verhaltens auf unserem einheitliche API, entsprechend der vorherigen Verwendung `int` (die in .NET wird als definiert immer `System.Int32`) in einen neuen Datentyp: `System.nint`. Sie können die "n" als Bedeutung "systemeigene" vorstellen, sodass die systemeigenen Integer-Datentyp der Plattform.

Mit dieser neuen Datentypen wird der gleiche Quellcode für 32-Bit und 64-Bit-Architekturen, je nach der Kompilierung Flags kompiliert.

## <a name="new-data-types"></a>Neue Datentypen

Die folgende Tabelle zeigt die Änderungen in unseren Datentypen entsprechend dieser neuen 32-/64-Bit-Umgebung:

|Systemeigener Typ|32-Bit-Unterstützungstyp|64-Bit-Unterstützungstyp|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

Wir haben diese Namen zu ermöglichen, dass Ihr C#-Code "Suchen", wenn Sie die gleiche Weise, die er heute aussehen würde, mehr oder weniger ausgewählt.

### <a name="implicit-and-explicit-conversions"></a>Implizite und explizite Konvertierungen

Der Entwurf der neuen Datentypen dient eine einzelne C#-Quelldatei an natürlich 32 oder 64-Bit-Speicher je nach die Hostplattform und die Einstellungen für die .NET-Kompilierung verwenden können.

Dieser erforderliche uns so entwerfen Sie einen Satz von implizite und explizite Konvertierungen in und aus der Clientplattform-spezifische Datentypen in der .NET ganzzahligen "oder" floating-Point-Datentypen.

Implizite Konvertierungen Operatoren werden bereitgestellt, wenn keine Möglichkeit von Datenverlust (32-Bit-Werte besteht, die auf einem 64-Bit-Leerzeichen gespeichert werden).

Explizite Konvertierungen Operatoren werden bereitgestellt, wenn es besteht die Gefahr von Datenverlust (64-Bit-Wert wird auf einen Speicherort 32 oder potenziell 32 gespeichert wird).

 `int`, `uint` und `float` werden alle implizit konvertierbar in `nint`, `nuint` und `nfloat` 32 Bits immer in 32 oder 64 Bits passt.

 `nint`, `nuint` und `nfloat` werden alle implizit konvertierbar in `long`, `ulong` und `double` 32 oder 64-Bit-Werte immer im 64-Bit-Speicher passt.

Verwenden Sie explizite Konvertierungen von `nint`, `nuint` und `nfloat` in `int`, `uint` und `float` seit der systemeigenen Typen 64 Bits des Speichers enthalten können.

Verwenden Sie explizite Konvertierungen von `long`, `ulong` und `double` in `nint`, `nuint` und `nfloat` da systemeigenen Typen nur 32 Bits der Speicher enthalten sein können.

## <a name="coregraphics-types"></a>CoreGraphics-Typen

Der Punkt, Größe und Rechteck-Datentypen, die mit CoreGraphics verwendet werden verwenden 32 oder 64 Bits, je nach Gerät, das sie ausgeführt werden.  Wenn es ursprünglich IOS- und Mac-APIs gebunden wird vorhandenen Datenstrukturen, die entsprechend der Größe der die Hostplattform aufgetreten sind (die Datentypen in `System.Drawing`).

Beim Verschieben auf **Unified**, müssen Sie ersetzen `System.Drawing` mit ihren `CoreGraphics` Gegenstücke, wie in der folgenden Tabelle dargestellt:

|Geben Sie "System.Drawing" alte|Neue Datentyp CoreGraphics|Beschreibung|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Enthält floating-point-Rechteck Informationen.|
|`SizeF`|`CGSize`|Gleitkommawert enthält zeigen Sie Informationen zur Datenbankgröße (Breite, Höhe)|
|`PointF`|`CGPoint`|Enthält eine Gleitkommazahl, zeigen Sie Informationen (X, Y)|

Die alten Daten verwendeten Typen Gleitkommawerte zum Speichern der Elemente der Datenstrukturen dagegen verwendet eine neue `System.nfloat`.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassische Vs einheitliche API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
