---
title: Erstellen einer xamarin. IOS-Anwendung mit der Elements-API
description: Dieser Artikel baut auf den Informationen auf, die im Dialog Feld Einführung in MonoTouch vorgestellt wurden. Sie enthält eine exemplarische Vorgehensweise, die zeigt, wie Sie das MonoTouch. Dialog (MT) verwenden. D) Elemente-API, um schnell mit dem Entwickeln einer Anwendung mit MT zu beginnen. D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 2258b2e8451f896aff59c89478833976ef7086e3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002372"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Erstellen einer xamarin. IOS-Anwendung mit der Elements-API

_Dieser Artikel baut auf den Informationen auf, die im Dialog Feld Einführung in MonoTouch vorgestellt wurden. Sie enthält eine exemplarische Vorgehensweise, die zeigt, wie Sie das MonoTouch. Dialog (MT) verwenden. D) Elemente-API, um schnell mit dem Entwickeln einer Anwendung mit MT zu beginnen. D._

In dieser exemplarischen Vorgehensweise verwenden wir das Mt. D Elements-API zum Erstellen eines Master/Detail-Anwendungs Stils, in dem eine Aufgabenliste angezeigt wird. Wenn der Benutzer die Schaltfläche **+** in der Navigationsleiste auswählt, wird der Tabelle für den Task eine neue Zeile hinzugefügt. Wenn Sie die Zeile auswählen, navigieren Sie zum Detailbildschirm, mit dem wir die Aufgabenbeschreibung und das Fälligkeitsdatum aktualisieren können, wie unten dargestellt:

[![](elements-api-walkthrough-images/01-task-list-app.png "Selecting the row will navigate to the detail screen that allows us to update the task description and the due date")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

## <a name="setting-up-mtd"></a>Einrichten von Mt. D

UV. D wird mit xamarin. IOS verteilt. Um es zu verwenden, klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** eines xamarin. IOS-Projekts in Visual Studio 2017 oder Visual Studio für Mac, und fügen Sie einen Verweis auf die Assembly **MonoTouch. Dialog-1** hinzu. Fügen Sie dann `using MonoTouch.Dialog`-Anweisungen in Ihrem Quellcode nach Bedarf hinzu.

## <a name="elements-api-walkthrough"></a>Exemplarische Vorgehensweise zu Elementen

Im [Dialog Artikel Introduction to MonoTouch](~/ios/user-interface/monotouch.dialog/index.md) haben wir ein solides Verständnis der verschiedenen Teile von MT erhalten. D. Verwenden Sie die Elements-API, um alle Elemente in einer Anwendung zu platzieren.

## <a name="setting-up-the-multi-screen-application"></a>Einrichten der Multiscreen-Anwendung

Um den Prozess der Bildschirm Erstellung zu starten, erstellt MonoTouch. Dialog eine `DialogViewController`und fügt dann eine `RootElement`hinzu.

Zum Erstellen einer Anwendung mit mehreren Bildschirmen mit MonoTouch. Dialog ist Folgendes erforderlich:

1. Erstellen einer `UINavigationController.`
1. Erstellen einer `DialogViewController.`
1. Fügen Sie die `DialogViewController` als Stammverzeichnis des `UINavigationController.` 
1. Fügen Sie der `DialogViewController.` ein `RootElement` hinzu.
1. Fügen Sie `Sections` und `Elements` dem `RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>Verwenden eines UINavigationController

Zum Erstellen einer Anwendung im navigationsstil müssen wir eine `UINavigationController`erstellen und Sie dann als `RootViewController` in der `FinishedLaunching`-Methode der `AppDelegate`hinzufügen. Um den `UINavigationController` mit MonoTouch. Dialog zu machen, fügen wir dem `UINavigationController` eine `DialogViewController` hinzu, wie unten dargestellt:

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
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

Der obige Code erstellt eine Instanz einer `RootElement` und übergibt sie an die `DialogViewController`. Die `DialogViewController` hat immer eine `RootElement` oben in der Hierarchie. In diesem Beispiel wird der `RootElement` mit der Zeichenfolge "to do list" erstellt, die als Titel in der Navigationsleiste des Navigations Controllers fungiert. An diesem Punkt wird durch Ausführen der Anwendung der folgende Bildschirm angezeigt:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Running the application will present the screen shown here")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Sehen wir uns an, wie die hierarchische Struktur von "MonoTouch. Dialog" `Sections` und `Elements` verwendet wird, um weitere Bildschirme hinzuzufügen.

### <a name="creating-the-dialog-screens"></a>Erstellen der Dialog Bildschirme

Ein `DialogViewController` ist eine `UITableViewController` Unterklasse, die von MonoTouch. Dialog zum Hinzufügen von Bildschirmen verwendet wird. MonoTouch. Dialog erstellt Bildschirme, indem einem `DialogViewController`ein `RootElement` hinzugefügt wird, wie oben gezeigt. Der `RootElement` kann `Section` Instanzen aufweisen, die die Abschnitte einer Tabelle darstellen.
Die Abschnitte bestehen aus Elementen, anderen Abschnitten oder sogar anderen `RootElements`. Durch die Schachtelung `RootElements`wird von MonoTouch. Dialog automatisch eine Anwendung im navigationsstil erstellt, wie im nächsten Schritt zu sehen.

### <a name="using-dialogviewcontroller"></a>Verwenden von dialogviewcontroller

Der `DialogViewController`, der `UITableViewController` Unterklasse ist, verfügt über einen `UITableView` als seine Ansicht. In diesem Beispiel möchten wir jedes Mal, wenn die Schaltfläche " **+** " abgetippt wird, Elemente zur Tabelle hinzufügen. Da der `DialogViewController` einem `UINavigationController`hinzugefügt wurde, können wir die **+** -Schaltfläche mit der `RightBarButton`-Eigenschaft des `NavigationItem`hinzufügen, wie unten dargestellt:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Als wir die `RootElement` zuvor erstellt haben, haben wir ihr eine einzelne `Section` Instanz weitergegeben, damit wir Elemente hinzufügen können, da der Benutzer die Schaltfläche **+** tippt. Sie können den folgenden Code verwenden, um dies im-Ereignishandler für die Schaltfläche zu erreichen:

```csharp
_addButton.Clicked += (sender, e) => {                
    ++n;
                
    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
    var taskElement = new RootElement (task.Name) {
        new Section () {
            new EntryElement (task.Name, "Enter task description", task.Description)
        },
        new Section () {
            new DateElement ("Due Date", task.DueDate)
        }
    };
    _rootElement [0].Add (taskElement);
};
```

Mit diesem Code wird jedes Mal ein neues `Task` Objekt erstellt, wenn die Schaltfläche getippt wird. Das folgende Beispiel zeigt die einfache Implementierung der `Task`-Klasse:

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

Die `Name`-Eigenschaft der Aufgabe wird verwendet, um die Beschriftung des `RootElement`zusammen mit einer Counter-Variablen mit dem Namen `n` zu erstellen, die für jede neue Aufgabe inkrementiert wird. "MonoTouch. Dialog" wandelt die Elemente in die Zeilen um, die der `TableView` hinzugefügt werden, wenn jede `taskElement` hinzugefügt wird.

## <a name="presenting-and-managing-dialog-screens"></a>Anzeigen und Verwalten von Dialog Schirmen

Wir haben eine `RootElement` verwendet, sodass MonoTouch. Dialog automatisch einen neuen Bildschirm für die Details der einzelnen Aufgaben erstellt und dorthin navigiert, wenn eine Zeile ausgewählt wird.

Der Task Detailbildschirm selbst besteht aus zwei Abschnitten: jeder dieser Abschnitte enthält ein einzelnes-Element. Das erste Element wird aus einer `EntryElement` erstellt, um eine bearbeitbare Zeile für die `Description`-Eigenschaft der Aufgabe bereitzustellen. Wenn das Element ausgewählt ist, wird eine Tastatur für die Textbearbeitung wie unten dargestellt angezeigt:

 [![](elements-api-walkthrough-images/03-create-task.png "When the element is selected, a keyboard for text editing is presented as shown")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Der zweite Abschnitt enthält eine `DateElement`, mit der wir die `DueDate`-Eigenschaft der Aufgabe verwalten können. Wenn Sie das Datum auswählen, wird eine Datumsauswahl automatisch wie folgt geladen:

 [![](elements-api-walkthrough-images/04-date-picker.png "Selecting the date automatically loads a date picker as")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

In den `EntryElement`-und `DateElement` Fällen (oder für beliebige Dateneingabe Elemente in MonoTouch. Dialog) werden alle Änderungen an den Werten automatisch beibehalten. Wir können dies veranschaulichen, indem wir das Datum bearbeiten und dann zwischen dem Stamm Bildschirm und den verschiedenen Aufgaben Details navigieren, wo die Werte in den Detail Bildschirmen beibehalten werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine exemplarische Vorgehensweise vorgestellt, die die Verwendung der monoouch. Dialog-Element-API veranschaulicht hat. Es wurden die grundlegenden Schritte zum Erstellen einer Anwendung mit mehreren Bildschirmen mit MT behandelt. D, einschließlich der Verwendung einer `DialogViewController` und Hinzufügen von Elementen und Abschnitten zum Erstellen von Bildschirmen. Außerdem wurde gezeigt, wie Sie MT verwenden können. D in Verbindung mit einem `UINavigationController`.

## <a name="related-links"></a>Verwandte Links

- [Mtdwalkthrough (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdwalkthrough)
- [Einführung in MonoTouch. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise zur Reflektion](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Exemplarische Vorgehensweise für JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch-Dialog Feld auf GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Tweetstation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [Uitableviewcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassen Verweis](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
