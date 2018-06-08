---
title: Benutzerdefinierte Animationen
description: Die Animation-Klasse ist der Baustein von allen Xamarin.Forms Animationen mit den Erweiterungsmethoden in den ViewExtensions-Klasse, erstellen eine oder mehrere Animationsobjekte. Dieser Artikel veranschaulicht, wie die Animation-Klasse zum Erstellen und "Abbrechen" Animationen mehrere Animationen zu synchronisieren, und Erstellen von benutzerdefinierten Animationen, die Eigenschaften zu animieren, die durch die vorhandenen Methoden für die Animation animiert werden nicht.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/14/2016
ms.openlocfilehash: 238268a3ad2b82494f1d096d0cbeba97edb90366
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34847783"
---
# <a name="custom-animations"></a>Benutzerdefinierte Animationen

_Die Animation-Klasse ist der Baustein von allen Xamarin.Forms Animationen mit den Erweiterungsmethoden in den ViewExtensions-Klasse, erstellen eine oder mehrere Animationsobjekte. Dieser Artikel veranschaulicht, wie die Animation-Klasse zum Erstellen und "Abbrechen" Animationen mehrere Animationen zu synchronisieren, und Erstellen von benutzerdefinierten Animationen, die Eigenschaften zu animieren, die durch die vorhandenen Methoden für die Animation animiert werden nicht._


Eine Anzahl von Parametern angegeben werden, für die Erstellung einer `Animation` -Objekt, einschließlich der Start- und Werte der zu animierenden Eigenschaft und einen Rückruf, der der Wert der Eigenschaft ändert. Ein `Animation` Objekt kann darüber hinaus zu einer Auflistung von untergeordneten Animationen, die ausgeführt und synchronisiert werden kann. Weitere Informationen finden Sie unter [untergeordneten Animationen](#child).

Ausführen einer Animation mit erstellt die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) -Klasse, die möglicherweise oder sind nicht untergeordneten Animationen, erfolgt durch Aufrufen der [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) Methode. Diese Methode gibt die Dauer der Animation von Initialisierungsausdrücken und andere Elemente, einen Rückruf, der steuert, ob die Animation wiederholt.

## <a name="creating-an-animation"></a>Erstellen einer Animation

Beim Erstellen einer [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) -Objekt, in der Regel mindestens drei Parameter sind erforderlich, wie im folgenden Codebeispiel gezeigt:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Dieser Code definiert eine Animation der [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) Instanz aus einem Wert von 1 auf den Wert 2. Übergeben der animierte-Wert, der durch Xamarin.Forms abgeleitet ist, an den Rückruf, der als erstes Argument angegeben wird zum Ändern des Werts, der die `Scale` Eigenschaft.

Die Animation wird gestartet, durch einen Aufruf der [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Beachten Sie, dass die [ `Commit` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Commit/p/Xamarin.Forms.IAnimatable/System.String/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{System.Double,System.Boolean}/System.Func{System.Boolean}/) Methode gibt keinen zurück ein `Task` Objekt. Stattdessen werden Benachrichtigungen über Rückrufmethoden bereitgestellt.

Die folgenden Argumente werden angegeben, der `Commit` Methode:

- Das erste Argument (*Besitzer*) identifiziert den Besitzer der Animation. Dies kann sich das visuelle Element, auf dem die Animation angewendet wird, oder ein anderes visuelles Element, z. B. die Seite.
- Das zweite Argument (*Namen*) die Animation mit einem Namen identifiziert. Der Name wird mit dem Besitzer zur eindeutigen Identifizierung die Animation kombiniert. Dieser eindeutige Kennung kann dann verwendet werden, um zu bestimmen, ob die Animation ausgeführt wird ([`AnimationIsRunning`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AnimationIsRunning/p/Xamarin.Forms.IAnimatable/System.String/)), oder er abgebrochen ([`AbortAnimation`](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/)).
- Das dritte Argument (*Rate*) gibt die Anzahl der Millisekunden zwischen den Aufrufen an die Rückrufmethode, die definiert, der [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Konstruktor
- Das vierte Argument (*Länge*) gibt die Dauer der Animation, in Millisekunden an.
- Das fünfte Argument (*Beschleunigungsfunktionen*) definiert die Beschleunigungsfunktion in der Animation verwendet werden. Alternativ kann die Beschleunigungsfunktion angegeben werden, als Argument an die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Konstruktor. Weitere Informationen zu Beschleunigungsfunktionen, finden Sie unter [einfachere Funktionen](~/xamarin-forms/user-interface/animation/easing.md).
- Das sechste Argument (*abgeschlossen*) ist ein Rückruf, der ausgeführt wird, wenn die Animation abgeschlossen wurde. Dieser Rückruf akzeptiert zwei Argumente, mit dem ersten Argument, der angibt, einen endgültigen Wert und das zweite Argument ist ein `bool` festgelegt `true` , wenn die Animation abgebrochen wurde. Alternativ können Sie die *abgeschlossen* Rückruf kann angegeben werden, als Argument an die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Konstruktor. Allerdings mit einer einzelnen Animation Wenn *abgeschlossen* Rückrufe in beide angegeben sind die `Animation` Konstruktor und die `Commit` -Methode, nur die im angegebenen Rückruf der `Commit` Methode wird ausgeführt.
- Das siebte Argument (*wiederholen*) ist ein Rückruf, der die Animation wiederholt werden kann. Sie wird am Ende der Animation aufgerufen und die Rückgabe `true` gibt an, dass die Animation wiederholt werden soll.

Der Gesamteffekt besteht darin, die eine Animation erstellen, die vergrößert die [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft ein [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) zwischen 1 und 2, mehr als 2 Sekunden (2000 Millisekunden), mit der [ `Linear` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.Linear/) Beschleunigungsfunktionen Funktion. Jedes Mal, die die Animation abgeschlossen ist, dessen `Scale` Eigenschaft auf 1 zurückgesetzt wurde und die Animation wiederholt wird.

> [!NOTE]
> Gleichzeitige Animationen, die unabhängig voneinander ausführbar konstruiert werden können, durch das Erstellen einer `Animation` -Objekt für jede Animation, und dem anschließenden Aufrufen der `Commit` -Methode für jede Animation.

<a name="child" />

### <a name="child-animations"></a>Untergeordneten Animationen

Die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) -Klasse unterstützt auch untergeordneten Animationen, die umfasst das Erstellen einer `Animation` Objekt, das die von anderen `Animation` Objekte werden hinzugefügt. Dies ermöglicht eine Reihe von Animationen zum Ausführen und synchronisiert werden. Das folgende Codebeispiel veranschaulicht das Erstellen und untergeordneten Animationen ausgeführt:

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

Alternativ kann das Codebeispiel präziser, wie im folgenden Codebeispiel wird geschrieben werden:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

In beiden Codebeispielen ein übergeordnetes Element [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) -Objekt erstellt wird, um die zusätzlichen `Animation` Objekte werden hinzugefügt. Die ersten beiden Argumente für die [ `Add` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.Add/p/System.Double/System.Double/Xamarin.Forms.Animation/) Methode angeben, wann er zu starten und beenden Sie die untergeordneten Animationen. Die Argumentwerte gelten müssen zwischen 0 und 1, und Darstellung die relative Periode innerhalb der übergeordneten Animation, dass die angegebene untergeordnete Animation aktiv sind. Deshalb in diesem Beispiel die `scaleUpAnimation` für die erste Hälfte der Animation, aktiviert die `scaleDownAnimation` wird für die zweite Hälfte der Animation, aktiv sein und die `rotateAnimation` für die gesamte Dauer aktiv sein.

Der Gesamteffekt besteht darin, dass die Animation über 4 Sekunden (4000 in Millisekunden) erfolgt. Die `scaleUpAnimation` eine Animation der [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft von 1, 2, mehr als 2 Sekunden. Die `scaleDownAnimation` klicken Sie dann eine Animation der `Scale` Eigenschaft von 2 auf 1, mehr als 2 Sekunden. Während beide Skalierung Animationen stattfinden, die `rotateAnimation` eine Animation der [ `Rotation` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Rotation/) Eigenschaft zwischen 0 und 360 liegen, mehr als 4 Sekunden. Beachten Sie, dass die Skalierung Animationen auch Beschleunigungsfunktionen verwenden. Die [ `SpringIn` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringIn/) Beschleunigungsfunktionen Funktion bewirkt, dass die [ `Image` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Image/) anfänglich vor dem Abrufen von größeren, verkleinert und die [ `SpringOut` ](https://developer.xamarin.com/api/field/Xamarin.Forms.Easing.SpringOut/) Beschleunigungsfunktionen-Funktion bewirkt, dass die `Image` kleiner als die tatsächliche Größe gegen Ende der Animation abgeschlossen werden.

Es gibt eine Reihe von Unterschieden zwischen einer [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) -Objekt, das untergeordneten Animationen verwendet, und eine, die nicht:

- Bei Verwendung von untergeordneten Animationen, die *abgeschlossen* -Rückruf für eine untergeordnete Animation gibt an, wenn das untergeordnete Element abgeschlossen wurde, und die *abgeschlossen* Rückruf übergeben wird, um die `Commit` Methode gibt an, wann die gesamte Animation wurde abgeschlossen.
- Bei Verwendung von untergeordneten Animationen zurückgeben `true` aus der *wiederholen* Rückruf für die `Commit` Methode führt nicht dazu, dass die Animation wiederholt, aber die Animation wird fortgesetzt, ohne neue Werte ausgeführt.
- Wenn eine Beschleunigungsfunktion in einschließlich der `Commit` -Methode und der Beschleunigungsfunktion gibt einen Wert größer als 1, die die Animation wird beendet. Wenn Beschleunigungsfunktion ein Wert kleiner als 0 zurückgibt, ist der Wert auf 0 festgelegt. Um eine Beschleunigungsfunktion zu verwenden, die einen Wert kleiner als 0 oder größer als 1 zurückgibt, muss er in einem untergeordneten Animationen, anstatt im angegeben, die `Commit` Methode.

Die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) -Klasse enthält zudem [ `WithConcurrent` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Animation.WithConcurrent/p/Xamarin.Forms.Animation/System.Double/System.Double/) Methoden, die zum Hinzufügen von untergeordneten Animationen zu einem übergeordneten Element verwendet werden können `Animation` Objekt. Jedoch ihre *beginnen* und *Fertig stellen* Argumentwerte werden nicht auf 0 bis 1 beschränkt, aber nur dieser Teil der untergeordneten Animation an, die auf einen Bereich von 0 bis 1 entspricht wird aktiv sein. Z. B. wenn ein `WithConcurrent` Methodenaufruf definiert eine untergeordneten Animation an, die als Ziel verwendet einen [ `Scale` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Scale/) Eigenschaft von 1 bis 6, jedoch mit *beginnen* und *Fertig stellen* Werte -2 und 3 der *beginnen* entspricht der Wert-2 eine `Scale` Wert 1, und die *Fertig stellen* entspricht der Wert 3 ein `Scale` Wert 6. Da Werte außerhalb des Bereichs von 0 bis 1 keine in einer Animation spielen die `Scale` Eigenschaft wird nur aus 3 bis 6 animiert.

## <a name="canceling-an-animation"></a>Abbrechen einer Animation

Eine Anwendung kann eine Animation mit einem Aufruf von "Abbrechen" die [ `AbortAnimation` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.AbortAnimation/p/Xamarin.Forms.IAnimatable/System.String/) Erweiterungsmethode, wie im folgenden Codebeispiel gezeigt:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Beachten Sie, dass Animationen durch eine Kombination der Animation Besitzer und den Namen der Animation eindeutig identifiziert werden. Aus diesem Grund den Besitzer und den Namen zusammen angegeben die Animation ausgeführt werden muss, die die Animation "Abbrechen". Im Codebeispiel wird deshalb sofort abgebrochen die Animation, mit dem Namen `SimpleAnimation` , deren Besitzer der von der Seite.

## <a name="creating-a-custom-animation"></a>Erstellen einer benutzerdefinierten Animation

Die bisher hier dargestellten Beispiele haben gezeigt Animationen, die gleichmäßig mit den Methoden in erreicht werden konnten die [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse. Jedoch den Vorteil, dass die [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Klasse ist, dass er Zugriff auf die Rückrufmethode darstellt, die ausgeführt wird, wenn der Wert der animierte ändert. Dadurch wird den Rückruf, der alle gewünschte Animation implementieren. Im folgenden Codebeispiel wird z. B. eine Animation der [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) Eigenschaft einer Seite durch Festlegen auf [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Werte erstellt, indem die [ `Color.FromHsla` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Color.FromHsla/p/System.Double/System.Double/System.Double/System.Double/)Methode mit Farbtonwerte im Bereich von 0 bis 1:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Die resultierende Animation stellt die Darstellung für einen Vorlauf der den Hintergrund der durch die Farben der Rainbow bereit.

Weitere Beispiele für das Erstellen von komplexer Animationen, z. B. eine Bézier-Kurve Animation finden Sie unter [Kapitel 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) von [Erstellen mobiler Apps mit Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="creating-a-custom-animation-extension-method"></a>Erstellen einer benutzerdefinierten Animation Extension-Methode

Die Erweiterungsmethoden in den [ `ViewExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ViewExtensions/) Klasse animieren eine Eigenschaft aus den aktuellen Wert auf einen angegebenen Wert. Dadurch ist es schwierig, zu erstellen, z. B. eine `ColorTo` Animation-Methode, die zu animierende eine Farbe aus einem Wert in eine andere verwendet werden kann, da:

- Die einzige [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) von definierte Eigenschaft den [ `VisualElement` ](https://developer.xamarin.com/api/type/Xamarin.Forms.VisualElement/) Klasse ist [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/), was nicht immer das gewünschte `Color` Eigenschaft um dem animiert werden soll.
- Oft der aktuelle Wert, der eine [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Eigenschaft ist [ `Color.Default` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.Default/), was eine echte Farbe nicht, und der nicht in Berechnungen der Interpolation verwendet werden.

Die Lösung für dieses Problem besteht darin, keine der `ColorTo` Methode als Ziel eine bestimmten [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Eigenschaft. Stattdessen kann mit einer Rückrufmethode, die die interpolierte übergibt geschrieben werden `Color` Wert an den Aufrufer zurück. Darüber hinaus wird die-Methode dauern starten und beenden `Color` Argumente.

Die `ColorTo` -Methode kann implementiert werden, als Erweiterungsmethode verwendet die [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) Methode in der [ `AnimationExtensions` ](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/) Klasse, um seine Funktionalität bereitzustellen. Grund hierfür ist die `Animate` Methode kann verwendet werden, um die Zieleigenschaften aus, die nicht vom Typ sind `double`, wie im folgenden Codebeispiel gezeigt:

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

Die [ `Animate` ](https://developer.xamarin.com/api/member/Xamarin.Forms.AnimationExtensions.Animate{T}/p/Xamarin.Forms.IAnimatable/System.String/System.Func{System.Double,T}/System.Action{T}/System.UInt32/System.UInt32/Xamarin.Forms.Easing/System.Action{T,System.Boolean}/System.Func{System.Boolean}/) Methode erfordert eine *transformieren* Argument, das eine Rückrufmethode ist. Die Eingabe für dieses Rückrufs ist immer ein `double` im Bereich von 0 bis 1. Aus diesem Grund die `ColorTo` Methode definiert seine eigenen Transformation `Func` , akzeptiert eine `double` im Bereich von 0 bis 1 festgelegt, und gibt eine [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Wert, der dieser Wert entspricht. Die `Color` Wert wird berechnet, indem Interpolieren der [ `R` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.R/), [ `G` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.G/), [ `B` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.B/), und [ `A` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Color.A/) Werte der beiden angegebenen `Color` Argumente. Die `Color` Wert wird dann an die Rückrufmethode für eine bestimmte Eigenschaft der Anwendung übergeben.

Dieser Ansatz ermöglicht die `ColorTo` Methode, um alle animieren [ `Color` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Color/) Eigenschaft, wie im folgenden Codebeispiel wird veranschaulicht:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

In diesem Codebeispiel wird die `ColorTo` Methode erstellt eine Animation der [ `TextColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Label.TextColor/) und [ `BackgroundColor` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.BackgroundColor/) Eigenschaften eine [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/), die `BackgroundColor`Eigenschaft einer Seite und die [ `Color` ](https://developer.xamarin.com/api/property/Xamarin.Forms.BoxView.Color/) Eigenschaft von einem [ `BoxView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.BoxView/).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie mithilfe der [ `Animation` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/) Klasse zum Erstellen und "Abbrechen" Animationen, mehrere Animationen zu synchronisieren, und Erstellen von benutzerdefinierten Animationen, die Eigenschaften zu animieren, die durch die vorhandene Animation animiert werden nicht Methoden. Die `Animation` Klasse ist der Baustein von allen Xamarin.Forms Animationen.


## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Animationen (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/animation/custom/)
- [Animation](https://developer.xamarin.com/api/type/Xamarin.Forms.Animation/)
- [AnimationExtensions](https://developer.xamarin.com/api/type/Xamarin.Forms.AnimationExtensions/)
