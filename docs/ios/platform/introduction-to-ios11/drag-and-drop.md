---
title: Drag & Drop
description: Implementieren von Drag & Drop für iOS 11
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 09/05/2016
ms.openlocfilehash: fa6fcb2c4f5f17011307b31e4644889d066b71a9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="drag-and-drop"></a>Drag & Drop

_Implementieren von Drag & Drop für iOS 11_

iOS 11 enthält Drag und drop-Unterstützung zum Kopieren von Daten zwischen Anwendungen auf dem iPad. Benutzer können auswählen, und ziehen Sie alle Typen von Inhalten von positioniert apps-Seite-an-Seite oder durch Ziehen über eine app das Symbol für die app zu öffnen und die Daten zu löschenden ermöglichen ausgelöst wird:

![Drag & Drop-Beispiel benutzerdefinierte App in den Anmerkungen zu dieser app](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Drag & Drop ist nur innerhalb der gleichen app auf dem iPhone verfügbar.

Betrachten Sie die Unterstützung von Drag und drop-Operationen an einer beliebigen Stelle Inhalt erstellt oder bearbeitet werden kann:

- Textsteuerelemente unterstützen die Drag & Drop für alle apps für iOS 11, ohne zusätzliche Aufgaben erstellt.
- Tabellenansichten und Auflistungsansichten enthalten Erweiterungen iOS 11, die Vereinfachung Hinzufügen von Drag und drop-Verhalten.
- Jede andere Sicht kann zur Unterstützung von Drag und drop mit zusätzlichen Anpassung vorgenommen werden.

Beim Hinzufügen von Drag & Drop auf Ihre apps unterstützen, können Sie verschiedene Ebenen der Inhalt Genauigkeit angeben; Sie können z. B. einen formatierten Text und die nur-Text-Version der Daten bereitstellen, damit die empfangende Anwendung kann die in das Ziel ziehen Sie am besten erfüllt. Es ist auch möglich, zum Anpassen der Visualisierung ziehen Sie und ziehen mehrere Elemente gleichzeitig aktivieren.

## <a name="drag-and-drop-with-text-controls"></a>Drag & Drop mit Textsteuerelemente

`UITextView` und `UITextField` out markierten Text ziehen und Drop von Text, die den Inhalt automatisch zu unterstützen.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Drag & Drop mit UITableView

`UITableView` bietet eine integrierte Behandlung für Drag & drop von Interaktionen mit Zeilen der Tabelle erfordern nur wenige Methoden, um das Standardverhalten zu aktivieren.

Es gibt zwei Schnittstellen beteiligten:

- `IUITableViewDragDelegate` – Pakete Informationen, wenn eine ziehen Sie in der Tabellenansicht initiiert wird.
- `IUITableViewDropDelegate` – Informationen verarbeitet, wenn ein solches Löschen wird versucht, und abgeschlossen.

In der [DragAndDropTableView Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) diese beiden Benutzeroberflächen werden sowohl auf implementiert die `UITableViewController` -Klasse, zusammen mit dem Delegaten und der Datenquelle. Sie zugewiesen sind die `ViewDidLoad` Methode:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Die minimale erforderliche Code für diese beiden Benutzeroberflächen wird im folgenden erläutert.

### <a name="table-view-drag-delegate"></a>Tabelle anzeigen, ziehen Sie Delegaten

Die einzige Methode _erforderlichen_ ist, ziehen eine Zeile aus einer Tabellenansicht unterstützen `GetItemsForBeginningDragSession`. Wenn der Benutzer beginnt, eine Zeile zu ziehen, wird diese Methode aufgerufen werden.

Eine Implementierung ist unten dargestellt. Ruft die Daten, die die gezogene Zeile zugeordnet, konfiguriert und codiert ihn ein `NSItemProvider` die bestimmt, wie Clientanwendungen die "Drop" Teil des Vorgangs verarbeiten soll (z. B., ob sie den Datentyp verarbeiten können `PlainText`, im Beispiel):

```csharp
public UIDragItem[] GetItemsForBeginningDragSession (UITableView tableView,
  IUIDragSession session, NSIndexPath indexPath)
{
  // gets the 'information' to be dragged
  var placeName = model.PlaceNames[indexPath.Row];
  // convert to NSData representation
  var data = NSData.FromString(placeName, NSStringEncoding.UTF8);
  // create an NSItemProvider to describe the data
  var itemProvider = new NSItemProvider();
  itemProvider.RegisterDataRepresentation(UTType.PlainText,
                                NSItemProviderRepresentationVisibility.All,
                                (completion) =>
  {
    completion(data, null);
    return null;
  });
  // wrap in a UIDragItem
  return new UIDragItem[] { new UIDragItem(itemProvider) };
}
```

Es gibt viele optionale Methoden für die Drag-Delegat, der implementiert werden kann, zum Anpassen der Drag-Verhalten, z. B. Bereitstellung mehrere Darstellungen der Daten der im Ziel-apps genutzt werden können (z. B. formatierte Text als auch als nur-Text oder einen Vektor und Bitmap Versionen einer Zeichnung). Sie können auch benutzerdefinierte Daten Darstellungen verwendet werden, wenn Drag & Drop innerhalb der gleichen app bereitstellen.

### <a name="table-view-drop-delegate"></a>Tabelle, Sicht Drop-Delegat

Die Methoden für die Drop-Delegaten werden aufgerufen, wenn ein Ziehvorgang über eine Tabellensicht erfolgt oder darüber liegenden abgeschlossen ist. Die erforderlichen Methoden ermitteln, ob die Daten gelöscht werden darf und welche Aktionen ausgeführt werden, wenn Sie der Löschvorgang abgeschlossen ist:

- `CanHandleDropSession` – Während ein Drag & läuft, und für die Anwendung möglicherweise gelöscht werden, diese Methode bestimmt, ob die gezogenen Daten zulässig ist, gelöscht werden sollen.
- `DropSessionDidUpdate` – Während der Ziehvorgang ausgeführt wird, wird diese Methode aufgerufen, um zu bestimmen, welche Aktion vorgesehen ist. Informationen aus der Tabellenansicht gezogen wird, Drag & Sitzung und den Pfad der Index ist möglicherweise alle dient zur Ermittlung des Verhalten und visuelles Feedback für den Benutzer bereitgestellten.
- `PerformDrop` – Wenn der Benutzer den Löschvorgang abgeschlossen ist (nach Aufhebung ihrer Finger), wird diese Methode die gezogenen Daten extrahiert, und ändert die Tabellenansicht, um die Daten in eine neue Zeile (oder Zeilen) hinzuzufügen.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Gibt an, ob die Tabellenansicht die gezogenen Daten annehmen kann. In diesem Codeausschnitt `CanLoadObjects` wird verwendet, um sicherzustellen, dass diese Tabellenansicht Zeichenfolgendaten annehmen kann.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

Die `DropSessionDidUpdate` -Methode wird wiederholt aufgerufen, während des Ziehvorgangs ausgeführt wird, um visuelle Hinweise für den Benutzer bereitgestellt wird.

Im folgenden Code `HasActiveDrag` wird verwendet, um zu bestimmen, ob der Vorgang in der Tabellenansicht der aktuellen stammt. Wenn dies der Fall ist, dürfen nur einzelne Zeilen verschoben werden.
Wenn das Ziehen von einer anderen Quelle stammt, wird ein Vorgang zum Kopieren angegeben:

```csharp
public UITableViewDropProposal DropSessionDidUpdate(UITableView tableView, IUIDropSession session, NSIndexPath destinationIndexPath)
{
  // The UIDropOperation.Move operation is available only for dragging within a single app.
  if (tableView.HasActiveDrag)
  {
    if (session.Items.Length > 1)
    {
        return new UITableViewDropProposal(UIDropOperation.Cancel);
    } else {
        return new UITableViewDropProposal(UIDropOperation.Move, UITableViewDropIntent.InsertAtDestinationIndexPath);
    }
  } else {
    return new UITableViewDropProposal(UIDropOperation.Copy, UITableViewDropIntent.InsertAtDestinationIndexPath);
  }
}
```

Der Drop-Vorgang kann eine der `Cancel`, `Move`, oder `Copy`.

Die Drop Absicht kann sein, um eine neue Zeile einzufügen oder zu hinzufügen/Anfügen von Daten in eine vorhandene Zeile.

#### <a name="performdrop"></a>PerformDrop

Die `PerformDrop` Methode wird aufgerufen, wenn der Benutzer schließt den Vorgang ab und die Tabelle anzeigen und der Datenquelle ändert um abgelegten Daten widerzuspiegeln.

```csharp
public void PerformDrop(UITableView tableView, IUITableViewDropCoordinator coordinator)
{
  NSIndexPath indexPath, destinationIndexPath;
  if (coordinator.DestinationIndexPath != null)
  {
    indexPath = coordinator.DestinationIndexPath;
    destinationIndexPath = indexPath;
  }
  else
  {
    // Get last index path of table view
    var section = tableView.NumberOfSections() - 1;
    var row = tableView.NumberOfRowsInSection(section);
    destinationIndexPath = NSIndexPath.FromRowSection(row, section);
  }
  coordinator.Session.LoadObjects(typeof(NSString), (items) =>
  {
    // Consume drag items
    List<string> stringItems = new List<string>();
    foreach (var i in items)
    {
      var q = NSString.FromHandle(i.Handle);
      stringItems.Add(q.ToString());
    }
    var indexPaths = new List<NSIndexPath>();
    for (var j = 0; j < stringItems.Count; j++)
    {
      var indexPath1 = NSIndexPath.FromRowSection(destinationIndexPath.Row + j, destinationIndexPath.Section);
      model.AddItem(stringItems[j], indexPath1.Row);
      indexPaths.Add(indexPath1);
    }
    tableView.InsertRows(indexPaths.ToArray(), UITableViewRowAnimation.Automatic);
  });
}
```

Kein zusätzlicher Code kann zum asynchronen Laden von großen Objekten hinzugefügt werden.

### <a name="testing-drag-and-drop"></a>Testen Drag & Drop

Sie müssen einem iPad zum Testen verwenden die [Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Öffnen Sie das Beispiel zusammen mit einer anderen app (z. B. Hinweise), und ziehen Sie Zeilen und Text, um zwischen ihnen:

![Screenshot des Ziehvorgangs wird ausgeführt](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Verwandte Links

- [Drag & Drop Human Interface-Richtlinien (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Drag & Drop-Tabelle-Viewer-Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Drag & Drop-Auflistung Viewer-Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Einführung in die Drag & Drop (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Drag & Drop-Auflistung mit Tabelle anzeigen (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/223/)
