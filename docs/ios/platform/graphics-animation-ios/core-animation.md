---
title: Animation "Core" in Xamarin.iOS
description: In diesem Artikel werden die Core Animation-Framework, das zeigt, wie dieses leistungsstarken und flüssigen Animationen in UIKit, und wie ermöglicht, direkt für Low-Level-Steuerelements verwendet.
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 3d26e58822385c20f3c08d0b75ba468467c2c9b1
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242130"
---
# <a name="core-animation-in-xamarinios"></a>Animation "Core" in Xamarin.iOS

_In diesem Artikel werden die Core Animation-Framework, das zeigt, wie dieses leistungsstarken und flüssigen Animationen in UIKit, und wie ermöglicht, direkt für Low-Level-Steuerelements verwendet._

iOS enthält [ *Core Animation* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) Ansichten in Ihrer Anwendung Unterstützung für Animationen bereit.
Alle der extrem flüssige Animationen in iOS z. B. Durchführen eines Bildlaufs von Tabellen und zwischen verschiedenen workflowansichten streifbewegung ausführen sowie dazu, da sie intern auf Core Animation abhängig sind.

Die Core Animation und Core Graphics-Frameworks zusammenarbeiten, um erstellen Sie ansprechende, 2D-Grafiken animiert. Core Animation können tatsächlich auch 2D-Grafiken im 3D-Raum, transformieren, Erstellen von staunenswerten, Kino-Funktionen. Allerdings um "true" 3D-Grafiken zu erstellen, müssten Sie etwas wie OpenGL-ES, oder für die Spiele-Reihe, die eine API wie MonoGame, verwenden auch 3D über den Rahmen dieses Artikels ist.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Core-Animation

iOS verwendet das Framework Core Animation, zum Erstellen von Animationseffekten, z. B. Übergang zwischen den Ansichten, gleitend Menüs und Durchführen eines Bildlaufs Effekte, um nur einige zu nennen. Es gibt zwei Möglichkeiten zum Arbeiten mit Animation:

- [Über UIKit](#Using_UIKit_Animation), wozu ansichtsbasierte Animationen sowie animierten Übergänge zwischen Domänencontrollern.
- [Über Core Animation](#Using_Core_Animation), welche Ebenen direkt für eine differenziertere Steuerung ermöglicht.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Verwenden von UIKit-Animation

UIKit bietet mehrere Features, die leicht Animation zu einer Anwendung hinzufügen. Obwohl sie intern Core Animation verwendet wird, abstrahiert sie es, sodass Sie nur mit Ansichten und Controller arbeiten.

In diesem Abschnitt wird erläutert, einschließlich UIKit-Animationsfunktionen:

-  Übergänge zwischen Domänencontrollern
-  Übergänge zwischen den Ansichten
-  Die Eigenschaft Animation anzeigen


### <a name="view-controller-transitions"></a>Übergänge von Ansichtscontrollern

 `UIViewController` bietet integrierte Unterstützung für den Übergang zwischen ansichtscontrollern über die `PresentViewController` Methode. Bei Verwendung `PresentViewController`, der Übergang mit dem zweiten Controller kann optional animiert werden.

Betrachten Sie beispielsweise eine Anwendung mit zwei Controllern, die, in denen durch Berühren einer Schaltfläche in der erste Controller aufruft `PresentViewController` um einen zweiten Controller anzuzeigen. Um zu steuern, welche übergangsanimation verwendet wird, um den zweiten Controller anzuzeigen, legen Sie einfach die [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) Eigenschaft wie unten dargestellt:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

In diesem Fall eine `PartialCurl` Animation verwendet wird, obwohl andere zur Verfügung, darunter sind:

-  `CoverVertical` – Folien sich aus den unteren Rand des Bildschirms
-  `CrossDissolve` – Die alte Ansicht ausgeblendet wird und die neue Ansicht eingeblendet
-  `FlipHorizontal` -Eine horizontale rechts-nach-Links spiegeln. Auf einer Entlassung spiegelt der Übergang links-nach-rechts.


Um den Übergang zu animieren, übergeben Sie `true` als das zweite Argument für `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Der folgende Screenshot zeigt, wie der Übergang aussieht, für die `PartialCurl` Fall:

 ![](core-animation-images/06-view-transitions.png "Dieser Screenshot zeigt den PartialCurl Übergang")

### <a name="view-transitions"></a>Anzeigen von Übergängen

Zusätzlich zu der Übergänge zwischen Domänencontrollern unterstützt UIKit auch die Animation von Übergänge zwischen den Ansichten, um eine Ansicht für eine andere auszutauschen.

Angenommen, Sie haben einen Controller mit `UIImageView`, in dem Tippen auf das Bild, ein zweites anzeigen soll `UIImageView`. Um das Bild zu animieren Ansicht Benachrichtigungen zum Übergang in die zweite Ansicht des Image ist so einfach wie Aufrufen `UIView.Transition`, und übergeben sie die `toView` und `fromView` wie unten dargestellt:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` nimmt auch einen `duration` Parameter, der steuert, wie lange die Animation ausgeführt wird, als auch [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) Dinge wie z. B. die Animation zu verwenden und die Beschleunigungsfunktion an. Darüber hinaus können Sie einen Abschlusshandler angeben, der aufgerufen wird, wenn die Animation abgeschlossen wird.

Der Screenshot unten zeigen der animierte Übergang zwischen dem Bild Sichten, wenn `TransitionFlipFromTop` wird verwendet:

 ![](core-animation-images/07-animated-transition.png "Dieser Screenshot zeigt die animierten Übergang zwischen den Image-Ansichten, wenn TransitionFlipFromTop verwendet wird")

### <a name="view-property-animations"></a>Die Eigenschaftenanimationen anzeigen

UIKit unterstützt eine Vielzahl von Eigenschaften animieren, auf die `UIView` kostenlos, Klasse, einschließlich:

-  Frame
-  Grenzen
-  Center
-  Alpha
-  Transformation
-  Farbe


Diese Animationen auftreten implizit durch Angabe von eigenschaftenänderungen in einer `NSAction` Delegaten zu übergeben, um die statische `UIView.Animate` Methode. Beispielsweise der folgende Code erstellt eine Animation den Mittelpunkt einer `UIImageView`:

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

Dies führt zu einem Bild animieren hin-und am oberen Rand des Bildschirms, wie unten dargestellt:

 ![](core-animation-images/08-animate-center.png "Ein Bild animieren hin-und am oberen Rand des Bildschirms als Ausgabe")

Wie bei der `Transition` Methode `Animate` können Sie die Dauer, die zusammen mit der Beschleunigungsfunktion festgelegt werden. In diesem Beispiel verwendet auch die `UIViewAnimationOptions.Autoreverse` auswählen, um die Animation vom Wert wieder mit dem ersten animiert wird. Im Code jedoch auch wird die `Center` wieder auf den ersten Wert in einen Abschlusshandler. Während eine Animation im Laufe der Zeit die Eigenschaftswerte interpolieren ist, ist der Eigenschaft vom tatsächlichen Modellwert immer den endgültigen Wert an, der festgelegt wurde. In diesem Beispiel ist der Wert einen Punkt in der Nähe der rechten Seite der Überblicksansicht. Festlegen, ohne die `Center` auf dem ersten Punkt, d.h., in dem der Animation aufgrund der `Autoreverse` festgelegt wird, das Bild würde ausrichten zurück auf die rechte Seite nach der Animation abgeschlossen wird, wie unten dargestellt:

 ![](core-animation-images/09-animation-complete.png "Ohne das Center auf den ersten Punkt festlegen, würde das Image zurück auf die rechte Seite ausrichten nach Ende der Animation")

## <a name="using-core-animation"></a>Mit der Core-Animation

 `UIView` Animationen können einen Großteil der Funktionalität und sollte möglichst aufgrund der einfachen Implementierung verwendet werden. Wie bereits erwähnt, verwenden Sie das Framework Core Animation UIView-Animationen. Jedoch einige Dinge nicht möglich sein mit `UIView` Animationen, z. B. Animieren von zusätzlichen Eigenschaften, die mit einer Ansicht nicht animiert werden können oder auf einem nicht linearen Pfad interpolieren. In solchen Fällen, in dem Sie eine präzisere Kontrolle benötigen, kann Core Animation auch direkt verwendet werden.

### <a name="layers"></a>Ebenen

Bei der Arbeit mit Core Animation geschieht Animation über *Ebenen*, des Typs `CALayer`. Eine Ebene ist konzeptionell identisch mit einer Ansicht, in, dass es viel eine Ebenenhierarchie, ist wie eine Hierarchie von Inhaltsansichten. Ebenen sichern, Ansichten, mit der Ansicht hinzufügen von Unterstützung für die Benutzerinteraktion. Sie können auf die Ebene einer beliebigen Ansicht über der Ansicht zugreifen `Layer` Eigenschaft. Der Kontext tatsächlich verwendet, `Draw` -Methode der `UIView` wird tatsächlich von der Ebene erstellt. Intern wird die Ebene, die Sicherung einer `UIView` verfügt der Delegat, legen Sie für die Sicht selbst, was bezeichnet wird `Draw`. So zeichnen Sie auf eine `UIView`, Sie tatsächlich Zeichnen auf der Ebene.

Layer-Animationen können entweder implizit oder explizit sein. Implizite Animationen sind deklarative. Deklarieren Sie lediglich was Ebeneneigenschaften geändert werden sollte, und die Animation funktioniert. Explizite Animationen werden auf der anderen Seite über eine Animationsklasse erstellt, die das einer Ebene hinzugefügt wird. Explizite Animationen können außerdem steuern wie eine Animation erstellt wird. In den folgenden Abschnitten detailliert implizite und explizite Animationen ausführlich ab.

### <a name="implicit-animations"></a>Implizite Animationen

Eine Möglichkeit zum Animieren von Eigenschaften einer Ebene erfolgt über eine implizite Animation. `UIView` Animationen werden implizite Animationen erstellen. Allerdings können Sie implizite Animationen direkt für eine Ebene auch erstellen.

Im folgenden Code wird z. B. einer Ebene des `Contents` aus einem Image eine Breite des Rahmens und die Farbe und fügt diese als eine Ebene der Ebene von der Ansicht:

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

Um eine implizite Animation für die Ebene hinzuzufügen, einfach einen Wrapper für eigenschaftenänderungen in einer `CATransaction`. Dies ermöglicht das Animieren von Eigenschaften, die nicht mit einer Animation anzeigen animierbaren wie z. B. die `BorderWidth` und `BorderColor` wie unten dargestellt:

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

Dieser Code erstellt eine Animation in der Schicht `Position`, d.h. den Speicherort der Schicht Ankerpunkt von oben links auf der die Superlayer die Koordinaten gemessen. Der Ankerpunkt der eine Ebene ist ein normalisierter Punkt im Koordinatensystem der Schicht.

Die folgende Abbildung zeigt die Position und den Anker Punkt:

 ![](core-animation-images/10-postion-anchorpt.png "Diese Abbildung zeigt die Position und den Anker-Punkt")

Wenn das Beispiel ausgeführt wird, die `Position`, `BorderWidth` und `BorderColor` animieren, wie in den folgenden Screenshots gezeigt:

 ![](core-animation-images/11-implicit-animation.png "Wenn das Beispiel ausgeführt wird, animieren die Position, BorderWidth und BorderColor Siehe")

### <a name="explicit-animations"></a>Explizite Animationen

Zusätzlich zu den impliziten Animationen, Core Animation, die eine Vielzahl von Klassen, die von erben enthält `CAAnimation` , mit denen Sie die Animationen zu kapseln, die auf eine Ebene dann explizit hinzugefügt werden. Diese ermöglichen eine differenziertere Kontrolle über die Animationen, z. B. den Startwert einer Animation ändern, Gruppieren von Animationen und des Angebens von Keyframes, um Pfade von nichtlinearen zu ermöglichen.

Der folgende Code zeigt ein Beispiel für explizite Animationen mit einem `CAKeyframeAnimation` für die Ebene, die weiter oben (im Abschnitt "implizite Animation") dargestellt:

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

Dieser Code ändert die `Position` der Schicht erstellen Sie einen Pfad, der dann zum Definieren einer Keyframeanimation verwendet wird. Beachten Sie, dass der Schicht `Position` festgelegt ist, um den endgültigen Wert von der `Position` aus der Animation. Ohne diesen Schritt würde die Ebene abrupt zum Zurückgeben der `Position` vor der Animation, da die Animation nur den Wert für die Präsentation und nicht vom tatsächlichen Modellwert ändert. Durch Festlegen von den Modellwert in den letzten Wert der Animation, bleiben die Ebene in direkte am Ende der Animation.

Die folgenden Screenshots zeigen die Ebene, in dem Image-Animation über den angegebenen Pfad:

 ![](core-animation-images/12-explicit-animation.png "Dieser Screenshot zeigt die Ebene, in dem Image-Animation über den angegebenen Pfad")
 
## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, die Animation Funktionen über die *Core Animation* Frameworks. Wir untersucht, Core Animation, gezeigt sowohl, wie sie Animationen in UIKit unterstützt und wie sie für Low-Level-Animationssteuerelements direkt verwendet werden kann.

## <a name="related-links"></a>Verwandte Links

- [Core Animation – Beispiel](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafiken und Animation Exemplarische Vorgehensweise](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
