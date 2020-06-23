---
title: 'Xamarin.FormsFormen: Pfad'
description: Die Xamarin.Forms Path-Klasse kann zum Zeichnen von Kurven und komplexen Formen verwendet werden.
ms.prod: xamarin
ms.assetid: B29486F4-9A5E-4588-ABDF-7EB1E69B9AE6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 384dcef3c2b480166f17e91d547f8cda39c83dc0
ms.sourcegitcommit: 7fc658bbdcb8130cd9d611e55e79a1830fc5d5a2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2020
ms.locfileid: "85132969"
---
# <a name="xamarinforms-shapes-path"></a>Xamarin.FormsFormen: Pfad

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Path` -Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Kurven und komplexen Formen verwendet werden. Diese Kurven und Formen werden oft mithilfe von `Geometry` Objekten beschrieben. Informationen zu den Eigenschaften, die die `Path` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Path` definiert die folgenden Eigenschaften:

- `Data`vom Typ `Geometry` , der die zu zeichende Form angibt.
- `RenderTransform`vom Typ `Transform` , der die Transformation darstellt, die auf die Geometrie eines Pfads angewendet wird, bevor Sie gezeichnet wird.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Weitere Informationen zu Transformationen finden Sie unter [ Xamarin.Forms Pfad Transformationen](path-transforms.md).

## <a name="create-a-path"></a>Erstellen eines Pfads

Das folgende XAML-Beispiel zeigt, wie ein Polygon mithilfe einer speziellen abgekürzten Syntax gezeichnet wird, die als Pfad Markup Syntax bezeichnet wird:

```xaml
<Path Data="M 10,50 L 200,70"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100" />
```

Die `Data` Zeichenfolge beginnt mit dem Befehl "muveto", der durch angegeben wird, `M` wodurch ein Startpunkt für den Pfad festgelegt wird. `L`ist der Line-Befehl, der eine gerade Linie vom Startpunkt bis zum angegebenen Endpunkt erstellt.

> [!NOTE]
> Die Pfad Markup Syntax ist nur in XAML verfügbar.

Weitere Informationen zur Pfad Markup Syntax finden Sie unter [ Xamarin.Forms Pfad Markup Syntax](path-markup-syntax.md).

## <a name="path-geometry"></a>Pfad Geometrie

Kurven und Formen können mithilfe von- `Geometry` Objekten beschrieben werden, die zum Festlegen der `Path` -Eigenschaft des Objekts verwendet werden `Data` .

Es gibt eine Vielzahl von `Geometry` Objekten, aus denen Sie auswählen können. Die `EllipseGeometry` `LineGeometry` Klassen, und `RectangleGeometry` beschreiben relativ einfache Formen. Um komplexere Formen zu erstellen oder Kurven zu erstellen, verwenden Sie eine `PathGeometry` .

`PathGeometry`Objekte bestehen aus einem oder mehreren- `PathFigure` Objekten. Jedes- `PathFigure` Objekt stellt eine andere Form dar. Jedes-Objekt besteht aus `PathFigure` einem oder mehreren- `PathSegment` Objekten, die jeweils einen Verbindungsteil der Form darstellen. Zu den Segment Typen zählen die folgenden: `LineSegment` , `BezierSegment` und `ArcSegment` .

Im folgenden Beispiel `Path` wird eine zum Zeichnen eines Dreiecks verwendet:

```xaml
<Path Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100">
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

Weitere Informationen zu Geometrien finden Sie unter [ Xamarin.Forms Geometries](geometries.md).

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
- [Xamarin.FormsGeometrien](geometries.md)
- [Xamarin.FormsPfad Markup Syntax](path-markup-syntax.md)
- [Xamarin.FormsPfad Transformationen](path-transforms.md)
