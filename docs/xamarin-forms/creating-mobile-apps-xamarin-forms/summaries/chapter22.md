---
title: Zusammenfassung von Kapitel 22. Animation
description: 'Erstellen von mobilen Apps mit Xamarin.Forms: Zusammenfassung von Kapitel 22. Animation'
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 47C2B9AB-E688-4412-8AF5-9F633B3DA695
author: davidbritch
ms.author: dabritch
ms.date: 11/07/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2a8a089c210a3fe2f48dbe32bf8cda6179af2a78
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84136629"
---
# <a name="summary-of-chapter-22-animation"></a>Zusammenfassung von Kapitel 22. Animation

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)

Sie haben bereits erfahren, dass Sie eigene Animationen mit dem Xamarin.Forms-Timer oder `Task.Delay` erstellen können. In der Regel ist es jedoch einfacher, die von Xamarin.Forms bereitgestellten Animationsfunktionen zu verwenden. Diese Animationen sind in drei Klassen implementiert:

- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions), der allgemeine Ansatz
- [`Animation`](xref:Xamarin.Forms.Animation), ein vielseitigerer, jedoch komplexerer Ansatz
- [`AnimationExtension`](xref:Xamarin.Forms.AnimationExtensions), der vielseitigste, aber auch detailreichste Ansatz

Im Allgemeinen betreffen Animationen Eigenschaften, die von bindbaren Eigenschaften gestützt werden. Dies ist keine Voraussetzung, aber diese Eigenschaften sind die einzigen Eigenschaften, die dynamisch auf Änderungen reagieren.

Es gibt keine XAML-Schnittstelle für diese Animationen. Sie können Animationen jedoch in XAML integrieren, indem Sie die in [**Kapitel 23: Trigger und Verhaltensweisen**](chapter23.md) beschriebenen Schritte ausführen.

## <a name="exploring-basic-animations"></a>Grundlegende Animationen

Bei den grundlegenden Animationsfunktionen handelt es sich um Erweiterungsmethoden in der Klasse [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions). Diese Methoden gelten für alle Objekte, die von `VisualElement` abgeleitet werden. Die einfachsten Animationen betreffen die in [`Chapter 21. Transforms`](chapter21.md) erörterten Transformationseigenschaften.

Unter [**AnimationTryout**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/AnimationTryout) wird veranschaulicht, wie der `Clicked`-Ereignishandler für ein `Button`-Element die Erweiterungsmethode [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) aufrufen kann, um die Schaltfläche im Kreis zu drehen.

Die `RotateTo`-Methode ändert die `Rotation`-Eigenschaft von `Button` innerhalb einer Viertelsekunde (Standardeinstellung) von 0 in 360. Wenn erneut auf den `Button` getippt wird, passiert nichts, da die Eigenschaft `Rotation` bereits den Wert von 360 Grad erreicht hat.

### <a name="setting-the-animation-duration"></a>Festlegen der Animationsdauer

Das zweite Argument für `RotateTo` ist eine Dauer in Millisekunden. Wenn für dieses Argument ein hoher Wert festgelegt wird, wird beim Tippen auf `Button` während einer Animation eine neue Animation gestartet (die beim aktuellen Winkel beginnt).

### <a name="relative-animations"></a>Relative Animationen

Die `RelRotateTo`-Methode führt eine relative Drehung durch, indem dem vorhandenen Wert ein weiterer Wert hinzugefügt wird. Mit dieser Methode kann `Button` mehrmals abgetippt werden kann, und jedes Mal wird der Wert der `Rotation`-Eigenschaft um 360 Grad erhöht.

### <a name="awaiting-animations"></a>Aufeinanderfolgende Animationen

Alle Animationsmethoden in `ViewExtensions` geben `Task<bool>`-Objekte zurück. Sie können also mit `ContinueWith` oder `await` eine Reihe aufeinanderfolgender Animationen definieren. Wenn die Animation ohne Unterbrechung beendet wurde, lautet der `bool`-Rückgabewert `false`. Wenn die Animation von der Methode [`CancelAnimation`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) abgebrochen wurde, lautet er `true`. Diese Methode bricht alle Animationen ab, die von der anderen Methode in `ViewExtensions` gestartet wurden und für dasselbe Element festgelegt sind.

### <a name="composite-animations"></a>Kombinieren von Animationen

Sie können erwartete und nicht erwartete Animationen kombinieren. Dabei handelt es sich um die Animationen in `ViewExtensions`, die die Transformationseigenschaften `TranslationX`, `TranslationY` und `Scale` betreffen:

- [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`ScaleTo`](xref:Xamarin.Forms.ViewExtensions.ScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))
- [`RelScaleTo`](xref:Xamarin.Forms.ViewExtensions.RelScaleTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))

Beachten Sie, dass `TranslateTo` sich potenziell sowohl auf `TranslationX` als auch auf `TranslationY` auswirkt.

### <a name="taskwhenall-and-taskwhenany"></a>„Task.WhenAll“ und „Task.WhenAny“

Sie können gleichzeitige Animationen auch mithilfe von [`Task.WhenAll`](xref:System.Threading.Tasks.Task.WhenAll*) verwalten. Damit wird angezeigt, wenn mehrere Aufgaben ausgeführt wurden. Auch die Verwendung von [`Task.WhenAny`](xref:System.Threading.Tasks.Task.WhenAny*) ist möglich. Damit wird angezeigt, wenn die erste Aufgabe aus einer Reihe von Aufgaben abgeschlossen wurde.

### <a name="rotation-and-anchors"></a>Drehungen und Anker

Beim Aufruf der Methoden `ScaleTo`, `RelScaleTo`, `RotateTo` und `RelRotateTo` können Sie die Eigenschaften [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX) und [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY) festlegen, um den Mittelpunkt für die Skalierung und Drehung festzulegen.

Diese Vorgehensweise ist im Beispiel [**CircleButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CircleButton) gezeigt, in dem ein `Button` in der Seitenmitte gedreht wird.

### <a name="easing-functions"></a>Beschleunigungsfunktionen

Im Allgemeinen sind Animationen linear (von einem Startwert zu einem Endwert). Beschleunigungsfunktionen können dazu führen, dass Animationen während ihres Verlaufs beschleunigt oder verlangsamt werden. Das letzte optionale Argument für die Animationsmethoden ist vom Typ [`Easing`](xref:Xamarin.Forms.Easing) – eine Klasse, die 11 statische schreibgeschützte Felder vom Typ `Easing` definiert:

- [`Linear`](xref:Xamarin.Forms.Easing.Linear), der Standardwert
- [`SinIn`](xref:Xamarin.Forms.Easing.SinIn), [`SinOut`](xref:Xamarin.Forms.Easing.SinOut) und [`SinInOut`](xref:Xamarin.Forms.Easing.SinInOut)
- [`CubicIn`](xref:Xamarin.Forms.Easing.CubicIn), [`CubicOut`](xref:Xamarin.Forms.Easing.CubicOut) und [`CubicInOut`](xref:Xamarin.Forms.Easing.CubicInOut)
- [`BounceIn`](xref:Xamarin.Forms.Easing.BounceIn) und [`BounceOut`](xref:Xamarin.Forms.Easing.BounceOut)
- [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) und [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut)

Das Suffix `In` gibt an, dass der Effekt am Anfang der Animation auftritt, `Out` bedeutet, dass der Effekt am Ende auftritt, und `InOut` bedeutet, dass der Effekt am Anfang und am Ende auftritt.

Die Verwendung von Beschleunigungsfunktionen wird im Beispiel [**BounceButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BounceButton) veranschaulicht.

### <a name="your-own-easing-functions"></a>Eigene Beschleunigungsfunktionen

Sie können auch eigene Beschleunigungsfunktionen definieren, indem Sie [`Func<double, double>`](xref:System.Func`2) an den [`Easing`-Konstruktor](xref:Xamarin.Forms.Easing.%23ctor(System.Func{System.Double,System.Double})) übergeben. `Easing` definiert zudem eine implizite Konvertierung von `Func<double, double>` nach `Easing`. Das Argument für die Beschleunigungsfunktion liegt immer im Bereich 0 bis 1, da die Animation von Anfang bis Ende linear verläuft. Die Funktion gibt *in der Regel* einen Wert im Bereich 0 bis 1 zurück, kann jedoch auch kurz negativ oder größer als 1 sein (wie dies bei den Funktionen `SpringIn` und `SpringOut` der Fall ist). Es ist auch möglich, die Regeln zu umgehen, wenn Sie über ausreichende Kenntnisse verfügen.

In den Beispielen [**UneasyScale**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/UneasyScale) und [**CustomCubicEase**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CustomCubicEase) werden benutzerdefinierte Beschleunigungsfunktionen veranschaulicht.

Das Beispiel [**SwingButton**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingButton) zeigt ebenfalls eine benutzerdefinierte Beschleunigungsfunktion sowie ein Verfahren zum Ändern der Eigenschaften `AnchorX` und `AnchorY` innerhalb einer Sequenz von Drehanimationen.

Die [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek umfasst eine Klasse [`JiggleButton`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/JiggleButton.cs), die eine benutzerdefinierte Beschleunigungsfunktion verwendet, um eine Schaltfläche zu rütteln, wenn ein Benutzer darauf klickt. Dies wird im Beispiel [**JiggleButtonDemo**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/JiggleButtonDemo) veranschaulicht.

### <a name="entrance-animations"></a>Eingangsanimationen

Eine beliebte Art von Animation kommt zum Einsatz, wenn eine Seite zum ersten Mal angezeigt wird. Eine solche Animation kann in der [`OnAppearing`](xref:Xamarin.Forms.Page.OnAppearing)-Außerkraftsetzung der Seite gestartet werden. Für diese Animationen sollten Sie die XAML zunächst so einrichten, wie die Seite *nach* der Animation erscheinen soll. Anschließend sollten Sie das Layout über Code initialisieren und animieren.

Im Beispiel [**FadingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingEntrance) wird die Erweiterungsmethode [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) verwendet, um den Inhalt der Seite einzublenden.

Im Beispiel [**SlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SlidingEntrance) wird die Erweiterungsmethode [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) verwendet, um den Seiteninhalt von der Seite einzuschieben.

Im Beispiel [**SwingingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SwingingEntrance) wird die Erweiterungsmethode [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) verwendet, um die Eigenschaft `RotationY` zu animieren. Eine [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing))-Methode ist ebenfalls verfügbar.

### <a name="forever-animations"></a>Endlose Animationen

Eine weitere Art von Animationen sind „endlose Animationen“, die bis zum Programmende laufen. Diese Animationen dienen üblicherweise zu Demonstrationszwecken.

Im Beispiel [**FadingTextAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/FadingTextAnimation) wird die Animation [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) verwendet, um zwei Textteile ein- und auszublenden.

[**PalindromeAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/PalindromeAnimation) zeigt ein Palindrom, bei dem die einzelnen Buchstaben nacheinander um 180 Grad gedreht werden, bis sie alle auf dem Kopf stehen. Die gesamte Zeichenfolge wird dann um 180 Grad gedreht, um wieder der ursprünglichen Zeichenfolge zu entsprechen.

Im Beispiel [**CopterAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/CopterAnimation) wird ein einfacher `BoxView`-Hubschrauber um den Bildschirmmittelpunkt gedreht.

In [**RotatingSpokes**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotatingSpokes) werden `BoxView`-Speichen um den Bildschirmmittelpunkt gedreht. Anschließend drehen sich die einzelnen Speichen um sich selbst, um neue Muster zu erstellen:

[![Screenshot von sich drehenden Speichen](images/ch22fg21-small.png "Rotierende Speichen")](images/ch22fg21-large.png#lightbox "Rotierende Speichen")

Möglicherweise ist es langfristig jedoch nicht möglich, den Wert der `Rotation`-Eigenschaft für ein Element zu erhöhen, wie im Beispiel [**RotationBreakdown**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/RotationBreakdown) gezeigt.

Im Beispiel [**SpinningImage**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpinningImage) werden [`RotateTo`](xref:Xamarin.Forms.ViewExtensions.RotateTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), [`RotateXTo`](xref:Xamarin.Forms.ViewExtensions.RotateXTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) und [`RotateYTo`](xref:Xamarin.Forms.ViewExtensions.RotateYTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)) verwendet, um eine Bitmap augenscheinlich im 3D-Raum rotieren zu lassen.

### <a name="animating-the-bounds-property"></a>Animieren der Bounds-Eigenschaft

Die einzige Erweiterungsmethode in `ViewExtensions`, die noch nicht veranschaulicht wurde, ist [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)). Damit wird die schreibgeschützte Eigenschaft [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds) durch Aufrufen der Methode [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) animiert. Sie wird üblicherweise durch `Layout`-Ableitungen aufgerufen, wie in [**Kapitel 26: Benutzerdefinierte Layouts**](chapter26.md) erläutert.

Die `LayoutTo`-Methode sollte nur für besondere Zwecke verwendet werden. Im Programm [**BouncingBox**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BouncingBox) wird die Methode verwendet, um ein `BoxView`-Element zusammenzudrücken und auseinanderzuziehen, wenn dieses über die Ränder einer Seite hinausgeht.

Im Beispiel [**XamagonXuzzle**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/XamagonXuzzle) wird `LayoutTo` verwendet, um die Kacheln in einer Implementierung eines klassischen Schiebepuzzles zu verschieben, das anstelle von nummerierten Kacheln ein Bild zeigt:

[![Screenshot von Xamarin Xuzzle](images/ch22fg26-small.png "Xuzzle-Schieberätsel")](images/ch22fg26-large.png#lightbox "Xuzzle-Schieberätsel")

### <a name="your-own-awaitable-animations"></a>Eigene Awaitable-Animationen

Mit dem Beispiel [**TryAwaitableAnimation**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/TryAwaitableAnimation) wird eine Awaitable-Animation erstellt. Die entscheidende Klasse, mit der über die Methode ein `Task`-Objekt zurückgegeben und signalisiert werden kann, wenn die Animation abgeschlossen ist, ist [`TaskCompletionSource`](xref:System.Threading.Tasks.TaskCompletionSource`1).

## <a name="deeper-into-animations"></a>Weitere Informationen zu Animationen

Das Animationssystem von Xamarin.Forms kann mitunter verwirrend sein. Neben der Klasse `Easing` umfasst das System die Klassen `ViewExtensions`, `Animation` und `AnimationExtension`.

### <a name="viewextensions-class"></a>Die Klasse „ViewExtensions“

Sie haben die Klasse [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) bereits kennengelernt. Sie definiert neun Methoden, die `Task<bool>` und [`CancelAnimations`](xref:Xamarin.Forms.ViewExtensions.CancelAnimations(Xamarin.Forms.VisualElement)) ausführen. Sieben der neun Methoden betreffen Transformationseigenschaften. Die anderen beiden sind [`FadeTo`](xref:Xamarin.Forms.ViewExtensions.FadeTo(Xamarin.Forms.VisualElement,System.Double,System.UInt32,Xamarin.Forms.Easing)), die die Eigenschaft [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity) betrifft, und [`LayoutTo`](xref:Xamarin.Forms.ViewExtensions.LayoutTo(Xamarin.Forms.VisualElement,Xamarin.Forms.Rectangle,System.UInt32,Xamarin.Forms.Easing)), mit der die Methode [`Layout`](xref:Xamarin.Forms.VisualElement.Layout(Xamarin.Forms.Rectangle)) aufgerufen wird.

### <a name="animation-class"></a>Animationsklasse

Die Klasse [`Animation`](xref:Xamarin.Forms.AnimationExtensions) verfügt über einen [Konstruktor](xref:Xamarin.Forms.Animation.%23ctor(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Action)) mit fünf Argumenten, die den Rückruf und die abgeschlossenen Methoden definieren, sowie über Parameter der Animation.

Untergeordnete Animationen können mit [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)), [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)), [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) und einer Überladung von [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(System.Action{System.Double},System.Double,System.Double,Xamarin.Forms.Easing,System.Double,System.Double)) hinzugefügt werden.

Das Animationsobjekt wird dann mit einem Aufruf der Methode [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) gestartet.

### <a name="animationextensions-class"></a>Die Klasse „AnimationExtensions“

Die Klasse [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) enthält überwiegend Erweiterungsmethoden. Da es mehrere Versionen einer `Animate`-Methode gibt und die generische [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*)-Methode überaus vielseitig ist, ist sie die einzige Animationsfunktion, die Sie wirklich benötigen.

## <a name="working-with-the-animation-class"></a>Arbeiten mit der Animationsklasse

Im Beispiel [**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) wird die Klasse [`Animation`](xref:Xamarin.Forms.Animation) mit einer Vielzahl unterschiedlicher Animationen veranschaulicht.

### <a name="child-animations"></a>Untergeordnete Animationen

Im Beispiel [**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) werden auch untergeordnete Animationen veranschaulicht, die die (sehr ähnlichen) Methoden [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) and [`Insert`](xref:Xamarin.Forms.Animation.Insert(System.Double,System.Double,Xamarin.Forms.Animation)) verwenden.

### <a name="beyond-the-high-level-animation-methods"></a>Weiterreichende Animationsmethoden

Im Beispiel [**ConcurrentAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ConcurrentAnimations) wird zudem gezeigt, wie Animationen implementiert werden, die über die Eigenschaften hinausgehen, die von den `ViewExtensions`-Methoden betroffen sind. In einem Beispiel wird eine Abfolge fortgesetzt, in einem anderen Beispiel wird eine `BackgroundColor`-Eigenschaft animiert.

### <a name="more-of-your-own-awaitable-methods"></a>Weitere Informationen zu eigenen Awaitable-Methoden

Die Methode [`TranslateTo`](xref:Xamarin.Forms.ViewExtensions.TranslateTo(Xamarin.Forms.VisualElement,System.Double,System.Double,System.UInt32,Xamarin.Forms.Easing)) von `ViewExtensions` funktioniert nicht mit der Funktion [`Easing.SpringOut`](xref:Xamarin.Forms.Easing.SpringOut). Sie wird beendet, wenn die Beschleunigungsausgabe den Wert 1 überschreitet.

Die [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek umfasst eine [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)-Klasse mit den Erweiterungsmethoden [`TranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L12) und [`TranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L49), bei denen dieses Problem nicht auftritt. Außerdem sind die Methoden [`CancelTranslateXTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L44) und [`CancelTranslateYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L71) enthalten, mit denen diese Animationen abgebrochen werden können.

Die Methode `TranslateXTo` wird im Beispiel [**SpringSlidingEntrance**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/SpringSlidingEntrance) veranschaulicht.

Die Klasse `MoreExtensions` umfasst zudem eine [`TranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L76)-Erweiterungsmethode, mit der die X- und Y-Verschiebung kombiniert wird, sowie eine [`CancelTranslateXYTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L113)-Methode.

### <a name="implementing-a-bezier-animation"></a>Implementieren einer Bézier-Animation

Es ist auch möglich, eine Animation zu entwickeln, bei der ein Element entlang einer Béziersplinekurve verschoben wird. Die [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek umfasst eine [`BezierSpline`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierSpline.cs)-Struktur, die eine Béziersplinekurve kapselt, sowie eine [`BezierTangent`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/BezierTangent.cs)-Enumeration zum Festlegen der Ausrichtung.

Die [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)-Klasse enthält eine [`BezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L118)-Erweiterungsmethode und eine [`CancelBezierPathTo`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L161)-Methode.

Im Beispiel [**BezierLoop**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/BezierLoop) wird gezeigt, wie ein Element entlang einer Béziersplinekurve animiert wird.

## <a name="working-with-animationextensions"></a>Arbeiten mit AnimationExtensions

Eine noch fehlende Animation der Standardsammlung ist die Farbanimation. Das Problem besteht darin, dass es keine optimale Möglichkeit für die Interpolation zwischen zwei `Color`-Werten gibt. Es ist möglich, die einzelnen RGB-Werte zu interpolieren, aber genauso zulässig ist es, die HSL-Werte zu interpolieren.

Aus diesem Grund umfasst die [`MoreViewExtensions`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs)-Klasse in der [**Xamarin.FormsBook.Toolkit**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Libraries/Xamarin.FormsBook.Toolkit)-Bibliothek zwei `Color`-Animationsmethoden: [`RgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L166) und [`HslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L188). (Es gibt auch zwei Methoden zum Abbrechen einer Animation: [`CancelRgbColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L183) und [`CancelHslColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L206)).

Beide Methoden verwenden [`ColorAnimation`](https://github.com/xamarin/xamarin-forms-book-samples/blob/master/Libraries/Xamarin.FormsBook.Toolkit/Xamarin.FormsBook.Toolkit/MoreViewExtensions.cs#L211) für die Animation, indem die umfangreiche generische [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*)-Methode in [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) aufgerufen wird.

Das Beispiel [**ColorAnimations**](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22/ColorAnimations) veranschaulicht, wie diese beiden Arten von Farbanimationen verwendet werden.

## <a name="structuring-your-animations"></a>Strukturieren Ihrer Animationen

In manchen Fällen kann es sinnvoll sein, Animationen in XAML auszudrücken und sie gemeinsam mit MVVM zu verwenden. Dieses Thema wird in [**Kapitel 23: Trigger und Verhaltensweisen**](chapter23.md) besprochen.

## <a name="related-links"></a>Verwandte Links

- [Kapitel 22 – vollständiger Text (PDF)](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf)
- [Kapitel 22 – Beispiele](https://github.com/xamarin/xamarin-forms-book-samples/tree/master/Chapter22)
- [Animation](~/xamarin-forms/user-interface/animation/index.md)
