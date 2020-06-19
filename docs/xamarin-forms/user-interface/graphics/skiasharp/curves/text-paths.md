---
title: Pfade und Text in skiasharp
description: Dieser Artikel untersucht die Schnittmenge von skiasharp-Pfaden und-Text und veranschaulicht dies mit Beispielcode.
ms.prod: xamarin
ms.assetid: C14C07F6-4A84-4A8C-BDB4-CD61FBF0F79B
ms.technology: xamarin-skiasharp
author: davidbritch
ms.author: dabritch
ms.date: 08/01/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b0cbb7d26a2aea02a3255fc75947c20a3d803b86
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84131897"
---
# <a name="paths-and-text-in-skiasharp"></a>Pfade und Text in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Untersuchen der Schnittmenge von Pfaden und Text_

In modernen Grafiksystemen sind Text Schriftarten Auflistungen von Zeichen umrissen, die normalerweise durch Quadratische Bézier-Kurven definiert werden. Folglich enthalten viele moderne Grafik Systeme eine Möglichkeit, Textzeichen in einen Grafik Pfad zu konvertieren.

Sie haben bereits gesehen, dass Sie die Kontur von Textzeichen zeichnen und ausfüllen können. Dies ermöglicht es Ihnen, diese Zeichen umrissen mit einer bestimmten Strichbreite und sogar einem Pfad Effekt anzuzeigen, wie im Artikel [**path-Effekte**](effects.md) beschrieben. Es ist jedoch auch möglich, eine Zeichenfolge in ein- `SKPath` Objekt zu konvertieren. Dies bedeutet, dass Text Gliederungen für das Clipping mit Techniken verwendet werden können, die im Artikel [**Clipping mit Pfaden und Regionen**](clipping.md) beschrieben wurden.

Neben der Verwendung eines Pfad Effekts zum Zeichnen einer Zeichen Gliederung können Sie auch Pfad Effekte erstellen, die auf einem Pfad basieren, der von einer Zeichenfolge abgeleitet ist, und Sie können sogar die beiden Effekte kombinieren:

![](text-paths-images/pathsandtextsample.png "Text Path Effect")

Im vorherigen Artikel zu [**path-Effekten**](effects.md)haben Sie gesehen, wie die- [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) Methode von eine Gliederung `SKPaint` eines gezeichneten Pfades abrufen kann. Sie können diese Methode auch mit Pfaden verwenden, die von Zeichen umrissen abgeleitet werden.

Schließlich wird in diesem Artikel eine weitere Schnittmenge von Pfaden und Text veranschaulicht: mit der- [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) Methode von `SKCanvas` können Sie eine Text Zeichenfolge anzeigen, sodass die Baseline des Texts einem gekrümmten Pfad folgt.

## <a name="text-to-path-conversion"></a>Konvertierung von Text in Pfad

Die- [`GetTextPath`](xref:SkiaSharp.SKPaint.GetTextPath(System.String,System.Single,System.Single)) Methode von `SKPaint` konvertiert eine Zeichenfolge in ein- `SKPath` Objekt:

```csharp
public SKPath GetTextPath (String text, Single x, Single y)
```

Das `x` - `y` Argument und das-Argument geben den Ausgangspunkt der Baseline auf der linken Seite des Texts an. Sie spielen hier dieselbe Rolle wie in der- `DrawText` Methode von `SKCanvas` . Innerhalb des Pfads weist die Baseline der linken Seite des Texts die Koordinaten (x, y) auf.

Die- `GetTextPath` Methode ist übertrieben, wenn Sie nur den resultierenden Pfad ausfüllen oder abzeichnen möchten. Die normale `DrawText` Methode ermöglicht dies. Die- `GetTextPath` Methode ist für andere Aufgaben, die Pfade betreffen, nützlicher.

Eine dieser Aufgaben ist das Clipping. Auf der Seite **Clipping-Text** wird ein clippingpfad basierend auf den Zeichen umrissen des Worts "Code" erstellt. Dieser Pfad wird auf die Größe der Seite gestreckt, um eine Bitmap zu schneiden, die ein Bild des **Clipping-Text** Quellcodes enthält:

[![](text-paths-images/clippingtext-small.png "Triple screenshot of the Clipping Text page")](text-paths-images/clippingtext-large.png#lightbox "Triple screenshot of the Clipping Text page")

Der- [`ClippingTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/ClippingTextPage.cs) Klassenkonstruktor lädt die Bitmap, die als eingebettete Ressource gespeichert ist, in den **Medien** Ordner der Projekt Mappe:

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

Der `PaintSurface` Handler erstellt zunächst ein `SKPaint` für Text geeignetes Objekt. Die- `Typeface` Eigenschaft und die-Eigenschaft werden ebenfalls festgelegt `TextSize` , obwohl für diese spezielle Anwendung die- `TextSize` Eigenschaft ausschließlich willkürlich ist. Beachten Sie auch, dass es keine `Style` Einstellung gibt.

Die `TextSize` -Eigenschaft und die- `Style` Eigenschaft sind nicht erforderlich, da dieses `SKPaint` Objekt ausschließlich für den-Befehl `GetTextPath` mit der Text Zeichenfolge "Code" verwendet wird. Der Handler misst dann das resultierende `SKPath` Objekt und wendet drei Transformationen an, um es zu zentrieren und auf die Größe der Seite zu skalieren. Der Pfad kann dann als clippingpfad festgelegt werden:

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

Nachdem der clippingpfad festgelegt wurde, kann die Bitmap angezeigt werden, und Sie wird auf die Zeichen umrissen zugeschnitten. Beachten Sie die Verwendung der- [`AspectFill`](xref:SkiaSharp.SKRect.AspectFill(SkiaSharp.SKSize)) Methode von `SKRect` , die ein Rechteck zum Auffüllen der Seite berechnet, während das Seitenverhältnis beibehalten wird.

Auf der Seite **Text Pfad Effekt** werden ein einzelnes kaufmännisches Zeichen und ein Zeichen in einen Pfad konvertiert, um einen 1D-Pfad Effekt zu erstellen. Ein Paint-Objekt mit diesem Pfad Effekt wird dann verwendet, um die Kontur einer größeren Version desselben Zeichens zu zeichnen:

[![](text-paths-images/textpatheffect-small.png "Triple screenshot of the Text Path Effect page")](text-paths-images/textpatheffect-large.png#lightbox "Triple screenshot of the Text Path Effect page")

Ein Großteil der Arbeit in der [`TextPathEffectPath`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/TextPathEffectPage.cs) -Klasse tritt in den Feldern und dem Konstruktor auf. Die beiden `SKPaint` -Objekte, die als Felder definiert sind, werden für zwei unterschiedliche Zwecke verwendet: die erste (mit dem Namen `textPathPaint` ) wird verwendet, um das kaufmännische und-Element mit einem `TextSize` von 50 in einen Pfad für den 1D-Pfad Effekt zu konvertieren. Die zweite ( `textPaint` ) wird verwendet, um die größere Version des kaufmännischen und mit diesem Pfad Effekt anzuzeigen. Aus diesem Grund wird der `Style` dieses zweiten Paint-Objekts auf festgelegt `Stroke` , aber die- `StrokeWidth` Eigenschaft ist nicht festgelegt, da diese Eigenschaft bei Verwendung eines 1D-Pfad Effekts nicht erforderlich ist:

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

Der-Konstruktor verwendet zunächst das `textPathPaint` -Objekt, um das kaufmännische und-Element mit einem-Wert `TextSize` von 50 zu messen. Die negativen Werte der Mittelpunkt Koordinaten dieses Rechtecks werden dann an die-Methode weitergegeben `GetTextPath` , um den Text in einen Pfad zu konvertieren. Der resultierende Pfad weist den (0,0) Punkt in der Mitte des Zeichens auf, der sich ideal für einen 1D-Pfad Effekt eignet.

Möglicherweise denken Sie daran, dass das `SKPathEffect` am Ende des Konstruktors erstellte Objekt auf die-Eigenschaft von festgelegt werden kann `PathEffect` `textPaint` und nicht als Feld gespeichert wird. Dies erwies sich jedoch nicht sehr gut, da die Ergebnisse des `MeasureText` Aufrufens im Handler verzerrt wurden `PaintSurface` :

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

Dieser-Befehl `MeasureText` wird verwendet, um das Zeichen auf der Seite zu zentrieren. Um Probleme zu vermeiden, `PathEffect` wird die-Eigenschaft auf das Paint-Objekt festgelegt, nachdem der Text gemessen, aber bevor es angezeigt wird.

## <a name="outlines-of-character-outlines"></a>Gliederungen von Zeichen umrissen

Normalerweise [`GetFillPath`](xref:SkiaSharp.SKPaint.GetFillPath(SkiaSharp.SKPath,SkiaSharp.SKPath,SkiaSharp.SKRect,System.Single)) konvertiert die-Methode von `SKPaint` einen Pfad in einen anderen, indem Paint-Eigenschaften angewendet werden, insbesondere die Strichbreite und der Pfad Effekt. Bei Verwendung ohne Pfad Effekte `GetFillPath` erstellt effektiv einen Pfad, der einen anderen Pfad beschreibt. Dies wurde im Artikel **Tippen Sie, um die Pfad** Seite im Artikel " [**path Effects**](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md) " veranschaulicht.

Sie können auch `GetFillPath` für den Pfad, der von zurückgegeben `GetTextPath` wird, anrufen, aber an erster Stelle ist es möglicherweise nicht ganz sicher, wie das aussehen würde.

Die Seite " **Zeichen** Gliederung" veranschaulicht das Verfahren. Der gesamte relevante Code befindet sich im- `PaintSurface` Handler der- [`CharacterOutlineOutlinesPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CharacterOutlineOutlinesPage.cs) Klasse.

Der Konstruktor beginnt mit dem Erstellen eines `SKPaint` Objekts namens `textPaint` mit einer-Eigenschaft, die `TextSize` auf der Größe der Seite basiert. Dies wird mithilfe der-Methode in einen Pfad konvertiert `GetTextPath` . Die Koordinaten Argumente zum `GetTextPath` effektiven Zentrieren des Pfads auf dem Bildschirm:

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

Der `PaintSurface` Handler erstellt dann einen neuen Pfad mit dem Namen `outlinePath` . Dies wird zum Zielpfad im-Befehl `GetFillPath` . Die- `StrokeWidth` Eigenschaft von 25 bewirkt, dass die Gliederung `outlinePath` eines 25 Pixel weiten Pfades, der die Textzeichen überschneidet, beschrieben wird. Dieser Pfad wird dann rot mit einer Strichbreite von 5 angezeigt:

[![](text-paths-images/characteroutlineoutlines-small.png "Triple screenshot of the Character Outline Outlines page")](text-paths-images/characteroutlineoutlines-large.png#lightbox "Triple screenshot of the Character Outline Outlines page")

Sehen Sie sich genau an, und Sie sehen Überschneidungen, wenn die Pfad Gliederung eine Spitze Ecke bildet. Dabei handelt es sich um normale Artefakte dieses Prozesses.

## <a name="text-along-a-path"></a>Text entlang eines Pfads

Text wird normalerweise in einer horizontalen Baseline angezeigt. Text kann gedreht werden, um vertikal oder diagonal auszuführen, die Baseline ist aber immer noch eine gerade Linie.

Es gibt jedoch Zeiten, in denen Sie Text entlang einer Kurve ausführen möchten. Dies ist der Zweck der- [`DrawTextOnPath`](xref:SkiaSharp.SKCanvas.DrawTextOnPath(System.String,SkiaSharp.SKPath,System.Single,System.Single,SkiaSharp.SKPaint)) Methode von `SKCanvas` :

```csharp
public Void DrawTextOnPath (String text, SKPath path, Single hOffset, Single vOffset, SKPaint paint)
```

Der im ersten Argument angegebene Text wird so erstellt, dass er an dem als zweites Argument angegebenen Pfad ausgeführt wird. Sie können den Text am Anfang des Pfads mit dem- `hOffset` Argument beginnen. Normalerweise bildet der Pfad die Baseline des Texts: Text-Aufsteiger befinden sich auf einer Seite des Pfads, und Text Nachfolger auf der anderen Seite. Allerdings können Sie die TextBaseline mit dem-Argument aus dem Pfad versetzt werden `vOffset` .

Diese Methode hat keine Möglichkeit, eine Anleitung zum Festlegen der- `TextSize` Eigenschaft von bereitzustellen `SKPaint` , um die Größe des Texts von Anfang bis Ende zu ändern. Manchmal können Sie diese Textgröße selbst ermitteln. In anderen Zeiten müssen Sie Pfad Messungs Funktionen verwenden, die im nächsten Artikel zu [**Pfadinformationen und Enumeration**](information.md)beschrieben werden.

Das **zirkuläre Text** Programm umschließt Text um einen Kreis. Es ist einfach, den Umfang eines Kreises zu ermitteln, sodass es einfach ist, den Text so zu verkleinern, dass er genau passt. Der- `PaintSurface` Handler der- [`CircularTextPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Curves/CircularTextPage.cs) Klasse berechnet einen Radius eines Kreises basierend auf der Größe der Seite. Dieser Kreis wird zu `circularPath` :

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

Die- `TextSize` Eigenschaft von `textPaint` wird dann so angepasst, dass die Text Breite mit dem Umfang des Kreises übereinstimmt:

[![](text-paths-images/circulartext-small.png "Triple screenshot of the Circular Text page")](text-paths-images/circulartext-large.png#lightbox "Triple screenshot of the Circular Text page")

Der Text selbst wurde auch etwas zirkulär gewählt: das Wort "Circle" ist sowohl der Betreff des Satzes als auch das Objekt eines Ausdrucks mit vorangestelltem Ausdruck.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
