---
title: Erstellen benutzerdefinierter Steuerelemente
description: "Dieser Artikel beschreibt, wie benutzerdefinierte Steuerelemente erstellen und im Benutzeroberflächen-Generator mit ihnen arbeiten."
ms.topic: article
ms.prod: xamarin
ms.assetid: 004534B1-5AEE-452C-BBBE-8C2673FD49B7
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 3ea88810384dfe8b1a08080953db19caddf25d6a
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="creating-custom-controls"></a>Erstellen von benutzerdefinierten Steuerelementen

_Dieser Artikel beschreibt, wie benutzerdefinierte Steuerelemente erstellen und im Benutzeroberflächen-Generator mit ihnen arbeiten._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleiche Benutzersteuerelemente, die ein Entwickler arbeiten in unter *Objective-C*, *Swift* und *Xcode* ist . Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ zu erstellen und verwalten Ihre Benutzer-Steuerelemente (oder erstellen sie optional direkt im C#-Code).

Bietet eine Vielzahl von integrierten Benutzersteuerelemente eine MacOS können Zeiten, die Sie zum Erstellen eines benutzerdefinierten Steuerelements zur Verfügung zu stellen Funktionen Out benötigen-of-Box nicht bereitgestellt oder ein benutzerdefiniertes Design der Benutzeroberfläche (z. B. ein Spiel-Schnittstelle) vorhanden sein.

[![](custom-controls-images/intro01.png "Beispiel für ein benutzerdefiniertes UI-Steuerelement")](custom-controls-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Erstellung eine wiederverwendbare benutzerdefinierte Benutzeroberflächensteuerelement in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="Introduction-to-Outline-Views" />

## <a name="introduction-to-custom-controls"></a>Einführung in benutzerdefinierte Steuerelemente

Wie oben erwähnt, gibt es möglicherweise vorkommen, dass Sie zum Erstellen einer wiederverwendbaren, benutzerdefinierte Benutzeroberflächensteuerelement um eindeutige Funktionen für Ihre Xamarin.Mac app-Benutzeroberfläche bereitzustellen oder um ein benutzerdefiniertes Design der Benutzeroberfläche (z. B. ein Spiel-Schnittstelle) zu erstellen müssen.

In diesen Fällen können Sie problemlos von erben `NSControl` , und erstellen Sie ein benutzerdefiniertes Tool, das der app UI über C#-Code oder über Xcodes-Schnittstelle-Generator entweder hinzugefügt werden kann. Durch Vererben von `NSControl` das benutzerdefinierte Steuerelement verfügen automatisch über alle standard-Funktionen, die über eine integrierte Benutzeroberflächensteuerelement verfügt (z. B. `NSButton`).

Wenn Ihr benutzerdefinierte Steuerelement für die Benutzeroberfläche zeigt nur Informationen (z. B. ein benutzerdefiniertes Diagramm und Grafik-Tool), möchten Sie möglicherweise erben `NSView` anstelle von `NSControl`.

Unabhängig davon, welche Basisklasse verwendet wird die grundlegenden Schritte zum Erstellen eines benutzerdefinierten Steuerelements ist identisch.

Erstellen Sie in diesem Artikel wird eine benutzerdefinierte kippen Switch-Komponente, die ein eindeutiger Benutzer Interface Design und ein Beispiel für eine voll funktionsfähige benutzerdefinierte Benutzeroberflächensteuerelement erstellen bereitstellt.

<a name="Building-the-Custom-Control" />

## <a name="building-the-custom-control"></a>Erstellen eines benutzerdefinierten Steuerelements

Da das benutzerdefinierte Steuerelement erstellt wird, eine Benutzereingabe (linke Mausklicks) reagieren werden wird, werden wir erben `NSControl`. Auf diese Weise unsere benutzerdefinierte Steuerelement automatisch alle Standardfunktionen, die über eine integrierte Benutzeroberflächensteuerelement verfügt und Verhalten sich wie ein standard MacOS-Steuerelement.

Öffnen Sie in Visual Studio für Mac das Xamarin.Mac-Projekt, das Sie verwenden möchten, erstellen eine benutzerdefinierte Schnittstelle Benutzersteuerelement für (oder eine neue erstellen). Fügen Sie eine neue Klasse hinzu, und nennen Sie es `NSFlipSwitch`:

[![](custom-controls-images/custom01.png "Hinzufügen einer neuen Klasse")](custom-controls-images/custom01.png#lightbox)

Als Nächstes Bearbeiten der `NSFlipSwitch.cs` Klasse, und stellen sie wie folgt aussehen:

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

Erstes sollten Sie über unsere benutzerdefinierte Klasse beachten, dass wir von erben `NSControl` und mithilfe der **registrieren** Befehl aus, um diese Klasse für Objective-C als auch Xcode des Benutzeroberflächen-Generator verfügbar machen:

```csharp
[Register("NSFlipSwitch")]
public class NSFlipSwitch : NSControl
```

In den folgenden Abschnitten führen wir einen Blick auf den Rest des vorangehenden Codes im Detail.

<a name="Tracking-the-Controls-State" />

### <a name="tracking-the-controls-state"></a>Nachverfolgen des Status des Steuerelements

Da unser benutzerdefiniertes Steuerelement einem Switch ist, benötigen wir eine Methode zum Nachverfolgen des On/Off-Status des Switches an. Wir behandeln, die mit den folgenden Code in `NSFlipSwitch`:

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

Wenn der Status des Switches ändert, benötigen wir eine Methode, die die Benutzeroberfläche aktualisiert. Wir machen, die durch das Steuerelement zum Neuzeichnen ihre Benutzeroberflächenautomatisierungs mit erzwingen `NeedsDisplay = true`.

Ggf. unserer Kontrolle mehr als eine einzelne aktivieren/deaktivieren Status (z. B. ein mit mehreren Zuständen Switch mit 3 Positionen), es hätte eine **Enum** um den Status zu verfolgen. In unserem Beispiel eine einfache **Bool** führen.

Es bietet nun auch eine Hilfsmethode, um den Status der Wechsel zwischen ein- und ausschalten austauschen:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
}
```

Später werden wir erweitern Sie diese Hilfsklasse, um den Aufrufer darüber zu informieren, wenn der Status des Switches geändert hat.

<a name="Drawing-the-Controls-Interface" />

### <a name="drawing-the-controls-interface"></a>Zeichnen die Schnittstelle des Steuerelements

Kegel zum Core Grafik zeichnen Routinen zu verwenden, um des benutzerdefinierten Steuerelements-Benutzeroberfläche zur Laufzeit zu zeichnen. Bevor wir dies durchführen können, müssen wir Ebenen für das Steuerelement zu aktivieren. Dies erfolgt durch die folgende private Methode:

```csharp
private void Initialize() {
    this.WantsLayer = true;
    this.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Von jeder der Konstruktoren des Steuerelements, um sicherzustellen, dass das Steuerelement ordnungsgemäß konfiguriert ist, ruft diese Methode aufgerufen. Zum Beispiel:

```csharp
public NSFlipSwitch (IntPtr handle) : base (handle)
{
    // Init
    Initialize();
}
```

Wir als Nächstes überschreiben Sie die `DrawRect` Methode und fügen Sie die Grafik Core Routinen zum Zeichnen des Steuerelements hinzu:

```csharp
public override void DrawRect (CGRect dirtyRect)
{
    base.DrawRect (dirtyRect);

    // Use Core Graphic routines to draw our UI
    ...

}
```

Wir müssen werden anpassen die visuelle Darstellung des Steuerelements, wenn dessen Zustand ändert (z. B. den Wechsel von **auf** auf **deaktiviert**). Jedes Mal, wenn der Status ändert, können wir die `NeedsDisplay = true` Befehl aus, um das Steuerelement zum Neuzeichnen für Zustand "Neu" zu erzwingen.

<a name="Responding-to-User-Input" />

### <a name="responding-to-user-input"></a>Reagieren auf Benutzereingaben

Es gibt zwei einfache Methode, dass wir eine Benutzereingabe auf unserem benutzerdefiniertes Steuerelement hinzufügen können: **überschreiben Maus Behandlung von Routinen** oder **Geste Prüfer**. Welche Methode, die wir verwenden, basieren auf die Funktionalität von unserer Kontrolle erforderlich.

> [!IMPORTANT]
> Für ein beliebiges benutzerdefiniertes Steuerelement, das Sie erstellen, sollten Sie verwenden Sie entweder **Methoden außer Kraft setzen** _oder_ **Geste Prüfer**, aber nicht beide zur gleichen Zeit wie diese miteinander in Konflikt stehen.

<a name="Summary" />

#### <a name="handling-user-input-with-override-methods"></a>Behandeln von Benutzereingaben mit Methoden zum Überschreiben

Objekte, die von erben `NSControl` (oder `NSView`) verfügen über mehrere Methoden zum Behandeln von Maus überschreiben oder die Tastatureingabe. Unserer Kontrolle Beispiel möchten wir den Status der Wechsel zwischen kippen **auf** und **deaktiviert** klickt der Benutzer auf das Steuerelement mit der linken Maustaste. Wir können hinzufügen, die folgenden Methoden zum Überschreiben der `NSFliwSwitch` Klasse, um dieses Problem behoben:

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

Im obigen Code, nennen wir die `FlipSwitchState` -Methode (oben definiert), spiegeln die On/Off-Status des Switches in der `MouseDown` Methode. Hierdurch wird auch das Steuerelement neu gezeichnet werden, um den aktuellen Status widerspiegelt erzwungen.

<a name="Handling-User-Input-with-Gesture-Recognizers" />

#### <a name="handling-user-input-with-gesture-recognizers"></a>Behandeln von Benutzereingaben mit Geste

Optional können Sie Geste Prüfer verwenden, den Benutzer, die Interaktion mit dem Steuerelement behandelt. Entfernen Sie die oben hinzugefügten Außerkraftsetzungen, bearbeiten Sie die `Initialize` Methode, und stellen sie wie folgt aussehen:

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

Hier werden wir ein neues erstellen `NSClickGestureRecognizer` und Aufrufen unsere `FlipSwitchState` Methode, um den Switch-Status zu ändern, wenn der Benutzer darauf mit der linken Maustaste klickt. Die `AddGestureRecognizer (click)` Methode fügt das Erkennungsmodul Geste an das Steuerelement.

Erneut aus, welche Methode verwendet, hängt davon ab, was wir mit unserem benutzerdefiniertes Steuerelement ausführen möchten. Wenn wir Zugriff auf niedriger Ebene benötigen die auf eine Benutzerinteraktion, verwenden Sie die Methoden überschreiben. Wenn wir vordefinierte Funktionen benötigt werden, verwenden Sie z. B. Mausklicks, Geste Merkmale.

<a name="Responding-to-State-Change-Events" />

### <a name="responding-to-state-change-events"></a>In den Status der Änderungsereignisse reagiert

Wenn der Benutzer den Status des unseres benutzerdefinierten Steuerelements ändert, benötigen wir eine Möglichkeit, auf die statusänderung in Code reagieren (z. B. Aktionen ausgeführt werden bei der auf eine benutzerdefinierte Schaltfläche klickt). 

Um diese Funktionalität bereitzustellen, bearbeiten die `NSFlipSwitch` -Klasse und fügen Sie den folgenden Code hinzu:

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

Als Nächstes bearbeiten die `FlipSwitchState` Methode, und stellen sie wie folgt aussehen:

```csharp
private void FlipSwitchState() {
    // Update state
    Value = !Value;
    RaiseValueChanged ();
}
```

Wir geben Sie zuerst eine `ValueChanged` Fall, dass wir einen Handler, der in C#-Code hinzufügen können, damit wir eine Aktion ausführen kann, wenn der Benutzer den Status des Switches ändert.

Zweiter, da unsere benutzerdefinierte Steuerelement erbt `NSControl`, verfügt automatisch über eine **Aktion** in Xcodes Benutzeroberflächen-Generator zugewiesen werden kann. Um diesen Aufruf **Aktion** bei Zustandsänderungen, verwenden wir den folgenden Code:

```csharp
if (this.Action !=null) 
    NSApplication.SharedApplication.SendAction (this.Action, this.Target, this);
```

Erstens wir überprüfen, wenn ein **Aktion** dem Steuerelement zugewiesen wurde. Wir rufen Sie als Nächstes die **Aktion** , wenn er definiert wurde.

<a name="Using-the-Custom-Control" />

## <a name="using-the-custom-control"></a>Verwenden des benutzerdefinierten Steuerelements

Mit unseren benutzerdefiniertes Steuerelement vollständig definiert können wir entweder sie unsere Xamarin.Mac-app-Benutzeroberfläche mit C#-Code oder in Xcodes Benutzeroberflächen-Generator hinzufügen.

Um das Steuerelement mit der Schnittstelle-Generator hinzuzufügen, führen Sie einen bereinigten Build des Projekts Xamarin.Mac zunächst Doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung im Benutzeroberflächen-Generator zu öffnen:

[![](custom-controls-images/custom02.png "Bearbeiten das Storyboard in Xcode")](custom-controls-images/custom02.png#lightbox)

Ziehen Sie anschließend eine `Custom View` in der Benutzeroberfläche entwerfen:

[![](custom-controls-images/custom03.png "Wählen eine benutzerdefinierte Ansicht aus der Bibliothek")](custom-controls-images/custom03.png#lightbox)

Mit der benutzerdefinierten Sicht ausgewählter, wechseln Sie zu der **Identität Inspektor** und ändern Sie die Ansicht **Klasse** auf `NSFlipSwitch`:

[![](custom-controls-images/custom04.png "Festlegen der View-Klasse")](custom-controls-images/custom04.png#lightbox)

Wechseln Sie zu der **Assistant-Editor** , und erstellen Sie eine **Steckdose** für das benutzerdefinierte Steuerelement (sicherstellen, binden sie in der `ViewControler.h` Datei und nicht die `.m` Datei):

[![](custom-controls-images/custom05.png "Konfigurieren eine neue Steckdose")](custom-controls-images/custom05.png#lightbox)

Speichern Sie die Änderungen, zurück zu Visual Studio für Mac, und lassen Sie die Änderungen zu synchronisieren. Bearbeiten der `ViewController.cs` Datei, und stellen die `ViewDidLoad` Methode aussehen wie folgt:

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

Hier auf die reagieren die `ValueChanged` Ereignis, die wir zuvor definierten, auf die `NSFlipSwitch` Klasse und schreibt die aktuelle **Wert** klickt der Benutzer auf das Steuerelement.

Optional, wir Builder-Schnittstelle zurück und definieren eine **Aktion** für das Steuerelement:

[![](custom-controls-images/custom06.png "Konfigurieren eine neue Aktion")](custom-controls-images/custom06.png#lightbox)

Bearbeiten Sie erneut, die `ViewController.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
partial void OptionTwoFlipped (Foundation.NSObject sender) {
    // Display the state of the option switch
    Console.WriteLine("Option Two: {0}", OptionTwo.Value);
}
```

> [!IMPORTANT]
> Verwenden Sie entweder die **Ereignis** oder definieren ein **Aktion** im Schnittstelle-Generator, aber Sie sollten nicht beide Methoden verwenden, zur gleichen Zeit oder sie miteinander in Konflikt stehen können.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Erstellen einer wiederverwendbaren benutzerdefinierte Benutzeroberflächensteuerelement in einer Anwendung Xamarin.Mac übernommen. Wir haben gesehen, wie die benutzerdefinierten Steuerelemente Benutzeroberfläche, die zwei verschiedene Arten gezeichnet wird, so reagieren Sie auf Tastatur- und Benutzereingaben und wie das neue Steuerelement Aktionen in Xcodes Benutzeroberflächen-Generator verfügbar gemacht wird.

## <a name="related-links"></a>Verwandte Links

- [MacCustomControl (Beispiel)](https://developer.xamarin.com/samples/mac/MacCustomControl/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Behandlung von Mausereignissen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/EventOverview/HandlingMouseEvents/HandlingMouseEvents.html)
