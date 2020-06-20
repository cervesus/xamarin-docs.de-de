---
title: 'Xamarin.FormsFormen: Rechteck'
description: Die Xamarin.Forms Rechteck Klasse kann zum Zeichnen von Rechtecke verwendet werden.
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: da9649a4abb2cb65930d98576eda81739b711886
ms.sourcegitcommit: d86b7a18cf8b1ef28cd0fe1d311f1c58a65101a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/19/2020
ms.locfileid: "85101366"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.FormsFormen: Rechteck

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

Die `Rectangle` -Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Rechtecke verwendet werden. Informationen zu den Eigenschaften, die die `Rectangle` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Rectangle` definiert die folgenden Eigenschaften:

- `RadiusX`vom Typ `double` , der der x-Achsen RADIUS ist, der zum Abrunden der Ecken des Rechtecks verwendet wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `RadiusY`vom Typ `double` . dabei handelt es sich um den Radius der y-Achse, der zum Abrunden der Ecken des Rechtecks verwendet wird. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die- `Rectangle` Klasse legt die `Aspect` von der-Klasse geerbte-Eigenschaft `Shape` auf fest `Stretch.Fill` .

## <a name="create-a-rectangle"></a>Erstellen eines Rechtecks

Das folgende XAML-Beispiel zeigt, wie Sie ein ausgefülltes Rechteck mit abgerundeten Ecken zeichnen:

```xaml
<Rectangle Fill="DarkBlue"
           Stroke="Red"
           StrokeThickness="4"
           RadiusX="12"
           RadiusY="24"           
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsFormen](index.md)
