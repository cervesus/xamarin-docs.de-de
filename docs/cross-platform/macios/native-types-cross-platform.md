---
title: Arbeiten mit systemeigenen Typen in plattformübergreifende Apps
description: Dieser Artikel umfasst, verwenden die neue iOS Unified API systemeigene Typen (Nint, Nuint, Nfloat) in einer plattformübergreifenden-Anwendung, in dem Code mit nicht-iOS-Geräten, z. B. Android oder Windows Phone Betriebssysteme freigegebenen.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 0b32cb68174183fd094f72a7ab20f7ed52b278ee
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Arbeiten mit systemeigenen Typen in plattformübergreifende Apps

_Dieser Artikel umfasst, verwenden die neue iOS Unified API systemeigene Typen (Nint, Nuint, Nfloat) in einer plattformübergreifenden-Anwendung, in dem Code mit nicht-iOS-Geräten, z. B. Android oder Windows Phone Betriebssysteme freigegebenen._


Die 64-Typen systemeigene Typen, mit denen die IOS- und Mac-APIs arbeiten. Wenn Sie freigegebenen Code, der auch unter Android oder Windows ausgeführt wird schreiben, müssen Sie die Umwandlung von Typen Unified in regulären .NET-oder Schematypen verwalten, die Sie freigeben können.

Dieses Dokument erläutert die verschiedene Möglichkeiten, mit der einheitliche API aus dem freigegebenen/gemeinsamen Code zusammenwirken.

## <a name="when-to-use-the-native-types"></a>Verwenden von systemeigenen Typen

Xamarin.iOS und Xamarin.Mac Unified APIs schließen nach wie vor die `int`, `uint` und `float` Datentypen, sowie die `RectangleF`, `SizeF` und `PointF` Typen. Diese vorhandenen Datentypen sollten weiterhin in freigegebene, plattformübergreifenden Code verwendet werden. Die neuen systemeigenen Datentypen sollte nur verwendet werden, bei einem Aufruf an eine Mac oder iOS-API unterstützen, in denen für Architektur unterstützende erforderlich sind.

Je nach Art des Codes wiederverwendet wird, möglicherweise, auf dem plattformübergreifenden Code für den Umgang mit müssen möglicherweise, die `nint`, `nuint` und `nfloat` -Datentypen. Zum Beispiel: eine Bibliothek, die Transformationen auf rechteckige Daten behandelt, die zuvor verwendet wurde `System.Drawing.RectangleF` Funktionalität zwischen verschiedenen Versionen von Xamarin.iOS und Xamarin.Android einer App freigeben müssen aktualisiert werden, um systemeigene Typen auf iOS zu behandeln.

Wie diese Änderungen behandelt werden, hängt davon ab, der Größe und Komplexität der Anwendung und Form des Codes freigeben, die verwendet wurde, da wir in den folgenden Abschnitten angezeigt wird.

## <a name="code-sharing-considerations"></a>Überlegungen für die Codefreigabe

Wie in der [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) dokumentieren, gibt es zwei Hauptmethoden zum Freigeben von Code für die plattformübergreifende Projekte: freigegebene Projekte und portablen Klassenbibliotheken. Welche zwei Arten verwendet wurde, Beschränken der Möglichkeiten, die wir haben, wenn Sie die systemeigenen Datentypen in plattformübergreifenden Code behandeln.

### <a name="portable-class-library-projects"></a>Portable Klassenbibliotheksprojekte

Eine Portable Klassenbibliothek (PCL) können Sie die Zielplattformen aus, die Sie unterstützen, und Verwenden von Schnittstellen Clientplattform-spezifische Funktionen bereitstellen möchten.

Da der Typ der PCL-Projekt kompiliert ist ein `.DLL` und sie hat keinen Sinn der einheitliche API, Sie müssen die vorhandenen Datentypen weiterhin mit erzwungen werden (`int`, `uint`, `float`) in der PCL-Quellcode und Typ der Aufrufe an die PCLs umgewandelt Klassen und Methoden in der Front-End-Anwendungen. Beispiel:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Gemeinsam genutzte Projekte

Der freigegebene Asset-Projekttyp können Sie zum Organisieren von Quellcode in einem separaten Projekt, die dann enthalten und kompiliert ruft in den einzelnen plattformspezifischen front-End-apps, und verwenden `#if` Compilerdirektiven nach Bedarf verwalten Clientplattform-spezifische Anforderungen an.

Die Größe und Komplexität der Vorderseite enden mobile Anwendungen, die verbrauchen freigegebenen Code, zusammen mit der Größe und die Komplexität des Codes wiederverwendet wird, muss bei der Auswahl der Methode der Unterstützung für systemeigene Daten plattformübergreifende mit berücksichtigt werden die Freigegebene Asset-Projekttyp.

Auf Basis dieser Faktoren, die folgenden Typen von Projektmappen implementiert werden könnten mithilfe der `if __UNIFIED__ ... #endif` Compiler-Direktiven, die bestimmten einheitliche API-Änderungen an den Code zu behandeln.

#### <a name="using-duplicate-methods"></a>Verwenden doppelte Methoden

Nehmen Sie als Beispiel einer Bibliothek, die Transformationen auf rechteckige Daten, die oben genannte ausführt. Wenn die Bibliothek nur ein oder zwei einfache Methoden enthält, möchten Sie möglicherweise doppelte Versionen dieser Methoden für Xamarin.iOS und Xamarin.Android zu erstellen. Zum Beispiel:

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

Im obigen Code da die `CalculateArea` Routine ist sehr einfach, wir haben bedingten Kompilierung verwendet und erstellt ein separates, eine einheitliche API-Version der Methode. Andererseits, wenn die Bibliothek viele Routinen oder mehrere komplexe Routinen enthalten, würde diese Lösung nicht möglich, sein, wie es ein Problem mit dem synchronisieren alle Methoden für Änderungen oder bei Fehlerbehebungen darstellen würde.

#### <a name="using-method-overloads"></a>Überladungen mit-Methode

In diesem Fall die Projektmappe so erstellen eine Überladung Version der 32-Bit-Datentypen verwenden, sodass jetzt sie nehmen Methoden möglicherweise `CGRect` als Parameter und/oder Rückgabewert, konvertieren Sie diesen Wert auf eine `RectangleF` (zu wissen, dass die Umwandlung von `nfloat` zu `float` ist eine Konvertierung lossy), und rufen Sie die ursprüngliche Version der Routine, um die eigentliche Arbeit. Zum Beispiel:

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

Erneut ist dies eine gute Lösung als der Verlust an Genauigkeit Auswirkungen auf die Ergebnisse nach Bedarf für Ihre Anwendung nicht.

#### <a name="using-alias-directives"></a>Using-Alias-Direktiven

Für Bereiche, in denen der Verlust an Genauigkeit ein Problem ist, eine weitere mögliche Lösung ist die Verwendung `using` Direktive, um einen Alias für den einheitlichen und den CoreGraphics Datentypen zu erstellen, indem z. B. den folgenden Code am Anfang der freigegebenen Quellcodedateien und konvertieren alle benötigt `int`, `uint` oder `float` Werte `nint`, `nuint` oder `nfloat`:

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

So, dass unsere Beispielcode dann wird:

```csharp
using System;
using System.Drawing;

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

Beachten Sie, dass auch hier haben wir geändert die `CalculateArea` -Methodenrückgabe eine `nfloat` anstelle des `float`. Diese wurde vorgenommen, damit wir keinen Kompilierungsfehler beim erhalten würden _implizit_ konvertieren die `nfloat` Ergebnis unserer Berechnung (da beide Werte, die zu multiplizierende sind `nfloat`) in einer `float` Rückgabewert.

Wenn der Code kompiliert ist, und Sie auf einem Gerät nicht einheitliche API führen der `using nfloat = global::System.Single;` ordnet die `nfloat` auf eine `Single` konvertiert der implizit in ein `float` ermöglicht die Front-End-Anwendung aufrufen, die `CalculateArea` Methode ohne Änderung.


#### <a name="using-type-conversions-in-the-front-end-app"></a>Verwenden Typkonvertierungen in der Front-End-App

Wenn die front-End-Anwendungen nur eine Handvoll Aufrufe Medienbibliothek freigegebenen Code vornehmen, wird eine andere Lösung könnte darin bestehen, die Bibliothek, die unverändert lassen und geben Sie in der Anwendung Xamarin.iOS oder Xamarin.Mac umwandeln, wenn die vorhandene Routine aufrufen. Beispiel:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Wenn die konsumierende Anwendung Hunderte von Aufrufen an die freigegebene Codebibliothek herstellt, diese erneut, möglicherweise keine gute Lösung.

Anhand unserer Anwendung Architektur, wir möglicherweise enden mit einem oder mehreren der oben aufgeführten Lösungen zur Unterstützung systemeigener Daten (sofern erforderlich) in unserer plattformübergreifenden Code eingibt.


## <a name="xamarinforms-applications"></a>Xamarin.Forms-Anwendungen

Folgendes ist erforderlich, um Xamarin.Forms für plattformübergreifende Benutzeroberflächen verwenden, die durch eine einheitliche API-Anwendung auch freigegeben wird:

- Die gesamte Lösung muss Version 1.3.1 verwenden (oder höher) das Xamarin.Forms-NuGet-Pakets.
- Verwenden Sie die gleichen Typen von Projektmappen, die oben aufgeführten für alle benutzerdefinierten Xamarin.iOS rendert basierend auf wie die Benutzeroberflächencode freigegebenen (freigegebenen Projekts oder PCL) wurde.

Wie eine plattformübergreifende Standardanwendungen sollte die vorhandenen 32-Bit-Datentypen in freigegebenen, plattformübergreifenden Code für die meisten alle Situationen verwendet werden. Die neuen systemeigenen Datentypen sollte nur verwendet werden, bei einem Aufruf an eine Mac oder iOS-API unterstützen, in denen für Architektur unterstützende erforderlich sind.

Weitere Informationen finden Sie unter unsere [Aktualisieren vorhandener Xamarin.Forms Apps](http://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir finden Sie, wenn wir die systemeigenen Datentypen in eine einheitliche API-Anwendung und deren Auswirkungen auf die plattformübergreifende verwenden soll. Wir haben mehrere Lösungen angezeigt, die in Situationen verwendet werden kann, wobei die neue systemeigene Datentypen an plattformübergreifende Bibliotheken verwendet werden muss. Und eine kurze Einführung in die Unterstützung von APIs Unified in Xamarin.Forms plattformübergreifende Anwendungen kennen gelernt haben.



## <a name="related-links"></a>Verwandte Links

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Native Typen](~/cross-platform/macios/nativetypes.md)
- [Optionen für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
- [Codebeispiel freigeben](https://developer.xamarin.com/samples/mobile/SharingCode/)
