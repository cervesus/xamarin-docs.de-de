---
title: Erstellen von benutzerdefinierten Steuerelementen in Xamarin.Mac
description: Dieses Dokument beschreibt, wie Sie benutzerdefinierte Steuerelemente in Xamarin.Mac zu erstellen. Es zeigt, wie das benutzerdefinierte Steuerelement erstellen, seinen Status zu verfolgen, zeichnen die Schnittstelle, auf Benutzereingaben reagieren und verwenden Sie das Steuerelement in einer Anwendung.
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 7fde60d48c23bc48ce1602a0643a3af8ad492ec6
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104010"
---
# <a name="creating-custom-controls-in-xamarinmac"></a>Erstellen von benutzerdefinierten Steuerelementen in Xamarin.Mac

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleichen Benutzersteuerelemente, die ein Entwickler *Objective-C-*, *Swift* und *Xcode* ist . Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und Verwalten Ihrer Benutzer-Steuerelemente (oder erstellen sie optional direkt in c#-Code).

Während MacOS eine Fülle von integrierten Steuerelementen bereitstellt, gibt es möglicherweise vorkommen, die Sie benötigen, erstellen Sie ein benutzerdefiniertes Steuerelement angeben, dass Funktionen nicht mit Out-of-the-Box bereitgestellt oder ein benutzerdefiniertes Design der Benutzeroberfläche (z. B. eine Spiele-Schnittstelle) entsprechend vor.

[![](custom-controls-images/intro01.png "Beispiel für ein benutzerdefiniertes UI-Steuerelement")](custom-controls-images/intro01.png#lightbox)

In diesem Artikel wird die Grundlagen zum Erstellen einer wiederverwendbaren benutzerdefinierte Benutzeroberflächensteuerelement in einer Xamarin.Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Einführung in benutzerdefinierte Steuerelemente

Wie bereits erwähnt, gibt es möglicherweise gelegentlich Sie zum Erstellen einer wiederverwendbaren, benutzerdefinierte Steuerelemente der Benutzeroberfläche einer Xamarin.Mac-app-Benutzeroberfläche einzigartige Funktionen bereit, oder erstellen Sie ein benutzerdefiniertes Design der Benutzeroberfläche (z. B. eine Spiele-Schnittstelle müssen).

In diesen Fällen können Sie ganz einfach von erben `NSControl` , und erstellen Sie ein benutzerdefiniertes Tool, das UIs Ihrer app über c#-Code oder mit Interface Builder von Xcode entweder hinzugefügt werden kann. Durch Erben von `NSControl` Ihr benutzerdefinierte Steuerelement verfügen automatisch über alle standard-Funktionen, die über eine integrierte Benutzeroberflächensteuerelement verfügt (z. B. `NSButton`).

Wenn Ihr benutzerdefinierte Steuerelement für die Benutzeroberfläche zeigt nur Informationen (z. B. einen benutzerdefinierten Diagrammen und grafisches Tool), sollten Sie das erben `NSView` anstelle von `NSControl`.

Unabhängig davon, welcher Basisklasse verwendet wird die grundlegenden Schritte zum Erstellen eines benutzerdefinierten Steuerelements ist identisch.

Erstellen Sie in diesem Artikel wird eine benutzerdefinierte Switch Flip-Komponente, die einen eindeutigen Design der Benutzeroberfläche und ein Beispiel für das Erstellen eines voll funktionsfähigen benutzerdefinierten Schnittstelle Benutzersteuerelements bereitstellt.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Erstellen des benutzerdefinierten Steuerelements

Da das benutzerdefinierte Steuerelement erstellt wird, wird auf eine Benutzereingabe (linke Mausklicks) reagieren sein wird, werden wir das erben `NSControl`. Auf diese Weise wird das benutzerdefinierte Steuerelement automatisch alle Standardfunktionen, die über eine integrierte Benutzeroberflächensteuerelement verfügt und Verhalten sich wie ein standard-Mac-OS-Steuerelement.

Öffnen Sie in Visual Studio für Mac die Xamarin.Mac-Projekt, das Sie verwenden möchten, erstellen eine benutzerdefinierte Schnittstelle Benutzersteuerelement für (oder eine neue erstellen). Fügen Sie eine neue Klasse und nenne `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Hinzufügen einer neuen Klasse")](custom-controls-images/custom01.png#lightbox)

Als Nächstes bearbeiten Sie die `NSFlipSwitch.cs` Klasse, und stellen sie wie folgt aussehen:

```csharp
using Foundation;
using System;
using System.CodeDom.Compiler;
using AppKit;
using CoreGraphics;

namespace MacCustomControl
{
    [Register("NSFlipSwitch")]
    public class NSFlipSwitch : NSControl
    {
        #region Private Variables
        private bool _value = false;
        #endregion

        #region Computed Properties
        public bool Value {
            get { return _value; }
            set {
                // Save value and force a redraw
                _value = value;
                NeedsDisplay = true;
            }
        }
        #endregion

        #region Constructors
        public NSFlipSwitch ()
        {
            // Init
            Initialize();
        }

        public NSFlipSwitch (IntPtr handle) : base (handle)
        {
            // Init
            Initialize();
        }

        [Export ("initWithFrame:")]
        public NSFlipSwitch (CGRect frameRect) : base(frameRect) {
            // Init
            Initialize();
        }

        private void Initialize() {
            this.WantsLayer = true;
            this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
        }
        #endregion

        #region Draw Methods
        public override void DrawRect (CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

            // Use Core Graphic routines to draw our UI
            ...

        }
        #endregion

        #region Private Methods
        private void FlipSwitchState() {
            // Update state
            Value = !Value;
        }
        #endregion

    }
}
```

Der zuerst fällt auf Informationen über unsere benutzerdefinierte Klasse, da sind wir erben von `NSControl` und Verwenden der **registrieren** Befehl aus, um diese Klasse für Objective-C und Xcode Interface Builder verfügbar machen:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

In den folgenden Abschnitten werden wir den Rest des obigen Codes im Detail ansehen.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Verfolgen den Zustand des Steuerelements

Da das benutzerdefinierte Steuerelement ein Switch ist, benötigen wir eine Möglichkeit zum Nachverfolgen des ein/aus-Zustands des Schalters. Wir behandeln, die mit den folgenden Code in `NSFlipSwitch`:

```csharp
private bool _value = false;
...

public bool Value {
    get { return _value; }
    set {
        // Save value and force a redraw
        _value = value;
        NeedsDisplay = true;
    }
}
```

Wenn der Status des Switches ändert, benötigen wir eine Methode, die die Benutzeroberfläche aktualisiert. Zu diesem Zweck erzwingen das Steuerelement zum Neuzeichnen der Benutzeroberflächenautomatisierungs mit `NeedsDisplay = true`.

Wenn das Steuerelement mehr als eine einzelne in/aus-Status (z. B. einen mehrstufigen Switch mit 3 Positionen) erforderlich, wir hätten auch ein **Enum** zum Nachverfolgen des Zustands. In unserem Beispiel eine einfache **"bool"** erfolgt.

Wir haben auch eine Hilfsmethode zum Wechseln von der Wechsel zwischen des Status ein- und ausschalten hinzugefügt:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Wir werden später erweitern, dass diese Hilfsklasse aus, um den Aufrufer darüber zu informieren, wenn der Schalter-Zustand geändert hat.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Die Schnittstelle des Steuerelements zeichnen

Wir werden Grafische Core zeichnen Routinen zu verwenden, um des benutzerdefinierten Steuerelements-Benutzeroberfläche zur Laufzeit zu zeichnen. Bevor wir dies tun können, müssen wir Ebenen für das Steuerelement zu aktivieren. Dies erfolgt über die folgende private Methode:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Diese Methode wird aufgerufen, von jeder der Konstruktoren des Steuerelements, um sicherzustellen, dass das Steuerelement ordnungsgemäß konfiguriert ist. Zum Beispiel:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Als Nächstes müssen wir überschreiben die `DrawRect` Methode, und fügen Sie die Grafische Core Routinen zum Zeichnen des Steuerelements hinzu:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Wir werden werden anpassen die visuelle Darstellung des Steuerelements, wenn dessen Zustand ändert (z. B. der Wechsel von **auf** zu **aus**). Sobald der Status geändert wird, können wir die `NeedsDisplay = true` Befehl aus, um zu erzwingen, dass das Steuerelement für den neuen Status neu zeichnet.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Reagieren auf Benutzereingabe

Es gibt zwei einfache Möglichkeit, dass wir die Benutzereingaben an das benutzerdefinierte Steuerelement hinzufügen können: **überschreiben Maus behandeln Routinen** oder **Geste Erkennungen**. Welche Methode, die wir verwenden, wird auf Grundlage der Funktionalität, die erforderlich sind, indem Sie das Steuerelement.

> [!IMPORTANT]
> Für jedes benutzerdefinierten Steuerelements, das Sie erstellen, sollten Sie verwenden entweder **Methoden außer Kraft setzen** _oder_ **Geste Erkennungen**, aber nicht beide zur gleichen Zeit wie sie miteinander in Konflikt stehen.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Behandeln von Benutzereingaben mit Methoden zum Überschreiben

Objekte, die von erben `NSControl` (oder `NSView`) verfügen über mehrere überschreiben Sie Methoden für die Behandlung von Maus- oder Tastatureingaben. Für unser Beispielsteuerelement, um den Status der Wechsel zwischen kippen sollten **auf** und **aus** klickt der Benutzer auf das Steuerelement mit der linken Maustaste. Wir können die folgenden überschreiben Sie Methoden zum Hinzufügen der `NSFliwSwitch` Klasse, um dies zu verarbeiten:

```csharp
#region Mouse Handling Methods
// --------------------------------------------------------------------------------
// Handle mouse with Override Methods.
// NOTE: Use either this method or Gesture Recognizers, NOT both!
// --------------------------------------------------------------------------------
public override void MouseDown (NSEvent theEvent)
{
    base.MouseDown (theEvent);

    FlipSwitchState ();
}

public override void MouseDragged (NSEvent theEvent)
{
    base.MouseDragged (theEvent);
}

public override void MouseUp (NSEvent theEvent)
{
    base.MouseUp (theEvent);
}

public override void MouseMoved (NSEvent theEvent)
{
    base.MouseMoved (theEvent);
}
## endregion
```

Im obigen Code rufen wir die `FlipSwitchState` Methode (wie oben definiert) zum Spiegeln der On/Off-Status, der den Switch in der `MouseDown` Methode. Dies zwingt auch das Steuerelement neu gezeichnet wird, um den aktuellen Zustand wiederzugeben.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Behandeln von Benutzereingaben für Geste Erkennungen

Optional können Sie die Bewegung Erkennungen, behandeln den Benutzer, die Interaktion mit dem Steuerelement verwenden. Entfernen Sie die oben hinzugefügten Außerkraftsetzungen, bearbeiten Sie die `Initialize` Methode und legen Sie ihn wie folgt aussehen:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;

    // --------------------------------------------------------------------------------
    // Handle mouse with Gesture Recognizers.
    // NOTE: Use either this method or the Override Methods, NOT both!
    // --------------------------------------------------------------------------------
    var click = new NSClickGestureRecognizer (() => {
        FlipSwitchState();
    });
    AddGestureRecognizer (click);
}
```

Hier erstellen wir ein neues `NSClickGestureRecognizer` und Aufrufen von unserem `FlipSwitchState` -Methode des Switches Status zu ändern, wenn der Benutzer darauf, mit der linken Maustaste klickt. Die `AddGestureRecognizer (click)` Methode fügt die Stiftbewegungserkennung auf das Steuerelement.

In diesem Fall die Methode, die wir verwenden, hängt davon ab, was wir mit das benutzerdefinierte Steuerelement erreichen möchten. Wenn wir Zugriff auf niedriger Ebene benötigen die auf eine Benutzerinteraktion, verwenden Sie die Methoden außer Kraft setzen. Wenn wir die vordefinierten Funktionen benötigen, verwenden Sie wie Mausklicks, Geste Erkennungen.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>In den Status des Change-Ereignissen reagiert.

Wenn der Benutzer den Zustand der das benutzerdefinierte Steuerelement ändert, benötigen wir eine Möglichkeit, auf der Statuswechsel im Code zu reagieren (z. B. nachgehen Wenn auf eine benutzerdefinierte Schaltfläche klickt). 

Um diese Funktionalität bereitzustellen, bearbeiten die `NSFlipSwitch` Klasse und fügen Sie den folgenden Code hinzu:

```csharp
#region Events
public event EventHandler ValueChanged;

internal void RaiseValueChanged() {
    if (this.ValueChanged != null)
        this.ValueChanged (this, EventArgs.Empty);

    // Perform any action bound to the control from Interface Builder
    // via an Action.
    if (this.Action !=null) 
        NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
}
## endregion
```

Als Nächstes bearbeiten Sie die `FlipSwitchState` Methode und legen Sie ihn wie folgt aussehen:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Wir geben Sie zuerst eine `ValueChanged` Fall, dass wir einen Handler, der in C#-Code hinzufügen können, sodass wir eine Aktion ausführen kann, wenn der Benutzer den Status des Switches ändert.

Zweitens: Da das benutzerdefinierte Steuerelement erbt `NSControl`, verfügt automatisch über eine **Aktion** in Interface Builder von Xcode zugewiesen werden können. Um diesen Aufruf **Aktion** bei Zustandsänderungen, verwenden wir den folgenden Code:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Zuerst überprüfen wir Wenn ein **Aktion** des Steuerelements zugewiesen wurde. Als Nächstes rufen wir die **Aktion** wenn er definiert wurde.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Verwenden des benutzerdefinierten Steuerelements

Mit das benutzerdefinierte Steuerelement vollständig definiert wird können wir entweder sie unsere Xamarin.Mac-app-Benutzeroberfläche, die mithilfe von c#-Code oder in Interface Builder von Xcode hinzufügen.

Zum Hinzufügen des Steuerelements mit Interface Builder zuerst führen eine bereinigte Erstellung der dem Xamarin.Mac-Projekt, und doppelklicken Sie dann die `Main.storyboard` Datei, die sie für die Bearbeitung in Interface Builder zu öffnen:

[![](custom-controls-images/custom02.png "Bearbeiten das Storyboard in Xcode")](custom-controls-images/custom02.png#lightbox)

Ziehen Sie jetzt eine `Custom View` in den Entwurf der Benutzeroberfläche:

[![](custom-controls-images/custom03.png "Wählen eine benutzerdefinierte Ansicht aus der Bibliothek")](custom-controls-images/custom03.png#lightbox)

Die benutzerdefinierte Sicht noch ausgewählt ist, wechseln Sie zu der **Identitätsinspektor** und ändern Sie die Ansicht des **Klasse** zu `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Festlegen der Ansicht-Klasse")](custom-controls-images/custom04.png#lightbox)

Wechseln Sie zu der **Assistant Editor** , und erstellen Sie eine **Outlet** für das benutzerdefinierte Steuerelement (dabei bindet, bindet es in der `ViewControler.h` Datei und nicht die `.m` Datei):

[![](custom-controls-images/custom05.png "Konfigurieren einen neuen Outlets")](custom-controls-images/custom05.png#lightbox)

Speichern Sie die Änderungen, wechseln Sie zurück zur Visual Studio für Mac, und ermöglichen Sie die Änderungen zu synchronisieren. Bearbeiten der `ViewController.cs` Datei, und stellen die `ViewDidLoad` Methode sehen wie folgt:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Do any additional setup after loading the view.
    OptionTwo.ValueChanged += (sender, e) => {
        // Display the state of the option switch
        Console.WriteLine("Option Two: {0}", OptionTwo.Value);
    };
}
``` 

Hier, wir reagieren auf die `ValueChanged` auf oben definierten Ereignis die `NSFlipSwitch` Klasse, und schreibt die aktuelle **Wert** klickt der Benutzer auf das Steuerelement.

Optional könnten wir zurück zu Interface Builder und definieren eine **Aktion** des Steuerelements:

[![](custom-controls-images/custom06.png "Konfigurieren eine neue Aktion")](custom-controls-images/custom06.png#lightbox)

Bearbeiten Sie in diesem Fall die `ViewController.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Verwenden Sie entweder die **Ereignis** oder definieren Sie eine **Aktion** in Interface Builder, aber Sie sollten nicht beide Methoden zur gleichen Zeit verwenden oder sie miteinander in Konflikt stehen können.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel hat es sich um einen detaillierten Einblick in die erstellen eine wiederverwendbare benutzerdefinierte Benutzeroberflächensteuerelement in einer Xamarin.Mac-Anwendung geführt. Erläutert, wie die benutzerdefinierten Steuerelemente der Benutzeroberfläche, die zwei Hauptverfahren zu zeichnen, Maus und eine Benutzereingabe reagieren und das neue Steuerelement auf Aktionen in Interface Builder von Xcode verfügbar zu machen.

## <a name="related-links"></a>Verwandte Links

- [MacCustomControl (Beispiel)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Behandeln von Mausereignissen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
