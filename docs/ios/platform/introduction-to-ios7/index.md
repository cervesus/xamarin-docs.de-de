---
title: Einführung in iOS 7
description: In diesem Artikel werden die wichtigsten neuen APIs behandelt, die in ios 7 eingeführt wurden. Hierzu zählen auch das Anzeigen von Controller Übergängen, Erweiterungen von UIView-Animationen, UIKit Dynamics und textkit. Außerdem werden einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen für die mehrstufige Multitasking behandelt.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 067d97e6a36dae6c11f056241c08c21899e96c08
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68649332"
---
# <a name="introduction-to-ios-7"></a>Einführung in iOS 7

_In diesem Artikel werden die wichtigsten neuen APIs behandelt, die in ios 7 eingeführt wurden. Hierzu zählen auch das Anzeigen von Controller Übergängen, Erweiterungen von UIView-Animationen, UIKit Dynamics und textkit. Außerdem werden einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen für die mehrstufige Multitasking behandelt._

IOS 7 ist ein wichtiges Update für IOS. Es wird ein völlig neues Design der Benutzeroberfläche eingeführt, das den Fokus auf den Inhalt und nicht auf die Anwendung Chrome setzt. Neben den visuellen Änderungen bietet IOS 7 eine Vielzahl neuer APIs, um umfangreichere Interaktionen und Erfahrungen zu erzielen. Dieses Dokument behandelt die neuen Technologien, die mit IOS 7 eingeführt wurden, und dient als Ausgangspunkt für weitere Untersuchungen.

## <a name="uiview-animation-enhancements"></a>Erweiterungen für UIView-Animationen

IOS 7 erweitert die Animations Unterstützung in UIKit und ermöglicht Anwendungen das Ausführen von Aufgaben, die zuvor notwendig waren, direkt in das Core Animation Framework zu löschen. Beispielsweise `UIView` kann jetzt Spring-Animationen und Keyframe-Animationen ausführen, die `CAKeyframeAnimation` zuvor auf einen `CALayer`angewendet wurden.

### <a name="spring-animations"></a>Spring-Animationen

 `UIView`unterstützt jetzt das Animieren von Eigenschafts Änderungen mit Spring Effect. Um dies hinzuzufügen, geben Sie `AnimateNotify` entweder `AnimateNotifyAsync` die-oder die-Methode an, und übergeben Sie Werte für das Spring Dämpfung-Verhältnis und die anfängliche Spring Velocity, wie unten beschrieben:

-  `springWithDampingRatio`– Ein Wert zwischen 0 und 1, bei dem die Schwingung für einen kleineren Wert zunimmt.
-  `initialSpringVelocity`– Die anfängliche Spring Velocity als Prozentsatz des gesamten Animations Abstands pro Sekunde.


Der folgende Code erzeugt eine Spring Effect-Struktur, wenn sich der Mittelpunkt der Bildansicht ändert:

```csharp
void AnimateWithSpring ()
{ 
    float springDampingRatio = 0.25f;
    float initialSpringVelocity = 1.0f;
    
    UIView.AnimateNotify (3.0, 0.0, springDampingRatio, initialSpringVelocity, 0, () => {
    
        imageView.Center = new CGPoint (imageView.Center.X, 400);   
            
    }, null);
}
```

Diese Spring Effect bewirkt, dass die Bildansicht beim Vervollständigen der Animation an einem neuen Center-Speicherort springt, wie unten dargestellt:

 ![](images/spring-animation.png "Diese Spring Effect bewirkt, dass die Bildansicht springt, wenn Sie Ihre Animation an einem neuen Center-Speicherort abschließt.")

### <a name="keyframe-animations"></a>Keyframe-Animationen

Die `UIView` -Klasse enthält nun `AnimateWithKeyframes` die-Methode zum Erstellen von Keyframe `UIView`-Animationen für einen. Diese Methode ähnelt anderen `UIView` Animations Methoden, mit dem Unterschied, dass ein zusätzlicher `NSAction` als Parameter übergeben wird, um die Keyframes einzuschließen. Innerhalb von werden Keyframes durch Aufrufen `UIView.AddKeyframeWithRelativeStartTime`von hinzugefügt. `NSAction`

Der folgende Code Ausschnitt erstellt z. b. eine Keyframe-Animation zum Animieren des Mittelpunkts einer Ansicht und zum Drehen der Ansicht:

```csharp
void AnimateViewWithKeyframes ()
{
    var initialTransform = imageView.Transform;
    var initialCeneter = imageView.Center;

    // can now use keyframes directly on UIView without needing to drop directly into Core Animation

    UIView.AnimateKeyframes (2.0, 0, UIViewKeyframeAnimationOptions.Autoreverse, () => {
        UIView.AddKeyframeWithRelativeStartTime (0.0, 0.5, () => { 
            imageView.Center = new CGPoint (200, 200);
        });

        UIView.AddKeyframeWithRelativeStartTime (0.5, 0.5, () => { 
            imageView.Transform = CGAffineTransform.MakeRotation ((float)Math.PI / 2);
        });
    }, (finished) => {
        imageView.Center = initialCeneter;
        imageView.Transform = initialTransform;

        AnimateWithSpring ();
    });
}
```

Die ersten beiden Parameter `AddKeyframeWithRelativeStartTime` der-Methode geben die Startzeit und die Dauer des Keyframes als Prozentsatz der Gesamtlänge der Animation an. Im obigen Beispiel wird die Bildansicht in der ersten Sekunde in der neuen Mitte animiert, gefolgt von der Drehung von 90 Grad in der nächsten Sekunde. Da die Animation als `UIViewKeyframeAnimationOptions.Autoreverse` Option angibt, animieren beide Keyframes ebenfalls in umgekehrter Reihenfolge. Schließlich werden die abschließenden Werte auf den Anfangszustand im Vervollständigungs Handler festgelegt.

Die folgenden Screenshots veranschaulichen die kombinierte Animation durch die Keyframes:

 ![](images/keyframes.png "Diese Screenshots veranschaulichen die kombinierte Animation durch Keyframes.")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics ist ein neuer Satz von APIs in UIKit, mit denen Anwendungen animierte Interaktionen basierend auf der Physik erstellen können. UIKit Dynamics kapselt eine 2D-Physik-Engine, um dies zu ermöglichen.

Die API ist in der Natur deklarativ. Sie deklarieren, wie sich die Physik Interaktionen Verhalten, indem Sie Objekte namens *Verhalten* zum Ausdrücken von Physik-Konzepten wie Schwerkraft, Kollisionen, Quellen usw. erstellen. Dann fügen Sie das Verhalten an ein anderes Objekt an, das als *dynamischer Animator*bezeichnet wird, der eine Ansicht kapselt. Der dynamische Animator übernimmt das Anwenden des deklarierten Physik Verhaltens auf *dynamische Elemente* , die implementieren `IUIDynamicItem`, z. b. `UIView`ein.

Es gibt mehrere verschiedene primitive Verhalten, um komplexe Interaktionen zu initiieren, einschließlich:

-  `UIAttachmentBehavior`– Fügt zwei dynamische Elemente so an, dass Sie aufeinander verschoben werden, oder fügt ein dynamisches Element an einen Anlage Punkt an.
-  `UICollisionBehavior`– Ermöglicht, dass dynamische Elemente an Konflikten beteiligt sind.
-  `UIDynamicItemBehavior`– Gibt einen allgemeinen Satz von Eigenschaften an, die auf dynamische Elemente (z. b. Elastizität, Dichte und Reibung) angewendet werden sollen.
-  `UIGravityBehavior`: Wendet die Schwerkraft auf ein dynamisches Element an und bewirkt, dass Elemente in der Gravitations Richtung beschleunigt werden.
-  `UIPushBehavior`– Wendet Force auf ein dynamisches Element an.
-  `UISnapBehavior`– Ermöglicht einem dynamischen Element das Andocken an einer Position mit einem Spring Effekt.


Obwohl viele primitive vorhanden sind, ist der allgemeine Prozess zum Hinzufügen von Physik-basierten Interaktionen zu einer Ansicht mithilfe von UIKit Dynamics zwischen Verhalten konsistent:

1.  Erstellen Sie einen dynamischen Animator.
1.  Erstellungs Verhalten (en).
1.  Fügen Sie dem dynamischen Animator Verhalten hinzu.


### <a name="dynamics-example"></a>Dynamics-Beispiel

Sehen wir uns ein Beispiel an, in dem eine Schwerkraft und eine Kollisions `UIView`Grenze zu einem hinzugefügt werden.

#### <a name="uigravitybehavior"></a>Uigravitybehavior

Das Hinzufügen von Schweregrad zu einer Bildansicht folgt den drei oben beschriebenen Schritten.

Wir arbeiten in der `ViewDidLoad` -Methode für dieses Beispiel. Fügen Sie zunächst wie `UIImageView` folgt eine-Instanz hinzu:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Dadurch wird eine Bildansicht erstellt, die sich am oberen Rand des Bildschirms befindet. Erstellen Sie eine Instanz von `UIDynamicAnimator`, um das Bild mit der Schwerkraft zu machen:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

Das `UIDynamicAnimator` -Element verwendet eine Instanz eines `UIView` Verweises `UICollectionViewLayout`oder eine, die die Elemente enthält, die pro angefügtem Verhalten animiert werden.

Erstellen Sie als nächstes `UIGravityBehavior` eine-Instanz. Sie können ein oder mehrere Objekte übergeben, die `IUIDynamicItem`implementieren, `UIView`wie z. b.:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Das Verhalten wird an ein Array von `IUIDynamicItem`weitergeleitet, das in diesem Fall die `UIImageView` einzelne Instanz enthält, die wir animieren.

Fügen Sie schließlich dem dynamischen Animator das Verhalten hinzu:

```csharp
dynAnimator.AddBehavior (gravity);
```

Dies führt dazu, dass das Bild nach unten mit der Schwerkraft animiert wird, wie unten dargestellt:

![](images/gravity2.png "Der") 
Speicherort des Start Images(images/gravity3.png "") ![]

Da die Begrenzungen des Bildschirms nicht eingeschränkt sind, wird die Bildansicht einfach vom unteren Rand entfernt. Um die Ansicht so einzuschränken, dass das Bild mit den Rändern des Bildschirms kollidiert, können wir eine `UICollisionBehavior`hinzufügen. Dies wird im nächsten Abschnitt behandelt.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Wir beginnen mit der Erstellung eines `UICollisionBehavior` und fügen es dem dynamischen Animator hinzu, wie dies auch bei der der `UIGravityBehavior`Fall war.

Ändern Sie den Code so, `UICollisionBehavior`dass er folgendes einschließt:

```csharp
using (image = UIImage.FromFile ("monkeys.jpg")) {

    imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
        Image =  image
    };

    View.AddSubview (imageView);

    // 1. create the dynamic animator
    dynAnimator = new UIDynamicAnimator (this.View);

    // 2. create behavior(s)
    var gravity = new UIGravityBehavior (imageView);
    var collision = new UICollisionBehavior (imageView) {
        TranslatesReferenceBoundsIntoBoundary = true
    };

    // 3. add behaviors(s) to the dynamic animator
    dynAnimator.AddBehaviors (gravity, collision);
}
```

Der `UICollisionBehavior` verfügt über eine- `TranslatesReferenceBoundsIntoBoundry`Eigenschaft mit dem Namen. Wenn diese Einstellung `true` auf festgelegt wird, werden die Begrenzungen der Verweis Ansicht als Kollisions Grenze verwendet.

Wenn das Bild nun nach unten mit der Schwerkraft animiert wird, springt es leicht vom unteren Bildschirmrand, bevor es sich an der restlichen Seite befindet.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Wir können das Verhalten der abstürzenden Bildansicht mit zusätzlichen Verhaltensweisen besser steuern. Beispielsweise könnten wir eine `UIDynamicItemBehavior` hinzufügen, um die Elastizität zu erhöhen, sodass die Bildansicht beim Konflikt mit dem unteren Rand des Bildschirms stärker springt.

Beim hinzu `UIDynamicItemBehavior` fügen eines werden die gleichen Schritte wie bei den anderen Verhaltensweisen befolgt. Erstellen Sie zuerst das Verhalten:

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Fügen Sie dann dem dynamischen Animator das Verhalten hinzu:

 `dynAnimator.AddBehavior (dynBehavior);`

Wenn dieses Verhalten vorhanden ist, springt die Bildansicht mehr, wenn Sie mit der Grenze kollidiert.

## <a name="general-user-interface-changes"></a>Allgemeine Änderungen an der Benutzeroberfläche

Zusätzlich zu den neuen UIKit-APIs wie UIKit Dynamics, Controller Übergänge und erweiterten UIView-Animationen, die oben beschrieben wurden, führt IOS 7 eine Reihe von visuellen Änderungen an der Benutzeroberfläche und verwandte API-Änderungen für verschiedene Sichten und Steuerelemente ein. Weitere Informationen finden Sie in der [Übersicht über die IOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Textkit

Das textkit ist eine neue API, die leistungsstarke Layout-und Renderingfeatures von Text bietet. Es baut auf dem Low-Level-Kern Text Framework auf, ist aber viel einfacher zu verwenden als Kerntext.

Weitere Informationen finden Sie in unserem [textkit](~/ios/platform/textkit.md) .

## <a name="multitasking"></a>Multitasking

IOS 7 ändert sich, wann und wie Hintergrundarbeit durchgeführt wird. Durch den Abschluss der Aufgabe in ios 7 werden Anwendungen nicht mehr wach, wenn Aufgaben im Hintergrund ausgeführt werden, und Anwendungen werden für die Hintergrundverarbeitung in einer nicht zusammenhängenden Weise aktiviert. IOS 7 fügt auch drei neue APIs zum Aktualisieren von Anwendungen mit neuen Inhalten im Hintergrund hinzu:

-  Hintergrund Abruf – ermöglicht Anwendungen, Inhalte im Hintergrund in regelmäßigen Abständen zu aktualisieren.
-  Remote Benachrichtigungen: Hiermit können Anwendungen Inhalte aktualisieren, wenn Sie eine Pushbenachrichtigung erhalten. Die Benachrichtigungen können entweder unbeaufsichtigt sein oder ein Banner auf dem Sperrbildschirm anzeigen.
-  Background Transfer Service – ermöglicht das Hochladen und Herunterladen von Daten, z. b. große Dateien, ohne festes Zeit Limit.


Weitere Informationen zu den neuen Multitasking-Funktionen finden Sie in den IOS-Abschnitten des xamarin- [backerden-Handbuchs](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden verschiedene wichtige neue Ergänzungen zu IOS behandelt. Zuerst wird gezeigt, wie Sie benutzerdefinierte Übergänge zu Ansichts Controllern hinzufügen. Anschließend wird gezeigt, wie Übergänge in Auflistungs Ansichten sowohl innerhalb eines Navigations Controllers als auch interaktiv zwischen Auflistungs Ansichten verwendet werden. Im nächsten Schritt werden mehrere Verbesserungen an den UIView-Animationen eingeführt, die zeigen, wie Anwendungen UIKit für Dinge verwenden, die zuvor die Programmierung direkt mit der Kern Animation erforderte. Schließlich wird die neue UIKit Dynamics-API, die eine Physik-Engine für UIKit bereitstellt, zusammen mit der Rich-Text-Unterstützung eingeführt, die jetzt im textkit-Framework verfügbar ist.

## <a name="related-links"></a>Verwandte Links

- [Einführung zu IOS 7 (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
