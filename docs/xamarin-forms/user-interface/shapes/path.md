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
ms.openlocfilehash: 48d68d2597986a941a6ac3a8df0d99f09f421e62
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990873"
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

Das folgende XAML-Beispiel zeigt, wie Sie ein Polygon mit einer speziellen abgekürzten Syntax zeichnen:

```xaml
<Path Data="M 10,50 L 200,70"
      Stroke="Black"
      StrokeThickness="1"
      Aspect="Uniform"
      HorizontalOptions="Start"
      HeightRequest="100"
      WidthRequest="100" />
```

Die `Data` Zeichenfolge beginnt mit dem Befehl "muveto", der durch angegeben wird, `M` wodurch ein Startpunkt für den Pfad festgelegt wird. Bei Pfad Daten Parametern wird die Groß-/Kleinschreibung beachtet Das Kapital `M` gibt einen absoluten Speicherort für den Startpunkt an. Ein Kleinbuchstabe `m` würde relative Koordinaten angeben. `L`ist der Line-Befehl, der eine gerade Linie vom Startpunkt bis zum angegebenen Endpunkt erstellt.

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
- [Xamarin.FormsShape-Geometrien](geometries.md)
- [Xamarin.FormsPfad Transformationen](path-transforms.md)
