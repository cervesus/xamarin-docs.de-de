---
title: Pfade und Text in SkiaSharp
description: In diesem Artikel wird erklärt, die Schnittmenge der SkiaSharp Pfade und Text, und wird dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/01/2017
ms.openlocfilehash: 305ee2946d3a291e6237d5a2860eda7331193b23
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35243904"
---
# <a name="paths-and-text-in-skiasharp"></a>Pfade und Text in SkiaSharp

_Untersuchen Sie die Schnittmenge von Pfaden und text_

In modernen Graphics-Systemen sind Textschriftarten Auflistungen von Zeichen Gliederungen quadratische Bézier-Kurven in der Regel definiert. Folglich umfassen viele moderne Graphics-Systeme eine Funktion zum Konvertieren von Textzeichen in einem Grafikpfad.

Sie haben bereits gesehen, dass Sie können die Umrisse von Textzeichen zeichnen sowie auszufüllen. Dies ermöglicht Ihnen, diese Zeichen werden mit einem bestimmten Strichbreite und sogar einen Pfad Effekt anzuzeigen, wie beschrieben in der [ **Pfad Effekte** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) Artikel. Es ist jedoch auch möglich, konvertieren eine Zeichenfolge in ein `SKPath` Objekt. Dies bedeutet, dass Text Gliederungen für Clipping mit Techniken verwendet werden können, die in beschrieben wurden die [ **Clipping mit Pfade und Regionen** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md) Artikel.

Neben dem Verwenden eines Effekts Pfad, um ein Zeichenumriss zu zeichnen, können Sie auch Pfad bei Ihnen die Auswirkungen, die auf eine Pfade basieren von Zeichenfolgen abgeleitet sind erstellen, und Sie können sogar kombinieren, die zwei Auswirkungen:

![](text-paths-images/pathsandtextsample.png "Pfad der Texteffekt")

In der [vorherigen Artikel](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) Sie gesehen haben wie die [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) Methode `SKPaint` Konturen eines gestrichelt Pfads abrufen können. Sie können auch diese Methode mit Pfaden Zeichen Gliederungen abgeleitet.

In diesem Artikel wird schließlich eine andere Schnittmenge von Pfaden und Text: der [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) Methode `SKCanvas` können Sie eine Textzeichenfolge angezeigt, damit die Basislinie des Texts ein Kurvenpfads folgt.

## <a name="text-to-path-conversion"></a>Text zur Pfad-Konvertierung

Die [ `GetTextPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetTextPath/p/System.String/System.Single/System.Single/) Methode `SKPaint` konvertiert eine Zeichenfolge in ein `SKPath` Objekt:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

Die `x` und `y` Argumente angeben, den Ausgangspunkt der Baseline an der linken Seite des Texts. Diese Rolle dabei spielen, die gleiche hier als der `DrawText` Methode `SKCanvas`. In diesem Pfad müssen die Basislinie des linken Rands des Texts der Koordinaten (X, y).

Die `GetTextPath` Methode ist übertrieben, wenn lediglich sollen die gewünschten Eintragungen oder den resultierenden Pfad mit Strichen zu zeichnen. Die normale `DrawText` -Methode ermöglicht es Ihnen dies. Die `GetTextPath` Methode eignet sich eher für andere Aufgaben im Zusammenhang mit Pfaden.

Diese Tasks ist abschneiden. Die **Clipping Text** -Seite erstellt einen Freistellungspfad basierend auf der Gliederungen Zeichen des Worts "CODE". Dieser Pfad wird gestreckt, um die Größe der Seite, um eine Bitmap zugeschnitten werden soll, die ein Bild enthält die **Text kürzen** Quellcode:

[![](text-paths-images/clippingtext-small.png "Dreifacher Screenshot der Seite Text kürzen")](text-paths-images/clippingtext-large.png#lightbox "dreifacher Screenshot der Seite Text kürzen")

Die [ `ClippingTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Klassenkonstruktor lädt die Bitmap, die als eingebettete Ressource in gespeichert ist die **Medien** Ordner der Projektmappe:

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
        using (SKManagedStream skStream = new SKManagedStream(stream))
        {
            bitmap = SKBitmap.Decode(skStream);
        }
    }
    ...
}
```

Die `PaintSurface` Handler beginnt mit der Erstellung einer `SKPaint` Objekt für Text geeignet ist. Die `Typeface` festgelegt wird, sowie die `TextSize`, obwohl für die jeweilige Anwendung die `TextSize` Eigenschaft ist rein Arbirtrary. Beachten Sie außerdem, es ist keine `Style` Einstellung.

Die `TextSize` und `Style` eigenschafteneinstellungen sind nicht erforderlich, da dies `SKPaint` Objekt dient ausschließlich für die `GetTextPath` aufrufen, verwenden die Zeichenfolge "CODE". Der Handler measures klicken Sie dann die resultierenden `SKPath` -Objekt aus und wendet Sie drei Transformationen zum Zentrieren sie und auf die Größe der Seite zu skalieren. Der Pfad kann dann als des Freistellungspfads festgelegt werden:

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

Nach des Freistellungspfads festlegen Bitmap angezeigt werden kann, und es an den Pfaden Zeichen abgeschnitten. Beachten Sie die Verwendung der [ `AspectFill` ](https://developer.xamarin.com/api/member/SkiaSharp.SKRect.AspectFill/p/SkiaSharp.SKSize/) Methode `SKRect` , die ein Rechteck für das Ausfüllen der Seite Beibehaltung des Seitenverhältnisses berechnet.

Die **Pfad Texteffekt** Seite konvertiert ein einzelnes kaufmännisches und-Zeichen, einen Pfad zu einen 1D Pfad Effekt zu erstellen. Ein Paint-Objekt mit dieser Pfad Effekt wird dann zum Zeichnen der Kontur einer größeren Version des gleichen Zeichens verwendet:

[![](text-paths-images/textpatheffect-small.png "Dreifacher Screenshot der Seite \"Pfad Texteffekt\"")](text-paths-images/textpatheffect-large.png#lightbox "dreifacher Screenshot der Seite \"Pfad Texteffekt\"")

Viele Aufgaben in der [ `TextPathEffectPath` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) Klasse tritt in den Feldern und der Konstruktor. Die beiden `SKPaint` Objekte definiert, wie Felder für zwei unterschiedliche Zwecke verwendet werden: die erste (mit dem Namen `textPathPaint`) wird das kaufmännische und-Zeichen mit konvertiert eine `TextSize` von 50 auf einen Pfad für die Auswirkung der 1D Pfad. Die zweite (`textPaint`) wird verwendet, um die größere Version der das kaufmännische und-Zeichen mit diesem Pfad Effekt anzuzeigen. Aus diesem Grund die `Style` der dieses zweite Paint Objekt festgelegt ist `Stroke`, aber die `StrokeWidth` Eigenschaft ist nicht festgelegt werden, da diese Eigenschaft nicht erforderlich ist, bei Verwendung einer 1D Pfad Auswirkungen:

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

Der Konstruktor zuerst verwendet die `textPathPaint` -Objekt, das kaufmännische und-Zeichen mit messen einer `TextSize` von 50. Klicken Sie dann an die negative Center Koordinaten dieses Rechtecks übergeben werden die `GetTextPath` Methode, um den Text in einen Pfad zu konvertieren. Der resultierende Pfad ist der (0, 0) zeigen Sie in der Mitte des Zeichens enthalten, die sich ideal für eine 1D Pfad wirksam ist.

Sie könnten meinen, dass die `SKPathEffect` Objekt am Ende des Konstruktors erstellt festgelegt werden die `PathEffect` Eigenschaft `textPaint` statt als Feld gespeichert. Aber diese ausgeschaltet, nicht zu sehr gut funktionieren, da es die Ergebnisse der verzerrt den `MeasureText` rufen Sie in der `PaintSurface` Handler:

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

Dass `MeasureText` Aufruf wird verwendet, um das Zeichen auf der Seite zentriert. Zur Vermeidung von Problemen, die `PathEffect` Eigenschaft mit dem Paint-Objekt festgelegt ist, nachdem Sie der Text gemessen wurde, aber bevor er angezeigt wird.

## <a name="outlines-of-character-outlines"></a>Zeigt Umrisse von Zeichen

Normalerweise die [ `GetFillPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKPaint.GetFillPath/p/SkiaSharp.SKPath/SkiaSharp.SKPath/SkiaSharp.SKRect/System.Single/) Methode `SKPaint` konvertiert einen Pfad zu einem anderen durch Anwenden von Paint-Eigenschaften, insbesondere die Kontur Breite und den Pfad wirksam. Bei der Verwendung ohne Pfad Effekte `GetFillPath` effektiv erstellt einen Pfad, der einen anderen Pfad beschreibt. Dies wurde veranschaulicht, der **Tippen Sie auf Gliederung der Pfad** auf der Seite der [ **Pfad Effekte** ](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) Artikel.

Sie können auch aufrufen `GetFillPath` im Pfad Merry `GetTextPath` zunächst werden aber möglicherweise nicht ganz sicher Was würde folgendermaßen aussehen.

Die **Zeichen Gliederung Gliederungen** das Verfahren veranschaulicht. Der relevante Code befindet sich in der `PaintSurface` Handler, der die [ `CharacterOutlineOutlinesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) Klasse.

Der Konstruktor beginnt mit der Erstellung einer `SKPaint` Objekt mit dem Namen `textPaint` mit einem `TextSize` -Eigenschaft basierend auf der Größe der Seite. Dies wird konvertiert in einen Pfad mit der `GetTextPath` Methode. Die-Koordinate Argumente `GetTextPath` effektiv den Pfad auf dem Bildschirm zu zentrieren:

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

Die `PaintSurface` Handler erstellt dann einen neuen Pfad mit dem Namen `outlinePath`. Dies wird im Aufruf der Zielpfad `GetFillPath`. Die `StrokeWidth` Eigenschaft 25 Ursachen `outlinePath` um den Umriss einen Verlauf der Textzeichen 25 Pixel breiten-Pfad zu beschreiben. Dieser Pfad wird rot mit einer Kontur Breite von 5 angezeigt:

[![](text-paths-images/characteroutlineoutlines-small.png "Dreifacher Screenshot der Seite Zeichen Gliederung Gliederungen")](text-paths-images/characteroutlineoutlines-large.png#lightbox "dreifacher Screenshot der Seite Zeichen Gliederung Konturen")

Sehen Sie, und sehen Sie überlappt, in dem die Kontur Pfad eine spitze Ecke macht. Hierbei handelt es sich um normale Elemente dieses Prozesses.

## <a name="text-along-a-path"></a>Text entlang eines Pfads

Text wird normalerweise für eine horizontale Basislinie angezeigt. Text gedreht werden kann, führen Sie vertikal oder diagonal angezeigt, aber die Basislinie ist immer noch eine gerade Linie.

Es gibt jedoch Situationen, wenn Sie Text entlang einer Kurve ausführen soll. Dies ist der Zweck der [ `DrawTextOnPath` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawTextOnPath/p/System.String/SkiaSharp.SKPath/System.Single/System.Single/SkiaSharp.SKPaint/) Methode `SKCanvas`:

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Der Text im ersten Argument angegebenen erfolgt auszuführende entlang des Pfads als zweites Argument angegeben. Sie können den Text an einem Offset vom Anfang des Pfads mit beginnen die `hOffset` Argument. Normalerweise bildet der Pfad die Basislinie des Texts: Text Oberlängen befinden sich auf einer Seite des Pfads und Text Unterlängen bestimmt werden, auf dem anderen. Aber Sie können den offset der Grundlinie des Textes aus den Pfad mit der `vOffset` Argument.

Diese Methode hat keine Möglichkeit, bieten Anleitungen zur Einstellung der `TextSize` Eigenschaft `SKPaint` , Text zu automatisierenden perfekt vom Beginn des Pfads bis zum Ende ausführen. In einigen Fällen können Sie diese Textgröße auf Ihren eigenen herauszufinden. In einigen Fällen müssen Sie zum Messen der Path Funktionen verwenden, um in einem zukünftigen Artikel beschrieben werden.

Die **zirkuläre Text** Programm umfließt Text mit einem Kreis. Es ist einfach zu bestimmen den Umfang eines Kreises, daher ist es einfach zu groß ist den Text, der genau entsprechen. Die `PaintSurface` Handler, der die [ `CircularTextPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) Klasse berechnet einen Radius Umfang eines Kreises anhand der Größe der Seite. Wird dieser Kreis `circularPath`:

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

Die `TextSize` Eigenschaft `textPaint` dann angepasst, sodass die Breite des Texts der Umfang des Kreises entspricht:

[![](text-paths-images/circulartext-small.png "Dreifacher Screenshot der Seite zirkuläre Text")](text-paths-images/circulartext-large.png#lightbox "dreifacher Screenshot der Seite zirkuläre Text")

Der Text selbst wurde gewählt, um auch etwas Zirkular sein: das Wort "Circle" sowohl das Subjekt des Satzes und das Objekt von einem Präposition enthalten ist.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
