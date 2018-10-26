---
title: Einführung in iOS 7
description: Dieser Artikel behandelt die wichtigsten neuen APIs in iOS 7, einschließlich der Ansichtscontroller Übergänge, Verbesserungen an UIView-Animationen, UIKit Dynamics und Text-Kit. Es werden auch einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen der erweitertes Multitasking behandelt.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: db2ce779962947e2121ff03280544a080e193e2e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50118044"
---
# <a name="introduction-to-ios-7"></a>Einführung in iOS 7

_Dieser Artikel behandelt die wichtigsten neuen APIs in iOS 7, einschließlich der Ansichtscontroller Übergänge, Verbesserungen an UIView-Animationen, UIKit Dynamics und Text-Kit. Es werden auch einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen der erweitertes Multitasking behandelt._

iOS 7 ist ein wichtiges Update für iOS. Er führt einen völlig neue Entwurf der Benutzeroberfläche, mit dem Fokus auf den Inhalt und nicht als Anwendung Chrome versetzt. Zusammen mit die Visualisierung ändert fügt die iOS 7 eine Fülle von neuen APIs, um umfangreichere Interaktionen und Erfahrungen zu erstellen. Dieses Dokument "Surveys" die neuen Technologien mit iOS 7 eingeführt und dient als Ausgangspunkt für die weitere Untersuchung.

## <a name="uiview-animation-enhancements"></a>UIView-Animation-Verbesserungen

iOS 7 erweitert die Unterstützung für Animationen in UIKit, ermöglicht es Anwendungen, die Aktionen, die zuvor gelöscht, die direkt in das Framework Core Animation erforderlich. Z. B. `UIView` kann nun ausführen,-Animationen als auch auf Keyframe-Animationen, dem zuvor ein `CAKeyframeAnimation` angewendet werden, um eine `CALayer`.

### <a name="spring-animations"></a>-Animationen

 `UIView` unterstützt jetzt die Animation von eigenschaftenänderungen mit einem Spring-Effekt. Um diese hinzuzufügen, rufen Sie entweder die `AnimateNotify` oder `AnimateNotifyAsync` -Methode, übergeben Sie Werte für das diese Spring-Verhältnis und die anfängliche Spring-Geschwindigkeit, wie im folgenden beschrieben:

-  `springWithDampingRatio` – Ein Wert zwischen 0 und 1, in denen die Oszillation für kleinere Wert erhöht.
-  `initialSpringVelocity` – Die ersten Spring Geschwindigkeit als Prozentsatz der gesamten Animation Entfernung pro Sekunde.


Der folgende Code erzeugt einen Spring-Effekt, wenn sich die Image-Kartenansicht-Mittelpunkt ändert:

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

Dieser Effekt Spring bewirkt, dass der Bildansicht zu hüpfen führt die Animation an eine neue Mittelpunktposition, wie unten dargestellt angezeigt:

 ![](images/spring-animation.png "Dieser Effekt Spring bewirkt, dass der Bildansicht zu springen, sobald sie die Animation auf eine neue Position der Mitte abgeschlossen scheint")

### <a name="keyframe-animations"></a>Keyframe-Animationen

Die `UIView` -Klasse enthält nun die `AnimateWithKeyframes` Methode zum Erstellen von Keyframe-Animationen auf eine `UIView`. Diese Methode ist ähnlich wie andere `UIView` Animation-Methoden, mit Ausnahme, die eine zusätzliche `NSAction` übergeben wird, als Parameter an die Keyframes enthalten. In der `NSAction`, Keyframes hinzugefügt werden, indem `UIView.AddKeyframeWithRelativeStartTime`.

Der folgende Codeausschnitt erstellt z. B. eine Keyframe-Animation aus, um eine Ansicht des Center sowie zu animieren, um die Ansicht zu drehen:

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

Die ersten beiden Parameter, um die `AddKeyframeWithRelativeStartTime` Methode geben Sie die Startzeit und Dauer des Keyframes, jeweils als Prozentsatz der gesamten Animationslänge. Im Beispiel oben führt die Images Ansicht animieren um seinen neuen Mittelpunkt über der ersten Sekunde gefolgt werden, indem Sie um 90 Grad drehen, auf der nächsten Sekunde. Da die Animation gibt `UIViewKeyframeAnimationOptions.Autoreverse` als Option, beide Keyframes animieren, in umgekehrter Reihenfolge auch. Schließlich werden die endgültigen Werte auf den ursprünglichen Zustand in den Abschlusshandler festgelegt.

Die folgenden Screenshots veranschaulicht die kombinierte Animation mit Keyframes:

 ![](images/keyframes.png "Diese Screenshots veranschaulicht die kombinierte Animation mit keyframes")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics ist es sich um einen neuen Satz von APIs in UIKit, mit denen Anwendungen, um animierte Aktivitäten basierend auf physikalische Eigenschaften zu erstellen. UIKit Dynamics kapselt eine 2D-Physik-Engine, um dies zu ermöglichen.

Die API ist die deklarative Natur. Sie deklarieren die Physik-Interaktionen wie Verhalten, durch das Erstellen von Objekten - wird aufgerufen, *Verhaltensweisen* – express Physik Konzepte wie z. B. Schwerkraft, Kollisionen, Springs. Sie anhängen, die ein eigenes Verhalten auf auf einem anderen Objekt, mit dem Namen einer *dynamische Animator*, die kapselt einer Ansicht. Die dynamische Animator akzeptiert, stellt der Anwendung der deklarierten physikalische Verhalten zu *dynamische Elemente* -Elemente, die implementieren `IUIDynamicItem`, z. B. eine `UIView`.

Es gibt mehrere unterschiedliche primitiven Verhalten komplexe Interaktionen empfohlen, einschließlich trigger zur Verfügung:

-  `UIAttachmentBehavior` – Fügt zwei dynamische Elemente so, dass sie zusammen verschoben, oder fügt eine dynamische Element zu einem Zeitpunkt für die Anlage.
-  `UICollisionBehavior` – Ermöglicht die dynamische Elemente zur Teilnahme an Konflikte.
-  `UIDynamicItemBehavior` – Gibt einen allgemeinen Satz von Eigenschaften, die dynamischen Elemente, z. B. Elastizität, Dichte und Reibung anwenden.
-  `UIGravityBehavior` -Ein dynamischen Element, sodass Elemente in der Schwerpunkte Richtung beschleunigen Schwerkraft gilt.
-  `UIPushBehavior` – Ein dynamisches Element erzwingen gilt.
-  `UISnapBehavior` – Ermöglicht ein dynamisches Element docken Sie an eine Position mit einem Spring-Effekt.


Es gibt, zwar viele primitive entspricht das allgemeine Verfahren zum Hinzufügen einer Ansicht mithilfe von UIKit Dynamics Physik-basierte Interaktionen für Verhalten:

1.  Erstellen Sie eine dynamische Animator.
1.  Erstellen Sie ein eigenes Verhalten auf.
1.  Die dynamische Animator Verhaltensweisen hinzufügen.


### <a name="dynamics-example"></a>Dynamics-Beispiel

Sehen wir uns ein Beispiel, das Schwerkraft- und eine Kollision Grenze an, fügt einen `UIView`.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

Schwerkraft einer Bildansicht hinzufügen, folgt die 3 oben beschriebenen Schritte.

Wir arbeiten der `ViewDidLoad` Methode für dieses Beispiel. Fügen Sie zunächst eine `UIImageView` Instanz wie folgt:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Dadurch wird eine Image-Ansicht zentriert am oberen Rand des Bildschirms erstellt. Damit das Image "Fall" mit der Schwerkraft, erstellen Sie eine Instanz von einem `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

Die `UIDynamicAnimator` akzeptiert eine Instanz eines Verweises `UIView` oder `UICollectionViewLayout`, enthält die Elemente, die pro die angefügten ein eigenes Verhalten auf animiert werden soll.

Als Nächstes erstellen Sie eine `UIGravityBehavior` Instanz. Sie können ein oder mehrere Objekte, die Implementierung übergeben die `IUIDynamicItem`, z. B. eine `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Das Verhalten wird ein Array von übergeben `IUIDynamicItem`, die in diesem Fall enthält des einzelnen `UIImageView` Instanz, die wir sind animieren.

Schließlich wird das Verhalten der dynamischen Animator hinzufügen:

```csharp
dynAnimator.AddBehavior (gravity);
```

Dadurch wird das Bild nach unten das Animieren mit Schwerpunkt, wie unten dargestellt:

![](images/gravity2.png "Die Anfangsposition des Image") 
![](images/gravity3.png "die Endposition des Image")

Da es keine Einschränkung der Grenzen des Bildschirms, fällt der Bildansicht einfach unten aus dem. Um die Ansicht zu beschränken, damit das Abbild mit den Rändern des Bildschirms kollidiert, fügen wir eine `UICollisionBehavior`. Wir werden dies im nächsten Abschnitt behandelt.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Wir erstellen zunächst ein `UICollisionBehavior` und das Hinzufügen zu den dynamischen Animator, wie wir für die `UIGravityBehavior`.

Ändern Sie den Code zum Einschließen der `UICollisionBehavior`:

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

Die `UICollisionBehavior` verfügt über eine Eigenschaft namens `TranslatesReferenceBoundsIntoBoundry`. Wenn dies auf `true` bewirkt, dass der Verweis der Ansicht Grenzen als eine Kollision Grenze verwendet werden soll.

Nun, wenn das Bild nach unten mit der Schwerkraft animiert, springt es etwas vom unteren Rand des Bildschirms, ehe Sie zur rest-vorhanden.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Wir können das Verhalten der fallender Bildansicht mit zusätzlichen Verhaltensweisen steuern. Wir könnten z. B. Hinzufügen einer `UIDynamicItemBehavior` erhöhen die Flexibilität, die Image-Ansicht, um weitere springt, wenn sie am unteren Rand des Bildschirms verursacht einen Konflikt verursachen.

Hinzufügen einer `UIDynamicItemBehavior` folgt den gleichen Schritten wie bei anderen Verhalten. Erstellen Sie zuerst das Verhalten aus:

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Klicken Sie dann das Verhalten der dynamischen Animator hinzufügen:

 `dynAnimator.AddBehavior (dynBehavior);`

Mit diesem Verhalten vorhanden ist springt der Bildansicht mehr, wenn sie mit der Grenze zusammenstößt.

## <a name="general-user-interface-changes"></a>Benutzeroberfläche für allgemeine Änderungen

Zusätzlich zu den neuen UIKit-APIs wie UIKit Dynamics, Übergänge von Ansichtscontrollern und verbesserte UIView-Animationen, die oben beschriebenen führt die iOS 7 eine Vielzahl von visuellen Änderungen an der Benutzeroberfläche und die zugehörigen API-Änderungen für die verschiedenen Ansichten und Steuerelementen. Weitere Informationen finden Sie unter den [iOS 7 Übersicht über die Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Text-Kit

Text-Kit ist eine neue API, die leistungsstarke Text Layout- und Renderingaufgaben Features bietet. Es basiert auf niedriger Ebene Text des Core-Framework und ist viel einfacher zu verwenden als Core Text.

Weitere Informationen finden Sie unserem [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>Multitasking

iOS 7 ändert, wann und wie die Verarbeitung im Hintergrund ausgeführt wird. Abschluss der Aufgabe in iOS 7 nicht mehr werden Anwendungen aktiv beibehalten, wenn die Aufgaben im Hintergrund ausgeführt werden, und Anwendungen für die hintergrundverarbeitung, die in einem nicht zusammenhängenden Bereich aktiviert werden. iOS 7 fügt außerdem drei neue APIs zum Aktualisieren von Anwendungen mit neuem Inhalt im Hintergrund hinzu:

-  Abrufen im Hintergrund – ermöglicht es Anwendungen, um Inhalt im Hintergrund in regelmäßigen Abständen zu aktualisieren.
-  Remotebenachrichtigungen - ermöglicht es Anwendungen, um Inhalt zu aktualisieren, wenn Sie eine Pushbenachrichtigung empfangen. Die Benachrichtigungen können es sich entweder automatische oder ein Banner auf dem Sperrbildschirm anzeigen.
-  Hintergrundübertragungsdienst – ermöglicht das Hochladen und Herunterladen von Daten, z. B. große Dateien, ohne eines festen Zeitraums.


Weitere Informationen zu den neuen Multitasking-Funktionen finden Sie unter den iOS-Abschnitten der Xamarin [Hintergrundverarbeitung Handbuch](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt einige wichtige neue iOS-Erweiterungen. Zuerst wird gezeigt, wie View Controller benutzerdefinierte Übergänge hinzugefügt werden. Anschließend wird gezeigt, wie Übergänge in der Auflistung, sowohl aus, die Sie in einen navigationscontroller sowie interaktiv zwischen Auflistungsansichten verwendet werden kann. Als Nächstes wird es verschiedene Verbesserungen UIView-Animationen, die zeigt, wie Anwendungen UIKit für Dinge verwenden, die zuvor direkten Programmierens mit Core Animation erforderlich. Schließlich wird die neue UIKit Dynamics-API, die eine Physik-Engine in UIKit zu einführt, zusammen mit der rich-Text-Unterstützung, die jetzt in das Text-Kit-Framework verfügbar eingeführt.

## <a name="related-links"></a>Verwandte Links

- [Einführung in iOS 7 (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
