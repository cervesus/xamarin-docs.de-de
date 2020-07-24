---
title: 'Xamarin.FormsFormen: Pfad'
description: Die Xamarin.Forms Path-Klasse kann zum Zeichnen von Kurven und komplexen Formen verwendet werden.
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8e23e4151c4841dd4dce80ba0358471c64a26f39
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937608"
---
# <a name="xamarinforms-shapes-path"></a>Xamarin.FormsFormen: Pfad

![Vorabversion-API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Path` -Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Kurven und komplexen Formen verwendet werden. Diese Kurven und Formen werden oft mithilfe von `Geometry` Objekten beschrieben. Informationen zu den Eigenschaften, die die `Path` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Path` definiert die folgenden Eigenschaften:

- `Data`vom Typ `Geometry` , der die zu zeichende Form angibt.
- `RenderTransform`vom Typ `Transform` , der die Transformation darstellt, die auf die Geometrie eines Pfads angewendet wird, bevor Sie gezeichnet wird.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Weitere Informationen zu Transformationen finden Sie unter [ Xamarin.Forms Pfad Transformationen](path-transforms.md).

## <a name="create-a-path"></a>Erstellen eines Pfads

Um einen Pfad zu zeichnen, erstellen Sie ein `Path` -Objekt, und legen Sie dessen- `Data` Eigenschaft fest Es gibt zwei Verfahren zum Festlegen der- `Data` Eigenschaft:

- Sie können einen Zeichen folgen Wert für `Data` in XAML festlegen, indem Sie die Pfad Markup Syntax verwenden. Bei diesem Ansatz beansprucht der `Path.Data` Wert ein Serialisierungsformat für Grafiken. Normalerweise bearbeiten Sie diesen Zeichen folgen Wert nicht manuell nach der Erstellung. Stattdessen verwenden Sie Entwurfs Tools, um die Daten zu bearbeiten, und exportieren Sie als Zeichen folgen Fragment, das von der-Eigenschaft verwendet werden kann `Data` .
- Sie können die- `Data` Eigenschaft auf ein- `Geometry` Objekt festlegen. Hierbei kann es sich um ein bestimmtes- `Geometry` Objekt oder ein-Objekt handeln, das `GeometryGroup` als Container fungiert, der mehrere Geometry-Objekte in einem einzelnen-Objekt kombinieren kann.

### <a name="create-a-path-with-path-markup-syntax"></a>Erstellen eines Pfads mit der Pfad Markup Syntax

Das folgende XAML-Beispiel zeigt, wie Sie ein Dreieck mithilfe der Pfad Markup Syntax zeichnen:

```xaml
<Path Data="M 10,100 L 100,100 100,50Z"
      Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Start" />
```

Die `Data` Zeichenfolge beginnt mit dem Move-Befehl, der durch angegeben wird, `M` wodurch ein absoluter Ausgangspunkt für den Pfad festgelegt wird. `L`ist der Line-Befehl, der eine gerade Linie vom Startpunkt bis zum angegebenen Endpunkt erstellt. `Z`ist der Close-Befehl, mit dem eine Zeile erstellt wird, die den aktuellen Punkt mit dem Ausgangspunkt verbindet. Das Ergebnis ist ein Dreieck:

![Pfad Dreieck](path-images/triangle.png "Pfad Dreieck")

> [!NOTE]
> Die Pfad Markup Syntax ist nur in XAML verfügbar.

Weitere Informationen zur Pfad Markup Syntax finden Sie unter [ Xamarin.Forms Pfad Markup Syntax](path-markup-syntax.md).

### <a name="create-a-path-with-geometry-objects"></a>Erstellen eines Pfads mit Geometry-Objekten

Kurven und Formen können mithilfe von- `Geometry` Objekten beschrieben werden, die zum Festlegen der `Path` -Eigenschaft des Objekts verwendet werden `Data` . Es gibt eine Vielzahl von `Geometry` Objekten, aus denen Sie auswählen können. Die `EllipseGeometry` `LineGeometry` Klassen, und `RectangleGeometry` beschreiben relativ einfache Formen. Um komplexere Formen zu erstellen oder Kurven zu erstellen, verwenden Sie eine `PathGeometry` .

`PathGeometry`Objekte bestehen aus einem oder mehreren- `PathFigure` Objekten. Jedes- `PathFigure` Objekt stellt eine andere Form dar. Jedes-Objekt besteht aus `PathFigure` einem oder mehreren- `PathSegment` Objekten, die jeweils einen Verbindungsteil der Form darstellen. Segment Typen umfassen die folgenden `LineSegment` Klassen, `BezierSegment` und `ArcSegment` .

Das folgende XAML-Beispiel zeigt, wie Sie ein Dreieck mithilfe eines- `PathGeometry` Objekts zeichnen:

```xaml
<Path Stroke="Black"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure IsClosed="True"
                                StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="100,100" />
                                <LineSegment Point="100,50" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel ist der Startpunkt des Dreiecks (10.100). Ein Liniensegment wird von (10.100) bis (100.100) und von (100.100) bis (100, 50) gezeichnet. Anschließend werden die Abbildungen erste und letzte Segmente verbunden, da die- `PathFigure.IsClosed` Eigenschaft auf festgelegt ist `true` . Das Ergebnis ist ein Dreieck:

![Pfad Dreieck](path-images/triangle.png "Pfad Dreieck")

Weitere Informationen zu Geometrien finden Sie unter [ Xamarin.Forms Geometries](geometries.md).

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
- [Xamarin.FormsGeometrien](geometries.md)
- [Xamarin.FormsPfad Markup Syntax](path-markup-syntax.md)
- [Xamarin.FormsPfad Transformationen](path-transforms.md)
