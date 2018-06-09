---
title: Zusammenfassung der Kapitel 21. Transformiert
description: 'Beim Erstellen mobiler Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 21. Transformiert'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 3642F112-C7FA-4A74-9000-F9087BA89AD9
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: b8f24a500c0d1a8359641c62e88779b795b0577a
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240556"
---
# <a name="summary-of-chapter-21-transforms"></a>Zusammenfassung der Kapitel 21. Transformiert

Eine Xamarin.Forms-Ansicht angezeigt wird, auf dem Bildschirm in einer Position und Größe von seinem übergeordneten Element, der in der Regel ermittelt einen `Layout` oder `Layout<View>` Ableitung. Die *transformieren* ist eine Xamarin.Forms-Funktion, die diesem Speicherort, Größe oder sogar Ausrichtung ändern können.

Xamarin.Forms unterstützt drei grundlegende Arten von Transformationen:

- *Übersetzung* &mdash; ein Element verschieben, horizontal oder vertikal
- *Skalierung* &mdash; Ändern der Größe eines Elements
- *Drehung* &mdash; aktivieren Sie ein Element aus, um einen Punkt oder einer Achse

Die Skalierung ist Xamarin.Forms kugelstrahler. Er wirkt sich auf die Breite und Höhe gleichmäßig. Rotation wird sowohl auf der zweidimensionale Oberfläche des Bildschirms und in einem 3D-Bereich unterstützt. Es gibt keine Transformation Zeitversatz (oder schieren) und keine generalisierte Matrixtransformation.

Transformationen werden unterstützt, mit acht Eigenschaften vom Typ `double` definiert, indem die `VisualElement` Klasse:

- [`TranslationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/)
- [`TranslationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/)
- [`Scale`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/)
- [`Rotation`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/)
- [`RotationX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/)
- [`RotationY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/)
- [`AnchorX`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/)
- [`AnchorY`](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/)

Alle diese Eigenschaften werden durch bindbare Eigenschaften unterstützt. Sie können Ziele die Datenbindung und formatiert. [**Kapitel 22. Animation** ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter22.md) wird veranschaulicht, wie diese Eigenschaften animiert werden können, aber einige Beispiele in diesem Kapitel zeigen, wie sie mithilfe der Xamarin.Forms animieren [Timer](~/xamarin-forms/platform/device.md#Device_StartTimer).

Transformieren von Eigenschaften beeinflussen nur wie das Element gerendert wird, und führen Sie *nicht* beeinflussen, wie das Element im Layout erkannt wird.

## <a name="the-translation-transform"></a>Die Übersetzung-Transformation

Ungleich NULL-Werte, der die [ `TranslationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationX/) und [ `TranslationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.TranslationY/) Eigenschaften ein Elements verschieben, horizontal oder vertikal.

Die [ **TranslationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TranslationDemo) Programm bietet die Möglichkeit zum Experimentieren mit diesen Eigenschaften mit zwei `Slider` Elemente, die steuern die `TranslationX` und `TranslationY` Eigenschaften einer `Frame`. Die Transformation wirkt sich auf alle untergeordneten Elemente dieses `Frame`.

### <a name="text-effects"></a>Texteffekte

Eine übliche Verwendung der Übersetzung Eigenschaften ist etwas offset von der Darstellung von Text. Dies wird dargestellt, der [ **TextOffsets** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/TextOffsets) Beispiel:

[![Dreifacher Screenshot von Text Offsets](images/ch21fg03-small.png "Text Offsets")](images/ch21fg03-large.png#lightbox "Offsets von Text")

Ein weiterer Effekt wird zum Rendern von mehreren Kopien einer `Label` zu einen 3D Block ähneln, wie z. B. im veranschaulicht die [ **BlockText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BlockText) Beispiel.

### <a name="jumps-and-animations"></a>Sprünge und Animationen

Die [ **ButtonJump** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonJump) Beispiel verwendet die Übersetzung zum Verschieben einer `Button` sobald es abgerufen wird, aber der primäre Zweck ist, veranschaulicht die `Button` empfängt Benutzereingaben an der Position, in denen die Schaltfläche wird gerendert.

Die [ **ButtonGlide** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonGlide) Beispiel ähnelt, aber verwendet einen Zeitgeber zum Animieren der `Button` von einem Punkt in eine andere.

## <a name="the-scale-transform"></a>Die Skalierungstransformation für die

Die [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Transformation kann erhöhen oder verringern Sie die gerenderte Größe des Elements. Der Standardwert ist 1. Der Wert 0 bewirkt, dass das Element nicht sichtbar sein. Negative Werte bewirken, dass das Element angezeigt werden, um 180 Grad gedreht werden. Die `Scale` Eigenschaft beeinflusst nicht die `Width` oder `Height` Eigenschaften des Elements. Diese Werte bleiben unverändert.

Sie können experimentieren Sie mit der `Scale` Eigenschaft mit der [ **SimpleScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/SimpleScaleDemo) Beispiel.

Die [ **ButtonScaler** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ButtonScaler) Beispiel zeigt den Unterschied zwischen Animieren der `Scale` Eigenschaft eine `Button` und Animieren der `FontSize` Eigenschaft. Die `FontSize` Eigenschaft wirkt sich auf wie die `Button` im Layout; wahrgenommen wird die `Scale` Eigenschaft nicht.

Die [ **ScaleToSize** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ScaleToSize) Beispiel berechnet eine `Scale` -Eigenschaft, die angewendet wird ein `Label` Element, um ihn so groß wie möglich machen, während Sie weiterhin auf der Seite passt.

### <a name="anchoring-the-scale"></a>Verankern die Skalierung

Die Elemente in der vorherigen drei Beispiele skaliert alle gestiegen oder gesunken ist groß, relativ zum Mittelpunkt des Elements. Das heißt, das Element erhöht oder verringert wird Größe in allen Richtungen identisch. Nur der Punkt in der Mitte des Elements bleibt am gleichen Speicherort während der Skalierung.

Sie können ändern Sie den Mittelpunkt der Skalierung durch Festlegen der [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) und [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Eigenschaften. Diese Eigenschaften gelten relativ zum Element selbst. Für `AnchorX`, der Wert 0 bezieht sich auf der linken Seite des Elements und 1 bezieht sich auf der rechten Seite. Auf ähnliche Weise für `AnchorY`0 ist die oberste Position und 1 wird im unteren Bereich. Beide Eigenschaften haben standardmäßig den Wert 0,5, also in der Mitte.

Die [ **AnchoredScaleDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/AnchoredScaleDemo) Beispiel können Sie zum Experimentieren mit der `AnchorX` und `AnchorY` Eigenschaften als auch die `Scale` Eigenschaft.

Auf iOS verwenden nicht standardmäßiger Werte `AnchorX` und `AnchorY` Eigenschaften mit Phone Ausrichtung Änderungen in der Regel nicht kompatibel ist.

## <a name="the-rotation-transform"></a>Die Drehungstransformation

Die [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft in Grad angegeben ist, und gibt die Drehung im Uhrzeigersinn um einen Punkt des Elements durch definierten an `AnchorX` und `AnchorY`. Die [ **PlaneRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/PlaneRotationDemo) können Sie diese drei Eigenschaften experimentieren.

### <a name="rotated-text-effects"></a>Auswirkungen der gedrehte text

Die [ **BoxViewCircle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewCircle) Beispiel wird veranschaulicht, die zum Zeichnen eines Kreises mit 64 denkbar gedreht Mathematik `BoxView` Elemente.

Die [ **RotatedText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/RotatedText) Beispiel zeigt mehrere `Label` Elemente mit derselben Textzeichenfolge gedreht wird, wie Spokes angezeigt werden.

Die [ **CircularText** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/CircularText) Beispiel zeigt eine Textzeichenfolge, die angezeigt wird, um in einen Kreis zu umschließen.

### <a name="an-analog-clock"></a>Einer analogen Uhr

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek enthält eine [ `AnalogClockViewModel` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/AnalogClockViewModel.cs) -Klasse, die für die Hände einer Uhr Winkel berechnet. Zur Vermeidung von plattformabhängigkeiten im ViewModel, verwendet der Klasse `Task.Delay` statt einen Zeitgeber für die Suche nach einer neuen `DateTime` Wert.

Auch in enthalten **Xamarin.FormsBook.Toolkit** ist ein [ `SecondTickConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondTickConverter.cs) Klasse, die implementiert `IValueConverter` und eine zweite Spitze auf die nächste Sekunde gerundet wird, dient.

Die [ **MinimalBoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/MinimalBoxViewClock) verwendet drei Drehen `BoxView` Elemente eine analoge Uhr gezeichnet werden soll.

Die [ **BoxViewClock** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/BoxViewClock) verwendet `BoxView` für mehr umfassenden Grafiken, einschließlich Teilstriche auf dem Zifferblatt der Uhr markiert, und übergibt, drehen eine Distanz aus ihren enden:

[![Dreifacher Screenshot BoxView Uhr](images/ch21fg17-small.png "Analog Zifferblatt")](images/ch21fg17-large.png#lightbox "Analog Zifferblatt")

Darüber hinaus eine [ `SecondBackEaseConverter` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/SecondBackEaseConverter.cs) -Klasse im **Xamarin.FormsBook.Toolkit** bewirkt, dass der zweite Zeiger, scheinbar an wieder etwas ziehen Sie vor der Installation fort, und dann wieder in der richtigen Position zu verschieben.

### <a name="vertical-sliders"></a>Vertikalen Schieberegler?

Die [ **VerticalSliders** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/VerticalSliders) Beispiel zeigt, dass `Slider` Elemente um 90 Grad gedreht werden kann und funktionieren weiterhin. Allerdings ist es schwierig, diese gedreht positionieren `Slider` Elemente da im Layout, in dem sie immer noch als horizontale angezeigt werden.

## <a name="3d-ish-rotations"></a>3D ische Drehungen

Die [ `RotationX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationX/) Eigenschaft angezeigt wird, ein Element aus, um eine 3D X-Achse gedreht, damit die oberen und unteren Rand des Elements anscheinend zu oder von der Viewer zu verschieben. Auf ähnliche Weise die [ `RotationY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.RotationY/) anscheinend handelt es sich um ein Element aus, um die Y-Achse auf der linken und rechten Seite des Elements, anscheinend zu oder von der Viewer verschieben stellen drehen.

Die `AnchorX` Eigenschaft wirkt sich auf `RotationY` , aber nicht `RotationX`. Die `AnchorY` Eigenschaft wirkt sich auf `RotationX` , aber nicht `RotationY`. Sie können experimentieren Sie mit der [ **ThreeDeeRotationDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21/ThreeDeeRotationDemo) Stichprobe, die die Interaktionen dieser Eigenschaften durchsuchen.

Impliziert durch Xamarin.Forms 3D Koordinatensystems ist Linkshändig. Wenn Sie zeigen die Zeigefinger Ihrer linken Hand in Richtung der zunehmenden X Koordinaten (rechts) und dem mittleren Finger in Richtung der zunehmenden Y-Koordinaten (unten), klicken Sie dann die Thumb-Punkte in der Richtung des zu erhöhenden Z-Koordinaten (außerhalb des Bildschirms).

Gibt auch, wenn Sie Ihre linken Ziehpunkt in die Richtung des zu erhöhenden Werte zeigen an für jede der drei Achse, klicken Sie dann die Kurve der Finger die Richtung der Drehung für positive rotierenden Winkel.



## <a name="related-links"></a>Verwandte Links

- [Kapitel 21 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch21-Apr2016.pdf)
- [Kapitel 21-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter21)
