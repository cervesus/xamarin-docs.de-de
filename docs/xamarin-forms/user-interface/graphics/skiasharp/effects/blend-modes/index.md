---
title: Skiasharp-Blend-Modi
description: Verwenden Sie Blend-Modi, um zu definieren, was geschieht, wenn grafische Objekte aufeinander gestapelt werden.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: CE1B222E-A2D0-4016-A532-EC1E59EE3D6B
author: davidbritch
ms.author: dabritch
ms.date: 08/23/2018
ms.openlocfilehash: 829d764f03dd77c6126c2f4bced750ae570a3bc6
ms.sourcegitcommit: bc0c1740aa0708459729c0e671ab3ff7de3e2eee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/15/2020
ms.locfileid: "83425696"
---
# <a name="skiasharp-blend-modes"></a>Skiasharp-Blend-Modi

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Diese Artikel konzentrieren sich auf die- [`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode) Eigenschaft von [`SKPaint`](xref:SkiaSharp.SKPaint) . Die- `BlendMode` Eigenschaft ist vom Typ [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) , eine Enumeration mit 29 Membern.

Die- `BlendMode` Eigenschaft bestimmt, was geschieht, wenn ein grafisches Objekt (das häufig als _Quelle_bezeichnet wird) oberhalb vorhandener grafischer Objekte (als _Ziel_bezeichnet) gerendert wird. Normalerweise wird erwartet, dass das neue grafische Objekt die darunter liegenden Objekte verdeckt. Dies geschieht jedoch nur, weil der standardmäßige Blend-Modus ist `SKBlendMode.SrcOver` , was bedeutet, dass die Quelle _über_ das Ziel gezeichnet wird. Die anderen 28 Member von `SKBlendMode` verursachen andere Effekte. Bei der Grafik Programmierung wird die Technik zum kombinieren grafischer Objekte auf verschiedene Weise als _Zusammensetzung_bezeichnet.

## <a name="the-skblendmodes-enumeration"></a>Die skblendmodes-Enumeration

Die skiasharp-Blend-Modi entsprechen den Funktionen, die in der Spezifikation der W3C-Zusammensetzung [**und-Blending der Ebene 1**](https://www.w3.org/TR/compositing-1/) beschrieben sind. In der [**Übersicht über Skia skblendmode**](https://skia.org/user/api/SkBlendMode_Overview) finden Sie auch hilfreiche Hintergrundinformationen. Für eine allgemeine Einführung in die Blend-Modi ist der Artikel [**Blend-Modi**](https://en.wikipedia.org/wiki/Blend_modes) in Wikipedia ein guter Ausgangswert. Blend-Modi werden in Adobe Photoshop unterstützt. es gibt also viele zusätzliche Online Informationen zu Blend-Modi in diesem Kontext.

Die 29 Member der- `SKBlendMode` Enumeration können in drei Kategorien unterteilt werden:

| Porter-Duff | Trennbarer    | Nicht trennbar |
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

Die Namen dieser drei Kategorien sind in den nachfolgenden Diskussionen besser gemeint. Die Reihenfolge, in der die Member aufgeführt sind, entspricht der Definition der- `SKBlendMode` Enumeration. Die 13 Enumerationsmember in der ersten Spalte weisen die ganzzahligen Werte 0 bis 12 auf. Die zweite Spalte sind Enumerationsmember, die den ganzen Zahlen 13 bis 24 entsprechen, und die Elemente in der dritten Spalte haben die Werte 25 bis 28.

Diese Blend-Modi werden in _etwa_ der gleichen Reihenfolge im Dokument mit dem W3C **-Kompositions-und Mischungsgrad 1** erläutert. es gibt jedoch einige Unterschiede: der `Src` Modus wird als " _Kopieren_ " im W3C-Dokument bezeichnet und `Plus` als _heller_bezeichnet. Das W3C-Dokument definiert einen _normalen_ Blend-Modus, der in nicht enthalten ist, `SKBlendModes` da er mit identisch ist `SrcOver` . Der `Modulate` Blend-Modus (oben in der ersten Spalte) ist nicht im W3C-Dokument enthalten, und es wird eine Erörterung des- `Multiply` Modus vorausgeht `Screen` .

Da der `Modulate` Blend-Modus für Skia eindeutig ist, wird er als zusätzlicher Porter-Duff-Modus und als trennbarer Modus behandelt.

## <a name="the-importance-of-transparency"></a>Die Wichtigkeit der Transparenz

In der Vergangenheit wurde die Zusammensetzung in Verbindung mit dem Konzept des _Alphakanals_entwickelt. In einer Anzeige Oberfläche, wie z. b. dem `SKCanvas` -Objekt und einer vollfarbliche Bitmap, besteht jedes Pixel aus 4 Bytes: 1 Byte für die roten, grünen und blauen Komponenten und ein zusätzliches Byte für Transparenz. Diese Alpha Komponente ist 0 für vollständige Transparenz und 0xFF für vollständige Deckkraft, mit unterschiedlichen Transparenz Ebenen zwischen diesen Werten.

Viele der Blend-Modi basieren auf Transparenz. Wenn ein `SKCanvas` erstmalig in einem Handler abgerufen wird `PaintSurface` oder wenn ein `SKCanvas` erstellt wird, um auf eine Bitmap zu zeichnen, ist der erste Schritt der folgende:

```csharp
canvas.Clear();
```

Diese Methode ersetzt alle Pixel der Canvas durch transparente schwarze Pixel, `new SKColor(0, 0, 0, 0)` die der Ganzzahl 0x00000000 entsprechen. Alle Bytes aller Pixel werden mit 0 (null) initialisiert.

Die Zeichnungs Oberfläche eines `SKCanvas` , das in einem Handler abgerufen wird `PaintSurface` , scheint einen weißen Hintergrund zu haben, aber das ist nur, da der `SKCanvasView` selbst über einen transparenten Hintergrund verfügt und die Seite einen weißen Hintergrund hat. Sie können diese Tatsache selbst veranschaulichen, indem Sie die xamarin. Forms- `BackgroundColor` Eigenschaft von `SKCanvasView` auf eine xamarin. Forms-Farbe festlegen:

```csharp
canvasView.BackgroundColor = Color.Red;
```

Oder in einer Klasse, die von abgeleitet `ContentPage` ist, können Sie die Seitenhintergrund Farbe festlegen:

```csharp
BackgroundColor = Color.Red;
```

Sie sehen diesen roten Hintergrund hinter ihrer skiasharp-Grafik, da der skiasharp-Zeichenbereich selbst transparent ist.

Der Artikel [**skiasharp-Transparenz**](../../basics/transparency.md) zeigte einige grundlegende Techniken bei der Verwendung von Transparenz zum Anordnen mehrerer Grafiken in einem zusammengesetzten Image. Die Blend-Modi gehen darüber hinaus, aber Transparenz bleibt für die Blend-Modi entscheidend.

## <a name="skiasharp-porter-duff-blend-modes"></a>[Skiasharp-Porter-Duff-Blend-Modi](porter-duff.md)

Verwenden Sie die Blend-Modi "Porter-Duff", um Szenen basierend auf Quell-und Ziel Images zu verfassen.

## <a name="skiasharp-separable-blend-modes"></a>[Skiasharp-Trenn Bare Blend-Modi](separable.md)

Verwenden Sie die trennbaren Blend-Modi, um die Farben rot, grün und blau zu ändern.

## <a name="skiasharp-non-separable-blend-modes"></a>[Nicht trennbare Blend-Modi skiasharp](non-separable.md)

Verwenden Sie die nicht trennbaren Blend-Modi, um Farbton, Sättigung oder Helligkeit zu ändern.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
