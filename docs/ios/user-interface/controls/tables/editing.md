---
title: Bearbeiten von Tabellen mit Xamarin.iOS
description: Dieses Dokument beschreibt, wie Tabellen in Xamarin.iOS bearbeitet wird. Es wird erläutert, Streifen zu löschen, bearbeiten, Modus und Zeile einfügen.
ms.prod: xamarin
ms.assetid: EC197F25-E865-AFA3-E5CF-B33FAB7744A0
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 1267de341a88130c18254f414d2fbb1c42595a0c
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50104810"
---
# <a name="editing-tables-with-xamarinios"></a>Bearbeiten von Tabellen mit Xamarin.iOS

Tabelle-Bearbeitungsfeatures aktiviert sind, durch das Überschreiben von Methoden in einer `UITableViewSource` Unterklasse. Das einfachste Bearbeitungsverhalten ist die wischgesten-zu-Delete-Aktion, die mit einer Außerkraftsetzung einer einzelnen Methode implementiert werden kann.
Komplexere bearbeiten (einschließlich verschieben Zeilen) kann mit der Tabelle im Bearbeitungsmodus erfolgen.

## <a name="swipe-to-delete"></a>Wischen löschen

Streifen So löschen Sie die Funktion ist eine natürliche Gesten in iOS, die Benutzer erwarten, dass an. 

 [![](editing-images/image10.png "Beispiel für Wischen löschen")](editing-images/image10.png#lightbox)

Es gibt drei Außerkraftsetzen von Methoden, die beeinflussen die wischgeste zum Anzeigen einer **löschen** Schaltfläche in einer Zelle:

-   **CommitEditingStyle** – die Tabellenquelle erkennt, wenn diese Methode außer Kraft gesetzt und wird automatisch, die wischgesten-zu-Delete-Aktion aktiviert. Sollten die Implementierung der Methode aufrufen, `DeleteRows` auf die `UITableView` , dazu führen, dass die Zellen ausgeblendet, und Entfernen auch die zugrunde liegenden Daten aus dem Modell (z. B. ein Array, Wörterbuch oder Datenbank). 
-   **CanEditRow** – Wenn CommitEditingStyle überschrieben wird, sind davon ausgegangen, dass alle Zeilen, die bearbeitet werden. Wenn diese Methode wird implementiert, und "false" (für einige bestimmten Zeilen oder für alle Zeilen gibt) wird dann die wischgesten-zu-Delete-Aktion in dieser Zelle nicht verfügbar. 
-   **TitleForDeleteConfirmation** – optional gibt den Text für die **löschen** Schaltfläche. Wenn diese Methode nicht implementiert, wird der Schaltflächentext wird "Löschen". 


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

In diesem Beispiel die `UITableViewSource` wurde aktualisiert, um Sie verwenden eine `List<TableItem>` (statt ein Array von Zeichenfolgen) wie die Datenquelle, da es unterstützt hinzufügen und Löschen von Elementen aus der Auflistung.


## <a name="edit-mode"></a>Bearbeitungsmodus

Wenn eine Tabelle im Bearbeitungsmodus befindet wird der Benutzer ein Rot 'Beenden' Widget für jede Zeile, eine Schaltfläche "löschen", wenn bearbeitete angezeigt. Die Tabelle enthält auch ein "Handle"-Symbol, um anzugeben, dass die Zeile gezogen werden kann, um die Reihenfolge ändern.
Die **TableEditMode** Beispiel implementiert diese Features wie dargestellt.

 [![](editing-images/image11.png "Das Beispiel TableEditMode implementiert diese Features wie dargestellt")](editing-images/image11.png#lightbox)

Es gibt eine Reihe von verschiedenen Methoden auf `UITableViewSource` , von denen beeinflusst das Verhalten einer Tabelle bearbeiten:

-   **CanEditRow** : Legt fest, ob jede Zeile bearbeitet werden kann. Zurückgeben Sie sowohl Streifen zu löschen und Löschen im Bearbeitungsmodus zu verhindern, dass "false". 
-   **CanMoveRow** : der Wert true zurückgeben der Verschiebepunkt' "oder" false ", um zu verhindern, dass das Verschieben zu aktivieren. 
-   **EditingStyleForRow** – Wenn die Tabelle im Bearbeitungsmodus befindet, ist der Rückgabewert dieser Methode bestimmt, ob die Zelle das Symbol Rot löschen zeigt oder das grüne Symbol "hinzufügen". Zurückgeben `UITableViewCellEditingStyle.None` , wenn die Zeile nicht bearbeitet werden soll. 
-   **Gemachten MoveRow** : wird aufgerufen, wenn eine Zeile verschoben wird, damit die zugrunde liegende Datenstruktur geändert werden kann, um die Daten übereinstimmen, wie er in der Tabelle angezeigt wird. 


Die Implementierung für die ersten drei Methoden ist relativ einfach – es sei denn, Sie verwenden möchten die `indexPath` um das Verhalten von bestimmten Zeilen zu ändern, einfach hartcodiert die Rückgabe Werte, für die gesamte Tabelle.

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

Die `MoveRow` Implementierung ist ein wenig komplizierter, da sie die zugrunde liegende Datenstruktur entsprechend die neue Reihenfolge zu ändern muss. Da die Daten als implementiert, werden eine `List` der folgende Code löscht das Datenelement am alten Speicherort, und fügt es am neuen Speicherort ein. Wenn die Daten (z. B.) in einer SQLite-Datenbanktabelle mit einer 'Order'-Spalte gespeichert wurde, müssen stattdessen diese Methode, um die Zahlen in der Spalte neu anzuordnen. einige SQL-Vorgänge auszuführen.

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

und nach Abschluss des Benutzers bearbeiten, die **Fertig** Schaltfläche Bearbeitungsmodus deaktivieren sollten:

```csharp
table.SetEditing (false, true);
```


## <a name="row-insertion-editing-style"></a>Zeilenstil einfügen bearbeiten

Zeile einfügen aus, in der Tabelle ist eine ungewöhnliche Benutzeroberfläche: das wichtigste Beispiel in der standard-iOS-apps die **Kontakt bearbeiten** Bildschirm. Dieser Screenshot zeigt, wie die Zeile einfügen-Richtlinie funktioniert – in Bearbeitung-Modus eine zusätzliche Zeile, die ist (wenn es sich bei Klick-Aktion), zusätzliche Zeilen in die Daten eingefügt. Wenn die Bearbeitung abgeschlossen ist, wird die temporäre **(neu hinzufügen)** Zeile entfernt.

 [![](editing-images/image12.png "Wenn Bearbeitung abgeschlossen ist, wird die temporäre neue hinzufügen Zeile wird entfernt.")](editing-images/image12.png#lightbox)

Es gibt eine Reihe von verschiedenen Methoden auf `UITableViewSource` , von denen beeinflusst das Verhalten einer Tabelle bearbeiten. Diese Methoden wurden im Beispielcode wie folgt implementiert:

-   **EditingStyleForRow** – gibt `UITableViewCellEditingStyle.Delete` für die Zeilen mit Daten und gibt `UITableViewCellEditingStyle.Insert` für die letzte Zeile (die speziell für die wie eine Einfügeschaltfläche Verhalten hinzugefügt werden wird). 
-   **CustomizeMoveTarget** – während der Benutzer eine Zelle der Rückgabewert dieser Methode mit optionalen kann außer Kraft setzen die Auswahl des Speicherorts für verschoben wird. Dies bedeutet, Sie können verhindern, dass sie "verwerfen" der Zelle an bestimmten Positionen beibehält – wie in diesem Beispiel, das verhindert, dass jede Zeile nach dem Verschieben der **(neu hinzufügen)** Zeile. 
-   **CanMoveRow** : der Wert true zurückgeben der Verschiebepunkt' "oder" false ", um zu verhindern, dass das Verschieben zu aktivieren. Im Beispiel hat die letzte Zeile den "Verschiebepunkt" ausgeblendet, da dies zu Server nur eine Einfügeschaltfläche soll. 


Wir fügen auch zwei benutzerdefinierte Methoden zum Hinzufügen der Zeile 'Insert', und entfernen sie Sie dann noch Mal, wenn nicht mehr erforderlich. Heißen sie aus der **bearbeiten** und **Fertig** Schaltflächen:

-   **WillBeginTableEditing** : bei der **bearbeiten** Schaltfläche verwendete es Aufrufe `SetEditing` in der Tabelle in den Bearbeitungsmodus zu versetzen. Dies löst die WillBeginTableEditing-Methode, die angezeigt, in dem die **(neu hinzufügen)** Zeile am Ende der Tabelle, die als eine Schaltfläche"Einfügen" fungiert. 
-   **DidFinishTableEditing** : Wenn Sie die Schaltfläche "Fertig" livekomponententests wird `SetEditing` erneut aufgerufen, um den Bearbeitungsmodus zu deaktivieren. Der Beispielcode entfernt die **(neu hinzufügen)** Zeile aus der Tabelle, die beim Bearbeiten ist nicht mehr erforderlich. 


Diese Methode Außerkraftsetzungen werden in der Beispieldatei implementiert **TableEditModeAdd/Code/TableSource.cs**:

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

Diese beiden benutzerdefinierten Methoden werden verwendet, um das Hinzufügen und Entfernen der **(neu hinzufügen)** Zeile, wenn Sie in der Tabelle Modus Bearbeiten der aktiviert oder deaktiviert ist:

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

Zum Schluss dieser Code instanziiert die **bearbeiten** und **Fertig** Schaltflächen mit Lambda-Ausdrücken, die aktivieren oder deaktivieren Bearbeitungsmodus wechseln, wenn sie betroffen sind:

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

Diese Zeile einfügen-UI-Muster wird nicht sehr oft verwendet, aber Sie können auch verwenden die `UITableView.BeginUpdates` und `EndUpdates` Methoden, um das Einfügen oder Entfernen von Zellen in einer Tabelle zu animieren. Die Regel für die Verwendung dieser Methoden ist, dass der Unterschied im Wert von zurückgegeben `RowsInSection` zwischen der `BeginUpdates` und `EndUpdates` Aufrufe müssen die Anzahl von Zellen, die hinzugefügt oder gelöscht sind, mit entsprechen den `InsertRows` und `DeleteRows` Methoden. Wenn die zugrunde liegenden Datenquelle geändert wird, entsprechend der einfügungen/löschungen in der Tabellenansicht tritt ein Fehler auf.


## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
