---
title: Bearbeiten von Tabellen mit xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Tabellen in xamarin. IOS bearbeitet werden. Er erläutert das Löschen, bearbeiten und Einfügen von Zeilen.
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 9960167e2f71531e5ffeaecac94aede5d5ea3340
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768893"
---
# <a name="editing-tables-with-xamarinios"></a>Bearbeiten von Tabellen mit xamarin. IOS

Tabellen Bearbeitungsfunktionen werden durch Überschreiben von Methoden in `UITableViewSource` einer Unterklasse aktiviert. Das einfachste Bearbeitungs Verhalten ist die Bewegung zum Löschen, die mit einer einzelnen Methoden Überschreibung implementiert werden kann.
Eine komplexere Bearbeitung (einschließlich der Zeilen Verschiebung) kann mit der Tabelle im Bearbeitungsmodus durchgeführt werden.

## <a name="swipe-to-delete"></a>Zum Löschen schwenken

Die Funktion zum Löschen zu löschen ist eine natürliche Geste in Ios, die von den Benutzern erwartet wird. 

 [![](editing-images/image10.png "Beispiel für den Löschvorgang")](editing-images/image10.png#lightbox)

Es gibt drei Methoden Überschreibungen, die die Schwenkbewegung beeinflussen, um eine **Lösch** Schaltfläche in einer Zelle anzuzeigen:

- **Commiteditingstyle** – die Tabellen Quelle erkennt, wenn diese Methode überschrieben wird, und aktiviert automatisch die Bewegung zum Löschen. Die-Implementierung der Methode sollte `DeleteRows` für den `UITableView` -Befehl auslösen, damit die Zellen ausgeblendet werden, und auch die zugrunde liegenden Daten aus dem Modell (z. b. ein Array, ein Wörterbuch oder eine Datenbank) entfernt werden. 
- **Caneditrow** – wenn commiteditingstyle überschrieben wird, wird davon ausgegangen, dass alle Zeilen bearbeitbar sind. Wenn diese Methode implementiert ist und false zurückgibt (für bestimmte Zeilen oder für alle Zeilen), ist die Bewegung zum Löschen in dieser Zelle nicht verfügbar. 
- **Titlefordeleteconfirmation** – gibt optional den Text für die Schaltfläche " **Löschen** " an. Wenn diese Methode nicht implementiert wird, lautet der Schaltflächen Text "Delete". 

Diese Methoden werden in der `TableSource` -Klasse folgendermaßen implementiert:

```csharp
public override void CommitEditingStyle (UITableView tableView, UITableViewCellEditingStyle editingStyle, Foundation.NSIndexPath indexPath)
{
    switch (editingStyle) {
        case UITableViewCellEditingStyle.Delete:
            // remove the item from the underlying data source
            tableItems.RemoveAt(indexPath.Row);
            // delete the row from the table
            tableView.DeleteRows (new NSIndexPath[] { indexPath }, UITableViewRowAnimation.Fade);
            break;
        case UITableViewCellEditingStyle.None:
            Console.WriteLine ("CommitEditingStyle:None called");
            break;
    }
}
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override string TitleForDeleteConfirmation (UITableView tableView, NSIndexPath indexPath)
{   // Optional - default text is 'Delete'
    return "Trash (" + tableItems[indexPath.Row].SubHeading + ")";
}
```

In diesem Beispiel `UITableViewSource` wurde aktualisiert, sodass ein `List<TableItem>` (anstelle eines Zeichen folgen Arrays) als Datenquelle verwendet wird, da es das Hinzufügen und Löschen von Elementen aus der Auflistung unterstützt.

## <a name="edit-mode"></a>Bearbeitungsmodus

Wenn sich eine Tabelle im Bearbeitungsmodus befindet, wird dem Benutzer ein rotes "beenden"-Widget für jede Zeile angezeigt, in dem eine Schaltfläche "Löschen" angezeigt wird. In der Tabelle wird auch ein Handle-Symbol angezeigt, um anzugeben, dass die Zeile gezogen werden kann, um die Reihenfolge zu ändern.
Das **tableeditmode** -Beispiel implementiert diese Funktionen wie gezeigt.

 [![](editing-images/image11.png "Das tableeditmode-Beispiel implementiert diese Features wie gezeigt.")](editing-images/image11.png#lightbox)

Es gibt eine Reihe von unterschiedlichen Methoden `UITableViewSource` , die sich auf das Verhalten der Tabellen im Bearbeitungsmodus auswirken:

- **Caneditrow** – gibt an, ob die einzelnen Zeilen bearbeitet werden können. Gibt false zurück, um zu verhindern, dass das Löschen und löschen im Bearbeitungsmodus ausgeführt wird. 
- **Canmoverow** – gibt true zurück, um das Verschieben von ' handle ' oder false zu aktivieren, um das Verschieben zu verhindern. 
- **Editingstyleforrow** – wenn sich die Tabelle im Bearbeitungsmodus befindet, bestimmt der Rückgabewert dieser Methode, ob die Zelle das rote Lösch Symbol oder das grüne Symbol "hinzufügen" anzeigt. Gibt `UITableViewCellEditingStyle.None` zurück, wenn die Zeile nicht bearbeitet werden darf. 
- " **–"** Wird aufgerufen, wenn eine Zeile verschoben wird, sodass die zugrunde liegende Datenstruktur so geändert werden kann, dass Sie den in der Tabelle angezeigten Daten entspricht. 

Die Implementierung für die ersten drei Methoden ist relativ unkompliziert – es sei denn, Sie möchten zum `indexPath` Ändern des Verhaltens bestimmter Zeilen verwenden, sondern nur die Rückgabewerte für die gesamte Tabelle hart codieren.

```csharp
public override bool CanEditRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you wish to disable editing for a specific indexPath or for all rows
}
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return true; // return false if you don't allow re-ordering
}
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    return UITableViewCellEditingStyle.Delete; // this example doesn't support Insert
}
```

Die `MoveRow` Implementierung ist etwas komplizierter, da Sie die zugrunde liegende Datenstruktur ändern muss, damit Sie der neuen Reihenfolge entspricht. Da die Daten als implementiert werden, `List` löscht der nachfolgende Code das Datenelement am alten Speicherort und fügt es am neuen Speicherort ein. Wenn die Daten in einer SQLite-Datenbanktabelle mit einer Order-Spalte (z. b.) gespeichert wurden, müsste diese Methode stattdessen einige SQL-Vorgänge durchführen, um die Zahlen in dieser Spalte neu anzuordnen.

```csharp
public override void MoveRow (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath destinationIndexPath)
{
    var item = tableItems[sourceIndexPath.Row];
    var deleteAt = sourceIndexPath.Row;
    var insertAt = destinationIndexPath.Row;
    
    // are we inserting 
    if (destinationIndexPath.Row < sourceIndexPath.Row) {
        // add one to where we delete, because we're increasing the index by inserting
        deleteAt += 1;
    } else {
        // add one to where we insert, because we haven't deleted the original yet
        insertAt += 1;
    }
    tableItems.Insert (insertAt, item);
    tableItems.RemoveAt (deleteAt);
}
```

Zum Schluss muss die Schaltfläche " **Bearbeiten** " wie folgt aufgerufen `SetEditing` werden, um die Tabelle in den Bearbeitungsmodus zu bringen.

```csharp
table.SetEditing (true, true);
```

und wenn der Benutzer die Bearbeitung abgeschlossen hat, sollte die Schaltfläche **Fertig** den Bearbeitungsmodus deaktivieren:

```csharp
table.SetEditing (false, true);
```

## <a name="row-insertion-editing-style"></a>Bearbeitungs Stil für Zeilen Einfügung

Das Einfügen von Zeilen innerhalb der Tabelle ist eine ungewöhnliche Benutzeroberfläche – das Hauptbeispiel in den standardmäßigen IOS-Apps ist der Bildschirm zum **Bearbeiten von Kontakten** . Dieser Screenshot zeigt, wie die Funktion zum Einfügen von Zeilen funktioniert – im Bearbeitungsmodus gibt es eine zusätzliche Zeile, die (wenn geklickt) zusätzliche Zeilen in die Daten einfügt. Wenn die Bearbeitung beendet ist, wird die temporäre Zeile **(neue Zeile hinzufügen)** entfernt.

 [![](editing-images/image12.png "Wenn die Bearbeitung beendet ist, wird die temporäre neue Zeile hinzufügen entfernt.")](editing-images/image12.png#lightbox)

Es gibt eine Reihe von unterschiedlichen Methoden `UITableViewSource` , die sich auf das Verhalten der Tabellen im Bearbeitungsmodus auswirken. Diese Methoden wurden im Beispielcode wie folgt implementiert:

- **Editingstyleforrow** – gibt `UITableViewCellEditingStyle.Delete` für die Zeilen zurück, die Daten enthalten `UITableViewCellEditingStyle.Insert` , und gibt für die letzte Zeile zurück (die sich speziell als einfügeschaltfläche verhält). 
- **Customizemuvetarget** – während der Benutzer eine Zelle verschiebt, kann der Rückgabewert aus dieser optionalen Methode den gewählten Speicherort überschreiben. Dies bedeutet, dass Sie verhindern können, dass Sie die Zelle an bestimmten Positionen ablegen – z. b. in diesem Beispiel, das verhindert, dass Zeilen nach der **(neuen)** Zeile verschoben werden. 
- **Canmoverow** – gibt true zurück, um das Verschieben von ' handle ' oder false zu aktivieren, um das Verschieben zu verhindern. In diesem Beispiel hat die letzte Zeile das Verschieben von ' handle ' ausgeblendet, da es nur als einfügeschaltfläche verwendet werden soll. 

Außerdem fügen wir zwei benutzerdefinierte Methoden zum Hinzufügen der Zeile "Einfügen" hinzu und entfernen Sie dann erneut, wenn Sie nicht mehr benötigt werden. Sie werden über **die Schaltflächen** " **Bearbeiten** " und "ausführen" aufgerufen:

- **Willbegintableedit** – wenn die **Bearbeitungs** Schaltfläche berührt ist `SetEditing` , ruft Sie auf, um die Tabelle in den Bearbeitungsmodus zu versetzen. Dadurch wird die Methode "willbegintableediting" ausgelöst, in der die Zeile **(Add New)** am Ende der Tabelle angezeigt wird, die als "Einfügen"-Schaltfläche fungiert. 
- **Didfinishtableedit** – wenn die done-Schaltfläche `SetEditing` berührt ist, wird erneut aufgerufen, um den Bearbeitungsmodus zu deaktivieren. Im Beispielcode wird die Zeile **(neue Zeile hinzufügen)** aus der Tabelle entfernt, wenn die Bearbeitung nicht mehr erforderlich ist. 

Diese Methoden Überschreibungen sind in der Beispieldatei **tableeditmodeadd/Code/tablesource. cs**implementiert:

```csharp
public override UITableViewCellEditingStyle EditingStyleForRow (UITableView tableView, NSIndexPath indexPath)
{
    if (tableView.Editing) {
        if (indexPath.Row == tableView.NumberOfRowsInSection (0) - 1)
            return UITableViewCellEditingStyle.Insert;
        else
            return UITableViewCellEditingStyle.Delete;
    } else // not in editing mode, enable swipe-to-delete for all rows
        return UITableViewCellEditingStyle.Delete;
}
public override NSIndexPath CustomizeMoveTarget (UITableView tableView, NSIndexPath sourceIndexPath, NSIndexPath proposedIndexPath)
{
    var numRows = tableView.NumberOfRowsInSection (0) - 1; // less the (add new) one
    if (proposedIndexPath.Row >= numRows)
        return NSIndexPath.FromRowSection(numRows - 1, 0);
    else
        return proposedIndexPath;
} 
public override bool CanMoveRow (UITableView tableView, NSIndexPath indexPath)
{
    return indexPath.Row < tableView.NumberOfRowsInSection (0) - 1;
}
```

Diese beiden benutzerdefinierten Methoden werden verwendet, um die Zeile **(Add New)** hinzuzufügen und zu entfernen, wenn der Bearbeitungsmodus der Tabelle aktiviert oder deaktiviert ist:

```csharp
public void WillBeginTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // insert the 'ADD NEW' row at the end of table display
    tableView.InsertRows (new NSIndexPath[] { 
            NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0), 0) 
        }, UITableViewRowAnimation.Fade);
    // create a new item and add it to our underlying data (it is not intended to be permanent)
    tableItems.Add (new TableItem ("(add new)"));
    tableView.EndUpdates (); // applies the changes
}
public void DidFinishTableEditing (UITableView tableView)
{
    tableView.BeginUpdates ();
    // remove our 'ADD NEW' row from the underlying data
    tableItems.RemoveAt ((int)tableView.NumberOfRowsInSection (0) - 1); // zero based :)
    // remove the row from the table display
    tableView.DeleteRows (new NSIndexPath[] { NSIndexPath.FromRowSection (tableView.NumberOfRowsInSection (0) - 1, 0) }, UITableViewRowAnimation.Fade);
    tableView.EndUpdates (); // applies the changes
}
```

Schließlich instanziiert dieser Code die Schaltflächen **Bearbeiten** und **Fertig** mit Lambdas, die den Bearbeitungsmodus aktivieren oder deaktivieren, wenn Sie berührt werden:

```csharp
done = new UIBarButtonItem(UIBarButtonSystemItem.Done, (s,e)=>{
    table.SetEditing (false, true);
    NavigationItem.RightBarButtonItem = edit;
    tableSource.DidFinishTableEditing(table);
});
edit = new UIBarButtonItem(UIBarButtonSystemItem.Edit, (s,e)=>{
    if (table.Editing)
        table.SetEditing (false, true); // if we've half-swiped a row
    tableSource.WillBeginTableEditing(table);
    table.SetEditing (true, true);
    NavigationItem.LeftBarButtonItem = null;
    NavigationItem.RightBarButtonItem = done;
});
```

Dieses UI-Muster für die Zeilen Einfügung wird nicht häufig verwendet, Sie können jedoch `UITableView.BeginUpdates` auch `EndUpdates` die-Methode und die-Methode verwenden, um das Einfügen oder Entfernen von Zellen in einer beliebigen Tabelle zu animieren. Die Regel für die Verwendung dieser Methoden besteht darin, dass der Unterschied `RowsInSection` des Werts `BeginUpdates` , `EndUpdates` der von zwischen den aufrufen und zurückgegeben wird, mit der Anzahl `InsertRows` der `DeleteRows` Zellen, die der-Methode und der-Methode hinzugefügt/ Wenn die zugrunde liegende Datenquelle nicht so geändert wird, dass Sie den Einfügungen/Löschungen der Tabellenansicht entspricht, tritt ein Fehler auf.

## <a name="related-links"></a>Verwandte Links

- [Workingwithtables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
