---
title: Durch Kopieren und Einfügen
description: Dieser Artikel behandelt die Arbeit mit der Zwischenablage kopieren und Einfügen in eine Anwendung Xamarin.Mac. Es wird gezeigt, wie zum Arbeiten mit standard-Datentypen, die mehrere apps und wie benutzerdefinierte Daten innerhalb einer bestimmten app unterstützt gemeinsam genutzt werden können.
ms.prod: xamarin
ms.assetid: 7E9C99FB-B7B4-4C48-B20F-84CB48543083
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cf81666403f687ce997e20f6f5f097dc9fcf1421
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="copy-and-paste"></a>Durch Kopieren und Einfügen

_Dieser Artikel behandelt die Arbeit mit der Zwischenablage kopieren und Einfügen in eine Anwendung Xamarin.Mac. Es wird gezeigt, wie zum Arbeiten mit standard-Datentypen, die mehrere apps und wie benutzerdefinierte Daten innerhalb einer bestimmten app unterstützt gemeinsam genutzt werden können._

## <a name="overview"></a>Übersicht

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleiche Montagefläche (Kopieren und Einfügen) Unterstützung, die ein Entwickler arbeiten in Objective-C verfügt.

In diesem Artikel wird es zwei Hauptmethoden zum Verwenden der Zwischenablage in eine app Xamarin.Mac behandelt:

1. **Standarddatentypen** -da Montagefläche-Vorgängen in der Regel zwischen zwei nicht verknüpfte apps durchgeführt werden, weiß weder app die Arten von Daten, die von den anderen unterstützt. Zur Maximierung der des Potenzial für die Freigabe der Zwischenablage kann mehrere Darstellungen eines angegebenen Elements (über einen standardmäßigen Satz von allgemeinen Datentypen) enthalten, dies kann die verbrauchende app Version auswählen, die für ihre Anforderungen am besten geeignet ist.
2. **Benutzerdefinierte Daten** – zur Unterstützung von kopieren und einfügen, komplexe Daten innerhalb Ihrer Xamarin.Mac können, definieren Sie einen benutzerdefinierten Datentyp, der von der Zwischenablage verarbeitet wird. Z. B. einen Vektor zeichnen app dem Benutzer ermöglicht, kopieren und Einfügen von komplexen Formen, die mehrere Datentypen und Punkten bestehen.

[![Beispiel für die ausgeführte app](copy-paste-images/intro01.png "Beispiel für die ausgeführte app")](copy-paste-images/intro01-large.png#lightbox)

In diesem Artikel wird beschrieben, die Grundlagen der Arbeit mit der Zwischenablage in einer Anwendung Xamarin.Mac zur Unterstützung von kopieren und einfügen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute zum Einrichten Ihrer C#-Klassen für Objective-C-Objekte und UI Elemente von Netzwerkdaten verwendet.

## <a name="getting-started-with-the-pasteboard"></a>Einstieg in die Zwischenablage

Der Arbeitsbereich stellt einen standardisierten Mechanismus zum Austauschen von Daten in einer bestimmten Anwendung oder zwischen Anwendungen. Die typische Verwendung für einen Arbeitsbereich in einer Anwendung Xamarin.Mac ist zum Behandeln von kopieren und einfügen, jedoch eine Anzahl von anderen Vorgängen auch (z. B. Drag & Drop und Anwendungsdienste) unterstützt werden.

Um Sie schnell aus dem Boden erhalten, werden wir mit einer einfachen und praktischen Einführung zur Verwendung von Montageflächen in einer Xamarin.Mac-app zu starten. Später stellen wir Ihnen eine nähere Erläuterung der Funktionsweise der Zwischenablage und die Methoden bereit.

In diesem Beispiel wird wir eine einfache basierendes Dokument-Anwendung erstellen, die ein Fenster mit einer Ansicht Image verwaltet. Der Benutzer wird kopieren und Einfügen von Bildern zwischen Dokumenten in der app zu oder von anderen apps oder mehrere Fenster innerhalb derselben app sein.

### <a name="creating-the-xamarin-project"></a>Erstellen die Xamarin-Projekt

Zunächst werden wir neue basierendes Dokument Xamarin.Mac-app erstellen, dass wir Hinzufügen von kopieren und fügen Sie Unterstützung für.

Führen Sie folgende Schritte aus:

1. Starten Sie Visual Studio für Mac, und klicken Sie auf die **neues Projekt...**  Link.
2. Wählen Sie **Mac** > **App** > **Kakao App**, klicken Sie dann auf die **Weiter** Schaltfläche: 

    [![Erstellen eines neuen Kakao app-Projekts](copy-paste-images/sample01.png "Erstellen eines neuen Kakao app-Projekts")](copy-paste-images/sample01-large.png#lightbox)
3. Geben Sie `MacCopyPaste` für die **Projektname** und alles andere als Standardeinstellung beibehalten. Klicken Sie auf Weiter: 

    [![Der Name des Projekts festlegen](copy-paste-images/sample01a.png "festlegen den Namen des Projekts")](copy-paste-images/sample01a-large.png#lightbox)

4. Klicken Sie auf die **erstellen** Schaltfläche: 

    [![Bestätigen die neue projekteinstellungen](copy-paste-images/sample02.png "neue projekteinstellungen bestätigen")](copy-paste-images/sample02-large.png#lightbox)

### <a name="add-an-nsdocument"></a>Hinzufügen einer NSDocument

Als Nächstes fügen wir der benutzerdefinierten `NSDocument` -Klasse, die als Hintergrund Speicher für die Benutzeroberfläche der Anwendung fungiert. Sie enthalten eine einzelne Image-Ansicht und wissen, wie ein Bild aus der Ansicht in der Standard-Zwischenablage kopieren und nehmen ein Bild aus der Zwischenablage Standard, und zeigen es in der Abbildung an.

Mit der rechten Maustaste auf das Projekt Xamarin.Mac in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neue Datei...** :

![Hinzufügen einer NSDocument zum Projekt](copy-paste-images/sample03.png "ein NSDocument zum Projekt hinzufügen")

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

Werfen wir einen Blick auf einigen Teilen des Codes im folgenden ausführlich.

Der folgende Code stellt eine Eigenschaft, um das Vorhandensein des Image-Daten auf den Standard-Arbeitsbereich testen, wenn ein Bild verfügbar ist, wird `true` wird zurückgegeben, andernfalls `false`:

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

Der folgende Code übernimmt ein Bild aus der Bildansicht angefügtes in der Standard-Arbeitsbereich:

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

Und der folgende Code Fügt ein Bild aus der Zwischenablage Standardeinstellung und (wenn der Zwischenablage ein gültiges Bild enthält) in der Bildansicht angefügtes angezeigt:

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

Mit diesem Dokument vorhanden erstellen wir die Benutzeroberfläche für die app Xamarin.Mac.

### <a name="building-the-user-interface"></a>Erstellen der Benutzeroberfläche

Doppelklicken Sie auf die **Main.storyboard** Datei, die sie in Xcode geöffnet. Als Nächstes fügen Sie auch eine Symbolleiste und ein Bild hinzu, und konfigurieren Sie sie wie folgt:

[![Bearbeiten die Symbolleiste](copy-paste-images/sample04.png "Bearbeiten der Symbolleiste")](copy-paste-images/sample04-large.png#lightbox)

Fügen Sie eine Kopie, und fügen Sie **Image Symbolleistenelement** auf der linken Seite der Symbolleiste. Wir verwenden diese Verknüpfungen kopieren und Einfügen aus dem Menü Bearbeiten. Als Nächstes fügen Sie vier **Symbolleiste Bildobjekte** auf die rechte Seite der Symbolleiste. Wir werden diese verwenden, um das Bild auch mit einigen Standardbilder aufzufüllen.

Weitere Informationen zum Arbeiten mit Symbolleisten finden Sie unter unsere [Symbolleisten](~/mac/user-interface/toolbar.md) Dokumentation.

Als Nächstes sehen wir verfügbar zu machen die folgenden Steckdosen und Aktionen für unsere Symbolleistenelemente und das Bild auch:

[![Erstellen von Steckdosen und Aktionen](copy-paste-images/sample05.png "erstellen Steckdosen und Aktionen")](copy-paste-images/sample05-large.png#lightbox)

Weitere Informationen zum Arbeiten mit Steckdosen und Aktionen finden Sie unter der [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Teil unserer [Hello, Mac](~/mac/get-started/hello-mac.md) Dokumentation.

### <a name="enabling-the-user-interface"></a>Aktivieren die Benutzeroberfläche

Mit unseren Benutzeroberfläche in Xcode und unsere Benutzeroberflächenelement verfügbar gemacht werden, über die Steckdosen und Aktionen erstellt haben müssen wir den Code zum Aktivieren der Benutzeroberflächenautomatisierungs hinzufügen. Doppelklicken Sie auf die **ImageWindow.cs** in der Datei die **Lösung Pad** , und stellen sie wie folgt aussehen:

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

Werfen wir einen Blick auf diesen Code im folgenden ausführlich.

Zunächst eine Instanz von zur Verfügung der `ImageDocument` -Klasse, die wir zuvor erstellt haben:

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

Mithilfe von `Export`, `WillChangeValue` und `DidChangeValue`, wir haben die `Document` Eigenschaft, um die Schlüssel-Wert zu codieren, und Binden von Daten in Xcode zu ermöglichen.

Wir richten sich auch um das Bild aus dem Abbild, das auch wir unserer GUI in Xcode mit der folgenden Eigenschaft hinzugefügt:

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

Wenn das Hauptfenster geladen und angezeigt wird, erstellen wir eine Instanz von unseren `ImageDocument` Klasse und ordnen Sie die Benutzeroberfläche Abbild auch mit den folgenden Code:

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

In der Antwort an den Benutzer durch Klicken auf die Symbolleistenelemente kopieren und einfügen, wir rufen Sie zum Schluss die Instanz von der `ImageDocument` Klasse, um die eigentliche Arbeit:

```csharp
partial void CopyImage (NSObject sender) {
    Document.CopyImage(sender);
}

partial void PasteImage (Foundation.NSObject sender) {
    Document.PasteImage(sender);
}
```

### <a name="enabling-the-file-and-edit-menus"></a>Aktivieren die Datei und bearbeiten-Menüs

Das letzte Ereignis, das erforderlich ist, Aktivieren der **neu** Menüelement aus der **Datei** Menü (zum Erstellen neuer Instanzen eines unserer im Hauptfenster) und ermöglichen die **Ausschneiden**, **kopieren**  und **einfügen** Menüelemente aus der **bearbeiten** Menü.

So aktivieren Sie die **neu** item, bearbeiten Sie im Menü der **AppDelegate.cs** Datei, und fügen Sie den folgenden Code hinzu:

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

Weitere Informationen finden Sie unter der [arbeiten mit mehreren Fenstern](~/mac/user-interface/window.md) Teil unserer [Windows](~/mac/user-interface/window.md) Dokumentation.

So aktivieren Sie die **Ausschneiden**, **kopieren** und **einfügen** Menüelemente, bearbeiten die **AppDelegate.cs** Datei, und fügen Sie den folgenden Code hinzu:

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

Für jedes Menüelement wir erhalten Sie die aktuelle oberste wichtigsten Fenster, und wandeln Sie sie in unserem `ImageWindow` Klasse:

```csharp
var window = NSApplication.SharedApplication.KeyWindow as ImageWindow;
```

Von dort aus nennen wir die `ImageDocument` Klasseninstanz dieses Fensters behandeln das Kopieren und Einfügen von Aktionen. Zum Beispiel: 

```csharp
window.Document.CopyImage (sender);
```

Nur Being **Ausschneiden**, **Kopie** und **einfügen** Menüelemente werden verfügbar, wenn es ist imagedaten auf die Standard-Arbeitsbereich oder in der Abbildung auch von der aktuellen aktiven Fensters.

Fügen wir eine **EditMenuDelegate.cs** -Datei in das Xamarin.Mac-Projekt, und stellen sie wie folgt aussehen:

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

Erneut, wir die aktuellen, oberste Fenster abrufen und verwenden seiner `ImageDocument` Klasseninstanz um festzustellen, ob die erforderlichen Bilddaten vorhanden ist. Wir verwenden die `MenuWillHighlightItem` Methode zum Aktivieren oder deaktivieren jedes Element auf Grundlage dieses Zustands.

Bearbeiten der **AppDelegate.cs** Datei, und stellen die `DidFinishLaunching` Methode aussehen wie folgt:
 
```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Disable automatic item enabling on the Edit menu
    EditMenu.AutoEnablesItems = false;
    EditMenu.Delegate = new EditMenuDelegate ();
}
```

Zunächst deaktivieren wir das automatische Aktivieren und Deaktivieren von Menüelementen im Menü Bearbeiten. Als Nächstes fügen wir eine Instanz von der `EditMenuDelegate` -Klasse, die wir zuvor erstellt haben.

Weitere Informationen finden Sie unter unsere [Menüs](~/mac/user-interface/menu.md) Dokumentation.

### <a name="testing-the-app"></a>Testen der app

Alles vorhanden werden wir zum Testen der Anwendung bereit. Erstellen und Ausführen der app und die Hauptkomponente der Benutzeroberfläche wird angezeigt:

![Ausführen der Anwendung](copy-paste-images/run01.png "Ausführen der Anwendung")

Wenn Sie im Menü Bearbeiten öffnen, beachten Sie, dass **Ausschneiden**, **Kopie** und **einfügen** sind deaktiviert, da kein Bild in der Abbildung gut oder in der Standard-Arbeitsbereich vorhanden sein:

![Öffnen im Menü Bearbeiten](copy-paste-images/run02.png "öffnen im Menü Bearbeiten")

Wenn Sie das Bild auch ein Bild hinzu, und öffnen im Menü Bearbeiten, werden die Elemente jetzt aktiviert werden:

![Zeigt die Menüelemente bearbeiten aktiviert sind](copy-paste-images/run03.png "zeigt die Menüelemente bearbeiten aktiviert sind")

Wenn Sie das Abbild kopieren, und wählen Sie **neu** über das Dateimenü können Sie sich das Bild in das neue Fenster einfügen:

![Ein Bild in einem neuen Fenster einfügen](copy-paste-images/run04.png "ein Bild in einem neuen Fenster einfügen")

In den folgenden Abschnitten fügen wir eine ausführliche Übersicht über das Arbeiten mit der Zwischenablage in einer Anwendung Xamarin.Mac dauern.

## <a name="about-the-pasteboard"></a>Über die Zwischenablage

In MacOS (ehemals OS X) der Zwischenablage (`NSPasteboard`) bieten Unterstützung für mehrere Server verarbeitet werden, z. B. kopieren und einfügen, Drag & Drop und Anwendungsdienste. In den folgenden Abschnitten führen wir mehrere Montagefläche Schlüsselkonzepte näher betrachten.

### <a name="what-is-a-pasteboard"></a>Was ist ein Arbeitsbereich?

Die `NSPasteboard` -Klasse bietet einen standardisierten Mechanismus zum Austauschen von Informationen zwischen Anwendungen oder innerhalb einer bestimmten app. Die Hauptfunktion des ein Arbeitsbereich ist für die Behandlung von Kopier- und Einfügevorgänge:

1. Wenn der Benutzer wählt ein Element in einer app, und verwendet die **Ausschneiden** oder **Kopie** Menüelement klicken, werden ein oder mehrere Darstellungen des ausgewählten Elements in der Zwischenablage platziert.
2. Wenn der Benutzer verwendet die **einfügen** Menüelement (innerhalb derselben app oder einer anderen), die Version der Daten, die er behandeln kann aus der Zwischenablage kopiert und die app hinzugefügt wird.

Weniger offensichtlich Montagefläche verwendet enthalten, suchen, ziehen Sie, Drag & Drop, und Anwendungsdienste Vorgänge:

- Wenn der Benutzer einen Ziehvorgang initiiert, ist die Drag-Daten in die Zwischenablage kopiert. Wenn eine Drop auf eine andere app der Ziehvorgang endet, kopiert dieser app die Daten aus der Zwischenablage an.
- Für Übersetzungsdienste ist die Daten übersetzt werden von der anfordernden app in die Zwischenablage kopiert. Der Anwendungsdienst Ruft die Daten aus der Zwischenablage ab, die Übersetzung ist, und fügt die Daten in die Zwischenablage zurück.

In ihrer einfachsten Form werden Montageflächen zum Verschieben von Daten innerhalb einer bestimmten app oder zwischen apps und Verlaufsebene in einem bestimmten globalen Speicherbereich außerhalb der app-Prozess vorhanden sein. Während die Konzepte von der Montageflächen problemlos sind ertönt, es sind mehrere komplexere Details, die berücksichtigt werden müssen. Diese werden nachfolgend detailliert behandelt.

### <a name="named-pasteboards"></a>Benannte Montageflächen

Ein Arbeitsbereich kann öffentlich oder privat sein und dürfen für verschiedene Zwecke verwenden, in einer Anwendung oder zwischen mehreren apps verwendet werden. MacOS bietet mehrere standard Montageflächen, jeweils mit einem bestimmten, gut definierter Auslastung:

- `NSGeneralPboard` -Die Standard-Arbeitsbereich für **Ausschneiden**, **Kopie** und **einfügen** Vorgänge.
- `NSRulerPboard` – Unterstützt **Ausschneiden**, **Kopie** und **einfügen** Vorgänge für **Lineale**.
- `NSFontPboard` – Unterstützt **Ausschneiden**, **Kopie** und **einfügen** Vorgänge für `NSFont` Objekte.
- `NSFindPboard` -Unterstützt anwendungsspezifische suchen Panels, an denen Suchtext freigeben können.
- `NSDragPboard` – Unterstützt **Drag & Drop** Vorgänge.

In den meisten Fällen verwenden Sie eines der vom System definierte Montageflächen. Aber es gibt möglicherweise Situationen, in denen Ihnen die Erstellung eigener Montageflächen erfordern. In diesen Fällen können Sie die `FromName (string name)` Methode der `NSPasteboard` Klasse, um einen benutzerdefinierten Arbeitsbereich mit dem angegebenen Namen zu erstellen.

Optional können Sie rufen die `CreateWithUniqueName` Methode der `NSPasteboard` Klasse, um eine eindeutig benannte Arbeitsbereich zu erstellen.

### <a name="pasteboard-items"></a>Montagefläche Elemente

Jedes Datenelement, das eine Anwendung in einem Arbeitsbereich schreibt gilt eine _Arbeitsbereich Element_ und ein Arbeitsbereich kann mehrere Elemente gleichzeitig speichern. Auf diese Weise kann eine Anwendung schreiben, dass mehrere Versionen der Daten auf einen Arbeitsbereich (z. B. nur-Text und formatierter Text) und der Abruf von app kopiert werden, nur die Daten lesen können, (z. B. nur die nur-Text) verarbeitet werden können.

### <a name="data-representations-and-uniform-type-identifiers"></a>Darstellung von Daten und alle denselben Typ-IDs

Montagefläche Vorgänge dauern in der Regel zwischen zwei (oder mehr)-Anwendungen, die jeweils anderen oder die Datentypen nicht bekannt ist, dass jeder behandelt werden kann. Wie im vorherigen Abschnitt erwähnt, kann zum Maximieren des Potenzial für den Informationsaustausch, einen Arbeitsbereich mehrere Darstellungen der Daten kopiert und eingefügt wird enthalten.

Identifiziert jede Darstellung über eine einheitliche Art Bezeichner (UTI), also nichts weiter als eine einfache Zeichenfolge, die eindeutig den Typ des Datums angezeigt wird (Weitere Informationen finden Sie in der Apple- [Uniform Typ-IDs (Übersicht) ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) Dokumentation). 

Wenn Sie einen benutzerdefinierten Datentyp (z. B. ein Objekt in die eines Vektors, indem die app) erstellen, können Sie eigene UTI um eindeutig identifizieren Sie ihn kopieren und einfügen erstellen.

Wenn eine app vorbereitet hat, eine Zwischenablage kopierte Daten einzufügen, müssen sie die Darstellung suchen, die am besten die Fähigkeiten (sofern vorhanden). Dies ist in der Regel wird der umfassendste Typ verfügbar (z. B. formatierten Text für ein Textverarbeitungsprogramm erstellten app), Fallback auf den einfachsten Formularen als erforderlich (nur-Text für einen einfachen Text-Editor) verfügbar.

<a name="Promised_Data" />

### <a name="promised-data"></a>Zugesagten Daten

Im Allgemeinen sollten Sie so viele Darstellungen der Daten kopiert werden, wie möglich zu maximieren, die gemeinsame Nutzung apps bereitstellen. Aufgrund der Zeit- oder Einschränkungen, könnte es unpraktisch, eigentlich jeden Datentyp in der Zwischenablage schreiben werden.

In diesem Fall die erste datendarstellung in der Zwischenablage platziert werden können, und die empfangende Anwendung kann eine andere Darstellung, die generierte auf dynamische unmittelbar vor dem Einfügevorgang kann anfordern.

Wenn Sie das erste Element in der Zwischenablage ablegen, werden Sie angeben, dass eine oder mehrere der anderen verfügbaren Darstellungen von einem Objekt bereitgestellt werden, die entspricht der `NSPasteboardItemDataProvider` Schnittstelle. Diese Objekte stellt die zusätzlichen Darstellungen on Demand bereit, wie von der empfangenden Anwendung angefordert.

### <a name="change-count"></a>Ändern der Anzahl

Jeder Arbeitsbereich verwaltet eine _Anzahl_ als bei einzelnen Schritten einen neuen Besitzer Ausführung deklariert ist. Eine app kann feststellen, wenn Inhalt der Zwischenablage seit der letzten Ausführung sie geändert haben durch Überprüfung des Werts von der Anzahl der untersucht.

Verwenden der `ChangeCount` und `ClearContents` Methoden die `NSPasteboard` Klasse, um die Anzahl der einem bestimmten Arbeitsbereich zu ändern.

## <a name="copying-data-to-a-pasteboard"></a>Kopieren von Daten in einem Arbeitsbereich

Einen Kopiervorgang wird ausgeführt, indem die ersten Zugriff auf einen Arbeitsbereich, löschen alle vorhandenen Inhalte und so viele Darstellungen der Daten als erforderlich sind, um der Zwischenablage schreiben.

Zum Beispiel:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add the current image to the pasteboard
pasteboard.WriteObjects (new NSImage[] {image});
```

In der Regel müssen Sie auf die allgemeine Zwischenablage verfasst werden wie wir im obigen Beispiel getan haben. Jedes Objekt, das Sie zum Senden der `WriteObjects` Methode *müssen* entsprechen den `INSPasteboardWriting` Schnittstelle. Mehrere integrierte Klassen (z. B. `NSString`, `NSImage`, `NSURL`, `NSColor`, `NSAttributedString`, und `NSPasteboardItem`) automatisch auf diese Schnittstelle entsprechen.

Wenn Sie eine Klasse benutzerdefinierte Daten in die Zwischenablage schreiben müssen entsprechen die `INSPasteboardWriting` -Schnittstelle oder in einer Instanz von umschlossen werden die `NSPasteboardItem` Klasse (finden Sie unter der [benutzerdefinierte Datentypen](#Custom_Data_Types) weiter unten im Abschnitt).

## <a name="reading-data-from-a-pasteboard"></a>Lesen von Daten aus einem Arbeitsbereich

Wie bereits erwähnt, können zum Maximieren des Potenzial für die Freigabe von Daten zwischen apps zu mehrere Darstellungen der kopierten Daten in die Zwischenablage geschrieben. Es liegt im Ermessen der empfangenden app, die die umfassendste Version möglich, dass seine Funktionen auswählen (sofern vorhanden).

### <a name="simple-paste-operation"></a>Einfache Einfügevorgang.

Sie Daten lesen, aus dem Montagefläche mithilfe der `ReadObjectsForClasses` Methode. Sie benötigen zwei Parameter:

1. Ein Array von `NSObject` basierten Klassentypen, die aus der Zwischenablage gelesen werden soll. Sie sollten dies mit den am häufigsten gewünschten Datentyp zunächst alle verbleibenden Typen in absteigender Voreinstellung Reihenfolge.
2. Ein Wörterbuch mit zusätzlichen Einschränkungen (z. B. bestimmte URL-Inhaltstypen beschränken) oder ein leeres Wörterbuch, wenn keine weiteren Einschränkungen erforderlich sind.

Die Methode gibt ein Array von Elementen, die die Kriterien erfüllen, denen wir übergeben und enthält daher höchstens die gleiche Anzahl von Datentypen, die angefordert werden. Es ist auch möglich, dass keiner der angeforderten Typen vorhanden sind und ein leeres Array zurückgegeben.

Im folgenden Codebeispiel beispielsweise überprüft, ob ein `NSImage` im Allgemeinen Arbeitsbereich vorhanden ist und in ein Bild sowie angezeigt wird, wenn dies der Fall ist:

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

Basierend auf den Typ der Xamarin.Mac-Anwendung erstellt wird, kann es mehrere Darstellungen der eingefügten Daten verarbeiten können. In diesem Fall kann gibt es zwei Szenarien für das Abrufen von Daten aus der Zwischenablage:

1. Stellen Sie nur einen Aufruf der `ReadObjectsForClasses` -Methode und über die ein Array aller von den Darstellungen, die Sie (in der gewünschten Reihenfolge) verwendet werden soll.
2. Mehrere Aufrufe an die `ReadObjectsForClasses` Methode, die in der ein anderes Array von Typen, jedes Mal.

Finden Sie unter der **einfache Einfügevorgang** im Abschnitt oben für Weitere Informationen zum Abrufen von Daten aus einem Arbeitsbereich.

### <a name="checking-for-existing-data-types"></a>Überprüfung auf vorhandenen Datentypen

Es gibt Situationen, die überprüft, ob ein Arbeitsbereich eine Darstellung der angegebenen Daten enthält, ohne die Daten tatsächlich aus der Zwischenablage gelesen werden sollen (z. B. das Aktivieren der **einfügen** Menüelement nur, wenn Sie gültige Daten vorhanden sind).

Rufen Sie die `CanReadObjectForClasses` Methode im Arbeitsbereich, um festzustellen, ob es sich um einen angegebenen Typ enthält.

Z. B. der folgende Code ermittelt, wenn die allgemeine Arbeitsbereich enthält eine `NSImage` Instanz:

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

Basierend auf die Funktion von einer bestimmten Xamarin.Mac-app, es möglicherweise erforderlich, um die URLs aus einem Arbeitsbereich gelesen, jedoch nur, wenn sie eine bestimmte Gruppe von Kriterien (z. B. auf Dateien oder URLs für einen bestimmten Datentyp verweist) erfüllen. In diesem Fall können Sie angeben zusätzlicher Suchkriterien, die mit dem zweiten Parameter von der `CanReadObjectForClasses` oder `ReadObjectsForClasses` Methoden.

<a name="Custom_Data_Types" />

## <a name="custom-data-types"></a>Benutzerdefinierte Datentypen

Es gibt gelegentlich Sie Ihre eigenen benutzerdefinierten Typen aus einer app Xamarin.Mac der Zwischenablage speichern müssen. Z. B. eines Vektors, indem app, die dem Benutzer, kopieren und Einfügen ermöglicht, Zeichnen von Objekten.

In diesem Fall müssen Sie die benutzerdefinierte Datenklasse so entwerfen, dass es erbt `NSObject` und es einige Schnittstellen entspricht (`INSCoding`, `INSPasteboardWriting` und `INSPasteboardReading`). Optional können Sie eine `NSPasteboardItem` zum Kapseln von Daten kopiert oder eingefügt werden soll.

Beide Optionen werden im folgenden ausführlich behandelt.

### <a name="using-a-custom-class"></a>Verwendung einer benutzerdefinierten Klasse

In diesem Abschnitt wird werden als Erweiterung der einfachen Beispiel-app, die wir am Anfang dieses Dokuments erstellt und Hinzufügen einer benutzerdefinierte Klasse zum Nachverfolgen von Informationen zu dem Bild, das wir kopieren und Einfügen zwischen Fenstern.

Das Projekt eine neue Klasse hinzu, und rufen sie **ImageInfo.cs**. Bearbeiten Sie die Datei, und stellen sie wie folgt aussehen:

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

In den folgenden Abschnitten führen wir eine ausführliche Übersicht über diese Klasse.

#### <a name="inheritance-and-interfaces"></a>Vererbung und Schnittstellen

Vor eine Klasse benutzerdefinierte Daten geschrieben oder aus einem Arbeitsbereich gelesen werden kann, muss die entsprechen die `INSPastebaordWriting` und `INSPasteboardReading` Schnittstellen. Darüber hinaus müssen Sie sich von erben `NSObject` und auch entsprechen den `INSCoding` Schnittstelle:

```csharp
[Register("ImageInfo")]
public class ImageInfo : NSObject, INSCoding, INSPasteboardWriting, INSPasteboardReading
...
```

Die Klasse muss für Objective-C auch verfügbar gemacht werden mithilfe der `Register` Richtlinie, und es müssen alle erforderlichen Eigenschaften oder Methoden verfügbar zu machen `Export`. Zum Beispiel:

```csharp
[Export("name")]
public string Name { get; set; }

[Export("imageType")]
public string ImageType { get; set; }
```

Wir werden die beiden Felder der Daten, die diese Klasse enthält - Namen für das Abbild und dessen (Jpg, Png, usw.) verfügbar gemacht werden. 

Weitere Informationen finden Sie unter der [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) Dokumentation, es wird erläutert, die `Register` und `Export` Attribute zum Einrichten Ihrer C#-Klassen für Objective-C-Objekte und UI Elemente von Netzwerkdaten verwendet.

#### <a name="constructors"></a>Konstruktoren

Zwei Konstruktoren (ordnungsgemäß verfügbar gemacht, Objective-C) werden für unsere benutzerdefinierte Klasse erforderlich, damit er von einem Arbeitsbereich gelesen werden kann:

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

Zunächst zur Verfügung der _leere_ Konstruktor unter die Objective-C-Standardmethode der `init`.

Wir als Nächstes verfügbar machen eine `NSCoding` kompatibler Konstruktor, der verwendet wird, um eine neue Instanz des Objekts aus der Zwischenablage zu erstellen, wenn mit dem exportierten Namen einfügen `initWithCoder`.

Dieser Konstruktor akzeptiert ein `NSCoder` (erstellten eine `NSKeyedArchiver` beim Schreiben in die Zwischenablage), extrahiert die Schlüssel/Wert-kombiniert Daten und speichert sie in die Eigenschaftenfelder der Datenklasse fest.

#### <a name="writing-to-the-pasteboard"></a>Das Schreiben in die Zwischenablage

Durch die mit der `INSPasteboardWriting` -Schnittstelle, wir zwei Methoden, und optional eine dritte Methode verfügbar machen müssen, damit die Klasse in die Zwischenablage geschrieben werden kann.

Zunächst müssen wir teilen Sie den Arbeitsbereich welchen Darstellungen Datentyp, denen die benutzerdefinierte Klasse geschrieben werden kann:

```csharp
[Export ("writableTypesForPasteboard:")]
public virtual string[] GetWritableTypesForPasteboard (NSPasteboard pasteboard) {
    string[] writableTypes = {"com.xamarin.image-info", "public.text"};
    return writableTypes;
}
```

Identifiziert jede Darstellung über eine einheitliche Art Bezeichner (UTI), also nichts weiter als eine einfache Zeichenfolge, die eindeutig den Typ der Daten, die angezeigt wird (Weitere Informationen finden Sie in der Apple- [Uniform Typ-IDs (Übersicht) ](https://developer.apple.com/library/prerelease/mac/documentation/FileManagement/Conceptual/understanding_utis/understand_utis_intro/understand_utis_intro.html#//apple_ref/doc/uid/TP40001319) Dokumentation).

Für unsere benutzerdefinierten Format erstellen wir unser eigenes UTI: "com.xamarin.image-Info" (Beachten Sie, dass in der umgekehrten Schreibweise genau wie ein App-Bezeichner). Unsere Klasse kann auch beim Schreiben einer Standardzeichenfolge in die Zwischenablage (`public.text`). 

Als Nächstes müssen wir das Objekt im gewünschten Format zu erstellen, die tatsächlich in die Zwischenablage geschrieben ruft:

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

Für die `public.text` geben, werden wir eine einfache zurückgeben formatiert `NSString` Objekt. Für das benutzerdefinierte `com.xamarin.image-info` Typ, verwenden wir eine `NSKeyedArchiver` und `NSCoder` Schnittstelle, um die benutzerdefinierten Daten-Klasse, um ein Schlüssel/Wert-gekoppelt Archiv zu codieren. Wir müssen die folgende Methode, um tatsächlich behandelt die Codierung zu implementieren:

```csharp
[Export ("encodeWithCoder:")]
public void EncodeTo (NSCoder encoder) {

    // Encode data
    encoder.Encode(new NSString(Name),"name");
    encoder.Encode(new NSString(ImageType),"imageType");
}
```

Die einzelnen Schlüssel/Wert-Paare, die an den Encoder geschrieben werden und werden mit dem zweiten Konstruktor, dem wir über hinzugefügt decodiert werden.

Optional können wir die folgende Methode, um Optionen zu definieren, beim Schreiben von Daten in die Zwischenablage einschließen:

```csharp
[Export ("writingOptionsForType:pasteboard:"), CompilerGenerated]
public virtual NSPasteboardWritingOptions GetWritingOptionsForType (string type, NSPasteboard pasteboard) {
    return NSPasteboardWritingOptions.WritingPromised;
}
```

Derzeit wird nur der `WritingPromised` Option ist verfügbar und sollte verwendet werden, wenn eines angegebenen Typs nur zugesagten und in die Zwischenablage nicht tatsächlich geschrieben wird. Weitere Informationen finden Sie unter der [zugesagten Daten](#Promised_Data) obigen Abschnitt.

Mit diesen Methoden vorhanden kann der folgende Code verwendet werden, um unsere benutzerdefinierte Klasse in die Zwischenablage zu schreiben:

```csharp
// Get the standard pasteboard
var pasteboard = NSPasteboard.GeneralPasteboard;

// Empty the current contents
pasteboard.ClearContents();

// Add info to the pasteboard
pasteboard.WriteObjects (new ImageInfo[] { Info });
```

#### <a name="reading-from-the-pasteboard"></a>Das Lesen aus der Zwischenablage

Durch die mit der `INSPasteboardReading` -Schnittstelle, müssen wir drei Methoden verfügbar zu machen, damit die Klasse benutzerdefinierte Daten aus der Zwischenablage gelesen werden kann.

Zunächst müssen wir teilen Sie der Zwischenablage Daten Darstellungen eingeben, die die benutzerdefinierte Klasse aus der Zwischenablage gelesen werden kann:

```csharp
[Export ("readableTypesForPasteboard:")]
public static string[] GetReadableTypesForPasteboard (NSPasteboard pasteboard){
    string[] readableTypes = {"com.xamarin.image-info", "public.text"};
    return readableTypes;
}
```

Erneut, diese werden als einfache UTIs definiert und sind die gleichen Typen, die wir in definiert die **Schreiben in die Zwischenablage** obigen Abschnitt.

Als Nächstes müssen wir informieren den Arbeitsbereich _wie_ aller UTI-Typen wird mithilfe des folgenden Verfahrens gelesen werden:

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

Für die `com.xamarin.image-info` Typ, sagen wir der Zwischenablage das Schlüssel/Wert-Paar zu decodieren, die wir mit erstellt die `NSKeyedArchiver` beim Schreiben der Klasse, der Zwischenablage durch Aufrufen der `initWithCoder:` Konstruktor, der der Klasse hinzugefügt.

Schließlich müssen wir die folgende Methode zum Lesen von der anderen UTI Darstellung von Daten aus der Zwischenablage hinzufügen:

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

Mit diesen Methoden vorhanden kann die Klasse benutzerdefinierte Daten aus der Zwischenablage mit dem folgenden Code gelesen werden:

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

Es gibt möglicherweise Zeiten, wenn Sie beim Schreiben von benutzerdefinierten Elemente in die Zwischenablage, die nicht die Erstellung einer benutzerdefinierten Klasse rechtfertigen, führen Sie oder benötigen Sie Daten in ein einheitliches Format, nur nach Bedarf bereitstellen möchten. In diesen Situationen können Sie eine `NSPasteboardItem`.

Ein `NSPasteboardItem` bietet eine präzisere Kontrolle über die Daten, die in die Zwischenablage geschrieben, und eignet sich für vorübergehenden Zugriff auf - es sollten beseitigt werden, nachdem es in die Zwischenablage geschrieben wurde.

#### <a name="writing-data"></a>Schreiben von Daten

Schreiben Sie Ihre benutzerdefinierten Daten zur ein `NSPasteboardItem` Sie müssen eine benutzerdefinierte bereitstellen `NSPasteboardItemDataProvider`. Das Projekt eine neue Klasse hinzu, und rufen sie **ImageInfoDataProvider.cs**. Bearbeiten Sie die Datei, und stellen sie wie folgt aussehen:

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

Wie wir mit der Klasse benutzerdefinierte Daten, müssen wir verwenden die `Register` und `Export` Direktiven für die bereitzustellenden zu Objective-c aus. Die Klasse muss von erben `NSPasteboardItemDataProvider` und Implementieren der `FinishedWithDataProvider` und `ProvideDataForType` Methoden.

Verwenden der `ProvideDataForType` Methode, um die Daten bereitzustellen, die in umbrochen wird die `NSPasteboardItem` wie folgt:

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

In diesem Fall wir zwei Angaben zu unserem Abbild (Name und ImageType) speichern und diese in eine einfache Zeichenfolge zu schreiben (`public.text`).

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

Verwenden Sie zum Lesen der Daten aus der Zwischenablage erneut den folgenden Code ein:

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

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit der Zwischenablage in einer Anwendung Xamarin.Mac zur Unterstützung von kopieren und Einfügen übernommen. Eingeführt sie ein einfaches Beispiel, um Sie mit standardmäßigen Montageflächen-Operationen vertraut. Als Nächstes dauert eine ausführliche Übersicht über die Zwischenablage und das Lesen und Schreiben von Daten daraus. Schließlich, betrachtet er mit einem benutzerdefinierten Datentyp unterstützen das Kopieren und Einfügen von komplexen Datentypen in einer app.



## <a name="related-links"></a>Verwandte Links

- [MacCopyPaste (sample)](https://developer.xamarin.com/samples/mac/MacCopyPaste/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Montagefläche Programmierhandbuch](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/PasteboardGuide106/Articles/pbGettingStarted.html)
- [MacOS von menschlichen Richtlinien zur Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/overview/themes/)
