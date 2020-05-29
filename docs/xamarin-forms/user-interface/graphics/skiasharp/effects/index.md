---
title: ''
description: ''
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: d9fa710f5dfc61c2892b8fc409a39b37cf449018
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136304"
---
# <a name="skiasharp-effects"></a>Skiasharp-Effekte

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

Die skiasharp- [`SKPaint`](xref:SkiaSharp.SKPaint) Klasse definiert sechs Eigenschaften, die unter allgemeiner Auswirkung klassifiziert werden können _effects_. Dabei handelt es sich um Eigenschaften, die die normale Anzeige von Grafiken auf irgendeine Weise ändern. Die skiasharp-Effekte fallen in sechs Kategorien:

## <a name="path-effects"></a>[Pfadeffekte](../curves/effects.md)

Legen Sie die- [`PathEffect`](xref:SkiaSharp.SKPaint.PathEffect) Eigenschaft von `SKPaint` auf ein Objekt vom Typ fest, [`SKPathEffect`](xref:SkiaSharp.SKPathEffect) um gestrichelte Linien anzuzeigen, oder, um einen Bereich mit einem Muster zu zeichnen oder auszufüllen, das aus Pfaden erstellt wurde. Der Pfad Effekt wurde weiter oben in dieser Reihe in den Artikeln [**Pfad Effekte in skiasharp**](../curves/effects.md)behandelt.

## <a name="shaders"></a>[Shader](shaders/index.md)

Legen Sie die- [`Shader`](xref:SkiaSharp.SKPaint.Shader) Eigenschaft von `SKPaint` auf ein Objekt vom Typ fest, [`SKShader`](xref:SkiaSharp.SKShader) um lineare oder zirkuläre Farbverläufe, gekachelte Bitmaps und Perlin-Rauschmuster anzuzeigen.

## <a name="blend-modes"></a>[Füllmethoden](blend-modes/index.md)

Legen Sie die- [`BlendMode`](xref:SkiaSharp.SKPaint.BlendMode) Eigenschaft von `SKPaint` auf einen Member der- [`SKBlendMode`](xref:SkiaSharp.SKBlendMode) Enumeration fest, um zu steuern, was geschieht, wenn eine Quell Grafik an einem Ziel angezeigt wird. Skiasharp unterstützt alle CSS-Zusammensetzung-und Blend-Modi, einschließlich der Porter-Duff-Modi, Trenn Bare Blend-Modi und nicht getrennte Blend-Modi.

## <a name="mask-filters"></a>[Maskenfilter](mask-filters.md)

Legen [`MaskFilter`](xref:SkiaSharp.SKPaint.MaskFilter) Sie die-Eigenschaft von `SKPaint` auf ein Objekt vom Typ [`SKMaskFilter`](xref:SkiaSharp.SKMaskFilter) für verwischt und andere Alpha Effekte fest.

## <a name="image-filters"></a>[Image-Filter](image-filters.md)

Legen Sie die- [`ImageFilter`](xref:SkiaSharp.SKPaint.ImageFilter) Eigenschaft von `SKPaint` auf ein Objekt vom Typ [`SKImageFilter`](xref:SkiaSharp.SKImageFilter) für die Unschärfe von Bitmaps und das Erstellen von Drop Shadows-, Embossing-oder Gravur-Effekten fest.

## <a name="color-filters"></a>[Farbfilter](color-filters.md)

Legen Sie die- [`ColorFilter`](xref:SkiaSharp.SKPaint.ColorFilter) Eigenschaft von `SKPaint` auf ein Objekt vom Typ fest [`SKColorFilter`](xref:SkiaSharp.SKColorFilter) , um Farben mithilfe von Tabellen oder Matrix Transformationen zu ändern.

Der gesamte Beispielcode für diese Artikel ist in [**skiasharpformsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)zu finden. Wählen Sie auf der Startseite die Option **skiasharp-Effekte**aus.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
