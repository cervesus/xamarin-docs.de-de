---
title: Zusammenfassung der Kapitel 22. Animation
ms.topic: article
ms.prod: xamarin
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 0ee99881a43b625cc8a70fb59e54710705c2d07a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="summary-of-chapter-22-animation"></a>Zusammenfassung der Kapitel 22. Animation

Sie gesehen haben, können Sie Animationen mit Xamarin.Forms Zeitgeber erstellen oder `Task.Delay`, aber es ist im Allgemeinen einfacher mithilfe der Animationsfunktionen, die von Xamarin.Forms bereitgestellt. Drei Klassen implementieren diese Animationen an:

- [`ViewExtensions`](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/), der allgemeine Ansatz
- [`Animation`](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/), jedoch wesentlich schwieriger flexibler
- [`AnimationExtension`](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/), die meisten vielseitige untersten Ebene Ansatz

Im allgemeinen Ziel Animationen Eigenschaften, die von der bindbare Eigenschaften unterstützt werden. Dies ist nicht erforderlich, aber dies sind die einzigen Eigenschaften, die dynamisch auf Änderungen zu reagieren.

Hat keine Verwendung von XAML-Benutzeroberfläche für diese Animationen, aber Sie können Animationen in XAML mit beschriebenen Verfahren integrieren [ **Kapitel 23. Trigger und Verhaltensweisen**](chapter23.md).

## <a name="exploring-basic-animations"></a>Untersuchen des grundlegenden Animationen

Die grundlegende Animation-Funktionen sind Erweiterungsmethoden finden der [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse. Diese Methoden gelten für jedes Objekt, das von abgeleitet ist `VisualElement`. Die einfachsten Animationen als Ziel die Transformationen, die Eigenschaften, die in beschriebenen [ `Chapter 21. Transforms` ](chapter21.md).

Die [ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) veranschaulicht, wie die `Clicked` -Ereignishandler für ein `Button` aufrufen können die [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Erweiterungsmethode zum Starten der Schaltfläche "in einem Kreis.

Die `RotateTo` ändert sich die `Rotation` Eigenschaft von der `Button` von 0 bis 360 im Verlauf des ein Viertel Sekunde (standardmäßig). Wenn der `Button` getippt wird erneut, es wird jedoch nichts da der `Rotation` Eigenschaft ist bereits 360 Grad.

### <a name="setting-the-animation-duration"></a>Festlegen der Animationsdauer

Das zweite Argument `RotateTo` ist eine Dauer in Millisekunden. Wenn auf einen hohen Wert festlegen Tippen Sie auf die `Button` während einer Animation startet eine neue Animation beginnt in der aktuellen Winkel.

### <a name="relative-animations"></a>Relative Animationen

Die `RelRotateTo` Methode führt eine relative Drehung durch Hinzufügen eines angegebenen Werts an den vorhandenen Wert. Diese Methode ermöglicht die `Button` auf mehrere Male und jedem abgerufen werden Zeit erhöht die `Rotation` Eigenschaft 360 Grad.

### <a name="awaiting-animations"></a>Erwarten Animationen

Alle Methoden, die Animation in `ViewExtensions` zurückgeben `Task<bool>` Objekte. Dies bedeutet, dass Sie eine Reihe von sequenziellen Animationen mit definieren können `ContinueWith` oder `await`. Die `bool` Abschluss Rückgabewert ist `false` , wenn die Animation ohne Unterbrechung abgeschlossen oder `true` , wenn er vom abgebrochen wurde die [ `CancelAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/) -Methode, die bricht alle Animationen gestartet, indem die andere Methode in `ViewExtensions` , die für dasselbe Element festgelegt werden.

### <a name="composite-animations"></a>Zusammengesetzte Animationen

Sie können eine Antwort erwartet und nicht erwartet Animationen zum Erstellen von zusammengesetzter Animationen kombinieren. Hierbei handelt es sich um die Animationen in `ViewExtensions` , die auf die `TranslatonX`, `TranslationY`, und `Scale` transformieren-Eigenschaften:

- [`TranslateTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`ScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.ScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)
- [`RelScaleTo`](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RelScaleTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/)

Beachten Sie, dass `TranslateTo` wirkt sich potenziell sowohl die `TranslationX` und `TranslationY` Eigenschaften.

### <a name="taskwhenall-and-taskwhenany"></a>"Task.WhenAll" und Task.WhenAny

Es ist auch möglich, zum Verwalten der gleichzeitiger Animationen mit [ `Task.WhenAll` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAll%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), dem signalisiert wird, wenn mehrere Aufgaben alle abgeschlossen haben, und [ `Task.WhenAny` ](https://developer.xamarin.com/api/member/System.Threading.Tasks.Task.WhenAny%7BTResult%7D/p/System.Collections.Generic.IEnumerable%7BSystem.Threading.Tasks.Task%7BTResult%7D%7D/), das signalisiert, wenn der erste von mehreren Aufgaben abgeschlossen wurde.

### <a name="rotation-and-anchors"></a>Rotation und Anker

Beim Aufrufen der `ScaleTo`, `RelScaleTo`, `RotateTo`, und `RelRotateTo` Methoden, Sie können festlegen, die [ `AnchorX` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorX/) und [ `AnchorY` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.AnchorY/) Eigenschaften an, dass die die Mitte der Skalierung und Drehung.

Die [ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) wird diese Technik veranschaulicht, durch Drehen eine `Button` um die Mitte der Seite.

### <a name="easing-functions"></a>Beschleunigungsfunktionen

Im Allgemeinen sind Animationen aus einen Startwert, einen Endwert linear. Beschleunigungsfunktionen kann dazu führen, dass Animationen zu beschleunigen oder verlangsamen Sie deren Verlauf. Die letzte optionale Argument für die Animation Methoden ist vom Typ [ `Easing` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Easing/), eine Klasse, 11 statische schreibgeschützte Felder des Typs definiert `Easing`:

- [`Linear`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/), der Standardwert
- [`SinIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinIn/), [ `SinOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinOut/), und [`SinInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SinInOut/)
- [`CubicIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicIn/), [ `CubicOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicOut/), und [`CubicInOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.CubicInOut/)
- [`BounceIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceIn/) und [`BounceOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.BounceOut/)
- [`SpringIn`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) und [`SpringOut`](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/)

Die `In` -Suffix gibt an, dass der Effekt am Anfang der Animation ist `Out` bedeutet, dass am Ende und `InOut` bedeutet, dass es am Anfang und Ende der Animation.

Die [ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) Beispiel veranschaulicht die Verwendung von Beschleunigungsfunktionen.

### <a name="your-own-easing-functions"></a>Eigene Beschleunigungsfunktionen

Außerdem können Sie eigene Beschleunigungsfunktionen durch Übergeben einer [ `Func<double, double>` ](https://developer.xamarin.com/api/type/System.Func%3CT,TResult%3E/) auf die [ `Easing` Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Easing.Easing/p/System.Func%7BSystem.Double,System.Double%7D/). `Easing` definiert auch eine implizite Konvertierung von `Func<double, double>` auf `Easing`. Das Argument für die Beschleunigungsfunktion ist immer im Bereich von 0 bis 1, während die Animation von Anfang bis Ende linear durchführt. Die Funktion *in der Regel* gibt einen Wert im Bereich von 0 bis 1 zurück, jedoch kann negativ oder größer als 1 kurz sein (wie bei der Fall ist die `SpringIn` und `SpringOut` Funktionen) oder konnte die Regeln unterbrechen, wenn Sie wissen, was Sie tun.

Die [ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) Beispiel wird veranschaulicht, einen benutzerdefinierten Beschleunigungsfunktion und [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) veranschaulicht, eine andere.

Die [ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) veranschaulicht einen benutzerdefinierten Beschleunigungsfunktion sowie eine Technik einer Änderung der `AnchorX` und `AnchorY` Eigenschaften in einer Sequenz von Drehung Animationen.

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek hat eine [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) -Klasse, verwendet eine benutzerdefinierte Beschleunigungsfunktionen, Funktion aus, um eine Schaltfläche jiggle, wenn darauf geklickt wird. Die [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) demonstriert, die es.

### <a name="entrance-animations"></a>Einblenden von Fenstern

Eine beliebte Typ der Animation tritt auf, wenn eine Seite zuerst angezeigt. Solche eine Animation gestartet werden kann, der [ `OnAppearing` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnAppearing()/) der Seite zu überschreiben. Für diese Animationen, empfiehlt es sich, das Einrichten des XAML-Code für die Vorstellung von der Seite angezeigt werden *nach* der Animation und initialisieren und das Layout von Code zu animieren.

Die [ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) -Beispiel verwendet die [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Erweiterungsmethode in den Inhalt der Seite ausgeblendet.

Die [ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) -Beispiel verwendet die [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Erweiterungsmethode in den Hintergrund der Inhalt der Seite auf den Seiten.

Die [ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) -Beispiel verwendet die [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Erweiterungsmethode zum Animieren der `RotationY` Eigenschaft. Ein [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode ist auch verfügbar.

### <a name="forever-animations"></a>Unbegrenzte Animationen

Andererseits auszuführen "forever" Animationen, bis das Programm beendet wird. Diese sind im Allgemeinen zu Demonstrationszwecken bestimmt.

Die [ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) -Beispiel verwendet [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) Animation ein-und Ausblenden von zwei Teile des Texts.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) zeigt ein Palindrom, und klicken Sie dann nacheinander dreht die einzelnen Buchstaben um 180 Grad, da alle Kopf sind. Dann wird die gesamte Zeichenfolge um 180 Grad gedreht, um identisch mit der ursprünglichen Zeichenfolge zu lesen.

Die [ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) Beispiel wird eine einfache `BoxView` Helikopter beim Drehen es um die Mitte des Bildschirms.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) dreht `BoxView` Speichen, um die Mitte des Bildschirms, und klicken Sie dann Dreht jedes Spoke selbst interessante Muster erstellen:

[![Dreifacher Screenshot der Rotation Speichen](images/ch22fg21-small.png "drehen Speichen")](images/ch22fg21-large.png "Speichen drehen")

Allerdings eine allmähliche Erhöhung der `Rotation` Eigenschaft eines Elements funktionieren ggf. nicht in der archivspeicherung als die [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) demonstriert.

Die [ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) -Beispiel verwendet [ `RotateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), [ `RotateXTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateXTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), und [ `RotateYTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.RotateYTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/) erleichtern scheint, als ob eine Bitmap in 3D-Bereich geändert wird.

### <a name="animating-the-bounds-property"></a>Animieren der Eigenschaft Grenzen

Die einzige Erweiterungsmethode in `ViewExtensions` ist noch nicht gezeigt [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), die effektiv eine Animation, die nur-Lese- [ `Bounds` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Bounds/) Eigenschaft durch Aufrufen der [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) Methode. Diese Methode wird normalerweise aufgerufen, indem `Layout` ableitungen, wie in behandelten [ **Kapitel 26. CustomLayouts**](chapter26.md).

Die `LayoutTo` Methode speziellen Zwecken eingeschränkt werden soll. Die [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) Programm zum Komprimieren und verwendet eine `BoxView` wie deaktiviert den Seiten einer Seite animiert werden soll.

Die [ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) -Beispiel verwendet `LayoutTo` verschieben Kacheln in eine Implementierung der klassischen 15 bis 16-Rätsel, in dem eine verschlüsselte Bild anstelle von nummerierten Kacheln angezeigt:

[![Dreifacher Screenshot des Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle Puzzle Spiel")](images/ch22fg26-large.png "Xuzzle Puzzle Spiel")

### <a name="your-own-awaitable-animations"></a>Awaitable Animationen

Die [ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) Beispiel erstellt eine awaitable-Animation. Die entscheidende-Klasse, die zurückgegeben werden kann eine `Task` Objekt aus der Methode und Signal nach Abschluss die Animation ist [ `TaskCompletionSource` ](https://developer.xamarin.com/api/type/System.Threading.Tasks.TaskCompletionSource%3CTResult%3E/).

## <a name="deeper-into-animations"></a>Eine umfassendere in Animationen

Das Xamarin.Forms-Animation-System kann etwas verwirrend sein. Zusätzlich zu den `Easing` -Klasse, die Animationssystem besteht aus den `ViewExtensions`, `Animation`, und `AnimationExtension` Brandverhaltensklassen.

### <a name="viewextensions-class"></a>ViewExtensions-Klasse

Sie haben bereits gesehen [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/). Er definiert neun-Methoden, die zurückgeben `Task<bool>` und [ `CancelAnimations` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.CancelAnimations/p/Xamarin.Forms.VisualElement/). Sieben des Ziels neun Methoden Transformationseigenschaften. Die anderen beiden sind [ `FadeTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.FadeTo/p/Xamarin.Forms.VisualElement/System.Double/System.UInt32/Xamarin.Forms.Easing/), welche Ziele der [ `Opacity` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Opacity/) -Eigenschaft, und [ `LayoutTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.LayoutTo/p/Xamarin.Forms.VisualElement/Xamarin.Forms.Rectangle/System.UInt32/Xamarin.Forms.Easing/), welche Aufrufe die [ `Layout` ](https://developer.xamarin.com/api/member/Xamarin.Forms.VisualElement.Layout/p/Xamarin.Forms.Rectangle/) Methode.

### <a name="animation-class"></a>Animation-Klasse

Die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) -Klasse verfügt über eine [Konstruktor](https://developer.xamarin.com/api/constructor/Xamarin.Forms.Animation.Animation/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Action/) mit fünf Argumente Rückruf und nicht mehr benötigen Methoden und Parameter, der die Animation definieren.

Untergeordneten Animationen hinzugefügt werden können, mit [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/), [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/), und und die Überladung der [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/System.Action%7BSystem.Double%7D/System.Double/System.Double/Xamarin.Forms.Easing/System.Double/System.Double/).

Das Animationsobjekt wird dann gestartet, durch einen Aufruf der [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BSystem.Double,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) Methode.

### <a name="animationextensions-class"></a>AnimationExtensions-Klasse

Die [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Klasse enthält hauptsächlich Erweiterungsmethoden. Es sind mehrere Versionen von einer `Animate` -Methode und die generische [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) -Methode ist, dass es die einzige Animationsfunktion wirklich handelt, Sie müssen, daher vielseitig verwendbar.

## <a name="working-with-the-animation-class"></a>Arbeiten mit der Animation-Klasse

Die [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) demonstriert die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Klasse mit mehrere verschiedenen Animationen.

### <a name="child-animations"></a>Untergeordneten Animationen

Die [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) Beispiel zeigt auch die untergeordneten Animationen, wodurch die (sehr ähnlich) nutzen [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) und [ `Insert` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Insert/p/System.Double/System.Double/Xamarin.Forms.Animation/) Methoden.

### <a name="beyond-the-high-level-animation-methods"></a>Außer den allgemeinen Animation-Methoden

Die [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) Beispiel zeigt auch, wie auszuführenden, die gezielt Eigenschaften von hinausgehen Animationen der `ViewExtensions` Methoden. In einem Beispiel eine Reihe von Punkten länger erhalten. in einem anderen Beispiel einem `BackgroundColor` Eigenschaft animiert wird.

### <a name="more-of-your-own-awaitable-methods"></a>Der eigene awaitable-Methoden

Die [ `TranslateTo` ](https://developer.xamarin.com/api/member/Xamarin.Forms.ViewExtensions.TranslateTo/p/Xamarin.Forms.VisualElement/System.Double/System.Double/System.UInt32/Xamarin.Forms.Easing/) Methode `ViewExtensions` funktioniert nicht mit der [ `Easing.SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) Funktion. Wenn die Beschleunigungsfunktionen Ausgabe über den Wert 1 erhält beendet.

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek enthält eine [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) -Klasse mit [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) und [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) Erweiterungsmethoden, die dieses Problem haben sowie [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) und [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) Methoden für das die Abbrechen Animationen.

Die [ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) veranschaulicht die `TranslateXTo` Methode.

Die `MoreExtensions` Klasse enthält auch eine [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) Erweiterungsmethode, die X- und Y-Verschiebung kombiniert und eine [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) Methode.

### <a name="implementing-a-bezier-animation"></a>Implementieren eine Bézier-animation

Es ist auch möglich, eine Animation entwickeln, die ein Element entlang des Pfads eines Bézier-Splines verschiebt. Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek enthält eine [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) -Struktur, die einen Bézier-Spline kapselt und ein [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) Enumeration in die Ausrichtung des Steuerelements.

Die [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Klasse enthält eine [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) Erweiterungsmethode und ein [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) Methode.

Die [ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) Beispiel wird veranschaulicht, ein Element entlang eines Pfads Beizer animieren.

## <a name="working-with-animationextensions"></a>Arbeiten mit AnimationExtensions

Eine ist Animation fehlt in der standard-Auflistung eine Farbe-Animation. Das Problem besteht darin, dass es keine richtigen lässt sich interpoliert zwischen zwei `Color` Werte. Es ist möglich, die einzelnen RGB-Werte zu interpolieren, aber nur als gültig ist interpolieren HSL-Werte.

Aus diesem Grund die [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek enthält zwei `Color` Animation-Methoden: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)und [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Es gibt auch zwei Kündigungsverfahren: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) und [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Beide Methoden stellen nutzen [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), die die Animation durchführt, durch Aufrufen den umfangreichen generischen [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate%7BT%7D/p/Xamarin.Forms.IAnimatable/System.String/System.Func%7BSystem.Double,T%7D/System.Action%7BT%7D/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action%7BT,System.Boolean%7D/System.Func%7BSystem.Boolean%7D/) Methode in [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/).

Die [ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) Beispiel veranschaulicht die Verwendung dieser beiden Arten von Animationen Farbe.

## <a name="structuring-your-animations"></a>Strukturieren von Animationen

Es ist manchmal hilfreich, express Animationen in XAML und in Verbindung mit MVVM zu verwenden. Dies wird im nächsten Kapitel behandelt [ **Kapitel 23. Trigger und Verhaltensweisen**](chapter23.md).



## <a name="related-links"></a>Verwandte Links

- [Kapitel 22 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Kapitel 22-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animation](~/xamarin-forms/user-interface/animation/index.md)
