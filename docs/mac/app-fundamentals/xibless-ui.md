---
title: ".Storyboard/.XIB-less Benutzeroberflächendesign"
description: "Dieser Artikel behandelt die Benutzeroberfläche einer Xamarin.Mac-Anwendung direkt aus dem C#-Code, ohne .storyboard-Dateien, .xib oder Schnittstelle-Generator erstellen."
ms.topic: article
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 544aad278b9bc66120e188eec54fa68be71dc625
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="storyboardxib-less-user-interface-design"></a>.Storyboard/.XIB-less Benutzeroberflächendesign

_Dieser Artikel behandelt die Benutzeroberfläche einer Xamarin.Mac-Anwendung direkt aus dem C#-Code, ohne .storyboard-Dateien, .xib oder Schnittstelle-Generator erstellen._


## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac Sie haben Zugriff auf die gleichen Elemente der Benutzeroberfläche und tools, die ein Entwickler arbeiten in *Objective-C* und *Xcode* verfügt. In der Regel beim Erstellen einer Anwendung Xamarin.Mac verwenden Sie Xcodes Benutzeroberflächen-Generator mit .storyboard oder .xib-Dateien erstellen und verwalten Sie die Benutzeroberfläche der Anwendung.

Sie haben auch die Option zum Erstellen einiger oder aller Ihrer Xamarin.Mac Anwendungsbenutzeroberfläche direkt im C#-Code. In diesem Artikel werden die Grundlagen der Erstellung von Benutzeroberflächen und Elemente der Benutzeroberfläche in C#-Code beschrieben.

[![Die Visual Studio für Mac-Code-Editor](xibless-ui-images/intro01.png "der Visual Studio für Mac-Code-Editor")](xibless-ui-images/intro01-large.png)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Wechseln ein Fenster, um Code zu verwenden.

Wenn Sie eine neue Xamarin.Mac Kakao-Anwendung erstellen, erhalten Sie Standardfensters leer ist, wird standardmäßig an. Dieses Windows wird definiert, einer **Main.storyboard** (oder normalerweise eine **MainWindow.xib**) automatisch im Projekt enthaltene Datei. Schließt dies auch eine **ViewController.cs** -Datei, die die app Hauptansicht verwaltet (oder erneut normalerweise eine **MainWindow.cs** und ein **MainWindowController.cs** Datei).

Führen Sie folgende Schritte aus, um in einem Fenster Xibless für eine Anwendung zu wechseln:

1. Öffnen Sie die Anwendung, die Sie nicht mehr verwenden möchten `.stroyboard` oder .xib Dateien so definieren Sie die Benutzeroberfläche in Visual Studio für Mac.
2. In der **Lösung Pad**, mit der rechten Maustaste auf die **Main.storyboard** oder **MainWindow.xib** Datei, und wählen Sie **entfernen**: 

    ![Entfernen die Haupt-Storyboard oder das Fenster](xibless-ui-images/switch01.png "Entfernen des Haupt-Storyboard oder Fenster")
3. Aus der **entfernen Dialogfeld**, klicken Sie auf die **löschen** Schaltfläche, um die .storyboard oder .xib vollständig aus dem Projekt zu entfernen: 

    ![Bestätigen den Löschvorgang](xibless-ui-images/switch02.png "den Löschvorgang bestätigen")

Jetzt wir ändern müssen die **MainWindow.cs** Datei des Fensters Layout zu definieren und Ändern der **ViewController.cs** oder **MainWindowController.cs** zu erstellenden Datei ein Instanz von unseren `MainWindow` Klasse, da wir die .storyboard oder .xib-Datei nicht mehr verwenden.

Moderne Xamarin.Mac-apps, die Storyboards zu verwenden, für ihre Benutzeroberfläche kann nicht automatisch berücksichtigt die **MainWindow.cs**, **ViewController.cs** oder **MainWindowController.cs** Dateien. Je nach Bedarf fügen Sie einfach eine neue leere C#-Klasse, um das Projekt (**hinzufügen** > **neue Datei...**   >  **Allgemeine** > **leere Klasse**), und nennen sie die fehlende Datei identisch. 


### <a name="defining-the-window-in-code"></a>Die Definition des Fensters im code

Als Nächstes Bearbeiten der **MainWindow.cs** Datei, und stellen sie wie folgt aussehen:

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

Betrachten Sie nur einige der wichtigen Elemente an.

Erstens wir ein Paar hinzugefügt _berechnete Eigenschaften_ , die verhält Sie sich wie Steckdosen (als ob das Fenster in einer Datei .storyboard oder .xib erstellt wurde):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Diese erhalten uns Zugriff auf die Elemente der Benutzeroberfläche, die wir werden auf das Fenster angezeigt werden sollen. Da das Fenster aus einer Datei .storyboard oder .xib vergrößert wird, ist nicht, benötigen wir eine Möglichkeit, instanziiert es (später im eingehendem der `MainWindowController` Klasse). Das heißt, was bewirkt, dass dieser neuen Konstruktormethode:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Dies ist, in dem wir das Layout des Fensters und platzieren Sie alle Benutzeroberflächenelemente, die erforderlich sind, um die erforderliche Benutzeroberfläche zu erstellen. Bevor wir alle Benutzeroberflächenelemente in einem Fenster hinzufügen können, muss ein _Inhaltsansicht_ Elemente enthalten:

```csharp
ContentView = new NSView (Frame);
```

Dadurch wird die Inhaltsansicht, das ausgefüllt werden, dass Sie das Fenster erstellt. Nachdem wir unsere erste Benutzeroberflächenelement Hinzufügen einer `NSButton`, in das Fenster:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Erstes sollten Sie beachten Sie hierbei ist, dass im Gegensatz zu iOS, MacOS mathematische Schreibweise verwendet, um seine Koordinatensystem Fenster zu definieren. Daher ist der Ursprung in der unteren linken Ecke des Fensters mit den Werten zu erhöhen, rechts und nach der oberen rechten Ecke des Fensters. Wenn wir beim Erstellen der neuen `NSButton`, wir dies berücksichtigt wir beim Definieren seiner Position und Größe auf dem Bildschirm.

Die `AutoresizingMask = NSViewResizingMask.MinYMargin` Eigenschaft teilt der Schaltfläche aus, dass es sinnvoll, bleiben Sie am gleichen Speicherort vom oberen Rand des Fensters, wenn die Fenstergröße vertikal geändert wird. Erneut, dies ist erforderlich, da (0,0) befindet sich auf der linken unteren Rand des Fensters.

Schließlich die `ContentView.AddSubview (ClickMeButton)` Methode fügt die `NSButton` auf der Inhaltsansicht so, dass die It auf dem Bildschirm angezeigt wird, wenn die Anwendung ausführen, um das Fenster angezeigt wird.

Als Nächstes wird eine Bezeichnung hinzugefügt, in das Fenster, in denen die Anzahl der angezeigt wird, die die `NSButton` geklickt wurde: 

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

Da MacOS auf einen bestimmten haben nicht _Bezeichnung_ Benutzeroberflächenelement, haben wir eine speziell formatierte hinzugefügt nicht bearbeitbare `NSTextField` als eine Bezeichnung zu agieren. So wie die Schaltfläche vor, die Größe und Position berücksichtigt wird, (0,0) unten links im Fenster. Die `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` -Eigenschaft die **oder** Operator, um zwei kombinieren `NSViewResizingMask` Funktionen. Dies veranlasst die Bezeichnung, die am gleichen Speicherort vom oberen Rand des Fensters bleiben, wenn die Fenstergröße vertikal geändert wird und verkleinern und angesichts der fortschreitenden in der Breite des Fensters horizontal angepasst wird.

Erneut, den `ContentView.AddSubview (ClickMeLabel)` Methode fügt die `NSTextField` auf die Inhaltsansicht, damit sie auf dem Bildschirm angezeigt werden wird, wenn die Anwendung ausgeführt wird und das Fenster geöffnet.


### <a name="adjusting-the-window-controller"></a>Anpassen des Fenster-Controllers

Seit das Design der der `MainWindow` nicht mehr geladen wird, aus einer Datei .storyboard oder .xib benötigen wir einige Anpassungen an den Controller Fenster vornehmen. Bearbeiten der **MainWindowController.cs** Datei, und stellen sie wie folgt aussehen:

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

Können Sie die wichtigsten Elemente dieser Änderung erläutert.

Zuerst definieren wir eine neue Instanz der dem `MainWindow` -Klasse und weisen Sie sie auf des Basis Fenster Controllers `Window` Eigenschaft:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Zunächst definieren Sie den Speicherort des Fensters Bildschirm mit einer `CGRect`. Genau wie das Koordinatensystem des Fensters definiert der Bildschirm der unteren linken Ecke (0,0). Als Nächstes definieren wir den Stil des Fensters mit den **oder** Operator zum Kombinieren von zwei oder mehr `NSWindowStyle` Funktionen:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
``` 

Die folgenden `NSWindowStyle` Funktionen sind verfügbar:

- **Randlos** -Fenster wird kein Rahmen haben.
- **Mit dem Titel** -Fenster hat eine Titelleiste.
- **Geschlossen** -Fenster verfügt über eine Schaltfläche "Schließen", und kann geschlossen werden.
- **Miniaturizable** -Fenster verfügt über eine Schaltfläche Miniaturize und minimiert werden kann.
- **In der Größe veränderbaren** -Fenster verfügen über eine Schaltfläche zum Ändern der Größe und in der Größe veränderbar sein wird.
- **Hilfsprogramm** -Fenster ist ein Hilfsprogramm Style-Fenster (Bereich).
- **DocModal** – Wenn das Fenster ein Bereich ist, wird Dokument modale statt System gebunden werden. 
- **NonactivatingPanel** – Wenn das Fenster ein Bereich ist, es wird nicht vorgenommen werden im Hauptfenster.
- **TexturedBackground** -Fensters wird ein Hintergrundbild haben.
- **Nicht skalierten** -Fenster wird nicht skaliert werden.
- **UnifiedTitleAndToolbar** -Titel und die Symbolleiste des Fensters-Bereiche werden verknüpft werden.
- **HUD** -Fenster wird als ein Heads-Up-Display Bereich angezeigt.
- **FullScreenWindow** -Fenster können Sie den Vollbildmodus eingeben.
- **FullSizeContentView** -Inhaltsansicht für das Fenster befindet sich hinter den Titel und die Symbolleiste Bereich.

Die letzten beiden Eigenschaften definieren die _Pufferung Typ_ für das Fenster, und wenn Zeichnung des Fensters wird verzögert werden. Weitere Informationen zu `NSWindows`, finden Sie in der Apple- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) Dokumentation.

Schließlich, da das Fenster aus einer Datei .storyboard oder .xib vergrößert wird, ist nicht, müssen wir zu simulieren, in unserem **MainWindowController.cs** durch Aufruf der Windows `AwakeFromNib` Methode:

```csharp
Window.AwakeFromNib ();
```

Dies ermöglicht, dass Sie Code anhand des Fensters, die nur aus einer Datei .storyboard oder .xib geladen Standardfensters wie.


### <a name="displaying-the-window"></a>Anzeigen des Fensters

Mit der Datei .storyboard oder .xib entfernt und die **MainWindow.cs** und **MainWindowController.cs** Dateien geändert haben, müssen verwenden Sie das Fenster ebenso wie alle normalen Fenster aus, die in erstellt wurde Xcode des Benutzeroberflächen-Generator mit einer .xib-Datei.

Die folgenden erstellt eine neue Instanz der Fenster und den zugehörigen Controller und das Fenster auch auf dem Bildschirm anzeigen:

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

An diesem Punkt wird, wenn die Anwendung ausgeführt wird, und ein paar Mal für das Klicken auf die Schaltfläche, die folgende angezeigt:

![Eine Beispiel-app ausführen](xibless-ui-images/run01.png "eine Beispiel-app ausführen")


## <a name="adding-a-code-only-window"></a>Eine einzige Code-Fenster hinzufügen

Wenn wir einen nur Code, Fenster "Xibless" für eine vorhandene Xamarin.Mac-Anwendung hinzufügen möchten, rechtsklicken Sie auf das Projekt in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neue Datei...** . In der **neue Datei** Dialogfeld Wählen Sie **Xamarin.Mac** > **Kakao-Fenster mit Controller**, wie unten gezeigt:

![Hinzufügen eines neuen Fensters Controllers](xibless-ui-images/add01.png "Hinzufügen eines neuen Fensters-Controllers") 

Wie bereits zuvor geschehen, wird die Standarddatei .storyboard oder .xib aus dem Projekt entfernt wird (in diesem Fall **SecondWindow.xib**) und führen Sie die Schritte in der [wechseln ein Fenster, wenn Sie Code verwenden](#Switching_a_Window_to_use_Code) Abschnitt oben zum Abdecken der die Fensterdefinition Code.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Hinzufügen von UI-Elements in einem Fenster im code

Ob ein Fenster im Code erstellt oder aus einer Datei .storyboard oder .xib geladen wurde, es gibt möglicherweise Zeiten, in denen wir ein Benutzeroberflächenelement in einem Fenster aus dem Code hinzufügen möchten. Zum Beispiel:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Der obige Code erstellt ein neues `NSButton` und fügt es der `MyWindow` Fensterinstanz für die Anzeige. Im Grunde kann Benutzeroberflächenelemente an, die in Xcodes Benutzeroberflächen-Generator in einer Datei .storyboard oder .xib definiert werden kann im Code erstellt und in einem Fenster angezeigt werden.


## <a name="defining-the-menu-bar-in-code"></a>Die Definition der Menüleiste im code

Aufgrund der aktuellen Einschränkungen in Xamarin.Mac wird nicht empfohlen, dass Sie Ihre Xamarin.Mac-Anwendungsverzeichnis im Menü Balken – erstellen`NSMenuBar`– im code jedoch weiterhin verwenden, die **Main.storyboard** oder **MainMenu.xib** Datei definieren Sie ihn. Dies bedeutet, dass Sie hinzufügen und Entfernen von Menüs und Menüelemente in C#-Code können.

Bearbeiten Sie z. B. die **AppDelegate.cs** Datei, und stellen die `DidFinishLaunching` Methode aussehen wie folgt:

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

Die oben genannten ein Menü der Statusleiste aus Code erstellt und wird angezeigt, wenn die Anwendung gestartet wird. Weitere Informationen zum Arbeiten mit Menüs finden Sie unter unsere [Menüs](~/mac/user-interface/menu.md) Dokumentation.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden eine ausführliche Übersicht über die Benutzeroberfläche einer Xamarin.Mac-Anwendung in C#-Code im Gegensatz zur Verwendung Xcodes Schnittstelle-Generator mit .storyboard oder .xib-Dateien erstellen.



## <a name="related-links"></a>Verwandte Links

- [MacXibless (Beispiel)](https://developer.xamarin.com/samples/mac/MacXibless/)
- [Windows](~/mac/user-interface/window.md)
- [Menüs](~/mac/user-interface/menu.md)
- [MacOS von menschlichen Richtlinien zur Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Einführung in Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
