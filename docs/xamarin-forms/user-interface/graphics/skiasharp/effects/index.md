---
title: SkiaSharp-Auswirkungen
description: Erfahren Sie, wie Sie die normale Anzeige von Grafiken mit einem Farbverlauf ändern, bitmap-Kacheln, Füllmethoden, blur- und andere Effekte.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: B3E06572-8E2A-49FA-90D1-444C394CD516
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2018
ms.openlocfilehash: 121d505d578aa20e86977c0da5d69626bbad1f53
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53049219"
---
# <a name="skiasharp-effects"></a>SkiaSharp-Auswirkungen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

Die SkiaSharp [ `SKPaint` ](xref:SkiaSharp.SKPaint) -Klasse definiert sechs Eigenschaften, die unter der allgemeine Begriff der klassifiziert werden können _Effekte_. Dies sind Eigenschaften, die die normale Anzeige von Grafiken in irgendeiner Weise zu ändern. Die Auswirkungen von SkiaSharp fallen in sechs Kategorien:

## <a name="path-effectscurveseffectsmd"></a>[Pfadeffekte](../curves/effects.md)

Legen Sie die [ `PathEffect` ](xref:SkiaSharp.SKPaint.PathEffect) Eigenschaft `SKPaint` auf ein Objekt des Typs [ `SKPathEffect` ](xref:SkiaSharp.SKPathEffect) gestrichelte Linien anzuzeigen, zu zeichnen, oder geben Sie einen Bereich mit einem Muster aus Pfaden erstellt. Die Auswirkungen der Pfad wurde weiter oben in dieser Serie im Artikel behandelten [ **Pfadeffekte in SkiaSharp**](../curves/effects.md).

## <a name="shadersshadersindexmd"></a>[Shader](shaders/index.md)

Legen Sie die [ `Shader` ](xref:SkiaSharp.SKPaint.Shader) Eigenschaft `SKPaint` auf ein Objekt des Typs [ `SKShader` ](xref:SkiaSharp.SKShader) um linearer oder zirkuläre Farbverläufe, unterteilten Bitmaps und Perlin-Noise-Muster anzuzeigen.

## <a name="blend-modesblend-modesindexmd"></a>[Füllmethoden](blend-modes/index.md)

Legen Sie die [ `BlendMode` ](xref:SkiaSharp.SKPaint.BlendMode) Eigenschaft `SKPaint` auf einen Member der [ `SKBlendMode` ](xref:SkiaSharp.SKBlendMode) Enumeration, die steuern, was geschieht, wenn eine Quellgrafik auf ein Ziel angezeigt wird. SkiaSharp unterstützt alle der CSS-Zusammensetzung und Blend Modi, wie z.B. die Porter-Duff Modi trennbare Füllmethoden einheitlich und nicht trennbare Füllmethoden einheitlich.

## <a name="mask-filtersmask-filtersmd"></a>[Mask-Filter](mask-filters.md)

Legen Sie die [ `MaskFilter` ](xref:SkiaSharp.SKPaint.MaskFilter) Eigenschaft `SKPaint` auf ein Objekt des Typs [ `SKMaskFilter` ](xref:SkiaSharp.SKMaskFilter) Weichzeichner und andere Effekte alpha.

## <a name="image-filtersimage-filtersmd"></a>[Images filtern](image-filters.md)

Legen Sie die [ `ImageFilter` ](xref:SkiaSharp.SKPaint.ImageFilter) Eigenschaft `SKPaint` auf ein Objekt vom Typ [ `SKImageFilter` ](xref:SkiaSharp.SKImageFilter) Weichzeichner Bitmaps und Erstellen von Schatten ablegen, Prägen, oder Eingravieren Auswirkungen.

## <a name="color-filterscolor-filtersmd"></a>[Filter "Farbe"](color-filters.md)

Legen Sie die [ `ColorFilter` ](xref:SkiaSharp.SKPaint.ColorFilter) Eigenschaft `SKPaint` auf ein Objekt des Typs [ `SKColorFilter` ](xref:SkiaSharp.SKColorFilter) , zu ändern, Farben mithilfe von Tabellen oder Matrix transformiert.

Alle der Beispielcode für diese Artikel enthalten, in der [ **SkiaSharpFormsDemos**](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/). Wählen Sie auf der Startseite des **SkiaSharp-Effekte**.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
