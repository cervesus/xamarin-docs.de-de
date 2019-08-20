---
title: Core-Animation in xamarin. IOS
description: In diesem Artikel wird das grundlegende Animations Framework erläutert, das zeigt, wie es leistungsstarke, fließende Animationen in UIKit ermöglicht, und wie es direkt für das Animations Steuerelement auf niedrigerer Ebene verwendet werden kann.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 4a5e4766321babebed9a84b37590ef1d50bfc77e
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/19/2019
ms.locfileid: "69621126"
---
# <a name="core-animation-in-xamarinios"></a>Core-Animation in xamarin. IOS

_In diesem Artikel wird das grundlegende Animations Framework erläutert, das zeigt, wie es leistungsstarke, fließende Animationen in UIKit ermöglicht, und wie es direkt für das Animations Steuerelement auf niedrigerer Ebene verwendet werden kann._

IOS umfasst die [*Kern Animation*](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) , um Animations Unterstützung für Ansichten in der Anwendung bereitzustellen.
Alle extrem Smooth Animationen in Ios, wie z. b. das Scrollen von Tabellen und das Schwenken zwischen verschiedenen Ansichten, funktionieren ebenso wie Sie, da Sie intern auf die Kern Animation basieren.

Die Kern Animationen und Kern Grafik-Frameworks können zusammenarbeiten, um schöne, animierte 2D-Grafiken zu erstellen. In der Tat kann die Kern Animation sogar 2D-Grafiken in 3D-Raum umwandeln und so beeindruckende, filmische Erfahrung schaffen. Wenn Sie jedoch echte 3D-Grafiken erstellen möchten, müssten Sie etwas wie "OpenGL es es" verwenden, oder für Spiele müssen Sie eine API wie monogame verwenden, obwohl 3D den Rahmen dieses Artikels sprengen würde.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Core Animation

IOS verwendet das Core Animation Framework zum Erstellen von Animationseffekten, wie z. b. den Übergang zwischen Sichten, gleitenden Menüs und Bild Lauf Effekten, um nur einige zu nennen. Es gibt zwei Möglichkeiten, mit Animationen zu arbeiten:

- [Über UIKit](#Using_UIKit_Animation), das Anzeige basierte Animationen sowie animierte Übergänge zwischen Controllern umfasst.
- [Über die Core-Animation](#Using_Core_Animation), die eine direkte Schicht bietet, die eine präzisere Steuerung ermöglicht.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Verwenden der UIKit-Animation

UIKit bietet verschiedene Funktionen, die das Hinzufügen von Animationen zu einer Anwendung vereinfachen. Obwohl die Kern Animation intern verwendet wird, wird Sie abstrahiert, sodass Sie nur mit Ansichten und Controllern arbeiten.

In diesem Abschnitt werden UIKit-Animations Features erläutert, einschließlich:

- Übergänge zwischen Controllern
- Übergänge zwischen Sichten
- Eigenschaften Animation anzeigen


### <a name="view-controller-transitions"></a>Übergänge von Ansichtscontrollern

 `UIViewController`bietet integrierte Unterstützung für den Übergang zwischen Sicht Controllern über die `PresentViewController` -Methode. Wenn Sie `PresentViewController`verwenden, kann der Übergang zum zweiten Controller optional animiert werden.

Angenommen, eine Anwendung mit zwei Controllern zeigt eine Schaltfläche in den ersten Controller aufrufen `PresentViewController` , um einen zweiten Controller anzuzeigen. Um zu steuern, welche Übergangs Animation verwendet wird, um den zweiten Controller anzuzeigen, [`ModalTransitionStyle`](xref:UIKit.UIModalTransitionStyle) legen Sie einfach die-Eigenschaft fest, wie unten dargestellt:

```csharp
SecondViewController vc2 = new SecondViewController {
  ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

In diesem Fall wird `PartialCurl` eine Animation verwendet, obwohl mehrere andere verfügbar sind, einschließlich:

- `CoverVertical`– Wird vom unteren Bildschirmrand nach oben verschoben.
- `CrossDissolve`– Die alte Ansicht wird ausgeblendet, & die neue Ansicht ausgeblendet wird.
- `FlipHorizontal`-Ein horizontaler, von rechts nach links. Bei einer Kündigung wird der Übergang von links nach rechts zurück geflippt.


Um den Übergang zu animieren, `true` übergeben Sie als zweites Argument `PresentViewController`an:

```csharp
PresentViewController (vc2, true, null);
```

Der folgende Screenshot zeigt, wie der Übergang für den `PartialCurl` Fall aussieht:

 ![](core-animation-images/06-view-transitions.png "Dieser Screenshot zeigt den partialcurl-Übergang.")

### <a name="view-transitions"></a>Übergänge anzeigen

Zusätzlich zu den Übergängen zwischen Controllern unterstützt UIKit auch das Animieren von Übergängen zwischen Sichten, um eine Ansicht für eine andere auszutauschen.

Angenommen, Sie haben einen Controller mit `UIImageView`, wobei auf das Bild ein zweites `UIImageView`angezeigt werden soll. Um die übergeordnete Ansicht der Bildansicht so zu animieren, dass Sie in die zweite Bildansicht übergeht `UIView.Transition`, ist der Aufruf `toView` von `fromView` so einfach wie das Aufrufen von und wie unten dargestellt:

```csharp
UIView.Transition (
  fromView: view1,
  toView: view2,
  duration: 2,
  options: UIViewAnimationOptions.TransitionFlipFromTop |
    UIViewAnimationOptions.CurveEaseInOut,
  completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition`nimmt auch einen `duration` Parameter [`options`](xref:UIKit.UIViewAnimationOptions) an, der steuert, wie lange die Animation ausgeführt wird, und wie die zu verwendende Animation und die Beschleunigungs Funktion angegeben werden. Darüber hinaus können Sie einen Abschluss Handler angeben, der nach Abschluss der Animation aufgerufen wird.

Der folgende Screenshot zeigt den animierten Übergang zwischen den Bildansichten, `TransitionFlipFromTop` wenn verwendet wird:

 ![](core-animation-images/07-animated-transition.png "Dieser Screenshot zeigt den animierten Übergang zwischen den Bildansichten, wenn transitionflipfromtop verwendet wird.")

### <a name="view-property-animations"></a>Anzeigen von Eigenschafts Animationen

UIKit unterstützt eine Vielzahl von Eigenschaften für die `UIView` -Klasse kostenlos, einschließlich:

- Frame
- Fugen
- Center
- Alpha
- Transformation
- Farbe


Diese Animationen werden implizit durch Angeben von Eigenschafts Änderungen `NSAction` in einem an die statische `UIView.Animate` Methode übergebenen Delegaten ausgeführt. Mit dem folgenden Code wird beispielsweise der Mittelpunkt eines `UIImageView`animiert:

```csharp
pt = imgView.Center;

UIView.Animate (
  duration: 2, 
  delay: 0, 
  options: UIViewAnimationOptions.CurveEaseInOut | 
    UIViewAnimationOptions.Autoreverse,
  animation: () => {
    imgView.Center = new CGPoint (View.Bounds.GetMaxX () 
      - imgView.Frame.Width / 2, pt.Y);},
  completion: () => {
    imgView.Center = pt; }
);
```

Dies führt dazu, dass ein Bild über den oberen Bildschirmrand hin und her animiert wird, wie unten dargestellt:

 ![](core-animation-images/08-animate-center.png "Ein Bild, das auf der oberen Seite des Bildschirms als Ausgabe hin und her animiert.")

Wie bei der `Transition` - `Animate` Methode ermöglicht die Festlegung der Dauer zusammen mit der Beschleunigungs Funktion. In diesem Beispiel wurde auch `UIViewAnimationOptions.Autoreverse` die-Option verwendet, die bewirkt, dass die Animation von dem Wert zurück zum ersten wird. Der Code legt jedoch auch die auf `Center` den Anfangswert in einem Abschluss Handler zurück. Während eine Animation Eigenschaftswerte im Laufe der Zeit interpolieren kann, ist der tatsächliche Modell Wert der Eigenschaft immer der endgültige Wert, der festgelegt wurde. In diesem Beispiel ist der Wert ein Punkt in der Nähe der rechten Seite der übergeordneten Ansicht. Wenn `Center` Sie `Autoreverse` nicht auf den Anfangspunkt festlegen, an dem die Animation abgeschlossen ist, weil Sie festgelegt wird, wird das Bild nach Abschluss der Animation wieder nach rechts ausgerichtet, wie unten dargestellt:

 ![](core-animation-images/09-animation-complete.png "Wenn die Mitte nicht auf den ersten Punkt festgelegt ist, wird das Bild nach Abschluss der Animation wieder nach rechts ausgerichtet.")

## <a name="using-core-animation"></a>Verwenden der Core-Animation

 `UIView`Animationen ermöglichen viele Funktionen und sollten nach Möglichkeit aufgrund der einfachen Implementierung verwendet werden. Wie bereits erwähnt, verwenden UIView-Animationen das Core Animation Framework. Allerdings können einige Dinge nicht mit `UIView` Animationen durchgeführt werden, wie z. b. das Animieren zusätzlicher Eigenschaften, die nicht mit einer Ansicht animiert werden können, oder das interpolieren an einem nichtlinearen Pfad. In solchen Fällen, in denen Sie eine präzisere Steuerung benötigen, kann die Kern Animation auch direkt verwendet werden.

### <a name="layers"></a>Maurer

Beim Arbeiten mit der Kern Animation erfolgt die Animation über *Ebenen*, die vom Typ `CALayer`sind. Eine Ebene ähnelt konzeptionell einer Ansicht darin, dass es eine Ebenenhierarchie gibt, ähnlich wie eine Ansichts Hierarchie. Tatsächlich werden Ansichten zurückgenommen, und die Ansicht fügt Unterstützung für die Benutzerinteraktion hinzu. Sie können über die- `Layer` Eigenschaft der Sicht auf die Ebene einer beliebigen Ansicht zugreifen. Tatsächlich wird der Kontext, der in `Draw` der- `UIView` Methode von verwendet wird, tatsächlich aus der-Ebene erstellt. Intern hat der Delegat, `UIView` der ein-Element unterstützt, auf die Ansicht selbst festgelegt `Draw`, was wiederum aufruft. Wenn Sie also auf einen `UIView`zeichnen, zeichnen Sie tatsächlich auf seine Ebene.

Ebenenanimationen können entweder implizit oder explizit sein. Implizite Animationen sind deklarativ. Sie deklarieren einfach, welche Ebeneneigenschaften geändert werden sollen, und die Animation funktioniert einfach. Explizite Animationen werden hingegen über eine Animations Klasse erstellt, die einer Ebene hinzugefügt wird. Explizite Animationen ermöglichen eine zusätzliche Kontrolle über die Erstellung einer Animation. In den folgenden Abschnitten werden implizite und explizite Animationen ausführlicher beschrieben.

### <a name="implicit-animations"></a>Implizite Animationen

Eine Möglichkeit, die Eigenschaften einer Ebene zu animieren, ist eine implizite Animation. `UIView`Animationen erstellen implizite Animationen. Sie können jedoch auch implizite Animationen direkt für eine Ebene erstellen.

Der folgende Code legt z. b. eine Ebene `Contents` von einem Bild fest, legt eine Rahmenbreite und-Farbe fest und fügt die Ebene als Unterebene der Ebene der Ansicht hinzu:

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  layer = new CALayer ();
  layer.Bounds = new CGRect (0, 0, 50, 50);
  layer.Position = new CGPoint (50, 50);
  layer.Contents = UIImage.FromFile ("monkey2.png").CGImage;
  layer.ContentsGravity = CALayer.GravityResize;
  layer.BorderWidth = 1.5f;
  layer.BorderColor = UIColor.Green.CGColor;

  View.Layer.AddSublayer (layer);
}
```

Um eine implizite Animation für die Ebene hinzuzufügen, können Sie Eigenschafts Änderungen `CATransaction`einfach in einem umschließen. Dies ermöglicht das Animieren von Eigenschaften, die nicht mit einer Ansichts Animation, wie z `BorderWidth` . b. und `BorderColor` , wie unten dargestellt animiert werden können:

```csharp
public override void ViewDidAppear (bool animated)
{
  base.ViewDidAppear (animated);

  CATransaction.Begin ();
  CATransaction.AnimationDuration = 10;
  layer.Position = new CGPoint (50, 400);
  layer.BorderWidth = 5.0f;
  layer.BorderColor = UIColor.Red.CGColor;
  CATransaction.Commit ();
}
```

Mit diesem Code `Position`werden auch die Ebenen der Ebene animiert, bei der es sich um die Position des Anker Punkts der Ebene handelt, die von der oberen linken Ecke der Koordinaten der oberebenenebene gemessen wird. Der Ankerpunkt einer Ebene ist ein normalisierter Punkt innerhalb des Koordinatensystems der Ebene.

Die folgende Abbildung zeigt die Position und den Ankerpunkt:

 ![](core-animation-images/10-postion-anchorpt.png "Diese Abbildung zeigt die Position und den Ankerpunkt.")

Wenn das Beispiel ausgeführt wird, werden `Position`die `BorderWidth` - `BorderColor` und animiert, wie in den folgenden Screenshots gezeigt:

 ![](core-animation-images/11-implicit-animation.png "Wenn das Beispiel ausgeführt wird, werden die Position, die BorderWidth-und die BorderColor-Animation wie dargestellt animiert.")

### <a name="explicit-animations"></a>Explizite Animationen

Neben impliziten Animationen umfasst die Core-Animation eine Reihe von Klassen, die von `CAAnimation` erben, mit denen Sie Animationen kapseln können, die dann explizit zu einer Ebene hinzugefügt werden. Diese ermöglichen eine präzisere Steuerung von Animationen, z. b. das Ändern des Startwerts einer Animation, das Gruppieren von Animationen und das Angeben von Keyframes, um nichtlineare Pfade zuzulassen.

Der folgende Code zeigt ein Beispiel für eine explizite Animation mit einem `CAKeyframeAnimation` für die zuvor gezeigte Ebene (im Abschnitt implizite Animation):

```csharp
public override void ViewDidAppear (bool animated)
{
  base.ViewDidAppear (animated);
  
  // get the initial value to start the animation from
  CGPoint fromPt = layer.Position;
  
  /* set the position to coincide with the final animation value
  to prevent it from snapping back to the starting position
  after the animation completes*/
  layer.Position = new CGPoint (200, 300);
  
  // create a path for the animation to follow
  CGPath path = new CGPath ();
  path.AddLines (new CGPoint[] { fromPt, new CGPoint (50, 300), new CGPoint (200, 50), new CGPoint (200, 300) });
  
  // create a keyframe animation for the position using the path
  CAKeyFrameAnimation animPosition = (CAKeyFrameAnimation)CAKeyFrameAnimation.FromKeyPath ("position");
  animPosition.Path = path;
  animPosition.Duration = 2;
  
  // add the animation to the layer.
  /* the "position" key is used to overwrite the implicit animation created
  when the layer positino is set above*/
  layer.AddAnimation (animPosition, "position");
}
```

Dieser Code ändert den `Position` der Ebene durch Erstellen eines Pfads, der dann zum Definieren einer Keyframe-Animation verwendet wird. Beachten Sie, dass der `Position` der Ebene auf den endgültigen Wert `Position` von aus der Animation festgelegt ist. Ohne diesen Wert würde die Ebene plötzlich zu Ihrer `Position` vor der Animation zurückkehren, da die Animation nur den Präsentations Wert und nicht den tatsächlichen Modell Wert ändert. Wenn Sie den Modell Wert auf den endgültigen Wert aus der Animation festlegen, bleibt die Ebene am Ende der Animation bestehen.

Die folgenden Screenshots zeigen die Ebene mit dem Bild, das die Animation durch den angegebenen Pfad enthält:

 ![](core-animation-images/12-explicit-animation.png "Dieser Screenshot zeigt die Ebene mit dem Bild, das die Animation durch den angegebenen Pfad enthält.")
 
## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir uns mit den Animations Funktionen befasst, die über die *wichtigsten Animations* Frameworks bereitgestellt werden. Wir haben die Kern Animation untersucht, die zeigt, wie Sie Animationen in UIKit unterstützt, und wie Sie direkt für das Animations Steuerelement auf niedrigerer Ebene verwendet werden kann.

## <a name="related-links"></a>Verwandte Links

- [Beispiel für eine Kern Animation](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Exemplarische Vorgehensweise zu Grafiken und Animationen](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
