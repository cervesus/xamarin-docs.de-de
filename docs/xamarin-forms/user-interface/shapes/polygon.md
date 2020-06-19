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
ms.openlocfilehash: de1848f7439e62e84daea56defa3f4583f5758ea
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947302"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.FormsFormen: Polygon

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Polygon` Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Polygonen verwendet werden, bei denen es sich um eine verbundene Reihe von Linien handelt, die geschlossene Formen bilden. Informationen zu den Eigenschaften, die die `Polygon` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Polygon` definiert die folgenden Eigenschaften:

- `Points`vom Typ `PointCollection` . dabei handelt es sich um eine Auflistung von-Strukturen, die `Point` die Scheitel Punkte des Polygons beschreiben.
- `FillRule`vom Typ `FillRule` , der angibt, wie die innere Füllung der Form bestimmt wird. Der Standardwert dieser Eigenschaft ist `FillRule.EvenOdd`.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

## <a name="create-a-polygon"></a>Erstellen eines Polygons

Das folgende XAML-Beispiel zeigt, wie Sie ein Polygon zeichnen:

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         HeightRequest="100"
         WidthRequest="200" />
```

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.FormsFormen](index.md)
