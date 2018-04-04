---
title: Bearbeiten
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: 161de0209217dde671b976afad90eaad18d8c7b0
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="editing"></a>Bearbeiten

Features für die Bearbeitung der Tabelle sind aktiviert, durch Überschreiben der Methoden in einer `UITableViewSource` Unterklasse. Das einfachste Bearbeitungsverhalten ist Bewegung Streifen zu löschen, die mit einer einzelnen methodenüberschreibung implementiert werden kann.
Komplexere bearbeiten (einschließlich verschieben Datenzeilen) kann mit der Tabelle im Bearbeitungsmodus erfolgen.

## <a name="swipe-to-delete"></a>Wischen löschen

Die Wischen So löschen Sie die Funktion ist eine natürliche Geste in iOS, die Benutzer erwarten. 

 [![](editing-images/image10.png "Beispiel für Wischen löschen")](editing-images/image10.png#lightbox)

Es gibt drei Methode überschreibt, die Bewegung Wischen anzuzeigende beeinflussen eine **löschen** Schaltfläche in einer Zelle:

-   **CommitEditingStyle** – Tabellenquelle erkennt, wenn diese Methode außer Kraft gesetzt, und automatisch den Gestenhandler Streifen zu löschen aktiviert. Die Implementierung der Methode sollten Aufrufen `DeleteRows` auf die `UITableView` , sodass die Zellen ausgeblendet, und entfernen die zugrunde liegenden Daten auch aus dem Modell (z. B. ein Array, Wörterbuch oder Datenbank). 
-   **CanEditRow** – Wenn CommitEditingStyle überschrieben wird, sind davon ausgegangen, dass alle Zeilen, die bearbeitet werden. Wenn diese Methode implementiert wird und "false" (für einige bestimmten Zeilen oder für alle Zeilen) zurückgegeben wird dann die Bewegung Streifen zu löschen nicht in dieser Zelle verfügbar. 
-   **TitleForDeleteConfirmation** – optional gibt den Text für die **löschen** Schaltfläche. Wenn diese Methode nicht implementiert, wird der Text der Schaltfläche "Löschen" werden. 


Diese Methoden werden implementiert, der `TableSource` -Klasse folgt:

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

In diesem Beispiel die `UITableViewSource` wurde aktualisiert, um die Verwendung einer `List<TableItem>` (statt ein Array von Zeichenfolgen) wie die Datenquelle, da er unterstützt hinzufügen und Löschen von Elementen aus der Auflistung.


## <a name="edit-mode"></a>Bearbeitungsmodus

Wenn eine Tabelle im Bearbeitungsmodus befindet sieht der Benutzer ein rotes "stop" Widget für jede Zeile, eine Schaltfläche "löschen", wenn der verwendete festgestellt werden. Die Tabelle enthält auch ein "Handle"-Symbol, um anzugeben, dass die Zeile gezogen werden kann, um die Reihenfolge ändern.
Die **TableEditMode** Beispiel implementiert diese Funktionen, wie dargestellt.

 [![](editing-images/image11.png "Das Beispiel TableEditMode implementiert diese Funktionen, wie dargestellt")](editing-images/image11.png#lightbox)

Es gibt eine Reihe von verschiedenen Methoden auf `UITableViewSource` , von denen beeinflusst das Verhalten einer Tabelle bearbeiten:

-   **CanEditRow** – ob jede Zeile bearbeitet werden kann. Geben Sie zur Vermeidung von Streifen zu löschen und löschen, klicken Sie im Bearbeitungsmodus "false" zurück. 
-   **CanMoveRow** – return true, aktivieren Sie die Move "Handle" oder "false", um zu verhindern, dass verschieben. 
-   **EditingStyleForRow** – Wenn die Tabelle im Bearbeitungsmodus befindet, ist der Rückgabewert dieser Methode bestimmt, ob die Zelle das Symbol "red löschen zeigt" oder über die grüne Symbol "hinzufügen". Zurückgeben `UITableViewCellEditingStyle.None` , wenn die Zeile nicht bearbeitet werden soll. 
-   **Gemachten MoveRow** – wird aufgerufen, wenn eine Zeile verschoben wird, sodass die Datenstruktur der zugrunde liegenden geändert werden kann, um die Daten anzuzeigen, wie er in der Tabelle angezeigt wird. 


Die Implementierung für die ersten drei Methoden ist relativ unkompliziert – es sei denn, Sie möchten die `indexPath` um das Verhalten bestimmter Zeilen zu ändern, nur hartcodieren das zurückgegebene Werte, für die gesamte Tabelle.

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

Die `MoveRow` Implementierung ist etwas komplizierter, da sie neu, die zugrunde liegende Datenstruktur entsprechend die neue Reihenfolge zu ändern. Da die Daten als implementiert, werden eine `List` der folgenden Code wird das Datenelement am alten Speicherort gelöscht und fügt es am neuen Speicherort. Wenn die Daten (z. B.) in einer Datenbanktabelle SQLite mit einer Spalte "Order" gespeichert wurde, müssen stattdessen diese Methode durchführen bestimmter Vorgänge SQL, um die Zahlen in dieser Spalte zu sortieren.

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

Schließlich zum Abrufen die Tabelle in den Bearbeitungsmodus, **bearbeiten** Schaltfläche aufrufen muss `SetEditing` folgendermaßen

```csharp
table.SetEditing (true, true);
```

und nach Abschluss der Benutzer bearbeiten, die **Fertig** Schaltfläche Bearbeitungsmodus deaktivieren sollten:

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>Zeilenformat einfügen bearbeiten

Zeile einfügen von innerhalb der Tabelle wird eine ungewöhnlich Benutzeroberfläche – ist das wichtigste Beispiel in der standard-iOS-apps die **Kontakt bearbeiten** Bildschirm. Diese Abbildung zeigt, wie die Zeile einfügen-Funktionalität funktioniert – in Bearbeitung-Modus eine zusätzliche Zeile, die ist (wenn geklickt haben), zusätzliche Zeilen in die Daten eingefügt. Wenn die Bearbeitung abgeschlossen ist, die temporäre **(neuen hinzufügen)** Zeile entfernt.

 [![](editing-images/image12.png "Nach Abschluss der Bearbeitung hinzufügen die temporäre neue Zeile wird entfernt.")](editing-images/image12.png#lightbox)

Es gibt eine Reihe von verschiedenen Methoden auf `UITableViewSource` , von denen beeinflusst das Verhalten einer Tabelle bearbeiten. Diese Methoden sind im Beispielcode wie folgt implementiert wurde:

-   **EditingStyleForRow** – gibt `UITableViewCellEditingStyle.Delete` für die Zeilen mit Daten und gibt `UITableViewCellEditingStyle.Insert` für die letzte Zeile (die speziell für Verhalten sich wie eine Schaltfläche zum Einfügen hinzugefügt werden wird). 
-   **CustomizeMoveTarget** – während der Benutzer eine Zelle der Rückgabewert dieser Methode optional kann ihren bevorzugten Speicherort überschreiben verschoben wird. Dies bedeutet, Sie können verhindern, dass sie "verwerfen" der Zelle an bestimmten Positionen – wie das folgende Beispiel, das verhindert, dass jede Zeile nach dem Verschieben der **(neuen hinzufügen)** Zeile. 
-   **CanMoveRow** – return true, aktivieren Sie die Move "Handle" oder "false", um zu verhindern, dass verschieben. Im Beispiel wurde die letzte Zeile der verschieben "Handle" ausgeblendet werden, da sie als nur eine Schaltfläche zum Einfügen an Server vorgesehen ist. 


Wir belaufen sich auch um zwei benutzerdefinierte Methoden zum Hinzufügen der Zeile 'Insert', und entfernen Sie ihn dann erneut, wenn nicht mehr erforderlich. Sie werden aufgerufen, aus der **bearbeiten** und **Fertig** Schaltflächen:

-   **WillBeginTableEditing** : bei der **bearbeiten** Schaltfläche ist es Aufrufe verwendete `SetEditing` die Tabelle in den Bearbeitungsmodus zu versetzen. Dies bewirkt, dass die WillBeginTableEditing-Methode, in dem wir anzeigen, der **(neuen hinzufügen)** Zeile am Ende der Tabelle als eine Schaltfläche"Einfügen" zu agieren. 
-   **DidFinishTableEditing** – Wenn die Schaltfläche "fertige" berührt wird `SetEditing` erneut aufgerufen, um den Bearbeitungsmodus zu deaktivieren. Der Beispielcode entfernt die **(neuen hinzufügen)** Zeile aus der Tabelle, die beim Bearbeiten ist nicht mehr erforderlich. 


Diese Methode überschreibt, werden in der Beispieldatei implementiert **TableEditModeAdd/Code/TableSource.cs**:

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

Diese beiden benutzerdefinierten Methoden werden zum Hinzufügen und Entfernen der **(neuen hinzufügen)** Zeile die Tabelle Modus Bearbeiten der aktiviert oder deaktiviert wird:

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

Schließlich dieser Code instanziiert die **bearbeiten** und **Fertig** Schaltflächen, mit Lambdas die aktivieren oder deaktivieren Bearbeitungsmodus wechseln, wenn sie berührt sind:

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

Diese Zeile einfügen UI-Muster wird nicht sehr häufig verwendet, aber Sie können auch verwenden die `UITableView.BeginUpdates` und `EndUpdates` Methoden, um das Einfügen oder Entfernen von Zellen in einer beliebigen Tabelle dem animiert werden soll. Die Regel für die Verwendung dieser Methoden ist, dass der Unterschied im Wert von zurückgegeben `RowsInSection` zwischen der `BeginUpdates` und `EndUpdates` Aufrufe müssen die Anzahl der Zellen mit hinzugefügt bzw. gelöscht entsprechen den `InsertRows` und `DeleteRows` Methoden. Wenn die zugrunde liegenden Datenquelle geändert wird nicht entsprechend der Einfügevorgängen/Löschvorgängen in der Tabellenansicht tritt ein Fehler auf.


## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
