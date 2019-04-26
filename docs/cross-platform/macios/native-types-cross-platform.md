---
title: Arbeiten mit nativen Typen in plattformübergreifenden Apps
description: Dieser Artikel behandelt die mithilfe des neuen iOS Unified API Native Typen (Nint, Nuint Nfloat) in einer plattformübergreifenden Anwendung, in dem Code mit nicht-iOS-Geräte wie Android oder Windows Phone-Betriebssystemen freigegeben.
ms.prod: xamarin
ms.assetid: B9C56C3B-E196-4ADA-A1DE-AC10D1001C2A
author: asb3993
ms.author: amburns
ms.date: 04/07/2016
ms.openlocfilehash: 489d2a76e6eff661360b24d1872ed1343c74b85e
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61261180"
---
# <a name="working-with-native-types-in-cross-platform-apps"></a>Arbeiten mit nativen Typen in plattformübergreifenden Apps

_Dieser Artikel behandelt die mithilfe des neuen iOS Unified API Native Typen (Nint, Nuint Nfloat) in einer plattformübergreifenden Anwendung, in dem Code mit nicht-iOS-Geräte wie Android oder Windows Phone-Betriebssystemen freigegeben._


Die 64-Typen, mit denen die IOS- und Mac-APIs systemeigene Typen zusammenarbeiten. Wenn Sie freigegebenen Code, der ebenfalls auf Android oder Windows ausgeführt wird schreiben, müssen Sie die Konvertierung von Unified-Typen in reguläre .NET-Typen zu verwalten, die Sie freigeben können.

Dieses Dokument erläutert die Möglichkeiten für die Zusammenarbeit mit der Unified API aus dem freigegebenen/allgemeiner Code.

## <a name="when-to-use-the-native-types"></a>Verwenden Sie die systemeigenen Typen

Schließen Sie Xamarin.iOS- und Xamarin.Mac Unified-APIs nach wie vor die `int`, `uint` und `float` -Datentypen, sowie die `RectangleF`, `SizeF` und `PointF` Typen. Diese vorhandenen Datentypen sollten weiterhin in freigegebene, plattformübergreifenden Code verwendet werden. Die neuen systemeigenen Datentypen sollte nur verwendet werden, bei einem Aufruf einer Mac oder iOS-API unterstützt, in dem Architektur-fähigen Typen erforderlich sind.

Je nach Art des Codes genutzt wird, möglicherweise, plattformübergreifenden Code für den Umgang mit müssen möglicherweise, die `nint`, `nuint` und `nfloat` -Datentypen. Zum Beispiel: eine Bibliothek, die Transformationen für rechteckige Daten behandelt, die zuvor verwendet wurde `System.Drawing.RectangleF` Funktionen zwischen Versionen eine app, Xamarin.iOS und Xamarin.Android freigeben müssen aktualisiert werden, um systemeigene Typen für iOS behandelt.

Wie diese Änderungen behandelt werden, hängt davon ab, der Größe und Komplexität der Anwendung und Form von gemeinsamen Codes verwendet wurde, wie wir in den folgenden Abschnitten sehen.

## <a name="code-sharing-considerations"></a>Überlegungen für die Codefreigabe

Gemäß der [Sharing Code Options](~/cross-platform/app-fundamentals/code-sharing.md) zu dokumentieren, gibt es zwei Hauptmethoden zum Freigeben von Code für plattformübergreifende Projekte: Freigegebene Projekte "und" Portable Klassenbibliotheken. Welcher der beiden Arten der verwendet wurde, werden die Optionen, die wir bei der Verarbeitung der systemeigene Datentypen in plattformübergreifenden Code zu beschränken.

### <a name="portable-class-library-projects"></a>Portable Class Library-Projekten

Eine Portable Klassenbibliothek (PCL) können Sie für die Plattformen, die Sie verwenden möchten, unterstützen, und verwenden Schnittstellen, um plattformspezifische Funktionalität bereitzustellen.

Da der Typ der PCL-Projekt kompiliert wird, nach unten zu einer `.DLL` und hat keinen Sinn der Unified API, Sie werden dazu gebracht werden, weiterhin die vorhandenen Datentypen verwenden (`int`, `uint`, `float`) in der PCL-Quellcode, und geben die Aufrufe an die PCLs umgewandelt Klassen und Methoden in den Front-End-Anwendungen. Beispiel:

```csharp
using NativePCL;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

### <a name="shared-projects"></a>Freigegebene Projekte

Der Typ des freigegebenen Projekts können Sie Ihren Quellcode in einem separaten Projekt, das dann enthalten und kompiliert abruft in den einzelnen plattformspezifischen Front-End-apps zu organisieren und `#if` Compiler-Direktiven, die nach Bedarf verwalten Clientplattform-spezifische Anforderungen an.

Die Größe und Komplexität der im Vordergrund, beenden Sie mobile Anwendungen, die verbrauchen freigegebener Code zusammen mit der Größe und die Komplexität des Codes gemeinsam genutzt, bei der Auswahl der Unterstützung für systemeigene Daten plattformübergreifend mit Typen berücksichtigt werden muss die Freigegebene Ressourcenprojekt-Typ.

Basierend auf diesen Faktoren, die folgenden Arten von Lösungen implementiert werden könnte mithilfe der `if __UNIFIED__ ... #endif` Compiler-Direktiven, die bestimmten Unified API-Änderungen an den Code zu behandeln.

#### <a name="using-duplicate-methods"></a>Verwenden doppelte Methoden

Betrachten Sie beispielsweise eine Bibliothek, die Transformationen für rechteckige Daten, die oben angegebenen vor sich geht. Wenn die Bibliothek, die nur eine oder zwei sehr einfache Methoden enthält, können Sie auch doppelte Versionen dieser Methoden für Xamarin.iOS und Xamarin.Android erstellen. Zum Beispiel:

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

Im obigen Code da die `CalculateArea` Routine ist sehr einfach, wir haben für die bedingte Kompilierung verwendet und erstellt ein separates, Unified API-Version der Methode. Andererseits, wenn die Bibliothek viele Routinen oder mehrere komplexe Routinen enthalten, wäre diese Lösung nicht durchführbar, es ein Problem mit dem synchronisieren alle Methoden für die Änderungen oder Fehlerbehebungen darstellen würde.

#### <a name="using-method-overloads"></a>Überladungen mit-Methode

Die Lösung in diesem Fall wäre, erstellen Sie eine Version der Überladung der Methoden, die 32-Bit-Datentypen verwenden, so dass sie jetzt benötigen `CGRect` als Parameter und/oder Rückgabewert, konvertieren Sie diesen Wert auf eine `RectangleF` (zu wissen, dass die Umwandlung von `nfloat` zu `float` ist eine Konvertierung verlustbehaftete), und rufen Sie die ursprüngliche Version der Routine, um die eigentliche Arbeit leisten. Zum Beispiel:

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

In diesem Fall ist dies eine gute Lösung, solange der Verlust der Genauigkeit der Ergebnisse für die spezifischen Anforderungen Ihrer Anwendung nicht beeinträchtigt.

#### <a name="using-alias-directives"></a>Using-Alias-Direktiven

Für Bereiche, in denen der Verlust an Genauigkeit ein Problem ist, eine weitere mögliche Lösung ist die Verwendung `using` Anweisungen, um einen Alias für Native und CoreGraphics-Datentypen zu erstellen, durch das Einschließen von des folgenden Codes am Anfang der freigegebenen Quellcodedateien, und konvertieren alle benötigt `int`, `uint` oder `float` Werte `nint`, `nuint` oder `nfloat`:

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

Damit unser Beispielcode wird:

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

Beachten Sie, dass auch hier haben wir geändert der `CalculateArea` -Methodenrückgabe eine `nfloat` anstelle des standardmäßigen `float`. Dies erfolgte, damit wir einen Kompilierungsfehler, die versuchen, nicht abgerufen werden sollen _implizit_ konvertieren die `nfloat` Ergebnis der Berechnung (da sind beide Werte multiplizierende `nfloat`) in eine `float` Rückgabewert.

Wenn der Code kompiliert ist, und auf einem Gerät nicht Unified API führen die `using nfloat = global::System.Single;` zugeordnet der `nfloat` auf eine `Single` konvertiert der implizit in eine `float` die verarbeitende Front-End-Anwendung aufrufen, sodass die `CalculateArea` -Methode ohne Änderung.


#### <a name="using-type-conversions-in-the-front-end-app"></a>Verwenden von Typkonvertierungen in der Front-End-App

Falls Ihre Front-End-Anwendungen, die nur eine Reihe von Aufrufen Ihrer Bibliothek freigegebener Code vornehmen, wird eine andere Lösung wäre die Bibliothek, die unverändert zu lassen, und geben Sie in der Xamarin.iOS oder Xamarin.Mac-Anwendung die Umwandlung beim Aufrufen der vorhandenen Routine. Beispiel:

```csharp
using NativeShared;
...

CGRect rect = new CGRect (0, 0, 200, 200);
Console.WriteLine ("Rectangle Area: {0}", Transformations.CalculateArea ((RectangleF)rect));
```

Wenn die konsumierende Anwendung Hunderte von der Bibliothek für freigegebenen Code Aufrufe vornimmt, dies in diesem Fall möglicherweise nicht die ideale Lösung.

Auf der Basis der Anwendung Architektur kann am Ende über eine oder mehrere der oben aufgeführten Lösungen zur Unterstützung von systemeigener Daten in unserer plattformübergreifenden Code-Typen (wo erforderlich).


## <a name="xamarinforms-applications"></a>Xamarin.Forms-Anwendungen

Folgendes ist erforderlich, um Xamarin.Forms für plattformübergreifende Benutzeroberflächen zu verwenden, die auch mit einer Unified API-Anwendung freigegeben wird:

- Die gesamte Lösung muss Version 1.3.1 verwenden (oder höher) das Xamarin.Forms-NuGet-Pakets.
- Verwenden Sie für alle benutzerdefinierten Xamarin.iOS-rendert dieselbe Art von Lösungen, die oben aufgeführten basierend auf wie die UI-Code freigegeben (freigegebenes Projekt oder PCL) wurde.

Wie in einer plattformübergreifenden Standardanwendungen sollten die vorhandenen 32-Bit-Datentypen in gemeinsam genutzter, plattformübergreifenden Code für die meisten alle Situationen verwendet werden. Die neuen systemeigenen Datentypen sollte nur verwendet werden, bei einem Aufruf einer Mac oder iOS-API unterstützt, in dem Architektur-fähigen Typen erforderlich sind.

Weitere Informationen finden Sie unserem [Aktualisieren von vorhandenen Xamarin.Forms-Apps](https://developer.xamarin.com/guides/cross-platform/macios/updating-xamarin-forms-apps/) Dokumentation.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir finden Sie, wenn wir die systemeigene Datentypen in einer vereinheitlichten-API-Anwendung und ihre Auswirkungen auf die plattformübergreifende verwenden sollten. Wir haben mehrere Lösungen angezeigt, die in Situationen verwendet werden kann, in denen die neuen systemeigenen Datentypen in plattformübergreifende Bibliotheken verwendet werden müssen. Und wir haben gesehen, dass eine kurze Einführung in Unified-APIs in plattformübergreifende Xamarin.Forms-Anwendungen unterstützen.



## <a name="related-links"></a>Verwandte Links

- [Unified API](~/cross-platform/macios/unified/index.md)
- [Native Typen](~/cross-platform/macios/nativetypes.md)
- [Optionen für die Codefreigabe](~/cross-platform/app-fundamentals/code-sharing.md)
- [Freigeben von Code-Beispiel](https://developer.xamarin.com/samples/mobile/SharingCode/)
