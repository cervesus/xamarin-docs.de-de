---
title: 'Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Reflektions-API'
description: Zusätzlich zu den Elementen-API, MonoTouch.Dialog (MT. D) von der ereignissteuerung eine Attribut-basierter Reflektions-API. Der Reflektions-API macht Erstellen von Bildschirmen mit MT. D so einfach wie ergänzen von Klassen mit Attributen. Dieser Artikel bietet erörtert erfahren, wie eine Anwendung mit der Reflektions-API zu erstellen.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: e56eaeccb2e09d9f1ad84245bf41e2a4bf1b56f1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-creating-an-application-using-the-reflection-api"></a>Exemplarische Vorgehensweise: Erstellen einer Anwendung mithilfe der Reflektions-API

_Zusätzlich zu den Elementen-API, MonoTouch.Dialog (MT. D) von der ereignissteuerung eine Attribut-basierter Reflektions-API. Der Reflektions-API macht Erstellen von Bildschirmen mit MT. D so einfach wie ergänzen von Klassen mit Attributen. Dieser Artikel bietet erörtert erfahren, wie eine Anwendung mit der Reflektions-API zu erstellen._


Die MT. D-Reflektions-API können Klassen werden mit Attributen ergänzt wurden, MT. D zum automatischen Erstellen von Bildschirmen verwendet. Die Reflektions-API bietet eine Bindung zwischen diesen Klassen und was auf dem Bildschirm angezeigt wird. Obwohl diese API die Differenzierte Steuerung, die die API-Elemente verfügt bietet, wird von automatisch auf die Hierarchie der Elemente basierend auf der Klasse Decoration Ausbau Komplexität reduziert.

 <a name="Getting_Started_with_the_Reflection_API" />


## <a name="getting-started-with-the-reflection-api"></a>Erste Schritte mit der Reflektions-API

Die Verwendung der Reflektions-API ist so einfach wie das:

1.  Erstellen einer Klasse ergänzt mit MT. D-Attribute.
1.  Erstellen einer `BindingContext` Instanz, und übergeben sie eine Instanz der oben genannten-Klasse. 
1.  Erstellen einer `DialogViewController` , und übergeben sie die `BindingContext’s` `RootElement` . 


Sehen wir uns ein Beispiel zum Veranschaulichen der Reflektions-API verwenden. In diesem Beispiel werden wir eine einfache dateneingabebildschirm wie folgt erstellen:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "In diesem Beispiel fügen wir eine einfache dateneingabebildschirm erstellen, wie hier gezeigt")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

 <a name="Creating_a_Class_with_MT.D_Attributes" />


## <a name="creating-a-class-with-mtd-attributes"></a>Erstellen eine Klasse mit MT. D-Attribute

Zunächst müssen wir die Reflektions-API zu verwenden ist eine Klasse, die mit Attributen versehen. Diese Attribute werden durch MT. verwendet werden D intern zum Erstellen von der API-Elemente-Objekten. Betrachten Sie beispielsweise die folgenden Klassendefinition aus:

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

Die `SectionAttribute` führt in den Abschnitten der `UITableView` erstellt wird, mit dem Zeichenfolgenargument verwendet, um den Bereich "Benutzerportal"-Header einzutragen. Sobald ein Abschnitt deklariert wird, wird jedes Feld, das es folgt in diesem Abschnitt enthalten werden, bis einem anderen Bereich deklariert wird.
Der Typ des Benutzeroberflächen-Elements für das Feld erstellt hängen der Typ des Felds und den MT. D-Attribut, indem diese.

Z. B. die `Name` Feld ist eine `string` und ergänzt wird mit einer `EntryAttribute`. Dies führt zu einer Zeile der Tabelle mit der angegebenen Beschriftung und ein Texteingabefeld hinzugefügt wird. Auf ähnliche Weise die `IsApproved` Feld ist eine `bool` mit einem `CheckboxAttribute`, wodurch die Zeile einer Tabelle mit einem Kontrollkästchen auf der rechten Seite der Zelle. MT. D verwendet den Feldnamen ein Leerzeichen automatisch hinzugefügt, die Beschriftung in diesem Fall erstellt werden, da sie nicht in einem Attribut angegeben ist.

 <a name="Adding_the_BindingContext" />


## <a name="adding-the-bindingcontext"></a>Hinzufügen von BindingContext

Verwenden der `Expense` -Klasse, müssen wir erstellen eine `BindingContext`. Ein `BindingContext` ist eine Klasse, die die Daten aus die attributierte Klasse zum Erstellen der Hierarchie von Elementen gebunden wird. Zum Erstellen eines wir einfach instanziiert und in einer Instanz von die attributierte Klasse an den Konstruktor übergeben.

Beispielsweise, um die Benutzeroberfläche hinzufügen, die wir mit deklarierten Attribut in der `Expense` -Klasse, schließen Sie den folgenden Code in die `FinishedLaunching` Methode der `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Müssen wir tun, um die Benutzeroberfläche zu erstellen ist hinzufügen, dann die `BindingContext` auf die `DialogViewController` und legen Sie ihn als das `RootViewController` des Fensters, wie unten dargestellt:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, 
        NSDictionary options)
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

Die Anwendung jetzt ausführen, führt zu dem Bildschirm oben angezeigt wird.

 <a name="Adding_a_UINavigationController" />


### <a name="adding-a-uinavigationcontroller"></a>Hinzufügen einer UINavigationController

Beachten Sie jedoch, dass der Titel "eines Tasks erstellen", die wir übergeben, mit der `BindingContext` wird nicht angezeigt. Grund hierfür ist die `DialogViewController` ist nicht Teil einer `UINavigatonController`. Ändern wir den Code zum Hinzufügen einer `UINavigationController` wie des Fensters `RootViewController,` und Hinzufügen der `DialogViewController` als Stammverzeichnis für die `UINavigationController` wie unten dargestellt:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Jetzt bei Ausführung die Anwendung, in der Titel wird das `UINavigationController’s` Navigationsleiste als Screenshot unten gezeigt:

 [![](reflection-api-walkthrough-images/02-create-task.png "Nun, wenn wir die Anwendung ausführen, wird der Titel in der Navigationsleiste UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Durch Einschließen einer `UINavigationController`, wir können jetzt nutzen von anderen Funktionen von MT. D, die für die Navigation erforderlich ist. Beispielsweise können wir eine Enumeration zum Hinzufügen der `Expense` Klasse, um die Kategorie für die Ausgaben und MT. definieren D wird eine Auswahlbildschirm automatisch erstellt. Um zu demonstrieren, Ändern der `Expense` Klasse einbeziehen, ein `ExpenseCategory` Feld wie folgt:

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

Die Anwendung jetzt ausführen Führt eine neue Zeile in der Tabelle für die Kategorie wie gezeigt:

 [![](reflection-api-walkthrough-images/03-set-details.png "Wie gezeigt führt in eine neue Zeile in der Tabelle für die Kategorie die Anwendung jetzt ausführen")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Die Datenzeile ausgewählt wird, erhalten Sie in der Anwendung navigieren zu einem neuen Bildschirm mit Zeilen, die die Enumeration entsprechen wie folgt:

 [![](reflection-api-walkthrough-images/04-set-category.png "In der Anwendung navigieren zu einem neuen Bildschirm mit Zeilen, die die Enumeration entsprechen führt Sie die Zeile auswählen")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Zusammenfassung

In diesem Artikel dargestellt eine exemplarische Vorgehensweise der Reflektions-API. Es wurde gezeigt, wie Hinzufügen von Attributen zu einer Klasse zu steuern, was angezeigt wird. Wir außerdem erläutert, wie eine `BindingContext` Daten aus einer Klasse in der Elementhierarchie zu binden, der erstellt wird, sowie zur Verwendung MT. D mit einem `UINavigationController`.


## <a name="related-links"></a>Verwandte Links

- [MTDReflectionWalkthrough (sample)](https://developer.xamarin.com/samples/MTDReflectionWalkthrough/)
- [Screencast - Miguel de Icaza erstellt ein iOS-Anmeldebildschirm mit MonoTouch.Dialog](http://youtu.be/3butqB1EG0c)
- [Screencast - problemlos iOS Benutzeroberflächen mit MonoTouch.Dialog erstellen](http://youtu.be/j7OC5r8ZkYg)
- [Einführung in die MonoTouch-Dialogfeld](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise Elemente-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise JSON-Element](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController-Klassenreferenz](http://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
