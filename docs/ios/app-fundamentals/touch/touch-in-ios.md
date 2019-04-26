---
title: Berührungsereignisse und Gesten in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie mit Touch-Ereignissen, die Multi-Touch, Gesten, mehrere Gesten und benutzerdefinierten stiftbewegungen in Xamarin.iOS-Anwendungen arbeiten.
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: f7160c48e1b1ac85f4aa0173c0eb9f42b8fefca2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218617"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>Berührungsereignisse und Gesten in Xamarin.iOS

Es ist wichtig, zu verstehen, die touchereignisse und touch-APIs in einer iOS-Anwendung, wie sie für alle physischen Interaktionen mit dem Gerät von zentraler Bedeutung sind. Alle Touch-Interaktionen umfassen eine `UITouch` Objekt. In diesem Artikel erfahren wir, wie Sie mit der `UITouch` -Klasse und den zugehörigen APIs Touch unterstützen. Später werden wir für unser Wissen zu erfahren, wie Sie die Unterstützung für Bewegungen erweitern.

## <a name="enabling-touch"></a>Aktivieren die Fingereingabe

Steuerelemente in `UIKit` – diese Unterklassen von UIControl – sind dies der Fall ist abhängig von Benutzerinteraktionen, die sie Bewegungen in UIKit integriert haben, und aus diesem Grund ist es nicht notwendig, die Toucheingabe zu aktivieren. Es ist bereits aktiviert.

Allerdings viele Ansichten im `UIKit` müssen sich nicht auf die Fingereingabe, die standardmäßig aktiviert. Es gibt zwei Möglichkeiten, Fingereingabe auf einem Steuerelement zu aktivieren. Die erste Möglichkeit besteht, überprüfen Sie die Benutzer die Interaktion aktiviert Kontrollkästchen im Bereich "Eigenschaft" des iOS-Designer, wie im folgenden Screenshot gezeigt:

 [![](touch-in-ios-images/image1.png "Aktivieren Sie das Benutzer Interaktion aktiviert Kontrollkästchen im Bereich \"Eigenschaft\" der iOS-Designer")](touch-in-ios-images/image1.png#lightbox)

Wir können auch einen Controller zum Festlegen der `UserInteractionEnabled` Eigenschaft auf "true", auf eine `UIView` Klasse. Dies ist erforderlich, wenn die Benutzeroberfläche in Code erstellt wird.

Die folgende Codezeile ist ein Beispiel:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Berührungsereignisse

Es gibt drei Phasen der Fingereingabe, die auftreten, wenn der Benutzer den Bildschirm berührt, den Finger bewegt oder den Finger entfernt. Diese Methoden werden in definiert `UIResponder`, dies ist die Basisklasse für UIView. iOS wird auf die zugeordneten Methoden überschreiben die `UIView` und `UIViewController` Fingereingabe behandelt:

-  `TouchesBegan` – Dies wird aufgerufen, wenn der Bildschirm zuerst berührt wird.
-  `TouchesMoved` – Dies wird aufgerufen, wenn der Standort die Touch-Änderungen wie der Benutzer ihren Finger auf dem Bildschirm gleitend ist.
-  `TouchesEnded` oder `TouchesCancelled` – `TouchesEnded` wird aufgerufen, wenn außerhalb des Bildschirms die Finger des Benutzers aufgehoben werden.  `TouchesCancelled` Ruft aufgerufen, wenn iOS die Berührung – z. B. abbricht, wenn ein Benutzer seine Finger Weg von einer Schaltfläche zum abzubrechen. eine Folien.


Touch-Ereignisse Travel rekursiv nach unten durch den Stapel der UIViews, zu überprüfen, ob das touchereignis innerhalb der Grenzen von ein-Objekt ist. Dies wird häufig als _Treffertests_. Sie werden zunächst auf den obersten aufgerufen werden `UIView` oder `UIViewController` und wird dann aufgerufen werden, auf die `UIView` und `UIViewControllers` unten in der Hierarchie von Inhaltsansichten.

Ein `UITouch` erstellt jedes Mal, die der Benutzer den Bildschirm berührt. Die `UITouch` Objekt enthält Daten über die Fingereingabe, z. B. die Berührung des Auftretens wo es aufgetreten ist, war die Berührung einer streifbewegung usw.,. Die touchereignisse abrufen eine Eigenschaft des Workflows – übergeben eine `NSSet` , die eine oder mehrere Workflows enthält. Wir können mit dieser Eigenschaft können Sie einen Verweis auf eine Fingereingabe erhalten und die Anwendung die Antwort bestimmen.

Klassen, die eine von der Fingereingabe-Ereignissen zu überschreiben sollte zuerst die basisimplementierung aufrufen und dann die `UITouch` Objekt, mit dem Ereignis verknüpft ist. Rufen Sie zum Abrufen eines Verweises auf die erste Berührung der `AnyObject` Eigenschaft und wandeln Sie sie als eine `UITouch` wie im folgenden Beispiel gezeigt:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        //code here to handle touch
    }
}
```

iOS erkennt automatisch, aufeinander folgende schnell auf dem Bildschirm berührt und erfasst sie als in einem einzigen fingertipp `UITouch` Objekt. Dadurch wird die Überprüfung für eine Doppeltippen so einfach wie das Überprüfen der `TapCount` -Eigenschaft, wie im folgenden Code dargestellt:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        if (touch.TapCount == 2)
        {
            // do something with the double touch.
        }
    }
}
```

## <a name="multi-touch"></a>Multi-Touch

Multitouch ist standardmäßig auf Steuerelemente nicht aktiviert. Multitouch kann in der iOS-Designer aktiviert werden, wie im folgenden Screenshot dargestellt:

 [![](touch-in-ios-images/image2.png "Mehrfingereingabe in der iOS-Designer aktiviert")](touch-in-ios-images/image2.png#lightbox)

Es ist auch möglich, festzulegende Multitouch programmgesteuert durch Festlegen der `MultipleTouchEnabled` Eigenschaft wie in der folgenden Zeile des Codes dargestellt:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Um zu bestimmen, wie viele Finger den Bildschirm berührt, verwenden die `Count` Eigenschaft für die `UITouch` Eigenschaft:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Bestimmen die Touch-Position

Die Methode `UITouch.LocationInView` gibt eine CGPoint-Objekt, das die Koordinaten der die Fingereingabe innerhalb einer bestimmten Ansicht enthält. Wir können außerdem testen, um festzustellen, ob dieser Position in einem Steuerelement durch Aufrufen der Methode `Frame.Contains`. Der folgende Codeausschnitt zeigt ein Beispiel hierfür:

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Nun, da wir einen Überblick über die touchereignisse in iOS haben, betrachten wir Geste Erkennungen.

## <a name="gesture-recognizers"></a>Geste Erkennungen

Geste Erkennungen können erheblich vereinfachen und reduzieren den Programmieraufwand zur Unterstützung der Fingereingabe in einer Anwendung. iOS-Geste Erkennungen aggregieren eine Reihe von Touch-Ereignissen in einem einzelnen Touch-Ereignis.

Xamarin.iOS enthält die Klasse `UIGestureRecognizer` als Basisklasse für die folgenden integrierten Geste Merkmale:

-  *UITapGestureRecognizer* – Dies ist für eine oder mehrere Taps.
-  *UIPinchGestureRecognizer* – Pinching und auseinander Finger verteilen.
-  *UIPanGestureRecognizer* : Schwenkansicht oder ziehen.
-  *UISwipeGestureRecognizer* – Wischen in eine beliebige Richtung.
-  *UIRotationGestureRecognizer* – zwei Fingern in eine Bewegung im Uhrzeigersinn oder gegen den Uhrzeigersinn zu drehen.
-  *UILongPressGestureRecognizer* – drücken und halten, auch bezeichnet als drücken Sie Long- oder Long-Wert auf.


Das grundlegende Muster mit einer stiftbewegungs-Erkennung ist wie folgt aus:

1.  **Instanziieren der stiftbewegungs-Erkennung** : Instanziieren Sie zunächst eine `UIGestureRecognizer` Unterklasse. Das Objekt, das Instanziieren von einer Ansicht verknüpft wird, und wird Garbage Collection verschoben werden, wenn die Ansicht verworfen wird. Es ist nicht erforderlich, diese Ansicht als eine Klassenvariable zu erstellen.
1.  **Konfigurieren Sie alle Einstellungen für die Bewegung** – im nächste Schritt wird die stiftbewegungs-Erkennung zu konfigurieren. Xamarin Dokumentation auf `UIGestureRecognizer` und deren Unterklassen eine Liste der Eigenschaften, die festgelegt werden, um die Steuerung des Verhaltens einer `UIGestureRecognizer` Instanz.
1.  **Konfigurieren des Ziels** – aufgrund der Objective-C-Erbes Xamarin.iOS keine Ereignisse auslösen, wenn eine stiftbewegungs-Erkennung einer Aktion entspricht.  `UIGestureRecognizer` verfügt über eine Methode – `AddTarget` –, kann ein anonymer Delegat oder ein Objective-C-Auswahl durch den Code ausgeführt werden, wenn die stiftbewegungs-Erkennung eine Übereinstimmung wird akzeptieren.
1.  **Aktivieren Sie stiftbewegungs-Erkennung** – genau wie mit Touch-Ereignissen, Gesten nur erkannt werden, wenn die touchinteraktion aktiviert sind.
1.  **Fügen Sie zur Ansicht der stiftbewegungs-Erkennung** – der letzte Schritt ist die Bewegung durch Aufrufen einer Ansicht hinzufügen `View.AddGestureRecognizer` , und übergeben sie eine Geste erkennerobjekt.

Finden Sie in der [Geste Erkennung Beispiele](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) für Weitere Informationen dazu, wie sie in Code implementiert.

Wenn die Bewegung des Ziel aufgerufen wird, wird es einen Verweis auf die Aktion übergeben werden, die aufgetreten sind. Dadurch wird das Ziel der Aktion zum Abrufen von Informationen über die Eingabeaktion, die aufgetreten sind. Der Umfang der Informationen verfügbar sind, hängt von der Art der stiftbewegungs-Erkennung, die verwendet wurde. Finden Sie in Xamarin Dokumentation Informationen zu den verfügbaren Daten für die einzelnen `UIGestureRecognizer` Unterklasse.

Es ist wichtig zu beachten, dass nach eine stiftbewegungs-Erkennung zu einer Ansicht hinzugefügt wurde, die Sicht (und alle Ansichten, darunter) keine Berührungsereignisse empfangen werden. Berührungsereignisse gleichzeitig mit Bewegungen können die `CancelsTouchesInView` Eigenschaft muss auf "False" festgelegt werden, wie der folgende Code veranschaulicht:

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Jede `UIGestureRecognizer` verfügt über eine Statuseigenschaft, die wichtige Informationen über den Status von stiftbewegungs-Erkennung bereitstellt. Jedes Mal, wenn der Wert dieser Eigenschaft ändert, wird iOS die abonnierende Methode legen sie ein Update aufrufen. Wenn eine benutzerdefinierte stiftbewegungs-Erkennung nie die State-Eigenschaft aktualisiert wird, wird der Abonnenten Rendern die stiftbewegungserkennung nutzlos nie aufgerufen.

Bewegungen können als eine der beiden Typen zusammengefasst werden:

1.  *Diskrete* – diese Gesten nur ausgelöst, der erste Zeitpunkt werden erkannt.
1.  *Fortlaufende* – für diese Gesten weiterhin ausgelöst werden, solange sie erkannt werden.


Geste Erkennungen vorhanden ist, in einem der folgenden Zustände:

-  *Mögliche* – Dies ist der ursprüngliche Zustand der Erkennungen für alle Bewegung. Dies ist der Standardwert der State-Eigenschaft.
-  *Began* – Wenn eine fortlaufende Geste zuerst erkannt wird, der Zustand auf Began festgelegt ist. Dadurch können abonniert werden, um die Unterschiede zwischen dem Beginn der gestenerkennung und wenn dieses geändert wird.
-  *Changed* – nach eine kontinuierliche Geste begonnen hat, jedoch noch nicht abgeschlossen ist, der Zustand auf festgelegt geändert jedes Mal, wenn eine Fingereingabe verschoben oder geändert wird, solange sie weiterhin in den erwarteten Parametern der Bewegung ist.
-  *Abgebrochen* – dieser Status wird festgelegt, wenn die Erkennung von Began geändert wurde, und klicken Sie dann die Workflows, die nicht mehr so, als geändert das Muster der Bewegung passen.
-  *Erkannt* – der Zustand festgelegt, wenn die stiftbewegungserkennung eine Sammlung von Workflows vergleicht und dem Abonnenten informiert, dass die Bewegung beendet wurde.
-  *Beendet* – Dies ist ein Alias für den Recognized-Zustand.
-  *Fehler bei* – Wenn die stiftbewegungs-Erkennung nicht mehr die Fingereingaben übereinstimmen kann gelauscht wird für der Status wird zu Fehler geändert.


Xamarin.iOS stellt diese Werte in der `UIGestureRecognizerState` Enumeration.

## <a name="working-with-multiple-gestures"></a>Arbeiten mit mehreren Bewegungen

Standardmäßig lässt iOS nicht standardmäßige Gesten gleichzeitig ausgeführt werden. Stattdessen wird jede stiftbewegungs-Erkennung Berührungsereignisse in einer nicht-deterministischen Reihenfolge empfangen. Der folgende Codeausschnitt veranschaulicht, wie Sie eine stiftbewegungs-Erkennung, die gleichzeitig ausgeführt werden:

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Es ist auch möglich, eine Geste in iOS zu deaktivieren. Es gibt zwei delegateigenschaften, mit denen eine stiftbewegungs-Erkennung zur statusbestimmung von einer Anwendung und die aktuellen touchereignisse, Entscheidungen dazu, wie und wenn eine Geste erkannt werden sollen. Die zwei Ereignisse sind:

1.  *ShouldReceiveTouch* : Dieser Delegat wird aufgerufen, unmittelbar bevor die stiftbewegungs-Erkennung ein touchereignis übergeben wird, und bietet die Möglichkeit, die Fingereingaben überprüfen und entscheiden, welche Workflows von stiftbewegungs-Erkennung verarbeitet werden.
1.  *ShouldBegin* – Dies wird aufgerufen, wenn versucht wird, dass eine Erkennung Zustand von möglich in einem anderen Zustand ändern. False zurückgibt, wird den Status der stiftbewegungs-Erkennung zu Fehler geändert werden erzwungen.


Sie können diese Methoden mit einem stark typisierten überschreiben `UIGestureRecognizerDelegate`, eine schwache Delegaten oder Bindung über die Ereignis-Handler-Syntax, wie der folgende Codeausschnitt veranschaulicht:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Abschließend ist es möglich, eine stiftbewegungs-Erkennung in die Warteschlange, damit es nur erfolgreich ist, wenn eine andere stiftbewegungs-Erkennung ein Fehler auftritt. Beispielsweise sollten eine einzigen fingertipp stiftbewegungs-Erkennung nur erfolgreich, wenn keine Doppeltippen stiftbewegungs-Erkennung. Der folgende Codeausschnitt enthält ein Beispiel hierfür:

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Erstellen eine benutzerdefinierte Aktion

Obwohl iOS einen Standard-Geste Erkennungen bereitstellt, kann es zum Erstellen von benutzerdefinierten Gesten Erkennungen in bestimmten Fällen erforderlich sein. Erstellen eine benutzerdefinierte stiftbewegungs-Erkennung umfasst die folgenden Schritte aus:

1.  Unterklasse `UIGestureRecognizer` .
1.  Überschreiben Sie die entsprechenden Touch-Event-Methoden.
1.  Status der Erkennung über die Basisklasse State-Eigenschaft übergeben.


Ein praktisches Beispiel davon wird in behandelt die [mithilfe von Toucheingabe in iOS](ios-touch-walkthrough.md) Exemplarische Vorgehensweise.
