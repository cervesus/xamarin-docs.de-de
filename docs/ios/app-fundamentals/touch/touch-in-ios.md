---
title: Tippen Sie in iOS
ms.topic: article
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 838a11f078d735759eda1d45a082ccbad51e2779
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="touch-in-ios"></a>Tippen Sie in iOS

Es ist wichtig, zu verstehen, die Berührungsereignisse touch-APIs in einer iOS-Anwendung, wie sie alle physischen Interaktionen mit dem Gerät von zentraler Bedeutung sind. Alle Touch Interaktionen umfassen eine `UITouch` Objekt. In diesem Artikel erfahren wir, wie Sie die `UITouch` Klasse und den APIs Touch unterstützen. Es wird später auf unser wissen, wie Sie Gesten unterstützen erweitert.

## <a name="enabling-touch"></a>Aktivieren Touch

Steuerelemente in `UIKit` – diese Unterklassen aus UIControl – sind dies der Fall ist abhängig von Eingreifen des Benutzers, die sie Gesten in UIKit integriert haben und daher ist es nicht notwendig, die durch Toucheingabe zu aktivieren. Es ist bereits aktiviert.

Jedoch viele Ansichten in `UIKit` verfügen nicht über die Fingereingabe, die standardmäßig aktiviert. Es gibt zwei Methoden, Fingereingabe auf ein Steuerelement zu aktivieren. Die erste Möglichkeit besteht darin das Kontrollkästchen Benutzer Interaktion aktiviert Pad-Eigenschaft des iOS-Designer zu prüfen, wie im folgenden Screenshot gezeigt:

 [![](touch-in-ios-images/image1.png "Aktivieren Sie das Kontrollkästchen Benutzer Interaktion aktiviert Pad-Eigenschaft des iOS-Designer")](touch-in-ios-images/image1.png#lightbox)

Wir können auch einen Controller legen Sie die `UserInteractionEnabled` Eigenschaft auf "true", auf eine `UIView` Klasse. Dies ist erforderlich, wenn die Benutzeroberfläche im Code erstellt wird.

Die folgende Codezeile ist ein Beispiel:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Berührungsereignisse

Es gibt drei Phasen der Fingereingabe, die auftreten, wenn der Benutzer den Bildschirm berührt, deren Finger verschiebt oder ihre Finger entfernt. Diese Methoden werden in definiert `UIResponder`, dies ist die Basisklasse für UIView. iOS wird auf die zugeordneten Methoden überschreiben die `UIView` und `UIViewController` Touch behandelt:

-  `TouchesBegan` – Dies wird aufgerufen, wenn der Bildschirm zuerst verwendet wird.
-  `TouchesMoved` – Dies wird aufgerufen, wenn der Speicherort der Touch Änderungen als der Benutzer ihre Finger auf dem Bildschirm gleitende ist.
-  `TouchesEnded` oder `TouchesCancelled` – `TouchesEnded` wird aufgerufen, wenn der Benutzer Finger außerhalb des Bildschirms aufgehoben werden.  `TouchesCancelled` Ruft aufgerufen, wenn iOS Touch – z. B. abbricht, wenn ein Benutzer sein eigenes Finger Weg von der eine Schaltfläche zum Abbrechen Drücken einer Folien.


Touch-Ereignisse Reisen rekursiv nach unten über den Stapel von UIViews, zum Überprüfen, ob das Touch-Ereignis innerhalb der Grenzen eines Objekts Sicht ist. Dies wird häufig aufgerufen _Treffertests_. Sie zuerst auf den obersten aufgerufen `UIView` oder `UIViewController` wird dann aufgerufen werden, auf die `UIView` und `UIViewControllers` weiter unten in der Hierarchie anzeigen.

Ein `UITouch` -Objekt wird erstellt, wenn Sie jedes Mal, die der Benutzer den Bildschirm berührt. Die `UITouch` -Objekt enthält Daten über die Fingereingabe, z. B. die Fingereingabe wo es aufgetreten ist, Auftretens war die Fingereingabe Wischen usw.,. Die Berührungsereignisse abrufen eine Eigenschaft Fingereingaben – übergeben ein `NSSet` , enthält eine oder mehrere berührt. Wir können mit dieser Eigenschaft können Sie einen Verweis auf eine Fingereingabe erhalten und die Anwendung Antwort bestimmen.

Klassen, die eine der Berührungsereignisse überschreiben soll zuerst die basisimplementierung aufrufen und rufen dann die `UITouch` mit dem Ereignis verknüpften Objekt. Um einen Verweis auf die erste Fingereingabe zu erhalten, rufen Sie die `AnyObject` Eigenschaft umgewandelt werden muss als eine `UITouch` wie im folgenden Beispiel angezeigt:

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

iOS erkennt automatisch die aufeinander folgenden schnell auf dem Bildschirm berührt und werden als einer einzelnen tippbewegung in einer einzelnen gesammelt `UITouch` Objekt. Dadurch wird die Überprüfung auf einem Doppeltippen genauso einfach wie das Überprüfen der `TapCount` -Eigenschaft verwenden, wie im folgenden Code dargestellt:

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

Multi-Touch ist standardmäßig auf die Steuerelemente nicht aktiviert. Multi-Touch kann in der iOS-Designer aktiviert werden, wie der folgende Screenshot veranschaulicht:

 [![](touch-in-ios-images/image2.png "Multi-Touch in der iOS-Designer aktiviert")](touch-in-ios-images/image2.png#lightbox)

Es ist auch möglich, legen Sie Multi-Touch programmgesteuert durch Festlegen der `MultipleTouchEnabled` Eigenschaft wie in der folgenden Zeile des Codes dargestellt:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Um zu bestimmen, wie viele Finger den Bildschirm berührt, verwenden die `Count` Eigenschaft auf die `UITouch` Eigenschaft:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Festlegen des Pfads Touch

Die Methode `UITouch.LocationInView` gibt ein CGPoint-Objekt, das die Koordinaten für die Fingereingabe innerhalb einer bestimmten Ansicht enthält. Wir können darüber hinaus testen, um festzustellen, ob dieser Position innerhalb eines Steuerelements durch Aufrufen der Methode `Frame.Contains`. Der folgende Codeausschnitt zeigt ein Beispiel dafür:

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Nun, da wir einen Überblick über die Berührungsereignisse in iOS haben, erfahren wir Geste Merkmale aus.

## <a name="gesture-recognizers"></a>Geste Prüfer

Geste Prüfer können erheblich vereinfachen und reduzieren den Programmierung Aufwand für die Toucheingabe in einer Anwendung zu unterstützen. iOS Geste Prüfer aggregiert werden eine Reihe von touchereignissen in ein einzelnes Touch-Ereignis.

Xamarin.iOS enthält die Klasse `UIGestureRecognizer` als Basisklasse für die folgenden integrierten Geste Merkmale:

-  *UITapGesturesRecognizer* – Dies ist für eine oder mehrere Taps.
-  *UIPinchGestureRecognizer* – Pinching und auseinander Finger verbreiten.
-  *UIPanGestureRecognizer* – Balance oder ziehen.
-  *UISwipeGestureRecognizer* – ein Lesegerät in beliebiger Richtung.
-  *UIRotationGestureRecognizer* – zwei Finger während des Verschiebens gegen den Uhrzeigersinn oder gegen den Uhrzeigersinn zu drehen.
-  *UILongPressGestureRecognizer* – drücken und halten, auch bezeichnet als lang drücken oder auf lang.


Das grundlegende Muster mit einer Geste Erkennung lautet wie folgt:

1.  **Instanziieren Sie das Gestenhandler-Erkennungsmodul** – instanziieren Sie zunächst eine `UIGestureRecognizer` Unterklasse. Das Objekt, das instanziiert wird durch eine Sicht verknüpft wird, und wird Garbage Collection verschoben werden, wenn die Sicht verworfen wird. Es ist nicht erforderlich, diese Sicht als eine Variable einer Klasse zu erstellen.
1.  **Konfigurieren Sie Clienteinstellungen Geste** – der nächste Schritt besteht darin die Erkennung Geste zu konfigurieren. Die Xamarin Dokumentation auf `UIGestureRecognizer` und seiner Unterklassen eine Liste der Eigenschaften, die festgelegt werden können, zum Steuern des Verhaltens von einem `UIGestureRecognizer` Instanz.
1.  **Konfigurieren des Ziels** – aufgrund seiner Heritage Objective-C Xamarin.iOS keine Ereignisse auslösen, wenn eine Geste Erkennung einer Aktion entspricht.  `UIGestureRecognizer` verfügt über eine Methode – `AddTarget` – eines anonymen Delegaten oder einen Objective-C-Selektor durch den Code ausführen, wenn die Gestenhandler-Erkennung eine Übereinstimmung stellt akzeptieren können.
1.  **Geste Erkennung aktivieren** – genau wie mit Berührungsereignisse, Gesten nur erkannt werden, wenn Touch Interaktionen aktiviert sind.
1.  **Das Erkennungsmodul Geste der Ansicht hinzufügen** – der letzte Schritt besteht darin Bewegung durch Aufrufen einer Ansicht hinzufügen `View.AddGestureRecognizer` , und eine Geste Erkennungsmodul-Objekt übergeben.

Finden Sie in der [Gestenhandler Erkennungsmodul Beispiele](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) für Weitere Informationen zum in Code zu implementieren.

Wenn die Bewegung Ziel aufgerufen wird, wird es einen Verweis auf die Aktion übergeben werden, die aufgetreten sind. Dadurch wird das Ziel der Aktion zum Abrufen von Informationen zur Bewegung, die aufgetreten sind. Das Ausmaß der verfügbaren Informationen hängt vom Typ der Gestenhandler-Erkennung, die verwendet wurde. Finden Sie in die Xamarin Dokumentation Informationen zu den verfügbaren Daten für jeden `UIGestureRecognizer` Unterklasse.

Es ist wichtig zu beachten, dass nach eine Bewegung Erkennung einer Ansicht hinzugefügt wurde, die Sicht (und alle Ansichten, darunter) keine Touch Ereignisse empfangen werden. Um Berührungsereignisse gleichzeitig mit Gesten, ermöglichen die `CancelsTouchesInView` Eigenschaft muss auf "false" festgelegt werden, wie im folgenden Code veranschaulicht:

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Jede `UIGestureRecognizer` verfügt über eine Statuseigenschaft, die wichtige Informationen über den Status der Erkennung Geste bereitstellt. Jedes Mal, wenn der Wert dieser Eigenschaft ändert, werden iOS die abonnierende Methode, die Übergabe eines Updates aufrufen. Falls eine benutzerdefinierte Aktion Erkennung nie die State-Eigenschaft aktualisiert werden, wird der Abonnenten Rendern die Erkennung Geste nutzlos nie aufgerufen.

Gesten können als eine oder zwei Arten zusammengefasst werden:

1.  *Diskrete* – diese Gesten Auslösen der ersten einmalig sie erkannt werden.
1.  *Fortlaufende* – diese Gesten weiterhin ausgelöst werden, solange sie erkannt werden.


Geste Prüfer ist in einem der folgenden Zustände vorhanden:

-  *Mögliche* – Dies ist der Anfangszustand aller Geste Prüfer sind. Dies ist der Standardwert der State-Eigenschaft.
-  *Began* – Wenn eine fortlaufende Geste zuerst erkannt wird, wird der Status auf Began festgelegt. Dadurch können abonniert werden, um die Unterscheidung zwischen Geste Recognition beim Starten und wenn er geändert wird.
-  *Geänderte* – nachdem eine kontinuierliche Geste begonnen hat, aber noch nicht abgeschlossen ist, der Status wird festgelegt auf geänderte jedes Mal, wenn eine Fingereingabe verschoben oder geändert wird, solange er immer noch innerhalb der erwarteten Parameter der Bewegung ist.
-  *Abgebrochen* – dieser Status wird festgelegt, wenn die Erkennung von Began Changed anonymousMethod, und klicken Sie dann die Fingereingaben so im Vergleich zu nicht mehr geändert das Muster der Bewegung passen.
-  *Erkannt* – der Status wird festgelegt werden, wenn die Gestenhandler-Erkennung eine Sammlung von Fingereingaben vergleicht und informiert dem Abonnenten, dass die Aktion abgeschlossen wurde.
-  *Beendet* – Dies ist ein Alias für den Status der erkannt.
-  *Fehler bei* – Wenn die Gestenhandler-Erkennung nicht mehr die Fingereingaben übereinstimmen kann für der Status wird geändert, um Fehler handelt, an.


Xamarin.iOS stellt diese Werte in der `UIGestureRecognizerState` Enumeration.

## <a name="working-with-multiple-gestures"></a>Arbeiten mit mehreren Gesten

Standardmäßig lässt iOS keine Standard-Gesten zum gleichzeitig ausgeführt werden. Stattdessen erhält jede Geste Erkennungsmodul Berührungsereignisse in einer nicht-deterministischen Reihenfolge. Der folgende Codeausschnitt veranschaulicht, wie Sie eine Geste-Erkennung, die gleichzeitig ausgeführt werden:

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Es ist auch möglich, eine Geste in iOS deaktivieren. Es gibt zwei delegateigenschaften, mit denen eine Geste Erkennung untersuchen Sie den Status der Anwendung und die aktuelle Berührungsereignisse Entscheidungen zum und wenn eine Geste erkannt werden sollen. Sind die zwei Ereignisse:

1.  *ShouldReceiveTouch* – dieser Delegat wird aufgerufen, unmittelbar bevor die Bewegung Erkennung wird ein Touch-Ereignis übergeben, und bietet die Möglichkeit, die Fingereingaben überprüfen und entscheiden, welche Fingereingaben durch die Erkennung Geste verarbeitet werden.
1.  *ShouldBegin* – Dies wird aufgerufen, wenn versucht wird, dass eine Erkennung Status von möglich zu einem anderen Status zu ändern. "False" zurückgeben, wird den Status der Erkennung Geste in konnte nicht geändert werden erzwungen.


Sie können diese Methoden mit einem stark typisierten überschreiben `UIGestureRecognizerDelegate`, einen schwachen Delegaten oder Bindung über die Ereignis-Handler-Syntax, wie der folgende Codeausschnitt veranschaulicht:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Schließlich ist es möglich, eine Geste Erkennung in die Warteschlange so, dass er nur erfolgreich ist, schlägt eine andere Erkennung der Bewegung. Beispielsweise sollte eine einfachen tippen Geste Erkennung nur erfolgreich, bei eine Doppeltippen Geste Erkennung ein Fehler auftritt. Der folgende Codeausschnitt enthält ein Beispiel dafür:

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Erstellen eine benutzerdefinierte Geste

Obwohl iOS einige Standardeinstellungen Gestenhandler-Merkmale enthält, zum Erstellen von benutzerdefinierten Geste Prüfer in bestimmten Fällen möglicherweise erforderlich. Erstellen eine benutzerdefinierte Aktion Erkennung umfasst die folgenden Schritte aus:

1.  Unterklasse `UIGestureRecognizer` .
1.  Überschreiben Sie die entsprechenden Touch Ereignismethoden.
1.  Blase Recognition Status über die Basisklasse State-Eigenschaft.


Dieses ein praktischen Beispiels werden ausführlicher den [berühren Sie mit der Verwendung in iOS](ios-touch-walkthrough.md) Exemplarische Vorgehensweise.
