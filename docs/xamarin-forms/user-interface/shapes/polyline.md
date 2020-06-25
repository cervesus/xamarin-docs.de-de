---
title: 'Xamarin.FormsFormen: Polyline'
description: Die Xamarin.Forms polylinienklasse kann verwendet werden, um eine Reihe verbundener gerader Linien zu zeichnen.
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 5ffbf452816eb9e60d70175a995419a124bcc457
ms.sourcegitcommit: 8f6cc5208f675c8cfb645bd9ffb0fc1f8ea71411
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/24/2020
ms.locfileid: "85326167"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.FormsFormen: Polyline

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Polyline` -Klasse wird von der `Shape` -Klasse abgeleitet und kann verwendet werden, um eine Reihe verbundener gerader Linien zu zeichnen. Ein Polylinie ähnelt einem Polygon, mit dem Unterschied, dass der letzte Punkt in einer Polylinie nicht mit dem ersten Punkt verbunden ist. Informationen zu den Eigenschaften, die die `Polyline` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Polyline` definiert die folgenden Eigenschaften:

- `Points`vom Typ `PointCollection` . dabei handelt es sich um eine Auflistung von-Strukturen, die `Point` die Scheitelpunkt Punkte der Polylinie beschreiben.
- `FillRule`vom Typ `FillRule` , der angibt, wie die sich überschneidenden Bereiche in der Polylinie kombiniert werden. Der Standardwert dieser Eigenschaft ist `FillRule.EvenOdd`.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Der `PointsCollection` Typ ist eine `ObservableCollection` von- [`Point`](xref:Xamarin.Forms.Point) Objekten. Die `Point` -Struktur definiert die `X` -Eigenschaft und die-Eigenschaft `Y` des Typs `double` , die ein x-und y-Koordinatenpaar im 2D-Raum darstellen. Daher sollte die `Points` -Eigenschaft auf eine Liste von x-Koordinate und y-Koordinatenpaaren festgelegt werden, die die polylinienscheitel Punkte beschreiben, getrennt durch ein einzelnes Komma und/oder ein oder mehrere Leerzeichen. Beispielsweise sind "40, 10 70, 80" und "40 10, 70 80" gültig.

Weitere Informationen zur- `FillRule` Enumeration finden Sie unter [ Xamarin.Forms Shapes: Füll Regeln](fillrules.md).

## <a name="create-a-polyline"></a>Erstellen einer Polylinie

Um eine Polylinie zu zeichnen, erstellen Sie ein `Polyline` -Objekt, und legen Sie dessen- `Points` Eigenschaft auf die Scheitel Punkte einer Form fest. Um der Polylinie einen Umriss zuzuweisen, legen Sie die zugehörige- `Stroke` Eigenschaft auf fest [`Color`](xref:Xamarin.Forms.Color) . Die- `StrokeThickness` Eigenschaft gibt die Stärke der polyliniengliederung an.

> [!IMPORTANT]
> Wenn Sie die- `Fill` Eigenschaft eines `Polyline` auf einen festlegen [`Color`](xref:Xamarin.Forms.Color) , wird der innere Bereich der Polylinie gezeichnet, auch wenn sich der Anfangspunkt und der Endpunkt nicht überschneiden.

Das folgende XAML-Beispiel zeigt, wie Sie eine Polylinie zeichnen:

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red" />
```

In diesem Beispiel wird ein rotes Polylinie gezeichnet:

![Polylinie](polyline-images/stroke.png "Polylinie")

Das folgende XAML-Beispiel zeigt, wie Sie eine gestrichelte Polylinie zeichnen:

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          StrokeThickness="2"
          StrokeDashArray="1,1"
          StrokeDashOffset="6" />
```

In diesem Beispiel ist die Polylinie gestrichelt:

![Gestrichelte Polylinie](polyline-images/dashed.png "Gestrichelte Polylinie")

Weitere Informationen zum Zeichnen einer gestrichelten Polylinie finden Sie unter [Zeichnen von gestrichelten Formen](index.md#draw-dashed-shapes).

Das folgende XAML-Beispiel zeigt eine Polylinie, in der die Standard Füll Regel verwendet wird:

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Blue"
          Stroke="Red"
          StrokeThickness="3" />
```

In diesem Beispiel wird das Füllverhalten der Polylinie mithilfe der `EvenOdd` Füllregel bestimmt.

![EvenOdd-Polylinie](polyline-images/evenodd.png "EvenOdd-Polyine")

Das folgende XAML-Beispiel zeigt eine Polylinie, in der die `Nonzero` Füllregel verwendet wird:

```xaml
<Polyline Points="0 48, 0 144, 96 150, 100 0, 192 0, 192 96, 50 96, 48 192, 150 200 144 48"
          Fill="Black"
          FillRule="Nonzero"
          Stroke="Yellow"
          StrokeThickness="3" />
```

![Polylinie ungleich 0 (null)](polyline-images/nonzero.png "Polylinie ungleich 0 (null)")

In diesem Beispiel wird das Füllverhalten der Polylinie mithilfe der `Nonzero` Füllregel bestimmt.

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
- [Xamarin.FormsFormen: Füll Regeln](fillrules.md)
