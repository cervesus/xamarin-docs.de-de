---
title: Auffüllen einer Tabelle mit Daten in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie eine Tabelle mit Daten in einer xamarin. IOS-Anwendung auffüllen. Dabei werden uitableviewsource, Wiederverwendbarkeit von Zellen, Hinzufügen eines Indexes und Kopf-und Fußzeilen erläutert.
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/22/2017
ms.openlocfilehash: 954273907fea1c57ca9d2aaac5ceb7d81115f553
ms.sourcegitcommit: 57b98c3c818a77cae94749665878d155ad031ff9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/27/2019
ms.locfileid: "75500335"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Auffüllen einer Tabelle mit Daten in xamarin. IOS

Um einer `UITableView` Zeilen hinzuzufügen, müssen Sie eine `UITableViewSource` Unterklasse implementieren und die von der Tabellenansicht aufgerufenen Methoden überschreiben, um sich selbst aufzufüllen.

In dieser Anleitung wird Folgendes behandelt:

- Unterklassen von "uitableviewsource"
- Wiederverwendung von Zellen
- Hinzufügen eines Indexes
- Hinzufügen von Kopf-und Fußzeilen

<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Subclassinguitableviewsource

Jeder `UITableView`wird eine `UITableViewSource`-Unterklasse zugewiesen. Die Tabellen Sicht fragt die Quell Klasse ab, um zu bestimmen, wie Sie sich selbst Rendering (z. b. wie viele Zeilen erforderlich sind, und die Höhe jeder Zeile, wenn Sie sich von der Standardeinstellung unterscheidet). Am wichtigsten ist, dass die Quelle jede Zell Ansicht bereitstellt, die mit Daten gefüllt ist.

Zum Erstellen von Daten in einer Tabelle sind nur zwei obligatorische Methoden erforderlich:

- **Rowsinsection** – gibt eine [`nint`](~/cross-platform/macios/nativetypes.md) Anzahl der Gesamtzahl der Daten Zeilen zurück, die in der Tabelle angezeigt werden sollen.
- **Getcell** – gibt eine `UITableCellView` zurück, die mit Daten für den entsprechenden Zeilen Index aufgefüllt wurde, der an die Methode übermittelt wurde.

Die basierbaren Beispieldatei **TableSource.cs** verfügt über die einfachste mögliche Implementierung von `UITableViewSource`. Im folgenden Code Ausschnitt sehen Sie, dass Sie ein Array von Zeichen folgen akzeptiert, die in der Tabelle angezeigt werden, und einen Standard Zellstil zurückgibt, der jede Zeichenfolge enthält:

```csharp
public class TableSource : UITableViewSource {

        string[] TableItems;
        string CellIdentifier = "TableCell";

        public TableSource (string[] items)
        {
            TableItems = items;
        }

        public override nint RowsInSection (UITableView tableview, nint section)
        {
            return TableItems.Length;
        }

        public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
        {
            UITableViewCell cell = tableView.DequeueReusableCell (CellIdentifier);
            string item = TableItems[indexPath.Row];

            //if there are no cells to reuse, create a new one
            if (cell == null){ 
                cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); 
            }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

Eine `UITableViewSource` kann jede beliebige Datenstruktur verwenden, von einem einfachen Zeichen folgen Array (wie in diesem Beispiel gezeigt) zu einer Liste < > oder einer anderen Auflistung. Durch die Implementierung von `UITableViewSource`-Methoden wird die Tabelle von der zugrunde liegenden Datenstruktur isoliert.

Um diese Unterklasse zu verwenden, erstellen Sie ein Zeichen folgen Array, um die Quelle zu erstellen, und weisen Sie Sie dann einer Instanz von `UITableView`zu:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    table = new UITableView(View.Bounds); // defaults to Plain style
    string[] tableItems = new string[] {"Vegetables","Fruits","Flower Buds","Legumes","Bulbs","Tubers"};
    table.Source = new TableSource(tableItems);
    Add (table);
}
```

Die resultierende Tabelle sieht wie folgt aus:

 [![](populating-a-table-with-data-images/image3.png "Sample table running")](populating-a-table-with-data-images/image3.png#lightbox)

In den meisten Tabellen können Benutzer eine Zeile berühren, um Sie auszuwählen und andere Aktionen auszuführen (z. b. das Abspielen eines Titels oder das Aufrufen eines Kontakts oder das Anzeigen eines anderen Bildschirms). Um dies zu erreichen, müssen wir einige Dinge tun. Erstellen Sie zunächst einen alertcontroller, um eine Meldung anzuzeigen, wenn der Benutzer auf eine Zeile klickt, indem Sie der `RowSelected`-Methode Folgendes hinzufügen:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Erstellen Sie als nächstes eine Instanz des Ansichts Controllers:

```csharp
HomeScreen owner;
```

Fügen Sie Ihrer uitableviewsource-Klasse einen Konstruktor hinzu, der einen Ansichts Controller als Parameter annimmt und ihn in einem Feld speichert:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```

Ändern Sie die viewDidLoad-Methode, in der die uitableviewsource-Klasse erstellt wird, um den `this` Verweis zu übergeben:

```csharp
table.Source = new TableSource(tableItems, this);
```

Zum Schluss müssen Sie in der `RowSelected`-Methode `PresentViewController` im zwischengespeicherten Feld aufzurufen:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```

Nun kann der Benutzer eine Zeile berühren, und es wird eine Warnung angezeigt:

 [![](populating-a-table-with-data-images/image4.png "The row selected alert")](populating-a-table-with-data-images/image4.png#lightbox)

## <a name="cell-reuse"></a>Wiederverwendung von Zellen

In diesem Beispiel gibt es nur sechs Elemente, sodass keine Wiederverwendung von Zellen erforderlich ist. Beim Anzeigen von Hunderten oder Tausenden von Zeilen wäre es jedoch ein Speicher Verschwendung, Hunderte oder Tausende von `UITableViewCell` Objekten zu erstellen, wenn nur ein paar gleichzeitig auf den Bildschirm passt.

Um diese Situation zu vermeiden, wird die Ansicht, wenn eine Zelle auf dem Bildschirm ausgeblendet wird, zur Wiederverwendung in eine Warteschlange eingefügt. Wenn der Benutzer einen Bildlauf ausführt, ruft die Tabelle `GetCell` auf, um neue Sichten zum Anzeigen von – anzufordern, um eine vorhandene Zelle wiederzuverwenden (die derzeit nicht angezeigt wird), rufen Sie einfach die `DequeueReusableCell`-Methode auf. Wenn eine Zelle für die Wiederverwendung verfügbar ist, wird Sie zurückgegeben; andernfalls wird ein NULL-Wert zurückgegeben, und der Code muss eine neue Zellen Instanz erstellen.

Dieser Code Ausschnitt aus dem Beispiel veranschaulicht das Muster:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

Der `cellIdentifier` erstellt effektiv separate Warteschlangen für verschiedene Arten von Zellen. In diesem Beispiel sehen alle Zellen so aus, dass nur ein hart codierter Bezeichner verwendet wird. Wenn verschiedene Arten von Zellen vorhanden sind, sollten Sie jeweils eine andere Bezeichnerzeichenfolge haben, sowohl wenn Sie instanziiert werden, als auch, wenn Sie von der Warteschlange für die Wiederverwendung angefordert werden.

### <a name="cell-reuse-in-ios-6"></a>Wiederverwendung von Zellen in ios 6 und höher

IOS 6 hat ein Muster für eine Zell Wiederverwendung hinzugefügt, das der Einführung in Auflistungs Ansichten ähnelt. Obwohl das oben gezeigte vorhandene Wiederverwendungs Muster weiterhin aus Gründen der Abwärtskompatibilität unterstützt wird, ist dieses neue Muster vorzuziehen, da es die Notwendigkeit der NULL-Überprüfung für die Zelle entfällt.

Beim neuen Muster registriert eine Anwendung die Cell-Klasse oder den XIb, die verwendet werden soll, indem entweder `RegisterClassForCellReuse` oder `RegisterNibForCellReuse` im Konstruktor des Controllers aufgerufen wird. Wenn Sie die Zelle in der `GetCell`-Methode aus der Warteschlange entfernen, müssen Sie einfach `DequeueReusableCell` den Bezeichner übergeben, den Sie für die Cell-Klasse oder XIb und den Indexpfad registriert haben.

Der folgende Code registriert z. b. eine benutzerdefinierte Cell-Klasse in einem uitableviewcontroller:

```csharp
public class MyTableViewController : UITableViewController
{
  static NSString MyCellId = new NSString ("MyCellId");

  public MyTableViewController ()
  {
    TableView.RegisterClassForCellReuse (typeof(MyCell), MyCellId);
  }
  ...
}
```

Wenn die myCell-Klasse registriert ist, kann die Zelle in der `GetCell`-Methode der `UITableViewSource` aus der Warteschlange entfernt werden, ohne dass die zusätzliche NULL-Überprüfung erforderlich ist, wie unten dargestellt:

```csharp
class MyTableSource : UITableViewSource
{
  public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
  {
    // if cell is not available in reuse pool, iOS will create one automatically
    // no need to do null check and create cell manually
    var cell = (MyCell) tableView.DequeueReusableCell (MyCellId, indexPath);

    // do whatever you need to with cell, such as assigning properties, etc.

    return cell;
  }
}
```

Beachten Sie, dass Sie bei Verwendung des neuen wiederzuverwendungs Musters mit einer benutzerdefinierten Zellen Klasse den Konstruktor implementieren müssen, der eine `IntPtr`annimmt, wie im folgenden Code Ausschnitt gezeigt. andernfalls kann mit "-C" keine Instanz der Cell-Klasse erstellt werden:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Hier finden Sie Beispiele für die oben erläuterten Themen im **basierbaren** Beispiel, das mit diesem Artikel verknüpft ist.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Hinzufügen eines Indexes

Ein Index unterstützt den Benutzer beim Scrollen durch lange Listen, die normalerweise alphabetisch sortiert sind, obwohl Sie die Indizierung nach beliebigen Kriterien durchführen können. Das **basictableindex** -Beispiel lädt eine wesentlich längere Liste von Elementen aus einer Datei, um den Index zu veranschaulichen. Jedes Element im Index entspricht einem ' section ' der Tabelle.

 [![](populating-a-table-with-data-images/image5.png "The Index display")](populating-a-table-with-data-images/image5.png#lightbox)

Um ' Abschnitte ' zu unterstützen, müssen die Daten, die der Tabelle zugrunde liegen, gruppiert werden. daher erstellt das basictableindex-Beispiel eine `Dictionary<>` aus dem Array von Zeichen folgen, wobei der erste Buchstabe jedes Elements als Wörterbuch Schlüssel verwendet wird:

```csharp
indexedTableItems = new Dictionary<string, List<string>>();
foreach (var t in items) {
    if (indexedTableItems.ContainsKey (t[0].ToString ())) {
        indexedTableItems[t[0].ToString ()].Add(t);
    } else {
        indexedTableItems.Add (t[0].ToString (), new List<string>() {t});
    }
}
keys = indexedTableItems.Keys.ToArray ();
```

Die `UITableViewSource`-Unterklasse benötigt dann die folgenden Methoden, die für die Verwendung der `Dictionary<>` hinzugefügt oder geändert werden:

- **Nummeriofsection** – diese Methode ist optional. die Tabelle nimmt standardmäßig einen Abschnitt an. Beim Anzeigen eines Indexes sollte diese Methode die Anzahl der Elemente im Index zurückgeben (z. b. 26, wenn der Index alle Buchstaben des englischen Alphabets enthält).
- **Rowsinsection** – gibt die Anzahl der Zeilen in einem angegebenen Abschnitt zurück.
- **Sectionindextiteln** – gibt das Array von Zeichen folgen zurück, das zum Anzeigen des Indexes verwendet wird. Der Beispielcode gibt ein Array von Buchstaben zurück.

Die aktualisierten Methoden in der Beispieldatei **basictableindex/tablesource. cs** sehen wie folgt aus:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    return keys.Length;
}
public override nint RowsInSection (UITableView tableview, nint section)
{
    return indexedTableItems[keys[section]].Count;
}
public override string[] SectionIndexTitles (UITableView tableView)
{
    return keys;
}
```

Indizes werden im Allgemeinen nur mit dem einfachen Tabellen Stil verwendet.

<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Hinzufügen von Kopf-und Fußzeilen

Kopf-und Fußzeilen können zum visuellen Gruppieren von Zeilen in einer Tabelle verwendet werden. Die erforderliche Datenstruktur ähnelt dem Hinzufügen eines Indexes – ein `Dictionary<>` funktioniert wirklich gut. Anstatt das Alphabet zum Gruppieren der Zellen zu verwenden, wird in diesem Beispiel das Gemüse nach dem botanischen Typ gruppiert.
Die Ausgabe sieht wie folgt aus:

 [![](populating-a-table-with-data-images/image6.png "Sample Headers and Footers")](populating-a-table-with-data-images/image6.png#lightbox)

Zum Anzeigen der Kopf-und Fußzeilen der `UITableViewSource` Unterklasse sind diese zusätzlichen Methoden erforderlich:

- **Titleforheader** – gibt den Text zurück, der als Header verwendet werden soll.
- **Titleforfooter** – gibt den Text zurück, der als Fußzeile verwendet werden soll.

Die aktualisierten Methoden in der Beispieldatei **basictableheaderfooter/Code/tablesource. cs** sehen wie folgt aus:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    return keys[section];
}
public override string TitleForFooter (UITableView tableView, nint section)
{
    return indexedTableItems[keys[section]].Count + " items";
}
```

Sie können die Darstellung der Kopf-und Fußzeile mit einem Ansichts Objekt weiter anpassen, indem Sie die `GetViewForHeader`-und `GetViewForFooter` Methoden Überschreibungen für `UITableViewSource`verwenden.

## <a name="related-links"></a>Verwandte Themen

- [Workingwithtables (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
