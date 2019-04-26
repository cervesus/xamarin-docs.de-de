---
title: Übergänge von Ansichtscontrollern in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie animierten Übergänge zwischen ansichtscontrollern in Xamarin.iOS-Anwendungen anpassen.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/14/2017
ms.openlocfilehash: 6711d3af06619aa54a2d735cb83862ed64abacec
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61199317"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Übergänge von Ansichtscontrollern in Xamarin.iOS

UIKit fügt die Unterstützung der Anpassung der animierten Übergangs, der tritt auf, wenn der ansichtscontroller darstellen. Diese Unterstützung ist im Lieferumfang von integrierten Controller als auch alle benutzerdefinierten Controller, die direkt von erben `UIViewController`. Darüber hinaus `UICollectionViewController` nutzt die Vorteile der Controller-Übergang-Anpassung animierten Übergänge in den Ansichtslayouts Sammlung nutzen.

## <a name="custom-transitions"></a>Benutzerdefinierte Übergänge

Die animierte Übergang zwischen ansichtscontrollern auf iOS 7 kann vollständig angepasst werden. `UIViewController` enthält jetzt eine `TransitioningDelegate` Eigenschaft, die eine benutzerdefinierte Animator-Klasse, um das System bereitstellt, tritt ein Übergang.

Verwenden Sie einen benutzerdefinierten Übergang mit `PresentViewController`:

1.  Legen Sie die `ModalPresentationStyle` zu `UIModalPresentationStyle.Custom` auf dem Controller dargestellt werden.
2.  Implementieren `UIViewControllerTransitioningDelegate` So erstellen Sie eine Animator-Klasse, die ist eine Instanz der `UIViewControllerAnimatedTransitioning` .
3.  Legen Sie die `TransitioningDelegate` Eigenschaft mit einer Instanz von `UIViewControllerTransitioningDelegate` , auch auf dem Controller dargestellt werden.
4.  Stellen Sie den ansichtscontroller an.


Der folgende Code zeigt z. B. einen View Controller des Typs `ControllerTwo` – eine `UIViewController` Unterklasse:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Die app ausführen, und tippen Sie auf die Schaltfläche führt dazu, dass die Standardanimation des zweiten Controllers-Ansicht, um das Animieren von unten nach, wie unten dargestellt:

 ![](transitions-images/no-custom-transition.png "Führt dazu, dass die Standardanimation der zweite Domänencontroller Sicht zu animierende in von unten nach Ausführen der app, und tippen Sie auf die Schaltfläche")

Festlegen der `ModalPresentationStyle` und `TransitioningDelegate` führt eine benutzerdefinierte Animation für die Umstellung:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

Die `TransitioningDelegate` dient zum Erstellen einer Instanz der `UIViewControllerAnimatedTransitioning` Unterklasse - wird aufgerufen, `CustomAnimator` im folgenden Beispiel:

```csharp
public class TransitioningDelegate : UIViewControllerTransitioningDelegate
{
    CustomTransitionAnimator animator;

    public override IUIViewControllerAnimatedTransitioning GetAnimationControllerForPresentedController (UIViewController presented, UIViewController presenting, UIViewController source)
    {
        animator = new CustomTransitionAnimator ();
        return animator;
    }
}
```

Wenn der Wechsel stattfindet, wird das System erstellt eine Instanz von `IUIViewControllerContextTransitioning`, die sie an der Animator Methoden übergeben. `IUIViewControllerContextTransitioning` enthält die `ContainerView` , in dem die Animation auftritt, sowie den View-Controller, den Übergang zu initiieren und den ansichtscontroller Übergang ausgeführt wird.

Die `UIViewControllerAnimatedTransitioning` Klasse übernimmt die eigentliche Animation. Es müssen zwei Methoden implementiert werden:

1.  `TransitionDuration` – Gibt die Dauer der Animation in Sekunden zurück.
1.  `AnimateTransition` – führt die eigentliche Animation.


Z. B. die folgende Klasse implementiert `UIViewControllerAnimatedTransitioning` den Rahmen des Controllers Ansicht animieren:

```csharp
public class CustomTransitionAnimator : UIViewControllerAnimatedTransitioning
{
    public CustomTransitionAnimator ()
    {
    }

    public override double TransitionDuration (IUIViewControllerContextTransitioning transitionContext)
    {
        return 1.0;
    }

    public override void AnimateTransition (IUIViewControllerContextTransitioning transitionContext)
    {
        var inView = transitionContext.ContainerView;
        var toVC = transitionContext.GetViewControllerForKey (UITransitionContext.ToViewControllerKey);
        var toView = toVC.View;

        inView.AddSubview (toView);

        var frame = toView.Frame;
        toView.Frame = CGRect.Empty;

        UIView.Animate (TransitionDuration (transitionContext), () => {
            toView.Frame = new CGRect (20, 20, frame.Width - 40, frame.Height - 40);
        }, () => {
            transitionContext.CompleteTransition (true);
        });
    }
}
```

Jetzt, wenn die Schaltfläche getippt wird wird, die Animation in implementiert die `UIViewControllerAnimatedTransitioning` Klasse wird verwendet:

 ![](transitions-images/custom-transition.png "Ein Beispiel für den Zoomfaktor in Kraft, die ausgeführt wird")

## <a name="collection-view-transitions"></a>Auflistung Ansicht Übergänge

Auflistungsansichten bieten eine integrierte Unterstützung für das Erstellen von animierten Übergänge:

-  **Navigationscontroller** : der Übergang zwischen zwei animiert `UICollectionViewController` Instanzen können optional behandelt werden, automatisch beim eine `UINavigationController` verwaltet sie.
-  **Übergang von Layout** – ein neues `UICollectionViewTransitionLayout` -Klasse ermöglicht es interaktiven Übergang zwischen Layouts.


### <a name="navigation-controller-transitions"></a>Übergänge von Ansichtscontrollern Navigation

Wenn Sie in einen navigationscontroller verwendet eine `UICollectionViewController` bietet Unterstützung für animierten Übergänge zwischen Domänencontrollern. Diese Unterstützung ist integriert und erfordert nur wenige einfache Schritte zum implementieren:

1.  Legen Sie `UseLayoutToLayoutNavigationTransitions` zu `false` auf eine `UICollectionViewController` .
1.  Fügen Sie eine Instanz von der `UICollectionViewController` in das Stammverzeichnis des navigationscontrollers Stapel.
1.  Erstellen Sie eine zweite `UICollectionViewController` und legen Sie dessen `UseLayoutToLayoutNavigtionTransitions` Eigenschaft `true` .
1.  Übertragen Sie die zweite `UICollectionViewController` Stapel des navigationscontrollers.


Der folgende Code Fügt eine `UICollectionViewController` Unterklasse namens `ImagesCollectionViewController` in das Stammverzeichnis des Stapels einen navigationscontroller, mit der `UseLayoutToLayoutNavigationTransitions` -Eigenschaftensatz auf `false`:

```csharp
UIWindow window;
ImagesCollectionViewController viewController;
UICollectionViewFlowLayout layout;
UINavigationController navController;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    window = new UIWindow (UIScreen.MainScreen.Bounds);

    // create and initialize a UICollectionViewFlowLayout
    layout = new UICollectionViewFlowLayout (){
        SectionInset = new UIEdgeInsets (10,5,10,5),
        MinimumInteritemSpacing = 5,
        MinimumLineSpacing = 5,
        ItemSize = new CGSize (100, 100)
    };

    viewController = new ImagesCollectionViewController (layout) {
            UseLayoutToLayoutNavigationTransitions = false
        };

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

Wenn ein Element ausgewählt ist, eine zweite Instanz von der `ImagesController` erstellt wird, wird nur dieses Mal eine anderes Layout-Klasse. Für diesen Controller `UseLayoutToLayoutNavigtionTransitions` nastaven NA hodnotu `true`, wie unten dargestellt:

```csharp
CircleLayout circleLayout;
ImagesCollectionViewController controller2;

...

public override void ItemSelected (UICollectionView collectionView, NSIndexPath indexPath)
{
    // UseLayoutToLayoutNavigationTransitions when item is selected
    circleLayout = new CircleLayout (Monkeys.Instance.Count){
        ItemSize = new CGSize (100, 100)
    };
            
    controller2 = new ImagesCollectionViewController (circleLayout) {
        UseLayoutToLayoutNavigationTransitions = true
        };

    NavigationController.PushViewController (controller2, true);
}
```

Die `UseLayoutToLayoutNavigationTransitions` Eigenschaft muss vor dem Hinzufügen des Controllers auf den Navigationsstapel festgelegt werden. Diese Eigenschaft festgelegt ist wird der normale horizontale gleitende Übergang mit einer animierten Übergang zwischen den Layouts der beiden Controller, ersetzt, wie unten gezeigt:

![](transitions-images/nav2.png "Eine animierte Übergang zwischen den Layouts der beiden Controller")

### <a name="transition-layout"></a>Transition-Layout

Neben der Layout-Übergang-Unterstützung in navigationscontroller, ein neues Layout namens `UICollectionViewTransitionLayout` ist jetzt verfügbar. Diese Layoutklasse ermöglicht über interaktive Steuerung während der Übergangsphase Layout, indem es ermöglicht die `TransitionProgress` in Code festgelegt werden. `UICollectionViewTransitionLayout` unterscheidet sich von – und kein Ersatz für – die `SetCollectionViewLayout` Methode IOS 6, die einen animierten Layout Übergang verursacht hat. Diese Methode hat keine integrierten Unterstützung bereitgestellt, für den Status des animierten Übergangs steuern.

 `UICollectionViewTransitionLayout` ermöglicht, z. B. eine stiftbewegungs-Erkennung konfiguriert werden, um den Übergang zwischen Layouts als Reaktion auf Benutzerinteraktionen, durch die Verwaltung des ursprünglichen Layouts sowie das beabsichtigte Layout für den Übergang zu steuern.

Die Schritte zum Implementieren eines interaktiven Übergangs in eine stiftbewegungs-Erkennung mit `UICollectionViewTransitionLayout` lauten wie folgt:

1.  Erstellen Sie eine stiftbewegungs-Erkennung.
1.  Rufen Sie die `StartInteractiveTransition` Methode der `UICollectionView` , und übergeben sie das Ziel-Layout und einen Abschlusshandler.
1.  Legen Sie die `TransitionProgress` Eigenschaft der `UICollectionViewTransitionLayout` von zurückgegebene Instanz der `StartInteractiveTransition` Methode.
1.  Das Layout ungültig wird.
1.  Rufen Sie die `FinishInteractiveTransition` Methode der `UICollectionView` zum Abschließen des Übergangs oder `CancelInteractiveTransition` Methode, um den Vorgang abbrechen.  `FinishInteractiveTransition` bewirkt, dass die Animation zum Abschließen der Übergang in die Ziel-Layout, wohingegen `CancelInteractiveTransition` führt die Animation, die an das ursprüngliche Layout zurückgegeben werden.
1.  Behandeln Sie den Abschluss der Übergang in den Abschlusshandler der der `StartInteractiveTransition` Methode.
1.  Fügen Sie der stiftbewegungs-Erkennung zur Auflistungsansicht.


Der folgende Code implementiert einen Übergang interaktive Layout innerhalb einer Pinch-stiftbewegungs-Erkennung:

```csharp
imagesController = new ImagesCollectionViewController (flowLayout);

nfloat sf = 0.4f;
UICollectionViewTransitionLayout trLayout = null;
UICollectionViewLayout nextLayout;

pinch = new UIPinchGestureRecognizer (g => {

    var progress = Math.Abs(1.0f -  g.Scale)/sf;

    if(trLayout == null){
        if(imagesController.CollectionView.CollectionViewLayout is CircleLayout)
            nextLayout = flowLayout;
        else
            nextLayout = circleLayout;

        trLayout = imagesController.CollectionView.StartInteractiveTransition (nextLayout, (completed, finished) => {   
            Console.WriteLine ("transition completed");
            trLayout = null;
        });
    }

    trLayout.TransitionProgress = (nfloat)progress;

    imagesController.CollectionView.CollectionViewLayout.InvalidateLayout ();

    if(g.State == UIGestureRecognizerState.Ended){
        if (trLayout.TransitionProgress > 0.5f)
            imagesController.CollectionView.FinishInteractiveTransition ();
        else
            imagesController.CollectionView.CancelInteractiveTransition ();
    }

});

imagesController.CollectionView.AddGestureRecognizer (pinch);

```

Da der Benutzer die Auflistungsansicht pinches der `TransitionProgress` relativ zur Skala von der Pinch festgelegt ist. Wenn der Benutzer die Pinch beendet, bevor der Übergang auf 50 % abgeschlossen ist, ist, wird in dieser Implementierung wird der Übergang abgebrochen. Andernfalls ist der Übergang abgeschlossen.




## <a name="related-links"></a>Verwandte Links

- [Einführung in iOS 7 (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
