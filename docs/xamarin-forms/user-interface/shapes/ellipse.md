---
title: 'Xamarin.FormsFormen: Ellipse'
description: Die Xamarin.Forms Ellipse-Klasse kann verwendet werden, um Ellipsen und Kreise zu zeichnen.
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 719713368971b0b278cd4a4a9721e863a17ae16d
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990782"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.FormsFormen: Ellipse

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Ellipse` -Klasse wird von der `Shape` -Klasse abgeleitet und kann verwendet werden, um Ellipsen und Kreise zu zeichnen. Informationen zu den Eigenschaften, die die `Ellipse` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

Die- `Ellipse` Klasse legt die `Aspect` von der-Klasse geerbte-Eigenschaft `Shape` auf fest `Stretch.Fill` .

## <a name="create-an-ellipse"></a>Erstellen einer Ellipse

Das folgende XAML-Beispiel zeigt, wie Sie eine gefüllte Ellipse zeichnen:

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

Das folgende XAML-Beispiel zeigt, wie Sie einen Kreis zeichnen:

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
