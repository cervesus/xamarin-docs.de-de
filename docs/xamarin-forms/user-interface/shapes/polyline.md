---
title: 'Xamarin.FormsFormen: Polyline'
description: Die Xamarin.Forms polylinienklasse kann verwendet werden, um eine Reihe verbundener gerader Linien zu zeichnen.
ms.prod: xamarin
ms.assetid: 15D02690-AC03-457E-8815-8E4C17E4D642
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b6c884caa7bfb301f949f353f3c6c3445704aa89
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101358"
---
# <a name="xamarinforms-shapes-polyline"></a>Xamarin.FormsFormen: Polyline

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

Die `Polyline` -Klasse wird von der `Shape` -Klasse abgeleitet und kann verwendet werden, um eine Reihe verbundener gerader Linien zu zeichnen. Informationen zu den Eigenschaften, die die `Polyline` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Polyline` definiert die folgenden Eigenschaften:

- `Points`vom Typ `PointCollection` . dabei handelt es sich um eine Auflistung von-Strukturen, die `Point` die Scheitelpunkt Punkte der Polylinie beschreiben.
- `FillRule`vom Typ `FillRule` , der angibt, wie die sich überschneidenden Bereiche in der Polylinie kombiniert werden. Der Standardwert dieser Eigenschaft ist `FillRule.EvenOdd`.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

## <a name="create-a-polyline"></a>Erstellen einer Polylinie

Das folgende XAML-Beispiel zeigt, wie Sie ein Polygon zeichnen:

```xaml
<Polyline Points="0,0 10,30, 15,0 18,60 23,30 35,30 40,0 43,60 48,30 100,30"
          Stroke="Red"
          HeightRequest="100"
          WidthRequest="500" />
```

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsFormen](index.md)
