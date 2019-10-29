---
title: Anzeigen von Controller Übergängen in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie animierte Übergänge zwischen Ansichts Controllern in xamarin. IOS-Anwendungen angepasst werden.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: f0886ac9d47e7ab08a6f74365bcc25163b803e11
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002396"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Anzeigen von Controller Übergängen in xamarin. IOS

UIKit bietet Unterstützung für das Anpassen des animierten Übergangs, der beim darstellen von Ansichts Controllern auftritt. Diese Unterstützung ist in integrierten Controllern sowie allen benutzerdefinierten Controllern enthalten, die direkt von `UIViewController`erben. Außerdem nutzt `UICollectionViewController` die Anpassung des Controller Übergangs, um die animierten Übergänge in Auflistungs Ansichts Layouts zu nutzen.

## <a name="custom-transitions"></a>Benutzerdefinierte Übergänge

Der animierte Übergang zwischen Ansichts Controllern in ios 7 ist vollständig anpassbar. `UIViewController` enthält jetzt eine `TransitioningDelegate`-Eigenschaft, die eine benutzerdefinierte animatorklasse für das System bereitstellt, wenn ein Übergang stattfindet.

So verwenden Sie einen benutzerdefinierten Übergang mit `PresentViewController`:

1. Legen Sie den `ModalPresentationStyle` auf dem Controller zu `UIModalPresentationStyle.Custom`, der angezeigt werden soll.
2. Implementieren Sie `UIViewControllerTransitioningDelegate`, um eine animatorklasse zu erstellen, bei der es sich um eine Instanz von `UIViewControllerAnimatedTransitioning` handelt.
3. Legen Sie die `TransitioningDelegate`-Eigenschaft auf eine Instanz von `UIViewControllerTransitioningDelegate`, auch auf dem Controller, der angezeigt werden soll.
4. Stellen Sie den Ansichts Controller dar.

Der folgende Code stellt z. b. einen Ansichts Controller vom Typ `ControllerTwo`-a `UIViewController` Unterklasse dar:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Wenn Sie die app ausführen und auf die Schaltfläche tippen, wird die Standard Animation der Ansicht des zweiten Controllers von unten nach unten animiert, wie unten dargestellt:

 ![](transitions-images/no-custom-transition.png "Running the app and tapping the button causes the default animation of the second controllers view to animate in from the bottom")

Das Festlegen der `ModalPresentationStyle` und `TransitioningDelegate` führt jedoch zu einer benutzerdefinierten Animation für den Übergang:

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

Der `TransitioningDelegate` ist für das Erstellen einer Instanz der `UIViewControllerAnimatedTransitioning` Unterklasse mit dem Namen `CustomAnimator` im folgenden Beispiel verantwortlich:

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

Wenn der Übergang stattfindet, erstellt das System eine Instanz von `IUIViewControllerContextTransitioning`, die an die Methoden des Animators übermittelt wurde. `IUIViewControllerContextTransitioning` enthält die `ContainerView`, in der die Animation stattfindet, sowie den Ansichts Controller, der den Übergang initiiert und der Ansichts Controller in den Übergang übergeht.

Die `UIViewControllerAnimatedTransitioning`-Klasse übernimmt die eigentliche Animation. Es müssen zwei Methoden implementiert werden:

1. `TransitionDuration` – gibt die Dauer der Animation in Sekunden zurück.
1. `AnimateTransition` – führt die eigentliche Animation aus.

Die folgende Klasse implementiert beispielsweise `UIViewControllerAnimatedTransitioning`, um den Frame der Ansicht des Controllers zu animieren:

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

Wenn die Schaltfläche nun abgetippt wird, wird die in der `UIViewControllerAnimatedTransitioning`-Klasse implementierte Animation verwendet:

 ![](transitions-images/custom-transition.png "An example of the zoom in effect running")

## <a name="collection-view-transitions"></a>Übergängen der Sammlungsansicht

Sammlungs Ansichten verfügen über integrierte Unterstützung für das Erstellen animierter Übergänge:

- **Navigations Controller** – der animierte Übergang zwischen zwei `UICollectionViewController` Instanzen kann optional automatisch verarbeitet werden, wenn Sie von einem `UINavigationController` verwaltet werden.
- **Übergangs Layout** – eine neue `UICollectionViewTransitionLayout` Klasse ermöglicht den interaktiven Übergang zwischen Layouts.

### <a name="navigation-controller-transitions"></a>Navigations Controller Übergänge

Bei der Verwendung in einem Navigations Controller bietet eine `UICollectionViewController` Unterstützung für animierte Übergänge zwischen Controllern. Diese Unterstützung ist integriert und erfordert nur einige einfache Schritte, die implementiert werden müssen:

1. Legen Sie `UseLayoutToLayoutNavigationTransitions` auf eine `UICollectionViewController` `false`.
1. Fügen Sie dem Stamm des Stapels des Navigations Controllers eine Instanz des `UICollectionViewController` hinzu.
1. Erstellen Sie eine zweite `UICollectionViewController`, und legen Sie die `UseLayoutToLayoutNavigtionTransitions`-Eigenschaft auf `true` fest.
1. Schieben Sie die zweite `UICollectionViewController` auf den Stapel des Navigations Controllers.

Der folgende Code fügt dem Stamm des Stapels eines Navigations Controllers eine `UICollectionViewController` Unterklasse mit dem Namen `ImagesCollectionViewController` hinzu, wobei die Eigenschaft `UseLayoutToLayoutNavigationTransitions` auf `false`festgelegt ist:

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

Wenn ein Element ausgewählt wird, wird eine zweite Instanz des `ImagesController` erstellt, nur dieses Mal mit einer anderen Layoutklasse. Für diesen Controller ist `UseLayoutToLayoutNavigtionTransitions` auf `true`festgelegt, wie unten dargestellt:

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

Die `UseLayoutToLayoutNavigationTransitions`-Eigenschaft muss festgelegt werden, bevor der Controller dem Navigations Stapel hinzugefügt wird. Wenn diese Eigenschaft festgelegt ist, wird der normale horizontale gleitende Übergang durch einen animierten Übergang zwischen den Layouts der beiden Controller ersetzt, wie unten dargestellt:

![](transitions-images/nav2.png "An animated transition between the layouts of the two controllers")

### <a name="transition-layout"></a>Übergangs Layout

Zusätzlich zur Unterstützung des layoutübergangs innerhalb von Navigations Controllern ist nun ein neues Layout namens `UICollectionViewTransitionLayout` verfügbar. Diese Layoutklasse ermöglicht das interaktive Steuern während des layoutübergangs Prozesses, indem das `TransitionProgress` über den Code festgelegt werden kann. `UICollectionViewTransitionLayout` unterscheidet sich von-und nicht als Ersatz für die `SetCollectionViewLayout`-Methode von IOS 6, die zu einem animierten layoutübergang geführt hat. Diese Methode hat keine integrierte Unterstützung zum Steuern des Fortschritts des animierten Übergangs bereitgestellt.

 `UICollectionViewTransitionLayout` ermöglicht beispielsweise die Konfiguration eines Gesten Erkennungs Moduls zum Steuern des Übergangs zwischen Layouts als Reaktion auf Benutzerinteraktion, indem das ursprüngliche Layout und das vorgesehene Layout für den Übergang verwaltet werden.

Die Schritte zum Implementieren eines interaktiven Übergangs innerhalb einer Gestenerkennung mithilfe von `UICollectionViewTransitionLayout` lauten wie folgt:

1. Erstellen Sie eine Gestenerkennung.
1. Nennen Sie die `StartInteractiveTransition`-Methode des `UICollectionView`, und übergeben Sie dabei das Ziel Layout und einen Vervollständigungs Handler.
1. Legen Sie die Eigenschaft `TransitionProgress` der `UICollectionViewTransitionLayout` Instanz fest, die von der `StartInteractiveTransition`-Methode zurückgegeben wird.
1. Das Layout für ungültig erklären.
1. Rufen Sie die `FinishInteractiveTransition`-Methode des `UICollectionView` auf, um den Übergang abzuschließen, oder die `CancelInteractiveTransition`-Methode, um Sie abzubrechen.  `FinishInteractiveTransition` bewirkt, dass die Animation den Übergang zum Ziel Layout abschließt, während `CancelInteractiveTransition` dazu führt, dass die Animation zum ursprünglichen Layout zurückkehrt.
1. Verarbeiten der Übergangs Vervollständigung im Vervollständigungs Handler der `StartInteractiveTransition` Methode.
1. Fügen Sie die Gestenerkennung der Auflistungs Ansicht hinzu.

Der folgende Code implementiert einen interaktiven layoutübergang innerhalb einer Pinch-Gestenerkennung:

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

Wenn der Benutzer die Auflistungs Ansicht pingt, wird der `TransitionProgress` relativ zur Skala der-Pinsel festgelegt. In dieser Implementierung wird der Übergang abgebrochen, wenn der Benutzer die Zeit endet, bevor der Übergang 50% abgeschlossen ist. Andernfalls ist der Übergang abgeschlossen.

## <a name="related-links"></a>Verwandte Links

- [Einführung zu IOS 7 (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
