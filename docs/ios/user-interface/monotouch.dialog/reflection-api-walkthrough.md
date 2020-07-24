---
title: Erstellen einer xamarin. IOS-Anwendung mit der Reflection-API
description: In diesem Dokument wird die Attribut basierte reflektionsapi MonoTouch. Dialog beschrieben, die eine Benutzeroberfläche basierend auf Klassen erstellt, die mit Attributen versehen sind.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: bdbff7760e7680173c57e5fc83cecb80967c0a51
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996096"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Erstellen einer xamarin. IOS-Anwendung mit der Reflection-API

Der Mt. Die D-reflektionsapi ermöglicht es, Klassen mit Attributen zu versehen, die Mt. D verwendet zum automatischen Erstellen von Bildschirmen. Die reflektionsapi bietet eine Bindung zwischen diesen Klassen und den auf dem Bildschirm angezeigten. Obwohl diese API nicht die differenzierte Steuerung bereitstellt, die die Elements-API durchführt, reduziert Sie die Komplexität, indem die Element Hierarchie basierend auf der Klassen Dekoration automatisch aufgebaut wird.

## <a name="setting-up-mtd"></a>Einrichten von Mt. D

UV. D wird mit xamarin. IOS verteilt. Um es zu verwenden, klicken Sie mit der rechten Maustaste auf den Knoten **Verweise** eines xamarin. IOS-Projekts in Visual Studio 2017 oder Visual Studio für Mac, und fügen Sie einen Verweis auf die Assembly **MonoTouch. Dialog-1** hinzu. Fügen Sie dann `using MonoTouch.Dialog` ggf. Anweisungen im Quellcode hinzu.

## <a name="getting-started-with-the-reflection-api"></a>Ersten Einstieg in die reflektionsinapi

Die Verwendung der reflektionserapi ist so einfach wie die folgenden:

1. Erstellen einer mit MT ergänzten Klasse D-Attribute.
1. Erstellen einer- `BindingContext` Instanz, indem Sie eine Instanz der obigen Klasse übergibt.
1. Erstellen eines- `DialogViewController` und übergeben des `BindingContext’s` `RootElement` .

Sehen wir uns ein Beispiel an, um die Verwendung der reflektionsszenarios zu veranschaulichen. In diesem Beispiel erstellen wir einen einfachen Dateneingabe-Bildschirm, wie unten dargestellt:

 [![In diesem Beispiel erstellen wir einen einfachen Dateneingabe Bildschirm, wie hier gezeigt.](reflection-api-walkthrough-images/01-expense-entry.png)](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

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

Das `SectionAttribute` führt dazu, dass Abschnitte des `UITableView` erstellt werden, wobei das Zeichen folgen Argument verwendet wird, um den Header des Abschnitts aufzufüllen. Nachdem ein Abschnitt deklariert wurde, wird jedes nachfolgende Feld in diesen Abschnitt eingeschlossen, bis ein anderer Abschnitt deklariert wird.
Der Typ des Benutzeroberflächen Elements, das für das Feld erstellt wird, hängt vom Typ des Felds und dem MT ab. D-Attribut, das es schmückt.

Beispielsweise ist das `Name` -Feld ein `string` und wird mit einem ergänzt `EntryAttribute` . Dies führt dazu, dass der Tabelle eine Zeile mit einem Texteingabefeld und der angegebenen Beschriftung hinzugefügt wird. `IsApproved`Entsprechend ist das-Feld ein `bool` mit einem `CheckboxAttribute` , das eine Tabellenzeile mit einem Kontrollkästchen rechts neben der Tabellenzelle ergibt. UV. D verwendet den Feldnamen und fügt automatisch ein Leerzeichen hinzu, um die Beschriftung in diesem Fall zu erstellen, da Sie nicht in einem Attribut angegeben ist.

## <a name="adding-the-bindingcontext"></a>Hinzufügen von BindingContext

Um die- `Expense` Klasse zu verwenden, müssen wir ein erstellen `BindingContext` . Eine `BindingContext` ist eine Klasse, die die Daten von der attributierten Klasse bindet, um die Hierarchie der Elemente zu erstellen. Um eines zu erstellen, wird es einfach instanziiert, und eine Instanz der attributierten Klasse wird an den Konstruktor übergeben.

Fügen Sie z. b. `Expense` den folgenden Code in die-Methode von ein, um `FinishedLaunching` `AppDelegate` die Benutzeroberfläche hinzuzufügen, die wir mithilfe des Attributs in der Klasse deklariert haben

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Nun müssen Sie nur noch die Benutzeroberfläche erstellen, indem Sie den hinzufügen `BindingContext` `DialogViewController` und ihn als `RootViewController` des Fensters festlegen, wie unten dargestellt:

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

Beachten Sie jedoch, dass der Titel "Create a Task", den wir an den übermittelt haben, `BindingContext` nicht angezeigt wird. Dies liegt daran, dass der `DialogViewController` nicht Teil eines ist `UINavigatonController` . Ändern Sie den Code so, dass ein `UINavigationController` als das Fenster hinzugefügt wird, `RootViewController,` und fügen Sie den als Stamm der hinzu, `DialogViewController` `UINavigationController` wie unten dargestellt:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Wenn Sie nun die Anwendung ausführen, wird der Titel in der `UINavigationController’s` Navigationsleiste angezeigt, wie im folgenden Screenshot gezeigt:

 [![Wenn Sie nun die Anwendung ausführen, wird der Titel in der UINavigationControllers-Navigationsleiste angezeigt.](reflection-api-walkthrough-images/02-create-task.png)](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Wenn Sie einen einschließen `UINavigationController` , können wir nun andere Features von MT nutzen. D, für die eine Navigation erforderlich ist. Beispielsweise können wir der-Klasse eine Enumeration hinzufügen, `Expense` um die Kategorie für die Ausgaben und MT zu definieren. D erstellt automatisch einen Auswahlbildschirm. Um dies zu veranschaulichen, ändern Sie die `Expense` Klasse so, dass Sie ein `ExpenseCategory` Feld wie folgt enthält:

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

 [![Wenn Sie die Anwendung ausführen, wird nun eine neue Zeile in der Tabelle für die Kategorie angezeigt.](reflection-api-walkthrough-images/03-set-details.png)](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Wenn die Zeile ausgewählt wird, navigiert die Anwendung zu einem neuen Bildschirm mit Zeilen, die der-Enumeration entsprechen, wie unten dargestellt:

 [![Wenn die Zeile ausgewählt wird, navigiert die Anwendung zu einem neuen Bildschirm mit Zeilen, die der-Enumeration entsprechen.](reflection-api-walkthrough-images/04-set-category.png)](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde eine exemplarische Vorgehensweise zur reflektionsinapi vorgestellt. Wir haben gezeigt, wie Sie einer Klasse Attribute hinzufügen, um zu steuern, was angezeigt wird. Wir haben auch erläutert, wie Sie eine verwenden, `BindingContext` um Daten aus einer Klasse an die erstellte Element Hierarchie zu binden, und wie Sie MT verwenden. D mit einem `UINavigationController` .

## <a name="related-links"></a>Verwandte Links

- [Mtdreflectionwalkthrough (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Dialog Feld "Einführung in MonoTouch"](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise zu Elementen](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise für JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [MonoTouch-Dialog Feld auf GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Tweetstation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [Uitableviewcontroller-Klassenreferenz](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassen Verweis](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
