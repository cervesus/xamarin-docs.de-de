---
title: SkiaSharp-Transformationen
description: In diesem Artikel untersucht die Transformationen für die Anzeige von Grafiken von SkiaSharp in Xamarin.Forms-Anwendungen und wird dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: d9c153f8da44c525b8851afb48682bd7a14a8c47
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70770502"
---
# <a name="skiasharp-transforms"></a>SkiaSharp-Transformationen

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Erfahren Sie mehr über die Transformationen für die Anzeige von SkiaSharp-Grafiken_

SkiaSharp unterstützt herkömmlichen grafiktransformationen, die als Methoden implementiert werden die [ `SKCanvas` ](xref:SkiaSharp.SKCanvas) Objekt. Transformationen aus mathematischer Sicht zu ändern, die Koordinaten und Größen, die Sie, in angeben `SKCanvas` zeichnen Funktionen wie die grafische Objekte gerendert werden. Transformationen sind häufig praktisch, für das Zeichnen von Grafiken oder Animationen. Einige Techniken &mdash; wie Drehen, Bitmaps oder Text &mdash; sind nicht möglich, ohne die Verwendung von Transformationen.

SkiaSharp-Transformationen unterstützen die folgenden Vorgänge:

- *Übersetzen* in UMSCHALT-Koordinaten von einem Speicherort in einen anderen
- *Skalierung* erhöhen oder verringern, Koordinaten und Größen
- *Drehen Sie* um Koordinaten aus, um einen Punkt zu drehen.
- *Neigen* koordiniert die horizontal oder vertikal verschoben, damit ein Rechteck ein Parallelogramm wird

Diese werden als bezeichnet *affine* transformiert. Affine Transformationen immer erhalten parallele Linien und niemals dazu führen, dass eine Koordinate oder die Größe unendlich sind. Ein Quadrat nie in etwas anderes als ein Parallelogramm transformiert wird, und ein Kreis wird nie in etwas anderes als eine Ellipse transformiert.

SkiaSharp unterstützt auch nicht affine Transformationen (so genannte *perspektivische* oder *Perspektive* transformiert) basierend auf einer standard-3-Mal-3-Transformationsmatrix. Eine nicht affine Transformation ermöglicht ein Quadrat in jeder konvexe Viereck darauf hin, transformiert werden, wird eine Abbildung vier Seiten mit inneren Winkel kleiner als 180 Grad. Nicht affine Transformationen können dazu führen, dass Koordinaten oder Größen, unendlich sind, aber sie sind wichtig für das 3D-Effekte.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Unterschiede zwischen SkiaSharp und Xamarin.Forms-Transformationen

Xamarin.Forms unterstützt auch die Transformationen, die in SkiaSharp ähneln. Die Xamarin.Forms [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) -Klasse definiert die folgenden Transformationseigenschaften:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation), [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX), und [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

Die `RotationX` und `RotationY` Eigenschaften sind Perspektive-Transformationen, die eine-3D-Effekte zu erstellen.

Es gibt mehrere wichtige Unterschiede zwischen SkiaSharp-Transformationen und Xamarin.Forms-Transformationen:

Ein Unterschied besteht darin, dass SkiaSharp-Transformationen, auf die gesamte angewendet werden `SKCanvas` -Objekt, während die Xamarin.Forms-Transformationen, um einzelne angewendet werden `VisualElement` ableitungen. (Können Sie die Xamarin.Forms-Transformationen, Anwenden der `SKCanvasView` Objekt selbst, da der `SKCanvasView` leitet sich von `VisualElement`, aber innerhalb der `SKCanvasView`, SkiaSkarp Transformationen anwenden.)

SkiaSharp-Transformationen sind relativ zur oberen linken Ecke des der `SKCanvas` Xamarin.Forms Transformationen zwar relativ zur oberen linken Ecke des der `VisualElement` auf die sie angewendet werden. Dieser Unterschied ist wichtig, beim Anwenden von Skalierung und Drehung transformiert werden, da diese Transformationen immer relativ zu einem bestimmten Punkt enthalten sind.

Der wirklich große Unterschied besteht darin, dass SKiaSharp-Transformationen sind *Methoden* während der Xamarin.Forms-Transformationen sind *Eigenschaften*. Dies ist ein semantischer Unterschied über den syntaktischen Unterschied: Skiasharp-Transformationen führen einen Vorgang aus, während xamarin. Forms-Transformationen einen Zustand festlegen. SkiaSharp-Transformationen anwenden auf nachfolgend gezeichneten Grafikobjekten, jedoch nicht zu Grafikobjekten, die gezeichnet werden, bevor die Transformation angewendet wird. Im Gegensatz dazu wird eine Xamarin.Forms-Transformation auf eine zuvor gerenderte-Element angewendet, als die Eigenschaft festgelegt ist. SkiaSharp-Transformationen sind kumulativ, wie die Methoden aufgerufen werden. Xamarin.Forms-Transformationen werden ersetzt, wenn die Eigenschaft durch einen anderen Wert festgelegt ist.

Die Beispielprogramme in diesem Abschnitt angezeigt, der **SkiaSharp-Transformationen** Teil der [ **SkiaSharpFormsDemos** ](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) Programm. Quellcode finden Sie der [ **transformiert** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) Ordner der Projektmappe.

## <a name="the-translate-transformtranslatemd"></a>[Die Verschiebungstransformation](translate.md)

Erfahren Sie, wie Sie mit, dass die Verschiebungstransformation um SkiaSharp-Grafiken zu verschieben.

## <a name="the-scale-transformscalemd"></a>[Die Skalierungstransformation](scale.md)

Entdecken Sie die Skalierungstransformation SkiaSharp, für die Skalierung von Objekten in verschiedenen Größen.

## <a name="the-rotate-transformrotatemd"></a>[Die Drehungstransformation](rotate.md)

Untersuchen Sie die Effekten und Animationen, die mit der die Drehungstransformation SkiaSharp möglich.

## <a name="the-skew-transformskewmd"></a>[Die Scherungstransformation](skew.md)

Sehen Sie, wie die Scherungstransformation Geneigter grafischen Objekten erstellen kann.

## <a name="matrix-transformsmatrixmd"></a>[Matrixtransformationen](matrix.md)

Eingehendere Untersuchung SkiaSharp-Transformationen mit der vielseitige Transformationsmatrix.

## <a name="touch-manipulationstouchmd"></a>[Manipulationen durch Toucheingaben](touch.md)

Verwenden von Matrixtransformationen zum Implementieren der Touch-Bearbeitungen für ziehen, Skalierung und Drehung.

## <a name="non-affine-transformsnon-affinemd"></a>[Nicht affine Transformationen](non-affine.md)

Wechseln Sie über die Oridinary nicht affine Transformation Auswirkungen.

## <a name="3d-rotation3d-rotationmd"></a>[3D-Drehung](3d-rotation.md)

Verwenden Sie nicht affine Transformationen, um 2D-Objekte im 3D-Raum zu drehen.

## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
