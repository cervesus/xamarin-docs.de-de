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
ms.openlocfilehash: b4369fdb7c5a35d6b9a192d9222fd82670b7e3f3
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86933587"
---
# <a name="xamarinforms-shapes-ellipse"></a>Xamarin.FormsFormen: Ellipse

![Vorabversion-API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Ellipse` -Klasse wird von der `Shape` -Klasse abgeleitet und kann verwendet werden, um Ellipsen und Kreise zu zeichnen. Informationen zu den Eigenschaften, die die `Ellipse` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

Die- `Ellipse` Klasse legt die `Aspect` von der-Klasse geerbte-Eigenschaft `Shape` auf fest `Stretch.Fill` . Weitere Informationen zur- `Aspect` Eigenschaft finden Sie unter [Stretch Shapes](index.md#stretch-shapes).

## <a name="create-an-ellipse"></a>Erstellen einer Ellipse

Um eine Ellipse zu zeichnen, erstellen Sie ein `Ellipse` -Objekt, und legen Sie dessen `WidthRequest` und- `HeightRequest` Eigenschaften fest. Um das Innere der Ellipse zu zeichnen, legen Sie die zugehörige- `Fill` Eigenschaft auf fest [`Color`](xref:Xamarin.Forms.Color) . Um der Ellipse einen Umriss zuzuweisen, legen Sie die zugehörige- `Stroke` Eigenschaft auf fest [`Color`](xref:Xamarin.Forms.Color) . Die- `StrokeThickness` Eigenschaft gibt die Stärke der Ellipse-Gliederung an.

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

![Circle](ellipse-images/circle.png "Circle")

Weitere Informationen zum Zeichnen einer gestrichelten Ellipse finden Sie unter [Zeichnen von gestrichelten Formen](index.md#draw-dashed-shapes).

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
