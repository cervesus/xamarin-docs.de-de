---
title: Benutzerdefinierte Animationen in Xamarin.Forms
description: In diesem Artikel wird veranschaulicht, wie Sie mithilfe der Xamarin.FOrms-Animation-Klasse erstellen und Abbrechen von Animationen, mehrere Animationen zu synchronisieren, und erstellen benutzerdefinierte Animationen, das Animieren von Eigenschaften, die durch die vorhandenen Methoden für die Animation animiert werden nicht.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
ms.openlocfilehash: 405d7990b622b890aa3d66bd632662f086441666
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/26/2020
ms.locfileid: "77635757"
---
# <a name="custom-animations-in-xamarinforms"></a>Benutzerdefinierte Animationen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_Die Animations Klasse ist der Baustein aller xamarin. Forms-Animationen, wobei die Erweiterungs Methoden in der viewextensions-Klasse ein oder mehrere Animations Objekte erstellen. In diesem Artikel wird veranschaulicht, wie Sie mit der Animations Klasse Animationen erstellen und Abbrechen, mehrere Animationen synchronisieren und benutzerdefinierte Animationen erstellen, die Eigenschaften animieren, die nicht von den vorhandenen Animations Methoden animiert werden._

Beim Erstellen eines `Animation` Objekts muss eine Reihe von Parametern angegeben werden, einschließlich Start-und Endwert der zu animierenden Eigenschaft sowie eines Rückrufs, der den Wert der-Eigenschaft ändert. Ein `Animation`-Objekt kann auch eine Auflistung von untergeordneten Animationen verwalten, die ausgeführt und synchronisiert werden können. Weitere Informationen finden Sie unter untergeordnete [Animationen](#child).

Das Ausführen einer mit der [`Animation`](xref:Xamarin.Forms.Animation) -Klasse erstellten Animation, die untergeordnete Animationen einschließen kann, wird durch Aufrufen der [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) -Methode erreicht. Diese Methode gibt die Dauer der Animation und zwischen anderen Elementen ein Rückruf, der steuert, ob die Animation wiederholt werden soll.

Außerdem verfügt die [`Animation`](xref:Xamarin.Forms.Animation) -Klasse über eine `IsEnabled`-Eigenschaft, die überprüft werden kann, um festzustellen, ob Animationen vom Betriebssystem deaktiviert wurden, z. b. wenn der Energiesparmodus aktiviert ist.

## <a name="create-an-animation"></a>Erstellen einer Animation

Beim Erstellen eines [`Animation`](xref:Xamarin.Forms.Animation) Objekts sind in der Regel mindestens drei Parameter erforderlich, wie im folgenden Codebeispiel gezeigt:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Dieser Code definiert eine Animation der [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft einer [`Image`](xref:Xamarin.Forms.Image) Instanz von einem Wert von 1 bis zum Wert 2. Der animierte Wert, der von xamarin. Forms abgeleitet wird, wird an den Rückruf übermittelt, der als erstes Argument angegeben ist, in dem es verwendet wird, um den Wert der `Scale`-Eigenschaft zu ändern.

Die Animation wird mit einem aufzurufenden [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) -Methode gestartet, wie im folgenden Codebeispiel gezeigt:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Beachten Sie, dass die [`Commit`](xref:Xamarin.Forms.Animation.Commit(Xamarin.Forms.IAnimatable,System.String,System.UInt32,System.UInt32,Xamarin.Forms.Easing,System.Action{System.Double,System.Boolean},System.Func{System.Boolean})) -Methode kein `Task`-Objekt zurückgibt. Stattdessen werden Benachrichtigungen über Rückrufmethoden bereitgestellt.

Die folgenden Argumente werden in der `Commit`-Methode angegeben:

- Das erste Argument (*Besitzer*) identifiziert den Besitzer der Animation. Dies kann sein, das visuelle Element, das auf dem die Animation angewendet wird, oder ein anderes visuelles Element, wie die Seite.
- Das zweite Argument (*Name*) identifiziert die Animation mit einem Namen. Der Name wird mit dem Besitzer zur eindeutigen Identifizierung die Animation kombiniert. Diese eindeutige Identifikation kann dann verwendet werden, um zu bestimmen, ob die Animation ausgeführt wird ([`AnimationIsRunning`](xref:Xamarin.Forms.AnimationExtensions.AnimationIsRunning(Xamarin.Forms.IAnimatable,System.String))) oder um Sie abzubrechen ([`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String))).
- Das dritte Argument (*Rate*) gibt die Anzahl von Millisekunden zwischen jedem Rückruf der Rückruf Methode an, die im [`Animation`](xref:Xamarin.Forms.Animation) -Konstruktor definiert ist.
- Das vierte Argument (*length*) gibt die Dauer der Animation in Millisekunden an.
- Das fünfte*Argument (Beschleunigung*) definiert die Beschleunigungs Funktion, die in der Animation verwendet werden soll. Alternativ kann die Beschleunigungs Funktion als Argument für den [`Animation`](xref:Xamarin.Forms.Animation) -Konstruktor angegeben werden. Weitere Informationen zu Beschleunigungsfunktionen finden Sie unter Beschleunigungs [Funktionen](~/xamarin-forms/user-interface/animation/easing.md).
- Das sechste Argument (*Fertig*) ist ein Rückruf, der ausgeführt wird, wenn die Animation abgeschlossen wurde. Dieser Rückruf erfordert zwei Argumente, wobei das erste Argument einen Endwert angibt, und das zweite Argument ist eine `bool`, die auf `true` festgelegt ist, wenn die Animation abgebrochen wurde. Alternativ kann der *fertige* Rückruf als Argument für den [`Animation`](xref:Xamarin.Forms.Animation) -Konstruktor angegeben werden. Wenn jedoch bei einer einzelnen Animation *fertige* Rückrufe sowohl im `Animation`-Konstruktor als auch in der `Commit`-Methode angegeben werden, wird nur der in der `Commit`-Methode angegebene Rückruf ausgeführt.
- Das siebte Argument (*Repeat*) ist ein Rückruf, mit dem die Animation wiederholt werden kann. Er wird am Ende der Animation aufgerufen, und die Rückgabe `true` gibt an, dass die Animation wiederholt werden soll.

Der Gesamteffekt besteht darin, eine Animation zu erstellen, die die [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft eines [`Image`](xref:Xamarin.Forms.Image) von 1 auf 2 erhöht, über 2 Sekunden (2000 Millisekunden), wobei die [`Linear`](xref:Xamarin.Forms.Easing.Linear) Beschleunigungs Funktion verwendet wird. Jedes Mal, wenn die Animation abgeschlossen ist, wird die `Scale` Eigenschaft auf 1 zurückgesetzt, und die Animation wird wiederholt.

> [!NOTE]
> Gleichzeitige Animationen, die unabhängig voneinander ausgeführt werden, können erstellt werden, indem ein `Animation`-Objekt für jede Animation erstellt und dann die `Commit`-Methode für jede Animation aufgerufen wird.

<a name="child" />

### <a name="child-animations"></a>Untergeordneten Animationen

Die [`Animation`](xref:Xamarin.Forms.Animation) -Klasse unterstützt auch untergeordnete Animationen, wozu das Erstellen eines `Animation` Objekts gehört, dem weitere `Animation`-Objekte hinzugefügt werden. Dies ermöglicht eine Reihe von Animationen zum Ausführen und synchronisiert werden. Im folgenden Codebeispiel veranschaulicht das Erstellen und die untergeordneten Animationen ausgeführt:

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

In beiden Codebeispielen wird ein übergeordnetes [`Animation`](xref:Xamarin.Forms.Animation) Objekt erstellt, dem zusätzliche `Animation` Objekte hinzugefügt werden. Die ersten beiden Argumente der [`Add`](xref:Xamarin.Forms.Animation.Add(System.Double,System.Double,Xamarin.Forms.Animation)) -Methode geben an, wann die untergeordnete Animation begonnen und beendet werden soll. Die Argumentwerte müssen zwischen 0 und 1 sein und Darstellung der relative Zeitraum innerhalb der übergeordneten Animation, dass die angegebene untergeordnete Animation aktiv sein werden. In diesem Beispiel ist die `scaleUpAnimation` in der ersten Hälfte der Animation aktiv, die `scaleDownAnimation` wird in der zweiten Hälfte der Animation aktiv sein, und die `rotateAnimation` wird für die gesamte Dauer aktiv sein.

Der Gesamteffekt besteht, dass die Animation über mehr als 4 Sekunden (4000 in Millisekunden) auftritt. Der `scaleUpAnimation` animiert die [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft von 1 bis 2, über 2 Sekunden. Der `scaleDownAnimation` animiert dann die `Scale`-Eigenschaft von 2 bis 1, über 2 Sekunden. Während beide Skalierungs Animationen auftreten, animiert der `rotateAnimation` die [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) -Eigenschaft von 0 bis 360, über 4 Sekunden. Beachten Sie, dass die Skalierung Animationen auch Beschleunigungsfunktionen verwenden. Die [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) Beschleunigungs Funktion bewirkt, dass die [`Image`](xref:Xamarin.Forms.Image) anfänglich verkleinert wird, bevor Sie vergrößert wird, und die [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) Beschleunigungs Funktion bewirkt, dass die `Image` zu einer geringeren Größe als die tatsächliche Größe am Ende der kompletten Animation werden.

Es gibt eine Reihe von Unterschieden zwischen einem [`Animation`](xref:Xamarin.Forms.Animation) Objekt, das untergeordnete Animationen verwendet, und einem, das nicht:

- Bei der Verwendung untergeordneter Animationen zeigt der *fertige* Rückruf für eine untergeordnete Animation an, wann das untergeordnete Element abgeschlossen wurde, und der *fertige* Rückruf, der an die `Commit`-Methode weitergegeben wurde, zeigt an, wann die gesamte Animation
- Bei der Verwendung von untergeordneten Animationen führt das Zurückgeben von `true` aus dem *Wiederholungs* Rückruf für die `Commit` Methode nicht zu einer Wiederholung der Animation, aber die Animation wird weiterhin ohne neue Werte ausgeführt.
- Wenn Sie in der `Commit`-Methode eine Beschleunigungs Funktion einschließen und die Beschleunigungs Funktion einen Wert größer als 1 zurückgibt, wird die Animation beendet. Wenn die Beschleunigungsfunktion auf einen Wert kleiner als 0 zurückgibt, wird der Wert auf 0 festgelegt. Um eine Beschleunigungs Funktion zu verwenden, die einen Wert kleiner als 0 oder größer als 1 zurückgibt, muss Sie in einer der untergeordneten Animationen anstatt in der `Commit`-Methode angegeben werden.

Die [`Animation`](xref:Xamarin.Forms.Animation) -Klasse enthält auch [`WithConcurrent`](xref:Xamarin.Forms.Animation.WithConcurrent(Xamarin.Forms.Animation,System.Double,System.Double)) Methoden, die zum Hinzufügen von untergeordneten Animationen zu einem übergeordneten `Animation` Objekt verwendet werden können. Allerdings sind die Werte für *Begin* und *Finish* Argument nicht auf 0 bis 1 beschränkt, aber nur der Teil der untergeordneten Animation, der einem Bereich von 0 bis 1 entspricht, ist aktiv. Wenn z. b. ein `WithConcurrent` Methoden Aufrufes eine untergeordnete Animation definiert, die eine [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) -Eigenschaft von 1 bis 6 als Ziel hat, aber mit den *Begin* -und *Finish* -Werten von-2 und 3, entspricht der *anfangs* Wert-2 dem `Scale` Wert 1, *und der* Endwert 3 entspricht einem `Scale` Wert von 6. Da Werte außerhalb des Bereichs von 0 und 1 nicht Teil einer Animation sind, wird die `Scale`-Eigenschaft nur von 3 bis 6 animiert.

## <a name="cancel-an-animation"></a>Abbrechen einer Animation

Eine Anwendung kann eine Animation mit einem aufzurufenden [`AbortAnimation`](xref:Xamarin.Forms.AnimationExtensions.AbortAnimation(Xamarin.Forms.IAnimatable,System.String)) -Erweiterungsmethode abbrechen, wie im folgenden Codebeispiel gezeigt:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Beachten Sie, dass Animationen durch eine Kombination aus den Animation-Besitzer und den Namen der Animation eindeutig identifiziert werden. Aus diesem Grund den Besitzer und den Namen zusammen angegeben die Animation ausgeführt werden muss, die Animation abzubrechen. Aus diesem Grund wird im Codebeispiel sofort die Animation mit dem Namen `SimpleAnimation` abgebrochen, die der Seite gehört.

## <a name="create-a-custom-animation"></a>Erstellen einer benutzerdefinierten Animation

Die hier gezeigten Beispiele haben Animationen demonstriert, die gleichermaßen mit den Methoden in der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse erzielt werden können. Der Vorteil der [`Animation`](xref:Xamarin.Forms.Animation) -Klasse besteht jedoch darin, dass Sie Zugriff auf die Rückruf Methode hat, die ausgeführt wird, wenn sich der animierte Wert ändert. Dadurch wird den Rückruf, der alle gewünschten Animationen zu implementieren. Im folgenden Codebeispiel wird die [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) -Eigenschaft einer Seite animiert, indem Sie auf [`Color`](xref:Xamarin.Forms.Color) von der [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) -Methode erstellten Werte festgelegt wird, wobei Hue-Werte zwischen 0 und 1 liegen:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Die resultierende Animation enthält, die Darstellung des Seitenhintergrunds über die Farben des Regenbogens Vorlauf.

Weitere Beispiele zum Erstellen komplexer Animationen, einschließlich einer Bezier-Kurven Animation, finden Sie in [Kapitel 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) der [Erstellung von Mobile Apps mit xamarin. Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="create-a-custom-animation-extension-method"></a>Erstellen einer benutzerdefinierten Animations Erweiterungsmethode

Die Erweiterungs Methoden in der [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) -Klasse animieren eine Eigenschaft von Ihrem aktuellen Wert zu einem angegebenen Wert. Dies erschwert das Erstellen, z. b. eine `ColorTo` Animations Methode, mit der eine Farbe von einem Wert zu einem anderen animiert werden kann, da:

- Die einzige [`Color`](xref:Xamarin.Forms.Color) Eigenschaft, die von der [`VisualElement`](xref:Xamarin.Forms.VisualElement) -Klasse definiert wird, ist [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor), was nicht immer die gewünschte `Color` Eigenschaft für die Animation ist.
- Häufig ist der aktuelle Wert einer [`Color`](xref:Xamarin.Forms.Color) -Eigenschaft [`Color.Default`](xref:Xamarin.Forms.Color.Default), was keine echte Farbe ist und nicht in Interpolations Berechnungen verwendet werden kann.

Die Lösung für dieses Problem besteht darin, dass die `ColorTo`-Methode nicht auf eine bestimmte [`Color`](xref:Xamarin.Forms.Color) Eigenschaft ausgerichtet ist. Stattdessen kann Sie mit einer Rückruf Methode geschrieben werden, die den interinterpoliert `Color` Wert an den Aufrufer zurück übergibt. Außerdem nimmt die Methode Start-und End`Color` Argumente an.

Die `ColorTo`-Methode kann als Erweiterungsmethode implementiert werden, die die [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) -Methode in der [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) -Klasse verwendet, um ihre Funktionalität bereitzustellen. Dies liegt daran, dass die `Animate`-Methode verwendet werden kann, um Eigenschaften zu verwenden, die nicht vom Typ "`double`" sind, wie im folgenden Codebeispiel gezeigt:

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

Die [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) -Methode erfordert ein *Transform* -Argument, bei dem es sich um eine Rückruf Methode handelt. Die Eingabe für diesen Rückruf ist immer eine `double` im Bereich von 0 bis 1. Aus diesem Grund definiert die `ColorTo`-Methode eine eigene Transformations `Func`, die einen `double` im Bereich von 0 bis 1 annimmt und einen [`Color`](xref:Xamarin.Forms.Color) Wert zurückgibt, der diesem Wert entspricht. Der `Color` Wert wird berechnet, indem die Werte für [`R`](xref:Xamarin.Forms.Color.R), [`G`](xref:Xamarin.Forms.Color.G), [`B`](xref:Xamarin.Forms.Color.B)und [`A`](xref:Xamarin.Forms.Color.A) der beiden `Color` Argumente interpolieren werden. Der `Color` Wert wird dann an die Rückruf Methode für die Anwendung an eine bestimmte Eigenschaft weitergegeben.

Dieser Ansatz ermöglicht der `ColorTo` Methode, jede [`Color`](xref:Xamarin.Forms.Color) Eigenschaft zu animieren, wie im folgenden Codebeispiel gezeigt:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

In diesem Codebeispiel animiert die `ColorTo`-Methode die [`TextColor`](xref:Xamarin.Forms.Label.TextColor) und [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) Eigenschaften einer [`Label`](xref:Xamarin.Forms.Label), die `BackgroundColor`-Eigenschaft einer Seite und die [`Color`](xref:Xamarin.Forms.BoxView.Color) -Eigenschaft einer [`BoxView`](xref:Xamarin.Forms.BoxView).

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Animationen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [Animation-API](xref:Xamarin.Forms.Animation)
- [AnimationExtensions-API](xref:Xamarin.Forms.AnimationExtensions)
