---
title: Einführung in die iOS 7
description: Dieser Artikel behandelt die wichtigsten neuen APIs in iOS 7, einschließlich View Controller-Übergänge, Verbesserungen an UIView Animationen, UIKit Dynamics und Text-Kit eingeführt. Es werden auch einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen der standardpunktdiagramm Multitasking behandelt.
ms.prod: xamarin
ms.assetid: 2C33018F-D64A-4BAA-A34E-082EF311D162
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 9ae82eba78f099f675d21bf53a250923630a0ff6
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-ios-7"></a>Einführung in die iOS 7

_Dieser Artikel behandelt die wichtigsten neuen APIs in iOS 7, einschließlich View Controller-Übergänge, Verbesserungen an UIView Animationen, UIKit Dynamics und Text-Kit eingeführt. Es werden auch einige der Änderungen an der Benutzeroberfläche und die neuen Funktionen der standardpunktdiagramm Multitasking behandelt._

iOS 7 ist ein wichtiges Update für iOS an. Es führt eine vollkommen neue Benutzeroberflächendesign, die Fokus auf den Inhalt und nicht als Anwendung Chrome versetzt. Neben die Visualisierung ändert sich fügt iOS 7 eine Fülle von neuen APIs zum Erstellen von Aktivitäten, die umfangreichere und Erfahrungen. Dieses Dokument Umfragen neuen Technologien eingeführt mit iOS 7 und dient als Ausgangspunkt für die weitere Untersuchung.

## <a name="uiview-animation-enhancements"></a>UIView Animation Erweiterungen

iOS 7 ergänzt die Animation-Unterstützung in UIKit, ermöglicht Anwendungen zu erledigen, die zuvor löschen direkt in das Framework Core-Animation erforderlich. Z. B. `UIView` können jetzt ausführen, für die Steifheit Animationen sowie Keyframe-Animationen, die zuvor eine `CAKeyframeAnimation` angewendet, um eine `CALayer`.

### <a name="spring-animations"></a>Spring-Animationen

 `UIView` unterstützt jetzt Animieren von eigenschaftenänderungen mit einem für die Steifheit Effekt. Um diese hinzuzufügen, rufen Sie entweder die `AnimateNotify` oder `AnimateNotifyAsync` Methode, die Werte für das Dämpfung Spring-Verhältnis und die Geschwindigkeit des ursprünglichen Spring übergeben wird, wie unten beschrieben:

-  `springWithDampingRatio` – Ein Wert zwischen 0 und 1, in dem die und angegebenem schwingungsbereich für kleinere Wert erhöht.
-  `initialSpringVelocity` – Die ersten Spring Geschwindigkeit als Prozentsatz der gesamten Animation Entfernung pro Sekunde.


Der folgende Code erzeugt einen Effekt für die Steifheit aus, wenn das Bild ansichtmittelpunkt ändert:

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

Dieser Effekt für die Steifheit bewirkt, dass das Image Ansicht angezeigt werden, springen die Animation an einem neuen Speicherort für die Center abgeschlossen wurde, wie unten gezeigt:

 ![](images/spring-animation.png "Dieser Effekt für die Steifheit bewirkt, dass das Image Ansicht angezeigt werden, zu springen, sobald sie die Animation an einem neuen Speicherort der Center abgeschlossen hat")

### <a name="keyframe-animations"></a>Keyframe-Animationen

Die `UIView` -Klasse enthält nun die `AnimateWithKeyframes` Methode zum Erstellen von Animationen Keyframe auf eine `UIView`. Diese Methode ist ähnlich wie andere `UIView` Animation-Methoden, mit Ausnahme, die eine zusätzliche `NSAction` als Parameter an die Keyframes enthalten übergeben wird. Innerhalb der `NSAction`, Keyframes hinzugefügt werden, indem `UIView.AddKeyframeWithRelativeStartTime`.

Der folgende Codeausschnitt erstellt z. B. eine Keyframe-Animation sowie den Mittelpunkt einer Ansicht animieren, um die Ansicht zu drehen:

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

Die ersten beiden Parameter für die `AddKeyframeWithRelativeStartTime` Methode geben Sie die Startzeit und Dauer des Keyframes, jeweils als Prozentsatz der gesamten Länge der Animation. Im Beispiel oben führt das Image Ansicht animieren an seiner neuen Rechenzentrum über die ersten zweite gefolgt von 90 Grad drehen, über der nächsten Sekunde. Da gibt an, die Animation `UIViewKeyframeAnimationOptions.Autoreverse` als Option animieren beide Keyframes in umgekehrter Reihenfolge als auch. Schließlich werden die endgültigen Werte auf den ursprünglichen Zustand in den Abschlusshandler festgelegt.

Die in den folgenden Screenshots werden die kombinierte Animation über die Keyframes veranschaulicht:

 ![](images/keyframes.png "Diese Screenshots veranschaulicht die kombinierte Animation über die keyframes")

## <a name="uikit-dynamics"></a>UIKit Dynamics

UIKit Dynamics ist, einen neuen Satz von APIs in UIKit, die Clientanwendungen animierte basierte auf physikalische Aktivitäten erstellen können. UIKit Dynamics kapselt ein 2D physikalische-Modul, um dies zu ermöglichen.

Die API ist deklarative Natur. Sie deklarieren, wie die physikalische Interaktionen durch Erstellen von Objekten - wird aufgerufen, verhalten sich *Verhalten* – express physikalische Konzepten wie Schwerpunkt, Konflikte, Springs. Sie anhängen, die Behavior(s) in ein anderes Objekt, das aufgerufen eine *dynamische Animator*, das eine Ansicht kapselt. Der dynamische Animator akzeptiert wird, dass die deklarierte physikalische Verhalten anwenden *dynamische Elemente* -Elemente implementiert, `IUIDynamicItem`, z. B. eine `UIView`.

Es gibt mehrere unterschiedliche primitiven Verhaltensweisen zum Auslösen von komplexer Interaktionen, einschließlich verfügbar:

-  `UIAttachmentBehavior` – Fügt zwei dynamische Elemente so, dass sie zusammen verschoben oder ein Ansatzpunkt Fügt ein dynamischen Elements.
-  `UICollisionBehavior` – Ermöglicht die dynamische Elemente zur Teilnahme an Konflikte.
-  `UIDynamicItemBehavior` – Gibt einen allgemeinen Satz von Eigenschaften, die auf dynamische Elemente, z. B. Elastizität und Dichte Unstimmigkeiten angewendet.
-  `UIGravityBehavior` -Ein dynamischen Element, verursacht der Elemente, die in Richtung der Schwerpunkte beschleunigen Schwerpunkt gilt.
-  `UIPushBehavior` – Ein dynamisches Element Force gilt.
-  `UISnapBehavior` – Ermöglicht ein dynamisches Element an eine Position mit einem für die Steifheit Effekt ausgerichtet werden soll.


Obwohl viele primitive Typen sind, ist der allgemeine Prozess zum Hinzufügen von physikalische-basierte Interaktionen zu einer Sicht mithilfe des UIKit Dynamics über Verhalten konsistent:

1.  Erstellen Sie eine dynamische Animator.
1.  Erstellen Sie Behavior(s).
1.  Der dynamische Animator Verhalten hinzufügen.


### <a name="dynamics-example"></a>Dynamics-Beispiel

Sehen wir uns ein Beispiel, das Dichte und ein Konflikt Grenze an, fügt eine `UIView`.

#### <a name="uigravitybehavior"></a>UIGravityBehavior

Schwerpunkt anzuzeigende ein Bild hinzufügen, folgt die 3 oben beschriebenen Schritte.

Wir arbeiten der `ViewDidLoad` Methode für dieses Beispiel. Fügen Sie zunächst eine `UIImageView` Instanz wie folgt:

```csharp
image = UIImage.FromFile ("monkeys.jpg");

imageView = new UIImageView (new CGRect (new CGPoint (View.Center.X - image.Size.Width / 2, 0), image.Size)) {
                    Image =  image
                }

View.AddSubview (imageView);
```

Dadurch wird eine Image-Sicht, die auf den oberen Rand des Bildschirms zentriert erstellt. Um das Image "Herbst" mit Schwerpunkt zu machen, erstellen Sie eine Instanz von einem `UIDynamicAnimator`:

```csharp
dynAnimator = new UIDynamicAnimator (this.View);
```

Die `UIDynamicAnimator` akzeptiert eine Instanz eines Verweises `UIView` oder ein `UICollectionViewLayout`, enthält die Elemente, die pro angefügten Behavior(s) animiert werden soll.

Als Nächstes erstellen Sie eine `UIGravityBehavior` Instanz. Sie können ein oder mehrere Objekte implementieren übergeben der `IUIDynamicItem`, z. B. eine `UIView`:

```csharp
var gravity = new UIGravityBehavior (dynItems);
```

Das Verhalten wird ein Array von übergeben `IUIDynamicItem`, die in diesem Fall enthält der einzelnen `UIImageView` Instanz, die wir animieren.

Fügen Sie abschließend das Verhalten der dynamischen Animator hinzu:

```csharp
dynAnimator.AddBehavior (gravity);
```

Dadurch wird in der Abbildung unten animieren mit Schwerpunkt, wie unten dargestellt:

![](images/gravity2.png "Die Anfangsposition des Image") 
![](images/gravity3.png "Endwert Bildspeicherort")

Da es keine Einschränken der Grenzen des Bildschirms, fällt die Image-Ansicht einfach im unteren Bereich. Um die Ansicht zu beschränken, damit das Abbild mit den Rändern des Bildschirms verursacht einen Konflikt, fügen wir eine `UICollisionBehavior`. Dies wird im nächsten Abschnitt beschrieben.

#### <a name="uicollisionbehavior"></a>UICollisionBehavior

Wir beginnen, indem Sie erstellen eine `UICollisionBehavior` und der dynamische Animator hinzufügen, genau wie wir für haben die `UIGravityBehavior`.

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

Die `UICollisionBehavior` enthält eine Eigenschaft namens `TranslatesReferenceBoundsIntoBoundry`. Wenn dieser `true` bewirkt, dass der Verweis der Sicht Grenzen als Konflikt Grenze verwendet werden.

Jetzt, wenn das Bild unten mit Schwerpunkt eine Animation, abprallt es etwas vom unteren Rand des Bildschirms vor ausgeglichen werden, um es zu bewegen.

<!--, as shown below:

 ![](images/bounce.png "Now, when the image animates downward with gravity, it bounces slightly off the bottom of the screen before settling to rest there")-->

#### <a name="uidynamicitembehavior"></a>UIDynamicItemBehavior

Wir können das Verhalten der Abnehmend Image Ansicht mit zusätzliche Verhalten weiter steuern. Es könnte z. B. Hinzufügen einer `UIDynamicItemBehavior` zum Erhöhen der Flexibilität, die Image-Ansicht, um weitere zurückspringt, wenn sie am unteren Rand des Bildschirms verursacht einen Konflikt verursacht.

Hinzufügen einer `UIDynamicItemBehavior` folgt den gleichen Schritten wie bei anderen Verhalten. Erstellen Sie zunächst das Verhalten an:

```csharp
var dynBehavior = new UIDynamicItemBehavior (dynItems) {
    Elasticity = 0.7f
};
```

Fügen Sie dann das Verhalten, um dynamische Animator:

 `dynAnimator.AddBehavior (dynBehavior);`

Mit diesem Verhalten festliegen abprallt die Image-Ansicht mehr, wenn es die Grenze verursacht einen Konflikt.

## <a name="general-user-interface-changes"></a>Benutzeroberfläche für allgemeine Änderungen

Zusätzlich zu der neuen UIKit-APIs wie z. B. UIKit Dynamics Controller Übergänge und verbesserte UIView-Animationen, die oben beschriebenen führt iOS 7 eine Vielzahl von visuelle Änderungen an der Benutzeroberfläche und die entsprechenden API-Änderungen für verschiedene Ansichten und Steuerelemente. Weitere Informationen finden Sie unter der [Ios7 Übersicht über die Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md).

## <a name="text-kit"></a>Text-Kit

Text-Kit ist eine neue API, die leistungsstarke Text Layout und Rendering Funktionen bietet. Basiert auf niedriger Ebene Framework Core-Text, aber ist sehr viel einfacher zu verwenden als Core Text.

Weitere Informationen finden Sie unter unsere [TextKit](~/ios/platform/textkit.md)

## <a name="multitasking"></a>Multitasking

iOS 7 geändert wird, wann und wie die Verarbeitung im Hintergrund ausgeführt wird. Abschluss der Aufgabe in iOS 7 mehr behält Anwendungen als aktiv, wenn Aufgaben im Hintergrund ausgeführt werden, und Anwendungen für die hintergrundverarbeitung in einem nicht zusammenhängenden Bereich reaktiviert werden. iOS 7 fügt auch drei neue APIs für das Aktualisieren von Anwendungen mit neuem Inhalt im Hintergrund hinzu:

-  Abrufen im Hintergrund – ermöglicht es Anwendungen, Inhalte im Hintergrund in regelmäßigen Abständen zu aktualisieren.
-  Remote-Benachrichtigungen - ermöglicht es Anwendungen, die Inhalt zu aktualisieren, wenn Sie eine Pushbenachrichtigung zu empfangen. Die Benachrichtigungen kann eine automatische oder ein Banner auf dem Sperrbildschirm anzeigen.
-  Hintergrundübertragungsdienst – ermöglicht das Hochladen und Herunterladen von Daten, z. B. große Dateien, eine feste Zeit unbegrenzt.


Weitere Informationen zu den neuen Funktionen für Multitasking finden Sie unter den iOS-Abschnitten von der Xamarin [Backgrounding Handbuch](~/ios/app-fundamentals/backgrounding/index.md).

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt einige wichtige neue Mitglieder für iOS an. Zuerst wird gezeigt, wie benutzerdefinierte Übergänge View-Controller hinzugefügt werden. Anschließend wird gezeigt, wie Übergänge in Auflistungsansichten, sowohl von innerhalb eines Controllers Navigation sowie zwischen Auflistungsansichten interaktiv verwendet werden. Als Nächstes stellt mehrere Erweiterungen UIView Animationen anzeigen, wie Anwendungen UIKit Dinge verwenden, die zuvor direkten Programmierens mit Core Animation erforderlich. Schließlich wird die neue UIKit Dynamics-API, die ein Modul physikalische UIKit Skriptentwicklern bereitstellt, zusammen mit der rich-Text-Unterstützung jetzt verfügbar in Text Kit Framework eingeführt.

## <a name="related-links"></a>Verwandte Links

- [Einführung in die iOS 7 (Beispiel)](https://developer.xamarin.com/samples/monotouch/IntroToiOS7)
- [Übersicht über die iOS 7-Benutzeroberfläche](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Hintergrundverarbeitung](~/ios/app-fundamentals/backgrounding/index.md)
