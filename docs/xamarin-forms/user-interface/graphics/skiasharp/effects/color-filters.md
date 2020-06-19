---
title: Skiasharp-Farbfilter
description: Verwenden Sie Farbfilter, um Farben mit Transformationen oder Tabellen zu konvertieren.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 774E7B55-AEC8-4F12-B657-1C0CEE01AD63
author: davidbritch
ms.author: dabritch
ms.date: 08/28/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b9c89d4d426884d678e77687ffa226cced97be58
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136382"
---
# <a name="skiasharp-color-filters"></a>Skiasharp-Farbfilter

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Farbfilter können Farben in einer Bitmap (oder einem anderen Bild) in andere Farben für Effekte, wie z. b. die Abwahl, übersetzen:

![Farbfilter Beispiel](color-filters-images/ColorFiltersExample.png "Farbfilter Beispiel")

Wenn Sie einen Farbfilter verwenden möchten, legen Sie die- [`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) Eigenschaft von `SKPaint` auf ein Objekt des Typs fest, der [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) von einer der statischen Methoden in dieser Klasse erstellt wurde. In diesem Artikel wird Folgendes veranschaulicht: 

- eine Farb Transformation, die mit der-Methode erstellt wurde [`CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) .
- eine mit der-Methode erstellte Farbtabelle [`CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) .

## <a name="the-color-transform"></a>Die Farb Transformation

Die Farb Transformation umfasst die Verwendung einer Matrix zum Ändern von Farben. Wie die meisten 2D-Grafik Systeme verwendet skiasharp Matrizen, die hauptsächlich zum Transformieren von Koordinaten Punkten in den Artikel [**Matrix Transformationen in skiasharp**](../transforms/matrix.md)verwendet werden. [`SKColorFilter`](xref:SkiaSharp.SKColorFilter)Unterstützt auch Matrix Transformationen, aber die Matrix transformiert RGB-Farben. Zum Verständnis dieser Farb Transformationen ist eine gewisse Vertrautheit mit Matrix Konzepten erforderlich. 

Die Farb Transformationsmatrix weist eine Dimension von vier Zeilen und fünf Spalten auf:

<pre>
| M11 M12 M13 M14 M15 |
| M21 M22 M23 M24 M25 |
| M31 M32 M33 M34 M35 |
| M41 M42 M43 M44 M45 |
</pre>

Sie wandelt eine RGB-Quellfarbe (r, G, B, a) in die Zielfarbe um (r ', g ', B ', a '). 

Zur Vorbereitung der Matrix Multiplikation wird die Quellfarbe in eine 5 × 1-Matrix konvertiert:

<pre>
| R |
| G |
| B |
| A |
| 1 |
</pre>

Diese R-, G-, B-und a-Werte sind die ursprünglichen Bytes im Bereich von 0 bis 255. Sie werden _nicht_ zu Gleit Komma Werten im Bereich von 0 bis 1 normalisiert.

Die zusätzliche Zelle ist für einen Übersetzungs Faktor erforderlich. Dies entspricht der Verwendung einer 3 × 3-Matrix zum Transformieren von zweidimensionalen Koordinaten Punkten, wie im Abschnitt [**Grund für die 3-x-3-Matrix**](../transforms/matrix.md#the-reason-for-the-3-by-3-matrix) im Artikel zum Verwenden von Matrizen zum Transformieren von Koordinaten Punkten beschrieben.

Die 4 × 5-Matrix wird mit der 5 × 1-Matrix multipliziert, und das Produkt ist eine 4 × 1-Matrix mit der transformierten Farbe:

<pre>
| M11 M12 M13 M14 M15 |    | R |   | R' |
| M21 M22 M23 M24 M25 |    | G |   | G' |
| M31 M32 M33 M34 M35 |  × | B | = | B' |
| M41 M42 M43 M44 M45 |    | A |   | A' |
                           | 1 |
</pre>

Im folgenden finden Sie die separaten Formeln für R ', G ', B ' und einen ':

`R' = M11·R + M12·G + M13·B + M14·A + M15` 

`G' = M21·R + M22·G + M23·B + M24·A + M25` 

`B' = M31·R + M32·G + M33·B + M34·A + M35` 

`A' = M41·R + M42·G + M43·B + M44·A + M45` 

Der größte Teil der Matrix besteht aus Multiplikationsfaktoren, die sich in der Regel im Bereich von 0 bis 2 befinden. Die letzte Spalte (M15 bis M45) enthält jedoch Werte, die in den Formeln hinzugefügt werden. Diese Werte liegen im Allgemeinen zwischen 0 und 255. Die Ergebnisse werden zwischen den Werten 0 und 255 geklammert.

Die Identitätsmatrix lautet:

<pre>
| 1 0 0 0 0 |
| 0 1 0 0 0 |
| 0 0 1 0 0 |
| 0 0 0 1 0 |
</pre>

Dies bewirkt keine Änderung der Farben. Die transformationsformeln lauten wie folgt:

`R' = R` 

`G' = G`

`B' = B`

`A' = A`

Die M44-Zelle ist sehr wichtig, da Sie die Deckkraft beibehält. Im Allgemeinen ist die Groß-/Kleinschreibung von M41, M42 und M43 gleich NULL, da Sie wahrscheinlich nicht möchten, dass die Deckkraft auf den roten, grünen und blauen Werten basiert. Wenn M44 jedoch 0 (null) ist, ist ein "0", und es wird nichts angezeigt.

Eine der häufigsten Verwendungsmöglichkeiten der Farbmatrix ist das Konvertieren einer Farb Bitmap in eine Graustufen Bitmap. Dies umfasst eine Formel für einen gewichteten Durchschnitt der roten, grünen und blauen Werte. Für Video-anzeigen, das den sRGB-Farbraum ("Standard rotes grün blau") verwendet, lautet diese Formel:

Grauer Farbton = 0,2126 R + 0,7152 G + 0,0722 B

Um eine Farb Bitmap in eine Graustufen Bitmap zu konvertieren, müssen die Ergebnisse der R-, G-und B-Ergebnisse denselben Wert haben. Die Matrix lautet:

<pre>
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0.21 0.72 0.07 0 0 |
| 0    0    0    1 0 |
</pre>

Es gibt keinen skiasharp-Datentyp, der dieser Matrix entspricht. Stattdessen müssen Sie die Matrix als Array von 20 `float` Werten in Zeilen Reihenfolge (erste Zeile, zweite Zeile usw.) darstellen.

Die statische- [`SKColorFilter.CreateColorMatrix`](xref:SkiaSharp.SKColorFilter.CreateColorMatrix*) Methode weist die folgende Syntax auf:

```csharp
public static SKColorFilter CreateColorMatrix (float[] matrix);
```

, wobei `matrix` ein Array der 20 `float` Werte ist. Wenn Sie das Array in c# erstellen, ist es einfach, die Zahlen so zu formatieren, dass Sie der 4 × 5-Matrix ähneln. Dies wird im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel auf der Seite " **graue Skalierungs Matrix** " veranschaulicht:

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

Die `DrawBitmap` in diesem Code verwendete Methode basiert auf der **BitmapExtension.cs** -Datei, die im [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Beispiel enthalten ist. 

Dies ist das Ergebnis, das unter IOS, Android und universelle Windows-Plattform ausgeführt wird:

[![Graustufen Matrix](color-filters-images/GrayScaleMatrix.png "Graustufen Matrix")](color-filters-images/GrayScaleMatrix-Large.png#lightbox)

Sehen Sie sich den Wert in der vierten Zeile und vierten Spalte an. Dies ist der entscheidende Faktor, der mit dem Wert der ursprünglichen Farbe für den Wert der transformierten Farbe multipliziert wird. Wenn diese Zelle 0 (null) ist, wird nichts angezeigt, und das Problem ist möglicherweise schwer zu finden.

Beim Experimentieren mit Farb Matrizen können Sie die Transformation entweder aus der Perspektive der Quelle oder der Perspektive des Ziels behandeln. Wie soll das rote Pixel der Quelle zu den roten, grünen und blauen Pixeln des Ziels beitragen? Dies wird durch die Werte in der ersten _Spalte_ der Matrix bestimmt. Oder wie soll das rote, grüne und blaue Pixel der Quelle von der roten, grünen und blauen Pixel beeinflusst werden? Dies wird durch die erste _Zeile_ der Matrix bestimmt.

Einige Ideen zur Verwendung von Farb Transformationen finden Sie auf den Seiten zum Neusortieren von [**Bildern**](https://docs.microsoft.com/dotnet/framework/winforms/advanced/recoloring-images) . Die Diskussion betrifft Windows Forms, und die Matrix weist ein anderes Format auf, aber die Konzepte sind identisch.

Die **Pastell Matrix** berechnet das rote Pixel des Ziels durch die Dämpfung des roten Quell Pixels und die Betonung der roten und grünen Pixel. Dieser Vorgang erfolgt auf ähnliche Weise für die grünen und blauen Pixel:

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

Das Ergebnis ist das stumm schalten der Intensität der Farben, wie Sie hier sehen können:

[![Pastell Matrix](color-filters-images/PastelMatrix.png "Pastell Matrix")](color-filters-images/PastelMatrix-Large.png#lightbox)

## <a name="color-tables"></a>Farbtabellen

Die statische- [`SKColorFilter.CreateTable`](xref:SkiaSharp.SKColorFilter.CreateTable*) Methode ist in zwei Versionen verfügbar:

```csharp
public static SKColorFilter CreateTable (byte[] table);

public static SKColorFilter CreateTable (byte[] tableA, byte[] tableR, byte[] tableG, byte[] tableB);
```

Die Arrays enthalten immer 256 Einträge. In der `CreateTable` -Methode mit einer Tabelle wird die gleiche Tabelle für die roten, grünen und blauen Komponenten verwendet. Es handelt sich um eine einfache Nachschlage Tabelle: Wenn die Quellfarbe (r, G, b) und die Zielfarbe (r ', b ', G ') ist, werden die Ziel Komponenten durch Indizierung `table` mit den Quell Komponenten abgerufen:

`R' = table[R]`

`G' = table[G]`

`B' = table[B]`

In der zweiten Methode kann jede der vier Farbkomponenten eine separate Farbtabelle aufweisen, oder die gleichen Farbtabellen können von zwei oder mehr Komponenten gemeinsam genutzt werden.

Wenn Sie eines der Argumente für die zweite `CreateTable` Methode auf eine Farbtabelle festlegen möchten, die die Werte 0 bis 255 nacheinander enthält, können Sie `null` stattdessen verwenden. Sehr häufig hat der-Befehl `CreateTable` ein `null` erstes Argument für den Alphakanal.

Im Abschnitt zur nach Wahl im **Artikel über das** zugreifen auf [skiasharp-Bitmap-Pixel Bits](../bitmaps/pixel-bits.md#posterization)haben Sie erfahren, wie die einzelnen Pixel Bits einer Bitmap geändert werden, um die Farbauflösung zu verringern. Dies ist eine Methode namens " _posterization_". 

Sie können eine Bitmap auch mit einer Farbtabelle verkleinern. Der Konstruktor der Tabelle mit der untersten **Tabelle** erstellt eine Farbtabelle, die den Index einem Byte zuordnet, bei dem die untersten 6 Bits auf 0 (null) festgelegt sind:

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

Das Programm wählt diese Farbtabelle nur für die grünen und blauen Kanäle aus. Die vollständige Auflösung des roten Kanals wird weiterhin durchgeführt:

[![Tabelle nach unten](color-filters-images/PosterizeTable.png "Tabelle nach unten")](color-filters-images/PosterizeTable-Large.png#lightbox)

Sie können verschiedene Farbtabellen für die verschiedenen Farbkanäle für verschiedene Effekte verwenden. 

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
