---
title: Kopieren und Einfügen in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit dem Zwischenablage erläutert, um das Kopieren und Einfügen in eine xamarin. Mac-Anwendung zu ermöglichen. Es wird gezeigt, wie Sie mit Standard Datentypen arbeiten, die von mehreren apps gemeinsam genutzt werden können, und wie benutzerdefinierte Daten in einer bestimmten App unterstützt werden.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: cf6835b99ea70c3922dd68bc21af3e44815cc92e
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/25/2019
ms.locfileid: "70769935"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Kopieren und Einfügen in xamarin. Mac

_In diesem Artikel wird das Arbeiten mit dem Zwischenablage erläutert, um das Kopieren und Einfügen in eine xamarin. Mac-Anwendung zu ermöglichen. Es wird gezeigt, wie Sie mit Standard Datentypen arbeiten, die von mehreren apps gemeinsam genutzt werden können, und wie benutzerdefinierte Daten in einer bestimmten App unterstützt werden._

## <a name="overview"></a>Übersicht

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf dieselbe Unterstützung von "Zwischenablage" (kopieren und Einfügen), die ein Entwickler in Ziel-C hat.

In diesem Artikel werden die beiden Hauptmethoden zur Verwendung des "Zwischenablage" in einer xamarin. Mac-app behandelt:

1. **Standard Datentypen** : da die Vorgänge in der Regel zwischen zwei nicht verknüpften apps durchgeführt werden, weiß keine app, welche Datentypen von den anderen unterstützt werden. Um das Potenzial für die Freigabe zu maximieren, kann das Zwischenablage mehrere Darstellungen eines bestimmten Elements (unter Verwendung eines Standardsatzes von Common Data Types) enthalten. Dadurch kann die verarbeitende APP die Version auswählen, die für Ihre Anforderungen am besten geeignet ist.
2. **Benutzerdefinierte Daten** : um das Kopieren und Einfügen komplexer Daten in xamarin. Mac zu unterstützen, können Sie einen benutzerdefinierten Datentyp definieren, der vom pasteboard verarbeitet wird. Beispielsweise eine Vector Drawing-APP, die es dem Benutzer ermöglicht, komplexe Formen zu kopieren und einzufügen, die aus mehreren Datentypen und Punkten bestehen.

[![Beispiel für die laufende App](copy-paste-images/intro01.png "Beispiel für die laufende App")](copy-paste-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit dem Zwischenablage in einer xamarin. Mac-Anwendung zur Unterstützung von Kopier-und Einfügevorgängen behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-C](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden das-Attribut `Register` und `Export` das-Attribut erläutert, mit C# denen die Klassen an gesendet werden. Ziel-C-Objekte und UI-Elemente.

## <a name="getting-started-with-the-pasteboard"></a>Ersten Einstieg in das Zwischenablage

Das Zwischenablage stellt einen standardisierten Mechanismus zum Austauschen von Daten in einer bestimmten Anwendung oder zwischen Anwendungen dar. Die typische Verwendung für ein Zwischenablage in einer xamarin. Mac-Anwendung ist die Handhabung von Kopier-und Einfügevorgängen, es werden jedoch auch einige andere Vorgänge unterstützt (z. b. Drag & Drop und Anwendungsdienste).

Wir beginnen mit einer einfachen, praktischen Einführung in die Verwendung von "pasteboards" in einer xamarin. Mac-app, um Sie schnell zu nutzen. Später wird eine ausführliche Erläuterung der Funktionsweise des pasteboards und der verwendeten Methoden bereitgestellt.

In diesem Beispiel erstellen wir eine einfache Dokument basierte Anwendung, die ein Fenster verwaltet, das eine Bildansicht enthält. Der Benutzer ist in der Lage, Bilder zwischen Dokumenten in der APP und oder aus anderen apps oder mehreren Fenstern innerhalb derselben APP zu kopieren und einzufügen.

### <a name="creating-the-xamarin-project"></a>Erstellen des xamarin-Projekts

Zuerst erstellen wir eine neue Dokument basierte xamarin. Mac-app, für die wir die Unterstützung für den Kopier-und Fügevorgang hinzufügen werden.

Führen Sie folgende Schritte aus:

1. Starten Sie Visual Studio für Mac, und klicken Sie auf den Link **Neues Projekt...** .
2. Wählen Sie **Mac** > **App** > **Cocoa App**aus, und klicken Sie dann auf die Schaltfläche **weiter** : 

    [![Erstellen eines neuen Cocoa-App-Projekts](copy-paste-images/sample01.png "Erstellen eines neuen Cocoa-App-Projekts")](copy-paste-images/sample01-large.png#lightbox)
3. Geben `MacCopyPaste` Sie als **Projektnamen** ein, und behalten Sie den Standardwert bei. Klicken Sie auf Weiter: 

    [![Festlegen des Projekt namens](copy-paste-images/sample01a.png "Festlegen des Projekt namens")](copy-paste-images/sample01a-large.png#lightbox)

4. Klicken Sie auf die Schaltfläche **Erstellen** : 

    [![Die neuen Projekteinstellungen werden bestätigt] . (copy-paste-images/sample02.png "Die neuen Projekteinstellungen werden bestätigt") .](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Hinzufügen eines NSDocument

Als Nächstes fügen wir eine `NSDocument` benutzerdefinierte Klasse hinzu, die als Hintergrund Speicher für die Benutzeroberfläche der Anwendung fungiert. Sie enthält eine einzelne Bildansicht und weiß, wie ein Bild aus der Ansicht in das Standard-Zwischenablage kopiert wird und wie ein Bild aus dem Standard-Zwischenablage übernommen und in der Bildansicht angezeigt wird.

Klicken Sie mit der rechten Maustaste auf das xamarin. Mac-Projekt im **Lösungspad** , und wählen Sie**neue Datei** **Hinzufügen** > aus.

![Hinzufügen eines NSDocument zum Projekt](copy-paste-images/sample03.png "Hinzufügen eines NSDocument zum Projekt")

Geben Sie für den **Namen** `ImageDocument` ein, und klicken Sie auf **Neu**. Bearbeiten Sie die **ImageDocument.cs** -Klasse, und führen Sie Sie wie folgt aus:

```csharp
using System;
using AppKit;
using Foundation;
using ObjCRuntime;

namespace MacCopyPaste
{
    [Register("ImageDocument")]
    public class ImageDocument : NSDocument
    {
        #region Computed Properties
        public NSImageView ImageView {get; set;}

        public ImageInfo Info { get; set; } = new ImageInfo();

        public bool ImageAvailableOnPasteboard {
            get {
                // Initialize the pasteboard
                NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
                Class [] classArray  = { new Class ("NSImage") };

                // Check to see if an image is on the pasteboard
                return pasteboard.CanReadObjectForClasses (classArray, null);
            }
        }
        #endregion

        #region Constructor
        public ImageDocument ()
        {
        }
        #endregion

        #region Public Methods
        [Export("CopyImage:")]
        public void CopyImage(NSObject sender) {

            // Grab the current image
            var image = ImageView.Image;

            // Anything to process?
            if (image != null) {
                // Get the standard pasteboard
                var pasteboard = NSPasteboard.GeneralPasteboard;

                // Empty the current contents
                pasteboard.ClearContents();

                // Add the current image to the pasteboard
                pasteboard.WriteObjects (new NSImage[] {image});

                // Save the custom data class to the pastebaord
                pasteboard.WriteObjects (new ImageInfo[] { Info });

                // Using a Pasteboard Item
                NSPasteboardItem item = new NSPasteboardItem();
                string[] writableTypes = {"public.text"};

                // Add a data provier to the item
                ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
                var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

                // Save to pasteboard
                if (ok) {
                    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
                }
            }

        }

        [Export("PasteImage:")]
        public void PasteImage(NSObject sender) {

            // Initialize the pasteboard
            NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
            Class [] classArray  = { new Class ("NSImage") };

            bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
                NSImage image = (NSImage)objectsToPaste[0];

                // Display the new image
                ImageView.Image = image;
            }

            Class [] classArray2 = { new Class ("ImageInfo") };
            ok = pasteboard.CanReadObjectForClasses (classArray2, null);
            if (ok) {
                // Read the image off of the pasteboard
                NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
                ImageInfo info = (ImageInfo)objectsToPaste[0];

            }

        }
        #endregion
    }
}
```

Werfen wir einen Blick auf den folgenden Code.

Der folgende Code stellt eine-Eigenschaft bereit, um zu testen, ob Bilddaten auf dem Standard-pasteboard vorhanden sind. Wenn `true` ein Bild verfügbar `false`ist, wird else zurückgegeben:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

Der folgende Code kopiert ein Bild aus der angefügten Bildansicht in das Standard-pasteboard:

```csharp
[Export("CopyImage:")]
public void CopyImage(NSObject sender) {

    // Grab the current image
    var image = ImageView.Image;

    // Anything to process?
    if (image != null) {
        // Get the standard pasteboard
        var pasteboard = NSPasteboard.GeneralPasteboard;

        // Empty the current contents
        pasteboard.ClearContents();

        // Add the current image to the pasteboard
        pasteboard.WriteObjects (new NSImage[] {image});

        // Save the custom data class to the pastebaord
        pasteboard.WriteObjects (new ImageInfo[] { Info });

        // Using a Pasteboard Item
        NSPasteboardItem item = new NSPasteboardItem();
        string[] writableTypes = {"public.text"};

        // Add a data provider to the item
        ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
        var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

        // Save to pasteboard
        if (ok) {
            pasteboard.WriteObjects (new NSPasteboardItem[] { item });
        }
    }

}
```

Mit dem folgenden Code wird ein Bild aus dem Standard-Zwischenablage eingefügt und in der angefügten Bildansicht angezeigt (wenn das Zwischenablage ein gültiges Bild enthält):

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0]
    }
}
```

Wenn dieses Dokument vorhanden ist, erstellen wir die Benutzeroberfläche für die xamarin. Mac-app.

### <a name="building-the-user-interface"></a>Die Benutzeroberfläche wird aufgebaut

Doppelklicken Sie auf die Datei **Main. Storyboard** , um Sie in Xcode zu öffnen. Fügen Sie als nächstes eine Symbolleiste und einen Bildtyp hinzu, und konfigurieren Sie Sie wie folgt:

[![Bearbeiten der Symbolleiste](copy-paste-images/sample04.png "Bearbeiten der Symbolleiste")](copy-paste-images/sample04-large.png#lightbox)

Fügen Sie ein Symbolleisten **Element** kopieren und Einfügen auf der linken Seite der Symbolleiste hinzu. Wir verwenden diese als Verknüpfungen zum Kopieren und Einfügen aus dem Menü "Bearbeiten". Fügen Sie dann auf der rechten Seite der Symbolleiste vier **Bildsymbol leisten Elemente** hinzu. Wir verwenden diese, um das Bild gut mit einigen Standardbildern aufzufüllen.

Weitere Informationen zum Arbeiten mit Symbolleisten finden Sie in unserer [Toolbars](~/mac/user-interface/toolbar.md) -Dokumentation.

Als nächstes machen wir die folgenden Outlets und Aktionen für unsere Symbolleisten Elemente und das Bild gut verfügbar:

[![Erstellen von Outlets und Aktionen](copy-paste-images/sample05.png "Erstellen von Outlets und Aktionen")](copy-paste-images/sample05-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Outlets und Aktionen finden Sie im Abschnitt [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) in unserer Hello- [, Mac](~/mac/get-started/hello-mac.md) -Dokumentation.

### <a name="enabling-the-user-interface"></a>Aktivieren der Benutzeroberfläche

Wenn unsere Benutzeroberfläche in Xcode erstellt wurde und das UI-Element über Outlets und Aktionen verfügbar gemacht wird, müssen wir den Code hinzufügen, um die Benutzeroberfläche zu aktivieren. Doppelklicken Sie in der **Lösungspad** auf die Datei **ImageWindow.cs** , und sehen Sie Sie wie folgt aus:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacCopyPaste
{
    public partial class ImageWindow : NSWindow
    {
        #region Private Variables
        ImageDocument document;
        #endregion

        #region Computed Properties
        [Export ("Document")]
        public ImageDocument Document {
            get {
                return document;
            }
            set {
                WillChangeValue ("Document");
                document = value;
                DidChangeValue ("Document");
            }
        }

        public ViewController ImageViewController {
            get { return ContentViewController as ViewController; }
        }

        public NSImage Image {
            get {
                return ImageViewController.Image;
            }
            set {
                ImageViewController.Image = value;
            }
        }
        #endregion

        #region Constructor
        public ImageWindow (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void AwakeFromNib ()
        {
            base.AwakeFromNib ();

            // Create a new document instance
            Document = new ImageDocument ();

            // Attach to image view
            Document.ImageView = ImageViewController.ContentView;
        }
        #endregion

        #region Public Methods
        public void CopyImage (NSObject sender)
        {
            Document.CopyImage (sender);
        }

        public void PasteImage (NSObject sender)
        {
            Document.PasteImage (sender);
        }

        public void ImageOne (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image01.jpg");

            // Set image info
            Document.Info.Name = "city";
            Document.Info.ImageType = "jpg";
        }

        public void ImageTwo (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image02.jpg");

            // Set image info
            Document.Info.Name = "theater";
            Document.Info.ImageType = "jpg";
        }

        public void ImageThree (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image03.jpg");

            // Set image info
            Document.Info.Name = "keyboard";
            Document.Info.ImageType = "jpg";
        }

        public void ImageFour (NSObject sender)
        {
            // Load image
            Image = NSImage.ImageNamed ("Image04.jpg");

            // Set image info
            Document.Info.Name = "trees";
            Document.Info.ImageType = "jpg";
        }
        #endregion
    }
}
```

Sehen wir uns diesen Code im folgenden ausführlich an.

Zuerst machen wir eine Instanz der `ImageDocument` -Klasse verfügbar, die wir oben erstellt haben:

```csharp
private ImageDocument _document;
...

[Export ("Document")]
public ImageDocument Document {
    get { return _document; }
    set {
        WillChangeValue ("Document");
        _document = value;
        DidChangeValue ("Document");
    }
}
```

Mithilfe `Export`von, `WillChangeValue` und `DidChangeValue`haben wir die-Eigenschaft `Document` so eingerichtet, dass die Schlüssel-Wert-Codierung und Datenbindung in Xcode zulässig ist.

Wir machen auch das Image aus dem Image verfügbar, das wir unserer Benutzeroberfläche in Xcode mit der folgenden Eigenschaft hinzugefügt haben:

```csharp
public ViewController ImageViewController {
    get { return ContentViewController as ViewController; }
}

public NSImage Image {
    get {
        return ImageViewController.Image;
    }
    set {
        ImageViewController.Image = value;
    }
}
```

Wenn das Hauptfenster geladen und angezeigt wird, erstellen wir eine Instanz der `ImageDocument` Klasse und fügen das Bild der Benutzeroberfläche mit folgendem Code an die Benutzeroberfläche an:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create a new document instance
    Document = new ImageDocument ();

    // Attach to image view
    Document.ImageView = ImageViewController.ContentView;
}
```

Als Reaktion darauf, dass der Benutzer auf die Symbolleisten Elemente kopieren und Einfügen klickt, wird die Instanz der `ImageDocument` -Klasse aufgerufen, um die eigentliche Arbeit zu erledigen:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Aktivieren der Datei und Bearbeiten von Menüs

Als letztes müssen Sie das **neue** Menü Element im Menü **Datei** aktivieren (um neue Instanzen des Hauptfensters zu erstellen) und die Menü Elemente **Ausschneiden**, **Kopieren** und **Einfügen** im Menü **Bearbeiten** aktivieren.

Um das **neue** Menü Element zu aktivieren, bearbeiten Sie die Datei **AppDelegate.cs** , und fügen Sie den folgenden Code hinzu:

```csharp
public int UntitledWindowCount { get; set;} =1;
...

[Export ("newDocument:")]
void NewDocument (NSObject sender) {
    // Get new window
    var storyboard = NSStoryboard.FromName ("Main", null);
    var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

    // Display
    controller.ShowWindow(this);

    // Set the title
    controller.Window.Title = (++UntitledWindowCount == 1) ? "untitled" : string.Format ("untitled {0}", UntitledWindowCount);
}
```

Weitere Informationen finden Sie im Abschnitt [Working with multiple Windows](~/mac/user-interface/window.md) der [Windows](~/mac/user-interface/window.md) -Dokumentation.

Um die Menü Elemente **Ausschneiden**, **Kopieren** und **Einfügen** zu aktivieren, bearbeiten Sie die Datei **AppDelegate.cs** , und fügen Sie den folgenden Code hinzu:

```csharp
[Export("copy:")]
void CopyImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);
}

[Export("cut:")]
void CutImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Copy the image to the clipboard
    window.Document.CopyImage (sender);

    // Clear the existing image
    window.Image = null;
}

[Export("paste:")]
void PasteImage (NSObject sender)
{
    // Get the main window
    var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;

    // Anything to do?
    if (window == null)
        return;

    // Paste the image from the clipboard
    window.Document.PasteImage (sender);
}
```

Für jedes Menü Element wird das aktuelle, oberste Schlüssel Fenster angezeigt und in `ImageWindow` die Klasse umgewandelt:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Von dort aus wird die `ImageDocument` Klasseninstanz dieses Fensters aufgerufen, um die Kopier-und Einfügeaktionen zu verarbeiten. Beispiel: 

```csharp
window.Document.CopyImage (sender);
```

Es sollen nur **Ausschneide** **-,** **Kopier** -und Einfügemenü Elemente verfügbar sein, wenn im Standard-Zwischenablage oder im Bildbereich des aktuellen aktiven Fensters Bild Daten vorhanden sind.

Fügen Sie dem xamarin. Mac-Projekt eine **EditMenuDelegate.cs** -Datei hinzu, und machen Sie Sie wie folgt:

```csharp
using System;
using AppKit;

namespace MacCopyPaste
{
    public class EditMenuDelegate : NSMenuDelegate
    {
        #region Override Methods
        public override void MenuWillHighlightItem (NSMenu menu, NSMenuItem item)
        {
        }

        public override void NeedsUpdate (NSMenu menu)
        {
            // Get list of menu items
            NSMenuItem[] Items = menu.ItemArray ();

            // Get the key window and determine if the required images are available
            var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
            var hasImage = (window != null) && (window.Image != null);
            var hasImageOnPasteboard = (window != null) && window.Document.ImageAvailableOnPasteboard;

            // Process every item in the menu
            foreach(NSMenuItem item in Items) {
                // Take action based on the menu title
                switch (item.Title) {
                case "Cut":
                case "Copy":
                case "Delete":
                    // Only enable if there is an image in the view
                    item.Enabled = hasImage;
                    break;
                case "Paste":
                    // Only enable if there is an image on the pasteboard
                    item.Enabled = hasImageOnPasteboard;
                    break;
                default:
                    // Only enable the item if it has a sub menu
                    item.Enabled = item.HasSubmenu;
                    break;
                }
            }
        }
        #endregion
    }
}
```

Auch hier wird das aktuelle und das oberste Fenster angezeigt `ImageDocument` , um zu überprüfen, ob die erforderlichen Bilddaten vorhanden sind. Anschließend verwenden wir die `MenuWillHighlightItem` -Methode, um jedes Element auf der Grundlage dieses Zustands zu aktivieren oder zu deaktivieren.

Bearbeiten Sie die Datei **AppDelegate.cs** , und `DidFinishLaunching` führen Sie die folgende Methode aus:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Zunächst deaktivieren wir die automatische Aktivierung und Deaktivierung von Menü Elementen im Menü Bearbeiten. Als Nächstes fügen wir eine Instanz der `EditMenuDelegate` -Klasse an, die wir oben erstellt haben.

Weitere Informationen finden Sie in unserer [Menüs](~/mac/user-interface/menu.md) -Dokumentation.

### <a name="testing-the-app"></a>Testen der App

Nachdem alles vorhanden ist, können wir die Anwendung testen. Erstellen Sie die APP, und führen Sie Sie aus, und die Hauptschnittstelle wird angezeigt:

![Ausführen der Anwendung](copy-paste-images/run01.png "Ausführen der Anwendung")

Wenn Sie das Menü Bearbeiten öffnen, beachten Sie, dass **Ausschneiden**, **Kopieren** und **Einfügen** deaktiviert ist, weil es im Bild-oder Standard-pasteboard kein Bild gibt:

![Öffnen des Menüs "Bearbeiten] " (copy-paste-images/run02.png "Öffnen des Menüs \"Bearbeiten") "

Wenn Sie dem Bild ein Bild hinzufügen und das Menü Bearbeiten erneut öffnen, werden die Elemente nun aktiviert:

![Anzeige der Menü Elemente bearbeiten ist aktiviert](copy-paste-images/run03.png "Anzeige der Menü Elemente bearbeiten ist aktiviert")

Wenn Sie das Bild kopieren und im Menü Datei die Option **neu** auswählen, können Sie dieses Bild in das neue Fenster einfügen:

![Einfügen eines Bilds in ein neues Fenster](copy-paste-images/run04.png "Einfügen eines Bilds in ein neues Fenster")

In den folgenden Abschnitten wird erläutert, wie Sie mit dem Zwischenablage in einer xamarin. Mac-Anwendung arbeiten.

## <a name="about-the-pasteboard"></a>Informationen zum Zwischenablage

In macOS (früher als OS X bezeichnet) unterstützt das Zwischenablage (`NSPasteboard`) mehrere Server Prozesse, z. b. Kopieren & einfügen, Drag & Drop und Anwendungsdienste. In den folgenden Abschnitten werden einige wichtige Konzepte von "Zwischenablage" näher betrachtet.

### <a name="what-is-a-pasteboard"></a>Was ist ein pasteboard?

Die `NSPasteboard` -Klasse stellt einen standardisierten Mechanismus zum Austauschen von Informationen zwischen Anwendungen oder innerhalb einer bestimmten app bereit. Die Hauptfunktion eines pasteboards ist die Verarbeitung von Kopier-und Einfügevorgängen:

1. Wenn der Benutzer ein Element in einer App auswählt und das **Ausschneide** -oder **Kopier** Menü Element verwendet, werden eine oder mehrere Darstellungen des ausgewählten Elements auf dem pasteboard platziert.
2. Wenn der Benutzer das Menü Element **Einfügen** (innerhalb derselben oder einer anderen APP) verwendet, wird die Version der Daten, die Sie verarbeiten kann, aus dem Zwischenablage kopiert und der app hinzugefügt.

Weniger offensichtliche Zwischenablage-Verwendungen sind u. a. Find-, Drag-, Drag & Drop-und Application Services-Vorgänge

- Wenn der Benutzer einen Zieh Vorgang initiiert, werden die Zieh Daten in das pasteboard kopiert. Wenn der Zieh Vorgang mit einem Drop auf eine andere APP endet, kopiert diese APP die Daten aus dem pasteboard.
- Bei Übersetzungsdiensten werden die zu über setzenden Daten von der anfordernden app in das Zwischenablage kopiert. Der Anwendungs Dienst ruft die Daten aus dem pasteboard ab, führt die Übersetzung aus und fügt die Daten dann auf dem pasteboard wieder ein.

In ihrer einfachsten Form werden mithilfe von "pasteboards" Daten innerhalb einer bestimmten app oder zwischen apps verschoben, sodass Sie in einem speziellen globalen Speicherbereich außerhalb des App-Prozesses vorhanden sind. Obwohl die Konzepte der pasteboards auf einfache Weise grasps sind, gibt es einige komplexere Details, die berücksichtigt werden müssen. Diese werden im folgenden ausführlich beschrieben.

### <a name="named-pasteboards"></a>Benannte "pasteboards"

Ein Zwischenablage kann öffentlich oder privat sein und kann für eine Vielzahl von Zwecken innerhalb einer Anwendung oder zwischen mehreren Apps verwendet werden. macOS bietet mehrere Standard-pasteboards, von denen jede über eine bestimmte, klar definierte Verwendung verfügt:

- `NSGeneralPboard`: Das Standard-Zwischenablage für Ausschneide- **,** **Kopier** -und **Einfügevorgänge**.
- `NSRulerPboard`-Unterstützt **Ausschneide**- **,** **Kopier** -und Einfügevorgänge für **Line**
- `NSFontPboard`-Unterstützt Ausschneide- **,** **Kopier** - `NSFont` und **Einfügevorgänge**für Objekte
- `NSFindPboard`: Unterstützt anwendungsspezifische Such Bereiche, die Suchtext gemeinsam verwenden können.
- `NSDragPboard`-Unterstützt **Drag & Drop** -Vorgänge.

In den meisten Fällen verwenden Sie eines der vom System definierten pasteboards. Es kann jedoch Situationen geben, in denen Sie Ihre eigenen pasteboards erstellen müssen. In diesen Fällen können Sie die `FromName (string name)` -Methode `NSPasteboard` der-Klasse verwenden, um ein benutzerdefiniertes "Zwischenablage" mit dem angegebenen Namen zu erstellen.

Optional können Sie die `CreateWithUniqueName` -Methode `NSPasteboard` der-Klasse aufzurufen, um ein eindeutig benanntes "pasteboard" zu erstellen.

### <a name="pasteboard-items"></a>PasteBoard-Elemente

Jedes Datenelement, das von einer Anwendung in ein "Zwischenablage" geschrieben wird, wird als " _Zwischenablage_ "-Element betrachtet, und ein "Zwischenablage" kann mehrere Elemente gleichzeitig enthalten. Auf diese Weise kann eine APP mehrere Versionen der Daten schreiben, die in ein Zwischenablage kopiert werden (z. b. nur-Text und formatierten Text), und die Abruf Ende App kann nur die Daten lesen, die verarbeitet werden können (z. b. nur den nur-Text).

### <a name="data-representations-and-uniform-type-identifiers"></a>Daten Darstellungen und einheitliche Typbezeichner

In der Regel erfolgt die Ausführung von "pasteboard"-Vorgängen zwischen zwei (oder mehr) Anwendungen, die weder über die anderen noch über die Datentypen verfügen, die jeweils verarbeitet werden können. Wie im obigen Abschnitt erwähnt, kann ein Zwischenablage mehrere Darstellungen der kopierten und eingefügten Daten enthalten, um das Potenzial für die Freigabe von Informationen zu maximieren.

Jede Darstellung wird über einen URI (Uniform Type Identifier) identifiziert, bei dem es sich nicht mehr um eine einfache Zeichenfolge handelt, die den Typ des dargestellten Datums eindeutig identifiziert (Weitere Informationen finden Sie in der [Übersicht über die Uniform Type Identifiers](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) von Apple). Dokumentation). 

Wenn Sie einen benutzerdefinierten Datentyp erstellen (z. b. ein Zeichnungsobjekt in einer Vektor Zeichnungs-APP), können Sie einen eigenen Wert erstellen, um ihn in Kopier-und Einfügevorgängen eindeutig zu identifizieren.

Wenn eine APP das Einfügen von Daten aus einem pasteboard vorbereitet, muss Sie die Darstellung ermitteln, die ihren Fähigkeiten am besten entspricht (sofern vorhanden). In der Regel ist dies der reichste verfügbare Typ (z. b. formatierter Text für eine Textverarbeitungs-app), der auf die einfachsten verfügbaren Formulare zurückfällt (nur Text für einen einfachen Text-Editor).

<a name="Promised_Data" />

### <a name="promised-data"></a>Versprochene Daten

Im Allgemeinen sollten Sie so viele Darstellungen der zu kopierenden Daten bereitstellen, um die gemeinsame Nutzung von apps zu maximieren. Aufgrund von Zeit-oder Arbeitsspeicher Einschränkungen ist es jedoch möglicherweise nicht praktikabel, jeden Datentyp in das pasteboard zu schreiben.

In dieser Situation können Sie die erste Datendarstellung auf dem Zwischenablage platzieren, und die empfangende App kann eine andere Darstellung anfordern, die direkt vor dem Einfügevorgang direkt generiert werden kann.

Wenn Sie das ursprüngliche Element im pasteboard platzieren, geben Sie an, dass eine oder mehrere der anderen verfügbaren Darstellungen von einem Objekt bereitgestellt werden, das der `NSPasteboardItemDataProvider` -Schnittstelle entspricht. Diese Objekte bieten die zusätzlichen Darstellungen nach Bedarf, die von der empfangenden App angefordert werden.

### <a name="change-count"></a>Änderungs Anzahl

Jedes Zwischenablage behält eine _Änderungs Anzahl_ bei, die jedes Mal erhöht wird, wenn ein neuer Besitzer deklariert wird. Eine APP kann ermitteln, ob sich der Inhalt des pasteboards seit der letzten untersuchten Änderung geändert hat, indem der Wert der Änderungs Anzahl überprüft wird.

Verwenden Sie `ChangeCount` die `ClearContents` -Methode und `NSPasteboard` die-Methode der-Klasse, um die Änderungs Anzahl eines bestimmten pasteboards zu ändern.

## <a name="copying-data-to-a-pasteboard"></a>Kopieren von Daten in ein Zwischenablage

Sie führen einen Kopiervorgang durch, indem Sie zuerst auf ein pasteboard zugreifen, vorhandene Inhalte löschen und so viele Darstellungen der Daten schreiben, wie dies für das pasteboard erforderlich ist.

Beispiel:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

In der Regel schreiben Sie einfach auf das allgemeine "pasteboard", wie im obigen Beispiel gezeigt. Alle Objekte, die Sie an die `WriteObjects` -Methode senden, *müssen* mit der `INSPasteboardWriting` -Schnittstelle übereinstimmen. Mehrere integrierte Klassen (z `NSString`. b., `NSURL` `NSColor` `NSImage`,,, `NSAttributedString`und `NSPasteboardItem`) entsprechen automatisch dieser Schnittstelle.

Wenn Sie eine benutzerdefinierte Datenklasse in das Zwischenablage schreiben, muss Sie der `INSPasteboardWriting` -Schnittstelle entsprechen oder in eine Instanz `NSPasteboardItem` der-Klasse umgeschrieben werden (siehe Abschnitt " [benutzerdefinierte Datentypen](#Custom_Data_Types) " unten).

## <a name="reading-data-from-a-pasteboard"></a>Lesen von Daten aus einem Zwischenablage

Wie bereits erwähnt, können zum Maximieren des Potenzials von Daten zwischen apps mehrere Darstellungen der kopierten Daten in das pasteboard geschrieben werden. Es liegt an der empfangenden APP, die reichste Version auszuwählen, die für ihre Funktionen möglich ist (sofern vorhanden).

### <a name="simple-paste-operation"></a>Einfacher Einfügevorgang

Mithilfe der `ReadObjectsForClasses` -Methode lesen Sie Daten aus dem-Steuer Board. Es sind zwei Parameter erforderlich:

1. Ein Array von `NSObject` Typen auf Basis von Klassen, die Sie aus dem pasteboard lesen möchten. Sie sollten dies zuerst mit dem gewünschten Datentyp sortieren, wobei alle verbleibenden Typen in absteigender Reihenfolge angegeben werden.
2. Ein Wörterbuch, das zusätzliche Einschränkungen (z. b. die Beschränkung auf bestimmte URL-Inhaltstypen) oder ein leeres Wörterbuch enthält, wenn keine weiteren Einschränkungen erforderlich sind.

Die-Methode gibt ein Array von Elementen zurück, die die von uns über gebenden Kriterien erfüllen und daher höchstens die gleiche Anzahl von Datentypen enthalten, die angefordert werden. Es ist auch möglich, dass keiner der angeforderten Typen vorhanden ist und ein leeres Array zurückgegeben wird.

Mit dem folgenden Code wird beispielsweise überprüft, ob eine `NSImage` im allgemeinen "Zwischenablage" vorhanden ist, und Sie wird in einem Bild angezeigt, wenn dies der Fall ist:

```csharp
[Export("PasteImage:")]
public void PasteImage(NSObject sender) {

    // Initialize the pasteboard
    NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
    Class [] classArray  = { new Class ("NSImage") };

    bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
        NSImage image = (NSImage)objectsToPaste[0];

        // Display the new image
        ImageView.Image = image;
    }

    Class [] classArray2 = { new Class ("ImageInfo") };
    ok = pasteboard.CanReadObjectForClasses (classArray2, null);
    if (ok) {
        // Read the image off of the pasteboard
        NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
        ImageInfo info = (ImageInfo)objectsToPaste[0];
            }
}
```

### <a name="requesting-multiple-data-types"></a>Anfordern mehrerer Datentypen

Basierend auf dem Typ der zu erstellenden xamarin. Mac-Anwendung kann Sie mehrere Darstellungen der eingefügten Daten verarbeiten. In dieser Situation gibt es zwei Szenarien zum Abrufen von Daten aus dem pasteboard:

1. Erstellen Sie einen einzelnen-Befehl `ReadObjectsForClasses` an die-Methode, und stellen Sie ein Array aller Darstellungen bereit, die Sie wünschen (in der bevorzugten Reihenfolge).
2. Nehmen Sie mehrere Aufrufe an `ReadObjectsForClasses` die-Methode an, die jedes Mal ein anderes Array von Typen anfordern.

Weitere Informationen zum Abrufen von Daten aus einem pasteboard finden Sie im obigen Abschnitt **Simple** -Einfügevorgang.

### <a name="checking-for-existing-data-types"></a>Überprüfen vorhandener Datentypen

Es kann vorkommen, dass Sie überprüfen möchten, ob ein "Zwischenablage" eine bestimmte Datendarstellung enthält, ohne die Daten tatsächlich aus dem Steuerelement zu lesen (z. b. Wenn Sie das Menü Element **Einfügen** nur dann aktivieren, wenn gültige Daten vorhanden sind).

Ruft die `CanReadObjectForClasses` -Methode des pasteboards auf, um festzustellen, ob Sie einen angegebenen Typ enthält.

Mit dem folgenden Code wird beispielsweise bestimmt, ob das allgemeine-Zwischenablage eine `NSImage` -Instanz enthält:

```csharp
public bool ImageAvailableOnPasteboard {
    get {
        // Initialize the pasteboard
        NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
        Class [] classArray  = { new Class ("NSImage") };

        // Check to see if an image is on the pasteboard
        return pasteboard.CanReadObjectForClasses (classArray, null);
    }
}
```

### <a name="reading-urls-from-the-pasteboard"></a>Lesen von URLs aus dem Zwischenablage

Basierend auf der Funktion einer bestimmten xamarin. Mac-app ist es möglicherweise erforderlich, URLs von einem pasteboard zu lesen, aber nur, wenn Sie einen bestimmten Satz von Kriterien erfüllen (z. b. das verweisen auf Dateien oder URLs eines bestimmten Datentyps). In dieser Situation können Sie zusätzliche Suchkriterien mit dem zweiten Parameter der `CanReadObjectForClasses` -Methode oder `ReadObjectsForClasses` der-Methode angeben.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Benutzerdefinierte Datentypen

Es gibt Zeiten, in denen Sie Ihre eigenen benutzerdefinierten Typen aus einer xamarin. Mac-app im Zwischenablage speichern müssen. Beispielsweise eine Vector Drawing-APP, die dem Benutzer das Kopieren und Einfügen von Zeichnungsobjekten ermöglicht.

In dieser Situation müssen Sie die benutzerdefinierte Datenklasse so entwerfen, dass `NSObject` Sie von erbt und einigen wenigen Schnittstellen entspricht `INSPasteboardWriting` (`INSCoding`und `INSPasteboardReading`). Optional können Sie einen `NSPasteboardItem` verwenden, um die zu kopierenden oder einzufügenden Daten zu kapseln.

Beide Optionen werden im folgenden ausführlich beschrieben.

### <a name="using-a-custom-class"></a>Verwenden einer benutzerdefinierten Klasse

In diesem Abschnitt wird die einfache Beispiel-App erweitert, die Sie am Anfang dieses Dokuments erstellt haben, und es wird eine benutzerdefinierte Klasse zum Nachverfolgen von Informationen zu dem Image hinzugefügt, das zwischen Windows kopiert und eingefügt wird.

Fügen Sie dem Projekt eine neue Klasse hinzu, und nennen Sie Sie **imageinfo.cs**. Bearbeiten Sie die Datei, und führen Sie Sie wie folgt aus:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfo")]
    public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
    {
        #region Computed Properties
        [Export("name")]
        public string Name { get; set; }

        [Export("imageType")]
        public string ImageType { get; set; }
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfo ()
        {
        }
        
        public ImageInfo (IntPtr p) : base (p)
        {
        }

        [Export ("initWithCoder:")]
        public ImageInfo(NSCoder decoder) {

            // Decode data
            NSString name = decoder.DecodeObject("name") as NSString;
            NSString type = decoder.DecodeObject("imageType") as NSString;

            // Save data
            Name = name.ToString();
            ImageType = type.ToString ();
        }
        #endregion

        #region Public Methods
        [Export ("encodeWithCoder:")]
        public void EncodeTo (NSCoder encoder) {

            // Encode data
            encoder.Encode(new NSString(Name),"name");
            encoder.Encode(new NSString(ImageType),"imageType");
        }

        [Export ("writableTypesForPasteboard:")]
        public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
            string[] writableTypes = {"com.xamarin.image-info", "public.text"};
            return writableTypes;
        }

        [Export ("pasteboardPropertyListForType:")]
        public virtual NSObject GetPasteboardPropertyListForType (string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSKeyedArchiver.ArchivedDataWithRootObject(this);
            case "public.text":
                return new NSString(string.Format("{0}.{1}", Name, ImageType));
            }

            // Failure, return null
            return null;
        }

        [Export ("readableTypesForPasteboard:")]
        public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
            string[] readableTypes = {"com.xamarin.image-info", "public.text"};
            return readableTypes;
        }

        [Export ("readingOptionsForType:pasteboard:")]
        public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return NSPasteboardReadingOptions.AsKeyedArchive;
            case "public.text":
                return NSPasteboardReadingOptions.AsString;
            }

            // Default to property list
            return NSPasteboardReadingOptions.AsPropertyList;
        }

        [Export ("initWithPasteboardPropertyList:ofType:")]
        public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

            // Take action based on the requested type
            switch (type) {
            case "com.xamarin.image-info":
                return new ImageInfo();
            case "public.text":
                return new ImageInfo();
            }

            // Failure, return null
            return null;
        }
        #endregion
    }
}
    
```

In den folgenden Abschnitten wird diese Klasse ausführlich erläutert.

#### <a name="inheritance-and-interfaces"></a>Vererbung und Schnittstellen

Bevor eine benutzerdefinierte Datenklasse in ein "pasteboard" geschrieben oder daraus gelesen werden kann, muss Sie `INSPastebaordWriting` den `INSPasteboardReading` -und-Schnittstellen entsprechen. Außerdem muss Sie von `NSObject` erben und auch der `INSCoding` -Schnittstelle entsprechen:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Die-Klasse muss auch mit der `Register` -Direktive für "Ziel-C" verfügbar gemacht werden, und Sie muss alle erforderlichen Eigenschaften oder Methoden mit `Export`verfügbar machen. Beispiel:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Wir stellen die beiden Datenfelder bereit, die diese Klasse enthalten wird: den Namen des Bilds und den Typ (JPG, PNG usw.). 

Weitere Informationen finden Sie im Abschnitt "verfügbar machen von [ C# Klassen/Methoden zu Ziel-C](~/mac/internals/how-it-works.md) " der Dokumentation zu [xamarin. Mac Internals.](~/mac/internals/how-it-works.md) darin `Export` werden die Attribute und erläutert, C# die zum Verknüpfen der `Register` Klassen verwendet werden. Ziel-C-Objekte und UI-Elemente.

#### <a name="constructors"></a>Konstruktoren

Für unsere benutzerdefinierte Datenklasse sind zwei Konstruktoren (die für "Ziel-C" ordnungsgemäß verfügbar gemacht werden) erforderlich, damit Sie von einem "pasteboard" gelesen werden können:

```csharp
[Export ("init")]
public ImageInfo ()
{
}

[Export ("initWithCoder:")]
public ImageInfo(NSCoder decoder) {

    // Decode data
    NSString name = decoder.DecodeObject("name") as NSString;
    NSString type = decoder.DecodeObject("imageType") as NSString;

    // Save data
    Name = name.ToString();
    ImageType = type.ToString ();
}
```

Zuerst machen wir den _leeren_ Konstruktor unter der Standardmethode "Ziel-C" `init`von verfügbar.

Als nächstes machen wir einen `NSCoding` kompatiblen Konstruktor verfügbar, der verwendet wird, um eine neue Instanz des Objekts aus dem Zwischenablage zu erstellen, wenn Sie unter den exportierten Namen von `initWithCoder`einfügen.

Dieser Konstruktor nimmt einen `NSCoder` (wie von einem `NSKeyedArchiver` erstellt, wenn er in das-Steuerelement geschrieben wird), extrahiert die gekoppelten Schlüssel-Wert-Daten und speichert Sie in den Eigenschaften Feldern der Datenklasse.

#### <a name="writing-to-the-pasteboard"></a>Schreiben in das Zwischenablage

Durch die Konformität mit `INSPasteboardWriting` der-Schnittstelle müssen wir zwei Methoden und optional eine dritte Methode verfügbar machen, damit die Klasse in das pasteboard geschrieben werden kann.

Zuerst müssen wir dem Zwischenablage mitteilen, auf welche Datentyp Darstellungen die benutzerdefinierte Klasse geschrieben werden kann:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Jede Darstellung wird über einen URI (Uniform Type Identifier) identifiziert, bei dem es sich nicht mehr um eine einfache Zeichenfolge handelt, die den Typ der dargestellten Daten eindeutig identifiziert (Weitere Informationen finden Sie in der [Übersicht über die Uniform Type Identifiers](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) von Apple). Dokumentation).

Für unser benutzerdefiniertes Format erstellen wir unseren eigenen Bezeichner: "com. xamarin. Image-Info" (Beachten Sie, dass sich in umgekehrter Schreibweise wie ein App-Bezeichner befindet). Unsere Klasse ist auch in der Lage, eine Standard Zeichenfolge in das`public.text`Zwischenablage () zu schreiben. 

Als nächstes müssen wir das Objekt im angeforderten Format erstellen, das tatsächlich in das pasteboard geschrieben wird:

```csharp
[Export ("pasteboardPropertyListForType:")]
public virtual NSObject GetPasteboardPropertyListForType (string type) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSKeyedArchiver.ArchivedDataWithRootObject(this);
    case "public.text":
        return new NSString(string.Format("{0}.{1}", Name, ImageType));
    }

    // Failure, return null
    return null;
}
```

Für den `public.text` -Typ wird ein einfaches, formatiertes `NSString` -Objekt zurückgegeben. Für den Benutzer `com.xamarin.image-info` definierten Typ verwenden wir eine `NSKeyedArchiver` und die `NSCoder` -Schnittstelle, um die benutzerdefinierte Datenklasse in ein gekoppeltes Schlüssel-Wert-Archiv zu codieren. Wir müssen die folgende Methode implementieren, damit die Codierung tatsächlich behandelt wird:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Die einzelnen Schlüssel-Wert-Paare werden in den Encoder geschrieben und mit dem zweiten Konstruktor decodiert, den wir oben hinzugefügt haben.

Optional können Sie die folgende Methode einschließen, um beim Schreiben von Daten in das pasteboard Optionen zu definieren:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Derzeit ist nur `WritingPromised` die-Option verfügbar und sollte verwendet werden, wenn ein bestimmter Typ nur eine Zusage erhält, die eigentlich nicht in das pasteboard geschrieben wurde. Weitere Informationen finden Sie weiter oben im Abschnitt " [versprochene Daten](#Promised_Data) ".

Wenn diese Methoden vorhanden sind, kann der folgende Code verwendet werden, um die benutzerdefinierte Klasse in das pasteboard zu schreiben:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Lesen aus dem Zwischenablage

Durch die Konformität mit `INSPasteboardReading` der-Schnittstelle müssen wir drei Methoden verfügbar machen, damit die benutzerdefinierte Datenklasse aus dem pasteboard gelesen werden kann.

Zuerst müssen wir dem Zwischenablage mitteilen, welche Datentyp Darstellungen die benutzerdefinierte Klasse aus der Zwischenablage lesen kann:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Diese werden wiederum als einfache utis definiert und sind dieselben Typen, die wir im obigen Abschnitt Schreiben in den Abschnitt " **pasteboard** " definiert haben.

Als nächstes müssen wir dem Zwischenablage mitteilen, _wie_ die einzelnen UTI-Typen mit der folgenden Methode gelesen werden:

```csharp
[Export ("readingOptionsForType:pasteboard:")]
public static NSPasteboardReadingOptions GetReadingOptionsForType (string type, NSPasteboard pasteboard) {

    // Take action based on the requested type
    switch (type) {
    case "com.xamarin.image-info":
        return NSPasteboardReadingOptions.AsKeyedArchive;
    case "public.text":
        return NSPasteboardReadingOptions.AsString;
    }

    // Default to property list
    return NSPasteboardReadingOptions.AsPropertyList;
}
```

Für den `com.xamarin.image-info` -Typ weisen wir das Zwischenablage an, das Schlüssel-Wert-Paar, das wir `NSKeyedArchiver` mit erstellt haben, zu decodieren, wenn die Klasse durch Aufrufen des `initWithCoder:` Konstruktors, den wir der Klasse hinzugefügt haben, in das Zwischenablage geschrieben wurde.

Schließlich müssen wir die folgende Methode hinzufügen, um die anderen UTI-Daten Darstellungen aus dem pasteboard zu lesen:

```csharp
[Export ("initWithPasteboardPropertyList:ofType:")]
public NSObject InitWithPasteboardPropertyList (NSObject propertyList, string type) {

    // Take action based on the requested type
    switch (type) {
    case "public.text":
        return new ImageInfo();
    }

    // Failure, return null
    return null;
}
```

Wenn alle diese Methoden vorhanden sind, kann die benutzerdefinierte Datenklasse mithilfe des folgenden Codes aus dem Zwischenablage gelesen werden:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
var classArrayPtrs = new [] { Class.GetHandle (typeof(ImageInfo)) };
NSArray classArray = NSArray.FromIntPtrs (classArrayPtrs);

// NOTE: Sending messages directly to the base Objective-C API because of this defect:
// https://bugzilla.xamarin.com/show_bug.cgi?id=31760
// Check to see if image info is on the pasteboard
ok = bool_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("canReadObjectForClasses:options:"), classArray.Handle, IntPtr.Zero);

if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = NSArray.ArrayFromHandle<Foundation.NSObject>(IntPtr_objc_msgSend_IntPtr_IntPtr (pasteboard.Handle, Selector.GetHandle ("readObjectsForClasses:options:"), classArray.Handle, IntPtr.Zero));
    ImageInfo info = (ImageInfo)objectsToPaste[0];
}
```

### <a name="using-a-nspasteboarditem"></a>Verwenden eines nspasteboarditem

Es kann vorkommen, dass Sie benutzerdefinierte Elemente in das Zwischenablage schreiben müssen, die die Erstellung einer benutzerdefinierten Klasse nicht garantieren, oder wenn Sie Daten in einem gemeinsamen Format bereitstellen möchten, nur nach Bedarf. In diesen Fällen können Sie einen `NSPasteboardItem`verwenden.

Ein `NSPasteboardItem` ermöglicht eine differenzierte Steuerung der Daten, die in das-Steuerelement geschrieben werden, und ist für den temporären Zugriff vorgesehen. es sollte verworfen werden, nachdem es in das-Steuerelement geschrieben wurde.

#### <a name="writing-data"></a>Schreiben von Daten

Um benutzerdefinierte Daten in einen `NSPasteboardItem` zu schreiben, müssen Sie ein benutzerdefiniertes `NSPasteboardItemDataProvider`bereitstellen. Fügen Sie dem Projekt eine neue Klasse hinzu, und nennen Sie Sie **ImageInfoDataProvider.cs**. Bearbeiten Sie die Datei, und führen Sie Sie wie folgt aus:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacCopyPaste
{
    [Register("ImageInfoDataProvider")]
    public class ImageInfoDataProvider : NSPasteboardItemDataProvider
    {
        #region Computed Properties
        public string Name { get; set;}
        public string ImageType { get; set;}
        #endregion

        #region Constructors
        [Export ("init")]
        public ImageInfoDataProvider ()
        {
        }

        public ImageInfoDataProvider (string name, string imageType)
        {
            // Initialize
            this.Name = name;
            this.ImageType = imageType;
        }

        protected ImageInfoDataProvider (NSObjectFlag t){
        }

        protected internal ImageInfoDataProvider (IntPtr handle){

        }
        #endregion

        #region Override Methods
        [Export ("pasteboardFinishedWithDataProvider:")]
        public override void FinishedWithDataProvider (NSPasteboard pasteboard)
        {
            
        }

        [Export ("pasteboard:item:provideDataForType:")]
        public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
        {

            // Take action based on the type
            switch (type) {
            case "public.text":
                // Encode the data to string 
                item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
                break;
            }

        }
        #endregion
    }
}
```

Wie bei der benutzerdefinierten Datenklasse, müssen wir die-Direktive `Register` und `Export` die-Direktive verwenden, um Sie für "Ziel-C" verfügbar zu machen. Die Klasse muss von `NSPasteboardItemDataProvider` erben und die-Methode und die `FinishedWithDataProvider` - `ProvideDataForType` Methode implementieren.

Verwenden Sie `ProvideDataForType` die-Methode, um die Daten bereitzustellen, die `NSPasteboardItem` in den wie folgt umschließt werden:

```csharp
[Export ("pasteboard:item:provideDataForType:")]
public override void ProvideDataForType (NSPasteboard pasteboard, NSPasteboardItem item, string type)
{

    // Take action based on the type
    switch (type) {
    case "public.text":
        // Encode the data to string 
        item.SetStringForType(string.Format("{0}.{1}", Name, ImageType),type);
        break;
    }

}
```

In diesem Fall speichern wir zwei Informationen über unser Image (Name und ImageType) und schreiben diese in eine einfache Zeichenfolge (`public.text`).

Geben Sie die Daten in das pasteboard schreiben ein, und verwenden Sie den folgenden Code:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Using a Pasteboard Item
NSPasteboardItem item = new NSPasteboardItem();
string[] writableTypes = {"public.text"};

// Add a data provider to the item
ImageInfoDataProvider dataProvider = new ImageInfoDataProvider (Info.Name, Info.ImageType);
var ok = item.SetDataProviderForTypes (dataProvider, writableTypes);

// Save to pasteboard
if (ok) {
    pasteboard.WriteObjects (new NSPasteboardItem[] { item });
}
```

#### <a name="reading-data"></a>Lesen von Daten

Verwenden Sie den folgenden Code, um die Daten aus dem pasteboard zu lesen:

```csharp
// Initialize the pasteboard
NSPasteboard pasteboard = NSPasteboard.GeneralPasteboard;
Class [] classArray  = { new Class ("NSImage") };

bool ok = pasteboard.CanReadObjectForClasses (classArray, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray, null);
    NSImage image = (NSImage)objectsToPaste[0];

    // Do something with data
    ...
}
            
Class [] classArray2 = { new Class ("ImageInfo") };
ok = pasteboard.CanReadObjectForClasses (classArray2, null);
if (ok) {
    // Read the image off of the pasteboard
    NSObject [] objectsToPaste = pasteboard.ReadObjectsForClasses (classArray2, null);
    
    // Do something with data
    ...
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie mit dem Zwischenablage in einer xamarin. Mac-Anwendung arbeiten, um Kopier-und Einfügevorgänge zu unterstützen. Zuerst wurde ein einfaches Beispiel eingeführt, mit dem Sie sich mit standardmäßigen pasteboards-Vorgängen vertraut machen können. Als nächstes wurde das Zwischenablage ausführlich erläutert, und es wird erläutert, wie Daten daraus gelesen und geschrieben werden. Schließlich wurde die Verwendung eines benutzerdefinierten Datentyps zum unterstützen des Kopierens und Einfügevorgangs komplexer Datentypen in einer APP behandelt.

## <a name="related-links"></a>Verwandte Links

- [Maccopypaste (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/maccopypaste)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Leitfaden für die pasteboard-Programmierung](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [macOS-Eingaberichtlinien](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
