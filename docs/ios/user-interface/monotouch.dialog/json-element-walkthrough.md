---
title: Erstellen eine Benutzeroberfläche in Xamarin.iOS mithilfe von JSON
description: MonoTouch.Dialog (MT. D) bietet Unterstützung für dynamische UI generieren über JSON-Daten. In diesem Lernprogramm werden protokollsuchen wie eine JSONElement verwenden, um eine neue Benutzeroberfläche aus JSON zu erstellen, die entweder mit einer Anwendung enthalten, oder von einem remote-Url geladen.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f9ba2cce1650260aa889e8282c091012ef8bbddc
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790652"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Erstellen eine Benutzeroberfläche in Xamarin.iOS mithilfe von JSON

_MonoTouch.Dialog (MT. D) bietet Unterstützung für dynamische UI generieren über JSON-Daten. In diesem Lernprogramm werden protokollsuchen wie eine JSONElement verwenden, um eine neue Benutzeroberfläche aus JSON zu erstellen, die entweder mit einer Anwendung enthalten, oder von einem remote-Url geladen._

MT. D unterstützt beim Erstellen von Benutzeroberflächen, die im JSON-Format deklariert. Wenn Elemente deklariert werden JSON, MT. mit D wird automatisch die zugehörigen Elemente für Sie erstellt. JSON kann geladen werden, entweder von einer lokalen Datei, die eine analysierte `JsonObject` Instanz oder sogar eine remote-Url.

MT. D unterstützt die volle Bandbreite von Funktionen, die bei Verwendung von JSON in der API-Elemente verfügbar sind. Die Anwendung im folgenden Screenshot wird z. B. vollständig mit JSON deklariert:

[![](json-element-walkthrough-images/01-load-from-file.png "Zum Beispiel die Anwendung in diesem Screenshot ist vollständig deklariert mit JSON") ](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![ ] (json-element-walkthrough-images/01-load-from-file.png "z. B. die Anwendung in diesem Screenshot ist vollständig mit deklarierten JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Wir rufen Sie das Beispiel aus der [Elemente API Exemplarische Vorgehensweise](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) Lernprogramm zum Hinzufügen eines Aufgabe Detailbildschirm mit JSON.

## <a name="json-walkthrough"></a>JSON-Exemplarische Vorgehensweise

Im Beispiel für diese exemplarische Vorgehensweise können Vorgänge erstellt werden. Wenn eine Aufgabe auf dem ersten Bildschirm ausgewählt ist, erhält ein Detailbildschirm, wie dargestellt:

 [![](json-element-walkthrough-images/03-task-list.png "Wenn eine Aufgabe auf dem ersten Bildschirm ausgewählt ist, ein Detailbildschirm erhält gezeigten")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Erstellen die JSON-Objekte

In diesem Beispiel laden wir die JSON-Objekte aus einer Datei in das Projekt mit dem Namen `task.json`. MT. D erwartet, dass die JSON entsprechen, eine Syntax, die die API-Elemente sind. Wie Sie mithilfe der API-Elemente, aus Code bei Verwendung von JSON, es wurden die Abschnitte deklariert und in solchen Abschnitten wir Elemente hinzufügen. Um Abschnitte und Elemente im JSON-Format zu deklarieren, verwenden wir die Zeichenfolgen "Abschnitte" und "Elemente" als Schlüssel. Für jedes Element wird der zugeordnete Elementtyp festgelegt, mit der `type` Schlüssel. Alle anderen Elemente-Eigenschaft wird mit dem Eigenschaftsnamen als Schlüssel festgelegt.

Die folgenden JSON-Code beschreibt z. B. den Abschnitten und die Elemente für die Taskdetails auf:

```csharp
{
    "title": "Task",
    "sections": [
        {
          "elements" : [
            {
                "id" : "task-description",
                "type": "entry",
                "placeholder": "Enter task description"
            },
            {
                "id" : "task-duedate",
                "type": "date",
                "caption": "Due Date",
                "value": "00:00"
            }
         ]
        }
    ]
  }
```

Beachten Sie, dass die oben genannten JSON enthält eine Id für jedes Element. Jedes Element zählen eine Id, um zur Laufzeit zu verweisen. Wir sehen wie dies im Anschluss verwendet wird beim veranschaulichen wir, wie JSON in Code zu laden.

 <a name="Loading_the_JSON_in_Code" />


## <a name="loading-the-json-in-code"></a>Laden von JSON in Code

Sobald die JSON definiert wurde, muss er in MT. geladen werden D mithilfe der `JsonElement` Klasse. Vorausgesetzt, eine Datei mit der oben erstellten JSON wurde dem Projekt mit dem Namen sample.json hinzugefügt und erhält einen Buildvorgang von Inhalten, die beim Laden der `JsonElement` ist so einfach wie das Aufrufen der folgenden Codezeile:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Da wir hinzufügen dies bei Bedarf jedes Mal, dass eine Aufgabe erstellt wird, können wir die Schaltfläche geklickt aus dem oben aufgeführten Elemente-API-Beispiel wie folgt ändern:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

 <a name="Accessing_Elements_at_Runtime" />


## <a name="accessing-elements-at-runtime"></a>Zugreifen auf Elemente zur Laufzeit

Denken Sie daran, dass wir eine Id für beide Elemente hinzugefügt, wenn wir diese in der JSON-Datei deklariert. Wir können die Id-Eigenschaft verwenden, um jedes Element zur Laufzeit so ändern Sie ihre Eigenschaften im Code zugreifen. Der folgende Code verweist, beispielsweise die Einstiegs- und Datum Elemente zum Festlegen der Werte aus dem Taskobjekt:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        taskElement.Caption = task.Name;

        var description = taskElement ["task-description"] as EntryElement;

        if (description != null) {
                description.Caption = task.Name;
                description.Value = task.Description;       
        }

        var duedate = taskElement ["task-duedate"] as DateElement;

        if (duedate != null) {                
                duedate.DateValue = task.DueDate;
        }
        _rootElement [0].Add (taskElement);
};
```

 <a name="Loading_JSON_from_a_Url" />


## <a name="loading-json-from-a-url"></a>Laden von JSON aus einer Url

MT. D unterstützt auch JSON dynamisch aus einer externen Url laden, indem Sie einfach die Url an den Konstruktor zu übergeben der `JsonElement`. MT. D wird die Hierarchie in der JSON-bedarfsgesteuert deklariert wird, während der Navigation zwischen Bildschirmen erweitert. Betrachten Sie beispielsweise eine JSON-Datei wie unten im Stammverzeichnis des Webservers lokalen:

```csharp
{
    "type": "root",
    "title": "home",
    "sections": [
       {
         "header": "Nested view!",
         "elements": [
           {
             "type": "boolean",
             "caption": "Just a boolean",
             "id": "first-boolean",
             "value": false
           },
           {
             "type": "string",
             "caption": "Welcome to the nested controller"
           }
         ]
       }
     ]
}
```

Wir können Laden Sie dies mit der `JsonElement` wie im folgenden Code:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

Zur Laufzeit wird wird die Datei abgerufen und analysiert werden MT. sein D, wenn der Benutzer auf die zweite Ansicht navigiert, wie im folgenden Screenshot gezeigt:

 [![](json-element-walkthrough-images/04-json-web-example.png "Die Datei abgerufen und analysiert werden MT. D, wenn der Benutzer in der zweiten Ansicht navigiert")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie ein erstellt Schnittstelle mit MT. D aus JSON. Es wurde gezeigt, wie beim Laden von JSON in eine Datei mit der Anwendung sowie von einem remote-Url enthalten. Es wurde gezeigt, wie Elemente im JSON-Format beschrieben wird, zur Laufzeit zugegriffen werden.


## <a name="related-links"></a>Verwandte Links

- [MTDJsonDemo (Beispiel)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Screencast - Miguel de Icaza erstellt ein iOS-Anmeldebildschirm mit MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast - problemlos iOS Benutzeroberflächen mit MonoTouch.Dialog erstellen](http://youtu.be/j7OC5r8ZkYg)
- [Einführung in MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise Elemente-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise Reflektion-API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController-Klassenreferenz](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
