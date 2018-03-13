---
title: Core-Animation
description: "In diesem Artikel untersucht das Core-Animation-Framework, das anzeigt, wie sie hohen Leistung, flexible Animationen in UIKit, sowie dazu, wie ermöglicht, direkt für Animationssteuerelements auf niedrigerer Ebene verwendet."
ms.topic: article
ms.prod: xamarin
ms.assetid: D4744147-FACB-415B-8155-3A6B3C35E527
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f0cb4e00abffead854c2590bde6df45c200ff0bb
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="core-animation"></a>Core-Animation

_In diesem Artikel untersucht das Core-Animation-Framework, das anzeigt, wie sie hohen Leistung, flexible Animationen in UIKit, sowie dazu, wie ermöglicht, direkt für Animationssteuerelements auf niedrigerer Ebene verwendet._

iOS umfasst [ *Core Animation* ](https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html) um Animation-Unterstützung für Ansichten in der Anwendung bereitzustellen.
Alle hochauflösende Animationen in iOS z. B. Durchführen eines Bildlaufs von Tabellen und zwischen verschiedenen Ansichten Streifen ausführen sowie diese zu tun, denn sie intern von Core Animation abhängig sind.

Die Core-Animationen und Core Grafiken Frameworks zusammenarbeiten, um ansprechende, erstellen Sie animierte 2D Grafiken. Tatsächlich kann Core Animation selbst 2D Grafiken in 3D-Bereich, transformiert erstaunliche, Kino Oberflächen zu erstellen. Allerdings würde um "true" 3D-Grafiken zu erstellen, Sie müssen etwas wie OpenGL ES oder für Spiele, aktivieren Sie auf eine API wie MonoGame, verwenden auch 3D nicht Gegenstand dieses Artikels ist.

<a name="Using_Core_Animation" />

## <a name="core-animation"></a>Core-Animation

iOS verwendet das Framework Core-Animation um Animationseffekte wie Übergang zwischen Ansichten, Menüs gleitende und Durchführen eines Bildlaufs Effekte, um nur einige zu nennen zu erstellen. Es gibt zwei Verfahren zum Arbeiten mit Animation:

- [Über UIKit](#Using_UIKit_Animation), inklusive der anzeigen-basierte Animationen sowie animierte Übergänge zwischen Domänencontrollern.
- [Über Core Animation](#Using_Core_Animation), welche Ebenen direkt, bei dem Steuerelement eine umfassendere.

<a name="Using_UIKit_Animation" />

## <a name="using-uikit-animation"></a>Verwenden von UIKit Animationen

UIKit stellt mehrere Funktionen, die zum Hinzufügen einer Animation zu einer Anwendung vereinfachen. Obwohl es Core Animation intern verwendet wird, abstrahiert er es so, dass Sie nur mit Ansichten und Controllern arbeiten.

In diesem Abschnitt wird erläutert, UIKit Animation-Funktionen, einschließlich:

-  Übergänge zwischen Domänencontrollern
-  Übergänge zwischen den Ansichten
-  Die Eigenschaft Animation anzeigen


### <a name="view-controller-transitions"></a>View-Controller-Übergänge

 `UIViewController` bietet integrierte Unterstützung für den Wechsel zwischen View-Controller über die `PresentViewController` Methode. Bei Verwendung `PresentViewController`, der Übergang mit dem zweiten Controller optional animiert werden kann.

Betrachten Sie beispielsweise eine Anwendung mit zwei Controllern, wenn eine Schaltfläche in der ersten Controller berühren aufruft `PresentViewController` um einen zweiten Controller anzuzeigen. Um zu steuern, welche Übergangsanimationen zum Anzeigen des zweiten Controllers verwendet wird, legen Sie einfach die [ `ModalTransitionStyle` ](https://developer.xamarin.com/api/type/UIKit.UIModalTransitionStyle/) Eigenschaft wie unten dargestellt:

```csharp
SecondViewController vc2 = new SecondViewController {
    ModalTransitionStyle = UIModalTransitionStyle.PartialCurl
};
```

In diesem Fall eine `PartialCurl` Animation verwendet wird, obwohl es sich bei einigen weiteren Bildschirmen zur Verfügung, darunter sind:

-  `CoverVertical` – Folien sich vom unteren Rand des Bildschirms
-  `CrossDissolve` – Die alte Ansicht ausgeblendet und die neue Ansicht eingeblendet
-  `FlipHorizontal` -Eine horizontale rechts-nach-Links spiegeln. Für die Kündigung spiegelt der Übergang links nach rechts.


Um den Übergang zu animieren, übergeben `true` als das zweite Argument `PresentViewController`:

```csharp
PresentViewController (vc2, true, null);
```

Der folgende Screenshot zeigt, wie der Übergang aussieht, für die `PartialCurl` Fall:

 ![](core-animation-images/06-view-transitions.png "Diese bildschirmabbildung zeigt den PartialCurl Übergang")

### <a name="view-transitions"></a>Ansicht Übergänge

Neben dem Übergänge zwischen Domänencontrollern unterstützt UIKit auch Animation Übergänge zwischen den Ansichten, eine Ansicht für einen anderen auszutauschen.

Sagen Sie z. B. einen Controller mit hatten `UIImageView`, in denen ein zweites Tippen auf das Bild angezeigt werden soll `UIImageView`. Um das Bild zu animieren der Sicht Überblicksansicht für den Übergang in der zweiten Ansicht Image ist so einfach wie Aufrufen `UIView.Transition`, und übergeben sie die `toView` und `fromView` wie unten dargestellt:

```csharp
UIView.Transition (
    fromView: view1,
    toView: view2,
    duration: 2,
    options: UIViewAnimationOptions.TransitionFlipFromTop |
        UIViewAnimationOptions.CurveEaseInOut,
    completion: () => { Console.WriteLine ("transition complete"); });
```

`UIView.Transition` nimmt auch einen `duration` Parameter, der steuert, wie lange die Animation ausgeführt wird, sowie [ `options` ](https://developer.xamarin.com/api/type/UIKit.UIViewAnimationOptions/) z. B. Animation und Beschleunigungsfunktion angeben. Darüber hinaus können Sie eine Abschlusshandler angeben, die aufgerufen wird, wenn der Animation.

Der Screenshot unten Anzeigen der animierte Übergang zwischen dem Bild Sichten, wenn `TransitionFlipFromTop` verwendet wird:

 ![](core-animation-images/07-animated-transition.png "Diese bildschirmabbildung zeigt den animierten Übergang zwischen den Ansichten des Bilds Wenn TransitionFlipFromTop verwendet wird")

### <a name="view-property-animations"></a>Die Eigenschaftenanimationen anzeigen

UIKit unterstützt Animieren einer Reihe von Eigenschaften auf der `UIView` Klasse kostenlos, einschließlich:

-  Frame
-  Grenzen
-  Center
-  Alpha
-  Transformation
-  Farbe


Diese Animationen geschehen implizit durch Angabe von eigenschaftenänderungen in einer `NSAction` Delegaten übergeben werden, um die statische `UIView.Animate` Methode. Z. B. der folgende Code erstellt eine Animation den Mittelpunkt einer `UIImageView`:

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

Daraus ergibt sich ein Bild animieren hin-und am oberen Fensterrand, wie unten dargestellt:

 ![](core-animation-images/08-animate-center.png "Ein Bild animieren hin-und am oberen Rand des Bildschirms als Ausgabe")

Wie bei der `Transition` Methode `Animate` können Sie die Dauer, die zusammen mit der Beschleunigungsfunktion festgelegt werden. In diesem Beispiel verwendet auch die `UIViewAnimationOptions.Autoreverse` , die bewirkt, die Animation zum Animieren aus dem Wert wieder auf das erste dass Option. Allerdings setzt der Code auch die `Center` wieder auf seinen ursprünglichen Wert in eine Abschlusshandler. Während eine Animation Eigenschaftswerte über die Zeit interpolieren ist, ist der Eigenschaft vom tatsächlichen Modellwert immer der endgültige Wert, der festgelegt wurde. In diesem Beispiel ist der Wert einen Punkt in der Nähe der rechten Seite der Überblicksansicht. Festlegen, ohne die `Center` auf den ersten Punkt ist, in dem der Animation aufgrund der `Autoreverse` festgelegt wird, das Bild würde ausrichten wieder auf die rechte Seite nach Ende der Animation, wie unten dargestellt:

 ![](core-animation-images/09-animation-complete.png "Ohne Festlegung der Center auf den ersten Punkt, würde das Bild wieder auf die rechte Seite ausrichten nach Ende der Animation")

## <a name="using-core-animation"></a>Mit der Animation Core

 `UIView` Animationen können viele Funktionen und sollte möglichst aufgrund der einfachen Implementierung verwendet werden. Wie bereits erwähnt, verwenden Sie UIView Animationen das Framework Core-Animation. Allerdings können nicht einige Aufgaben können ausgeführt werden, mit `UIView` Animationen, z. B. Animieren von zusätzlichen Eigenschaften, die mit einer Ansicht animiert werden können oder für einen nicht linearen Interpolation. In solchen Fällen, in dem Sie eine genauere Steuerung des benötigen, können Core Animation ebenfalls direkt verwendet werden.

### <a name="layers"></a>Ebenen

Bei der Arbeit mit Core Animation geschieht Animation über *Ebenen*, vom Typ `CALayer`. Eine Ebene gleicht konzeptionell eine Ansicht, dass es sehr viel eine Ebenenhierarchie, ist wie es eine Hierarchie anzeigen ist. Ebenen sichern tatsächlich, Ansichten, mit der Ansicht hinzufügen der Unterstützung für die Interaktion des Benutzers. Sie können auf die Ebene von einer beliebigen Ansicht über der Ansicht zugreifen `Layer` Eigenschaft. Tatsächlich verwendet der Kontext `Draw` Methode `UIView` wird tatsächlich von der Ebene erstellt. Intern wird die Ebene, Sichern einer `UIView` seines Delegaten für die Sicht selbst, was ruft also festgelegt hat `Draw`. Daher bei der zum Zeichnen einer `UIView`, Sie sind tatsächlich auf einer Ebene gezeichnet werden.

Layer-Animationen können entweder implizit oder explizit sein. Implizite Animationen sind deklarative. Deklarieren Sie einfach was Ebeneneigenschaften geändert werden sollte und genauso funktioniert wie die Animation. Explizite Animationen werden dagegen über eine Animationsklasse erstellt, die das einer Ebene hinzugefügt wird. Explizite Animationen ermöglichen außerdem die Erstellung einer Animation steuern. In den folgenden Abschnitten stürzen implizite und explizite Animationen ausführlicher beschrieben.

### <a name="implicit-animations"></a>Implizite Animationen

Eine Möglichkeit, die Eigenschaften einer Ebene animiert wird über eine implizite Animation. `UIView` Animationen erstellen implizite Animationen. Allerdings können Sie direkt für eine Ebene als auch implizite Animationen erstellen.

Im folgenden Code wird z. B. einer Ebene `Contents` aus einem Image eine Rahmenbreite und die Farbe und die Sicherheitsebene als eine Unterebene Ebene für die Sicht:

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

Um eine implizite Animation für die Ebene hinzuzufügen, umschließen Sie einfach eigenschaftenänderungen in einer `CATransaction`. Animieren von Eigenschaften, die nicht mit einer Animation anzeigen animierbaren wie z. B. wäre dadurch die `BorderWidth` und `BorderColor` wie unten dargestellt:

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

Dieser Code erstellt eine Animation in der Ebene `Position`, also den Speicherort der der Ebene Ankerpunkt vom oberen linken Rand der Superlayer Koordinaten gemessen. Der Ankerpunkt einer Ebene wird eine normalisierte Punkt innerhalb der Ebene-Koordinatensystem.

Die folgende Abbildung zeigt die Position und Anker Punkt:

 ![](core-animation-images/10-postion-anchorpt.png "Diese Abbildung zeigt die Position und Premium-Punkt")

Wenn das Beispiel ausgeführt wird, die `Position`, `BorderWidth` und `BorderColor` animieren, wie in den folgenden Screenshots dargestellt:

 ![](core-animation-images/11-implicit-animation.png "Wenn das Beispiel ausgeführt wird, animieren die Position, BorderWidth und BorderColor gezeigten")

### <a name="explicit-animations"></a>Explizite Animationen

Zusätzlich zu den impliziten Animationen Core Animation enthält eine Reihe von Klassen, die von erben `CAAnimation` , mit deren Hilfe Sie Animationen kapseln, die anschließend explizit an eine Ebene hinzugefügt werden. Diese ermöglichen eine umfassendere Steuerung für Animationen, z. B. ändern den Startwert der Animation, gruppieren Animationen und des Angebens von Keyframes um nichtlineare Pfade zuzulassen.

Der folgende Code zeigt ein Beispiel für eine explizite Animation mit einer `CAKeyframeAnimation` für die Ebene, die zuvor (im Abschnitt "implizite Animation") dargestellt:

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

Dieser Code ändert den `Position` der Ebene von beim Erstellen eines Pfads, der dann zum Definieren der Keyframe-Animation verwendet wird. Beachten Sie, dass der Ebene `Position` festgelegt ist, um den endgültigen Wert von der `Position` aus der Animation. Ohne diesen Schritt würde die Ebene abrupt zum Zurückgeben der `Position` vor der Animation, da die Animation nur den Wert der Präsentation und nicht vom tatsächlichen Modellwert ändert. Durch Festlegen von den Modellwert auf den endgültigen Wert der Animation, bleiben die Ebene an das Ende der Animation.

Die folgenden Screenshots zeigen die Ebene mit dem Image animieren über den angegebenen Pfad an:

 ![](core-animation-images/12-explicit-animation.png "Diese bildschirmabbildung zeigt die Ebene mit dem Image animieren über den angegebenen Pfad")
 
## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, die Funktionen für Animation über die *Core Animation* Frameworks. Untersucht Core Animation, mit sowohl wie sie Animationen in UIKit verwendet und wie es bei Animationssteuerelements auf niedrigerer Ebene direkt verwendet werden kann.

## <a name="related-links"></a>Verwandte Links

- [Core-Animation-Beispiel](https://developer.xamarin.com/samples/monotouch/GraphicsAndAnimation/)
- [Core Graphics](~/ios/platform/graphics-animation-ios/core-graphics.md)
- [Grafiken und Animationen Exemplarische Vorgehensweise](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](https://developer.xamarin.com/recipes/ios/animation/coreanimation)
