---
title: Dialogfelder in Xamarin.Mac
description: Dieser Artikel behandelt die Arbeit mit Dialogfeldern und modalen Fenstern in einer Xamarin.Mac-Anwendung. Es wird beschrieben, erstellen modale Fenster in Xcode und Interface Builder, mit Standarddialogfeldern arbeiten und interagieren mit diesen Steuerelementen im C#-Code.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 329984d44318b2204f2f5ee253402eb158c85b9f
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61283154"
---
# <a name="dialogs-in-xamarinmac"></a>Dialogfelder in Xamarin.Mac

Bei der Arbeit mit C# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleichen Dialogfeldern und modalen Windows, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und verwalten Ihre modalen Windows (oder erstellen sie optional direkt in c#-Code).

Ein Dialogfeld angezeigt, die als Reaktion auf eine Benutzeraktion wird und in der Regel enthält, dass sämtlicher Benutzer die Aktion abgeschlossen werden können. Ein Dialogfeld ist eine Antwort des Benutzers erforderlich, bevor sie geschlossen werden kann.

Windows kann sein, in einem nicht modalen Zustand (z. B. in einem Text-Editor, der mehrere Dokumente gleichzeitig geöffnet sein kann) verwendeten "oder" Modal (z. B. ein Dialogfeld "Exportieren", die geschlossen werden muss, bevor die Anwendung fortgesetzt werden kann).

[![](dialog-images/dialog03.png "Ein geöffnetes Dialogfenster")](dialog-images/dialog03.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Dialogfeldern und modalen Windows in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Einführung in Dialogfeldern

Ein Dialogfeld angezeigt wird, als Reaktion auf eine Benutzeraktion (z. B. Speichern einer Datei) und bietet eine Möglichkeit für Benutzer, um diese Aktion abzuschließen. Ein Dialogfeld ist eine Antwort des Benutzers erforderlich, bevor sie geschlossen werden kann.

Es gibt drei Möglichkeiten, um ein Dialogfeld zu präsentieren, gemäß den Apple:

- **Dokumentieren Sie modale** -ein Dokument modales Dialogfeld wird verhindert, dass den Benutzer zusätzlichen Aufwand in ein bestimmtes Dokument an, bis es geschlossen wird.
- **App Ansicht** : eine App modales Dialogfeld wird verhindert, dass den Benutzer der Anwendung interagieren, bis es geschlossen wird.
- **Ohne Modus** ein nicht modalen Dialogfeld ermöglicht Benutzern, die Einstellungen im Dialogfeld zu ändern, während Sie weiterhin das Dokumentfenster interagieren.

### <a name="modal-window"></a>Modales Fenster

Ein Standard `NSWindow` kann als ein angepasstes Dialogfeld verwendet werden, indem sie modal angezeigt:

[![](dialog-images/modal01.png "Ein Beispiel für modale Fenster")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Dokument modales Dialogfeld Tabellen

Ein _Blatt_ ist ein modales Dialogfeld, das angefügt ist, zu einem bestimmten Dokumentfenster verhindert, dass Benutzer die Interaktion mit dem Fenster, bis sie das Dialogfeld zu schließen. Ein Blatt ist für das Fenster angefügt, aus dem hervorgeht, tritt ein, und nur eine Tabelle kann gleichzeitig für ein Fenster geöffnet sein.

[![](dialog-images/sheet08.png "Eine Beispiel-modal-Blatt")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Einstellungen von Windows

Ein Fenster "Einstellungen" ist ein nicht modales Dialogfeld mit den Einstellungen der Anwendung, die der Benutzer nur selten ändert. Einstellungen von Windows enthalten häufig eine Symbolleiste, die dem Benutzer ermöglicht, zwischen verschiedenen Gruppen von Einstellungen zu wechseln:

[![](dialog-images/dialog02.png "Ein Fenster der Beispiel-Einstellung")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Dialogfeld "Öffnen"

Das Dialogfeld "Öffnen" bietet Benutzern eine einheitliche Methode zum Suchen und öffnen Sie ein Element in einer Anwendung:

[![](dialog-images/dialog03.png "Ein Dialogfeld geöffnet")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Druck- und Seiteneinrichtung-Dialogfelder

MacOS bietet Standarddruckvorgänge Seite Setup Dialogfelder, die Ihre Anwendung anzeigen können, damit Benutzer einen konsistenten Druck haben Erfahrung und in jede Anwendung, die sie verwenden.

Das Dialogfeld "Drucken" können als sowohl kostenlose unverankerte Dialogfeld angezeigt werden:

[![](dialog-images/print01.png "Ein Dialogfeld \"Drucken\"-Feld")](dialog-images/print01.png#lightbox)

Oder er kann als Tabelle angezeigt werden:

[![](dialog-images/print02.png "Ein Arbeitsblatt drucken")](dialog-images/print02.png#lightbox)

Das Dialogfeld "Seite einrichten" können als sowohl kostenlose unverankerte Dialogfeld angezeigt werden:

[![](dialog-images/print03.png "Ein Dialogfeld \"Seite einrichten\"")](dialog-images/print03.png#lightbox)

Oder er kann als Tabelle angezeigt werden:

[![](dialog-images/print04.png "Eine Seite-Setup-Tabelle")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Speichern Sie die Dialogfelder

Das Dialogfeld "Speichern" bietet Benutzern eine konsistente Weise Element in einer Anwendung zu speichern. Das Dialogfeld "Speichern" verfügt über zwei Zustände: **Minimale** (auch bekannt als reduzierten):

[![](dialog-images/save01.png "Ein Dialogfeld \"Speichern\"")](dialog-images/save01.png#lightbox)

Und die **erweitert** Zustand:

[![](dialog-images/save02.png "Einen erweiterten Dialogfeld \"Speichern\"")](dialog-images/save02.png#lightbox)

Die **minimale** Speichern Dialogfeld kann auch als Tabelle angezeigt werden:

[![](dialog-images/save03.png "Eine minimale Blatt gespeichert")](dialog-images/save03.png#lightbox)

Wie können die **erweitert** Dialogfeld Speichern:

[![](dialog-images/save04.png "Speichern Sie einen erweiterten Blatt")](dialog-images/save04.png#lightbox)

Weitere Informationen finden Sie unter den [Dialogfelder](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Ein modales Fenster hinzugefügt zu einem Projekt.

Abgesehen von der wichtigsten Dokumentfenster müssen eine Xamarin.Mac-Anwendung möglicherweise andere Typen von Windows für den Benutzer, z. B. Einstellungen oder Inspektor-Fenster angezeigt.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**öffnen die `Main.storyboard` Datei zur Bearbeitung in Interface Builder von Xcode.
2. Ziehen Sie ein neues **Ansichtscontroller** in der Entwurfsoberfläche:

    [![](dialog-images/new01.png "Wenn Sie einen Ansichtscontroller auswählen, aus der Bibliothek")](dialog-images/new01.png#lightbox)
3. In der **Identitätsinspektor**, geben Sie `CustomDialogController` für die **Klassenname**: 

    [![](dialog-images/new02.png "Den Namen der Klasse festlegen")](dialog-images/new02.png#lightbox)
4. Wechseln Sie zurück zu Visual Studio für Mac, zuzulassen, dass sie mit Xcode synchronisieren, und erstellen Sie die `CustomDialogController.h` Datei.
5. Zurück zu Xcode, und Entwerfen Sie Ihre Schnittstelle: 

    [![](dialog-images/new03.png "Entwerfen der Benutzeroberfläche in Xcode")](dialog-images/new03.png#lightbox)
6. Erstellen Sie eine **modale Segue** über das Hauptfenster der app auf die neue View Controller durch Ziehen von Steuerelementen aus dem UI-Element, das im Dialogfeld, um das Dialogfeld "-Fenster geöffnet wird. Weisen Sie die **Bezeichner** `ModalSegue`: 

    [![](dialog-images/new06.png "Eine modale segue")](dialog-images/new06.png#lightbox)
6. Über das Netzwerk von einem **Aktionen** und **Outlets**: 

    [![](dialog-images/new04.png "Konfigurieren eine Aktion")](dialog-images/new04.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

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

Dieser Code macht einige Eigenschaften, die den Titel und die Beschreibung des Dialogfelds festgelegt und einige Ereignisse zu reagieren, um das Dialogfeld abgebrochen oder akzeptiert wird.

Als Nächstes bearbeiten Sie die `ViewController.cs` Datei außer Kraft, indem die `PrepareForSegue` Methode und legen Sie ihn wie folgt aussehen:

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

Dieser Code initialisiert den Segue, den wir in Interface Builder von Xcode auf unser Dialogfeld definiert und richtet den Titel und Beschreibung. Es übernimmt auch die Wahl, die der Benutzer im Dialogfeld vornimmt.

Wir können die Anwendung auszuführen und das benutzerdefinierte Dialogfeld angezeigt:

[![](dialog-images/new05.png "Ein Beispiel für das Dialogfeld")](dialog-images/new05.png#lightbox)

Weitere Informationen zur Verwendung von Windows in einer Xamarin.Mac-Anwendung finden Sie unserem [arbeiten mit Windows](~/mac/user-interface/window.md) Dokumentation.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Erstellen eine benutzerdefinierte Tabelle

Ein _Blatt_ ist ein modales Dialogfeld, das angefügt ist, zu einem bestimmten Dokumentfenster verhindert, dass Benutzer die Interaktion mit dem Fenster, bis sie das Dialogfeld zu schließen. Ein Blatt ist für das Fenster angefügt, aus dem hervorgeht, tritt ein, und nur eine Tabelle kann gleichzeitig für ein Fenster geöffnet sein. 

Führen Sie zum Erstellen einer benutzerdefinierten Tabelle in Xamarin.Mac wir Folgendes:

1. In der **Projektmappen-Explorer**öffnen die `Main.storyboard` Datei zur Bearbeitung in Interface Builder von Xcode.
2. Ziehen Sie ein neues **Ansichtscontroller** in der Entwurfsoberfläche:

    [![](dialog-images/new01.png "Wenn Sie einen Ansichtscontroller auswählen, aus der Bibliothek")](dialog-images/new01.png#lightbox)
2. Entwerfen der Benutzeroberfläche an:

    [![](dialog-images/sheet01.png "Das Design der Benutzeroberfläche")](dialog-images/sheet01.png#lightbox)
3. Erstellen Sie eine **Blatt Segue** aus das Hauptfenster auf die neue View Controller: 

    [![](dialog-images/sheet02.png "Blatt Segue-Typ auswählen")](dialog-images/sheet02.png#lightbox)
4. In der **Identitätsinspektor**, benennen Sie den Ansichtscontroller **Klasse** `SheetViewController`: 

    [![](dialog-images/sheet03.png "Den Namen der Klasse festlegen")](dialog-images/sheet03.png#lightbox)
5. Definieren Sie alle erforderlichen **Outlets** und **Aktionen**: 

    [![](dialog-images/sheet04.png "Definieren die erforderlichen Outlets und Aktionen")](dialog-images/sheet04.png#lightbox)
6. Die Änderungen zu speichern und zurück zu Visual Studio für Mac, um zu synchronisieren.

Als Nächstes bearbeiten Sie die `SheetViewController.cs` Datei, und stellen sie wie folgt aussehen:

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

Als Nächstes bearbeiten Sie die `ViewController.cs` Datei, bearbeiten Sie die `PrepareForSegue` Methode und legen Sie ihn wie folgt aussehen:

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

Wenn wir die Anwendung auszuführen und das Blatt zu öffnen, wird es an das Fenster angefügt werden:

[![](dialog-images/sheet08.png "Eine Beispiel-Blatt")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Erstellen ein Dialogfeld "Einstellungen"

Bevor wir den Entwurf, um die Ansicht "Einstellungen" in Interface Builder müssen wir einen benutzerdefinierten Segue-Typ zum Behandeln der Wechsel von den Einstellungen hinzufügen. Fügen Sie dem Projekt eine neue Klasse und nenne `ReplaceViewSeque `. Bearbeiten Sie die Klasse, und stellen sie wie folgt aussehen:

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

Mit der benutzerdefinierten Segue erstellt haben können wir ein neues Fenster in Interface Builder von Xcode, um unsere Voreinstellungen zu behandeln hinzufügen.

Um ein neues Fenster hinzuzufügen, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**öffnen die `Main.storyboard` Datei zur Bearbeitung in Interface Builder von Xcode.
2. Ziehen Sie ein neues **Fenstercontroller** in der Entwurfsoberfläche:

    [![](dialog-images/pref01.png "Wählen Sie aus der Bibliothek einen Fenstercontroller")](dialog-images/pref01.png#lightbox)
3. Ordnen Sie das Fenster in der Nähe der **Menüleiste** Designer:

    [![](dialog-images/pref02.png "Das neue Fenster hinzufügen")](dialog-images/pref02.png#lightbox)
4. Erstellen Sie Kopien des angefügten View-Controller, wie in der Ansicht Einstellung Registerkarten fallen:

    [![](dialog-images/pref03.png "Die erforderliche View-Controller hinzufügen")](dialog-images/pref03.png#lightbox)
5. Ziehen Sie ein neues **Symbolleiste Controller** aus der **Bibliothek**:

    [![](dialog-images/pref04.png "Wählen Sie einen Controller für die Symbolleiste aus der Bibliothek")](dialog-images/pref04.png#lightbox)
6. Ein, und legen Sie sie im Fenster auf der Entwurfsoberfläche:

    [![](dialog-images/pref05.png "Hinzufügen eines neuen Controllers für die Symbolleiste")](dialog-images/pref05.png#lightbox)
7. Layout der Entwurf Ihrer Symbolleiste:

    [![](dialog-images/pref06.png "Layout der Symbolleiste")](dialog-images/pref06.png#lightbox)
8. Strg + Klick und ziehen Sie aus jeder **Symbolleisten-Schaltfläche** in den Ansichten, die Sie soeben haben erstellt. Wählen Sie eine **benutzerdefinierte** segue-Typ:

    [![](dialog-images/pref07.png "Segue-Typ")](dialog-images/pref07.png#lightbox)
9. Der neue Segue auswählen und Festlegen der **Klasse** zu `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Festlegen der Segue-Klasse")](dialog-images/pref08.png#lightbox)
10. In der **Menüleiste Designer** auf der Entwurfsoberfläche, wählen Sie im Anwendungsmenü **Einstellungen...** , Strg + Klick und ziehen Sie in das Fenster "Einstellungen" zum Erstellen einer **anzeigen** segue:

    [![](dialog-images/pref09.png "Segue-Typ")](dialog-images/pref09.png#lightbox)
11. Die Änderungen zu speichern und zurück zu Visual Studio für Mac, um zu synchronisieren.

Wenn wir den Code auszuführen, und wählen Sie die **Einstellungen...**  aus der **Anwendungsmenü**, das Fenster wird angezeigt:

[![](dialog-images/pref10.png "Eine Beispiel-Fenster \"Einstellungen\"")](dialog-images/pref10.png#lightbox)

Weitere Informationen zum Arbeiten mit Windows und Symbolleisten finden Sie unserem [Windows](~/mac/user-interface/window.md) und [Symbolleisten](~/mac/user-interface/toolbar.md) Dokumentation.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Speichern und Laden von Einstellungen

Wenn der Benutzer eines der App-Benutzereinstellungen, ändert, werden diese Änderungen in einer typischen MacOS-App automatisch gespeichert. Die einfachste Möglichkeit, dies in einer Xamarin.Mac-app zu verarbeiten ist die Erstellung eine einzelne Klasse zum Verwalten aller benutzereinstellungen und teilen Sie sie systemweite.

Fügen Sie zunächst ein neues `AppPreferences` -Klasse auf das Projekt, und erben von `NSObject`. Die Einstellungen werden mit entworfen werden, dass [Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md) die werden den Prozess zum Erstellen und verwalten die Einstellung einfacher bildet. Da die Einstellungen für eine kleine Menge von einfachen Datentypen enthalten soll, verwenden Sie das integrierte `NSUserDefaults` zum Speichern und Abrufen von Werten.

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

Diese Klasse enthält einige Hilfsroutinen, z. B. `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, usw., denen das Arbeiten mit `NSUserDefaults` einfacher. Da außerdem `NSUserDefaults` verfügt nicht über eine integrierte Methode zum Verarbeiten `NSColors`, `NSColorToHexString` und `NSColorFromHexString` Methoden zum Konvertieren von Farben in hexadezimalen Zeichenfolgen webbasierte (`#RRGGBBAA` , in dem `AA` ist der alpha-Transparenz) sein kann, ganz einfach gespeichert und abgerufen.

In der `AppDelegate.cs` Datei, erstellen Sie eine Instanz von der **AppPreferences** -Objekt, das anwendungsweite verwendet wird:

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

### <a name="wiring-preferences-to-preference-views"></a>Verknüpfen Sie die Einstellung für den Preference-Ansichten

Schließen Sie dann die Preference-Klasse für UI-Elemente für die Einstellung im Fenster und Ansichten, die oben erstellt haben. Klicken Sie in Interface Builder, wählen Sie einen Ansichtscontroller für die Einstellung, und wechseln Sie zu der **Identitätsinspektor**, erstellen Sie eine benutzerdefinierte Klasse für den Controller: 

[![](dialog-images/prefs12.png "Der Identitätsinspektor")](dialog-images/prefs12.png#lightbox)

Wechseln Sie zurück zu Visual Studio für Mac zum Synchronisieren von Änderungen, und öffnen Sie die neu erstellte Klasse für die Bearbeitung. Legen Sie die Klasse, die wie folgt aussehen:

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

Beachten Sie, dass diese Klasse zwei Dinge geschehen: Als Erstes ist eine Hilfsprogramm `App` Eigenschaft, um den Zugriff auf die **AppDelegate** einfacher. Zweitens wird die `Preferences` Eigenschaft verfügbar macht, die globale **AppPreferences** -Klasse für die Datenbindung mit UI-Steuerelementen in dieser Ansicht platziert.

Als Nächstes Doppelklicken klicken Sie auf die Storyboard-Datei, um erneut in Interface Builder öffnen (und nur über vorgenommenen Änderungen sehen). Ziehen Sie die UI-Steuerelementen erforderlich, um die Einstellungen-Schnittstelle in die Ansicht zu erstellen. Für jedes Steuerelement, wechseln Sie zu der **Bindung Inspektor** und Binden an die einzelnen Eigenschaften der **AppPreference** Klasse:

[![](dialog-images/prefs13.png "Der Inspektor Bindung")](dialog-images/prefs13.png#lightbox)

Wiederholen Sie die oben genannten Schritte für alle Bereiche (View Controller) und Eigenschaften der Einstellung erforderlich.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Anwenden von Einstellungen auf alle geöffneten Windows ändert

Wie bereits erwähnt, kann in einer typischen MacOS-App, wenn der Benutzer Änderungen an eines der App-Benutzereinstellungen, diese Änderungen vornimmt, werden automatisch gespeichert und auf allen Windows-angewendet der Benutzer in der Anwendung geöffnet haben.

Eine sorgfältige Planung und Entwurf Ihrer app Einstellungen und Windows kann dieser Prozess nahtlos und transparent für den Endbenutzer mit einer minimalen Menge an Arbeit Codierung auftreten.

Für alle Fenster, das App-Einstellungen verwenden, fügen Sie die folgende Hilfsprogramm-Eigenschaft, um die Content View-Controller zu den Zugriff auf unsere **AppDelegate** einfacher:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Als Nächstes fügen Sie eine Klasse aus, um den Inhalt oder das Verhalten basierend auf den Einstellungen des Benutzers konfigurieren:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Sie müssen die Configuration-Methode aufrufen, wenn das Fenster erstmalig geöffnet wird, um sicherzustellen, dass sie die Einstellungen des Benutzers entspricht:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Als Nächstes bearbeiten Sie die `AppDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu, wenden jede Einstellung ändert, um alle geöffneten Fenster hinzu:

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

Fügen Sie als Nächstes eine `PreferenceWindowDelegate` Klasse dem Projekt, und stellen sie wie folgt aussehen:

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

Dadurch werden Änderungen Einstellung an alle geöffneten Windows gesendet werden, wenn die Einstellung Fenster geschlossen wird.

Schließlich bearbeiten Sie die Fenstercontroller Einstellung, und fügen Sie den oben erstellten Delegaten hinzu:

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

Mit der alle diese Änderungen vorgenommen Wenn der Benutzer die App Einstellungen bearbeitet und das Fenster Einstellung schließt werden die Änderungen auf alle geöffneten Windows angewendet werden:

[![](dialog-images/prefs14.png "Eine Beispiel-Fenster \"Einstellungen\"")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Das Dialogfeld "Öffnen"

Das Dialogfeld "Öffnen" bietet Benutzern eine einheitliche Methode zum Suchen und öffnen Sie ein Element in einer Anwendung. Um ein Dialogfeld "Öffnen" in einer Xamarin.Mac-Anwendung anzuzeigen, verwenden Sie den folgenden Code:

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

Im obigen Code öffnen wir einem neuen Dokumentfenster angezeigt, um den Inhalt der Datei anzuzeigen. Sie müssen diese ersetzen Code mit der Funktionalität ist erforderlich, von der Anwendung.

Die folgenden Eigenschaften sind verfügbar, bei der Arbeit mit einem `NSOpenPanel`:

- **CanChooseFiles** – Wenn `true` der Benutzer kann die Dateien auswählen.
- **CanChooseDirectories** – Wenn `true` der Benutzer kann Verzeichnisse auswählen.
- **AllowsMultipleSelection** – Wenn `true` der Benutzer kann mehrere Dateien gleichzeitig auswählen.
- **ResolveAliases** – Wenn `true` auswählen und den Alias, löst Sie diesen in den ursprünglichen Dateipfad.
- **AllowedFileTypes** -ist ein Zeichenfolgenarray der Dateitypen, die der Benutzer, als entweder eine Erweiterung auswählen können oder _UTI_. Der Standardwert ist `null`, wodurch alle Dateien geöffnet werden.

Die `RunModal ()` Methode zeigt das Dialogfeld "Öffnen" und ermöglicht dem Benutzer, wählen Sie Dateien oder Verzeichnisse (wie durch die Eigenschaften angegeben) und gibt `1` klickt der Benutzer die **öffnen** Schaltfläche.

Das Dialogfeld "Öffnen" gibt die ausgewählten Dateien oder Verzeichnisse des Benutzers als ein Array von URLs in die `URL` Eigenschaft.

Wenn wir das Programm auszuführen, und wählen Sie die **öffnen...**  Element aus der **Datei** folgende Menü angezeigt wird: 

[![](dialog-images/dialog03.png "Ein geöffnetes Dialogfenster")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Die Druck- und der Seiteneinrichtung-Dialogfelder

MacOS bietet Standarddruckvorgänge Seite Setup Dialogfelder, die Ihre Anwendung anzeigen können, damit Benutzer einen konsistenten Druck haben Erfahrung und in jede Anwendung, die sie verwenden.

Der folgende Code zeigt das Standarddialogfeld Drucken:

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

Wenn wir Festlegen der `ShowPrintAsSheet` Eigenschaft `false`, führen Sie die Anwendung und zeigt das Dialogfeld "Drucken", wird Folgendes angezeigt:

[![](dialog-images/print01.png "Ein Dialogfeld \"Drucken\"-Feld")](dialog-images/print01.png#lightbox)

Wenn legen Sie die `ShowPrintAsSheet` Eigenschaft `true`, führen Sie die Anwendung und zeigt das Dialogfeld "Drucken", wird Folgendes angezeigt:

[![](dialog-images/print02.png "Ein Arbeitsblatt drucken")](dialog-images/print02.png#lightbox)

Der folgende Code zeigt das Dialogfeld "Seite Layout":

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

Wenn wir Festlegen der `ShowPrintAsSheet` Eigenschaft `false`, führen Sie die Anwendung und zeigt das Dialogfeld "Seitenlayout", wird Folgendes angezeigt:

[![](dialog-images/print03.png "Ein Dialogfeld \"Seite einrichten\"")](dialog-images/print03.png#lightbox)

Wenn legen Sie die `ShowPrintAsSheet` Eigenschaft `true`, führen Sie die Anwendung und zeigt das Dialogfeld "Seitenlayout", wird Folgendes angezeigt:

[![](dialog-images/print04.png "Eine Seite-Setup-Tabelle")](dialog-images/print04.png#lightbox)

Weitere Informationen zum Arbeiten mit die Druck- und die Seite Setup Dialogfelder finden Sie unter Apple [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) und [Einführung in das Drucken](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) Dokumentation.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Der Speichervorgang Dialogfeld

Das Dialogfeld "Speichern" bietet Benutzern eine konsistente Weise Element in einer Anwendung zu speichern.

Der folgende Code wird das standardmäßige Dialogfeld "Speichern" angezeigt:

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

Die `AllowedFileTypes` -Eigenschaft ist ein Zeichenfolgenarray von Dateitypen, die der Benutzer auswählen kann, um die Datei zu speichern. Der Dateityp kann entweder als eine Erweiterung angegeben werden oder _UTI_. Der Standardwert ist `null`, wodurch auf einen beliebigen Dateityp verwendet werden.

Wenn wir legen die `ShowSaveAsSheet` Eigenschaft `false`, führen Sie die Anwendung, und wählen Sie **speichern unter...**  aus der **Datei** Menü wird Folgendes angezeigt:

[![](dialog-images/save01.png "Ein speichern (Dialogfeld)")](dialog-images/save01.png#lightbox)

Der Benutzer kann es sich um das Dialogfeld erweitern:

[![](dialog-images/save02.png "Einen erweiterten speichern (Dialogfeld)")](dialog-images/save02.png#lightbox)

Wenn wir legen die `ShowSaveAsSheet` Eigenschaft `true`, führen Sie die Anwendung, und wählen Sie **speichern unter...**  aus der **Datei** Menü wird Folgendes angezeigt:

[![](dialog-images/save03.png "Ein Blatt gespeichert")](dialog-images/save03.png#lightbox)

Der Benutzer kann es sich um das Dialogfeld erweitern:

[![](dialog-images/save04.png "Speichern Sie einen erweiterten Blatt")](dialog-images/save04.png#lightbox)

Weitere Informationen zum Arbeiten mit das Dialogfeld "Speichern" finden Sie unter Apple [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit modalen Windows, Tabellen und das Standardbetriebssystem Dialogfelder in einer Xamarin.Mac-Anwendung geführt. Erläutert die verschiedenen Typen und Verwendungen von modalen Windows, Stylesheets und Dialogfeldern, wie Sie zum Erstellen und Verwalten von modalen Windows und Blätter in Xcode ist Interface Builder und Arbeiten mit modalen Windows, Stylesheets und Dialogfelder in C#-Code.

## <a name="related-links"></a>Verwandte Links

- [MacWindows (Beispiel)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Menüs](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Symbolleisten](~/mac/user-interface/toolbar.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Einführung in Tabellen](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Einführung in das Drucken](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
