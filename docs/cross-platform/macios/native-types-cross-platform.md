---
title: Arbeiten mit nativen Typen in plattformübergreifenden Apps
description: In diesem Artikel wird beschrieben, wie Sie die neuen systemeigenen IOS-Unified API Typen (NINT, nuint, nfloat) in einer plattformübergreifenden Anwendung verwenden, bei der Code für nicht-IOS-Geräte (z. b. Android-oder Windows Phone-Betriebssysteme)
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: deb4caa4d23d23b2997361cca161b218c1ff7b61
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511287"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Arbeiten mit nativen Typen in plattformübergreifenden Apps

_In diesem Artikel wird beschrieben, wie Sie die neuen systemeigenen IOS-Unified API Typen (NINT, nuint, nfloat) in einer plattformübergreifenden Anwendung verwenden, bei der Code für nicht-IOS-Geräte (z. b. Android-oder Windows Phone-Betriebssysteme)_


Die systemeigenen Typen 64-Typen funktionieren mit den IOS-und Mac-APIs. Wenn Sie gemeinsam genutzten Code schreiben, der auch unter Android oder Windows ausgeführt wird, müssen Sie die Konvertierung einheitlicher Typen in reguläre .NET-Typen, die Sie freigeben können, verwalten.

In diesem Dokument werden verschiedene Möglichkeiten erläutert, wie Sie mit dem Unified API aus dem freigegebenen/allgemeinen Code interagieren.

## <a name="when-to-use-the-native-types"></a>Verwendungszwecke der systemeigenen Typen

Die vereinheitlichten APIs xamarin. IOS und xamarin. Mac enthalten `int`weiterhin `uint` die `float` Datentypen, und sowie die `RectangleF`Typen, `SizeF` und `PointF` . Diese vorhandenen Datentypen sollten weiterhin in jedem gemeinsam genutzten, plattformübergreifenden Code verwendet werden. Die neuen systemeigenen Datentypen sollten nur verwendet werden, wenn eine Mac-oder IOS-API aufgerufen wird, bei der Unterstützung für architekturabhängige Typen erforderlich ist.

Abhängig von der Art des freigegebenen Codes kann es vorkommen, dass plattformübergreifender Code mit den `nint`Datentypen, `nuint` und `nfloat` umgehen muss. Beispiel: eine Bibliothek, die Transformationen für rechteckige Daten verarbeitet, die zuvor `System.Drawing.RectangleF` zum Freigeben von Funktionen zwischen xamarin. IOS-und xamarin. Android-Versionen einer App verwendet haben, muss aktualisiert werden, damit systemeigene Typen unter IOS behandelt werden.

Die Art und Weise, wie diese Änderungen behandelt werden, hängt von der Größe und Komplexität der Anwendung und der Art der verwendeten Code Freigabe ab, wie in den folgenden Abschnitten zu sehen ist.

## <a name="code-sharing-considerations"></a>Überlegungen zur Code Freigabe

Wie im Dokument [Freigabe Code Optionen](~/cross-platform/app-fundamentals/code-sharing.md) angegeben, gibt es zwei Hauptmethoden zum Freigeben von Code zwischen plattformübergreifenden Projekten: Freigegebene Projekte und Portable Klassenbibliotheken. Welcher der beiden Typen verwendet wurde, schränkt die Optionen ein, die bei der Verarbeitung der systemeigenen Datentypen im plattformübergreifenden Code vorhanden sind.

### <a name="portable-class-library-projects"></a>Projekte für Portable Klassenbibliotheken

Eine portable Klassenbibliothek (Portable Class Library, PCL) ermöglicht Ihnen das Ausrichten der Plattformen, die Sie unterstützen möchten, und die Verwendung von Schnittstellen für plattformspezifische Funktionen.

Da der PCL-Projekttyp in einen `.DLL` kompiliert wird und der Unified API nicht sinnvoll ist, müssen Sie die vorhandenen Datentypen (`int`, `uint`, `float`) im PCL-Quellcode weiterhin verwenden und die Aufrufe in die PCL-Dateien umwandeln. Klassen und Methoden in den Front-End-Anwendungen. Beispiel:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Freigegebene Projekte

Mit dem Projekttyp für freigegebene Medienobjekte können Sie den Quellcode in einem separaten Projekt organisieren, das dann in die einzelnen plattformspezifischen Front-End-apps `#if` eingeschlossen und kompiliert wird. Außerdem können Sie die Compilerdirektiven verwenden, um plattformspezifische Anforderungen.

Die Größe und Komplexität der mobilen Front-End-Anwendungen, die gemeinsam genutzten Code zusammen mit der Größe und Komplexität des freigegebenen Codes verwenden, muss berücksichtigt werden, wenn die Methode der Unterstützung für systemeigene Datentypen in einer plattformübergreifenden Methode ausgewählt wird. Frei gegebenes Ressourcen Projekt.

Basierend auf diesen Faktoren können die folgenden Typen von Lösungen mithilfe der `if __UNIFIED__ ... #endif` Compilerdirektiven implementiert werden, um die Unified API spezifischen Änderungen am Code zu behandeln.

#### <a name="using-duplicate-methods"></a>Verwenden von doppelten Methoden

Betrachten Sie das Beispiel für eine Bibliothek, die Transformationen für rechteckige Daten vornimmt. Wenn die Bibliothek nur eine oder zwei sehr einfache Methoden enthält, können Sie für xamarin. IOS und xamarin. Android doppelte Versionen dieser Methoden erstellen. Beispiel:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #else
            public static float CalculateArea(RectangleF rect) {

                // Calculate area...
                return (rect.Width * rect.Height);

            }
        #endif
        #endregion
    }
}
```

Da die `CalculateArea` Routine im obigen Code sehr einfach ist, haben wir die bedingte Kompilierung verwendet und eine separate, Unified API Version der Methode erstellt. Wenn die Bibliothek andererseits viele Routinen oder mehrere komplexe Routinen enthielt, wäre diese Lösung nicht möglich, da Sie ein Problem darstellen würde, bei dem alle Methoden für Änderungen oder Fehlerbehebungen synchron bleiben.

#### <a name="using-method-overloads"></a>Verwenden von Methoden Überladungen

In diesem Fall kann die Lösung darin bestehen, eine Überladungs Version der Methoden zu erstellen, die 32-Bit-Datentypen verwendet, `CGRect` damit Sie nun als Parameter und/oder als Rückgabewert `RectangleF` `nfloat` verwendet werden. `float` ist eine verlustfreie Konvertierung) und ruft die ursprüngliche Version der Routine auf, um die eigentliche Arbeit zu erledigen. Beispiel:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
using CoreGraphics;
#endif

namespace NativeShared
{
    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        #if __UNIFIED__
            public static nfloat CalculateArea(CGRect rect) {

                // Call original routine to calculate area
                return (nfloat)CalculateArea((RectangleF)rect);

            }
        #endif

        public static float CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }

        #endregion
    }
}

```

Dies ist auch dann eine gute Lösung, wenn sich der Genauigkeits Verlust nicht auf die Ergebnisse der spezifischen Anforderungen Ihrer Anwendung auswirkt.

#### <a name="using-alias-directives"></a>Using-Alias Direktiven

Für Bereiche, in denen der Genauigkeits Verlust ein Problem ist, besteht `using` eine weitere mögliche Lösung darin, mithilfe von-Direktiven einen Alias für Native und CoreGraphics-Datentypen zu erstellen, indem der folgende Code an den Anfang der freigegebenen Quell Code Dateien und alle `int` erforderliche`float` oder-Werte für`nint`oder :`nuint` `uint` `nfloat`

```csharp
#if __UNIFIED__
    // Mappings Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Mappings Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif
```

Der Beispielcode wird dann wie folgt:

```csharp
using System;
using System.Drawing;

#if __UNIFIED__
    // Map Unified CoreGraphic classes to MonoTouch classes
    using RectangleF = global::CoreGraphics.CGRect;
    using SizeF = global::CoreGraphics.CGSize;
    using PointF = global::CoreGraphics.CGPoint;
#else
    // Map Unified types to MonoTouch types
    using nfloat = global::System.Single;
    using nint = global::System.Int32;
    using nuint = global::System.UInt32;
#endif

namespace NativeShared
{

    public class Transformations
    {
        #region Constructors
        public Transformations ()
        {
        }
        #endregion

        #region Public Methods
        public static nfloat CalculateArea(RectangleF rect) {

            // Calculate area...
            return (rect.Width * rect.Height);

        }
        #endregion
    }
}
```

Beachten Sie, dass hier die `CalculateArea` -Methode so geändert wurde, dass `nfloat` anstelle des `float`Standard-zurückgegeben wird. Dies wurde erreicht, damit keine Kompilierungsfehler auftreten, wenn versucht wird  , das Ergebnis `nfloat` der Berechnung implizit zu konvertieren (da beide Werte, die multipliziert werden `nfloat`, vom Typ `float` sind), in einen Rückgabewert.

Wenn der Code auf einem nicht Unified API Gerät kompiliert und ausgeführt wird, ordnet `using nfloat = global::System.Single;` die einem `nfloat` `Single` zu, das implizit in eine `float` konvertiert, sodass die Anwendung `CalculateArea` die Methode ohne Änderungs.


#### <a name="using-type-conversions-in-the-front-end-app"></a>Verwenden von Typkonvertierungen in der Front-End-App

Wenn Ihre Front-End-Anwendungen nur eine Handvoll Aufrufe an die freigegebene Code Bibliothek ausführen, könnte eine andere Lösung darin bestehen, die Bibliothek unverändert zu lassen und beim Aufrufen der vorhandenen Routine eine Typumwandlung in die xamarin. IOS-oder xamarin. Mac-Anwendung vorzunehmen. Beispiel:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Wenn die konsumierende Anwendung Hunderte von Aufrufen der freigegebenen Code Bibliothek durchführt, ist dies möglicherweise keine gute Lösung.

Basierend auf der Architektur unserer Anwendung können wir möglicherweise eine oder mehrere der oben genannten Lösungen verwenden, um systemeigene Datentypen (falls erforderlich) in unserem plattformübergreifenden Code zu unterstützen.


## <a name="xamarinforms-applications"></a>Xamarin. Forms-Anwendungen

Folgendes ist erforderlich, um xamarin. Forms für plattformübergreifende Benutzeroberflächen zu verwenden, die auch für eine Unified API Anwendung freigegeben werden:

- Die gesamte Projekt Mappe muss die Version 1.3.1 (oder höher) des xamarin. Forms-nuget-Pakets verwenden.
- Verwenden Sie für alle benutzerdefinierten xamarin. IOS-renderobjekte die gleichen Typen von Projektmappen, die oben dargestellt werden, je nachdem, wie der UI-Code freigegeben wurde (Shared Project oder PCL)

Wie bei einer plattformübergreifenden Standardanwendung sollten die vorhandenen 32-Bit-Datentypen in den meisten Situationen in jedem gemeinsam genutzten, plattformübergreifenden Code verwendet werden. Die neuen systemeigenen Datentypen sollten nur verwendet werden, wenn eine Mac-oder IOS-API aufgerufen wird, bei der Unterstützung für architekturabhängige Typen erforderlich ist.

Weitere Informationen finden Sie in unserer Dokumentation zum [Aktualisieren vorhandener xamarin. Forms-apps](~/cross-platform/macios/unified/updating-xamarin-forms-apps.md) .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir gesehen, wann die systemeigenen Datentypen in einer Unified API Anwendung verwendet werden, und ihre Implikationen sind plattformübergreifend. Wir haben verschiedene Lösungen vorgestellt, die in Situationen verwendet werden können, in denen die neuen systemeigenen Datentypen in plattformübergreifenden Bibliotheken verwendet werden müssen. Außerdem haben wir eine kurze Anleitung zur Unterstützung einheitlicher APIs in xamarin. Forms-plattformübergreifenden Anwendungen gesehen.



## <a name="related-links"></a>Verwandte Links

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Native Typen](~/cross-platform/macios/nativetypes.md)
- [Freigeben von Code Optionen](~/cross-platform/app-fundamentals/code-sharing.md)
- [Beispiel für Code Freigabe](https://developer.xamarin.com/samples/mobile/SharingCode/)
