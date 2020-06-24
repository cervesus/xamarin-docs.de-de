---
title: 'Zusammenfassung für Kapitel 21: Transformationen'
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung für Kapitel 21: Transformationen'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 32393108f84ea3a57079c86b6a9a8e628ceca03a
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136668"
---
# <a name="summary-of-chapter-21-transforms"></a>Zusammenfassung für Kapitel 21: Transformationen

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)

Eine Xamarin.Forms-Ansicht wird auf dem Bildschirm an einer Position und in einer Größe angezeigt, die durch das übergeordnete Element bestimmt wird. Hierbei handelt es sich in der Regel um eine Ableitung von `Layout` oder `Layout<View>`. Die *Transformation* ist ein Xamarin.Forms-Feature, mit dem die Position, die Größe oder sogar die Ausrichtung geändert werden kann.

Xamarin.Forms unterstützt drei grundlegende Transformationsarten:

- *Übersetzung:* Ein Element wird horizontal oder vertikal verschoben.
- *Skalierung:* Die Größe eines Elements wird geändert.
- *Drehung:* Ein Element wird um einen Punkt oder eine Achse herum gedreht.

In Xamarin.Forms ist die Skalierung isotrop. Sie wirkt sich also gleichmäßig auf die Breite und auf die Höhe aus. Die Drehung wird sowohl auf der zweidimensionalen Bildschirmoberfläche als auch im 3D-Raum unterstützt. Es gibt keine geneigte Transformation und keine generalisierte Matrixtransformation.

Transformationen werden mit acht Eigenschaften vom Typ `double` unterstützt, die von der `VisualElement`-Klasse definiert werden:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Alle diese Eigenschaften werden durch bindbare Eigenschaften unterstützt. Diese können formatiert und Ziele der Datenbindung sein. In [**Kapitel 22: Animation**](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) wird veranschaulicht, wie diese Eigenschaften animiert werden können. Einige Beispiele in diesem Kapitel verdeutlichen aber auch, wie Sie diese Eigenschaften mit dem [Timer](~/xamarin-forms/platform/device.md#devicestarttimer) von Xamarin.Forms animieren können.

Transformationseigenschaften beeinflussen nur, wie das Element gerendert wird, und *nicht*, wie das Element im Layout wahrgenommen wird.

## <a name="the-translation-transform"></a>Die Übersetzungstransformation

Werte ungleich 0 (null) der Eigenschaften [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX) und [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY) verschieben ein Element horizontal oder vertikal.

Mit dem [**TranslationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo)-Programm können Sie mit diesen Eigenschaften mit zwei `Slider`-Elementen experimentieren, die die `TranslationX`- und `TranslationY`-Eigenschaften eines `Frame`-Elements steuern. Die Transformation wirkt sich auch auf alle untergeordneten Elemente von `Frame` aus.

### <a name="text-effects"></a>Texteffekte

Eine häufige Verwendung der Übersetzungseigenschaften besteht darin, das Rendering von Text leicht zu versetzen. Dies wird im Beispiel [**TextOffsets**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) veranschaulicht:

[![Dreifacher Screenshot von TextOffsets](images/ch21fg03-small.png "TextOffsets")](images/ch21fg03-large.png#lightbox "TextOffsets")

Ein weiterer Effekt ist das Rendern mehrerer Kopien eines `Label`-Elements, damit diese einen 3D-Block darstellen. Dieser Vorgang wird im Beispiel [**BlockText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) veranschaulicht.

### <a name="jumps-and-animations"></a>Sprünge und Animationen

Das [**ButtonJump**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump)-Beispiel verwendet die Übersetzung, um ein `Button`-Element zu verschieben, wenn auf dieses getippt wird. Der primäre Zweck besteht jedoch darin, zu verdeutlichen, dass `Button` Benutzereingaben an der Stelle empfängt, an der die Schaltfläche gerendert wird.

Das [**ButtonGlide**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide)-Beispiel ist ähnlich, verwendet aber einen Timer, um `Button` von einem Punkt zu einem anderen zu animieren.

## <a name="the-scale-transform"></a>Die Skalierungstransformation

Die [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)-Transformation kann das gerenderte Element vergrößern oder verkleinern. Der Standardwert ist 1. Der Wert 0 bewirkt, dass das Element unsichtbar ist. Negative Werte bewirken, dass das Element um 180 Grad gedreht wird. Die `Scale`-Eigenschaft hat keine Auswirkung auf die `Width`- oder `Height`-Eigenschaften des-Elements. Diese Werte bleiben unverändert.

Sie können mit der `Scale`-Eigenschaft experimentieren, indem Sie das Beispiel [**SimpleScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) nutzen.

Das Beispiel [**ButtonScaler**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) veranschaulicht den Unterschied zwischen der Animation der `Scale`-Eigenschaft von `Button` und der Animation der `FontSize`-Eigenschaft. Die `FontSize`-Eigenschaft wirkt sich darauf aus, wie `Button` im Layout wahrgenommen wird, die `Scale`-Eigenschaft jedoch nicht.

Das Beispiel [**ScaleToSize**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) berechnet eine `Scale`-Eigenschaft, die auf ein `Label`-Element angewendet wird, um weit wie möglich zu vergrößern, solange es noch auf die Seite passt.

### <a name="anchoring-the-scale"></a>Verankern der Skalierung

Die Elemente, die in den vorherigen drei Beispielen skaliert wurden, wurden alle relativ zum Mittelpunkt des Elements vergrößert oder verkleinert. Die Elemente wurden also in alle Richtungen gleichmäßig vergrößert oder verkleinert. Nur der Mittelpunkt des Elements bleibt während der Skalierung an derselben Position.

Sie können den Mittelpunkt der Skalierung ändern, indem Sie die Eigenschaften [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) und [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) festlegen. Diese Eigenschaften sind relativ zum Element. Bei `AnchorX` bezieht sich der Wert 0 auf die linke Seite und der Wert 1 auf die rechte Seite des Elements. Gleichermaßen bezieht sich 0 bei `AnchorY` auf die obere Seite und 1 auf die untere Seite. Beide Eigenschaften haben den Standardwerte 0.5, der dem Mittelpunkt entspricht.

Das Beispiel [**AnchoredScaleDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) ermöglicht Ihnen das Experimentieren mit den Eigenschaften `AnchorX`, `AnchorY` und `Scale`.

Unter iOS sind Änderungen der Standardwerte von `AnchorX` und `AnchorY` nicht mit der Ausrichtung des Smartphones kompatibel.

## <a name="the-rotation-transform"></a>Die Drehungstransformation

Die [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)-Eigenschaft wird in Grad angegeben und gibt eine Drehung im Uhrzeigersinn um einen Punkt des Elements an, der durch `AnchorX` und `AnchorY` definiert wird. Mit dem Beispiel [**PlaneRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) können Sie mit diesen drei Eigenschaften experimentieren.

### <a name="rotated-text-effects"></a>Gedrehte Texteffekte

Das Beispiel [**BoxViewCircle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) veranschaulicht die erforderliche Mathematik zum Zeichnen eines Kreises mithilfe von 64 winzigen gedrehten `BoxView`-Elementen.

Im Beispiel [**RotatedText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) werden mehrere `Label`-Elemente angezeigt, bei denen die gleiche Textzeichenfolge so gedreht wurde, dass sie wie Speichen aussehen.

Im Beispiel [**CircularText**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) wird eine Textzeichenfolge angezeigt, die von einem Kreis umschlossen zu sein scheint.

### <a name="an-analog-clock"></a>Eine Analoguhr

Die Bibliothek [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) enthält eine [`AnalogClockViewModel`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs)-Klasse, die Winkel für die Zeiger einer Uhr berechnet. Um Plattformabhängigkeiten in ViewModel zu vermeiden, verwendet die Klasse `Task.Delay` anstelle eines Timers, um einen neuen `DateTime`-Wert zu ermitteln.

In **Xamarin.FormsBook.Toolkit** ist zudem eine [`SecondTickConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs)-Klasse enthalten, die `IValueConverter` implementiert und zum Runden eines zweiten Winkels auf die nächste Sekunde dient.

[**MinimalBoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) verwendet drei rotierende `BoxView`-Elemente zum Zeichnen einer Analoguhr.

[**BoxViewClock**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) verwendet `BoxView` für umfangreichere Grafiken wie die Teilstriche auf dem Ziffernblatt der Uhr und Zeiger, die sich in geringem Abstand zu ihren Enden drehen:

[![Dreifacher Screenshot der BoxViewClock](images/ch21fg17-small.png "AnalogClockFace")](images/ch21fg17-large.png#lightbox "AnalogClockFace")

Zusätzlich bewirkt eine [`SecondBackEaseConverter`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs)-Klasse in **Xamarin.FormsBook.Toolkit**, dass der zweite Zeiger sich vor dem Vorwärtssprung ein wenig zurückbewegt und dann wieder in die richtige Position wechselt.

### <a name="vertical-sliders"></a>Vertikale Schieberegler

Das Beispiel [**VerticalSliders**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) veranschaulicht, dass `Slider`-Elemente um 90 Grad gedreht werden und dabei weiterhin funktionieren können. Allerdings ist es schwierig, diese gedrehten `Slider`-Elemente zu positionieren, da sie im Layout immer noch horizontal erscheinen.

## <a name="3d-ish-rotations"></a>3D-artige Drehung

Mit der [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)-Eigenschaft wird ein Element scheinbar um eine 3D-X-Achse herum gedreht, sodass es scheint, als ob der obere oder untere Rand des Elements sich auf den Betrachter zu- oder von diesem wegbewegen. Ebenso scheint [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY) ein Element um die Y-Achse zu drehen, sodass es scheint, als ob die linke und die rechte Seite des Elements sich auf den Betrachter zu- oder von diesem wegbewegen.

Die `AnchorX`-Eigenschaft wirkt sich auf `RotationY`, jedoch nicht auf `RotationX` aus. Die `AnchorY`-Eigenschaft wirkt sich auf `RotationX`, jedoch nicht auf `RotationY` aus. Sie können mit dem Beispiel [**ThreeDeeRotationDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) experimentieren, um die Interaktionen dieser Eigenschaften kennenzulernen.

Das 3D-Koordinatensystem von Xamarin.Forms ist auf die Verwendung der linken Hand ausgelegt. Wenn Sie den Zeigefinger der linken Hand in Richtung der ansteigenden X-Koordinaten (auf der rechten Seite) und den Mittelfinger in Richtung der ansteigenden Y-Koordinaten (unten) bewegen, zeigt Ihr Daumen in Richtung der ansteigenden Z-Koordinaten (außerhalb des Bildschirms).

Wenn Sie auf einer der drei Achsen mit dem linken Daumen in Richtung der ansteigenden Werte zeigen, gibt die Kurve Ihrer Finger die Drehrichtung für positive Drehwinkel an.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 21, Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Kapitel 21, Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
