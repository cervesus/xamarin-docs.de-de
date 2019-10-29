---
title: Arbeiten mit Tabellen im iOS-Designer
description: In den vorherigen Abschnitten wurde das entwickeln mithilfe von Tabellen untersucht. Im fünften und letzten Abschnitt aggregieren wir das, was wir bisher gelernt haben, und erstellen eine einfache Chore List-Anwendung mithilfe eines Storyboards.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 7eff434c21b5e2330d320f2eb85174dc6fe65b34
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021938"
---
# <a name="working-with-tables-in-the-ios-designer"></a>Arbeiten mit Tabellen im iOS-Designer

Storyboards sind eine WYSIWYG-Methode zum Erstellen von IOS-Anwendungen, die in Visual Studio unter Mac und Windows unterstützt werden. Weitere Informationen zu Storyboards finden Sie im Dokument [Introduction to Storyboards](~/ios/user-interface/storyboards/index.md) . Mithilfe von Storyboards können Sie auch die Zellen Layouts *in* der Tabelle bearbeiten, wodurch die Entwicklung mit Tabellen und Zellen vereinfacht wird.

Beim Konfigurieren der Eigenschaften einer Tabellenansicht im IOS-Designer stehen zwei Arten von Zell Inhalten zur Auswahl: **dynamischer** oder **statischer** prototypinhalt.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Inhalt des dynamischen Prototyps

Eine `UITableView` mit prototypinhalten soll normalerweise eine Liste von Daten anzeigen, bei denen die prototypenzelle (oder Zellen, wie Sie mehr als eins definieren können) für jedes Element in der Liste wieder verwendet wird. Die Zellen müssen nicht instanziiert werden, Sie werden in der `GetView`-Methode abgerufen, indem die `DequeueReusableCell`-Methode der `UITableViewSource`aufgerufen wird.

 <a name="Static_Content" />

## <a name="static-content"></a>Statischer Inhalt

durch `UITableView`s mit statischem Inhalt können Tabellen direkt auf der Entwurfs Oberfläche entworfen werden. Zellen können in die Tabelle gezogen und angepasst werden, indem Eigenschaften geändert und Steuerelemente hinzugefügt werden.

 <a name="Creating_a_Storyboard-driven_app" />

## <a name="creating-a-storyboard-driven-app"></a>Erstellen einer Storyboard-gesteuerten App

Das storyboardtable-Beispiel enthält eine einfache Master/Detail-APP, die beide Typen von "uitableview" in einem Storyboard verwendet. Im restlichen Teil dieses Abschnitts wird beschrieben, wie ein kleines to-do-Listen Beispiel erstellt wird, das wie folgt aussieht:

 [![Beispiel Bildschirme](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

Die Benutzeroberfläche wird mit einem Storyboard erstellt, und für beide Bildschirme wird eine "uitableview" verwendet. Der Hauptbildschirm verwendet *prototypinhalte* zum LayoutLayout der Zeile, und der Detailbildschirm verwendet *statische Inhalte* , um mithilfe von benutzerdefinierten Zell Layouts ein Dateneingabe Formular zu erstellen.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Erstellen Sie eine neue Projekt Mappe in Visual Studio mithilfe von **(erstellen) neues Projekt... > Einzelansicht-APPC#()** , und nennen Sie Sie " _storyboardtables_".

 [![Dialogfeld "Neues Projekt erstellen"](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

Die Lösung wird mit einigen C# Dateien geöffnet, und eine`Main.storyboard`Datei wird bereits erstellt. Doppelklicken Sie auf die Datei `Main.storyboard`, um Sie im IOS-Designer zu öffnen.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Ändern des Storyboards

Das Storyboard wird in drei Schritten bearbeitet:

- Richten Sie zuerst die erforderlichen Ansichts Controller ein, und legen Sie deren Eigenschaften fest.
- Erstellen Sie dann die Benutzeroberfläche, indem Sie Objekte per Drag & Drop auf Ihre Ansicht ziehen.
- Fügen Sie schließlich der Ansicht die erforderliche UIKit-Klasse hinzu, und geben Sie verschiedenen Steuerelementen einen Namen, damit auf Sie im Code verwiesen werden kann.

Nachdem das Storyboard vollständig ist, kann Code hinzugefügt werden, damit alles funktioniert.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Layout der Ansichts Controller

Die erste Änderung am Storyboard besteht darin, die vorhandene Detail Ansicht zu löschen und durch einen uitableviewcontroller zu ersetzen. Führen Sie folgende Schritte aus:

1. Wählen Sie die Leiste am unteren Rand des Ansichts Controllers aus, und löschen Sie Sie.
2. Ziehen Sie einen **Navigations Controller** und einen **Tabellen Ansichts Controller** aus der Toolbox auf das Storyboard. 
3. Erstellen Sie einen segue vom Root View Controller zum zweiten Tabellen Ansichts Controller, der soeben hinzugefügt wurde. Zum Erstellen des-Steuer Elements + Drag & amp; Drop *aus der Detail Zelle* auf den neu hinzugefügten uitableviewcontroller. Wählen Sie die Option unter **Auswahl** **anzeigen** aus. 
4. Wählen Sie den neuen segue aus, den Sie erstellt haben, und weisen Sie ihm einen Bezeichner zu Klicken Sie auf den-Typ, und geben Sie `TaskSegue` für den **Bezeichner** in der **Eigenschaftenpad**ein:    
  [Benennungs-![im Eigenschaften Panel](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. Konfigurieren Sie als nächstes die beiden Tabellen Sichten, indem Sie Sie auswählen und die Eigenschaftenpad verwenden. Stellen Sie sicher, dass Sie "View" und "Not View Controller" auswählen – Sie können die Dokument Gliederung zur Auswahl verwenden.

6. Ändern Sie den root View Controller in **Content: dynamische Prototypen** (die Ansicht auf der Designoberfläche wird als **prototypinhalt** gekennzeichnet):

    [![Festlegen der Content-Eigenschaft auf dynamische Prototypen](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7. Ändern Sie den neuen **uitableviewcontroller** in " **Content: static Cells**". 

8. Für den neuen uitableviewcontroller müssen der Klassenname und der Bezeichner festgelegt sein. Wählen Sie den Ansichts Controller aus, und geben Sie _taskdetailviewcontroller_ für die **Klasse** im **Eigenschaftenpad** ein – Hierdurch wird eine neue `TaskDetailViewController.cs` Datei in der Lösungspad erstellt. Geben Sie die **storyboardid** wie im folgenden Beispiel gezeigt als _Detail_ein. Diese wird später verwendet, um diese Ansicht in C# Code zu laden:  

    [![Festlegen der Storyboard-ID](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. Die Storyboard-Entwurfs Oberfläche sollte nun wie folgt aussehen (der Titel des Navigations Elements des root View Controller wurde in "Chore Board" geändert):

    [![Entwurfs Oberfläche](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  

<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Benutzeroberfläche erstellen

Nachdem die Ansichten und die-Elemente konfiguriert wurden, müssen die Benutzeroberflächen Elemente hinzugefügt werden.

#### <a name="root-view-controller"></a>Root View Controller

Wählen Sie zunächst die prototypenzelle im Master Ansichts Controller aus, und legen Sie den **Bezeichner** als _taskcell_fest, wie unten gezeigt. Diese wird später im Code zum Abrufen einer Instanz dieser uitableviewcell verwendet:

 [![Festlegen des Zellen Bezeichners](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

Als nächstes müssen Sie eine Schaltfläche erstellen, mit der neue Aufgaben hinzugefügt werden, wie unten dargestellt:

[Schaltflächen Element![Leiste in der Navigationsleiste](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

Führen Sie folgende Schritte aus: 

- Ziehen Sie ein **Balken-Schaltflächen Element** aus der Toolbox auf die Rechte Seite _der Navigationsleiste_.
- Wählen Sie in der **Eigenschaftenpad**unter leisten **Schaltfläche** die Option **Bezeichner: Hinzufügen** (um die Schaltfläche *+* Plus). 
- Geben Sie einen Namen ein, damit er zu einem späteren Zeitpunkt im Code identifiziert werden kann. Beachten Sie, dass Sie dem stammansichts Controller einen Klassennamen (z. b. **itemviewcontroller**) geben müssen, damit Sie den Namen des leisten-Schaltflächen Elements festlegen können.

#### <a name="taskdetail-view-controller"></a>Taskdetail-Ansichts Controller

Die Detail Ansicht erfordert viel mehr Arbeit. Tabellen Ansichts Zellen müssen auf die Ansicht gezogen und dann mit Bezeichnungen, Text Ansichten und Schaltflächen aufgefüllt werden. Der folgende Screenshot zeigt die fertige Benutzeroberfläche mit zwei Abschnitten. Ein Abschnitt enthält drei Zellen, drei Bezeichnungen, zwei Textfelder und einen Switch, während der zweite Abschnitt eine Zelle mit zwei Schaltflächen aufweist:

 [Layout der![Detailansicht](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

Führen Sie die folgenden Schritte aus, um das komplette Layout zu erstellen:

Wählen Sie die Tabellenansicht aus, und öffnen Sie das **Eigenschafts Pad**. Aktualisieren Sie die folgenden Eigenschaften:

- **Abschnitte**: _2_ 
- **Stil**: _gruppiert_
- **Trenn**Zeichen: _keine_
- **Auswahl**: _keine Auswahl_

Wählen Sie den oberen Abschnitt aus, und ändern Sie unter **Eigenschaften > Tabellen Ansichts Abschnitt** **Zeilen** ändern in _3_, wie unten dargestellt:

 [![Festlegen des obersten Abschnitts auf drei Zeilen](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

Öffnen Sie für jede Zelle die **Eigenschaftenpad** , und legen Sie Folgendes fest:

- **Stil**: _Benutzer_ definiert
- **Bezeichner**: Wählen Sie einen eindeutigen Bezeichner für jede Zelle aus (z. b. "_Title_", "_Notizen_", "_done_").
- Ziehen Sie die erforderlichen Steuerelemente, um das im Screenshot gezeigte Layout (" **UILabel**", " **UITextField** " und " **uiswitch** " in den richtigen Zellen zu platzieren, und legen Sie die Bezeichnungen entsprechend fest. Titel, Notizen und Done).

Legen Sie im zweiten Abschnitt **Zeilen** auf _1_ fest, und ziehen Sie den unteren Zieh Punkt der Zelle, um ihn zu vergrößern.

- **Legen Sie den Bezeichner**: auf einen eindeutigen Wert fest (z. b. "Speichern"). 
- **Legen Sie den Hintergrund fest**: _Farbe löschen_ .
- Ziehen Sie zwei Schaltflächen in die Zelle, und legen Sie Ihre Titel wie unten dargestellt entsprechend fest (z.b. _Speichern_ und _Löschen_):

   [![Festlegen von zwei Schaltflächen im unteren Abschnitt](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

An diesem Punkt können Sie auch Einschränkungen für Ihre Zellen und Steuerelemente festlegen, um ein adaptives Layout sicherzustellen.

### <a name="adding-uikit-class-and-naming-controls"></a>Hinzufügen von UIKit-und Benennungs Steuerelementen

Beim Erstellen des Storyboards sind einige abschließende Schritte erforderlich. Zuerst müssen wir den einzelnen Steuerelementen unter **Identity > Name** einen Namen geben, damit Sie später im Code verwendet werden können. Benennen Sie diese wie folgt:

- **Titel UITextField** : _TitleText_
- **Notizen UITextField** : _notestext_
- **Uiswitch** : _doneswitch_
- **UIButton löschen** : _DeleteButton_
- **UIButton speichern** : _SaveButton_

<a name="Adding_Code" />

## <a name="adding-code"></a>Hinzufügen von Code

Der Rest der Arbeit erfolgt in Visual Studio unter Mac oder Windows mit C#. Beachten Sie, dass die im Code verwendeten Eigenschaften Namen die in der obigen exemplarischen Vorgehensweise festgelegten Eigenschaften Namen widerspiegeln

Zuerst möchten wir eine `Chores` Klasse erstellen, die eine Möglichkeit bietet, den Wert von ID, Name, Notizen und den fertigen booleschen Wert zu erhalten und festzulegen, damit wir diese Werte in der gesamten Anwendung verwenden können.

Fügen Sie in der `Chores`-Klasse den folgenden Code hinzu:

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Erstellen Sie als nächstes eine `RootTableSource` Klasse, die von `UITableViewSource`erbt. 

Der Unterschied zwischen dieser und einer nicht-Storyboard-Tabellenansicht besteht darin, dass die `GetView`-Methode keine Zellen instanziieren muss – `theDequeueReusableCell` Methode immer eine Instanz der prototypzelle zurückgibt (mit übereinstimmenden Bezeichner).

Der folgende Code wird aus der `RootTableSource.cs` Datei abgeleitet:

```csharp
public class RootTableSource : UITableViewSource
{
// there is NO database or storage of Tasks in this example, just an in-memory List<>
Chores[] tableItems;
string cellIdentifier = "taskcell"; // set in the Storyboard

    public RootTableSource(Chores[] items)
    {
        tableItems = items;
    }

public override nint RowsInSection(UITableView tableview, nint section)
{
  return tableItems.Length;
}

public override UITableViewCell GetCell(UITableView tableView, NSIndexPath indexPath)
{
  // in a Storyboard, Dequeue will ALWAYS return a cell, 
  var cell = tableView.DequeueReusableCell(cellIdentifier);
  // now set the properties as normal
  cell.TextLabel.Text = tableItems[indexPath.Row].Name;
  if (tableItems[indexPath.Row].Done)
    cell.Accessory = UITableViewCellAccessory.Checkmark;
  else
    cell.Accessory = UITableViewCellAccessory.None;
  return cell;
}
public Chores GetItem(int id)
{
  return tableItems[id];
}
```

Um die `RootTableSource`-Klasse zu verwenden, erstellen Sie eine neue-Auflistung im Konstruktor der `ItemViewController`:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

In `ViewWillAppear` die Auflistung an die Quelle übergeben und der Tabellenansicht zugewiesen:

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Wenn Sie die APP jetzt ausführen, wird der Hauptbildschirm nun geladen, und es wird eine Liste mit zwei Tasks angezeigt. Wenn eine Aufgabe berührt wird, führt der durch das Storyboard definierte durch das Storyboard, dass der Detailbildschirm angezeigt wird. es werden jedoch momentan keine Daten angezeigt.

Um einen Parameter in einem segue zu senden, überschreiben Sie die `PrepareForSegue`-Methode, und legen Sie die Eigenschaften für die `DestinationViewController` (die `TaskDetailViewController` in diesem Beispiel) fest. Die Controller Klasse der Ziel Ansicht wurde instanziiert, aber für den Benutzer noch nicht angezeigt – dies bedeutet, dass Sie Eigenschaften für die Klasse festlegen, aber keine UI-Steuerelemente ändern können:

```csharp
public override void PrepareForSegue (UIStoryboardSegue segue, NSObject sender)
    {
      if (segue.Identifier == "TaskSegue") { // set in Storyboard
        var navctlr = segue.DestinationViewController as TaskDetailViewController;
        if (navctlr != null) {
          var source = TableView.Source as RootTableSource;
          var rowPath = TableView.IndexPathForSelectedRow;
          var item = source.GetItem(rowPath.Row);
          navctlr.SetTask (this, item); // to be defined on the TaskDetailViewController
        }
      }
    }
```

In `TaskDetailViewController` weist die `SetTask`-Methode ihren Parametern Eigenschaften zu, sodass in viewwillview auf Sie verwiesen werden kann. Die Steuerelement Eigenschaften können nicht in `SetTask` geändert werden, da möglicherweise nicht vorhanden ist, wenn `PrepareForSegue` aufgerufen wird:

```csharp
Chore currentTask {get;set;}
    public ItemViewController Delegate {get;set;} // will be used to Save, Delete later

public override void ViewWillAppear (bool animated)
    {
      base.ViewWillAppear (animated);
      TitleText.Text = currentTask.Name;
      NotesText.Text = currentTask.Notes;
      DoneSwitch.On = currentTask.Done;
    }

    // this will be called before the view is displayed
    public void SetTask (ItemViewController d, Chore task) {
      Delegate = d;
      currentTask = task;
    }
```

Der "*" öffnet nun den Detailbildschirm und zeigt die ausgewählten Aufgabeninformationen an. Leider gibt es keine Implementierung für die Schaltflächen " **Speichern** " und " **Löschen** ". Fügen Sie vor dem Implementieren der Schaltflächen **ItemViewController.cs** die folgenden Methoden hinzu, um die zugrunde liegenden Daten zu aktualisieren, und schließen Sie den Detailbildschirm:

```csharp
public void SaveTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
        NavigationController.PopViewController(true);
}

public void DeleteTask(Chores chore)
{
  var oldTask = chores.Find(t => t.Id == chore.Id);
  chores.Remove(oldTask);
        NavigationController.PopViewController(true);
}
```

Als nächstes müssen Sie den `TouchUpInside` Ereignishandler der Schaltfläche der `ViewDidLoad`-Methode von **TaskDetailViewController.cs**hinzufügen. Der `Delegate`-Eigenschafts Verweis auf die `ItemViewController` wurde speziell erstellt, sodass wir `SaveTask` und `DeleteTask`aufzurufen können, die diese Ansicht im Rahmen des Vorgangs schließen:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

Die letzte verbleibende Funktionalität, die erstellt werden soll, ist die Erstellung neuer Tasks. Fügen Sie in **ItemViewController.cs** eine Methode hinzu, die neue Aufgaben erstellt und die Detailansicht öffnet. Um eine Sicht aus einem Storyboard zu instanziieren, verwenden Sie die `InstantiateViewController`-Methode mit der `Identifier` für diese Ansicht. in diesem Beispiel wird "Detail" angezeigt:

```csharp
public void CreateTask () 
    {
      // first, add the task to the underlying data
      var newId = chores[chores.Count - 1].Id + 1;
      var newChore = new Chore{Id = newId};
      chores.Add (newChore);

      // then open the detail view to edit it
      var detail = Storyboard.InstantiateViewController("detail") as TaskDetailViewController;
      detail.SetTask (this, newChore);
      NavigationController.PushViewController (detail, true);
    }
```

Verknüpfen Sie schließlich die Schaltfläche in der Navigationsleiste in der `ViewDidLoad`-Methode von **ItemViewController.cs**, um Sie aufzurufen:

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Dadurch wird das Storyboard-Beispiel abgeschlossen – die fertige App sieht wie folgt aus:

[![fertige App](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

Das Beispiel veranschaulicht Folgendes:

- Erstellen einer Tabelle mit prototypinhalten, in denen Zellen zur erneuten Verwendung zum Anzeigen von Listen mit Daten definiert werden. 
- Erstellen einer Tabelle mit statischem Inhalt zum Erstellen eines Eingabeformulars. Dies umfasste das Ändern des Tabellen Stils und das Hinzufügen von Abschnitten, Zellen und UI-Steuerelementen. 
- Erstellen eines abgs und Überschreiben der `PrepareForSegue`-Methode, um die Ziel Ansicht aller erforderlichen Parameter zu benachrichtigen. 
- Die storyboardsichten werden direkt mit der `Storyboard.InstantiateViewController`-Methode geladen.

## <a name="related-links"></a>Verwandte Links

- [Storyboardtable (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable)
- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
