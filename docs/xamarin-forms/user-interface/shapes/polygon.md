---
title: 'Xamarin.FormsFormen: Polygon'
description: Mithilfe der Xamarin.Forms Polygon-Klasse können Polygone gezeichnet werden, bei denen es sich um eine verbundene Reihe von Linien handelt, die geschlossene Formen bilden.
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 628218f47f318aebe7e4d0ff30efdce9617eb2c6
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86937643"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.FormsFormen: Polygon

![Vorabversion-API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Polygon` Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Polygonen verwendet werden, bei denen es sich um eine verbundene Reihe von Linien handelt, die geschlossene Formen bilden. Informationen zu den Eigenschaften, die die `Polygon` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Polygon` definiert die folgenden Eigenschaften:

- `Points`vom Typ `PointCollection` . dabei handelt es sich um eine Auflistung von-Strukturen, die [`Point`](xref:Xamarin.Forms.Point) die Scheitel Punkte des Polygons beschreiben.
- `FillRule`vom Typ `FillRule` , der angibt, wie die innere Füllung der Form bestimmt wird. Der Standardwert dieser Eigenschaft ist `FillRule.EvenOdd`.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Der `PointsCollection` Typ ist eine `ObservableCollection` von- [`Point`](xref:Xamarin.Forms.Point) Objekten. Die `Point` -Struktur definiert die `X` -Eigenschaft und die-Eigenschaft `Y` des Typs `double` , die ein x-und y-Koordinatenpaar im 2D-Raum darstellen. Daher sollte die `Points` -Eigenschaft auf eine Liste von x-Koordinate und y-Koordinatenpaaren festgelegt werden, die die Polygon Scheitel Punkte beschreiben, getrennt durch ein einzelnes Komma und/oder ein oder mehrere Leerzeichen. Beispielsweise sind "40, 10 70, 80" und "40 10, 70 80" gültig.

Weitere Informationen zur- `FillRule` Enumeration finden Sie unter [ Xamarin.Forms Shapes: Füll Regeln](fillrules.md).

## <a name="create-a-polygon"></a>Erstellen eines Polygons

Um ein Polygon zu zeichnen, erstellen Sie ein `Polygon` -Objekt, und legen Sie dessen- `Points` Eigenschaft auf die Scheitel Punkte einer Form fest. Es wird automatisch eine Linie gezeichnet, die den ersten und den letzten Punkt verbindet. Um den inneren Bereich des Polygons zu zeichnen, legen Sie seine- `Fill` Eigenschaft auf fest [`Color`](xref:Xamarin.Forms.Color) . Um dem Polygon einen Umriss zuzuweisen, legen Sie seine- `Stroke` Eigenschaft auf fest [`Color`](xref:Xamarin.Forms.Color) . Die- `StrokeThickness` Eigenschaft gibt die Stärke der Polygon Gliederung an.

Das folgende XAML-Beispiel zeigt, wie Sie ein ausgefülltes Polygon zeichnen:

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5" />
```

In diesem Beispiel wird ein ausgefülltes Polygon gezeichnet, das ein Dreieck darstellt:

![Ausgefülltes Polygon](polygon-images/filled.png "Ausgefülltes Polygon")

Das folgende XAML-Beispiel zeigt, wie Sie ein gestricheltes Polygon zeichnen:

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         StrokeDashArray="1,1"
         StrokeDashOffset="6" />
```

In diesem Beispiel wird die Polygon Gliederung gestrichelt:

![Gestricheltes Polygon](polygon-images/dashed.png "Gestricheltes Polygon")

Weitere Informationen zum Zeichnen eines gestrichelten Polygons finden Sie unter [Zeichnen von gestrichelten Formen](index.md#draw-dashed-shapes).

Das folgende XAML-Beispiel zeigt ein Polygon, das die Standard Füll Regel verwendet:

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Blue"
         Stroke="Red"
         StrokeThickness="3" />
```

In diesem Beispiel wird das Füllverhalten der einzelnen Polygon mithilfe der `EvenOdd` Füllregel bestimmt.

![EvenOdd-Polygon](polygon-images/evenodd.png "EvenOdd-Polygon")

Das folgende XAML-Beispiel zeigt ein Polygon, das die `Nonzero` Füllregel verwendet:

```xaml
<Polygon Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
         Fill="Black"
         FillRule="Nonzero"
         Stroke="Yellow"
         StrokeThickness="3" />
```

![Polygon ungleich NULL](polygon-images/nonzero.png "Polygon ungleich NULL")

In diesem Beispiel wird das Füllverhalten der einzelnen Polygon mithilfe der `Nonzero` Füllregel bestimmt.

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
- [Xamarin.FormsFormen: Füll Regeln](fillrules.md)
