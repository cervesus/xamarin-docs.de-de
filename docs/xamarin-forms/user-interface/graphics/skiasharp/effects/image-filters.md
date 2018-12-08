---
title: SkiaSharp-Bildfilter
description: Erfahren Sie, wie Sie mit der Image-Filter zu erstellen Weichzeichner Schlagschatten.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 173E7B22-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/27/2018
ms.openlocfilehash: 517ebfb529dd26236ba157d40168fa7c75288d27
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050373"
---
# <a name="skiasharp-image-filters"></a>SkiaSharp-Bildfilter

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Bildfilter sind, die Auswirkungen, die ausgeführt werden auf alle Bits Farbe der Pixel, die sich ein Bild zusammensetzt. Sie sind flexibler als Maske-Filter, mit denen nur auf den alpha-Kanal ausgeführt werden, wie in diesem Artikel beschrieben [ **SkiaSharp Maske Filter**](mask-filters.md). Um ein Image-Filter verwenden, legen die [ `ImageFilter` ](xref:SkiaSharp.SKPaint.ImageFilter) Eigenschaft `SKPaint` auf ein Objekt des Typs [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter) , dass Sie durch einen Aufruf der statischen Methoden der Klasse erstellt haben.

Die beste Möglichkeit, mit der Maske Filter vertraut ist durch Experimentieren mit diesen statischen Methoden. Sie können einen Filter für die Maske verwenden, eine gesamte Bitmap Weichzeichnen:

![Blur-Beispiel](image-filters-images/ImageFilterExample.png "Blur-Beispiel")

Dieser Artikel veranschaulicht auch, einen Image-Filter verwenden, um einen Schatten, und für Prägen und Eingravieren Effekte zu erstellen.

## <a name="blurring-vector-graphics-and-bitmaps"></a>Weichzeichner Vektorgrafiken und bitmaps

Weichzeichnereffekts erstellt die [ `SKImageFilter.CreateBlur` ](xref:SkiaSharp.SKImageFilter.CreateBlur*) statische Methode hat einen deutlicher Vorteil gegenüber den Blur-Methoden in der [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter) Klasse: der Abbildfilter kann eine gesamte Bitmap Weichzeichner. Die Methode hat die folgende Syntax:

```csharp
public static SkiaSharp.SKImageFilter CreateBlur (float sigmaX, float sigmaY,
                                                  SKImageFilter input = null,
                                                  SKImageFilter.CropRect cropRect = null);
```

Die Methode verfügt über zwei Sigma Werte &mdash; die erste für den Wertebereich Blur in horizontaler Richtung und die zweite für den vertikaler Richtung. Sie können Bildfilter zu Kaskadieren von einem anderen Image-Filter angeben, wie das optionale dritte Argument. Ein zuschnittrechteck kann auch angegeben werden.

Die **Image experimentieren Blur-** auf der Seite die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) enthält zwei `Slider` Ansichten, mit denen Sie experimentieren mit unterschiedlicher Blur festlegen:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.ImageBlurExperimentPage"
             Title="Image Blur Experiment">

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei verwendet die beiden `Slider` Werte aufrufen `SKImageFilter.CreateBlur` für die `SKPaint` Objekt verwendet, um sowohl Text als auch eine Bitmap anzuzeigen:


```csharp
public partial class ImageBlurExperimentPage : ContentPage
{
    const string TEXT = "Blur My Text";

    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                            typeof(MaskBlurExperimentPage),
                            "SkiaSharpFormsDemos.Media.SeatedMonkey.jpg");

    public ImageBlurExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear(SKColors.Pink);

        // Get values from sliders
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = (info.Width - 100) / (TEXT.Length / 2);
            paint.ImageFilter = SKImageFilter.CreateBlur(sigmaX, sigmaY);

            // Get text bounds and calculate display rectangle
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);
            SKRect textRect = new SKRect(0, 0, info.Width, textBounds.Height + 50);

            // Center the text in the display rectangle
            float xText = textRect.Width / 2 - textBounds.MidX;
            float yText = textRect.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);

            // Calculate rectangle for bitmap
            SKRect bitmapRect = new SKRect(0, textRect.Bottom, info.Width, info.Height);
            bitmapRect.Inflate(-50, -50);

            canvas.DrawBitmap(bitmap, bitmapRect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

Die drei Screenshots zeigen verschiedene Einstellungen für die `sigmaX` und `sigmaY` Einstellungen:

[![Abbildung Blur-Experiment](image-filters-images/ImageBlurExperiment.png "Image Blur-Experiment")](image-filters-images/ImageBlurExperiment-Large.png#lightbox)

Um die Unschärfe zwischen verschiedenen Anzeigegrößen und Auflösungen in Einklang zu bringen, legen Sie `sigmaX` und `sigmaY` auf Werte, die proportional zur Pixelgröße des Bilds gerenderten sind, die die Unschärfe auf angewendet wird.

## <a name="drop-shadow"></a>Schlagschatten

Die [ `SKImageFilter.CreateDropShadow` ](xref:SkiaSharp.SKImageFilter.CreateDropShadow*) statische Methode erstellt eine `SKImageFilter` -Objekt für einen Schlagschatten:

```csharp
public static SKImageFilter CreateDropShadow (float dx, float dy,
                                              float sigmaX, float sigmaY,
                                              SKColor color,
                                              SKDropShadowImageFilterShadowMode shadowMode,
                                              SKImageFilter input = null,
                                              SKImageFilter.CropRect cropRect = null);
```

Legen Sie dieses Objekt auf der `ImageFilter` Eigenschaft eine `SKPaint` Objekt und alles, was Sie mit diesem Objekt zeichnen einen Schlagschatten zugrunde liegende haben.

Die `dx` und `dy` Parameter geben die horizontalen und vertikalen Offsets des Schattens in Pixel des grafischen Objekts an. Die Konvention bei 2D-Grafiken ist davon aus einer Lichtquelle, die von oben links das bedeutet, dass beide dieser Argumente zum Positionieren des Schattens nach unten und rechts des grafischen Objekts positive werden soll.

Die `sigmaX` und `sigmaY` Parameter verschwimmen Faktoren für das Rendern des Schlagschattens.

Die `color` -Parameter ist die Farbe des Schlagschattens. Dies `SKColor` Wert kann Transparenz enthalten. Eine Möglichkeit besteht darin, den Farbwert `SKColors.Black.WithAlpha(0x80)` auf eine Farbe im Hintergrund laufende abgedunkelt werden soll.

Die letzten beiden Parameter sind optional.

Die **löschen Schatten experimentieren** programmieren können, die Sie mit den Werten experimentieren `dx`, `dy`, `sigmaX`, und `sigmaY` eine Textzeichenfolge mit einem Schlagschatteneffekt angezeigt. Die XAML-Datei instanziiert vier `Slider` Ansichten, um diese Werte festgelegt:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Effects.DropShadowExperimentPage"
             Title="Drop Shadow Experiment">
    <ContentPage.Resources>
        <Style TargetType="Slider">
            <Setter Property="Margin" Value="10, 0" />
        </Style>

        <Style TargetType="Label">
            <Setter Property="HorizontalTextAlignment" Value="Center" />
        </Style>
    </ContentPage.Resources>

    <StackLayout>
        <skia:SKCanvasView x:Name="canvasView"
                           VerticalOptions="FillAndExpand"
                           PaintSurface="OnCanvasViewPaintSurface" />

        <Slider x:Name="dxSlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dxSlider},
                              Path=Value,
                              StringFormat='Horizontal offset = {0:F1}'}" />

        <Slider x:Name="dySlider"
                Minimum="-20"
                Maximum="20"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference dySlider},
                              Path=Value,
                              StringFormat='Vertical offset = {0:F1}'}" />

        <Slider x:Name="sigmaXSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaXSlider},
                              Path=Value,
                              StringFormat='Sigma X = {0:F1}'}" />

        <Slider x:Name="sigmaYSlider"
                Maximum="10"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference sigmaYSlider},
                              Path=Value,
                              StringFormat='Sigma Y = {0:F1}'}" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei verwendet diese Werte, um einen roten Schlagschatten auf einer blauen Text-Zeichenfolge zu erstellen:

```csharp
public partial class DropShadowExperimentPage : ContentPage
{
    const string TEXT = "Drop Shadow";

    public DropShadowExperimentPage ()
    {
        InitializeComponent ();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        // Get values from sliders
        float dx = (float)dxSlider.Value;
        float dy = (float)dySlider.Value;
        float sigmaX = (float)sigmaXSlider.Value;
        float sigmaY = (float)sigmaYSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            // Set SKPaint properties
            paint.TextSize = info.Width / 7;
            paint.Color = SKColors.Blue;
            paint.ImageFilter = SKImageFilter.CreateDropShadow(
                                    dx,
                                    dy,
                                    sigmaX,
                                    sigmaY,
                                    SKColors.Red,
                                    SKDropShadowImageFilterShadowMode.DrawShadowAndForeground);

            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            // Center the text in the display rectangle
            float xText = info.Width / 2 - textBounds.MidX;
            float yText = info.Height / 2 - textBounds.MidY;

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Hier wird das Programm ausgeführt wird:

[![Drop-Shadow Experiment](image-filters-images/DropShadowExperiment.png "Schatten Experiment löschen")](image-filters-images/DropShadowExperiment-Large.png#lightbox)

Die negativen Offset-Werte im Screenshot ganz rechts auf die universelle Windows-Plattform dazu führen, dass den Schatten oben und links neben dem Text angezeigt werden. Dies deutet darauf hin, einer Lichtquelle in der unteren rechten Ecke, die nicht die Konvention für Computergrafiken ist. Aber es scheint nicht auf keinerlei Weise falsche vielleicht daran, dass der Schatten auch sehr unscharf gemacht wird und besser als die meisten Schlagschatten waren scheint.

## <a name="lighting-effects"></a>Beleuchtungseffekten

Die `SKImageFilter` -Klasse definiert sechs Methoden, die ähnliche Namen und Parameter, die hier in der Reihenfolge zunehmender Komplexität aufgeführt:

- [`CreateDistantLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateDistantLitDiffuse*)
- [`CreateDistantLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateDistantLitSpecular*)
- [`CreatePointLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreatePointLitDiffuse*)
- [`CreatePointLitSpecular`](xref:SkiaSharp.SKImageFilter.CreatePointLitSpecular*)
- [`CreateSpotLitDiffuse`](xref:SkiaSharp.SKImageFilter.CreateSpotLitDiffuse*)
- [`CreateSpotLitSpecular`](xref:SkiaSharp.SKImageFilter.CreateSpotLitSpecular*)

Diese Methoden erstellen Bildfilter, die die Auswirkungen der verschiedenen Arten von Licht auf dreidimensionale Oberflächen zu simulieren. Zweidimensionale Objekte werden von der sich ergebende Gesamtbild Filter beleuchtet, als ob sie vorhanden war, im 3D-Raum, wodurch diese Objekte mit erhöhten Rechten oder vertiefte angezeigt werden kann, oder mit Glanzlichter.

Die `Distant` einfache Methoden wird davon ausgegangen, dass das Licht von einem äußeren Abstand stammt. Im Rahmen der leuchtenden Objekte wird das Licht angenommen, dass in einer konsistenten Richtung im 3D-Raum, ähnlich wie die Sonne auf einem kleinen Bereich der Erde. Die `Point` einfache Methoden bilden eine Glühbirne, die im 3D-Raum, die Licht in alle Richtungen ausgibt positioniert. Die `Spot` Light besitzt sowohl eine Position und eine Richtung, ähnlich wie eine Taschenlampe.

Speicherorte und erfahren Sie, wie im 3D-Raum werden mit den Werten angegeben die [ `SKPoint3` ](xref:SkiaSharp.SKPoint3) -Struktur, die ähnelt `SKPoint` jedoch mit drei Eigenschaften, die mit dem Namen `X`, `Y`, und `Z`.

Die Anzahl und Komplexität der Parameter für diese Methoden stellen Experimentieren mit ihnen schwierig. Zum Einstieg die **entfernten Licht experimentieren** Seite können Sie mit Parametern, um experimentieren die `CreateDistantLightDiffuse` Methode:

```csharp
public static SKImageFilter CreateDistantLitDiffuse (SKPoint3 direction,
                                                     SKColor lightColor,
                                                     float surfaceScale,
                                                     float kd,
                                                     SKImageFilter input = null,
                                                     SKImageFilter.CropRect cropRect = null);
```

Die Seite nicht über die letzten zwei optionale Parameter verwenden.

Drei `Slider` Ansichten in der XAML-Datei können Sie auswählen, die `Z` -Koordinate des der `SKPoint3` Wert, der `surfaceScale` Parameter, und die `kd` -Parameter, der in der API-Dokumentation als "diffuse Beleuchtung Konstante" definiert ist:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaLightExperiment.MainPage"
             Title="Distant Light Experiment">

    <StackLayout>

        <skia:SKCanvasView x:Name="canvasView"
                           PaintSurface="OnCanvasViewPaintSurface"
                           VerticalOptions="FillAndExpand" />

        <Slider x:Name="zSlider"
                Minimum="-10"
                Maximum="10"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference zSlider},
                              Path=Value,
                              StringFormat='Z = {0:F0}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="surfaceScaleSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference surfaceScaleSlider},
                              Path=Value,
                              StringFormat='Surface Scale = {0:F1}'}"
               HorizontalTextAlignment="Center" />

        <Slider x:Name="lightConstantSlider"
                Minimum="-1"
                Maximum="1"
                Margin="10, 0"
                ValueChanged="OnSliderValueChanged" />

        <Label Text="{Binding Source={x:Reference lightConstantSlider},
                              Path=Value,
                              StringFormat='Light Constant = {0:F1}'}"
               HorizontalTextAlignment="Center" />
    </StackLayout>
</ContentPage>
```

Die Code-Behind-Datei diese drei Werte abgerufen und dazu verwendet, erstellen Sie einen Image-Filter, um eine Textzeichenfolge anzuzeigen:

```csharp
public partial class DistantLightExperimentPage : ContentPage
{
    const string TEXT = "Lighting";

    public DistantLightExperimentPage()
    {
        InitializeComponent();
    }

    void OnSliderValueChanged(object sender, ValueChangedEventArgs args)
    {
        canvasView.InvalidateSurface();
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        float z = (float)zSlider.Value;
        float surfaceScale = (float)surfaceScaleSlider.Value;
        float lightConstant = (float)lightConstantSlider.Value;

        using (SKPaint paint = new SKPaint())
        {
            paint.IsAntialias = true;

            // Size text to 90% of canvas width
            paint.TextSize = 100;
            float textWidth = paint.MeasureText(TEXT);
            paint.TextSize *= 0.9f * info.Width / textWidth;

            // Find coordinates to center text
            SKRect textBounds = new SKRect();
            paint.MeasureText(TEXT, ref textBounds);

            float xText = info.Rect.MidX - textBounds.MidX;
            float yText = info.Rect.MidY - textBounds.MidY;

            // Create distant light image filter
            paint.ImageFilter = SKImageFilter.CreateDistantLitDiffuse(
                                    new SKPoint3(2, 3, z),
                                    SKColors.White,
                                    surfaceScale,
                                    lightConstant);

            canvas.DrawText(TEXT, xText, yText, paint);
        }
    }
}
```

Das erste Argument von `SKImageFilter.CreateDistantLitDiffuse` ist die Richtung des Lichts. Die positive X und Y Koordinaten anzugeben, dass das Licht zeigt nach rechts und nach unten. Positive Z-Koordinaten Punkt in die Anzeige. Die XAML-Datei können Sie negative Z-Werte, aber das ist, damit Sie sehen können, was passiert: negative Z-Koordinaten dazu vom Konzept her führen, dass das Licht aus dem Bildschirm heraus verweisen. Andere kleinere negative Werte und funktioniert für alle Elemente die Beleuchtung Auswirkungen mehr.

Die `surfaceScale` Argument kann zwischen – 1 und 1 liegen. (Höhere oder niedrigere Werte haben keine weitere Auswirkung.) Dies sind die relativen Werte in der Z-Achse, die angeben, die Verschiebung des grafischen Objekts (in diesem Fall die Textzeichenfolge) aus der Canvas-Oberfläche. Verwenden Sie negative Werte zum Auslösen der Textzeichenfolge oberhalb der Fläche im Zeichenbereich und positive Werte, die sie in den Zeichenbereich drücken.

Die `lightConstant` Wert muss positiv sein. (Das Programm kann negative Werte, damit Sie sehen, dass sie bewirken, die Auswirkungen dass mehr.) Höhere Werte führen intensiver Licht.

Diese Faktoren können abgewogen werden, um eine Geprägte erhalten wirksam, wenn `surfaceScale` ist negativ (wie bei den IOS- und Android Screenshots) und eine Gravur wirksam, wenn `surfaceScale` positiv ist, werden als mit der UWP-Screenshot auf der rechten Seite:

[![Dabei liegt weniger hellen Experiment](image-filters-images/DistantLightExperiment.png "entfernten hell Experiment")](image-filters-images/DistantLightExperiment-Large.png#lightbox)

Der Android-Screenshot verfügt über eine Z-Wert, der 0, was bedeutet, dass das Licht nur nach unten und nach rechts zeigt. Der Hintergrund ist nicht beleuchtet und die Oberfläche der Textzeichenfolge beleuchtet ist nicht beide. Das Licht wirkt sich nur der Rand des Texts für einen kleinen Effekt.

Eine alternative Methode zum Geprägte und Gravur Text wurde in diesem Artikel gezeigt [das Übersetzen transformieren](../transforms/translate.md): die Textzeichenfolge wird zweimal mit verschiedenen Farben, die ausgeglichen werden, leicht voneinander angezeigt.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
