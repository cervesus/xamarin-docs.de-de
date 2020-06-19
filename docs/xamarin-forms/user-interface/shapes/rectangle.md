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
ms.openlocfilehash: 979b7ab610091ea805237874eb25f4e052716010
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84947301"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.FormsFormen: Rechteck

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

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

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapedemos/)
- [Xamarin.FormsFormen](index.md)
