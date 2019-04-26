---
title: SkiaSharp Füllmethoden einheitlich
description: Verwenden Mischmodi definieren, was geschieht, wenn Sie grafische Objekte auf anderen gestapelt sind.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CE1B222E-A2D0-4016-A532-EC1E59EE3D6B
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 3ea05563ecbca95d26d692d5424c30e961229ac5
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61021197"
---
# <a name="skiasharp-blend-modes"></a>SkiaSharp Füllmethoden einheitlich

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Diese Artikel konzentrieren sich auf die [ `BlendMode` ](xref:SkiaSharp.SKPaint.BlendMode) Eigenschaft [ `SKPaint` ](xref:SkiaSharp.SKPaint). Die `BlendMode` Eigenschaft ist vom Typ [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode), eine Enumeration mit Membern von 29.

Die `BlendMode` Eigenschaft bestimmt, was passiert, wenn eines grafischen Objekts (wird häufig aufgerufen, die _Quelle_) zusätzlich zu vorhandenen Grafikobjekte gerendert wird (wird aufgerufen, die _Ziel_). In der Regel erwarten wir uns das neue grafische-Objekt, die Objekte, darunter zu verschleiern. Aber dies geschieht nur, weil der Standardmodus für Blend ist `SKBlendMode.SrcOver`, was bedeutet, dass die Quelle wird _über_ das Ziel. Die anderen 28 Mitglieder des `SKBlendMode` dazu führen, dass andere Effekte. In der Grafikprogrammierung wird das Verfahren zum Kombinieren von grafischen Objekten auf verschiedene Weise als bezeichnet _Zusammensetzung_.

## <a name="the-skblendmodes-enumeration"></a>Die SKBlendModes-enumeration

Die Blend-Modi von SkiaSharp entsprechen eng jenen in der W3C [ **zusammensetzt und auf Ebene1 Blending** ](https://www.w3.org/TR/compositing-1/) Spezifikation. Die Skia [ **SkBlendMode Verweis** ](https://skia.org/user/api/SkBlendMode_Reference) auch bietet hilfreiche Informationen. Für eine allgemeine Einführung in blend-Modi, die [ **Füllmethoden** ](https://en.wikipedia.org/wiki/Blend_modes) Artikel in Wikipedia ist ein guter Anfang. Blend-Modi werden in Adobe Photoshop unterstützt, es gibt also viele zusätzliche Onlineinformationen zu Füllmethoden einheitlich in diesem Kontext.

Die 29 Mitglieder der `SKBlendMode` Enumeration kann in drei Kategorien unterteilt werden:

| Porter-Duff | Trennbare    | Nicht trennbare |
| ----------- | ------------ | ------------- |
| `Clear`     | `Modulate`   | `Hue`         |
| `Src`       | `Screen`     | `Saturation`  |
| `Dst`       | `Overlay`    | `Color`       |
| `SrcOver`   | `Darken`     | `Luminosity`  |
| `DstOver`   | `Lighten`    |               |
| `SrcIn`     | `ColorDodge` |               |
| `DstIn`     | `ColorBurn`  |               |
| `SrcOut`    | `HardLight`  |               |
| `DstOut`    | `SoftLight`  |               |
| `SrcATop`   | `Difference` |               |
| `DstATop`   | `Exclusion`  |               |
| `Xor`       | `Multiply`   |               |
| `Plus`      |              |               |

Die Namen dieser drei Kategorien dauert mehr Bedeutung in den Diskussionen bemerkt, die folgen. Die Reihenfolge, in der hier aufgeführten Elemente ist der gleiche wie in der Definition der `SKBlendMode` Enumeration. In der ersten Spalte der 13 Enumerationsmember haben die ganzzahligen Werte 0 bis 12. Die zweite Spalte sind Enumerationsmember, die zu einer ganzen Zahl 13 bis 24 entsprechen, und die Elemente in der dritten Spalte haben Werte 25, 28.

Diese Modi finden Sie im Blend _ungefähr_ der gleichen Reihenfolge, in der W3C **zusammensetzt und auf Ebene1 Blending** Dokument, es gibt jedoch einige Unterschiede: Die `Src` Modus hat die Bezeichnung _Kopie_ im W3C-Dokument und `Plus` heißt _heller_. Das W3C-Dokument definiert eine _Normal_ Blendingmodus, der nicht in enthalten ist `SKBlendModes` da wäre es identisch `SrcOver`. Die `Modulate` Blend-Modus (am oberen Rand der ersten Spalte) nicht in der W3C-Dokument und eine Erläuterung der enthalten die `Multiply` Modus steht `Screen`.

Da die `Modulate` Blend-Modus für Skia eindeutig ist, werden als eine zusätzliche Porter-Duff-Modus und einem trennbare Modus behandelt werden.

## <a name="the-importance-of-transparency"></a>Die Bedeutung von Transparenz

In der Vergangenheit wurde Zusammensetzung entwickelt, in Verbindung mit dem Konzept der _alpha-Kanal_. In der Anzeige einer Oberfläche wie z. B. die `SKCanvas` Objekt oder eine Bitmap für die Farbe, jedes Pixels 4 Bytes besteht aus: 1 Byte jeder für die Komponenten roten, grünen und blauen und einer zusätzlichen Byte für Transparenz. Diese alpha-Komponente ist 0 für vollständige Transparenz und 0xFF für vollständige Deckkraft mit unterschiedlichen Transparenz zwischen diesen Werten.

Viele der Füllmethoden einheitlich basieren auf der Transparenz. In der Regel, wenn ein `SKCanvas` wird zuerst abgerufen, eine `PaintSurface` Handler auf, oder wenn ein `SKCanvas` wird erstellt, um eine Bitmap zu zeichnen, ist der erste Schritt dieses Aufrufs:

```csharp
canvas.Clear();
```

Diese Methode ersetzt alle Pixel des Zeichnungsbereichs durch transparente schwarzen Pixel entspricht `new SKColor(0, 0, 0, 0)` oder die ganze Zahl 0 x 00000000. Alle Bytes aller Pixel werden mit 0 (null) initialisiert.

Die Zeichenoberfläche des ein `SKCanvas` abgerufen, die einem `PaintSurface` Handler scheinen einen weißen Hintergrund aufweist, aber das ist nur, weil die `SKCanvasView` selbst über einen transparenten Hintergrund verfügt, und die Seite verfügt über einen weißen Hintergrund. Sie können dies an sich selbst darstellen, durch Festlegen der Xamarin.Forms `BackgroundColor` Eigenschaft `SKCanvasView` zu einer Xamarin.Forms-Farbe:

```csharp
canvasView.BackgroundColor = Color.Red;
```

Oder in eine abgeleitete Klasse `ContentPage`, Sie können die Hintergrundfarbe festlegen:

```csharp
BackgroundColor = Color.Red;
```

Sie sehen, dass dieser roten Hintergrund hinter von SkiaSharp-Grafiken, da die SkiaSharp canvas selbst transparent ist.

Der Artikel [ **SkiaSharp Transparenz** ](../../basics/transparency.md) einige der grundlegenden Techniken mit Transparenz zum Anordnen von mehreren Grafiken in ein zusammengesetztes Bild gezeigt. Die Blend-Modi wechseln, darüber hinaus, aber Transparenz bleibt die Blend-Modi von entscheidender Bedeutung. 

## <a name="skiasharp-porter-duff-blend-modesporter-duffmd"></a>[SkiaSharp Porter-Duff Füllmethoden einheitlich](porter-duff.md)

Verwenden Sie die Porter-Duff Blend-Modi, um im Hintergrund basierend auf Quelle und Ziel-Images zu erstellen.

## <a name="skiasharp-separable-blend-modesseparablemd"></a>[SkiaSharp trennbare Füllmethoden einheitlich](separable.md)

Verwenden Sie die trennbare Füllmethoden einheitlich, um die Farben rote, grüne und blaue zu ändern.

## <a name="skiasharp-non-separable-blend-modesnon-separablemd"></a>[SkiaSharp nicht trennbare Füllmethoden einheitlich](non-separable.md)

Verwenden Sie die nicht trennbare Füllmethoden einheitlich Farbton, Sättigung und Helligkeit zu ändern.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
