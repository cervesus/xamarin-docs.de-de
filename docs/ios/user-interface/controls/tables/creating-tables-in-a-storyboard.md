---
title: Arbeiten mit Tabellen in der iOS-Designer
description: In den vorherigen Abschnitten vorgestellt, die mit der Tabellen-Entwicklung. In diesem Abschnitt fünfte und letzte wir aggregieren, was wir bisher gelernt haben und eine Aufgabenliste-basisanwendung mit einem Storyboard zu erstellen.
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: d161a267c8ffa5040327db8e6e4f867a324b04f2
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105805"
---
# <a name="working-with-tables-in-the-ios-designer"></a>Arbeiten mit Tabellen in der iOS-Designer

Storyboards sind eine WYSIWYG-Möglichkeit zum Erstellen von iOS-Anwendungen und innerhalb von Visual Studio für Mac und Windows verwendet werden. Weitere Informationen zu Storyboards, finden Sie in der [Einführung zu Storyboards](~/ios/user-interface/storyboards/index.md) Dokument. Storyboards können Sie so bearbeiten Sie die Zelle Layouts außerdem *in* die Tabelle, die vereinfacht die Entwicklung mit Tabellen und Zellen

Beim Konfigurieren der Eigenschaften einer Tabellenansicht im iOS-Designer, es gibt zwei Arten von Inhalt der Zelle können Sie auswählen: **dynamische** oder **statische** Prototyp-Inhalt.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Dynamische Prototyp-Inhalt

Ein `UITableView` mit Prototyp Inhalt in der Regel sollte für eine Liste der Daten anzuzeigen, in denen die Zelle Prototyp (oder Zellen, als Sie können definieren, mehr als eine) werden für jedes Element in der Liste erneut verwendet. Die Zellen müssen nicht instanziiert werden, erhalten sie der `GetView` -Methode durch Aufrufen der `DequeueReusableCell` -Methode der seine `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>Statischer Inhalt

`UITableView`s mit statischem Inhalt können Tabellen, auf der Entwurfsoberfläche entworfen werden. Zellen in der Tabelle gezogen, und durch Ändern der Eigenschaften und Hinzufügen von Steuerelementen angepasst werden können.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>Erstellen einer Storyboard-Driven App

Im Beispiel StoryboardTable enthält eine einfachen Master / Detail-app, die beide Arten von UITableView in einem Storyboard verwendet. Der übrige Teil dieses Abschnitts wird beschrieben, wie das Beispiel für eine kleine-to-Do-Liste erstellen, die nach Abschluss des Vorgangs aussieht:

 [![Beispiel-Bildschirme](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

Die Benutzeroberfläche mit einem Storyboard erstellt werden, und beiden Bildschirmen werden eine UITableView verwenden. Verwendet der Hauptbildschirm *Prototyp Inhalt* Layout der Zeile, und die Details Screen verwendet die *statische Inhalte* ein Dateneingabeformular mithilfe von benutzerdefinierten Zelle Layouts zu erstellen.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Erstellen Sie eine neue Projektmappe in Visual Studio mithilfe **(erstellen) Neues Projekt > Einzelansicht-App (C#)**, und nennen Sie sie _StoryboardTables_.

 [![Erstellen Sie ein Dialogfeld "Neues Projekt"](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

Öffnen Sie die Projektmappe wird mit einigen C# Dateien und eine `Main.storyboard` Datei, die bereits erstellt. Doppelklicken Sie auf die `Main.storyboard` Datei, die sie in der iOS-Designer zu öffnen.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Ändern das Storyboard

Das Storyboard wird in drei Schritten bearbeitet werden:

-  Erste, Layout, das die erforderlichen Domänencontroller anzuzeigen und deren Eigenschaften festlegen.
-  Zweitens: Erstellen Sie die Benutzeroberfläche durch Ziehen und Ablegen von Objekten in der Ansicht
-  Abschließend fügen Sie die erforderlichen UIKit-Klasse zu den einzelnen Sichten aus, und geben Sie verschiedene Steuerelemente einen Namen, damit sie im Code verwiesen werden können.


Sobald das Storyboard abgeschlossen ist, kann Code hinzugefügt werden, um alles zu machen.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Layout der View-Controller

Die erste Änderung an das Storyboard ist das Löschen der vorhandenen Detailansicht, und Ersetzen durch eine UITableViewController. Führen Sie folgende Schritte aus:

1.  Wählen Sie die Leiste am unteren Rand der Ansichtscontroller und gelöscht werden.
2.  Ziehen Sie eine **Navigationscontroller** und **Tabellenansichtscontroller** auf das Storyboard aus der Toolbox. 
3.  Erstellen Sie eines segues aus der Root View Controller, auf der zweiten Tabelle-View-Controller, das soeben hinzugefügt wurde. Erstellen den Segue, Control + ziehen *aus der Detailzelle* auf die neu hinzugefügte UITableViewController. Wählen Sie die Option **anzeigen*** unter **Segue Auswahl**. 
4.  Wählen Sie das neue erstellte segue und weisen ihm eine ID zu verweisen, die dieser segue im Code. Klicken Sie auf der Segue, und geben Sie `TaskSegue` für die **Bezeichner** in die **Pad "Eigenschaften"**, wie folgt aus:    
  [![Im Eigenschaftenbereich segue benennen](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. Als Nächstes konfigurieren Sie die beiden Ansichten für die Tabelle, indem Sie sie auswählen und das Pad "Eigenschaften". Achten Sie darauf, wählen Sie die Ansicht und nicht die View-Controller – Sie können die Dokumentgliederung verwenden, um Unterstützung bei der Auswahl.

6.  Ändern der Root View Controller als **Inhalt: dynamische Prototypen** (die Ansicht auf der Entwurfsoberfläche wird bezeichnet **Prototyp Content** ):

    [![Die Content-Eigenschaft wird auf dynamische Prototypen festlegen.](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  Ändern Sie die neue **UITableViewController** sein **Inhalt: statische Zellen**. 


8. Die neue UITableViewController müssen die Klassennamen und einen Bezeichner festgelegt. Wählen Sie den Ansichtscontroller und den Typ _TaskDetailViewController_ für die **Klasse** in die **Pad "Eigenschaften"** – Hiermit wird ein neues `TaskDetailViewController.cs` Datei in der Projektmappe Pad. Geben Sie die **StoryboardID** als _Detail_, wie im folgenden Beispiel dargestellt. Dies wird später verwendet, laden diese Ansicht in C# Code:  

    [![Festlegen der Storyboard-ID](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. Die Storyboard-Entwurfsoberfläche sollte jetzt wie folgt aussehen (Root View Controller-Navigation Elementtitel geändert wurde auf"Aufgabe"):

    [![Entwurfsoberfläche](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Benutzeroberfläche erstellen

Nachdem die Ansichten und Übergänge werden konfiguriert, die Elemente der Benutzeroberfläche hinzugefügt werden müssen.

#### <a name="root-view-controller"></a>Root View Controller

Wählen Sie zunächst die Prototyp-Zelle in der Master-View-Controller und Festlegen der **Bezeichner** als _Taskcell_, wie unten dargestellt. Dies wird später im Code verwendet werden, um eine Instanz dieser UITableViewCell abzurufen:

 [![der zellenbezeichner festlegen](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

Auf diese Weise müssen Sie als Nächstes erstellen eine Schaltfläche, die neue Aufgaben hinzugefügt wird, wie unten gezeigt:

[![Schaltfläche "-Element in der Navigationsleiste](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

Führen Sie folgende Schritte aus: 

-  Ziehen Sie eine **Schaltflächenelement** aus der Toolbox an die _der Navigationsleiste rechts_.
-  In der **Pad "Eigenschaften"** unter **Leiste Schaltflächenelement** wählen **Bezeichner: Hinzufügen** (zu vereinfachen ein *+* plus Schaltfläche). 
-  Benennen Sie es, damit es zu einem späteren Zeitpunkt im Code identifiziert werden kann. Beachten Sie, dass Sie der Root View Controller-Klasse benennen müssen (z. B. **ItemViewController**), sodass Sie Balken Schaltfläche den Namen des Elements festlegen können.


#### <a name="taskdetail-view-controller"></a>TaskDetail-Ansichtscontroller

Die Detailansicht erfordert viel mehr Arbeit. Anzeigen der Tabellenzellen müssen auf die Ansicht gezogen und dann mit den Beschriftungen, Text-Ansichten und Schaltflächen aufgefüllt werden. Der folgende Screenshot zeigt die fertige Benutzeroberfläche mit zwei Abschnitte. Ein Abschnitt verfügt über drei Zellen, drei Bezeichnungen, zwei Textfelder und eine wechseln, während der zweite Abschnitt eine Zelle mit zwei Schaltflächen enthält:

 [![Layouts für Auftragsdetails anzeigen](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

Sind die Schritte, um das vollständige Layout zu erstellen:

Wählen Sie die Tabellenansicht, und öffnen Sie die **Pad "Eigenschaft"**. Aktualisieren Sie die folgenden Eigenschaften:

-  **Abschnitte**: _2_ 
-  **Stil**: _gruppiert_
-  **Trennzeichen**: _keine_
-  **Auswahl**: _keine Auswahl_

Wählen Sie im oberen Abschnitt und unter **Eigenschaften > Tabelle "im Abschnitt"** ändern **Zeilen** zu _3_, wie unten gezeigt:


 [![Wenn im oberen Abschnitt auf drei Zeilen](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

Für jede Zelle öffnen die **Pad "Eigenschaften"** und festlegen:

-  **Stil**: _benutzerdefinierte_
-  **Bezeichner**: Wählen Sie einen eindeutigen Bezeichner für jede Zelle (z. b. "_Titel_","_Anmerkungen zu dieser_","_Fertig_").
-  Ziehen Sie die erforderlichen Steuerelemente, um das Layout im Screenshot gezeigten zu erzeugen (platzieren Sie **UILabel**, **UITextField** und **UISwitch** auf die richtigen Zellen, und legen Sie die Bezeichnungen entsprechend, d. h. Titel, Hinweise und abgeschlossen).


Legen Sie im zweiten Abschnitt **Zeilen** zu _1_ und klicken Sie auf den unteren Ziehpunkt der Zelle, die sie höher zu machen.

-  **Legen Sie den Bezeichner**: in einen eindeutigen Wert (z. b. "Speichern Sie"). 
-  **Festlegen des Hintergrunds**: _Löschen der Farbe_ .
-  Ziehen Sie zwei Schaltflächen in der Zelle, und legen Sie deren Titel entsprechend fest (d. h. _speichern_ und _löschen_), wie unten gezeigt:

   [![Festlegen von zwei Schaltflächen im unteren Abschnitt](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

An diesem Punkt können Sie auch Einschränkungen für die Zellen und Steuerelemente, um sicherzustellen, dass ein Layout für die adaptive festlegen möchten.

### <a name="adding-uikit-class-and-naming-controls"></a>Hinzufügen von UIKit-Klasse, und benennen die Steuerelemente

Es gibt einige Letzte Schritte bei der Erstellung unserer Storyboard. Zuerst wir müssen benennen Sie jede der unsere Steuerelemente unter **Identität > Namen** , damit sie später im Code verwendet werden können. Benennen Sie diese wie folgt:

-  **Titel UITextField** : _TitleText_
-  **Anmerkungen zu dieser UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **Löschen von UIButton** : _DeleteButton_
-  **Speichern Sie UIButton** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>Hinzufügen von Code

Der Rest der Arbeit erfolgt in Visual Studio unter Mac oder Windows mit C#. Beachten Sie, dass die Eigenschaftennamen im Code verwendet den in der obigen exemplarischen Vorgehensweise festgelegten widerspiegeln.

Zunächst soll zum Erstellen einer `Chores` -Klasse, die bieten wird eine Möglichkeit, den Wert von ID, Name, Anmerkungen zu dieser Version und die fertig Boolesch, abrufen und festlegen, damit wir diese Werte in der gesamten Anwendung verwenden können.

In Ihrer `Chores` Klasse fügen Sie den folgenden Code hinzu:

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Als Nächstes erstellen Sie eine `RootTableSource` erbt von `UITableViewSource`. 

Der Unterschied zwischen diesem und einer Tabellenansicht für nicht-Storyboard ist, dass die `GetView` Methode muss keine Zellen – instanziieren `theDequeueReusableCell` Methode gibt immer eine Instanz der Prototyp Zelle (mit übereinstimmenden Bezeichner) zurück.

Der folgende Code stammt aus dem `RootTableSource.cs` Datei:

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

Verwenden der `RootTableSource` Klasse, erstellen Sie eine neue Sammlung in der `ItemViewController`Konstruktor:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

In `ViewWillAppear` übergeben Sie die Auflistung an der Quelle, und weisen Sie der Tabelle anzeigen:

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Wenn Sie die app ausführen, der Hauptbildschirm jetzt lädt und zeigt eine Liste der zwei Aufgaben. Wenn eine Aufgabe berührt, wird der definiert, von dem Storyboard Segue führt dazu, dass den Detailbildschirm angezeigt werden, jedoch wird derzeit keine Daten angezeigt.

Um "einen Parameter in einem Segue senden", überschreiben die `PrepareForSegue` Methode und legen Sie Eigenschaften für die `DestinationViewController` (die `TaskDetailViewController` in diesem Beispiel). Die Ziel-View-Controller-Klasse wird instanziiert wurde, aber nicht noch angezeigt, die dem Benutzer – dies bedeutet, Sie können Eigenschaften für die Klasse festlegen, aber nicht ändern UI-Steuerelementen:

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

In `TaskDetailViewController` der `SetTask` Methode Eigenschaften seiner Parameter zugewiesen, damit sie in ViewWillAppear verwiesen werden können. Eigenschaften des Steuerelements können nicht geändert werden, `SetTask` da möglicherweise nicht vorhanden. wenn `PrepareForSegue` aufgerufen wird:

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

Der Segue jetzt den Detailbildschirm zu öffnen und die Informationen zum ausgewählten Vorgang angezeigt. Leider besteht keine Implementierung für die **speichern** und **löschen** Schaltflächen. Fügen Sie vor der Implementierung der Schaltflächen, die diese Methoden **ItemViewController.cs** , aktualisieren die zugrunde liegenden Daten, und schließen den Detailbildschirm:

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

Als Nächstes müssen Sie der Schaltfläche hinzufügen `TouchUpInside` -Ereignishandler der `ViewDidLoad` -Methode der **TaskDetailViewController.cs**. Die `Delegate` Eigenschaftenverweis auf die `ItemViewController` wurde erstellt, insbesondere, sodass wir aufrufen können `SaveTask` und `DeleteTask`, schließen Sie diese Ansicht der im Rahmen des Vorgangs:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

Der letzte verbleibende Schritt Funktionen zum Erstellen, ist die Erstellung von neuen Aufgaben. In **ItemViewController.cs** Hinzufügen einer Methode, die neu erstellt Aufgaben und die Detailansicht geöffnet. Um eine Ansicht aus einer Storyboard Instanziieren der `InstantiateViewController` -Methode mit der `Identifier` für diese Ansicht – in diesem Beispiel, die "Detail" werden:

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

Abschließend die Schaltfläche in der Navigationsleiste in socialsharing.Share **ItemViewController.cs**des `ViewDidLoad` Methode aufgerufen:

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Der abgeschlossen wird die Storyboard-Beispiel: die fertige app sieht wie folgt aus:

[![Fertige app](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

Das Beispiel veranschaulicht:

-  Erstellen eine Tabelle mit Prototyp Inhalt, auf die Zellen für die erneute Verwendung zum Anzeigen von Listen mit Daten definiert werden. 
-  Erstellen eine Tabelle mit statischem Inhalt, um ein Eingabeformular zu erstellen. Dazu gehörten das Tabellenformat ändern und Hinzufügen von Abschnitten, Zellen und UI-Steuerelemente. 
-  Das Erstellen eines segues und überschreiben die `PrepareForSegue` Methode, um die Ziel-Ansicht aller Parameter zu benachrichtigen, es erfordert. 
-  Laden der Storyboard-Ansichten direkt mit der `Storyboard.InstantiateViewController` Methode.



## <a name="related-links"></a>Verwandte Links

- [StoryboardTable (Beispiel)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
