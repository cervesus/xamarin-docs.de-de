---
title: Touchereignisse und Gesten in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie in xamarin. IOS-Anwendungen mit Touch-Ereignissen, Multitouch, Gesten, mehreren Gesten und benutzerdefinierten Gesten arbeiten.
ms.prod: xamarin
ms.assetid: DA666DC9-446E-4CD1-B5A0-C6FFBC7E53AD
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/18/2017
ms.openlocfilehash: 492682b1f7647201f15678a5162281e0a7a916d6
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70280093"
---
# <a name="touch-events-and-gestures-in-xamarinios"></a>Touchereignisse und Gesten in xamarin. IOS

Es ist wichtig, die touchereignisse und touchapis in einer IOS-Anwendung zu verstehen, da Sie für alle physischen Interaktionen mit dem Gerät von zentraler Bedeutung sind. Alle touchinteraktionen umfassen ein `UITouch` -Objekt. In diesem Artikel erfahren Sie, wie Sie die `UITouch` -Klasse und ihre APIs zur Unterstützung von Finger Eingaben verwenden. Später werden wir unsere Kenntnisse erweitern, um zu erfahren, wie Gesten unterstützt werden.

## <a name="enabling-touch"></a>Aktivieren der Fingereingabe

Steuerelemente `UIKit` in – diese untergeordneten Elemente von UIControl – sind so von der Benutzerinteraktion abhängig, dass Sie Gesten in UIKit integriert haben. Daher ist es nicht erforderlich, die Toucheingabe zu aktivieren. Sie ist bereits aktiviert.

Für viele der Sichten in `UIKit` ist die Fingereingabe jedoch standardmäßig nicht aktiviert. Es gibt zwei Möglichkeiten, die Fingereingabe für ein Steuerelement zu aktivieren. Die erste Möglichkeit ist das Aktivieren des Kontrollkästchens aktivierte Benutzerinteraktion im Eigenschaften-Pad des IOS-Designers, wie im folgenden Screenshot zu sehen:

 [![](touch-in-ios-images/image1.png "Aktivieren Sie das Kontrollkästchen aktivierte Benutzerinteraktion im Eigenschaften-Pad des IOS-Designers.")](touch-in-ios-images/image1.png#lightbox)

Wir können auch einen Controller verwenden, um die `UserInteractionEnabled` -Eigenschaft für eine `UIView` Klasse auf true festzulegen. Dies ist erforderlich, wenn die Benutzeroberfläche im Code erstellt wird.

Die folgende Codezeile ist ein Beispiel:

```csharp
imgTouchMe.UserInteractionEnabled = true;
```

## <a name="touch-events"></a>Berührungsereignisse

Es gibt drei Berührungs Phasen, die auftreten, wenn der Benutzer den Bildschirm berührt, seinen Finger bewegt oder den Finger entfernt. Diese Methoden werden in `UIResponder`definiert. Dies ist die Basisklasse für UIView. IOS überschreibt die zugeordneten Methoden in `UIView` `UIViewController` und, um die Berührungs Behandlung zu behandeln:

- `TouchesBegan`– Dies wird aufgerufen, wenn der Bildschirm zum ersten Mal berührt wird.
- `TouchesMoved`– Dies wird aufgerufen, wenn sich die Position der Fingereingabe ändert, während der Benutzer die Finger auf dem Bildschirm verrutscht.
- `TouchesEnded`oder `TouchesCancelled` –`TouchesEnded` wird aufgerufen, wenn die Finger des Benutzers vom Bildschirm entfernt werden.  `TouchesCancelled`wird aufgerufen, wenn IOS den Touch-Vorgang abbricht – beispielsweise, wenn ein Benutzer seinen Finger von einer Schaltfläche bewegt, um eine Taste abzubrechen.


Berührungs Ereignisse Reisen rekursiv durch den Stapel von UIViews, um zu überprüfen, ob sich das Berührungs Ereignis innerhalb der Grenzen eines Ansichts Objekts befindet. Dies wird häufig als _Treffer Test_bezeichnet. Sie werden zuerst `UIView` auf der obersten Ebene oder `UIViewController` aufgerufen und dann in der Ansichts Hierarchie `UIView` unter `UIViewControllers` und unter Ihnen aufgerufen.

Jedes `UITouch` Mal, wenn der Benutzer den Bildschirm berührt, wird ein-Objekt erstellt. Das `UITouch` Objekt enthält Daten über die Fingereingabe, wie z. b. den Zeitpunkt, zu dem die Fingereingabe aufgetreten ist, den Zeitpunkt, an dem sich der Fingerabdruck befand, usw. Die Touch-Ereignisse werden an eine berührt-Eigenschaft `NSSet` weitergegeben – eine, die einen oder mehrere Berührungen enthält. Wir können diese Eigenschaft verwenden, um einen Verweis auf eine Fingereingabe zu erhalten und die Antwort der Anwendung zu bestimmen.

Klassen, die eines der Berührungs Ereignisse überschreiben, sollten zunächst die Basis Implementierung und dann das `UITouch` dem Ereignis zugeordnete-Objekt abrufen. Um einen Verweis auf den ersten Fingerabdruck abzurufen, rufen `AnyObject` Sie die-Eigenschaft auf, `UITouch` und wandeln Sie Sie als ein, wie im folgenden Beispiel gezeigt:

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

IOS erkennt automatisch aufeinander folgende schnell Berührungen auf dem Bildschirm und sammelt Sie alle als einen Tap in `UITouch` einem einzelnen Objekt. Dadurch wird die Überprüfung auf eine doppelte Tap genauso einfach wie `TapCount` das Überprüfen der Eigenschaft, wie im folgenden Code veranschaulicht:

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

## <a name="multi-touch"></a>Multitouch

Multi-Touch ist für Steuerelemente standardmäßig nicht aktiviert. Multi-Touch kann im IOS-Designer aktiviert werden, wie im folgenden Screenshot veranschaulicht:

 [![](touch-in-ios-images/image2.png "Multitouch-Aktivierung im IOS-Designer")](touch-in-ios-images/image2.png#lightbox)

Es ist auch möglich, Multitouch Programm gesteuert festzulegen, indem Sie `MultipleTouchEnabled` die-Eigenschaft wie in der folgenden Codezeile gezeigt festlegen:

```csharp
imgTouchMe.MultipleTouchEnabled = true;
```

Verwenden Sie die `Count` -Eigenschaft für die `UITouch` -Eigenschaft, um zu bestimmen, wie viele Finger den Bildschirm berührt haben:

```csharp
public override void TouchesBegan (NSSet touches, UIEvent evt)
{
    base.TouchesBegan (touches, evt);
    lblNumberOfFingers.Text = "Number of fingers: " + touches.Count.ToString();
}
```

## <a name="determining-touch-location"></a>Bestimmen des Berührungs Orts

Die- `UITouch.LocationInView` Methode gibt ein CGPoint-Objekt zurück, das die Koordinaten der Fingereingabe innerhalb einer angegebenen Ansicht enthält. Außerdem können wir testen, ob sich diese Position innerhalb eines Steuer Elements befindet, indem Sie die `Frame.Contains`-Methode aufrufen. Der folgende Code Ausschnitt zeigt ein Beispiel für Folgendes:

```csharp
if (this.imgTouchMe.Frame.Contains (touch.LocationInView (this.View)))
{
    // the touch event happened inside the UIView imgTouchMe.
}
```

Nachdem wir uns mit den touchereignissen in ios vertraut machen, wollen wir uns nun über Gesten erkenungen informieren.

## <a name="gesture-recognizers"></a>Gesten Erkennungs Tools

Gesten erkenungen können den Programmieraufwand für die Unterstützung von toucheingaben in einer Anwendung erheblich vereinfachen und reduzieren. IOS-Gesten erkenungen aggregieren eine Reihe von touchereignissen in ein einzelnes touchereignis.

Xamarin. IOS stellt die- `UIGestureRecognizer` Klasse als Basisklasse für die folgenden integrierten Gesten Erkennungs Tools bereit:

- *Uitapgesturerecognizer* – Dies ist für einen oder mehrere tippen.
- *Uipinchgesturerecognizer* – fixieren und Verteilen von Fingern.
- *Uipangesturerecognizer* – schwenken oder ziehen.
- *Uiswipeer GestureRecognizer* – Schwenken in beliebiger Richtung.
- *Uirotationgesturerecognizer* – Drehen von zwei Fingern in einer Bewegung im Uhrzeigersinn oder gegen den Uhrzeigersinn.
- *Uilongpressgesturerecognizer* – drücken und halten, manchmal auch als Long-Press oder Long-Click bezeichnet.


Das grundlegende Muster für die Verwendung eines Gesten Erkennungs Moduls lautet wie folgt:

1. **Instanziieren Sie die Gestenerkennung** – zuerst instanziieren Sie eine `UIGestureRecognizer` Unterklasse. Das Objekt, das instanziiert wird, wird durch eine Sicht verknüpft, und die Garbage Collection wird durchgeführt, wenn die Sicht verworfen wird. Es ist nicht erforderlich, diese Sicht als Variable auf Klassenebene zu erstellen.
1. **Konfigurieren Sie alle Gesten Einstellungen** – der nächste Schritt besteht darin, die Gestenerkennung zu konfigurieren. Eine Liste der Eigenschaften, die fest `UIGestureRecognizer` gelegt werden können, um das Verhalten einer `UIGestureRecognizer` -Instanz zu steuern, finden Sie in der Dokumentation zu xamarin und den zugehörigen Unterklassen.
1. **Konfigurieren des Ziel** – aufgrund seines Ziels C-Erbes werden von xamarin. IOS keine Ereignisse erhoben, wenn eine Gestenerkennung mit einer Geste übereinstimmt.  `UIGestureRecognizer`verfügt über eine Methode `AddTarget` – –, die einen anonymen Delegaten oder einen Ziel-C-Selektor mit dem Code akzeptieren kann, der ausgeführt werden soll, wenn die Gestenerkennung eine Entsprechung trifft.
1. **Aktivieren** der Gestenerkennung – genau wie bei touchereignissen werden Gesten nur erkannt, wenn Berührungs Interaktionen aktiviert sind.
1. **Fügen Sie der Ansicht die Gestenerkennung hinzu** – der letzte Schritt besteht darin, die Geste einer Ansicht hinzuzufügen, `View.AddGestureRecognizer` indem Sie aufrufen und ihr ein Gesten Erkennungs Objekt übergeben.

Weitere Informationen zur Implementierung in Code finden Sie in den [Beispielen zur Gestenerkennung](~/ios/app-fundamentals/touch/ios-touch-walkthrough.md#Gesture_Recognizer_Samples) .

Wenn das Ziel der Geste aufgerufen wird, wird ein Verweis auf die aufgetretene Geste an Sie übermittelt. Dies ermöglicht dem Gesten Ziel das Abrufen von Informationen über die aufgetretene Geste. Der Umfang der verfügbaren Informationen richtet sich nach dem Typ der verwendeten Gestenerkennung. Informationen zu den Daten, die für jede `UIGestureRecognizer` Unterklasse verfügbar sind, finden Sie in der Dokumentation zu xamarin.

Beachten Sie, dass die Ansicht (und alle darunter liegenden Ansichten) keine Berührungs Ereignisse empfangen, sobald einer Ansicht eine Gestenerkennung hinzugefügt wurde. Um Berührungs Ereignisse gleichzeitig mit Gesten zuzulassen, `CancelsTouchesInView` muss die-Eigenschaft auf false festgelegt werden, wie im folgenden Code veranschaulicht:

```csharp
_tapGesture.Recognizer.CancelsTouchesInView = false;
```

Jede `UIGestureRecognizer` verfügt über eine State-Eigenschaft, die wichtige Informationen über den Status der Gestenerkennung bereitstellt. Jedes Mal, wenn sich der Wert dieser Eigenschaft ändert, ruft IOS die Abonnierung der Methode auf, um ein Update zu erhalten. Wenn eine benutzerdefinierte Gestenerkennung nie die State-Eigenschaft aktualisiert, wird der Abonnent nie aufgerufen, sodass die Gestenerkennung nutzlos ist.

Gesten können als einer von zwei Typen zusammengefasst werden:

1. *Diskret* – diese Gesten werden nur ausgelöst, wenn Sie zum ersten Mal erkannt werden.
1. *Kontinuierlich* – diese Gesten werden weiterhin ausgelöst, solange Sie erkannt werden.


Gesten Erkennungs Tools sind in einem der folgenden Zustände vorhanden:

- *Möglich* – Dies ist der anfängliche Status aller Gesten Erkennungs Modul. Dies ist der Standardwert der State-Eigenschaft.
- *Gestartet* – wenn eine fortlaufende Geste zuerst erkannt wird, wird der Zustand auf "gestartet" festgelegt. Dadurch können abonniert werden, wann die Gestenerkennung beginnt und wann Sie geändert wird.
- *Geändert* – nachdem eine fortlaufende Geste begonnen hat, aber noch nicht abgeschlossen wurde, wird der Status jedes Mal geändert, wenn sich eine Fingereingabe bewegt oder geändert wird, solange Sie sich noch innerhalb der erwarteten Parameter der Bewegung befindet.
- *Abgebrochen* – dieser Status wird festgelegt, wenn die Erkennung von gestartet wurde, und die Berührungen so geändert wurden, dass Sie nicht mehr dem Muster der Geste entsprechen.
- *Erkannt* – der Status wird festgelegt, wenn die Gestenerkennung mit einem Satz von Berührungen übereinstimmt, und der Abonnent informiert, dass die Geste abgeschlossen wurde.
- *Beendet* – Dies ist ein Alias für den erkannten Zustand.
- Fehler – wenn die Gestenerkennung nicht mehr mit den Berührungen identisch ist, die Sie überwacht, wird der Status in *"failed"* geändert.


Xamarin. IOS stellt diese Werte in der `UIGestureRecognizerState` -Enumeration dar.

## <a name="working-with-multiple-gestures"></a>Arbeiten mit mehreren Gesten

Standardmäßig lässt IOS nicht zu, dass Standard Gesten gleichzeitig ausgeführt werden. Stattdessen empfängt jede Gestenerkennung touchereignisse in einer nicht deterministischen Reihenfolge. Der folgende Code Ausschnitt veranschaulicht, wie Sie eine Gestenerkennung gleichzeitig ausführen können:

```csharp
gesture.ShouldRecognizeSimultaneously += (UIGestureRecognizer r) => { return true; };
```

Es ist auch möglich, eine Geste in IOS zu deaktivieren. Es gibt zwei delegateigenschaften, die einer Gestenerkennung ermöglichen, den Zustand einer Anwendung und der aktuellen touchereignisse zu untersuchen, um Entscheidungen darüber zu treffen, wie und ob eine Geste erkannt werden sollte. Die beiden Ereignisse lauten:

1. *Rechdreceivetouch* – dieser Delegat wird aufgerufen, unmittelbar bevor die Gestenerkennung an ein Touch-Ereignis geleitet wird, und bietet die Möglichkeit, die Berührungen zu untersuchen und zu entscheiden, welche Berührungen von der Gestenerkennung behandelt werden.
1. *Schulter* – Dies wird aufgerufen, wenn eine Erkennung versucht, den Zustand von einem möglichen in einen anderen Zustand zu ändern. Wenn Sie false zurückgeben, erzwingen Sie, dass der Status der Gestenerkennung in "failed" geändert wird.


Sie können diese Methoden mit einem stark typisierten `UIGestureRecognizerDelegate`, einem schwachen Delegaten oder einer Bindung über die Ereignishandlersyntax überschreiben, wie im folgenden Code Ausschnitt veranschaulicht:

```csharp
gesture.ShouldReceiveTouch += (UIGestureRecognizer r, UITouch t) => { return true; };
```

Schließlich ist es möglich, eine Gestenerkennung in eine Warteschlange zu stellen, sodass Sie nur erfolgreich ist, wenn eine andere Gestenerkennung fehlschlägt. So sollte z. b. eine einzelne Tap-Gestenerkennung nur erfolgreich ausgeführt werden, wenn eine Doppel tippen-Gestenerkennung fehlschlägt. Der folgende Code Ausschnitt enthält ein Beispiel für Folgendes:

```csharp
singleTapGesture.RequireGestureRecognizerToFail(doubleTapGesture);
```

## <a name="creating-a-custom-gesture"></a>Erstellen einer benutzerdefinierten Geste

Obwohl IOS einige Standard Gesten Erkennungs Tools bereitstellt, kann es erforderlich sein, in bestimmten Fällen benutzerdefinierte Gesten Erkennungs Tools zu erstellen. Das Erstellen einer benutzerdefinierten Gestenerkennung umfasst die folgenden Schritte:

1. Unterklasse `UIGestureRecognizer` .
1. Überschreiben Sie die entsprechenden Berührungs Ereignis Methoden.
1. Blasen Sie den Erkennungs Status mithilfe der "State"-Eigenschaft der Basisklasse.


Ein praktisches Beispiel hierfür finden Sie unter Exemplarische Vorgehensweise: [Verwenden von Touchscreen in ios](ios-touch-walkthrough.md) .
