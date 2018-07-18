---
title: Zusammenfassung der Kapitel 22. Animation
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung der Kapitel 22. Animation'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: charlespetzold
ms.author: chape
ms.date: 11/07/2017
ms.openlocfilehash: 58a49b92b654e673eddd63da29cdb1866a217fca
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996398"
---
# <a name="summary-of-chapter-22-animation"></a>Zusammenfassung der Kapitel 22. Animation

Sie haben gesehen, dass Sie eigene Animationen mithilfe des Xamarin.Forms-Timers erstellen können oder `Task.Delay`, aber es ist im Allgemeinen einfacher die Animationsfunktionen von Xamarin.Forms bereitgestelltes verwenden. Drei Klassen implementieren diese Animationen an:

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions), der allgemeine Ansatz
- [`Animation`](xref:Xamarin.Forms.Animation), flexibler jedoch wesentlich schwieriger
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions), die vielseitige, niedrigsten Ansatz

Animationen als Ziel im allgemeinen Eigenschaften, die durch die bindbare Eigenschaften unterstützt werden. Dies ist nicht erforderlich, aber dies sind die einzigen Eigenschaften, die dynamisch auf Änderungen reagieren.

Es ist keine XAML-Schnittstelle für diese Animationen, aber Sie können Animationen in XAML mithilfe von Techniken, die in beschriebenen integrieren [ **Kapitel 23. Trigger und Verhaltensweisen**](chapter23.md).

## <a name="exploring-basic-animations"></a>Untersuchen einfache Animationen

Die grundlegende Animation-Funktionen sind Erweiterungsmethoden finden Sie in der [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse. Diese Methoden gelten für jedes Objekt, das von abgeleitet ist `VisualElement`. Die einfachste Animationen als Ziel die Transformationen, die Eigenschaften, die in beschriebenen [ `Chapter 21. Transforms` ](chapter21.md).

Die [ **AnimationTryout** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) wird veranschaulicht, wie die `Clicked` -Ereignishandler für ein `Button` aufrufen können die [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Erweiterungsmethode zum Starten der die Schaltfläche in einem Kreis.

Die `RotateTo` Änderungen der `Rotation` Eigenschaft der `Button` von 0 bis 360 im Verlauf von einem Viertel Sekunde (standardmäßig). Wenn die `Button` getippt wird in diesem Fall allerdings es ist da die `Rotation` Eigenschaft ist bereits 360 Grad.

### <a name="setting-the-animation-duration"></a>Festlegen der Animationsdauer

Das zweite Argument für `RotateTo` ist eine Dauer in Millisekunden. Wenn auf einen hohen Wert fest, legen Sie durch Tippen auf die `Button` während einer Animation beginnt eine neue Animation ab dem aktuellen Winkel.

### <a name="relative-animations"></a>Relative Animationen

Die `RelRotateTo` Methode führt eine relative-Rotation durch Hinzufügen eines angegebenen Werts an den vorhandenen Wert. Mit dieser Methode können die `Button` auf mehrere Male, und jeder getippt werden Zeit erhöht sich die `Rotation` Eigenschaft um 360 Grad.

### <a name="awaiting-animations"></a>Wartet auf Animationen

Alle Methoden, die Animation in `ViewExtensions` zurückgeben `Task<bool>` Objekte. Dies bedeutet, dass Sie eine Reihe von sequenziellen Animationen mit definieren können `ContinueWith` oder `await`. Die `bool` Abschluss Rückgabewert ist `false` , wenn die Animation ohne Unterbrechung abgeschlossen oder `true` , wenn es vom abgebrochen wurde die [ `CancelAnimation` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) -Methode, die alle Animationen von initiiert bricht die andere Methode in der `ViewExtensions` , die auf dem selben Element festgelegt werden.

### <a name="composite-animations"></a>Zusammengesetzte Animationen

Sie können eine Antwort erwartet und nicht erwartete Animationen, die zum Erstellen von zusammengesetzter Animationen kombinieren. Hierbei handelt es sich um die Animationen in `ViewExtensions` , die auf die `TranslatonX`, `TranslationY`, und `Scale` Transformationseigenschaften:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

Beachten Sie, dass `TranslateTo` wirkt sich möglicherweise auf beide die `TranslationX` und `TranslationY` Eigenschaften.

### <a name="taskwhenall-and-taskwhenany"></a>Task.WhenAll "und" Task.WhenAny

Es ist auch möglich, zum Verwalten von gleichzeitiger Animationen mit [ `Task.WhenAll` ](xref:System.Threading.Tasks.Task.WhenAll*), die signalisiert werden, wenn mehrere Aufgaben alle abgeschlossen haben, und [ `Task.WhenAny` ](xref:System.Threading.Tasks.Task.WhenAny*), die signalisiert, wann die erste von mehreren Tasks ist abgeschlossen.

### <a name="rotation-and-anchors"></a>Rotation und Anker

Beim Aufrufen der `ScaleTo`, `RelScaleTo`, `RotateTo`, und `RelRotateTo` Methoden, die Sie festlegen können die [ `AnchorX` ](xref:Xamarin.Forms.VisualElement.AnchorX) und [ `AnchorY` ](xref:Xamarin.Forms.VisualElement.AnchorY) Eigenschaften an die der Mittelpunkt der Skalierung und Drehung.

Die [ **CircleButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) veranschaulicht dieses Verfahren durch Drehen eine `Button` um den Mittelpunkt der Seite.

### <a name="easing-functions"></a>Beschleunigungsfunktionen

Im Allgemeinen werden Animationen lineare aus einen Startwert, einen Endwert. Beschleunigungsfunktionen kann dazu führen, dass Animationen zu beschleunigen oder verlangsamen ihre Verlauf. Das letzte optionale Argument an die Methoden der Animation ist vom Typ [ `Easing` ](xref:Xamarin.Forms.Easing), eine Klasse, 11 statische schreibgeschützte Felder des Typs definiert `Easing`:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear), der Standardwert
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn), [ `SinOut` ](xref:Xamarin.Forms.Easing.SinOut), und [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn), [ `CubicOut` ](xref:Xamarin.Forms.Easing.CubicOut), und [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) und [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) und [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

Die `In` Suffix gibt an, dass die Auswirkungen zu Beginn der Animation, `Out` bedeutet, dass am Ende und `InOut` bedeutet, dass es am Anfang und Ende der Animation.

Die [ **BounceButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) Beispiel veranschaulicht die Verwendung von Beschleunigungsfunktionen.

### <a name="your-own-easing-functions"></a>Eigene Beschleunigungsfunktionen

Sie können auch definieren Sie eigene Beschleunigungsfunktionen durch Übergeben einer [ `Func<double, double>` ](xref:System.Func`2) auf die [ `Easing` Konstruktor](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double})). `Easing` definiert auch eine implizite Konvertierung von `Func<double, double>` zu `Easing`. Das Argument für die Beschleunigungsfunktion ist immer im Bereich von 0 bis 1, wie die Animation linear fortgesetzt wird, von Anfang bis Ende. Die Funktion *in der Regel* gibt einen Wert im Bereich von 0 bis 1 zurück, jedoch kann negativ oder größer als 1 kurz sein (wie bei der Fall ist die `SpringIn` und `SpringOut` Funktionen) oder die Regeln könnten beschädigt werden, wenn Sie wissen, was Sie tun.

Die [ **UneasyScale** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) Beispiel veranschaulicht eine benutzerdefinierte Beschleunigungsfunktion und [ **CustomCubicEase** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) zeigt eine andere.

Die [ **SwingButton** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) Beispiel zeigt auch einen benutzerdefinierten Beschleunigungsfunktion sowie ein Verfahren zum Ändern der `AnchorX` und `AnchorY` Eigenschaften in einer Sequenz von Drehung Animationen.

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek verfügt über eine [ `JiggleButton` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs) Funktion, Klasse, die eine benutzerdefinierte Beschleunigungsfunktionen verwendet eine Schaltfläche jiggle, wenn darauf geklickt wird. Die [ **JiggleButtonDemo** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) veranschaulicht sie.

### <a name="entrance-animations"></a>"Entrance" Animationen

Eine beliebte Animation tritt auf, beim ersten eine Seite öffnen. Eine solche Animation gestartet werden kann, der [ `OnAppearing` ](xref:Xamarin.Forms.Page.OnAppearing) außer Kraft setzen, der Seite. Für diese Animationen, empfiehlt es sich, richten Sie die XAML zur Darstellung der Seite aus, um angezeigt *nach* der Animation, und klicken Sie dann zu initialisieren und das Layout von Code zu animieren.

Die [ **FadingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) -Beispiel verwendet die [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Erweiterungsmethode, um den Inhalt der Seite eingeblendet.

Die [ **SlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) -Beispiel verwendet die [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) Erweiterungsmethode, um den Inhalt der Seite auf den Seiten zu schieben.

Die [ **SwingingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) -Beispiel verwendet die [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Erweiterungsmethode zum Animieren der `RotationY` Eigenschaft. Ein [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Methode ist ebenfalls verfügbar.

### <a name="forever-animations"></a>Immer Animationen

Im anderen Extremfall auszuführen "forever" Animationen, bis das Programm beendet wird. Diese sind in der Regel für Demonstrationszwecke gedacht.

Die [ **FadingTextAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) wird [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) Animation zwei Textteile ein-und ausgeblendet.

[**PalindromeAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) zeigt ein Palindrom und dreht die einzelnen Buchstaben dann nacheinander um 180 Grad ein, damit alle kopfstehenden sind. Klicken Sie dann die gesamte Zeichenfolge um 180 Grad gekippt wird, um die identisch mit der ursprünglichen Zeichenfolge zu lesen.

Die [ **CopterAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) Beispiel wird eine einfache `BoxView` Helikopter beim Drehen ihn um die Mitte des Bildschirms.

[**RotatingSpokes** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) dreht sich `BoxView` spokes in der Mitte des Bildschirms, und klicken Sie dann dreht einzelnen Spokes zu interessante Muster zu erstellen:

[![Dreifacher Screenshot der Rotation-Spokes](images/ch22fg21-small.png "drehen Spokes")](images/ch22fg21-large.png#lightbox "Spokes drehen")

Allerdings eine allmähliche Erhöhung der `Rotation` Eigenschaft eines Elements funktionieren möglicherweise nicht auf lange Sicht, als die [ **RotationBreakdown** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) veranschaulicht.

Die [ **SpinningImage** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) wird [ `RotateTo` ](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [ `RotateXTo` ](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), und [ `RotateYTo` ](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) , damit es so scheinen, als ob eine Bitmap im 3D-Raum geändert wird.

### <a name="animating-the-bounds-property"></a>Animieren von der Bounds-Eigenschaft

Die einzige Erweiterungsmethode im `ViewExtensions` ist noch nicht veranschaulicht [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), die effektiv eine Animation, die schreibgeschützte [ `Bounds` ](xref:Xamarin.Forms.VisualElement.Bounds) Eigenschaft durch Aufrufen der [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Methode. Diese Methode wird normalerweise aufgerufen, indem `Layout` ableitungen, wie in besprochen werden [ **Kapitel 26. CustomLayouts**](chapter26.md).

Die `LayoutTo` Methode besondere Zwecke eingeschränkt werden soll. Die [ **BouncingBox** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) verwendet wird, komprimieren und Dekomprimieren von einem `BoxView` , wie sie aus den Seiten einer Seite springt.

Die [ **XamagonXuzzle** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) wird `LayoutTo` zum Verschieben von Kacheln in eine Implementierung des klassischen 15-16-Rätsel, die eine unverständliche Bild anstelle von nummerierten Kacheln anzeigt:

[![Dreifacher Screenshot des Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle Rätsel Spiel")](images/ch22fg26-large.png#lightbox "Xuzzle Puzzle-Spiel")

### <a name="your-own-awaitable-animations"></a>Eigene awaitable Animationen

Die [ **TryAwaitableAnimation** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) Beispiel erstellt eine awaitable-Animation. Die entscheidende-Klasse, die zurückgegeben werden kann eine `Task` Objekt aus der Methode und Signal nach Abschluss die Animation ist [ `TaskCompletionSource` ](xref:System.Threading.Tasks.TaskCompletionSource`1).

## <a name="deeper-into-animations"></a>Tiefer in Animationen

Das Animationssystem Xamarin.Forms kann etwas verwirrend sein. Zusätzlich zu den `Easing` -Klasse, die das Animationssystem besteht aus den `ViewExtensions`, `Animation`, und `AnimationExtension` Brandverhaltensklassen.

### <a name="viewextensions-class"></a>ViewExtensions-Klasse

Sie haben bereits gesehen [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions). Es definiert neun Methoden, die zurückgeben `Task<bool>` und [ `CancelAnimations` ](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)). Sieben des Ziels neun Methoden wandeln Sie Eigenschaften. Die anderen zwei sind [ `FadeTo` ](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), die Ziele der [ `Opacity` ](xref:Xamarin.Forms.VisualElement.Opacity) -Eigenschaft und [ `LayoutTo` ](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), welche Aufrufe die [ `Layout` ](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) Methode.

### <a name="animation-class"></a>Animation-Klasse

Die [ `Animation` ](xref:Xamarin.Forms.AnimationExtensions) -Klasse verfügt über eine [Konstruktor](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action)) mit fünf Argumente zum Rückruf und fertig Methoden und Parameter, der die Animation definieren.

Untergeordneten Animationen hinzugefügt werden können, mit [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)), [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)), [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)), und und Überladung der [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)).

Das Animationsobjekt wird dann gestartet, durch einen Aufruf der [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) Methode.

### <a name="animationextensions-class"></a>AnimationExtensions-Klasse

Die [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) Klasse enthält hauptsächlich Erweiterungsmethoden. Es gibt mehrere Versionen von einer `Animate` -Methode und die generische [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) -Methode ist, dass es wirklich die einzige Animationsfunktion Sie müssen also vielseitig verwendbar.

## <a name="working-with-the-animation-class"></a>Arbeiten mit der Animation-Klasse

Die [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) Beispiel veranschaulicht die [ `Animation` ](xref:Xamarin.Forms.Animation) Klasse mit mehreren verschiedenen Animationen.

### <a name="child-animations"></a>Untergeordneten Animationen

Die [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) Beispiel zeigt auch die untergeordneten Animationen, wodurch die (ähnlich) nutzen [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) und [ `Insert` ](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) Methoden.

### <a name="beyond-the-high-level-animation-methods"></a>Außer den allgemeinen Animation-Methoden

Die [ **ConcurrentAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) Beispiel veranschaulicht auch, wie Animationen durchzuführen, die die Zielgruppe der Eigenschaften von hinausgehen der `ViewExtensions` Methoden. In einem Beispiel eine Reihe von Punkten länger erhalten; in einem anderen Beispiel einem `BackgroundColor` Eigenschaft animiert wird.

### <a name="more-of-your-own-awaitable-methods"></a>Eigene awaitable-Methoden

Die [ `TranslateTo` ](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) -Methode der `ViewExtensions` funktioniert nicht mit der [ `Easing.SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) Funktion. Es wird beendet, wenn. die Ads Ausgabe größer als 1 abruft.

Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek enthält eine [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) Klasse mit [ `TranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) und [ `TranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49) Erweiterungsmethoden, die dieses Problem aufweisen, nicht als auch [ `CancelTranslateXTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) und [ `CancelTranslateYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) Methoden für die Abbrechen Animationen.

Die [ **SpringSlidingEntrance** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) veranschaulicht die `TranslateXTo` Methode.

Die `MoreExtensions` Klasse enthält auch eine [ `TranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76) -Erweiterungsmethode, die X- und Y-Übersetzung, kombiniert und eine [ `CancelTranslateXYTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113) Methode.

### <a name="implementing-a-bezier-animation"></a>Implementieren einer Bezier-animation

Es ist auch möglich, eine Animation zu entwickeln, die ein Element entlang des Pfads, der einen Bezier-Spline verschiebt. Die [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) -Bibliothek enthält eine [ `BezierSpline` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs) Struktur, die einen Bezier-Spline kapselt und [ `BezierTangent` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs) Enumeration in die steuerelementausrichtung.

Die [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) -Klasse enthält eine [ `BezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118) Erweiterungsmethode und ein [ `CancelBezierPathTo` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161) Methode.

Die [ **BezierLoop** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) Beispiel wird veranschaulicht, ein Element entlang eines Pfads Beizer animieren.

## <a name="working-with-animationextensions"></a>Arbeiten mit AnimationExtensions

Eine Art von Animation fehlt in der Standardsammlung ist eine Farbe-Animation. Das Problem ist, dass es keine richtige Weg interpoliert zwischen zwei `Color` Werte. Es ist möglich, die die einzelnen RGB-Werte zu interpolieren, aber nur als gültig ist die HSL-Werte interpolieren.

Aus diesem Grund die [ `MoreViewExtensions` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs) -Klasse in der [ **Xamarin.FormsBook.Toolkit** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit) Bibliothek enthält zwei `Color` Animation-Methoden: [ `RgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166)und [ `HslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Es gibt auch zwei Abbruchmethoden: [ `CancelRgbColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) und [ `CancelHslColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Beide Methoden verwenden [ `ColorAnimation` ](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211), die die Animation durch Aufrufen den generischen umfangreichen ausführt [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) -Methode in der [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions).

Die [ **ColorAnimations** ](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) Beispiel zeigt die Verwendung dieser beiden Arten von Animationen mit Farbe.

## <a name="structuring-your-animations"></a>Strukturieren Ihre Animationen

Manchmal ist es nützlich, um die express-Animationen in XAML, und deren Verwendung in Verbindung mit MVVM. Dies wird im nächsten Kapitel behandelt [ **Kapitel 23. Trigger und Verhaltensweisen**](chapter23.md).



## <a name="related-links"></a>Verwandte Links

- [Kapitel 22 Volltext (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Kapitel 22-Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animation](~/xamarin-forms/user-interface/animation/index.md)
