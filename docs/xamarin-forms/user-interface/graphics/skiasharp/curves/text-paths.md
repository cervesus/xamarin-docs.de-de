---
title: Pfade und Text in SkiaSharp
description: In diesem Artikel untersucht die Schnittmenge von SkiaSharp-Pfade und Text, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2017
ms.openlocfilehash: 366a6e9585817c5a47ba5bec14fb2f238ab23a6b
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61022039"
---
# <a name="paths-and-text-in-skiasharp"></a>Pfade und Text in SkiaSharp

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Untersuchen Sie die Schnittmenge der Pfade und text_

In moderner Grafikhardware-Systemen handelt Schriftarten es sich um Sammlungen von Zeichenumrisse, in der Regel von quadratischen Bézierkurven definiert. Daher gilt: eine Funktion zum Konvertieren von Text-Zeichen in einen Grafikpfad vieler moderner Grafikhardware Systeme enthalten.

Sie haben bereits gesehen, können Sie die Umrisse von Textzeichen zu zeichnen und als füllen sie. Dadurch können Sie diese Zeichenumrisse mit einer bestimmten Strichbreite und sogar einen Pfad Effekt anzuzeigen, wie beschrieben in der [ **Pfadeffekte** ](effects.md) Artikel. Es ist jedoch auch möglich, konvertieren eine Zeichenfolge in eine `SKPath` Objekt. Dies bedeutet, dass die Textumrisse für Clipping mit Techniken verwendet werden können, die in beschrieben wurden die [ **Schneiden mit Pfaden und Regionen** ](clipping.md) Artikel.

Neben verwenden einen Effekt Pfad, um eine Zeichenumriss zu zeichnen, Sie können auch erstellen pfadeffekte, die auf einem Pfad basieren, die von einer Zeichenfolge abgeleitet ist, und Sie können sogar Kombinieren der zwei Auswirkungen:

![](text-paths-images/pathsandtextsample.png "Pfad der Texteffekt")

Im vorherigen Artikel auf [ **Pfadeffekte**](effects.md), Sie haben gesehen, wie die [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) -Methode der `SKPaint` erhalten einen Überblick über eine gestrichelte Pfad. Sie können diese Methode auch verwenden, mit Pfaden Zeichenumrisse abgeleitet.

Schließlich wird in diesem Artikel veranschaulicht, eine andere Schnittmenge der Pfade und Text: Die [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) -Methode der `SKCanvas` können Sie eine Textzeichenfolge angezeigt wird, sodass die Baseline des Texts auf einen gekrümmten Pfad folgt.

## <a name="text-to-path-conversion"></a>Text, der Pfad-Konvertierung

Die [ `GetTextPath` ](xref:SkiaSharp.SKPaint.GetTextPath(System.String,System.Single,System.Single)) -Methode der `SKPaint` konvertiert eine Zeichenfolge in eine `SKPath` Objekt:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

Die `x` und `y` Argumente angeben, den Ausgangspunkt der Baseline der linken Seite des Texts. Menschen spielen die gleiche Rolle wie in der `DrawText` -Methode der `SKCanvas`. In dem Pfad müssen die Baseline der linken Seite des Texts, die Koordinaten (X, y).

Die `GetTextPath` Methode ist zu viel des guten aus, wenn Sie lediglich füllen oder den resultierenden Pfad zeichnen möchten. Die normale `DrawText` Methode können Sie dies tun. Die `GetTextPath` Methode eignet sich besser für andere Aufgaben im Zusammenhang mit Pfaden.

Eine der folgenden Aufgaben wird abgeschnitten. Die **Clipping Text** -Seite erstellt einen Freistellungspfad basierend auf den Zeichenumrisse des Worts "CODE". Dieser Pfad wird gestreckt, um die Größe der Seite, um eine Bitmap zugeschnitten wird, die ein Abbild von enthält die **Text kürzen** Quellcode:

[![](text-paths-images/clippingtext-small.png "Dreifacher Screenshot der Seite Text kürzen")](text-paths-images/clippingtext-large.png#lightbox "dreifachen Screenshot der Seite Text kürzen")

Die [ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Klassenkonstruktor lädt der Bitmap, die als eingebettete Ressource in gespeichert ist die **Media** Ordner der Projektmappe:

```csharp
public class ClippingTextPage : ContentPage
{
    SKBitmap bitmap;

    public ClippingTextPage()
    {
        Title = "Clipping Text";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        string resourceID = "SkiaSharpFormsDemos.Media.PageOfCode.png";
        Assembly assembly = GetType().GetTypeInfo().Assembly;

        using (Stream stream = assembly.GetManifestResourceStream(resourceID))
        {
            bitmap = SKBitmap.Decode(stream);
        }
    }
    ...
}
```

Die `PaintSurface` Handler beginnt mit der Erstellung einer `SKPaint` Objekt, das Text. Die `Typeface` festgelegt wird, sowie die `TextSize`, obwohl für diese bestimmte Anwendung der `TextSize` -Eigenschaft ist rein beliebiger. Beachten Sie außerdem gibt es keine `Style` festlegen.

Die `TextSize` und `Style` eigenschafteneinstellungen sind nicht erforderlich, da dies `SKPaint` betriebssystemobjekts wird ausschließlich für die `GetTextPath` aufrufen, verwenden die Zeichenfolge "CODE". Der Handler für measures klicken Sie dann die resultierenden `SKPath` -Objekt aus und wendet drei Transformationen zum Zentrieren und skalieren Sie sie auf die Größe der Seite. Der Pfad kann dann als des Freistellungspfads festgelegt werden:

```csharp
public class ClippingTextPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Blue);

        using (SKPaint paint = new SKPaint())
        {
            paint.Typeface = SKTypeface.FromFamilyName(null, SKTypefaceStyle.Bold);
            paint.TextSize = 10;

            using (SKPath textPath = paint.GetTextPath("CODE", 0, 0))
            {
                // Set transform to center and enlarge clip path to window height
                SKRect bounds;
                textPath.GetTightBounds(out bounds);

                canvas.Translate(info.Width / 2, info.Height / 2);
                canvas.Scale(info.Width / bounds.Width, info.Height / bounds.Height);
                canvas.Translate(-bounds.MidX, -bounds.MidY);

                // Set the clip path
                canvas.ClipPath(textPath);
            }
        }

        // Reset transforms
        canvas.ResetMatrix();

        // Display bitmap to fill window but maintain aspect ratio
        SKRect rect = new SKRect(0, 0, info.Width, info.Height);
        canvas.DrawBitmap(bitmap,
            rect.AspectFill(new SKSize(bitmap.Width, bitmap.Height)));
    }
}
```

Sobald des Freistellungspfads festgelegt ist, die Bitmap angezeigt werden kann, und es wird auf die Zeichenumrisse abgeschnitten werden. Beachten Sie die Verwendung der [ `AspectFill` ](xref:SkiaSharp.SKRect.AspectFill(SkiaSharp.SKSize)) -Methode der `SKRect` , die ein Rechteck für das Ausfüllen der Seite und unter Beibehaltung des Seitenverhältnisses berechnet.

Die **Texteffekts zum Pfad** Seite konvertiert ein einzelnes kaufmännisches und-Zeichen-Zeichen in einen Pfad zu einen 1D Pfad-Effekt zu erstellen. Ein Paint-Objekt mit diesem Pfad-Effekt wird zum Zeichnen der Überblick über eine größere Version des gleichen Zeichens verwendet:

[![](text-paths-images/textpatheffect-small.png "Dreifacher Screenshot der Seite Texteffekts zum Pfad")](text-paths-images/textpatheffect-large.png#lightbox "dreifachen Screenshot der Seite Texteffekts zum Pfad")

Viele Aufgaben in der [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) Klasse tritt in den Feldern und einen Konstruktor. Die beiden `SKPaint` Objekte definiert, wie Felder für zwei verschiedene Zwecke verwendet werden: Die erste (mit dem Namen `textPathPaint`) verwendet, um das kaufmännische und-Zeichen mit konvertieren eine `TextSize` von 50 auf einen Pfad für den Effekt der 1D-Pfad. Die zweite (`textPaint`) wird verwendet, um die größere Version das kaufmännische und-Zeichen mit, die sich auf Pfad angezeigt. Aus diesem Grund die `Style` dieses zweite lacktyp Objekt festgelegt ist, um `Stroke`, aber die `StrokeWidth` Eigenschaft ist nicht festgelegt werden, da diese Eigenschaft nicht erforderlich ist, bei Verwendung einer 1D Pfad Auswirkungen:

```csharp
public class TextPathEffectPage : ContentPage
{
    const string character = "@";
    const float littleSize = 50;

    SKPathEffect pathEffect;

    SKPaint textPathPaint = new SKPaint
    {
        TextSize = littleSize
    };

    SKPaint textPaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black
    };

    public TextPathEffectPage()
    {
        Title = "Text Path Effect";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;

        // Get the bounds of textPathPaint
        SKRect textPathPaintBounds = new SKRect();
        textPathPaint.MeasureText(character, ref textPathPaintBounds);

        // Create textPath centered around (0, 0)
        SKPath textPath = textPathPaint.GetTextPath(character,
                                                    -textPathPaintBounds.MidX,
                                                    -textPathPaintBounds.MidY);
        // Create the path effect
        pathEffect = SKPathEffect.Create1DPath(textPath, littleSize, 0,
                                               SKPath1DPathEffectStyle.Translate);
    }
    ...
}
```

Zunächst verwendet der Konstruktor der `textPathPaint` Objekt, das kaufmännische und-Zeichen mit messen einer `TextSize` von 50. Klicken Sie dann an die negative, der die Mittelpunktkoordinaten dieses Rechtecks übergeben werden die `GetTextPath` Methode, um den Text in einen Pfad zu konvertieren. Der resultierende Pfad ist der (0, 0) in der Mitte des Zeichens, eignet sich ideal für eine 1D Pfad Wirkung zeigen.

Sie könnten meinen, dass die `SKPathEffect` Objekt erstellt wurde, am Ende des Konstruktors festgelegt werden auf die `PathEffect` Eigenschaft `textPaint` statt als Feld gespeichert. Aber diese erwies nicht sehr gut funktionieren, da es die Ergebnisse der verzerrt die `MeasureText` rufen Sie in der `PaintSurface` Handler:

```csharp
public class TextPathEffectPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Set textPaint TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Do not measure the text with PathEffect set!
        SKRect textBounds = new SKRect();
        textPaint.MeasureText(character, ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Set the PathEffect property and display text
        textPaint.PathEffect = pathEffect;
        canvas.DrawText(character, xText, yText, textPaint);
    }
}
```

Dass `MeasureText` Aufruf wird verwendet, um das Zeichen, auf der Seite zentriert. Zur Vermeidung von Problemen, die `PathEffect` Eigenschaft für das Paint-Objekt festgelegt ist, nachdem der Text gemessen wurde, aber bevor sie angezeigt wird.

## <a name="outlines-of-character-outlines"></a>Beschreibung der Zeichenumrisse

Normalerweise die [ `GetFillPath` ](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) -Methode der `SKPaint` konvertiert einen Pfad zu einem anderen durch Anwenden von Paint-Eigenschaften, insbesondere die Stroke Breite und den Pfad Auswirkungen. Bei der Verwendung ohne Auswirkungen auf die Pfad, `GetFillPath` effektiv erstellt einen Pfad, der einen anderen Pfad beschreibt. Dies wurde veranschaulicht, der **Tippen Sie auf, um den Umriss des Pfads** auf der Seite die [ **Pfadeffekte** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) Artikel.

Sie können auch aufrufen `GetFillPath` auf dem Pfad, der von zurückgegebene `GetTextPath` zunächst werden aber möglicherweise nicht genau, was dies sieht ungefähr folgendermaßen.

Die **Gliederung Zeichenumrisse** Seite wird die Technik veranschaulicht. Der gesamte relevante Code befindet sich in der `PaintSurface` Handler, der die [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) Klasse.

Der Konstruktor beginnt mit der Erstellung einer `SKPaint` Objekt mit dem Namen `textPaint` mit einem `TextSize` -Eigenschaft basierend auf der Größe der Seite. Konvertiert einen Pfad mit der `GetTextPath` Methode. Die Koordinate Argumente `GetTextPath` center effektiv den Pfad auf dem Bildschirm:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    using (SKPaint textPaint = new SKPaint())
    {
        // Set Style for the character outlines
        textPaint.Style = SKPaintStyle.Stroke;

        // Set TextSize based on screen size
        textPaint.TextSize = Math.Min(info.Width, info.Height);

        // Measure the text
        SKRect textBounds = new SKRect();
        textPaint.MeasureText("@", ref textBounds);

        // Coordinates to center text on screen
        float xText = info.Width / 2 - textBounds.MidX;
        float yText = info.Height / 2 - textBounds.MidY;

        // Get the path for the character outlines
        using (SKPath textPath = textPaint.GetTextPath("@", xText, yText))
        {
            // Create a new path for the outlines of the path
            using (SKPath outlinePath = new SKPath())
            {
                // Convert the path to the outlines of the stroked path
                textPaint.StrokeWidth = 25;
                textPaint.GetFillPath(textPath, outlinePath);

                // Stroke that new path
                using (SKPaint outlinePaint = new SKPaint())
                {
                    outlinePaint.Style = SKPaintStyle.Stroke;
                    outlinePaint.StrokeWidth = 5;
                    outlinePaint.Color = SKColors.Red;

                    canvas.DrawPath(outlinePath, outlinePaint);
                }
            }
        }
    }
}
```

Die `PaintSurface` Handler erstellt dann einen neuen Pfad, der mit dem Namen `outlinePath`. Dies ist der angegebene Zielpfad im Aufruf von `GetFillPath`. Die `StrokeWidth` Eigenschaft bewirkt, dass 25 `outlinePath` um den Umriss eines 25 Pixel breiten Pfads Kontur zuweisen Textzeichen zu beschreiben. Dieser Pfad wird rot mit einer Kontur Breite von 5 angezeigt:

[![](text-paths-images/characteroutlineoutlines-small.png "Dreifacher Screenshot der Seite Gliederung Zeichenumrisse")](text-paths-images/characteroutlineoutlines-large.png#lightbox "dreifachen Screenshot der Seite Gliederung Zeichenumrisse")

Sehen Sie sich genau und Sie sehen überlappt, in denen eine spitze Ecke ist der Pfad-Gliederung. Hierbei handelt es sich um normale Elemente dieses Prozesses.

## <a name="text-along-a-path"></a>Text entlang eines Pfads

Text wird normalerweise auf eine horizontale Baseline angezeigt. Text gedreht werden kann, führen Sie vertikal oder diagonal angezeigt, aber die Baseline ist immer noch eine gerade Linie.

Es gibt jedoch manchmal, wenn Sie Text entlang einer Kurve ausführen möchten. Dies ist der Zweck der [ `DrawTextOnPath` ](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) -Methode der `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Im ersten Argument angegebene Text wird durchgeführt, entlang des Pfads angegeben wird, als zweites Argument ausgeführt. Sie können beginnen, den Text mit einem Offset vom Anfang des Pfads mit dem `hOffset` Argument. Normalerweise bildet der Pfad die Baseline des Texts: Text Oberlängen sind auf einer Seite des Pfads und Text Unterlängen werden auf dem anderen. Aber Sie können den offset der Text-Baseline aus dem Pfad mit der `vOffset` Argument.

Diese Methode hat keine Möglichkeit, eine Anleitung zum Einrichten der `TextSize` Eigenschaft `SKPaint` damit der Text, der Größe perfekt vom Anfang des Pfads bis zum Ende ausgeführt wird. In einigen Fällen können Sie dieser Größe selbst ermitteln. In anderen Fällen müssen Sie die Pfad-messen-Funktionen verwenden, um im nächsten Artikel beschrieben werden, auf [ **Pfadinformationen und-Enumeration**](information.md).

Die **zirkuläre Text** Programm umfließt Text mit einem Kreis. Es ist einfach, den Umfang eines Kreises, ermittelt werden, sodass es einfach, die Größe des Texts, der genau ist. Die `PaintSurface` Handler, der die [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) -Klasse berechnet einen Radius eines Kreises anhand der Größe der Seite. Diese Kreis wird `circularPath`:

```csharp
public class CircularTextPage : ContentPage
{
    const string text = "xt in a circle that shapes the te";
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPath circularPath = new SKPath())
        {
            float radius = 0.35f * Math.Min(info.Width, info.Height);
            circularPath.AddCircle(info.Width / 2, info.Height / 2, radius);

            using (SKPaint textPaint = new SKPaint())
            {
                textPaint.TextSize = 100;
                float textWidth = textPaint.MeasureText(text);
                textPaint.TextSize *= 2 * 3.14f * radius / textWidth;

                canvas.DrawTextOnPath(text, circularPath, 0, 0, textPaint);
            }
        }
    }
}
```

Die `TextSize` Eigenschaft `textPaint` wird dann so angepasst, dass die Textbreite der Umfang des Kreises entspricht:

[![](text-paths-images/circulartext-small.png "Dreifacher Screenshot der Seite für zirkuläre Text")](text-paths-images/circulartext-large.png#lightbox "dreifachen Screenshot der Seite für zirkuläre Text")

Der eigentliche Text gewählt wurde, um auch etwas zirkulär sein: Das Wort "Circle" ist sowohl das Subjekt des Satzes und das Objekt von einem Präposition.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
