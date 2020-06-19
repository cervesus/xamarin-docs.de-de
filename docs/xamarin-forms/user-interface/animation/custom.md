---
title: Benutzerdefinierte Animationen inXamarin.Forms
description: In diesem Artikel wird veranschaulicht, wie Sie mit der Animations Klasse von xamarin. FOrms Animationen erstellen und Abbrechen, mehrere Animationen synchronisieren und benutzerdefinierte Animationen zum Animieren von Eigenschaften erstellen, die nicht von den vorhandenen Animations Methoden animiert werden.
ms.prod: xamarin
ms.assetid: 03B2E3FC-E720-4D45-B9A0-711081FC1907
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/10/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 573f18de0d7593d832505eb6bb2b492caea024a1
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/18/2020
ms.locfileid: "84946103"
---
# <a name="custom-animations-in-xamarinforms"></a>Benutzerdefinierte Animationen inXamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)

_Die Animations Klasse ist der Baustein aller Xamarin.Forms Animationen, wobei die Erweiterungs Methoden in der viewextensions-Klasse ein oder mehrere Animations Objekte erstellen. In diesem Artikel wird veranschaulicht, wie Sie mit der Animations Klasse Animationen erstellen und Abbrechen, mehrere Animationen synchronisieren und benutzerdefinierte Animationen erstellen, die Eigenschaften animieren, die nicht von den vorhandenen Animations Methoden animiert werden._

Beim Erstellen eines Objekts muss eine Reihe von Parametern angegeben werden `Animation` , einschließlich Start-und Endwert der zu animierenden Eigenschaft sowie eines Rückrufs, der den Wert der-Eigenschaft ändert. Ein `Animation` Objekt kann auch eine Auflistung von untergeordneten Animationen verwalten, die ausgeführt und synchronisiert werden können. Weitere Informationen finden Sie unter untergeordnete [Animationen](#child-animations).

Das Ausführen einer mit der- [`Animation`](xref:Xamarin.Forms.Animation) Klasse erstellten Animation, die untergeordnete Animationen einschließen kann, wird durch Aufrufen von [ `Commit` ] (Xref: Xamarin.Forms . Animation. Commit () erreicht Xamarin.Forms . Ianimabel, System. String, System. UInt32, System. UInt32, Xamarin.Forms . Beschleunigungs Methode, System. Action {System. Double, System. Boolean}, System. Func {System. Boolean})-Methode. Diese Methode gibt die Dauer der Animation und unter anderen Elementen einen Rückruf an, der steuert, ob die Animation wiederholt werden soll.

Außerdem verfügt die- [`Animation`](xref:Xamarin.Forms.Animation) Klasse über eine- `IsEnabled` Eigenschaft, die überprüft werden kann, um zu bestimmen, ob Animationen vom Betriebssystem deaktiviert wurden, z. b. wenn der Energiesparmodus aktiviert ist.

## <a name="create-an-animation"></a>Erstellen einer Animation

Wenn Sie ein [`Animation`](xref:Xamarin.Forms.Animation) Objekt erstellen, sind in der Regel mindestens drei Parameter erforderlich, wie im folgenden Codebeispiel gezeigt:

```csharp
var animation = new Animation (v => image.Scale = v, 1, 2);
```

Dieser Code definiert eine Animation der- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft einer- [`Image`](xref:Xamarin.Forms.Image) Instanz von einem Wert von 1 bis zu einem Wert von 2. Der animierte Wert, der von abgeleitet wird Xamarin.Forms , wird an den Rückruf übermittelt, der als erstes Argument angegeben ist, in dem es verwendet wird, um den Wert der-Eigenschaft zu ändern `Scale` .

Die Animation wird durch einen-aufrufbefehl [ `Commit` ] (Xref: Xamarin.Forms . Animation. Commit () gestartet Xamarin.Forms . Ianimabel, System. String, System. UInt32, System. UInt32, Xamarin.Forms . Beschleunigungs Methode, System. Action {System. Double, System. Boolean}, System. Func {System. Boolean})), wie im folgenden Codebeispiel gezeigt:

```csharp
animation.Commit (this, "SimpleAnimation", 16, 2000, Easing.Linear, (v, c) => image.Scale = 1, () => true);
```

Beachten Sie, dass [ `Commit` ] (Xref: Xamarin.Forms . Animation. Commit () Xamarin.Forms ist. Ianimabel, System. String, System. UInt32, System. UInt32, Xamarin.Forms . Die Methode "Beschleunigung", System. Action {System. Double, System. Boolean}, System. Func {System. Boolean})) gibt kein- `Task` Objekt zurück. Stattdessen werden Benachrichtigungen durch Rückruf Methoden bereitgestellt.

Die folgenden Argumente werden in der- `Commit` Methode angegeben:

- Das erste Argument (*Besitzer*) identifiziert den Besitzer der Animation. Dies kann das visuelle Element sein, auf das die Animation angewendet wird, oder ein anderes visuelles Element, z. b. die Seite.
- Das zweite Argument (*Name*) identifiziert die Animation mit einem Namen. Der Name wird mit dem Besitzer kombiniert, um die Animation eindeutig zu identifizieren. Diese eindeutige Identifikation kann dann verwendet werden, um zu bestimmen, ob die Animation ausgeführt wird ([ `AnimationIsRunning` ] (Xref:) Xamarin.Forms . AnimationExtensions. animationisrunning ( Xamarin.Forms . Ianimabel, System. String)) oder, um es abzubrechen ([ `AbortAnimation` ] (Xref:) Xamarin.Forms . AnimationExtensions. abortanimation ( Xamarin.Forms . Ianimabel, System. String)).
- Das dritte Argument (*Rate*) gibt die Anzahl von Millisekunden zwischen jedem Rückruf der Rückruf Methode an, die im [`Animation`](xref:Xamarin.Forms.Animation) Konstruktor definiert ist.
- Das vierte Argument (*length*) gibt die Dauer der Animation in Millisekunden an.
- Das fünfte*Argument (Beschleunigung*) definiert die Beschleunigungs Funktion, die in der Animation verwendet werden soll. Alternativ kann die Beschleunigungs Funktion als Argument für den Konstruktor angegeben werden [`Animation`](xref:Xamarin.Forms.Animation) . Weitere Informationen zu Beschleunigungsfunktionen finden Sie unter Beschleunigungs [Funktionen](~/xamarin-forms/user-interface/animation/easing.md).
- Das sechste Argument (*Fertig*) ist ein Rückruf, der ausgeführt wird, wenn die Animation abgeschlossen wurde. Dieser Rückruf erfordert zwei Argumente, wobei das erste Argument einen Endwert angibt und das zweite Argument ein ist, `bool` das auf festgelegt ist, `true` Wenn die Animation abgebrochen wurde. Alternativ kann der *fertige* Rückruf als Argument für den Konstruktor angegeben werden [`Animation`](xref:Xamarin.Forms.Animation) . Wenn jedoch bei einer einzelnen Animation *fertige* Rückrufe sowohl im `Animation` Konstruktor als auch in der-Methode angegeben werden `Commit` , wird nur der in der- `Commit` Methode angegebene Rückruf ausgeführt.
- Das siebte Argument (*Repeat*) ist ein Rückruf, mit dem die Animation wiederholt werden kann. Er wird am Ende der Animation aufgerufen und `true` gibt an, dass die Animation wiederholt werden soll.

Der Gesamteffekt besteht darin, eine Animation zu erstellen, die die- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft von [`Image`](xref:Xamarin.Forms.Image) 1 bis 2, über 2 Sekunden (2000 Millisekunden) mithilfe der Beschleunigungs [`Linear`](xref:Xamarin.Forms.Easing.Linear) Funktion vergrößert. Wenn die Animation abgeschlossen ist, `Scale` wird die-Eigenschaft auf 1 zurückgesetzt, und die Animation wird wiederholt.

> [!NOTE]
> Gleichzeitige Animationen, die unabhängig voneinander ausgeführt werden, können erstellt werden, indem ein `Animation` -Objekt für jede Animation erstellt und dann die- `Commit` Methode für jede Animation aufgerufen wird.

### <a name="child-animations"></a>Untergeordnete Animationen

Die- [`Animation`](xref:Xamarin.Forms.Animation) Klasse unterstützt auch untergeordnete Animationen. dazu gehört das Erstellen eines Objekts, dem `Animation` andere- `Animation` Objekte hinzugefügt werden. Dadurch kann eine Reihe von Animationen ausgeführt und synchronisiert werden. Im folgenden Codebeispiel wird das Erstellen und Ausführen von untergeordneten Animationen veranschaulicht:

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

Alternativ kann das Codebeispiel ausführlicher geschrieben werden, wie im folgenden Codebeispiel gezeigt:

```csharp
new Animation {
    { 0, 0.5, new Animation (v => image.Scale = v, 1, 2) },
    { 0, 1, new Animation (v => image.Rotation = v, 0, 360) },
    { 0.5, 1, new Animation (v => image.Scale = v, 2, 1) }
    }.Commit (this, "ChildAnimations", 16, 4000, null, (v, c) => SetIsEnabledButtonState (true, false));
```

In beiden Codebeispielen [`Animation`](xref:Xamarin.Forms.Animation) wird ein übergeordnetes Objekt erstellt, dem weitere `Animation` Objekte hinzugefügt werden. Die ersten beiden Argumente für [ `Add` ] (Xref: Xamarin.Forms . Animation. Add (System. Double, System. Double, Xamarin.Forms . Animation))-Methode geben Sie an, wann die untergeordnete Animation gestartet und beendet werden soll. Die Argument Werte müssen zwischen 0 und 1 liegen und den relativen Zeitraum innerhalb der übergeordneten Animation darstellen, dass die angegebene untergeordnete Animation aktiv ist. Daher wird in diesem Beispiel in der `scaleUpAnimation` ersten Hälfte der Animation aktiv sein, die `scaleDownAnimation` wird in der zweiten Hälfte der Animation aktiv sein, und die wird `rotateAnimation` für die gesamte Dauer aktiv sein.

Der Gesamteffekt ist, dass die Animation über 4 Sekunden (4000 Millisekunden) erfolgt. Der `scaleUpAnimation` animiert die- [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft von 1 bis 2, über 2 Sekunden. Die- `scaleDownAnimation` Eigenschaft animiert dann die- `Scale` Eigenschaft von 2 bis 1, über 2 Sekunden. Während beide Skalierungs Animationen auftreten, animiert die- `rotateAnimation` [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) Eigenschaft von 0 bis 360, über 4 Sekunden. Beachten Sie, dass die Skalierungs Animationen auch Beschleunigungsfunktionen verwenden. Die Beschleunigungs [`SpringIn`](xref:Xamarin.Forms.Easing.SpringIn) Funktion bewirkt, dass der [`Image`](xref:Xamarin.Forms.Image) anfänglich verkleinert wird, bevor er vergrößert wird, und die Beschleunigungs [`SpringOut`](xref:Xamarin.Forms.Easing.SpringOut) Funktion bewirkt, dass die- `Image` Klasse kleiner ist als die tatsächliche Größe bis zum Ende der kompletten Animation.

Es gibt eine Reihe von Unterschieden zwischen einem Objekt, das untergeordnete [`Animation`](xref:Xamarin.Forms.Animation) Animationen verwendet, und einem Objekt, das nicht:

- Bei der Verwendung untergeordneter Animationen zeigt der *fertige* Rückruf für eine untergeordnete Animation an, wann das untergeordnete Element abgeschlossen wurde, und der *fertige* Rückruf, der an die-Methode weitergegeben wurde, zeigt an, `Commit` wann die gesamte Animation
- Bei der Verwendung von untergeordneten Animationen bewirkt die Rückgabe des `true` *Wiederholungs* Rückrufs für die `Commit` Methode nicht, dass die Animation wiederholt wird, aber die Animation wird weiterhin ohne neue Werte ausgeführt.
- Wenn Sie eine Beschleunigungs Funktion in die `Commit` -Methode einschließen und die Beschleunigungs Funktion einen Wert größer als 1 zurückgibt, wird die Animation beendet. Wenn die Beschleunigungs Funktion einen Wert kleiner als 0 (null) zurückgibt, wird der Wert auf 0 (null) geklammert. Um eine Beschleunigungs Funktion zu verwenden, die einen Wert kleiner als 0 oder größer als 1 zurückgibt, muss Sie in einer der untergeordneten Animationen und nicht in der-Methode angegeben werden `Commit` .

Die- [`Animation`](xref:Xamarin.Forms.Animation) Klasse enthält auch [ `WithConcurrent` ] (Xref: Xamarin.Forms . Animation. withconcurrent ( Xamarin.Forms . Animation-, System. Double-, System. Double)-Methoden, die zum Hinzufügen von untergeordneten Animationen zu einem übergeordneten Objekt verwendet werden können `Animation` . Allerdings sind die Werte für *Begin* und *Finish* Argument nicht auf 0 bis 1 beschränkt, aber nur der Teil der untergeordneten Animation, der einem Bereich von 0 bis 1 entspricht, ist aktiv. Wenn ein `WithConcurrent` Methoden aufrufz. b. eine untergeordnete Animation definiert, die eine [`Scale`](xref:Xamarin.Forms.VisualElement.Scale) Eigenschaft von 1 bis 6 als Ziel hat, aber mit den *anfangs* -und *Abschluss* Werten von-2 und 3, entspricht der *anfangs* Wert-2 dem `Scale` Wert 1, und der *Abschluss* Wert 3 entspricht dem `Scale` Wert 6. Da Werte außerhalb des Bereichs von 0 und 1 nicht Teil einer Animation sind, wird die- `Scale` Eigenschaft nur von 3 bis 6 animiert.

## <a name="cancel-an-animation"></a>Abbrechen einer Animation

Eine Anwendung kann eine Animation durch einen []-Aufrufvorgang Abbrechen `AbortAnimation` (Xref:) Xamarin.Forms . AnimationExtensions. abortanimation ( Xamarin.Forms . Ianimabel, System. String))-Erweiterungsmethode, wie im folgenden Codebeispiel gezeigt:

```csharp
this.AbortAnimation ("SimpleAnimation");
```

Beachten Sie, dass Animationen durch eine Kombination aus dem Animations Besitzer und dem Animations Namen eindeutig identifiziert werden. Daher müssen der beim Ausführen der Animation angegebene Besitzer und Name angegeben werden, um die Animation abzubrechen. Aus diesem Grund wird im Codebeispiel sofort die Animation mit dem Namen abgebrochen `SimpleAnimation` , die sich im Besitz der Seite befindet.

## <a name="create-a-custom-animation"></a>Erstellen einer benutzerdefinierten Animation

Die hier gezeigten Beispiele haben Animationen demonstriert, die gleichermaßen mit den Methoden in der-Klasse erzielt werden können [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) . Der Vorteil der- [`Animation`](xref:Xamarin.Forms.Animation) Klasse besteht jedoch darin, dass Sie Zugriff auf die Rückruf Methode hat, die ausgeführt wird, wenn sich der animierte Wert ändert. Dadurch kann der Rückruf jede gewünschte Animation implementieren. Im folgenden Codebeispiel wird die- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) Eigenschaft einer Seite animiert, indem Sie auf Werte festgelegt [`Color`](xref:Xamarin.Forms.Color) wird, die von der- [`Color.FromHsla`](xref:Xamarin.Forms.Color.FromHsla(System.Double,System.Double,System.Double,System.Double)) Methode erstellt werden, wobei Hue-Werte zwischen 0 und 1 liegen:

```csharp
new Animation (callback: v => BackgroundColor = Color.FromHsla (v, 1, 0.5),
  start: 0,
  end: 1).Commit (this, "Animation", 16, 4000, Easing.Linear, (v, c) => BackgroundColor = Color.Default);
```

Die resultierende Animation stellt die Darstellung des Seiten Hintergrunds durch die Farben des Regenbogens dar.

Weitere Beispiele zum Erstellen komplexer Animationen, einschließlich einer Bezier-Kurven Animation, finden Sie in [Kapitel 22](https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch22-Apr2016.pdf) der [Erstellung Xamarin.Forms von Mobile Apps mit ](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

## <a name="create-a-custom-animation-extension-method"></a>Erstellen einer benutzerdefinierten Animations Erweiterungsmethode

Die Erweiterungs Methoden in der- [`ViewExtensions`](xref:Xamarin.Forms.ViewExtensions) Klasse animieren eine Eigenschaft von Ihrem aktuellen Wert zu einem angegebenen Wert. Dies erschwert das Erstellen, z. b. eine `ColorTo` Animations Methode, mit der eine Farbe von einem Wert zu einem anderen animiert werden kann, da Folgendes gilt:

- Die einzige [`Color`](xref:Xamarin.Forms.Color) von der-Klasse definierte Eigenschaft [`VisualElement`](xref:Xamarin.Forms.VisualElement) ist. Dies ist [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) nicht immer die gewünschte Eigenschaft, die `Color` animiert werden soll.
- Häufig ist der aktuelle Wert einer [`Color`](xref:Xamarin.Forms.Color) Eigenschaft [`Color.Default`](xref:Xamarin.Forms.Color.Default) , bei der es sich nicht um eine echte Farbe handelt und der nicht in Interpolations Berechnungen verwendet werden kann.

Die Lösung für dieses Problem besteht darin, dass die `ColorTo` Methode nicht auf eine bestimmte Eigenschaft ausgerichtet ist [`Color`](xref:Xamarin.Forms.Color) . Stattdessen kann Sie mit einer Rückruf Methode geschrieben werden, die den interinterpolierten `Color` Wert zurück an den Aufrufer übergibt. Außerdem nimmt die Methode Start-und End- `Color` Argumente an.

Die- `ColorTo` Methode kann als Erweiterungsmethode implementiert werden, die die- [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) Methode in der- [`AnimationExtensions`](xref:Xamarin.Forms.AnimationExtensions) Klasse verwendet, um ihre Funktionalität bereitzustellen. Dies liegt daran `Animate` , dass die-Methode verwendet werden kann, um Eigenschaften zu verwenden, die nicht vom Typ sind `double` , wie im folgenden Codebeispiel gezeigt:

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

Die- [`Animate`](xref:Xamarin.Forms.AnimationExtensions.Animate*) Methode erfordert ein *Transform* -Argument, bei dem es sich um eine Rückruf Methode handelt. Die Eingabe für diesen Rückruf liegt immer im Bereich `double` von 0 bis 1. Daher definiert die- `ColorTo` Methode eine eigene Transformation, `Func` die einen Bereich `double` von 0 bis 1 annimmt und einen Wert zurückgibt, der [`Color`](xref:Xamarin.Forms.Color) diesem Wert entspricht. Der `Color` Wert wird berechnet, indem die [`R`](xref:Xamarin.Forms.Color.R) Werte, [`G`](xref:Xamarin.Forms.Color.G) , [`B`](xref:Xamarin.Forms.Color.B) und [`A`](xref:Xamarin.Forms.Color.A) der beiden angegebenen Argumente `Color` interpolieren werden. Der `Color` Wert wird dann an die Rückruf Methode für die Anwendung an eine bestimmte Eigenschaft weitergegeben.

Bei diesem Ansatz kann die `ColorTo` Methode jede [`Color`](xref:Xamarin.Forms.Color) Eigenschaft animieren, wie im folgenden Codebeispiel gezeigt:

```csharp
await Task.WhenAll(
  label.ColorTo(Color.Red, Color.Blue, c => label.TextColor = c, 5000),
  label.ColorTo(Color.Blue, Color.Red, c => label.BackgroundColor = c, 5000));
await this.ColorTo(Color.FromRgb(0, 0, 0), Color.FromRgb(255, 255, 255), c => BackgroundColor = c, 5000);
await boxView.ColorTo(Color.Blue, Color.Red, c => boxView.Color = c, 4000);
```

In diesem Codebeispiel animiert die `ColorTo` -Methode die [`TextColor`](xref:Xamarin.Forms.Label.TextColor) -und- [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor) Eigenschaften von [`Label`](xref:Xamarin.Forms.Label) , die- `BackgroundColor` Eigenschaft einer Seite und die- [`Color`](xref:Xamarin.Forms.BoxView.Color) Eigenschaft eines [`BoxView`](xref:Xamarin.Forms.BoxView) .

## <a name="related-links"></a>Verwandte Links

- [Benutzerdefinierte Animationen (Beispiel)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-animation-custom)
- [Animation-API](xref:Xamarin.Forms.Animation)
- [AnimationExtensions-API](xref:Xamarin.Forms.AnimationExtensions)
