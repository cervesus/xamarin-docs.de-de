---
title: SkiaSharp Transformationen
description: In diesem Artikel wird erklärt, Transformationen für die Anzeige von Grafiken SkiaSharp in Xamarin.Forms Anwendungen, und dies mit Beispielcode veranschaulicht.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: E9BE322E-ECB3-4395-AFE4-4474A0F25551
author: charlespetzold
ms.author: chape
ms.date: 03/10/2017
ms.openlocfilehash: d8e9c26df9286cec94562b5d3d7b7721cc3f3f3d
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35244947"
---
# <a name="skiasharp-transforms"></a>SkiaSharp Transformationen

_Erfahren Sie mehr über die Transformationen für die Anzeige von Grafiken SkiaSharp_

SkiaSharp unterstützt herkömmliche Graphics-Transformationen, die als Methoden implementiert werden die [ `SKCanvas` ](https://developer.xamarin.com/api/type/SkiaSharp.SKCanvas/) Objekt. Transformationen mathematisch, zu ändern, die Koordinaten und die Größe an die von Ihnen in `SKCanvas` zeichnen Funktionen, wie Sie grafische Objekte gerendert werden. Transformationen eignen sich oft gut für das Zeichnen von Grafiken oder Animation. Einige Techniken &mdash; z. B. Bitmaps oder Text Drehen &mdash; sind nicht möglich, ohne die Verwendung von Transformationen.

SkiaSharp Transformationen unterstützen die folgenden Vorgänge:

- *Übersetzen* UMSCHALT-Koordinaten von einem Speicherort zu einem anderen
- *Skalierung* zum Vergrößern oder verkleinern Koordinaten und Größen
- *Drehen* Koordinaten, um einen Punkt drehen
- *Neigen* zu verschiebenden koordiniert, horizontal oder vertikal so, dass ein Rechteck ein Parallelogramm wird

Diese werden als bezeichnet *affine* transformiert. Affine Transformationen parallele Linien immer beibehalten und nie verursachen eine Koordinate oder die Größe, unendlich sind. Ein Quadrat nie in etwas anderes als ein Parallelogramm umgewandelt, und ein Kreis wird in etwas anderes als eine Ellipse, die nie transformiert.

SkiaSharp unterstützt auch nicht affine Transformationen (so genannte *perspektivische* oder *Perspektive* transformiert) basierend auf einem standard 3 x 3-Transformationsmatrix. Eine nicht affinen Transformation ermöglicht ein Quadrat in jeder konvexe Quadrat (vier Spitzen Abbildung mit allen innere Winkel kleiner als 180 Grad) transformiert werden sollen. Nicht affine Transformationen können bewirken, Koordinaten oder Größen, unendlich sind, aber sie sind wichtig für 3D-Effekte.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Unterschiede zwischen SkiaSharp und Xamarin.Forms-Transformationen

Xamarin.Forms unterstützt auch die Transformationen, die in SkiaSharp vergleichbar sind. Der Xamarin.Forms [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Klasse definiert die folgenden Transformationseigenschaften:

- `TranslationX` und `TranslationY`
- `Scale`
- `Rotation`, `RotationX` und `RotationY`

Die `RotationX` und `RotationY` Eigenschaften sind Perspektive Transformationen, die eine 3D Auswirkungen zu erstellen.

Es gibt einige wichtige Unterschiede zwischen SkiaSharp Transformationen und Xamarin.Forms Transformationen:

Ein Unterschied besteht darin, dass SkiaSharp Transformationen, auf die gesamte angewendet werden `SKCanvas` Objekt beim Anwenden der Transformationen Xamarin.Forms auf einzelne `VisualElement` ableitungen. (Können Sie das Xamarin.Forms Transformationen anwenden der `SKCanvasView` Objekt selbst, da der `SKCanvasView` leitet sich von `VisualElement`, aber innerhalb der `SKCanvasView`, Anwenden der Transformationen SkiaSkarp.)

Die SkiaSharp Transformationen sind relativ zur linken oberen Ecke von der `SKCanvas` während relativ zur linken oberen Ecke von Xamarin.Forms Transformationen sind die `VisualElement` auf die sie angewendet werden. Dieser Unterschied ist wichtig, beim Anwenden der Skalierung und Drehung transformiert werden, da diese Transformationen immer relativ zu einem bestimmten Punkt sind.

Die sehr große Unterschied ist, dass SKiaSharp Transformationen sind *Methoden* dagegen die Transformationen Xamarin.Forms *Eigenschaften*. Dies ist ein semantisches Unterschied hinter der syntaktische Unterschied: SkiaSharp Transformationen einen Vorgang ausführen, während Xamarin.Forms Transformationen einen Status festgelegt. SkiaSharp Transformationen anwenden, anschließend gezeichneten Grafikobjekten, nicht jedoch Graphics-Objekten, die gezeichnet werden, bevor die Transformation angewendet wird. Eine Transformation Xamarin.Forms gilt hingegen eine zuvor gerenderten Elements als die Eigenschaft festgelegt ist. SkiaSharp Transformationen sind kumulativ, wie die Methoden aufgerufen werden. Xamarin.Forms Transformationen werden ersetzt, wenn die Eigenschaft mit einem anderen Wert festgelegt ist.

Die Beispielprogramme in diesem Abschnitt angezeigt wird, unter der Überschrift **transformiert** auf der Startseite von der [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) Programm, und klicken Sie in der [ **Transformiert** ](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) Ordner der Projektmappe.

## <a name="the-translate-transformtranslatemd"></a>[Die Verschiebungstransformation](translate.md)

Erfahren Sie, wie die übersetzen-Transformation verwenden, um SkiaSharp Grafiken zu verschieben.

## <a name="the-scale-transformscalemd"></a>[Die Skalierungstransformation](scale.md)

Ermitteln Sie die SkiaSharp Skalierungstransformation für Objekte in verschiedenen Größen skalieren.

## <a name="the-rotate-transformrotatemd"></a>[Die Drehungstransformation](rotate.md)

Untersuchen Sie die Auswirkung und Animationen, die mit der SkiaSharp Rotationstransformation möglich.

## <a name="the-skew-transformskewmd"></a>[Die Scherungstransformation](skew.md)

Finden Sie, wie die Neigung Transformation Geneigter Grafikobjekte in SkiaSharp erstellen kann.

## <a name="matrix-transformsmatrixmd"></a>[Matrixtransformationen](matrix.md)

Eingehendere Untersuchung SkiaSharp Transformationen mit vielseitigen Transformationsmatrix.

## <a name="touch-manipulationstouchmd"></a>[Manipulationen durch Toucheingaben](touch.md)

Matrixtransformationen um Touch Manipulationen für ziehen, Skalierung und Drehung zu implementieren.

## <a name="non-affine-transformsnon-affinemd"></a>[Nicht affine Transformationen](non-affine.md)

Die Oridinary mit nicht affinen Transformation Effekte werden hinausgehen.

## <a name="3d-rotation3d-rotationmd"></a>[3D-Drehung](3d-rotation.md)

Verwenden Sie nicht affine Transformationen 2D Objekte im 3D-Raum drehen.


## <a name="related-links"></a>Verwandte Links

- [SkiaSharp-APIs](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
