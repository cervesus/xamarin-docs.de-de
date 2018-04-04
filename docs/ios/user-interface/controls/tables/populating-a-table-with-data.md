---
title: Auffüllen einer Tabelle mit Daten
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/22/2017
ms.openlocfilehash: c139b96adfc325e7c251f8093eab338ddf0c6337
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="populating-a-table-with-data"></a>Auffüllen einer Tabelle mit Daten

Zum Hinzufügen von Zeilen zu einer `UITableView` Sie implementieren müssen eine `UITableViewSource` -Unterklasse und überschreiben die Methoden, die in der Tabelle anzeigen aufruft, um selbst aufzufüllen.

Dieser Leitfaden behandelt:

- Erstellen von Unterklassen für eine UITableViewSource
- Zelle Wiederverwendung
- Hinzufügen eines Indexes
- Hinzufügen von Kopf- und Fußzeilen


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Erstellen von Unterklassen für UITableViewSource

Ein `UITableViewSource` Unterklasse wird allen zugewiesen `UITableView`. Die Tabellenansicht fragt die Quellklasse, um zu bestimmen, wie selbst (z. B., wie viele Zeilen erforderlich sind und die Höhe jeder Zeile, falls abweichend vom Standard) gerendert. Am wichtigsten ist, stellt die Quelle jede Zelle anzeigen, die mit Daten aufgefüllt.

Es gibt nur zwei obligatorische Methoden erforderlich, damit eine Tabelle, die Daten angezeigt:

-   **RowsInSection** – return ein [ `nint` ](http://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) die Anzahl von Zeilen mit Daten, die die Tabelle angezeigt werden sollte.
-   **GetCell** – return eine `UITableCellView` mit Daten für den entsprechenden Index der Zeile an die Methode übergebenen aufgefüllt.


Die Beispieldatei BasicTable **TableSource.cs** ist die einfachste mögliche Implementierung `UITableViewSource`. Im Codeausschnitt unten sehen Sie, dass er ein Array von Zeichenfolgen nimmt, die in der Tabelle anzeigen und eine Standardzellenformat mit jeder Zeichenfolge zurückgibt:

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

            //---- if there are no cells to reuse, create a new one
            if (cell == null)
            { cell = new UITableViewCell (UITableViewCellStyle.Default, CellIdentifier); }

            cell.TextLabel.Text = item;

            return cell;
        }
}
```

Ein `UITableViewSource` können eine beliebige Datenstruktur aus einem einfachen Zeichenfolgen-Array (wie im folgenden Beispiel gezeigt) eine Liste <> oder einer anderen Auflistung. Die Implementierung der `UITableViewSource` Methoden isoliert die Tabelle aus der Datenstruktur der zugrunde liegenden.

Um diese Unterklasse verwenden zu können, erstellen Sie ein Array von Zeichenfolgen zum Erstellen der Datenquelle, und klicken Sie dann weisen Sie es mit einer Instanz von `UITableView`:

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

Die daraus resultierende Tabelle sieht wie folgt:

 [![](populating-a-table-with-data-images/image3.png "Beispiel für die Tabelle ausgeführt wird")](populating-a-table-with-data-images/image3.png#lightbox)

Die meisten Tabellen ermöglicht dem Benutzer, eine Zeile aus, um Sie auszuwählen, und führen Sie eine andere Aktion (z. B. Wiedergabe eines Musiktitels Aufrufen eines Kontakts oder einem anderen Bildschirm anzeigen) zu berühren. Um dies zu erreichen, gibt es einige Dinge, die erforderlich ist. Zunächst erstellen wir eine AlertController um eine Meldung angezeigt wird, wenn der Benutzer durch das Hinzufügen der folgenden, klicken Sie auf eine Zeile auf die `RowSelected` Methode:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Als Nächstes erstellen Sie eine Instanz von unseren View-Controller:

```csharp
HomeScreen owner;
```
Fügen Sie einen Konstruktor, um UITableViewSource Klasse, die einen Controller Ansicht als Parameter akzeptiert und speichert es in einem Feld:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
Ändern Sie die ViewDidLoad-Methode, in dem die UITableViewSource-Klasse erstellt wird, übergeben, die `this` Referenz:

```csharp
table.Source = new TableSource(tableItems, this);
```
Schließlich zurück Ihre `RowSelected` -Methode, rufen `PresentViewController` auf das zwischengespeicherte Feld:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


Jetzt kann der Benutzer eine Zeile berühren, und eine Warnung wird angezeigt:



 [![](populating-a-table-with-data-images/image4.png "Die ausgewählte Zeile-Warnung")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>Zelle Wiederverwendung

In diesem Beispiel sind nur sechs Elemente, daher keine Zelle Wiederverwendung erforderlich besteht. Anzeigen von Hunderten oder Tausenden von Zeilen jedoch wäre es ein Abfall des Arbeitsspeichers zum Erstellen von Hunderten oder Tausenden von `UITableViewCell` Objekte, wenn nur wenige auf dem Bildschirm datensatzweise anzupassen.

Um diese Situation zu vermeiden, wenn eine Zelle aus dem Bildschirm ausgeblendet, die eine Sicht in einer Warteschlange für die Wiederverwendung platziert wird. Wenn der Benutzer einen Bildlauf durchführt, ruft die Tabelle `GetCell` zum Anfordern von neuer Ansichten angezeigt – eine vorhandene Zelle wiederverwenden (die nicht aktuell angezeigt wird) rufen Sie einfach die `DequeueReusableCell` Methode. Wenn eine Zelle für die Wiederverwendung verfügbar, die zurückgegeben wird ist, andernfalls ein NULL-Wert zurückgegeben, und Code muss eine neue Instanz der Zelle erstellen.

Dieser Codeausschnitt aus dem Beispiel wird das Muster veranschaulicht:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

Die `cellIdentifier` effektiv separate Warteschlangen für verschiedene Arten von Zelle erstellt. In diesem Beispiel, dass alle Zellen der gleichen, sodass immer nur jeweils hartcodiert aussehen wird Bezeichner verwendet. Gäbe es verschiedene Typen von Zelle sollten sie jeweils zu einer anderen-Bezeichnerzeichenfolge folgt, haben, bei der sie instanziiert werden und wenn sie aus der Warteschlange für die Wiederverwendung angefordert werden.

### <a name="cell-reuse-in-ios-6"></a>Zelle Wiederverwendung in iOS 6 und höher

iOS 6 hinzugefügt, eine Zelle Wiederverwendung Muster eine Einführung mit Auflistungsansichten ähnelt. Obwohl das oben dargestellte vorhandene Wiederverwendung Muster weiterhin unterstützt wird, für die Abwärtskompatibilität Kompatibilität, diesem neuen Muster vorzuziehen ist, wie die Notwendigkeit für die null-Prüfung auf die Zelle wird entfernt.

Eine Anwendung mit dem neuen Muster registriert, die Zellenklasse oder Xib verwendet werden, durch den Aufruf eines `RegisterClassForCellReuse` oder `RegisterNibForCellReuse` im Konstruktor des Controllers. Nachdem Entfernen der Zelle in der `GetCell` -Methode einfach aufzurufen, `DequeueReusableCell` übergeben den Bezeichner, die Sie für die Zellenklasse oder Xib und der Indexpfad registriert.

Der folgende Code registriert z. B. eine benutzerdefinierte Zellenklasse in einem UITableViewController:

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

Mit der MyCell-Klasse registriert, die Zelle kann werden aus der Warteschlange entfernt der `GetCell` Methode der `UITableViewSource` ohne die Notwendigkeit für zusätzliche Überprüfung auf null, wie unten:

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

Beachten Sie, wenn das neue Wiederverwendung Muster mit eine benutzerdefinierte Zellenklasse verwenden zu können, müssen Sie den Konstruktor implementieren, die eine `IntPtr`, wie im folgenden Codeausschnitt dargestellt wird, andernfalls Objective-C kann nicht so erstellen Sie eine Instanz der Zellenklasse:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Sehen Sie Beispiele für die im oben erläuterten Themen der **BasicTable** Beispiel in diesem Artikel verknüpft.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Hinzufügen eines Indexes

Ein Index hilft dem Benutzer, die einen Bildlauf durch lange Listen, die in der Regel alphabetisch sortiert werden, obwohl Sie indizieren können von Ihnen ausgewählten Kriterien werden sollen. Die **BasicTableIndex** Beispiel lädt eine längere Liste von Elementen aus einer Datei, um den Index zu veranschaulichen. Jedes Element im Index entspricht zu einem Abschnitt der Tabelle.

 [![](populating-a-table-with-data-images/image5.png "Die Index-Anzeige")](populating-a-table-with-data-images/image5.png#lightbox)

Zur Unterstützung von "Abschnitte" z. B. Daten hinter der Tabelle gruppiert werden, müssen, damit das Beispiel BasicTableIndex erstellt eine `Dictionary<>` aus dem Array von Zeichenfolgen, die den ersten Buchstaben jedes Elements als Wörterbuchschlüssel verwenden:

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

Die `UITableViewSource` Unterklasse benötigt klicken Sie dann die folgenden Methoden hinzugefügt oder geändert, sodass verwendet die `Dictionary<>` :

-   **NumberOfSections** – diese Methode ist optional, wird standardmäßig nimmt der Tabelle ein Abschnitt. Beim Anzeigen eines Indexes sollte diese Methode die Anzahl der Elemente zurück, in den Index (z. B. 26, wenn der Index alle Buchstaben des englischen Alphabets enthält).
-   **RowsInSection** – gibt die Anzahl der Zeilen in einem angegebenen Bereich zurück.
-   **SectionIndexTitles** – gibt das Array von Zeichenfolgen, die verwendet wird, um den Index anzuzeigen. Der Beispielcode gibt ein Array von Buchstaben zurück.


Die aktualisierte Methoden in der Beispieldatei **BasicTableIndex/TableSource.cs** wie folgt aussehen:

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

Indizes werden im Allgemeinen nur mit einfachen Tabellenformat verwendet.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Hinzufügen von Kopf- und Fußzeilen

Kopf- und Fußzeilen können verwendet werden, um Zeilen in einer Tabelle visuell zu gruppieren. Die Datenstruktur, die erforderlich ist sehr ähnlich, einen Index hinzufügen – `Dictionary<>` wirklich gut funktioniert. Anstatt das Alphabet um Zellen zu gruppieren, wird in diesem Beispiel wird die Gemüse botanischen Typ gruppieren.
Die Ausgabe sieht wie folgt aus:

 [![](populating-a-table-with-data-images/image6.png "Beispiel Kopf- und Fußzeilen")](populating-a-table-with-data-images/image6.png#lightbox)

Um die Anzeige von Kopf- und Fußzeilen der `UITableViewSource` Unterklasse erfordert diese zusätzlichen Methoden:

-   **TitleForHeader** – gibt den Text als Header verwenden
-   **TitleForFooter** – gibt den Text für die Verwendung der Fußzeile zurück.


Die aktualisierte Methoden in der Beispieldatei **BasicTableHeaderFooter/Code/TableSource.cs** wie folgt aussehen:

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

Sie können die Darstellung von Kopf- und Fußzeilen mit einer Ansicht weiter anpassen-Objekt unter Verwendung der `GetViewForHeader` und `GetViewForFooter` methodenüberschreibungen auf `UITableViewSource`.


## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
