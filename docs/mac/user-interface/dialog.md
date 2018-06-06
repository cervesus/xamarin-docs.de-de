---
title: Dialogfelder in Xamarin.Mac
description: In diesem Artikel wird das Arbeiten mit Dialogfeldern und modale Fenster in einer Anwendung Xamarin.Mac behandelt. Es beschreibt das Erstellen von modalen Fenstern in Xcode und Schnittstelle-Generator arbeiten mit Standarddialoge und interagieren mit diesen Steuerelementen im C#-Code.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7d9a93c8503d7e25f098e871378a22455b597e90
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792693"
---
# <a name="dialogs-in-xamarinmac"></a>Dialogfelder in Xamarin.Mac

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleichen Dialogfelder und modale Fenster, die ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ zu erstellen und Verwalten Ihrer modale Fenster (oder erstellen sie optional direkt im C#-Code).

Ein Dialogfeld angezeigt, die als Antwort auf eine Benutzeraktion wird und in der Regel enthält, dass Benutzer die Aktion abgeschlossen werden können. Ein Dialogfeld erfordert eine Antwort des Benutzers an, bevor sie geschlossen werden kann.

Windows kann, modale (z. B. ein Dialogfeld "Export", die verworfen werden muss, bevor die Anwendung fortgesetzt werden kann) oder in einem nicht modalen Status (z. B. einem Text-Editor, der mehrere Dokumente gleichzeitig geöffnet sein kann) verwendet werden.

[![](dialog-images/dialog03.png "Ein geöffnetes Dialogfenster")](dialog-images/dialog03.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Dialogfeldern und modale Fenster in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Einführung in Dialogfeldern

Ein Dialogfeld wird angezeigt, in Reaktion auf eine Benutzeraktion (z. B. das Speichern einer Datei) und bietet eine Möglichkeit für Benutzer zum Abschließen dieser Aktion. Ein Dialogfeld erfordert eine Antwort des Benutzers an, bevor sie geschlossen werden kann.

Gemäß den Apple an gibt es drei Möglichkeiten zum Präsentieren von einem Dialogfeld:

- **Dokumentieren Sie modale** -ein Dokument modales Dialogfeld verhindert, dass den Benutzer auf diese Weise Sonstiges innerhalb eines angegebenen Dokuments, solange sie geöffnet ist.
- **App-modale** – eine App modales Dialogfeld verhindert, dass den Benutzer der Anwendung interagieren, solange sie geöffnet ist.
- **Ohne Modus** A nicht modalen Dialogfeld ermöglicht Benutzern, die Einstellungen im Dialogfeld "" zu ändern, während Sie weiterhin mit dem Dokumentfenster interagieren.

### <a name="modal-window"></a>Modales Fenster

Ein Standard, der `NSWindow` kann als ein benutzerdefiniertes Dialogfeld verwendet werden, indem er modal angezeigt:

[![](dialog-images/modal01.png "Ein Beispiel modale Fenster")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Dokument modales Dialogfeld Blätter

Ein _Blatt_ ist ein modales Dialogfeld an, die angefügt wird, zu einem bestimmten Dokumentfenster verhindern, dass Benutzer von der Interaktion mit dem Fenster, bis sie das Dialogfeld schließen. Ein Blatt wird an das Fenster angefügt, von dem es entsteht, und nur jeweils ein Blatt gleichzeitig für ein Fenster geöffnet werden kann.

[![](dialog-images/sheet08.png "Ein Beispiel für modale Arbeitsblatt")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Einstellungen für Windows

Ein Fenster Voreinstellungen ist ein nicht modales Dialogfeld, das die Einstellungen der Anwendung enthält, die der Benutzer nur selten ändert. Einstellungen für Windows sind häufig eine Symbolleiste, die dem Benutzer ermöglicht, zwischen verschiedenen Gruppen von Einstellungen zu wechseln:

[![](dialog-images/dialog02.png "Ein Beispiel-Preference-Fenster")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Dialogfeld "Öffnen"

Das Dialogfeld "Öffnen" weisen den Benutzern eine konsistente Möglichkeit zum Suchen und öffnen ein Element in einer Anwendung:

[![](dialog-images/dialog03.png "Ein Dialogfeld \"Öffnen\"")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Druck- und Seite Setup-Dialogfelder

MacOS bietet standard Druck- und Seite Setup Dialoge, die Ihre Anwendung angezeigt werden kann, damit Benutzer einen konsistenten Druck können auftreten, in jeder Anwendung, die sie verwenden.

Das Druckdialogfeld können als beide frei schwebenden Dialogfeld angezeigt werden:

[![](dialog-images/print01.png "Ein Dialogfeld Drucken")](dialog-images/print01.png#lightbox)

Oder sie können als Blatt angezeigt werden:

[![](dialog-images/print02.png "Ein Blatt drucken")](dialog-images/print02.png#lightbox)

Das Seiteneinrichtungsdialogfeld kann als beide frei schwebenden Dialogfeld angezeigt werden:

[![](dialog-images/print03.png "Eine Seite-Setupdialogfeld")](dialog-images/print03.png#lightbox)

Oder sie können als Blatt angezeigt werden:

[![](dialog-images/print04.png "Eine Seite Setup Blatt")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Speichern Sie die Dialogfenster

Das Dialogfeld "Speichern" ermöglicht Benutzern eine konsistente Weise Element in einer Anwendung zu speichern. Das Dialogfeld "Speichern" verfügt über zwei Zustände: **minimale** (auch bekannt als "reduzierter"):

[![](dialog-images/save01.png "Ein Dialogfeld zum Speichern")](dialog-images/save01.png#lightbox)

Und die **erweitert** Zustand:

[![](dialog-images/save02.png "Einen erweiterten Dialogfeld \"Speichern\"")](dialog-images/save02.png#lightbox)

Die **minimale** Speichern Dialogfeld kann auch als Blatt angezeigt werden:

[![](dialog-images/save03.png "Eine minimale Blatt gespeichert")](dialog-images/save03.png#lightbox)

Wie können die **erweitert** Dialogfeld Speichern:

[![](dialog-images/save04.png "Speichern Sie einen erweiterten Blatt")](dialog-images/save04.png#lightbox)

Weitere Informationen finden Sie unter der [Dialoge](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche-Schnittstelle](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Ein modales Fenster hinzufügen zu einem Projekt

Abgesehen von der main-Dokumentfenster müssen eine Xamarin.Mac-Anwendung möglicherweise andere Arten von Fenstern, die der Benutzer, z. B. Voreinstellungen oder Inspektor-Fenstern angezeigt.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**öffnen die `Main.storyboard` Datei zur Bearbeitung in Xcodes Benutzeroberflächen-Generator.
2. Ziehen Sie ein neues **Modellansichtcontroller** in der Entwurfsoberfläche angezeigt:

    [![](dialog-images/new01.png "Auswählen eines Controllers Ansicht aus der Bibliothek")](dialog-images/new01.png#lightbox)
3. In der **Identität Inspektor**, geben Sie `CustomDialogController` für die **Klassenname**: 

    [![](dialog-images/new02.png "Der Name der Klasse festlegen")](dialog-images/new02.png#lightbox)
4. Wechseln Sie zurück zu Visual Studio für Mac, lassen sie zum Synchronisieren mit Xcode und zum Erstellen der `CustomDialogController.h` Datei.
5. Zurück zu Xcode und Entwerfen Ihrer Schnittstelle: 

    [![](dialog-images/new03.png "Entwerfen der Benutzeroberflächenautomatisierungs in Xcode")](dialog-images/new03.png#lightbox)
6. Erstellen einer **modale Segue** im Hauptfenster der app mit dem neuen Sicht Controller durch Ziehen von Steuerelement aus dem Benutzeroberflächenautomatisierungs-Element, die im Dialogfeld, um das Dialogfeld "-Fenster geöffnet werden. Weisen Sie die **Bezeichner** `ModalSegue`: 

    [![](dialog-images/new06.png "Eine modale segue")](dialog-images/new06.png#lightbox)
6. Über das Netzwerk von einem **Aktionen** und **Steckdosen**: 

    [![](dialog-images/new04.png "Konfigurieren eine Aktion")](dialog-images/new04.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Stellen Sie die `CustomDialogController.cs` Datei aussehen wie folgt:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Dieser Code macht einige Eigenschaften den Titel und die Beschreibung des Dialogs festlegen und einige Ereignisse, zu reagieren, abgebrochen oder akzeptiert wird das Dialogfeld ".

Als Nächstes Bearbeiten der `ViewController.cs` Datei außer Kraft, indem die `PrepareForSegue` Methode, und stellen sie wie folgt aussehen:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Dieser Code initialisiert das Segue, das wir in Xcodes Benutzeroberflächen-Generator, um das Dialogfeld definiert und richtet den Titel und Beschreibung. Er verarbeitet auch die Auswahl, die der Benutzer im Dialogfeld vornimmt.

Wir können unsere Anwendung auszuführen und das benutzerdefinierte Dialogfeld angezeigt:

[![](dialog-images/new05.png "Ein Beispiel für das Dialogfeld")](dialog-images/new05.png#lightbox)

Weitere Informationen zur Verwendung von Windows in einer Anwendung Xamarin.Mac finden Sie unter unsere [arbeiten mit Fenstern](~/mac/user-interface/window.md) Dokumentation.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Erstellen ein benutzerdefiniertes Stylesheet

Ein _Blatt_ ist ein modales Dialogfeld an, die angefügt wird, zu einem bestimmten Dokumentfenster verhindern, dass Benutzer von der Interaktion mit dem Fenster, bis sie das Dialogfeld schließen. Ein Blatt wird an das Fenster angefügt, von dem es entsteht, und nur jeweils ein Blatt gleichzeitig für ein Fenster geöffnet werden kann. 

Führen Sie zum Erstellen einer benutzerdefinierten Blatt in Xamarin.Mac wir Folgendes:

1. In der **Projektmappen-Explorer**öffnen die `Main.storyboard` Datei zur Bearbeitung in Xcodes Benutzeroberflächen-Generator.
2. Ziehen Sie ein neues **Modellansichtcontroller** in der Entwurfsoberfläche angezeigt:

    [![](dialog-images/new01.png "Auswählen eines Controllers Ansicht aus der Bibliothek")](dialog-images/new01.png#lightbox)
2. Entwerfen der Benutzeroberfläche:

    [![](dialog-images/sheet01.png "Das Design der Benutzeroberfläche")](dialog-images/sheet01.png#lightbox)
3. Erstellen einer **Blatt Segue** aus Ihrer Fenster "Main" mit dem neuen View-Controller: 

    [![](dialog-images/sheet02.png "Der Typ des Blatts Segue auswählen")](dialog-images/sheet02.png#lightbox)
4. In der **Identität Inspektor**, benennen Sie die View-Controller **Klasse** `SheetViewController`: 

    [![](dialog-images/sheet03.png "Der Name der Klasse festlegen")](dialog-images/sheet03.png#lightbox)
5. Definieren Sie alle erforderlichen **Steckdosen** und **Aktionen**: 

    [![](dialog-images/sheet04.png "Definieren die erforderlichen Ausgänge und Aktionen")](dialog-images/sheet04.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac synchronisiert werden.

Als Nächstes Bearbeiten der `SheetViewController.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Als Nächstes Bearbeiten der `ViewController.cs` Datei, bearbeiten Sie die `PrepareForSegue` Methode, und stellen sie wie folgt aussehen:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Wenn wir unsere Anwendung ausführen, und öffnen Sie das Blatt, wird es in das Fenster zugeordnet werden:

[![](dialog-images/sheet08.png "Ein Beispiel-Arbeitsblatt")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Erstellen ein Dialogfeld Voreinstellungen

Bevor wir die Preference-Ansicht im Benutzeroberflächen-Generator erstellen, müssen wir zum Hinzufügen eines benutzerdefinierten Segue-Typs zum Auslagern aus der Einstellungen behandeln. Fügen Sie dem Projekt eine neue Klasse, und rufen sie `ReplaceViewSeque `. Bearbeiten Sie die Klasse, und stellen sie wie folgt aussehen:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

Mit der benutzerdefinierten Segue erstellt haben können wir ein neues Fenster in Xcodes Benutzeroberflächen-Generator, behandeln unsere Voreinstellungen hinzufügen.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**öffnen die `Main.storyboard` Datei zur Bearbeitung in Xcodes Benutzeroberflächen-Generator.
2. Ziehen Sie ein neues **Fenster Controller** in der Entwurfsoberfläche angezeigt:

    [![](dialog-images/pref01.png "Wählen Sie einen Controller Fenster aus der Bibliothek")](dialog-images/pref01.png#lightbox)
3. Ordnen Sie das Fenster in der Nähe der **Menüleiste** Designer:

    [![](dialog-images/pref02.png "Das neue Fenster hinzufügen")](dialog-images/pref02.png#lightbox)
4. Erstellen Sie Kopien der angefügten View-Controller an, wie in Ihrer Ansicht bevorzugt werden Registerkarten vorhanden sein:

    [![](dialog-images/pref03.png "Die erforderliche View-Controller hinzufügen")](dialog-images/pref03.png#lightbox)
5. Ziehen Sie ein neues **Symbolleiste Controller** aus der **Bibliothek**:

    [![](dialog-images/pref04.png "Wählen Sie einen Controller Symbolleiste aus der Bibliothek")](dialog-images/pref04.png#lightbox)
6. Ein, und legen Sie sie auf das Fenster in der Entwurfsoberfläche angezeigt:

    [![](dialog-images/pref05.png "Hinzufügen eines neuen Symbolleiste-Controllers")](dialog-images/pref05.png#lightbox)
7. Layout der Entwurf der Symbolleiste angezeigt wird:

    [![](dialog-images/pref06.png "Layout der Symbolleiste")](dialog-images/pref06.png#lightbox)
8. Steuerelement klicken und ziehen Sie aus allen **Symbolleisten-Schaltfläche** auf die Ansichten, die Sie soeben erstellt haben. Wählen Sie eine **benutzerdefinierte** segue Typ:

    [![](dialog-images/pref07.png "Festlegen der Segue-Typs")](dialog-images/pref07.png#lightbox)
9. Wählen Sie die neue Segue und legen Sie die **Klasse** auf `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Festlegen der Segue-Klasse")](dialog-images/pref08.png#lightbox)
10. In der **Menubar-Designer** wählen Sie auf der Entwurfsoberfläche angezeigt, aus dem Anwendungsmenü **Einstellungen...** , Steuerelement klicken und ziehen Sie in den Voreinstellungen Fenster zum Erstellen einer **anzeigen** segue:

    [![](dialog-images/pref09.png "Festlegen der Segue-Typs")](dialog-images/pref09.png#lightbox)
11. Die Änderungen zu speichern und zurück zu Visual Studio für Mac synchronisiert werden.

Wenn wir führen Sie den Code, und wählen Sie die **Einstellungen...**  aus der **Anwendungsmenü**, das Fenster wird angezeigt:

[![](dialog-images/pref10.png "Ein Beispiel-Voreinstellungen-Fenster")](dialog-images/pref10.png#lightbox)

Weitere Informationen zum Arbeiten mit Fenster und Symbolleisten finden Sie unter unsere [Windows](~/mac/user-interface/window.md) und [Symbolleisten](~/mac/user-interface/toolbar.md) Dokumentation.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Speichern und Laden von Einstellungen

Wenn der Benutzer eine der Benutzervoreinstellungen für der App, ändert, werden diese Änderungen in einer typischen MacOS App automatisch gespeichert. Die einfachste Möglichkeit in einer app Xamarin.Mac Behandlung dieses Vorgangs ist die Erstellung eine einzelne Klasse zum Verwalten aller Einstellungen des Benutzers, und teilen Sie diese systemweite.

Fügen Sie zunächst ein neues `AppPreferences` -Klasse auf das Projekt und Vererben `NSObject`. Die Voreinstellungen werden entworfen werden, verwenden [zum Binden von Daten und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) stellen die des Prozess zum Erstellen und verwalten die Voreinstellung forms viel einfacher. Da die Einstellungen für eine kleine Anzahl von einfachen Datentypen enthalten soll, verwenden Sie das integrierte `NSUserDefaults` zum Speichern und Abrufen von Werten.

Bearbeiten der `AppPreferences.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Diese Klasse enthält einige Hilfsroutinen, z. B. `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, usw., zum Arbeiten mit `NSUserDefaults` einfacher. Darüber hinaus seit `NSUserDefaults` verfügt nicht über eine integrierte Option zum Behandeln `NSColors`, die `NSColorToHexString` und `NSColorFromHexString` Methoden werden verwendet, um die Farben in webbasierten hexadezimale Zeichenfolgen zu konvertieren (`#RRGGBBAA` , in dem `AA` alpha Transparenz wird), werden kann einfach gespeichert und abgerufen.

In der `AppDelegate.cs` Datei, erstellen Sie eine Instanz von der **AppPreferences** -Objekt, das app-Wide verwendet wird:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Verkabelung Voreinstellungen Preference-Ansichten

Verbinden Sie anschließend die Preference-Klasse, Elemente der Benutzeroberfläche für die Preference-Fenster und Ansichten, die oben erstellte. Schnittstelle-Generator wählen Sie eine Einstellung-View-Controller, und wechseln Sie zu der **Identität Inspektor**, erstellen Sie eine benutzerdefinierte Klasse für den Controller: 

[![](dialog-images/prefs12.png "Der Identity-Inspektor")](dialog-images/prefs12.png#lightbox)

Wechseln Sie zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren, und öffnen Sie die neu erstellte Klasse für die Bearbeitung. Legen Sie die Klasse, die wie folgt aussehen:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Beachten Sie, dass diese Klasse zwei Dinge durchgeführt hat: Erstens wird eine Hilfsprogramm `App` Eigenschaft, um den Zugriff auf die **AppDelegate** einfacher. Zweitens stellt die `Preferences` Eigenschaft macht die globale **AppPreferences** -Klasse für Datenbindung mit UI-Steuerelementen für diese Sicht platziert.

Als Nächstes Doppelklicken klicken Sie auf die Storyboard-Datei, um öffnen Sie es erneut im Benutzeroberflächen-Generator (und finden Sie unter nur Änderungen oben). Ziehen Sie alle UI-Steuerelemente erforderlich, um die Voreinstellungen-Schnittstelle in die Sicht zu erstellen. Für jedes Steuerelement, wechseln Sie zu der **Inspektor binden** und Binden an die einzelnen Eigenschaften der **AppPreference** Klasse:

[![](dialog-images/prefs13.png "Der Inspektor Bindung")](dialog-images/prefs13.png#lightbox)

Wiederholen Sie die oben genannten Schritte für alle von Bereichen (View Controller) und Eigenschaften der Einstellung erforderlich.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Anwenden der Einstellung ändert sich in allen geöffneten Fenstern

Wie oben erwähnt, möglicherweise in einer typischen MacOS App, wenn der Benutzer eine der Benutzervoreinstellungen für der App, diese Änderungen ändert automatisch gespeichert und auf allen Windows-angewendet wurden der Benutzer in der Anwendung geöffnet haben.

Eine sorgfältige Planung und Entwurf Ihrer app Voreinstellungen und Windows lässt dieser Prozess reibungslos und transparent für den Endbenutzer, mit einer minimalen Menge an Arbeit Codierung durchgeführt werden soll.

Für ein Fenster, das App-Einstellungen genutzt wird, fügen Sie die folgende Helper-Eigenschaft, um seine Inhalte View-Controller zum Zugriff auf unsere **AppDelegate** einfacher:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Fügen Sie eine Klasse, um den Inhalt oder das Verhalten, die basierend auf die Einstellungen des Benutzers zu konfigurieren:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Sie müssen die Konfigurationsmethode aufrufen, wenn das Fenster erstmalig geöffnet wird, um sicherzustellen, dass es die Einstellungen des Benutzers entspricht:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Als Nächstes Bearbeiten der `AppDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu, wenden jede Einstellung ändert sich in allen geöffneten Fenstern hinzu:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

Als Nächstes fügen Sie eine `PreferenceWindowDelegate` -Klasse auf das Projekt, und stellen sie wie folgt aussehen:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

Dadurch wird bevorzugt Änderungen an alle geöffneten Fenster gesendet werden, wenn die Voreinstellung für Fenster geschlossen wird.

Schließlich bearbeiten Sie die Voreinstellung Fenster Controller aus, und fügen Sie den oben erstellten Delegaten hinzu:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Mit diesen Änderungen vorhanden Wenn der Benutzer die App-Einstellungen bearbeitet und das Fenster Einstellung schließt werden die Änderungen auf alle geöffneten Fenster angewendet werden:

[![](dialog-images/prefs14.png "Ein Beispiel-Voreinstellungen-Fenster")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Das Dialogfeld "Öffnen"

Das Dialogfeld "Öffnen" bietet Benutzern eine einheitliche Methode zum Suchen und öffnen ein Element in einer Anwendung. Um ein Dialogfeld "Öffnen" in einer Anwendung Xamarin.Mac anzuzeigen, verwenden Sie den folgenden Code ein:

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

Im obigen Code öffnen wir einem neuen Dokumentfenster angezeigt, um den Inhalt der Datei anzuzeigen. Sie müssen diese ersetzen Code mit der Funktionalität von der Anwendung erforderlich ist.

Die folgenden Eigenschaften stehen bei der Arbeit mit einem `NSOpenPanel`:

- **CanChooseFiles** – Wenn `true` der Benutzer die Dateien auswählen kann.
- **CanChooseDirectories** – Wenn `true` der Benutzer kann die Verzeichnisse auswählen.
- **AllowsMultipleSelection** – Wenn `true` der Benutzer kann mehrere Dateien gleichzeitig auswählen.
- **ResolveAliases** – Wenn `true` auswählen und Alias löst diesen Pfad der ursprünglichen Datei.
- **AllowedFileTypes** -ist ein Zeichenfolgenarray der Dateitypen, die der Benutzer als entweder Erweiterung auswählen kann oder _UTI_. Der Standardwert ist `null`, womit alle Dateien geöffnet werden.

Die `RunModal ()` -Methode zeigt das Dialogfeld "Öffnen" und ermöglicht dem Benutzer das Auswählen von Dateien oder Verzeichnisse (wie durch die Eigenschaften angegeben) und gibt `1` , wenn der Benutzer klickt auf die **öffnen** Schaltfläche.

Das Dialogfeld "Öffnen" gibt die ausgewählten Dateien oder Verzeichnisse des Benutzers als Array von URLs in der `URL` Eigenschaft.

Wenn wir das Programm auszuführen, und wählen Sie die **öffnen...**  Element aus der **Datei** folgende Menü wird angezeigt: 

[![](dialog-images/dialog03.png "Ein geöffnetes Dialogfenster")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Die Druck- und die Seite Setup-Dialogfelder

MacOS bietet standard Druck- und Seite Setup Dialoge, die Ihre Anwendung angezeigt werden kann, damit Benutzer einen konsistenten Druck können auftreten, in jeder Anwendung, die sie verwenden.

Der folgende Code zeigt die Standarddialog drucken:

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Wenn wir setzen die `ShowPrintAsSheet` Eigenschaft `false`, führen Sie die Anwendung und das Dialogfeld "Drucken" anzuzeigen, wird Folgendes angezeigt:

[![](dialog-images/print01.png "Ein Dialogfeld Drucken")](dialog-images/print01.png#lightbox)

Wenn legen Sie die `ShowPrintAsSheet` Eigenschaft `true`, führen Sie die Anwendung und das Dialogfeld "Drucken" anzuzeigen, wird Folgendes angezeigt:

[![](dialog-images/print02.png "Ein Blatt drucken")](dialog-images/print02.png#lightbox)

Der folgende Code zeigt das Dialogfeld "Layout Seite":

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Wenn wir setzen die `ShowPrintAsSheet` Eigenschaft `false`, führen Sie die Anwendung und das Dialogfeld "Seitenlayout" anzeigen, wird Folgendes angezeigt:

[![](dialog-images/print03.png "Eine Seite-Setupdialogfeld")](dialog-images/print03.png#lightbox)

Wenn legen Sie die `ShowPrintAsSheet` Eigenschaft `true`, führen Sie die Anwendung und das Dialogfeld "Seitenlayout" anzeigen, wird Folgendes angezeigt:

[![](dialog-images/print04.png "Eine Seite Setup Blatt")](dialog-images/print04.png#lightbox)

Weitere Informationen zum Arbeiten mit die Druck- und die Seite Setup Dialogfelder finden Sie unter der Apple- [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) und [Einführung in das Drucken](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) (Dokumentation).

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Der Speichervorgang Dialogfeld

Das Dialogfeld "Speichern" ermöglicht Benutzern eine konsistente Weise Element in einer Anwendung zu speichern.

Der folgende Code zeigt ein Standarddialog speichern:

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

Die `AllowedFileTypes` Eigenschaft ist ein Zeichenfolgenarray der Dateitypen, die der Benutzer auswählen können, um die Datei zu speichern. Der Dateityp kann entweder als eine Erweiterung angegeben werden oder _UTI_. Der Standardwert ist `null`, womit auf einen beliebigen Dateityp verwendet werden.

Wenn wir setzen die `ShowSaveAsSheet` Eigenschaft `false`, führen Sie die Anwendung, und wählen Sie **speichern unter...**  aus der **Datei** Menü wird Folgendes angezeigt:

[![](dialog-images/save01.png "Ein speichern (Dialogfeld)")](dialog-images/save01.png#lightbox)

Der Benutzer kann das Dialogfeld erweitern:

[![](dialog-images/save02.png "Einen erweiterten speichern (Dialogfeld)")](dialog-images/save02.png#lightbox)

Wenn wir setzen die `ShowSaveAsSheet` Eigenschaft `true`, führen Sie die Anwendung, und wählen Sie **speichern unter...**  aus der **Datei** Menü wird Folgendes angezeigt:

[![](dialog-images/save03.png "Ein Blatt gespeichert")](dialog-images/save03.png#lightbox)

Der Benutzer kann das Dialogfeld erweitern:

[![](dialog-images/save04.png "Speichern Sie einen erweiterten Blatt")](dialog-images/save04.png#lightbox)

Weitere Informationen zum Arbeiten mit das Dialogfeld "Speichern", finden Sie in der Apple- [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit modale Fenster, Blätter und Standardbetriebssystem Dialogfelder in einer Anwendung Xamarin.Mac übernommen. Wir haben gesehen, die verschiedene Typen und Verwendungen von modalen Fenstern, Blätter und Dialogfelder, wie zum Erstellen und Verwalten von modalen Fenstern und Blätter in Xcode Schnittstelle-Generator und zum Arbeiten mit modale Fenster des möchten, blättern und Dialogfelder in C#-Code.

## <a name="related-links"></a>Verwandte Links

- [MacWindows (Beispiel)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Symbolleisten](~/mac/user-interface/toolbar.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Einführung in die Blätter](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Einführung in das Drucken](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
