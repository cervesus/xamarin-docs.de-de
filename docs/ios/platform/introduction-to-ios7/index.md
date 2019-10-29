---
title: Einführung in iOS 7
description: In diesem Artikel werden die wichtigsten neuen APIs behandelt, die in ios 7 eingeführt wurden. Hierzu zählen auch das Anzeigen von Controller Übergängen, Erweiterungen von UIView-Animationen, UIKit Dynamics und textkit. Außerdem werden einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen für die mehrstufige Multitasking behandelt.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: b405643096699e1d965f485bdc590afa178881d6
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031822"
---
# <a name="introduction-to-ios-7"></a>Einführung in iOS 7

_diesem Artikel werden die wichtigsten neuen APIs behandelt, die in ios 7 eingeführt wurden, einschließlich der Ansicht von Controller Übergängen, Erweiterungen von UIView-Animationen, UIKit Dynamics und textkit. Außerdem werden einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen für die mehrstufige Multitasking behandelt._

IOS 7 ist ein wichtiges Update für IOS. Es wird ein völlig neues Design der Benutzeroberfläche eingeführt, das den Fokus auf den Inhalt und nicht auf die Anwendung Chrome setzt. Neben den visuellen Änderungen bietet IOS 7 eine Vielzahl neuer APIs, um umfangreichere Interaktionen und Erfahrungen zu erzielen. Dieses Dokument behandelt die neuen Technologien, die mit IOS 7 eingeführt wurden, und dient als Ausgangspunkt für weitere Untersuchungen.

## <a name="uiview-animation-enhancements"></a>Erweiterungen für UIView-Animationen

IOS 7 erweitert die Animations Unterstützung in UIKit und ermöglicht Anwendungen das Ausführen von Aufgaben, die zuvor notwendig waren, direkt in das Core Animation Framework zu löschen. Beispielsweise können `UIView` jetzt Spring-Animationen und Keyframe-Animationen ausführen, die zuvor eine `CAKeyframeAnimation` auf eine `CALayer`angewendet haben.

### <a name="spring-animations"></a>Spring-Animationen

 `UIView` unterstützt jetzt das Animieren von Eigenschafts Änderungen mit Spring Effect. Um dies hinzuzufügen, müssen Sie entweder die `AnimateNotify`-oder `AnimateNotifyAsync`-Methode abrufen und Werte für das Spring Dämpfung-Verhältnis und die anfängliche Spring Velocity übergeben, wie unten beschrieben:

- `springWithDampingRatio` – ein Wert zwischen 0 und 1, bei dem die Schwingung für einen kleineren Wert zunimmt.
- `initialSpringVelocity` – die anfängliche Spring Velocity als Prozentsatz des gesamten Animations Abstands pro Sekunde.

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

 ![](images/spring-animation.png "This spring effect causes the image view to appear to bounce as it completes its animation to a new center location")

### <a name="keyframe-animations"></a>Keyframe-Animationen

Die `UIView`-Klasse enthält nun die `AnimateWithKeyframes`-Methode zum Erstellen von Keyframe-Animationen auf einem `UIView`. Diese Methode ähnelt anderen `UIView` Animations Methoden, mit dem Unterschied, dass ein zusätzlicher `NSAction` als Parameter übergeben wird, um die Keyframes einzuschließen. Im `NSAction`werden Keyframes durch Aufrufen von `UIView.AddKeyframeWithRelativeStartTime`hinzugefügt.

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

Die ersten beiden Parameter für die `AddKeyframeWithRelativeStartTime`-Methode geben die Startzeit und die Dauer des Keyframes als Prozentsatz der Gesamtlänge der Animation an. Im obigen Beispiel wird die Bildansicht in der ersten Sekunde in der neuen Mitte animiert, gefolgt von der Drehung von 90 Grad in der nächsten Sekunde. Da in der Animation `UIViewKeyframeAnimationOptions.Autoreverse` als Option angegeben wird, animieren beide Keyframes ebenfalls in umgekehrter Reihenfolge. Schließlich werden die abschließenden Werte auf den Anfangszustand im Vervollständigungs Handler festgelegt.

Die folgenden Screenshots veranschaulichen die kombinierte Animation durch die Keyframes:

 ![](images/keyframes.png "This screenshots illustrates the combined animation through the keyframes")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics ist ein neuer Satz von APIs in UIKit, mit denen Anwendungen animierte Interaktionen basierend auf der Physik erstellen können. UIKit Dynamics kapselt eine 2D-Physik-Engine, um dies zu ermöglichen.

Die API ist in der Natur deklarativ. Sie deklarieren, wie sich die Physik Interaktionen Verhalten, indem Sie Objekte namens *Verhalten* zum Ausdrücken von Physik-Konzepten wie Schwerkraft, Kollisionen, Quellen usw. erstellen. Dann fügen Sie das Verhalten an ein anderes Objekt an, das als *dynamischer Animator*bezeichnet wird, der eine Ansicht kapselt. Der dynamische Animator übernimmt das Anwenden des deklarierten Physik Verhaltens auf *dynamische Elemente* -Elemente, die `IUIDynamicItem`implementieren, z. b. ein `UIView`.

Es gibt mehrere verschiedene primitive Verhalten, um komplexe Interaktionen zu initiieren, einschließlich:

- `UIAttachmentBehavior` – fügt zwei dynamische Elemente so an, dass Sie aufeinander verschoben werden, oder fügt ein dynamisches Element an einen Anlage Punkt an.
- `UICollisionBehavior` – ermöglicht, dass dynamische Elemente an Kollisionen teilnehmen.
- `UIDynamicItemBehavior` – gibt einen allgemeinen Satz von Eigenschaften an, die auf dynamische Elemente (z. b. Elastizität, Dichte und Reibung) angewendet werden sollen.
- `UIGravityBehavior`-die Schwerkraft auf ein dynamisches Element anwendet, wodurch Elemente in der Gravitations Richtung beschleunigt werden.
- `UIPushBehavior` – wendet Force auf ein dynamisches Element an.
- `UISnapBehavior` – ermöglicht einem dynamischen Element das Andocken an einer Position mit einem Spring Effekt.

Obwohl viele primitive vorhanden sind, ist der allgemeine Prozess zum Hinzufügen von Physik-basierten Interaktionen zu einer Ansicht mithilfe von UIKit Dynamics zwischen Verhalten konsistent:

1. Erstellen Sie einen dynamischen Animator.
1. Erstellungs Verhalten (en).
1. Fügen Sie dem dynamischen Animator Verhalten hinzu.

### <a name="dynamics-example"></a>Dynamics-Beispiel

Sehen wir uns ein Beispiel an, das einer `UIView`Schweregrad und eine Kollisions Grenze hinzufügt.

#### <a name="uigravitybehavior"></a>Uigravitybehavior

Das Hinzufügen von Schweregrad zu einer Bildansicht folgt den drei oben beschriebenen Schritten.

Wir arbeiten in der `ViewDidLoad`-Methode für dieses Beispiel. Fügen Sie zunächst wie folgt eine `UIImageView`-Instanz hinzu:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Dadurch wird eine Bildansicht erstellt, die sich am oberen Rand des Bildschirms befindet. Erstellen Sie eine Instanz einer `UIDynamicAnimator`, um das Bild mit der Schwerkraft zu machen:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

Der `UIDynamicAnimator` nimmt eine Instanz eines Verweises `UIView` oder eine `UICollectionViewLayout`, die die Elemente enthält, die gemäß angehängtem Verhalten animiert werden.

Erstellen Sie als nächstes eine `UIGravityBehavior` Instanz. Sie können ein oder mehrere Objekte, die die `IUIDynamicItem`implementieren, wie eine `UIView`übergeben:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Das Verhalten wird an ein Array von `IUIDynamicItem`weitergeleitet, das in diesem Fall die einzige `UIImageView` Instanz enthält, die wir animieren.

Fügen Sie schließlich dem dynamischen Animator das Verhalten hinzu:

```csharp
dynAnimator.AddBehavior (gravity);
```

Dies führt dazu, dass das Bild nach unten mit der Schwerkraft animiert wird, wie unten dargestellt:

![](images/gravity2.png "The starting image location")
![](images/gravity3.png "The ending image location")

Da die Begrenzungen des Bildschirms nicht eingeschränkt sind, wird die Bildansicht einfach vom unteren Rand entfernt. Um die Ansicht so einzuschränken, dass das Bild mit den Rändern des Bildschirms kollidiert, können wir eine `UICollisionBehavior`hinzufügen. Dies wird im nächsten Abschnitt behandelt.

#### <a name="uicollisionbehavior"></a>Uicollisionbehavior

Zunächst erstellen wir eine `UICollisionBehavior` und fügen Sie dem dynamischen Animator hinzu, wie dies für die `UIGravityBehavior`der Fall war.

Ändern Sie den Code, um den `UICollisionBehavior`einzubeziehen:

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

Der `UICollisionBehavior` verfügt über eine Eigenschaft mit dem Namen `TranslatesReferenceBoundsIntoBoundry`. Wenn diese Einstellung auf `true` festgelegt wird, werden die Begrenzungen der Verweis Ansicht als Kollisions Grenze verwendet.

Wenn das Bild nun nach unten mit der Schwerkraft animiert wird, springt es leicht vom unteren Bildschirmrand, bevor es sich an der restlichen Seite befindet.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>Uidynamicitembehavior

Wir können das Verhalten der abstürzenden Bildansicht mit zusätzlichen Verhaltensweisen besser steuern. Beispielsweise könnten wir eine `UIDynamicItemBehavior` hinzufügen, um die Elastizität zu erhöhen, sodass die Bildansicht mehr springt, wenn Sie mit dem unteren Bildschirmrand kollidiert.

Beim Hinzufügen eines `UIDynamicItemBehavior` werden die gleichen Schritte wie bei den anderen Verhalten ausgeführt. Erstellen Sie zuerst das Verhalten:

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

- Hintergrund Abruf – ermöglicht Anwendungen, Inhalte im Hintergrund in regelmäßigen Abständen zu aktualisieren.
- Remote Benachrichtigungen: Hiermit können Anwendungen Inhalte aktualisieren, wenn Sie eine Pushbenachrichtigung erhalten. Die Benachrichtigungen können entweder unbeaufsichtigt sein oder ein Banner auf dem Sperrbildschirm anzeigen.
- Background Transfer Service – ermöglicht das Hochladen und Herunterladen von Daten, z. b. große Dateien, ohne festes Zeit Limit.

Weitere Informationen zu den neuen Multitasking-Funktionen finden Sie in den IOS-Abschnitten des xamarin- [backerden-Handbuchs](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Zusammenfassung

In diesem Artikel werden verschiedene wichtige neue Ergänzungen zu IOS behandelt. Zuerst wird gezeigt, wie Sie benutzerdefinierte Übergänge zu Ansichts Controllern hinzufügen. Anschließend wird gezeigt, wie Übergänge in Auflistungs Ansichten sowohl innerhalb eines Navigations Controllers als auch interaktiv zwischen Auflistungs Ansichten verwendet werden. Im nächsten Schritt werden mehrere Verbesserungen an den UIView-Animationen eingeführt, die zeigen, wie Anwendungen UIKit für Dinge verwenden, die zuvor die Programmierung direkt mit der Kern Animation erforderte. Schließlich wird die neue UIKit Dynamics-API, die eine Physik-Engine für UIKit bereitstellt, zusammen mit der Rich-Text-Unterstützung eingeführt, die jetzt im textkit-Framework verfügbar ist.

## <a name="related-links"></a>Verwandte Links

- [Einführung zu IOS 7 (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
