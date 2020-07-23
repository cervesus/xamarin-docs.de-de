---
title: 'Xamarin.FormsFormen: Rechteck'
description: Die Xamarin.Forms Rechteck Klasse kann zum Zeichnen von Rechtecke verwendet werden.
ms.prod: xamarin
ms.assetid: 2DD663D3-DAEC-495C-AB6D-8A143FC97637
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 91960c08640a412e02298ed051469a86c1a9f368
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930571"
---
# <a name="xamarinforms-shapes-rectangle"></a>Xamarin.FormsFormen: Rechteck

![Vorabversion-API](~/media/shared/preview.png "Diese API ist derzeit als Vorabversion erhältlich.")

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Rectangle` -Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Rechtecke und Quadraten verwendet werden. Informationen zu den Eigenschaften, die die `Rectangle` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Rectangle` definiert die folgenden Eigenschaften:

- `RadiusX`vom Typ `double` , der der x-Achsen RADIUS ist, der zum Abrunden der Ecken des Rechtecks verwendet wird. Der Standardwert dieser Eigenschaft ist 0,0.
- `RadiusY`vom Typ `double` . dabei handelt es sich um den Radius der y-Achse, der zum Abrunden der Ecken des Rechtecks verwendet wird. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Die- `Rectangle` Klasse legt die `Aspect` von der-Klasse geerbte-Eigenschaft `Shape` auf fest `Stretch.Fill` . Weitere Informationen zur- `Aspect` Eigenschaft finden Sie unter [Stretch Shapes](index.md#stretch-shapes).

## <a name="create-a-rectangle"></a>Erstellen eines Rechtecks

Um ein Rechteck zu zeichnen, erstellen Sie ein `Rectangle` -Objekt und legen dessen `WidthRequest` -Eigenschaft und-Eigenschaft fest `HeightRequest` Um den inneren Bereich des Rechtecks zu zeichnen, legen Sie seine- `Fill` Eigenschaft auf fest [`Color`](xref:Xamarin.Forms.Color) . Um dem Rechteck eine Kontur zuzuweisen, legen Sie dessen- `Stroke` Eigenschaft auf fest [`Color`](xref:Xamarin.Forms.Color) . Die- `StrokeThickness` Eigenschaft gibt die Stärke des Rechteck Gliederung an.

Legen Sie die-Eigenschaft und die-Eigenschaft fest, um dem Rechteck abgerundete Ecken zuzuweisen `RadiusX` `RadiusY` Mit diesen Eigenschaften wird die Basis für die x-Achse und die y-Achse festgelegt, die zum Abrunden der Ecken des Rechtecks verwendet wird.

Um ein Quadrat zu zeichnen, machen Sie die `WidthRequest` -Eigenschaft und die-Eigenschaft `HeightRequest` des- `Rectangle` Objekts gleich.

Das folgende XAML-Beispiel zeigt, wie Sie ein ausgefülltes Rechteck zeichnen:

```xaml
<Rectangle Fill="Red"
           WidthRequest="150"
           HeightRequest="50"
           HorizontalOptions="Start" />
```

In diesem Beispiel wird ein rot ausgefülltes Rechteck mit den Abmessungen 150x50 (geräteunabhängige Einheiten) gezeichnet:

![Ausgefülltes Rechteck](rectangle-images/filled.png "Ausgefülltes Rechteck")

Das folgende XAML-Beispiel zeigt, wie Sie ein ausgefülltes Rechteck mit abgerundeten Ecken zeichnen:

```xaml
<Rectangle Fill="Blue"
           Stroke="Black"
           StrokeThickness="3"
           RadiusX="50"
           RadiusY="10"
           WidthRequest="200"
           HeightRequest="100"
           HorizontalOptions="Start" />
```

In diesem Beispiel wird ein blaues ausgefülltes Rechteck mit abgerundeten Ecken gezeichnet:

![Rechteck mit abgerundeten Ecken](rectangle-images/rounded.png "Rechteck mit abgerundeten Ecken")

Informationen zum Zeichnen eines gestrichelten Rechtecks finden Sie unter [Zeichnen von gestrichelten Formen](index.md#draw-dashed-shapes)

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
