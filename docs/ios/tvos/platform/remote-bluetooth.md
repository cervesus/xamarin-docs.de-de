---
title: Siri Remote- und Bluetooth-Controller für TvOS in Xamarin
description: In diesem Artikel wird beschrieben, wie zum Arbeiten mit der Siri Remote- und Bluetooth-Gamecontroller in TvOS-apps mit Xamarin geschrieben wird.
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 14c62051afd7489389f154c21b3a76b9aad3f32e
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50115535"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Siri Remote- und Bluetooth-Controller für TvOS in Xamarin

Benutzer Ihrer Xamarin.tvOS-app werden nicht mit seinem-Schnittstelle direkt als mit iOS, in dem sie-Images auf den Bildschirm des Geräts, jedoch indirekt von über den Raum mithilfe Tap, interagiert werden die [Siri Remote](#The-Siri-Remote).

Wenn Ihre app ein Spiel ist, können Sie optional erstellen in der Unterstützung für 3rd Party, gemacht für iOS (MFI) [Bluetooth Gamecontroller](#Bluetooth-Game-Controllers) in Ihrer app ebenfalls.

[![](remote-bluetooth-images/intro01.png "Die Bluetooth-Remote und Gamecontroller")](remote-bluetooth-images/intro01.png#lightbox)

In diesem Artikel wird beschrieben, die [Siri Remote](#The-Siri-Remote), [Surface-Touch-Gesten](#Touch-Surface-Gestures) und [Siri Remote Schaltflächen](#Siri-Remote-Buttons) und zeigt, wie Arbeiten sie über [Gesten und Storyboards](#Gestures-and-Storyboards), [Gesten und Code](#Gestures-and-Code) und [Low-Level-Ereignisbehandlung](#Low-Level-Event-Handling). Außerdem wird es [arbeiten mit Gamecontroller](#Working-with-Game-Controllers) in einer Xamarin.tvOS-app.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri-Remoteinstanz

Die wichtigste Methode, die Benutzer mit der Apple-TV und Ihre Xamarin.tvOS-app zu tun haben werden, erfolgt über Siri Remote enthalten. Apple entwickelt, die Remote zum überbrücken des Abstand zwischen dem Benutzer, die auf dem Sofa, und die Apple TV-Benutzeroberfläche, die durch das Zimmer auf dem TV-Bildschirm angezeigt.

Die Herausforderung dabei als ein TvOS-app-Entwickler ist das Erstellen einer schnell, einfach zu verwenden und visuell ansprechende Benutzeroberfläche, die die Siri Remotes Touch-Oberfläche, Beschleunigungsmesser, Gyroskop und Schaltflächen nutzt.

[![](remote-bluetooth-images/remote01.png "Siri-Remoteinstanz")](remote-bluetooth-images/remote01.png#lightbox)

Siri Remote verfügt über die folgenden Features und die erwartete Verwendung innerhalb Ihrer TvOS-app:

|Funktion|Allgemeine App-Nutzung|Spiele-App-Nutzung|
|---|---|---|
|**Touch-Oberfläche**<br />Streichen Sie nach, um zu navigieren, drücken Sie die EINGABETASTE, um auszuwählen, und speichern für Kontextmenüs.|**Tippen Sie auf/Wischen**<br />UI-Navigation zwischen den Fokus erhalten kann.<br /><br />**Klicken Sie auf**<br />Aktiviert die ausgewählte (bildschärfenmodus)-Element.|**Tippen Sie auf/Wischen**<br />Hängt Spiel Entwurf und kann als ein Steuerkreuz durch Tippen auf die Ränder verwendet werden.<br /><br />**Klicken Sie auf**<br />Führen Sie die Funktion der primären Schaltfläche.|
|**Menü**<br />Drücken Sie EINGABETASTE, um zum vorherigen Bildschirm oder im Menü zurückzukehren.|Kehrt zum vorherigen Bildschirm zurück und beendet die Haupt-app-Bildschirm auf Apple TV-Startbildschirm.|Anhalten und Fortsetzen von Gaming, kehrt zur vorherigen Bildschirm zurück und wird beendet, Apple TV-Startbildschirm aus dem Haupt-app-Bildschirm.|
|**Siri/Search**<br />Halten Sie in Ländern mit Siri und für Voice-Steuerelement, in allen anderen Ländern, zeigt Suchbildschirm.|n/v|n/v|
|**Abspielen/Pause**<br />Wiedergeben, Anhalten von Medien und bietet eine sekundäre Funktion in apps.|Startet die Wiedergabe von Medien und Wiedergabe anhalten/fortsetzen.|Führt die Funktion der sekundären Schaltfläche, oder überspringt Sie Einführungsvideos zu (falls vorhanden).|
|**Home**<br />Drücken Sie, um zur Startseite zurück, und doppelklicken Sie zum Ausführen von apps anzuzeigen, halten Sie Geräte in den Standbymodus.|n/v|n/v|
|**Volume**<br />Steuert, angefügte Volume von Audio-/Videogeräte.|n/v|n/v|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Oberfläche Touch-Gesten

Die Siri Remotes Touch-Oberfläche kann eine Vielzahl von Single-Finger-Bewegungen zu erkennen, die Sie in Ihrer app Xamarin.tvOS reagieren können:

|Wischen|Klicken|Tippen Sie auf|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|Auswahl (Fokus) zwischen UI-Elemente auf dem Bildschirm verschoben (nach oben, unten links, rechts). Ein Lesegerät kann verwendet werden, einen Bildlauf durch große Listen von Inhalten schnell mit Trägheit.|Das ausgewählte (bildschärfenmodus)-Element aktiviert oder verhält sich wie die primäre Maustaste in einem Spiel. Durch Klicken auf, und speichern können Kontextmenüs oder sekundären Funktionen aktivieren.|Geringfügig auf die Touch-Oberfläche, auf die Ränder verhält sich wie die entsprechenden Schaltflächen auf einer Steuerkreuz, verschieben den Fokus nach oben, unten, links oder rechts je nach Bereich getippt. Abhängig von der app kann verwendet werden, um ausgeblendete Steuerelemente anzuzeigen.|

Apple bietet die folgenden Vorschläge für die Arbeit mit Oberfläche Touch-Gesten:

* **Unterscheiden zwischen Klicks und Taps** -ist eine Aktion, die vom Benutzer beabsichtigt und eignet sich gut für die Auswahl, Aktivierung und die primäre Maustaste eines Spiels auf. Tippen ist weniger offensichtlich und sollten sparsam eingesetzt werden, da der Benutzer die Siri Remote in ihre Westentasche häufig gedrückt halten ist und kann eine Tap-Ereignis versehentlich ganz einfach aktivieren.
* **Nicht standardmäßige Gesten neu definieren** – der Benutzer hat eine Erwartung, dass der spezifische Bewegungen bestimmte Aktionen ausführen, werden Sie sollte nicht neu definieren, der Bedeutung oder der Funktion für diese Gesten in Ihrer app. Die einzige Ausnahme ist eine Spiele-app während der aktiven Spiel.
* **Definieren Sie neue Gesten sparsam** – in diesem Fall der Benutzer hat eine Erwartung, dass bestimmte Gesten bestimmte Aktionen ausgeführt werden. Sie sollten vermeiden, Definieren von benutzerdefinierten stiftbewegungen um standard Aktionen auszuführen. Und in diesem Fall Spiele sind die häufigste Ursache Ausnahme in dem benutzerdefinierte stiftbewegungen interessante und faszinierende Play dem Spiel hinzufügen können.
* **Falls zutreffend, reagieren auf Steuerkreuz tippt** – in der Ecke Kanten der Fläche Fingereingabe reagieren, wie ein Steuerkreuz auf einem gleitenden Fokus Gamecontroller oder Richtung nach oben, unten, links oder rechts werden nur wenig tippen. Gegebenenfalls sollten Sie auf diese Aktionen in Ihren App- oder Spiele reagieren.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri-Remote-Schaltflächen

Zusätzlich zu den Aktionen auf der Oberfläche Touch kann Ihre app für den Benutzer durch Klicken auf die Touch-Oberfläche, oder drücken die Schaltfläche "Abspielen/Pause" reagieren. Wenn Sie die Siri Remote mit dem Game Controller-Framework zugreifen, erkennen Sie auch die Menü-Schaltfläche gedrückt wird. 

Darüber hinaus Menü betätigen können erkannt werden mithilfe einer Stiftbewegungs-Erkennung mit Standard `UIKit` Elemente. Wenn Sie die Schaltfläche gedrückt wird abzufangen, Sie verantwortlich für das Schließen der aktuellen Ansicht und View Controller und zurück zu dem vorherigen Beispiel.

> [!IMPORTANT]
> Sie sollten **immer** weisen Sie eine Funktion auf die Schaltfläche "Abspielen/Pause", auf dem Remotecomputer. Müssen eine nicht funktionierende Schaltfläche können Sie Ihre app unterbrochen, um den Endbenutzer aussehen vornehmen. Wenn Sie eine gültige Funktion für diese Schaltfläche nicht haben, weisen Sie die gleiche Funktion wie die primäre Maustaste (Touch-Oberfläche auf klicken).

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Gesten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit dem Remoterepository Siri in Ihrer app Xamarin.tvOS ist Geste Erkennungen an die Ansichten im Benutzeroberflächen-Designer hinzufügen.

Um eine Erkennung Gesten hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` und öffnen Sie sie für die Bearbeitung von Benutzeroberflächen-Designer.
2. Ziehen Sie eine **Stiftbewegungs-Erkennung tippen** aus der **Bibliothek** und legen Sie sie in der Ansicht: 

    [![](remote-bluetooth-images/storyboard01.png "Eine Tap-Stiftbewegungs-Erkennung")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Überprüfen Sie **wählen** in die **Schaltfläche** Teil der **Attributinspektor**: 

    [![](remote-bluetooth-images/storyboard02.png "Überprüfen Sie die Option")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Wählen Sie** bedeutet, dass die Aktion reagiert auf die Benutzer auf die **Touch Oberfläche** auf dem Remotecomputer Siri. Sie haben auch die Möglichkeit zur Reaktion auf die **Menü**, **abspielen/Pause**, **einrichten**, **nach unten**, **Links** und **Rechts** Schaltflächen.
5. Als Nächstes socialsharing.Share ein **Aktion** aus der **Stiftbewegungs-Erkennung tippen** und nennen Sie sie `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "Eine Aktion aus der Gestenerkennung Tap")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac.

Bearbeiten Sie Ihr View-Controller (Beispiel `FirstViewController.cs`)-Datei und fügen Sie den folgenden Code, um die Aktion, die ausgelöst wird, und zu verarbeiten:

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

Weitere Informationen zum Arbeiten mit Storyboards, informieren Sie sich unsere [Hello, TvOS – Kurzanleitung](~/ios/tvos/get-started/hello-tvos.md). Insbesondere die [Erstellen der Benutzeroberfläche](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) und [Schreiben von Code mit Outlets und Aktionen](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) Abschnitte.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Gesten und Code

Sie können erstellen Sie optional Bewegungen direkt in C# code und Ansichten in der Benutzeroberfläche hinzufügen. Beispielsweise um eine Reihe von Wischen Geste Erkennungen hinzuzufügen, bearbeiten Sie Ihr View-Controller, und fügen Sie den folgenden Code hinzu:

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

## <a name="low-level-event-handling"></a>Low-Level-Ereignisbehandlung

Bei der Erstellung eines benutzerdefinierten Typs basierend auf `UIKit` in Ihrer Xamarin.tvOS-app (z. B. `UIView`), haben auch die Möglichkeit, Low-Level Verarbeitung von Tastendruck über bereitstellen `UIPress` Ereignisse. 

Ein `UIPress` Ereignis tvos Was ist eine `UITouch` Ereignis ist für iOS, mit Ausnahme von `UIPress` gibt Informationen über Schaltfläche drückt, auf dem Remotecomputer Siri oder anderen angeschlossenen Bluetooth-Geräten (z. B. ein Game Controller) zurück. `UIPress` Ereignisse werden die Schaltfläche gedrückt wird und den Zustand ("Began", "abgebrochen", "Changed" oder "beendet") beschrieben. 

Für analoge Schaltflächen auf Geräten wie Gamecontroller Bluetooth `UIPress` auch wird die Größe des erzwingen, die auf die Schaltfläche angewendet wird. Die `Type` Eigenschaft der `UIPress` Ereignis definiert, welche Taste hat, geänderten Zustand, während sich der Rest der Eigenschaften die Änderung zu beschreiben, die aufgetreten sind.

Der folgende Code zeigt ein Beispiel Behandlung auf niedriger Ebene `UIPress` Ereignisse für eine `UIView`:

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

Wie bei `UITouch` Ereignisse, wenn Sie implementieren müssen die `UIPress` Außerkraftsetzungen von Ereignissen, sollten Sie alle vier implementieren.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth-Gamecontroller

Zusätzlich zur standardmäßigen Siri Remoteinstanz, die im Lieferumfang von den Apple TV, 3rd Party vorgenommen für iOS können zur Steuerung der Xamarin.tvOS-app (MFI) Bluetooth-Gamecontroller Apple TV zugeordnet und verwendet werden.

[![](remote-bluetooth-images/game01.png "Bluetooth-Gamecontroller")](remote-bluetooth-images/game01.png#lightbox)

Gamecontroller können verwendet werden, verbessern Spiele, und geben Sie einen Eindruck davon zu Entwicklungsthemen in einem Spiel. Sie können auch verwendet werden, um die Standardschnittstelle für Apple TV zu steuern, sodass die Verwendung nicht zum Wechseln zwischen dem Remotecomputer und der Controller.

> [!IMPORTANT]
> Bluetooth-Gamecontroller sind eine optionale erwerben, die Endbenutzer machen können, Ihre app kann nicht zwingen den Benutzer, eine erwerben. Wenn Ihre app Gamecontroller unterstützt muss auch die Siri Remote unterstützt werden, damit das Spiel von allen Benutzern von Apple TV verwendbar ist.

Ein Game Controller verfügt über die folgenden Features und die erwartete Verwendung innerhalb Ihrer TvOS-app:

|Funktion|Allgemeine App-Nutzung|Spiele-App-Nutzung|
|---|---|---|
|**D-Pad**|Durch die Elemente der Benutzeroberfläche (den Fokus) navigiert.|Hängt von dem Spiel.|
|**A**|Aktiviert das ausgewählte (bildschärfenmodus)-Element.|Führt die Funktion der primären Schaltfläche und Dialogfeldaktionen bestätigt.|
|**B**|Kehrt zum vorherigen Bildschirm zurück, oder klicken Sie auf der Startseite wird beendet, wenn es sich um die app Hauptbildschirm.|Führt die Funktion der sekundären Schaltfläche, oder gibt Sie zurück zum vorherigen Bildschirm.|
|**X**|Startet die Wiedergabe von Medien oder anhaltevorgängen und Fortsetzungen Wiedergabe.|Hängt von dem Spiel.|
|**Y**|n/v|Hängt von dem Spiel.|
|**Menü**|Kehrt zum vorherigen Bildschirm zurück, oder klicken Sie auf der Startseite wird beendet, wenn es sich um die app Hauptbildschirm.|Fortsetzen Sie anhalten/Gaming, kehrt zur vorherigen Bildschirm zurück oder beendet wird, klicken Sie auf der Startseite auf die app Hauptbildschirm.|
|**Linken Schulter-Schaltfläche**|Navigiert nach links.|Hängt von dem Spiel.|
|**Linken Trigger**|Navigiert nach links.|Hängt von dem Spiel.|
|**Schaltfläche "rechts Schulter"**|Navigiert rechts.|Hängt von dem Spiel.|
|**Richtige Trigger**|Wechselt von rechts|Hängt von dem Spiel.|
|**Linken Ministick**|Durch die Elemente der Benutzeroberfläche (den Fokus) navigiert.|Hängt von dem Spiel.|
|**Ministick rechts**|n/v|Hängt von dem Spiel.|

Apple bietet die folgenden Vorschläge für die Arbeit mit Gamecontroller:

- **Vergewissern Sie sich Game Controller Verbindungen** -TvOS-app gestartet und zu einem beliebigen Zeitpunkt vom Benutzer beendet werden kann. Sie sollten prüfen, ob das Vorhandensein einer Game Controller auf app-Start oder aktiv Zeiten und Maßnahmen ergreifen, je nach Bedarf.
- **Stellen Sie sicher Ihre App funktioniert auf Siri Remote und Gamecontroller** -nicht erfordern, dass Benutzer zwischen den Siri Remote und ein Game Controller zur Verwendung Ihrer app zu wechseln. Testen Sie Ihre app häufig mit beiden Arten von Controllern, die sicherstellen, dass alles, was einfach zu navigieren und wie erwartet funktioniert.
- **Geben Sie eine Möglichkeit wieder** -drücken die Menütaste sollte immer zum vorherigen Bildschirm zurück. Wenn der Benutzer im Haupt-app-Bildschirm ist, muss die Menüschaltfläche sie an das Apple TV-Startbildschirm zurückgeben. Während der Spiele sollte die Menüschaltfläche angezeigt werden eine Warnung so erhält der Benutzer die Möglichkeit zum Anhalten/Fortsetzen Spiel oder kehren Sie zum Hauptmenü zurück.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Arbeiten mit Gamecontroller

Wie bereits erwähnt, zusätzlich zu den Standard, Siri Remote, die bereitgestellt wird mit der Apple TV, die der Benutzer optional angefügt werden kann, eine 3rd party, gemacht für iOS (MFI) Bluetooth-Gamecontroller und zur Steuerung der Xamarin.tvOS-app verwenden.

Wenn Ihre app die Low-Level-Controller-Eingabe erforderlich, können Sie von Apple verwendet [Game Controller-Framework](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) IValidator.h für tvos verwendet. die folgenden Änderungen:

- Das controllerprofil der Micro-Spiel (`GCMicroGamepad`) Siri Remote Ziel hinzugefügt wurde.
- Die neue `GCEventViewController` Klasse kann verwendet werden, um Gamecontroller Ereignisse über Ihre app weitergeleitet. Finden Sie unter den [bestimmen Game Controller Eingabe](#Determining-Game-Controller-Input) Informationen weiter unten im Abschnitt.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Anforderungen für die Unterstützung von Gamecontroller

Apple hat mehrere bestimmte Anforderungen, die erfüllt sein müssen, wenn Ihre app Xamarin.tvOS Gamecontroller unterstützt:

- **Müssen Sie die Siri Remote unterstützen** -müssen Sie immer die Siri Remote unterstützen. Ihr Spiel kann keine 3. Partei Game Controller, es kann gespielt werden erforderlich.
- **Müssen Sie erweiterte Layouts unterstützen** -alle TvOS Gamecontroller sind nicht formfitting Controller erweitert.
- **Spiele muss Playable mit Stand-Alone Controllern** – Wenn Ihre app eine erweiterte Game Controller unterstützt, muss sein ausschließlich mit diesem Controller Spiel gespielt werden.
- **Müssen Sie die Schaltfläche "Abspielen/Pause" unterstützen** -während Spiel, wenn der Benutzer die Schaltfläche "Abspielen/Pause" drückt Sie sollten eine Warnung so erhält der Benutzer die Möglichkeit zum Anhalten/Fortsetzen Gameplay anzeigen oder zum Hauptmenü zurück.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Aktivieren der Unterstützung für Gamecontroller

Um Gamecontroller-Unterstützung in Ihrer app Xamarin.tvOS aktivieren möchten, doppelklicken Sie auf die `Info.plist` Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen:

[![](remote-bluetooth-images/game02.png "Die Datei \"Info.plist\"-editor")](remote-bluetooth-images/game02.png#lightbox)

Unter den **Game Controller** Abschnitt, aktivieren Sie das Kontrollkästchen von **Gamecontroller aktivieren**, überprüfen Sie dann alle von der Game Controller-Typen, die von der app unterstützt werden.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Verwenden die Siri Remote als ein Game Controller

Siri-Remoteinstanz, die die im Lieferumfang von Apple TV können als eine eingeschränkte Game Controller verwendet werden. Ähnlich wie andere Gamecontroller, wird Sie in der Game Controller-Framework als eine `GCController` Objekt und unterstützt die `GCMotion` und `GCMicroGamepad` Profile.

Siri Remote weist folgende Merkmale auf, wenn als Game Controller verwendet werden:

- Die Oberfläche des Touch kann als ein Steuerkreuz verwendet werden, die analoge Eingabedaten bereitstellt.
- Die Remote in entweder eine hoch-oder Querformat verwendet werden kann, und Ihre app entscheidet sich, wenn das Profilobjekt Eingabedaten automatisch kippt, sollte.
- Klicken Sie auf die Oberfläche des Touch verhält sich wie die Schaltfläche **ein** in einem Spiel-Controller.
- Die Schaltfläche "Abspielen/Pause" verhält sich wie Schaltfläche **X** in einem Spiel-Controller.
- Eine Warnung so erhält der Benutzer die Möglichkeit zum Anhalten/Fortsetzen Spiel oder kehren Sie zum Hauptmenü zurück sollte von die Menüschaltfläche angezeigt werden.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Bestimmen von Gamecontroller-Eingabe

Im Gegensatz zu iOS, in dem Game Controller-Ereignisse parallel mit Touch-Ereignisse empfangen werden können, verarbeitet TvOS alle Low-Level-Ereignisse auf hoher Ebene übermitteln `UIKit` Ereignisse. Daher, wenn Sie Zugriff auf die low-Level Game Controller-Ereignisse benötigen, Sie müssen deaktivieren `UIKit`Standardverhalten.

Für TvOS Game Controller verarbeiten direkt verwenden, Sie müssen möchten eine `GCEventViewController` (oder eine Unterklasse) an, um das Spiel zeigt die Benutzeroberfläche. Wenn eine `GCEventViewController` ist die *erste Beantworter*, Game Controller Eingabe erfasst und an Ihre app über das Game Controller-Framework bereitgestellt werden.

Können Sie die `UserInteractionEnabled` Eigenschaft der `GCEventViewController` Klasse, um umzuschalten, wie Ereignisse verarbeitet und behandelt werden.

Informationen zum Implementieren von Gamecontroller-Unterstützung finden Sie in Apple [arbeiten mit Gamecontroller](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) im Abschnitt der [App-Programmierhandbuch für TvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) und [Game Controller Programmierhandbuch](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen Remotedebugging-Siri behandelt, die mit dem Apple TV, Fläche Touch-Gesten und Siri Remote Schaltflächen enthalten ist. Als Nächstes konnte damit die Arbeit mit Bewegungen und Storyboards, Gesten und Code und Ereignisse auf niedriger Ebene. Zum Schluss Wenn erläutert die Verwendung der Game-Controller.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
