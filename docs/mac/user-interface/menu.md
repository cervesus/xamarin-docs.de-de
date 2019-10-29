---
title: Menüs in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit Menüs in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Menüs und Menü Elementen in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen.
ms.prod: xamarin
ms.assetid: 5D367F8E-3A76-4995-8A89-488530FAD802
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 7cca5be2ea13deb17b27e5452df389a998c6eb09
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026169"
---
# <a name="menus-in-xamarinmac"></a>Menüs in xamarin. Mac

_In diesem Artikel wird das Arbeiten mit Menüs in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Menüs und Menü Elementen in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen._

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf die gleichen Cocoa-Menüs, die ein Entwickler in Ziel-C und Xcode verwendet. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die Interface Builder von Xcode verwenden, um die Menüleisten, Menüs und Menü Elemente zu erstellen und zu verwalten (oder Sie C# optional direkt im Code zu erstellen).

Menüs sind ein wesentlicher Bestandteil der Benutzeroberfläche einer Mac-Anwendung und werden im Allgemeinen in verschiedenen Teilen der Benutzeroberfläche angezeigt:

- **Die Menüleiste der Anwendung** : Dies ist das Hauptmenü, das im oberen Bereich des Bildschirms für jede Mac-Anwendung angezeigt wird.
- **Kontextmenüs** : Diese werden angezeigt, wenn der Benutzer mit der rechten Maustaste klickt oder auf ein Element in einem Fenster klickt.
- **Die Statusleiste** : Dies ist der Bereich ganz rechts in der Anwendungsmenü Leiste, der oben auf dem Bildschirm angezeigt wird (auf der linken Seite der Menüleiste) und nach links vergrößert wird, wenn Elemente hinzugefügt werden.
- **Menü Andocken** : das Menü für jede Anwendung im andocken, das angezeigt wird, wenn der Benutzer mit der rechten Maustaste klickt oder auf das Symbol der Anwendung klickt, oder wenn der Benutzer auf das Symbol klickt und die Maustaste gedrückt hält.
- **Popup Schaltfläche und pulldownlisten** : eine Popup Schaltfläche zeigt ein ausgewähltes Element an und zeigt eine Liste der Optionen an, die Sie auswählen können, wenn der Benutzer darauf klickt. Eine Pulldownliste ist ein Typ von Popup Schaltfläche, der in der Regel für die Auswahl von Befehlen verwendet wird, die für den Kontext der aktuellen Aufgabe spezifisch sind. Beides kann an beliebiger Stelle in einem Fenster angezeigt werden.

[![Ein Beispiel Menü](menu-images/intro01.png "Ein Beispiel Menü")](menu-images/intro01-large.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Cocoa-Menüleisten, Menüs und Menü Elementen in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die `Register` und `Export` Attribute erläutert, die zum Verknüpfen der C# Klassen mit "Ziel-c" verwendet werden. Objekte und UI-Elemente.

## <a name="the-applications-menu-bar"></a>Die Menüleiste der Anwendung 

Anders als bei Anwendungen, die unter dem Windows-Betriebssystem ausgeführt werden, an das jedem Fenster eine eigene Menüleiste angefügt werden kann, verfügt jede Anwendung, die unter macOS ausgeführt wird, über eine einzelne Menüleiste, die für jedes Fenster in dieser Anwendung verwendet wird:

[![Eine Menüleiste](menu-images/appmenu01.png "Eine Menüleiste")](menu-images/appmenu01-large.png#lightbox)

Elemente in dieser Menüleiste werden basierend auf dem aktuellen Kontext oder Zustand der Anwendung und ihrer Benutzeroberfläche zu einem beliebigen Zeitpunkt aktiviert oder deaktiviert. Beispiel: Wenn der Benutzer ein Textfeld auswählt, werden die Elemente im Menü **Bearbeiten** aktiviert, z. b. **Kopieren** und **Ausschneiden**.

Gemäß Apple und standardmäßig verfügen alle macOS-Anwendungen über einen Standardsatz von Menüs und Menü Elementen, die in der Menüleiste der Anwendung angezeigt werden:

- **Apple-Menü** : Dieses Menü ermöglicht den Zugriff auf systemweite Elemente, die für den Benutzer jederzeit verfügbar sind, unabhängig davon, welche Anwendung ausgeführt wird. Diese Elemente können vom Entwickler nicht geändert werden.
- **App-Menü** : in diesem Menü wird der Name der Anwendung in Fett Schrift angezeigt, und der Benutzer kann ermitteln, welche Anwendung derzeit ausgeführt wird. Sie enthält Elemente, die für die Anwendung als Ganzes gelten, nicht für ein bestimmtes Dokument oder einen Prozess, wie z. b. das Beenden der Anwendung.
- **Menü "Datei** ": Elemente, die zum Erstellen, öffnen oder Speichern von Dokumenten verwendet werden, mit denen die Anwendung arbeitet. Wenn Ihre Anwendung nicht Dokument basiert ist, kann dieses Menü umbenannt oder entfernt werden.
- **Menü Bearbeiten** : enthält Befehle wie **Ausschneiden**, **Kopieren**und **Einfügen** , die zum Bearbeiten oder Ändern von Elementen in der Benutzeroberfläche der Anwendung verwendet werden.
- **Menü "Format** ": Wenn die Anwendung mit Text funktioniert, enthält dieses Menübefehle, mit denen die Formatierung des Texts angepasst wird.
- **Menü anzeigen** : enthält Befehle, die beeinflussen, wie Inhalt in der Benutzeroberfläche der Anwendung angezeigt wird (angezeigt).
- **Anwendungsspezifische Menüs** : Hierbei handelt es sich um Menüs, die für Ihre Anwendung spezifisch sind (z. b. ein Lesezeichen Menü für einen Webbrowser). Sie sollten zwischen den Menüs **Ansicht** und **Fenster** auf der Leiste angezeigt werden.
- **Fenstermenü** : enthält Befehle für die Arbeit mit Fenstern in der Anwendung sowie eine Liste der aktuellen geöffneten Fenster.
- **Menü "Hilfe** ": Wenn Ihre Anwendung die Bildschirm Hilfe bereitstellt, sollte das Menü "Hilfe" das ganz rechts Menü auf der Leiste enthalten. 

Weitere Informationen zur Anwendungsmenü Leiste sowie zu Standardmenüs und Menü Elementen finden Sie in den [Richtlinien für die Benutzeroberfläche](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)von Apple.

### <a name="the-default-application-menu-bar"></a>Die Standard Anwendungsmenü Leiste

Wenn Sie ein neues xamarin. Mac-Projekt erstellen, erhalten Sie automatisch eine standardmäßige Standard Anwendungsmenü Leiste, die die typischen Elemente enthält, die eine macOS-Anwendung normalerweise hätte (wie im obigen Abschnitt erläutert). Die Standardmenü Leiste Ihrer Anwendung wird in der Datei " **Main. Storyboard** " (zusammen mit dem Rest der Benutzeroberfläche Ihrer APP) unter dem Projekt im **Lösungspad**definiert:  

![Wählen Sie das Haupt Storyboard aus.](menu-images/appmenu02.png "Wählen Sie das Haupt Storyboard aus.")

Doppelklicken Sie auf die Datei **Main. Storyboard** , um Sie in der Interface Builder von Xcode zur Bearbeitung zu öffnen, und die Menü-Editor-Schnittstelle wird angezeigt:

[![Bearbeiten der Benutzeroberfläche in Xcode](menu-images/defaultbar01.png "Bearbeiten der Benutzeroberfläche in Xcode")](menu-images/defaultbar01-large.png#lightbox)

Hier können Sie auf Elemente wie das Menü Element **Öffnen** im Menü **Datei** klicken und ihre Eigenschaften im **Attribute Inspector**bearbeiten oder anpassen:

[![Bearbeiten der Attribute eines Menüs](menu-images/defaultbar02.png "Bearbeiten der Attribute eines Menüs")](menu-images/defaultbar02-large.png#lightbox)

Wir werden später in diesem Artikel das Hinzufügen, bearbeiten und Löschen von Menüs und Elementen Unternehmen. Nun möchten wir nur noch sehen, welche Menüs und Menü Elemente standardmäßig verfügbar sind und wie Sie automatisch über einen Satz vordefinierter Outlets und Aktionen für Code verfügbar gemacht wurden (Weitere Informationen finden Sie in der Dokumentation zu [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) ).

Wenn Sie z. b. auf den **Verbindungs Inspektor** für das Menü Element **Öffnen** klicken, sehen Sie, dass es automatisch mit der `openDocument:` Aktion verknüpft ist: 

[![Anzeigen der angefügten Aktion](menu-images/defaultbar03.png "Anzeigen der angefügten Aktion")](menu-images/defaultbar03-large.png#lightbox)

Wenn Sie den **ersten Responder** in der **Schnittstellen Hierarchie** auswählen und im **Verbindungs Inspektor**einen Bildlauf nach unten durchführen, wird die Definition der `openDocument:` Aktion angezeigt, an die das Menü Element **Öffnen** angefügt ist (zusammen mit einigen anderen Standard Aktionen für die Anwendung, die und nicht automatisch mit Steuerelementen verknüpft sind:

[![Anzeigen aller angefügten Aktionen](menu-images/defaultbar04.png "Anzeigen aller angefügten Aktionen")](menu-images/defaultbar04-large.png#lightbox) 

Warum ist das wichtig? Im nächsten Abschnitt erfahren Sie, wie diese automatisch definierten Aktionen mit anderen Cocoa-Benutzeroberflächen Elementen funktionieren, um Menü Elemente automatisch zu aktivieren und zu deaktivieren sowie um integrierte Funktionen für die Elemente bereitzustellen.

Später werden diese integrierten Aktionen verwendet, um Elemente aus dem Code zu aktivieren und zu deaktivieren und um unsere eigene Funktionalität bereitzustellen, wenn Sie ausgewählt werden.

<a name="Built-In_Menu_Functionality" />

### <a name="built-in-menu-functionality"></a>Integrierte Menü Funktionalität

Wenn Sie eine neu erstellte xamarin. Mac-Anwendung ausgeführt haben, bevor Sie Benutzeroberflächen Elemente oder-Code hinzugefügt haben, werden Sie feststellen, dass einige Elemente automatisch verdrahtet und für Sie aktiviert werden (mit vollständig integrierter Funktionalität), wie z. b. das Element " **Beenden** " im  **App** -Menü:

![Ein aktiviertes Menü Element](menu-images/appmenu03.png "Ein aktiviertes Menü Element")

Andere Menü Elemente, wie z. b. **Ausschneiden**, **Kopieren**und **Einfügen** , sind nicht:

![Deaktivierte Menü Elemente](menu-images/appmenu04.png "Deaktivierte Menü Elemente")

Nehmen wir an, dass die Anwendung beendet wird, und Doppel klicke auf die Datei " **Main. Storyboard** " im **Lösungspad** , um Sie zur Bearbeitung in der Interface Builder von Xcode zu öffnen. Ziehen Sie als nächstes eine **Text Ansicht** aus der **Bibliothek** auf den Ansichts Controller des Fensters im Schnittstellen- **Editor**:

[![Auswählen einer Text Ansicht aus der Bibliothek](menu-images/appmenu05.png "Auswählen einer Text Ansicht aus der Bibliothek")](menu-images/appmenu05-large.png#lightbox)

Fügen Sie im Einschränkungs- **Editor** die Textansicht an die Ränder des Fensters an, und legen Sie Sie dort ab, wo Sie mit dem Fenster vergrößert und verkleinert wird, indem Sie auf alle vier roten I-Balken am oberen Rand des Editors klicken und dann auf die Schaltfläche **4 Einschränkungen hinzufügen** klicken:

[![Bearbeiten der Einschränkungen](menu-images/appmenu06.png "Bearbeiten der Einschränkungen")](menu-images/appmenu06-large.png#lightbox)

Speichern Sie die Änderungen am Design der Benutzeroberfläche, und wechseln Sie zurück zum Visual Studio für Mac, um die Änderungen mit Ihrem xamarin. Mac-Projekt zu synchronisieren. Starten Sie nun die Anwendung, geben Sie Text in die Textansicht ein, wählen Sie Sie aus, und öffnen Sie das Menü **Bearbeiten** :

![Die Menü Elemente werden automatisch aktiviert/deaktiviert.](menu-images/appmenu07.png "Die Menü Elemente werden automatisch aktiviert/deaktiviert.")

Beachten Sie, dass die **Ausschneide**-, **Kopier**-und **Einfüge** Elemente automatisch aktiviert und voll funktionsfähig sind, ohne eine einzige Codezeile schreiben zu müssen. 

Was ist hier los? Merken Sie sich die integrierten vordefinieren-Aktionen, die mit den Standardmenü Elementen verknüpft sind (wie oben dargestellt). die meisten Cocoa-Benutzeroberflächen Elemente, die Teil von macOS sind, haben in bestimmte Aktionen integriert (z. b. `copy:`). Wenn Sie also einem Fenster hinzugefügt, aktiv und ausgewählt werden, werden die entsprechenden Menü Elemente bzw. Elemente, die an diese Aktion angefügt sind, automatisch aktiviert. Wenn der Benutzer das Menü Element auswählt, wird die Funktionalität, die in das UI-Element integriert ist, aufgerufen und ausgeführt, alle ohne Eingriff des Entwicklers.

### <a name="enabling-and-disabling-menus-and-items"></a>Aktivieren und Deaktivieren von Menüs und Elementen

Standardmäßig wird jedes Mal, wenn ein Benutzer Ereignis auftritt, `NSMenu` jedes sichtbare Menü und Menü Element basierend auf dem Kontext der Anwendung automatisch aktiviert und deaktiviert. Es gibt drei Möglichkeiten, ein Element zu aktivieren/deaktivieren:

- **Automatisches Aktivieren des Menüs** : ein Menü Element ist aktiviert, wenn `NSMenu` ein entsprechendes Objekt finden kann, das auf die Aktion reagiert, mit der das Element verknüpft ist. Beispielsweise die oben genannte Textansicht, die über einen integrierten Hook zum `copy:` Aktion verfügt.
- **Benutzerdefinierte Aktionen und validatemenuitem::** für ein beliebiges Menü Element, das an ein [Fenster oder eine benutzerdefinierte Aktion des Ansichts Controllers](#Working-with-Custom-Window-Actions)gebunden ist, können Sie die `validateMenuItem:` Aktion hinzufügen und Menü Elemente manuell aktivieren oder deaktivieren.
- **Manuelles** Aktivieren des Menüs: Sie legen die `Enabled`-Eigenschaft der einzelnen `NSMenuItem` manuell fest, um jedes Element einzeln in einem Menü zu aktivieren bzw. zu deaktivieren.

Um ein System auszuwählen, legen Sie die `AutoEnablesItems`-Eigenschaft eines `NSMenu` fest. `true` ist automatisch (Standardverhalten) und `false` ist manuell. 

> [!IMPORTANT]
> Wenn Sie die manuelle Menü Aktivierung verwenden, werden keines der Menü Elemente, auch solche, die von AppKit-Klassen wie `NSTextView` gesteuert werden, automatisch aktualisiert. Sie sind dafür verantwortlich, alle Elemente per Hand in Code zu aktivieren und zu deaktivieren.

#### <a name="using-validatemenuitem"></a>Verwenden von validatemenuitem

Wie bereits erwähnt, können Sie für jedes Menü Element, das an ein [Fenster oder eine benutzerdefinierte Aktion zum Anzeigen des Controllers](#Working-with-Custom-Window-Actions)gebunden ist, die `validateMenuItem:` Aktion hinzufügen und Menü Elemente manuell aktivieren bzw. deaktivieren.

Im folgenden Beispiel wird die `Tag`-Eigenschaft verwendet, um den Typ des Menü Elements zu entscheiden, das von der `validateMenuItem:` Aktion basierend auf dem Zustand des ausgewählten Texts in einem `NSTextView` aktiviert/deaktiviert wird. Die `Tag`-Eigenschaft wurde für jedes Menü Element Interface Builder festgelegt:

![Festlegen der Tageigenschaft](menu-images/validate01.png "Festlegen der Tageigenschaft")

Der folgende Code wurde dem Ansichts Controller hinzugefügt:

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

Wenn dieser Code ausgeführt wird und im `NSTextView` kein Text ausgewählt ist, werden die beiden Wrap-Menü Elemente deaktiviert (auch wenn Sie mit Aktionen auf dem Ansichts Controller verknüpft sind):

![Anzeige deaktivierter Elemente](menu-images/validate02.png "Anzeige deaktivierter Elemente")

Wenn ein Textabschnitt ausgewählt und das Menü erneut geöffnet wird, sind die beiden Menü Elemente für das Wrap verfügbar:

![Aktivierte Elemente werden angezeigt](menu-images/validate03.png "Aktivierte Elemente werden angezeigt")

## <a name="enabling-and-responding-to-menu-items-in-code"></a>Aktivieren von und Antworten auf Menü Elemente in Code

Wie bereits erwähnt, werden einige der Standardmenü Elemente nur durch Hinzufügen spezifischer Cocoa-Benutzeroberflächen Elemente zum UI-Design (z. b. ein Textfeld) aktiviert und funktionieren automatisch, ohne dass Sie Code schreiben müssen. Als nächstes sehen wir uns das Hinzufügen C# unseres eigenen Codes zu unserem xamarin. Mac-Projekt an, um ein Menü Element zu aktivieren und Funktionen bereitzustellen, wenn der Benutzer es auswählt.

Angenommen, der Benutzer soll das **geöffnete** Element im Menü **Datei** verwenden können, um einen Ordner auszuwählen. Da dies eine Anwendungs weite Funktion sein soll, die nicht auf ein Give-Fenster oder ein UI-Element beschränkt ist, fügen wir den Code hinzu, um dies für den Anwendungs Delegaten zu verarbeiten.

Doppelklicken Sie im **Lösungspad**auf die `AppDelegate.CS` Datei, um Sie zur Bearbeitung zu öffnen:

![Auswählen des App-Delegaten](menu-images/appmenu08.png "Auswählen des App-Delegaten")

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

Führen Sie die Anwendung jetzt aus, und öffnen Sie das Menü " **Datei** ": 

![Menü "Datei"](menu-images/appmenu09.png "Menü "Datei"")

Beachten Sie, dass das Menü Element **Öffnen** jetzt aktiviert ist. Wenn Sie diese Option auswählen, wird das Dialogfeld Öffnen angezeigt:

![Ein offenes Dialogfeld](menu-images/appmenu10.png "Ein offenes Dialogfeld")

Wenn Sie auf die Schaltfläche " **Öffnen** " klicken, wird unsere Warnmeldung angezeigt:

![Beispiel Dialogfeld Meldung](menu-images/appmenu11.png "Beispiel Dialogfeld Meldung")

Die Schlüssel Zeile, die hier `[Export ("openDocument:")]`ist, weist `NSMenu`, dass unser **appdelegat** über eine Methode `void OpenDialog (NSObject sender)` verfügt, die auf die `openDocument:` Aktion antwortet. Wenn Sie sich weiter oben merken, wird das Menü Element **Öffnen** in Interface Builder standardmäßig automatisch mit dieser Aktion verknüpft:

[![Anzeigen der angefügten Aktionen](menu-images/defaultbar03.png "Anzeigen der angefügten Aktionen")](menu-images/defaultbar03-large.png#lightbox)

Als nächstes sehen wir uns das Erstellen von eigenen Menüs, Menü Elementen und Aktionen an und Antworten im Code darauf.

### <a name="working-with-the-open-recent-menu"></a>Arbeiten mit dem Menü "zuletzt öffnen"

Standardmäßig enthält das Menü **Datei** ein **geöffnetes Aktuelles** Element, mit dem die letzten Dateien nachverfolgt werden, die der Benutzer mit Ihrer APP geöffnet hat. Wenn Sie eine `NSDocument` basierte xamarin. Mac-app erstellen, wird dieses Menü automatisch behandelt. Für alle anderen Typen von xamarin. Mac-apps sind Sie dafür verantwortlich, dieses Menü Element manuell zu verwalten und darauf zu reagieren.

Um das Menü " **zuletzt geöffnet** " manuell zu behandeln, müssen Sie es zunächst darüber informieren, dass eine neue Datei geöffnet oder gespeichert wurde, indem Sie Folgendes verwenden:

```csharp
// Add document to the Open Recent menu
NSDocumentController.SharedDocumentController.NoteNewRecentDocumentURL(url);
```

Obwohl Ihre APP `NSDocuments` nicht verwendet, verwenden Sie weiterhin die `NSDocumentController`, um das Menü " **zuletzt geöffnet** " zu verwalten, indem Sie eine `NSUrl` mit dem Speicherort der Datei an die `NoteNewRecentDocumentURL`-Methode der `SharedDocumentController` senden.

Als nächstes müssen Sie die `OpenFile`-Methode des App-Delegaten überschreiben, um eine beliebige Datei zu öffnen, die der Benutzer aus dem Menü " **zuletzt geöffnet** " auswählt. Beispiel:

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

Rückgabe `true` wenn die Datei geöffnet werden kann, geben Sie `false` zurück, und dem Benutzer wird eine integrierte Warnung angezeigt, dass die Datei nicht geöffnet werden konnte.

Da der Dateiname und der Pfad, die im Menü " **zuletzt geöffnet** " angezeigt werden, ein Leerzeichen enthalten, müssen wir dieses Zeichen ordnungsgemäß mit Escapezeichen versehen, bevor Sie eine `NSUrl` erstellen. Dies geschieht mit folgendem Code:

```csharp
filename = filename.Replace (" ", "%20");
```

Schließlich erstellen wir eine `NSUrl`, die auf die Datei verweist, und verwenden eine Hilfsmethode im App-Delegaten, um ein neues Fenster zu öffnen und die Datei in die Datei zu laden:

```csharp
var url = new NSUrl ("file://"+filename);
return OpenFile(url);
```

Um alles zusammenzuleiten, werfen wir einen Blick auf eine Beispiel Implementierung in einer **AppDelegate.cs** -Datei:

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

Basierend auf den Anforderungen Ihrer APP soll der Benutzer möglicherweise nicht die gleiche Datei in mehr als einem Fenster gleichzeitig öffnen. In unserer Beispiel-APP, wenn der Benutzer eine Datei auswählt, die bereits geöffnet ist (entweder über das **geöffnete "zuletzt** geöffnet" oder " **Öffnen".** Menü Elemente), das Fenster, das die Datei enthält, wird in den Vordergrund gestellt.

Um dies zu erreichen, haben wir den folgenden Code in unserer Hilfsmethode verwendet:

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

Wir haben unsere `ViewController` Klasse so entworfen, dass Sie den Pfad zu der Datei in ihrer `Path`-Eigenschaft enthält. Als nächstes durchlaufen wir alle gegenwärtig geöffneten Fenster in der app. Wenn die Datei bereits in einem der Fenster geöffnet ist, wird Sie mithilfe von an die Vorderseite aller anderen Fenster übertragen:

```csharp
NSApplication.SharedApplication.Windows[n].MakeKeyAndOrderFront(this);
```

Wenn keine Entsprechung gefunden wird, wird ein neues Fenster mit der geladenen Datei geöffnet, und die Datei wird im Menü " **zuletzt geöffnet** " angezeigt:

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

### <a name="working-with-custom-window-actions"></a>Arbeiten mit benutzerdefinierten Fenster Aktionen

Ebenso wie die integrierten **ersten Response** -Aktionen, die mit Standardmenü Elementen vorverdrahtet werden, können Sie neue, benutzerdefinierte Aktionen erstellen und Sie an Menü Elemente in Interface Builder übertragen.

Definieren Sie zunächst eine benutzerdefinierte Aktion auf einem der Fenster Controller Ihrer APP. Beispiel:

```csharp
[Action("defineKeyword:")]
public void defineKeyword (NSObject sender) {
    // Preform some action when the menu is selected
    Console.WriteLine ("Request to define keyword");
}
```

Doppelklicken Sie im **Lösungspad** auf die storyboarddatei der APP, um Sie in der Interface Builder von Xcode zur Bearbeitung zu öffnen. Wählen Sie in der **Anwendungs Szene**den **ersten Responder** aus, und wechseln Sie dann zum **Attribut Inspektor**:

![Der Attribut Inspektor](menu-images/action01.png "Der Attribut Inspektor")

Klicken Sie unten im **attributerinspektor** auf die Schaltfläche **+** , um eine neue benutzerdefinierte Aktion hinzuzufügen:

![Hinzufügen einer neuen Aktion](menu-images/action02.png "Hinzufügen einer neuen Aktion")

Benennen Sie den gleichen Namen wie die benutzerdefinierte Aktion, die Sie auf Ihrem Fenster Controller erstellt haben:

![Bearbeiten des Aktions namens](menu-images/action03.png "Bearbeiten des Aktions namens")

Klicken Sie auf ein Menü Element, und ziehen Sie es von einem Menü Element auf den **ersten** Beantworter unter der **Anwendungs Szene**. Wählen Sie in der Popup Liste die neue Aktion aus, die Sie soeben erstellt haben (in diesem Beispiel `defineKeyword:`):

![Anfügen einer Aktion](menu-images/action04.png "Anfügen einer Aktion")

Speichern Sie die Änderungen am Storyboard, und kehren Sie zu Visual Studio für Mac zurück, um die Änderungen zu synchronisieren. Wenn Sie die app ausführen, wird das Menü Element, mit dem Sie die benutzerdefinierte Aktion verbunden haben, automatisch aktiviert/deaktiviert (basierend auf dem Fenster, in dem die Aktion geöffnet wird). Wenn Sie das Menü Element auswählen, wird die Aktion ausgelöst:

[![Testen der neuen Aktion](menu-images/action05.png "Testen der neuen Aktion")](menu-images/action05-large.png#lightbox)

<a name="Adding,_Editing_and_Deleting_Menus" />

### <a name="adding-editing-and-deleting-menus"></a>Hinzufügen, bearbeiten und Löschen von Menüs

Wie aus den vorherigen Abschnitten ersichtlich ist, enthält eine xamarin. Mac-Anwendung eine vordefinierte Anzahl von Standardmenüs und Menü Elementen, auf die bestimmte UI-Steuerelemente automatisch aktiviert werden und auf die Sie reagieren. Wir haben auch gesehen, wie Sie der Anwendung Code hinzufügen, der diese Standardelemente ebenfalls aktiviert und antwortet.

In diesem Abschnitt wird das Entfernen von nicht benötigten Menü Elementen, das Neuorganisieren von Menüs und das Hinzufügen neuer Menüs, Menü Elemente und Aktionen erläutert.

Doppelklicken Sie auf die Datei " **Main. Storyboard** " im **Lösungspad** , um Sie zur Bearbeitung zu öffnen:

[![Bearbeiten der Benutzeroberfläche in Xcode](menu-images/maint01.png "Bearbeiten der Benutzeroberfläche in Xcode")](menu-images/maint01-large.png#lightbox)

Für unsere spezifische xamarin. Mac-Anwendung verwenden wir nicht das Standardmenü " **Ansicht** ", um es zu entfernen. Wählen Sie in der **Schnittstellen Hierarchie** das Menü Element **Ansicht** aus, das Teil der Hauptmenüleiste ist:

![Auswählen des Menü Elements "Ansicht"](menu-images/maint02.png "Auswählen des Menü Elements "Ansicht"")

Drücken Sie ENTF oder RÜCKTASTE, um das Menü zu löschen. Als nächstes verwenden wir nicht alle Elemente im Menü " **Format** " und möchten die Elemente, die wir verwenden, aus den Untermenüs verschieben. Wählen Sie in der **Schnittstellen Hierarchie** die folgenden Menü Elemente aus:

![Markieren mehrerer Elemente](menu-images/maint03.png "Markieren mehrerer Elemente")

Ziehen Sie die Elemente unter dem übergeordneten **Menü** aus dem Untermenü, in dem Sie aktuell sind:

[![Ziehen von Menü Elementen in das übergeordnete Menü](menu-images/maint04.png "Ziehen von Menü Elementen in das übergeordnete Menü")](menu-images/maint04-large.png#lightbox)

Das Menü sollte nun wie folgt aussehen:

[![Die Elemente am neuen Speicherort](menu-images/maint05.png "Die Elemente am neuen Speicherort")](menu-images/maint05-large.png#lightbox)

Ziehen Sie als nächstes das **textsubmenü** aus dem Menü **Format** aus, und platzieren Sie es in der Hauptmenüleiste zwischen den Menüs **Format** und **Fenster** :

[![Das Textmenü](menu-images/maint06.png "Das Textmenü")](menu-images/maint06-large.png#lightbox)

Wechseln wir zurück zum Menü **Format** , und löschen Sie das unter Menü Element **Schriftart** . Wählen Sie als nächstes das Menü **Format** aus, und benennen Sie es "Font" um:

[![Das Schriftart Menü](menu-images/maint07.png "Das Schriftart Menü")](menu-images/maint07-large.png#lightbox)

Als Nächstes erstellen wir ein benutzerdefiniertes Menü mit vordefinierenden Ausdrücken, die automatisch an den Text in der Textansicht angehängt werden, wenn Sie ausgewählt werden. Geben Sie im Suchfeld im unteren Bereich des **Bibliothek inspektortyps** in "Menu" ein. Dies erleichtert die Suche nach und die Arbeit mit allen Menü Elemente der Benutzeroberfläche:

![Der Bibliotheks Inspektor](menu-images/maint08.png "Der Bibliotheks Inspektor")

Führen Sie nun Folgendes aus, um das Menü zu erstellen:

1. Ziehen Sie ein **Menü Element** aus dem **Bibliotheks Inspektor** in die Menüleiste zwischen den **Text** -und **Fenster** Menüs: 

    ![Auswählen eines neuen Menü Elements in der Bibliothek](menu-images/maint10.png "Auswählen eines neuen Menü Elements in der Bibliothek")
2. Benennen Sie das Element "Ausdrücke" um: 

    [![Festlegen des Menü namens](menu-images/maint09.png "Festlegen des Menü namens")](menu-images/maint09-large.png#lightbox)
3. Ziehen Sie als nächstes ein **Menü** aus der **Bibliothek Inspector**: 

    ![Auswählen eines Menüs aus der Bibliothek](menu-images/maint11.png "Auswählen eines Menüs aus der Bibliothek")
4. Löschen Sie das **Menü** im neuen, soeben erstellten **Menü Element** , und ändern Sie den Namen in "Ausdrücke": 

    [![Bearbeiten des Menü namens](menu-images/maint12.png "Bearbeiten des Menü namens")](menu-images/maint12-large.png#lightbox)
5. Benennen wir nun die drei Standard **Menü Elemente** "Address", "Date" und "Gruß" um: 

    [![Das Menü "Ausdrücke"](menu-images/maint13.png "Das Menü "Ausdrücke"")](menu-images/maint13-large.png#lightbox)
6. Fügen Sie ein viertes **Menü Element** hinzu, indem Sie ein **Menü Element** aus dem **Bibliotheks Inspektor** ziehen und es "Signature" aufrufen: 

    [![Bearbeiten des Menü Element namens](menu-images/maint14.png "Bearbeiten des Menü Element namens")](menu-images/maint14-large.png#lightbox)
7. Speichern Sie die Änderungen in der Menüleiste.

Nun erstellen wir einen Satz benutzerdefinierter Aktionen, damit unsere neuen Menü Elemente für C# Code verfügbar gemacht werden. Wechseln Sie in Xcode zur Ansicht **Assistant** :

[![Erstellen der erforderlichen Aktionen](menu-images/maint15.png "Erstellen der erforderlichen Aktionen")](menu-images/maint15-large.png#lightbox)

Führen Sie die folgenden Schritte aus:

1. Steuerelement: ziehen Sie das **Adress** Menü Element in die Datei **appdelegat. h** .
2. Wechseln Sie zum **Verbindungstyp** in **Aktion**: 

    [![Auswählen des Aktions Typs](menu-images/maint17.png "Auswählen des Aktions Typs")](menu-images/maint17-large.png#lightbox)
3. Geben Sie den **Namen** "phrasaddress" ein, und klicken Sie auf die Schaltfläche **verbinden** , um die neue Aktion zu erstellen: 

    [![Konfigurieren der Aktion](menu-images/maint18.png "Konfigurieren der Aktion")](menu-images/maint18-large.png#lightbox)
4. Wiederholen Sie die obigen Schritte für die Menü Elemente " **Datum**", " **Begrüßung**" und " **Signatur** ": 

    [![Abgeschlossene Aktionen](menu-images/maint19.png "Abgeschlossene Aktionen")](menu-images/maint19-large.png#lightbox)
5. Speichern Sie die Änderungen in der Menüleiste.

Als nächstes müssen wir ein Outlet für unsere Textansicht erstellen, damit wir den Inhalt aus dem Code anpassen können. Wählen Sie im **Assistenten-Editor** die Datei " **ViewController. h** " aus, und erstellen Sie ein neues Outlet namens `documentText`:

[![Erstellen eines Outlets](menu-images/maint20.png "Erstellen eines Outlets")](menu-images/maint20-large.png#lightbox)

Kehren Sie zu Visual Studio für Mac zurück, um die Änderungen von Xcode zu synchronisieren. Bearbeiten Sie als nächstes die Datei **ViewController.cs** , und führen Sie Sie wie folgt aus:

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

Dadurch wird der Text der Textansicht außerhalb der `ViewController` Klasse angezeigt, und der APP-Delegat wird informiert, wenn das Fenster den Fokus erhält oder verliert. Bearbeiten Sie nun die Datei **AppDelegate.cs** , und führen Sie Sie wie folgt aus:

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

Hier haben wir die `AppDelegate` zu einer partiellen Klasse gemacht, sodass wir die Aktionen und Outlets verwenden können, die wir in Interface Builder definiert haben. Außerdem wird eine `textEditor` verfügbar gemacht, um zu verfolgen, welches Fenster derzeit den Fokus hat.

Die folgenden Methoden werden verwendet, um das benutzerdefinierte Menü und die Menü Elemente zu behandeln:

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

Wenn wir nun die Anwendung ausführen, sind alle Elemente im Menü " **Ausdruck** " aktiv und fügen der Textansicht den Ausdruck "geben" hinzu, wenn Sie ausgewählt wird:

![Ein Beispiel für die Ausführung der APP](menu-images/maint21.png "Ein Beispiel für die Ausführung der APP")

Nun, da wir die Grundlagen der Arbeit mit der Menüleiste der Anwendung haben, sehen wir uns das Erstellen eines benutzerdefinierten Kontextmenüs an.

### <a name="creating-menus-from-code"></a>Erstellen von Menüs aus Code

Zusätzlich zum Erstellen von Menüs und Menü Elementen mit der Interface Builder von Xcode kann es vorkommen, dass eine xamarin. Mac-app ein Menü, ein Untermenü oder ein Menü Element aus dem Code erstellen, ändern oder entfernen muss.

Im folgenden Beispiel wird eine Klasse erstellt, die die Informationen über die Menü Elemente und Untermenüs enthält, die dynamisch dynamisch erstellt werden:

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

#### <a name="adding-menus-and-items"></a>Hinzufügen von Menüs und Elementen

Wenn diese Klasse definiert ist, analysiert die folgende Routine eine Auflistung von `LanguageFormatCommand`objects und erstellt rekursiv neue Menüs und Menü Elemente, indem Sie an den unteren Rand des vorhandenen Menüs angehängt wird (in Interface Builder erstellt), das übergeben wurde:

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

Für alle `LanguageFormatCommand` Objekte, die über eine leere `Title`-Eigenschaft verfügen, erstellt diese Routine ein **Menü Element mit Trenn** Zeichen (eine schlanke graue Linie) zwischen menüabschnitten:

```csharp
menuItem = NSMenuItem.SeparatorItem;
```

Wenn ein Titel angegeben wird, wird ein neues Menü Element mit diesem Titel erstellt:

```csharp
menuItem = new NSMenuItem (command.Title);
``` 

Wenn das `LanguageFormatCommand`-Objekt untergeordnete `LanguageFormatCommand` Objekte enthält, wird ein Untermenü erstellt, und die `AssembleMenu`-Methode wird rekursiv aufgerufen, um das Menü zu erstellen:

```csharp
menuItem.Submenu = new NSMenu (command.Title);
AssembleMenu (menuItem.Submenu, command.SubCommands);
```

Für jedes neue Menü Element, das keine Untermenüs enthält, wird Code hinzugefügt, um das Menü Element zu behandeln, das vom Benutzer ausgewählt wird:

```csharp
menuItem.Activated += (sender, e) => {
    // Do something when the menu item is selected
    ...
};
```

#### <a name="testing-the-menu-creation"></a>Testen der Menüerstellung

Wenn der gesamte obige Code vorhanden ist, wenn die folgende Auflistung von `LanguageFormatCommand`-Objekten erstellt wurde:

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
FormattingCommands.Add(new LanguageFormatCommand("Image Link","[![](",")](LinkImageHere)"));
```

Und diese Auflistung, die an die `AssembleMenu`-Funktion übertragen wird (wobei das Menü **Format** als Basis festgelegt ist), werden die folgenden dynamischen Menüs und Menü Elemente erstellt:

![Die neuen Menü Elemente in der laufenden app](menu-images/dynamic01.png "Die neuen Menü Elemente in der laufenden app")

#### <a name="removing-menus-and-items"></a>Entfernen von Menüs und Elementen

Wenn Sie ein Menü oder ein Menü Element von der Benutzeroberfläche der APP entfernen müssen, können Sie die `RemoveItemAt`-Methode der `NSMenu`-Klasse verwenden, indem Sie Ihr einfach den NULL basierten Index des zu entfernenden Elements bereitstellen.

Wenn Sie z. b. die Menüs und Menü Elemente entfernen möchten, die von der obigen Routine erstellt wurden, können Sie den folgenden Code verwenden:

```csharp
public void UnpopulateFormattingMenu(NSMenu menu) {

    // Remove any additional items
    for (int n = (int)menu.Count - 1; n > 4; --n) {
        menu.RemoveItemAt (n);
    }
}
```

Im Fall des obigen Codes werden die ersten vier Menü Elemente in der Xcode-Interface Builder und in der App zur Verfügung gestellt, sodass Sie nicht dynamisch entfernt werden.

<a name="Contextual_Menus" />

## <a name="contextual-menus"></a>Kontextmenüs

Kontextmenüs werden angezeigt, wenn der Benutzer mit der rechten Maustaste klickt oder auf ein Element in einem Fenster klickt. Standardmäßig verfügen mehrere in macOS integrierte Benutzeroberflächen Elemente bereits über Kontextmenüs (z. b. die Textansicht). Es kann jedoch vorkommen, dass Sie eigene benutzerdefinierte Kontextmenüs für ein UI-Element erstellen möchten, das einem Fenster hinzugefügt wurde.

Wir bearbeiten nun die Datei " **Main. Storyboard** " in Xcode und fügen dem Design ein **Fenster** Fenster hinzu, legen die **Klasse** auf "nspanel" im **Identitäts Inspektor**fest, fügen dem Menü " **Fenster** " ein neues **Assistenten** Element hinzu und fügen es an den neuen Fenster mit der **Anzeige**"*":

[![Festlegen des Typs "Typ"](menu-images/context01.png "Festlegen des Typs "Typ"")](menu-images/context01-large.png#lightbox)

Führen Sie die folgenden Schritte aus:

1. Ziehen Sie eine **Bezeichnung** aus dem **Bibliotheks Inspektor** in das **Panel** Fenster, und legen Sie den Text auf "Property" fest: 

    [![Bearbeiten des Werts der Bezeichnung](menu-images/context03.png "Bearbeiten des Werts der Bezeichnung")](menu-images/context03-large.png#lightbox)
2. Ziehen Sie als nächstes ein **Menü** aus dem **Bibliotheks Inspektor** auf den Ansichts Controller in der Ansichts Hierarchie, und benennen Sie die drei Standardmenü Elemente **Document**, **Text** und **Font**um:

    [![Die erforderlichen Menü Elemente](menu-images/context02.png "Die erforderlichen Menü Elemente")](menu-images/context02-large.png#lightbox)
3. Ziehen Sie jetzt das Steuerelement aus der **Eigenschaften Bezeichnung** in das **Menü**:

    [![Ziehen zum Erstellen einer Tabelle](menu-images/context04.png "Ziehen zum Erstellen einer Tabelle")](menu-images/context04-large.png#lightbox)
4. Wählen Sie im Popup Dialogfeld die Option **Menü**: 

    ![Festlegen des Typs "Typ"](menu-images/context05.png "Festlegen des Typs "Typ"")
5. Legen Sie aus dem **Identitäts Inspektor**die Klasse des Ansichts Controllers auf "panelviewcontroller" fest: 

    [![Festlegen der Klasse "Klasse"](menu-images/context10.png "Festlegen der Klasse "Klasse"")](menu-images/context10-large.png#lightbox)
6. Wechseln Sie zurück zu Visual Studio für Mac synchronisieren, und kehren Sie dann zu Interface Builder zurück.
7. Wechseln Sie zum **Assistenten-Editor** , und wählen Sie die Datei **panelviewcontroller. h** aus.
8. Erstellen Sie eine Aktion für das **Dokument** Menü Element mit dem Namen `propertyDocument`: 

    [![Konfigurieren der Aktion](menu-images/context06.png "Konfigurieren der Aktion")](menu-images/context06-large.png#lightbox)
9. Wiederholen Sie das Erstellen von Aktionen für die verbleibenden Menü Elemente: 

    [![Erforderliche Aktionen](menu-images/context07.png "Erforderliche Aktionen")](menu-images/context07-large.png#lightbox)
10. Erstellen Sie abschließend ein Outlet für die **Eigenschaften Bezeichnung** namens `propertyLabel`: 

    [![Konfigurieren des Outlets](menu-images/context08.png "Konfigurieren des Outlets")](menu-images/context08-large.png#lightbox)
11. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie die Datei **PanelViewController.cs** , und fügen Sie den folgenden Code hinzu:

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

Wenn wir nun die Anwendung ausführen und mit der rechten Maustaste auf die Eigenschaften Bezeichnung im Panel klicken, wird unser benutzerdefiniertes Kontextmenü angezeigt. Wenn Sie ein Element im Menü auswählen, ändert sich der Wert der Bezeichnung:

![Das Kontextmenü, das ausgeführt wird](menu-images/context09.png "Das Kontextmenü, das ausgeführt wird")

Im nächsten Abschnitt sehen wir uns das Erstellen von Status leisten Menüs an.

## <a name="status-bar-menus"></a>StatusBar-Menüs

In den Menüs der Statusleiste wird eine Auflistung von Status Menü Elementen angezeigt, die eine Interaktion mit oder Feedback für den Benutzer bereitstellen, z. b. ein Menü oder ein Bild, das den Zustand einer Anwendung widerspiegelt Das Menü Statusleiste einer Anwendung ist aktiviert und aktiv, selbst wenn die Anwendung im Hintergrund ausgeführt wird. Die systemweite Statusleiste befindet sich auf der rechten Seite der Anwendungsmenü Leiste und ist die einzige Statusleiste, die derzeit in macOS verfügbar ist.

Bearbeiten Sie die Datei **AppDelegate.cs** , und legen Sie die `DidFinishLaunching`-Methode wie folgt an:

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

`NSStatusBar statusBar = NSStatusBar.SystemStatusBar;` ermöglicht uns den Zugriff auf die systemweite Statusleiste. `var item = statusBar.CreateStatusItem (NSStatusItemLength.Variable);` erstellt ein neues Status leisten Element. Von dort aus erstellen wir ein Menü und mehrere Menü Elemente und fügen das Menü an das soeben erstellte Status leisten Element an. 

Wenn die Anwendung ausgeführt wird, wird das neue StatusBar-Element angezeigt. Wenn Sie im Menü ein Element auswählen, wird der Text in der Textansicht geändert: 

![Das Menü "Statusleiste" wird ausgeführt](menu-images/statusbar01.png "Das Menü "Statusleiste" wird ausgeführt")

Sehen wir uns nun die Erstellung benutzerdefinierter Dock Menü Elemente an.

## <a name="custom-dock-menus"></a>Benutzerdefinierte Andock Menüs

Das Menü Andocken wird für Ihre Macintosh-Anwendung angezeigt, wenn der Benutzer mit der rechten Maustaste klickt oder mit der Maus auf das Symbol der Anwendung im Dock klickt:

![Ein benutzerdefiniertes Andock Menü](menu-images/dock01.png "Ein benutzerdefiniertes Andock Menü")

Erstellen Sie ein benutzerdefiniertes Andock Menü für die Anwendung, indem Sie die folgenden Schritte ausführen:

1. Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf das Anwendungsprojekt, und wählen Sie  > **neue Datei** **Hinzufügen** ... aus. Wählen Sie im Dialogfeld neue Datei die Option **xamarin. Mac**  > **leere Schnittstellen Definition**aus, verwenden Sie "DockMenu" als **Name** , und klicken Sie auf die Schaltfläche " **neu** ", um die neue Datei " **DockMenu. XIb** " zu erstellen:

    ![Hinzufügen einer leeren Schnittstellen Definition](menu-images/dock02.png "Hinzufügen einer leeren Schnittstellen Definition")
2. Doppelklicken Sie im **Lösungspad**auf die Datei " **DockMenu. XIb** ", um Sie für die Bearbeitung in Xcode zu öffnen. Erstellen Sie ein neues **Menü** mit den folgenden Elementen **: Address**, **Date**, **Gruß**und **Signature** . 

    [![Festlegen der Benutzeroberfläche](menu-images/dock03.png "Festlegen der Benutzeroberfläche")](menu-images/dock03-large.png#lightbox)
3. Als nächstes verbinden wir unsere neuen Menü Elemente mit den vorhandenen Aktionen, die wir im Abschnitt [hinzufügen, bearbeiten und Löschen von Menüs](#Adding,_Editing_and_Deleting_Menus) für das benutzerdefinierte Menü erstellt haben. Wechseln Sie zum **Verbindungs Inspektor** , und wählen Sie den **ersten Responder** in der **Schnittstellen Hierarchie**aus. Scrollen Sie nach unten, und suchen Sie die `phraseAddress:` Aktion. Ziehen Sie eine Linie vom Kreis für diese Aktion in das **Adress** Menü Element:

    [![Ziehen, um eine Aktion zu übertragen](menu-images/dock04.png "Ziehen, um eine Aktion zu übertragen")](menu-images/dock04-large.png#lightbox)
4. Wiederholen Sie diesen Vorgang für alle anderen Menü Elemente, die an die entsprechenden Aktionen angehängt werden: 

    [![Erforderliche Aktionen](menu-images/dock05.png "Erforderliche Aktionen")](menu-images/dock05-large.png#lightbox)
5. Wählen Sie anschließend die **Anwendung** in der **Schnittstellen Hierarchie**aus. Ziehen Sie im **Verbindungs Inspektor**eine Linie aus dem Kreis des `dockMenu` Outlets in das soeben erstellte Menü:

    [![Ziehen Sie die Übertragung auf das Outlet.](menu-images/dock06.png "Ziehen Sie die Übertragung auf das Outlet.")](menu-images/dock06-large.png#lightbox)
6. Speichern Sie die Änderungen, und wechseln Sie zurück zu Visual Studio für Mac, um mit Xcode zu synchronisieren.
7. Doppelklicken Sie auf die **Info. plist** -Datei, um Sie zur Bearbeitung zu öffnen: 

    [![Bearbeiten der Datei "Info. plist"](menu-images/dock07.png "Bearbeiten der Datei "Info. plist"")](menu-images/dock07-large.png#lightbox)
8. Klicken Sie unten auf dem Bildschirm auf die Registerkarte **Quelle** : 

    [![Auswählen der Quell Ansicht](menu-images/dock08.png "Auswählen der Quell Ansicht")](menu-images/dock08-large.png#lightbox)
9. Klicken Sie auf **neuen Eintrag hinzufügen**, klicken Sie auf die grüne Plus-Schaltfläche, legen Sie den Eigenschaftsnamen auf "appledockmenu" und den Wert auf "DockMenu" (der Name der neuen XIb-Datei ohne Erweiterung) fest: 

    [![Hinzufügen des DockMenu-Elements](menu-images/dock09.png "Hinzufügen des DockMenu-Elements")](menu-images/dock09-large.png#lightbox)

Wenn wir nun die Anwendung ausführen und mit der rechten Maustaste auf das zugehörige Symbol im Dock klicken, werden die neuen Menü Elemente angezeigt:

![Ein Beispiel für das Andock Menü, das ausgeführt wird.](menu-images/dock10.png "Ein Beispiel für das Andock Menü, das ausgeführt wird.")

Wenn wir eines der benutzerdefinierten Elemente aus dem Menü auswählen, wird der Text in der Textansicht geändert.

<a name="Pop-up_Menus_and_Pull-Down_Lists" />

## <a name="pop-up-button-and-pull-down-lists"></a>Popup Schaltfläche und pulldownlisten

Eine Popup Schaltfläche zeigt ein ausgewähltes Element an und zeigt eine Liste der Optionen an, die Sie auswählen können, wenn der Benutzer darauf klickt. Eine Pulldownliste ist ein Typ von Popup Schaltfläche, der in der Regel für die Auswahl von Befehlen verwendet wird, die für den Kontext der aktuellen Aufgabe spezifisch sind. Beides kann an beliebiger Stelle in einem Fenster angezeigt werden.

Erstellen Sie eine benutzerdefinierte Popup Schaltfläche für die Anwendung, indem Sie die folgenden Schritte ausführen:

1. Bearbeiten Sie die Datei " **Main. Storyboard** " in Xcode, und ziehen Sie eine **Popup Schaltfläche** aus dem **Bibliotheks Inspektor** in das **Panel** Fenster, das im Abschnitt " [Kontextmenüs](#Contextual_Menus) " erstellt wurde: 

    [![Hinzufügen einer Popup Schaltfläche](menu-images/popup01.png "Hinzufügen einer Popup Schaltfläche")](menu-images/popup01-large.png#lightbox)
2. Fügen Sie ein neues Menü Element hinzu, und legen Sie die Titel der Elemente im Popup auf Folgendes fest: **Address**, **Date**, **Gruß**und **Signature** . 

    [![Konfigurieren der Menü Elemente](menu-images/popup02.png "Konfigurieren der Menü Elemente")](menu-images/popup02-large.png#lightbox)
3. Als nächstes verbinden wir unsere neuen Menü Elemente mit den vorhandenen Aktionen, die wir im Abschnitt [hinzufügen, bearbeiten und Löschen von Menüs](#Adding,_Editing_and_Deleting_Menus) für das benutzerdefinierte Menü erstellt haben. Wechseln Sie zum **Verbindungs Inspektor** , und wählen Sie den **ersten Responder** in der **Schnittstellen Hierarchie**aus. Scrollen Sie nach unten, und suchen Sie die `phraseAddress:` Aktion. Ziehen Sie eine Linie vom Kreis für diese Aktion in das **Adress** Menü Element: 

    [![Ziehen, um eine Aktion zu übertragen](menu-images/popup03.png "Ziehen, um eine Aktion zu übertragen")](menu-images/popup03-large.png#lightbox)
4. Wiederholen Sie diesen Vorgang für alle anderen Menü Elemente, die an die entsprechenden Aktionen angehängt werden: 

    [![Alle erforderlichen Aktionen](menu-images/popup04.png "Alle erforderlichen Aktionen")](menu-images/popup04-large.png#lightbox)
5. Speichern Sie die Änderungen, und wechseln Sie zurück zu Visual Studio für Mac, um mit Xcode zu synchronisieren.

Wenn wir nun die Anwendung ausführen und ein Element aus dem Popup auswählen, ändert sich der Text in der Textansicht:

![Ein Beispiel für die Ausführung des Popups](menu-images/popup05.png "Ein Beispiel für die Ausführung des Popups")

Sie können pulldownlisten genau wie Popup Schaltflächen erstellen und mit diesen arbeiten. Anstatt an eine vorhandene Aktion anzufügen, können Sie eigene benutzerdefinierte Aktionen wie im Kontextmenü des Abschnitts [Kontextmenüs](#Contextual_Menus) erstellen.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Menüs und Menü Elementen in einer xamarin. Mac-Anwendung ausführlich erläutert. Zuerst haben wir die Menüleiste der Anwendung untersucht. Anschließend haben wir uns mit dem Erstellen von Kontextmenüs beschäftigt. als nächstes haben wir Status leisten Menüs und benutzerdefinierte Andock Menüs überprüft. Schließlich haben wir Popup Menüs und pulldownlisten behandelt.

## <a name="related-links"></a>Verwandte Links

- [Macmenüs (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macmenus)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Richtlinien für die Benutzeroberfläche: Menüs](https://developer.apple.com/macos/human-interface-guidelines/menus/menu-anatomy/)
- [Einführung in Anwendungs Menüs und Popup Listen](https://developer.apple.com/library/content/documentation/Cocoa/Conceptual/MenuList/MenuList.html)
