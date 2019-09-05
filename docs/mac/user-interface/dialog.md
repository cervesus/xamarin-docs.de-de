---
title: Dialogfelder in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit Dialogfeldern und modalen Fenstern in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen von modalen Fenstern in Xcode und Interface Builder, das Arbeiten mit Standard Dialogfeldern und C# das interagieren mit diesen Steuerelementen im Code.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 723f751b93af304c7d70fb1250dbda2f0220e77e
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292217"
---
# <a name="dialogs-in-xamarinmac"></a>Dialogfelder in xamarin. Mac

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf dieselben Dialoge und modale Fenster, die ein Entwickler in " *Ziel-C* " und " *Xcode* " verwendet. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um Ihre modalen Fenster zu erstellen und zu verwalten ( C# oder Sie optional direkt im Code zu erstellen).

Ein Dialogfeld wird als Reaktion auf eine Benutzeraktion angezeigt und bietet in der Regel Möglichkeiten, wie Benutzer die Aktion durchführen können. Ein Dialogfeld erfordert eine Antwort vom Benutzer, bevor es geschlossen werden kann.

Windows kann in einem nicht modalen Zustand (z. b. in einem Text-Editor, in dem mehrere Dokumente gleichzeitig geöffnet werden können) oder Modal verwendet werden (z. b. ein Export Dialogfeld, das vor dem Fortsetzen der Anwendung verworfen werden muss)

[![](dialog-images/dialog03.png "Dialogfeld \"Öffnen\"")](dialog-images/dialog03.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Dialogfeldern und modalen Fenstern in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-C](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die-und `Register` `Export` -Befehle erläutert, die zum Verknüpfen der C# Klassen mit verwendet werden. Ziel-C-Objekte und UI-Elemente.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Einführung in Dialogfelder

Ein Dialogfeld wird als Reaktion auf eine Benutzeraktion (z. b. das Speichern einer Datei) angezeigt und bietet Benutzern die Möglichkeit, diese Aktion abzuschließen. Ein Dialogfeld erfordert eine Antwort vom Benutzer, bevor es geschlossen werden kann.

Gemäß Apple gibt es drei Möglichkeiten, einen Dialog anzuzeigen:

- **Modales Dokument** : in einem modalen Dialogfeld wird verhindert, dass der Benutzer innerhalb eines bestimmten Dokuments nichts anderes tun kann, bis er verworfen wird.
- **Modaler App** : ein modales App-Dialogfeld verhindert, dass der Benutzer mit der Anwendung interagiert, bis er verworfen wird.
- Ohne **Modell** Ein nicht modalem Dialogfeld ermöglicht es Benutzern, die Einstellungen im Dialog Feld zu ändern, während Sie weiterhin mit dem Dokument Fenster interagieren.

### <a name="modal-window"></a>Modales Fenster

Jeder Standard `NSWindow` kann als angepasstes Dialogfeld verwendet werden, indem er modal angezeigt wird:

[![](dialog-images/modal01.png "Ein modales Beispiel Fenster")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Modale Dialog Blätter für Dokumente

Ein _Blatt_ ist ein modales Dialogfeld, das an ein bestimmtes Dokument Fenster angefügt wird. Dadurch wird verhindert, dass Benutzer mit dem Fenster interagieren, bis Sie das Dialogfeld schließen. Ein Blatt wird an das Fenster angefügt, aus dem es hervorgeht, und es kann jeweils nur ein Blatt für ein Fenster geöffnet werden.

[![](dialog-images/sheet08.png "Ein Beispiel eines modalen Blatts")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Fenster "Einstellungen"

Ein Einstellungsfenster ist ein nicht modalem Dialogfeld, das die Einstellungen der Anwendung enthält, die der Benutzer nur selten ändert. In den Einstellungs Fenstern ist häufig eine Symbolleiste enthalten, die es dem Benutzer ermöglicht, zwischen verschiedenen Gruppen von Einstellungen zu wechseln:

[![](dialog-images/dialog02.png "Ein Beispiel für ein Einstellungsfenster")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Dialog Feld Öffnen

Das Dialog Feld Öffnen bietet Benutzern eine konsistente Möglichkeit, ein Element in einer Anwendung zu suchen und zu öffnen:

[![](dialog-images/dialog03.png "Dialogfeld \"Öffnen\"")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Dialogfelder zum Drucken und Einrichten der Seite

macOS bietet Standard Dialogfelder für die Druck-und Seiten Einrichtung, die von Ihrer Anwendung angezeigt werden können, damit Benutzer in jeder verwendeten Anwendung über eine konsistente Druckfunktion verfügen können.

Das Dialogfeld Drucken kann als freies Dialogfeld angezeigt werden:

[![](dialog-images/print01.png "Dialogfeld \"Drucken\"")](dialog-images/print01.png#lightbox)

Oder es kann als Blatt angezeigt werden:

[![](dialog-images/print02.png "Ein druckblatt")](dialog-images/print02.png#lightbox)

Das Dialogfeld Seite einrichten kann sowohl als freies Dialogfeld für unverankerte Felder angezeigt werden:

[![](dialog-images/print03.png "Dialogfeld \"Seite einrichten\"")](dialog-images/print03.png#lightbox)

Oder es kann als Blatt angezeigt werden:

[![](dialog-images/print04.png "Ein Blatt für die Seiten Einrichtung")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Dialogfelder speichern

Das Dialog Feld "Speichern" bietet Benutzern eine konsistente Möglichkeit, ein Element in einer Anwendung zu speichern. Das Dialog Feld "Speichern" hat zwei Zustände: **Minimal** (auch als "reduziert" bezeichnet):

[![](dialog-images/save01.png "Ein Save-Dialogfeld")](dialog-images/save01.png#lightbox)

Und **Erweiterter** Status:

[![](dialog-images/save02.png "Ein erweitertes Dialogfeld \"Speichern\"")](dialog-images/save02.png#lightbox)

Das Dialog Feld zum **minimalen** speichern kann auch als Blatt angezeigt werden:

[![](dialog-images/save03.png "Ein minimales Speicher Blatt")](dialog-images/save03.png#lightbox)

Wie kann das **Erweiterte** Dialog Feld "Speichern":

[![](dialog-images/save04.png "Ein erweitertes Speicher Blatt")](dialog-images/save04.png#lightbox)

Weitere Informationen finden Sie im Abschnitt mit den [Dialog](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) Feldern von Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Hinzufügen eines modalen Fensters zu einem Projekt

Abgesehen vom Hauptdokument Fenster muss eine xamarin. Mac-Anwendung möglicherweise andere Windows-Typen für den Benutzer anzeigen, z. b. Einstellungen oder Inspektor-Panels.

Gehen Sie folgendermaßen vor, um ein neues Fenster hinzuzufügen:

1. Öffnen Sie im **Projektmappen-Explorer**die `Main.storyboard` Datei zur Bearbeitung in der Interface Builder von Xcode.
2. Ziehen Sie einen neuen **Ansichts Controller** in den Designoberfläche:

    [![](dialog-images/new01.png "Auswählen eines Ansichts Controllers aus der Bibliothek")](dialog-images/new01.png#lightbox)
3. Geben Sie `CustomDialogController` im **Identitäts Inspektor**als **Klassennamen**ein: 

    [![](dialog-images/new02.png "Festlegen des Klassen namens")](dialog-images/new02.png#lightbox)
4. Wechseln Sie zurück zu Visual Studio für Mac, lassen Sie die Synchronisierung mit Xcode zu `CustomDialogController.h` , und erstellen Sie die Datei.
5. Kehren Sie zu Xcode zurück, und entwerfen Sie Ihre Schnittstelle: 

    [![](dialog-images/new03.png "Entwerfen der Benutzeroberfläche in Xcode")](dialog-images/new03.png#lightbox)
6. Erstellen Sie aus dem Hauptfenster der APP auf den neuen Ansichts Controller eine **modale** Tabelle, indem Sie das Steuerelement aus dem UI-Element ziehen, das das Dialogfeld in das Fenster des Dialog Felds öffnet. Weisen Sie den **Bezeichner** `ModalSegue`zu: 

    [![](dialog-images/new06.png "Ein modaler *")](dialog-images/new06.png#lightbox)
7. Richten Sie alle **Aktionen** und **Outlets**ein: 

    [![](dialog-images/new04.png "Konfigurieren einer Aktion")](dialog-images/new04.png#lightbox)
8. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Erstellen Sie `CustomDialogController.cs` die Datei wie folgt:

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

Dieser Code macht einige Eigenschaften verfügbar, um den Titel und die Beschreibung des Dialog Felds festzulegen, und einige Ereignisse, um darauf zu reagieren, dass das Dialogfeld abgebrochen oder akzeptiert wird.

Bearbeiten Sie als nächstes `ViewController.cs` die Datei, über `PrepareForSegue` schreiben Sie die-Methode, und führen Sie Sie wie folgt aus:

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

Dieser Code initialisiert den segue, den wir in der Interface Builder von Xcode definiert haben, in unserem Dialogfeld und richtet den Titel und die Beschreibung ein. Außerdem wird die Auswahl behandelt, die der Benutzer im Dialogfeld vornimmt.

Wir können die Anwendung ausführen und das benutzerdefinierte Dialogfeld anzeigen:

[![](dialog-images/new05.png "Ein Beispiel Dialogfeld")](dialog-images/new05.png#lightbox)

Weitere Informationen zur Verwendung von Windows in einer xamarin. Mac-Anwendung finden Sie in unserer Dokumentation zum [Arbeiten mit Windows](~/mac/user-interface/window.md) .

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Erstellen eines benutzerdefinierten Blatts

Ein _Blatt_ ist ein modales Dialogfeld, das an ein bestimmtes Dokument Fenster angefügt wird. Dadurch wird verhindert, dass Benutzer mit dem Fenster interagieren, bis Sie das Dialogfeld schließen. Ein Blatt wird an das Fenster angefügt, aus dem es hervorgeht, und es kann jeweils nur ein Blatt für ein Fenster geöffnet werden. 

Gehen Sie folgendermaßen vor, um ein benutzerdefiniertes Blatt in xamarin. Mac zu erstellen:

1. Öffnen Sie im **Projektmappen-Explorer**die `Main.storyboard` Datei zur Bearbeitung in der Interface Builder von Xcode.
2. Ziehen Sie einen neuen **Ansichts Controller** in den Designoberfläche:

    [![](dialog-images/new01.png "Auswählen eines Ansichts Controllers aus der Bibliothek")](dialog-images/new01.png#lightbox)
3. Entwerfen Sie die Benutzeroberfläche:

    [![](dialog-images/sheet01.png "Der Benutzeroberflächen Entwurf")](dialog-images/sheet01.png#lightbox)
4. Erstellen Sie eine **Blatt Tabelle** aus dem Hauptfenster für den neuen Ansichts Controller: 

    [![](dialog-images/sheet02.png "Auswählen des Blatts \"Blatt-Typ\"")](dialog-images/sheet02.png#lightbox)
5. Benennen Sie im **Identitäts Inspektor**die **Klasse** `SheetViewController`des Ansichts Controllers: 

    [![](dialog-images/sheet03.png "Festlegen des Klassen namens")](dialog-images/sheet03.png#lightbox)
6. Definieren Sie alle erforderlichen **Outlets** und **Aktionen**: 

    [![](dialog-images/sheet04.png "Definieren der erforderlichen Outlets und Aktionen")](dialog-images/sheet04.png#lightbox)
7. Speichern Sie die Änderungen, und kehren Sie zur Synchronisierung Visual Studio für Mac zurück.

Bearbeiten Sie anschließend die `SheetViewController.cs` Datei, und führen Sie Sie wie folgt aus:

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

Bearbeiten Sie als nächstes `ViewController.cs` die Datei, bearbeiten `PrepareForSegue` Sie die-Methode, und führen Sie Sie wie folgt aus:

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

Wenn wir die Anwendung ausführen und das Blatt öffnen, wird es an das Fenster angefügt:

[![](dialog-images/sheet08.png "Ein Beispiel Blatt")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Erstellen eines Einstellungs Dialogfelds

Bevor wir die Einstellungs Ansicht in Interface Builder aufstellen, müssen wir einen benutzerdefinierten Typ "*" hinzufügen, um das Auslagern der Einstellungen zu behandeln. Fügen Sie dem Projekt eine neue Klasse hinzu, und `ReplaceViewSeque`nennen Sie Sie. Bearbeiten Sie die-Klasse, und sehen Sie Sie wie folgt aus:

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

Wenn Sie den benutzerdefinierten segue erstellt haben, können Sie ein neues Fenster in der Interface Builder von Xcode hinzufügen, um unsere Einstellungen zu verarbeiten.

Gehen Sie folgendermaßen vor, um ein neues Fenster hinzuzufügen:

1. Öffnen Sie im **Projektmappen-Explorer**die `Main.storyboard` Datei zur Bearbeitung in der Interface Builder von Xcode.
2. Ziehen Sie einen neuen **Fenster Controller** in den Designoberfläche:

    [![](dialog-images/pref01.png "Auswählen eines Fenster Controllers aus der Bibliothek")](dialog-images/pref01.png#lightbox)
3. Ordnen Sie das Fenster in der Nähe des **Menü** leisten-Designers an:

    [![](dialog-images/pref02.png "Hinzufügen des neuen Fensters")](dialog-images/pref02.png#lightbox)
4. Erstellen Sie Kopien des angefügten Ansichts Controllers, da in Ihrer Einstellungs Ansicht Registerkarten angezeigt werden:

    [![](dialog-images/pref03.png "Hinzufügen der erforderlichen Ansichts Controller")](dialog-images/pref03.png#lightbox)
5. Ziehen Sie einen neuen **Symbolleisten Controller** aus der **Bibliothek**:

    [![](dialog-images/pref04.png "Auswählen eines Symbolleisten Controllers aus der Bibliothek")](dialog-images/pref04.png#lightbox)
6. Und legen Sie Sie im Fenster im Designoberfläche ab:

    [![](dialog-images/pref05.png "Hinzufügen eines neuen Symbolleisten Controllers")](dialog-images/pref05.png#lightbox)
7. Layout des Entwurfs der Symbolleiste:

    [![](dialog-images/pref06.png "Layout der Symbolleiste")](dialog-images/pref06.png#lightbox)
8. Klicken und ziehen Sie von jeder Symbolleisten- **Schaltfläche** auf die Ansichten, die Sie oben erstellt haben. Wählen Sie einen **benutzerdefinierten** Typ "Typ" aus:

    [![](dialog-images/pref07.png "Festlegen des Typs \"Typ\"")](dialog-images/pref07.png#lightbox)
9. Wählen Sie den neuen segue aus, und legen `ReplaceViewSegue`Sie die Klasse auf fest:

    [![](dialog-images/pref08.png "Festlegen der Klasse \"Klasse\"")](dialog-images/pref08.png#lightbox)
10. Wählen Sie im **MenuBar-Designer** auf der Designoberfläche im Menü Anwendung die Option **Einstellungen...** aus, und klicken Sie dann auf das Fenster "Einstellungen", um einen **anzeigen** -segue zu erstellen:

    [![](dialog-images/pref09.png "Festlegen des Typs \"Typ\"")](dialog-images/pref09.png#lightbox)
11. Speichern Sie die Änderungen, und kehren Sie zur Synchronisierung Visual Studio für Mac zurück.

Wenn wir den Code ausführen und die **Einstellungen** im **Anwendungsmenü**auswählen, wird das Fenster angezeigt:

[![](dialog-images/pref10.png "Ein Beispiel für ein Einstellungsfenster")](dialog-images/pref10.png#lightbox)

Weitere Informationen zum Arbeiten mit Fenstern und Symbolleisten finden Sie in unserer [Windows](~/mac/user-interface/window.md) -und [Toolbars](~/mac/user-interface/toolbar.md) -Dokumentation.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Speichern und Laden von Einstellungen

Wenn der Benutzer in einer typischen macOS-App Änderungen an den Benutzereinstellungen der APP vornimmt, werden diese Änderungen automatisch gespeichert. Die einfachste Möglichkeit, dies in einer xamarin. Mac-app zu behandeln, besteht darin, eine einzelne Klasse zu erstellen, um alle Benutzereinstellungen zu verwalten und Sie systemweit freizugeben.

Fügen Sie zunächst dem Projekt `AppPreferences` eine neue Klasse hinzu, und Erben `NSObject`Sie von. Die Einstellungen werden für die Verwendung der [Datenbindung und der Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) entworfen, die das Erstellen und Verwalten von Einstellungs Formularen erheblich vereinfachen. Da die Einstellungen aus einer kleinen Menge an einfachen Datentypen bestehen, verwenden `NSUserDefaults` Sie das integrierte, um Werte zu speichern und abzurufen.

Bearbeiten Sie `AppPreferences.cs` die Datei, und führen Sie Sie wie folgt aus:

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
        [Export("DefaultLanguage")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLanguage", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLanguage");
                SaveInt ("DefaultLanguage", value, true);
                DidChangeValue ("DefaultLanguage");
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

Diese Klasse enthält einige Hilfsroutinen `SaveInt`, wie z. b., `LoadInt`, `SaveColor`, `LoadColor`usw., um `NSUserDefaults` die Arbeit mit zu vereinfachen. `NSUserDefaults` Da darüber hinaus nicht über eine integrierte Methode zum verarbeiten `NSColorFromHexString` `NSColors`von verfügt, werden die `NSColorToHexString` -Methode und die-Methode verwendet, um Farben in webbasierte hexadezimal Zeichenfolgen (`#RRGGBBAA` wobei `AA` die Alpha Transparenz ist) zu konvertieren, die leicht gespeichert und abgerufen.

Erstellen Sie `AppDelegate.cs` in der Datei eine Instanz des **apppreferences** -Objekts, das App-weit verwendet wird:

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

### <a name="wiring-preferences-to-preference-views"></a>Verdrahtung von Voreinstellungen in Einstellungs Ansichten

Verbinden Sie als nächstes die Preference-Klasse mit den Benutzeroberflächen Elementen im Einstellungsfenster und den oben erstellten Ansichten. Wählen Sie in Interface Builder einen Ansichts Controller aus, und wechseln Sie zum **Identitäts Inspektor**, und erstellen Sie eine benutzerdefinierte Klasse für den Controller: 

[![](dialog-images/prefs12.png "Der Identitäts Inspektor")](dialog-images/prefs12.png#lightbox)

Wechseln Sie zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren und die neu erstellte Klasse zum Bearbeiten zu öffnen. Legen Sie die Klasse wie folgt an:

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

Beachten Sie, dass diese Klasse zwei Dinge durchgeführt hat: Erstens gibt es eine `App` Hilfseigenschaft, um den Zugriff auf den **appdelegaten** zu vereinfachen. Zweitens macht die `Preferences` -Eigenschaft die globale **apppreferences** -Klasse für die Datenbindung mit allen in dieser Ansicht platzierten UI-Steuerelementen verfügbar.

Doppelklicken Sie dann auf die Storyboard-Datei, um Sie erneut in Interface Builder zu öffnen (und sehen Sie sich die oben beschriebenen Änderungen an). Ziehen Sie alle UI-Steuerelemente, die zum Erstellen der Einstellungs Schnittstelle in die Ansicht erforderlich sind. Wechseln Sie für jedes Steuerelement zum **Bindungs Inspektor** , und binden Sie an die einzelnen Eigenschaften der **apppreference** -Klasse:

[![](dialog-images/prefs13.png "Der Bindungs Inspektor")](dialog-images/prefs13.png#lightbox)

Wiederholen Sie die obigen Schritte für alle Panels (Ansichts Controller) und bevorzugte Eigenschaften.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Anwenden von Voreinstellungs Änderungen auf alle geöffneten Fenster

Wie oben erwähnt, werden die Änderungen in einer typischen macOS-APP, wenn der Benutzer Änderungen an den Benutzereinstellungen der APP vornimmt, automatisch gespeichert und auf alle Windows-Benutzer angewendet, die der Benutzer möglicherweise in der Anwendung geöffnet hat.

Die sorgfältige Planung und der Entwurf der Einstellungen Ihrer APP ermöglicht die reibungslose und transparente Ausführung dieses Prozesses für den Endbenutzer mit minimalem Codierungsaufwand.

Fügen Sie für jedes Fenster, das die App-Einstellungen verbraucht, dem Content View Controller die folgende Hilfseigenschaft hinzu, um den Zugriff auf den **appdelegaten** zu vereinfachen:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Fügen Sie als nächstes eine Klasse hinzu, um den Inhalt oder das Verhalten auf der Grundlage der Benutzereinstellungen zu konfigurieren:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Wenn das Fenster zum ersten Mal geöffnet wird, müssen Sie die Konfigurations Methode aufzurufen, um sicherzustellen, dass Sie den Einstellungen des Benutzers entspricht:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Bearbeiten Sie anschließend die `AppDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu, um alle vorgenommenen Änderungen auf alle geöffneten Fenster anzuwenden:

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

Fügen Sie dem Projekt `PreferenceWindowDelegate` eine Klasse hinzu, und machen Sie Sie wie folgt:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWindowDelegate : NSWindowDelegate
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
        public PreferenceWindowDelegate (NSWindow window)
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

Dadurch werden alle Voreinstellungs Änderungen an alle geöffneten Fenster gesendet, wenn das Einstellungsfenster geschlossen wird.

Bearbeiten Sie schließlich den Preference Window-Controller, und fügen Sie den oben erstellten Delegaten hinzu:

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
            Window.Delegate = new PreferenceWindowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Wenn alle diese Änderungen vorhanden sind und der Benutzer die Einstellungen der APP bearbeitet und das Einstellungsfenster schließt, werden die Änderungen auf alle geöffneten Fenster angewendet:

[![](dialog-images/prefs14.png "Ein Beispiel für ein Einstellungsfenster")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Dialog Feld "Öffnen"

Das Dialog Feld Öffnen bietet Benutzern eine konsistente Möglichkeit, ein Element in einer Anwendung zu suchen und zu öffnen. Verwenden Sie den folgenden Code, um ein Dialog Feld Öffnen in einer xamarin. Mac-Anwendung anzuzeigen:

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

Im obigen Code wird ein neues Dokument Fenster geöffnet, in dem der Inhalt der Datei angezeigt wird. Sie müssen diesen Code durch die Funktionalität ersetzen, die für Ihre Anwendung erforderlich ist.

Die folgenden Eigenschaften sind beim Arbeiten mit einem `NSOpenPanel`verfügbar:

- **Canchoosefiles** : gibt `true` an, ob der Benutzer Dateien auswählen kann.
- **Canchoogdirectories** : Wenn `true` der Benutzerverzeichnisse auswählen kann.
- **Allowsmultipleselection** : gibt `true` an, ob der Benutzer mehrere Dateien gleichzeitig auswählen kann.
- **Resolvealiases** : Wenn `true` Sie und Alias auswählen, wird Sie in den Pfad der ursprünglichen Datei aufgelöst.
- "" Ist ein Zeichen folgen Array von **Dateitypen,** die der Benutzer als Erweiterung oder " _UTI_" auswählen kann. Der Standardwert ist `null`, wodurch eine beliebige Datei geöffnet werden kann.

Die `RunModal ()` -Methode zeigt das Dialog Feld Öffnen an und ermöglicht dem Benutzer die Auswahl von Dateien oder Verzeichnissen (wie von den Eigenschaften `1` angegeben) und gibt zurück, wenn der Benutzer auf die Schaltfläche **Öffnen** klickt.

Im Dialog Feld Öffnen werden die ausgewählten Dateien oder Verzeichnisse des Benutzers als Array von URLs in der `URL` -Eigenschaft zurückgegeben.

Wenn Sie das Programm ausführen und im Menü **Datei** das Element **Öffnen** auswählen, wird Folgendes angezeigt: 

[![](dialog-images/dialog03.png "Dialogfeld \"Öffnen\"")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Dialogfelder zum Drucken und Seite einrichten

macOS bietet Standard Dialogfelder für die Druck-und Seiten Einrichtung, die von Ihrer Anwendung angezeigt werden können, damit Benutzer in jeder verwendeten Anwendung über eine konsistente Druckfunktion verfügen können.

Der folgende Code zeigt das Standard Dialogfeld "Drucken":

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

Wenn Sie die `ShowPrintAsSheet` -Eigenschaft auf `false`festlegen, die Anwendung ausführen und das Dialogfeld Drucken anzeigen, wird Folgendes angezeigt:

[![](dialog-images/print01.png "Dialogfeld \"Drucken\"")](dialog-images/print01.png#lightbox)

Wenn Sie die `ShowPrintAsSheet` -Eigenschaft `true`auf festlegen, die Anwendung ausführen und das Dialogfeld Drucken anzeigen, wird Folgendes angezeigt:

[![](dialog-images/print02.png "Ein druckblatt")](dialog-images/print02.png#lightbox)

Im folgenden Code wird das Dialog Feld "Seiten Layout" angezeigt:

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

Wenn die `ShowPrintAsSheet` -Eigenschaft auf fest `false`gelegt ist, führen Sie die Anwendung aus, und zeigen Sie das Dialogfeld zum Drucken des Layouts an. Folgendes wird angezeigt:

[![](dialog-images/print03.png "Dialogfeld \"Seite einrichten\"")](dialog-images/print03.png#lightbox)

Wenn Sie die `ShowPrintAsSheet` -Eigenschaft `true`auf festlegen, die Anwendung ausführen und das Dialogfeld "Drucklayout" anzeigen, wird Folgendes angezeigt:

[![](dialog-images/print04.png "Ein Blatt für die Seiten Einrichtung")](dialog-images/print04.png#lightbox)

Weitere Informationen zum Arbeiten mit den Dialogfeldern "Drucken" und "Seite einrichten" finden Sie in den Apple [nsprintpanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092)-, [nspagelayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) -und [Introduction to Printing](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) -Dokumentation.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Dialog Feld "Speichern"

Das Dialog Feld "Speichern" bietet Benutzern eine konsistente Möglichkeit, ein Element in einer Anwendung zu speichern.

Im folgenden Code wird das Standard Dialogfeld Speichern angezeigt:

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

Die `AllowedFileTypes` -Eigenschaft ist ein Zeichen folgen Array von Dateitypen, die der Benutzer auswählen kann, um die Datei zu speichern. Der Dateityp kann entweder als eine Erweiterung oder eine " _UTI_" angegeben werden. Der Standardwert ist `null`, wodurch ein beliebiger Dateityp verwendet werden kann.

Wenn Sie die `ShowSaveAsSheet` -Eigenschaft auf `false`festlegen, führen Sie die Anwendung aus, und wählen Sie im Menü **Datei** die Option **Speichern unter... aus.** Folgendes wird angezeigt:

[![](dialog-images/save01.png "Dialogfeld \"Speichern\"")](dialog-images/save01.png#lightbox)

Der Benutzer kann das Dialogfeld erweitern:

[![](dialog-images/save02.png "Ein erweitertes Dialogfeld \"Speichern\"")](dialog-images/save02.png#lightbox)

Wenn Sie die `ShowSaveAsSheet` -Eigenschaft auf `true`festlegen, führen Sie die Anwendung aus, und wählen Sie im Menü **Datei** die Option **Speichern unter... aus.** Folgendes wird angezeigt:

[![](dialog-images/save03.png "Ein Speicher Blatt")](dialog-images/save03.png#lightbox)

Der Benutzer kann das Dialogfeld erweitern:

[![](dialog-images/save04.png "Ein erweitertes Speicher Blatt")](dialog-images/save04.png#lightbox)

Weitere Informationen zum Arbeiten mit dem Dialog Feld "Speichern" finden Sie in der Dokumentation zu [nssavepanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) von Apple.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie mit modalen Fenstern, Blättern und den standardmäßigen System Dialogfeldern in einer xamarin. Mac-Anwendung arbeiten. Wir haben die verschiedenen Typen und Verwendungsmöglichkeiten von modalen Fenstern, Blättern und Dialogfeldern, das Erstellen und Verwalten von modalen Fenstern und Blättern in der Interface Builder von Xcode und das Arbeiten mit modalen Fenstern C# , Blättern und Dialogfeldern im Code gesehen.

## <a name="related-links"></a>Verwandte Links

- [Macwindows (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macwindows)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Symbolleisten](~/mac/user-interface/toolbar.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Einführung in Blätter](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Einführung in den Druck](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
