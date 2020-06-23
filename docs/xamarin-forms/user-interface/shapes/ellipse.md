---
title: 'Xamarin.FormsFormen: Ellipse'
description: Die Xamarin.Forms Ellipse-Klasse kann verwendet werden, um Ellipsen und Kreise zu zeichnen.
ms.prod: xamarin
ms.assetid: 5BF81E25-12E5-49F0-A40C-0CF4C5D63B9B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 053805c14f195ef0dd3ae8f0cfcac2ee7425271d
ms.sourcegitcommit: dc49ba58510eeb52048a866e5d3daf5f1f68fbd2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/22/2020
ms.locfileid: "85130904"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.FormsFormen: Ellipse

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)

Die `Ellipse` -Klasse wird von der `Shape` -Klasse abgeleitet und kann verwendet werden, um Ellipsen und Kreise zu zeichnen. Informationen zu den Eigenschaften, die die `Ellipse` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

Die- `Ellipse` Klasse legt die `Aspect` von der-Klasse geerbte-Eigenschaft `Shape` auf fest `Stretch.Fill` . Weitere Informationen zur- `Aspect` Eigenschaft finden Sie unter [Stretch Shapes](index.md#stretch-shapes).

## <a name="create-an-ellipse"></a>Erstellen einer Ellipse

Um eine Ellipse zu zeichnen, erstellen Sie ein `Ellipse` -Objekt, und legen Sie dessen `WidthRequest` und- `HeightRequest` Eigenschaften fest. Verwenden `Fill` Sie die-Eigenschaft, um den anzugeben [`Color`](xref:Xamarin.Forms.Color) , der zum Zeichnen des Inneren der Ellipse verwendet wird. Verwenden `Stroke` Sie die-Eigenschaft, um das anzugeben `Color` , das verwendet wird, um die Gliederung der Ellipse zu zeichnen. Die- `StrokeThickness` Eigenschaft gibt die Stärke der Ellipse-Gliederung an.

Um einen Kreis zu zeichnen, machen Sie die `WidthRequest` -Eigenschaft und die-Eigenschaft `HeightRequest` des- `Ellipse` Objekts gleich.

Das folgende XAML-Beispiel zeigt, wie Sie eine gefüllte Ellipse zeichnen:

```xaml
<Ellipse Fill="Red"
         WidthRequest="150"
         HeightRequest="50"
         HorizontalOptions="Start" />
```

In diesem Beispiel wird eine rot Gefüllte Ellipse mit Dimensionen 150x50 (geräteunabhängige Einheiten) gezeichnet:

![Gefüllte Ellipse](ellipse-images/filled.png "Gefüllte Ellipse")

Das folgende XAML-Beispiel zeigt, wie Sie einen Kreis zeichnen:

```xaml
<Ellipse Stroke="Red"
         StrokeThickness="4"
         WidthRequest="150"
         HeightRequest="150"
         HorizontalOptions="Start" />
```

In diesem Beispiel wird ein roter Kreis mit den Dimensionen 150x150 (geräteunabhängige Einheiten) gezeichnet:

![Kreisen](ellipse-images/circle.png "Circle")

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://github.com/xamarin/xamarin-forms-samples/tree/master/UserInterface/ShapesDemos/)
- [Xamarin.FormsFormen](index.md)
