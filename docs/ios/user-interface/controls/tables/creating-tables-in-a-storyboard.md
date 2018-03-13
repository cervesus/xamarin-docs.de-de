---
title: Arbeiten mit Tabellen in der iOS-Designer
description: "In den vorherigen Abschnitten untersucht wir Entwickeln mit Tabellen. In diesem Abschnitt fünfte und letzte wir aggregieren, was wir bisher gelernt haben und erstellen eine basisanwendung der Aufgabenliste ein Storyboard mit."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8416E10-481A-0B6E-4081-B146E6358004
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: e46038b21327fe8847d2c04ee1ba16960f6a059b
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-tables-in-the-ios-designer"></a>Arbeiten mit Tabellen in der iOS-Designer

Storyboards sind eine WYSIWYG-Möglichkeit zum Erstellen von iOS-Anwendungen und innerhalb von Visual Studio auf Macintosh- und Windows unterstützt werden. Weitere Informationen zu Storyboards, finden Sie in der [Einführung zu Storyboards](~/ios/user-interface/storyboards/index.md) Dokument. Storyboards ermöglicht das Bearbeiten der Zelle Layouts *in* die Tabelle, die vereinfacht die Entwicklung mit Tabellen und Zellen

Bei Konfiguration der Eigenschaften einer Sicht für die Tabelle in der iOS-Designer stehen zwei Arten von Ihnen stehen Zelleninhalt: **dynamische** oder **statische** Prototyp-Inhalt.

<a name="Prototype_Content" />

## <a name="dynamic-prototype-content"></a>Dynamische Prototyp-Inhalt

Ein `UITableView` mit Prototyp Inhalt ist in der Regel dafür konzipiert, eine Liste der Daten anzuzeigen, in denen die Zelle Prototyp (oder Zellen, als Sie können definieren, mehr als eine) werden für jedes Element in der Liste erneut verwendet. Die Zellen müssen nicht instanziiert werden, erhalten sie der `GetView` Methode durch Aufrufen der `DequeueReusableCell` Methode seine `UITableViewSource`.

 <a name="Static_Content" />


## <a name="static-content"></a>Statischer Inhalt

`UITableView`s mit statischem Inhalt können Tabellen, auf der Entwurfsoberfläche nach rechts entworfen werden. Zellen können in die Tabelle gezogen und durch Ändern der Eigenschaften und Hinzufügen von Steuerelementen angepasst werden.

 <a name="Creating_a_Storyboard-driven_app" />


## <a name="creating-a-storyboard-driven-app"></a>Erstellen einer App Storyboard-Driven

Das StoryboardTable-Beispiel enthält eine einfache Master / Detail-app, die beide Arten von UITableView in einem Storyboard verwendet. Der übrige Teil dieses Abschnitts wird beschrieben, wie eine kleine to-Do-Liste-Beispiel zu erstellen, die nach Abschluss des Vorgangs aussieht:

 [![Beispiel-Bildschirme](creating-tables-in-a-storyboard-images/image13a.png)](creating-tables-in-a-storyboard-images/image13a.png#lightbox)

Ein Storyboard die Benutzeroberfläche erstellt werden, und beide Seiten einer UITableView verwendet. Verwendet den Hauptbildschirm *Prototyp Inhalt* Layout der Zeile, und an das Detail Bildschirm verwendet *statischer Inhalte* ein Dateneingabeformular mithilfe von benutzerdefinierten Zelle Layouts zu erstellen.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Erstellen Sie eine neue Projektmappe in Visual Studio mithilfe **(erstellen) in neues Projekt… > einzelne Ansicht App(C#)**, und nennen Sie es _StoryboardTables_.

 [![Erstellen Sie ein Dialogfeld "Neues Projekt"](creating-tables-in-a-storyboard-images/npd.png)](creating-tables-in-a-storyboard-images/npd.png#lightbox)

Die Lösung wird mit einigen C#-Dateien öffnen und eine `Main.storyboard` Datei, die bereits erstellt. Doppelklicken Sie auf die `Main.storyboard` Datei, die sie in der iOS-Designer zu öffnen.

<a name="Modifying_the_Storyboard" />

## <a name="modifying-the-storyboard"></a>Ändern das Storyboard

Das Storyboard wird in drei Schritten bearbeitet werden:

-  Erste, Layout, das die erforderlichen Domänencontroller anzeigen und ihre Eigenschaften festlegen.
-  Zweitens erstellen Sie Ihre Benutzeroberfläche durch Ziehen und Ablegen von Objekten in Ihrer Ansicht
-  Schließlich fügen Sie die erforderlichen UIKit-Klasse zu den einzelnen Sichten aus, und geben Sie verschiedene Steuerelemente einen Namen, damit sie im Code verwiesen werden können.


Wenn das Storyboard abgeschlossen ist, kann Code hinzugefügt werden, alles funktioniert vornehmen.

<a name="Layout_The_View_Controllers" />

### <a name="layout-the-view-controllers"></a>Layout-View-Controller

Die erste Änderung an der Storyboard wird durch das Löschen der vorhandenen Detailansicht und Ersetzen durch eine UITableViewController. Führen Sie folgende Schritte aus:

1.  Wählen Sie die Leiste am unteren Rand der View-Controller, und löschen Sie ihn.
2.  Ziehen Sie eine **Navigation Controller** und ein **Tabelle Modellansichtcontroller** auf das Storyboard aus der Toolbox. 
3.  Erstellen Sie eine Segue aus der Stamm-View-Controller auf der zweiten Tabelle-View-Controller, die soeben hinzugefügt wurde. Erstellen die Segue Steuerelement + ziehen *aus der Detailzelle* auf die neu hinzugefügte UITableViewController. Wählen Sie die Option **anzeigen*** unter **Segue Auswahl**. 
4.  Wählen Sie die neue segue Sie erstellt haben, und weisen Sie ihm einen Bezeichner zu verweisen, die dies segue im Code. Klicken Sie auf die Segue, und geben Sie `TaskSegue` für die **Bezeichner** in der **Eigenschaften Pad**, wie folgt:    
  [![Im Eigenschaftenbereich segue benennen](creating-tables-in-a-storyboard-images/image16a-sml.png)](creating-tables-in-a-storyboard-images/image16a.png#lightbox) 

5. Als Nächstes konfigurieren Sie die beiden Ansichten für die Tabelle, indem Sie sie auswählen und mithilfe der Eigenschaften. Achten Sie darauf, Ansicht und nicht die View-Controller auswählen – können Sie bei Auswahl der Dokumentgliederung.

6.  Ändern Sie die Stamm-View-Controller werden **Content: dynamische Prototypen** (die Ansicht auf der Entwurfsoberfläche wird etikettiert werden **Prototyp Content** ):

    [![Die Content-Eigenschaft festlegen, um dynamische Prototypen](creating-tables-in-a-storyboard-images/image17a.png)](creating-tables-in-a-storyboard-images/image17a.png#lightbox)

7.  Ändern Sie die neue **UITableViewController** werden **Content: statische Zellen**. 


8. Neue UITableViewController muss der Klassenname und ein Bezeichner festgelegt haben. Wählen Sie die View-Controller und den Typ _TaskDetailViewController_ für die **Klasse** in der **Eigenschaften Pad** – Dies erstellt eine neue `TaskDetailViewController.cs` Datei in der Projektmappe Mit Leerstellen auffüllen. Geben Sie die **StoryboardID** als _Detail_, wie im folgenden Beispiel veranschaulicht. Dies wird später verwendet werden, um diese Ansicht in C#-Code zu laden:  

    [![Festlegen der Storyboard-ID](creating-tables-in-a-storyboard-images/image18a.png)](creating-tables-in-a-storyboard-images/image18a.png#lightbox)

9. Storyboard-Entwurfsoberfläche sollte jetzt wie folgt aussehen (der Stamm-View-Controller Navigation Elementtitel geändert wurde "Mühsam Board"):

    [![Entwurfsoberfläche](creating-tables-in-a-storyboard-images/image20a-sml.png)](creating-tables-in-a-storyboard-images/image20a.png#lightbox)  



<a name="Create_the_UI" />

### <a name="create-the-ui"></a>Benutzeroberfläche erstellen

Nachdem die Ansichten und segues werden konfiguriert, die Elemente der Benutzeroberfläche hinzugefügt werden müssen.

#### <a name="root-view-controller"></a>Root View Controller

Wählen Sie zunächst die Prototyp Zelle in der Master-View-Controller und Festlegen der **Bezeichner** als _Taskcell_, wie unten gezeigt. Dies wird später im Code verwendet werden, um eine Instanz dieser UITableViewCell abzurufen:

 [![den zellenbezeichner festlegen](creating-tables-in-a-storyboard-images/image22a-sml.png)](creating-tables-in-a-storyboard-images/image22a.png#lightbox)

Auf diese Weise müssen Sie als Nächstes erstellen eine Schaltfläche, die neue Vorgänge hinzufügen, wie unten gezeigt:

[![Schaltfläche-Element in der Navigationsleiste](creating-tables-in-a-storyboard-images/image23-sml.png)](creating-tables-in-a-storyboard-images/image23.png#lightbox)

Führen Sie folgende Schritte aus: 

-  Ziehen Sie eine **Leiste Schaltflächenelement** aus der Toolbox an die _rechten Seite der Navigationsleiste_.
-  In der **Eigenschaften Pad**unter **Leiste Schaltflächenelement** wählen **Bezeichner: Hinzufügen** (zu erleichtern eine  *+*  plus Schaltfläche). 
-  Benennen Sie es so, dass er im Code zu einem späteren Zeitpunkt identifiziert werden kann. Beachten Sie, dass Sie die Stamm-View-Controller Geben Sie einen Klassennamen müssen (z. B. **ItemViewController**), die Leiste Schaltfläche den Namen des Elements festlegen können.


#### <a name="taskdetail-view-controller"></a>TaskDetail-View-Controller

Die Detailansicht erforderlich viel mehr Arbeit. Anzeigen von Tabellenzellen müssen auf die Ansicht gezogen und dann mit den Bezeichnungen, Text-Ansichten und Schaltflächen aufgefüllt werden. Der folgende Screenshot zeigt die Benutzeroberfläche nicht mehr benötigen, mit zwei Abschnitten. Ein Abschnitt verfügt über drei Zellen, drei Bezeichnungen, zwei Textfelder und ein wechseln, während im zweite Abschnitt über eine Zelle mit zwei Schaltflächen hat:

 [![Layouts für Auftragsdetails anzeigen](creating-tables-in-a-storyboard-images/image24a-sml.png)](creating-tables-in-a-storyboard-images/image24a.png#lightbox)

Die Schritte zum Erstellen des Layouts abgeschlossen sind:

Wählen Sie die Tabellenansicht, und öffnen Sie die **Eigenschaft Pad**. Aktualisieren Sie die folgenden Eigenschaften:

-  **Abschnitte**: _2_ 
-  **Stil**: _gruppiert_
-  **Trennzeichen**: _None_
-  **Auswahl**: _keine Auswahl getroffen wurde_

Wählen Sie im oberen Abschnitt und unter **Eigenschaften > Tabellenabschnitt** ändern **Zeilen** auf _3_, wie unten gezeigt:


 [![den oberen Abschnitt festlegen auf drei Zeilen](creating-tables-in-a-storyboard-images/image29-sml.png)](creating-tables-in-a-storyboard-images/image29.png#lightbox)

Für jede Zelle öffnen die **Eigenschaften Pad** und festlegen:

-  **Stil**: _benutzerdefinierte_
-  **Bezeichner**: Wählen Sie einen eindeutigen Bezeichner für jede Zelle (z. b. "_Titel_","_Hinweise_","_Fertig_").
-  Ziehen Sie die erforderlichen Steuerelemente zum Erstellen des Layouts im Screenshot gezeigten (platzieren Sie **UILabel**, **UITextField** und **UISwitch** auf die richtigen Zellen, und legen Sie die Bezeichnungen entsprechend, d. h. Titel, Hinweise und erfolgen).


Legen Sie im zweiten Abschnitt **Zeilen** auf _1_ und ziehen Sie den unteren Ziehpunkt der Zelle, die höher zu machen.

-  **Legen Sie den Bezeichner**: in einen eindeutigen Wert (z. b. "Speichern Sie"). 
-  **Festlegen des Hintergrunds**: _Löschen der Farbe_ .
-  Ziehen Sie zwei Schaltflächen auf die Zelle, und legen Sie ihre Titel entsprechend (d. h. _speichern_ und _löschen_), wie unten gezeigt:

   [![Festlegen von zwei Schaltflächen im unteren Abschnitt](creating-tables-in-a-storyboard-images/image30-sml.png)](creating-tables-in-a-storyboard-images/image30.png#lightbox)

An diesem Punkt können Sie auch Einschränkungen auf die Zellen und Steuerelemente, um sicherzustellen, dass ein Layout für adaptive festlegen möchten.

### <a name="adding-uikit-class-and-naming-controls"></a>Hinzufügen von UIKit-Klasse, und benennen Steuerelemente

Es gibt einige abschließende Schritte bei der Erstellung unserer Storyboard. Zuerst wir müssen benennen Sie jede der unsere Steuerelemente unter **Identität > Name** , damit sie später im Code verwendet werden können. Benennen Sie diese wie folgt:

-  **Titel UITextField** : _TitleText_
-  **Anmerkungen zu dieser UITextField** : _NotesText_
-  **UISwitch** : _DoneSwitch_
-  **Löschen von UIButton** : _DeleteButton_
-  **Speichern von UIButton** : _SaveButton_


<a name="Adding_Code" />

## <a name="adding-code"></a>Hinzufügen von Code

Im weiteren Verlauf der Arbeit in Visual Studio auf dem Mac oder Windows mit c# erfolgt. Beachten Sie, dass die Eigenschaftennamen im Code verwendet die in der exemplarischen Vorgehensweise oben widerspiegeln.

Zuerst möchten wir erstellen eine `Chores` -Klasse, die eine Möglichkeit zum Abrufen und Festlegen des Werts von ID, Name, Hinweise und zum fertig Boolean, bereitstellen, sodass wir diese Werte in der gesamten Anwendung verwenden können.

In Ihrem `Chores` Klasse fügen Sie den folgenden Code hinzu:

```csharp
public class Chores {
    public int Id { get; set; }
    public string Name { get; set; }
    public string Notes { get; set; }
    public bool Done { get; set; }
  }
```

Als Nächstes erstellen Sie eine `RootTableSource` erbt von `UITableViewSource`. 

Der Unterschied zwischen diesem und eine nicht-Storyboard-Tabellensicht ist, die die `GetView` Methode keine Zellen – instanziieren muss `theDequeueReusableCell` Methode gibt immer eine Instanz der Prototyp Zelle (mit übereinstimmender Bezeichner) zurück.

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

Verwenden der `RootTableSource` Klasse, erstellen Sie eine neue Sammlung in den `ItemViewController`Konstruktor:

```csharp
chores = new List<Chore> {
      new Chore {Name="Groceries", Notes="Buy bread, cheese, apples", Done=false},
      new Chore {Name="Devices", Notes="Buy Nexus, Galaxy, Droid", Done=false}
    };
```

In `ViewWillAppear` übergeben Sie die Auflistung an der Quelle und die Tabellenansicht zuweisen:

```csharp
public override void ViewWillAppear(bool animated)
{
    base.ViewWillAppear(animated);

    TableView.Source = new RootTableSource(chores.ToArray());
}
```

Wenn das Ausführen der app nun der Hauptbildschirm jetzt geladen und zeigt eine Liste der zwei Aufgaben. Wenn ein Task verwendet wird die definiert, indem Sie das Storyboard Segue führt dazu, dass die Detailbildschirm angezeigt werden, jedoch wird im Moment keine Daten anzuzeigen.

Um "einen Parameter in einer Segue senden", überschreiben die `PrepareForSegue` Methode und Festlegen von Eigenschaften für die `DestinationViewController` (die `TaskDetailViewController` in diesem Beispiel). Die Ziel-View-Controller-Klasse wird instanziiert, aber nicht noch angezeigt, die dem Benutzer – dies bedeutet, Sie können Eigenschaften für die Klasse festlegen, aber nicht ändern alle UI-Steuerelemente:

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

In `TaskDetailViewController` die `SetTask` Methode Eigenschaften Parameter zugewiesen, sodass in ViewWillAppear auf Sie verwiesen werden kann. Eigenschaften des Steuerelements können nicht geändert werden, `SetTask` da möglicherweise nicht vorhanden. wenn `PrepareForSegue` aufgerufen wird:

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

Die Segue wird jetzt öffnen Sie den Detailbildschirm und Informationen zum ausgewählten Vorgang angezeigt. Es ist leider keine Implementierung für die **speichern** und **löschen** Schaltflächen. Vor der Implementierung der Schaltflächen, fügen Sie diese Methoden **ItemViewController.cs** die zugrunde liegenden Daten aktualisieren, und schließen den Detailbildschirm:

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

Als Nächstes müssen Sie der Schaltfläche hinzufügen `TouchUpInside` -Ereignishandler, um die `ViewDidLoad` Methode **TaskDetailViewController.cs**. Der `Delegate` Eigenschaftenverweis auf die `ItemViewController` erstellt wurde, insbesondere, damit wir aufrufen können `SaveTask` und `DeleteTask`, die in dieser Ansicht schließen, als Teil ihrer Vorgangs:

```csharp
SaveButton.TouchUpInside += (sender, e) => {
        currentTask.Name = TitleText.Text;
        currentTask.Notes = NotesText.Text;
        currentTask.Done = DoneSwitch.On;
        Delegate.SaveTask(currentTask);
      };

DeleteButton.TouchUpInside += (sender, e) => Delegate.DeleteTask(currentTask);
```

Das letzte verbleibende Teil der Funktionen zum Erstellen ist die Erstellung neuer Aufgaben. In **ItemViewController.cs** hinzufügen, eine Methode, die neu erstellt, Aufgaben und die Detailansicht geöffnet. Instanziieren Sie eine Ansicht aus einem Storyboard verwenden die `InstantiateViewController` Methode mit dem `Identifier` für diese Ansicht – in diesem Beispiel, die "Detail" werden:

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

Abschließend die Schaltfläche in der Navigationsleiste in verknüpfen **ItemViewController.cs**des `ViewDidLoad` Methode aufrufen:

```csharp
AddButton.Clicked += (sender, e) => CreateTask ();
```

Somit ist das Storyboard-Beispiel – die fertige app sieht folgendermaßen aus:

[![Fertige app](creating-tables-in-a-storyboard-images/image28a.png)](creating-tables-in-a-storyboard-images/image28a.png#lightbox)

Das Beispiel veranschaulicht:

-  Erstellen eine Tabelle mit Prototyp Inhalt, auf die Zellen für die erneute Verwendung zum Anzeigen der Listen der Daten definiert werden. 
-  Erstellen eine Tabelle mit statischer Inhalt zum Erstellen einer Eingabe Formulars an. Diese enthalten das Tabellenformat ändern und Hinzufügen von Abschnitten, Zellen und Benutzeroberflächen-Steuerelemente. 
-  Erstellen eine Segue und überschreiben die `PrepareForSegue` Methode, um die Ziel-Ansicht aller Parameter benachrichtigen erfordert. 
-  Laden von Storyboard-Ansichten direkt mit der `Storyboard.InstantiateViewController` Methode.



## <a name="related-links"></a>Verwandte Links

- [StoryboardTable (Beispiel)](https://developer.xamarin.com/samples/monotouch/StoryboardTable/)
- [Einführung in Storyboards](~/ios/user-interface/storyboards/index.md)
