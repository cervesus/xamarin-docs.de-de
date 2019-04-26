---
title: Systemeigene Typen für iOS und macOS
description: Dieses Dokument beschreibt basierend auf der Zielarchitektur Kompilierung wie Xamarin Unified API-Typen in .NET 32-Bit- und 64-Bit systemeigenen Typen nach Bedarf zugeordnet.
ms.prod: xamarin
ms.assetid: B5237770-0FC3-4B01-9E22-766B35C9A952
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: fc2b91a9265fcf09e4f58d5de27a1fdef9350b2d
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199628"
---
# <a name="native-types-for-ios-and-macos"></a>Systemeigene Typen für iOS und macOS

Verwenden Architektur-spezifische Datentypen, die immer 32 Bits auf 32-Bit-Plattformen und 64-Bit auf 64-Bit-Plattformen sind, Mac und iOS-APIs.

Objective-C-beispielsweise ordnet die `NSInteger` Datentyp, `int32_t` auf 32-Bit-Systemen und zu `int64_t` auf 64-Bit-Systemen.

Dieses Verhalten auf unsere einheitliche API, entsprechend der vorherigen Verwendung ersetzt werden `int` (die in .NET wird als definiert immer `System.Int32`) in einen neuen Datentyp: `System.nint`. Sie können das "n" wie "native" vorstellen, damit die systemeigene ganze Zahl eingeben, der Plattform.

Mit dieser neuen Datentypen wird der gleiche Quellcode für 32-Bit- und 64-Bit-Architekturen, je nach Ihrer Kompilierung-Flags kompiliert.

## <a name="new-data-types"></a>Neue Datentypen

Die folgende Tabelle zeigt die Änderungen in unseren Datentypen entsprechend dieser neuen 32/64-Bit-Welt:

|Nativer Typ|32-Bit-Unterstützungstyp|64-Bit-Unterstützungstyp|
|--- |--- |--- |
|`System.nint`|`System.Int32` (`int`)|`System.Int64` (`long`)|
|`System.nuint`|`System.UInt32` (`uint`)|`System.UInt64` (`ulong`)|
|`System.nfloat`|`System.Single` (`float`)|`System.Double` (`double`)|

Wir haben uns entschieden, diese Namen können Ihre C# Code mehr oder weniger die gleiche Weise zu suchen, die es heute aussehen würde.

### <a name="implicit-and-explicit-conversions"></a>Implizite und explizite Konvertierungen

Der Entwurf der neuen Datentypen soll es erlauben, eine einzelne C# Quelldatei 32 oder 64-Bit-Speicher je nach die Hostplattform und die Einstellungen für die .NET-Kompilierung auf natürliche Weise zu verwenden.

Dies erforderlich, einen Satz von implizite und explizite Konvertierungen in und aus den plattformspezifischen Datentypen für die .NET Ganzzahl- und Gleitkommatyps Point-Datentypen zu entwerfen.

Implizite Konvertierungen Operatoren stehen zur Verfügung, wenn kein Risiko von Datenverlusten (32-Bit-Werte besteht, die auf einem 64-Bit-Leerzeichen gespeichert werden).

Explizite Konvertierungen Operatoren stehen zur Verfügung, wenn es besteht die Gefahr von Datenverlusten (64-Bit-Wert wird auf einem 32 oder potenziell 32 Storage-Speicherort gespeichert wird).

 `int`, `uint` und `float` sind implizit konvertierbar in `nint`, `nuint` und `nfloat` wie 32 Bits immer in 32 oder 64 Bit passen.

 `nint`, `nuint` und `nfloat` sind implizit konvertierbar in `long`, `ulong` und `double` wie 32 oder 64-Bit-Werte immer in 64-Bit-Speicher passen.

Verwenden Sie explizite Konvertierungen von `nint`, `nuint` und `nfloat` in `int`, `uint` und `float` , da die systemeigenen Typen 64 Bits des Speichers enthalten können.

Verwenden Sie explizite Konvertierungen von `long`, `ulong` und `double` in `nint`, `nuint` und `nfloat` da die systemeigenen Typen möglicherweise nur Lage 32 Bits von Speicher zu speichern.

## <a name="coregraphics-types"></a>CoreGraphics-Typen

Der Punkt, Größe und Rechteck-Datentypen, die mit CoreGraphics verwenden 32 oder 64 Bits je nach Gerät, die, das Sie ausgeführt werden.  Wenn wir ursprünglich die IOS- und Mac-APIs gebunden wir die vorhandenen Datenstrukturen, die aufgetreten sind, entsprechend der Größe der Hostplattform verwendet (die Datentypen in `System.Drawing`).

Bei der Umstellung auf **Unified**, Sie ersetzen müssen `System.Drawing` mit ihren `CoreGraphics` Entsprechungen wie in der folgenden Tabelle dargestellt:

|Geben Sie "System.Drawing" alte|Neue Datentyp CoreGraphics|Beschreibung|
|--- |--- |--- |
|`RectangleF`|`CGRect`|Enthält Schwebendes Rechteck-Informationen.|
|`SizeF`|`CGSize`|Unverankerte enthält zeigen Sie Informationen zur Tabellengröße (Width, Height)|
|`PointF`|`CGPoint`|Enthält eine Gleitkommazahl, zeigen Sie Informationen (X, Y)|

Die alten Daten verwendeten Typen Gleitkommawerte zum Speichern der Elemente der Datenstrukturen, die neue verwendet zwar `System.nfloat`.

## <a name="related-links"></a>Verwandte Links

- [Arbeiten mit nativen Typen in plattformübergreifenden Apps](~/cross-platform/macios/native-types-cross-platform.md)
- [Klassischen Vs Unified API-Unterschiede](https://developer.xamarin.com/releases/ios/api_changes/classic-vs-unified-8.6.0/)
