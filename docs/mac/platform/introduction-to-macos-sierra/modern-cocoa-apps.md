---
title: Erstellen moderner macOS-Apps
description: In diesem Artikel werden verschiedene Tipps, Features und Techniken behandelt, die ein Entwickler zum Erstellen einer modernen macOS-app in xamarin. Mac verwenden kann.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 0ddf6cfc26e811505a50c2d89596f830658f0c1d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73029905"
---
# <a name="building-modern-macos-apps"></a>Erstellen moderner macOS-Apps

_In diesem Artikel werden verschiedene Tipps, Features und Techniken behandelt, die ein Entwickler zum Erstellen einer modernen macOS-app in xamarin. Mac verwenden kann._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Modernes Aussehen mit modernen Ansichten

Ein modernes Erscheinungsbild enthält ein modernes Fenster und Symbolleisten Darstellung, wie z. b. die unten gezeigte Beispiel-App:

[![](modern-cocoa-apps-images/content08.png "An example of a modern Mac app UI")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>Aktivieren von Inhalts Ansichten mit voller Größenanpassung

Um dies zu erreichen, wird in einer xamarin. Mac-app der Entwickler eine _Inhaltsansicht mit vollständiger Größe_verwenden, was bedeutet, dass sich der Inhalt in den Bereichen "Tool" und "Titelleiste" erstreckt und automatisch von macOS verwischt wird.

Um dieses Feature im Code zu aktivieren, erstellen Sie eine benutzerdefinierte Klasse für den `NSWindowController`, und machen Sie Sie wie folgt:

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

Diese Funktion kann auch im Interface Builder von Xcode aktiviert werden, indem Sie das Fenster auswählen und die **Inhaltsansicht in voller Größen**Ordnung überprüfen:

[![](modern-cocoa-apps-images/content01.png "Editing the main storyboard in Xcode's Interface Builder")](modern-cocoa-apps-images/content01.png#lightbox)

Wenn eine Inhaltsansicht in voller Größe verwendet wird, muss der Entwickler den Inhalt möglicherweise unterhalb der Titel-und Symbolleisten Bereiche verschieben, damit spezifischer Inhalt (z. b. Bezeichnungen) nicht darunter verschoben wird.

Um dieses Problem zu erschweren, können die Titel-und Symbolleisten Bereiche eine dynamische Höhe aufweisen, die auf der Aktion basiert, die der Benutzer gerade ausführt, der Version von macOS, die der Benutzer installiert hat, und/oder der Mac-Hardware, auf der die app ausgeführt wird.

Daher kann der Offset beim Anordnen der Benutzeroberfläche nicht einfach hart codiert werden. Der Entwickler muss einen dynamischen Ansatz treffen.

Apple hat die [Observable-Eigenschaft für den Schlüssel-Wert-](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` der `NSWindow`-Klasse eingeschlossen, um den aktuellen Inhalts Bereich im Code zu erhalten. Der Entwickler kann diesen Wert verwenden, um die erforderlichen Elemente manuell zu positionieren, wenn sich der Inhalts Bereich ändert.

Die bessere Lösung besteht darin, die Benutzeroberflächen Elemente mithilfe der Klassen für automatisches Layout und Größe entweder in Code oder Interface Builder zu positionieren.

Code wie im folgenden Beispiel kann verwendet werden, um Benutzeroberflächen Elemente mithilfe der Klassen "AutoLayout" und "size" im Ansichts Controller der APP zu positionieren:

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

Dieser Code erstellt Speicher für eine Top-Einschränkung, die auf eine Bezeichnung (`ItemTitle`) angewendet wird, um sicherzustellen, dass Sie nicht im Titel-und Symbolleisten Bereich verrutscht:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

Wenn Sie die `UpdateViewConstraints`-Methode des Ansichts Controllers überschreiben, kann der Entwickler testen, ob die erforderliche Einschränkung bereits erstellt wurde, und Sie bei Bedarf erstellen.

Wenn eine neue Einschränkung erstellt werden muss, wird der Zugriff auf die `ContentLayoutGuide`-Eigenschaft des Fensters, auf das das Steuerelement beschränkt werden muss, benötigt und in ein `NSLayoutGuide`umgewandelt:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

Auf die topanchor-Eigenschaft des `NSLayoutGuide` wird zugegriffen, und wenn Sie verfügbar ist, wird Sie verwendet, um eine neue Einschränkung mit der gewünschten Offset Menge zu erstellen, und die neue Einschränkung wird aktiviert, um Sie anzuwenden:

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>Aktivieren von optimierten Symbolleisten

Ein normales macOS-Fenster enthält eine Standard Titelleiste an der Ausführung bis zum oberen Rand des Fensters. Wenn das Fenster auch eine Symbolleiste enthält, wird es unter diesem Titelleisten Bereich angezeigt:

[![](modern-cocoa-apps-images/content02.png "A standard Mac Toolbar")](modern-cocoa-apps-images/content02.png#lightbox)

Wenn Sie eine optimierte Symbolleiste verwenden, wird der Titelbereich ausgeblendet, und die Symbolleiste wechselt in die Position der Titelleiste, Inline mit den Schaltflächen Fenster schließen, minimieren und maximieren:

[![](modern-cocoa-apps-images/content03.png "A streamlined Mac Toolbar")](modern-cocoa-apps-images/content03.png#lightbox)

Die optimierte Symbolleiste wird durch Überschreiben der `ViewWillAppear`-Methode des `NSViewController` aktiviert und sieht wie folgt aus:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Enable streamlined Toolbars
    View.Window.TitleVisibility = NSWindowTitleVisibility.Hidden;
}
```

Dieser Effekt wird in der Regel für _Shoebox-Anwendungen_ (eine Window-APP) wie Karten, Kalender, Notizen und System Einstellungen verwendet. 

<a name="Using-Accessory-View-Controllers" />

### <a name="using-accessory-view-controllers"></a>Verwenden von Zubehör Ansichts Controllern

Abhängig vom Entwurf der APP möchte der Entwickler möglicherweise auch den Titelleisten Bereich durch einen Zubehör Ansichts Controller ergänzen, der direkt unterhalb des Titels/der Symbolleiste angezeigt wird, um dem Benutzer kontextabhängige Steuerelemente basierend auf der Aktivität bereitzustellen, die Sie sind. derzeit in:

[![](modern-cocoa-apps-images/content04.png "An example Accessory View Controller")](modern-cocoa-apps-images/content04.png#lightbox)

Der Zubehör Ansichts Controller wird vom System ohne Eingriff des Entwicklers automatisch verwischt und seine Größe geändert.

Gehen Sie folgendermaßen vor, um einen Zubehör Ansichts Controller hinzuzufügen:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
2. Ziehen Sie einen **benutzerdefinierten Ansichts Controller** in die Hierarchie des Fensters: 

    [![](modern-cocoa-apps-images/content05.png "Adding a new Custom View Controller")](modern-cocoa-apps-images/content05.png#lightbox)
3. Layout der Benutzeroberfläche der Zubehör Ansicht: 

    [![](modern-cocoa-apps-images/content06.png "Designing the new view")](modern-cocoa-apps-images/content06.png#lightbox)
4. Machen Sie die Zubehör Ansicht als **Outlet** und alle anderen **Aktionen** oder **Outlets** für die Benutzeroberfläche verfügbar: 

    [![](modern-cocoa-apps-images/content07.png "Adding the required OUtlet")](modern-cocoa-apps-images/content07.png#lightbox)
5. Speichern Sie die Änderungen.
6. Kehren Sie zu Visual Studio für Mac zurück, um die Änderungen zu synchronisieren.

Bearbeiten Sie die `NSWindowController`, und führen Sie Sie wie folgt aus:

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

Die wichtigsten Punkte dieses Codes sind der Ort, an dem die Sicht auf die benutzerdefinierte Ansicht festgelegt ist, die in Interface Builder definiert und als **Outlet**verfügbar gemacht wurde:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

Und der `LayoutAttribute`, der definiert, _wo_ das Zubehör angezeigt wird:

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Da macOS nun vollständig lokalisiert ist, wurden die Eigenschaften `Left` und `Right` `NSLayoutAttribute` als veraltet markiert und sollten durch `Leading` und `Trailing`ersetzt werden.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>Verwenden von Fenstern im Registerkarten Format

Außerdem kann das macOS-System dem Fenster der APP Zubehör Ansichts Controller hinzufügen. Beispielsweise zum Erstellen von Fenstern im Registerkarten Format, in denen mehrere der App-Fenster in einem virtuellen Fenster zusammengeführt werden:

[![](modern-cocoa-apps-images/content08.png "An example of a tabbed Mac Window")](modern-cocoa-apps-images/content08.png#lightbox)

Der Entwickler muss in der Regel eingeschränkte Maßnahmen zum Verwenden von Fenstern im Registerkarten Format in ihren xamarin. Mac-Apps verwenden. das System behandelt diese automatisch wie folgt:

- Wenn die `OrderFront`-Methode aufgerufen wird, wird Windows automatisch im Registerkarten Format angezeigt.
- Wenn die `OrderOut`-Methode aufgerufen wird, wird die Registerkarte automatisch in den Registerkarten Format aufgelöst.
- Im Code werden alle Fenster im Registerkarten Format weiterhin als "sichtbar" betrachtet, aber alle nicht vordersten Registerkarten werden vom System mithilfe von CoreGraphics ausgeblendet.
- Verwenden Sie die `TabbingIdentifier`-Eigenschaft von `NSWindow`, um Fenster in Registerkarten zu gruppieren.
- Wenn es sich um eine `NSDocument` basierte App handelt, werden einige dieser Features automatisch aktiviert (z. b. die Schaltfläche "Plus", die der Registerkarten Leiste hinzugefügt wird), ohne dass Entwickler Aktionen ausgeführt werden.
- Nicht`NSDocument` basierte Apps können die Schaltfläche "Plus" in der Registerkarten Gruppe aktivieren, um ein neues Dokument hinzuzufügen, indem Sie die `GetNewWindowForTab`-Methode der `NSWindowsController`überschreiben.

Wenn alle Teile zusammengeführt werden, kann der `AppDelegate` einer APP, die System basierte Fenster im Registerkarten Format verwenden wollte, wie folgt aussehen:

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

Mit der `NewDocumentNumber`-Eigenschaft wird die Anzahl der erstellten neuen Dokumente nachverfolgt, und die `NewDocument`-Methode erstellt ein neues Dokument und zeigt es an.

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

Gibt an, wo die statische `App` Eigenschaft eine Verknüpfung bereitstellt, um die `AppDelegate`zu erhalten. Die `SetDefaultDocumentTitle`-Methode legt basierend auf der Anzahl der erstellten neuen Dokumente einen neuen Dokumententitel fest.

Der folgende Code weist macOS an, dass die APP die Verwendung von Registerkarten bevorzugt und eine Zeichenfolge bereitstellt, mit der die Fenster der app in Registerkarten gruppiert werden können:

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

Mit der folgenden Überschreibungs Methode wird der Registerkarten Leiste eine Plus-Schaltfläche hinzugefügt, die ein neues Dokument erstellt, wenn der Benutzer darauf klickt:

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>Verwenden der Core-Animation

Die Core-Animation ist eine hochleistungsfähige Grafik Rendering-Engine, die in macOS integriert ist. Die Core-Animation wurde optimiert, um die GPU (Graphics Processing Unit) zu nutzen, die in moderner macOS-Hardware verfügbar ist, anstatt die Grafik Vorgänge auf der CPU auszuführen, was den Computer verlangsamen kann.

Der `CALayer`, der von der Core-Animation bereitgestellt wird, kann für Aufgaben wie schnelles und flüssiges scrollen und Animationen verwendet werden. Die Benutzeroberfläche einer APP sollte aus mehreren unter Sichten und Ebenen bestehen, um die Vorteile der Kern Animation vollständig nutzen zu können.

Ein `CALayer`-Objekt bietet verschiedene Eigenschaften, mit denen der Entwickler steuern kann, was dem Benutzer angezeigt wird, z. b.:

- `Content` können eine `NSImage` oder `CGImage` sein, die den Inhalt der Ebene bereitstellt.
- `BackgroundColor`: legt die Hintergrundfarbe der Ebene als `CGColor`
- `BorderWidth` legt die Rahmenbreite fest.
- legt `BorderColor` die Rahmenfarbe fest.

Wenn Sie Kern Grafiken in der Benutzeroberfläche der App verwenden möchten, müssen Sie _ebenengestützte_ Ansichten verwenden, die Apple vorschlägt, dass der Entwickler in der Inhaltsansicht des Fensters immer aktivieren soll. Auf diese Weise erben alle untergeordneten Ansichten die Ebenenunterstützung ebenfalls automatisch.

Außerdem schlägt Apple die Verwendung von ebenengestützten Sichten vor, anstatt eine neue `CALayer` als untergeordnete Ebene hinzuzufügen, da das System mehrere der erforderlichen Einstellungen automatisch verarbeitet (z. b. die, die für eine Retina-Anzeige erforderlich sind).

Die Ebenenunterstützung kann aktiviert werden, indem die `WantsLayer` einer `NSView` auf `true` oder innerhalb des Interface Builder von Xcode unter dem **View Effects-Inspektor** festgelegt wird, indem die **kernanimations Ebene**überprüft wird:

[![](modern-cocoa-apps-images/content09.png "The View Effects Inspector")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Neuzeichnen von Sichten mit Ebenen

Ein weiterer wichtiger Schritt bei der Verwendung von ebenengestützten Sichten in einer xamarin. Mac-app ist das Festlegen des `LayerContentsRedrawPolicy` der `NSView` auf `OnSetNeedsDisplay` in der `NSViewController`. Beispiel:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Wenn der Entwickler diese Eigenschaft nicht festgelegt hat, wird die Ansicht neu gezeichnet, wenn sich der Frame Ursprung ändert, was aus Leistungsgründen nicht gewünscht ist. Wenn diese Eigenschaft `OnSetNeedsDisplay` auf festgelegt ist, muss der Entwickler `NeedsDisplay` manuell auf `true` festlegen, um zu erzwingen, dass der Inhalt neu gezeichnet wird.

Wenn eine Ansicht als geändert markiert ist, überprüft das System die `WantsUpdateLayer`-Eigenschaft der Sicht. Wenn `true` zurückgegeben wird, wird die `UpdateLayer`-Methode aufgerufen. andernfalls wird die `DrawRect`-Methode der Sicht aufgerufen, um den Inhalt der Sicht zu aktualisieren.

Apple hat die folgenden Vorschläge zum Aktualisieren von Ansichten von Ansichten, wenn dies erforderlich ist:

- Apple bevorzugt die Verwendung von `UpdateLater` über `DrawRect`, wenn dies möglich ist, da es eine deutliche Leistungssteigerung ermöglicht.
- Verwenden Sie dieselbe `layer.Contents` für Benutzeroberflächen Elemente, die ähnlich aussehen.
- Apple bevorzugt auch den Entwickler, um die Benutzeroberfläche mit Standardansichten wie `NSTextField`zu verfassen, wann immer dies möglich ist.

Um `UpdateLayer`zu verwenden, erstellen Sie eine benutzerdefinierte Klasse für die `NSView`, und machen Sie den Code wie folgt aussehen:

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

## <a name="using-modern-drag-and-drop"></a>Verwenden von modernem Drag & Drop

Um dem Benutzer eine moderne Drag & Drop-Benutzer Darstellung zu präsentieren, sollte der Entwickler _Drag_ -and-Drop-Vorgänge in den Drag & Drop-Vorgängen der APP anwenden. Bei Drag Flocking wird jede einzelne Datei bzw. jedes Element, das zuerst gezogen wird, als einzelnes Element angezeigt, das sich unter dem Cursor mit der Anzahl der Elemente gruppieren wird, während der Benutzer den Zieh Vorgang fortsetzt.

Wenn der Benutzer den Zieh Vorgang beendet, werden die einzelnen Elemente entflosse und kehren zu ihren ursprünglichen Speicherorten zurück.

Der folgende Beispielcode ermöglicht das ziehen per Drag & amp; Drop in eine benutzerdefinierte Ansicht:

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

Der Effekt wird durch das Senden aller gezogenen Elemente an die `BeginDraggingSession`-Methode der `NSView` als separates Element in einem Array erreicht.

Wenn Sie mit einem `NSTableView` oder `NSOutlineView`arbeiten, verwenden Sie die `PastboardWriterForRow`-Methode der `NSTableViewDataSource`-Klasse, um den Zieh Vorgang zu starten:

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

Dies ermöglicht es dem Entwickler, für jedes Element in der Tabelle, das gezogen wird, eine einzelne `NSDraggingItem` bereitzustellen, anstatt der älteren Methode `WriteRowsWith`, die alle Zeilen als eine einzelne Gruppe in das pasteboard schreiben.

Verwenden Sie beim Arbeiten mit `NSCollectionViews`erneut die `PasteboardWriterForItemAt`-Methode im Gegensatz zur `WriteItemsAt`-Methode, wenn der Zieh Vorgang beginnt.

Der Entwickler sollte immer vermeiden, große Dateien auf dem pasteboard zu platzieren. Neu in macOS Sierra ermöglicht es dem Entwickler, Verweise auf angegebene Dateien auf dem Zwischenablage zu _platzieren, die_ später erfüllt werden, wenn der Benutzer den Drop-Vorgang mithilfe der neuen Klassen "`NSFilePromiseProvider`" und "`NSFilePromiseReceiver`" abschließt.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>Verwenden der modernen Ereignisüberwachung

Bei einem Benutzeroberflächen Element (z. b. einem `NSButton`), das einem Titel oder einem Symbolleisten Bereich hinzugefügt wurde, sollte der Benutzer in der Lage sein, auf das Element zu klicken und ein Ereignis wie gewohnt zu auslösen (z. b. das Anzeigen eines Popup Fensters). Da sich das Element jedoch auch im Titel-oder Symbolleisten Bereich befindet, sollte der Benutzer auf das Element klicken und es ziehen können, um das Fenster ebenfalls zu verschieben.

Um dies im Code zu erreichen, erstellen Sie eine benutzerdefinierte Klasse für das Element (z. b. `NSButton`), und überschreiben Sie das `MouseDown`-Ereignis wie folgt:

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

In diesem Code wird die `TrackEventsMatching`-Methode der `NSWindow` verwendet, an die das UI-Element angefügt ist, um die `LeftMouseUp`-und `LeftMouseDragged` Ereignisse abzufangen. Bei einem `LeftMouseUp`-Ereignis antwortet das UI-Element wie gewohnt. Für das `LeftMouseDragged`-Ereignis wird das-Ereignis an die `PerformWindowDrag`-Methode des `NSWindow`weitergegeben, um das Fenster auf dem Bildschirm zu verschieben.

Der Aufruf der `PerformWindowDrag`-Methode der `NSWindow`-Klasse bietet die folgenden Vorteile:

- Dadurch kann das Fenster verschoben werden, auch wenn die APP nicht reagiert (z. b. bei der Verarbeitung einer Deep-Schleife).
- Der Speicherplatz Wechsel funktioniert erwartungsgemäß.
- Die Leertaste wird in normaler Form angezeigt.
- Das Ausrichten und Ausrichten von Fenstern funktioniert wie üblich.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>Verwenden von modernen Container Ansicht-Steuerelementen

macOS Sierra bietet viele moderne Verbesserungen an den vorhandenen Container Ansicht-Steuerelementen, die in der vorherigen Version des Betriebssystems verfügbar sind.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Erweiterungen der Tabellenansicht

Der Entwickler sollte immer die neue `NSView` basierte Version von Container View-Steuerelementen verwenden, z. b. `NSTableView`. Beispiel:

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

Dies ermöglicht das Anfügen benutzerdefinierter Tabellenzeilen Aktionen an bestimmte Zeilen in der Tabelle (z. b. das Löschen der Zeile nach rechts). Überschreiben Sie die `RowActions`-Methode des `NSTableViewDelegate`, um dieses Verhalten zu aktivieren:

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

Der statische `NSTableViewRowAction.FromStyle` wird verwendet, um eine neue Tabellenzeilen Aktion der folgenden Stile zu erstellen:

- `Regular`: führt eine nicht destruktive Standardaktion aus, z. b. den Inhalt der Zeile zu bearbeiten.
- `Destructive`: führt eine zerstörerische Aktion aus, z. b. das Löschen der Zeile aus der Tabelle. Diese Aktionen werden mit einem roten Hintergrund gerendert.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Erweiterungen der scrollansicht 

Wenn Sie eine Bild Lauf Ansicht (`NSScrollView`) direkt oder als Teil eines anderen Steuer Elements (z. b. `NSTableView`) verwenden, kann der Inhalt der Bild Lauf Ansicht unter den Bereichen Titel und Symbolleiste in einer xamarin. Mac-App mithilfe eines modernen Erscheinungsbild und mit Ansichten in einer xamarin. Mac-app

Folglich kann das erste Element im Inhalts Bereich der Bild Lauf Ansicht teilweise durch den Titel und den Werkzeugleisten Bereich verdeckt werden.

Um dieses Problem zu beheben, hat Apple der `NSScrollView`-Klasse zwei neue Eigenschaften hinzugefügt:

- `ContentInsets`: ermöglicht es dem Entwickler, ein `NSEdgeInsets` Objekt bereitzustellen, das den Offset definiert, der auf den oberen Rand der scrollansicht angewendet wird.
- `AutomaticallyAdjustsContentInsets`: Wenn `true` führt die scrollansicht automatisch die `ContentInsets` für den Entwickler aus.

Wenn Sie die `ContentInsets` verwenden, kann der Entwickler den Start der scrollansicht so anpassen, dass er das Einschließen von Zubehör ermöglicht, wie z. b.:

- Ein Sortier Indikator wie der, der in der Mail-App angezeigt wird.
- Ein Suchfeld.
- Schaltfläche "Aktualisieren" oder "Aktualisieren".

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Automatisches Layout und Lokalisierung in modernen apps

Apple hat mehrere Technologien in Xcode integriert, mit denen Entwickler problemlos eine internationalisierte macOS-app erstellen können. Xcode ermöglicht es dem Entwickler jetzt, den Benutzer seitigen Text vom Design der Benutzeroberfläche der app in seinen Storyboard-Dateien zu trennen, und stellt Tools bereit, um diese Trennung beizubehalten, wenn sich die Benutzeroberfläche ändert.

Weitere Informationen finden Sie in der [Internationalisierungs-und Lokalisierungs Anleitung](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html)von Apple.

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Implementieren der grundlegenden Internationalisierung 

Durch Implementieren der Basis Internationalisierung kann der Entwickler eine einzelne storyboarddatei bereitstellen, um die Benutzeroberfläche der APP darzustellen und alle Benutzer seitigen Zeichen folgen voneinander zu trennen. 

Wenn der Entwickler die anfängliche storyboarddatei (oder Dateien) erstellt, die die Benutzeroberfläche der app definieren, werden diese in der Basis Internationalisierung erstellt (die Sprache, die der Entwickler spricht).

Als nächstes kann der Entwickler Lokalisierungs-und Basis-Internationalisierungs Zeichenfolgen (im Storyboard-UI-Design) exportieren, die in mehrere Sprachen übersetzt werden können.

Später können diese lokzierungen importiert werden, und Xcode generiert die sprachspezifischen Zeichen folgen Dateien für das Storyboard.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Implementieren von automatischem Layout zur Unterstützung der Lokalisierung

Da lokalisierte Versionen von Zeichen folgen Werten eine sehr unterschiedliche Größe und/oder Leserichtung aufweisen können, sollte der Entwickler das automatische Layout verwenden, um die Benutzeroberfläche der app in einer Storyboard-Datei zu positionieren und zu verkleinern.

Apple schlägt vor, Folgendes zu tun:

- **Einschränkungen für eine Einschränkung mit fester Breite entfernen** : alle textbasierten Sichten sollten die Größe auf Grundlage ihres Inhalts ändern können. Die Ansicht mit fester Breite kann ihren Inhalt in bestimmten Sprachen angleichen.
- **Intrinsische Inhalts Größen verwenden** : Standardmäßig werden textbasierte Ansichten automatisch entsprechend ihren Inhalten angepasst. Wählen Sie für eine textbasierte Ansicht, die keine korrekte Größe hat, Sie in Xcode aus Interface Builder klicken Sie dann auf > Größe **Bearbeiten** **, um Inhalt anzupassen**.
- **Anwenden führender und** nachfolgender Attribute: Wenn die Textrichtung basierend auf der Sprache des Benutzers geändert werden kann, verwenden Sie die neuen `Leading`-und `Trailing` Einschränkungs Attribute im Gegensatz zu den vorhandenen `Right` und `Left` Attributen. `Leading` und `Trailing` werden automatisch basierend auf der Sprachen Richtung angepasst.
- **Anheften von Ansichten an angrenzende Ansichten** : Dadurch können die Ansichten geändert und die Größe geändert werden, wenn sich die Ansichten in der Antwort auf die ausgewählte Sprache ändern.
- **Legen Sie keine Windows-Mindestgröße und/oder maximale Größe fest** . lassen Sie Windows das Ändern der Größe zu, da die ausgewählte Sprache die Größe Ihrer Inhaltsbereiche ändert.
- Das **testlayoutlayout wird ständig geändert** . während der Entwicklung bei der APP sollten Sie ständig in verschiedenen Sprachen getestet werden. Weitere Informationen finden Sie in der Dokumentation zu den [Tests Ihrer internationalisierten App](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) von Apple.
- **Verwenden von nsstackviews zum** Anheften von Sichten - `NSStackViews` ermöglicht, dass Ihre Inhalte auf vorhersagbare Weise verschoben und vergrößert werden, und die Größe des Inhalts ändert sich basierend auf der ausgewählten Sprache.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Lokalisieren in Xcode-Interface Builder

Apple hat mehrere Features in der Interface Builder von Xcode bereitgestellt, die der Entwickler beim Entwerfen oder Bearbeiten der Benutzeroberfläche einer App zur Unterstützung der Lokalisierung verwenden kann. Der Abschnitt **Text Richtung** des **Attribut Inspektors** ermöglicht es dem Entwickler, Hinweise dazu bereitzustellen, wie die Richtung verwendet und in einer ausgewählten Text basierten Ansicht aktualisiert werden soll (z. b. `NSTextField`):

[![](modern-cocoa-apps-images/content10.png "The Text Direction options")](modern-cocoa-apps-images/content10.png#lightbox)

Es gibt drei mögliche Werte für die **Text Richtung**:

- **Natürlich** : das Layout basiert auf der Zeichenfolge, die dem Steuerelement zugewiesen ist.
- Von **Links nach rechts** : das Layout wird immer von links nach rechts erzwungen.
- **Von rechts nach links** : das Layout wird immer von rechts nach links erzwungen.

Es gibt zwei mögliche Werte für das **Layout**:

- **Von links nach rechts** : das Layout ist immer von links nach rechts.
- **Von rechts nach links** : das Layout ist immer von rechts nach links.

Diese sollten in der Regel nicht geändert werden, es sei denn, eine bestimmte Ausrichtung ist erforderlich.

Die **Spiegel** Eigenschaft weist das System an, bestimmte Steuerelement Eigenschaften (z. b. die Position des Zellen Bilds) zu kippen. Sie verfügt über drei mögliche Werte:

- **Automatisch** : die Position ändert sich automatisch abhängig von der Richtung der gewählten Sprache.
- **Rechts-nach-links-Schnittstelle** : die Position wird nur in der Sprache rechts in Links geändert.
- **Nie** : die Position ändert sich nie.

Wenn der Entwickler den Inhalt einer textbasierten Ansicht zentriert, **rechtfertigen** oder **vollständig** ausrichten **hat, werden**diese nicht basierend auf ausgewählter Sprache gekippt.

Vor macOS Sierra werden Steuerelemente, die im Code erstellt wurden, nicht automatisch gespiegelt. Der Entwickler musste Code wie den folgenden verwenden, um die Spiegelung zu behandeln:

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

, Wo die `Alignment` und `ImagePosition` basierend auf dem `UserInterfaceLayoutDirection` des Steuer Elements festgelegt werden.

macOS Sierra fügt mehrere neue handlerkonstruktoren (über die statische `CreateButton`-Methode) hinzu, die mehrere Parameter annehmen (z. b. Titel, Bild und Aktion) und automatisch widerspiegeln. Beispiel:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>Verwenden von System Erscheinungen

Moderne macOS-Apps können eine neue Darstellung der dunklen Oberfläche übernehmen, die sich gut für die Image Erstellung, Bearbeitung oder Präsentation von apps eignet:

[![](modern-cocoa-apps-images/content11.png "An example of a dark Mac Window UI")](modern-cocoa-apps-images/content11.png#lightbox)

Dies kann erreicht werden, indem eine Codezeile hinzugefügt wird, bevor das Fenster angezeigt wird. Beispiel:

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

Die statische `GetAppearance`-Methode der `NSAppearance`-Klasse wird verwendet, um eine benannte Darstellung vom System zu erhalten (in diesem Fall `NSAppearance.NameVibrantDark`).

Apple hat die folgenden Vorschläge für die Verwendung von System Erscheinungen:

- Bevorzugen Sie benannte Farben über hart codierte Werte (z. b. `LabelColor` und `SelectedControlColor`).
- Verwenden Sie nach Möglichkeit den System Standard-Steuerelement Stil.

Eine macOS-APP, die die System Auftritte verwendet, funktioniert automatisch ordnungsgemäß für Benutzer, die Barrierefreiheits Funktionen in der System Einstellungs-APP aktiviert haben. Apple schlägt daher vor, dass der Entwickler immer System Auftritte in seinen macOS-Apps verwenden sollte.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Entwerfen von Benutzeroberflächen mit Storyboards

Storyboards ermöglichen es dem Entwickler nicht, nur die einzelnen Elemente zu entwerfen, die die Benutzeroberfläche einer APP bilden, sondern den UI-Flow und die Hierarchie der angegebenen Elemente zu visualisieren und zu entwerfen.

Controller ermöglichen es dem Entwickler, Elemente in einer Zusammenfassungs Einheit und in einer abstrakter Struktur zu erfassen und den typischen "Verbindungs Code" zu entfernen, der für das Verschieben in der Ansichts Hierarchie erforderlich ist:

[![](modern-cocoa-apps-images/content12.png "Editing the UI in Xcode's Interface Builder")](modern-cocoa-apps-images/content12.png#lightbox)

Weitere Informationen finden Sie [in unserer Einführung in die Storyboards](~/mac/platform/storyboards/index.md) -Dokumentation.

Es gibt viele Instanzen, bei denen eine bestimmte Szene, die in einem Storyboard definiert ist, Daten aus einer vorherigen Szene in der Ansichts Hierarchie erfordert. Apple hat die folgenden Vorschläge zum Übergeben von Informationen zwischen den Kulissen:

- Daten abhängige Daten sollten immer abwärts durch die Hierarchie durchlaufen werden.
- Vermeiden Sie das hart codieren von UI-strukturellen Abhängigkeiten, da dies die Flexibilität der Benutzeroberfläche einschränkt.
- Verwenden C# Sie Schnittstellen, um generische Daten Abhängigkeiten bereitzustellen.

Der Ansichts Controller, der als Quelle des segue fungiert, kann die `PrepareForSegue` Methode überschreiben und jede erforderliche Initialisierung ausführen (z. b. das Übergeben von Daten), bevor der segue ausgeführt wird, um den Ziel Ansichts Controller anzuzeigen. Beispiel:

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

Weitere Informationen finden Sie [in unserer Dokumentation](~/mac/platform/storyboards/indepth.md#Segues) zu.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>Propagieren von Aktionen

Basierend auf dem Entwurf der macOS-App kann es vorkommen, dass sich der beste Handler für eine Aktion in einem UI-Steuerelement an anderer Stelle in der UI-Hierarchie befindet. Dies gilt in der Regel für Menüs und Menü Elemente, die sich in ihrer eigenen Szene befinden, getrennt von der restlichen Benutzeroberfläche der app.

Um diese Situation zu beheben, kann der Entwickler eine benutzerdefinierte Aktion erstellen und die Aktion an die Antwort Kette übergeben. Weitere Informationen finden Sie in der Dokumentation [Arbeiten mit benutzerdefinierten Fenster Aktionen](~/mac/user-interface/menu.md) .

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Moderne Mac-Features

Apple verfügt über mehrere benutzerorientierte Features in macOS Sierra, die es dem Entwickler ermöglichen, die Mac-Plattform optimal zu nutzen, z. b.:

- **Nsuseractivity** : Dies ermöglicht der APP, die Aktivität zu beschreiben, an der der Benutzer zurzeit beteiligt ist. `NSUserActivity` ursprünglich zur Unterstützung von Handoff erstellt, wobei eine Aktivität, die auf einem der Geräte des Benutzers gestartet wurde, auf einem anderen Gerät übernommen und fortgesetzt werden konnte. `NSUserActivity` funktioniert in macOS genauso wie in ios. Weitere Informationen finden Sie in der [Einführung](~/ios/platform/handoff.md) in die IOS-Dokumentation.
- **Siri auf Mac** -Siri verwendet die aktuelle Aktivität (`NSUserActivity`), um Kontext für die Befehle bereitzustellen, die ein Benutzer ausgeben kann.
- **Wiederherstellung des Zustands** : Wenn der Benutzer eine APP unter macOS beendet und später neu startet, wird die APP automatisch in den vorherigen Zustand zurückversetzt. Der Entwickler kann die Zustands Wiederherstellungs-API verwenden, um vorübergehende UI-Zustände zu codieren und wiederherzustellen, bevor die Benutzeroberfläche für den Benutzer angezeigt wird. Wenn die APP `NSDocument` basiert, wird die Zustands Wiederherstellung automatisch durchgeführt. Legen Sie zum Aktivieren der Zustands Wiederherstellung für nicht`NSDocument` basierte Apps die `Restorable` der `NSWindow`-Klasse auf `true`fest.
- **Dokumente in der Cloud** : vor macOS Sierra musste eine APP explizit die Arbeit mit Dokumenten im icloud-Laufwerk des Benutzers abonnieren. In macOS Sierra die Ordner Desktop und **Dokumente** des Benutzers möglicherweise automatisch vom System mit dem icloud **-** Laufwerk synchronisiert. Daher können lokale Kopien von Dokumenten gelöscht werden, um Speicherplatz auf dem Computer des Benutzers freizugeben. Diese Änderung wird von `NSDocument` basierten Apps automatisch verarbeitet. Alle anderen APP-Typen müssen eine `NSFileCoordinator` zum Synchronisieren von Lese-und Schreib vordokumenten verwenden.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden mehrere Tipps, Features und Techniken behandelt, die ein Entwickler zum Erstellen einer modernen macOS-app in xamarin. Mac verwenden kann.

## <a name="related-links"></a>Verwandte Links

- [macOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Mac)
