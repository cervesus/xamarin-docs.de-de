---
title: SVG-Pfaddaten
description: Definieren von Textzeichenfolgen im Scalable Vector Graphics-Format verwenden
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: charlespetzold
ms.author: chape
ms.date: 05/24/2017
ms.openlocfilehash: fe9699894224d9a33b3a79e9b5bcd4cd41c635dd
ms.sourcegitcommit: 66807f8927d472fbfd0ff8bc77cea9b37e7b9a4f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/05/2018
---
# <a name="svg-path-data"></a>SVG-Pfaddaten

_Definieren von Textzeichenfolgen im Scalable Vector Graphics-Format verwenden_

Die `SKPath` Klasse unterstützt die Definition von Objekten der gesamten Pfad aus Textzeichenfolgen in einem Format, das durch die Spezifikation (Scalable Vector Graphics, SVG) hergestellt. Sie werden weiter unten in diesem Artikel angezeigt, wie Sie einen vollständigen Pfad, wie diese in einer Textzeichenfolge darstellen können:

![](path-data-images/pathdatasample.png "Ein Beispielpfad mit SVG-Pfaddaten definiert")

SVG wird eine XML-basierte Grafiken Programmiersprache für Webseiten. Da SVG Pfade im Markup anstelle einer Reihe von Funktionsaufrufen definiert werden kann muss, enthält der SVG-standard eine sehr präzise Methode, eine gesamte Grafikpfad als Textzeichenfolge angeben.

In SkiaSharp wird dieses Format als "SVG-Pfaddaten." bezeichnet. Das Format wird auch unterstützt, in der Windows-XAML-basierte programmierumgebungen, einschließlich Windows Presentation Foundation und der universellen Windows-Plattform, bei der bekannt als die [Markup Pfadsyntax](https://msdn.microsoft.com/library/ms752293%28v=vs.110%29.aspx) oder [verschieben und zeichnen Sie die Syntax Befehle](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Es kann auch als ein Exchange-Format für Vektorgrafiken, insbesondere in textbasierten wie z. B. XML-Dateien dienen.

SkiaSharp definiert zwei Methoden mit den Wörtern `SvgPathData` in ihren Namen:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Die statische [ `ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) -Methode konvertiert eine Zeichenfolge in eine `SKPath` -Objekt, während [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) konvertiert ein `SKPath` Objekt in eine Zeichenfolge.

Hier ist eine SVG-Zeichenfolge für einen fünf Spitzen Stern, dessen Mitte sich an den Punkt (0, 0) mit einem Radius von 100:

```csharp
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Die Buchstaben sind Befehle, die erstellen ein `SKPath` Objekt. `M` Gibt an, eine `MoveTo` aufzurufen, `L` ist `LineTo`, und `Z` ist `Close` zu eine Kontur zu schließen. Jedes Paar bietet eine X- und Y-Koordinate eines Punkts. Beachten Sie, dass die `L` Befehl gefolgt von mehreren Punkten, die durch Kommas getrennt. In einer Reihe von Koordinaten und Punkte, Kommas und Leerzeichen werden identisch behandelt. Einige Programmierer Kommas zwischen den X- und Y-Koordinaten statt zwischen den Punkten versetzen möchten, aber durch Kommas oder Leerzeichen sind nur erforderlich, um Mehrdeutigkeiten zu vermeiden. Dies ist durchaus gültig:

```csharp
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Die Syntax der SVG-Pfaddaten formal finden Sie im [Abschnitt 8.3 der SVG-Spezifikation](http://www.w3.org/TR/SVG11/paths.html#PathData). Hier wird eine Zusammenfassung:

## <a name="moveto"></a>**MoveTo**

```csharp
M x y
```

Dies startet eine neue Kontur im Pfad durch Festlegen der aktuellen Position. Pfaddaten beginnen sollte immer mit einem `M` Befehl.

## <a name="lineto"></a>**LineTo**

```csharp
L x y ...
```

Mit diesem Befehl wird eine gerade Linie umwandeln (oder Zeilen) auf den Pfad und die neue aktuelle Position bis zum Ende der letzten Zeile. Führen Sie die `L` mit mehrere Paare von *x* und *y* Koordinaten.

## <a name="horizontal-lineto"></a>**Horizontale Linie**

```csharp
H x ...
```

Mit diesem Befehl wird eine horizontale Linie auf den Pfad und die neue aktuelle Position bis zum Ende der Linie. Führen Sie die `H` Befehl mit mehreren *x* Koordinaten, aber es nicht sinnvoll.

## <a name="vertical-line"></a>**Vertikale Linie**

```csharp
V y ...
```

Mit diesem Befehl wird eine vertikale Linie auf den Pfad und die neue aktuelle Position bis zum Ende der Zeile.

## <a name="close"></a>**Schließen**

```csharp
Z
```

Die `C` Befehl schließt die Kontur durch Hinzufügen einer geraden Linie von der aktuellen Position, an den Anfang der Kontur.

## <a name="arcto"></a>**ArcTo**

Der Befehl die Kontur ein elliptischen Bogens hinzuzufügende ist weitem den komplexesten Befehl in der gesamten SVG Pfaddaten Spezifikation. Es ist der einzige Befehl, in dem Zahlen etwas anderes als Koordinatenwerte darstellen können:

```csharp
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

Die *Rx* und *schnellen* Parameter sind die horizontalen und vertikalen Radien der Ellipse. Die *Winkel der Drehung* wird in Grad im Uhrzeigersinn.

Legen Sie die *große Bogen-Flag* für den großen Bogen auf 1 oder 0 für den kleinen Bogen.

Festlegen der *-Sweep-Flag* 1 im Uhrzeigersinn und 0 für gegen den Uhrzeigersinn.

Der Bogen gezeichnet wird, zu dem Punkt (*x*, *y*), das die neue Position wird.

## <a name="cubicto"></a>**CubicTo**

```csharp
C x1 y1 x2 y2 x3 y3 ...
```

Dieser Befehl fügt eine kubische Bézier-Kurve aus der aktuellen Position bis (*X3*, *y3*), das die neue Position wird. Die Punkte (*X1*, *y1*) und (*X2*, *y2*) Steuerpunkte sind.

Mehrere Bézier-Kurven können angegeben werden, über einen einzelnen `C` Befehl. Die Anzahl von Punkten muss ein Vielfaches von 3 sein.

Es gibt auch ein "smooth" Bézier-Kurve-Befehl:

```csharp
S x2 y2 x3 y3 ...
```

Dieser Befehl sollte einen normalen Bézier-Befehl folgen, (obwohl dies nicht unbedingt erforderlich ist). Smooth Bézier-Befehl berechnet den ersten Kontrollpunkt, damit es eine Reflektion des zweiten Kontrollpunkts von der vorherigen Bézier, um deren gegenseitige Punkt ist. Diese drei Punkte sind daher kollineare, und die Verbindung zwischen den zwei Bézier-Kurven smooth ist.

## <a name="quadto"></a>**QuadTo**

```csharp
Q x1 y1 x2 y2 ...
```

Die Anzahl von Punkten muss ein Vielfaches von 2 sein, für quadratische Bézier-Kurven. Der Kontrollpunkt ist (*X1*, *y1*) und der Endpunkt (und die neue Position) ist (*X2*, *y2*)

Es gibt auch ein reibungslose quadratische Kurve-Befehl:

```csharp
T x2 y2 ...
```

Der Kontrollpunkt wird basierend auf den Kontrollpunkt der Kurve an der vorherigen quadratische berechnet.

Alle diese Befehle stehen auch in "relative" Versionen verfügbar sind, in denen die Koordinaten relativ zur aktuellen Position. Diese relativen Befehle beginnen mit Kleinbuchstaben enthalten, z. B. `c` statt `C` für die relative Version kubische Bézier-Befehl.

Dies ist das Ausmaß der Definition der SVG-Pfad-Daten. Es gibt keine Möglichkeit für Gruppen von Befehlen wiederholter oder für jede Art von Berechnung ausführen. Befehle für `ConicTo` oder anderen Typen von Bogen Spezifikationen sind nicht verfügbar.

Die statische [ `SKPath.ParseSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ParseSvgPathData/p/System.String/) Methode erwartet eine gültige Zeichenfolge der SVG-Befehle. Wenn Syntaxfehler erkannt wird, gibt die Methode `null`. Dies ist die einzige Fehleranzeige.

Die [ `ToSvgPathData` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPath.ToSvgPathData()/) Methode eignet sich zum Abrufen von Daten der SVG-Pfads aus einer vorhandenen `SKPath` Objekt in ein anderes Programm zu übertragen, oder in einem textbasierten Format wie z. B. XML gespeichert. (Die `ToSvgPathData` Methode wird im Beispielcode in diesem Artikel nicht vorgeführt.) Führen Sie *nicht* erwarten `ToSvgPathData` zum Zurückgeben einer Zeichenfolge entspricht genau der Methodenaufrufe, die den Pfad erstellt. Insbesondere werden Sie feststellen, dass Bögen mit mehreren konvertiert werden `QuadTo` Befehle aus, und ist wie in der vom zurückgegebenen Pfaddaten angezeigt werden `ToSvgPathData`.

Die **Pfad Daten-Hello** Seite Spells das Wort "HELLO" Pfaddaten für SVG. Sowohl die `SKPath` und `SKPaint` Objekte werden als Felder in definiert die [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) Klasse:

```csharp
public class PathDataHelloPage : ContentPage
{
    SKPath helloPath = SKPath.ParseSvgPathData(
        "M 0 0 L 0 100 M 0 50 L 50 50 M 50 0 L 50 100" +                // H
        "M 125 0 C 60 -10, 60 60, 125 50, 60 40, 60 110, 125 100" +     // E
        "M 150 0 L 150 100, 200 100" +                                  // L
        "M 225 0 L 225 100, 275 100" +                                  // L
        "M 300 50 A 25 50 0 1 0 300 49.9 Z");                           // O

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Blue,
        StrokeWidth = 10,
        StrokeCap = SKStrokeCap.Round,
        StrokeJoin = SKStrokeJoin.Round
    };
    ...
}
```

Der Pfad, definieren die Zeichenfolge beginnt in der linken oberen Ecke an dem Punkt (0, 0). Jedes Buchstabens ist 50 Einheiten, die Breite und Höhe von 100 Einheiten, und durch eine andere 25 Einheiten Buchstaben getrennt werden, was bedeutet, dass der gesamte Pfad 350 Einheiten breit ist.

"H" von "Hello" besteht aus drei einzeilige Konturen, während 'E' zwei verbundene kubische Bézier-Kurven wird. Beachten Sie, dass die `C` Befehl gefolgt von sechs Punkte und zwei der Steuerpunkte Y-Koordinaten der – 10 und 110, die sie außerhalb des Bereichs, der die Y-Koordinaten der anderen Buchstaben setzt verfügen. Die "L" ist, zwei miteinander verbundenen Linien, während die ' O' ist eine Ellipse, die mit gerendert wird ein `A` Befehl.

Beachten Sie, dass die `M` -Befehl, der die letzte Kontur beginnt legt die Position fest, um den Punkt (350, 50), also die vertikale Mitte des linken-Seite der ' O'. Durch das erste Zahlen folgt die `A` Befehl, das die Ellipse hat einen horizontalen Radius des 25 und dem vertikalen Radius von 50. Der Endpunkt wird angegeben, indem die letzten paar der Zahlen in den `A` Befehl, der den Punkt (300, 49,9) darstellt. Die ist absichtlich nur geringfügig von den Ausgangspunkt. Wenn der Endpunkt den Ausgangspunkt gleich festgelegt ist, wird der Bogen nicht gerendert werden. Um eine vollständige Ellipse zu zeichnen, müssen Sie festlegen des Endpunkts nahe an (aber nicht gleich) den Ausgangspunkt, oder Sie verwenden müssen, zwei oder mehr `A` Befehle, die für jedes Teil der vollständigen Ellipse.

Möglicherweise möchten die Seite Konstruktor die folgende Anweisung hinzu, und legen Sie einen Breakpoint um die resultierende Zeichenfolge zu untersuchen:

```csharp
string str = helloPath.ToSvgPathData();
```

Erfahren Sie, dass der Bogen durch eine lange Reihe von ersetzt wurde `Q` Befehle für eine schrittweise Annäherung an den Bogen mit quadratische Bézier-Kurven.

Die `PaintSurface` Ereignishandler ruft die engen Grenzen des Pfads, zzgl. der Steuerpunkte für 'E' und ' O' Kurven. Die drei Transformationen die Mitte des Pfads zu dem Punkt (0, 0) verschieben, skalieren den Pfad auf die Größe des Zeichenbereichs (aber berücksichtigt auch die Strichbreite) und klicken Sie dann die Mitte des Pfads in die Mitte des Zeichenbereichs verschieben:

```csharp
public class PathDataHelloPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKRect bounds;
        helloPath.GetTightBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(info.Width / (bounds.Width + paint.StrokeWidth),
                     info.Height / (bounds.Height + paint.StrokeWidth));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(helloPath, paint);
    }
}
```

Der Pfad füllt den Zeichenbereich, der bei der Anzeige im Querformat günstiger aussieht:

[![](path-data-images/pathdatahello-small.png "Dreifacher Screenshot, der den Pfad Daten-Hello-Seite")](path-data-images/pathdatahello-large.png#lightbox "dreifacher Screenshot, der den Pfad Daten-Hello-Seite")

Die **Pfad Daten Cat** Seite ähnelt. Der Pfad und Paint-Objekte werden als Felder in definiert die [ `PathDataCatPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) Klasse:

```csharp
public class PathDataCatPage : ContentPage
{
    SKPath catPath = SKPath.ParseSvgPathData(
        "M 160 140 L 150 50 220 103" +              // Left ear
        "M 320 140 L 330 50 260 103" +              // Right ear
        "M 215 230 L 40 200" +                      // Left whiskers
        "M 215 240 L 40 240" +
        "M 215 250 L 40 280" +
        "M 265 230 L 440 200" +                     // Right whiskers
        "M 265 240 L 440 240" +
        "M 265 250 L 440 280" +
        "M 240 100" +                               // Head
        "A 100 100 0 0 1 240 300" +
        "A 100 100 0 0 1 240 100 Z" +
        "M 180 170" +                               // Left eye
        "A 40 40 0 0 1 220 170" +
        "A 40 40 0 0 1 180 170 Z" +
        "M 300 170" +                               // Right eye
        "A 40 40 0 0 1 260 170" +
        "A 40 40 0 0 1 300 170 Z");

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 5
    };
    ...
}
```

Der Anfang einer Katze ist ein Kreis, und er wird hier gerendert, mit zwei `A` Befehle, von denen jeder ein Halbkreis zeichnet. Beide `A` Befehle für die Head horizontale und vertikale Radii 100 definieren. Der ersten Bogen beginnt bei (240, 100) und endet (240, 300), das den Ausgangspunkt für den zweiten Bogen, der wieder an endet wird (240, 100).

Die beiden Augen werden ebenfalls gerendert, mit zwei `A` Befehle, und wie bei der Cat-Head, das zweite `A` Befehl endet am gleichen Punkt als Anfang der ersten `A` Befehl. Allerdings diese Paare von `A` keine Befehle eine Ellipse definiert. Die mit jeder Bogens 40 Einheiten und der Radius ist auch 40 Einheiten, was bedeutet, dass diese Bögen nicht vollständige Semicircles sind.

Die `PaintSurface` Handler führt Sie ähnliche Transformationen wie das vorherige Beispiel, jedoch legt eine einzelne `Scale` Faktor, der das Seitenverhältnis beibehalten, und geben Sie einen wenig Rand aus, damit die Cat Whisker die Seiten des Bildschirms berühren nicht:

```csharp
public class PathDataCatPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Black);

        SKRect bounds;
        catPath.GetBounds(out bounds);

        canvas.Translate(info.Width / 2, info.Height / 2);

        canvas.Scale(0.9f * Math.Min(info.Width / bounds.Width,
                                     info.Height / bounds.Height));

        canvas.Translate(-bounds.MidX, -bounds.MidY);

        canvas.DrawPath(catPath, paint);
    }
}
```

Hier wird das Programm auf allen drei Plattformen ausgeführt:

[![](path-data-images/pathdatacat-small.png "Dreifacher Screenshot der Seite Pfad Daten Cat")](path-data-images/pathdatacat-large.png#lightbox "dreifacher Screenshot von der Seite "Pfad Daten Cat"")

Normalerweise, wenn ein `SKPath` Objekt als ein Feld definiert ist, müssen die Kontur des Pfads in den Konstruktor oder eine andere Methode definiert werden. Bei Verwendung von SVG-Pfaddaten jedoch haben Sie angezeigt, dass der Pfad vollständig in der Field-Definition angegeben werden kann.

Die frühere **problematischen analogen Uhr** -Beispiel in der [ **der drehen transformieren** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) Artikel die Zeiger der Uhr als einfache Linien angezeigt. Die **ziemlich analogen Uhr** Programm unten ersetzt die Zeilen mit `SKPath` Objekte definiert, die als Felder in der [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) -Klasse zusammen mit `SKPaint` Objekte:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    // Clock hands pointing straight up
    SKPath hourHandPath = SKPath.ParseSvgPathData(
        "M 0 -60 C   0 -30 20 -30  5 -20 L  5   0" +
                "C   5 7.5 -5 7.5 -5   0 L -5 -20" +
                "C -20 -30  0 -30  0 -60 Z");

    SKPath minuteHandPath = SKPath.ParseSvgPathData(
        "M 0 -80 C   0 -75  0 -70  2.5 -60 L  2.5   0" +
                "C   2.5 5 -2.5 5 -2.5   0 L -2.5 -60" +
                "C 0 -70  0 -75  0 -80 Z");

    SKPath secondHandPath = SKPath.ParseSvgPathData(
        "M 0 10 L 0 -80");

    // SKPaint objects
    SKPaint handStrokePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2,
        StrokeCap = SKStrokeCap.Round
    };

    SKPaint handFillPaint = new SKPaint
    {
        Style = SKPaintStyle.Fill,
        Color = SKColors.Gray
    };
    ...
}
```

Die Stunde und Minute Hände haben jetzt Bereiche, um die Hände voneinander stellen eingeschlossen, die sie mit einem schwarzen Rahmen und Grau mit gezeichnet werden die `handStrokePaint` und `handFillPaint` Objekte.

In den früheren **problematischen analogen Uhr** Beispiel, die kleinen Kreise, markiert die Stunden und Minuten wurden in einer Schleife gezeichnet. In diesem **ziemlich analogen Uhr** Beispiel ein vollkommen andere Ansatz wird verwendet: die Stunde und Minute Markierungen werden gepunktete Linien gezeichnet, die mit der `minuteMarkPaint` und `hourMarkPaint` Objekte:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    SKPaint minuteMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 3,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 3 * 3.14159f }, 0)
    };

    SKPaint hourMarkPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 6,
        StrokeCap = SKStrokeCap.Round,
        PathEffect = SKPathEffect.CreateDash(new float[] { 0, 15 * 3.14159f }, 0)
    };
    ...
}
```

Die [ **Punkte und Bindestriche** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) Handbuch erläutert, wie Sie verwenden, können die `SKPathEffect.CreateDash` Methode, um eine gestrichelte Linie zu erstellen. Das erste Argument ist ein `float` Array, das in der Regel über zwei Elemente verfügt: das erste Element ist die Länge der Bindestriche aus, und das zweite Element ist die Lücke zwischen der Bindestriche. Wenn die `StrokeCap` -Eigenschaftensatz auf `SKStrokeCap.Round`, und klicken Sie dann die abgerundeten Enden, der den Beginn der Strichelung effektiv die Länge der Bindestrich Strichbreite auf beiden Seiten des Bindestrichs verlängern. Daher das erste Arrayelement auf 0 festlegen, wird eine gepunktete Linie erstellt.

Der Abstand zwischen diesen Punkten unterliegt den zweiten Arrayelement. Wie Sie in Kürze, diese zwei sehen `SKPaint` Objekte werden verwendet, um Kreise mit einem Radius von 90 Einheiten gezeichnet werden soll. Der Umfang dieser Kreises ist daher 180π, was bedeutet, dass die Markierungen 60 Minuten jeder 3π Einheiten angezeigt werden müssen, dies ist der zweite Wert in der `float` array `minuteMarkPaint`. Die 12-Stunden-Markierungen müssen angezeigt werden alle Einheiten 15π Dies ist der Wert in der zweiten `float` Array.

Die `PrettyAnalogClockPage` Klasse wird ein Timer für ungültig zu erklärende Fläche alle 16 Millisekunden und dem `PaintSurface` Ereignishandler wird bei dieser Rate aufgerufen. Die früheren Definitionen der `SKPath` und `SKPaint` Objekte können für sehr sauber Zeichnen von Code:

```csharp
public class PrettyAnalogClockPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Transform for 100-radius circle in center
        canvas.Translate(info.Width / 2, info.Height / 2);
        canvas.Scale(Math.Min(info.Width / 200, info.Height / 200));

        // Draw circles for hour and minute marks
        SKRect rect = new SKRect(-90, -90, 90, 90);
        canvas.DrawOval(rect, minuteMarkPaint);
        canvas.DrawOval(rect, hourMarkPaint);

        // Get time
        DateTime dateTime = DateTime.Now;

        // Draw hour hand
        canvas.Save();
        canvas.RotateDegrees(30 * dateTime.Hour + dateTime.Minute / 2f);
        canvas.DrawPath(hourHandPath, handStrokePaint);
        canvas.DrawPath(hourHandPath, handFillPaint);
        canvas.Restore();

        // Draw minute hand
        canvas.Save();
        canvas.RotateDegrees(6 * dateTime.Minute + dateTime.Second / 10f);
        canvas.DrawPath(minuteHandPath, handStrokePaint);
        canvas.DrawPath(minuteHandPath, handFillPaint);
        canvas.Restore();

        // Draw second hand
        double t = dateTime.Millisecond / 1000.0;

        if (t < 0.5)
        {
            t = 0.5 * Easing.SpringIn.Ease(t / 0.5);
        }
        else
        {
            t = 0.5 * (1 + Easing.SpringOut.Ease((t - 0.5) / 0.5));
        }

        canvas.Save();
        canvas.RotateDegrees(6 * (dateTime.Second + (float)t));
        canvas.DrawPath(secondHandPath, handStrokePaint);
        canvas.Restore();
    }
}
```

Eine Besonderheit wird jedoch mit der zweiten Seite vorgenommen. Da die Uhr aktualisiert wird alle 16 Millisekunden der `Millisecond` Eigenschaft von der `DateTime` Wert kann potenziell dazu genutzt werden, um ein Sweep Zweitens hand statt in einem animieren, die in diskrete Sprünge verschoben aus zweiter zweiten. Dieser Code, nicht jedoch die datenverschiebung zu sein. Stattdessen wird der Xamarin.Forms [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) und [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) Animation Beschleunigungsfunktionen für eine andere Art der Verschiebung. Diese Beschleunigungsfunktionen dazu führen, dass der zweite Zeiger auf eine Weise wird ruckartiger verschieben &mdash; zurückzusetzen ein kleines, bevor es verschoben werden, und klicken Sie dann leicht zu stark behandeln das Ziel, einen Effekt, leider nicht in dieser statischen Screenshots reproduzieren:

[![](path-data-images/prettyanalogclock-small.png "Dreifacher Screenshot der Seite ziemlich analogen Uhr")](path-data-images/prettyanalogclock-large.png#lightbox "dreifacher Screenshot der Seite ziemlich analogen Uhr")


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
