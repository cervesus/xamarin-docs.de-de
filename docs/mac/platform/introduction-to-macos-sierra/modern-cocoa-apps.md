---
title: Erstellen von modernen MacOS Apps
description: Dieser Artikel behandelt einige Tipps, Funktionen und Techniken, die ein Entwickler zum Erstellen einer app moderner MacOS in Xamarin.Mac verwenden kann.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 4eb4ff4a9e4784d816e2cbe8734e0422573cad92
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="building-modern-macos-apps"></a>Erstellen von modernen MacOS Apps

_Dieser Artikel behandelt einige Tipps, Funktionen und Techniken, die ein Entwickler zum Erstellen einer app moderner MacOS in Xamarin.Mac verwenden kann._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Erstellen von modernen sieht mit modernen Ansichten

Ein modernes Erscheinungsbild gehören eine moderne Fenster und Symbolleisten Darstellung wie z. B. der unten gezeigten Beispiel-app:

[![](modern-cocoa-apps-images/content08.png "Ein Beispiel für eine moderne Mac app-Benutzeroberfläche")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>Aktivieren die vollständige Größe Content-Ansichten

Um sieht in einer app Xamarin.Mac zu erreichen, möchte der Entwickler verwenden ein _Größe Inhalt Vollbilds_, d. h., die Inhalte unter den Bereichen Tool und Titelleiste erweitert und wird automatisch vom MacOS verzerrt werden.

Um diese Funktion im Code zu aktivieren, erstellen Sie eine benutzerdefinierte Klasse für die `NSWindowController` , und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Set window to use Full Size Content View
            Window.StyleMask = NSWindowStyle.FullSizeContentView;
        }
        #endregion
    }
}
```

Diese Funktion kann ebenfalls in Xcodes Benutzeroberflächen-Generator aktiviert werden, indem Sie die Fenster auswählen, und überprüfen **vollständige Größe Inhaltsansicht**:

[![](modern-cocoa-apps-images/content01.png "Die Haupt-Storyboard in Xcodes Benutzeroberflächen-Generator bearbeiten")](modern-cocoa-apps-images/content01.png#lightbox)

Wenn Sie eine vollständige Ansicht der Größe Inhalt zu verwenden, muss der Entwickler kann den Inhalt unter den Titel und Tool-Leiste Bereichen zu versetzen, sodass bestimmte Inhaltstypen (wie Beschriftungen) darunter Folie nicht.

Um dieses Problem zu erschweren, können die Bereiche für Titel und die Symbolleiste eine dynamische Höhe basierend auf die Aktion, die der Benutzer derzeit ausgeführt werden, haben die Version des MacOS, die der Benutzer hat installiert und/oder die Mac-Hardware, die die app ausgeführt wird.

Den Offset einfach hart codieren, beim Layout der Benutzeroberfläche wird daher nicht funktionieren. Entwickler müssen einen dynamischen Ansatz gewählt.

Apple noch Bestandteil der [Observable Schlüssel-Wert](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` Eigenschaft von der `NSWindow` Klasse, um die aktuellen Inhaltsbereichs in Code zu erhalten. Entwickler kann diesen Wert verwenden, um die erforderlichen Elemente manuell zu positionieren, wenn Inhaltsbereichs ändert.

Die bessere Lösung besteht darin Automatisches Layout und Größenklassen verwenden, um die Elemente der Benutzeroberfläche im Code oder Schnittstelle-Generator zu positionieren.

Code wie im folgenden Beispiel kann verwendet werden, um Benutzeroberflächenelemente, die mithilfe von AutoLayout und Größenklassen in der app-View-Controller einzufügen:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        #region Computed Properties
        public NSLayoutConstraint topConstraint { get; set; }
        #endregion

        ...

        #region Override Methods
        public override void UpdateViewConstraints ()
        {
            // Has the constraint already been set?
            if (topConstraint == null) {
                // Get the top anchor point
                var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
                var topAnchor = contentLayoutGuide.TopAnchor;

                // Found?
                if (topAnchor != null) {
                    // Assemble constraint and activate it
                    topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
                    topConstraint.Active = true;
                }
            }

            base.UpdateViewConstraints ();
        }
        #endregion
    }
}
```

Dieser Code erstellt den Speicher für eine Top-Einschränkung, die auf eine Bezeichnung angewendet werden soll (`ItemTitle`), stellen Sie sicher, dass er unter dem Titel und Symbolleiste Bereich Verteiler nicht:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

Durch Überschreiben der View-Controller `UpdateViewConstraints` -Methode testen, um festzustellen, ob die erforderliche Einschränkung bereits erstellt wurde und kann bei Bedarf erstellen.

Wenn eine neue Einschränkung erstellt werden sollen, muss die `ContentLayoutGuide` Eigenschaft des Fensters wird das Steuerelement, das einzuschränkende muss zugegriffen und umgewandelt in ein `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

Die TopAnchor-Eigenschaft von der `NSLayoutGuide` zugegriffen wird, und sofern dieser verfügbar ist, wird verwendet, um eine neue Einschränkung mit der gewünschten Offset zu erstellen und die neue Einschränkung erfolgt aktiv sein, um sie anzuwenden:

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>Aktivieren optimierte Symbolleisten

Eine normale MacOS Fenster umfasst einen Standard-Titelleiste am oberen Rand des Fensters ausgeführt, zusammen wird. Wenn das Fenster auch eine Symbolleiste enthält, wird es unter dieser Bereich der Titelleiste angezeigt:

[![](modern-cocoa-apps-images/content02.png "Eine standard-Mac-Symbolleiste")](modern-cocoa-apps-images/content02.png#lightbox)

Wenn mithilfe einer optimierten Symbolleiste Titelbereichs nicht mehr angezeigt wird, und der Symbolleiste in der Titelleiste Position nach oben, inline-mit den Schaltflächen im Fenster schließen, minimieren und maximieren:

[![](modern-cocoa-apps-images/content03.png "Eine optimierte Mac-Symbolleiste")](modern-cocoa-apps-images/content03.png#lightbox)

Der optimierte Symbolleiste aktiviert ist, durch Überschreiben der `ViewWillAppear` Methode der `NSViewController` und somit zu suchen, wie im folgenden:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

Dieser Effekt wird normalerweise zum _Shoebox Anwendungen_ (ein Fenster-apps), wie Karten, Kalender, Hinweise und Systemeinstellungen. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>Mithilfe von Controllern Zubehörs anzeigen

Je nach Entwurf der app möchte der Entwickler auch zur Ergänzung der Titelleiste Bereich mit einer Zubehör-View-Controller, die unter dem Titel/Symbolleiste Bereich bereitstellen kontextabhängig steuert, die dem Benutzer basierend auf der Aktivität sie rechts angezeigt werden. die zurzeit an:

[![](modern-cocoa-apps-images/content04.png "Ein Beispiel für Zubehör-View-Controller")](modern-cocoa-apps-images/content04.png#lightbox)

Die Zubehör-View-Controller werden automatisch unscharf und vom System ohne Eingriffe angepasst.

Um ein Zubehör-View-Controller hinzuzufügen, führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
2. Ziehen Sie eine **Custom View-Controller** in das Fenster Hierarchie: 

    [![](modern-cocoa-apps-images/content05.png "Hinzufügen einer neuen benutzerdefinierten-View-Controller")](modern-cocoa-apps-images/content05.png#lightbox)
3. Layout der Zubehörs Ansicht UI: 

    [![](modern-cocoa-apps-images/content06.png "Entwerfen die neue Sicht")](modern-cocoa-apps-images/content06.png#lightbox)
4. Verfügbarmachen die Zubehörs Ansicht als ein **Steckdose** und alle weiteren **Aktionen** oder **Steckdosen** für ihre Benutzeroberfläche: 

    [![](modern-cocoa-apps-images/content07.png "Hinzufügen den erforderlichen Ausgang")](modern-cocoa-apps-images/content07.png#lightbox)
5. Speichern Sie die Änderungen.
6. Zurück zu Visual Studio für Mac Änderungen synchronisiert.

Bearbeiten der `NSWindowController` , und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }
        #endregion
    }
}
```

Die wichtigsten Punkte dieses Codes sind, die Ansicht auf der benutzerdefinierten Sicht festgelegt ist, die in der Benutzeroberflächen-Generator definiert und als verfügbar gemacht wurde ein **Nachrichtenplattform**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

Und die `LayoutAttribute` , definiert _, in dem_ der Accessor wird angezeigt:

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Da MacOS jetzt vollständig lokalisiert wird, die `Left` und `Right` `NSLayoutAttribute` Eigenschaften sind veraltet und sollten ersetzt werden, mit `Leading` und `Trailing`.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>Verwenden von Fenstern im Registerformat

Darüber hinaus möglicherweise das System MacOS Zubehör-View-Controller für die app-Fenster hinzufügen. Geben Sie beispielsweise Folgendes ein, um das Fenster im Registerkartenformat erstellt, in dem mehrere Windows die App in einem Fenster "virtuelle" zusammengeführt werden:

[![](modern-cocoa-apps-images/content08.png "Ein Beispiel für einen Macintosh-Fenster im Registerkartenformat")](modern-cocoa-apps-images/content08.png#lightbox)

In der Regel Entwickler müssen eingeschränkten Aktion verwenden im Registerformat Windows in ihren apps Xamarin.Mac schalten, das System automatisch wie folgt behandeln:

- Windows verwendet, werden automatisch im Registerkartenformat, wenn die `OrderFront` Methode aufgerufen wird.
- Windows verwendet, werden automatisch Untabbed, wenn die `OrderOut` Methode aufgerufen wird.
- Im Code, die alle im Registerformat Fenster weiterhin als "sichtbar" betrachtet werden, werden alle Registerkarten nicht vordersten jedoch durch das System über CoreGraphics ausgeblendet.
- Verwenden der `TabbingIdentifier` Eigenschaft `NSWindow` Gruppe Windows zusammen in Registerkarten.
- Wird jedoch ein `NSDocument` je app, einige dieser Funktionen wird ohne Eingreifen Developer automatisch (z. B. das Plussymbol (+) hinzugefügt wird, auf die Registerkartenleiste) aktiviert werden.
- Nicht-`NSDocument` basierend apps können die Schaltfläche "plus" in der Gruppe von Registerkarten zum Hinzufügen eines neuen Dokuments durch Überschreiben der `GetNewWindowForTab` Methode der `NSWindowsController`.

Schalten alle Teile zusammen, die `AppDelegate` einer App, die verwenden wollten, systembasierte im Registerkartenformat Windows kann wie folgt aussehen:

```csharp
using AppKit;
using Foundation;

namespace MacModern
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewDocumentNumber { get; set; } = 0;
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidFinishLaunching (NSNotification notification)
        {
            // Insert code here to initialize your application
        }

        public override void WillTerminate (NSNotification notification)
        {
            // Insert code here to tear down your application
        }
        #endregion

        #region Custom Actions
        [Export ("newDocument:")]
        public void NewDocument (NSObject sender)
        {
            // Get new window
            var storyboard = NSStoryboard.FromName ("Main", null);
            var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

            // Display
            controller.ShowWindow (this);
        } 
        #endregion
    }
}
```

In dem die `NewDocumentNumber` Eigenschaft behält den Überblick über der Anzahl der neu erstellten Dokumente und die `NewDocument` Methode ein neues Dokument erstellt und angezeigt.

Die `NSWindowController` könnte dann wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainWindowController : NSWindowController
    {
        #region Application Access
        /// <summary>
        /// A helper shortcut to the app delegate.
        /// </summary>
        /// <value>The app.</value>
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Constructor
        public MainWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void SetDefaultDocumentTitle ()
        {
            // Is this the first document?
            if (App.NewDocumentNumber == 0) {
                // Yes, set title and increment
                Window.Title = "Untitled";
                ++App.NewDocumentNumber;
            } else {
                // No, show title and count
                Window.Title = $"Untitled {App.NewDocumentNumber++}";
            }
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Prefer Tabbed Windows
            Window.TabbingMode = NSWindowTabbingMode.Preferred;
            Window.TabbingIdentifier = "Main";

            // Set default window title
            SetDefaultDocumentTitle ();

            // Set window to use Full Size Content View
            // Window.StyleMask = NSWindowStyle.FullSizeContentView;

            // Create a title bar accessory view controller and attach
            // the view created in Interface Builder
            var accessoryView = new NSTitlebarAccessoryViewController ();
            accessoryView.View = AccessoryViewGoBar;

            // Set the location and attach the accessory view to the
            // titlebar to be displayed
            accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
            Window.AddTitlebarAccessoryViewController (accessoryView);

        }

        public override void GetNewWindowForTab (NSObject sender)
        {
            // Ask app to open a new document window
            App.NewDocument (this);
        }
        #endregion
    }
}
```

In dem die statische `App` Eigenschaft verfügt über eine Tastenkombination, um auf die `AppDelegate`. Die `SetDefaultDocumentTitle` Methode legt einen neuen Dokumente Titel basierend auf der Anzahl der neu erstellten Dokumente.

Der folgende Code weist MacOS, die die app Registerkarten bevorzugt und stellt eine Zeichenfolge, die die app von Windows in Registerkarten gruppiert werden kann:

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

Und die folgende Methode für die Außerkraftsetzung der Registerkartenleiste, das Erstellen eines neuen Dokuments, wenn der Benutzer darauf klickt, wird ein Plussymbol (+) hinzugefügt:

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>Mit der Animation Core

Core-Animation ist eine hohe Leistung Graphics-Renderingmodul, das in MacOS integriert ist. Core-Animation wurde optimiert, um nutzen, die GPU (Graphics Processing Unit) in der modernen MacOS Hardware im Gegensatz zum Ausführen der Grafikoperationen für die CPU des Computers verlangsamt verfügbar.

Die `CALayer`, Core Animation angegeben wurde, können für Aufgaben wie z. B. schnellen und flüssigen Durchführen eines Bildlaufs und Animationen verwendet werden. Mehrere Unteransichten und Ebenen Core Animation vollständig nutzen sollte einer app-Benutzeroberfläche erstellt werden.

Ein `CALayer` Objekt stellt mehrere Eigenschaften, die den Entwickler, die steuern, was angezeigt wird wie auf dem Bildschirm, für den Benutzer zulassen:

- `Content` -Kann eine `NSImage` oder `CGImage` , der den Inhalt der Ebene bereitstellt.
- `BackgroundColor` -Legt die Hintergrundfarbe der Ebene als ein `CGColor`
- `BorderWidth` – Legt die Breite des Rahmens fest.
- `BorderColor` – Legt die Farbe des Rahmens fest.

Um Core Grafiken in der Benutzeroberfläche der Anwendung zu nutzen, es muss verwenden _Ebene gesichert_ Ansichten, was Apple darauf hinweist, dass der Entwickler in des Fensters Inhaltsansicht immer aktiviert werden sollte. Auf diese Weise alle untergeordneten Ansichten automatisch sichern Ebene als auch erben.

Darüber hinaus Apple empfiehlt die Verwendung von Ebene gesichert Ansichten im Gegensatz zum Hinzufügen eines neuen `CALayer` als eine Unterebene, da das System automatisch einige der erforderlichen Einstellungen (z. B. die von einem Retina-Display) behandelt.

Ebene sichern kann aktiviert werden, durch Festlegen der `WantsLayer` des eine `NSView` auf `true` oder innerhalb Xcodes Schnittstelle-Generator unter der **Ansicht Effekte Inspektor** durch Überprüfen **Core Animation Ebene**:

[![](modern-cocoa-apps-images/content09.png "Die Ansicht Effekte Inspektor")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Durch das Neuzeichnen erhalten Ansichten mit Ebenen

Ein weiterer wichtiger Schritt beim Verwenden von Ebene gesichert Ansichten in einer app Xamarin.Mac festlegen, wird die `LayerContentsRedrawPolicy` von der `NSView` auf `OnSetNeedsDisplay` in der `NSViewController`. Zum Beispiel:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Wenn der Entwickler diese Eigenschaft festgelegt wird nicht, die Ansicht wird neu gezeichnet werden bei jeder Änderung Ursprungssite Frame, der nicht zur Verbesserung der Leistung erwünscht ist. Mit dieser Eigenschaft `OnSetNeedsDisplay` der Entwickler müssen manuell festlegen `NeedsDisplay` auf `true` erzwingen Sie den Inhalt an, jedoch neu gezeichnet werden.

Wenn eine Sicht als geändert markiert ist, überprüft das System die `WantsUpdateLayer` -Eigenschaft der Ansicht. Wenn zurückgegeben `true` die `UpdateLayer` Methode aufgerufen wird, andernfalls der `DrawRect` Methode der Ansicht aufgerufen, um den Inhalt der Ansicht zu aktualisieren.

Apple hat die folgenden Vorschläge für die Aktualisierung einer Ansichten Inhalt im Bedarfsfall an:

- Apple bevorzugt mit `UpdateLater` über `DrawRect` immer möglich, wie er bietet eine erhebliche Leistungssteigerung bewirken.
- Verwenden Sie den gleichen `layer.Contents` für Benutzeroberflächenelemente, die ähnlich aussehen.
- Apple bevorzugt auch den Entwickler, ihre Benutzeroberfläche mit Standardansichten wie verfassen `NSTextField`, wann immer möglich.

Mit `UpdateLayer`, erstellen Sie eine benutzerdefinierte Klasse für die `NSView` und den Code wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView
    {
        #region Computed Properties
        public override bool WantsLayer {
            get { return true; }
        }

        public override bool WantsUpdateLayer {
            get { return true; }
        }
        #endregion

        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void DrawRect (CoreGraphics.CGRect dirtyRect)
        {
            base.DrawRect (dirtyRect);

        }

        public override void UpdateLayer ()
        {
            base.UpdateLayer ();

            // Draw view 
            Layer.BackgroundColor = NSColor.Red.CGColor;
        }
        #endregion
    }
}
```

<a name="Using-Modern-Drag-and-Drop" />

## <a name="using-modern-drag-and-drop"></a>Mit modernen Drag & Drop

Um eine moderne Drag & Drop-Erfahrung für den Benutzer zu präsentieren, die Entwickler übernehmen _ziehen Flocking_ in ihrer app Drag & Drop-Operationen. Ziehen Sie Flocking ist, wobei jede einzelne Datei oder ein Element gezogenen anfänglich als ein einzelnes Element angezeigt, die Beständen (Gruppe zusammen unter dem Cursor mit der Anzahl der Elemente wird), wie der Benutzer den Ziehvorgang fortgesetzt wird.

Wenn der Benutzer den Ziehvorgang beendet wird, werden die einzelnen Elemente Unflock und zurück an ihren ursprünglichen Speicherorten.

Der folgende Beispielcode ermöglicht, ziehen Sie auf eine benutzerdefinierte Ansicht Flocking:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public partial class MainView : NSView, INSDraggingSource, INSDraggingDestination
    {
        #region Constructor
        public MainView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void MouseDragged (NSEvent theEvent)
        {
            // Create group of string to be dragged
            var string1 = new NSDraggingItem ((NSString)"Item 1");
            var string2 = new NSDraggingItem ((NSString)"Item 2");
            var string3 = new NSDraggingItem ((NSString)"Item 3");

            // Drag a cluster of items
            BeginDraggingSession (new [] { string1, string2, string3 }, theEvent, this);
        }
        #endregion
    }
}
```

Flocking Effekt wurde erreicht, senden jedes Element gezogen wird, um die `BeginDraggingSession` Methode der `NSView` als separates Element in einem Array.

Beim Arbeiten mit einem `NSTableView` oder `NSOutlineView`, verwenden die `PastboardWriterForRow` Methode der `NSTableViewDataSource` Klasse Starten des Vorgangs ziehen:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDataSource: NSTableViewDataSource
    {
        #region Constructors
        public ContentsTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override INSPasteboardWriting GetPasteboardWriterForRow (NSTableView tableView, nint row)
        {
            // Return required pasteboard writer
            ...
            
            // Pasteboard writer failed
            return null;
        }
        #endregion
    }
}
```

Dies ermöglicht es dem Entwickler, geben Sie ein einzelnes `NSDraggingItem` für jedes Element in der Tabelle, die im Gegensatz zum älteren Methode gezogen wird `WriteRowsWith` , die alle Zeilen als einzelne Gruppe in die Zwischenablage schreiben.

Bei der Arbeit mit `NSCollectionViews`, erneut verwenden die `PasteboardWriterForItemAt` Methode im Gegensatz zu den `WriteItemsAt` Methode beim Ziehen beginnt.

Der Entwickler sollte immer vermeiden Sie große Dateien auf die Zwischenablage. Erfahrung mit MacOS Sierra, _Datei Zusagen_ ermöglichen den Entwickler, platzieren Sie Verweise auf angegebenen Dateien in die Zwischenablage, die später erfüllt wird, wenn der Benutzer mit den Drop-Vorgang mit dem neuen beendet `NSFilePromiseProvider` und `NSFilePromiseReceiver` Klassen.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>Mithilfe der modernen Ereignis nachverfolgung

Für ein Element der Benutzeroberfläche (z. B. eine `NSButton`) hinzugefügt wurde in einen Titel oder die Symbolleiste, der Benutzer sollte in der Lage, klicken Sie auf das Element, und dass er das Auslösen eines Ereignisses wie gewohnt (z. B. das Anzeigen einer Popup-Fenster) sein. Jedoch, da das Element auch in den Titel "oder" Symbolleiste "ist, sollte der Benutzer Lage klicken und ziehen das Element, um das Fenster auch verschieben.

Um dies im Code zu erreichen, erstellen Sie eine benutzerdefinierte Klasse für das Element (z. B. `NSButton`), und überschreiben die `MouseDown` Ereignis wie folgt:

```csharp
public override void MouseDown (NSEvent theEvent)
{
    var shouldCallSuper = false;

    Window.TrackEventsMatching (NSEventMask.LeftMouseUp, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Handle event as normal
        stop = true;
        shouldCallSuper = true;
    });

    Window.TrackEventsMatching(NSEventMask.LeftMouseDragged, 2000, NSRunLoop.NSRunLoopEventTracking, (NSEvent evt, ref bool stop) => {
        // Pass drag event to window
        stop = true;
        Window.PerformWindowDrag (evt);
    });

    // Call super to handle mousedown
    if (shouldCallSuper) {
        base.MouseDown (theEvent);
    }
}
```

Dieser Code verwendet die `TrackEventsMatching` Methode der `NSWindow` , die das Benutzeroberflächenelement zum Abfangen angefügt ist die `LeftMouseUp` und `LeftMouseDragged` Ereignisse. Für eine `LeftMouseUp` Ereignis, das Benutzeroberflächenelement reagiert wie gewohnt. Für die `LeftMouseDragged` Ereignis, das Ereignis wird zum Übergeben der `NSWindow`des `PerformWindowDrag` Methode, um das Fenster auf dem Bildschirm zu verschieben.

Aufrufen der `PerformWindowDrag` Methode der `NSWindow` Klasse bietet folgende Vorteile:

- Für das Fenster zu verschieben, ermöglicht, selbst wenn die app nicht reagiert, wird (z. B. wenn eine umfassende Schleife verarbeitet).
- Wechseln von Speicherplatz funktioniert wie erwartet.
- Die Leiste Leerzeichen werden wie gewohnt angezeigt.
- Fenster andocken und Ausrichtung funktionieren wie gewohnt.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>Verwenden moderne Container-Steuerelemente

MacOS bietet Sierra viele moderne Verbesserungen an der vorhandene Container Ansicht Steuerelemente in der vorherigen Version des Betriebssystems verfügbar.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Systemsichterweiterungen für Tabelle

Der Entwickler sollte immer verwenden Sie die neue `NSView` Version des Container-Steuerelemente wie z. B. basierend `NSTableView`. Zum Beispiel:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }
        #endregion
    }
}
```

Dadurch können für benutzerdefinierte Aktionen für die Tabelle Zeile an die angegebenen Zeilen in der Tabelle (z. B. ein Lesegerät Rechte zum Löschen der Zeile) angefügt werden. Um dieses Verhalten zu aktivieren, außer Kraft setzen der `RowActions` Methode der `NSTableViewDelegate`:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace MacModern
{
    public class ContentsTableDelegate : NSTableViewDelegate
    {
        #region Constructors
        public ContentsTableDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // Build new view
            var view = new NSView ();
            ...

            // Return the view representing the item to display
            return view;
        }

        public override NSTableViewRowAction [] RowActions (NSTableView tableView, nint row, NSTableRowActionEdge edge)
        {
            // Take action based on the edge
            if (edge == NSTableRowActionEdge.Trailing) {
                // Create row actions
                var editAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Regular, "Edit", (action, rowNum) => {
                    // Handle row being edited
                    ...
                });

                var deleteAction = NSTableViewRowAction.FromStyle (NSTableViewRowActionStyle.Destructive, "Delete", (action, rowNum) => {
                    // Handle row being deleted
                    ...
                });

                // Return actions
                return new [] { editAction, deleteAction };
            } else {
                // No matching actions
                return null;
            }
        }
        #endregion
    }
}
```

Die statische `NSTableViewRowAction.FromStyle` wird verwendet, um eine neue Tabelle Zeilenaktion der folgenden Eigenschaften erstellen:

- `Regular` – Führt eine zerstörungsfreier Standardaktionen, z. B. Bearbeiten Inhalt der Zeile.
- `Destructive` – Führt einen destruktiven Vorgang beispielsweise ' löschen ' die Zeile aus der Tabelle. Diese Aktionen werden mit rotem Hintergrund dargestellt.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Führen Sie einen Bildlauf Systemsichterweiterungen 

Bei Verwendung einer Sicht Scroll (`NSScrollView`) direkt oder als Teil eines anderen Steuerelements (z. B. `NSTableView`), den Inhalt der Ansicht Scroll können unter den Titel und die Symbolleiste Bereichen in einer Xamarin.Mac-app, die mit einer modernen suchen und Ansichten schieben.

Daher kann das erste Element im Inhaltsbereich Scroll-Sicht durch den Titel und die Symbolleiste Bereich teilweise verdeckt werden.

Um dieses Problem zu beheben, Apple zwei neue Eigenschaften hinzugefügt hat die `NSScrollView` Klasse:

- `ContentInsets` – Ermöglicht es den Entwickler, geben Sie einen `NSEdgeInsets` Objekt, das des Offsets, die an den Anfang der Scroll-Sicht angewendet werden.
- `AutomaticallyAdjustsContentInsets` -If `true` Scroll Ansicht behandelt automatisch die `ContentInsets` für den Entwickler.

Mithilfe der `ContentInsets` der Entwickler kann den Beginn der Ansicht einen Bildlauf um z. B. für die Aufnahme von Zubehör ermöglichen anpassen:

- Ein Indikator sortieren wie in der e-Mail-app dargestellt.
- Das Suchfeld.
- Eine Aktualisierung oder Update-Schaltfläche.

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Automatisches Layout und Lokalisierung in moderne Apps

Apple hat mehrere Technologien enthalten, in Xcode, mit denen den Entwickler auf einfache Weise eine internationalisierte MacOS-app erstellen können. Xcode ist nun ermöglicht es dem Entwickler, die Benutzer gegenübersteht Text aus der app-Benutzeroberfläche Entwurf in seiner Storyboard-Dateien und bietet Tools, um diese Trennung zu gewährleisten, wenn die Benutzeroberfläche ändert.

Weitere Informationen finden Sie in der Apple- [Internationalisierung und Lokalisierung Handbuch](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html).

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Implementieren grundlegende Internationalisierung 

Durch die Implementierung Base Internationalisierung können kann der Entwickler eine einzelne Storyboarddatei zum Darstellen der Benutzeroberfläche der Anwendung und alle Zeichenfolgen benutzerseitige Aussondern bereitstellen. 

Wenn der Entwickler ist das Erstellen der anfänglichen Storyboard-Datei (oder Dateien), die der app-Benutzeroberfläche definieren, in die Basis-Internationalisierung (die Sprache, die der Entwickler spricht) erstellt.

Als Nächstes kann der Entwickler exportieren, lokalisierten Versionen und die Basis-Internationalisierung-Zeichenfolgen (im Entwurf Storyboard-UI), die in mehreren Sprachen übersetzt werden können.

Später wird diese lokalisierten Versionen können importiert werden, sodass Xcode generiert die sprachspezifische Zeichenfolge-Dateien für das Storyboard.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Implementieren Automatisches Layout zum unterstützen der Lokalisierung

Da lokalisierte Versionen der Zeichenfolge Werte völlig unterschiedliche Größen aufweisen können, und/oder Richtung gelesen wurden, sollte der Entwickler Automatisches Layout, position und Größe der app-Benutzeroberfläche in eine Storyboarddatei verwenden.

Apple vorschlagen auf folgende Weise:

- **Feste Breite Einschränkungen entfernen** -alle textbasierten Ansichten darf Größe anhand ihres Inhalts. Feste Breite Ansicht möglicherweise ihre Inhalte in bestimmten Sprachen zuzuschneiden.
- **Verwenden der systeminternen Inhalt Größen** - von textbasierten Ansichten werden automatisch-Standardgröße an ihren Inhalt an. Für textbasierte Sicht, die Größe nicht korrekt sind, wählen Sie sie in Xcodes Benutzeroberflächen-Generator aus und wählen Sie dann **bearbeiten** > **Größe an Inhalt anpassen**.
- **Führende und nachfolgende Attribute gelten** : Da die Richtung des Texts zu ändern, kann basierend auf der Sprache des Benutzers, verwenden Sie die neue `Leading` und `Trailing` Einschränkung Attribute im Gegensatz zu den vorhandenen `Right` und `Left` Attribute. `Leading` und `Trailing` wird automatisch basierend auf die Richtung Sprachen angepasst.
- **PIN-Ansichten mit benachbarten Ansichten** – Dies ermöglicht die Ansichten aus, die Position und Größe wie die Ansichten darum als Antwort auf die ausgewählte Sprache ändern.
- **Nicht festgelegt werden, ein Windows-Minimum und/oder eine maximale Größe** -Windows Größe zu ändern, wie die zuvor ausgewählte Sprache ihre Inhaltsbereiche ändert.
- **Layout Änderungen ständig testen** – während der Entwicklung zur app in verschiedenen Sprachen ständig getestet werden sollten. Finden Sie in der Apple- [app Testen Ihrer Internationalized](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) Dokumentation.
- **NSStackViews, Pin Sichten zusammen verwenden**  -  `NSStackViews` ermöglicht, deren Inhalt auf die UMSCHALTTASTE gedrückt halten und vorhersagbar Vergrößerung und dem Inhalt des Größe, die basierend auf der ausgewählten Sprache ändern.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Lokalisieren von in Xcode des Benutzeroberflächen-Generator

Apple bereitgestellt hat mehrere Funktionen in Xcodes Benutzeroberflächen-Generator, der Entwickler zum unterstützen der Lokalisierung beim Entwerfen oder bearbeiten die Benutzeroberfläche einer Anwendung verwenden kann. Die **Textrichtung** Teil der **Attribut Inspektor** ermöglicht es dem Entwickler zum Bereitstellen von Hinweisen auf wie Richtung verwendet und auf eine SELECT-Anweisung textbasierten Sicht aktualisiert werden soll (z. B. `NSTextField`):

[![](modern-cocoa-apps-images/content10.png "Die Textrichtung-Optionen")](modern-cocoa-apps-images/content10.png#lightbox)

Es gibt drei mögliche Werte für die **Textrichtung**:

- **Natürliche** -das Layout basiert auf dem Steuerelement zugewiesene Zeichenfolge.
- **Von links nach rechts** -Layout immer gezwungen, von links nach rechts.
- **Von rechts nach links** -Layout immer gezwungen, rechts nach links.

Es gibt zwei mögliche Werte für die **Layout**:

- **Von links nach rechts** -das Layout wird immer von links nach rechts.
- **Von rechts nach links** -es ist immer das Layout von rechts nach links.

Diese sollte in der Regel nicht geändert werden, wenn eine bestimmte Ausrichtung erforderlich ist.

Die **Spiegel** Eigenschaft weist das System zu spiegelnde bestimmte Steuerelementeigenschaften (z. B. die Zellposition Image). Es sind drei Werte möglich:

- **Automatisch** -die Position wird automatisch basierend auf die Richtung der ausgewählten Sprache ändern.
- **Rechts auf Links Benutzeroberfläche** -die Position wird nur in rechts nach links basierend Sprachen geändert werden.
- **Nie** -ändern Sie die Position wird nie.

Wenn der Entwickler angegeben hat **Center**, **Blocksatz** oder **vollständige** Ausrichtung auf den Inhalt einer textbasierten Sicht, diese werden nie gekippte basierend auf Sprache ausgewählt.

Vor dem MacOS Sierra würde Steuerelemente im Code erstellt nicht automatisch gespiegelt werden. Der Entwickler mussten Code wie folgt verwenden, um die Spiegelung zu behandeln:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Setting a button's mirroring based on the layout direction
    var button = new NSButton ();
    if (button.UserInterfaceLayoutDirection == NSUserInterfaceLayoutDirection.LeftToRight) {
        button.Alignment = NSTextAlignment.Right;
        button.ImagePosition = NSCellImagePosition.ImageLeft;
    } else {
        button.Alignment = NSTextAlignment.Left;
        button.ImagePosition = NSCellImagePosition.ImageRight;
    }
}
```

In dem die `Alignment` und `ImagePosition` sind einzurichtenden basierend auf den `UserInterfaceLayoutDirection` des Steuerelements.

MacOS Sierra fügt mehrere neue halber Konstruktoren (über die statische `CreateButton` Methode) verwenden Sie mehrere Parameter (z. B. "Title", "Image" und "Aktion") und wird automatisch korrekt widerspiegeln. Zum Beispiel:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>Verwenden System-Darstellung

MacOS moderne apps können eine neue dunkel Schnittstelle Darstellung zu verwenden, die für Image erstellen, bearbeiten oder eine Präsentation apps gut funktioniert:

[![](modern-cocoa-apps-images/content11.png "Ein Beispiel für eine dunkle Mac-Fenster-Benutzeroberfläche")](modern-cocoa-apps-images/content11.png#lightbox)

Dies kann erfolgen, indem Sie eine Codezeile hinzufügen, bevor das Fenster angezeigt wird. Zum Beispiel:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacModern
{
    public partial class ViewController : NSViewController
    {
        ...
    
        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Apply the Dark Interface Appearance
            View.Window.Appearance = NSAppearance.GetAppearance (NSAppearance.NameVibrantDark);

            ...
        }
        #endregion
    }
}
```

Die statische `GetAppearance` Methode der `NSAppearance` Klasse wird verwendet, um eine benannte Darstellung vom System abgerufen werden (in diesem Fall `NSAppearance.NameVibrantDark`).

Apple hat die folgenden Vorschläge für die Verwendung von System aussehen:

- Ziehen Sie benannte Farben, die hartcodierte Werte (z. B. `LabelColor` und `SelectedControlColor`).
- Verwenden Sie möglichst System Standardsteuerelement Stil.

Eine MacOS-app, die System-Eindeutigkeitsmetrik verwendet, funktioniert automatisch für Benutzer ordnungsgemäß, die Barrierefreiheitsfunktionen von den Systemeinstellungen app aktiviert haben. Folglich schlägt Apple an, dass der Entwickler System Eindeutigkeitsmetrik immer in ihren apps MacOS verwenden soll.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Entwerfen von Benutzeroberflächen mit Storyboards

Storyboards ermöglichen den Entwickler, nicht nur Entwurfsmuster für die einzelnen Elemente, richten Sie einer app-Benutzeroberfläche, aber zum Visualisieren und Entwerfen der Benutzeroberflächenautomatisierungs übertragen und die Hierarchie der angegebenen Elemente.

Domänencontroller, dass der Entwickler zum Zusammenfassen von Elementen in eine Einheit der Zusammensetzung und Segues abstrakt und entfernen Sie die typischen "Kleben Code" erforderlich, um in der gesamten Hierarchie Ansicht verschieben:

[![](modern-cocoa-apps-images/content12.png "Bearbeiten die Benutzeroberfläche in Xcode des Benutzeroberflächen-Generator")](modern-cocoa-apps-images/content12.png#lightbox)

Weitere Informationen finden Sie unter unsere [Einführung in Storyboards](~/mac/platform/storyboards/index.md) Dokumentation.

Es gibt viele Instanzen, in dem angegebenen themawechsel definiert, die in einem Storyboard Daten aus einer vorherigen Szene in der Hierarchie anzeigen benötigen. Apple hat die folgenden Vorschläge für die Übergabe von Informationen zwischen Szenen:

- Daten Dependancies sollte immer in der Hierarchie abwärts kaskadiert werden.
- Vermeiden Sie hartcodierten UI strukturellen Dependancies, wie dies UI flexibel beschränkt.
- Verwenden Sie C#-Schnittstellen, um allgemeine Daten Dependancies bereitzustellen.

View-Controller, der als Quelle für die Segue fungiert können außer Kraft setzen die `PrepareForSegue` Methode "und" Do Initialisierungen, die (z. B. das Übergeben von Daten) erforderlich ist, bevor die Segue anzuzeigenden Ziel View Controller ausgeführt wird. Zum Beispiel:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on Segue ID
    switch (segue.Identifier) {
    case "MyNamedSegue":
        // Prepare for the segue to happen
        ...
        break;
    }
}
```

Weitere Informationen finden Sie unter unsere [Segues](~/mac/platform/storyboards/indepth.md#Segues) Dokumentation.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>Weitergeben von Aktionen

Basierend auf den Entwurf der app MacOS kann möglicherweise der beste Handler für eine Aktion auf ein UI-Steuerelement in an anderer Stelle in der Hierarchie der Benutzeroberfläche werden kann. Dies ist in der Regel "true", Menüs und Menüelemente, die in ihren eigenen Szene, getrennt von den Rest der Benutzeroberfläche der Anwendung befinden.

Um diese Situation zu behandeln, kann der Entwickler eine benutzerdefinierte Aktion zu erstellen und übergeben die Aktion der Beantworter aktivitätenkette. Weitere Informationen finden Sie unter unsere [arbeiten mit benutzerdefinierten Fensteraktionen](~/mac/user-interface/menu.md) Dokumentation.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Moderne Mac-Funktionen

Apple hat mehrere Benutzer sichtbaren Funktionen enthalten, in MacOS Sierra, mit denen den Entwickler einen Großteil der Macintosh-Plattform, z. B. vornehmen können:

- **NSUserActivity** -Dadurch kann die Anwendung aus, um die Aktivität zu beschreiben, die der Benutzer derzeit beteiligt ist. `NSUserActivity` ursprünglich erstellt wurde Übergabe, unterstützt werden, in dem eine Aktivität, die auf einem der Geräte des Benutzers gestartet übernommen und auf einem anderen Gerät fortgesetzt werden konnte. `NSUserActivity` funktioniert in MacOS, wie er in iOS wird daher bitte unsere [Einführung in die Übergabe](~/ios/platform/handoff.md) iOS-Dokumentation.
- **Siri auf Mac** -Siri verwendet die aktuelle Aktivität (`NSUserActivity`) um den Kontext auf die Befehle bereitzustellen, kann ein Benutzer ausstellen.
- **Status der Wiederherstellung** : Wenn der Benutzer eine app auf MacOS und dann später relaunches sie die app wird automatisch beendet in den vorherigen Zustand zurückversetzt werden. Entwickler können die Wiederherstellung Sitzungsstatus-API zum Codieren und Übergangszustände Benutzeroberfläche wiederherzustellen, bevor die Benutzeroberfläche für den Benutzer angezeigt wird. Wenn die app ist `NSDocument` basiert, ist Wiederherstellung des Benutzerzustand automatisch behandelt. So aktivieren Sie die Wiederherstellung des Systemstatus für nicht-`NSDocument` legen Sie die Basis-apps die `Restorable` von der `NSWindow` Klasse `true`.
- **Dokumente in der Cloud** -MacOS Sierra, eine app mussten explizit opt-in mit dem Arbeiten mit Dokumenten in iCloud für den Benutzer Laufwerk. In MacOS Sierra Benutzer **Desktop** und **Dokumente** Ordner möglicherweise vom System automatisch mit ihren iCloud Laufwerk synchronisiert werden. Folglich können lokale Kopien von Dokumenten gelöscht werden, um Speicherplatz auf dem Computer des Benutzers. `NSDocument` Basis-apps werden automatisch diese Änderung nicht verarbeiten. App-Typen müssen verwenden eine `NSFileCoordinator` zum Lesen und Schreiben von Dokumenten zu synchronisieren.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt einige Tipps, Funktionen und Techniken, die ein Entwickler zum Erstellen einer app moderner MacOS in Xamarin.Mac verwenden kann.



## <a name="related-links"></a>Verwandte Links

- [MacOS-Beispiele](https://developer.xamarin.com/samples/mac/)
