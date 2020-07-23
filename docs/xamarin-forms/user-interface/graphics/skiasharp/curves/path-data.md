---
title: SVG-Pfaddaten in skiasharp
description: In diesem Artikel wird erläutert, wie Sie skiasharp-Pfade mithilfe von Text Zeichenfolgen im Format der skalierbaren Vektorgrafik definieren und dies mit Beispielcode veranschaulichen.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 1D53067B-3502-4D74-B89D-7EC496901AE2
author: davidbritch
ms.author: dabritch
ms.date: 05/24/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2571375e7ad28acbf367870b5c48e19d3a7525e7
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86931247"
---
# <a name="svg-path-data-in-skiasharp"></a>SVG-Pfaddaten in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Definieren von Pfaden mit Text Zeichenfolgen im Format der skalierbaren Vektorgrafiken_

Die- [`SKPath`](xref:SkiaSharp.SKPath) Klasse unterstützt die Definition ganzer Pfad Objekte aus Text Zeichenfolgen in einem Format, das von der SVG-Spezifikation (Scalable Vector Graphics) festgelegt wird. Weiter unten in diesem Artikel erfahren Sie, wie Sie einen vollständigen Pfad, z. b. diesen, in einer Text Zeichenfolge darstellen können:

![Ein mit SVG-Pfaddaten definierter Beispiel Pfad](path-data-images/pathdatasample.png)

SVG ist eine XML-basierte Grafik Programmiersprache für Webseiten. Da SVG zulassen muss, dass Pfade im Markup und nicht in einer Reihe von Funktionsaufrufen definiert werden, bietet der SVG-Standard eine sehr präzise Möglichkeit, einen gesamten Grafik Pfad als Text Zeichenfolge anzugeben.

In skiasharp wird dieses Format als "SVG Path-Data" bezeichnet. Das Format wird auch in Windows XAML-basierten Programmierumgebungen unterstützt, einschließlich der Windows Presentation Foundation und der universelle Windows-Plattform, wo es als [Pfad Markup Syntax](/dotnet/framework/wpf/graphics-multimedia/path-markup-syntax) oder die Syntax für Verschiebe [-und Zeichnungs Befehle](/windows/uwp/xaml-platform/move-draw-commands-syntax/)bezeichnet wird. Sie kann auch als Austauschformat für Vektorgrafik Bilder dienen, insbesondere in textbasierten Dateien wie XML.

Die- [`SKPath`](xref:SkiaSharp.SKPath) Klasse definiert zwei Methoden mit den Wörtern `SvgPathData` in ihren Namen:

```csharp
public static SKPath ParseSvgPathData(string svgPath)

public string ToSvgPathData()
```

Die statische- [`ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) Methode konvertiert eine Zeichenfolge in ein- `SKPath` Objekt, während [`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData) ein- `SKPath` Objekt in eine Zeichenfolge konvertiert.

Hier ist eine SVG-Zeichenfolge für einen fünf-Spitzen-Stern, der am Punkt (0,0) mit einem Radius von 100 zentriert ist:

```
"M 0 -100 L 58.8 90.9, -95.1 -30.9, 95.1 -30.9, -58.8 80.9 Z"
```

Bei den Buchstaben handelt es sich um Befehle, die ein `SKPath` Objekt erstellen: `M` gibt einen `MoveTo` -Befehl `L` an, ist und soll `LineTo` `Z` `Close` eine Kontur schließen. Jedes Zahlenpaar stellt eine X-und Y-Koordinate eines Punkts bereit. Beachten Sie, dass `L` auf den Befehl mehrere Punkte folgen, die durch Kommas getrennt sind. In einer Reihe von Koordinaten und Punkten werden Kommas und Leerzeichen identisch behandelt. Einige Programmierer bevorzugen Kommas zwischen den X-und Y-Koordinaten anstatt zwischen den Punkten, aber Kommas oder Leerzeichen sind nur erforderlich, um Mehrdeutigkeit zu vermeiden. Dies ist vollkommen rechtmäßig:

```
"M0-100L58.8 90.9-95.1-30.9 95.1-30.9-58.8 80.9Z"
```

Die Syntax der SVG-Pfaddaten ist in [Abschnitt 8,3 der SVG-Spezifikation](https://www.w3.org/TR/SVG11/paths.html#PathData)formal dokumentiert. Im folgenden finden Sie eine Zusammenfassung:

## <a name="moveto"></a>**MoveTo**

```
M x y
```

Dadurch wird eine neue Kontur im Pfad durch Festlegen der aktuellen Position begonnen. Pfaddaten sollten immer mit einem `M` Befehl beginnen.

## <a name="lineto"></a>**LineTo**

```
L x y ...
```

Dieser Befehl fügt dem Pfad eine gerade Linie (oder Zeilen) hinzu und legt die neue aktuelle Position auf das Ende der letzten Zeile fest. Sie können dem `L` Befehl mehrere Paare von *x* -und *y* -Koordinaten folgen.

## <a name="horizontal-lineto"></a>**Horizontal LineTo**

```
H x ...
```

Mit diesem Befehl wird dem Pfad eine horizontale Linie hinzugefügt, und die neue aktuelle Position wird auf das Ende der Zeile festgelegt. Der `H` Befehl kann mit mehreren *x* -Koordinaten befolgt werden, ist jedoch nicht sinnvoll.

## <a name="vertical-line"></a>**Vertikale Linie**

```
V y ...
```

Mit diesem Befehl wird dem Pfad eine vertikale Linie hinzugefügt, und die neue aktuelle Position wird auf das Ende der Zeile festgelegt.

## <a name="close"></a>**Close**

```
Z
```

Der- `C` Befehl schließt die Kontur, indem eine gerade Linie von der aktuellen Position zum Anfang der Kontur hinzugefügt wird.

## <a name="arcto"></a>**ArcTo**

Der Befehl zum Hinzufügen eines elliptischen Bogens zur Kontur ist der weitaus komplexere Befehl in der gesamten SVG-Pfad Daten Spezifikation. Dabei handelt es sich um den einzigen Befehl, bei dem Zahlen etwas anderes als Koordinaten Werte darstellen können:

```
A rx ry rotation-angle large-arc-flag sweep-flag x y ...
```

Die Parameter *RX* und *Ry* sind die horizontale und vertikale Radien der Ellipse. Der *Drehungs Winkel* liegt im Uhrzeigersinn in Grad.

Legen Sie für den großen Bogen das Arkus *Flag* auf 1 und für den kleinen Bogen den Wert auf 0 (null) fest.

Legen Sie das *Sweep-Flag* für den Uhrzeigersinn auf 1 und für gegen den Uhrzeigersinn auf 0 fest.

Der Bogen wird bis zum Punkt (*x*, *y*) gezeichnet, der zur neuen aktuellen Position wird.

## <a name="cubicto"></a>**Kubicto**

```
C x1 y1 x2 y2 x3 y3 ...
```

Dieser Befehl fügt eine kubische Bézier-Kurve von der aktuellen Position zu (*X3*, *Y3*) hinzu, die zur neuen aktuellen Position wird. Die Punkte (*x1*, *Y1*) und (*x2*, *Y2*) sind Steuerungs Punkte.

Mehrere Bézier-Kurven können mit einem einzelnen Befehl angegeben werden `C` . Die Anzahl der Punkte muss ein Vielfaches von 3 sein.

Es gibt auch einen "Smooth"-Befehl der Bézier-Kurve:

```
S x2 y2 x3 y3 ...
```

Dieser Befehl sollte einem regulären Befehl von "Bézier" folgen (obwohl dies nicht zwingend erforderlich ist). Der Smooth Bézier-Befehl berechnet den ersten Kontrollpunkt so, dass es sich um eine Reflektion des zweiten Kontroll Punkts der vorherigen Bézier um den gegenseitigen Punkt handelt. Diese drei Punkte sind daher colinear, und die Verbindung zwischen den beiden Bézier-Kurven ist reibungslos.

## <a name="quadto"></a>**QuadTo**

```
Q x1 y1 x2 y2 ...
```

Bei quadratischen Bézier-Kurven muss die Anzahl der Punkte ein Vielfaches von 2 sein. Der Kontrollpunkt ist (*x1*, *Y1*), und der Endpunkt (und die neue aktuelle Position) ist (*x2*, *Y2*).

Außerdem gibt es einen Smooth-Befehl mit quadratischer Kurve:

```
T x2 y2 ...
```

Der Kontrollpunkt wird basierend auf dem Kontrollpunkt der vorherigen quadratischen Kurve berechnet.

Alle diese Befehle sind auch in relativen Versionen verfügbar, in denen die Koordinaten Punkte relativ zur aktuellen Position sind. Diese relativen Befehle beginnen mit Kleinbuchstaben, z `c` `C` . b. anstelle von für die relative Version des kubischen Bézier-Befehls.

Dies ist der Umfang der SVG-Pfad Datendefinition. Es gibt keine Möglichkeit zum Wiederholen von Gruppen von Befehlen oder zum Ausführen beliebiger Berechnungs Typen. Befehle für `ConicTo` oder die anderen Typen von Bogen Spezifikationen sind nicht verfügbar.

Die statische- [`SKPath.ParseSvgPathData`](xref:SkiaSharp.SKPath.ParseSvgPathData(System.String)) Methode erwartet eine gültige Zeichenfolge von SVG-Befehlen. Wenn ein Syntax Fehler erkannt wird, gibt die Methode zurück `null` . Dies ist die einzige Fehlermeldung.

Die- [`ToSvgPathData`](xref:SkiaSharp.SKPath.ToSvgPathData) Methode ist praktisch zum Abrufen von SVG-Pfaddaten von einem vorhandenen `SKPath` Objekt zur Übertragung an ein anderes Programm oder zum Speichern in einem textbasierten Dateiformat (z. b. Xml). (Die- `ToSvgPathData` Methode wird im Beispielcode in diesem Artikel nicht gezeigt.) Erwarten *not* Sie nicht `ToSvgPathData` , dass eine Zeichenfolge zurückgegeben wird, die exakt den Methoden aufrufen entspricht, die den Pfad erstellt haben. Insbesondere werden Sie feststellen, dass Bögen in mehrere Befehle konvertiert werden `QuadTo` und wie Sie in den von zurückgegebenen Pfaddaten angezeigt werden `ToSvgPathData` .

Auf der Seite "Hello" der **Pfad Daten** wird das Wort "Hello" mithilfe der SVG-Pfaddaten angezeigt. Sowohl das `SKPath` - `SKPaint` Objekt als auch das-Objekt werden als Felder in der- [`PathDataHelloPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataHelloPage.cs) Klasse definiert:

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

Der Pfad, der die Text Zeichenfolge definiert, beginnt in der oberen linken Ecke am Punkt (0,0). Jeder Buchstabe ist 50 Einheiten breit und 100 Einheiten hoch, und Buchstaben sind durch andere 25 Einheiten getrennt, was bedeutet, dass der gesamte Pfad 350 Einheiten breit ist.

Das "H" von "Hello" besteht aus 3 1-zeiligen Kontur, während "E" zwei verbundene kubische Bézier-Kurven ist. Beachten Sie, dass auf den `C` Befehl sechs Punkte folgen und zwei der Kontrollpunkte über Y-Koordinaten von – 10 und 110 verfügen, die diese außerhalb des Bereichs der y-Koordinaten der anderen Buchstaben liegen. "L" ist zwei verbundene Zeilen, während "O" eine Ellipse ist, die mit einem-Befehl gerendert wird `A` .

Beachten Sie, dass der `M` Befehl, der die letzte Kontur startet, die Position auf den Punkt (350, 50) festlegt, bei dem es sich um die vertikale Mitte der linken Seite von ' O ' handelt. Wie durch die ersten Zahlen nach dem `A` Befehl angegeben, hat die Ellipse einen horizontalen Radius von 25 und einen vertikalen Radius von 50. Der Endpunkt wird durch das letzte Zahlenpaar im Befehl angegeben, das `A` den Punkt (300, 49,9) darstellt. Das ist absichtlich etwas anders als der Startpunkt. Wenn der Endpunkt gleich dem Startpunkt festgelegt ist, wird der Bogen nicht gerendert. Um eine komplette Ellipse zu zeichnen, müssen Sie den Endpunkt in der Nähe des Startpunkts (aber nicht gleich) festlegen, oder Sie müssen zwei oder mehr `A` Befehle verwenden, jeweils für einen Teil der gesamten Ellipse.

Möglicherweise möchten Sie dem Konstruktor der Seite die folgende-Anweisung hinzufügen und dann einen Haltepunkt festlegen, um die resultierende Zeichenfolge zu überprüfen:

```csharp
string str = helloPath.ToSvgPathData();
```

Sie werden feststellen, dass der Bogen durch eine lange Reihe von `Q` Befehlen für eine schrittweise Näherung des Bogens mithilfe quadratischer Bézier-Kurven ersetzt wurde.

Der `PaintSurface` Handler Ruft die engen Begrenzungen des Pfads ab, der die Steuerungs Punkte für die Kurven "E" und "O" nicht enthält. Die drei Transformationen verschieben den Mittelpunkt des Pfads an den Punkt (0,0), Skalieren den Pfad auf die Größe des Zeichen Bereichs (wobei auch die Strichbreite berücksichtigt wird) und verschieben dann den Mittelpunkt des Pfades in den Mittelpunkt der Canvas:

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

Der Pfad füllt den Zeichenbereich, der beim Anzeigen im Querformat sinnvoller aussieht:

[![Dreifacher Screenshot der Pfad Daten-Hello-Seite](path-data-images/pathdatahello-small.png)](path-data-images/pathdatahello-large.png#lightbox "Dreifacher Screenshot der Pfad Daten-Hello-Seite")

Die Seite **path Data Cat** ist ähnlich. Die Pfad-und Zeichnungsobjekte sind beide als Felder in der- [`PathDataCatPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PathDataCatPage.cs) Klasse definiert:

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

Der Anfang eines Cat ist ein Kreis, und hier wird er mit zwei Befehlen gerendert `A` , von denen jeder einen Halbkreis zeichnet. Beide `A` Befehle für die Kopfzeile definieren horizontales und vertikales Radien von 100. Der erste Bogen beginnt bei (240, 100) und endet bei (240, 300), der zum Startpunkt für den zweiten Bogen wird, der auf zurück endet (240, 100).

Die beiden Augen werden ebenfalls mit zwei `A` Befehlen gerendert, und wie bei der Kopfzeile des CAT `A` endet der zweite Befehl an demselben Punkt wie der erste `A` Befehl. Diese Befehls Paare definieren jedoch `A` keine Ellipse. Der mit jedem Bogen ist 40 Einheiten, und der RADIUS ist auch 40 Einheiten, was bedeutet, dass diese Bögen keine vollen semikreise sind.

Der `PaintSurface` Handler führt ähnliche Transformationen wie das vorherige Beispiel aus, legt jedoch einen einzelnen Faktor fest, `Scale` um das Seitenverhältnis beizubehalten, und bietet einen kleinen Rand, sodass die Whisker der Cat nicht die Seiten des Bildschirms berühren:

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

Dies ist das Programm, das ausgeführt wird:

[![Dreifacher Screenshot der Seite "Path Data Cat"](path-data-images/pathdatacat-small.png)](path-data-images/pathdatacat-large.png#lightbox "Dreifacher Screenshot der Seite "Path Data Cat"")

Wenn ein `SKPath` Objekt als Feld definiert ist, müssen die Konturen des Pfads normalerweise im Konstruktor oder einer anderen Methode definiert werden. Wenn Sie SVG-Pfaddaten verwenden, haben Sie jedoch gesehen, dass der Pfad vollständig in der Felddefinition angegeben werden kann.

Das frühere Beispiel für eine **hässliche analoge Uhr** im Artikel [**Drehen der Transformation**](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md) hat die Hände der Uhr als einfache Linien angezeigt. Das **Recht analoge Takt** Programm ersetzt diese Zeilen mit Objekten, die `SKPath` als Felder in der Klasse definiert sind, [`PrettyAnalogClockPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/PrettyAnalogClockPage.cs) zusammen mit `SKPaint` Objekten:

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

Die Stunden-und Minuten Zeitangabe haben nun eingeschlossene Bereiche. Damit sich die Hände voneinander unterscheiden, werden Sie sowohl mit einem schwarzen Umriss als auch mit grauer Füllung mithilfe der `handStrokePaint` -und- `handFillPaint` Objekte gezeichnet.

Im vorherigen Beispiel für eine **hässliche analoge Uhr** wurden die kleinen Kreise, die die Stunden und Minuten markierten, in einer Schleife gezeichnet. In diesem Beispiel für eine **Recht analoge Uhr** wird ein völlig anderer Ansatz verwendet: die Zeichen für die Stunde und die Minute sind gepunktete Linien, die mit den `minuteMarkPaint` Objekten und gezeichnet werden `hourMarkPaint` :

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

Im Artikel [**Punkte und Bindestriche**](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md) wurde erläutert, wie Sie die- [`SKPathEffect.CreateDash`](xref:SkiaSharp.SKPathEffect.CreateDash*) Methode zum Erstellen einer gestrichelten Linie verwenden können. Das erste Argument ist ein- `float` Array, das im Allgemeinen über zwei-Elemente verfügt: das erste Element ist die Länge der Bindestriche, und das zweite Element ist die Lücke zwischen den Bindestrichen. Wenn die- `StrokeCap` Eigenschaft auf festgelegt ist `SKStrokeCap.Round` , werden die gerundeten Enden des Bindestrichs die Strichlänge effektiv um die Strichbreite auf beiden Seiten des Bindestrichs verlängert. Wenn Sie also das erste Array Element auf 0 festlegen, wird eine gepunktete Linie erstellt.

Der Abstand zwischen diesen Punkten wird durch das zweite Array Element gesteuert. Wie Sie sehen werden, werden diese beiden `SKPaint` Objekte zum Zeichnen von Kreisen mit einem Radius von 90 Einheiten verwendet. Der Umfang dieses Kreises ist aus diesem Grund 180, d. h., die 60 Minuten müssen alle 3D-Einheiten enthalten, bei denen es sich um den zweiten Wert im `float` Array in handelt `minuteMarkPaint` . Die 12-Stunden-Markierungen müssen alle 15d-Einheiten (der Wert im zweiten Array) enthalten `float` .

Mit der- `PrettyAnalogClockPage` Klasse wird ein Timer festgelegt, um die Oberfläche alle 16 Millisekunden für ungültig zu erklären, und der `PaintSurface` Handler wird an dieser Rate aufgerufen. Die früheren Definitionen der `SKPath` Objekte und `SKPaint` ermöglichen sehr sauberen Zeichnungs Code:

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

Mit der zweiten Hand wird jedoch etwas besonderes erreicht. Da die Uhr alle 16 Millisekunden aktualisiert wird, kann die- `Millisecond` Eigenschaft des- `DateTime` Werts möglicherweise verwendet werden, um eine geleerte zweite Hand zu animieren, anstatt eine zu animieren, die in diskrete Sprünge von Sekunde zu Sekunde verschoben wird. Dieser Code lässt jedoch nicht zu, dass die Bewegung reibungslos ist. Stattdessen werden die Xamarin.Forms [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) Animations Funktionen und für eine andere Art von Bewegung verwendet. Diese Beschleunigungsfunktionen bewirken, dass die zweite Hand in eine jerkier-Weise verschoben wird, &mdash; bevor Sie verschoben wird, und dann etwas über das Ziel hinausgeht, was leider nicht in den statischen Screenshots reproduziert werden kann:

[![Dreifacher Screenshot der Seite mit der Recht analogen Uhr](path-data-images/prettyanalogclock-small.png)](path-data-images/prettyanalogclock-large.png#lightbox "Dreifacher Screenshot der Seite mit der Recht analogen Uhr")

## <a name="related-links"></a>Ähnliche Themen

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
