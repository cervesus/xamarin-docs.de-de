---
title: Erstellen eine Benutzeroberfläche in Xamarin.iOS mithilfe von JSON
description: MonoTouch.Dialog (MT.) (D) bietet Unterstützung für die dynamische Benutzeroberfläche-Generierung über JSON-Daten. In diesem Tutorial erstellen wir schrittweise so eine JSONElement zu verwenden, zum Erstellen einer Benutzeroberfläche aus JSON-Datei, die entweder mit einer Anwendung enthalten, oder von einem remote-Url geladen.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 24d73bfcc28601f6a7ebbecb71d8c0e9f6864ac3
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50675340"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Erstellen eine Benutzeroberfläche in Xamarin.iOS mithilfe von JSON

_MonoTouch.Dialog (MT.) (D) bietet Unterstützung für die dynamische Benutzeroberfläche-Generierung über JSON-Daten. In diesem Tutorial erstellen wir schrittweise so eine JSONElement zu verwenden, zum Erstellen einer Benutzeroberfläche aus JSON-Datei, die entweder mit einer Anwendung enthalten, oder von einem remote-Url geladen._

MT. D unterstützt die Erstellung von Benutzeroberflächen, die im JSON-Format deklariert. Wenn Elemente mithilfe von JSON, masterziel deklariert werden D erstellt die zugeordneten Elemente automatisch für Sie. Der JSON-Code kann geladen werden, entweder aus einer lokalen Datei, die eine analysierte `JsonObject` Instanz oder sogar eine remote-Url.

MT. D unterstützt das gesamte Spektrum an Funktionen, die bei Verwendung von JSON in der Elemente-API verfügbar sind. Die Anwendung im folgenden Screenshot wird z. B. vollständig mit JSON deklariert:

[![](json-element-walkthrough-images/01-load-from-file.png "Zum Beispiel die Anwendung in diesem Screenshot ist vollständig deklariert mithilfe von JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![](json-element-walkthrough-images/01-load-from-file.png "z. B. die Anwendung in diesem Screenshot ist vollständig mit deklariert JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Sehen wir uns erneut das Beispiel aus der [Elemente-API-Exemplarische Vorgehensweise](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) Tutorial zeigt, wie zum Hinzufügen eines Task-Detail-Bildschirms mithilfe von JSON.

## <a name="setting-up-mtd"></a>Masterziel einrichten D

MT. D ist mit Xamarin.iOS verteilt. Um es zu verwenden, mit der Maustaste auf die **Verweise** Knoten eine Xamarin.iOS-Projekt in Visual Studio 2017 oder Visual Studio für Mac und Hinzufügen eines Verweises auf die **MonoTouch.Dialog-1-** Assembly. Fügen Sie dann `using MonoTouch.Dialog` Anweisungen in Ihrer Quelle code nach Bedarf.

## <a name="json-walkthrough"></a>JSON-Exemplarische Vorgehensweise

Im Beispiel in dieser exemplarischen Vorgehensweise können Vorgänge erstellt werden. Wenn eine Aufgabe auf dem ersten Bildschirm ausgewählt ist, erhält ein Detailbildschirm, wie gezeigt:

 [![](json-element-walkthrough-images/03-task-list.png "Wenn eine Aufgabe auf dem ersten Bildschirm ausgewählt ist, wird ein Detail-Bildschirm angezeigt, wie gezeigt")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Erstellen den JSON-Code

In diesem Beispiel laden wir den JSON-Code aus einer Datei in das Projekt mit dem Namen `task.json`. MT. D erwartet, dass den JSON-Code, um eine Syntax zu entsprechen, die die Elemente-API widerspiegelt. Vergleichbar mit dem die Elemente-API von Code verwenden, wenn Sie JSON verwenden, deklarieren wir Abschnitte aus, und in diesen Abschnitten Elemente hinzufügen. Um Abschnitte und Elemente im JSON-Format zu deklarieren, verwenden wir die Zeichenfolgen "Abschnitte" und "Elemente" bzw. als Schlüssel. Für jedes Element wird der Typ des zugeordneten Elements festgelegt, mit der `type` Schlüssel. Alle anderen Elemente-Eigenschaft wird mit dem Eigenschaftennamen als Schlüssel festgelegt.

Die folgende JSON-Code beschreibt zum Beispiel den Abschnitten und die Elemente für die Taskdetails:

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

Beachten Sie, dass der obigen JSON-Code enthält eine Id für jedes Element. Eine Id, um es zur Laufzeit verweisen kann ein beliebiges Element einschließen. Es wird angezeigt, wie dies in Kürze verwendet wird beim zeigen wir, wie den JSON-Code im Code zu laden.

## <a name="loading-the-json-in-code"></a>Laden den JSON-Code im code

Nachdem Sie der JSON-Code definiert haben, müssen wir zum Laden in MT. D mithilfe der `JsonElement` Klasse. Vorausgesetzt, eine Datei mit den oben erstellten JSON-Code wurde mit dem Namen sample.json zum Projekt hinzugefügt und erhält eine Buildaktion der Inhalt, laden die `JsonElement` ist so einfach wie das Aufrufen der folgenden Codezeile:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Da wir Hinzufügen dieses bei Bedarf jedes Mal, dass eine Aufgabe erstellt wird, können wir die Schaltfläche geklickt aus dem vorherigen Elemente-API-Beispiel wie folgt ändern:

```csharp
_addButton.Clicked += (sender, e) => {

        ++n;

        var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

        var taskElement = JsonElement.FromFile ("task.json");

        _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>Zugreifen auf Elemente zur Laufzeit

Denken Sie daran, dass wir eine Id für beide Elemente hinzugefügt, wenn wir sie in der JSON-Datei deklariert. Wir können die Id-Eigenschaft verwenden, auf jedes Element zur Laufzeit, um ihre Eigenschaften im Code zu ändern. Beispielsweise verweist der folgende Code die Elemente Einstiegs- und Datum, um die Werte aus dem Taskobjekt festzulegen:

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

## <a name="loading-json-from-a-url"></a>Laden von JSON aus einer url

MT. D unterstützt auch eine dynamisch Laden von JSON aus einer externen Url übergeben Sie einfach die Url an den Konstruktor des der `JsonElement`. MT. D wird erweitert, die Hierarchie, die in der JSON-Code nach Bedarf deklariert wird, wie Sie zwischen Bildschirmen navigieren. Betrachten Sie beispielsweise eine JSON-Datei wie unten befindet sich im Stammverzeichnis des lokalen Webservers:

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

Wir können laden das mit der `JsonElement` wie im folgenden Code:

```csharp
_rootElement = new RootElement ("Json Example"){
        new Section (""){ new JsonElement ("Load from url",
                "http://localhost/sample.json")
        }
};
```

Zur Laufzeit wird werden die Datei abgerufen und analysiert werden masterziel sein D, wenn der Benutzer auf die zweite Ansicht ist, navigiert wird, wie im folgenden Screenshot gezeigt:

 [![](json-element-walkthrough-images/04-json-web-example.png "Die Datei wird abgerufen und analysiert MT. D, wenn der Benutzer, um die zweite Ansicht navigiert")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie erstellen Sie eine mit Schnittstelle mit MT. D aus JSON-Datei. Es wurde gezeigt, wie beim Laden von JSON in einer Datei mit der Anwendung als auch von einer remote-Url enthalten. Es hat auch gezeigt, wie Sie den Zugriff auf Elemente, die zur Laufzeit in JSON beschrieben.

## <a name="related-links"></a>Verwandte Links

- [MTDJsonDemo (Beispiel)](https://developer.xamarin.com/samples/MTDJsonDemo/)
- [Screencast - Miguel de Icaza erstellt ein iOS-Anmeldebildschirm mit MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast - problemlos iOS Benutzeroberflächen mit MonoTouch.Dialog erstellen](http://youtu.be/j7OC5r8ZkYg)
- [Einführung in die MonoTouch.Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise Elemente-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise Reflection-API](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController-Klassenreferenz](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
