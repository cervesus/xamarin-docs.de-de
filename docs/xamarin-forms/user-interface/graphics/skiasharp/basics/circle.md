---
Title: "Drawing a Simple Circle in skiasharp" Description: "in diesem Artikel werden die Grundlagen der skiasharp-Zeichnung erläutert, einschließlich der canvasen und Zeichnungsobjekte in Xamarin.Forms Anwendungen, und es wird ein Beispielcode veranschaulicht."
ms. Prod: xamarin ms. Technology: xamarin-skiasharp ms. assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8 Author: davidbritch ms. Author: dabritch ms. Date: 03/10/2017 NO-LOC: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="drawing-a-simple-circle-in-skiasharp"></a>Zeichnen eines einfachen Kreises in skiasharp

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erlernen Sie die Grundlagen der skiasharp-Zeichnung, einschließlich der canvasen und der Paint-Objekte_

In diesem Artikel werden die Konzepte des Zeichnens von Grafiken in Xamarin.Forms mithilfe von skiasharp vorgestellt. dazu gehören das Erstellen eines `SKCanvasView` Objekts zum Hosten der Grafiken, das Behandeln des `PaintSurface` Ereignisses und das Verwenden eines- `SKPaint` Objekts zum Angeben von Farben und anderen Zeichnungs Attributen.

Das [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programm enthält den gesamten Beispielcode für diese Reihe von skiasharp-Artikeln. Auf der ersten Seite wird der **einfache Kreis** aufgerufen und die Page-Klasse aufgerufen [`SimpleCirclePage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) . Dieser Code zeigt, wie ein Kreis in der Mitte der Seite mit einem Radius von 100 Pixeln gezeichnet wird. Die Gliederung des Kreises ist rot, und das Innere des Kreises ist blau.

![](circle-images/circleexample.png "A blue circle outlined in red")

Die [`SimpleCircle`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Page-Klasse wird von abgeleitet `ContentPage` und enthält zwei `using` Direktiven für die skiasharp-Namespaces:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Der folgende Konstruktor der-Klasse erstellt ein [`SKCanvasView`](xref:SkiaSharp.Views.Forms.SKCanvasView) -Objekt, fügt einen Handler für das [`PaintSurface`](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface) -Ereignis an und legt das- `SKCanvasView` Objekt als Inhalt der Seite fest:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

Der `SKCanvasView` belegt den gesamten Inhalts Bereich der Seite. Alternativ können Sie eine `SKCanvasView` mit anderen Xamarin.Forms `View` Ableitungen kombinieren, wie Sie in anderen Beispielen sehen werden.

Der `PaintSurface` Ereignishandler ist der Ort, an dem Sie Ihre Zeichnung durchführen. Diese Methode kann mehrmals aufgerufen werden, während das Programm ausgeführt wird. Daher sollten Sie alle Informationen erhalten, die zum erneuten Erstellen der Grafik Anzeige erforderlich sind:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

Das [`SKPaintSurfaceEventArgs`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs) Objekt, das das Ereignis begleitet, verfügt über zwei Eigenschaften:

- [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info) vom Typ [`SKImageInfo`](xref:SkiaSharp.SKImageInfo)
- [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface) vom Typ [`SKSurface`](xref:SkiaSharp.SKSurface)

Die `SKImageInfo` Struktur enthält Informationen über die Zeichen Oberfläche, vor allem deren Breite und Höhe in Pixel. Das- `SKSurface` Objekt stellt die Zeichen Oberfläche selbst dar. In diesem Programm ist die Zeichen Oberfläche eine Videoanzeige, aber in anderen Programmen kann ein `SKSurface` Objekt auch eine Bitmap darstellen, die Sie zum Zeichnen von skiasharp verwenden.

Die wichtigste Eigenschaft von `SKSurface` ist [`Canvas`](xref:SkiaSharp.SKSurface.Canvas) vom Typ [`SKCanvas`](xref:SkiaSharp.SKCanvas) . Diese Klasse ist ein Grafik Zeichnungs Kontext, den Sie verwenden, um die eigentliche Zeichnung auszuführen. Das `SKCanvas` -Objekt kapselt einen Grafik Zustand, der Grafik Transformationen und Clipping umfasst.

Hier ist ein typischer Start eines- `PaintSurface` Ereignis Handlers:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

Die- [`Clear`](xref:SkiaSharp.SKCanvas.Clear) Methode löscht den Zeichenbereich mit einer transparenten Farbe. Mit einer Überladung können Sie eine Hintergrundfarbe für den Zeichenbereich angeben.

Das Ziel hierbei ist das Zeichnen eines roten Kreises, der blau gefüllt ist. Da dieses Grafik Bild zwei verschiedene Farben enthält, muss der Auftrag in zwei Schritten ausgeführt werden. Der erste Schritt besteht darin, die Kontur des Kreises zu zeichnen. Um die Farbe und andere Merkmale der Zeile anzugeben, erstellen und initialisieren Sie ein [`SKPaint`](xref:SkiaSharp.SKPaint) Objekt:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

Die- [`Style`](xref:SkiaSharp.SKPaint.Style) Eigenschaft gibt an, dass Sie eine Linie (in diesem Fall die Gliederung des Kreises) als *Strich* zeichnen möchten, anstatt das innere *auszufüllen* . Die drei Member der- [`SKPaintStyle`](xref:SkiaSharp.SKPaintStyle) Enumeration lauten wie folgt:

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

Der Standardwert lautet `Fill`. Verwenden Sie die dritte Option, um die Zeile zu zeichnen, und füllen Sie das Innere mit der gleichen Farbe aus.

Legen Sie die- [`Color`](xref:SkiaSharp.SKPaint.Color) Eigenschaft auf einen Wert vom Typ fest [`SKColor`](xref:SkiaSharp.SKColor) . Eine Möglichkeit, einen-Wert zu erhalten, `SKColor` besteht darin, einen Xamarin.Forms `Color` Wert `SKColor` mithilfe der-Erweiterungsmethode in einen-Wert umzuwandeln [`ToSKColor`](xref:SkiaSharp.Views.Forms.Extensions.ToSKColor*) . Die- [`Extensions`](xref:SkiaSharp.Views.Forms.Extensions) Klasse im- `SkiaSharp.Views.Forms` Namespace enthält andere Methoden, die zwischen Xamarin.Forms Werten und skiasharp-Werten konvertieren.

Die- [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) Eigenschaft gibt die Stärke der Linie an. Hier ist der Wert auf 25 Pixel festgelegt.

Sie verwenden dieses `SKPaint` Objekt, um den Kreis zu zeichnen:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Koordinaten werden relativ zur oberen linken Ecke der Anzeige Oberfläche angegeben. X-Koordinaten erhöhen den rechten und Y-Koordinaten erhöhen das Herunterfahren. Im Erörterung von Grafiken wird häufig die Mathematische Notation (x, y) verwendet, um einen Punkt anzugeben. Der Punkt (0, 0) ist die linke obere Ecke der Anzeige Oberfläche und wird häufig als *Ursprung*bezeichnet.

Die ersten beiden Argumente von `DrawCircle` geben die X-und Y-Koordinaten der Mitte des Kreises an. Diese werden der Hälfte der Breite und Höhe der Anzeige Oberfläche zugewiesen, um die Mitte des Kreises in der Mitte der Anzeige Oberfläche zu platzieren. Das dritte Argument gibt den Radius des Kreises an, und das letzte Argument ist das- `SKPaint` Objekt.

Um das Innere des Kreises auszufüllen, können Sie zwei Eigenschaften des `SKPaint` Objekts ändern und erneut aufzurufen `DrawCircle` . Dieser Code zeigt auch eine alternative Methode, um einen `SKColor` Wert aus einem der vielen Felder der Struktur zu erhalten [`SKColors`](xref:SkiaSharp.SKColors) :

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```

Dieses Mal füllt der `DrawCircle` Aufruf den Kreis mithilfe der neuen Eigenschaften des- `SKPaint` Objekts.

Hier ist das Programm, das unter IOS und Android ausgeführt wird:

[![](circle-images/simplecircle-small.png "Triple screenshot of the Simple Circle page")](circle-images/simplecircle-large.png#lightbox "Triple screenshot of the Simple Circle page")

Wenn Sie das Programm selbst ausführen, können Sie das Telefon oder den Simulator seitwärts schalten, um zu sehen, wie die Grafik neu gezeichnet wird. Jedes Mal, wenn die Grafik neu gezeichnet werden muss, `PaintSurface` wird der Ereignishandler erneut aufgerufen.

Es ist auch möglich, grafische Objekte mit Farbverläufen oder bitmapkacheln zu färben. Diese Optionen werden im Abschnitt über [**skiasharp-Shader**](../effects/shaders/index.md)erläutert.

Bei einem `SKPaint` Objekt handelt es sich nur um eine Auflistung von Grafik Zeichnungs Eigenschaften. Diese Objekte sind einfach. Sie können `SKPaint` Objekte wie dieses Programm wieder verwenden, oder Sie können mehrere `SKPaint` Objekte für verschiedene Kombinationen von Zeichnungs Eigenschaften erstellen. Sie können diese Objekte außerhalb des Ereignis Handlers erstellen und initialisieren `PaintSurface` , und Sie können Sie als Felder in der Page-Klasse speichern.

> [!NOTE]
> Die- `SKPaint` Klasse definiert ein [`IsAntialias`](xref:SkiaSharp.SKPaint.IsAntialias) , um das Antialiasing beim Rendering Ihrer Grafiken zu aktivieren. Antialiasing führt in der Regel zu optisch reibungslosen Kanten, sodass Sie diese Eigenschaft wahrscheinlich `true` in den meisten ihrer Objekte auf festlegen möchten `SKPaint` . Aus Gründen der Einfachheit wird diese Eigenschaft _nicht_ in den meisten Beispielseiten festgelegt.

Obwohl die Breite des Kreis Gliederung als 25 Pixel &mdash; oder ein Viertel des Radius des Kreises festgelegt ist &mdash; , scheint es dünner zu sein, und es gibt einen guten Grund dafür: die Hälfte der Breite der Linie wird durch den blauen Kreis verdeckt. Die Argumente für die `DrawCircle` Methode definieren die abstrakten geometrischen Koordinaten eines Kreises. Das blaue Innere ist für diese Dimension auf das nächste Pixel dimensioniert, aber die 25 Pixel weite Gliederung verspannt die geometrische Kreis &mdash; Hälfte auf der inneren und der Hälfte der Außenseite.

Das nächste Beispiel im Artikel " [Integration Xamarin.Forms in](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) " veranschaulicht dies visuell.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
