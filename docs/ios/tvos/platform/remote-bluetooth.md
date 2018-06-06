---
title: Siri Remote und Bluetooth-Controller für tvos. außerdem wurden in Xamarin
description: In diesem Artikel wird beschrieben, wie zum Arbeiten mit der Siri Remote und Bluetooth Gamecontroller in tvos. außerdem wurden-apps mit Xamarin geschrieben wird.
ms.prod: xamarin
ms.assetid: BDB9894A-236B-424B-9032-ACD12A6C5720
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 3fc2abed202f8b2e6993890ca4e6b3c6875522e5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789189"
---
# <a name="siri-remote-and-bluetooth-controllers-for-tvos-in-xamarin"></a>Siri Remote und Bluetooth-Controller für tvos. außerdem wurden in Xamarin

Benutzer der app Xamarin.tvOS werden nicht werden interagieren mit der Schnittstelle direkt als mit iOS, in dem sie tippen Sie auf Bilder auf dem Gerätebildschirm jedoch indirekt von über den Raum mithilfe, der [Siri Remote](#The-Siri-Remote).

Wenn Ihre app ein Spiel ist, Sie können optional erstellen in der Unterstützung für 3rd Party, vorgenommen für iOS (Konfiguration) [Bluetooth Gamecontroller](#Bluetooth-Game-Controllers) in Ihrer app ebenfalls.

[![](remote-bluetooth-images/intro01.png "Die Bluetooth-Remote und Gamecontroller")](remote-bluetooth-images/intro01.png#lightbox)

Dieser Artikel beschreibt die [Siri Remote](#The-Siri-Remote), [Oberflächen-Gesten berühren](#Touch-Surface-Gestures) und [Siri Remote Schaltflächen](#Siri-Remote-Buttons) und gezeigt, wie Arbeiten sie über [Gesten und Storyboards](#Gestures-and-Storyboards), [Gesten und Code](#Gestures-and-Code) und [Low-Level-Ereignisbehandlung](#Low-Level-Event-Handling). Schließlich behandelt wird [arbeiten mit Gamecontroller](#Working-with-Game-Controllers) in einer Xamarin.tvOS-app.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Siri Remote

Die wichtigste Methode, die Benutzer mit dem Apple TV und Ihre app Xamarin.tvOS interagieren werden, ist über die enthalten Siri Remote. Apple entwickelt Remoteinstanz, den Abstand zwischen dem Benutzer, die sich direkt auf die Couch und die Apple TV-Benutzeroberfläche angezeigt wird, über den Raum auf dem Bildschirm TV zu überbrücken.

Herausforderung darin ein app-Entwickler tvos. außerdem wurden ist das Erstellen einer schnell, einfach zu verwenden und grafisch anspruchsvollen Benutzeroberfläche, die die Siri Remote Touch-Oberfläche, Beschleunigungsmesser, Schaltflächen und Gyroskop nutzt.

[![](remote-bluetooth-images/remote01.png "Siri Remote")](remote-bluetooth-images/remote01.png#lightbox)

Siri Remoteinstanz verfügt über die folgenden Features und die erwartete Verwendung innerhalb Ihrer app tvos. außerdem wurden:

|Funktion|Allgemein-App-Nutzung|Spiel App-Nutzung|
|---|---|---|
|**Touch-Oberfläche**<br />Streichen Sie nach, um zu navigieren, drücken Sie die EINGABETASTE, um auszuwählen, und halten Sie für Kontextmenüs.|**Tap/Wischen**<br />UI-Navigation zwischen den Fokus erhalten kann.<br /><br />**Klicken Sie auf**<br />Aktiviert die ausgewählte (bildschärfenmodus)-Element.|**Tap/Wischen**<br />Spiel Entwurf abhängig und kann als eine Steuerkreuz durch Tippen auf die Ränder verwendet werden.<br /><br />**Klicken Sie auf**<br />Führen Sie die primäre Maustaste-Funktion.|
|**Menü**<br />Drücken Sie, um zum vorherigen Bildschirm oder einem Menü zurückzukehren.|Kehrt zum vorherigen Bildschirm zurück und beendet, Apple TV-Startbildschirm aus dem Haupt-app-Bildschirm.|Anhalten und fortsetzen Spielzüge, kehrt zum vorherigen Bildschirm zurück und wird beendet, Apple TV-Startbildschirm aus dem Haupt-app-Bildschirm.|
|**Siri/suchen**<br />In Ländern mit Siri halten für Voice-Steuerelement in allen anderen Ländern, zeigt Suchbildschirm.|n/v|n/v|
|**Wiedergabe und Pause**<br />Wiedergeben und Anhalten von Medien oder eine sekundären Funktion in apps bietet.|Startet die Wiedergabe von Medien und Wiedergabe anhalten/fortsetzen.|Sekundäre Schaltfläche Funktion ausführt, oder überspringt Sie Intro Video (falls vorhanden).|
|**Home**<br />Drücken Sie die EINGABETASTE, um zur Startseite zurückzukehren, und doppelklicken Sie zum Anzeigen von ausgeführten apps, halten Sie Geräte in den Ruhezustand.|n/v|n/v|
|**Volume**<br />Steuert, angefügte Volume Audio/Video-Geräte.|n/v|n/v|

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Fingereingabe-Oberfläche

Die Siri Remote berühren Oberfläche kann eine Vielzahl von einem Finger Gesten erkennen, die Sie in Ihrer app Xamarin.tvOS reagieren können:

|Wischen|Klicken|Tippen Sie auf|
|---|---|---|
|![](remote-bluetooth-images/Gesture01.png)|![](remote-bluetooth-images/Gesture02.png)|![](remote-bluetooth-images/Gesture03.png)|
|Auswahl (Fokus) zwischen Elementen der Benutzeroberfläche auf dem Bildschirm bewegt (oben, unten links, mit der rechten Maustaste). Ein Lesegerät kann einen Bildlauf durch umfangreiche Listen von Inhalten mithilfe schnell Trägheit verwendet werden.|Das ausgewählte (bildschärfenmodus)-Element aktiviert oder verhält sich wie die primäre Maustaste in ein Spiel. Klicken Sie auf, und Speichern der speicherresidenten können Kontextmenüs oder sekundäre Funktionen aktivieren.|Leicht Tippen auf die Oberfläche berühren, an den Kanten verhält sich wie direktionale Schaltflächen in einem Steuerkreuz, verschieben den Fokus, oben, unten, links oder rechts je nach Bereich abgerufen. Je nach der app kann verwendet werden, um ausgeblendete Steuerelemente anzuzeigen.|

Apple bietet die folgenden Vorschläge für die Arbeit mit Gesten berühren Oberfläche:

* **Unterscheiden zwischen Klicks und Taps** -ist eine Aktion, die vom Benutzer beabsichtigt und eignet sich für die Auswahl, Aktivierung und die primäre Maustaste eines Spiels auf. Beim Tippen auf ist weniger offensichtlich und sollte nur selten verwendet werden, da der Benutzer die Siri Remote in ihrer Hand häufig gedrückt halten ist und kann eine Tap-Ereignis versehentlich problemlos aktivieren.
* **Nicht standardmäßige Gesten Redefine** -der Benutzer hat eine Annahme, dass bestimmte Gesten bestimmte Aktionen ausführen, werden die Bedeutung oder die Funktion des diese Gesten darf nicht in Ihrer app definieren. Die einzige Ausnahme ist ein spieleanwendungen während active Spielverlauf.
* **Definieren Sie neue Gesten sparsam** -Vorgang, der Benutzer hat eine Annahme, dass bestimmte Gesten bestimmte Aktionen ausführen, werden. Definieren von benutzerdefinierten Gesten zum Ausführen von Standardaktionen sollte vermieden werden. Und erneut, Spiele sind die meisten üblichen Ausnahme in dem benutzerdefinierte Gesten Arbeitserleichterung, faszinierend Play auf das Spiel hinzufügen können.
* **Falls zutreffend, reagieren auf Steuerkreuz tippt** - leicht in der Ecke Ränder der Touch-Oberfläche werden zu reagieren, wie eine Steuerkreuz auf einem Gamecontroller verschieben den Fokus oder die Richtung nach oben, unten, links oder mit der rechten Maustaste auf tippen. Falls zutreffend, sollten Sie auf diese Gesten in Ihrer app oder ein Spiel reagieren.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Siri Remote-Schaltflächen

Zusätzlich zu den Gesten auf der Oberfläche berühren kann Ihre app für den Benutzer durch Klicken auf die Oberfläche berühren, oder drücken Sie die Schaltfläche für Wiedergabe und Pause reagieren. Wenn Sie mit dem Game-Controller-Framework Siri Remote zugreifen, können Sie auch die Schaltfläche wird gedrückt erkennen. 

Darüber hinaus Menü Schaltfläche drückt können erkannt werden mithilfe einer Geste Erkennung mit normalen `UIKit` Elemente. Wenn Sie die Schaltfläche wird gedrückt abzufangen, Sie ist zuständig für das Schließen des aktuellen Ansicht View Controller und der vorherigen Abfrage zurück.

> [!IMPORTANT]
> Sie sollten **immer** die Wiedergabe und Pause-Taste auf der Remoteinstanz eine Funktion zuweisen. Mit einer nicht-funktionale Schaltfläche können Sie Ihre app unterbrochen, um den Endbenutzer aussehen vornehmen. Wenn Sie eine gültige Funktion für Schaltflächenname besitzen, weisen Sie die gleiche Funktion wie die primäre Maustaste (berühren Oberfläche klicken).

<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Gesten und Storyboards

Die einfachste Möglichkeit zum Arbeiten mit der entfernten Siri in Ihrer app Xamarin.tvOS wird Ihre Ansichten in der Benutzeroberflächen-Designer Geste Prüfer hinzugefügt.

Um eine Erkennung Gesten hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei und öffnet ihn zur Bearbeitung der Benutzeroberflächen-Designer.
2. Ziehen Sie eine **Geste Erkennungsmodul Tippen Sie auf** aus der **Bibliothek** und legen Sie sie in der Sicht: 

    [![](remote-bluetooth-images/storyboard01.png "Eine Tap-Geste-Erkennung")](remote-bluetooth-images/storyboard01.png#lightbox)
3. Überprüfen Sie **wählen** in der **Schaltfläche** Teil der **Attribut Inspektor**: 

    [![](remote-bluetooth-images/storyboard02.png "Aktivieren Sie")](remote-bluetooth-images/storyboard02.png#lightbox)
4. **Wählen Sie** bedeutet, dass die Bewegung antwortet auf die Benutzer klicken auf die **berühren Oberfläche** auf der Remoteinstanz Siri. Sie haben auch die Möglichkeit, Antworten auf die **Menü**, **für Wiedergabe und Pause**, **einrichten**, **unten**, **Links** und **Rechts** Schaltflächen.
5. Als Nächstes Verknüpfen einer **Aktion** aus der **Geste Erkennungsmodul Tippen Sie auf** und Aufruf der `TouchSurfaceClicked`: 

    [![](remote-bluetooth-images/storyboard03.png "Eine Aktion aus der Erkennung der Tap-Geste")](remote-bluetooth-images/storyboard03.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac.

Bearbeiten Sie die View-Controller (Beispiel `FirstViewController.cs`) Datei, und fügen Sie den folgenden Code zum Behandeln der Geste, die ausgelöst wird:

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

Weitere Informationen zum Arbeiten mit Storyboards finden Sie unter unsere [Hello tvos. außerdem wurden Quick Start Guide](~/ios/tvos/get-started/hello-tvos.md). Insbesondere die [Erstellen der Benutzeroberfläche](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) und [Schreiben von Code mit Steckdosen und Aktionen](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) Abschnitte.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Gesten und Code

Sie können optional Gesten direkt in C#-Code erstellen und mit Ansichten in der Benutzeroberfläche hinzufügen. Beispielsweise um eine Reihe von Wischen Geste Prüfer hinzufügen, bearbeiten Sie die View-Controller, und fügen Sie den folgenden Code hinzu:

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

Wenn Sie einen benutzerdefinierten Typ, der basierend auf Erstellen `UIKit` in Ihrer app Xamarin.tvOS (z. B. `UIView`), haben Sie auch die Möglichkeit, die auf niedriger Ebene Verarbeitung von Drücken der Taste über bereitstellen `UIPress` Ereignisse. 

Ein `UIPress` Ereignis um tvos. außerdem wurden Was ist ein `UITouch` Ereignis ist für iOS, mit Ausnahme von `UIPress` gibt Informationen über Schaltfläche drückt, auf der Remoteinstanz Siri oder anderen angeschlossenen Bluetooth-Geräte (z. B. ein Spiel-Controller). `UIPress` Ereignisse beschrieben, die gedrückt und dessen Status ("Began", "abgebrochen", "geändert" oder "abgeschlossen"). 

Für analogen Schaltflächen auf Geräten wie Bluetooth Gamecontroller `UIPress` wird auch die Größe des erzwingen, die auf die Schaltfläche "" angewendet wird. Die `Type` Eigenschaft von der `UIPress` Ereignis definiert, welche Taste hat, geänderten Status, während sich der Rest der Eigenschaften beschrieben, die Änderung, die aufgetreten sind.

Der folgende Code zeigt ein Beispiel für Handhabung auf niedriger Ebene `UIPress` Ereignisse für eine `UIView`:

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

Zusätzlich zu den standardmäßigen Siri Remoteinstanz, die im Lieferumfang von der Apple TV, 3rd Party, vorgenommen für iOS können Ihre app Xamarin.tvOS gesteuert (Konfiguration) Bluetooth-Gamecontroller in Verbindung mit dem Apple TV und verwendet werden.

[![](remote-bluetooth-images/game01.png "Bluetooth-Gamecontroller")](remote-bluetooth-images/game01.png#lightbox)

Gamecontroller können verwendet werden, um Spielverlauf verbessern, und geben Sie einen Eindruck von stärker in ein Spiel. Sie können auch die standardmäßige Apple TV-Schnittstelle zu steuern, damit die Verwendung zwischen dem Remotecomputer und dem Controller wechseln muss, nicht verwendet werden.

> [!IMPORTANT]
> Ihre app kann nicht den Benutzer einen erwerben erzwingen, dass Bluetooth-Gamecontroller sind eine optionale Kauf, der Endbenutzer vorgenommen werden können. Wenn Ihre app Gamecontroller unterstützt, muss es auch Siri Remote unterstützen, damit das Spiel von allen Benutzern von Apple TV verwendbar ist.

Eine Gamecontroller verfügt über die folgenden Features und die erwartete Verwendung innerhalb Ihrer app tvos. außerdem wurden:

|Funktion|Allgemein-App-Nutzung|Spiel App-Nutzung|
|---|---|---|
|**D-Pad**|Benutzeroberflächenelemente (ändert den Fokus) navigiert.|Hängt von Spiel.|
|**A**|Aktiviert das ausgewählte (bildschärfenmodus)-Element.|Primäre Schaltfläche Funktion ausführt, und vergewissert sich Dialogfeldaktionen.|
|**B**|Kehrt zum vorherigen Bildschirm zurück, oder auf dem Startbildschirm des beendet wird, wenn es sich um die app-Hauptbildschirm aus.|Führt die zweite Schaltfläche-Funktion, oder zum vorherigen Bildschirm zurück.|
|**X**|Startet die Wiedergabe von Medien oder Wiedergabe angehalten/fortgesetzt wird.|Hängt von Spiel.|
|**Y**|n/v|Hängt von Spiel.|
|**Menü**|Kehrt zum vorherigen Bildschirm zurück, oder auf dem Startbildschirm des beendet wird, wenn es sich um die app-Hauptbildschirm aus.|Fortsetzen Sie anhalten/Spielverlauf, kehrt zum vorherigen Bildschirm zurück oder beendet wird, auf dem Startbildschirm des Wenn es sich um die app-Hauptbildschirm.|
|**Linke Schulter-Schaltfläche**|Navigiert nach links.|Hängt von Spiel.|
|**Linke Trigger**|Navigiert nach links.|Hängt von Spiel.|
|**Rechte Schulter-Schaltfläche**|Navigiert rechts an.|Hängt von Spiel.|
|**Rechter Auslöser**|Navigiert rechts|Hängt von Spiel.|
|**Linken Ministick**|Benutzeroberflächenelemente (ändert den Fokus) navigiert.|Hängt von Spiel.|
|**Ministick rechts**|n/v|Hängt von Spiel.|

Apple bietet die folgenden Vorschläge für die Arbeit mit Gamecontroller:

- **Bestätigen Sie Game Controller Verbindungen** -Ihre app tvos. außerdem wurden gestartet und jederzeit vom Benutzer beendet werden. Sollten Sie immer prüfen auf Vorhandensein eines Controllers Spiel auf app-Start oder aktiv Zeiten und Aktionen nach Bedarf.
- **Stellen Sie sicher Ihre App funktioniert auf Siri Remotecomputer und Gamecontroller** -erfordern keine Benutzer wechseln zwischen Siri Remote und eine Gamecontroller, um Ihre app verwenden. Testen Sie Ihre app häufig mit beiden Typen von Domänencontrollern, die sicherstellen, dass alles einfach ist zu navigieren und wie erwartet funktioniert.
- **Bereitstellen einer Möglichkeit wieder** -drücken die Menütaste sollte immer zum vorherigen Bildschirm zurück. Wenn der Benutzer auf dem Haupt-app-Bildschirm ist, sollte die Menüschaltfläche auf dem Bildschirm Apple TV-Startseite zurück. Während der Spielverlauf sollten die Menüschaltfläche angezeigt werden eine Warnung, dass der Benutzer die Möglichkeit zum Anhalten/Fortsetzen Spielverlauf oder zum Hauptmenü zurück.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Arbeiten mit Gamecontroller

Wie bereits erwähnt, zusätzlich zu den Standard-Siri Remote, die geliefert wird mit der Apple TV, die Benutzer optional anfügen können a 3rd party, vorgenommen für iOS (Konfiguration) Bluetooth-Gamecontroller und verwenden, um Ihre app Xamarin.tvOS steuern.

Wenn Ihre app Controller auf niedriger Ebene Eingabe erforderlich, können Sie Apple verwendet [Game-Controller-Framework](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) verfügt über die folgenden Änderungen für tvos. außerdem wurden:

- Das Profil Micro Gamecontroller (`GCMicroGamepad`) wurde hinzugefügt, um die Siri Remote als Ziel.
- Die neue `GCEventViewController` Klasse zum Weiterleiten von Gamecontroller Ereignisse über Ihre app verwendet werden kann. Finden Sie unter der [bestimmen Game Controller Eingabe](#Determining-Game-Controller-Input) weiter unten für weitere Details.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Supportanforderungen Gamecontroller

Apple hat mehrere bestimmte Anforderungen, die erfüllt sein müssen, wenn Ihre app Xamarin.tvOS Gamecontroller unterstützt:

- **Sie müssen die Siri Remote unterstützen** -Sie müssen immer Siri Remote unterstützen. Spiel kann keine 3rd Party Gamecontroller wiedergegeben werden muss.
- **Müssen Sie die erweiterten Steuerelementlayouts Unterstützung** -alle tvos. außerdem wurden Gamecontroller sind nicht formfitting Controller erweitert.
- **Spiele muss Playable mit Stand-Alone Controllern** -Wenn Ihre app eine erweiterte Gamecontroller unterstützt, muss er ausschließlich mit diesem Controller Spiel wiedergegeben werden können.
- **Müssen Sie die Schaltfläche für Wiedergabe und Pause Unterstützung** -während Spielverlauf, wenn der Benutzer die Wiedergabe und Pause-Taste drückt Sie sollte eine Warnung aus, sodass der Benutzer die Möglichkeit, anhalten/fortsetzen Spielverlauf anzeigen, oder kehren Sie zum Hauptmenü zurück.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Aktivieren der Unterstützung für Gamecontroller

Um Gamecontroller-Unterstützung in Ihrer app Xamarin.tvOS aktivieren möchten, doppelklicken Sie auf die `Info.plist` in der Datei die **Projektmappen-Explorer** um ihn zur Bearbeitung zu öffnen:

[![](remote-bluetooth-images/game02.png "Die Datei \"Info.plist\"-editor")](remote-bluetooth-images/game02.png#lightbox)

Klicken Sie unter der **Gamecontroller** Abschnitt, aktivieren Sie das Kontrollkästchen durch **Gamecontroller aktivieren**, überprüfen Sie alle Gamecontroller-Typen, die von der app unterstützt werden.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>Mithilfe von Siri Remote als einen Gamecontroller

Siri Remoteinstanz, die mit dem Apple TV stammen können als eine begrenzte Gamecontroller verwendet werden. Wie andere Gamecontroller werden Sie in das Spiel Controller-Framework als eine `GCController` Objekt und unterstützt die `GCMotion` und `GCMicroGamepad` Profile.

Siri Remote weist folgende Merkmale auf, wenn als eine Gamecontroller verwendet werden:

- Die Oberfläche berühren kann als eine Steuerkreuz verwendet werden, die analoge Eingabedaten bereitstellt.
- Remote kann entweder eine hoch-oder im Querformat verwendet werden und Ihre app entscheidet sich, wenn das Profilobjekt Eingabedaten automatisch sollten.
- Auf der Oberfläche berühren verhält sich wie das Drücken der Schaltfläche **ein** auf einem Domänencontroller Spiel.
- Die Schaltfläche für Wiedergabe und Pause verhält sich wie Schaltfläche **X** auf einem Domänencontroller Spiel.
- Die Menüschaltfläche sollte anzeigen, eine Warnung, dass der Benutzer die Möglichkeit zum Anhalten/Fortsetzen Spielverlauf oder zum Hauptmenü zurück.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Bestimmen der Gamecontroller-Eingabe

Im Gegensatz zu iOS, in denen parallel mit Berührungsereignisse Gamecontroller Ereignisse empfangen werden können, tvos. außerdem wurden verarbeitet alle auf niedriger Ebene Ereignisse zum Übermitteln von allgemeinen `UIKit` Ereignisse. Daher, wenn Sie Zugriff auf die low-Level Gamecontroller Ereignisse benötigen, müssen Sie zum Ausschalten `UIKit`Standardverhalten.

Für tvos. außerdem wurden, wenn Sie Gamecontroller Eingaben zu verarbeiten, direkt Sie verwenden müssen möchten eine `GCEventViewController` (oder eine Unterklasse) das Spiel Anzeigen der Benutzeroberfläche. Wenn eine `GCEventViewController` ist die *erste Beantworter*, Gamecontroller Eingabe aufgezeichnet und an Ihre Anwendung über das Spiel Controller-Framework übermittelt werden.

Können Sie die `UserInteractionEnabled` Eigenschaft von der `GCEventViewController` Klasse, um umzuschalten, wie Ereignisse verarbeitet und behandelt werden.

Informationen über das Spiel Domänencontroller-Unterstützung implementieren, finden Sie unter der Apple- [arbeiten mit Gamecontroller](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) im Abschnitt der [App Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) und [Gamecontroller Programmierhandbuch](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die neuen Siri Remote behandelt, die mit dem Apple TV, Touch Oberfläche Gesten und Siri Remote Schaltflächen geliefert wird. Als Nächstes behandelt sie arbeiten mit Gesten und Storyboards, Gesten und Code und Ereignisse auf niedriger Ebene. Abschließend, wenn Arbeiten mit Gamecontroller erläutert.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
