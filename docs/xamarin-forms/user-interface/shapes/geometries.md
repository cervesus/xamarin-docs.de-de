---
title: 'Xamarin.FormsFormen: Geometrien'
description: Xamarin.FormsGeometry-Klassen ermöglichen es Ihnen, die Geometrie einer 2D-Form zu beschreiben.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: c3b869d10d454453172065b30eb7ce32da81c8ce
ms.sourcegitcommit: 7fc658bbdcb8130cd9d611e55e79a1830fc5d5a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133034"
---
# <a name="xamarinforms-shapes-geometries"></a>Xamarin.FormsFormen: Geometrien

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Geometry` -Klasse und die Klassen, die davon abgeleitet werden, ermöglichen es Ihnen, die Geometrie einer 2D-Form zu beschreiben. `Geometry`Objekte können einfach sein, z. b. Rechtecke und Kreise oder zusammengesetzte, die aus zwei oder mehr Geometrie Objekten erstellt werden. Komplexere Geometrien können mithilfe der-Klasse erstellt werden `PathGeometry` , die es Ihnen ermöglicht, Bögen und Kurven zu beschreiben.

Die `Geometry` `Shape` Klassen und scheinen ähnlich, da Sie beide 2D-Formen beschreiben, aber einen wichtigen Unterschied aufweisen. Die-Klasse `Geometry` wird von der- `BindableObject` Klasse abgeleitet, während die- `Shape` Klasse von der-Klasse abgeleitet wird `View` . Daher `Shape` können-Objekte sich selbst Rendering und am Layoutsystem teilnehmen, während-Objekte dies nicht möglich sind `Geometry` . Obwohl- `Shape` Objekte leichter verwendbar sind als- `Geometry` Objekte, `Geometry` sind-Objekte vielseitiger. Während ein- `Shape` Objekt zum rendering2d-Grafiken verwendet wird, `Geometry` kann ein-Objekt verwendet werden, um den geometrischen Bereich für 2D-Grafiken zu definieren und einen Bereich für das Abschneiden zu definieren.

Die Basisklasse für alle Geometrien ist die abstrakte Klasse `Geometry` . Die Klassen, die von dieser Klasse abgeleitet werden, können in drei Kategorien unterteilt werden: einfache Geometrien, Pfadgeometrien und zusammengesetzte Geometrien.

## <a name="simple-geometries"></a>Einfache Geometrien

Die einfachen Geometry-Klassen sind `EllipseGeometry` , `LineGeometry` und `RectangleGeometry` . Sie werden verwendet, um grundlegende geometrische Formen zu erstellen, z. b. Kreise, Linien und Rechtecke. Diese Formen und komplexere Formen können mithilfe von oder erstellt werden, `PathGeometry` indem Geometry-Objekte kombiniert werden, aber diese Klassen bieten eine einfachere Möglichkeit zum Erstellen dieser grundlegenden geometrischen Formen.

### <a name="ellipsegeometry"></a>EllipseGeometry

Eine Ellipse-Geometrie stellt die Geometrie oder eine Ellipse oder einen Kreis dar und wird durch einen Mittelpunkt, einen x-Radius und einen y-Radius definiert.

Die- `EllipseGeometry` Klasse definiert die folgenden Eigenschaften:

- `Center`vom Typ `Point` , der den Mittelpunkt der Geometrie darstellt.
- `RadiusX`vom Typ `double` , der den x-Radius-Wert der Geometrie darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `RadiusY`vom Typ `double` , der den y-Radius-Wert der Geometrie darstellt. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Im folgenden Beispiel wird gezeigt, wie ein erstellt und dargestellt wird `EllipseGeometry` :

```xaml
<Path Fill="Blue"
      Stroke="Red"
      StrokeThickness="1">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

In diesem Beispiel ist der Mittelpunkt von `EllipseGeometry` auf (50, 50) festgelegt, und der x-Radius-und der y-Radius sind auf 50 festgelegt. Dadurch wird ein Kreis mit einem Durchmesser von 100 erstellt, dessen innere blau gezeichnet ist.

### <a name="linegeometry"></a>LineGeometry

Eine Linien Geometrie stellt die Geometrie einer Linie dar und wird durch Angabe des Anfangs Punkts der Linie und des Endpunkts definiert.

Die- `LimeGeometry` Klasse definiert die folgenden Eigenschaften:

- `StartPoint`vom Typ `Point` , der den Anfangspunkt der Linie darstellt.
- `EndPoint`vom Typ `Point` , der den Endpunkt der Zeile darstellt.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Im folgenden Beispiel wird gezeigt, wie ein erstellt und dargestellt wird `LineGeometry` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

In diesem Beispiel wird ein `LineGeometry` von (10, 20) bis (100.130) gezeichnet.

### <a name="rectanglegeometry"></a>RectangleGeometry

Eine Rechteck Geometrie stellt ein Rechteck dar und wird mit einer- `Rect` Struktur definiert, die ihre relative Position und ihre Höhe und Breite angibt.

Die- `RectangleGeometry` Klasse definiert die folgenden Eigenschaften:

- `Rect`vom Typ `FormsRect` , der die Abmessungen des Rechtecks darstellt.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Im folgenden Beispiel wird gezeigt, wie ein erstellt und dargestellt wird `RectangleGeometry` :

```xaml
<Path Fill="Blue"
      Stroke="Black"
      StrokeThickness="1">
  <Path.Data>
    <RectangleGeometry Rect="50,50,25,25" />
  </Path.Data>
</Path>
```

Die Position und die Dimensionen des Rechtecks werden durch eine- `Rect` Struktur definiert. In diesem Beispiel ist die Position (50, 50), und die Höhe und die Breite sind 25, wodurch ein Quadrat entsteht.

## <a name="path-geometries"></a>Pfadgeometrien

Eine Pfad Geometrie beschreibt eine komplexe Form, die aus Arcs, Kurven, Ellipsen, Linien und Rechtecke bestehen kann.

Die- `PathGeometry` Klasse definiert die folgenden Eigenschaften:

- `Figures`vom Typ `PathFigureCollection` , der die Auflistung von-Objekten darstellt, die `PathFigure` den Pfad Inhalt beschreiben.
- `FillRule`vom Typ `FillRule` , der bestimmt, wie die sich überschneidenden Bereiche in der Geometrie kombiniert werden. Der Standardwert dieser Eigenschaft ist `FillRule.EvenOdd`.

> [!NOTE]
> Die `Figures` -Eigenschaft ist der der `ContentProperty` `PathGeometry` -Klasse und muss daher nicht explizit aus XAML festgelegt werden.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Eine `PathGeometry` besteht aus einer Auflistung von- `PathFigure` Objekten, wobei jede `PathFigure` eine Form in der Geometrie beschreibt. Jede besteht aus `PathFigure` einem oder mehreren- `PathSegment` Objekten, von denen jedes ein Segment der Form beschreibt. Es gibt viele Arten von Segmenten:

- `ArcSegment`, wodurch ein elliptischer Bogen zwischen zwei Punkten erstellt wird.
- `BezierSegment`, wodurch eine kubische Bezier-Kurve zwischen zwei Punkten erstellt wird.
- `LineSegment`, wodurch eine Linie zwischen zwei Punkten erstellt wird.
- `PolyBezierSegment`erstellt eine Reihe von kubischen Bezier-Kurven.
- `PolyLineSegment`, wodurch eine Reihe von Zeilen erstellt wird.
- `PolyQuadraticBezierSegment`, wodurch eine Reihe quadratischer Bezier-Kurven erstellt wird.
- `QuadraticBezierSegment`, wodurch eine quadratische Bezier-Kurve erstellt wird.

Die Segmente in einer `PathFigure` werden zu einer einzelnen geometrischen Form zusammengefasst, wobei der Endpunkt der einzelnen Segmente der Anfangspunkt des nächsten Segments ist. Die- `StartPoint` Eigenschaft eines `PathFigure` gibt den Punkt an, von dem das erste Segment gezeichnet wird. Jedes nachfolgende Segment beginnt am Endpunkt des vorherigen Segments. Beispielsweise kann eine vertikale Linie von `10,50` zu `10,150` definiert werden, indem die `StartPoint` -Eigenschaft auf festgelegt `10,50` und ein `LineSegment` mit `Point` der-Eigenschafts Einstellung erstellt wird `10,150` :

```xaml
<Path Stroke="Black"
      StrokeThickness="1">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,50">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <LineSegment Point="10,150" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

Komplexere Geometrien können mithilfe einer Kombination aus `PathSegment` -Objekten und mithilfe mehrerer- `PathFigure` Objekte in einer erstellt werden `PathGeometry` .

## <a name="composite-geometries"></a>Zusammengesetzte Geometrien

Zusammengesetzte Geometry-Objekte können mithilfe von erstellt werden `GeometryGroup` . Die- `GeometryGroup` Klasse erstellt eine zusammengesetzte Geometrie aus einem oder mehreren- `Geometry` Objekten. Einer kann eine beliebige Anzahl von- `Geometry` Objekten hinzugefügt werden `GeometryGroup` .

Im folgenden Beispiel wird gezeigt, wie Geometrien in einem kombiniert werden `GeometryGroup` :

```xaml
<Path Stroke="Green"
      StrokeThickness="2"
      Fill="Orange">
    <Path.Data>
        <GeometryGroup>
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,150" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="150,250" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="250,250" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

In diesem Beispiel werden vier `EllipseGeometry` Objekte mit identischen x-Radius-und y-Radius-Koordinaten, aber mit unterschiedlichen mittelkoordinaten kombiniert. Dadurch werden vier überlappende Kreise erstellt, deren inneren innen mit der Füllregel Orange gefüllt sind `EvenOdd` .

## <a name="clip-geometries"></a>Cligeometries

Die- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Klasse verfügt über eine- `Clip` Eigenschaft vom Typ, die die Gliederung `Geometry` des Inhalts eines Elements definiert. Wenn die- `Clip` Eigenschaft auf ein-Objekt festgelegt ist `Geometry` , wird nur der Bereich angezeigt, der sich innerhalb des Bereichs von befindet `Geometry` .

Im folgenden Beispiel wird gezeigt, wie ein- `Geometry` Objekt als Ausschneide Bereich für einen verwendet wird [`Image`](xref:Xamarin.Forms.Image) :

```xaml
<Image Source="monkeyface.png">
    <Image.Clip>
        <EllipseGeometry RadiusX="100"
                         RadiusY="100"
                         Center="180,180" />
    </Image.Clip>
</Image>
```

In diesem Beispiel wird ein `EllipseGeometry` mit `RadiusX` -und- `RadiusY` Werten von 100 und der `Center` Wert (180.180) auf die- `Clip` Eigenschaft eines festgelegt [`Image`](xref:Xamarin.Forms.Image) . Es wird nur der Teil des Bilds angezeigt, der sich innerhalb des Bereichs der Ellipse befindet:

![Schneiden eines Bilds mit einem ellipetgeometry-Typ](geometries-images/clip-ellipsegeometry.png "Schneiden eines Bilds mit einem ellipetgeometry-Typ")

> [!NOTE]
> Einfache Geometrien, Pfadgeometrien und zusammengesetzte Geometrien können zum Ausschneiden von Objekten verwendet werden [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

## <a name="other-features"></a>Andere Funktionen

Die- `GeometryHelper` Klasse stellt die folgenden Hilfsmethoden bereit:

- `FlattenGeometry`
- `FlattenCubicBezier`
- `FlattenQuadraticBezier`
- `FlattenArc`

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
