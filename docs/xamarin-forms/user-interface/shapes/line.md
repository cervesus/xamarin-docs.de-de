---
title: 'Xamarin.FormsFormen: Linie'
description: Die Linien Xamarin.Forms Klasse kann zum Zeichnen von Linien verwendet werden.
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 8419a9880bb22980fd0f5465a813cd000838976c
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990820"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.FormsFormen: Linie

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Line` -Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Linien verwendet werden. Informationen zu den Eigenschaften, die die `Line` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Line` definiert die folgenden Eigenschaften:

- `X1`Gibt die x-Koordinate des Anfangs Punkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.
- `X2`Gibt die x-Koordinate des Endpunkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.
- `Y1`Gibt die y-Koordinate des Anfangs Punkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.
- `Y2`Gibt die y-Koordinate des Endpunkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

## <a name="create-a-line"></a>Erstellen einer Zeile

Das folgende XAML-Beispiel zeigt, wie Sie eine Linie zeichnen:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeThickness="4"
      HeightRequest="120"
      WidthRequest="120" />
```

> [!NOTE]
> Das Festlegen der- `Fill` Eigenschaft eines `Line` hat keine Auswirkung, da eine Linie über kein inneren verfügt.

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
