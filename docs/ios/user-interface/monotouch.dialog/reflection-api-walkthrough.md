---
title: Erstellen einer Xamarin.iOS-Anwendung mithilfe der Reflektions-API
description: Dieses Dokument beschreibt die MonoTouch.Dialog attributbasierte Reflection-API-Benutzeroberfläche auf Basis von Klassen, die mit Attributen versehen wird erstellt.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 2ac828c8efca3a1a5c54d4dcdaf175ebfc63fdc2
ms.sourcegitcommit: 2eb8961dd7e2a3e06183923adab6e73ecb38a17f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/11/2019
ms.locfileid: "66827305"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Erstellen einer Xamarin.iOS-Anwendung mithilfe der Reflektions-API

Das masterziel D-Reflektions-API können Sie Klassen werden mit Attributen versehen, masterziel D zum automatischen Erstellen von Bildschirmen verwendet. Die Reflektions-API stellt eine Bindung zwischen diesen Klassen und was auf dem Bildschirm angezeigt wird. Obwohl diese API nicht die eine präzisere Kontrolle, die die API-Elemente bereitstellen, reduziert Komplexität, von der Hierarchie des Elements basierend auf der Klasse Decoration automatisch erstellen.

## <a name="setting-up-mtd"></a>Masterziel einrichten D

MT. D ist mit Xamarin.iOS verteilt. Um es zu verwenden, mit der Maustaste auf die **Verweise** Knoten eine Xamarin.iOS-Projekt in Visual Studio 2017 oder Visual Studio für Mac und Hinzufügen eines Verweises auf die **MonoTouch.Dialog-1-** Assembly. Fügen Sie dann `using MonoTouch.Dialog` Anweisungen in Ihrer Quelle code nach Bedarf.

## <a name="getting-started-with-the-reflection-api"></a>Erste Schritte mit der Reflektions-API

Mithilfe der Reflektions-API ist ganz einfach:

1.  Erstellen einer Klasse ergänzt MT. D-Attribute.
1.  Erstellen einer `BindingContext` Instanz, und übergeben sie eine Instanz der oben genannten-Klasse. 
1.  Erstellen einer `DialogViewController` , und übergeben sie die `BindingContext’s` `RootElement` . 


Sehen wir uns ein Beispiel zum Veranschaulichen der Reflektions-API verwenden. In diesem Beispiel erstellen wir einen einfachen dateneingabebildschirm wie unten dargestellt:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "In diesem Beispiel wollen wir einen einfachen dateneingabebildschirm entwickeln, wie hier gezeigt.")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>Erstellen eine Klasse mit MT. D-Attribute

Zunächst müssen wir die Reflektions-API zu verwenden ist, eine Klasse, die mit Attributen versehen wird. Diese Attribute werden von MT verwendet D, intern, um den Computer neu zu erstellen, Objekte aus der Elemente-API. Betrachten Sie beispielsweise die folgende Klassendefinition:

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

Die `SectionAttribute` führt in den Abschnitten der `UITableView` erstellt wird, mit dem String-Argument, das zum Auffüllen des Abschnitts Header verwendet. Nach ein Abschnitt deklariert wird, wird jedes Feld, die darauf folgt in diesem Abschnitt enthalten werden bis auf einen anderen Abschnitt deklariert ist.
Der Typ des Benutzeroberflächenelements erstellt, die für das Feld wird von der Typ des Felds und das masterziel abhängen. Indem diese D-Attribut.

Z. B. die `Name` Feld ist ein `string` und ihn durch einen `EntryAttribute`. Dies führt zu einer Zeile der Tabelle mit der angegebenen Beschriftung und ein Texteingabefeld hinzugefügt wird. Auf ähnliche Weise die `IsApproved` Feld ist ein `bool` mit einem `CheckboxAttribute`, sodass die Zeile einer Tabelle mit einem Kontrollkästchen auf der rechten Seite der Zelle. MT. D verwendet, dem Namen des Felds Automatisches Hinzufügen von einem Leerzeichen, um die Beschriftung in diesem Fall erstellt werden, da sie nicht in ein Attribut angegeben ist.

## <a name="adding-the-bindingcontext"></a>Hinzufügen von BindingContext

Verwenden der `Expense` -Klasse müssen wir erstellen eine `BindingContext`. Ein `BindingContext` ist eine Klasse, die die Daten aus die attributierte Klasse zum Erstellen der Hierarchie von Elementen gebunden wird. Zum Erstellen wir einfach instanziieren und in einer Instanz von die attributierte Klasse an den Konstruktor übergeben.

Beispielsweise, um die Benutzeroberfläche hinzufügen, die wir mit deklariert-Attribut in der `Expense` Klasse, enthalten den folgenden Code in die `FinishedLaunching` -Methode der der `AppDelegate`:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Dann ist alles, was wir tun, um das Erstellen der Benutzeroberfläche hinzufügen der `BindingContext` auf die `DialogViewController` und legen Sie es als die `RootViewController` des Fensters, wie unten dargestellt:

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

Die Anwendung jetzt ausführen, führt in der Abbildung oben angezeigt wird.

### <a name="adding-a-uinavigationcontroller"></a>Hinzufügen einer UINavigationController

Beachten Sie, dass der Titel "eines Tasks erstellen", die wir zum Übergeben der `BindingContext` wird nicht angezeigt. Grund hierfür ist die `DialogViewController` ist nicht Teil einer `UINavigatonController`. Ändern wir den Code zum Hinzufügen einer `UINavigationController` als des Fensters `RootViewController,` und Hinzufügen der `DialogViewController` als Stamm der `UINavigationController` wie unten dargestellt:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Jetzt Wenn die Anwendung ausgeführt wird, in der Titel wird das `UINavigationController’s` Navigationsleiste wie der folgende Screenshot zeigt:

 [![](reflection-api-walkthrough-images/02-create-task.png "Jetzt Wenn die Anwendung ausgeführt wird, wird der Titel in der Navigationsleiste UINavigationControllers")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Durch Einschließen einer `UINavigationController`, wir können jetzt nutzen, andere Features des masterzielservers D, die für die Navigation erforderlich ist. Wir können beispielsweise eine Enumeration zum Hinzufügen der `Expense` Klasse, um die Kategorie für die Kosten und das masterziel definieren D erstellt automatisch einen Auswahlbildschirm. Um zu veranschaulichen, ändern Sie die `Expense` Klasse einbeziehen, ein `ExpenseCategory` Feld wie folgt:

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

Auswählen der zeilenupdates führt die Anwendung Navigation zu einem neuen Bildschirm mit Zeilen, die für die Enumeration, wie unten dargestellt:

 [![](reflection-api-walkthrough-images/04-set-category.png "Die Anwendung Navigation zu einem neuen Bildschirm mit Zeilen, die für die Enumeration führt Sie die Zeile auswählen")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Zusammenfassung

Dieser Artikel enthält eine exemplarische Vorgehensweise für die Reflektions-API. Es wurde gezeigt, wie Hinzufügen von Attributen zu einer Klasse zu steuern, was angezeigt wird. Wir auch erläutert, wie mit einem `BindingContext` Daten aus einer Klasse in der Elementhierarchie gebunden, die erstellt wird, sowie zur Verwendung MT. D mit einem `UINavigationController`.


## <a name="related-links"></a>Verwandte Links

- [MTDReflectionWalkthrough (sample)](https://developer.xamarin.com/samples/monotouch/MTDReflectionWalkthrough/)
- [Einführung in MonoTouch-Dialogfeld](~/ios/user-interface/monotouch.dialog/index.md)
- [Exemplarische Vorgehensweise Elemente-API](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Exemplarische Vorgehensweise JSON-Element](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [MonoTouch-Dialogfeld auf Github](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [TweetStation-Anwendung](https://github.com/migueldeicaza/TweetStation)
- [UITableViewController-Klassenreferenz](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [UINavigationController-Klassenreferenz](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
