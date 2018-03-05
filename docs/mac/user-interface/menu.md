---
title: "Menüs"
description: "In diesem Artikel wird das Arbeiten mit Menüs in einer Anwendung Xamarin.Mac behandelt. Erstellen und Verwalten von Menüs und Menüelemente in Xcode und Benutzeroberflächen-Generator und Arbeiten mit diesen programmgesteuerten beschrieben."
ms.topic: article
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: cf43cfe31811e91524af7894ea347e3dba784d92
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/28/2018
---
# <a name="menus"></a>Menüs

_In diesem Artikel wird das Arbeiten mit Menüs in einer Anwendung Xamarin.Mac behandelt. Erstellen und Verwalten von Menüs und Menüelemente in Xcode und Benutzeroberflächen-Generator und Arbeiten mit diesen programmgesteuerten beschrieben._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleichen Kakao-Menüs, die ein Entwickler arbeiten in Objective-C und Xcode ausführt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie Xcodes Benutzeroberflächen-Generator zu erstellen und Verwalten Ihrer Menüleisten, Menüs und Menüelemente (oder erstellen sie optional direkt im C#-Code) verwenden.

Menüs sind ein wesentlicher Bestandteil der Benutzeroberfläche für einen Macintosh-Anwendung und häufig in verschiedene Teile der Benutzeroberfläche angezeigt werden:

- **Die Anwendung Menüleiste** -Dies ist das Hauptmenü, die am oberen Rand des Bildschirms für jede Mac-Anwendung angezeigt wird.
- **Kontextmenüs** – diese werden angezeigt, wenn der Benutzer klickt oder ein Element in einem Fenster Steuerelement klickt.
- **Die Statusleiste** -Dies ist der Bereich am ganz rechts auf der Menüleiste Anwendung, die am oberen Bildschirmrand (auf der linken Seite der Uhr Balken im Menü) angezeigt wird und auf der linken Seite vergrößert wird, wenn Elemente hinzugefügt werden.
- **Andocken von Menü** -klicken Sie im Menü für jede Anwendung in das Andocken, die angezeigt, wenn der Benutzer klickt oder das Anwendungssymbol Steuerelement klickt oder wenn der Benutzer das Symbol klicken und die Maustaste gedrückt hält.
- **Popup-Schaltfläche und Pull-Dropdownlisten** -Popup-Schaltfläche zeigt ein ausgewähltes Element und zeigt eine Liste mit Optionen zur Auswahl aus, wenn der Benutzer darauf klickt. Eine Drop-Down-Liste ist ein Popup-Schaltfläche in der Regel wird verwendet, um Befehle, die nur für den Kontext des aktuellen Task. Beide können an einer beliebigen Stelle in einem Fenster angezeigt.

[![Ein Beispiel-Menü](menu-images/intro01.png "eine Beispiel-Menü")](menu-images/intro01-large.png)

In diesem Artikel werden die Grundlagen der Arbeit mit Kakao Menüleisten, Menüs und Menüelemente in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Attribute verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

## <a name="the-applications-menu-bar"></a>Menüleiste der Anwendung 

Im Gegensatz zu Anwendungen, die auf, wobei jedes Fenster eine eigene Menüleiste angefügt haben, kann, das Windows-Betriebssystem ausgeführt wird, hat jede Anwendung MacOS unter einer einzelnen Menüleiste, die entlang des oberen Rand des Bildschirms ausgeführt wird, die für jedes Fenster in der Anwendung verwendet wird:

[![Eine Menüleiste](menu-images/appmenu01.png "einer Menüleiste")](menu-images/appmenu01-large.png)

Elemente in dieser Menüleiste aktiviert oder deaktiviert werden basierend auf den aktuellen Kontext oder den Status der Anwendung und der Benutzeroberfläche eines beliebigen Moments. Z. B.: Wenn der Benutzer ein Textfeld auswählt, Elemente, auf die **bearbeiten** Menü aktiviert z. B. stammen **Kopie** und **Ausschneiden**.

Gemäß den Apple und in der Standardeinstellung haben alle MacOS Anwendungen einen Standardsatz von Menüs und Menüelemente, die in der Menüleiste der Anwendung angezeigt werden:

- **Apple-Menü** -dieses Menü bietet Zugriff auf System wide Elemente, die für den Benutzer verfügbar sind, immer, unabhängig davon, welche Anwendung ausgeführt wird. Diese Elemente können nicht vom Entwickler geändert werden.
- **App-Menü** -dieses Menü zeigt den Namen der Anwendung in Fettschrift angezeigt und hilft dem Benutzer zu identifizieren, welche Anwendung derzeit ausgeführt wird. Es enthält Elemente, die für die Anwendung als Ganzes und kein bestimmtes Dokument bzw. Vorgang wie z. B. das Beenden der Anwendung gelten.
- **Menü "Datei"** - Elemente verwendet, um zu erstellen, öffnen oder Speichern Dokumente, die Ihre Anwendung ausgeführt wird. Wenn die Anwendung kein Dokument-basiert ist, kann dieses Menü umbenannt oder entfernt werden.
- **Menü "Bearbeiten"** -Befehle wie z. B. enthält **Ausschneiden**, **Kopie**, und **einfügen** die zum Bearbeiten oder Ändern von Elementen in der Benutzeroberfläche der Anwendung verwendet werden.
- **Menü ' Format '** – Wenn die Anwendung mit Text dieses Menü enthält Befehle zum Anpassen der Formatierung von Text funktioniert.
- **Menü "Ansicht"** -enthält Befehle, die Auswirkungen auf wie der Inhalt angezeigt wird, die in der Benutzeroberfläche der Anwendung (angezeigt).
- **Anwendungsspezifische Menüs** -Hierbei handelt es sich um eine beliebige Menüs, die für Ihre Anwendung (z. B. ein Lesezeichenmenü für ein Webbrowser) spezifisch sind. Zwar zwischen den **Ansicht** und **Fenster** Menüs auf der Menüleiste angezeigt.
- **Menü "Fenster"** -enthält Befehle zum Arbeiten mit Fenstern in der Anwendung als auch eine Liste der aktuellen geöffneten Fenstern.
- **Menü "Hilfe"** – Wenn Ihre Anwendung auf dem Bildschirm Hilfe bereitstellt, muss im Hilfemenü die Menüs ganz rechts auf der Menüleiste. 

Weitere Informationen zur Menüleiste der Anwendung und standard-Menüs und Menüelemente finden Sie im Apple [menschlichen Richtlinien zur Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/).

### <a name="the-default-application-menu-bar"></a>Die Menüleiste des Standard-Anwendung

Wenn Sie ein neues Xamarin.Mac-Projekt erstellen, erhalten Sie automatisch eine standard-Anwendung Standardmenüleiste, die die typischen Elemente verfügt, die normalerweise eine MacOS-Anwendung (wie im vorherigen Abschnitt erläutert) hätte. Standardmenüleiste für Ihre Anwendung wird definiert, der **Main.storyboard** Datei (zusammen mit der Rest der Benutzeroberfläche der Anwendung) unter dem Projekt die **Lösung Pad**:  

![Wählen Sie die Haupt-Storyboard](menu-images/appmenu02.png "wählen Sie die Haupt-Storyboard")

Doppelklicken Sie auf die **Main.storyboard** Datei, um ihn zu öffnen, für die Bearbeitung in Xcodes Benutzeroberflächen-Generator, und Sie mit der Menü-Editor-Benutzeroberfläche angezeigt:

[![Bearbeiten die Benutzeroberfläche in Xcode](menu-images/defaultbar01.png "bearbeiten die Benutzeroberfläche in Xcode")](menu-images/defaultbar01-large.png)

Von hier aus können klicken auf Elemente wie z. B. die **öffnen** Menüelements in der **Datei** im Menü und bearbeiten, oder passen Sie die entsprechenden Eigenschaften in der **Attribute Inspektor**:

[![Bearbeiten eines Menüs Attribute](menu-images/defaultbar02.png "ein Menü Attribute bearbeiten")](menu-images/defaultbar02-large.png)

Wir kommen in hinzufügen, bearbeiten und Löschen von Menüs und Elementen weiter unten in diesem Artikel. Für jetzt möchten wir sehen, welche Menüs und Menüelemente standardmäßig verfügbar und wie sie automatisch zum Code über eine Reihe von vordefinierten Steckdosen und Aktionen verfügbar gemacht wurden (Weitere Informationen finden Sie in unserer [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Dokumentation).

Angenommen, wir klicken Sie auf die **Verbindung Inspektor** für die **öffnen** Menüelement ersichtlich automatisch bis zu wired die `openDocument:` Aktion: 

[![Anzeigen der angefügten Aktion](menu-images/defaultbar03.png "angefügten Aktion anzeigen")](menu-images/defaultbar03-large.png)

Bei Auswahl der **erste Beantworter** in der **Schnittstellenhierarchie** und einen Bildlauf nach unten der **Verbindung Inspektor**, und sehen Sie die Definition der `openDocument:` Aktion, die die **öffnen** Menüelement (zusammen mit mehreren anderen Standardaktionen für die Anwendung, die werden und werden nicht automatisch angeschlossen an Steuerelemente) angefügt ist:

[![Anzeigen von allen angefügten Aktionen](menu-images/defaultbar04.png "alle angefügten Aktionen anzeigen")](menu-images/defaultbar04-large.png) 

Warum ist das wichtig? In den nächsten sehen Abschnitt Funktionsweise mit anderen Kakao Benutzeroberflächenelemente automatisch aktivieren und Deaktivieren von Menüelementen, als auch, bieten integrierte Funktionen für die Elemente von diese Aktionen automatisch definiert.

Später werden wir verwenden diese integrierten Aktionen aktivieren und deaktivieren Elemente aus dem Code und unsere eigene Funktionen bereitstellen, bei der Auswahl.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Integrierte Menü-Funktion

Wenn Sie die Ausführung einer neu erstellten Xamarin.Mac Anwendung vor dem UI-Elemente oder Code hinzugefügt wurden, werden Sie bemerken, dass einige Elemente werden automatisch wired-nach-oben und für Sie (mit vollständig Funktionalität automatisch integrierte) werden, z. B. aktiviert die **Quit** Element in der **App** Menü:

![Eine aktivierte Menüelement](menu-images/appmenu03.png "ein Menüelement aktiviert")

Während andere Menüelemente, z. B. **Ausschneiden**, **Kopie**, und **einfügen** sind nicht:

![Deaktiviert die Menüelemente](menu-images/appmenu04.png "Menüelemente deaktiviert")

Wir die Anwendung beenden, und doppelklicken Sie auf die **Main.storyboard** in der Datei die **Lösung Pad** zur Bearbeitung in Xcode Öffnen des Benutzeroberflächen-Generator. Ziehen Sie anschließend eine **Textansicht** aus der **Bibliothek** auf modellansichtcontroller in das Fenster der **Benutzeroberflächen-Editors**:

[![Auswählen einer Textansicht aus der Bibliothek](menu-images/appmenu05.png "eine Textansicht aus der Bibliothek auswählen")](menu-images/appmenu05-large.png)

In der **Einschränkung Editor** wir anheften Textansicht an das Fenster Rändern und legen Sie sie, wo es vergrößert und verkleinert wird mit dem Fenster alle vier rote I-Balken in den oberen Rand des Editors klicken und auf, die **hinzufügen 4 Einschränkungen** Schaltfläche:

[![Bearbeiten die Einschränkung](menu-images/appmenu06.png "die Einschränkung bearbeiten")](menu-images/appmenu06-large.png)

Speichern Sie die Änderungen an der Entwurf der Benutzeroberfläche, und wechseln Sie wieder die Visual Studio für Mac, in der Änderungen in Ihrem Projekt Xamarin.Mac zu synchronisieren. Jetzt starten Sie die Anwendung, geben Sie Text in der Textansicht, wählen Sie es und öffnen Sie die **bearbeiten** Menü:

![Die Menüelemente werden automatisch aktiviert oder deaktiviert](menu-images/appmenu07.png "Menüelemente werden automatisch aktiviert oder deaktiviert")

Beachten Sie, dass wie die **Ausschneiden**, **Kopie**, und **einfügen** Elemente werden automatisch aktiviert und voll funktional, ohne eine einzige Codezeile schreiben. 

Was ist hier los? Beachten Sie die integrierte vordefinieren, Aktionen, die bis zu den Standard-Menüelemente kabelgebundenen stammen (wie oben dargestellt), die meisten Kakao Benutzeroberflächenelemente, die Teil der MacOS sind in Hooks für bestimmte Aktionen erstellt haben (z. B. `copy:`). Also wenn sie ein Fenster aktiv ist, hinzugefügt und ausgewählt, das entsprechende Menüelement oder die Elemente, die angefügt werden mit dieser Aktion werden automatisch aktiviert. Wenn der Benutzer das Menüelement auswählt, wird die Funktionalität integriert das Benutzeroberflächenelement aufgerufen und ausgeführt werden, ohne Eingriffe.

### <a name="enabling-and-disabling-menus-and-items"></a>Aktivieren und Deaktivieren von Menüs und Elemente

Standardmäßig wird jedes Mal, wenn ein Ereignis auftritt `NSMenu` automatisch aktiviert und deaktiviert die einzelnen sichtbar und Menü Element auf Grundlage des Kontexts der Anwendung. Es gibt drei Möglichkeiten zum Aktivieren/Deaktivieren Sie ein Element aus:

- **Aktivieren von Menüs, automatisch** -ein Menüelement ist aktiviert, wenn `NSMenu` können Suchen eines entsprechenden Objekts, das auf die Aktion reagiert, die das Element wired-zum durchführen wird. Beispielsweise Textansicht oben, die einen integrierten Hook, mussten die `copy:` Aktion.
- **Benutzerdefinierte Aktionen und ValidateMenuItem:** – Geben Sie für jedes Menüelement, das gebunden ist ein [Fenster oder Ansicht benutzerdefinierte Controlleraktion](#Working-with-Custom-Window-Actions), können Sie hinzufügen, die `validateMenuItem:` Aktion manuell aktivieren oder Deaktivieren von Menüelementen.
- **Manuelle Menü aktivieren** -Sie manuell festlegen, die `Enabled` -Eigenschaft jedes `NSMenuItem` aktivieren oder deaktivieren jedes Element in einem Menü einzeln.

Zum Auswählen eines Systems legen die `AutoEnablesItems` Eigenschaft von einem `NSMenu`. `true` automatisch (das Standardverhalten) und `false` ist "manuell". 

> [!IMPORTANT]
> Wenn Sie entscheiden, verwenden die manuelle Menü zu aktivieren, keine des Menüs Elemente, auch für diejenigen, die von AppKit Klassen wie gesteuert `NSTextView`, werden automatisch aktualisiert. Sie sind für aktivieren und deaktivieren alle Elemente in den Code manuell zuständig.

#### <a name="using-validatemenuitem"></a>Verwenden von validateMenuItem

Wie oben für jedes Menüelement im angegeben, die gebunden ist ein [Fenster oder Ansicht benutzerdefinierte Controlleraktion](#Working-with-Custom-Window-Actions), können Sie hinzufügen, die `validateMenuItem:` Aktion manuell aktivieren oder Deaktivieren von Menüelementen.

Im folgenden Beispiel die `Tag` Eigenschaft wird verwendet, um den Typ des Menüelements entscheiden, die aktiviert oder durch deaktiviert wird werden die `validateMenuItem:` Aktion basierend auf den Zustand des markierten Texts in einem `NSTextView`. Die `Tag` Eigenschaft im Benutzeroberflächen-Generator für jedes Menüelement festgelegt wurde:

![Festlegen der Tag-Eigenschaft](menu-images/validate01.png "Festlegen der Tag-Eigenschaft")

Sowie den folgenden Code, der die View-Controller hinzugefügt:

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

Wenn dieser Code ausgeführt wird und kein Text ausgewählt ist, der `NSTextView`, die zwei Wrap Menüelemente sind deaktiviert, (obwohl sie mit Aktionen auf dem View-Controller verbunden sind):

![Deaktiviert die Anzeige von Elementen](menu-images/validate02.png "zeigt Deaktivierte Elemente")

Wenn ein Abschnitt des Texts ausgewählt ist, und klicken Sie im Menü erneut geöffnet, werden die Menüelemente zwei Wrap verfügbar:

![Aktiviert die Anzeige von Elementen](menu-images/validate03.png "anzeigen aktiviert, Elemente")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Aktivieren von und reagieren auf Menüelemente im code

Wie oben gesehen haben, indem Sie einfach unser Design der Benutzeroberfläche (z. B. ein Textfeld) bestimmte Kakao Benutzeroberflächenelemente hinzugefügt, die mehrere Menüelemente standardmäßig aktiviert werden sollen und Funktion automatisch, ohne Code schreiben zu müssen. Nächste betrachten wir unser eigenes C#-Code hinzufügen, mit unserem Xamarin.Mac-Projekt, um ein Menüelement zu aktivieren, und stellen Funktionalität bereit, wenn der Benutzer sie auswählt.

Beispielsweise ermöglichen sagen, wir möchten, dass den Benutzer zu verwenden, können, die **öffnen** Element in der **Datei** Menü, um einen Ordner auszuwählen. Da wir möchten diese Option, um eine anwendungsweite-Funktion werden und nicht auf eine erteilen Fenster oder ein Benutzeroberflächenelement beschränkt ist, werden wir den Code zum Behandeln dieses Delegaten Anwendung hinzufügen.

In der **Lösung Pad**, doppelklicken Sie auf die `AppDelegate.CS` Datei zur Bearbeitung zu öffnen:

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

Wir führen Sie nun die Anwendung, und öffnen Sie die **Datei** Menü: 

![Das Dateimenü](menu-images/appmenu09.png "der Datei (Menü)")

Beachten Sie, dass die **öffnen** Menüelement ist jetzt aktiviert. Wenn wir diese Option auswählen, wird das Dialogfeld "Öffnen" angezeigt:

![Ein Dialogfeld "Öffnen"](menu-images/appmenu10.png "ein Dialogfeld "Öffnen"")

Wenn wir auf die **öffnen** Schaltfläche unsere Warnmeldung wird angezeigt:

![Eine Beispiel-Dialog-Nachricht](menu-images/appmenu11.png "eine Beispiel-Dialog-Nachricht")

Die wichtigsten wurde `[Export ("openDocument:")]`, teilt `NSMenu` , unsere **AppDelegate** verfügt über eine Methode `void OpenDialog (NSObject sender)` , die beantwortet die `openDocument:` Aktion. Wenn Sie von oben merkenden der **öffnen** Menüelement wird automatisch wired-bis zu dieser Aktion standardmäßig im Benutzeroberflächen-Generator:

[![Die angefügten Aktionen anzeigen](menu-images/defaultbar03.png "angefügten Aktionen anzeigen")](menu-images/defaultbar03-large.png)

Nächste betrachten wir unser eigenes Menü Menüelemente und Aktionen erstellen und Antworten im Code.

### <a name="working-with-the-open-recent-menu"></a>Arbeiten mit dem aktuellen Menü öffnen

Wird standardmäßig die **Datei** Menü enthält eine **öffnen aktuelle** Element, das verfolgt den letzten mehrere Dateien, die der Benutzer mit der app geöffnet hat. Wenn Sie erstellen eine `NSDocument` Xamarin.Mac app basierend dieses Menü wird automatisch für Sie verarbeitet werden. Für alle anderen Arten von Xamarin.Mac app werden Sie verantwortlich für das Verwalten von und reagieren auf dieses Menüelement manuell.

Manuell behandeln die **öffnen aktuelle** Menü zunächst müssen Sie ihn informieren, dass eine neue Datei geöffnet oder gespeichert, mit der folgenden wurde:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Obwohl Ihre app nicht verwendet `NSDocuments`, Sie weiterhin verwenden die `NSDocumentController` Verwalten der **zuletzt verwendete öffnen** Menü durch Senden einer `NSUrl` durch den Speicherort der Datei, die die `NoteNewRecentDocumentURL` Methode der `SharedDocumentController`.

Als Nächstes müssen Sie überschreiben die `OpenFile` Methode von der app-Delegat, der eine beliebige Datei zu öffnen, die der Benutzer auswählt der **letzte** Menü. Zum Beispiel:

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

Zurückgeben `true` , wenn die Datei geöffnet werden kann, andernfalls zurückgeben `false` und eine integrierte Warnung wird angezeigt, die dem Benutzer, dass die Datei konnte nicht geöffnet werden.

Da von den Dateinamen und Pfad zurückgegeben der **zuletzt verwendete öffnen** Menü kann ein Leerzeichen enthalten, müssen wir diese Zeichen mit Escapezeichen vor dem Erstellen einer `NSUrl` oder erhalten wir einen Fehler. Wir führen, die durch den folgenden Code:

```csharp
filename = filename.Replace (" ", "%20");
```

Schließlich erstellen wir eine `NSUrl` , der auf die Datei und verwenden eine Hilfsmethode in der app an ein neues Fenster öffnen, und Laden Sie die Datei hinein delegieren verweist:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Um alles zusammen per Pull abzurufen, werfen wir einen Blick auf eine beispielimplementierung in einer **AppDelegate.cs** Datei:

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

Basierend auf den Anforderungen der app, empfiehlt nicht den Benutzer die gleiche Datei in mehrere Fenster gleichzeitig geöffnet. In unserem Beispiel-app, wenn der Benutzer eine Datei auswählt, die bereits geöffnet ist (entweder über die **letzte** oder **öffnen...** Menüelemente), wird das Fenster mit der Datei in den Vordergrund geholt.

Um dies zu erreichen, verwendet haben wir den folgenden Code in unserer Hilfsmethode:

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

Wir entworfen unsere `ViewController` Klasse zum Halten des Pfads zur Datei in die `Path` Eigenschaft. Als Nächstes durchlaufen wir alle derzeit geöffneten Fenstern in der app ein. Wenn die Datei in eines der Fenster bereits geöffnet ist, wird es in der Vorderseite des anderen Fenstern, die mit versetzt:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Wenn keine Übereinstimmung gefunden wird, ein neues Fenster, mit der Datei geladen geöffnet wird und die Datei, in aufgeführt wird der **öffnen aktuelle** Menü:

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

### <a name="working-with-custom-window-actions"></a>Arbeiten mit Fenster für benutzerdefinierte Aktionen

Ebenso wie die integrierte **erste Beantworter** Aktionen, die auf Standardmenüelementen verkabelt stammen, können Sie erstellen Sie eine neue, benutzerdefinierte Aktionen und verknüpfen sie auf die Menüelemente im Benutzeroberflächen-Generator.

Definieren Sie zuerst eine benutzerdefinierte Aktion auf einer der Controller für die app-Fenster ein. Zum Beispiel:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Doppelklicken Sie anschließend auf der app-Storyboard-Datei in die **Lösung Pad** zur Bearbeitung in Xcode Öffnen des Benutzeroberflächen-Generator. Wählen Sie die **erste Beantworter** unter der **Szene Anwendung**, wechseln Sie zu der **Attribute Inspektor**:

![Der Inspektor Attribute](menu-images/action01.png "der Attribute-Inspektor")

Klicken Sie auf die  **+**  Schaltfläche am unteren Rand der **Attribute Inspektor** So fügen Sie eine neue benutzerdefinierte Aktion hinzu:

![Eine neue Aktion hinzufügen](menu-images/action02.png "eine neue Aktion hinzufügen")

Geben Sie ihm den gleichen Namen wie die benutzerdefinierte Aktion, die Sie auf dem Controller Fenster erstellt:

![Bearbeiten den Aktionsnamen](menu-images/action03.png "der Aktionsname bearbeiten")

Steuerelement klicken und ziehen Sie ein Menüelement zu dem **erste Beantworter** unter der **Szene Anwendung**. Wählen Sie aus der Popupliste aus, die neue Aktion, die Sie soeben erstellt haben (`defineKeyword:` in diesem Beispiel):

![Anfügen einer Aktion](menu-images/action04.png "Anfügen einer Aktion")

Speichern Sie die Änderungen auf das Storyboard und zurück zu Visual Studio für Mac Änderungen synchronisiert. Wenn Sie die app auszuführen, das Menüelement, dem Sie die benutzerdefinierte Aktion angeschlossen wird automatisch aktiviert/deaktiviert werden (basierend auf das Fenster mit der Aktion, die geöffnet wird), und dabei das Menüelement auswählen deaktiviert die Aktion ausgelöst wird:

[![Testen die neue Aktion](menu-images/action05.png "testen die neue Aktion")](menu-images/action05-large.png)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Hinzufügen, bearbeiten und Löschen von Menüs

Wie wir in den vorherigen Abschnitten gesehen haben, kommt eine Xamarin.Mac-Anwendung mit einer vordefinierten Anzahl von Standardmenüs und Menüelemente, die bestimmte UI-Steuerelementen automatisch zu aktivieren und zu reagieren. Wir haben auch gesehen, wie die Anwendung Code hinzugefügt, die ebenfalls zu aktivieren und die Antworten auf diese Standardelemente.

In diesem Abschnitt untersuchen wir auf Menüelemente, die wir nicht brauchen entfernen, durch das Neuorganisieren Menüs und Hinzufügen von neuen Menüs, Menüelemente und Aktionen.

Doppelklicken Sie auf die **Main.storyboard** in der Datei die **Lösung Pad** um ihn zur Bearbeitung zu öffnen:

[![Bearbeiten die Benutzeroberfläche in Xcode](menu-images/maint01.png "bearbeiten die Benutzeroberfläche in Xcode")](menu-images/maint01-large.png)

Für unsere Xamarin.Mac anwendungsspezifische nicht Kegel zum Verwenden der Standard **Ansicht** Menü wir also zu entfernen. In der **Schnittstellenhierarchie** wählen Sie die **Ansicht** Menüelement, das Teil der Hauptmenüleiste ist:

![Auswahl des Menüelements Ansicht](menu-images/maint02.png "dabei das Menüelement Ansicht auswählen")

Drücken Sie ENTF oder RÜCKTASTE, um das Menü zu löschen. Als Nächstes wird hier nicht alle Elemente in verwenden, werden die **Format** Menü- und wir die Elemente verschieben, werden wir aus den Untermenüs out verwenden, möchten. In der **Schnittstellenhierarchie** wählen Sie die folgenden Punkte:

![Mehrere Elemente markieren](menu-images/maint03.png "mehrere Elemente markieren")

Ziehen Sie die Elemente unter dem übergeordneten Element **Menü** im Untermenü, wo sie derzeit sind:

[![Ziehen zum übergeordneten Menü Menüelemente](menu-images/maint04.png "Menüelemente im übergeordneten Menü ziehen")](menu-images/maint04-large.png)

Das Menü sollte jetzt wie aussehen:

[![Die Elemente in den neuen Speicherort](menu-images/maint05.png "die Elemente in den neuen Speicherort")](menu-images/maint05-large.png)

Weiter Wir ziehen Sie die **Text** Untermenü, unter der **Format** Menü und platzieren Sie es auf der Hauptmenüleiste zwischen der **Format** und **Fenster** Menüs:

[![Klicken Sie im Menü Text](menu-images/maint06.png "die Text-Menü")](menu-images/maint06-large.png)

Wieder unter gehen wir die **Format** Menü- und löschen die **Schriftart** Untermenü-Element. Wählen Sie als Nächstes die **Format** Menü, und benennen sie "Schriftart":

[![Klicken Sie im Menü Schriftart](menu-images/maint07.png "Menü der Schriftart")](menu-images/maint07-large.png)

Als Nächstes erstellen wir der vordefinierten Zeichenfolgen, das automatisch wird auf den Text in der Textansicht angefügt abrufen bei der Auswahl ein benutzerdefiniertes Menüs. In das Suchfeld im unteren Bereich auf die **Bibliothek Inspektor** Typ im Menü """. Dies können sie einfacher zu finden und mit allen der im Menü von Elementen der Benutzeroberfläche arbeiten:

![Der Inspektor Bibliothek](menu-images/maint08.png "der Bibliothek Inspektor")

Jetzt führen wir Folgendes ein, um unsere ein Menü zu erstellen:

1. Ziehen Sie eine **Menüelement** aus der **Bibliothek Inspektor** auf der Menüleiste zwischen den **Text** und **Fenster** Menüs: 

    ![Wählen ein neues Menüelement in der Bibliothek](menu-images/maint10.png "ein neues Menüelement in der Bibliothek auswählen")
2. Benennen Sie das Element "Ausdrücke": 

    [![Festlegen der Menünamen](menu-images/maint09.png "Menünamen festlegen")](menu-images/maint09-large.png)
3. Ziehen Sie als Nächstes eine **Menü** aus der **Bibliothek Inspektor**: 

    ![Wählen ein Menü, aus der Bibliothek](menu-images/maint11.png "ein Menü aus der Bibliothek auswählen")
4. Anschließend können Sie **Menü** auf dem neuen **Menüelement** wir gerade erstellte und ändern Sie den Namen auf "Ausdrücke": 

    [![Bearbeiten die Menünamen](menu-images/maint12.png "bearbeiten den Namen des Menüs")](menu-images/maint12-large.png)
5. Nun wir benennen die drei standardskriptaktivitäten **Menüelemente** "Address", "Date" und "Greeting": 

    [![Klicken Sie im Menü Ausdrücke](menu-images/maint13.png "der Ausdrücke im Menü")](menu-images/maint13-large.png)
6. Fügen wir eine vierte **Menüelement** durch Ziehen einer **Menüelement** aus der **Bibliothek Inspektor** und Aufrufen dieser "Signatur": 

    [![Bearbeiten die Menüelementname](menu-images/maint14.png "der Elementname im Menü Bearbeiten")](menu-images/maint14-large.png)
7. Speichern der Änderungen in der Menüleiste.

Jetzt erstellen wir eine Reihe benutzerdefinierter Aktionen aus, sodass unsere neue Menüelemente C#-Code verfügbar gemacht werden. Nehmen wir in Xcode wechseln Sie zu der **Assistant** anzeigen:

[![Erstellen die erforderlichen Aktionen](menu-images/maint15.png "erstellen die erforderlichen Aktionen")](menu-images/maint15-large.png)

Führen Sie wir Folgendes:

1. Steuerelement ziehen Sie aus der **Adresse** Menüelement zu dem **AppDelegate.h** Datei.
2. Wechseln der **Verbindung** zu Typ **Aktion**: 

    [![Den Aktionstyp auswählen](menu-images/maint17.png "den Aktionstyp auswählen")](menu-images/maint17-large.png)
3. Geben Sie einen **Namen** "PhraseAddress", und drücken Sie die **verbinden** Schaltfläche, um die neue Aktion zu erstellen: 

    [![Konfigurieren der Aktion](menu-images/maint18.png "Konfigurieren der Aktion")](menu-images/maint18-large.png)
4. Wiederholen Sie die oben genannten Schritte für die **Datum**, **Greeting**, und **Signatur** Menüelemente: 

    [![Die abgeschlossenen Aktionen](menu-images/maint19.png "die abgeschlossenen Aktionen")](menu-images/maint19-large.png)
5. Speichern der Änderungen in der Menüleiste.

Als Nächstes nehmen wir eine Steckdose für unsere Textansicht zu erstellen, sodass dessen Inhalt aus Code angepasst werden kann. Wählen Sie die **ViewController.h** in der Datei die **Assistant Editor** , und erstellen Sie eine neue Steckdose aufgerufen `documentText`:

[![Erstellen eine Steckdose](menu-images/maint20.png "eine Steckdose erstellen")](menu-images/maint20-large.png)

Zurück zu Visual Studio für Mac, um die Änderungen von Xcode zu synchronisieren. Als Nächstes Bearbeiten der **ViewController.cs** Datei, und stellen sie wie folgt aussehen:

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

Dadurch wird den Text der unsere Textansicht außerhalb von der `ViewController` -Klasse und der app-Delegat informiert, wenn das Fenster erhält, oder den Fokus verliert. Bearbeiten Sie jetzt die **AppDelegate.cs** Datei, und stellen sie wie folgt aussehen:

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

Hier haben wir versucht die `AppDelegate` eine partielle Klasse, sodass wir verwenden können, die Aktionen und Ausgänge, die wir in Benutzeroberflächen-Generator definiert. Wir auch verfügbar machen eine `textEditor` nachverfolgen, welches das aktuell im Fokus ist.

Die folgenden Methoden werden verwendet, um unsere benutzerdefinierten Menüs und Menüelemente zu behandeln:

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

Jetzt alle Elemente im vorliegenden Anwendung beim Ausführen der **Ausdruck** Menü aktiv sein und der Textansicht bei Auswahl des angegebenen Ausdrucks hinzu:

![Ein Beispiel für die app, die](menu-images/maint21.png "ein Beispiel für die ausgeführte app")

Nun mit den Grundlagen der Arbeit mit der Menüleiste der Anwendung nach unten, sehen wir uns das Erstellen eines benutzerdefinierten kontextbezogenen Menüs.

### <a name="creating-menus-from-code"></a>Erstellen von Menüs von code

Zusätzlich zum Erstellen von Menüs und Menüelemente mit Xcodes Benutzeroberflächen-Generator, es gibt möglicherweise Zeiten, wenn eine Xamarin.Mac-app erstellen, ändern oder entfernen Sie ein Menü, Untermenüs oder Menüelement aus Code muss.

Im folgenden Beispiel wird eine Klasse erstellt, zum Speichern der Informationen über Menüelemente und Untermenüs, die dynamisch auf dynamisch erstellt werden:

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

Mit dieser Klasse definiert ist, werden die folgenden Routine eine Auflistung von analysiert `LanguageFormatCommand`Objekte rekursiv erstellen und neue Menüs und Menüelemente durch Anfügen an das Ende der vorhandenen Menü (in der Benutzeroberflächen-Generator erstellt), der im übergeben wurde:

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

Für eine beliebige `LanguageFormatCommand` -Objekt, das eine leere `Title` -Eigenschaft, um diese Routine erstellt eine **Trennzeichen Menüelement** (eine schlanke graue Linie) zwischen Menü Abschnitte:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Wenn Sie ein Titel angegeben wird, wird ein neues Menüelement mit diesem Namen erstellt:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Wenn die `LanguageFormatCommand` Objekt enthält untergeordnete `LanguageFormatCommand` -Objekten, Untermenü erstellt wird und die `AssembleMenu` Methode ist rekursiv aufgerufen, um das Menü zu erstellen:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Für jedes neue Menüelement, das nicht die Untermenüs verfügt, wird Code hinzugefügt, wird vom Benutzer ausgewählten Menüelements behandeln:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Testen das Menü erstellen

Mit allen des vorangehenden Codes eingerichtet ist wenn die folgende Sammlung von `LanguageFormatCommand` Objekte erstellt wurden:

```csharp
// Define formatting commands
FormattingCommands.Add(new LanguageFormatCommand("Stong","**","**"));
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
FormattingCommands.Add(new LanguageFormatCommand("Image","![]("-")"));
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[ ![]("-")](linkimagehere/index.md)"));
```

Und dass die Auflistung an die übergeben der `AssembleMenu` Funktion (mit der **Format** Menü als Basis festgelegt), würde die folgenden dynamischen Menüs und Menüelemente erstellt werden:

![Die neuen Menüelemente in der ausgeführten app](menu-images/dynamic01.png "neue Menüelemente in der ausgeführten app")

#### <a name="removing-menus-and-items"></a>Entfernen von Menüs und Elemente

Wenn Sie die app-Benutzeroberfläche ein Menü oder Menüelement aufheben müssen, können Sie mithilfe der `RemoveItemAt` Methode der `NSMenu` einfach durch die Übergabe der 0 (null) klassenbasierter Index des zu entfernenden Elements.

Um die Menüs und Menüelemente, die von der Routine, die oben erstellte zu entfernen, verwenden Sie beispielsweise den folgenden Code:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

Im Fall der obige Code werden die ersten vier Menüelemente in Xcodes Benutzeroberflächen-Generator und Zusammenfassung der Punkte in der app verfügbar erstellt, damit sie nicht dynamisch entfernt werden.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Kontextmenüs

Kontextbezogene Menüs angezeigt werden, wenn der Benutzer klickt oder ein Element in einem Fenster Steuerelement klickt. Standardmäßig verfügen über mehrere UI-Elemente, die bereits in MacOS integriert Kontextmenüs (z. B. die Textansicht) verbunden. Möglicherweise gibt es jedoch Situationen wir unsere eigene benutzerdefinierte Kontextmenüs für ein Element der Benutzeroberfläche zu erstellen, die wir in einem Fenster hinzugefügt haben.

Ermöglicht das Bearbeiten unsere **Main.storyboard** in Xcode-Datei und fügen eine **Fenster** Fenster aus, um unsere Entwurf festlegen seiner **Klasse** zu "NSPanel" in der **Identität Inspektor**, fügen Sie einen neuen **-Assistenten** Element zum der **Fenster** Menü, und fügen Sie es an das neue Fenster mit einer **Segue anzeigen**:

[![Festlegen des Typs Segue](menu-images/context01.png "der Segue Einstellungstyp")](menu-images/context01-large.png)

Führen Sie wir Folgendes:

1. Ziehen Sie eine **Bezeichnung** aus der **Bibliothek Inspektor** auf die **Bereich** Fenster und der Text "Property" festgelegt: 

    [![Bearbeiten der Bezeichnung Werts](menu-images/context03.png "des Bezeichnungsfelds Wert bearbeiten")](menu-images/context03-large.png)
2. Ziehen Sie als Nächstes eine **Menü** aus der **Bibliothek Inspektor** auf die View-Controller in der Hierarchie anzeigen und Rename drei Standard-Menüelemente **Dokument**, **Text**  und **Schriftart**:

    [![Die erforderlichen Menüelemente](menu-images/context02.png "Menüelemente erforderlich")](menu-images/context02-large.png)
3. Jetzt Steuerelement ziehen Sie aus der **Eigenschaft Bezeichnung** auf die **Menü**:

    [![Ziehen Sie zum Erstellen einer Segue](menu-images/context04.png "ziehen, um eine Segue erstellen")](menu-images/context04-large.png)
4. Wählen Sie im Popup-Dialogfeld **Menü**: 

    ![Festlegen des Typs Segue](menu-images/context05.png "der Segue Einstellungstyp")
5. Aus der **Identität Inspektor**, legen Sie die View-Controller-Klasse, um "PanelViewController": 

    [![Festlegen der Klasse Segue](menu-images/context10.png "Festlegen der Segue-Klasse")](menu-images/context10-large.png)
6. Wechseln Sie zurück zu Visual Studio für Mac synchronisiert werden, dann würden Sie zum Schnittstelle-Generator.
7. Wechseln Sie zu der **Assistant Editor** , und wählen Sie die **PanelViewController.h** Datei.
8. Erstellen Sie eine Aktion für die **Dokument** Menüelement aufgerufen `propertyDocument`: 

    [![Konfigurieren der Aktion](menu-images/context06.png "Konfigurieren der Aktion")](menu-images/context06-large.png)
9. Wiederholen Sie erstellen Aktionen für die verbleibenden Menüelemente aus: 

    [![Die erforderlichen Aktionen](menu-images/context07.png "die erforderlichen Aktionen")](menu-images/context07-large.png)
10. Schließlich erstellen Sie eine Steckdose für die **Eigenschaft Bezeichnung** aufgerufen `propertyLabel`: 

    [![Konfigurieren den Ausgang](menu-images/context08.png "den Ausgang konfigurieren")](menu-images/context08-large.png)
11. Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

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

Wenn wir die Anwendung ausführen und mit der rechten auf die Beschriftung der Eigenschaft im Bereich Maustaste, wird wir nun unsere benutzerdefinierte Kontextmenü angezeigt. Wenn wir wählen und aus dem Menü Element, ändert sich der Wert für die Bezeichnung:

![Im Kontextmenü ausführen](menu-images/context09.png "im Kontextmenü ausführen")

Nächste sehen wir uns Status Leiste Menüs erstellen.

## <a name="status-bar-menus"></a>Status Leiste Menüs

Status Leiste Menüs eine Auflistung von Status-Menüelemente, die Interaktion mit bereitstellen oder Feedback der Benutzer, z. B. eines Menüs oder ein Bild Darstellung des Status einer Anwendung angezeigt. Statusleiste und eine Anwendung ist aktiviert und aktiv, selbst wenn die Anwendung im Hintergrund ausgeführt wird. Die systemweite Statusleiste befindet sich rechts neben der Menüleiste der Anwendung und nur auf der Statusleiste in MacOS derzeit verfügbar ist.

Ermöglicht das Bearbeiten unsere **AppDelegate.cs** Datei, und stellen die `DidFinishLaunching` Methode aussehen wie folgt:

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` erhalten Sie Zugriff auf die Leiste systemweiten Status. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` erstellt ein neues Element des Status-Leiste. Von dort aus werden ein Menü und eine Anzahl von Menüelementen im erstellen und fügen Sie im Menü an das Status Leiste-Element, das soeben erstellt wurde. 

Wenn wir die Anwendung ausführen, wird das neue Element des Status-Leiste angezeigt. Auswählen eines Elements aus dem Menü, ändern Sie den Text in der Textansicht: 

![Die Statusleiste ausgeführt](menu-images/statusbar01.png "die Statusleiste ausgeführt wird")

Als Nächstes sehen wir uns das Erstellen von benutzerdefinierten Andocken Menüelemente.

## <a name="custom-dock-menus"></a>Andocken von benutzerdefinierten Menüs

Wenn der Benutzer klickt oder der Anwendung-Symbol im Dock Steuerelement klickt, wird für Sie Macintosh-Anwendung des andockmenüs angezeigt:

![Eine benutzerdefinierte Andocken Menü](menu-images/dock01.png "eine benutzerdefinierte Andocken Menü")

Wir erstellen eine benutzerdefinierte andockmenüs für unsere Anwendung wie folgt:

1. Visual Studio für Mac, mit der Maustaste auf das Projekt und wählen Sie der Anwendungsverzeichnis **hinzufügen** > **neue Datei...** Wählen Sie im Dialogfeld neue Datei **Xamarin.Mac** > **leere Schnittstellendefinition**, verwenden Sie "DockMenu" für die **Namen** , und klicken Sie auf die **neu**  Schaltfläche zum Erstellen des neuen **DockMenu.xib** Datei:

    ![Hinzufügen eine leere Schnittstellendefinition](menu-images/dock02.png "eine leere Schnittstellendefinition hinzufügen")
2. In der **Lösung Pad**, doppelklicken Sie auf die **DockMenu.xib** Datei, die sie zur Bearbeitung in Xcode geöffnet. Erstellen Sie ein neues **Menü** mit den folgenden Elementen: **Adresse**, **Datum**, **Greeting**, und **Signatur** 

    [![Layout der Benutzeroberflächenautomatisierungs](menu-images/dock03.png "Layouts auf der Benutzeroberflächenautomatisierungs")](menu-images/dock03-large.png)
3. Als Nächstes verbinden wir unsere neue Menüelemente mit unseren vorhandenen Aktionen, die wir für unsere benutzerdefinierten Menüs in erstellt die [hinzufügen, bearbeiten und Löschen von Menüs](#Adding,_Editing_and_Deleting_Menus) obigen Abschnitt. Wechseln Sie zu der **Verbindung Inspektor** , und wählen Sie die **erste Beantworter** in der **Schnittstellenhierarchie**. Führen Sie einen Bildlauf nach unten, und suchen Sie die `phraseAddress:` Aktion. Ziehen Sie eine Zeile aus dem Kreis für diese Aktion die **Adresse** Menüelement:

    [![Ziehen Weise wird eine Aktion](menu-images/dock04.png "Weise wird eine Aktion ziehen")](menu-images/dock04-large.png)
4. Wiederholen Sie für alle anderen Menüelemente anfügen an die entsprechenden Aktionen: 

    [![Die erforderlichen Aktionen](menu-images/dock05.png "die erforderlichen Aktionen")](menu-images/dock05-large.png)
5. Wählen Sie als Nächstes die **Anwendung** in der **Schnittstellenhierarchie**. In der **Verbindung Inspektor**, ziehen Sie eine Zeile aus dem Kreis der `dockMenu` nachrichtenplattform auf das Menü, das soeben erstellt wurde:

    [![Ziehen das Verbinden der Ausgang](menu-images/dock06.png "der Übertragung von der Steckdose ziehen")](menu-images/dock06-large.png)
6. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.
7. Doppelklicken Sie auf die **"Info.plist"** Datei zur Bearbeitung zu öffnen: 

    [![Bearbeiten die Datei "Info.plist"](menu-images/dock07.png "Bearbeiten der Datei "Info.plist"")](menu-images/dock07-large.png)
8. Klicken Sie auf die **Quelle** Registerkarte am unteren Rand des Bildschirms: 

    [![Auswählen der Datenquellensicht](menu-images/dock08.png "die Datenquellensicht auswählen")](menu-images/dock08-large.png)
9. Klicken Sie auf **neuen Eintrag hinzufügen**, klicken Sie auf die grüne sowie eine Schaltfläche, legen Sie den Namen der Eigenschaft auf "AppleDockMenu" und der Wert "DockMenu" (der Name der neuen .xib Datei ohne Erweiterung): 

    [![Hinzufügen des Elements DockMenu](menu-images/dock09.png "das DockMenu-Element hinzufügen")](menu-images/dock09-large.png)

Wenn wir unsere Anwendung ausführen und mit der rechten auf das zugehörige Symbol im Dock Maustaste, werden nun unsere neue Menüelemente angezeigt:

![Ein Beispiel des andockmenüs ausführen](menu-images/dock10.png "ein Beispiel des andockmenüs ausführen")

Wenn wir eine benutzerdefinierte Elemente aus dem Menü auswählen, wird der Text in unserer Textansicht geändert werden.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Popup-Schaltfläche und Pull-Dropdownlisten

Eine Popup-Schaltfläche zeigt ein ausgewähltes Element und zeigt eine Liste mit Optionen zur Auswahl aus, wenn der Benutzer darauf klickt. Eine Drop-Down-Liste ist ein Popup-Schaltfläche in der Regel wird verwendet, um Befehle, die nur für den Kontext des aktuellen Task. Beide können an einer beliebigen Stelle in einem Fenster angezeigt.

Wir erstellen eine benutzerdefinierte Popup-Schaltfläche für die Anwendung wie folgt:

1. Bearbeiten der **Main.storyboard** Datei in Xcode, und ziehen Sie eine **Popup-Schaltfläche** aus der **Bibliothek Inspektor** auf die **Bereich** Fenster, das wir in erstellt die [Kontextmenüs](#Contextual_Menus) Abschnitt: 

    [![Hinzufügen einer Schaltfläche Popup](menu-images/popup01.png "Hinzufügen einer Popup-Schaltfläche")](menu-images/popup01-large.png)
2. Fügen Sie ein neues Menüelement, und legen Sie den Titel der Elemente in der Popup: **Adresse**, **Datum**, **Greeting**, und **Signatur** 

    [![Konfigurieren die Menüelemente](menu-images/popup02.png "Menüelemente konfigurieren")](menu-images/popup02-large.png)
3. Als Nächstes verbinden wir unsere neue Menüelemente für die vorhandenen Aktionen, die wir für unsere benutzerdefinierten Menüs in erstellt die [hinzufügen, bearbeiten und Löschen von Menüs](#Adding,_Editing_and_Deleting_Menus) obigen Abschnitt. Wechseln Sie zu der **Verbindung Inspektor** , und wählen Sie die **erste Beantworter** in der **Schnittstellenhierarchie**. Führen Sie einen Bildlauf nach unten, und suchen Sie die `phraseAddress:` Aktion. Ziehen Sie eine Zeile aus dem Kreis für diese Aktion die **Adresse** Menüelement: 

    [![Ziehen Weise wird eine Aktion](menu-images/popup03.png "Weise wird eine Aktion ziehen")](menu-images/popup03-large.png)
4. Wiederholen Sie für alle anderen Menüelemente anfügen an die entsprechenden Aktionen: 

    [![Alle erforderlichen Aktionen](menu-images/popup04.png "alle erforderlichen Aktionen")](menu-images/popup04-large.png)
5. Speichern Sie die Änderungen zu, und wechseln Sie zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Wenn wir unsere Anwendung ausführen und das Popup ein Element auswählen, wird nun den Text in unserer Textansicht ändern:

![Ein Beispiel für das Popup mit](menu-images/popup05.png "ein Beispiel für das Popupfenster ausgeführt wird")

Sie können erstellt und funktionieren mit Pulldownliste Listen in genau dieselbe Weise als Popupmenü Schaltflächen. Sie können anstatt durch Anfügen an vorhandene Aktion, Ihre eigenen benutzerdefinierten Aktionen erstellen, wie wir für unsere Kontextmenü in haben der [Kontextmenüs](#Contextual_Menus) Abschnitt.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Menüs und Menüelemente in einer Anwendung Xamarin.Mac übernommen. Zuerst untersucht wir Menüleiste der Anwendung und erläutert, Kontextmenüs erstellen, als Nächstes Status Leiste Menüs untersucht und benutzerdefinierte Andocken von Menüs. Schließlich behandelt wir Popupmenüs und Pull-Dropdownlisten.


## <a name="related-links"></a>Verwandte Links

- [MacMenus (Beispiel)](https://developer.xamarin.com/samples/mac/MacMenus/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Richtlinien für menschliche Benutzeroberflächen - Menüs](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Einführung in die Anwendungsmenüs und Popup-Listen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
