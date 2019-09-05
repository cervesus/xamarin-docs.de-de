---
title: Storyboard/. XIb-Loses Design der Benutzeroberfläche in xamarin. Mac
description: In diesem Artikel wird beschrieben, wie Sie die Benutzeroberfläche einer xamarin. C# Mac-Anwendung direkt aus dem Code erstellen, ohne Storyboard-Dateien, XIb-Dateien oder Interface Builder.
ms.prod: xamarin
ms.assetid: 02310F58-DCF1-4589-9F4A-065DF64FC0E1
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 5776855039120b0c856a76a31334420ded2a2d65
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70283333"
---
# <a name="storyboardxib-less-user-interface-design-in-xamarinmac"></a>Storyboard/. XIb-Loses Design der Benutzeroberfläche in xamarin. Mac

_In diesem Artikel wird beschrieben, wie Sie die Benutzeroberfläche einer xamarin. C# Mac-Anwendung direkt aus dem Code erstellen, ohne Storyboard-Dateien, XIb-Dateien oder Interface Builder._

## <a name="overview"></a>Übersicht

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf dieselben Elemente und Tools wie die Benutzeroberfläche, die ein Entwickler in *Ziel-C* und *Xcode* verwendet. Wenn Sie eine xamarin. Mac-Anwendung erstellen, verwenden Sie in der Regel Xcode-Interface Builder mit Storyboard-oder XIb-Dateien, um die Benutzeroberfläche der Anwendung zu erstellen und zu verwalten.

Sie haben auch die Möglichkeit, die Benutzeroberfläche Ihrer xamarin. Mac-Anwendung direkt im C# Code zu erstellen. In diesem Artikel werden die Grundlagen der Erstellung von Benutzeroberflächen und Benutzeroberflächen Elementen im C# Code behandelt.

[![Der Visual Studio für Mac Code-Editor](xibless-ui-images/intro01.png "Der Visual Studio für Mac Code-Editor")](xibless-ui-images/intro01-large.png#lightbox)

<a name="Switching_a_Window_to_use_Code" />

## <a name="switching-a-window-to-use-code"></a>Wechseln eines Fensters zur Verwendung von Code

Wenn Sie eine neue xamarin. Mac-Cocoa-Anwendung erstellen, erhalten Sie standardmäßig ein Standardmäßiges leeres Standardfenster. Dieses Fenster wird in einer Datei " **Main. Storyboard** " (oder in der Regel " **MainWindow. XIb**") definiert, die automatisch im Projekt enthalten ist. Dazu gehört auch eine **ViewController.cs** -Datei, die die Hauptansicht der APP verwaltet (oder auch in der Regel eine **MainWindow.cs** -und **MainWindowController.cs** -Datei).

Gehen Sie folgendermaßen vor, um zu einem xibless-Fenster für eine Anwendung zu wechseln:

1. Öffnen Sie die Anwendung, die Sie nicht mehr `.storyboard` verwenden möchten, oder XIb-Dateien, um die Benutzeroberfläche in Visual Studio für Mac zu definieren.
2. Klicken Sie in der **Lösungspad**mit der rechten Maustaste auf die Datei " **Main. Storyboard** " oder " **MainWindow. XIb** ", und wählen Sie **Entfernen**aus:

    ![Entfernen des Haupt Storyboards oder Fensters](xibless-ui-images/switch01.png "Entfernen des Haupt Storyboards oder Fensters")
3. Klicken Sie im **Dialog Feld entfernen**auf die Schaltfläche **Löschen** , um die Storyboard-oder XIb-Datei vollständig aus dem Projekt zu entfernen:

    ![Bestätigen des Lösch] Vorgangs (xibless-ui-images/switch02.png "Bestätigen des Lösch") Vorgangs

Nun müssen wir die **MainWindow.cs** -Datei ändern, um das Fenster Layout zu definieren und die **ViewController.cs** -oder **MainWindowController.cs** -Datei zu ändern, um eine `MainWindow` Instanz der Klasse zu erstellen, da wir nicht mehr verwenden. Storyboard-oder XIb-Datei.

Moderne xamarin. Mac-apps, die Storyboards für Ihre Benutzeroberfläche verwenden, enthalten möglicherweise nicht automatisch die Dateien **MainWindow.cs**, **ViewController.cs** oder **MainWindowController.cs** . Fügen Sie dem Projekt einfach eine neue leere C# Klasse hinzu (neue Datei**Hinzufügen** >  **...**  > Allgemeineleere > **Klasse**), und benennen Sie Sie mit der fehlenden Datei.


### <a name="defining-the-window-in-code"></a>Definieren des Fensters im Code

Bearbeiten Sie als nächstes die Datei **MainWindow.cs** , und führen Sie Sie wie folgt aus:

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

Im folgenden werden einige wichtige Elemente erläutert.

Zuerst haben wir einige _berechnete Eigenschaften_ hinzugefügt, die wie Outlets fungieren (als ob das Fenster in einer Storyboard-oder XIb-Datei erstellt wurde):

```csharp
public NSButton ClickMeButton { get; set;}
public NSTextField ClickMeLabel { get ; set;}
```

Dadurch erhalten Sie Zugriff auf die Benutzeroberflächen Elemente, die im Fenster angezeigt werden. Da das Fenster nicht aus einer Storyboard-oder XIb-Datei aufgeblasen wird, benötigen wir eine Möglichkeit, es zu instanziieren (wie später in der `MainWindowController` -Klasse zu sehen ist). Diese neue Konstruktormethode führt Folgendes aus:

```csharp
public MainWindow(CGRect contentRect, NSWindowStyle aStyle, NSBackingStore bufferingType, bool deferCreation): base (contentRect, aStyle,bufferingType,deferCreation) {
    ...
}
```

Hier entwerfen wir das Layout des Fensters und platzieren alle Elemente der Benutzeroberfläche, die zum Erstellen der erforderlichen Benutzeroberfläche benötigt werden. Bevor Sie einem Fenster beliebige Benutzeroberflächen Elemente hinzufügen können, muss eine _Inhaltsansicht_ vorhanden sein, die die Elemente enthält:

```csharp
ContentView = new NSView (Frame);
```

Dadurch wird eine Inhaltsansicht erstellt, in der das Fenster ausgefüllt wird. Nun fügen wir das erste Benutzeroberflächen Element, `NSButton`ein, dem Fenster hinzu:

```csharp
ClickMeButton = new NSButton (new CGRect (10, Frame.Height-70, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
ContentView.AddSubview (ClickMeButton);
```

Der erste Punkt ist, dass macOS im Gegensatz zu IOS die Mathematische Notation verwendet, um das Fenster Koordinatensystem zu definieren. Daher befindet sich der Ursprungs Punkt in der unteren linken Ecke des Fensters, wobei die Werte rechts und in der oberen rechten Ecke des Fensters zunehmen. Wenn wir die neue `NSButton`erstellen, berücksichtigen wir dies bei der Definition der Position und Größe auf dem Bildschirm.

Die `AutoresizingMask = NSViewResizingMask.MinYMargin` -Eigenschaft teilt der Schaltfläche mit, dass Sie an derselben Position am oberen Rand des Fensters bleiben soll, wenn die Größe des Fensters vertikal geändert wird. Dies ist auch dann erforderlich, wenn sich (0,0) unten links im Fenster befindet.

`ContentView.AddSubview (ClickMeButton)` Schließlich`NSButton` fügt die-Methode der Inhaltsansicht hinzu, sodass Sie auf dem Bildschirm angezeigt wird, wenn die Anwendung ausgeführt wird und das Fenster angezeigt wird.

Im nächsten Schritt wird dem Fenster eine Bezeichnung hinzugefügt, in der angezeigt wird, wie `NSButton` oft auf das geklickt wurde:

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

Da macOS nicht über ein bestimmtes Benutzeroberflächen Element der _Bezeichnung_ verfügt, haben wir eine speziell formatierte, nicht `NSTextField` bearbeitbare Funktion hinzugefügt, die als Bezeichnung fungiert. Genau wie die Schaltfläche vor berücksichtigt Größe und Position, dass sich (0,0) unten links im Fenster befindet. Die `AutoresizingMask = NSViewResizingMask.WidthSizable | NSViewResizingMask.MinYMargin` -Eigenschaft verwendet den **or** -Operator, um `NSViewResizingMask` zwei-Funktionen zu kombinieren. Dadurch bleibt die Bezeichnung am oberen Rand des Fensters am gleichen Ort, wenn die Größe des Fensters vertikal geändert wird, und die Größe wird verkleinert und vergrößert, wenn die Größe des Fensters horizontal angepasst wird.

Die- `NSTextField` Methode fügt der Inhaltsansicht erneut hinzu, sodass Sie auf dem Bildschirm angezeigt wird, wenn die Anwendung ausgeführt und das Fenster geöffnet wird. `ContentView.AddSubview (ClickMeLabel)`


### <a name="adjusting-the-window-controller"></a>Anpassen des Fenster Controllers

Da der Entwurf von `MainWindow` nicht mehr aus einer Storyboard-oder XIb-Datei geladen wird, müssen wir einige Anpassungen am Fenster Controller vornehmen. Bearbeiten Sie die Datei **MainWindowController.cs** , und führen Sie Sie wie folgt aus:

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

Erörtern Sie die wesentlichen Elemente dieser Änderung.

Zuerst definieren wir eine neue Instanz der `MainWindow` -Klasse und weisen Sie der- `Window` Eigenschaft des Basis Fenster Controllers zu:

```csharp
CGRect contentRect = new CGRect (0, 0, 1000, 500);
base.Window = new MainWindow(contentRect, (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable), NSBackingStore.Buffered, false);
```

Der Speicherort des Fenster Fensters wird mit einem `CGRect`definiert. Ebenso wie das Koordinatensystem des Fensters definiert der Bildschirm (0,0) als untere linke Ecke. Als nächstes definieren wir den Stil des Fensters mithilfe des **or** -Operators, um zwei oder mehr `NSWindowStyle` Features zu kombinieren:

```csharp
... (NSWindowStyle.Titled | NSWindowStyle.Closable | NSWindowStyle.Miniaturizable | NSWindowStyle.Resizable) ...
```

Die folgenden `NSWindowStyle` Funktionen sind verfügbar:

- **Borderless** : das Fenster weist keinen Rahmen auf.
- **Mit dem Titel** : im Fenster wird eine Titelleiste angezeigt.
- **Closable** : das Fenster verfügt über eine Schaltfläche zum Schließen und kann geschlossen werden.
- **Miniaturierbar** : das Fenster verfügt über die Schaltfläche "minimieren" und kann minimiert werden.
- **Größe** kann geändert werden: das Fenster hat eine Schaltfläche zum Ändern der Größe und kann die Größe ändern.
- **Hilfsprogramm** : das Fenster ist ein Fenster des hilfsprogrammstils (Panel).
- **Docmodal** : Wenn es sich bei dem Fenster um einen Bereich handelt, wird es als dokumentmodaler anstelle eines systemmodalen verwendet.
- **Nonactivatingpanel** : Wenn das Fenster ein Panel ist, wird es nicht zum Hauptfenster.
- **Texturedbackground** : das Fenster verfügt über einen strukturierten Hintergrund.
- Nicht **skaliert** : das Fenster wird nicht skaliert.
- **Unifedtitleandtoolbar** : der Titel und die Symbolleisten Bereiche des Fensters werden verknüpft.
- **HUD** : das Fenster wird als Heads-Up-Anzeige Panel angezeigt.
- **Fullscreenwindow** : das Fenster kann in den Vollbildmodus wechseln.
- **Fullsizecontentview** : die Inhaltsansicht des Fensters befindet sich hinter dem Titel und dem Symbolleisten Bereich.

Die letzten beiden Eigenschaften definieren den _pufferungstyp_ für das Fenster und, wenn das Zeichnen des Fensters verzögert wird. Weitere Informationen zu finden `NSWindows`Sie in der Dokumentation [zu Apple Introduction to Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1) .

Da das Fenster nicht aus einer Storyboard-oder XIb-Datei aufgeblasen wird, muss es schließlich in unserer **MainWindowController.cs** simuliert werden, indem die Windows `AwakeFromNib` -Methode aufgerufen wird:

```csharp
Window.AwakeFromNib ();
```

Dies ermöglicht es Ihnen, wie ein Standardfenster, das aus einer. Storyboard-oder XIb-Datei geladen wird, mit dem-Fenster zu codieren.


### <a name="displaying-the-window"></a>Anzeigen des Fensters

Wenn die Storyboard-oder XIb-Datei entfernt wurde und die Dateien **MainWindow.cs** und **MainWindowController.cs** geändert wurden, verwenden Sie das Fenster genauso wie jedes normale Fenster, das in der Interface Builder von Xcode mit einer XIb-Datei erstellt wurde.

Im folgenden Beispiel wird eine neue Instanz des-Fensters und dessen Controller erstellt und das Fenster auf dem Bildschirm angezeigt:

```csharp
private MainWindowController mainWindowController;
...

mainWindowController = new MainWindowController ();
mainWindowController.Window.MakeKeyAndOrderFront (this);
```

Wenn die Anwendung ausgeführt wird und die Schaltfläche mehrmals geklickt hat, wird Folgendes angezeigt:

![Ein Beispiel für eine APP-Laufzeit](xibless-ui-images/run01.png "Ein Beispiel für eine APP-Laufzeit")


## <a name="adding-a-code-only-window"></a>Hinzufügen eines nur-Code-Fensters

Wenn Sie einer vorhandenen xamarin. Mac-Anwendung ein Code only, xibless-Fenster hinzufügen möchten, klicken Sie im **Lösungspad** mit der rechten Maustaste auf das Projekt, und wählen Sie**neue Datei** **Hinzufügen** > ... aus. Wählen Sie im Dialogfeld **neue Datei** die Option **xamarin. Mac** > **Cocoa-Fenster mit Controller**aus, wie unten dargestellt:

![Hinzufügen eines neuen Fenster Controllers](xibless-ui-images/add01.png "Hinzufügen eines neuen Fenster Controllers")

Ebenso wie zuvor löschen wir die default. Storyboard-oder XIb-Datei aus dem Projekt (in diesem Fall **secondwindow. XIb**) und führen die Schritte im Abschnitt [Wechseln eines Fensters zur Verwendung von Code](#Switching_a_Window_to_use_Code) aus, um die Definition des Fensters in Code abzudecken.


## <a name="adding-a-ui-element-to-a-window-in-code"></a>Hinzufügen eines UI-Elements zu einem Fenster im Code

Unabhängig davon, ob ein Fenster im Code erstellt oder aus einer Storyboard-oder XIb-Datei geladen wurde, kann es vorkommen, dass ein Benutzeroberflächen Element aus dem Code einem Fenster hinzugefügt werden soll. Beispiel:

```csharp
var ClickMeButton = new NSButton (new CGRect (10, 10, 100, 30)){
    AutoresizingMask = NSViewResizingMask.MinYMargin
};
MyWindow.ContentView.AddSubview (ClickMeButton);
```

Der obige Code erstellt einen neuen `NSButton` und fügt ihn der `MyWindow` Fenster Instanz zur Anzeige hinzu. Im Grunde können alle Benutzeroberflächen Elemente, die in Xcode-Interface Builder in einer. Storyboard-oder XIb-Datei definiert werden können, im Code erstellt und in einem-Fenster angezeigt werden.


## <a name="defining-the-menu-bar-in-code"></a>Definieren der Menüleiste im Code

Aufgrund der aktuellen Einschränkungen in xamarin. Mac ist es nicht empfehlenswert, die Menüleiste der xamarin. Mac-Anwendung –`NSMenuBar`– im Code zu erstellen, aber Sie können die Datei " **Main. Storyboard** " oder " **MainMenu. XIb** " weiterhin verwenden, um Sie zu definieren. Das heißt, Sie können Menüs und Menü Elemente im C# Code hinzufügen und entfernen.

Bearbeiten Sie z. b. die Datei **AppDelegate.cs** , `DidFinishLaunching` und führen Sie die folgende Methode aus:

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

Der obige Code erstellt ein Status leisten Menü aus dem Code und zeigt es an, wenn die Anwendung gestartet wird. Weitere Informationen zum Arbeiten mit Menüs finden Sie in unserer [Menüs](~/mac/user-interface/menu.md) -Dokumentation.


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie die Benutzeroberfläche einer xamarin. Mac-Anwendung im C# Code erstellen, anstatt die Interface Builder von Xcode mit. Storyboard-oder XIb-Dateien zu verwenden.



## <a name="related-links"></a>Verwandte Links

- [Macxibless (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macxibless)
- [Windows](~/mac/user-interface/window.md)
- [Menüs](~/mac/user-interface/menu.md)
- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
- [Einführung in Windows](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/WinPanel/Introduction.html)
