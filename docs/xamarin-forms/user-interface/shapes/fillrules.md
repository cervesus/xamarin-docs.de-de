---
title: 'Xamarin.FormsFormen: Füll Regeln'
description: Xamarin.FormsFormen Füll Regeln bestimmen, ob sich ein Punkt im Füllbereich einer Form befindet.
ms.prod: xamarin
ms.assetid: 5CABB22B-C6BE-43D1-91D9-6E90A4BD5622
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/24/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 86bbad476f206c13e6437f867c8e85e6bea5063a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937292"
---
# <a name="xamarinforms-shapes-fill-rules"></a>Xamarin.FormsFormen: Füll Regeln

![Vorabversion-API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Mehrere Xamarin.Forms Formen Klassen verfügen über `FillRule` Eigenschaften vom Typ `FillRule` . Dazu zählen `Polygon` , `Polyline` und `GeometryGroup` .

Die `FillRule` -Enumeration definiert `EvenOdd` -Member und-Member `Nonzero` . Jeder Member stellt eine andere Regel dar, um zu bestimmen, ob sich ein Punkt im Füllbereich einer Form befindet.

> [!IMPORTANT]
> Alle Formen werden für Füll Regeln als geschlossen betrachtet.

## <a name="evenodd"></a>EvenOdd

Die `EvenOdd` Füll Regel zeichnet einen Strahl vom Punkt auf unendlich in beliebiger Richtung und zählt die Anzahl der Segmente in der Form, die der Strahl kreuzt. Wenn diese Zahl ungerade ist, befindet sich der Punkt innerhalb von. Wenn diese Zahl gerade ist, befindet sich der Punkt außerhalb von.

Im folgenden XAML-Beispiel wird eine zusammengesetzte Form erstellt und gerendert, wobei der standardmäßig `FillRule` auf folgt `EvenOdd` :

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <!-- FillRule doesn't need to be set, because EvenOdd is the default. -->
        <GeometryGroup>
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

In diesem Beispiel wird eine zusammengesetzte Form, die aus einer Reihe von konzentrischen Ringen besteht, angezeigt:

![Zusammengesetzte Form mit EvenOdd-Füllregel](fillrule-images/evenodd.png "Zusammengesetzte Form mit EvenOdd-Füllregel")

Beachten Sie in der zusammengesetzten Form, dass der Mittelpunkt und der dritte Ring nicht ausgefüllt sind. Dies liegt daran, dass ein Strahl, der von einem beliebigen Punkt innerhalb eines dieser beiden Ringe gezeichnet wird, eine gerade Anzahl von Segmenten durchläuft:

![Zusammengesetzte Form mit Anmerkungen mit EvenOdd-Füllregel](fillrule-images/evenodd-annotated.png "Zusammengesetzte Form mit Anmerkungen mit EvenOdd-Füllregel")

In der obigen Abbildung stellen die roten Kreise Punkte dar, und die Linien stellen beliebige Strahlen dar. Für den oberen Punkt durchlaufen die beiden beliebigen Strahlen jeweils eine gerade Anzahl von Liniensegmenten. Daher wird der Ring, in dem sich der Punkt befindet, nicht ausgefüllt. Für den unteren Punkt durchlaufen die beiden beliebigen Strahlen jeweils eine ungerade Anzahl von Liniensegmenten. Daher wird der Ring, in dem sich der Punkt befindet, ausgefüllt.

## <a name="nonzero"></a>Ungleich NULL

Die `Nonzero` Füll Regel zeichnet einen Strahl vom Punkt auf unendlich in beliebiger Richtung und überprüft dann die stellen, an denen ein Segment der Form den Strahl schneidet. Beginnend mit einer Anzahl von 0 (null) wird die Anzahl jedes Mal erhöht, wenn ein Segment den Strahl von links nach rechts schneidet und dekrementiert wird, wenn ein Segment den Strahl von rechts nach links überschreitet. Wenn das Ergebnis nach dem zählen der Übergänge 0 (null) ist, liegt der Punkt außerhalb des Polygons. Andernfalls befindet er sich in.

Im folgenden XAML-Beispiel wird eine zusammengesetzte Form erstellt und rendert, wobei `FillRule` auf festgelegt ist `Nonzero` :

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF"
      Aspect="Uniform"
      HorizontalOptions="Start">
    <Path.Data>
        <GeometryGroup FillRule="Nonzero">
            <EllipseGeometry RadiusX="50"
                             RadiusY="50"
                             Center="75,75" />
            <EllipseGeometry RadiusX="70"
                             RadiusY="70"
                             Center="75,75" />
            <EllipseGeometry RadiusX="100"
                             RadiusY="100"
                             Center="75,75" />
            <EllipseGeometry RadiusX="120"
                             RadiusY="120"
                             Center="75,75" />
        </GeometryGroup>
    </Path.Data>
</Path>
```

In diesem Beispiel wird eine zusammengesetzte Form, die aus einer Reihe von konzentrischen Ringen besteht, angezeigt:

![Zusammengesetzte Form mit Füllregel ungleich NULL](fillrule-images/nonzero.png "Zusammengesetzte Form mit Füllregel ungleich NULL")

Beachten Sie, dass in der zusammengesetzten Form alle Ringe ausgefüllt sind. Dies liegt daran, dass alle Segmente in der gleichen Richtung ausgeführt werden, sodass ein Strahl, der von einem beliebigen Punkt gezeichnet wird, ein oder mehrere Segmente überschreitet und die Summe der Übergänge nicht gleich 0 ist:

![Zusammengesetzte Form mit Anmerkungen mit einer Füll Regel ungleich NULL](fillrule-images/nonzero-annotated.png "Zusammengesetzte Form mit Anmerkungen mit einer Füll Regel ungleich NULL")

In der obigen Abbildung stellen die roten Pfeile die Richtung dar, in der die Segmente gezeichnet werden, und der schwarze Pfeil steht für ein beliebiges Strahl, der von einem Punkt im inneren Ring ausgeht. Ausgehend vom Wert 0 (null) wird für jedes Segment, das der Strahl schneidet, der Wert 1 addiert, da das Segment den Strahl von links nach rechts schneidet.

Eine komplexere Form, in der Segmente in verschiedenen Richtungen ausgeführt werden, ist erforderlich, um das Verhalten der Füllregel besser zu veranschaulichen `Nonzero` . Im folgenden XAML-Beispiel wird eine ähnliche Form wie im vorherigen Beispiel erstellt, mit der Ausnahme, dass Sie mit einem `PathGeometry` anstelle eines erstellt wird `EllipseGeometry` :

```xaml
<Path Stroke="Black"
      Fill="#CCCCFF">
     <Path.Data>
         <GeometryGroup FillRule="Nonzero">
             <PathGeometry>
                 <PathGeometry.Figures>
                     <!-- Inner ring -->
                     <PathFigure StartPoint="120,120">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="50,50"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,120" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Second ring -->
                     <PathFigure StartPoint="120,100">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="70,70"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,100" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Third ring  -->
                         <PathFigure StartPoint="120,70">
                         <PathFigure.Segments>
                             <PathSegmentCollection>
                                 <ArcSegment Size="100,100"
                                             IsLargeArc="True"
                                             SweepDirection="CounterClockwise"
                                             Point="140,70" />
                             </PathSegmentCollection>
                         </PathFigure.Segments>
                     </PathFigure>

                     <!-- Outer ring -->
                     <PathFigure StartPoint="120,300">
                         <PathFigure.Segments>
                             <ArcSegment Size="130,130"
                                         IsLargeArc="True"
                                         SweepDirection="Clockwise"
                                         Point="140,300" />
                         </PathFigure.Segments>
                     </PathFigure>
                 </PathGeometry.Figures>
             </PathGeometry>
         </GeometryGroup>
     </Path.Data>
 </Path>
```

In diesem Beispiel werden eine Reihe von Bogen Segmenten gezeichnet, die nicht geschlossen sind:

![Zusammengesetzte Form mit Füllregel ungleich NULL](fillrule-images/nonzero-gaps.png "Zusammengesetzte Form mit Füllregel ungleich NULL")

In der obigen Abbildung wird der dritte Bogen aus dem Mittelpunkt nicht ausgefüllt. Dies liegt daran, dass die Summe der Werte eines bestimmten Strahls, der die Segmente im Pfad überschreitet, 0 (null) ist:

![Zusammengesetzte Form mit Anmerkungen mit einer Füll Regel ungleich NULL](fillrule-images/nonzero-gaps-annotated.png "Zusammengesetzte Form mit Anmerkungen mit einer Füll Regel ungleich NULL")

In der obigen Abbildung stellt der rote Kreis einen Punkt dar, die schwarzen Linien stellen beliebige Strahlen dar, die sich von dem Punkt im nicht gefüllten Bereich bewegen, und die roten Pfeile stellen die Richtung dar, in der die Segmente gezeichnet werden. Wie Sie sehen können, ist die Summe der Werte aus den Strahlen, die die Segmente überschreiten, 0 (null):

- Der beliebige Strahl, der Diagonal nach rechts verläuft, überschreitet zwei Segmente, die in unterschiedlichen Richtungen ausgeführt werden. Daher brechen die Segmente einander ab und geben den Wert 0 (null) an.
- Der beliebige Strahl, der Diagonal nach links verläuft, überschreitet insgesamt sechs Segmente. Die Übergänge brechen jedoch gegenseitig aus, sodass NULL die endgültige Summe ist.

Eine Summe von 0 (null) führt dazu, dass der Ring nicht ausgefüllt wird.

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
