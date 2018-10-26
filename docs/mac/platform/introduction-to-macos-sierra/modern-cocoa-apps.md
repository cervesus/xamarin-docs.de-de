---
title: Erstellen moderner MacOS-Apps
description: Dieser Artikel behandelt einige Tipps, Funktionen und Techniken, mit denen ein Entwickler eine moderner MacOS-app in Xamarin.Mac zu erstellen.
ms.prod: xamarin
ms.assetid: F20EE590-246E-40EB-B309-D9D8C090C7F1
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 53bfc9f147b6cf369b8f5ce8d1257dbaf6b0f807
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50113520"
---
# <a name="building-modern-macos-apps"></a>Erstellen moderner MacOS-Apps

_Dieser Artikel behandelt einige Tipps, Funktionen und Techniken, mit denen ein Entwickler eine moderner MacOS-app in Xamarin.Mac zu erstellen._

<a name="Building-Modern-Looks-with-Modern-Views" />

## <a name="building-modern-looks-with-modern-views"></a>Erstellen von modernen sieht mit modernen Ansichten

Ein modernes Erscheinungsbild umfasst eine moderne Fenster und Symbolleiste Darstellung wie z. B. die unten gezeigten Beispiel-app:

[![](modern-cocoa-apps-images/content08.png "Ein Beispiel für eine moderne Mac-app-Benutzeroberfläche")](modern-cocoa-apps-images/content08.png#lightbox)

<a name="Enabling-Full-Sized-Content-Views" />

### <a name="enabling-full-sized-content-views"></a>Aktivieren die vollständige Größe Content-Ansichten

Um sieht in einer Xamarin.Mac-app zu erreichen, sollten sich der Entwickler verwenden eine _vollständige Größe Inhalt Ansicht_, d. h. die Inhalte erweitern, in den Bereichen Tool und die Titelleiste und wird automatisch von Mac OS verwischt werden.

Um dieses Feature im Code zu aktivieren, erstellen Sie eine benutzerdefinierte Klasse für den `NSWindowController` und legen Sie ihn wie folgt aussehen:

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

Dieses Feature kann auch in Interface Builder von Xcode aktiviert werden, indem Sie Sie im Fenster auswählen, und überprüfen **vollständige Größe Inhaltsansicht**:

[![](modern-cocoa-apps-images/content01.png "Bearbeiten die Haupt-Storyboard in Interface Builder von Xcode")](modern-cocoa-apps-images/content01.png#lightbox)

Wenn Sie eine vollständige Ansicht der Größe Inhalte verwenden zu können, müssen der Entwickler den Inhalt unter den Titel und Tool-Leiste Bereichen zu versetzen, sodass bestimmte Inhalte (wie Beschriftungen) unter diese Folie nicht.

Um dieses Problem zu verkomplizieren, haben die Titel und die Symbolleiste-Bereichen eine dynamische Höhe, die basierend auf die Aktion, die der Benutzer gerade ausführt, die Version von MacOS, die der Benutzer hat installiert bzw. die Mac-Hardware, die die app ausgeführt wird.

Den Offset einfach fest zu programmieren, wenn die Benutzeroberfläche wird daher nicht funktionieren. Der Entwickler muss einen dynamischen Ansatz.

Apple hat enthalten die [Observable Schlüssel-Wert](~/mac/app-fundamentals/databinding.md#Observing_Value_Changes) `ContentLayoutRect` Eigenschaft der `NSWindow` Klasse, um den aktuellen Inhaltsbereich im Code zu erhalten. Entwickler kann diesen Wert verwenden, um die erforderlichen Elemente manuell zu positionieren, wenn Sie den Inhaltsbereich ändert.

Die bessere Lösung ist auf Automatisches Layout und Größe-Klassen verwenden, um die Elemente der Benutzeroberfläche in Code oder in Interface Builder zu positionieren.

Code, wie im folgenden Beispiel kann verwendet werden, um UI-Elemente, die mit AutoLayout Größenklassen in der app-View-Controller zu positionieren:

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

Dieser Code erstellt den Speicher für eine Top-Einschränkung, die mit einer Bezeichnung angewendet werden (`ItemTitle`) um sicherzustellen, dass es unter dem Titel "und" Symbolleiste Bereich verzögern nicht:

```csharp
public NSLayoutConstraint topConstraint { get; set; }
```

Durch Überschreiben der Ansichtscontroller `UpdateViewConstraints` -Methode testen, um festzustellen, ob die erforderliche Einschränkung bereits erstellt wurde und kann bei Bedarf zu erstellen.

Wenn eine neue Einschränkung erstellt werden, muss die `ContentLayoutGuide` -Eigenschaft des Fensters auf die zugegriffen wird das Steuerelement, die eingeschränkt werden muss und Umwandeln in einen `NSLayoutGuide`:

```csharp
var contentLayoutGuide = ItemTitle.Window?.ContentLayoutGuide as NSLayoutGuide;
```

Die TopAnchor-Eigenschaft, der die `NSLayoutGuide` zugegriffen wird und wenn es verfügbar ist, er wird verwendet, um eine neue Einschränkung mit der gewünschten Offset zu erstellen und die neue Einschränkung zum aktiven Server gemacht, die sie anwenden:

```csharp
// Assemble constraint and activate it
topConstraint = topAnchor.ConstraintEqualToAnchor (topAnchor, 20);
topConstraint.Active = true;
```

<a name="Enabling-Streamlined-Toolbars" />

### <a name="enabling-streamlined-toolbars"></a>Optimierte Symbolleisten aktivieren

Ein normaler MacOS Fenster enthält einen Standard an, auf die, den Titelleiste am oberen Rand des Fensters wird. Wenn das Fenster auch eine Symbolleiste enthält, wird es in diesem Bereich der Titelleiste angezeigt:

[![](modern-cocoa-apps-images/content02.png "Eine standard-Mac-Symbolleiste")](modern-cocoa-apps-images/content02.png#lightbox)

Wenn über eine optimierte Symbolleiste, den Titelbereich nicht mehr angezeigt wird, und der Symbolleiste in der Titelleiste Position nach oben, inline-mit den Schaltflächen im Fenster schließen, minimieren und maximieren:

[![](modern-cocoa-apps-images/content03.png "Eine optimierte Mac-Symbolleiste")](modern-cocoa-apps-images/content03.png#lightbox)

Der optimierte Symbolleiste aktiviert ist, durch Überschreiben der `ViewWillAppear` -Methode der der `NSViewController` und somit zu sehen, wie im folgenden Beispiel:

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

### <a name="using-accessory-view-controllers"></a>Verwenden von Zubehör Ansichtscontroller

Je nach Entwurf der app möchte der Entwickler auch zur Ergänzung der Bereich mit einer Zubehör-View-Controller, der angezeigt unter dem Titel/Symbolleiste Bereich wird bereitstellen, kontextbezogene steuert, die dem Benutzer basierend auf der Aktivität diese Rechte sind Titelleiste derzeit beteiligt:

[![](modern-cocoa-apps-images/content04.png "Ein Beispiel für Zubehör-View-Controller")](modern-cocoa-apps-images/content04.png#lightbox)

Der ansichtscontroller für Zubehör werden automatisch verwischt und vom System ohne Eingriffe von Entwicklern geändert.

Um ein Zubehör-View-Controller hinzuzufügen, führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Main.storyboard`, um sie zur Bearbeitung zu öffnen.
2. Ziehen Sie eine **Custom View Controller** in des Fensters Hierarchie: 

    [![](modern-cocoa-apps-images/content05.png "Hinzufügen einer neuen benutzerdefinierten-View-Controller")](modern-cocoa-apps-images/content05.png#lightbox)
3. Layout der Ansicht Zubehör Benutzeroberfläche: 

    [![](modern-cocoa-apps-images/content06.png "Entwerfen die neue Ansicht")](modern-cocoa-apps-images/content06.png#lightbox)
4. Verfügbar machen die Zubehör Sicht als eine **Outlet** und alle anderen **Aktionen** oder **Outlets** für die Benutzeroberfläche: 

    [![](modern-cocoa-apps-images/content07.png "Hinzufügen der erforderlichen Outlets")](modern-cocoa-apps-images/content07.png#lightbox)
5. Speichern Sie die Änderungen.
6. Zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren.

Bearbeiten der `NSWindowController` und legen Sie ihn wie folgt aussehen:

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

Die wichtigsten Punkte dieses Codes sind, in dem die Ansicht für die benutzerdefinierte Ansicht festgelegt ist, die in Interface Builder definiert und als verfügbar gemacht wurde ein **Outlet**:

```csharp
accessoryView.View = AccessoryViewGoBar;
```

Und die `LayoutAttribute` , definiert _, in denen_ die Zugriffsmethode wird angezeigt:

```csharp
accessoryView.LayoutAttribute = NSLayoutAttribute.Bottom;
```

Da MacOS jetzt vollständig lokalisiert wird, die `Left` und `Right` `NSLayoutAttribute` Eigenschaften sind veraltet und sollte ersetzt werden durch `Leading` und `Trailing`.

<a name="Using-Tabbed-Windows" />

### <a name="using-tabbed-windows"></a>Mithilfe von Registerkarten Windows

Darüber hinaus kann das MacOS-System-Zubehör View Controller das Fenster der app hinzugefügt werden. Geben Sie beispielsweise Folgendes ein, um Windows im Registerkartenformat zu erstellen, in denen einige Windows von der App in einem Fenster "virtuelle" zusammengeführt werden:

[![](modern-cocoa-apps-images/content08.png "Ein Beispiel für ein Mac-Fenster im Registerkartenformat")](modern-cocoa-apps-images/content08.png#lightbox)

In der Regel der Entwickler beschränkt verwenden im Registerformat Windows in ihre Xamarin.Mac-apps werden muss, das System automatisch wie folgt behandeln:

- Windows verwendet, werden automatisch im Registerkartenformat, wenn die `OrderFront` -Methode wird aufgerufen.
- Windows verwendet, werden automatisch Untabbed, wenn die `OrderOut` -Methode wird aufgerufen.
- Im Code, die alle im Registerformat Fenster immer noch als "sichtbar" betrachtet werden, werden alle Registerkarten nicht vordersten jedoch durch das System über CoreGraphics ausgeblendet.
- Verwenden der `TabbingIdentifier` Eigenschaft `NSWindow` zu einer Gruppe von Windows in Registerkarten.
- Ist dies ein `NSDocument` -basierten Anwendung, mehrere dieser Features wird automatisch (z. B. die plus-Schaltfläche auf der Registerkartenleiste hinzugefügt wird) aktiviert werden, ohne eine Developer-Aktion.
- Nicht -`NSDocument` basierend apps können auf die Schaltfläche "plus" in der Gruppe von Registerkarten Hinzufügen eines neuen Dokuments durch Überschreiben der `GetNewWindowForTab` Methode der `NSWindowsController`.

Vereint alle Bestandteile, die `AppDelegate` systembasierte im Registerkartenformat Windows konnte eine App, die möchten, verwenden Sie wie folgt aussehen:

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

In denen die `NewDocumentNumber` Eigenschaft behält den Überblick darüber der Anzahl der neu erstellten Dokumente und die `NewDocument` Methode erstellt ein neues Dokument und wird angezeigt.

Die `NSWindowController` kann dann wie folgt aussehen:

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

In dem die statische `App` Eigenschaft verfügt über eine Tastenkombination, um erhalten die `AppDelegate`. Die `SetDefaultDocumentTitle` Methode legt fest, basierten auf der Anzahl von neuen Dokumenten erstellt einen neuen Dokumente-Titel.

Der folgende Code weist MacOS, unter denen die app Registerkarten bevorzugt und stellt eine Zeichenfolge, die der app von Windows in Registerkarten gruppiert werden kann:

```csharp
// Prefer Tabbed Windows
Window.TabbingMode = NSWindowTabbingMode.Preferred;
Window.TabbingIdentifier = "Main";
```

Und die folgende Methode für die Außerkraftsetzung der Registerkartenleiste, das Erstellen ein neues Dokuments, wenn der Benutzer darauf klickt, wird eine plus-Schaltfläche hinzugefügt:

```csharp
public override void GetNewWindowForTab (NSObject sender)
{
    // Ask app to open a new document window
    App.NewDocument (this);
}
```

<a name="Using-Core-Animation" />

### <a name="using-core-animation"></a>Mit der Core-Animation

Core Animation ist, einen leistungsstarken Grafikwiedergabemodul, die in MacOS integriert ist. Core Animation wurde optimiert, damit das Potenzial der GPU (Graphics Processing Unit) zur Verfügung, moderner MacOS-Hardware im Gegensatz zur Ausführung der Grafikvorgänge für die CPU, die den Computer verlangsamen können.

Die `CALayer`, Core Animation bereitgestellt, für Aufgaben wie schnell und fließend Durchführen eines Bildlaufs und Animationen verwendet werden kann. Der app-Benutzeroberfläche sollte aus mehreren Unteransichten und Ebenen, Core Animation voll ausnutzen bestehen.

Ein `CALayer` Objekt stellt mehrere Eigenschaften, die dem Entwickler ermöglichen, steuern, was angezeigt wird, die dem Benutzer auf dem Bildschirm z. B. bereit:

- `Content` – Werden Sie können eine `NSImage` oder `CGImage` , der den Inhalt der Ebene bereitstellt.
- `BackgroundColor` -Legt die Hintergrundfarbe der Schicht als ein `CGColor`
- `BorderWidth` -Legt die Breite des Rahmens an.
- `BorderColor` -Legt die Farbe des Rahmens.

Um wichtigste Grafik in der Benutzeroberfläche der Anwendung zu nutzen, es muss verwenden _Ebene gesichert_ Ansichten, mit denen Apple empfiehlt, dass der Entwickler in der Ansicht "Inhalt des Fensters" immer aktiviert werden sollte. Auf diese Weise werden alle untergeordneten Ansichten automatisch Ebene zu sichern sowie geerbt.

Darüber hinaus Apple empfiehlt Ebene gesichert Ansichten verwenden, im Gegensatz zu Hinzufügen eines neuen `CALayer` als eine Ebene, da das System automatisch einige der erforderlichen Einstellungen (z. B. die von einem Retina-Display) behandelt.

Ebene sichern kann aktiviert werden, durch Festlegen der `WantsLayer` von einer `NSView` zu `true` oder innerhalb Xcodes Interface Builder unter der **Ansichteffektinspektor** anhand **Animation Kernebene**:

[![](modern-cocoa-apps-images/content09.png "Die Ansichteffektinspektor")](modern-cocoa-apps-images/content09.png#lightbox)

<a name="Redrawing-Views-with-Layers" />

#### <a name="redrawing-views-with-layers"></a>Neuzeichnen von Ansichten mit Ebenen

Ein weiterer wichtiger Schritt beim Verwenden von Ebene gesichert-Ansichten in einer Xamarin.Mac-app festlegen, ist die `LayerContentsRedrawPolicy` von der `NSView` zu `OnSetNeedsDisplay` in die `NSViewController`. Zum Beispiel:

```csharp
public override void ViewWillAppear ()
{
    base.ViewWillAppear ();

    // Set the content redraw policy
    View.LayerContentsRedrawPolicy = NSViewLayerContentsRedrawPolicy.OnSetNeedsDisplay;
}
```

Wenn der Entwickler diese Eigenschaft nicht festlegt, die Ansicht wird neu gezeichnet werden nach ihrem Ursprung Frame geändert wird, die nicht aus Leistungsgründen erwünscht. Mit dieser Eigenschaft auf festgelegt `OnSetNeedsDisplay` Entwickler müssen manuell festlegen `NeedsDisplay` zu `true` zu erzwingen, dass den Inhalt, jedoch neu gezeichnet.

Wenn eine Ansicht als geändert markiert ist, überprüft das System die `WantsUpdateLayer` -Eigenschaft der Ansicht. Wenn zurückgegeben `true` die `UpdateLayer` -Methode aufgerufen wird, andernfalls die `DrawRect` der Ansicht aufgerufen, um den Inhalt der Ansicht zu aktualisieren.

Apple hat die folgenden Vorschläge für die Aktualisierung einer Ansichten Inhalt bei Bedarf:

- Apple bevorzugt mit `UpdateLater` über `DrawRect` jedes Mal, wenn möglich, da es stellt eine erhebliche Leistungssteigerung bewirken.
- Verwenden Sie den gleichen `layer.Contents` für UI-Elemente, die ähnlich aussehen.
- Apple bevorzugt auch den Entwickler zum Erstellen der Benutzeroberfläche mithilfe von standard-Ansichten wie `NSTextField`, erneut nach Möglichkeit.

Verwendung von `UpdateLayer`, eine benutzerdefinierte Klasse zum Erstellen der `NSView` , und stellen Sie den Code wie folgt aussehen:

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

Um eine moderne Drag & Drop-Umgebung für den Benutzer vorhanden ist, kann der Entwickler sollten beachten _Drag Flocking_ in ihr app Drag & Drop-Vorgängen. Ziehen Sie Flocking ist, in denen jede einzelne Datei oder der ursprünglich gezogene Element als ein einzelnes Element angezeigt, die wird (Gruppe unter dem Cursor mit der Anzahl von Elementen)-Beständen wächst der Benutzer den Ziehvorgang entsteht.

Wenn der Benutzer den Ziehvorgang beendet wird, werden die einzelnen Elemente Unflock und zurück an ihren ursprünglichen Speicherorten.

Im folgenden Beispielcode wird ermöglicht, ziehen Sie auf eine benutzerdefinierte Ansicht Flocking:

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

Die flocking Auswirkung wurde erreicht, indem Sie jedes Element, das gezogen wird senden die `BeginDraggingSession` Methode der `NSView` als separates Element in einem Array.

Bei der Arbeit mit einer `NSTableView` oder `NSOutlineView`, verwenden Sie die `PastboardWriterForRow` -Methode der der `NSTableViewDataSource` Klasse, um den Vorgang ziehen zu starten:

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

Dadurch kann der Entwickler zu einer Einzelperson `NSDraggingItem` für jedes Element in der Tabelle, die im Gegensatz zu den älteren Methode gezogen wird, wird `WriteRowsWith` , die alle Zeilen als eine einzelne Gruppe in der Zwischenablage schreiben.

Bei der Arbeit mit `NSCollectionViews`, erneut verwenden die `PasteboardWriterForItemAt` Methode im Gegensatz zu den `WriteItemsAt` -Methode beim Ziehen von beginnt.

Entwickler sollten immer die große Dateien in der Zwischenablage einfügen. MacOS Sierra, neue _Datei Zusagen_ ermöglichen es Entwicklern, platzieren Sie Verweise auf erhalten Sie die Dateien in die Zwischenablage, die später erfüllt wird, wenn der Benutzer mit den Drop-Vorgang mit dem neuen abschließt `NSFilePromiseProvider` und `NSFilePromiseReceiver` Klassen.

<a name="Using-Modern-Event-Tracking" />

## <a name="using-modern-event-tracking"></a>Mithilfe von modernen Ereignisüberwachung

Für ein Element der Benutzeroberfläche (z. B. eine `NSButton`) hinzugefügt wurde, einen Titel oder die Symbolleiste-Bereich, der Benutzer sollte sein können, klicken Sie auf das Element und Auslösen eines Ereignisses wie gewohnt (z. B. ein Popupfenster anzeigt). Aber da das Element auch in der Titel oder die Symbolleiste ist, der Benutzer sollte klicken und ziehen das Element, um das Fenster auch verschieben können.

Zu diesem Zweck im Code erstellen Sie eine benutzerdefinierte Klasse für das Element (z. B. `NSButton`), und überschreiben Sie die `MouseDown` Ereignis wie folgt:

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

Dieser Code verwendet die `TrackEventsMatching` Methode der `NSWindow` , die das UI-Element angefügt wird, zum Abfangen von der `LeftMouseUp` und `LeftMouseDragged` Ereignisse. Für eine `LeftMouseUp` Ereignis, das Element der Benutzeroberfläche reagiert, wie gewohnt. Für die `LeftMouseDragged` -Ereignis, das Ereignis wird zum Übergeben der `NSWindow`des `PerformWindowDrag` Methode, um das Fenster auf dem Bildschirm zu verschieben.

Aufrufen der `PerformWindowDrag` Methode der `NSWindow` Klasse bietet folgende Vorteile:

- Dadurch für das Fenster zu verschieben, selbst wenn die app hängen geblieben ist (z. B. beim Verarbeiten einer tiefen Schleife).
- Leerzeichen umschalten funktioniert wie erwartet.
- Die Leiste Leerzeichen werden wie gewohnt angezeigt.
- Fenster andocken und Ausrichtung funktionieren wie gewohnt.

<a name="Using-Modern-Container-View-Controls" />

## <a name="using-modern-container-view-controls"></a>Verwenden von Steuerelementen für moderne Containerbasierte anzeigen

MacOS Sierra bietet viele moderne Verbesserungen, an die vorhandene Container-Ansicht-Steuerelemente, die für Sie in der vorherigen Version des Betriebssystems verfügbar.

<a name="Table View Enhancements" />

## <a name="table-view-enhancements"></a>Erweiterungen der Ansicht

Entwickler sollten immer verwenden, die neue `NSView` Version des Container-Steuerelemente wie z. B. basierte `NSTableView`. Zum Beispiel:

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

Dadurch können für benutzerdefinierte Aktionen von Tabelle Zeile an die angegebenen Zeilen in der Tabelle (z. B. das Wischen direkt zum Löschen der Zeile) angefügt werden. Um dieses Verhalten zu aktivieren, überschreiben die `RowActions` -Methode der der `NSTableViewDelegate`:

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

Die statische `NSTableViewRowAction.FromStyle` wird verwendet, um eine neue Tabelle-Zeile-Aktion der folgenden Formate erstellen:

- `Regular` – Führt eine nichtdestruktive Standardaktionen wie Bearbeiten der Zeile Inhalt.
- `Destructive` – Führt einer destruktiven Aktion beispielsweise ' löschen ' die Zeile aus der Tabelle. Diese Aktionen werden mit rotem Hintergrund dargestellt.

<a name="Scroll-View-Enhancements" />

## <a name="scroll-view-enhancements"></a>Führen Sie einen Bildlauf Systemsichterweiterungen 

Wenn Sie eine Ansicht mit Bildlauf verwenden (`NSScrollView`) direkt oder als Teil eines anderen Steuerelements (z. B. `NSTableView`), ziehen Sie den Inhalt der Bildlaufansicht unter den Titel und die Symbolleiste Bereichen in einer Xamarin.Mac-app mit einem moderne Aussehen und Ansichten.

Daher kann das erste Element im Bereich Scrollansicht teilweise durch den Titel und die Symbolleiste Bereich verdeckt werden.

Um dieses Problem zu beheben, Apple zwei neue Eigenschaften hinzugefügt hat die `NSScrollView` Klasse:

- `ContentInsets` – Ermöglicht es dem Entwickler zu einem `NSEdgeInsets` Objekt, das des Offsets, die am Anfang die Scrollansicht angewendet werden.
- `AutomaticallyAdjustsContentInsets` -If `true` die Scrollansicht übernimmt automatisch die `ContentInsets` für Entwickler.

Mithilfe der `ContentInsets` der Entwickler kann den Anfang die Scrollansicht um z. B. die Einbeziehung von Zubehör ermöglichen anpassen:

- Ein Indikator für die Sortierreihenfolge wie der in der Mail-app.
- Das Suchfeld.
- Eine Schaltfläche "Aktualisieren" oder "Update".

<a name="Auto-Layout-and-Localization-in-Modern-Apps" />

## <a name="auto-layout-and-localization-in-modern-apps"></a>Automatisches Layout und Lokalisierung in modernen Apps

Apple hat mehrere Technologien enthalten, in Xcode, mit denen den Entwickler auf einfache Weise eine internationale MacOS-app erstellen können. Xcode ist nun kann der Entwickler benutzerseitigen Text vom Entwurf der Benutzeroberfläche der app in der Storyboard-Dateien zu trennen und bietet Tools, um diese Trennung zu gewährleisten, wenn die Benutzeroberfläche geändert hat.

Weitere Informationen finden Sie unter Apple [Internationalisierung und Lokalisierung Handbuch](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/InternationalizingYourUserInterface/InternationalizingYourUserInterface.html).

<a name="Implementing-Base-Internationalization" />

### <a name="implementing-base-internationalization"></a>Implementieren von grundlegenden Internationalisierung 

Implementieren Sie die Basis-Internationalisierung, kann der Entwickler eine einzelne Storyboarddatei zum Darstellen der Benutzeroberfläche der Anwendung und alle Zeichenfolgen benutzerseitigen Aussondern bereitstellen. 

Wenn der Entwickler ist das Erstellen der anfänglichen Storyboard-Datei (oder Dateien), die Benutzeroberfläche der app definieren, die sie in der Basis-Internationalisierung (die Sprache, die der Entwickler spricht) erstellt.

Entwickler kann dann exportieren, lokalisierten Versionen und die Basis-Internationalisierung-Zeichenfolgen (in den Entwurf der Storyboard-Benutzeroberfläche), die in mehrere Sprachen übersetzt werden können.

Später können diese lokalisierungen importiert werden und Xcode generiert die sprachspezifische Zeichenfolge-Dateien für das Storyboard.

<a name="Implementing-Auto-Layout-to-Support-Localization" />

### <a name="implementing-auto-layout-to-support-localization"></a>Implementieren Automatisches Layout zur Unterstützung der Lokalisierung

Da Versionen Zeichenfolge Werte höchst unterschiedliche Größen aufweisen können, bzw. lesen die Richtung, sollte der Entwickler Automatisches Layout zu positionieren und ihre Größe der app-Benutzeroberfläche in eine Storyboard-Datei verwenden.

Apple wird empfohlen, wie folgt vorgehen:

- **Entfernen von Einschränkungen für feste Breite** -alle textbasierten Ansichten dürfen zum Ändern der Größe auf Grundlage ihres Inhalts. Mit fester Breite kann ihre Inhalte in einer bestimmten Ansicht zuschneiden.
- **Systeminterne Inhalt Größen verwenden** - von der textbasierten Ansichten werden automatisch mit Standardgröße an ihren Inhalt an. Für die textbasierte Sicht, die nicht ordnungsgemäß Ändern der Größe sind, wählen Sie diese im Interface Builder von Xcode und wählen Sie dann **bearbeiten** > **Größe an Inhalt anpassen**.
- **Anwenden von führenden und nachfolgenden Attribute** : Da die Richtung des Texts zu ändern, kann basierend auf der Sprache des Benutzers, verwenden Sie die neue `Leading` und `Trailing` Einschränkung Attribute im Gegensatz zu den vorhandenen `Right` und `Left` Attribute. `Leading` und `Trailing` wird automatisch basierend auf die Richtung Sprachen angepasst.
- **PIN-Ansichten, um die benachbarten Ansichten** – Dadurch können die Sichten enthalten, neu positionieren, und Ändern der Größe wie die Ansichten, um sie als Reaktion auf die ausgewählte Sprache ändern.
- **Legen Sie ein Minimum für Windows und/oder die maximale Größe nicht** -Windows zulassen, um Größe zu ändern, wie die zuvor ausgewählte Sprache ihre Inhaltsbereiche ändert.
- **Layout Änderungen fortlaufend testen** – während der Entwicklung bei der app ständig in verschiedenen Sprachen getestet werden sollte. Finden Sie unter Apple [app Testen Ihre Internationalized](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/TestingYourInternationalApp/TestingYourInternationalApp.html#//apple_ref/doc/uid/10000171i-CH7-SW1) Dokumentation.
- **NSStackViews, Pin Sichten zusammen verwenden**  -  `NSStackViews` ermöglicht, ihre Inhalte zu verschieben und auf vorhersagbare Weise erweitern und den Inhalt ändern die Größe basierend auf der zuvor ausgewählten Sprache.

<a name="Localizing-in-Xcodes-Interface-Builder" />

### <a name="localizing-in-xcodes-interface-builder"></a>Lokalisieren von in Interface Builder von Xcode

Apple hat mehrere Features in Interface Builder von Xcode bereitgestellt, dass der Entwickler zur Unterstützung der Lokalisierung beim Entwerfen oder Bearbeiten der Benutzeroberfläche einer app verwenden kann. Die **Textrichtung** Teil der **Attributinspektor** kann der Entwickler geben Hinweise wie Richtung verwendet und auf eine SELECT-Anweisung einer textbasierten Sicht aktualisiert werden soll (z. B. `NSTextField`):

[![](modern-cocoa-apps-images/content10.png "Die Textrichtung-Optionen")](modern-cocoa-apps-images/content10.png#lightbox)

Es gibt drei mögliche Werte für die **Textrichtung**:

- **Natürliche** -Layout basiert darauf, dass die Zeichenfolge, die dem Steuerelement zugewiesen.
- **Von links nach rechts** -wird das Layout von links nach rechts immer erzwungen.
- **Von rechts nach links** -das Layout wird immer erzwungen, um rechts nach links.

Es gibt zwei mögliche Werte für die **Layout**:

- **Von links nach rechts** -das Layout wird immer von links nach rechts.
- **Von rechts nach links** – es ist immer das Layout von rechts nach links.

Diese sollte in der Regel nicht geändert werden, wenn eine bestimmte Ausrichtung erforderlich ist.

Die **Spiegel** Eigenschaft weist das System So kippen Sie bestimmte Eigenschaften (z. B. die Zellposition Image). Es verfügt über drei mögliche Werte:

- **Automatisch** -die Position wird automatisch basierend auf die Richtung der ausgewählten Sprache geändert.
- **In rechts zu Links Schnittstelle** -die Position wird nur geändert werden, rechts, um Links basierend-Sprachen.
- **Nie** -die Position ändert sich nie.

Wenn der Entwickler angegeben hat **Center**, **Justify** oder **vollständige** Ausrichtung für den Inhalt einer textbasierten Sicht, diese werden nie gekippt basierend auf zuvor ausgewählten Sprache.

Vor dem MacOS Sierra würde Steuerelemente, die im Code erstellt nicht automatisch gespiegelt werden. Der Entwickler mussten Code wie folgt verwenden, um die Spiegelung verarbeiten:

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

In denen die `Alignment` und `ImagePosition` sind festzulegende basierend auf der `UserInterfaceLayoutDirection` des Steuerelements.

MacOS Sierra fügt mehrere neue Konstruktoren der Einfachheit halber (über die statische `CreateButton` Methode) verwenden Sie mehrere Parameter (z. B. Titel, Bild und Aktion) und spiegelt automatisch richtig. Zum Beispiel:

```csharp
var button2 = NSButton.CreateButton (myTitle, myImage, () => {
    // Take action when the button is pressed
    ...
});
```

<a name="Using-System-Appearances" />

## <a name="using-system-appearances"></a>System-Darstellungen verwenden

Moderner MacOS-apps können ein neues Erscheinungsbild für dunkle-Schnittstelle verwenden, die für die Image-Erstellung, Bearbeitung oder Präsentation apps gut funktioniert:

[![](modern-cocoa-apps-images/content11.png "Ein Beispiel für eine dunkle Mac-Fenster-Benutzeroberfläche")](modern-cocoa-apps-images/content11.png#lightbox)

Dies kann erfolgen, indem eine Codezeile hinzufügen, bevor das Fenster angezeigt wird. Zum Beispiel:

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

Die statische `GetAppearance` Methode der `NSAppearance` Klasse wird verwendet, um die Darstellung eines benannten aus dem System zu erhalten (in diesem Fall `NSAppearance.NameVibrantDark`).

Apple hat die folgenden Vorschläge für die Verwendung von System-Darstellungen:

- Ziehen Sie benannte Farben, hartcodierte Werte vor (wie z. B. `LabelColor` und `SelectedControlColor`).
- Verwenden Sie System-Standardsteuerelement-Stil, wenn möglich.

Eine MacOS-app, die System-Darstellungen verwendet, wird automatisch für Benutzer ordnungsgemäß, die Funktionen zur Barrierefreiheit über die Systemeinstellungen-app aktiviert haben. Daher schlägt Apple an, dass der Entwickler System Darstellungen immer in ihren MacOS-apps verwenden soll.

<a name="Designing-UIs-with-Storyboards" />

## <a name="designing-uis-with-storyboards"></a>Entwerfen von Benutzeroberflächen mit Storyboards

Storyboards können Entwickler nicht nur entwerfen, dass die einzelnen Elemente, bis der app-Benutzeroberfläche, aber zu visualisieren und entwerfen die Benutzeroberfläche übertragen und die Hierarchie der angegebenen Elemente.

Controller können Entwickler die Elemente in einer Einheit der Komposition und Segues abstrakte sammelt und entfernen Sie die typischen "Verbindungscode glue Code" erforderlich, um in der Hierarchie von Inhaltsansichten zu verschieben:

[![](modern-cocoa-apps-images/content12.png "Bearbeiten die Benutzeroberfläche in Interface Builder von Xcode")](modern-cocoa-apps-images/content12.png#lightbox)

Weitere Informationen finden Sie unserem [Einführung in Storyboards](~/mac/platform/storyboards/index.md) Dokumentation.

Es gibt viele Instanzen, in denen eine bestimmte Szene in einem Storyboard definiert die Daten aus einer vorherigen Szene in der Hierarchie von Inhaltsansichten benötigen. Apple hat die folgenden Vorschläge für die Übergabe von Informationen zwischen Szenen:

- Daten Dependancies sollte immer in der Hierarchie abwärts kaskadiert werden.
- Vermeiden Sie hartcodieren Benutzeroberfläche der strukturellen Dependancies, da dies die Flexibilität der Benutzeroberfläche schränkt.
- Verwendung C# Schnittstellen für generische Daten Dependancies bereitzustellen.

Die View-Controller, der als Quelle für die Segue fungiert können außer Kraft setzen der `PrepareForSegue` -Methode, und führen alle Initialisierungsschritte (z. B. das Übergeben von Daten) erforderlich sind, bevor Sie den Segue zum Anzeigen des Ziels View Controller ausgeführt wird. Zum Beispiel:

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

Weitere Informationen finden Sie unserem [Segues](~/mac/platform/storyboards/indepth.md#Segues) Dokumentation.

<a name="Propagating-Actions" />

## <a name="propagating-actions"></a>Weitergeben von Aktionen

Basierend auf den Entwurf der MacOS-app, es gibt möglicherweise Zeiten, wenn der beste Handler für eine Aktion für ein UI-Steuerelement an anderer Stelle in der Hierarchie der Benutzeroberfläche unter in Umständen. Dies gilt normalerweise für Menüs und Menüelemente, die sich in ihre eigenen Szene, die getrennt von den restlichen über die Benutzeroberfläche der app befinden.

Um diese Situation zu behandeln, kann der Entwickler eine benutzerdefinierte Aktion zu erstellen und übergeben die Aktion die-Responder-Kette. Weitere Informationen finden Sie unserem [arbeiten mit benutzerdefinierten Fensteraktionen](~/mac/user-interface/menu.md) Dokumentation.

<a name="Modern-Mac-Features" />

## <a name="modern-mac-features"></a>Moderne Mac-Funktionen

Apple hat mehrere-Funktionen für Benutzer enthalten, unter MacOS Sierra, mit denen Entwickler die Mac-Plattform, z. B. optimal nutzen können:

- **NSUserActivity** – Dadurch kann die app aus, um die Aktivität zu beschreiben, die der Benutzer derzeit beteiligt ist. `NSUserActivity` wurde anfänglich erstellt, um Übergabe, unterstützen, in dem eine Aktivität, die auf einem der Geräte des Benutzers gestartet übernommen und auf einem anderen Gerät fortgesetzt werden konnte. `NSUserActivity` funktioniert in MacOS, wie er unter iOS ist. daher finden Sie unsere [Einführung in die Übergabe](~/ios/platform/handoff.md) iOS-Dokumentation.
- **Siri auf dem Mac** -Siri verwendet die aktuelle Aktivität (`NSUserActivity`) Kontext auf die Befehle bereit, ein Benutzer senden kann.
- **Status der Wiederherstellung** : Wenn der Benutzer eine app unter MacOS und später relaunches es, die app wird automatisch kündigt den ursprünglichen Zustand zurückgegeben werden. Der Entwickler kann der Zustand Wiederherstellung-API verwenden, zum Codieren und flüchtige Zustände der Benutzeroberfläche wiederherzustellen, bevor die Benutzeroberfläche für den Benutzer angezeigt wird. Wenn die app ist `NSDocument` basiert, ist Wiederherstellung des Benutzerzustand automatisch behandelt. Zum Aktivieren der Wiederherstellung des Systemstatus für nicht-`NSDocument` legen Sie die Basis-apps, die `Restorable` von der `NSWindow` -Klasse `true`.
- **Dokumente in der Cloud** -MacOS Sierra, bevor Sie eine app musste sich zum Arbeiten mit Dokumenten in des Benutzers iCloud Drive. Unter MacOS Sierra-der Benutzer **Desktop** und **Dokumente** Ordner können vom System automatisch mit ihren iCloud Drive synchronisiert werden. Lokale Kopien von Dokumenten können demzufolge gelöscht werden, um mehr Speicherplatz auf dem Computer des Benutzers bereitzustellen. `NSDocument` Basis-apps werden automatisch diese Änderung nicht verarbeiten. Müssen alle app-Typen verwenden, eine `NSFileCoordinator` zu lesen und Schreiben von Dokumenten zu synchronisieren.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt werden, einige Tipps, Funktionen und Techniken, mit denen ein Entwickler eine moderner MacOS-app in Xamarin.Mac zu erstellen.



## <a name="related-links"></a>Verwandte Links

- [Beispiele für macOS](https://developer.xamarin.com/samples/mac/)
