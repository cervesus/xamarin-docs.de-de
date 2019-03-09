---
title: Erstellen einer Xamarin.iOS-Anwendung mithilfe der Elemente-API
description: Dieser Artikel baut auf den Informationen in der Einführung zu MonoTouch-Dialogfeld Artikel. Es bietet eine exemplarische Vorgehensweise, die zeigt, wie Sie mit der MonoTouch.Dialog (MT.) (D) Elemente API einen schnellen Einstieg Erstellen einer Anwendung mit MT. D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 14711f9cc2c34d72765e28db158379bc2a26849b
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57670285"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Erstellen einer Xamarin.iOS-Anwendung mithilfe der Elemente-API

_Dieser Artikel baut auf den Informationen in der Einführung zu MonoTouch-Dialogfeld Artikel. Es bietet eine exemplarische Vorgehensweise, die zeigt, wie Sie mit der MonoTouch.Dialog (MT.) (D) Elemente API einen schnellen Einstieg Erstellen einer Anwendung mit MT. D._

In dieser exemplarischen Vorgehensweise verwenden wir das masterziel D Elemente-API zum Erstellen eines Master / Detail-Stils, der Anwendung, die eine Aufgabenliste anzeigt. Wenn der Benutzer wählt die <span class="ui"> + </span> Schaltfläche in der Navigationsleiste wird eine neue Zeile in die Tabelle für die Aufgabe hinzugefügt werden. Auswählen der zeilenupdates wird der Detailbildschirm navigieren Sie zu, die uns so aktualisieren Sie die Beschreibung des Tasks "und" Fälligkeitsdatum, ermöglicht, wie unten gezeigt:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Auswählen der zeilenupdates wird auf den Detailbildschirm navigieren, die Bezeichnung ermöglicht uns die Taskbeschreibung und dem Fälligkeitsdatum aktualisieren")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 ## <a name="setting-up-mtd"></a>Masterziel einrichten D

MT. D ist mit Xamarin.iOS verteilt. Um es zu verwenden, mit der Maustaste auf die **Verweise** Knoten eine Xamarin.iOS-Projekt in Visual Studio 2017 oder Visual Studio für Mac und Hinzufügen eines Verweises auf die **MonoTouch.Dialog-1-** Assembly. Fügen Sie dann `using MonoTouch.Dialog` Anweisungen in Ihrer Quelle code nach Bedarf.

## <a name="elements-api-walkthrough"></a>Elemente-API-Exemplarische Vorgehensweise

In der [Einführung in MonoTouch-Dialogfeld](~/ios/user-interface/monotouch.dialog/index.md) Artikel, erhielten wir ein solides Grundwissen über die verschiedenen Teile des masterzielservers D. Wir verwenden Sie die Elemente-API, Sie sie alle zusammen in einer Anwendung einsetzen.

## <a name="setting-up-the-multi-screen-application"></a>Einrichten der multiscreen-Anwendung

Zum Starten des Prozesses der Bildschirm MonoTouch.Dialog erstellt eine `DialogViewController`, und fügt dann ein `RootElement`.

Um einer multiscreen-Anwendung mit MonoTouch.Dialog erstellen zu können, müssen wir:

1.  Erstellen einer `UINavigationController.`
1.  Erstellen einer `DialogViewController.`
1.  Hinzufügen der `DialogViewController` als Stamm der  `UINavigationController.` 
1.  Hinzufügen einer `RootElement` auf der  `DialogViewController.`
1.  Hinzufügen `Sections` und `Elements` auf der  `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>Verwenden eine UINavigationController

Um eine Anwendung für die Navigation-Stil zu erstellen, müssen wir erstellen eine `UINavigationController`, und fügen Sie es als die `RootViewController` in die `FinishedLaunching` Methode der `AppDelegate`. Vornehmen der `UINavigationController` arbeiten Sie mit MonoTouch.Dialog, fügen wir eine `DialogViewController` auf die `UINavigationController` wie unten dargestellt:

```csharp
public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
{
        _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
        _rootElement = new RootElement ("To Do List"){new Section ()};

        // code to create screens with MT.D will go here …

        _rootVC = new DialogViewController (_rootElement);
        _nav = new UINavigationController (_rootVC);
        _window.RootViewController = _nav;
        _window.MakeKeyAndVisible ();
            
        return true;
}
```

Der obige Code erstellt eine Instanz von einem `RootElement` und übergibt ihn in die `DialogViewController`. Die `DialogViewController` verfügt immer über eine `RootElement` am Anfang der Hierarchie. In diesem Beispiel die `RootElement` wird erstellt, mit der Zeichenfolge "To Do List," der als Titel des navigationscontrollers Navigationsleiste dient. An diesem Punkt würde die Ausführung der Anwendung den unten angezeigten Bildschirm darstellen:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Ausführen der Anwendung werden die hier gezeigten Bildschirm angezeigt werden.")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Sehen wir uns mit der MonoTouch.Dialog hierarchische Struktur der `Sections` und `Elements` weitere Bildschirme hinzufügen.

### <a name="creating-the-dialog-screens"></a>Erstellen die Dialogfeld-Bildschirme

Ein `DialogViewController` ist eine `UITableViewController` Unterklasse, die MonoTouch.Dialog verwendet wird, um die Bildschirme hinzufügen. MonoTouch.Dialog erstellt Bildschirme durch Hinzufügen einer `RootElement` auf eine `DialogViewController`, wie wir oben gesehen haben. Die `RootElement` können `Section` Instanzen, die in den Abschnitten einer Tabelle darstellen.
In den Abschnitten bestehen aus der Elemente, die anderen Abschnitte oder sogar anderen `RootElements`. Mit Verschachtelung `RootElements`, MonoTouch.Dialog erstellt automatisch eine Navigationsstil Anwendung als Nächstes sehen.

### <a name="using-dialogviewcontroller"></a>Verwenden von DialogViewController

Die `DialogViewController`, wird eine `UITableViewController` -Unterklasse, verfügt über eine `UITableView` als Ansicht. In diesem Beispiel möchten wir Elemente in der Tabelle jedes Mal hinzufügen der <span class="ui"> + </span> Schaltfläche getippt wird. Da die `DialogViewController` wurde hinzugefügt, um eine `UINavigationController`, verwenden wir die `NavigationItem`des `RightBarButton` hinzuzufügende Eigenschaft der <span class="ui"> + </span> Schaltfläche wie unten dargestellt:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Wir die Erstellung der `RootElement` zuvor wir übergeben sie ein einzelnes `Section` -Instanz, damit wir Elemente hinzufügen, können die <span class="ui"> + </span> Schaltfläche getippt wird, vom Benutzer. Wir können den folgenden Code verwenden, um Handler für die Schaltfläche mit den im Ereignis diesem auszuführen:

```csharp
_addButton.Clicked += (sender, e) => {
                
        ++n;
                
        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
        var taskElement = new RootElement (task.Name){
                new Section () {
                        new EntryElement (task.Name, 
                                "Enter task description", task.Description)
                },
                new Section () {
                        new DateElement ("Due Date", task.DueDate)
                }
        };
        _rootElement [0].Add (taskElement);
};
```

Dieser Code erstellt ein neues `Task` Objekt jedes Mal die Schaltfläche getippt wird. Das folgende Beispiel zeigt die einfache Implementierung der `Task` Klasse:

```csharp
public class Task
{   
        public Task ()
        {
        }
        
        public string Name { get; set; }
        
        public string Description { get; set; }

        public DateTime DueDate { get; set; }
}
```

Der Aufgabe `Name` Eigenschaft wird zum Erstellen der `RootElement`die Beschriftung zusammen mit einem Counter-Variable, die mit dem Namen `n` , die für jede neue Aufgabe erhöht wird. MonoTouch.Dialog wandelt die Elemente in die Zeilen, die hinzugefügt werden, die `TableView` bei jeder `taskElement` hinzugefügt wird.

## <a name="presenting-and-managing-dialog-screens"></a>Präsentieren und Verwalten von Dialogbildschirme

Wir verwendeten eine `RootElement` , damit MonoTouch.Dialog würde automatisch ausführliche jedes einzelnen Vorgangs einen neuen Bildschirm erstellen und zu dieser navigieren, wenn eine Zeile ausgewählt ist.

Der Task-Detail-Bildschirm selbst besteht aus zwei Abschnitten; Jeder dieser Abschnitte enthält ein einzelnes Element. Das erste Element wird erstellt, aus einer `EntryElement` der Aufgabe eine bearbeitbare Zeile bereit `Description` Eigenschaft. Wenn das Element ausgewählt ist, wird eine Tastatur für die Textbearbeitung dargestellt, wie unten dargestellt:

 [![](elements-api-walkthrough-images/03-create-task.png "Wenn das Element ausgewählt ist, wird eine Tastatur für die Textbearbeitung angezeigt, siehe")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Der zweite Abschnitt enthält eine `DateElement` können wir das Verwalten der Aufgabe `DueDate` Eigenschaft. Auswahl des Datums automatisch lädt eine Datumsauswahl, wie dargestellt:

 [![](elements-api-walkthrough-images/04-date-picker.png "Auswahl des Datums automatisch lädt eine Datumsauswahl als")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

Sowohl die `EntryElement` und `DateElement` Fällen (oder jedes Element Dateneingabe MonoTouch.Dialog), alle Änderungen auf die Werte werden automatisch beibehalten. Wir können dies demonstrieren, bearbeiten das Datum aus, und navigieren dann hin und her, zwischen dem Root-Bildschirm und verschiedene Aufgabendetails, in denen die Werte in dem Detailbildschirm beibehalten werden.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine exemplarische Vorgehensweise, die wurde gezeigt, wie Sie mit der MonoTouch.Dialog-Elemente-API. Es behandelt die grundlegenden Schritte zum Erstellen einer multiscreen-Anwendung mit MT. D, einschließlich Informationen zum Verwenden einer `DialogViewController` und zum Hinzufügen der Elemente und Abschnitte, um Bildschirme zu erstellen. Darüber hinaus wird aufgezeigt, wie Sie mit der MT. D in Verbindung mit einem `UINavigationController`.

## <a name="related-links"></a>Verwandte Links

- [MTDWalkthrough (Beispiel)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Screencast - Miguel de Icaza erstellt ein iOS-Anmeldebildschirm mit MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast - problemlos iOS Benutzeroberflächen mit MonoTouch.Dialog erstellen](http://youtu.be/j7OC5r8ZkYg)
- [Einführung in die MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise Reflection-API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Exemplarische Vorgehensweise JSON-Element](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController-Klassenreferenz](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
