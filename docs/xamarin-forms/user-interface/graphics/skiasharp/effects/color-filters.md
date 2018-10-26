---
title: SkiaSharp-Color-Filter
description: Verwenden Sie Filter "Farbe", um Farben mit Transformationen oder Tabellen konvertieren.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 774E7B55-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/28/2018
ms.openlocfilehash: fafc9db542d804745dbd0eb0fb73db47068ea465
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50131751"
---
# <a name="skiasharp-color-filters"></a>SkiaSharp-Color-Filter

Filter "Farbe" können die Farben in eine Bitmap (oder anderen Bilds) auf andere Farben Effekte wie z. B. keine Farbsprünge auftreten übersetzen:

![Beispiel der Filter "Farbe"](color-filters-images/ColorFiltersExample.png "Farbe Filter-Beispiel")

Legen Sie zum Verwenden eines Filters für die Farbe der [ `ColorFilter` ](xref:SkiaSharp.SKPaint.ColorFilter) Eigenschaft `SKPaint` auf ein Objekt des Typs [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter) von einer der statischen Methoden in dieser Klasse erstellt. Dieser Artikel veranschaulicht: 

- eine Transformation Farbe erstellt, mit der [ `CreateColorMatrix` ](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) Methode.
- eine Farbe-Tabelle erstellt, mit der [ `CreateTable` ](xref:SkiaSharp.SKColorFilter.CreateTable*) Methode.

## <a name="the-color-transform"></a>Die Farbe-Transformation

Die Farbe Transformation umfasst, verwenden eine Matrix zum Ändern von Farben. Wie die meisten Systeme für 2D-Grafiken, verwendet die SkiaSharp Matrizen hauptsächlich für das Transformieren von Koordinaten als Iscussed in diesem Artikel [ **Matrix transformiert in SkiaSharp**](../transforms/matrix.md). Die [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter) unterstützt auch Matrixtransformationen, aber die Matrixtransformationen RGB-Farben. Einige Kenntnisse im Umgang mit den Konzepten der Matrix ist erforderlich, diese Farbe Transformationen zu verstehen. 

Die Farbe Transformationsmatrix verfügt über eine Dimension mit vier Zeilen und fünf Spalten:

<pre>
| M11 M12 M13 M14 M15 |
| M21 M22 M23 M24 M25 |
| M31 M32 M33 M34 M35 |
| M41 M42 M43 M44 M45 |
</pre>

Es wandelt eine Quelle RGB-Farbe (R, G, B, A) mit der Zielfarbe (R ", G', B","). 

Als Vorbereitung für die Matrixmultiplikation wird die Quellfarbe zu einer Matrix 5 × 1 konvertiert:

<pre>
| R |
| G |
| B |
| A |
| 1 |
</pre>

Diese Werte R, G, B und A sind die ursprünglichen, im Bereich von 0 bis 255 Bytes. Sie sind _nicht_ normalisiert, um Gleitkommawerte im Bereich von 0 bis 1.

Die zusätzliche Zelle ist erforderlich, damit ein Übersetzungsfaktor. Dies ist vergleichbar mit der Verwendung einer 3 × 3-Matrix um zweidimensionalen Koordinatenpunkte zu transformieren, wie im Abschnitt beschrieben [ **der Grund für die 3-Mal-3-Matrix** ](../transforms/matrix.md#the-reason-for-the-3-by-3-matrix) im Artikel zur Verwendung von Matrizen für das Transformieren Koordinieren Sie Punkte.

Die 4 x 5-Matrix wird die Matrix 5 × 1 multipliziert, und das Produkt ist eine 4 x 1-Matrix mit der transformierten Farbe:

<pre>
| M11 M12 M13 M14 M15 |    | R |   | R' |
| M21 M22 M23 M24 M25 |    | G |   | G' |
| M31 M32 M33 M34 M35 |  × | B | = | B' |
| M41 M42 M43 M44 M45 |    | A |   | A' |
                           | 1 |
</pre>

Hier sind die separaten Formeln für R ", G', 'B und A':

R' = M11· R + M12· G + M13· B + M14· A + M15 

G' = M21· R + M22· G + M23· B + M24· A + M25 

B = M31· R + M32· G + M33· B + M34· A + M35 

EIN "M41· = R + M42· G + M43· B + M44· A + M45 

Die meisten der Matrix besteht aus multiplikative Faktoren, die in der Regel im Bereich von 0 bis 2 liegen. Jedoch die letzte Spalte (M15 über M45) enthält Werte, die in den Formeln hinzugefügt werden. Diese Werte reichen in der Regel von 0 bis 255. Die Ergebnisse sind die Werte von 0 bis 255 gebunden sind.

Die Identitätsmatrix ist:

<pre>
| 1 0 0 0 0 |
| 0 1 0 0 0 |
| 0 0 1 0 0 |
| 0 0 0 1 0 |
</pre>

Dies bewirkt, dass keine Änderung an der Farben. Die Transformation-Formeln sind:

R' = R 

G' G =

B = B

EIN "= A

Die Zelle M44 ist sehr wichtig, da er die Deckkraft beibehält. Es ist in der Regel der Fall, dass M41 M42 und M43 werden alle 0 (null), basieren auf den Werten roten, grünen und blauen-Opacity ' wahrscheinlich nicht möchten, dass. Aber M44 ist 0 (null), und klicken Sie dann auf ein "wird auf NULL gesetzt, und" nothing "werden angezeigt.

Einer der häufigsten Verwendungsmöglichkeiten für die Farbmatrix ist eine Farbbitmap in eine Bitmap Graustufen konvertiert. Dies umfasst eine Formel für einen gewichteten Durchschnitt der Werte Rot, Grün und Blau. Für video anzeigen, die mit dem Farbraum sRGB ("standardmäßige Rot Grün blue") wird diese Formel:

Grau-Shade 0.2126· = R + 0.7152· G + 0.0722· B

Um eine Farbbitmap in Graustufen Bitmap, die R konvertieren ", G', und B Ergebnisse müssen alle entspricht dieser Wert. Die Matrix ist:

<pre>
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0    0    0    1 0 |
</pre>

Es ist kein SkiaSharp-Datentyp, der mit dieser Matrix entspricht. Stattdessen müssen Sie die Matrix darstellen, als ein Array von 20 `float` Werte in der Reihenfolge der Zeilen: erste Zeile und zweiten Zeile und So weiter.

Die statische [ `SKColorFilter.CreateColorMatrix` ](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) Methode hat die folgende Syntax:

```csharp
public static SKColorFilter CreateColorMatrix (float[] matrix);
```

wo `matrix` ist ein Array von der 20 `float` Werte. Beim Erstellen des Arrays C#, es ist einfach, die Zahlen zu formatieren, damit sie die 4 x 5-Matrix ähneln. Dies wird veranschaulicht, der **Graustufen Matrix** auf der Seite die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel:

```csharp
public class GrayScaleMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(CenteredTilesPage),
                        "SkiaSharpFormsDemos.Media.Banana.jpg");

    public GrayScaleMatrixPage()
    {
        Title = "Gray-Scale Matrix";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0.21f, 0.72f, 0.07f, 0, 0,
                    0,     0,     0,     1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

Die `DrawBitmap` Methode verwendet, die in diesem Code stammt aus der **BitmapExtension.cs** enthaltene Datei die [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Beispiel. 

Hier ist das Ergebnis auf iOS-, Android- und universelle Windows-Plattform ausgeführt wird:

[![Graustufen Matrix](color-filters-images/GrayScaleMatrix.png "Graustufen-Matrix")](color-filters-images/GrayScaleMatrix-Large.png#lightbox)

Achten Sie auf den Wert in der vierten Zeile und vierten Spalte. Dies ist der entscheidende Faktor, der mit dem Wert der ursprünglichen Farbe multipliziert wird, für die ein "Wert der transformierten Farbe. Wenn diese Zelle 0 (null) ist, wird nichts angezeigt, und das Problem ist möglicherweise schwer zu lokalisieren.

Wenn Sie mit Farbmatrizen experimentieren möchten, können Sie der Transformation entweder aus der Perspektive der Quelle oder der Perspektive des Ziels behandeln. Wie soll das rote Pixel von der Quelle den Rot-, Grün- und Blau-Pixeln, des Ziels beitragen? Richtet sich nach den Werten in der ersten _Spalte_ der Matrix. Alternativ sollten wie das roten Zielpixel die roten, grünen und blauen Pixel von der Quelle auswirkt? Richtet sich nach der ersten _Zeile_ der Matrix.

Einige Ideen zur Verwendung von Farbe Transformationen finden Sie unter den [ **Neufärben von Bildern** ](https://docs.microsoft.com/dotnet/framework/winforms/advanced/recoloring-images) Seiten. Die Diskussion bezieht sich auf die Windows Forms, und die Matrix ist ein anderes Format, aber die Konzepte sind identisch.

Die **Pastell Matrix** rechnet ferner damit das roten Zielpixel gedämpft das rote Quelle Pixel und etwas Betonung der roten und grünen Pixel. Dieser Vorgang erfolgt auf ähnliche Weise für die grünen und blauen Pixel:

```csharp
public class PastelMatrixPage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PastelMatrixPage),
                        "SkiaSharpFormsDemos.Media.MountainClimbers.jpg");

    public PastelMatrixPage()
    {
        Title = "Pastel Matrix";

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateColorMatrix(new float[]
                {
                    0.75f, 0.25f, 0.25f, 0, 0,
                    0.25f, 0.75f, 0.25f, 0, 0,
                    0.25f, 0.25f, 0.75f, 0, 0,
                    0, 0, 0, 1, 0
                });

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

Das Ergebnis ist die Intensität der Farben stumm geschaltet, wie Sie hier sehen können:

[![Pastell Matrix](color-filters-images/PastelMatrix.png "Pastell Matrix")](color-filters-images/PastelMatrix-Large.png#lightbox)

## <a name="color-tables"></a>Farbtabellen

Die statische [ `SKColorFilter.CreateTable` ](xref:SkiaSharp.SKColorFilter.CreateTable*) Methode steht in zwei Versionen:

```csharp
public static SKColorFilter CreateTable (byte[] table);

public static SKColorFilter CreateTable (byte[] tableA, byte[] tableR, byte[] tableG, byte[] tableB);
```

Die Arrays enthalten immer 256 Einträgen. In der `CreateTable` Methode mit einer Tabelle, die gleiche Tabelle wird verwendet, für die Komponenten roten, grünen und blauen. Ist eine einfache Nachschlagetabelle: Wenn Quellfarbe (R, G, B), und die Zielfarbe darstellt (R ", B", "G"), und klicken Sie dann die Zielkomponenten, von der Indizierung abgerufen werden `table` mit der Source-Komponenten:

R' = die Tabelle [R]

G' = die Tabelle [G]

B = Table [B]

Bei der zweiten Methode kann jedes der vier Komponenten der Farbe eine separaten Farbtabelle aufweisen, oder die gleichen Farbe Tabellen zwischen zwei oder mehr Komponenten gemeinsam verwendet werden können.

Wenn Sie eines der Argumente für die zweite festlegen möchten `CreateTable` Methode, um eine Farbe-Tabelle mit den Werten von 0 bis 255 nacheinander, können Sie `null` stattdessen. Sehr häufig die `CreateTable` Aufruf hat einen `null` erste Argument für den alpha-Kanal.

Im Abschnitt zu **keine Farbsprünge auftreten** in diesem Artikel auf [Pixelbits für den Zugriff auf SkiaSharp-Bitmap](../bitmaps/pixel-bits.md#posterization), Sie haben gesehen, wie so ändern Sie die einzelnen Pixelbits einer Bitmap, dessen Farbe Auflösung zu reduzieren. Dies ist eine Technik namens _keine Farbsprünge auftreten_. 

Sie können auch eine Bitmap mit einer Farbtabelle feste Framerate. Der Konstruktor der der **feste Framerate Tabelle** Seite erstellt eine Farbe-Tabelle, der dessen Index, in ein Byte am unteren 6 Bits auf 0 (null) festgelegt zugeordnet:

```csharp
public class PosterizeTablePage : ContentPage
{
    SKBitmap bitmap = BitmapExtensions.LoadBitmapResource(
                        typeof(PosterizeTablePage),
                        "SkiaSharpFormsDemos.Media.MonkeyFace.png");

    byte[] colorTable = new byte[256];

    public PosterizeTablePage()
    {
        Title = "Posterize Table";

        // Create color table
        for (int i = 0; i < 256; i++)
        {
            colorTable[i] = (byte)(0xC0 & i);
        }

        SKCanvasView canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }

    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        using (SKPaint paint = new SKPaint())
        {
            paint.ColorFilter =
                SKColorFilter.CreateTable(null, null, colorTable, colorTable);

            canvas.DrawBitmap(bitmap, info.Rect, BitmapStretch.Uniform, paint: paint);
        }
    }
}
```

Das Programm entscheidet sich für diese Farbe-Tabelle nur für den grünen und blauen-Kanälen verwendet. Rote Kanal weiterhin voller Auflösung:

[![Trennen Sie die Tabelle](color-filters-images/PosterizeTable.png "feste Framerate, Tabelle")](color-filters-images/PosterizeTable-Large.png#lightbox)

Sie können verschiedene Farbtabellen für die verschiedene Farbkanäle für verschiedene Effekte verwenden. 

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
