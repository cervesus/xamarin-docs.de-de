---
title: Zusammenfassung der Kapitel 21. Transformiert
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 21. Transformiert'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 16fcdb345fd9aeb9201a00a0bb98d143e6468f01
ms.sourcegitcommit: 8555a4dd1a579b2206f86c867125ee20fbc3d264
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/19/2018
ms.locfileid: "39156768"
---
# <a name="summary-of-chapter-21-transforms"></a>Zusammenfassung der Kapitel 21. Transformiert

Eine Xamarin.Forms-Ansicht wird angezeigt, auf dem Bildschirm in einer Position und Größe von seinem übergeordneten Element, der in der Regel ermittelt eine `Layout` oder `Layout<View>` Ableitung. Die *transformieren* ist eine Xamarin.Forms-Funktion, die diesem Speicherort, Größe oder sogar Ausrichtung ändern können.

Xamarin.Forms unterstützt drei grundlegende Arten von Transformationen:

- *Übersetzung* &mdash; ein Element verschieben, horizontal oder vertikal
- *Skalierung* &mdash; Ändern der Größe eines Elements
- *Drehung* &mdash; aktivieren ein Elements zu einem Punkt oder einer Achse

Die Skalierung erfolgt in Xamarin.Forms kugelstrahler; Es wirkt sich auf die Breite und Höhe einheitlich. Drehung wird sowohl auf der zweidimensionalen Oberfläche des Bildschirms und im 3D-Raum unterstützt. Es gibt keine Transformation datenschiefe (oder bloße) und keine generalisierte Matrixtransformation.

Transformationen werden unterstützt, mit acht Eigenschaften vom Typ `double` von definiert die `VisualElement` Klasse:

- [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)
- [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)
- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)
- [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)
- [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)
- [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)
- [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)
- [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

Alle diese Eigenschaften werden von bindbare Eigenschaften unterstützt. Sie können die Ziele der Datenbindung und Stil. [**Kapitel 22. Animation** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) wird veranschaulicht, wie diese Eigenschaften animiert werden können, aber einige Beispiele in diesem Kapitel zu zeigen, wie Sie mithilfe der Xamarin.Forms animieren können [Timer](~/xamarin-forms/platform/device.md#Device_StartTimer).

Transformieren Sie die Eigenschaften beeinflussen nur, wie das Element gerendert wird, und führen *nicht* beeinflussen, wie das Element im Layout angesehen wird.

## <a name="the-translation-transform"></a>Die Verschiebungstransformation

Ungleich NULL-Werte, der die [ `TranslationX` ](xref:Xamarin.Forms.VisualElement.TranslationX) und [ `TranslationY` ](xref:Xamarin.Forms.VisualElement.TranslationY) Eigenschaften ein Elements verschieben, horizontal oder vertikal.

Die [ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) Programm ermöglicht Ihnen das Experimentieren mit diesen Eigenschaften mit zwei `Slider` Elemente, die steuern die `TranslationX` und `TranslationY` Eigenschaften eine `Frame`. Die Transformation wirkt sich auch auf alle untergeordneten Elemente dieses `Frame`.

### <a name="text-effects"></a>Texteffekte

Eine häufige Verwendung von den Eigenschaften der Netzwerkadressübersetzung werden etwas das Rendern von Text zu versetzen. Dies wird veranschaulicht, der [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) Beispiel:

[![Dreifacher Screenshot des Text-Offsets](images/ch21fg03-small.png "Text Offsets")](images/ch21fg03-large.png#lightbox "Offsets von Text")

Eine andere Wirkung besteht, zum Rendern mehrere Kopien einer `Label` 3D Block wie folgt, so wie in der [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) Beispiel.

### <a name="jumps-and-animations"></a>Springt und Animationen

Die [ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) -Beispiel verwendet die Übersetzung verschieben eine `Button` immer es getippt wird, aber die primäre Absicht besteht darin, die veranschaulichen die `Button` empfängt Benutzereingaben an der Position, in denen die Schaltfläche wird gerendert.

Die [ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) Beispiel ist ähnlich, jedoch wird einen Zeitgeber animiert die `Button` von einem Punkt in eine andere.

## <a name="the-scale-transform"></a>Die Skalierungstransformation

Die [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Transformation kann erhöhen oder verringern Sie die gerenderte Größe des Elements. Der Standardwert ist 1. Der Wert 0 bewirkt, dass das Element nicht sichtbar sein. Negative Werte dazu führen, dass das Element angezeigt werden, um 180 Grad gedreht werden soll. Die `Scale` Eigenschaft hat keine Auswirkungen auf die `Width` oder `Height` Eigenschaften des Elements. Diese Werte bleiben unverändert.

Sie können experimentieren mit der `Scale` Eigenschaft mithilfe der [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) Beispiel.

Die [ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) Beispiel veranschaulicht den Unterschied zwischen Animieren der `Scale` Eigenschaft eine `Button` und Animieren von der `FontSize` Eigenschaft. Die `FontSize` Eigenschaft wirkt sich auf wie die `Button` wahrgenommen wird, im Layout; die `Scale` Eigenschaft jedoch nicht.

Die [ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) Beispiel berechnet eine `Scale` Eigenschaft, die auf eine `Label` Element, um es so groß wie möglich zu, während Sie weiterhin auf der Seite anpassen.

### <a name="anchoring-the-scale"></a>Verankern die Skalierung

Die Elemente in der vorherigen drei Beispiele skaliert alle gestiegen oder gesunken ist groß Bezug auf den Mittelpunkt des Elements. Das heißt, das Element erhöht oder verringert wird Größe in alle Richtungen gleich. Nur der Punkt in der Mitte des Elements bleibt am gleichen Speicherort während der Skalierung.

Sie können ändern Sie den Mittelpunkt der Skalierung durch Festlegen der [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) und [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Eigenschaften. Diese Eigenschaften gelten relativ zum Element selbst. Für `AnchorX`, wird der Wert 0 bezieht sich auf die linke Seite des Elements, und 1 bezieht sich auf der rechten Seite. Auf ähnliche Weise für `AnchorY`, 0 im oberen Bereich und 1 ist unten. Beide Eigenschaften verfügen über Standardwerte von 0,5, d.h. das Center werden.

Die [ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) Beispiel ermöglicht Ihnen das Experimentieren mit der `AnchorX` und `AnchorY` Eigenschaften als auch die `Scale` Eigenschaft.

Unter iOS, die mithilfe von nicht standardmäßigen Werten von `AnchorX` und `AnchorY` Eigenschaften telefonausrichtung generell nicht kompatibel ist.

## <a name="the-rotation-transform"></a>Die Drehungstransformation

Die [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft wird in Grad angegeben und gibt die Drehung im Uhrzeigersinn um einen Punkt, der das Element, definiert `AnchorX` und `AnchorY`. Die [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) ermöglicht Ihnen das Experimentieren mit diesen drei Eigenschaften.

### <a name="rotated-text-effects"></a>Effekten mit gedrehtem text

Die [ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) Beispiel wird veranschaulicht, die zum Zeichnen eines Kreises mit kleinen gedreht 64 Mathematik `BoxView` Elemente.

Die [ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) Beispiel zeigt mehrere `Label` Elemente mit die gleiche Textzeichenfolge gedreht wird, wie die Spokes angezeigt werden.

Die [ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) Beispiel zeigt eine Zeichenfolge, die angezeigt wird, im Kreis zu umschließen.

### <a name="an-analog-clock"></a>Einer analogen Uhr

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek enthält eine [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) -Klasse, die Winkel für die Zeiger der Uhr berechnet. Vermeiden von plattformabhängigkeiten in "ViewModel", die Klasse verwendet `Task.Delay` anstatt einen Timer für die Suche nach einer neuen `DateTime` Wert.

Enthält auch **Xamarin.FormsBook.Toolkit** ist eine [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) Klasse, die implementiert `IValueConverter` und dient, einen zweiten Winkel um die nächste Sekunde zu runden.

Die [ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) verwendet drei Drehen `BoxView` Elemente zum Zeichnen einer analogen Uhr.

Die [ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) verwendet `BoxView` für umfangreichere Grafiken, einschließlich der Teilstriche auf dem Zifferblatt der Uhr markiert, und übergibt, drehen einen kleinen Abstand an ihre enden:

[![Dreifacher Screenshot BoxView Uhr](images/ch21fg17-small.png "Analog Uhrenziffernblatts")](images/ch21fg17-large.png#lightbox "Analog Uhrenziffernblatts")

Darüber hinaus eine [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) -Klasse im **Xamarin.FormsBook.Toolkit** bewirkt, dass der zweite Zeiger angezeigt werden, wieder etwas vorher, abrufen und dann wieder in der richtigen Position zu verschieben.

### <a name="vertical-sliders"></a>Vertikalen Schieberegler?

Die [ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) Beispiel zeigt, dass `Slider` Elemente um 90 Grad gedreht werden kann und funktionieren weiterhin. Es ist jedoch schwierig, diese gedreht positionieren `Slider` Elemente, da im Layout, in dem sie immer noch auf horizontal angezeigt werden.

## <a name="3d-ish-rotations"></a>Im Zusammenhang mit 3D Drehungen

Die [ `RotationX` ](xref:Xamarin.Forms.VisualElement.RotationX) Eigenschaft angezeigt wird, um ein Element um ein 3D-Objekt X-Achse drehen, damit die oberen und unteren Rand des Elements scheint in Richtung oder vom Betrachter weglenken zu verschieben. Auf ähnliche Weise die [ `RotationY` ](xref:Xamarin.Forms.VisualElement.RotationY) scheint ein Element aus, um die Y-Achse auf der linken und rechten Seite des Elements, zum Verschieben in Richtung oder vom Betrachter weglenken scheinen stellen zu drehen.

Die `AnchorX` Eigenschaft wirkt sich auf `RotationY` , nicht jedoch `RotationX`. Die `AnchorY` Eigenschaft wirkt sich auf `RotationX` , nicht jedoch `RotationY`. Sie können experimentieren mit der [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) ein Beispiel zum Untersuchen von Aktivitäten dieser Eigenschaften.

Impliziert durch die Xamarin.Forms 3D-Koordinatensystem ist für Linkshänder. Wenn Sie zeigen die Zeigefinger von Ihrem linken in Richtung der zunehmenden X-Koordinaten (nach rechts) und dem mittleren Finger in Richtung der zunehmenden Y-Koordinaten (unten), klicken Sie dann Ihre Thumb-Steuerelement zeigt in Richtung der Z-Koordinaten (außerhalb des Bildschirms) zunimmt.

Auch wenn Sie Ihre linken Ziehpunkt in Richtung der ansteigenden Werten, zeigen gibt an für eine der drei Achsen, klicken Sie dann die Kurve der Finger die Richtung der Drehung für positive Drehen von Winkel.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 21 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Kapitel 21-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
