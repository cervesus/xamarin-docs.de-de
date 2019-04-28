---
title: Entwurf der Benutzeroberfläche in Xamarin.Mac.Storyboard/.XIB-less
description: Dieser Artikel behandelt das Erstellen der Benutzeroberfläche einer Xamarin.Mac-Anwendung, direkt aus C# Code, ohne Storyboard-Dateien, XIB-Dateien oder Interface Builder.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 076c6464359a58c2b36d157d9620673b0644cd4a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61042287"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>Entwurf der Benutzeroberfläche in Xamarin.Mac.Storyboard/.XIB-less

_Dieser Artikel behandelt das Erstellen der Benutzeroberfläche einer Xamarin.Mac-Anwendung, direkt aus C# Code, ohne Storyboard-Dateien, XIB-Dateien oder Interface Builder._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit C# und .NET in einer Xamarin.Mac-Anwendung, Sie haben Zugriff auf die gleichen Elemente der Benutzeroberfläche und tools, die ein Entwickler *Objective-C-* und *Xcode* ist. In der Regel, wenn Sie eine Xamarin.Mac-Anwendung zu erstellen, verwenden Sie Interface Builder von Xcode mit Storyboard oder XIB-Dateien erstellen und verwalten Sie die Benutzeroberfläche der Anwendung.

Sie haben auch die Möglichkeit zum Erstellen einiger oder aller Xamarin.Mac der Benutzeroberfläche Ihrer Anwendung direkt in C# Code. In diesem Artikel behandeln wir die Grundlagen der Erstellung von Benutzeroberflächen und UI-Elemente in C# Code.

[![Visual Studio für Mac Code-Editor](xibless-ui-images/intro01.png "der Visual Studio für Mac Code-Editor")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Wechseln ein Fenster, um Code zu verwenden

Wenn Sie eine neue Xamarin.Mac-Cocoa-Anwendung erstellen, erhalten Sie ein Standardfenster leer ist, wird standardmäßig an. Dieses Windows wird definiert, eine **"Main.Storyboard"** (oder normalerweise eine **MainWindow.xib**) Datei automatisch im Projekt enthalten ist. Dies umfasst auch eine **ViewController.cs** -Datei, die Hauptansicht der app verwaltet (oder erneut normalerweise eine **MainWindow.cs** und ein **MainWindowController.cs** Datei).

Führen Sie folgende Schritte aus, um in einem Fenster Xibless für eine Anwendung zu wechseln:

1. Öffnen Sie die Anwendung, die Sie nicht mehr verwenden möchten `.storyboard` oder XIB-Dateien zum Definieren der Benutzeroberfläche in Visual Studio für Mac.
2. In der **Lösungspad**, mit der rechten Maustaste auf die **"Main.Storyboard"** oder **MainWindow.xib** und wählen Sie **entfernen**: 

    ![Entfernen die Haupt-Storyboard oder das Fenster](xibless-ui-images/switch01.png "Entfernen der Haupt-Storyboard oder das Fenster")
3. Von der **entfernen Dialogfeld**, klicken Sie auf die **löschen** Schaltfläche, um die Storyboard oder die XIB vollständig aus dem Projekt entfernen: 

    ![Bestätigen den Löschvorgang](xibless-ui-images/switch02.png "den Löschvorgang bestätigen")

Nun wir ändern müssen die **MainWindow.cs** Datei des Fensters Layout zu definieren und Ändern der **ViewController.cs** oder **MainWindowController.cs** zu erstellenden Datei ein Instanz von unserem `MainWindow` Klasse, da wir nicht mehr die Storyboard oder XIB-Datei verwenden.

Moderne Xamarin.Mac-apps, die Storyboards zu verwenden, für ihre Benutzeroberfläche kann nicht automatisch berücksichtigt die **MainWindow.cs**, **ViewController.cs** oder **MainWindowController.cs** Dateien. Fügen Sie je nach Bedarf einfach eine neue leere C# Klasse, um das Projekt (**hinzufügen** > **neue Datei...**   >  **Allgemeine** > **leere Klasse**) und nennen sie die fehlende Datei identisch. 


### <a name="defining-the-window-in-code"></a>Definieren das Fenster im code

Als Nächstes bearbeiten Sie die **MainWindow.cs** Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindow : NSWindow
    {
        #region Private Variables
        private int NumberOfTimesClicked = 0;
        #endregion

        #region Computed Properties
        public NSButton ClickMeButton { get; set;}
        public NSTextField ClickMeLabel { get ; set;}
        #endregion

        #region Constructors
        public MainWindow (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindow (NSCoder coder) : base (coder)
        {
        }

        public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
            // Define the user interface of the window here
            Title = "Window From Code";

            // Create the content view for the window and make it fill the window
            ContentView = new NSView (Frame);

            // Add UI elements to window
            ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
                AutoresizingMask = NSViewResizingMask.MinYMargin
            };
            ContentView.AddSubview (ClickMeButton);

            ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
                BackgroundColor = NSColor.Clear,
                TextColor = NSColor.Black,
                Editable = false,
                Bezeled = false,
                AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
                StringValue = "Button has not been clicked yet."
            };
            ContentView.AddSubview (ClickMeLabel);
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Wireup events
            ClickMeButton.Activated += (sender, e) => {
                // Update count
                ClickMeLabel.StringValue = (++NumberOfTimesClicked == 1) ? "Button clicked one time." : string.Format("Button clicked {0} times.",NumberOfTimesClicked);
            };
        }
        #endregion

    }
}
```

Sehen wir uns ein paar wichtige Elemente.

Wir haben zunächst wenige hinzugefügt _berechnete Eigenschaften_ , wird die fungieren wie Outlets (als ob das Fenster in einem Storyboard oder XIB-Datei erstellt wurde):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Diese erhalten uns Zugriff auf die Elemente der Benutzeroberfläche, die an das Fenster angezeigt werden sollen. Da das Fenster aus einem Storyboard oder XIB-Datei vergrößert wird, ist nicht, benötigen wir eine Möglichkeit zum Instanziieren (wie weiter unten in wir sehen die `MainWindowController` Klasse). Das ist die Funktionsweise dieser neuen Konstruktormethode:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Dies ist, in dem wir das Layout des Fensters und platzieren Sie alle Elemente der Benutzeroberfläche erforderlich, um die erforderliche Benutzeroberfläche erstellen. Bevor wir alle Elemente der Benutzeroberfläche in einem Fenster hinzufügen können, muss ein _Ansicht "Inhalt"_ auf die Elemente enthalten:

```csharp
ContentView = new NSView (Frame);
```

Dadurch wird eine Ansicht "Inhalt", die das Fenster zu füllen, wird erstellt. Nachdem wir unsere erste Element der Benutzeroberfläche, fügen eine `NSButton`, an das Fenster:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Die erste, was zu beachten ist, dass im Gegensatz zu iOS, MacOS mathematischen Notation verwendet, um seine Koordinatensystem Fenster zu definieren. Daher ist der Ursprung in der unteren linken Ecke des Fensters mit den Werten zu erhöhen, rechts, und klicken Sie auf der oberen rechten Ecke des Fensters. Wenn wir beim Erstellen der neuen `NSButton`, wir dies berücksichtigt, wie wir die Position und Größe auf dem Bildschirm definieren.

Die `AutoresizingMask = NSViewResizingMask.MinYMargin` Eigenschaft teilt der Schaltfläche, ich möchte am gleichen Speicherort aus dem oberen Rand des Fensters zu bleiben, wenn das Fenster vertikal angepasst wird. In diesem Fall Dies ist erforderlich, da (0,0) befindet sich am linken unteren Rand des Fensters.

Zum Schluss die `ContentView.AddSubview (ClickMeButton)` Methode fügt die `NSButton` auf die Ansicht "Inhalt" so, dass die It auf dem Bildschirm angezeigt wird, wenn die Anwendung ausführen und das Fenster angezeigt wird.

Als Nächstes wird eine Bezeichnung hinzugefügt, um das Fenster, das die Anzahl der angezeigt wird, die die `NSButton` geklickt wurde: 

```csharp
ClickMeLabel = new NSTextField (new CGRect (120, Frame.Height - 65, Frame.Width - 130, 20)) {
    BackgroundColor = NSColor.Clear,
    TextColor = NSColor.Black,
    Editable = false,
    Bezeled = false,
    AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin,
    StringValue = "Button has not been clicked yet."
};
ContentView.AddSubview (ClickMeLabel);
``` 

Da MacOS eine bestimmte nicht _Bezeichnung_ Benutzeroberflächenelement, haben wir eine speziell formatierte hinzugefügt nicht bearbeitbare `NSTextField` , als eine Bezeichnung zu fungieren. Genau wie die Schaltfläche "vor, die Größe und Position verwendet berücksichtigt wird, (0,0) unten links im Fenster. Die `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` Eigenschaft verwendet die **oder** Operator, um zwei kombinieren `NSViewResizingMask` Funktionen. Dadurch wird die Bezeichnung, die am gleichen Speicherort aus dem oberen Rand des Fensters zu bleiben, wenn das Fenster vertikal angepasst wird, und verkleinern und das Fenster horizontal angepasst wird, in der Breite wächst.

In diesem Fall die `ContentView.AddSubview (ClickMeLabel)` Methode fügt die `NSTextField` auf die Ansicht "Inhalt", damit sie auf dem Bildschirm angezeigt werden wird, wenn die Anwendung ausgeführt wird und das Fenster geöffnet.


### <a name="adjusting-the-window-controller"></a>Anpassen der fenstercontroller

Seit den Entwurf der `MainWindow` nicht mehr geladen wird, aus einer Storyboard oder XIB-Datei, müssen wir einige Anpassungen der fenstercontroller vornehmen. Bearbeiten der **MainWindowController.cs** Datei, und stellen sie wie folgt aussehen:

```csharp
using System;

using Foundation;
using AppKit;
using CoreGraphics;

namespace MacXibless
{
    public partial class MainWindowController : NSWindowController
    {
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }

        [Export ("initWithCoder:")]
        public MainWindowController (NSCoder coder) : base (coder)
        {
        }

        public MainWindowController () : base ("MainWindow")
        {
            // Construct the window from code here
            CGRect contentRect = new CGRect (0, 0, 1000, 500);
            base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);

            // Simulate Awaking from Nib
            Window.AwakeFromNib ();
        }

        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();
        }

        public new MainWindow Window {
            get { return (MainWindow)base.Window; }
        }
    }
}

```

Können Sie die wichtigsten Elemente der Änderung erläutert.

Zuerst definieren wir eine neue Instanz der dem `MainWindow` -Klasse und weisen sie auf der Basis fenstercontroller `Window` Eigenschaft:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Definieren wir die Position des Fensters der Bildschirm mit einer `CGRect`. Genau wie das Koordinatensystem des Fensters definiert der Bildschirm der unteren linken Ecke (0,0). Als Nächstes definieren wir den Stil des Fensters mit der **oder** Operator, um zwei oder mehr kombinieren `NSWindowStyle` Features:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Die folgenden `NSWindowStyle` Funktionen sind verfügbar:

- **Randloses** -Fenster hat kein Rahmen.
- **Mit dem Titel** -Fenster hat eine Titelleiste.
- **Geschlossen** -Fenster verfügt über eine Schaltfläche "Schließen" und kann geschlossen werden.
- **Miniaturizable** -Fenster verfügt über eine Schaltfläche Miniaturize und minimiert werden kann.
- **In der Größe veränderbaren** -Fenster wird eine Schaltfläche zum Ändern der Größe und mit veränderbarer Größe.
- **Hilfsprogramm** -Fenster ist ein Hilfsprogramm-Style-Fenster (Bereich).
- **DocModal** -, wenn das Fenster ein Bereich ist, werden modale Dokument anstelle von modalen System werden. 
- **NonactivatingPanel** – Wenn das Fenster ein Bereich ist, es kommt nicht zustande, das Hauptfenster.
- **TexturedBackground** -Fenster wird einen strukturierten Hintergrund haben.
- **Unscaled** -Fenster wird nicht skaliert werden.
- **UnifiedTitleAndToolbar** -Titel und die Symbolleiste des Fensters-Bereiche werden verknüpft werden.
- **HUD** -Fenster wird als ein Heads-Up-Display-Bereich angezeigt.
- **FullScreenWindow** -Fenster kann den Vollbildmodus eingeben.
- **FullSizeContentView** -Ansicht "Inhalt des Fensters" befindet sich hinter den Titel und die Symbolleiste Bereich.

Die letzten beiden Eigenschaften definieren die _Pufferung Typ_ für das Fenster und Funktionen zum Zeichnen des Fensters wird verschoben werden. Weitere Informationen zu `NSWindows`, informieren Sie sich von Apple [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) Dokumentation.

Schließlich, da das Fenster aus einem Storyboard oder XIB-Datei vergrößert wird, ist nicht, müssen wir simulieren sie in unserem **MainWindowController.cs** durch Aufruf der Windows `AwakeFromNib` Methode:

```csharp
Window.AwakeFromNib ();
```

Dies ermöglicht, dass Sie Code für das Fenster einfach ein Standardfenster von einer Storyboard oder XIB-Datei geladen wie.


### <a name="displaying-the-window"></a>Anzeigen des Fensters

Mit dem Storyboard oder XIB-Datei entfernt und die **MainWindow.cs** und **MainWindowController.cs** Dateien geändert haben, verwenden Sie das Fenster genau wie alle normalen Fensters, die in erstellt wurde Interface Builder von Xcode eine XIB-Datei.

Im folgenden wird eine neue Instanz der Fenster und den zugehörigen Controller zu erstellen und das Fenster auf dem Bildschirm anzeigen:

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

An diesem Punkt wird, wenn die Anwendung ausgeführt wird, und ein paar Mal für das Klicken auf die Schaltfläche, die folgende angezeigt:

![Eine Beispiel-app-Ausführung](xibless-ui-images/run01.png "eine Beispiel-app-Ausführung")


## <a name="adding-a-code-only-window"></a>Eine einzige Code-Fenster hinzufügen

Wenn eine nur-Code, Xibless Fenster zu einer vorhandenen Xamarin.Mac-Anwendung hinzugefügt werden soll, mit der Maustaste auf das Projekt in der **Lösungspad** , und wählen Sie **hinzufügen** > **neue Datei...** . In der **neue Datei** wählen Sie Dialogfeld **Xamarin.Mac** > **Cocoa-Fenster mit Controller**, wie unten gezeigt:

![Hinzufügen eines neuen Fensters Controllers](xibless-ui-images/add01.png "Hinzufügen eines neuen Fensters-Controllers") 

Wie bereits zuvor geschehen, wir die Standard-Storyboard oder XIB-Datei aus dem Projekt gelöscht werden (in diesem Fall **SecondWindow.xib**), und befolgen Sie die Schritte in der [Wechsel eines Fensters, um Code zu verwenden](#Switching_a_Window_to_use_Code) Abschnitt oben zum Abdecken der die Fensterdefinition Code.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Ein Element der Benutzeroberfläche hinzufügen in einem Fenster im code

Ob ein Fenster im Code erstellt oder aus einer Storyboard oder XIB-Datei geladen wurde, gibt es möglicherweise vorkommen, in dem wir ein Element der Benutzeroberfläche für ein Fenster aus dem Code hinzufügen möchten. Zum Beispiel:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Der obige Code erstellt ein neues `NSButton` und fügt es der `MyWindow` Fensterinstanz für die Anzeige. Im Grunde kann beliebiges Element der Benutzeroberfläche, die in Xcode Interface Builder in einem Storyboard oder XIB-Datei definiert werden kann im Code erstellt und in einem Fenster angezeigt werden.


## <a name="defining-the-menu-bar-in-code"></a>Definieren der Menüleiste im code

Aufgrund der derzeitigen Einschränkungen xamarin.Mac wird nicht empfohlen, dass Sie Ihre Xamarin.Mac-Anwendung im Menü-Leiste – erstellen`NSMenuBar`– im code jedoch weiterhin verwenden, die **"Main.Storyboard"** oder **MainMenu.xib** Datei, um es zu definieren. Dies bedeutet, dass Sie können das Hinzufügen und Entfernen von Menüs und Menüelemente in C# Code.

Bearbeiten Sie z. B. die **Datei "appdelegate.cs"** Datei, und stellen die `DidFinishLaunching` Methode sehen wie folgt:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    mainWindowController = new MainWindowController ();
    mainWindowController.Window.MakeKeyAndOrderFront (this);

    // Create a Status Bar Menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Phrases";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Phrases");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        Console.WriteLine("Address Selected");
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        Console.WriteLine("Date Selected");
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        Console.WriteLine("Greetings Selected");
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        Console.WriteLine("Signature Selected");
    };
    item.Menu.AddItem (signature);
}
```

Die oben genannten erstellt ein Menü der Statusleiste aus Code und wird angezeigt, wenn die Anwendung gestartet wird. Weitere Informationen zum Arbeiten mit Menüs, informieren Sie sich unsere [Menüs](~/mac/user-interface/menu.md) Dokumentation.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine ausführliche Übersicht über das Erstellen der Benutzeroberfläche einer Xamarin.Mac-Anwendung, in verwendet C# Code im Gegensatz zur Verwendung von Interface Builder von Xcode mit Storyboard oder XIB-Dateien.



## <a name="related-links"></a>Verwandte Links

- [MacXibless (Beispiel)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [Menüs](~/mac/user-interface/menu.md)
- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Einführung in Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
