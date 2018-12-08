---
title: Benutzerdefinierte Animationen in Xamarin.Forms
description: In diesem Artikel wird veranschaulicht, wie Sie mithilfe der Xamarin.FOrms-Animation-Klasse erstellen und Abbrechen von Animationen, mehrere Animationen zu synchronisieren, und erstellen benutzerdefinierte Animationen, das Animieren von Eigenschaften, die durch die vorhandenen Methoden für die Animation animiert werden nicht.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 157e044fd96cdeff87d8fb56029fe625b7312bf4
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53056239"
---
# <a name="custom-animations-in-xamarinforms"></a>Benutzerdefinierte Animationen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)

_Die Animation-Klasse ist der Baustein von alle Xamarin.Forms-Animationen, mit der Erweiterungsmethoden in der ViewExtensions-Klasse, die ein oder mehrere Animation-Objekte erstellen. In diesem Artikel wird veranschaulicht, wie die Animation-Klasse erstellt und Abbrechen von Animationen, mehrere Animationen zu synchronisieren, und erstellen benutzerdefinierte Animationen, das Animieren von Eigenschaften, die durch die vorhandenen Methoden für die Animation animiert werden nicht wird._


Eine Reihe von Parametern angegeben werden, beim Erstellen einer `Animation` -Objekts, einschließlich der Start- und enduhrzeitwerten der zu animierenden Eigenschaft und einen Rückruf, der der Wert der Eigenschaft geändert wird. Ein `Animation` Objekt kann auch eine Auflistung der untergeordneten Animationen, die ausgeführt und synchronisiert werden kann verwalten. Weitere Informationen finden Sie unter [untergeordneten Animationen](#child).

Ausführen einer Animation erstellt, mit der [ `Animation` ](xref:Xamarin.Forms.Animation) -Klasse, die möglicherweise oder umfassen möglicherweise nicht die untergeordneten Animationen, erfolgt durch Aufrufen der [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) Methode. Diese Methode gibt die Dauer der Animation und zwischen anderen Elementen ein Rückruf, der steuert, ob die Animation wiederholt werden soll.

## <a name="creating-an-animation"></a>Erstellen einer Animation

Beim Erstellen einer [ `Animation` ](xref:Xamarin.Forms.Animation) Objekt, in der Regel mindestens drei Parameter sind erforderlich, wie im folgenden Codebeispiel gezeigt:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Dieser Code definiert eine Animation von dem [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eine [ `Image` ](xref:Xamarin.Forms.Image) Instanz aus einem Wert von 1 auf den Wert 2. An den Rückruf, der als erstes Argument angegeben der animierte Wert, der von Xamarin.Forms abgeleitet ist, übergeben wird, wird zum Ändern des Werts, der die `Scale` Eigenschaft.

Die Animation wird gestartet, durch einen Aufruf der [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) -Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Beachten Sie, dass die [ `Commit` ](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) gibt keine Methode zurück. ein `Task` Objekt. Stattdessen werden Benachrichtigungen über Rückrufmethoden bereitgestellt.

Die folgenden Argumente angegeben werden, der `Commit` Methode:

- Das erste Argument (*Besitzer*) identifiziert den Besitzer der Animation. Dies kann sein, das visuelle Element, das auf dem die Animation angewendet wird, oder ein anderes visuelles Element, wie die Seite.
- Das zweite Argument (*Namen*) die Animation mit einem Namen identifiziert. Der Name wird mit dem Besitzer zur eindeutigen Identifizierung die Animation kombiniert. Dieser eindeutige Kennung kann dann verwendet werden, um zu bestimmen, ob die Animation ausgeführt wird ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))), oder es abzubrechen ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- Das dritte Argument (*Rate*) gibt die Anzahl der Millisekunden zwischen den Aufrufen an die Rückrufmethode, die definiert, der [ `Animation` ](xref:Xamarin.Forms.Animation) Konstruktor
- Das vierte Argument (*Länge*) gibt die Dauer der Animation, in Millisekunden.
- Das fünfte Argument (*Beschleunigungsfunktion*) definiert die Beschleunigungsfunktion an, in der Animation verwendet werden. Alternativ kann die Beschleunigungsfunktion angegeben werden, als Argument an die [ `Animation` ](xref:Xamarin.Forms.Animation) Konstruktor. Weitere Informationen zu Beschleunigungsfunktionen, finden Sie unter [einfachere Funktionen](~/xamarin-forms/user-interface/animation/easing.md).
- Das sechste Argument (*abgeschlossen*) ist ein Rückruf, der ausgeführt wird, wenn die Animation abgeschlossen wurde. Dieser Rückruf akzeptiert zwei Argumente, mit dem ersten Argument, das angibt, einen endgültigen Wert und das zweite Argument wird ein `bool` , nastaven NA hodnotu `true` , wenn die Animation abgebrochen wurde. Sie können auch die *abgeschlossen* Rückruf kann angegeben werden, als Argument an die [ `Animation` ](xref:Xamarin.Forms.Animation) Konstruktor. Allerdings mit einer einzelnen Animation Wenn *abgeschlossen* Rückrufe in beide angegeben werden die `Animation` Konstruktor und die `Commit` -Methode, nur die im angegebenen Rückruf der `Commit` -Methode ausgeführt wird.
- Das siebte Argument (*wiederholen*) ist ein Rückruf, der die Animation wiederholt werden kann. Sie wird am Ende der Animation, aufgerufen und die Rückgabe `true` gibt an, dass die Animation wiederholt werden soll.

Der Gesamteffekt besteht eine Animation erstellt, das erhöht die [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft eine [ `Image` ](xref:Xamarin.Forms.Image) zwischen 1 und 2, mehr als 2 Sekunden (2000 Millisekunden), mit der [ `Linear` ](xref:Xamarin.Forms.Easing.Linear) Beschleunigungsfunktion. Jedes Mal der Animation abgeschlossen wird, dessen `Scale` Eigenschaft auf 1 zurückgesetzt, und Wiederholen der Animation.

> [!NOTE]
> Gleichzeitige Animationen, die unabhängig voneinander ausführbar konstruiert werden können, durch das Erstellen einer `Animation` Objekt für jede Animation und dem anschließenden Aufrufen der `Commit` Methode für jede Animation.

<a name="child" />

### <a name="child-animations"></a>Untergeordneten Animationen

Die [ `Animation` ](xref:Xamarin.Forms.Animation) -Klasse unterstützt außerdem untergeordneten Animationen, die umfasst das Erstellen einer `Animation` Objekt, das die von anderen `Animation` Objekte werden hinzugefügt. Dies ermöglicht eine Reihe von Animationen zum Ausführen und synchronisiert werden. Im folgenden Codebeispiel veranschaulicht das Erstellen und die untergeordneten Animationen ausgeführt:

```csharp
var parentAnimation = new Animation ();
var scaleUpAnimation = new Animation (v => image.Scale = v, 1, 2, Easing.SpringIn);
var rotateAnimation = new Animation (v => image.Rotation = v, 0, 360);
var scaleDownAnimation = new Animation (v => image.Scale = v, 2, 1, Easing.SpringOut);

parentAnimation.Add (0, 0.5, scaleUpAnimation);
parentAnimation.Add (0, 1, rotateAnimation);
parentAnimation.Add (0.5, 1, scaleDownAnimation);

parentAnimation.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

Alternativ kann das Codebeispiel präziser, wie im folgenden Codebeispiel veranschaulicht geschrieben werden:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

In beiden Codebeispiele, die ein übergeordnetes Element [ `Animation` ](xref:Xamarin.Forms.Animation) -Objekt erstellt wird, auf die zusätzlichen `Animation` Objekte werden hinzugefügt. Die ersten beiden Argumente für die [ `Add` ](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) Methode festlegen, wann zu beginnen und beenden Sie die untergeordneten Animationen. Die Argumentwerte müssen zwischen 0 und 1 sein und Darstellung der relative Zeitraum innerhalb der übergeordneten Animation, dass die angegebene untergeordnete Animation aktiv sein werden. Aus diesem Grund in diesem Beispiel die `scaleUpAnimation` aktiv für die erste Hälfte der Animation, die `scaleDownAnimation` aktiv für die zweite Hälfte der Animation, und die `rotateAnimation` wird für die gesamte Laufzeit aktiv sein.

Der Gesamteffekt besteht, dass die Animation über mehr als 4 Sekunden (4000 in Millisekunden) auftritt. Die `scaleUpAnimation` animiert die [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft von 1, 2, mehr als 2 Sekunden. Die `scaleDownAnimation` Animation, bei der `Scale` Eigenschaft von 2 auf 1 über 2 Sekunden. Während beide Animationen für die Skalierung ausgeführt werden, die `rotateAnimation` animiert die [ `Rotation` ](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft zwischen 0 und 360 liegen, 4 Sekunden. Beachten Sie, dass die Skalierung Animationen auch Beschleunigungsfunktionen verwenden. Die [ `SpringIn` ](xref:Xamarin.Forms.Easing.SpringIn) Beschleunigungsfunktion bewirkt, dass die [ `Image` ](xref:Xamarin.Forms.Image) anfänglich verkleinert, bevor Sie größer werden, und die [ `SpringOut` ](xref:Xamarin.Forms.Easing.SpringOut) EasingFunction bewirkt, dass die `Image` kleiner als die tatsächliche Größe am Ende der Animation abgeschlossen wird.

Es gibt eine Reihe von Unterschieden zwischen einem [ `Animation` ](xref:Xamarin.Forms.Animation) Objekt, das untergeordneten Animationen verwendet, und eine, die nicht:

- Bei Verwendung von untergeordneten Animationen, die *abgeschlossen* Rückruf für einen untergeordneten Animation gibt an, wenn das untergeordnete Element abgeschlossen wurde, und die *abgeschlossen* Rückruf übergeben wird, um die `Commit` Methode gibt an, wann die gesamte Animation abgeschlossen wurde.
- Zurückgeben der Verwendung von untergeordneten Animationen `true` aus der *wiederholen* Rückruf für die `Commit` Methode führt nicht dazu, dass die Animation wiederholt werden soll, aber die Animation wird fortgesetzt, ohne neue Werte ausgeführt.
- Wenn eine Beschleunigungsfunktion in einschließlich der `Commit` -Methode, und die Beschleunigungsfunktion, zu denen einen Wert größer als 1 zurückgibt, wird die Animation beendet werden. Wenn die Beschleunigungsfunktion auf einen Wert kleiner als 0 zurückgibt, wird der Wert auf 0 festgelegt. Um eine Beschleunigungsfunktion zu verwenden, die einen Wert kleiner als 0 oder größer als 1 zurückgibt, muss er in einem untergeordneten Animationen, sondern in angegeben, die `Commit` Methode.

Die [ `Animation` ](xref:Xamarin.Forms.Animation) -Klasse enthält auch [ `WithConcurrent` ](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) Methoden, die zum Hinzufügen von untergeordneten Animationen zu einem übergeordneten Element verwendet werden können `Animation` Objekt. Allerdings ihre *beginnen* und *Fertig stellen* Argumentwerte werden nicht auf 0 bis 1 beschränkt, aber nur dieser Teil der untergeordneten Animation, die einen Bereich von 0 bis 1 entspricht, wird aktiv sein. Z. B. wenn ein `WithConcurrent` Methodenaufruf definiert eine untergeordnete-Animation, dessen Ziel eine [ `Scale` ](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft von 1 bis 6, jedoch mit *beginnen* und *Fertig stellen* Werte – 2 und 3 der *beginnen* entspricht der Wert-2 eine `Scale` Wert von 1, und die *Fertig stellen* entspricht der Wert 3 ein `Scale` Wert 6. Da die Werte außerhalb des Bereichs von 0 bis 1 keine Rolle in einer Animation, spielen die `Scale` Eigenschaft wird nur aus 3 bis 6 animiert werden.

## <a name="canceling-an-animation"></a>Abbrechen einer Animation

Eine Anwendung kann eine Animation mit einem Aufruf von Abbrechen der [ `AbortAnimation` ](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) Erweiterungsmethode, wie im folgenden Codebeispiel gezeigt:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Beachten Sie, dass Animationen durch eine Kombination aus den Animation-Besitzer und den Namen der Animation eindeutig identifiziert werden. Aus diesem Grund den Besitzer und den Namen zusammen angegeben die Animation ausgeführt werden muss, die Animation abzubrechen. Aus diesem Grund wird im Codebeispiel wird sofort die Animation, die mit dem Namen abgebrochen `SimpleAnimation` , der von der Seite gehört.

## <a name="creating-a-custom-animation"></a>Erstellen eine benutzerdefinierte Animation

Die bisher dargestellten Beispiele haben gezeigt Animationen, die gleichmäßig mit den Methoden in erreicht werden konnte die [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse. Jedoch den Vorteil der [ `Animation` ](xref:Xamarin.Forms.Animation) Klasse ist, dass es sich um den Zugriff auf die Callback-Methode, der ausgeführt wird besitzt, wenn der animierte Wert ändert. Dadurch wird den Rückruf, der alle gewünschten Animationen zu implementieren. Im folgenden Codebeispiel wird z. B. eine Animation die [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) Eigenschaft einer Seite durch Festlegung auf [ `Color` ](xref:Xamarin.Forms.Color) durch erstellt die [ `Color.FromHsla` ](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double))Methode mit Hue-Werte, die im Bereich von 0 bis 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Die resultierende Animation enthält, die Darstellung des Seitenhintergrunds über die Farben des Regenbogens Vorlauf.

Weitere Beispiele zum Erstellen komplexer Animationen, einschließlich einer Animation Bezier-Kurve finden Sie unter [Kapitel 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) von [Erstellen mobiler Apps mit Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Erstellen eine benutzerdefinierte Animation-Erweiterungsmethode

Die Erweiterungsmethoden in den [ `ViewExtensions` ](xref:Xamarin.Forms.ViewExtensions) Klasse Animieren einer Eigenschaft vom aktuellen Wert auf einen angegebenen Wert. Daher ist es schwierig zu erstellen, z. B. eine `ColorTo` Animation-Methode, die zum Animieren einer Farbe aus einem Wert in eine andere verwendet werden kann, da:

- Die einzige [ `Color` ](xref:Xamarin.Forms.Color) von definierte Eigenschaft der [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Klasse [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor), was nicht immer die gewünschte `Color` Eigenschaft zum animieren.
- Häufig auf den aktuellen Wert des einem [ `Color` ](xref:Xamarin.Forms.Color) -Eigenschaft ist [ `Color.Default` ](xref:Xamarin.Forms.Color.Default), was eine echte Farbe nicht, und der nicht in Berechnungen der Interpolation verwendet werden.

Die Lösung für dieses Problem besteht darin, nicht die `ColorTo` Methode als Ziel eine bestimmten [ `Color` ](xref:Xamarin.Forms.Color) Eigenschaft. Sie können stattdessen geschrieben werden, mit einer Rückrufmethode, die die interpolierten übergibt `Color` Wert an den Aufrufer. Darüber hinaus die Methode übernimmt starten und beenden `Color` Argumente.

Die `ColorTo` -Methode kann implementiert werden, als eine Erweiterungsmethode, die verwendet die [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) -Methode in der die [ `AnimationExtensions` ](xref:Xamarin.Forms.AnimationExtensions) Klasse, um ihre Funktionalität bereitzustellen. Grund hierfür ist die `Animate` Methode kann verwendet werden, um die Eigenschaften, die nicht vom Typ `double`, wie im folgenden Codebeispiel gezeigt:

```csharp
public static class ViewExtensions
{
  public static Task<bool> ColorTo(this VisualElement self, Color fromColor, Color toColor, Action<Color> callback, uint length = 250, Easing easing = null)
  {
    Func<double, Color> transform = (t) =>
      Color.FromRgba(fromColor.R + t * (toColor.R - fromColor.R),
                     fromColor.G + t * (toColor.G - fromColor.G),
                     fromColor.B + t * (toColor.B - fromColor.B),
                     fromColor.A + t * (toColor.A - fromColor.A));
    return ColorAnimation(self, "ColorTo", transform, callback, length, easing);
  }

  public static void CancelAnimation(this VisualElement self)
  {
    self.AbortAnimation("ColorTo");
  }

  static Task<bool> ColorAnimation(VisualElement element, string name, Func<double, Color> transform, Action<Color> callback, uint length, Easing easing)
  {
    easing = easing ?? Easing.Linear;
    var taskCompletionSource = new TaskCompletionSource<bool>();

    element.Animate<Color>(name, transform, callback, 16, length, easing, (v, c) => taskCompletionSource.SetResult(c));
    return taskCompletionSource.Task;
  }
}
```

Die [ `Animate` ](xref:Xamarin.Forms.AnimationExtensions.Animate*) Methode erfordert eine *transformieren* Argument, das eine Rückrufmethode ist. Die Eingabe dieser Rückruf ist immer eine `double` im Bereich von 0 bis 1. Aus diesem Grund die `ColorTo` Methode definiert seine eigenen Transformation `Func` , akzeptiert eine `double` Zahl zwischen 0 und 1 festgelegt ist, und gibt eine [ `Color` ](xref:Xamarin.Forms.Color) Wert, der dieser Wert entspricht. Die `Color` Wert wird berechnet, indem die Interpolation der [ `R` ](xref:Xamarin.Forms.Color.R), [ `G` ](xref:Xamarin.Forms.Color.G), [ `B` ](xref:Xamarin.Forms.Color.B), und [ `A` ](xref:Xamarin.Forms.Color.A) Werte der beiden angegebenen `Color` Argumente. Die `Color` Wert wird dann an die Rückrufmethode für die Anwendung auf eine bestimmte Eigenschaft übergeben.

Dieser Ansatz ermöglicht es der `ColorTo` Methode, um alle animieren [ `Color` ](xref:Xamarin.Forms.Color) -Eigenschaft, wie im folgenden Codebeispiel gezeigt:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

In diesem Codebeispiel wird die `ColorTo` Methode erstellt eine Animation die [ `TextColor` ](xref:Xamarin.Forms.Label.TextColor) und [ `BackgroundColor` ](xref:Xamarin.Forms.VisualElement.BackgroundColor) Eigenschaften eine [ `Label` ](xref:Xamarin.Forms.Label), die `BackgroundColor`Eigenschaft einer Seite, und die [ `Color` ](xref:Xamarin.Forms.BoxView.Color) Eigenschaft eine [ `BoxView` ](xref:Xamarin.Forms.BoxView).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel veranschaulicht, wie Sie mit der [ `Animation` ](xref:Xamarin.Forms.Animation) Klasse erstellen und Abbrechen von Animationen, mehrere Animationen zu synchronisieren, und erstellen benutzerdefinierte Animationen, das Animieren von Eigenschaften, die von der vorhandenen Animation animiert werden nicht -Methoden. Die `Animation` -Klasse ist der Baustein von alle Xamarin.Forms-Animationen.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Animationen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animation](xref:Xamarin.Forms.Animation)
- [AnimationExtensions](xref:Xamarin.Forms.AnimationExtensions)
