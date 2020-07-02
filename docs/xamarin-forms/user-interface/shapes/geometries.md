---
title: 'Xamarin.FormsFormen: Geometrien'
description: Xamarin.FormsGeometry-Klassen ermöglichen es Ihnen, die Geometrie einer 2D-Form zu beschreiben.
ms.prod: xamarin
ms.assetid: 07DE3D66-1820-4642-BDDF-84146D40C99D
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 00c11c07e796ffa24c3ed1f23ca3ea29885a3d8b
ms.sourcegitcommit: 91b4d2f93687fadec5c3f80aadc8f7298d911624
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/01/2020
ms.locfileid: "85795224"
---
# <a name="xamarinforms-shapes-geometries"></a>Xamarin.FormsFormen: Geometrien

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Geometry` -Klasse und die Klassen, die davon abgeleitet werden, ermöglichen es Ihnen, die Geometrie einer 2D-Form zu beschreiben. `Geometry`Objekte können einfach sein, z. b. Rechtecke und Kreise oder zusammengesetzte, die aus zwei oder mehr Geometrie Objekten erstellt werden. Außerdem können komplexere Geometrien erstellt werden, die Bögen und Kurven enthalten.

Die- `Geometry` Klasse ist die übergeordnete Klasse für verschiedene Klassen, die verschiedene Kategorien von Geometrien definieren:

- `EllipseGeometry`, die die Geometrie einer Ellipse oder eines Kreises darstellt.
- `GeometryGroup`stellt einen Container dar, der mehrere Geometry-Objekte in einem einzelnen-Objekt kombinieren kann.
- `LineGeometry`stellt die Geometrie einer Linie dar.
- `PathGeometry`stellt die Geometrie einer komplexen Form dar, die aus Bögen, Kurven, Ellipsen, Linien und Rechtecke bestehen kann.
- `RectangleGeometry`stellt die Geometrie eines Rechtecks oder Quadrats dar.

Die `Geometry` `Shape` Klassen und scheinen ähnlich, da Sie beide 2D-Formen beschreiben, aber einen wichtigen Unterschied aufweisen. Die-Klasse `Geometry` wird von der- [`BindableObject`](xref:Xamarin.Forms.BindableObject) Klasse abgeleitet, während die- `Shape` Klasse von der-Klasse abgeleitet wird [`View`](xref:Xamarin.Forms.View) . Daher `Shape` können-Objekte sich selbst Rendering und am Layoutsystem teilnehmen, während-Objekte dies nicht möglich sind `Geometry` . Obwohl- `Shape` Objekte leichter verwendbar sind als- `Geometry` Objekte, `Geometry` sind-Objekte vielseitiger. Während ein- `Shape` Objekt zum rendering2d-Grafiken verwendet wird, `Geometry` kann ein-Objekt verwendet werden, um den geometrischen Bereich für 2D-Grafiken zu definieren und einen Bereich für das Abschneiden zu definieren.

Die folgenden Klassen verfügen über Eigenschaften, die auf-Objekte festgelegt werden können `Geometry` :

- Die- `Path` Klasse verwendet eine `Geometry` , um ihren Inhalt zu beschreiben. Sie können ein `Geometry` -Objekt erstellen, indem Sie die `Path.Data` -Eigenschaft auf ein `Geometry` -Objekt festlegen und die `Path` -und-Eigenschaften des-Objekts festlegen `Fill` `Stroke` .
- Die- [`VisualElement`](xref:Xamarin.Forms.VisualElement) Klasse verfügt über eine- `Clip` Eigenschaft vom Typ, die die Gliederung `Geometry` des Inhalts eines Elements definiert. Wenn die- `Clip` Eigenschaft auf ein-Objekt festgelegt ist `Geometry` , wird nur der Bereich angezeigt, der sich innerhalb des Bereichs von befindet `Geometry` . Weitere Informationen finden Sie unter [Clip with a Geometry](#clip-with-a-geometry).

Die Klassen, die von der- `Geometry` Klasse abgeleitet werden, können in drei Kategorien unterteilt werden: einfache Geometrien, Pfadgeometrien und zusammengesetzte Geometrien.

## <a name="simple-geometries"></a>Einfache Geometrien

Die einfachen Geometry-Klassen sind `EllipseGeometry` , `LineGeometry` und `RectangleGeometry` . Sie werden verwendet, um grundlegende geometrische Formen zu erstellen, z. b. Kreise, Linien und Rechtecke. Diese Formen sowie komplexere Formen können mithilfe von oder erstellt werden, `PathGeometry` indem Geometry-Objekte kombiniert werden, aber diese Klassen bieten einen einfacheren Ansatz für die Erstellung dieser grundlegenden geometrischen Formen.

### <a name="ellipsegeometry"></a>EllipseGeometry

Eine Ellipse-Geometrie stellt die Geometrie oder eine Ellipse oder einen Kreis dar und wird durch einen Mittelpunkt, einen x-Radius und einen y-Radius definiert.

Die `EllipseGeometry`-Klasse definiert die folgenden Eigenschaften:

- `Center`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den Mittelpunkt der Geometrie darstellt.
- `RadiusX`vom Typ `double` , der den x-Radius-Wert der Geometrie darstellt. Der Standardwert dieser Eigenschaft ist 0,0.
- `RadiusY`vom Typ `double` , der den y-Radius-Wert der Geometrie darstellt. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Im folgenden Beispiel wird gezeigt, wie ein- `EllipseGeometry` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <EllipseGeometry Center="50,50"
                     RadiusX="50"
                     RadiusY="50" />
  </Path.Data>
</Path>
```

In diesem Beispiel ist der Mittelpunkt von `EllipseGeometry` auf (50, 50) festgelegt, und der x-Radius-und der y-Radius sind auf 50 festgelegt. Dadurch wird ein roter Kreis mit einem Durchmesser von 100 geräteunabhängigen Einheiten erstellt, deren innere blau dargestellt ist:

![EllipseGeometry](geometry-images/ellipse.png "EllipseGeometry")

### <a name="linegeometry"></a>LineGeometry

Eine Linien Geometrie stellt die Geometrie einer Linie dar und wird durch Angabe des Anfangs Punkts der Linie und des Endpunkts definiert.

Die `LineGeometry`-Klasse definiert die folgenden Eigenschaften:

- `StartPoint`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den Anfangspunkt der Linie darstellt.
- `EndPoint`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den Endpunkt der Zeile darstellt.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Im folgenden Beispiel wird gezeigt, wie ein- `LineGeometry` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Stroke="Black">
  <Path.Data>
    <LineGeometry StartPoint="10,20"
                  EndPoint="100,130" />
  </Path.Data>
</Path>
```

In diesem Beispiel wird ein `LineGeometry` von (10, 20) bis (100.130) gezeichnet:

![LineGeometry](geometry-images/line.png "LineGeometry")

> [!NOTE]
> Das Festlegen der- `Fill` Eigenschaft eines-Objekts `Path` , das eine rendert `LineGeometry` , hat keine Auswirkung, da eine Linie über kein inneren verfügt.

### <a name="rectanglegeometry"></a>RectangleGeometry

Eine Rechteck Geometrie stellt die Geometrie eines Rechtecks oder Quadrats dar und wird mit einer- `Rect` Struktur definiert, die ihre relative Position und ihre Höhe und Breite angibt.

Die- `RectangleGeometry` Klasse definiert die- `Rect` Eigenschaft vom Typ [`Rectangle`](xref:Xamarin.Forms.Rectangle) , die die Abmessungen des Rechtecks darstellt. Diese Eigenschaft wird von einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass es sich um das Ziel der Daten Bindungen handeln und formatiert werden kann.

Im folgenden Beispiel wird gezeigt, wie ein- `RectangleGeometry` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Fill="Blue"
      Stroke="Red">
  <Path.Data>
    <RectangleGeometry Rect="10,10,150,100" />
  </Path.Data>
</Path>
```

Die Position und die Dimensionen des Rechtecks werden durch eine- `Rect` Struktur definiert. In diesem Beispiel ist die Position (10, 10), die Breite 150 und die Höhe 100 geräteunabhängige Einheiten:

![RectangleGeometry](geometry-images/rectangle.png "RectangleGeometry")

## <a name="path-geometries"></a>Pfadgeometrien

Eine Pfad Geometrie beschreibt eine komplexe Form, die aus Arcs, Kurven, Ellipsen, Linien und Rechtecke bestehen kann.

Die `PathGeometry`-Klasse definiert die folgenden Eigenschaften:

- `Figures`vom Typ `PathFigureCollection` , der die Auflistung von-Objekten darstellt, die `PathFigure` den Pfad Inhalt beschreiben.
- `FillRule`vom Typ `FillRule` , der bestimmt, wie die sich überschneidenden Bereiche in der Geometrie kombiniert werden. Der Standardwert dieser Eigenschaft ist `FillRule.EvenOdd`.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Weitere Informationen zur- `FillRule` Enumeration finden Sie unter [ Xamarin.Forms Shapes: Füll Regeln](fillrules.md).

> [!NOTE]
> Die `Figures` -Eigenschaft ist der der `ContentProperty` `PathGeometry` -Klasse und muss daher nicht explizit aus XAML festgelegt werden.

Eine `PathGeometry` besteht aus einer Auflistung von- `PathFigure` Objekten, wobei jede `PathFigure` eine Form in der Geometrie beschreibt. Jede besteht aus `PathFigure` einem oder mehreren- `PathSegment` Objekten, von denen jedes ein Segment der Form beschreibt. Es gibt viele Arten von Segmenten:

- `ArcSegment`, wodurch ein elliptischer Bogen zwischen zwei Punkten erstellt wird.
- `BezierSegment`, wodurch eine kubische Bezier-Kurve zwischen zwei Punkten erstellt wird.
- `LineSegment`, wodurch eine Linie zwischen zwei Punkten erstellt wird.
- `PolyBezierSegment`erstellt eine Reihe von kubischen Bezier-Kurven.
- `PolyLineSegment`, wodurch eine Reihe von Zeilen erstellt wird.
- `PolyQuadraticBezierSegment`, wodurch eine Reihe quadratischer Bezier-Kurven erstellt wird.
- `QuadraticBezierSegment`, wodurch eine quadratische Bezier-Kurve erstellt wird.

Alle oben aufgeführten Klassen werden von der abstrakten- `PathSegment` Klasse abgeleitet.

Die Segmente in einer `PathFigure` werden zu einer einzelnen geometrischen Form zusammengefasst, wobei der Endpunkt der einzelnen Segmente der Anfangspunkt des nächsten Segments ist. Die- `StartPoint` Eigenschaft eines `PathFigure` gibt den Punkt an, von dem das erste Segment gezeichnet wird. Jedes nachfolgende Segment beginnt am Endpunkt des vorherigen Segments. Beispielsweise kann eine vertikale Linie von `10,50` zu `10,150` definiert werden, indem die `StartPoint` -Eigenschaft auf festgelegt `10,50` und ein `LineSegment` mit `Point` der-Eigenschafts Einstellung erstellt wird `10,150` :

```xaml
<Path Stroke="Black">
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

### <a name="create-an-arcsegment"></a>Erstellen eines ArcSegments

Ein `ArcSegment` erstellt einen elliptischen Bogen zwischen zwei Punkten. Ein elliptischer Bogen wird durch die Start-und Endpunkte, den x-und y-Radius, den x-Achsen Dreh Faktor, einen Wert, der angibt, ob der Bogen größer als 180 Grad sein soll, und einen-Wert definiert, der die Richtung beschreibt, in der der Bogen gezeichnet wird.

Die `ArcSegment`-Klasse definiert die folgenden Eigenschaften:

- `Point`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den Endpunkt des elliptischen Bogens darstellt. Der Standardwert dieser Eigenschaft ist (0,0).
- `Size`vom Typ [`Size`](xref:Xamarin.Forms.Size) , der den x-und y-Radius des Bogens darstellt. Der Standardwert dieser Eigenschaft ist (0,0).
- `RotationAngle`vom Typ `double` , der den Betrag in Grad darstellt, um den die Ellipse um die x-Achse gedreht wird. Der Standardwert dieser Eigenschaft ist 0.
- `SweepDirection`vom Typ `SweepDirection` , der die Richtung angibt, in der der Bogen gezeichnet wird. Der Standardwert dieser Eigenschaft ist `SweepDirection.CounterClockwise`.
- `IsLargeArc`vom Typ `bool` , der angibt, ob der Bogen größer als 180 Grad sein soll. Der Standardwert dieser Eigenschaft ist `false`.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

> [!NOTE]
> Die- `ArcSegment` Klasse enthält keine-Eigenschaft für den Anfangspunkt des Bogens. Er definiert nur den Endpunkt des Bogens, den er darstellt. Der Anfangspunkt des Bogens ist der aktuelle Punkt des, dem `PathFigure` das `ArcSegment` hinzugefügt wird.

Die `SweepDirection`-Enumeration definiert die folgenden Member:

- `CounterClockwise`Gibt an, dass Bögen in einer Richtung im Uhrzeigersinn gezeichnet werden.
- `Clockwise`, das angibt, dass Bögen in der Richtung im Uhrzeigersinn gezeichnet werden.

Im folgenden Beispiel wird gezeigt, wie ein- `ArcSegment` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,100">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <ArcSegment Size="100,50"
                                            RotationAngle="45"
                                            IsLargeArc="True"
                                            SweepDirection="CounterClockwise"
                                            Point="200,100" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel wird ein elliptischer Bogen von (10.100) bis (200.100) gezeichnet.

### <a name="create-a-beziersegment"></a>Erstellen eines BezierSegment

Eine `BezierSegment` erstellt eine kubische Bezier-Kurve zwischen zwei Punkten. Eine kubische Bezier-Kurve wird von vier Punkten definiert: einem Startpunkt, einem Endpunkt und zwei Kontrollpunkten.

Die `BezierSegment`-Klasse definiert die folgenden Eigenschaften:

- `Point1`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den ersten Kontrollpunkt der Kurve darstellt. Der Standardwert dieser Eigenschaft ist (0,0).
- `Point2`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den zweiten Kontrollpunkt der Kurve darstellt. Der Standardwert dieser Eigenschaft ist (0,0).
- `Point3`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den Endpunkt der Kurve darstellt. Der Standardwert dieser Eigenschaft ist (0,0).

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

> [!NOTE]
> Die- `BezierSegment` Klasse enthält keine-Eigenschaft für den Anfangspunkt der Kurve. Der Anfangspunkt der Kurve ist der aktuelle Punkt des, dem `PathFigure` das `BezierSegment` hinzugefügt wird.

Die beiden Steuerungs Punkte einer kubischen Bezier-Kurve Verhalten sich wie Magnete, wobei Teile davon, was andernfalls eine gerade Linie ist, an sich selbst und eine Kurve erzeugt werden. Der erste Kontrollpunkt wirkt sich auf den Startteil der Kurve aus. Der zweite Kontrollpunkt wirkt sich auf den Endabschnitt der Kurve aus. Die Kurve übergibt nicht notwendigerweise einen der Steuerungs Punkte. Stattdessen verschiebt jeder Steuerungspunkt seinen Teil der Zeile in sich selbst, aber nicht durch sich selbst.

Im folgenden Beispiel wird gezeigt, wie ein- `BezierSegment` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <BezierSegment Point1="100,0"
                                               Point2="200,200"
                                               Point3="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel wird eine kubische Bezier-Kurve von (10, 10) zu (300, 10) gezeichnet. Die Kurve verfügt über zwei Kontrollpunkte bei (100, 0) und (200.200):

![BezierSegment](geometry-images/beziersegment.png "BezierSegment")

### <a name="create-a-linesegment"></a>Erstellen eines LineSegment

Eine `LineSegment` erstellt eine Linie zwischen zwei Punkten.

Die- `LineSegment` Klasse definiert die- `Point` Eigenschaft vom Typ [`Point`](xref:Xamarin.Forms.Point) , die den Endpunkt des Linien Segments darstellt. Der Standardwert dieser Eigenschaft ist (0,0) und wird von einem-Objekt unterstützt. dies [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) bedeutet, dass es sich um das Ziel der Daten Bindungen und die Formatierung handeln kann.

> [!NOTE]
> Die- `LineSegment` Klasse enthält keine-Eigenschaft für den Ausgangspunkt der Zeile. Der Endpunkt wird nur definiert. Der Anfangspunkt der Linie ist der aktuelle Punkt des, dem `PathFigure` das `LineSegment` hinzugefügt wird.

Im folgenden Beispiel wird gezeigt, wie- `LineSegment` Objekte in einem-Objekt erstellt und dargestellt werden `Path` :

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

In diesem Beispiel wird ein Liniensegment von (10.100) bis (100.100) und von (100.100) bis (100, 50) gezeichnet. Außerdem `PathFigure` wird der geschlossen, da seine- `IsClosed` Eigenschaft auf festgelegt ist `true` . Dies führt dazu, dass ein Dreieck gezeichnet wird:

![Linesegments](geometry-images/linesegments.png "Linesegments")

### <a name="create-a-polybeziersegment"></a>Erstellen eines PolyBezierSegment

Ein `PolyBezierSegment` erstellt eine oder mehrere kubische Bezier-Kurven.

Die- `PolyBezierSegment` Klasse definiert die- `Points` Eigenschaft vom Typ `PointCollection` , die die Punkte darstellt, die definieren `PolyBezierSegment` . Ein- `PointCollection` Objekt ist ein `ObservableCollection` von- [`Point`](xref:Xamarin.Forms.Point) Objekten. Diese Eigenschaft wird von einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass es sich um das Ziel der Daten Bindungen handeln und formatiert werden kann.

> [!NOTE]
> Die- `PolyBezierSegment` Klasse enthält keine-Eigenschaft für den Anfangspunkt der Kurve. Der Anfangspunkt der Kurve ist der aktuelle Punkt des, dem `PathFigure` das `PolyBezierSegment` hinzugefügt wird.

Im folgenden Beispiel wird gezeigt, wie ein- `PolyBezierSegment` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyBezierSegment Points="0,0 100,0 150,100 150,0 200,0 300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel gibt die `PolyBezierSegment` zwei kubische Bezier-Kurven an. Die erste Kurve ist von (10, 10) bis (150.100) mit dem Kontrollpunkt (0,0) und einem anderen Kontrollpunkt (100, 0). Die zweite Kurve ist von (150.100) bis (300, 10) mit einem Kontrollpunkt (150, 0) und einem anderen Kontrollpunkt (200, 0):

![PolyBezierSegment](geometry-images/polybeziersegment.png "PolyBezierSegment")

### <a name="create-a-polylinesegment"></a>Erstellen eines PolyLineSegment

Ein- `PolyLineSegment` erstellt ein oder mehrere Liniensegmente.

Die- `PolyLineSegment` Klasse definiert die- `Points` Eigenschaft vom Typ `PointCollection` , die die Punkte darstellt, die definieren `PolyLineSegment` . Ein- `PointCollection` Objekt ist ein `ObservableCollection` von- [`Point`](xref:Xamarin.Forms.Point) Objekten. Diese Eigenschaft wird von einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass es sich um das Ziel der Daten Bindungen handeln und formatiert werden kann.

> [!NOTE]
> Die- `PolyLineSegment` Klasse enthält keine-Eigenschaft für den Ausgangspunkt der Zeile. Der Anfangspunkt der Linie ist der aktuelle Punkt des, dem `PathFigure` das `PolyLineSegment` hinzugefügt wird.

Im folgenden Beispiel wird gezeigt, wie ein- `PolyLineSegment` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,10">
                    <PathFigure.Segments>
                        <PolyLineSegment Points="50,10 50,50" />
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel gibt die `PolyLineSegment` zwei Zeilen an. Die erste Zeile liegt zwischen (10, 10) und (50, 10), und die zweite Zeile liegt zwischen (50, 10) und (50, 50):

![PolyLineSegment](geometry-images/polylinesegment.png "PolyLineSegment")

### <a name="create-a-polyquadraticbeziersegment"></a>Erstellen eines PolyQuadraticBezierSegment

Ein `PolyQuadraticBezierSegment` erstellt eine oder mehrere quadratische Bezier-Kurven.

Die- `PolyQuadraticBezierSegment` Klasse definiert die- `Points` Eigenschaft vom Typ `PointCollection` , die die Punkte darstellt, die definieren `PolyQuadraticBezierSegment` . Ein- `PointCollection` Objekt ist ein `ObservableCollection` von- [`Point`](xref:Xamarin.Forms.Point) Objekten. Diese Eigenschaft wird von einem [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) -Objekt unterstützt. Dies bedeutet, dass es sich um das Ziel der Daten Bindungen handeln und formatiert werden kann.

> [!NOTE]
> Die- `PolyQuadraticBezierSegment` Klasse enthält keine-Eigenschaft für den Anfangspunkt der Kurve. Der Anfangspunkt der Kurve ist der aktuelle Punkt des, dem `PathFigure` das `PolyQuadraticBezierSegment` hinzugefügt wird.

Im folgenden Beispiel wird gezeigt, wie ein- `PolyQuadraticBezierSegment` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <PolyQuadraticBezierSegment Points="100,100 150,50 0,100 15,200" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel gibt die `PolyQuadraticBezierSegment` zwei Bezier-Kurven an. Die erste Kurve liegt zwischen (10, 10) und (150, 50) mit einem Kontrollpunkt auf (100.100). Die zweite Kurve liegt zwischen (100.100) und (15.200) mit einem Kontrollpunkt bei (0100):

![PolyQuadraticBezierSegment](geometry-images/polyquadraticbeziersegment.png "PolyQuadraticBezierSegment")

### <a name="create-a-quadraticbeziersegment"></a>Erstellen Sie ein QuadraticBezierSegment.

Eine `QuadraticBezierSegment` erstellt eine quadratische Bezier-Kurve zwischen zwei Punkten.

Die `QuadraticBezierSegment`-Klasse definiert die folgenden Eigenschaften:

- `Point1`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den Kontrollpunkt der Kurve darstellt. Der Standardwert dieser Eigenschaft ist (0,0).
- `Point2`vom Typ [`Point`](xref:Xamarin.Forms.Point) , der den Endpunkt der Kurve darstellt. Der Standardwert dieser Eigenschaft ist (0,0).

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

> [!NOTE]
> Die- `QuadraticBezierSegment` Klasse enthält keine-Eigenschaft für den Anfangspunkt der Kurve. Der Anfangspunkt der Kurve ist der aktuelle Punkt des, dem `PathFigure` das `QuadraticBezierSegment` hinzugefügt wird.

Im folgenden Beispiel wird gezeigt, wie ein- `QuadraticBezierSegment` Objekt in einem-Objekt erstellt und dargestellt wird `Path` :

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigureCollection>
                    <PathFigure StartPoint="10,10">
                        <PathFigure.Segments>
                            <PathSegmentCollection>
                                <QuadraticBezierSegment Point1="200,200"
                                                        Point2="300,10" />
                            </PathSegmentCollection>
                        </PathFigure.Segments>
                    </PathFigure>
                </PathFigureCollection>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel wird eine quadratische Bezier-Kurve von (10, 10) zu (300, 10) gezeichnet. Die Kurve weist einen Kontrollpunkt an (200.200):

![QuadraticBezierSegment](geometry-images/quadraticbeziersegment.png "QuadraticBezierSegment")

### <a name="create-complex-geometries"></a>Erstellen komplexer Geometrien

Komplexere Geometrien können mithilfe einer Kombination von-Objekten erstellt werden `PathSegment` . Im folgenden Beispiel wird eine Form mithilfe von `BezierSegment` , `LineSegment` und erstellt `ArcSegment` :

```xaml
<Path Stroke="Black">
    <Path.Data>
        <PathGeometry>
            <PathGeometry.Figures>
                <PathFigure StartPoint="10,50">
                    <PathFigure.Segments>
                        <BezierSegment Point1="100,0"
                                       Point2="200,200"
                                       Point3="300,100"/>
                        <LineSegment Point="400,100" />
                        <ArcSegment Size="50,50"
                                    RotationAngle="45"
                                    IsLargeArc="True"
                                    SweepDirection="Clockwise"
                                    Point="200,100"/>
                    </PathFigure.Segments>
                </PathFigure>
            </PathGeometry.Figures>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel wird eine `BezierSegment` zuerst mit vier Punkten definiert. Im Beispiel wird dann eine hinzugefügt `LineSegment` , die zwischen dem Endpunkt von `BezierSegment` und dem durch angegebenen Punkt gezeichnet wird `LineSegment` . Schließlich wird ein `ArcSegment` vom Endpunkt des `LineSegment` bis zu dem Punkt gezeichnet, der durch angegeben wird `ArcSegment` .

Noch komplexere Geometrien können mithilfe mehrerer- `PathFigure` Objekte in einer erstellt werden `PathGeometry` . Im folgenden Beispiel wird ein `PathGeometry` aus sieben- `PathFigure` Objekten erstellt, von denen einige mehrere- `PathSegment` Objekte enthalten:

```xaml
<Path Stroke="Red"
      StrokeThickness="12"
      StrokeLineJoin="Round">
    <Path.Data>
        <PathGeometry>
            <!-- H -->
            <PathFigure StartPoint="0,0">
                <LineSegment Point="0,100" />
            </PathFigure>
            <PathFigure StartPoint="0,50">
                <LineSegment Point="50,50" />
            </PathFigure>
            <PathFigure StartPoint="50,0">
                <LineSegment Point="50,100" />
            </PathFigure>

            <!-- E -->
            <PathFigure StartPoint="125, 0">
                <BezierSegment Point1="60, -10"
                               Point2="60, 60"
                               Point3="125, 50" />
                <BezierSegment Point1="60, 40"
                               Point2="60, 110"
                               Point3="125, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="150, 0">
                <LineSegment Point="150, 100" />
                <LineSegment Point="200, 100" />
            </PathFigure>

            <!-- L -->
            <PathFigure StartPoint="225, 0">
                <LineSegment Point="225, 100" />
                <LineSegment Point="275, 100" />
            </PathFigure>

            <!-- O -->
            <PathFigure StartPoint="300, 50">
                <ArcSegment Size="25, 50"
                            Point="300, 49.9"
                            IsLargeArc="True" />
            </PathFigure>
        </PathGeometry>
    </Path.Data>
</Path>
```

In diesem Beispiel wird das Wort "Hello" mit einer Kombination aus `LineSegment` -und- `BezierSegment` Objekten zusammen mit einem einzelnen- `ArcSegment` Objekt gezeichnet:

![Mehrere PathFigure-Objekte](geometry-images/multiple-pathfigures.png "Mehrere PathFigure-Objekte")

## <a name="composite-geometries"></a>Zusammengesetzte Geometrien

Zusammengesetzte Geometry-Objekte können mithilfe von erstellt werden `GeometryGroup` . Die- `GeometryGroup` Klasse erstellt eine zusammengesetzte Geometrie aus einem oder mehreren- `Geometry` Objekten. Einer kann eine beliebige Anzahl von- `Geometry` Objekten hinzugefügt werden `GeometryGroup` .

Die `GeometryGroup`-Klasse definiert die folgenden Eigenschaften:

- `Children`vom Typ, mit dem die Objekte, die den definieren, bestimmt werden `GeometryCollection` `GeomtryGroup` . Ein- `GeometryCollection` Objekt ist ein `ObservableCollection` von- `Geometry` Objekten.
- `FillRule`vom Typ `FillRule` , der angibt, wie die sich überschneidenden Bereiche im `GeometryGroup` kombiniert werden. Der Standardwert dieser Eigenschaft ist `FillRule.EvenOdd`.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

> [!NOTE]
> Die `Children` -Eigenschaft ist der der `ContentProperty` `GeometryGroup` -Klasse und muss daher nicht explizit aus XAML festgelegt werden.

Weitere Informationen zur- `FillRule` Enumeration finden Sie unter [ Xamarin.Forms Shapes: Füll Regeln](fillrules.md).

Um eine zusammengesetzte Geometrie zu zeichnen, legen Sie die erforderlichen Objekte als untergeordnete Elemente eines fest `Geometry` `GeometryGroup` , und zeigen Sie Sie mit einem- `Path` Objekt an. Der folgende XAML-Code zeigt ein Beispiel für Folgendes:

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

In diesem Beispiel werden vier `EllipseGeometry` Objekte mit identischen x-Radius-und y-Radius-Koordinaten, aber mit unterschiedlichen mittelkoordinaten kombiniert. Dadurch werden vier überlappende Kreise erstellt, deren inneren innen aufgrund der Standard `EvenOdd` Füll Regel Orange gefüllt ist:

![GeometryGroup](geometry-images/geometrygroup.png "GeometryGroup")

## <a name="clip-with-a-geometry"></a>Clip mit einer Geometrie

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

- `FlattenGeometry`, wodurch ein `Geometry` in eine vereinfacht wird `PathGeometry` .
- `FlattenCubicBezier`, wodurch eine kubische Bezier-Kurve in eine Auflistung vereinfacht wird `List<Point>` .
- `FlattenQuadraticBezier`, wodurch eine quadratische Bezier-Kurve in eine Auflistung vereinfacht wird `List<Point>` .
- `FlattenArc`, wodurch ein elliptischer Bogen in eine Auflistung vereinfacht wird `List<Point>` .

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
- [Xamarin.FormsFormen: Füll Regeln](fillrules.md)
