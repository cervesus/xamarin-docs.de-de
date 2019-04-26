---
title: Menüs in Xamarin.Mac
description: In diesem Artikel wird das Arbeiten mit Menüs in einer Xamarin.Mac-Anwendung behandelt. Es wird beschrieben, erstellen und Verwalten von Menüs und Menüelemente in Xcode und Interface Builder und Programmgesteuertes Arbeiten mit ihnen.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 0a14e3e3eb58b264d1909b6576bbbc4f7e8f4068
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61166595"
---
# <a name="menus-in-xamarinmac"></a>Menüs in Xamarin.Mac

_In diesem Artikel wird das Arbeiten mit Menüs in einer Xamarin.Mac-Anwendung behandelt. Es wird beschrieben, erstellen und Verwalten von Menüs und Menüelemente in Xcode und Interface Builder und Programmgesteuertes Arbeiten mit ihnen._

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleiche Cocoa-Menüs, die ein Entwickler in Objective-C und Xcode. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie Interface Builder von Xcode verwenden, erstellen und Verwalten Ihrer Menüleisten, Menüs und Menüelemente (oder erstellen sie optional direkt in c#-Code).

Menüs sind ein wesentlicher Bestandteil der Benutzeroberfläche einer Mac-Anwendung und häufig in verschiedenen Teilen der Benutzeroberfläche angezeigt werden:

- **Der Anwendungsmenüleiste** – Dies ist im Hauptmenü, die am oberen Rand des Bildschirms für jedes Mac-Anwendung angezeigt wird.
- **Kontextmenüs** – diese werden angezeigt, wenn der Benutzer mit der rechten Maustaste oder ein Element in einem Fenster Steuerelement klickt.
- **Die Statusleiste** – Dies ist der Bereich ganz rechts neben der Menüleiste, die auf den oberen Rand des Bildschirms (auf der linken Seite der Uhr auf der Leiste Menü) angezeigt wird und auf der linken Seite zunimmt, wenn Elemente hinzugefügt werden.
- **Docken Sie im Menü** -Menü für jede Anwendung im Dock, das angezeigt wird, wenn der Benutzer mit der rechten Maustaste oder das Symbol der Anwendung Steuerelement klickt oder wenn der Benutzer das Symbol klicken und die Maustaste gedrückt hält.
- **Popup-Schaltfläche und Dropdownmenü Listen** -eine Popup-Schaltfläche ein ausgewählten Elements angezeigt und zeigt eine Liste der Optionen aus, wenn der Benutzer darauf klickt. Eine Drop-Down-Liste ist ein Popup-Schaltfläche in der Regel wird verwendet, um Befehle, die spezifisch für den Kontext der aktuellen Aufgabe. Beide können an einer beliebigen Stelle in einem Fenster angezeigt.

[![Ein Beispiel-Menü](menu-images/intro01.png "eine Beispiel-Menü")](menu-images/intro01-large.png#lightbox)

In diesem Artikel wird die Grundlagen der Arbeit mit Cocoa-Menüleisten, Menüs und Menüelemente in einer Xamarin.Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

## <a name="the-applications-menu-bar"></a>Menüleiste der Anwendung 

Im Gegensatz zu Anwendungen, die unter dem Windows-Betriebssystem, wobei jedes Fenster eine eigene Menüleiste angefügt haben kann, hat jede Anwendung, die unter MacOS eine einzelne Menüleiste, die am oberen Rand des Bildschirms ausgeführt wird, die für jedes Fenster in der Anwendung verwendet wird:

[![Eine Menüleiste](menu-images/appmenu01.png "einer Menüleiste")](menu-images/appmenu01-large.png#lightbox)

Elemente in dieser Menüleiste aktiviert oder deaktiviert werden basierend auf dem aktuellen Kontext oder Zustand der Anwendung und die Benutzeroberfläche zu einem bestimmten Zeitpunkt. Z. B.: Wenn der Benutzer ein Textfeld auswählt, Elemente auf der **bearbeiten** Menü kommen aktiviert ist, z. B. **kopieren** und **Ausschneiden**.

Gemäß den Apple und standardmäßig haben alle Anwendungen für MacOS eine Reihe von Menüs und Menüelemente, die in der Menüleiste der Anwendung angezeigt werden:

- **Apple-Menü** -dieses Menü bietet Zugriff auf System wide Elemente, die für den Benutzer verfügbar sind, immer, unabhängig davon, welche Anwendung ausgeführt wird. Diese Elemente werden nicht vom Entwickler geändert.
- **App-Menü** -dieses Menü zeigt den Namen der Anwendung in Fettschrift angezeigt und hilft dem Benutzer zu identifizieren, welche Anwendung derzeit ausgeführt wird. Es enthält Elemente, die für die Anwendung als Ganzes und nicht mit einem vorhandenen Dokument oder der Prozess, z. B. Beenden der Anwendung gelten.
- **Menü "Datei"** - Elemente verwendet wird, öffnen Sie zum Erstellen oder Dokumente speichern, die Ihre Anwendung ausgeführt wird. Wenn Ihre Anwendung nicht dokumentbasierte ist, kann dieses Menü umbenannt oder entfernt werden.
- **Menü "Bearbeiten"** -Befehle wie z. B. enthält **Ausschneiden**, **Kopie**, und **einfügen** die bearbeiten oder ändern Sie die Elemente in der Benutzeroberfläche der Anwendung verwendet werden.
- **Menü "Format"** : Wenn die Anwendung funktioniert, mit dem Text, der dieses Menü enthält Befehle zum Anpassen der Formatierung des Texts.
- **Menü "Ansicht"** -Befehle, die beeinflussen wie Inhalt angezeigt wird (angezeigt), in der Benutzeroberfläche der Anwendung enthält.
- **Anwendungsspezifische Menüs** -Hierbei handelt es sich um alle Menüs, die für Ihre Anwendung (z. B. ein Lesezeichenmenü für einen Webbrowser) spezifisch sind. Sie sollte angezeigt werden, zwischen der **Ansicht** und **Fenster** Menüs auf der Leiste.
- **Menü "Fenster"** – enthält Befehle für die Arbeit mit Windows in Ihrer Anwendung sowie eine Liste der aktuellen geöffneten Fenster.
- **Menü "Hilfe"** – Wenn Ihre Anwendung auf dem Bildschirm Hilfe bereitstellt, sollte das Menü "Hilfe" klicken Sie im Menü rechts auf der Leiste sein. 

Weitere Informationen über die Menüleiste und standard-Menüs und Menüelemente, finden Sie unter Apple [Human Interface Guidelines](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>Der Standardwert Menüleiste

Wenn Sie ein neues Xamarin.Mac-Projekt erstellen, erhalten Sie automatisch eine standard-Anwendung Standardmenüleiste, die die typische Elemente, die normalerweise eine MacOS-Anwendung (wie im vorherigen Abschnitt erläutert) hätte. Standardmenüleiste Ihrer Anwendung wird definiert, der **"Main.Storyboard"** -Datei (zusammen mit den restlichen Ihrer app Benutzeroberfläche) unter dem Projekt die **Lösungspad**:  

![Wählen Sie die Haupt-Storyboard](menu-images/appmenu02.png "wählen Sie die Haupt-Storyboard")

Doppelklicken Sie auf die **"Main.Storyboard"** Datei, um ihn zu öffnen, für die Bearbeitung in Interface Builder von Xcode, und Sie die Benutzeroberfläche des Menü-Editors angezeigt werden:

[![Bearbeiten die Benutzeroberfläche in Xcode](menu-images/defaultbar01.png "Bearbeitung der Benutzeroberflächenautomatisierungs in Xcode")](menu-images/defaultbar01-large.png#lightbox)

Von hier aus können klicken wir auf Elemente wie z. B. die **öffnen** Menüelements in der **Datei** Menü und bearbeiten, oder passen Sie seine Eigenschaften im der **Attributes Inspector**:

[![Bearbeiten eines Menüs Attribute](menu-images/defaultbar02.png "eines Menüs Attribute bearbeiten")](menu-images/defaultbar02-large.png#lightbox)

Wir kommen in hinzufügen, bearbeiten und Löschen von Menüs und Elementen weiter unten in diesem Artikel. Für jetzt wir wollen, um herauszufinden, welche Menüs und Menüelemente standardmäßig verfügbar sind und wie sie automatisch Code über einen Satz von vordefinierten Outlets und Aktionen verfügbar gemacht wurden (Weitere Informationen finden Sie unserem [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Dokumentation).

Z. B. durch Klicken auf die **Verbindung Inspektor** für die **öffnen** Menüelement wir sehen, es ist automatisch verknüpft bis zu der `openDocument:` Aktion: 

[![Anzeigen der angefügten Aktion](menu-images/defaultbar03.png "die angefügten Aktion anzeigen")](menu-images/defaultbar03-large.png#lightbox)

Bei Auswahl von der **erste Beantworter** in die **Schnittstellenhierarchie** und scrollen Sie in der **Verbindung Inspektor**, und sehen Sie die Definition der der `openDocument:` Aktion, die die **öffnen** Menüelement (zusammen mit mehreren anderen Standardaktionen für die Anwendung, die und werden nicht automatisch angeschlossen an Steuerelemente) angefügt ist:

[![Anzeigen von alle angeschlossenen Aktionen](menu-images/defaultbar04.png "alle angefügten Aktionen anzeigen")](menu-images/defaultbar04-large.png#lightbox) 

Warum ist das wichtig? In den nächsten wird im Abschnitt angezeigt, wie diese Aktionen automatisch definiert, die mit anderen Cocoa-Benutzeroberflächenelementen automatisch zu aktivieren und Deaktivieren von Menüelementen, als auch, integrierte Funktionen für die Elemente bereitstellen arbeiten.

Später werden wir verwenden diese integrierten Aktionen aktivieren oder Deaktivieren von Elementen aus Code und unserer eigenen Funktionen bereitstellen, wenn sie ausgewählt werden.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Integriertes Menü-Funktion

Wenn Sie die Ausführung einer neu erstellten Xamarin.Mac-Anwendung vor dem UI-Elemente oder Code hinzugefügt haben, werden Sie feststellen, dass einige Elemente werden automatisch verbunden und für Sie (mit vollständig Funktionalität automatisch integrierte) werden, z. B. aktiviert die **Quit** Element in der **App** Menü:

![Ein aktiviert Menüelement](menu-images/appmenu03.png "ein aktiviert Menüelement")

Während Sie andere Menüelemente, z. B. **Ausschneiden**, **Kopie**, und **einfügen** nicht:

![Deaktiviert die Menüelemente](menu-images/appmenu04.png "Menüelemente deaktiviert")

Lassen Sie uns die Anwendung beenden, und doppelklicken Sie auf die **"Main.Storyboard"** Datei die **Lösungspad** es für die Bearbeitung in Xcode öffnen, die Sie Interface Builder. Ziehen Sie jetzt eine **Textansicht** aus der **Bibliothek** auf Ansicht des Fensters in der **Schnittstellen-Editor**:

[![Wählen aus der Bibliothek eine Textansicht](menu-images/appmenu05.png "eine Textansicht aus der Bibliothek auswählen")](menu-images/appmenu05-large.png#lightbox)

In der **Einschränkung Editor** wir heften Sie die Textansicht an des Fensters Ränder und festlegen, wo sie wächst und schrumpft mit dem Fenster alle vier rote Balken an den oberen Rand des Editors klicken und auf, die **hinzufügen 4 Einschränkungen** Schaltfläche:

[![Bearbeiten die Einschränkungen](menu-images/appmenu06.png "die Einschränkungen bearbeiten")](menu-images/appmenu06-large.png#lightbox)

Speichern Sie Ihre Änderungen an der Entwurf der Benutzeroberfläche aus, und wechseln Sie wieder die Visual Studio für Mac, um die Änderungen mit dem Xamarin.Mac-Projekt zu synchronisieren. Jetzt die Anwendung zu starten, geben Sie Text in der Textansicht, wählen Sie ihn, und Öffnen der **bearbeiten** Menü:

![Die Menüelemente werden automatisch aktiviert oder deaktiviert](menu-images/appmenu07.png "Menüelemente werden automatisch aktiviert oder deaktiviert")

Beachten Sie, dass die **Ausschneiden**, **Kopie**, und **einfügen** Elemente sind automatisch aktiviert und voll funktionsfähig, ohne eine einzige Zeile Code schreiben. 

Was ist hier los? Denken Sie daran, die integrierte vordefinieren, Aktionen, die die Standard-Menüelemente anwendungskerns stammen (wie oben dargestellt), die meisten Cocoa Elemente der Benutzeroberfläche, die Teil von MacOS sind über eine integrierte Hooks, um bestimmte Aktionen (z. B. `copy:`). Also, wenn sie ein Fenster aktiv ist, hinzugefügt und ausgewählt, das entsprechende Menüelement oder die Elemente, die angefügt werden mit dieser Aktion werden automatisch aktiviert. Wenn der Benutzer das Menüelement auswählt, ist die Funktionen in das Element der Benutzeroberfläche aufgerufen und ausgeführt werden, ohne Eingriffe von Entwicklern.

### <a name="enabling-and-disabling-menus-and-items"></a>Aktivieren und Deaktivieren von Menüs und Elemente

Standardmäßig wird jedes Mal, wenn ein Ereignis auftritt `NSMenu` automatisch aktiviert und deaktiviert die einzelnen sichtbar und Menü-Element auf Grundlage des Kontexts der Anwendung. Es gibt drei Möglichkeiten zum Aktivieren/Deaktivieren Sie ein Element:

- **Aktivieren von Menüs, automatisch** -ein Menüelement aktiviert ist, wenn `NSMenu` finde, dass ein Objekt, das an die Aktion reagiert, die das Element verbunden, von ist. Z. B. die Textansicht oben, die einen integrierten Hook, mussten die `copy:` Aktion.
- **Benutzerdefinierte Aktionen und ValidateMenuItem:** : für jedes Menüelement, das gebunden ist eine [Fenster oder einer Ansicht benutzerdefinierte Controlleraktion](#Working-with-Custom-Window-Actions), hinzufügbaren den `validateMenuItem:` Aktion und manuell aktivieren oder Deaktivieren von Menüelementen.
- **Manuelle Menü aktivieren** -Sie manuell festlegen, die `Enabled` Eigenschaft der einzelnen `NSMenuItem` aktivieren oder deaktivieren jedes Element in einem Menü einzeln.

Zum Auswählen eines Systems legen die `AutoEnablesItems` Eigenschaft eine `NSMenu`. `true` automatisch (das Standardverhalten) und `false` ist "manuell". 

> [!IMPORTANT]
> Wenn Sie entscheiden, verwenden die manuelle Menü zu aktivieren, keine der im Menü Elemente, auch solche, die von AppKit-Klassen wie gesteuert `NSTextView`, werden automatisch aktualisiert. Sie werden zum Aktivieren und deaktivieren alle Elemente manuell im Code verantwortlich.

#### <a name="using-validatemenuitem"></a>Verwenden von validateMenuItem

Wie oben für jedes Menüelement im angegeben, die gebunden ist eine [Fenster oder View Controller benutzerdefinierte Aktion](#Working-with-Custom-Window-Actions), hinzufügbaren den `validateMenuItem:` Aktion und manuell aktivieren oder Deaktivieren von Menüelementen.

Im folgenden Beispiel die `Tag` Eigenschaft wird verwendet, um den Typ des Menüelements entscheiden, die vom Aktivierungszustand wird werden die `validateMenuItem:` Aktion basierend auf den Zustand des markierten Texts in einem `NSTextView`. Die `Tag` Eigenschaft in Interface Builder für jedes Menüelement festgelegt wurde:

![Festlegen der Tag-Eigenschaft](menu-images/validate01.png "Festlegen der Tag-Eigenschaft")

Und den folgenden Code, der mit dem Ansicht-Controller hinzugefügt:

```csharp
[Action("validateMenuItem:")]
public bool ValidateMenuItem (NSMenuItem item) {

    // Take action based on the menu item type
    // (As specified in its Tag)
    switch (item.Tag) {
    case 1:
        // Wrap menu items should only be available if
        // a range of text is selected
        return (TextEditor.SelectedRange.Length > 0);
    case 2:
        // Quote menu items should only be available if
        // a range is NOT selected.
        return (TextEditor.SelectedRange.Length == 0);
    }

    return true;
}
```

Wenn dieser Code ausgeführt wird und kein Text markiert ist, der `NSTextView`, die zwei Wrap Menüelemente sind deaktiviert (auch wenn sie mit Aktionen für den ansichtscontroller verbunden sind):

![Mit deaktiviert Elemente](menu-images/validate02.png "zeigt Deaktivierte Elemente")

Wenn Sie ein Textabschnitt ausgewählt ist, und klicken Sie im Menü erneut geöffnet wird, werden die Menüelemente zwei Wrap verfügbar sein:

![Aktiviert mit Elementen](menu-images/validate03.png "zeigt aktivierte Elemente")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Aktivieren von und reagieren auf Menüelemente im code

Wie wir oben gesehen haben, einfach durch Hinzufügen von bestimmten Cocoa-Benutzeroberflächenelemente zu unserem Entwurf von Benutzeroberflächen (z. B. ein Textfeld), mehrere Menüelemente standardmäßig aktiviert und die Funktion automatisch, ohne Code schreiben zu müssen. Nächste sehen wir uns unsere Xamarin.Mac-Projekt, aktivieren ein Menüelement, und stellen Funktionalität bereit, wenn der Benutzer, es auswählt unseren eigenen C#-Code hinzugefügt.

Beispielsweise können sagen, wir möchten, dass die Benutzer verwenden können, die **öffnen** Element in der **Datei** Menü zur Auswahl eines Ordners. Da wir möchten diese Option, um eine anwendungsweite-Funktion werden und nicht auf ein bieten-Fenster oder ein Element der Benutzeroberfläche beschränkt ist, werden wir den Code zum Behandeln dieses zuzuweisende unsere Anwendung hinzufügen.

In der **Lösungspad**, doppelklicken Sie auf die `AppDelegate.CS` Datei, die sie für die Bearbeitung zu öffnen:

![Auswählen der app-Delegat](menu-images/appmenu08.png "Auswählen der app-Delegat")

Fügen Sie unter der `DidFinishLaunching`-Methode folgenden Code hinzu:

```csharp
[Export ("openDocument:")]
void OpenDialog (NSObject sender)
{
    var dlg = NSOpenPanel.OpenPanel;
    dlg.CanChooseFiles = false;
    dlg.CanChooseDirectories = true;

    if (dlg.RunModal () == 1) {
        var alert = new NSAlert () {
            AlertStyle = NSAlertStyle.Informational,
            InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
            MessageText = "Folder Selected"
        };
        alert.RunModal ();
    }
}
```

Lassen Sie uns nun die Anwendung auszuführen, und öffnen Sie die **Datei** Menü: 

![Das Menü "Datei"](menu-images/appmenu09.png "Menü \"die Datei\"")

Beachten Sie, dass die **öffnen** Menüelement ist jetzt aktiviert. Wenn wir diese Option auswählen, wird das Dialogfeld "Öffnen" angezeigt:

![Ein Dialogfeld "Öffnen"](menu-images/appmenu10.png "ein Dialogfeld \"Öffnen\"")

Wenn wir auf die **öffnen** Schaltfläche unsere Warnmeldung wird angezeigt:

![Eine Beispiel-Dialog-Nachricht](menu-images/appmenu11.png "eine Beispiel-Dialog-Nachricht")

Der Schlüssel für diese Zeile hier `[Export ("openDocument:")]`, weist `NSMenu` , unsere **AppDelegate** verfügt über eine Methode `void OpenDialog (NSObject sender)` reagiert, die auf der `openDocument:` Aktion. Wenn Sie von oben, wissen die **öffnen** Menüelement ist automatisch verknüpft – auf bis zu dieser Aktion standardmäßig in Interface Builder:

[![Anzeigen von angefügten Aktionen](menu-images/defaultbar03.png "die angefügten Aktionen anzeigen")](menu-images/defaultbar03-large.png#lightbox)

Nächste sehen wir uns erstellen unserer eigenen Menüs, Menüelemente und Aktionen von und reagieren auf diese im Code.

### <a name="working-with-the-open-recent-menu"></a>Arbeiten mit dem aktuellen Menü öffnen

In der Standardeinstellung die **Datei** Menü enthält eine **öffnen aktuelle** Element, das verfolgt den letzten mehrere Dateien, die der Benutzer mit Ihrer app geöffnet hat. Bei der Erstellung einer `NSDocument` basierend Xamarin.Mac-app in diesem Menü werden automatisch für Sie verarbeitet werden. Für jede andere Art von Xamarin.Mac-app werden Sie verantwortlich für das Verwalten von und reagieren auf dieses Menüelement manuell.

Manuell die **öffnen aktuelle** Menü müssen Sie zuerst, sie zu informieren, dass eine neue Datei wurde geöffnet oder gespeichert, mit dem folgenden:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Auch wenn Ihre app nicht verwendet `NSDocuments`, Sie weiterhin verwenden, der `NSDocumentController` zu verwalten der **zuletzt verwendete öffnen** Menü durch Senden einer `NSUrl` durch den Speicherort der Datei, die der `NoteNewRecentDocumentURL` -Methode der der `SharedDocumentController`.

Als Nächstes müssen Sie überschreiben die `OpenFile` Methode von der app-Delegat, der eine beliebige Datei öffnen, die der Benutzer auswählt der **zuletzt verwendete öffnen** Menü. Zum Beispiel:

```csharp
public override bool OpenFile (NSApplication sender, string filename)
{
    // Trap all errors
    try {
        filename = filename.Replace (" ", "%20");
        var url = new NSUrl ("file://"+filename);
        return OpenFile(url);
    } catch {
        return false;
    }
}
```

Zurückgeben `true` , wenn die Datei geöffnet werden kann, andernfalls zurückgeben `false` und eine integrierte Warnung wird angezeigt, die dem Benutzer, dass die Datei nicht geöffnet werden konnte.

Da die Dateinamen und den Pfad aus zurückgegeben der **zuletzt verwendete öffnen** Menü kann Leerzeichen enthalten, müssen wir dieses Zeichen mit Escapezeichen vor dem Erstellen einer `NSUrl` oder ein Fehler ausgegeben wird. Hierzu verwenden Sie den folgenden Code:

```csharp
filename = filename.Replace (" ", "%20");
```

Schließlich erstellen wir eine `NSUrl` , der auf die Datei, und verwenden eine Hilfsmethode in der app ein neues Fenster öffnen, und Laden Sie die Datei in diesen Delegaten verweist:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Um alles zusammentragen, werfen wir einen Blick auf ein Beispiel einer Implementierung in einer **Datei "appdelegate.cs"** Datei:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace MacHyperlink
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;
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

        public override bool OpenFile (NSApplication sender, string filename)
        {
            // Trap all errors
            try {
                filename = filename.Replace (" ", "%20");
                var url = new NSUrl ("file://"+filename);
                return OpenFile(url);
            } catch {
                return false;
            }
        }
        #endregion

        #region Private Methods
        private bool OpenFile(NSUrl url) {
            var good = false;

            // Trap all errors
            try {
                var path = url.Path;

                // Is the file already open?
                for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
                    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
                    if (content != null && path == content.FilePath) {
                        // Bring window to front
                        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
                        return true;
                    }
                }

                // Get new window
                var storyboard = NSStoryboard.FromName ("Main", null);
                var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

                // Display
                controller.ShowWindow(this);

                // Load the text into the window
                var viewController = controller.Window.ContentViewController as ViewController;
                viewController.Text = File.ReadAllText(path);
                viewController.SetLanguageFromPath(path);
                viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
                viewController.View.Window.RepresentedUrl = url;

                // Add document to the Open Recent menu
                NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);

                // Make as successful
                good = true;
            } catch {
                // Mark as bad file on error
                good = false;
            }

            // Return results
            return good;
        }
        #endregion

        #region actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = true;
            dlg.CanChooseDirectories = false;

            if (dlg.RunModal () == 1) {
                // Nab the first file
                var url = dlg.Urls [0];

                if (url != null) {
                    // Open the document in a new window
                    OpenFile (url);
                }
            }
        }
        #endregion
    }
}
```

Je nach den Anforderungen Ihrer App empfiehlt nicht den Benutzer, öffnen Sie die gleiche Datei in mehrere Fenster zur gleichen Zeit. In unserer Beispiel-app, wenn der Benutzer eine Datei auswählt, die bereits geöffnet ist (entweder über die **zuletzt verwendete öffnen** oder **öffnen...** Menüelemente), wird das Fenster mit der Datei in den Vordergrund geholt.

Um dies zu erreichen, verwendet haben wir den folgenden Code in die Hilfsmethode:

```csharp
var path = url.Path;

// Is the file already open?
for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
    var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
    if (content != null && path == content.FilePath) {
        // Bring window to front
        NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
        return true;
    }
}
```

Wir gestaltet, dass unsere `ViewController` Klasse für den Pfad zur Datei in seine `Path` Eigenschaft. Als Nächstes durchlaufen wir alle derzeit geöffneten Fenster in der app ein. Ist die Datei bereits in einem Fenster geöffnet ist, wird es im Vordergrund aller anderen Fenster mit angezeigt:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Wenn keine Übereinstimmung gefunden wird, wird ein neues Fenster, mit der Datei geladen geöffnet wird und die Datei Sie in finden der **öffnen aktuelle** Menü:

```csharp
// Get new window
var storyboard = NSStoryboard.FromName ("Main", null);
var controller = storyboard.InstantiateControllerWithIdentifier ("MainWindow") as NSWindowController;

// Display
controller.ShowWindow(this);

// Load the text into the window
var viewController = controller.Window.ContentViewController as ViewController;
viewController.Text = File.ReadAllText(path);
viewController.SetLanguageFromPath(path);
viewController.View.Window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
viewController.View.Window.RepresentedUrl = url;

// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

<a name="Working-with-Custom-Window-Actions" />

### <a name="working-with-custom-window-actions"></a>Arbeiten mit benutzerdefinierten Fensteraktionen

Ebenso wie die integrierte **erste Beantworter** Aktionen, die richtig verkabelt Standardmenüelementen, können Sie neu erstellen, benutzerdefinierte Aktionen und werden, Menüelemente in Interface Builder zu verknüpfen.

Definieren Sie zuerst eine benutzerdefinierte Aktion auf einer der Controller für Ihre app Fenster. Zum Beispiel:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Doppelklicken Sie dann auf die app Storyboard-Datei in die **Lösungspad** es für die Bearbeitung in Xcode öffnen, die Sie Interface Builder. Wählen Sie die **erste Beantworter** unter der **Szene Anwendung**, wechseln Sie zu der **Attributes Inspector**:

![Attributes Inspector](menu-images/action01.png "Attributes Inspector")

Klicken Sie auf die **+** Schaltfläche am unteren Rand der **Attributes Inspector** eine neue benutzerdefinierte Aktion hinzufügen:

![Hinzufügen einer neuen Aktion](menu-images/action02.png "eine neue Aktion hinzufügen")

Geben sie den gleichen Namen wie die benutzerdefinierte Aktion, die Sie auf dem fenstercontroller erstellt:

![Bearbeiten den Aktionsnamen](menu-images/action03.png "der Name der Aktion bearbeiten")

Strg + Klick und ziehen Sie ein Menüelement der **erste Beantworter** unter der **Szene Anwendung**. Wählen Sie aus der Popupliste, die neue Aktion, die Sie gerade erstellt haben (`defineKeyword:` in diesem Beispiel):

![Anfügen einer Aktion](menu-images/action04.png "Anfügen einer Aktion")

Speichern Sie die Änderungen auf das Storyboard und zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren. Wenn Sie die app ausführen, das Menüelement, dem Sie die benutzerdefinierte Aktion angeschlossen wird automatisch aktiviert bzw. deaktiviert werden (basierend auf das Fenster mit der Aktion, die geöffnet wird), und Auswählen des Menüelements wird der Vorgang ausgelöst:

[![Testen die neue Aktion](menu-images/action05.png "testen die neue Aktion")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Hinzufügen, bearbeiten und Löschen von Menüs

Wie wir in den vorherigen Abschnitten gesehen haben, wird eine Xamarin.Mac-Anwendung mit einer vordefinierten Anzahl von Standardmenüs und Menüelemente, die bestimmte UI-Steuerelementen werden automatisch zu aktivieren und darauf reagieren. Wir haben auch gesehen, wie unsere Anwendung Code hinzugefügt, die auch aktivieren und auf diese Standardelemente reagieren.

In diesem Abschnitt betrachten wir Entfernen von Menüelementen, die wir nicht brauchen, durch das Neuorganisieren Menüs und Hinzufügen von neuen Menüs, Menüelemente und Aktionen.

Doppelklicken Sie auf die **"Main.Storyboard"** Datei die **Lösungspad** um ihn zur Bearbeitung zu öffnen:

[![Bearbeiten die Benutzeroberfläche in Xcode](menu-images/maint01.png "Bearbeitung der Benutzeroberflächenautomatisierungs in Xcode")](menu-images/maint01-large.png#lightbox)

Für die bestimmte Xamarin.Mac-Anwendung werden wir nicht die Standardeinstellung verwenden **Ansicht** Menü wir also um ihn zu entfernen. In der **Schnittstellenhierarchie** wählen Sie die **Ansicht** Menüelement, das Teil der Hauptmenüleiste ist:

![Auswählen des Menüelements Ansicht](menu-images/maint02.png "auswählen des Menüelements anzeigen")

Drücken Sie ENTF oder die RÜCKTASTE, um das Menü zu löschen. Als Nächstes wird hier nicht alle Elemente im Verwenden der **Format** Menü, und wir die Elemente zu verschieben, werden wir Sie unter den Untermenüs verwenden, möchten. In der **Schnittstellenhierarchie** wählen Sie die folgenden Punkte:

![Mehrere Elemente markieren](menu-images/maint03.png "mehrere Elemente markieren")

Ziehen Sie die Elemente unter dem übergeordneten Element **Menü** im Untermenü, in denen sie aktuell sind:

[![Ziehen auf das übergeordnete Menü Menüelemente](menu-images/maint04.png "Ziehen auf das übergeordnete Menü Menüelemente")](menu-images/maint04-large.png#lightbox)

Das Menü sollte jetzt aussehen:

[![Die Elemente in den neuen Speicherort](menu-images/maint05.png "die Elemente in den neuen Speicherort")](menu-images/maint05-large.png#lightbox)

Weiter ziehen Sie die **Text** Untermenü, unter der **Format** Menü und platzieren Sie sie in der Hauptmenüleiste zwischen der **Format** und **Fenster** Menüs:

[![Klicken Sie im Menü Text](menu-images/maint06.png "die Text-Menü")](menu-images/maint06-large.png#lightbox)

Kehren wir wieder unter das **Format** Menü- und löschen die **Schriftart** Untermenü-Element. Wählen Sie als Nächstes die **Format** Menü, und geben sie "Schriftart":

[![Klicken Sie im Menü Schriftart](menu-images/maint07.png "Menü der Schriftart")](menu-images/maint07-large.png#lightbox)

Als Nächstes erstellen wir ein benutzerdefiniertes Menü der vordefinierten Zeichenfolgen, das automatisch werden auf den Text in der Textansicht angefügt werden bei der Auswahl. In das Suchfeld am unteren Rand auf die **Bibliotheksinspektor** Geben Sie "Menu". Dadurch wird es leichter zu finden und mit allen die Elemente der Benutzeroberfläche arbeiten:

![Der Bibliotheksinspektor](menu-images/maint08.png "im Bibliotheksinspektor festlegen")

Jetzt führe Folgendes aus, um unser Menü zu erstellen:

1. Ziehen Sie eine **Menüelement** aus der **Bibliotheksinspektor** auf der Menüleiste zwischen der **Text** und **Fenster** Menüs: 

    ![Wählen ein neues Menüelement in der Bibliothek](menu-images/maint10.png "ein neues Menüelement in der Bibliothek auswählen")
2. Benennen Sie das Element "Ausdrücke": 

    [![Festlegen des Namens im Menü](menu-images/maint09.png "Festlegen des Namens im Menü")](menu-images/maint09-large.png#lightbox)
3. Ziehen Sie als Nächstes eine **Menü** aus der **Bibliotheksinspektor**: 

    ![Wählen ein Menü, aus der Bibliothek](menu-images/maint11.png "ein Menü aus der Bibliothek auswählen")
4. Anschließend können Sie **Menü** auf dem neuen **Menüelement** wir gerade erstellt, und ändern Sie den Namen auf "Ausdrücke": 

    [![Bearbeiten den Namen des Menüs](menu-images/maint12.png "bearbeiten den Namen des Menüs")](menu-images/maint12-large.png#lightbox)
5. Nun wir benennen die drei Standard **Menüelemente** "Address", "Date" und "Greeting": 

    [![Das Menü "Ausdrücke"](menu-images/maint13.png "der Ausdrücke im Menü")](menu-images/maint13-large.png#lightbox)
6. Fügen Sie einen vierten **Menüelement** durch Ziehen einer **Menüelement** aus der **Bibliotheksinspektor** und Aufrufen dieser "Signatur": 

    [![Bearbeiten die Namen des Menüelements](menu-images/maint14.png "Namen des Menüelements bearbeiten")](menu-images/maint14-large.png#lightbox)
7. Speichern der Änderungen in der Menüleiste.

Jetzt erstellen wir eine Reihe von benutzerdefinierten Aktionen aus, sodass unsere neue Menüelemente, die für c#-Code verfügbar gemacht werden. Beginnen wir nun in Xcode mit der **Assistant** anzeigen:

[![Erstellen die erforderlichen Aktionen](menu-images/maint15.png "erstellen die erforderlichen Aktionen")](menu-images/maint15-large.png#lightbox)

Lassen Sie uns wie folgt vor:

1. Steuerelement ziehen Sie aus der **Adresse** Menüelement der **"appdelegate.h"** Datei.
2. Wechseln der **Verbindung** Typ **Aktion**: 

    [![Wählen den Aktionstyp](menu-images/maint17.png "den Aktionstyp auswählen")](menu-images/maint17-large.png#lightbox)
3. Geben Sie einen **Namen** "PhraseAddress", und drücken Sie die **Connect** klicken, um die neue Aktion zu erstellen: 

    [![Konfigurieren die Aktion](menu-images/maint18.png "Konfigurieren der Aktion")](menu-images/maint18-large.png#lightbox)
4. Wiederholen Sie die oben genannten Schritte für die **Datum**, **Begrüßung**, und **Signatur** Menüelemente: 

    [![Die abgeschlossenen Aktionen](menu-images/maint19.png "die abgeschlossenen Aktionen")](menu-images/maint19-large.png#lightbox)
5. Speichern der Änderungen in der Menüleiste.

Als Nächstes müssen wir ein outlets für unsere Textansicht zu erstellen, sodass wir den Inhalt von Code anpassen können. Wählen Sie die **"viewcontroller.h"** Datei die **Assistenten-Editor** , und erstellen Sie eine neue Outlet namens `documentText`:

[![Erstellen eines outlets](menu-images/maint20.png "Erstellen eines outlets")](menu-images/maint20-large.png#lightbox)

Zurück zu Visual Studio für Mac, um die Änderungen von Xcode zu synchronisieren. Als Nächstes bearbeiten Sie die **ViewController.cs** Datei, und stellen sie wie folgt aussehen:

```csharp
using System;

using AppKit;
using Foundation;

namespace MacMenus
{
    public partial class ViewController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public override NSObject RepresentedObject {
            get {
                return base.RepresentedObject;
            }
            set {
                base.RepresentedObject = value;
                // Update the view, if already loaded.
            }
        }

        public string Text {
            get { return documentText.Value; }
            set { documentText.Value = value; }
        } 
        #endregion

        #region Constructors
        public ViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any additional setup after loading the view.
        }

        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            App.textEditor = this;
        }

        public override void ViewWillDisappear ()
        {
            base.ViewDidDisappear ();

            App.textEditor = null;
        }
        #endregion
    }
}
```

Dadurch wird den Text unserer Ansicht Text außerhalb des dem `ViewController` Klasse und informiert Sie der app-Delegat, wenn das Fenster erhält oder verliert den Fokus. Bearbeiten Sie jetzt die **Datei "appdelegate.cs"** Datei, und stellen sie wie folgt aussehen:

```csharp
using AppKit;
using Foundation;
using System;

namespace MacMenus
{
    [Register ("AppDelegate")]
    public partial class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public ViewController textEditor { get; set;} = null;
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

        #region Custom actions
        [Export ("openDocument:")]
        void OpenDialog (NSObject sender)
        {
            var dlg = NSOpenPanel.OpenPanel;
            dlg.CanChooseFiles = false;
            dlg.CanChooseDirectories = true;

            if (dlg.RunModal () == 1) {
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = "At this point we should do something with the folder that the user just selected in the Open File Dialog box...",
                    MessageText = "Folder Selected"
                };
                alert.RunModal ();
            }
        }

        partial void phrasesAddress (Foundation.NSObject sender) {

            textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
        }

        partial void phrasesDate (Foundation.NSObject sender) {

            textEditor.Text += DateTime.Now.ToString("D");
        }

        partial void phrasesGreeting (Foundation.NSObject sender) {

            textEditor.Text += "Dear Sirs,\n\n";
        }

        partial void phrasesSignature (Foundation.NSObject sender) {

            textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
        }
        #endregion
    }
}
```

Hier haben Sie die `AppDelegate` eine partielle Klasse, sodass wir verwenden können, Aktionen und Ergebnisdaten, die wir in Interface Builder definiert. Wir auch zur Verfügung stellen eine `textEditor` nachverfolgen, welches Fenster gegenwärtig den Fokus hat.

Die folgenden Methoden werden verwendet, um unseren benutzerdefinierten Menüs und Menüelemente zu behandeln:

```csharp
partial void phrasesAddress (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Xamarin HQ\n394 Pacific Ave, 4th Floor\nSan Francisco CA 94111\n\n";
}

partial void phrasesDate (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += DateTime.Now.ToString("D");
}

partial void phrasesGreeting (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Dear Sirs,\n\n";
}

partial void phrasesSignature (Foundation.NSObject sender) {

    if (textEditor == null) return;
    textEditor.Text += "Sincerely,\n\nKevin Mullins\nXamarin,Inc.\n";
}
```

Jetzt ausführen unserer Anwendung alle Elemente in der **Ausdruck** Menü aktiv und die Textansicht, die bei Auswahl der bieten Ausdruck hinzu:

![Ein Beispiel für die Ausführung der app](menu-images/maint21.png "ein Beispiel für die app ausgeführt wird")

Nun, wir die Grundlagen der Arbeit mit der Menüleiste unten haben, sehen wir uns ein benutzerdefiniertes Kontextmenü erstellen.

### <a name="creating-menus-from-code"></a>Erstellen von Menüs in code

Zusätzlich zum Erstellen von Menüs und Menüelemente mit Interface Builder von Xcode, es gibt möglicherweise Zeiten, wenn eine Xamarin.Mac-app erstellen, ändern oder entfernen Sie ein Menü, Untermenü oder Menüelement aus Code muss.

Im folgenden Beispiel wird eine Klasse erstellt, zum Speichern der Informationen über die Menüelemente und Untermenüs, die dynamisch auf dynamisch erstellt werden:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using AppKit;

namespace AppKit.TextKit.Formatter
{
    public class LanguageFormatCommand : NSObject
    {
        #region Computed Properties
        public string Title { get; set; } = "";
        public string Prefix { get; set; } = "";
        public string Postfix { get; set; } = "";
        public List<LanguageFormatCommand> SubCommands { get; set; } = new List<LanguageFormatCommand>();
        #endregion

        #region Constructors
        public LanguageFormatCommand () {

        }

        public LanguageFormatCommand (string title)
        {
            // Initialize
            this.Title = title;
        }

        public LanguageFormatCommand (string title, string prefix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
        }

        public LanguageFormatCommand (string title, string prefix, string postfix)
        {
            // Initialize
            this.Title = title;
            this.Prefix = prefix;
            this.Postfix = postfix;
        }
        #endregion
    }
}
```

#### <a name="adding-menus-and-items"></a>Hinzufügen von Menüs und Elemente

Mit dieser Klasse definiert ist, werden die folgende Routine eine Auflistung von analysiert `LanguageFormatCommand`Objekte und rekursiven erstellen neue Menüs und Menüelemente durch Anfügen an den unteren Rand des vorhandenen Menüs, die (in Interface Builder erstellt), der in übergeben wurde:

```csharp
private void AssembleMenu(NSMenu menu, List<LanguageFormatCommand> commands) {
    NSMenuItem menuItem;

    // Add any formatting commands to the Formatting menu
    foreach (LanguageFormatCommand command in commands) {
        // Add separator or item?
        if (command.Title == "") {
            menuItem = NSMenuItem.SeparatorItem;
        } else {
            menuItem = new NSMenuItem (command.Title);

            // Submenu?
            if (command.SubCommands.Count > 0) {
                // Yes, populate submenu
                menuItem.Submenu = new NSMenu (command.Title);
                AssembleMenu (menuItem.Submenu, command.SubCommands);
            } else {
                // No, add normal menu item
                menuItem.Activated += (sender, e) => {
                    // Apply the command on the selected text
                    TextEditor.PerformFormattingCommand (command);
                };
            }
        }
        menu.AddItem (menuItem);
    }
}
``` 

Für alle `LanguageFormatCommand` -Objekt, das ein leeres hat `Title` -Eigenschaft, die diese Routine erstellt eine **Trennzeichen-Menüelement** (eine schlanke graue Linie) zwischen Menü Abschnitten:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Wenn ein Titel angegeben wird, wird ein neues Menüelement mit dem Titel erstellt:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Wenn die `LanguageFormatCommand` -Objekt enthält untergeordnete `LanguageFormatCommand` -Objekten, Untermenü erstellt wird und die `AssembleMenu` Methode ist rekursiv aufgerufen, um diesem Menü zu erstellen:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Für jedes neue Menüelement, das nicht über untergeordnete Menüs verfügt, wird Code hinzugefügt, wird vom Benutzer ausgewählten Menüelements behandeln:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Testen das Menü "erstellen

Alle obigen Code vorhanden Wenn die folgende Sammlung von `LanguageFormatCommand` Objekte erstellt wurden:

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Strong","**","**"));
FormattingCommands.Add(new LanguageFormatCommand("Emphasize","_","_"));
FormattingCommands.Add(new LanguageFormatCommand("Inline Code","`","`"));
FormattingCommands.Add(new LanguageFormatCommand("Code Block","```\n","\n```"));
FormattingCommands.Add(new LanguageFormatCommand("Comment","<!--","-->"));
FormattingCommands.Add (new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Unordered List","* "));
FormattingCommands.Add(new LanguageFormatCommand("Ordered List","1. "));
FormattingCommands.Add(new LanguageFormatCommand("Block Quote","> "));
FormattingCommands.Add (new LanguageFormatCommand ());

var Headings = new LanguageFormatCommand ("Headings");
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 1","# "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 2","## "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 3","### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 4","#### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 5","##### "));
Headings.SubCommands.Add(new LanguageFormatCommand("Heading 6","###### "));
FormattingCommands.Add (Headings);

FormattingCommands.Add(new LanguageFormatCommand ());
FormattingCommands.Add(new LanguageFormatCommand("Link","[","]()"));
FormattingCommands.Add(new LanguageFormatCommand("Image","![](",")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![](",")](LinkImageHere)"));
```

Dieser Sammlung zum Übergeben der `AssembleMenu` Funktion (mit der **Format** Menü als Basis festgelegt), würden die folgenden dynamischen Menüs und Menüelemente erstellt werden:

![Die neuen Menüelemente in der ausgeführten app](menu-images/dynamic01.png "die neuen Menüelemente in der ausgeführten app")

#### <a name="removing-menus-and-items"></a>Entfernen von Menüs und Elemente

Wenn Sie einem Menü oder das Menüelement aus der app-Benutzeroberfläche entfernen möchten, können Sie mithilfe der `RemoveItemAt` Methode der `NSMenu` Klasse einfach indem Sie die 0 (null) Ihren nullbasierte Index des zu entfernenden Elements.

Um die Menüs und Menüelemente, die von der Routine, die oben erstellte zu entfernen, könnten Sie z. B. den folgenden Code verwenden:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

Bei den obigen Code werden die ersten vier Menüelemente in Xcode Interface Builder und Zusammenfassung der Punkte in der app verfügbar erstellt, daher nicht dynamisch entfernt werden.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Kontextmenüs

Kontextbezogene Menüs angezeigt werden, wenn der Benutzer mit der rechten Maustaste oder ein Element in einem Fenster Steuerelement klickt. Standardmäßig verfügen einige Elemente der Benutzeroberfläche, die bereits in MacOS erstellt Kontextmenüs, die angefügt werden (z. B. die Textansicht). Möglicherweise gibt es jedoch vorkommen, dass wir unseren eigenen benutzerdefinierten Kontextmenüs für ein Element der Benutzeroberfläche zu erstellen, die wir in einem Fenster hinzugefügt haben möchten.

Ermöglicht das Bearbeiten von unserer **"Main.Storyboard"** -Datei in Xcode, und fügen eine **Fenster** legen Sie Fenster aus, um unser Entwurf, die **Klasse** auf "NSPanel" in der **Identitätsinspektor**, fügen Sie einen neuen **-Assistenten** Element für die **Fenster** im Menü, und fügen Sie es an das neue unter Verwendung einer **Segue anzeigen**:

[![Segue-Typ](menu-images/context01.png "Segue-Typ")](menu-images/context01-large.png#lightbox)

Lassen Sie uns wie folgt vor:

1. Ziehen Sie eine **Bezeichnung** aus der **Bibliotheksinspektor** auf die **Bereich** Fenster, und legen Sie den Text auf "Property": 

    [![Bearbeiten der Bezeichnung Werts](menu-images/context03.png "Wert mit der Bezeichnung bearbeiten")](menu-images/context03-large.png#lightbox)
2. Ziehen Sie als Nächstes eine **Menü** aus der **Bibliotheksinspektor** auf der View-Controller in der Hierarchie von Inhaltsansichten und benennen Sie die drei Standard-Menüelemente **Dokument**, **Text**  und **Schriftart**:

    [![Die erforderlichen Menüelemente](menu-images/context02.png "Menüelemente erforderlich")](menu-images/context02-large.png#lightbox)
3. Jetzt Steuerelement ziehen Sie aus der **Eigenschaftenbezeichnung** auf die **Menü**:

    [![Ziehen Sie zum Erstellen eines segues](menu-images/context04.png "Ziehen zum Erstellen eines segues")](menu-images/context04-large.png#lightbox)
4. Wählen Sie im Popupdialogfeld **Menü**: 

    ![Segue-Typ](menu-images/context05.png "Segue-Typ")
5. Von der **Identitätsinspektor**, legen Sie den Ansichtscontroller-Klasse, um "PanelViewController": 

    [![Festlegen der Segue-Klasse](menu-images/context10.png "Festlegen der Segue-Klasse")](menu-images/context10-large.png#lightbox)
6. Wechseln Sie zurück zu Visual Studio für Mac, um zu synchronisieren und dann auf Interface Builder zurückgeben.
7. Wechseln Sie zu der **Assistant Editor** , und wählen Sie die **PanelViewController.h** Datei.
8. Erstellen Sie eine Aktion für die **Dokument** Menüelement namens `propertyDocument`: 

    [![Konfigurieren die Aktion](menu-images/context06.png "Konfigurieren der Aktion")](menu-images/context06-large.png#lightbox)
9. Wiederholen Sie erstellen Aktionen für die verbleibenden Menüelemente ein: 

    [![Die erforderlichen Aktionen](menu-images/context07.png "die erforderlichen Aktionen")](menu-images/context07-large.png#lightbox)
10. Schließlich erstellen ein outlets für die **Eigenschaftenbezeichnung** namens `propertyLabel`: 

    [![Konfigurieren den Ausgang](menu-images/context08.png "Outlet konfigurieren")](menu-images/context08-large.png#lightbox)
11. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Bearbeiten der **PanelViewController.cs** Datei, und fügen Sie den folgenden Code hinzu:

```csharp
partial void propertyDocument (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Document";
}

partial void propertyFont (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Font";
}

partial void propertyText (Foundation.NSObject sender) {
    propertyLabel.StringValue = "Text";
}
```

Wenn wir die Anwendung auszuführen und mit der rechten auf die Beschriftung der Eigenschaft im Bereich Maustaste, wird wir jetzt unsere benutzerdefinierte über das Kontextmenü angezeigt. Wenn wir aus dem Menü Element auswählen, ändert sich der Wert mit der Bezeichnung:

![Im Kontextmenü ausführen](menu-images/context09.png "im Kontextmenü auf, die ausgeführt wird")

Nächste sehen wir uns die Status-Leiste Menüs erstellen.

## <a name="status-bar-menus"></a>Status-Leiste Menüs

Status Leiste Menüs angezeigt, eine Sammlung von Status-Menüelemente, die Interaktion mit bereitstellen oder Feedback der Benutzer, z. B. ein Menü oder ein Bild, das den Zustand einer Anwendung widerspiegelt. Statusleiste und einer Anwendung ist aktiviert und aktiv, selbst wenn die Anwendung im Hintergrund ausgeführt wird. Die systemweite Statusleiste befindet sich rechts neben der Menüleiste und ist der einzige Statusleiste, die derzeit in MacOS verfügbar.

Ermöglicht das Bearbeiten von unserem **Datei "appdelegate.cs"** Datei, und stellen die `DidFinishLaunching` Methode sehen wie folgt:

```csharp
public override void DidFinishLaunching (NSNotification notification)
{
    // Create a status bar menu
    NSStatusBar statusBar = NSStatusBar.SystemStatusBar;

    var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);
    item.Title = "Text";
    item.HighlightMode = true;
    item.Menu = new NSMenu ("Text");

    var address = new NSMenuItem ("Address");
    address.Activated += (sender, e) => {
        PhraseAddress(address);
    };
    item.Menu.AddItem (address);

    var date = new NSMenuItem ("Date");
    date.Activated += (sender, e) => {
        PhraseDate(date);
    };
    item.Menu.AddItem (date);

    var greeting = new NSMenuItem ("Greeting");
    greeting.Activated += (sender, e) => {
        PhraseGreeting(greeting);
    };
    item.Menu.AddItem (greeting);

    var signature = new NSMenuItem ("Signature");
    signature.Activated += (sender, e) => {
        PhraseSignature(signature);
    };
    item.Menu.AddItem (signature);
}
```

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` erhalten wir Zugriff auf die Leiste des systemweiten Status. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` erstellt ein neues Element des Status-Leiste. Von dort aus erstellen wir ein Menü und eine Anzahl von Menüelementen und fügen Sie im Menü auf das Element der Status-Leiste, die, das wir gerade erstellt haben. 

Wenn wir die Anwendung ausführen, wird das neue Element des Status-Leiste angezeigt. Auswählen eines Elements aus dem Menü ändert sich den Text in der Textansicht an: 

![Die Statusleiste ausführen](menu-images/statusbar01.png "die Statusleiste, die ausgeführt wird")

Als Nächstes sehen wir uns über das Erstellen von benutzerdefinierten Dock Menüelemente.

## <a name="custom-dock-menus"></a>Benutzerdefinierte Dock-Menüs

Wenn der Benutzer klickt oder der Anwendung-Symbol im Dock-Steuerelement klickt, wird für Sie Macintosh-Anwendung des andockmenüs angezeigt:

![Eine benutzerdefinierte Andocken Menü](menu-images/dock01.png "ein benutzerdefiniertes Andocken Menü")

Erstellen Sie eine benutzerdefinierte andockmenü für unsere Anwendung nun wie folgt:

1. Visual Studio für Mac, mit der Maustaste auf das Projekt, und wählen die Anwendung **hinzufügen** > **neue Datei...** Wählen Sie in einem Dialogfeld für die neue Datei **Xamarin.Mac** > **leere Schnittstellendefinition**, verwenden Sie "DockMenu" für die **Namen** , und klicken Sie auf die **neu**  Schaltfläche zum Erstellen des neuen **DockMenu.xib** Datei:

    ![Hinzufügen eine leere Schnittstellendefinition](menu-images/dock02.png "eine leere Schnittstellendefinition hinzufügen")
2. In der **Lösungspad**, doppelklicken Sie auf die **DockMenu.xib** Datei, die sie zur Bearbeitung in Xcode geöffnet. Erstellen Sie ein neues **Menü** mit den folgenden Elementen: **Adresse**, **Datum**, **Begrüßung**, und **Signatur** 

    [![Anordnen von der Benutzeroberflächenautomatisierungs](menu-images/dock03.png "Anordnung der Benutzeroberflächenautomatisierungs")](menu-images/dock03-large.png#lightbox)
3. Als Nächstes verbinden wir unsere neue Menüelemente für unsere vorhandenen Aktionen, die wir für unsere benutzerdefinierten Menüs in erstellt haben die [hinzufügen, bearbeiten und Löschen von Menüs](#Adding,_Editing_and_Deleting_Menus) weiter oben. Wechseln Sie zu der **Verbindung Inspektor** , und wählen Sie die **erste Beantworter** in die **Schnittstellenhierarchie**. Scrollen Sie nach unten, und suchen die `phraseAddress:` Aktion. Ziehen Sie eine Linie von der Kreis für diese Aktion die **Adresse** Menüelements:

    [![Verknüpfen einer Aktion ziehen](menu-images/dock04.png "Verknüpfen einer Aktion ziehen")](menu-images/dock04-large.png#lightbox)
4. Wiederholen Sie für alle anderen Menüelemente anfügen an die entsprechenden Aktionen aus: 

    [![Die erforderlichen Aktionen](menu-images/dock05.png "die erforderlichen Aktionen")](menu-images/dock05-large.png#lightbox)
5. Wählen Sie als Nächstes die **Anwendung** in die **Schnittstellenhierarchie**. In der **Verbindung Inspektor**, ziehen Sie eine Zeile aus den Kreis der `dockMenu` Outlet auf das Menü, die wir gerade erstellt haben:

    [![Ziehen das Verknüpfen des outlets](menu-images/dock06.png "ziehen das Verknüpfen des outlets")](menu-images/dock06-large.png#lightbox)
6. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisiert.
7. Doppelklicken Sie auf die **"Info.plist"** Datei, die sie für die Bearbeitung zu öffnen: 

    [![Bearbeiten der „Info.plist“-Datei](menu-images/dock07.png "Editing the Info.plist file")](menu-images/dock07-large.png#lightbox)
8. Klicken Sie auf die **Quelle** Registerkarte am unteren Rand des Bildschirms: 

    [![Wählen die Datenquellensicht](menu-images/dock08.png "die Datenquellensicht auswählen")](menu-images/dock08-large.png#lightbox)
9. Klicken Sie auf **neuen Eintrag hinzufügen**, klicken Sie auf die grüne Schaltfläche mit dem Pluszeichen und legen Sie den Namen der Eigenschaft auf "AppleDockMenu" und der Wert, der "DockMenu" (der Name des der neuen XIB-Datei ohne Erweiterung): 

    [![Hinzufügen des Elements DockMenu](menu-images/dock09.png "Hinzufügen des Elements DockMenu")](menu-images/dock09-large.png#lightbox)

Wenn wir die Anwendung auszuführen und mit der rechten auf das zugehörige Symbol im Dock Maustaste, werden jetzt unsere neue Menüelemente angezeigt:

![Ein Beispiel für die andockmenü Ausführung](menu-images/dock10.png "ein Beispiel des andockmenüs ausgeführt wird")

Wenn wir eine benutzerdefinierte Elemente im Menü auswählen, wird der Text in unseren Textansicht geändert werden.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Popup-Schaltfläche und Dropdownmenü-Listen

Eine Popup-Schaltfläche ein ausgewählten Elements angezeigt und zeigt eine Liste der Optionen aus, wenn der Benutzer darauf klickt. Eine Drop-Down-Liste ist ein Popup-Schaltfläche in der Regel wird verwendet, um Befehle, die spezifisch für den Kontext der aktuellen Aufgabe. Beide können an einer beliebigen Stelle in einem Fenster angezeigt.

Erstellen Sie eine benutzerdefinierte Popup-Schaltfläche für die Anwendung nun wie folgt:

1. Bearbeiten der **"Main.Storyboard"** -Datei in Xcode, und ziehen Sie eine **Popup-Schaltfläche** aus der **Bibliotheksinspektor** auf die **Bereich** wir erstellt, im haben Fenster die [Kontextmenüs](#Contextual_Menus) Abschnitt: 

    [![Hinzufügen einer Schaltfläche Popup](menu-images/popup01.png "eine Popup-Schaltfläche hinzufügen")](menu-images/popup01-large.png#lightbox)
2. Fügen Sie ein neues Menüelement hinzu, und legen Sie den Titel der Elemente in das Popup auf: **Adresse**, **Datum**, **Begrüßung**, und **Signatur** 

    [![Konfigurieren die Menüelemente](menu-images/popup02.png "Menüelemente konfigurieren")](menu-images/popup02-large.png#lightbox)
3. Als Nächstes verbinden wir unsere neue Menüelemente, die vorhandenen Aktionen, die wir für unsere benutzerdefinierten Menüs in erstellt die [hinzufügen, bearbeiten und Löschen von Menüs](#Adding,_Editing_and_Deleting_Menus) weiter oben. Wechseln Sie zu der **Verbindung Inspektor** , und wählen Sie die **erste Beantworter** in die **Schnittstellenhierarchie**. Scrollen Sie nach unten, und suchen die `phraseAddress:` Aktion. Ziehen Sie eine Linie von der Kreis für diese Aktion die **Adresse** Menüelements: 

    [![Verknüpfen einer Aktion ziehen](menu-images/popup03.png "Verknüpfen einer Aktion ziehen")](menu-images/popup03-large.png#lightbox)
4. Wiederholen Sie für alle anderen Menüelemente anfügen an die entsprechenden Aktionen aus: 

    [![Alle erforderlichen Aktionen](menu-images/popup04.png "alle erforderlichen Aktionen")](menu-images/popup04-large.png#lightbox)
5. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Wenn wir die Anwendung auszuführen, und wählen Sie ein Element aus das Popup, wird jetzt der Text in unseren Textansicht ändern:

![Ein Beispiel für das Popupfenster mit](menu-images/popup05.png "ein Beispiel für das Popup ausgeführt wird")

Sie können erstellen und Verwenden von Pull-Dropdownlisten im genau die gleiche Weise als Popup Schaltflächen. Sie können anstatt durch Anfügen an vorhandene Aktion, Ihre eigenen benutzerdefinierten Aktionen erstellen, wie wir für unsere über das Kontextmenü in der [Kontextmenüs](#Contextual_Menus) Abschnitt.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat es sich um einen detaillierten Einblick in die Arbeit mit Menüs und Menüelemente in einer Xamarin.Mac-Anwendung geführt. Wir untersucht zunächst Menüleiste der Anwendung, und wir haben uns über das Erstellen von Kontextmenüs, die als Nächstes wir Status Leiste Menüs untersuchten und Andocken von benutzerdefinierten Menüs. Abschließend werden wir Popupmenüs und Drop-Down-Listen.


## <a name="related-links"></a>Verwandte Links

- [MacMenus (Beispiel)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Human Interface Guidelines - Menüs](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Einführung in die Anwendungsmenüs und Popup-Listen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
