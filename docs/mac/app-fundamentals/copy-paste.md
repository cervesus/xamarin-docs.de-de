---
title: Kopieren und Einfügen in Xamarin.Mac
description: Dieser Artikel behandelt die Verwendung der Zwischenablage kopieren und fügen in einer Xamarin.Mac-Anwendung. Es zeigt das Arbeiten mit standard-Datentypen, die mehrere apps und Unterstützung für benutzerdefinierte Daten innerhalb einer bestimmten app gemeinsam verwendet werden können.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: f9e05b6d16210021257fe3958966739e526aed18
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123204"
---
# <a name="copy-and-paste-in-xamarinmac"></a>Kopieren und Einfügen in Xamarin.Mac

_Dieser Artikel behandelt die Verwendung der Zwischenablage kopieren und fügen in einer Xamarin.Mac-Anwendung. Es zeigt das Arbeiten mit standard-Datentypen, die mehrere apps und Unterstützung für benutzerdefinierte Daten innerhalb einer bestimmten app gemeinsam verwendet werden können._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleiche Montagefläche (Kopieren und Einfügen) Unterstützung, die ein Entwickler in Objective-C verfügt.

In diesem Artikel werden wir die zwei Hauptmethoden zum Verwenden der Zwischenablage in einer Xamarin.Mac-app zu folgenden Themen werden:

1. **Standarddatentypen** -da Montagefläche Vorgänge in der Regel zwischen zwei nicht verknüpfte apps ausgeführt werden, handelt es sich bei keiner app weiß die Typen von Daten, die andere zu unterstützen. Der Arbeitsbereich kann mehrere Darstellungen eines angegebenen Elements, das (über einen standardmäßigen Satz allgemeine Datentypen) enthalten, dadurch, dass der verwendeten app, die die Version auswählen, die für ihre Anforderungen am besten geeignet ist, um das Potenzial für die Freigabe zu maximieren.
2. **Benutzerdefinierte Daten** – unterstützen das Kopieren und Einfügen von komplexen Daten in Ihre Xamarin.Mac können Sie definieren, einen benutzerdefinierten Datentyp, der von der Zwischenablage verarbeitet wird. Z. B. einen Vektor zeichnen app dem Benutzer ermöglicht, kopieren und Einfügen von komplexen Formen, die von mehreren Datentypen und Punkten bestehen.

[![Beispiel für die ausgeführte app](copy-paste-images/intro01.png "Beispiel für die ausgeführte app")](copy-paste-images/intro01-large.png#lightbox)

In diesem Artikel behandeln wir die Grundlagen der Arbeit mit der Zwischenablage in einer Xamarin.Mac-Anwendung zum Unterstützen von kopieren und einfügen. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Benutzeroberfläche Elemente zu verknüpfen.

## <a name="getting-started-with-the-pasteboard"></a>Erste Schritte mit der Zwischenablage

Im gedruckten bietet einen standardisierten Mechanismus zum Austauschen von Daten innerhalb einer bestimmten Anwendung oder zwischen Anwendungen. Die typische Verwendung für einen Arbeitsbereich in einer Xamarin.Mac-Anwendung ist, behandeln kopieren und einfügen, jedoch eine Anzahl von anderen Vorgängen (z. B. Drag & Drop und Anwendungsdienste) ebenfalls unterstützt werden.

Um Sie schnell anfangen, werden wir mit der eine einfache praktische Einführung in die Verwendung von Montageflächen in einer Xamarin.Mac-app zu starten. Später stellen wir eine ausführliche Erläuterung der Funktionsweise von der Zwischenablage und die verwendeten Methoden bereit.

In diesem Beispiel werden wir eine einfache basierend Dokuments-Anwendung erstellen, die ein Fenster mit einer Ansicht Image verwaltet. Der Benutzer wird kopieren und Einfügen von Bildern zwischen Dokumenten, die in der app und oder zu anderen apps oder mehrere Fenster in der gleichen app sein.

### <a name="creating-the-xamarin-project"></a>Erstellen das Xamarin-Projekt

Zunächst werden wir neue basierend Dokuments Xamarin.Mac-app erstellen, dass wir hinzufügen kopieren und fügen Sie Unterstützung für.

Führen Sie folgende Schritte aus:

1. Starten Sie Visual Studio für Mac, und klicken Sie auf die **neues Projekt...**  Link.
2. Wählen Sie **Mac** > **App** > **Cocoa-App**, klicken Sie dann auf die **Weiter** Schaltfläche: 

    [![Erstellen eines neuen Cocoa-app-Projekts](copy-paste-images/sample01.png "Erstellen eines neuen Cocoa-app-Projekts")](copy-paste-images/sample01-large.png#lightbox)
3. Geben Sie `MacCopyPaste` für die **Projektname** und alles andere als Standard. Klicken Sie auf Weiter: 

    [![Festlegen des Namens des Projekts](copy-paste-images/sample01a.png "Festlegen des Namens des Projekts")](copy-paste-images/sample01a-large.png#lightbox)

4. Klicken Sie auf die **erstellen** Schaltfläche: 

    [![Bestätigen die neue projekteinstellungen](copy-paste-images/sample02.png "bestätigen die neue projekteinstellungen")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Eine NSDocument hinzufügen

Als Nächstes fügen wir benutzerdefinierte `NSDocument` Klasse, die als Hintergrund Speicher für die Benutzeroberfläche der Anwendung fungiert. Sie enthalten eine Einzelansicht-Image und wissen, wie Sie ein Image aus der Ansicht in der Standardarbeitsbereichs kopieren und wie Sie ein Image aus dem Standardarbeitsbereichs in der Image-Ansicht anzeigen.

Mit der rechten Maustaste auf die Xamarin.Mac-Projekt in der **Lösungspad** , und wählen Sie **hinzufügen** > **neue Datei...** :

![Dem Projekt eine NSDocument hinzugefügt](copy-paste-images/sample03.png "eine NSDocument zum Projekt hinzugefügt.")

Geben Sie für den **Namen** `ImageDocument` ein, und klicken Sie auf **Neu**. Bearbeiten der **ImageDocument.cs** Klasse, und stellen sie wie folgt aussehen:

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

Werfen wir einen Blick auf einige der im Code unten im Detail.

Der folgende Code stellt eine Eigenschaft, um das Vorhandensein des Image-Daten auf den Standardarbeitsbereichs zu testen, wenn ein Bild verfügbar ist, ist `true` wird zurückgegeben, andernfalls `false`:

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

Der folgende Code kopiert ein Bild aus der angefügten Bilds-Ansicht in der Standard-Arbeitsbereich:

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

Und der folgende Code Fügt ein Bild aus der Standardarbeitsbereichs und wird in der Ansicht des angefügten Bilds (wenn im gedruckten kein gültiges Image enthält):

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

Mit diesem Dokument vorhanden erstellen wir die Benutzeroberfläche für die Xamarin.Mac-app.

### <a name="building-the-user-interface"></a>Erstellen der Benutzeroberfläche

Doppelklicken Sie auf die **"Main.Storyboard"** Datei, die sie in Xcode geöffnet. Als Nächstes fügen Sie auch eine Symbolleiste und ein Bild hinzu, und konfigurieren Sie sie wie folgt:

[![Bearbeiten der Symbolleiste](copy-paste-images/sample04.png "Bearbeiten der Symbolleiste")](copy-paste-images/sample04-large.png#lightbox)

Fügen Sie eine Kopie, und fügen Sie **Image Symbolleistenelement** auf die linke Seite der Symbolleiste. Wir verwenden diese Verknüpfungen kopieren und Einfügen aus dem Menü "Bearbeiten". Als Nächstes fügen Sie vier **Image Symbolleistenelemente** auf die rechte Seite der Symbolleiste. Wir verwenden diese, um das Bild auch mit einigen Standardbilder aufzufüllen.

Weitere Informationen zum Arbeiten mit der Symbolleisten finden Sie unserem [Symbolleisten](~/mac/user-interface/toolbar.md) Dokumentation.

Dann machen wir die folgenden Outlets und Aktionen für Elemente der Symbolleiste und das Bild auch:

[![Erstellen von Outlets und Aktionen](copy-paste-images/sample05.png "erstellen Outlets und Aktionen")](copy-paste-images/sample05-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Outlets und Aktionen finden Sie unter den [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Teil unserer [Hallo, Mac](~/mac/get-started/hello-mac.md) Dokumentation.

### <a name="enabling-the-user-interface"></a>Aktivieren die Benutzeroberfläche

Mit unserer Benutzeroberfläche in Xcode und unsere UI-Element, das über einen Outlets und Aktionen verfügbar gemachte erstellt haben müssen wir den Code zum Aktivieren der Benutzeroberflächenautomatisierungs hinzufügen. Doppelklicken Sie auf die **ImageWindow.cs** Datei die **Lösungspad** und legen Sie ihn wie folgt aussehen:

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

Werfen wir einen Blick auf diesen Code unten im Detail.

Zuerst machen wir eine Instanz von der `ImageDocument` -Klasse, die wir zuvor erstellt haben:

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

Mithilfe von `Export`, `WillChangeValue` und `DidChangeValue`, dass Setup die `Document` Eigenschaft, um Schlüssel / Wert-Codierung und die Datenbindung in Xcode zu ermöglichen.

Wir machen auch das Image aus dem Image, das auch wir zu unserer Benutzeroberfläche in Xcode mit der folgenden Eigenschaft hinzugefügt:

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

Wenn das Hauptfenster geladen und angezeigt wird, erstellen wir eine Instanz von unserem `ImageDocument` Klasse und ordnen Sie der Benutzeroberfläche-Abbild auch, mit dem folgenden Code:

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

Als Antwort auf den Benutzer durch Klicken auf die Symbolleistenelemente kopieren und einfügen, wir rufen Sie abschließend die Instanz von der `ImageDocument` Klasse, um die eigentliche Arbeit leisten:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Aktivieren die Datei "und" Bearbeiten-Menüs

Der letzte Schritt, der erforderlich ist, ist, aktivieren Sie die **neu** Menüelement der **Datei** Menü (um neue Instanzen von unserem Hauptfenster erstellen) und ermöglichen die **Ausschneiden**, **kopieren**  und **einfügen** Menüelemente aus der **bearbeiten** Menü.

So aktivieren Sie die **neu** , bearbeiten Sie im Menü der **Datei "appdelegate.cs"** Datei, und fügen Sie den folgenden Code hinzu:

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

Weitere Informationen finden Sie unter den [arbeiten mit mehreren Windows](~/mac/user-interface/window.md) Teil unserer [Windows](~/mac/user-interface/window.md) Dokumentation.

So aktivieren Sie die **Ausschneiden**, **kopieren** und **einfügen** Menüelemente bearbeiten der **Datei "appdelegate.cs"** Datei, und fügen Sie den folgenden Code hinzu:

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

Für jedes Menü, wir erhalten Sie aktuelle, wichtige oberste Fenster, und wandeln Sie sie in unserem `ImageWindow` Klasse:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Von dort aus rufen wir die `ImageDocument` Klasseninstanz dieses Fensters behandeln das Kopieren und einfügen. Zum Beispiel: 

```csharp
window.Document.CopyImage (sender);
```

Nur **Ausschneiden**, **Kopie** und **einfügen** Menüelemente darauf zugreifen können, ist es image-Daten auf den Standard-Arbeitsbereich oder in der Abbildung und der das momentan aktive Fenster.

Fügen Sie eine **EditMenuDelegate.cs** -Datei in dem Xamarin.Mac-Projekt, und stellen sie wie folgt aussehen:

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

In diesem Fall wir den aktuelle, oberste Fenster zu erhalten und verwenden dessen `ImageDocument` Klasseninstanz aus, um festzustellen, ob die erforderlichen Bildgröße Daten vorhanden sind. Anschließend wir verwenden die `MenuWillHighlightItem` zu aktivieren oder deaktivieren jedes Element basierend auf diesen Zustand.

Bearbeiten der **Datei "appdelegate.cs"** Datei, und stellen die `DidFinishLaunching` Methode sehen wie folgt:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Deaktivieren Sie es zuerst das automatische Aktivieren und Deaktivieren von Menüelementen in das Menü "Bearbeiten". Als Nächstes fügen wir eine Instanz von der `EditMenuDelegate` -Klasse, die wir zuvor erstellt haben.

Weitere Informationen finden Sie unserem [Menüs](~/mac/user-interface/menu.md) Dokumentation.

### <a name="testing-the-app"></a>Testen der App

Nach Abschluss der Vorbereitung können wir die Anwendung zu testen. Erstellen und Ausführen der app und die Hauptkomponente der Benutzeroberfläche angezeigt wird:

![Ausführen der Anwendung](copy-paste-images/run01.png "Ausführen der Anwendung")

Wenn Sie im Menü Bearbeiten öffnen, beachten Sie, dass **Ausschneiden**, **Kopie** und **einfügen** sind deaktiviert, da kein Bild in der Abbildung gut oder in der Standard-Arbeitsbereich vorhanden sein:

![Öffnen das Menü "Bearbeiten"](copy-paste-images/run02.png "öffnen das Menü \"Bearbeiten\"")

Wenn Sie das Bild auch ein Bild hinzugefügt, und öffnen das Menü "Bearbeiten", werden die Elemente jetzt aktiviert werden:

![Die Bearbeiten-Menü-Elemente werden angezeigt, sind aktiviert](copy-paste-images/run03.png "aktiviert sind die Bearbeiten-Menü-Elemente werden angezeigt")

Wenn Sie das Abbild kopieren, und wählen Sie **neu** über das Dateimenü können Sie das Image in das neue Fenster einfügen:

![Ein Bild in einem neuen Fenster einfügen](copy-paste-images/run04.png "ein Bild in einem neuen Fenster einfügen")

In den folgenden Abschnitten werden wir einen detaillierten Einblick in die Arbeit mit der Zwischenablage in einer Xamarin.Mac-Anwendung nutzen.

## <a name="about-the-pasteboard"></a>Über die Zwischenablage

MacOS (früher als OS X) im gedruckten (`NSPasteboard`) bieten Sie Unterstützung für mehrere Server verarbeitet werden, z. B. kopieren und einfügen, Drag & Drop und Anwendungsdienste. In den folgenden Abschnitten werden wir einen genaueren Blick auf einige Schlüsselkonzepte Montagefläche dauern.

### <a name="what-is-a-pasteboard"></a>Was ist ein Arbeitsbereich?

Die `NSPasteboard` -Klasse bietet einen standardisierten Mechanismus zum Austausch von Informationen zwischen Anwendungen oder innerhalb einer bestimmten app. Die main-Funktion von einem Arbeitsbereich ist für die Behandlung von Kopier-und Einfügevorgänge:

1. Wenn der Benutzer wählt ein Element in einer app, und verwendet die **Ausschneiden** oder **Kopie** Menüelement eine oder mehrere Darstellungen des ausgewählten Elements in der Zwischenablage platziert werden.
2. Wenn der Benutzer verwendet die **einfügen** Menüelement (innerhalb derselben app oder ein anderes Gerät), ist die Version der Daten, die er verarbeiten kann aus der Zwischenablage kopiert und der app hinzugefügt.

Hauptsächlich für weniger offensichtliche Montagefläche verwendet suchen, ziehen Sie Drag & Drop, und Application services-Vorgänge:

- Wenn der Benutzer einen Ziehvorgang initiiert, werden die Drag-Daten in die Zwischenablage kopiert. Wenn eine Drop auf eine andere app der Ziehvorgang endet, kopiert die app die Daten aus der Zwischenablage ein.
- Für Translation-Dienste werden übersetzt werden die Daten in im gedruckten von der anfordernden app kopiert. Anwendungsdienst Ruft die Daten aus der Zwischenablage ab, die Übersetzung ist, und fügt die Daten in der Zwischenablage zurück.

In ihrer einfachsten Form werden Montageflächen zum Verschieben von Daten in einer bestimmten app oder zwischen apps und deshalb in einer Region besonderen globaler Speicher außerhalb der app-Prozesses vorhanden sind. Die Konzepte der Montageflächen zwar problemlos ertönt, es gibt einige komplexere Details, die berücksichtigt werden müssen. Dies werden weiter unten ausführlich erläutert.

### <a name="named-pasteboards"></a>Benannte Montageflächen

Ein Arbeitsbereich kann öffentlich oder privat sein und kann für eine Vielzahl von Zwecken, die in einer Anwendung oder zwischen mehreren apps verwendet werden. MacOS bietet mehrere standard Montageflächen, jeweils mit einer bestimmten, gut abgegrenzten Verwendung:

- `NSGeneralPboard` – Die Standardarbeitsbereichs für **Ausschneiden**, **Kopie** und **einfügen** Vorgänge.
- `NSRulerPboard` -Unterstützt **Ausschneiden**, **Kopie** und **einfügen** Vorgänge für **Lineale**.
- `NSFontPboard` -Unterstützt **Ausschneiden**, **Kopie** und **einfügen** Vorgänge für `NSFont` Objekte.
- `NSFindPboard` – Unterstützung von anwendungsspezifischen finden Sie Bereiche, die Suchtext freigeben können.
- `NSDragPboard` -Unterstützt **Drag & Drop** Vorgänge.

In den meisten Fällen verwenden Sie eines der vom System definierten Montageflächen. Aber es gibt möglicherweise Situationen, in denen Ihnen die Erstellung eigener Montageflächen erfordern. In diesen Fällen können Sie die `FromName (string name)` Methode der `NSPasteboard` Klasse, um einen benutzerdefinierten Arbeitsbereich mit dem angegebenen Namen zu erstellen.

Optional können Sie rufen die `CreateWithUniqueName` -Methode der der `NSPasteboard` Klasse, um eine eindeutig benannte Arbeitsbereich zu erstellen.

### <a name="pasteboard-items"></a>Montagefläche Elemente

Jedes Datenelement, das eine Anwendung in einem Arbeitsbereich schreibt gilt eine _Arbeitsbereich Element_ und ein Arbeitsbereich kann mehrere Elemente gleichzeitig speichern. Auf diese Weise kann eine app schreiben, dass mehrere Versionen der Daten in einem Arbeitsbereich (z. B. nur-Text und formatierten Text) und der beim Abrufen der Anwendung kopiert werden nur die Daten lesen können, (z. B. nur den nur-Text) verarbeitet werden können.

### <a name="data-representations-and-uniform-type-identifiers"></a>Datendarstellungen und denselben Typ-IDs

Montagefläche Vorgänge dauern in der Regel zwischen zwei (oder mehr)-Anwendungen, die jeweils anderen oder die Datentypen nicht bekannt ist, dass jede verarbeiten kann. Wie im vorherigen Abschnitt erwähnt, um das Potenzial für den Informationsaustausch, kann einen Arbeitsbereich mehrere Darstellungen der Daten kopiert und eingefügt wird enthalten.

Identifiziert jede Darstellung über eine einheitliche Typ Bezeichner (UTI), handelt es sich nicht mehr als eine einfache Zeichenfolge, die eindeutig den Typ des Datums, der angezeigt wird (Weitere Informationen finden Sie unter Apple [Uniform Übersicht über die Typ-IDs ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) Dokumentation). 

Wenn Sie einen benutzerdefinierten Datentyp (z. B. ein zeichnen-Objekt in einem Vektor mit dem Zeichnen der app) erstellen, können Sie Ihre eigenen UTI eindeutig in der Kopie zu ermitteln und Einfügevorgänge erstellen.

Wenn eine app zum Einfügen von Daten aus einem Arbeitsbereich kopiert haben vorbereitet, müssen sie die Darstellung suchen, die am besten dessen Fähigkeiten (sofern vorhanden). Diese werden in der Regel den größten Typ verfügbar sind (z. B. formatierten Text für eine app für die Textverarbeitung), auf die einfachste Formulare als erforderlich (nur-Text für einen einfachen Text-Editor) zur Verfügung.

<a name="Promised_Data" />

### <a name="promised-data"></a>Zugesagten Daten

Im Allgemeinen sollten Sie so viele Darstellungen der Daten kopiert werden, wie möglich zum Maximieren der Datenfreigabe zwischen apps bereitstellen. Allerdings kann möglicherweise aufgrund von Zeit- oder Einschränkungen, eigentlich jeden Datentyp in die Zwischenablage geschrieben werden.

Sie können die Darstellung der Daten für das erste platzieren, in die Zwischenablage, und die empfangende Anwendung kann eine andere Darstellung, die generierten auf dynamische unmittelbar vor dem Einfügevorgang sein können anfordern, in diesem Fall.

Wenn Sie das erste Element in der Zwischenablage platzieren, werden Sie angeben, dass eine oder mehrere der anderen verfügbaren Darstellungen von einem Objekt bereitgestellt werden, das entspricht, dem `NSPasteboardItemDataProvider` Schnittstelle. Diese Objekte stellt bei Bedarf die zusätzlichen Darstellungen bereit, wie von der empfangenden app angefordert.

### <a name="change-count"></a>Anzahl der

Jeder Arbeitsbereich verwaltet eine _Änderungsanzahl_ , einzelnen Schritten einen neuen Besitzer Zeit deklariert werden. Eine app kann bestimmt, ob der Zwischenablage der Inhalt seit dem letzten es geändert anhand des Werts von der Anzahl der Zustandsänderungen überprüft.

Verwenden der `ChangeCount` und `ClearContents` Methoden der `NSPasteboard` Klasse so ändern Sie einen bestimmten Arbeitsbereich die Anzahl der Zustandsänderungen.

## <a name="copying-data-to-a-pasteboard"></a>Kopieren von Daten in einem Arbeitsbereich

Ausführen ein Kopiervorgangs nach erste den Zugriff auf einen Arbeitsbereich, löschen alle vorhandenen Inhalte und Schreiben so viele Darstellungen der Daten wie im gedruckten erforderlich sind.

Zum Beispiel:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

In der Regel müssen Sie nur im Allgemeinen gedruckten, schreibt in werden wie im obigen Beispiel haben. Jedes Objekt, das Sie zum Senden der `WriteObjects` Methode *müssen* entsprechen den `INSPasteboardWriting` Schnittstelle. Mehrere integrierte Klassen (z. B. `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, und `NSPasteboardItem`) automatisch auf diese Schnittstelle entsprechen.

Wenn Sie eine benutzerdefinierte Datenklasse in die Zwischenablage schreiben sie entsprechen den `INSPasteboardWriting` Schnittstelle oder in einer Instanz von umschlossen werden die `NSPasteboardItem` Klasse (finden Sie unter der [benutzerdefinierte Datentypen](#Custom_Data_Types) weiter unten im Abschnitt).

## <a name="reading-data-from-a-pasteboard"></a>Lesen von Daten aus einem Arbeitsbereich

Wie oben erwähnt, können um das Potenzial für die Freigabe von Daten zwischen apps, mehrere Darstellungen der kopierten Daten in die Zwischenablage geschrieben werden. Es ist Aufgabe der empfangenden app, die die größten Version möglich, dass die Funktionen auswählen (sofern vorhanden).

### <a name="simple-paste-operation"></a>Einfache Einfügen-Vorgang

Lesen Sie Daten aus der Montagefläche mithilfe der `ReadObjectsForClasses` Methode. Sie benötigen zwei Parameter:

1. Ein Array von `NSObject` basierend Klassentypen, die Sie aus der Zwischenablage lesen möchten. Dies bestellen mit dem am häufigsten gewünschten Datentyp zuerst alle übrigen Typen in absteigender Einstellung Sie.
2. Ein Wörterbuch mit zusätzlichen Einschränkungen (z. B. auf bestimmte URL-Inhaltstypen zu beschränken) oder ein leeres Wörterbuch, wenn keine weiteren Einschränkungen erforderlich sind.

Die Methode gibt ein Array von Elementen, die die Kriterien erfüllen, denen wir übergeben und enthält daher höchstens die gleiche Anzahl von Datentypen, die angefordert werden. Es ist auch möglich, dass keiner der angeforderten Typen vorhanden sind und ein leeres Array zurückgegeben.

Im folgenden Codebeispiel beispielsweise überprüft, ob ein `NSImage` vorhanden ist, im Allgemeinen Arbeitsbereich befindet, und klicken Sie in eine Bildquelle angezeigt wird, wenn dies der Fall ist:

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

### <a name="requesting-multiple-data-types"></a>Mehrere Datentypen anfordern

Basierend auf den Typ der Xamarin.Mac-Anwendung erstellt wird, kann es mehrere Darstellungen der eingefügten Daten zu verarbeiten sein. In diesem Fall gibt es zwei Szenarien für das Abrufen von Daten aus der Zwischenablage:

1. Ausführen eines einzelnen Aufrufs an die `ReadObjectsForClasses` -Methode und ein Array aller der Darstellungen, die gewünschte (in der gewünschten Reihenfolge) bereitstellen.
2. Mehrere Aufrufe an die `ReadObjectsForClasses` jedes Mal-Methode, die in der ein anderes Array von Typen.

Finden Sie unter den **einfachen Einfügevorgang** Abschnitt ausführliche Informationen zum Abrufen von Daten aus einem Arbeitsbereich.

### <a name="checking-for-existing-data-types"></a>Überprüfen auf vorhandenen Datentypen

Es gibt Fälle, in denen Sie überprüfen, ob ein Arbeitsbereich eine Darstellung der angegebenen Daten enthält, ohne tatsächlich Lesen von Daten aus der Zwischenablage möchten (z. B. das Aktivieren der **einfügen** Menüelement nur, wenn gültige Daten vorhanden sind).

Rufen Sie die `CanReadObjectForClasses` Methode im Arbeitsbereich aus, um festzustellen, ob es sich um einen angegebenen Typ enthält.

Der folgende Code z. B. ermittelt wird, wenn im Allgemeinen gedruckten enthält eine `NSImage` Instanz:

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

### <a name="reading-urls-from-the-pasteboard"></a>Lesen von Urls aus der Zwischenablage

Basierend auf der Funktion einer gegebenen Xamarin.Mac-App, es möglicherweise erforderlich, um die URLs aus einem Arbeitsbereich zu lesen, jedoch nur, wenn sie eine bestimmte Gruppe von Kriterien (wie z. B. auf Dateien oder URLs, die von einem bestimmten Datentyp verweist) erfüllen. In diesem Fall können Sie angeben, zusätzlichen Suchkriterien, die mit dem zweiten Parameter von der `CanReadObjectForClasses` oder `ReadObjectsForClasses` Methoden.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Benutzerdefinierte Datentypen

Es gibt Situationen, wenn Sie eigene benutzerdefinierten Typen in der Zwischenablage in einer Xamarin.Mac-app zu speichern müssen. Z. B. ein Vektor, zeichnen die app, die dem Benutzer ermöglicht, kopieren und einfügen, die Objekte zeichnen.

In diesem Fall müssen Sie Ihrer benutzerdefinierten Datenklasse so entwerfen, dass es erbt `NSObject` und es einige Schnittstellen entspricht (`INSCoding`, `INSPasteboardWriting` und `INSPasteboardReading`). Optional können Sie verwenden eine `NSPasteboardItem` zum Kapseln der Daten, die kopiert oder eingefügt werden.

Beide Optionen werden weiter unten ausführlich behandelt.

### <a name="using-a-custom-class"></a>Verwendung einer benutzerdefinierten Klasse

In diesem Abschnitt werden wir erweitern in der einfachen Beispiel-app, die wir zu Beginn dieses Dokuments erstellt und Hinzufügen einer benutzerdefinierte Klasse zum Nachverfolgen von Informationen über das Image, das wir kopieren und Einfügen zwischen Fenstern werden.

Fügen Sie dem Projekt eine neue Klasse und nenne **ImageInfo.cs**. Bearbeiten Sie die Datei, und stellen sie wie folgt aussehen:

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

In den folgenden Abschnitten werden wir einen detaillierten Einblick in diese Klasse nutzen.

#### <a name="inheritance-and-interfaces"></a>Vererbung und Schnittstellen

Bevor Sie eine benutzerdefinierte Datenklasse geschrieben, oder eine Zwischenablage gelesen werden kann, muss die entsprechen die `INSPastebaordWriting` und `INSPasteboardReading` Schnittstellen. Darüber hinaus müssen sie erbt von `NSObject` und auch entsprechen den `INSCoding` Schnittstelle:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Die Klasse muss mit Objective-C auch verfügbar gemacht werden mithilfe der `Register` Richtlinie, und es müssen alle erforderlichen Eigenschaften oder Methoden mit verfügbar zu machen `Export`. Zum Beispiel:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Wir werden die beiden Felder der Daten, die diese Klasse enthält – des Bilds-Namen und den Typ (Jpg, Png, usw.) verfügbar gemacht werden. 

Weitere Informationen finden Sie unter den [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) Dokumentation, es wird erläutert, die `Register` und `Export` Attribute verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Benutzeroberfläche Elemente zu verknüpfen.

#### <a name="constructors"></a>Konstruktoren

Zwei Konstruktoren (ordnungsgemäß verfügbar gemacht werden, der mit Objective-C entspricht) werden für unsere benutzerdefinierte Klasse erforderlich, damit sie aus der ein Zwischenablage gelesen werden kann:

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

Zuerst machen wir die _leere_ Konstruktor in die Objective-C-Standardmethode der `init`.

Als Nächstes, wir machen einen `NSCoding` konformer Konstruktor, der verwendet wird, um eine neue Instanz des Objekts aus der Zwischenablage zu erstellen, beim Einfügen in den exportierten Namen des `initWithCoder`.

Dieser Konstruktor akzeptiert ein `NSCoder` (erstellten eine `NSKeyedArchiver` beim Schreiben in die Zwischenablage), extrahiert die Daten für die Schlüssel-Wert zugeordnet, und speichert es in die Eigenschaftenfelder der Datenklasse.

#### <a name="writing-to-the-pasteboard"></a>Das Schreiben in die Zwischenablage

Indem Sie mit der `INSPasteboardWriting` -Schnittstelle, wir müssen zwei Methoden, und optional eine dritte Methode verfügbar zu machen, damit die Klasse in die Zwischenablage geschrieben werden kann.

Zunächst müssen wir teilen im gedruckten Daten Darstellungen eingeben, denen die benutzerdefinierte Klasse geschrieben werden kann:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Identifiziert jede Darstellung über eine einheitliche Typ Bezeichner (UTI), handelt es sich nicht mehr als eine einfache Zeichenfolge, die eindeutig den Typ der Daten, die angezeigt wird (Weitere Informationen finden Sie unter Apple [Uniform Übersicht über die Typ-IDs ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) Dokumentation).

Für unsere benutzerdefinierten Formats, d.h. erstellen wir unsere eigene UTI: "com.xamarin.image-Info" (Beachten Sie, dass in der umgekehrten Notation wie eine App-ID). Unsere Klasse kann auch eine Standardzeichenfolge in der Zwischenablage zu schreiben (`public.text`). 

Als Nächstes müssen wir zum Erstellen des Objekts im angeforderten Format, die tatsächlich in die die Zwischenablage geschrieben wird:

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

Für die `public.text` Typ zurückgeben wir eine einfache, formatiert `NSString` Objekt. Für das benutzerdefinierte `com.xamarin.image-info` Typ verwenden wir eine `NSKeyedArchiver` und `NSCoder` Schnittstelle zum Codieren der benutzerdefinierten Datenklasse in ein Archiv mit Schlüssel-Wert zugeordnet. Wir müssen die folgende Methode zur tatsächlich verarbeitet die Codierung zu implementieren:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Die einzelnen Schlüssel/Wert-Paare werden an den Encoder geschrieben und werden mit dem zweiten Konstruktor, dem wir oben hinzugefügt decodiert werden.

Optional können wir die folgende Methode, um alle Optionen zu definieren, beim Schreiben von Daten in die Zwischenablage einschließen:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Derzeit werden nur die `WritingPromised` Option ist verfügbar und sollte verwendet werden, wenn ein bestimmtes Typs nur versprochen und nicht in die Zwischenablage geschrieben. Weitere Informationen finden Sie unter den [versprochen Daten](#Promised_Data) weiter oben.

Mit diesen Methoden vorhanden kann der folgende Code verwendet werden, um unsere benutzerdefinierte Klasse in die Zwischenablage zu schreiben:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Lesen aus der Zwischenablage

Indem Sie mit der `INSPasteboardReading` Schnittstelle müssen wir drei Methoden verfügbar zu machen, damit die benutzerdefinierten Datenklasse, die aus der Zwischenablage gelesen werden kann.

Zunächst müssen wir teilen im gedruckten Daten Darstellungen geben, die die benutzerdefinierte Klasse aus der Zwischenablage gelesen werden können:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

In diesem Fall diese werden als einfache UTIs definiert und sind die gleichen Typen, die wir in definiert die **Schreiben in die Zwischenablage** weiter oben.

Als Nächstes müssen wir im gedruckten teilen _wie_ aller UTI-Typen werden mit dem folgenden Verfahren gelesen werden:

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

Für die `com.xamarin.image-info` Typ, weisen wir im gedruckten Schlüssel/Wert-Paar zu decodieren, die wir mit erstellt die `NSKeyedArchiver` beim Schreiben der Klasse in die der Zwischenablage durch Aufrufen der `initWithCoder:` Konstruktor, der der Klasse hinzugefügt.

Abschließend müssen wir die folgende Methode zum Lesen von den anderen UTI-datendarstellungen aus der Zwischenablage hinzufügen:

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

Mit diesen Methoden vorhanden kann die benutzerdefinierten Datenklasse aus der Zwischenablage mit dem folgenden Code gelesen werden:

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

### <a name="using-a-nspasteboarditem"></a>Verwenden eine NSPasteboardItem

Es gibt möglicherweise Zeiten, wenn Sie benutzerdefinierte Elemente in die Zwischenablage, die die Erstellung einer benutzerdefinierten Klasse nicht zu rechtfertigen, führen Sie schreiben müssen oder Sie Daten in ein einheitliches Format, nur nach Bedarf bereitstellen möchten. In diesen Situationen können Sie eine `NSPasteboardItem`.

Ein `NSPasteboardItem` bietet präzise steuerungsmöglichkeiten über die Daten, die in die Zwischenablage geschrieben werden und dient zur temporären Zugriff - es sollten verworfen, nachdem es in die Zwischenablage geschrieben wurde.

#### <a name="writing-data"></a>Beim Schreiben von Daten

Schreiben Sie Ihren benutzerdefinierten Daten zu einer `NSPasteboardItem` müssen Sie eine benutzerdefinierte bieten `NSPasteboardItemDataProvider`. Fügen Sie dem Projekt eine neue Klasse und nenne **ImageInfoDataProvider.cs**. Bearbeiten Sie die Datei, und stellen sie wie folgt aussehen:

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

Wie wir mit der benutzerdefinierten Datenklasse, müssen wir verwenden die `Register` und `Export` Anweisungen für die sie für Objective-c verfügbar machen Die Klasse erben muss `NSPasteboardItemDataProvider` und Implementieren der `FinishedWithDataProvider` und `ProvideDataForType` Methoden.

Verwenden der `ProvideDataForType` Methode, um die Daten bereitzustellen, die in eingebunden werden soll, wird die `NSPasteboardItem` wie folgt:

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

In diesem Fall sind wir Speichern von zwei Arten von Informationen zu unserem Image ("Name" und "ImageType") und schreiben, die mit einer einfachen Zeichenfolge (`public.text`).

Typ schreiben die Daten in die Zwischenablage, verwenden Sie den folgenden Code:

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

Um die Daten aus der Zwischenablage zu lesen, verwenden Sie den folgenden Code:

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

In diesem Artikel wurde unternommen, einen detaillierten Einblick in die Arbeit mit der Zwischenablage in einer Xamarin.Mac-Anwendung zum Unterstützen von kopieren und einfügen. Zuerst eingeführt es ein einfaches Beispiel, um Sie mit standard Montageflächen Operationen vertraut zu machen. Als Nächstes dauert einen detaillierten Einblick in die Zwischenablage und lesen und Schreiben von Daten aus. Schließlich haben sie einen benutzerdefinierten Datentyp zur Unterstützung von kopieren und Einfügen von komplexen Datentypen in einer app mit.



## <a name="related-links"></a>Verwandte Links

- [MacCopyPaste (Beispiel)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Montagefläche-Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [MacOS Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
