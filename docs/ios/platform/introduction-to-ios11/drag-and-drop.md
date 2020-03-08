---
title: Drag & Drop in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie Drag & Drop in xamarin. IOS-Apps mithilfe der in ios 11 eingeführten APIs implementieren. Insbesondere wird das Aktivieren von Drag & Drop in "uitableview" erläutert.
ms.prod: xamarin
ms.assetid: 0D39C4C3-D169-42F8-B3FA-7F98CF0B6F1F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 09/05/2017
ms.openlocfilehash: 928936815c89dd74d0ad3775f59ea210702c8857
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2020
ms.locfileid: "78915740"
---
# <a name="drag-and-drop-in-xamarinios"></a>Drag & Drop in xamarin. IOS

_Implementieren von Drag & Drop für IOS 11_

IOS 11 umfasst Drag & Drop-Unterstützung zum Kopieren von Daten zwischen Anwendungen auf dem iPad. Benutzer können alle Inhaltstypen von apps auswählen und ziehen, die nebeneinander positioniert sind, oder indem Sie ein App-Symbol ziehen, das auslöst, dass die APP geöffnet wird und die Daten gelöscht werden können:

![Drag & Drop-Beispiel von benutzerdefinierter app in Notes-App](drag-and-drop-images/drag-drop-sml.png)

> [!NOTE]
> Drag & Drop ist nur innerhalb derselben App auf dem iPhone verfügbar.

Ziehen Sie in Erwägung, Drag & Drop-Vorgänge zu unterstützen, wo Inhalte erstellt oder bearbeitet werden

- Text Steuerelemente unterstützen Drag & Drop für alle apps, die mit IOS 11 erstellt wurden, ohne zusätzliche Arbeit.
- Tabellen Sichten und Auflistungs Sichten enthalten Erweiterungen in ios 11, die das Hinzufügen von Drag & Drop-Verhalten vereinfachen.
- Jede andere Ansicht kann zur Unterstützung von Drag & Drop mit zusätzlicher Anpassung erstellt werden.

Wenn Sie Drag & Drop-Unterstützung für Ihre apps hinzufügen, können Sie unterschiedliche Ebenen der Inhalts Treue bereitstellen. Beispielsweise können Sie sowohl einen formatierten Text als auch eine nur-Text-Version der Daten bereitstellen, damit die empfangende App auswählen kann, was am besten in das Zieh Ziel passt. Es ist auch möglich, die Zieh Visualisierung anzupassen und gleichzeitig mehrere Elemente gleichzeitig zu aktivieren.

## <a name="drag-and-drop-with-text-controls"></a>Drag & Drop mit Text Steuerelementen

`UITextView` und `UITextField` unterstützen automatisch das Ziehen von ausgewähltem Text und das Ablegen von Text Inhalt in.

<a name="uitableview" />

## <a name="drag-and-drop-with-uitableview"></a>Drag & Drop mit uitableview

`UITableView` verfügt über eine integrierte Behandlung von Drag & Drop-Interaktionen mit Tabellenzeilen, die nur wenige Methoden zum Aktivieren des Standard Verhaltens erfordert.

Es sind zwei Schnittstellen beteiligt:

- `IUITableViewDragDelegate` – Paketinformationen, wenn ein Drag-Vorgang in der Tabellenansicht initiiert wird.
- `IUITableViewDropDelegate` – verarbeitet Informationen, wenn ein Löschvorgang versucht und abgeschlossen wird.

Im [draganddroptableview-Beispiel](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview) werden diese beiden Schnittstellen zusammen mit dem Delegaten und der Datenquelle auf der `UITableViewController`-Klasse implementiert. Sie werden in der `ViewDidLoad`-Methode zugewiesen:

```csharp
this.TableView.DragDelegate = this;
this.TableView.DropDelegate = this;
```

Der minimal erforderliche Code für diese beiden Schnittstellen wird weiter unten erläutert.

### <a name="table-view-drag-delegate"></a>Tabellenansicht, Drag-Delegat

Die einzige Methode, die zum unterstützen der Durchführung einer Zeile aus einer Tabellenansicht _erforderlich_ ist, ist `GetItemsForBeginningDragSession`. Wenn der Benutzer mit dem Ziehen einer Zeile beginnt, wird diese Methode aufgerufen.

Eine-Implementierung ist unten dargestellt. Er ruft die der gezogenen Zeile zugeordneten Daten ab, codiert Sie und konfiguriert eine `NSItemProvider`, die bestimmt, wie Anwendungen den "Drop"-Teil des Vorgangs behandeln (z. b. ob Sie den Datentyp `PlainText`im Beispiel verarbeiten können):

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

Es gibt viele optionale Methoden für den Drag-Delegaten, die implementiert werden können, um das Zieh Verhalten anzupassen, z. b. das Bereitstellen mehrerer Daten Darstellungen, die in Ziel-apps genutzt werden können (z. b. formatierten Text und nur-Text oder ein Vektor und). Bitmap-Versionen einer Zeichnung). Sie können auch benutzerdefinierte Daten Darstellungen bereitstellen, die beim Ziehen und ablegen innerhalb derselben App verwendet werden.

### <a name="table-view-drop-delegate"></a>Tabellen Sicht-Drop-Delegat

Die Methoden auf dem Drop-Delegaten werden aufgerufen, wenn ein Zieh Vorgang über eine Tabellen Sicht erfolgt oder über diesem Vorgang abgeschlossen wird. Die erforderlichen Methoden bestimmen, ob die Daten gelöscht werden dürfen und welche Aktionen durchgeführt werden, wenn der Löschvorgang abgeschlossen ist:

- `CanHandleDropSession` –, während ein Drag-Vorgang ausgeführt wird und möglicherweise in der Anwendung gelöscht wird, bestimmt diese Methode, ob die gezogenen Daten gelöscht werden können.
- `DropSessionDidUpdate` –, während der Zieh Vorgang ausgeführt wird, wird diese Methode aufgerufen, um zu bestimmen, welche Aktion beabsichtigt ist. Informationen aus der über gezogenen Tabellenansicht, der Zieh Sitzung und dem möglichen Indexpfad können verwendet werden, um das Verhalten und das visuelle Feedback zu ermitteln, die dem Benutzer bereitgestellt werden.
- `PerformDrop` – wenn der Benutzer den Löschvorgang abschließt (durch das Heben des Fingers), extrahiert diese Methode die gezogenen Daten und ändert die Tabellenansicht, um die Daten in einer neuen Zeile (oder Zeilen) hinzuzufügen.

#### <a name="canhandledropsession"></a>CanHandleDropSession

`CanHandleDropSession` gibt an, ob die Tabellen Sicht die gezogenen Daten akzeptieren kann. In diesem Code Ausschnitt wird `CanLoadObjects` verwendet, um zu bestätigen, dass diese Tabellenansicht Zeichen folgen Daten akzeptieren kann.

```csharp
public bool CanHandleDropSession(UITableView tableView, IUIDropSession session)
{
  return session.CanLoadObjects(typeof(NSString));
}
```

#### <a name="dropsessiondidupdate"></a>DropSessionDidUpdate

Die `DropSessionDidUpdate`-Methode wird wiederholt aufgerufen, während der Zieh Vorgang ausgeführt wird, um dem Benutzer visuelle Hinweise bereitzustellen.

Im folgenden Code wird `HasActiveDrag` verwendet, um zu bestimmen, ob der Vorgang aus der aktuellen Tabellenansicht stammt. Wenn dies der Fall ist, dürfen nur einzelne Zeilen verschoben werden.
Wenn der Zieh Vorgang aus einer anderen Quelle erfolgt, wird ein Kopiervorgang angezeigt:

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

Der Drop-Vorgang kann eine der `Cancel`, `Move`oder `Copy`sein.

Die Lösch Absicht kann das Einfügen einer neuen Zeile oder das Hinzufügen/Anfügen von Daten zu einer vorhandenen Zeile sein.

#### <a name="performdrop"></a>Performdrop

Die `PerformDrop`-Methode wird aufgerufen, wenn der Benutzer den Vorgang abschließt, und ändert die Tabellenansicht und die Datenquelle, um die gelöschten Daten widerzuspiegeln.

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

### <a name="testing-drag-and-drop"></a>Testen von Drag & amp; Drop

[Zum Testen des Beispiels](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview)müssen Sie ein iPad verwenden.
Öffnen Sie das Beispiel neben einer anderen APP (z. b. Notizen), und ziehen Sie Zeilen und Text zwischen diesen:

![Screenshot des Zieh Vorgangs in Bearbeitung](drag-and-drop-images/01-sml.png)

## <a name="related-links"></a>Verwandte Links

- [Richtlinien für die Benutzeroberfläche per Drag & Drop (Apple)](https://developer.apple.com/ios/human-interface-guidelines/interaction/drag-and-drop/)
- [Beispiel für eine Drag & Drop-Tabellenansicht](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddroptableview)
- [Beispiel für eine Drag & Drop-Sammlungsansicht](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-draganddropcollectionview)
- [Einführung in Drag & Drop (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/203/)
- [Ziehen und Ablegen mit der Sammlungs-und Tabellenansicht (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/223/)
