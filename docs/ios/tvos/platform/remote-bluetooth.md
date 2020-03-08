---
title: Siri-Remote-und Bluetooth-Controller für tvos in xamarin
description: In diesem Artikel wird beschrieben, wie Sie in tvos-apps, die mit xamarin geschrieben wurden, mit den Remote-und Bluetooth-spielen von Siri arbeiten
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: c0338fce694d61dc19484c56dbc00bb854d0d0d7
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915790"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Siri-Remote-und Bluetooth-Controller für tvos in xamarin

Benutzer Ihrer xamarin. tvos-App interagieren nicht direkt mit ihrer Schnittstelle wie IOS, wo Sie auf Bilder auf dem Bildschirm des Geräts tippen, aber indirekt über den Raum mithilfe von [Siri Remote](#The-Siri-Remote).

Wenn Ihre APP ein Spiel ist, können Sie optional auch Unterstützung für Drittanbieter erstellen, die für IOS- [Spiele Controller](#Bluetooth-Game-Controllers) (MFI) in ihrer App erstellt wurden.

[![](remote-bluetooth-images/intro01.png "The Bluetooth Remote and Game Controller")](remote-bluetooth-images/intro01.png#lightbox)

In diesem Artikel werden die Remote-Schaltflächen " [Siri](#The-Siri-Remote)", " [touchsurface](#Touch-Surface-Gestures) " und " [Siri](#Siri-Remote-Buttons) " beschrieben, und es wird gezeigt, wie Sie mit Ihnen über [Gesten und Storyboards](#Gestures-and-Storyboards), [Gesten und Code](#Gestures-and-Code) und eine [Ereignis Behandlung auf niedriger Ebene](#Low-Level-Event-Handling)arbeiten Schließlich wird die [Arbeit mit Spiel Controllern](#Working-with-Game-Controllers) in einer xamarin. tvos-App erläutert.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Die Siri-Remote

Die Hauptmethode, mit der Benutzer mit dem Apple TV und ihrer xamarin. tvos-App interagieren, erfolgt über die enthaltene Siri-Remote. Apple hat den Remote Computer so entworfen, dass der Abstand zwischen dem Benutzer, der sich auf der Couch befindet, und der Benutzeroberfläche von Apple TV über den Raum auf dem Fernsehbildschirm angezeigt wird.

Die Herausforderung als tvos-App-Entwickler besteht darin, eine schnelle, benutzerfreundliche und visuell überzeugende Benutzeroberfläche zu erstellen, die die Berührungs Oberfläche von Siri, den Beschleunigungsmesser, das Gyroskop und Schaltflächen nutzt.

[![](remote-bluetooth-images/remote01.png "The Siri Remote")](remote-bluetooth-images/remote01.png#lightbox)

Der Siri-Remote Computer verfügt über folgende Features und erwartete Verwendungen in ihrer tvos-App:

|Funktion|Allgemeine App-Nutzung|Spiele-App-Verwendung|
|---|---|---|
|**Berührungs Oberfläche**<br />Wischen Sie, um zu navigieren, und drücken Sie, um Kontextmenüs auszuwählen und zu halten.|**Tippen/schwenken**<br />UI-Navigation zwischen fokussierbaren Elementen.<br /><br />**Mausaktion**<br />Aktiviert das ausgewählte (im Fokus) Element.|**Tippen/schwenken**<br />Hängt vom Spiel Entwurf ab und kann als D-Pad durch Tippen auf die Ränder verwendet werden.<br /><br />**Mausaktion**<br />Ausführen der primären Schaltflächen Funktion.|
|**Menü**<br />Drücken Sie, um zum vorherigen Bildschirm oder Menü zurückzukehren.|Kehrt zum vorherigen Bildschirm zurück und beendet den Apple TV-Startbildschirm über den Hauptbildschirm der app.|Anhalten und Fortsetzen des Spiels, zurück zum vorherigen Bildschirm und Beenden des Apple TV-Startbildschirms über den Bildschirm der Haupt-app.|
|**Siri/Suche**<br />Klicken Sie in Ländern mit Siri auf Sprachsteuerung, und in allen anderen Ländern wird Suchbildschirm angezeigt.|–|–|
|**Wiedergabe/Pause**<br />Wiedergabe und Pause von Medien oder Bereitstellen einer sekundären Funktion in-apps.|Startet die Medienwiedergabe und hält die Wiedergabe wieder auf.|Führt eine sekundäre Schaltflächen Funktion aus oder überspringt das Intro-Video (sofern vorhanden).|
|**Startseite**<br />Drücken Sie, um zum Startbildschirm zurückzukehren, doppelklicken Sie, um ausgestellte apps anzuzeigen, und halten Sie das Gerät gedrückt.|–|–|
|**Umfang**<br />Steuert das Volume für angefügte audiovolumes und Videogeräte.|–|–|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Touchoberflächen Gesten

Die Touch-Oberfläche von Siri ist in der Lage, eine Vielzahl von Einzel Fingergesten zu erkennen, auf die Sie in ihrer xamarin. tvos-App reagieren können:

|Wischen|Klicken Sie auf|Tippen|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|Verschiebt die Auswahl (Fokus) zwischen den Benutzeroberflächen Elementen auf dem Bildschirm (nach oben, nach links, rechts). Mithilfe von Striping können Sie mithilfe von Trägheit schnell durch große Inhaltslisten scrollen.|Aktiviert das ausgewählte (im Fokus) Element oder verhält sich wie die primäre Schaltfläche in einem Spiel. Durch Klicken und halten können Kontextmenüs oder sekundäre Funktionen aktiviert werden.|Das leichte Tippen auf die Berührungs Oberfläche der Kanten verhält sich wie direktionale Schaltflächen auf einem D-Pad, wobei der Fokus abhängig von der gekippten Fläche nach oben, unten, Links oder rechts verschoben wird. Abhängig von der APP kann verwendet werden, um ausgeblendete Steuerelemente anzuzeigen.|

Apple bietet die folgenden Vorschläge zum Arbeiten mit touchoberflächen Gesten:

- Die unter **Scheidung zwischen Klicks und tippen** auf das Klicken ist eine absichtliche Aktion durch den Benutzer und eignet sich gut für Auswahl, Aktivierung und die primäre Schaltfläche eines Spiels. Das Tippen ist geringer und sollte sparsam verwendet werden, da der Benutzer oft die Siri-Remote Verbindung hält und das Tap-Ereignis versehentlich problemlos aktivieren kann.
- **Standard Gesten nicht neu definieren** : der Benutzer hat die Erwartung, dass bestimmte Gesten bestimmte Aktionen ausführen. Sie sollten die Bedeutung oder Funktion dieser Gesten in Ihrer APP nicht neu definieren. Die einzige Ausnahme ist eine Spiele-APP während des aktiven Spiels.
- **Definieren Sie neue Gesten sparsam** . der Benutzer hat eine Erwartung, dass bestimmte Gesten bestimmte Aktionen ausführen. Sie sollten keine benutzerdefinierten Gesten definieren, um Standard Aktionen auszuführen. Spiele sind die häufigste Ausnahme, bei der benutzerdefinierte Gesten dem Spiel Spaß machen können.
- Antworten Sie ggf. auf **d-Pad** -Abzweigungen: bei einem leichten tippen an den eckrändern der Touchoberfläche wird wie ein D-Pad auf einem Spielcontroller reagiert, der den Fokus oder die Richtung nach oben, unten, Links oder rechts verschiebt. Gegebenenfalls sollten Sie diese Gesten in Ihrer APP oder Ihrem Spiel beantworten.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri Remote Schaltflächen

Zusätzlich zu Gesten auf der Touchoberfläche kann Ihre APP darauf reagieren, dass der Benutzer auf die Berührungs Oberfläche klickt oder die Schaltfläche Wiedergabe/Pause drückt. Wenn Sie über das Game Controller-Framework auf die Siri-Remote zugreifen, können Sie auch die Menü Schaltfläche erkennen, die gedrückt wird.

Außerdem können Menü Schaltflächen mit einer Gestenerkennung mit Standard `UIKit` Elementen erkannt werden. Wenn Sie die Menü Schaltfläche abfangen, die gedrückt wird, sind Sie dafür verantwortlich, die aktuelle Ansicht und den Ansichts Controller zu schließen und zur vorherigen Ansicht zurückzukehren.

> [!IMPORTANT]
> Sie sollten der Schaltfläche Wiedergabe/Pause auf der Remote-Schaltfläche **immer** eine Funktion zuweisen. Wenn Sie eine nicht funktionsfähige Schaltfläche haben, kann das Aussehen Ihrer APP für den Endbenutzer beschädigt werden. Wenn Sie nicht über eine gültige Funktion für diese Schaltfläche verfügen, weisen Sie die gleiche Funktion wie die primäre Schaltfläche (Fingereingabe Oberfläche) zu.

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Gesten und Storyboards

Die einfachste Möglichkeit, mit der Siri-Remote Verbindung in ihrer xamarin. tvos-APP zu arbeiten, besteht darin, ihren Ansichten im Schnittstellen-Designer Gesten Erkennungs Tools hinzuzufügen.

Gehen Sie folgendermaßen vor, um eine Gestenerkennung hinzuzufügen:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, und öffnen Sie Sie zum Bearbeiten des Schnittstellen-Designers.
2. Ziehen Sie eine **Tap-Gestenerkennung** aus der **Bibliothek** , und legen Sie Sie in der Ansicht ab:

    [![](remote-bluetooth-images/storyboard01.png "A Tap Gesture Recognizer")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Aktivieren **Sie im** **Schalt** Flächen Abschnitt des **Attribut Inspektors**Folgendes:

    [![](remote-bluetooth-images/storyboard02.png "Check Select")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Select** bedeutet, dass die Geste auf den Benutzer reagiert, indem er auf die **Touchoberfläche** der Siri-Remote klickt. Sie haben auch die Möglichkeit, auf die Schaltflächen **Menü**, wieder **Gabe/Pause**, nach **oben**, **nach unten**, **Links** und **Rechts** zu antworten.
5. Richten Sie als nächstes eine **Aktion** aus der **Tap-Gestenerkennung** ein, und nennen Sie Sie `TouchSurfaceClicked`:

    [![](remote-bluetooth-images/storyboard03.png "An Action from the Tap Gesture Recognizer")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück.

Bearbeiten Sie die Datei des Ansichts Controllers (z. b. `FirstViewController.cs`), und fügen Sie den folgenden Code hinzu, um die ausgelöste Bewegung

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

Weitere Informationen zum Arbeiten mit Storyboards finden Sie in unserer [Hello-, tvos-Schnellstarthandbuch](~/ios/tvos/get-started/hello-tvos.md). Insbesondere das [Erstellen der Benutzeroberfläche](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) und [das Schreiben des Codes mit den Abschnitten Outlets und Aktionen](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) .

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Gesten und Code

Optional können Sie Gesten direkt im C# Code erstellen und Sie den Ansichten in der Benutzeroberfläche hinzufügen. Wenn Sie z. b. eine Reihe von Swipe Gesten Erkennungs Tools hinzufügen möchten, bearbeiten Sie den Ansichts Controller, und fügen Sie den folgenden Code hinzu:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>Ereignis Behandlung auf niedriger Ebene

Wenn Sie einen benutzerdefinierten Typ erstellen, der auf `UIKit` in ihrer xamarin. tvos-App basiert (z. b. `UIView`), haben Sie auch die Möglichkeit, den Schaltflächen Druck auf niedriger Ebene über `UIPress`-Ereignisse bereitzustellen.

Ein `UIPress` Ereignis ist das tvos, was ein `UITouch` Ereignis für IOS ist, außer `UIPress` gibt Informationen zu Schaltflächen-drücken auf der Siri-Remote-oder anderen angeschlossenen Bluetooth-Geräte (wie z. b. ein Spiele Controller) zurück. `UIPress` Ereignisse beschreiben die gedrückte Schaltfläche und ihren Zustand (gestartet, abgebrochen, geändert oder beendet).

Bei analogen Schaltflächen auf Geräten wie Bluetooth-Spiel Controllern gibt `UIPress` auch die Menge der auf die Schaltfläche angewendeten Force zurück. Die `Type`-Eigenschaft des `UIPress` Ereignisses definiert, welche physische Schaltfläche den Zustand geändert hat, während die restlichen Eigenschaften die aufgetretene Änderung beschreiben.

Der folgende Code zeigt ein Beispiel für die Behandlung von `UIPress` Ereignissen auf niedriger Ebene für ein `UIView`:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

Wie bei `UITouch` Ereignissen sollten Sie alle vier implementieren, wenn Sie eine der `UIPress` Ereignis Überschreibungen implementieren müssen.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth-Spielcontroller

Zusätzlich zu den standardmäßigen Siri-Remote Geräten, die mit dem Apple TV ausgeliefert werden, können Drittanbieter für IOS-Spiele Controller (MFI) mit dem Apple TV gekoppelt und zum Steuern ihrer xamarin. tvos-App verwendet werden.

[![](remote-bluetooth-images/game01.png "Bluetooth Game Controllers")](remote-bluetooth-images/game01.png#lightbox)

Spiele Controller können verwendet werden, um das Spiel zu verbessern und ein Gefühl für das Eintauchen in ein Spiel zu bieten. Sie können auch verwendet werden, um die standardmäßige Apple TV-Schnittstelle zu steuern, sodass die Verwendung nicht zwischen dem Remote-und dem Controller wechseln muss.

> [!IMPORTANT]
> Bluetooth-Spiele Controller sind ein optionaler Kauf, den Endbenutzer möglicherweise treffen, Ihre APP kann den Benutzer nicht zwingen, eine solche zu erwerben. Wenn Ihre APP Game Controller unterstützt, muss Sie auch die Siri-Remote Unterstützung unterstützen, damit das Spiel von allen Apple TV-Benutzern verwendbar ist.

Ein Game Controller verfügt über die folgenden Features und erwarteten Verwendungen in ihrer tvos-App:

|Funktion|Allgemeine App-Nutzung|Spiele-App-Verwendung|
|---|---|---|
|**D-Pad**|Navigiert durch Benutzeroberflächen Elemente (ändert den Fokus).|Hängt von Spiel ab.|
|**A**|Aktiviert das ausgewählte (im Fokus) Element.|Führt die primäre Schaltflächen Funktion aus und bestätigt Dialog Aktionen.|
|**B**|Kehrt zum vorherigen Bildschirm zurück oder beendet den Startbildschirm, wenn er auf dem Hauptbildschirm der App angezeigt wird.|Führt die Funktion der sekundären Schaltfläche aus oder kehrt zum vorherigen Bildschirm zurück.|
|**X**|Startet die Medienwiedergabe oder hält die Wiedergabe an.|Hängt von Spiel ab.|
|**J**|–|Hängt von Spiel ab.|
|**Menü**|Kehrt zum vorherigen Bildschirm zurück oder beendet den Startbildschirm, wenn er auf dem Hauptbildschirm der App angezeigt wird.|Das Anhalten/Fortsetzen des Spiels wird zum vorherigen Bildschirm zurückgegeben oder auf dem Startbildschirm angezeigt, wenn es auf dem Hauptbildschirm der App angezeigt wird.|
|**Linke Schulter Schaltfläche**|Navigiert nach links.|Hängt von Spiel ab.|
|**Linker-Auslösung**|Navigiert nach links.|Hängt von Spiel ab.|
|**Schaltfläche mit rechter Schulter**|Navigiert nach rechts.|Hängt von Spiel ab.|
|**Rechter Auslösers**|Navigiert nach rechts|Hängt von Spiel ab.|
|**Linker fingerstick**|Navigiert durch Benutzeroberflächen Elemente (ändert den Fokus).|Hängt von Spiel ab.|
|**Rechter fingerstick**|–|Hängt von Spiel ab.|

Apple bietet die folgenden Vorschläge zum Arbeiten mit Spiel Controllern:

- **Spiele Controller Verbindungen bestätigen** : Ihre tvos-App kann jederzeit vom Endbenutzer gestartet und beendet werden. Sie sollten immer überprüfen, ob ein Spiele Controller bei App-Start-oder Wachzeiten vorhanden ist, und die erforderlichen Maßnahmen ergreifen.
- **Stellen Sie sicher, dass Ihre APP auf den Remote-und Spiel Controllern von Siri funktioniert** . Benutzer müssen nicht zwischen dem Siri-Remote Computer und einem Game Controller wechseln, um Ihre APP zu verwenden. Testen Sie Ihre APP häufig mit beiden Arten von Controllern, um sicherzustellen, dass alles einfach zu navigieren ist und wie erwartet funktioniert.
- **Stellen** Sie sicher, dass das zurück Drücken der Menü Schaltfläche immer zum vorherigen Bildschirm zurückkehren soll. Wenn sich der Benutzer auf dem Haupt-App-Bildschirm befindet, sollte die Menü Schaltfläche diese an den Apple TV-Startbildschirm zurückgeben. Während des Spiels sollte auf der Menü Schaltfläche eine Warnung angezeigt werden, die dem Benutzer die Möglichkeit gibt, das Spiel anzuhalten/fortzusetzen oder zum Hauptmenü zurückzukehren.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Arbeiten mit Spiel Controllern

Wie oben bereits erwähnt, kann der Benutzer neben dem standardmäßigen Siri-Remote Computer, der im Lieferumfang von Apple TV enthalten ist, optional einen Drittanbieter für IOS-Spiele Controller (MFI) anfügen und verwenden, um die xamarin. tvos-APP zu steuern.

Wenn Ihre APP auf niedriger Ebene Controller Eingaben benötigt, können Sie das [Spiele Controller-Framework](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) von Apple verwenden, das die folgenden Änderungen für tvos hat:

- Das Micro Game Controller-Profil (`GCMicroGamepad`) wurde hinzugefügt, um die Siri-Remote Version als Ziel festzustellen.
- Die neue `GCEventViewController`-Klasse kann verwendet werden, um Spiele Controller Ereignisse über Ihre APP weiterzuleiten. Weitere Informationen finden Sie weiter unten im Abschnitt [bestimmen der Spiele Controller Eingabe](#determining-game-controller-input) .

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Support Anforderungen für Game Controller

Apple hat mehrere spezielle Anforderungen, die erfüllt sein müssen, wenn Ihre xamarin. tvos-App Game Controller unterstützt:

- **Sie müssen die Siri-Remote Unterstützung unter** stützen, da Sie immer die Siri-Remote Unterstützung unterstützen müssen. Ihr Spiel kann nicht verlangen, dass ein Drittanbieter Spiel-Controller Wiedergabe fähig ist.
- **Sie müssen das Layout für erweiterte Steuerelemente unterstützen** . alle tvos-Spiele Controller sind nicht formpassend und erweiterte Controller.
- **Spiele müssen mit eigenständigen Controllern** Wiedergabe fähig sein. Wenn Ihre APP einen erweiterten Spiele Controller unterstützt, muss Sie nur mit dem Spielcontroller Wiedergabe fähig sein.
- **Sie müssen die Schaltfläche Wiedergabe/Pause unterstützen** : Wenn der Benutzer die Schaltfläche Wiedergabe/Pause drückt, sollte eine Warnung angezeigt werden, die dem Benutzer die Möglichkeit gibt, das Spiel anzuhalten/fortzusetzen oder zum Hauptmenü zurückzukehren.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Aktivieren der Game Controller-Unterstützung

Um die Spiele Controller Unterstützung in ihrer xamarin. tvos-APP zu aktivieren, doppelklicken Sie auf die `Info.plist` Datei im **Projektmappen-Explorer** , um Sie für die Bearbeitung zu öffnen:

[![](remote-bluetooth-images/game02.png "The Info.plist editor")](remote-bluetooth-images/game02.png#lightbox)

Platzieren Sie im Abschnitt **Game Controller** eine Prüfung, indem Sie **Game Controller aktivieren**, und überprüfen Sie dann alle Spiele Controller Typen, die von der App unterstützt werden.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Verwenden von Siri Remote als Spiel Controller

Die Siri-Remote Verbindung mit Apple TV kann als eingeschränkter Spiel Controller verwendet werden. Wie bei anderen Spiel Controllern wird es im Game Controller-Framework als `GCController` Objekt angezeigt und unterstützt sowohl das `GCMotion` als auch das `GCMicroGamepad` Profile.

Bei Verwendung als Spiel Controller hat die Siri-Remote die folgenden Eigenschaften:

- Die Berührungs Oberfläche kann als ein D-Pad verwendet werden, das analoge Eingabedaten bereitstellt.
- Die Remote-kann entweder in einer hoch-oder Querformat verwendet werden, und Ihre APP entscheidet, ob das Profil Objekt Eingabedaten automatisch kippen soll.
- Das Klicken auf die Berührungs Oberfläche verhält sich wie das Drücken der Schaltfläche **a** auf einem Spiel Controller.
- Die Schaltfläche Wiedergabe/Pause verhält sich wie Button **X** auf einem Spiel Controller.
- Die Menü Schaltfläche sollte eine Warnung anzeigen, die dem Benutzer die Möglichkeit gibt, das Spiel anzuhalten/fortzusetzen oder zum Hauptmenü zurückzukehren.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Bestimmen der Spiel Controller Eingabe

Anders als bei IOS, bei dem Game Controller-Ereignisse parallel mit Berührungs Ereignissen empfangen werden können, verarbeitet tvos alle Ereignisse auf niedriger Ebene, um `UIKit` Ereignisse auf hoher Ebene zu liefern. Wenn Sie Zugriff auf die Low-Level-Spiele Controller Ereignisse benötigen, müssen Sie daher das Standardverhalten `UIKit`deaktivieren.

Wenn Sie für tvos eine direkte Verarbeitung von Game Controller Eingaben durchführen möchten, müssen Sie eine `GCEventViewController` (oder eine Unterklasse) verwenden, um die Benutzeroberfläche des Spiels anzuzeigen. Wenn ein `GCEventViewController` der *erste Responder*ist, wird die Spiele Controller Eingabe aufgezeichnet und über das Game Controller-Framework an Ihre APP übermittelt.

Sie können die `UserInteractionEnabled`-Eigenschaft der `GCEventViewController`-Klasse verwenden, um die Art und Weise zu ändern, wie Ereignisse verarbeitet und behandelt werden.

Weitere Informationen zum Implementieren von Game Controller-Unterstützung finden Sie im Abschnitt " [Arbeiten mit Spiel Controllern](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) von Apple" im Leitfaden zur [App-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) und [Game Controller Programming Guide](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel behandelt den neuen Siri-Remote Computer, der im Lieferumfang von Apple TV, touchoberflächen Gesten und Siri-Remote Schaltflächen enthalten ist. Im nächsten Schritt wird die Arbeit mit Gesten und Storyboards, Gesten und Code und Low-Level-Ereignissen behandelt. Abschließend wird erläutert, wie Sie mit Spiel Controllern arbeiten.

## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
