---
title: View-Controller-Übergänge im Xamarin.iOS
description: Dieses Dokument beschreibt die animierte Übergänge zwischen View-Controller in Xamarin.iOS Anwendungen anpassen.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/14/2017
ms.openlocfilehash: 35795002310cd79a1897061fe6e3e41b48b45b4d
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790447"
---
# <a name="view-controller-transitions-in-xamarinios"></a>View-Controller-Übergänge im Xamarin.iOS

UIKit fügt Unterstützung zum Anpassen der animierten Übergangs, der auftritt, bei der Darstellung von View-Controller hinzu. Diese Unterstützung ist im Lieferumfang von integrierten Controller sowie alle benutzerdefinierten Controller, die direkt von erben `UIViewController`. Darüber hinaus `UICollectionViewController` nutzt Controller Übergang Anpassung animierte Übergänge in der Auflistung anzeigen Layouts zu nutzen.

## <a name="custom-transitions"></a>Benutzerdefinierte Übergänge

Die animierte Übergang zwischen View-Controller in iOS 7 ist vollständig anpassbar. `UIViewController` enthält jetzt eine `TransitioningDelegate` Eigenschaft, die eine benutzerdefinierte Animator-Klasse, um das System bereitstellt, wenn ein Übergang auftritt.

Verwenden Sie einen benutzerdefinierten Übergang mit `PresentViewController`:

1.  Legen Sie die `ModalPresentationStyle` auf `UIModalPresentationStyle.Custom` auf dem Controller dargestellt werden soll.
2.  Implementieren `UIViewControllerTransitioningDelegate` So erstellen Sie eine Klasse Animator also in eine Instanz von `UIViewControllerAnimatedTransitioning` .
3.  Legen Sie die `TransitioningDelegate` Eigenschaft zu einer Instanz von `UIViewControllerTransitioningDelegate` , auch auf dem Controller dargestellt werden soll.
4.  Stellen Sie die View-Controller.


Der folgende Code zeigt z. B. eine Sicht, Typ des Controllers `ControllerTwo` – ein `UIViewController` Unterklasse:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Ausführen der app, und tippen Sie auf die Schaltfläche bewirkt, dass die Standardanimation der zweite Controller-Sicht in der unteren animiert wie unten dargestellt:

 ![](transitions-images/no-custom-transition.png "Führt dazu, dass die Standardanimation der zweite Domänencontroller Sicht zu animierende in von unten nach Ausführen der app, und tippen Sie auf die Schaltfläche")

Festlegen der `ModalPresentationStyle` und `TransitioningDelegate` führt zu einer benutzerdefinierten Animation für den Übergang:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo () {
        ModalPresentationStyle = UIModalPresentationStyle.Custom;
        };

    transitioningDelegate = new TransitioningDelegate ();
    controllerTwo.TransitioningDelegate = transitioningDelegate;

    this.PresentViewController (controllerTwo, true, null);
};
```

Die `TransitioningDelegate` ist verantwortlich für das Erstellen einer Instanz von der `UIViewControllerAnimatedTransitioning` Unterklasse - wird aufgerufen, `CustomAnimator` im folgenden Beispiel:

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

Sobald der Übergang ausgeführt wird, erstellt das System eine Instanz des `IUIViewControllerContextTransitioning`, die sie an der Animator-Methoden übergeben. `IUIViewControllerContextTransitioning` enthält die `ContainerView` , in dem die Animation auftritt, sowie die modellansichtcontroller Initiieren des Übergangs und die View-Controller Übergang ausgeführt wird.

Die `UIViewControllerAnimatedTransitioning` Klasse übernimmt die eigentliche Animation. Zwei Methoden müssen implementiert werden:

1.  `TransitionDuration` – Gibt die Dauer der Animation in Sekunden.
1.  `AnimateTransition` – die tatsächlichen Animation führt.


Z. B. die folgende Klasse implementiert `UIViewControllerAnimatedTransitioning` zu animierende den Rahmen des Controllers anzeigen:

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

Jetzt, wenn die Schaltfläche "" abgerufen werden, die Animation in implementiert die `UIViewControllerAnimatedTransitioning` Klasse wird verwendet:

 ![](transitions-images/custom-transition.png "Ein Beispiel für den Zoomfaktor faktisch ausgeführt")

## <a name="collection-view-transitions"></a>Auflistung Ansicht Übergänge

Auflistungsansichten bieten eine integrierte Unterstützung für das animierte Übergänge erstellen:

-  **Navigation Controller** – die animiert Übergang zwischen zwei `UICollectionViewController` Instanzen können optional behandelt werden automatisch bei einer `UINavigationController` verwaltet sie.
-  **Ein Übergang erfolgt, Layout** – ein neues `UICollectionViewTransitionLayout` Klasse ermöglicht das interaktive Übergang zwischen Layouts.


### <a name="navigation-controller-transitions"></a>Navigation Controller Übergänge

Bei Verwendung innerhalb eines Controllers Navigation eine `UICollectionViewController` bietet Unterstützung für animierte Übergänge zwischen Domänencontrollern. Diese Unterstützung ist eine integrierte Funktion und erfordert nur wenige einfache Schritten implementiert:

1.  Legen Sie `UseLayoutToLayoutNavigationTransitions` auf `false` auf eine `UICollectionViewController` .
1.  Fügen Sie eine Instanz von der `UICollectionViewController` in das Stammverzeichnis des Stapel für die Navigation des Controllers.
1.  Erstellen Sie eine zweite `UICollectionViewController` und legen Sie dessen `UseLayoutToLayoutNavigtionTransitions` Eigenschaft `true` .
1.  Die zweite Push `UICollectionViewController` auf die Navigation des Controllers ab.


Der folgende Code Fügt eine `UICollectionViewController` Unterklasse namens `ImagesCollectionViewController` in das Stammverzeichnis des Stapels eine Navigation Controller, mit der `UseLayoutToLayoutNavigationTransitions` -Eigenschaftensatz auf `false`:

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
            UseLayoutToLayoutNavigationTransitions = false;
        }

    navController = new UINavigationController (viewController);

    window.RootViewController = navController;
    window.MakeKeyAndVisible ();
    
    return true;
}
```

Wenn ein Element ausgewählt ist, eine zweite Instanz von der `ImagesController` erstellt wird, wird nur dieses Mal jedoch mithilfe einer anderen Layout-Klasse. Für diesen Controller `UseLayoutToLayoutNavigtionTransitions` festgelegt ist, um `true`, wie unten dargestellt:

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
        UseLayoutToLayoutNavigationTransitions = true;
        }

    NavigationController.PushViewController (controller2, true);
}
```

Die `UseLayoutToLayoutNavigationTransitions` Eigenschaft muss festgelegt werden, bevor Navigationsstapel Controller hinzugefügt. Diese Eigenschaft festgelegt ist wird der normale horizontale gleitende Übergang durch einen animierten Übergang zwischen Layouts der beiden Controller ersetzt, wie unten gezeigt:

![](transitions-images/nav2.png "Eine animierte Übergang zwischen Layouts der beiden Controller")

### <a name="transition-layout"></a>Transition-Layout

Zusätzlich zum Layout-Übergang-Unterstützung im Navigationsbereich Controller, ein neues Layout aufgerufen `UICollectionViewTransitionLayout` ist jetzt verfügbar. Diese Layoutklasse ermöglicht die interaktive Steuerung während der Übergangsphase Layout können die `TransitionProgress` aus Code festgelegt werden. `UICollectionViewTransitionLayout` unterscheidet sich von - und Ersatz für nicht - die `SetCollectionViewLayout` Methode IOS 6, die einen animierte Layout Übergang verursacht hat. Diese Methode bot keine integrierten Unterstützung für den Status der animierten Übergang steuern.

 `UICollectionViewTransitionLayout` ermöglicht, z. B. eine Geste Erkennung konfiguriert werden, um den Übergang zwischen Layouts in Reaktion auf Benutzerinteraktionen, durch die Verwaltung des ursprünglichen Layouts sowie das beabsichtigte Layout für den Übergang zu steuern.

Die Schritte zur Implementierung der Übergangs eines interaktiven innerhalb einer Geste Erkennung mit `UICollectionViewTransitionLayout` lauten wie folgt:

1.  Erstellen Sie eine Geste-Erkennung.
1.  Rufen Sie die `StartInteractiveTransition` Methode der `UICollectionView` , und übergeben sie das Ziel und eine Abschlusshandler.
1.  Festlegen der `TransitionProgress` Eigenschaft von der `UICollectionViewTransitionLayout` von zurückgegebene Instanz der `StartInteractiveTransition` Methode.
1.  Das Layout ungültig wird.
1.  Aufrufen der `FinishInteractiveTransition` Methode der `UICollectionView` zum Abschließen des Übergangs oder `CancelInteractiveTransition` Methode, um den Vorgang abbrechen.  `FinishInteractiveTransition` bewirkt, dass die Animation zum Abschließen der Übergang in den Ziel-Layout, wohingegen `CancelInteractiveTransition` führt die Animation des ursprünglichen Layouts zurückgeben.
1.  Behandeln Sie den Abschluss der Übergang in den Abschlusshandler der der `StartInteractiveTransition` Methode.
1.  Die Auflistungsansicht das Erkennungsmodul Geste hinzugefügt.


Der folgende Code implementiert einen interaktive Layout Übergang in eine zwei-Finger-Geste-Erkennung:

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

Wie der Benutzer die Auflistungsansicht pinches die `TransitionProgress` relativ zur Skala für das Verkleinern festgelegt ist. In dieser Implementierung wird, wenn der Benutzer das Verkleinern beendet, bevor der Übergang 50 % abgeschlossen ist, wird der Übergang abgebrochen. Andernfalls wird der Übergang abgeschlossen.




## <a name="related-links"></a>Verwandte Links

- [Einführung in die iOS 7 (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
