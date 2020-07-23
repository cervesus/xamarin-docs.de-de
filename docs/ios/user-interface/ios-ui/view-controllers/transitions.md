---
title: Anzeigen von Controller Übergängen in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie animierte Übergänge zwischen Ansichts Controllern in xamarin. IOS-Anwendungen angepasst werden.
ms.prod: xamarin
ms.assetid: CB3AC8E2-8A47-4839-AFA5-AE33047BB26C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 06/14/2017
ms.openlocfilehash: 902e59aa9f5c4aec1dc73f10410132b500932094
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928730"
---
# <a name="view-controller-transitions-in-xamarinios"></a>Anzeigen von Controller Übergängen in xamarin. IOS

UIKit bietet Unterstützung für das Anpassen des animierten Übergangs, der beim darstellen von Ansichts Controllern auftritt. Diese Unterstützung ist in integrierten Controllern sowie allen benutzerdefinierten Controllern enthalten, die direkt von Erben `UIViewController` . Außerdem nutzt `UICollectionViewController` die Anpassung des Controller Übergangs, um die animierten Übergänge in Auflistungs Ansichts Layouts zu nutzen.

## <a name="custom-transitions"></a>Benutzerdefinierte Übergänge

Der animierte Übergang zwischen Ansichts Controllern in ios 7 ist vollständig anpassbar. `UIViewController`enthält jetzt eine `TransitioningDelegate` Eigenschaft, die eine benutzerdefinierte animatorklasse für das System bereitstellt, wenn ein Übergang stattfindet.

So verwenden Sie einen benutzerdefinierten Übergang mit `PresentViewController` :

1. Legen `ModalPresentationStyle` Sie auf dem Controller, der angezeigt werden soll, auf fest `UIModalPresentationStyle.Custom` .
2. Implementieren `UIViewControllerTransitioningDelegate` Sie, um eine animatorklasse zu erstellen, die eine Instanz von ist `UIViewControllerAnimatedTransitioning` .
3. Legen Sie die- `TransitioningDelegate` Eigenschaft auf eine Instanz von fest `UIViewControllerTransitioningDelegate` , auch auf dem Controller, der angezeigt werden soll.
4. Stellen Sie den Ansichts Controller dar.

Der folgende Code stellt z. b. einen Ansichts Controller des Typs `ControllerTwo` -a- `UIViewController` Unterklasse dar:

```csharp
showTwo.TouchUpInside += (object sender, EventArgs e) => {

    controllerTwo = new ControllerTwo ();

    this.PresentViewController (controllerTwo, true, null);
};
```

Wenn Sie die app ausführen und auf die Schaltfläche tippen, wird die Standard Animation der Ansicht des zweiten Controllers von unten nach unten animiert, wie unten dargestellt:

 ![Wenn Sie die app ausführen und auf die Schaltfläche tippen, wird die Standard Animation der zweiten Controller Ansicht von unten nach oben animiert.](transitions-images/no-custom-transition.png)

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

Der `TransitioningDelegate` ist verantwortlich für das Erstellen einer Instanz der `UIViewControllerAnimatedTransitioning` Unterklasse, die `CustomAnimator` im folgenden Beispiel aufgerufen wird:

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

Wenn der Übergang stattfindet, erstellt das System eine Instanz von `IUIViewControllerContextTransitioning` , die an die Methoden des Animators übermittelt wurde. `IUIViewControllerContextTransitioning`enthält die `ContainerView` , in der die Animation stattfindet, sowie den Ansichts Controller, der den Übergang initiiert, und der Ansichts Controller, in den gewechselt wird.

Die- `UIViewControllerAnimatedTransitioning` Klasse übernimmt die eigentliche Animation. Es müssen zwei Methoden implementiert werden:

1. `TransitionDuration`– Gibt die Dauer der Animation in Sekunden zurück.
1. `AnimateTransition`– führt die eigentliche Animation aus.

Beispielsweise wird von der folgenden Klasse implementiert, `UIViewControllerAnimatedTransitioning` um den Frame der Ansicht des Controllers zu animieren:

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

Wenn die Schaltfläche nun abgetippt wird, wird die in der-Klasse implementierte Animation `UIViewControllerAnimatedTransitioning` verwendet:

 ![Ein Beispiel für die Ausführung des Zoom Effekts](transitions-images/custom-transition.png)

## <a name="collection-view-transitions"></a>Übergängen der Sammlungsansicht

Sammlungs Ansichten verfügen über integrierte Unterstützung für das Erstellen animierter Übergänge:

- **Navigations Controller** – der animierte Übergang zwischen zwei- `UICollectionViewController` Instanzen kann optional automatisch behandelt werden, wenn ein von einem `UINavigationController` verwaltet wird.
- **Übergangs Layout** – eine neue `UICollectionViewTransitionLayout` Klasse ermöglicht den interaktiven Übergang zwischen Layouts.

### <a name="navigation-controller-transitions"></a>Navigations Controller Übergänge

Bei der Verwendung in einem Navigations Controller `UICollectionViewController` enthält eine Unterstützung für animierte Übergänge zwischen Controllern. Diese Unterstützung ist integriert und erfordert nur einige einfache Schritte, die implementiert werden müssen:

1. Legen `UseLayoutToLayoutNavigationTransitions` Sie `false` auf fest `UICollectionViewController` .
1. Fügen Sie dem Stamm `UICollectionViewController` des Stapels des Navigations Controllers eine Instanz von hinzu.
1. Erstellen Sie eine Sekunde `UICollectionViewController` , und legen Sie Ihre- `UseLayoutToLayoutNavigtionTransitions` Eigenschaft auf fest `true` .
1. Überträgt die zweite `UICollectionViewController` auf den Stapel des Navigations Controllers.

Der folgende Code fügt dem Stamm `UICollectionViewController` des Stapels eines Navigations Controllers eine Unterklasse mit dem Namen hinzu `ImagesCollectionViewController` , wobei die- `UseLayoutToLayoutNavigationTransitions` Eigenschaft auf festgelegt ist `false` :

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

Wenn ein Element ausgewählt wird, wird eine zweite Instanz von `ImagesController` erstellt, nur dieses Mal mit einer anderen Layoutklasse. Für diesen Controller `UseLayoutToLayoutNavigtionTransitions` ist auf festgelegt `true` , wie unten dargestellt:

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

Die- `UseLayoutToLayoutNavigationTransitions` Eigenschaft muss festgelegt werden, bevor der Controller dem Navigations Stapel hinzugefügt wird. Wenn diese Eigenschaft festgelegt ist, wird der normale horizontale gleitende Übergang durch einen animierten Übergang zwischen den Layouts der beiden Controller ersetzt, wie unten dargestellt:

![Ein animierter Übergang zwischen den Layouts der beiden Controller](transitions-images/nav2.png)

### <a name="transition-layout"></a>Übergangs Layout

Zusätzlich zur Unterstützung des layoutübergangs innerhalb von Navigations Controllern ist nun ein neues Layout mit dem Namen `UICollectionViewTransitionLayout` verfügbar. Diese Layoutklasse ermöglicht das interaktive Steuern während des layoutübergangs Prozesses, indem das `TransitionProgress` Festlegen von aus Code ermöglicht wird. `UICollectionViewTransitionLayout`unterscheidet sich von und nicht von einem Ersatz für die- `SetCollectionViewLayout` Methode von IOS 6, die einen animierten layoutübergang verursacht hat. Diese Methode hat keine integrierte Unterstützung zum Steuern des Fortschritts des animierten Übergangs bereitgestellt.

 `UICollectionViewTransitionLayout`ermöglicht beispielsweise die Konfiguration eines Gesten Erkennungs Moduls, um den Übergang zwischen Layouts als Reaktion auf Benutzerinteraktion zu steuern, indem das ursprüngliche Layout und das vorgesehene Layout für den Übergang verwaltet werden.

Die Schritte zum Implementieren eines interaktiven Übergangs innerhalb einer Gestenerkennung mit `UICollectionViewTransitionLayout` lauten wie folgt:

1. Erstellen Sie eine Gestenerkennung.
1. Greifen `StartInteractiveTransition` Sie auf die-Methode von zu `UICollectionView` , und übergeben Sie das Ziel Layout und einen Vervollständigungs Handler.
1. Legen Sie die- `TransitionProgress` Eigenschaft der `UICollectionViewTransitionLayout` von der-Methode zurückgegebenen-Instanz fest `StartInteractiveTransition` .
1. Das Layout für ungültig erklären.
1. Rufen Sie die- `FinishInteractiveTransition` Methode von `UICollectionView` auf, um den Übergang abzuschließen, oder die- `CancelInteractiveTransition` Methode, um Sie abzubrechen.  `FinishInteractiveTransition`bewirkt, dass die Animation den Übergang zum Ziel Layout abschließt, wohingegen die `CancelInteractiveTransition` Animation zum ursprünglichen Layout zurückkehrt.
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

Wenn der Benutzer die Auflistungs Ansicht pingt, `TransitionProgress` wird der in Relation zur Skala der Pinsel festgelegt. In dieser Implementierung wird der Übergang abgebrochen, wenn der Benutzer die Zeit endet, bevor der Übergang 50% abgeschlossen ist. Andernfalls ist der Übergang abgeschlossen.

## <a name="related-links"></a>Ähnliche Themen

- [Einführung zu IOS 7 (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
