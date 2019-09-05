---
title: Verwenden von JSON zum Erstellen einer Benutzeroberfläche in xamarin. IOS
description: MonoTouch. Dialog (Mt. D) bietet Unterstützung für die dynamische Generierung von Benutzeroberflächen über JSON-Daten. In diesem Tutorial wird erläutert, wie Sie ein jsonelement verwenden, um eine Benutzeroberfläche aus JSON zu erstellen, die entweder in einer Anwendung enthalten ist oder von einer Remote-URL geladen wird.
ms.prod: xamarin
ms.assetid: E353DF14-51D7-98E3-59EA-16683C770C23
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: conceptdev
ms.author: crdun
ms.openlocfilehash: c7deda17a7a4936f000fbfce285b3dc3932795e2
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292277"
---
# <a name="using-json-to-create-a-user-interface-in-xamarinios"></a>Verwenden von JSON zum Erstellen einer Benutzeroberfläche in xamarin. IOS

_MonoTouch. Dialog (Mt. D) bietet Unterstützung für die dynamische Generierung von Benutzeroberflächen über JSON-Daten. In diesem Tutorial wird erläutert, wie Sie ein jsonelement verwenden, um eine Benutzeroberfläche aus JSON zu erstellen, die entweder in einer Anwendung enthalten ist oder von einer Remote-URL geladen wird._

UV. D unterstützt das Erstellen von in JSON deklarierten Benutzeroberflächen. Wenn Elemente mit JSON deklariert werden, Mt. D erstellt die zugeordneten Elemente automatisch für Sie. Der JSON-Code kann entweder aus einer lokalen Datei, einer `JsonObject` analysierten Instanz oder sogar aus einer Remote-URL geladen werden.

UV. D unterstützt die vollständige Palette der Features, die in der Elements-API verfügbar sind, wenn JSON verwendet wird. Beispielsweise wird die Anwendung im folgenden Screenshot vollständig mit JSON deklariert:

[![](json-element-walkthrough-images/01-load-from-file.png "Zum Beispiel die Anwendung in diesem Screenshot ist vollständig deklariert mithilfe von JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox) [ ![](json-element-walkthrough-images/01-load-from-file.png "z. B. die Anwendung in diesem Screenshot ist vollständig mit deklariert JSON")](json-element-walkthrough-images/01-load-from-file.png#lightbox)

Sehen wir uns das Beispiel aus dem Tutorial zu den [Element-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md) -exemplarischen Vorgehensweisen an, das zeigt, wie ein Task Detailbildschirm mithilfe von JSON hinzugefügt

## <a name="setting-up-mtd"></a>Einrichten von Mt. D

UV. D wird mit xamarin. IOS verteilt. Um es zu verwenden, klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** eines xamarin. IOS-Projekts in Visual Studio 2017 oder Visual Studio für Mac, und fügen Sie einen Verweis auf die Assembly **MonoTouch. Dialog-1** hinzu. Fügen Sie dann `using MonoTouch.Dialog` ggf. Anweisungen im Quellcode hinzu.

## <a name="json-walkthrough"></a>Exemplarische Vorgehensweise für JSON

Im Beispiel für diese exemplarische Vorgehensweise können Aufgaben erstellt werden. Wenn ein Task auf dem ersten Bildschirm ausgewählt wird, wird wie im folgenden dargestellt ein Detailbildschirm angezeigt:

 [![](json-element-walkthrough-images/03-task-list.png "Wenn ein Task auf dem ersten Bildschirm ausgewählt wird, wird ein Detailbildschirm angezeigt.")](json-element-walkthrough-images/03-task-list.png#lightbox)

## <a name="creating-the-json"></a>Erstellen der JSON-Datei

In diesem Beispiel laden wir den JSON-Code aus einer Datei im Projekt mit dem `task.json`Namen. UV. D erwartet, dass der JSON-Code einer Syntax entspricht, die die Elements-API widerspiegelt. Genau wie bei der Verwendung der Elements-API aus Code deklarieren wir bei der Verwendung von JSON Abschnitte. in diesen Abschnitten fügen wir Elemente hinzu. Um Abschnitte und Elemente in JSON zu deklarieren, werden die Zeichen folgen "Abschnitte" und "Elemente" als Schlüssel verwendet. Für jedes Element wird der zugeordnete Elementtyp mithilfe des `type` Schlüssels festgelegt. Jede andere Elements-Eigenschaft wird mit dem Eigenschaftsnamen als Schlüssel festgelegt.

Der folgende JSON-Code beschreibt z. b. die Abschnitte und Elemente für die Aufgaben Details:

```json
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

Beachten Sie, dass die oben genannte JSON-Datei eine ID für jedes Element enthält. Jedes Element kann eine ID enthalten, um zur Laufzeit darauf zu verweisen. Wir sehen, wie dies in einem Moment verwendet wird, in dem gezeigt wird, wie der JSON-Code in Code geladen wird.

## <a name="loading-the-json-in-code"></a>Laden des JSON-Codes in Code

Nachdem der JSON-Code definiert wurde, müssen wir ihn in Mt laden. D mit der `JsonElement` -Klasse. Angenommen, eine Datei mit dem oben erstellten JSON-Code wurde dem Projekt mit dem Namen Sample. JSON hinzugefügt, und `JsonElement` es wurde eine Buildaktion von Content angegeben. das Laden von ist so einfach wie das Aufrufen der folgenden Codezeile:

```csharp
var taskElement = JsonElement.FromFile ("task.json");
```

Da wir diese on-Demand-Anforderung jedes Mal hinzufügen, wenn eine Aufgabe erstellt wird, können wir wie folgt die Schaltfläche ändern, auf die Sie aus dem Beispiel der früheren Elemente-API

```csharp
_addButton.Clicked += (sender, e) => {
    ++n;

    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};

    var taskElement = JsonElement.FromFile ("task.json");

    _rootElement [0].Add (taskElement);
};
```

## <a name="accessing-elements-at-runtime"></a>Zugreifen auf Elemente zur Laufzeit

Rückruf wir haben eine ID zu beiden Elementen hinzugefügt, als wir Sie in der JSON-Datei deklariert haben. Wir können die ID-Eigenschaft für den Zugriff auf jedes Element zur Laufzeit verwenden, um die Eigenschaften im Code zu ändern. Der folgende Code verweist z. b. auf das Entry-Element und das Date-Element, um die Werte des Task-Objekts festzulegen:

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

## <a name="loading-json-from-a-url"></a>Laden von JSON aus einer URL

UV. D unterstützt auch das dynamische Laden von JSON aus einer externen URL, indem die URL einfach an den Konstruktor von `JsonElement`übergeben wird. UV. D erweitert bei Bedarf die Hierarchie, die in der JSON-Datei deklariert wird, während Sie zwischen Bildschirmen navigieren. Betrachten Sie z. b. eine JSON-Datei, die sich im Stammverzeichnis des lokalen Webservers befindet:

```json
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

Wir können dies mit dem `JsonElement` wie im folgenden Code laden:

```csharp
_rootElement = new RootElement ("Json Example") {
    new Section ("") {
        new JsonElement ("Load from url", "http://localhost/sample.json")
    }
};
```

Zur Laufzeit wird die Datei von MT abgerufen und analysiert. D, wenn der Benutzer zur zweiten Ansicht navigiert, wie im folgenden Screenshot zu sehen:

 [![](json-element-walkthrough-images/04-json-web-example.png "Die Datei wird von MT abgerufen und analysiert. D, wenn der Benutzer zur zweiten Ansicht navigiert")](json-element-walkthrough-images/04-json-web-example.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde gezeigt, wie eine using-Schnittstelle mit MT erstellt wird. D aus JSON. Es wurde gezeigt, wie JSON, das in einer Datei enthalten ist, mit der Anwendung und einer Remote-URL geladen wird. Außerdem wurde gezeigt, wie auf Elemente zugegriffen wird, die zur Laufzeit in JSON beschrieben werden.

## <a name="related-links"></a>Verwandte Links

- [Mtdjsondemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdjsondemo)
- [Einführung in MonoTouch. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise zu Elementen](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise zur Reflektion](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [MonoTouch-Dialog Feld auf GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Tweetstation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [Uitableviewcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassen Verweis](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
