---
title: Exemplarische Vorgehensweise – erstellen einer Anwendung mithilfe der API-Elemente
description: In diesem Artikel baut auf den Informationen in der Einführung MonoTouch Dialogfeld Artikel. Er enthält eine exemplarische Vorgehensweise verwenden Sie die MonoTouch.Dialog (MT. D) Elemente API zum schnellen Einstieg Erstellen einer Anwendung mit MT. D.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e4fbf744c6f967d09e0033212024c2e2398fb768
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough---creating-an-application-using-the-elements-api"></a>Exemplarische Vorgehensweise – erstellen einer Anwendung mithilfe der API-Elemente

_In diesem Artikel baut auf den Informationen in der Einführung MonoTouch Dialogfeld Artikel. Er enthält eine exemplarische Vorgehensweise verwenden Sie die MonoTouch.Dialog (MT. D) Elemente API zum schnellen Einstieg Erstellen einer Anwendung mit MT. D._

In dieser exemplarischen Vorgehensweise verwenden wir die MT. D-API-Elemente einen Master / Detail-Stil der Anwendung zu erstellen, eine Aufgabenliste anzeigt. Wenn der Benutzer wählt die <span class="ui"> + </span> Schaltfläche in der Navigationsleiste wird eine neue Zeile zur Tabelle für die Aufgabe hinzugefügt werden. Auswählen der zeilenupdates wird auf dem Detailbildschirm navigieren, die uns so aktualisieren Sie die Beschreibung der Aufgabe und das Fälligkeitsdatum zulässt, wie unten gezeigt:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Auswählen der zeilenupdates wird zu den Detailbildschirm navigieren, die zum Aktualisieren der Beschreibung der Aufgabe und das Fälligkeitsdatum ermöglicht")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 <a name="Elements_API_Walkthrough" />


## <a name="elements-api-walkthrough"></a>Exemplarische Vorgehensweise Elemente-API

In der [Einführung zum Dialogfeld "MonoTouch"](~/ios/user-interface/monotouch.dialog/index.md) Artikel erhalten wir ein gutes Verständnis der verschiedenen Teile des MT. D. Ermöglicht die Verwendung der API-Elemente, diese jedoch in eine Anwendung eingefügt werden soll.

 <a name="Setting_up_the_Multi-Screen_Application" />


## <a name="setting-up-the-multi-screen-application"></a>Einrichten der Anwendung für Multi-Bildschirm

Um den Erstellungsprozess für den Bildschirm zu starten, MonoTouch.Dialog erstellt eine `DialogViewController`, und fügt dann ein `RootElement`.

Um eine Anwendung mit mehreren Bildschirm mit MonoTouch.Dialog erstellen zu können, muss Folgendes:

1.  Erstellen Sie eine  `UINavigationController.`
1.  Erstellen Sie eine  `DialogViewController.`
1.  Hinzufügen der `DialogViewController` als Stammverzeichnis für die  `UINavigationController.` 
1.  Hinzufügen einer `RootElement` auf der  `DialogViewController.`
1.  Hinzufügen `Sections` und `Elements` auf die  `RootElement.` 


 <a name="Using_A_UINavigationController" />


### <a name="using-a-uinavigationcontroller"></a>Verwenden eine UINavigationController

Zum Erstellen einer Anwendung im Stil Navigation wir erstellen müssen eine `UINavigationController`, und fügen Sie es als die `RootViewController` in der `FinishedLaunching` Methode der `AppDelegate`. Vornehmen der `UINavigationController` arbeiten mit MonoTouch.Dialog, fügen wir eine `DialogViewController` auf die `UINavigationController` wie unten dargestellt:

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

Der obige Code erstellt eine Instanz von einem `RootElement` und übergibt ihn in die `DialogViewController`. Die `DialogViewController` verfügt immer über einen `RootElement` am Anfang der Hierarchie. In diesem Beispiel wird die `RootElement` wird erstellt, mit der Zeichenfolge "Aufgabenliste" die als Titel des Controllers für die Navigation Navigationsleiste dient. An diesem Punkt würde Ausführen der Anwendung den Bildschirm unten dargestellt werden:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "Ausführen der Anwendung wird den hier gezeigten Bildschirm vorgelegt.")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Sehen wir, wie mithilfe des MonoTouch.Dialog hierarchischen Struktur von `Sections` und `Elements` weitere Bildschirme hinzufügen.

 <a name="Creating_the_Dialog_Screens" />


### <a name="creating-the-dialog-screens"></a>Erstellen das Dialogfeld Bildschirme

Ein `DialogViewController` ist eine `UITableViewController` Unterklasse, die MonoTouch.Dialog verwendet wird, um Bildschirme hinzuzufügen. MonoTouch.Dialog erstellt Bildschirme durch Hinzufügen einer `RootElement` auf eine `DialogViewController`, wie oben veranschaulichte Filtermenü. Die `RootElement` können `Section` Instanzen, die in den Abschnitten in einer Tabelle darstellen.
In den Abschnitten bestehen aus Elementen, die anderen Abschnitte oder sogar andere `RootElements`. Durch die Schachtelung `RootElements`, MonoTouch.Dialog erstellt automatisch eine Anwendung Navigation-Format, wie wir als Nächstes sehen.

 <a name="Using_DialogViewController" />


### <a name="using-dialogviewcontroller"></a>Verwenden von DialogViewController

Die `DialogViewController`, wird eine `UITableViewController` -Unterklasse, verfügt über eine `UITableView` als seinen Ansichtszustand. In diesem Beispiel wir hinzuzufügenden Elemente in der Tabelle jedes Mal die <span class="ui"> + </span> Schaltfläche abgerufen wird. Da die `DialogViewController` wurde hinzugefügt, um eine `UINavigationController`, verwenden wir die `NavigationItem`des `RightBarButton` hinzuzufügenden Eigenschaft der <span class="ui"> + </span> Schaltfläche, wie unten dargestellt:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Wir der Erstellung der `RootElement` früher wir übergeben sie ein einzelnes `Section` Instanz fest, damit Elemente als eingerichtet werden konnte die <span class="ui"> + </span> Schaltfläche vom Benutzer abgerufen wird. Wir können den folgenden Code im Ereignis diesem Ereignishandler für die Schaltfläche ausführen:

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

Dieser Code erstellt ein neues `Task` Objekt jedes Mal die Schaltfläche abgerufen wird. Nachstehend sehen Sie die einfache Implementierung der `Task` Klasse:

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

 []()

Des Tasks `Name` Eigenschaft dient zum Erstellen der `RootElement`des Beschriftung zusammen mit der eine Zählervariable, die mit dem Namen `n` , wird für jeden neuen Vorgang erhöht. MonoTouch.Dialog wandelt die Elemente in den Zeilen, die hinzugefügt werden die `TableView` bei jeder `taskElement` hinzugefügt wird.

 <a name="Presenting_and_Managing_Dialog_Screens" />


## <a name="presenting-and-managing-dialog-screens"></a>Bereitstellen und Verwalten von Dialogfelder

Es verwendet ein `RootElement` , damit MonoTouch.Dialog würde automatisch einen neuen Bildschirm für jede Aufgabe Details zu erstellen und ihm navigieren, wenn eine Zeile ausgewählt ist.

Der Task Detailbildschirm selbst besteht aus zwei Abschnitten; den folgenden Abschnitten enthält ein einzelnes Element. Das erste Element wird erstellt, aus einer `EntryElement` eine bearbeitbare Zeile für der Aufgabe bereitstellen `Description` Eigenschaft. Wenn das Element ausgewählt ist, erhält eine Tastatur für die Textbearbeitung, wie unten dargestellt:

 [![](elements-api-walkthrough-images/03-create-task.png "Wenn das Element ausgewählt ist, wird eine Tastatur für die Textbearbeitung angezeigt, wie gezeigt")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Der zweite Abschnitt enthält eine `DateElement` , mit der uns die Verwaltung der Aufgabe `DueDate` Eigenschaft. Auswahl des Datums automatisch lädt eine Datumsauswahl wie gezeigt:

 [![](elements-api-walkthrough-images/04-date-picker.png "Auswahl des Datums automatisch lädt eine Datumsauswahl als")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

Sowohl die `EntryElement` und `DateElement` Fällen (oder für ein bestimmtes Element im MonoTouch.Dialog-Dateneingabe), werden automatisch alle Änderungen an den Werten beibehalten. Wir können dadurch veranschaulichen, bearbeiten das Datum und und dann weiter navigieren hin-und zwischen dem Stamm Bildschirm und verschiedene Aufgabendetails, in denen die Werte in den Bildschirmen der Detail beibehalten werden.

 <a name="Summary" />


## <a name="summary"></a>Zusammenfassung

In diesem Artikel dargestellt eine exemplarische Vorgehensweise, die wurde gezeigt, wie die MonoTouch.Dialog Elemente-API verwenden. Er behandelt die grundlegenden Schritte zum Erstellen einer Anwendung mit mehreren Bildschirm mit MT. D, einschließlich Informationen zum Verwenden einer `DialogViewController` sowie zum Hinzufügen von Elementen und Abschnitte, um Bildschirme zu erstellen. Darüber hinaus wurde gezeigt, es zum Verwenden von MT. D in Verbindung mit einem `UINavigationController`.


## <a name="related-links"></a>Verwandte Links

- [MTDWalkthrough (sample)](https://developer.xamarin.com/samples/MTDWalkthrough/)
- [Screencast - Miguel de Icaza erstellt ein iOS-Anmeldebildschirm mit MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast - problemlos iOS Benutzeroberflächen mit MonoTouch.Dialog erstellen](http://youtu.be/j7OC5r8ZkYg)
- [Einführung in MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise Reflektion-API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Exemplarische Vorgehensweise JSON-Element](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController-Klassenreferenz](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
