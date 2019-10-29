---
title: Erstellen einer xamarin. IOS-Anwendung mit der Reflection-API
description: In diesem Dokument wird die Attribut basierte reflektionsapi MonoTouch. Dialog beschrieben, die eine Benutzeroberfläche basierend auf Klassen erstellt, die mit Attributen versehen sind.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: 323b92190dc3ea18bc78871f5c19e51d0a6ea94e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73002214"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Erstellen einer xamarin. IOS-Anwendung mit der Reflection-API

Der Mt. Die D-reflektionsapi ermöglicht es, Klassen mit Attributen zu versehen, die Mt. D verwendet zum automatischen Erstellen von Bildschirmen. Die reflektionsapi bietet eine Bindung zwischen diesen Klassen und den auf dem Bildschirm angezeigten. Obwohl diese API nicht die differenzierte Steuerung bereitstellt, die die Elements-API durchführt, reduziert Sie die Komplexität, indem die Element Hierarchie basierend auf der Klassen Dekoration automatisch aufgebaut wird.

## <a name="setting-up-mtd"></a>Einrichten von Mt. D

UV. D wird mit xamarin. IOS verteilt. Um es zu verwenden, klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** eines xamarin. IOS-Projekts in Visual Studio 2017 oder Visual Studio für Mac, und fügen Sie einen Verweis auf die Assembly **MonoTouch. Dialog-1** hinzu. Fügen Sie dann `using MonoTouch.Dialog`-Anweisungen in Ihrem Quellcode nach Bedarf hinzu.

## <a name="getting-started-with-the-reflection-api"></a>Ersten Einstieg in die reflektionsinapi

Die Verwendung der reflektionserapi ist so einfach wie die folgenden:

1. Erstellen einer mit MT ergänzten Klasse D-Attribute.
1. Erstellen einer `BindingContext`-Instanz, indem Sie eine Instanz der obigen Klasse übergibt. 
1. Erstellen einer `DialogViewController` und übergeben der `BindingContext’s` `RootElement`. 

Sehen wir uns ein Beispiel an, um die Verwendung der reflektionsszenarios zu veranschaulichen. In diesem Beispiel erstellen wir einen einfachen Dateneingabe-Bildschirm, wie unten dargestellt:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "In this example, we'll build a simple data entry screen as shown here")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>Erstellen einer Klasse mit Mt. D-Attribute

Als erstes müssen wir die reflektionsapi verwenden, eine mit Attributen ergänzte Klasse. Diese Attribute werden von MT verwendet. D: intern zum Erstellen von Objekten aus der Elements-API. Beachten Sie beispielsweise die folgende Klassendefinition:

```csharp
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
        
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
}
```

Die `SectionAttribute` führt dazu, dass Abschnitte der `UITableView` erstellt werden, wobei das Zeichen folgen Argument zum Auffüllen des Abschnitts Headers verwendet wird. Nachdem ein Abschnitt deklariert wurde, wird jedes nachfolgende Feld in diesen Abschnitt eingeschlossen, bis ein anderer Abschnitt deklariert wird.
Der Typ des Benutzeroberflächen Elements, das für das Feld erstellt wird, hängt vom Typ des Felds und dem MT ab. D-Attribut, das es schmückt.

Beispielsweise handelt es sich bei dem `Name`-Feld um eine `string`, die mit einem `EntryAttribute`versehen ist. Dies führt dazu, dass der Tabelle eine Zeile mit einem Texteingabefeld und der angegebenen Beschriftung hinzugefügt wird. Entsprechend ist das `IsApproved` Feld ein `bool` mit einem `CheckboxAttribute`, wodurch eine Tabellenzeile mit einem Kontrollkästchen auf der rechten Seite der Tabellenzelle entsteht. UV. D verwendet den Feldnamen und fügt automatisch ein Leerzeichen hinzu, um die Beschriftung in diesem Fall zu erstellen, da Sie nicht in einem Attribut angegeben ist.

## <a name="adding-the-bindingcontext"></a>Hinzufügen von BindingContext

Um die `Expense`-Klasse zu verwenden, müssen wir eine `BindingContext`erstellen. Eine `BindingContext` ist eine Klasse, die die Daten von der attributierten Klasse bindet, um die Hierarchie der Elemente zu erstellen. Um eines zu erstellen, wird es einfach instanziiert, und eine Instanz der attributierten Klasse wird an den Konstruktor übergeben.

Fügen Sie z. b. den folgenden Code in die `FinishedLaunching`-Methode der `AppDelegate`ein, um die Benutzeroberfläche hinzuzufügen, die wir mithilfe des Attributs in der `Expense` Klasse deklariert haben:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Zum Erstellen der Benutzeroberfläche müssen Sie lediglich die `BindingContext` der `DialogViewController` hinzufügen und als `RootViewController` des Fensters festlegen, wie unten dargestellt:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{   
    window = new UIWindow (UIScreen.MainScreen.Bounds);
            
    var expense = new Expense ();
    var bctx = new BindingContext (null, expense, "Create a task");
    var dvc = new DialogViewController (bctx.Root);
            
    window.RootViewController = dvc;
    window.MakeKeyAndVisible ();
            
    return true;
}
```

Das Ausführen der Anwendung führt jetzt dazu, dass der oben gezeigte Bildschirm angezeigt wird.

### <a name="adding-a-uinavigationcontroller"></a>Hinzufügen eines UINavigationController

Beachten Sie jedoch, dass der Titel "Create a Task", den wir an den `BindingContext` übermittelt haben, nicht angezeigt wird. Dies liegt daran, dass der `DialogViewController` nicht Teil eines `UINavigatonController`ist. Ändern Sie den Code, um ein `UINavigationController` als `RootViewController,` des Fensters hinzuzufügen, und fügen Sie die `DialogViewController` als Stamm der `UINavigationController` hinzu, wie unten dargestellt:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Wenn Sie nun die Anwendung ausführen, wird der Titel in der Navigationsleiste `UINavigationController’s` angezeigt, wie im folgenden Screenshot gezeigt:

 [![](reflection-api-walkthrough-images/02-create-task.png "Now when we run the application, the title appears in the UINavigationControllers navigation bar")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Wenn Sie einen `UINavigationController`einschließen, können wir nun andere Features von MT nutzen. D, für die eine Navigation erforderlich ist. Beispielsweise können Sie der `Expense`-Klasse eine Enumeration hinzufügen, um die Kategorie für die Ausgaben und MT zu definieren. D erstellt automatisch einen Auswahlbildschirm. Um dies zu veranschaulichen, ändern Sie die `Expense` Klasse so, dass Sie ein `ExpenseCategory` Feld wie folgt enthält:

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    …

    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Das Ausführen der Anwendung führt jetzt zu einer neuen Zeile in der Tabelle für die Kategorie, wie in der folgenden Abbildung dargestellt:

 [![](reflection-api-walkthrough-images/03-set-details.png "Running the application now results in a new row in the table for the category as shown")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Wenn die Zeile ausgewählt wird, navigiert die Anwendung zu einem neuen Bildschirm mit Zeilen, die der-Enumeration entsprechen, wie unten dargestellt:

 [![](reflection-api-walkthrough-images/04-set-category.png "Selecting the row results in the application navigating to a new screen with rows corresponding to the enumeration")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine exemplarische Vorgehensweise zur reflektionsinapi vorgestellt. Wir haben gezeigt, wie Sie einer Klasse Attribute hinzufügen, um zu steuern, was angezeigt wird. Außerdem wurde erläutert, wie Sie eine `BindingContext` verwenden, um Daten aus einer Klasse an die erstellte Element Hierarchie zu binden, und wie Sie MT verwenden. D mit einem `UINavigationController`.

## <a name="related-links"></a>Verwandte Links

- [Mtdreflectionwalkthrough (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Dialog Feld "Einführung in MonoTouch"](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise zu Elementen](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise für JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [MonoTouch-Dialog Feld auf GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Tweetstation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [Uitableviewcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassen Verweis](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
