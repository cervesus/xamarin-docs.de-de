---
title: 'Xamarin.FormsFormen: Linie'
description: Die Linien Xamarin.Forms Klasse kann zum Zeichnen von Linien verwendet werden.
ms.prod: xamarin
ms.assetid: 384F1A72-6D3B-4FD3-BC40-E00A73A463EC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/20/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: f97e6930f543f47383e2b8da09b36573bd3192d0
ms.sourcegitcommit: ef3d4a70e70927c4f231b763842c5355f1571d15
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243749"
---
# <a name="xamarinforms-shapes-line"></a>Xamarin.FormsFormen: Linie

![](~/media/shared/preview.png "This API is currently pre-release")

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

Die `Line` -Klasse wird von der `Shape` -Klasse abgeleitet und kann zum Zeichnen von Linien verwendet werden. Informationen zu den Eigenschaften, die die `Line` Klasse von der-Klasse erbt `Shape` , finden Sie unter [ Xamarin.Forms Shapes](index.md).

`Line` definiert die folgenden Eigenschaften:

- `X1`Gibt die x-Koordinate des Anfangs Punkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.
- `Y1`Gibt die y-Koordinate des Anfangs Punkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.
- `X2`Gibt die x-Koordinate des Endpunkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.
- `Y2`Gibt die y-Koordinate des Endpunkts der Linie an, vom Typ Double. Der Standardwert dieser Eigenschaft ist 0,0.

Diese Eigenschaften werden von Objekten unterstützt [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) . Dies bedeutet, dass Sie Ziele von Daten Bindungen und formatiert sein können.

Informationen zum Steuern, wie Zeilenenden gezeichnet werden, finden Sie unter [Steuerungs Zeilenende](index.md#control-line-ends).

## <a name="create-a-line"></a>Erstellen einer Zeile

Um eine Linie zu zeichnen, erstellen Sie ein `Line` -Objekt, und legen `X1` Sie dessen-und- `Y1` Eigenschaften auf den Startpunkt und dessen `X2` -Eigenschaft und-Eigenschaft `Y` auf den zugehörigen Endpunkt fest Legen Sie außerdem die- `Stroke` Eigenschaft auf einen fest, [`Color`](xref:Xamarin.Forms.Color) da eine Zeile ohne einen Strich unsichtbar ist.

> [!NOTE]
> Das Festlegen der- `Fill` Eigenschaft eines `Line` hat keine Auswirkung, da eine Linie über kein inneren verfügt.

Das folgende XAML-Beispiel zeigt, wie Sie eine Linie zeichnen:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="Red" />
```

In diesem Beispiel wird eine rote diagonal Linie von (40, 0) bis (0120) gezeichnet:

![Linie](line-images/line.png "Linie")

Da die `X1` `Y1` Eigenschaften,, `X2` und die `Y2` Standardwerte 0 aufweisen, können einige Zeilen mit minimaler Syntax gezeichnet werden:

```xaml
<Line Stroke="Red"
      X2="200" />
```

In diesem Beispiel wird eine horizontale Linie mit einer Länge von 200 geräteunabhängigen Einheiten definiert. Da die anderen Eigenschaften standardmäßig 0 sind, wird eine Linie von (0,0) bis (200, 0) gezeichnet.

Das folgende XAML-Beispiel zeigt, wie Sie eine gestrichelte Linie zeichnen:

```xaml
<Line X1="40"
      Y1="0"
      X2="0"
      Y2="120"
      Stroke="DarkBlue"
      StrokeDashArray="1,1"
      StrokeDashOffset="6" />
```

In diesem Beispiel wird eine dunkelblaue gestrichelte diagonal Linie von (40, 0) bis (0120) gezeichnet:

![Gestrichelte Linie](line-images/dashed-line.png "Gestrichelte Linie")

Weitere Informationen zum Zeichnen einer gestrichelten Linie finden Sie unter [Zeichnen von gestrichelten Formen](index.md#draw-dashed-shapes).

## <a name="related-links"></a>Verwandte Links

- [Shapedemos (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsFormen](index.md)
