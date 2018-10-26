---
title: Drag & Drop in Xamarin.iOS
description: Dieses Dokument beschreibt das Implementieren von Drag und drop in Xamarin.iOS-apps unter Verwendung der APIs in iOS 11 eingeführte. Insbesondere erläutert Aktivieren der Drag & drop in UITableView.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/05/2017
ms.openlocfilehash: aa93e015a399e733a2bb52f087a1e482bc23a00a
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50112077"
---
# <a name="drag-and-drop-in-xamarinios"></a>Drag & Drop in Xamarin.iOS

_Implementieren von Drag & Drop für iOS 11_

iOS 11 enthält Drag und drop-Unterstützung zum Kopieren von Daten zwischen Anwendungen auf dem iPad. Benutzer können auswählen, und ziehen alle Arten von Inhalten aus apps positioniert-Seite-an-Seite oder durch Ziehen über app-Symbol, die die app zu öffnen und damit die Daten gelöscht werden können auslösen wird:

![Drag & Drop-Beispiel aus der benutzerdefinierten app in den Anmerkungen zu dieser app](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Drag & Drop ist nur innerhalb derselben app auf einem iPhone verfügbar.

Erwägen Sie die Unterstützung von Drag und Drop-Vorgänge, die an einer beliebigen Stelle Inhalt erstellt oder bearbeitet werden können:

- Textsteuerelemente unterstützen Drag & Drop für alle apps für iOS 11, ohne zusätzlichen Aufwand erstellt.
- Tabellenansichten und Auflistungsansichten enthalten Erweiterungen in iOS 11, die vereinfachen Hinzufügen von Drag und drop-Vorgang aus.
- Jeder anderen Ansicht kann vorgenommen werden, zu unterstützen der Drag & drop mit zusätzliche Anpassungen vorzunehmen.

Beim Hinzufügen von Drag & Drop auf Ihre apps zu unterstützen, können Sie unterschiedliche Inhalte Genauigkeit angeben; Sie können z. B. einen formatierten Text und die nur-Text-Version der Daten bereitstellen, damit die empfangende app auswählen kann, die in das Ziel ziehen Sie am besten passt. Es ist auch möglich, zum Anpassen der Visualisierung ziehen Sie und ziehen mehrere Elemente gleichzeitig aktivieren.

## <a name="drag-and-drop-with-text-controls"></a>Drag & Drop Textsteuerelementen

`UITextView` und `UITextField` markierten Text, ziehen und Ablegen von Textinhalt in automatisch zu unterstützen.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Drag & Drop mit UITableView

`UITableView` bietet eine integrierte Behandlung für Drag & drop-Interaktionen mit den Tabellenzeilen, erfordern nur ein paar Methoden, um das Standardverhalten zu aktivieren.

Es gibt zwei Schnittstellen beteiligt:

- `IUITableViewDragDelegate` – Pakete Informationen, wenn ein Ziehvorgang in der Tabellenansicht initiiert wird.
- `IUITableViewDropDelegate` – Informationen verarbeitet, wenn ein Ablegen wird versucht, und abgeschlossen.

In der [DragAndDropTableView Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/) diese beiden Schnittstellen werden sowohl auf implementiert die `UITableViewController` -Klasse, zusammen mit dem Delegaten und der Datenquelle. Sie zugewiesen sind die `ViewDidLoad` Methode:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Der minimale erforderlichen Code für diese beiden Schnittstellen wird unten erläutert.

### <a name="table-view-drag-delegate"></a>Tabelle anzeigen Drag Delegat

Die einzige Methode _erforderlichen_ zu unterstützen, ziehen eine Zeile aus einer Tabelle fungiert `GetItemsForBeginningDragSession`. Wenn der Benutzer beginnt, eine Zeile zu ziehen, wird diese Methode aufgerufen werden.

Eine Implementierung ist unten dargestellt. Ruft ab, die Daten, die gezogenen Zeile zugeordnet ist, codiert und konfiguriert eine `NSItemProvider` die bestimmt, wie Anwendungen die "Drop" Teil des Vorgangs verarbeiten soll (z. B., ob sie den Datentyp behandeln können `PlainText`, im Beispiel):

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

Es gibt viele optionale Methoden auf der Drag Delegat, der implementiert werden kann, um das Anpassen des Verhaltens ziehen Sie, wie das Bereitstellen von mehreren datendarstellungen dem Ziel-Apps genutzt werden können (z. B. den formatierten Text als auch als nur-Text oder einen Vektor und Bitmap-Versionen einer Zeichnung). Sie können auch benutzerdefinierte datendarstellungen verwendet werden, wenn Drag & Drop innerhalb derselben app bereitstellen.

### <a name="table-view-drop-delegate"></a>Tabelle, Sicht Drop-Delegat

Die Methoden für die Drop-Delegaten werden immer dann aufgerufen, wenn ein Ziehvorgang über eine Tabellenansicht erfolgt oder darüber abgeschlossen ist. Die erforderlichen Methoden ermitteln, ob die Daten gelöscht werden darf und welche Aktionen ausgeführt werden, wenn der Drop ausgeführt wird:

- `CanHandleDropSession` – Während ein Ziehvorgang ausgeführt wird, und für die Anwendung möglicherweise gelöscht wird, die diese Methode bestimmt, ob die gezogenen Daten gelöscht werden darf.
- `DropSessionDidUpdate` – Während der Ziehvorgang ausgeführt wird, wird diese Methode aufgerufen, um zu bestimmen, welche Aktion soll. Informationen aus der Tabellenansicht gezogen wird, über die Drag-Sitzung und der Index ist möglicherweise Pfad können alle verwendet werden um das Verhalten und das visuelle Feedback, die dem Benutzer bereitgestellt, zu bestimmen.
- `PerformDrop` – Wenn der Benutzer die Dropdownliste (durch ihren Finger abheben) abgeschlossen ist, wird diese Methode die gezogenen Daten extrahiert und ändert die Tabellenansicht, um die Daten in eine neue Zeile (oder Zeilen) hinzuzufügen.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` Gibt an, ob die Tabellenansicht die gezogenen Daten akzeptieren kann. In diesem Codeausschnitt `CanLoadObjects` wird verwendet, um sicherzustellen, dass diese Tabellenansicht Zeichenfolgendaten annehmen kann.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

Die `DropSessionDidUpdate` Methode wird wiederholt aufgerufen, während des Ziehvorgangs ausgeführt wird, zu dem Benutzer visuelle Hinweise bereitgestellt wird.

Im folgenden Code `HasActiveDrag` wird verwendet, um zu bestimmen, ob der Vorgang in der Tabellenansicht der aktuellen stammt. Wenn dies der Fall ist, werden nur einzelne Zeilen verschoben werden darf.
Wenn für der Ziehvorgang aus einer anderen Quelle ist, wird ein Kopiervorgang angegeben:

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

Das Drop-Ziel kann sein, um eine neue Zeile einzufügen oder zu Daten in eine vorhandene Zeile hinzufügen/anfügen.

#### <a name="performdrop"></a>PerformDrop

Die `PerformDrop` Methode wird aufgerufen, wenn der Benutzer schließt den Vorgang ab und die Tabelle anzeigen und der Datenquelle ändert entsprechend die abgelegten Daten.

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

Zusätzlicher Code kann hinzugefügt werden, um große Datenobjekte asynchron zu laden.

### <a name="testing-drag-and-drop"></a>Tests ziehen und ablegen

Sie müssen ein iPad zum Testen verwenden die [Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/).
Öffnen Sie das Beispiel zusammen mit einer anderen app (z. B. Anmerkungen zu dieser Version), und ziehen Sie Zeilen und der Text zwischen ihnen:

![Screenshot des Ziehvorgangs in Bearbeitung](drag-and-drop-images/01-sml.png)


## <a name="related-links"></a>Verwandte Links

- [Drag & Drop Human Interface Guidelines (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Drag & Drop Table-Viewer-Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropTableView/)
- [Drag & Drop-Auflistung-Viewer-Beispiel](https://developer.xamarin.com/samples/monotouch/ios11/DragAndDropCollectionView)
- [Einführung in die Drag & Drop (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Drag & Drop-Sammlung mit Tabelle anzeigen (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/223/)
