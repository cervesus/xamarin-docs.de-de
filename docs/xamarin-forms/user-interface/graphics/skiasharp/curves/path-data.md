---
title: SVG-Pfaddaten in SkiaSharp
description: In diesem Artikel wird erläutert, wie SkiaSharp-Pfade, die Verwendung von Zeichenfolgen im Format Scalable Vector Graphics definiert, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
ms.openlocfilehash: 467863dba2f5757e0590ccf64927ae2af292f285
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770571"
---
# <a name="svg-path-data-in-skiasharp"></a>SVG-Pfaddaten in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Definieren Sie Pfade, die Verwendung von Zeichenfolgen im Format Scalable Vector Graphics_

Die [ `SKPath` ](xref:SkiaSharp.SKPath) Klasse unterstützt die Definition der gesamte Pfad-Objekte von Textzeichenfolgen in einem Format hergestellt, indem die Scalable Vector Graphics (SVG)-Spezifikation. Sie werden später in diesem Artikel finden Sie unter wie Sie einen vollständigen Pfad, wie diese in einer Textzeichenfolge darstellen können:

![](path-data-images/pathdatasample.png "Ein Beispielpfad mit SVG-Pfaddaten definiert")

SVG wird ein XML-basierte Grafiken, die Programmiersprache für Webseiten. Da SVG Pfade im Markup statt mit einer Reihe von Funktionsaufrufen definiert werden kann muss, enthält der SVG-standard eine äußerst präzise Möglichkeit zum Angeben einer gesamten Grafikpfad als Textzeichenfolge.

Innerhalb von SkiaSharp wird dieses Format als "SVG-Pfaddaten." bezeichnet. Das Format wird auch in XAML für Windows-basierten programmierumgebungen, einschließlich der Windows Presentation Foundation und der universellen Windows-Plattform, in denen es heißt unterstützt die [Pfadmarkupsyntax](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax) oder [verschieben und zeichnen-Befehle Syntax](/windows/uwp/xaml-platform/move-draw-commands-syntax/). Sie können auch als ein Exchange-Format für Vektorgrafiken, insbesondere in textbasierten Dateien wie z. B. XML dienen.

Die [ `SKPath` ](xref:SkiaSharp.SKPath) -Klasse definiert zwei Methoden mit den Wörtern `SvgPathData` im Namen:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Die statische [ `ParseSvgPathData` ](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) -Methode konvertiert eine Zeichenfolge, die eine `SKPath` Objekt während [ `ToSvgPathData` ](xref:SkiaSharp.SKPath.ToSvgPathData) konvertiert eine `SKPath` -Objekt in eine Zeichenfolge.

Hier ist eine SVG-Zeichenfolge für einen fünf Spitzen Stern, dessen Mitte sich an den Punkt (0, 0) mit einem Radius von 100:

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Die Buchstaben sind Befehle, die erstellen ein `SKPath` Objekt: `M` gibt an eine `MoveTo` aufrufen, `L` ist `LineTo`, und `Z` ist `Close` zu eine Kontur zu schließen. Jedes Paar stellt eine X- und Y-Koordinate eines Punkts. Beachten Sie, dass die `L` Befehl gefolgt von mehreren Punkten, die durch Kommas getrennt. In einer Reihe von Koordinaten und Punkten, Kommas und Leerzeichen werden identisch behandelt. Einige Programmierer bevorzugen einzufügenden Kommas zwischen die X- und Y-Koordinaten und nicht zwischen den Punkten, aber durch Kommas oder Leerzeichen sind nur erforderlich, um Mehrdeutigkeiten zu vermeiden. Dies ist durchaus zulässig:

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Die Syntax der SVG-Pfaddaten formal finden Sie unter [Abschnitt 8.3 der SVG-Spezifikation](http://www.w3.org/TR/SVG11/paths.html#PathData). Es folgt eine Zusammenfassung:

## <a name="moveto"></a>**MoveTo**

```
M x y
```

Eine neue Kontur im Pfad wird durch Festlegen der aktuellen Position gestartet. Pfaddaten sollten beginnen immer mit einem `M` Befehl.

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

Dieser Befehl fügt eine gerade Linie (oder Zeilen) in den Pfad und legt die neue Position am Ende der letzten Zeile. Führen Sie die `L` Befehl mehrere Paare von *x* und *y* Koordinaten.

## <a name="horizontal-lineto"></a>**Horizontalen LineTo**

```
H x ...
```

Dieser Befehl fügt eine horizontale Linie auf den Pfad und legt die neue Position am Ende der Zeile. Führen Sie die `H` Befehl mit mehreren *x* Koordinaten, aber es nicht viel Sinn.

## <a name="vertical-line"></a>**Vertikale Linie**

```
V y ...
```

Dieser Befehl fügt eine vertikale Linie auf den Pfad und legt die neue Position am Ende der Zeile.

## <a name="close"></a>**Schließen**

```
Z
```

Die `C` Befehl schließt die Kontur durch Hinzufügen einer geraden Linie von der aktuellen Position, an den Anfang der Kontur.

## <a name="arcto"></a>**ArcTo**

Der Befehl zum Hinzufügen eines elliptischen Bogens an die Kontur ist bei weitem den komplexesten Befehl in der gesamten SVG-Pfaddaten Spezifikation. Es ist der einzige Befehl, in dem Zahlen etwas anderes als Koordinatenwerte darstellen können:

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

Die *Rx* und *Rage* Parameter sind die horizontalen und senkrechten RADIUS der Ellipse. Die *Drehwinkel* wird im Uhrzeigersinn in Grad.

Legen Sie die *große-Arc-Flag* für den großen Bogen auf 1 oder 0 für den kleinen Bogen.

Legen Sie die *-Sweep-Flag* für auf 1 im Uhrzeigersinn und 0 für gegen den Uhrzeigersinn.

Der Bogen gezeichnet wird, zu dem Punkt (*x*, *y*), wird die neue Position.

## <a name="cubicto"></a>**CubicTo**

```
C x1 y1 x2 y2 x3 y3 ...
```

Dieser Befehl fügt eine kubische Bézierkurve aus der aktuellen Position bis (*X3*, *y3*), wird die neue Position. Die Punkte (*X1*, *y1*) und (*X2*, *y2*) Control-Punkte sind.

Mehrere Bézierkurven können angegeben werden, durch eine einzelne `C` Befehl. Die Anzahl der Punkte muss es sich um ein Vielfaches von 3 sein.

Es gibt auch ein Befehl für "smooth" Bézier:

```
S x2 y2 x3 y3 ...
```

Mit diesem Befehl sollte einen normalen Bézier Befehl folgen, (obwohl dies nicht unbedingt erforderlich ist). Befehls Bézier smooth berechnet den ersten Kontrollpunkt, damit es als Reaktion auf den zweiten Kontrollpunkt, der die vorherige Bézier um deren gegenseitige Punkt ist. Diese drei Punkte sind daher kollineare, und die Verbindung zwischen den zwei Bézierkurven ist.

## <a name="quadto"></a>**QuadTo**

```
Q x1 y1 x2 y2 ...
```

Für quadratische Bézierkurven muss die Anzahl der Punkte ein Vielfaches von 2 sein. Der Kontrollpunkt ist (*X1*, *y1*) und den Endpunkt (und die neue Position) ist (*X2*, *y2*)

Es gibt auch ein reibungslosen quadratischen Kurve-Befehl:

```
T x2 y2 ...
```

Der Kontrollpunkt, werden basierend auf den Kontrollpunkt der vorherigen quadratischen Kurve berechnet.

Alle diese Befehle sind auch in "relativer" Versionen verfügbar sind, in denen Koordinatenpunkte relativ zur aktuellen Position. Diese relativen Befehle beginnen mit Kleinbuchstaben, z. B. `c` statt `C` für die relative-Version des Befehls Bézier kubische.

Dies ist das Ausmaß der SVG-Pfaddaten Definition. Es gibt keine Möglichkeit, die für die wiederholte Gruppen von Befehlen oder jeder Typ, der Berechnung ausführen. Befehle für `ConicTo` oder anderen Typen von Arc-Spezifikationen sind nicht verfügbar.

Die statische [ `SKPath.ParseSvgPathData` ](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) Methode erwartet eine gültige Zeichenfolge der SVG-Befehle. Wenn Syntaxfehler erkannt wird, gibt die Methode `null`. Dies ist die einzige Fehleranzeige.

Die [ `ToSvgPathData` ](xref:SkiaSharp.SKPath.ToSvgPathData) Methode ist nützlich zum Abrufen von SVG-Pfaddaten aus einer vorhandenen `SKPath` Objekt in ein anderes Programm zu übertragen oder in einem Text-Datei-Format wie z. B. XML gespeichert. (Die `ToSvgPathData` Methode wird nicht im Beispielcode in diesem Artikel gezeigt.) Führen Sie *nicht* erwarten `ToSvgPathData` zum Zurückgeben einer Zeichenfolge entspricht genau der Methodenaufrufe, die den Pfad erstellt. Insbesondere erfahren Sie, dass Bögen, um mehrere konvertiert werden `QuadTo` Befehle aus, und das ist die Anzeige in den Path-Daten, die von zurückgegeben `ToSvgPathData`.

Die **Pfad Daten-Hello** Seite Spells sich das Wort "HELLO" Verwenden von SVG-Pfaddaten. Sowohl die `SKPath` und `SKPaint` werden Objekte als Felder in definiert die [ `PathDataHelloPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) Klasse:

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

Der Pfad, definieren die Zeichenfolge beginnt in der oberen linken Ecke an dem Punkt (0, 0). Jeder Buchstabe beträgt 50 Einheiten, die Breite und Höhe von 100 Einheiten und durch eine andere 25 Einheiten Buchstaben getrennt sind, was bedeutet, dass der gesamte Pfad 350 Einheiten breit ist.

Die "H", "Hallo" besteht drei einzeilige Konturen, während 'E' zwei verbundene kubische Bézierkurven ist. Beachten Sie, dass die `C` Befehl 6 Punkt folgt, und zwei der Control-Punkte der Y-Koordinaten des – 10 und 110, die sie außerhalb des Bereichs, der die Y-Koordinaten der anderen Buchstaben setzt verfügen. "L" ist, zwei verbundene Linien, während die ' O' wird eine Ellipse, die mit gerendert wird ein `A` Befehl.

Beachten Sie, dass die `M` -Befehl, der die letzte Kontur beginnt legt die Position fest, zu dem Punkt (350, 50), handelt es sich vertikalen Mitte des linken Seite von der "O'. Durch das erste Zahlen folgt die `A` Befehl, das die Ellipse hat einen horizontalen Radius des 25 und einem vertikalen Radius von 50. Der Endpunkt wird angegeben, durch die letzten paar der Zahlen in der `A` -Befehl, der den Punkt (300, 49,9) darstellt. Das ist absichtlich nur geringfügig von den Ausgangspunkt. Wenn der Endpunkt den Startpunkt auf gleich festgelegt ist, wird der Bogen nicht gerendert werden. Um eine vollständige Ellipse zu zeichnen, Sie müssen festlegen, den Endpunkt nahe an (aber nicht gleich) den Anfangspunkt, oder Sie müssen mindestens zwei verwenden `A` Befehle, die jeweils für einen Teil der vollständigen Ellipse.

Möglicherweise möchten fügen die folgende Anweisung in den Konstruktor der Seite, und legen einen Haltepunkt aus, um die resultierende Zeichenfolge zu überprüfen:

```csharp
string str = helloPath.ToSvgPathData();
```

Erfahren Sie, dass der Bogen durch eine lange Reihe von ersetzt wurde `Q` -Befehle für eine schrittweise Annäherung an den Bogen mit quadratischen Bézierkurven.

Die `PaintSurface` Handler Ruft die engen Begrenzungen des Pfads, der nicht die Kontrollpunkte für 'E' enthalten ist und ' O' Kurven. Die drei transformierungen die Mitte des Pfads zu verschieben, zu dem Punkt (0, 0), skalieren den Pfad zu die Größe der Leinwand (jedoch berücksichtigt auch die Strichbreite) und verschieben Sie die Mitte des Pfads in der Mitte der Canvas:

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

Der Pfad füllt den Zeichenbereich, der bei der Anzeige im Querformat geeigneteren aussieht:

[![](path-data-images/pathdatahello-small.png "Dreifacher Screenshot der Pfad Daten-Hello-Seite")](path-data-images/pathdatahello-large.png#lightbox "dreifachen Screenshot der Pfad Daten-Hello-Seite")

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

Den Anfang eine Katze ist ein Kreis, und es wird hier gerendert, mit zwei `A` Befehle aus, von denen jeder ein Halbkreis zeichnet. Beide `A` Befehle für das Anfangselement horizontalen und senkrechten RADIUS von 100 zu definieren. Erste Bogen beginnt (240, 100) und endet mit (240, 300), wird den Startpunkt für den zweiten Bogen, der zur endet (240, 100).

Die beiden Augen werden ebenfalls gerendert, mit zwei `A` Befehle aus, und wie Sie mit der Katze Head, die zweite `A` -Befehl beendet am selben Punkt als Ausgangspunkt des ersten `A` Befehl. Aber diese Paare von `A` Befehle definieren Sie eine Ellipse. Die mit der einzelnen Bogen ist 40 Einheiten und des Radius 40 Einheiten, was bedeutet, dass diese Bögen nicht vollständige Semicircles sind.

Die `PaintSurface` Handler führt Sie ähnlich wie Transformationen, wie das vorherige Beispiel, jedoch legt eine einzelne `Scale` Faktor, der das Seitenverhältnis beibehalten, und geben Sie einen wenig Rand aus, damit der Katze Whisker Tippen Sie auf die Seiten des Bildschirms nicht:

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

Hier wird das Programm ausgeführt wird:

[![](path-data-images/pathdatacat-small.png "Dreifacher Screenshot der Seite Pfad Daten Cat")](path-data-images/pathdatacat-large.png#lightbox "dreifachen Screenshot der Seite Pfad Daten Cat")

Normalerweise, wenn ein `SKPath` Objekt als ein Feld definiert ist, müssen der Pfadkonturen im Konstruktor oder einer anderen Methode definiert werden. Wenn SVG-Pfaddaten verwenden zu können, allerdings haben Sie gelernt, dass der Pfad vollständig in der Felddefinition angegeben werden kann.

Die vorherige **hässlichen analogen Uhr** -Beispiel in der [ **der Rotate-Transformation** ](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) Artikel der Zeiger der Uhr als einfache Linien angezeigt. Die **ziemlich analogen Uhr** nachfolgende Programm ersetzt die Zeilen mit `SKPath` Objekte definiert, die als Felder in der [ `PrettyAnalogClockPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) -Klasse zusammen mit `SKPaint` Objekte:

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

Die Stunde und Minute Hände haben jetzt Bereiche eingeschlossen. Um diese praktische voneinander zu machen, werden sie mit einem schwarzen Rand und mit grauer Füllung gezeichnet, die `handStrokePaint` und `handFillPaint` Objekte.

In den früheren **hässlichen analogen Uhr** Beispiel, das kleine Kreise, markiert die Stunden und Minuten wurden in einer Schleife gezeichnet. In diesem **ziemlich analogen Uhr** Beispiel ein ganz anderen Ansatz wird verwendet: die Markierungen Stunden und Minuten werden gepunkteten Linien gezeichnet, die mit der `minuteMarkPaint` und `hourMarkPaint` Objekte:

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

Die [ **Punkte und Gedankenstriche** ](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) Artikel wurde erläutert, wie Sie verwenden können die [ `SKPathEffect.CreateDash` ](xref:SkiaSharp.SKPathEffect.CreateDash*) Methode, um eine gestrichelte Linie zu erstellen. Das erste Argument ist ein `float` -Array, das in der Regel über zwei-Elemente verfügt: Das erste Element ist die Länge der Bindestriche, und das zweite Element ist die Lücke zwischen den Bindestrichen. Wenn die `StrokeCap` -Eigenschaftensatz auf `SKStrokeCap.Round`, und klicken Sie dann die abgerundeten Enden des Strichs effektiv die Dash Länge durch die Strichbreite auf beiden Seiten des Strichs verlängern. Wenn das erste Arrayelement auf 0 erstellt daher eine gestrichelte Linie.

Die Entfernung zwischen diesen Punkten wird durch das zweite Arrayelement bestimmt. Wie Sie diese in Kürze, zwei sehen werden `SKPaint` Objekte dienen zum Zeichnen der Kreise mit einem Radius von 90 Einheiten. Der Umfang dieser Kreises ist daher 180π, was bedeutet, dass die 60-Minuten-Markierungen jeder 3π Einheiten angezeigt werden müssen, dies ist der zweite Wert in der `float` im array `minuteMarkPaint`. Die 12-Stunden-Markierungen müssen angezeigt werden alle Einheiten 15π, dies ist der Wert in der zweiten `float` Array.

Die `PrettyAnalogClockPage` Klasse wird ein Timer ungültig gemacht werden, die Oberfläche alle 16 Millisekunden ein, und die `PaintSurface` Handler wird aufgerufen, bei dieser Rate. Die früheren Definitionen der `SKPath` und `SKPaint` Objekte können für sehr sauberen Code Zeichnen:

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

Eine Besonderheit des zweiten Zeigers, jedoch erfolgt mit. Da die Uhr aktualisiert wird alle 16 Millisekunden, die `Millisecond` Eigenschaft der `DateTime` Wert kann möglicherweise verwendet werden, um einen Sweep zweite Seite anstelle eines animieren, die in einzelne Sprünge verschiebt von Sekunde zu Sekunde. Dieser Code, nicht jedoch die Bewegung auf smooth sein. Stattdessen wird der Xamarin.Forms [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) und [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) Animation Beschleunigungsfunktionen für eine andere Art von Verschiebung. Diese Beschleunigungsfunktionen dazu führen, dass der zweite Zeiger zum Verschieben in eine wird ruckartiger Weise &mdash; zurückzusetzen ein kleines, bevor es verschoben, und klicken Sie dann leicht zu schießen das Ziel, einen Effekt, leider nicht in diese statischen Screenshots reproduzieren:

[![](path-data-images/prettyanalogclock-small.png "Dreifacher Screenshot der Seite ziemlich analogen Uhr")](path-data-images/prettyanalogclock-large.png#lightbox "dreifachen Screenshot der Seite ziemlich analogen Uhr")

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
