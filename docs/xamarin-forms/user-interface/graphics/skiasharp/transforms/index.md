---
title: ''
description: In diesem Artikel werden Transformationen zum Anzeigen von skiasharp Xamarin.Forms -Grafiken in-Anwendungen erläutert und mit Beispielcode veranschaulicht.
ms.prod: ''
ms.technology: ''
ms.assetid: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e20ea5d1d3f813b04a927601fbe1180ff39ed176
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/28/2020
ms.locfileid: "84140191"
---
# <a name="skiasharp-transforms"></a>SkiaSharp-Transformationen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Weitere Informationen zu Transformationen zum Anzeigen von skiasharp-Grafiken_

Skiasharp unterstützt herkömmliche Grafik Transformationen, die als Methoden des Objekts implementiert werden [`SKCanvas`](xref:SkiaSharp.SKCanvas) . Mathematisch ändern Transformationen die Koordinaten und Größen, die Sie in `SKCanvas` Zeichnungsfunktionen angeben, wenn die grafischen Objekte gerendert werden. Transformationen sind häufig zum Zeichnen von wiederkehrenden Grafiken oder für Animationen geeignet. Einige Techniken, &mdash; z. b. das Drehen von Bitmaps oder Text, &mdash; sind ohne die Verwendung von Transformationen nicht möglich.

Skiasharp-Transformationen unterstützen die folgenden Vorgänge:

- *In* Verschiebungs Koordinaten von einem Speicherort zu einem anderen verschieben
- *Skalieren* , um Koordinaten und Größen zu vergrößern oder zu verringern
- *Drehen* , um Koordinaten um einen Punkt zu drehen
- Neigen Sie dazu, die Koordinaten horizontal oder vertikal zu *verschieben, sodass* ein Rechteck zu einem Parallelogramm wird.

Diese werden als *affine* Transformationen bezeichnet. Affine Transformationen behalten immer parallele Linien bei und bewirken nie, dass eine Koordinate oder eine Größe unendlich wird. Ein Quadrat wird nie in etwas anderes als ein Parallelogramm transformiert, und ein Kreis wird nie in etwas anderes als eine Ellipse transformiert.

Skiasharp unterstützt auch nicht affine Transformationen (auch als *Projective* oder *Perspektiven* Transformationen bezeichnet), die auf einer standardmäßigen 3-by-3-Transformationsmatrix basieren. Eine nicht affine Transformation ermöglicht das Transformieren eines Quadrats in eine beliebige beliebige, vierseitige Abbildung, bei der alle inneren Winkel kleiner als 180 Grad sind. Nicht affine Transformationen können dazu führen, dass Koordinaten oder Größen unendlich werden, aber Sie sind wichtig für 3D-Effekte.

## <a name="differences-between-skiasharp-and-xamarinforms-transforms"></a>Unterschiede zwischen skiasharp und Xamarin.Forms Transformationen

Xamarin.Formsunterstützt auch Transformationen, die denen in skiasharp ähneln. Die- Xamarin.Forms [`VisualElement`](xref:Xamarin.Forms.VisualElement) Klasse definiert die folgenden Transformations Eigenschaften:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)immer[`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation), [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX) und[`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

Die `RotationX` `RotationY` Eigenschaften und sind Perspektiven Transformationen, die quasi 3D-Effekte erstellen.

Es gibt mehrere wichtige Unterschiede zwischen skiasharp-Transformationen und Xamarin.Forms Transformationen:

Der erste Unterschied besteht darin, dass skiasharp-Transformationen auf das gesamte Objekt angewendet werden, `SKCanvas` während die Xamarin.Forms Transformationen auf einzelne `VisualElement` Ableitungen angewendet werden. (Sie können die Xamarin.Forms Transformationen auf das `SKCanvasView` Objekt selbst anwenden, da `SKCanvasView` von abgeleitet ist, `VisualElement` aber innerhalb dieses `SKCanvasView` s werden die skiaskarp-Transformationen angewendet.)

Die skiasharp-Transformationen sind relativ zur oberen linken Ecke der, `SKCanvas` während Xamarin.Forms Transformationen relativ zur oberen linken Ecke der sind `VisualElement` , auf die Sie angewendet werden. Dieser Unterschied ist wichtig, wenn Skalierungs-und Rotations Transformationen angewendet werden, da diese Transformationen immer relativ zu einem bestimmten Punkt sind.

Der wirklich große Unterschied besteht darin, dass skiasharp-Transformationen *Methoden* sind, während die Xamarin.Forms Transformationen *Eigenschaften*sind. Dies ist ein semantischer Unterschied über den syntaktischen Unterschied: skiasharp-Transformationen führen einen Vorgang aus, während Xamarin.Forms Transformationen einen Zustand festlegen. Skiasharp-Transformationen gelten für nachträglich gezeichnete Grafik Objekte, aber nicht für Grafik Objekte, die vor dem Anwenden der Transformation gezeichnet werden. Im Gegensatz dazu Xamarin.Forms gilt eine Transformation für ein zuvor gerendertes Element, sobald die Eigenschaft festgelegt ist. Skiasharp-Transformationen sind kumulativ, wenn die Methoden aufgerufen werden. Xamarin.FormsTransformationen werden ersetzt, wenn die Eigenschaft mit einem anderen Wert festgelegt wird.

Alle Beispiel Programme in diesem Abschnitt werden im skiasharp [**formsdemos**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) -Programm im Abschnitt **skiasharp-Transformationen** angezeigt. Quellcode finden Sie im Ordner [**Transformationen**](https://github.com/xamarin/xamarin-forms-samples/tree/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Transforms) der Projekt Mappe.

## <a name="the-translate-transform"></a>[Die Verschiebungstransformation](translate.md)

Erfahren Sie, wie Sie die Transform-Transformation verwenden, um skiasharp-Grafiken zu verschieben.

## <a name="the-scale-transform"></a>[Die Skalierungstransformation](scale.md)

Entdecken Sie die skiasharp-Skalierungs Transformation zum Skalieren von Objekten auf verschiedene Größen.

## <a name="the-rotate-transform"></a>[Die Drehungstransformation](rotate.md)

Erkunden Sie die möglichen Auswirkungen und Animationen mit der skiasharp-Transformation zum drehen.

## <a name="the-skew-transform"></a>[Die Scherungstransformation](skew.md)

Sehen Sie, wie die Schiefe Transformation ein gekipptes grafisches Objekt erstellen kann.

## <a name="matrix-transforms"></a>[Matrixtransformationen](matrix.md)

Vertiefen Sie sich in skiasharp-Transformationen mit der vielseitigen Transformationsmatrix.

## <a name="touch-manipulations"></a>[Manipulationen durch Toucheingaben](touch.md)

Verwenden Sie Matrix Transformationen, um Berührungs Manipulationen zum ziehen, skalieren und drehen zu implementieren.

## <a name="non-affine-transforms"></a>[Nicht affine Transformationen](non-affine.md)

Gehen Sie über den oridinary hinaus mit nicht affinen Transformations Effekten.

## <a name="3d-rotation"></a>[3D-Drehung](3d-rotation.md)

Verwenden Sie nicht-affine Transformationen, um 2D-Objekte im 3D-Raum zu drehen.

## <a name="related-links"></a>Verwandte Links

- [Skiasharp-APIs](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Skiasharpformsdemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
