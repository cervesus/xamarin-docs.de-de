---
title: Auffüllen einer Tabelle mit Daten in Xamarin.iOS
description: Dieses Dokument beschreibt, wie eine Tabelle in einer Xamarin.iOS-Anwendung mit Daten aufgefüllt wird. Es wird erläutert, UITableViewSource, Wiederverwendung der Zelle, ein Index, und die Kopf- und Fußzeilen hinzufügen.
ms.prod: xamarin
ms.assetid: 6FE64DDF-1029-EB9B-6EEC-1C7DFDFDF3AF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/22/2017
ms.openlocfilehash: 5363e3a2210bdcf1efb870ac808ecb37584de6a7
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/08/2019
ms.locfileid: "57668919"
---
# <a name="populating-a-table-with-data-in-xamarinios"></a>Auffüllen einer Tabelle mit Daten in Xamarin.iOS

Zum Hinzufügen von Zeilen zu einer `UITableView` Sie implementieren müssen, eine `UITableViewSource` -Unterklasse und überschreiben die Methoden, die in der Tabelle anzeigen Aufrufe auffüllen.

Dieser Leitfaden behandelt:

- Erstellen von Unterklassen für eine UITableViewSource
- Zelle Wiederverwendung
- Hinzufügen eines Indexes
- Hinzufügen von Kopf- und Fußzeilen


<a name="Subclassing_UITableViewSource" />

## <a name="subclassing-uitableviewsource"></a>Erstellen von Unterklassen für UITableViewSource

Ein `UITableViewSource` Unterklasse zugewiesen wird, die jeder `UITableView`. Die Tabellenansicht fragt die Source-Klasse, um zu bestimmen, wie zum Rendern (z. B., wie viele Zeilen erforderlich sind und die Höhe der einzelnen Zeilen an, falls abweichend vom Standard). Wichtiger ist, stellt die Quelle jeder Zelle Ansicht mit Daten aufgefüllt.

Es gibt nur zwei erforderliche Methoden erforderlich, um eine Tabelle, die Daten anzuzeigen:

-   **RowsInSection** : Zurückgeben einer [ `nint` ](https://developer.xamarin.com/guides/cross-platform/macios/nativetypes/) die Anzahl von Zeilen mit Daten, die in der Tabelle angezeigt werden sollte.
-   **GetCell** : Zurückgeben einer `UITableCellView` aufgefüllt, die mit Daten für den entsprechenden Zeilenindex, die an die Methode übergeben.


Die Beispieldatei BasicTable **TableSource.cs** ist die einfachste mögliche Implementierung der `UITableViewSource`. Im folgenden Codeausschnitt ersichtlich, dass es ein Array von Zeichenfolgen akzeptiert, die in der Tabelle angezeigt und eine Standardzellenstil gibt, jeder Zeichenfolge enthält:

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

Ein `UITableViewSource` können eine beliebige Datenstruktur, aus einem Array von einfachen Zeichenfolge (wie im folgenden Beispiel gezeigt) eine Liste <> oder einer anderen Auflistung. Die Implementierung der `UITableViewSource` Methoden wird in der Tabelle aus der zugrunde liegende Datenstruktur isoliert.

Um diese Unterklasse verwenden möchten, erstellen Sie ein Zeichenfolgenarray zum Erstellen der Datenquelle, und klicken Sie dann weisen Sie es mit einer Instanz von `UITableView`:

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

Die daraus resultierende Tabelle sieht folgendermaßen aus:

 [![](populating-a-table-with-data-images/image3.png "Beispiel für eine Tabelle ausgeführt wird")](populating-a-table-with-data-images/image3.png#lightbox)

Die meisten Tabellen ermöglichen den Benutzer, tippen Sie auf eine Zeile aus, um Sie auszuwählen, und führen Sie eine andere Aktion (z. B. Wiedergabe eines Musiktitels, Aufruf eines Kontakts oder einem anderen Bildschirm anzeigen). Um dies zu erreichen, gibt es einige Dinge, die wir tun müssen. Zunächst erstellen wir eine AlertController um eine Meldung angezeigt, wenn der Benutzer klicken auf eine Zeile durch Hinzufügen der folgenden Optionen, um die `RowSelected` Methode:

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    UIAlertController okAlertController = UIAlertController.Create ("Row Selected", tableItems[indexPath.Row], UIAlertControllerStyle.Alert);
    okAlertController.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Default, null));
    ...

    tableView.DeselectRow (indexPath, true);
}
```

Als Nächstes erstellen Sie eine Instanz des Controllers anzeigen:

```csharp
HomeScreen owner;
```
Fügen Sie einen Konstruktor, der UITableViewSource-Klasse, die einen View Controller als Parameter akzeptiert und speichert es in einem Feld:

```csharp
public TableSource (string[] items, HomeScreen owner)
{
    ...
    this.owner = owner;

}
```
Ändern Sie die ViewDidLoad-Methode, in die UITableViewSource-Klasse erstellt wird, übergeben, die `this` Verweis:

```csharp
table.Source = new TableSource(tableItems, this);
```
Schließlich zurück Ihre `RowSelected` -Methode, rufen `PresentViewController` auf das Feld "zwischengespeicherten":

```csharp
public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
{
    ...
    owner.PresentViewController (okAlertController, true, null);

    ...
}
```


Jetzt kann Benutzer eine Zeile berühren, und eine Warnung wird angezeigt:



 [![](populating-a-table-with-data-images/image4.png "Die ausgewählte Zeile-Warnung")](populating-a-table-with-data-images/image4.png#lightbox)


## <a name="cell-reuse"></a>Zelle Wiederverwendung

In diesem Beispiel sind nur sechs Elemente, es gibt also keine Zelle wiederverwenden, die erforderlich sind. Wenn Sie Hunderte oder Tausende von Zeilen anzeigen, jedoch wäre es eine arbeitsspeicherverschwendung, Hunderte oder Tausende von erstellen `UITableViewCell` Objekte, wenn nur einige zu einem Zeitpunkt auf dem Bildschirm passen.

Um diese Situation zu vermeiden, wenn eine Zelle vom Bildschirm verschwindet, die in eine Warteschlange für die Wiederverwendung platziert wird. Der Benutzer scrollt, ruft die Tabelle `GetCell` zum Anfordern von neuer Ansichten angezeigt: eine vorhandene Zelle wiederverwendet werden (die nicht gerade angezeigt wird) rufen Sie einfach die `DequeueReusableCell` Methode. Wenn eine Zelle für die Wiederverwendung verfügbar, die es zurückgegeben wird ist, andernfalls wird Null zurückgegeben, und Ihr Code muss eine neue Instanz der Zelle erstellen zu können.

Dieser Codeausschnitt aus dem Beispiel veranschaulicht das Muster:

```csharp
// request a recycled cell to save memory
UITableViewCell cell = tableView.DequeueReusableCell (cellIdentifier);
// if there are no cells to reuse, create a new one
if (cell == null)
    cell = new UITableViewCell (UITableViewCellStyle.Default, cellIdentifier);
```

Die `cellIdentifier` effektiv erstellt separate Warteschlangen für verschiedene Arten der Zelle. In diesem Beispiel, dass alle Zellen die gleiche also nur eine hartcodierte aussehen wird Bezeichner verwendet. Gäbe es verschiedene Arten von Zelle müssen sie jeweils zu eine anderen ID-Zeichenfolge, verfügen, wenn sie instanziiert werden und wenn sie aus der Warteschlange für die Wiederverwendung angefordert werden.

### <a name="cell-reuse-in-ios-6"></a>Zelle Wiederverwendung in iOS 6 und höher

iOS 6 hinzugefügt Zelle Wiederverwendung ähnlich vorgehen, die eine Einführung mit Auflistungsansichten. Obwohl das vorhandene Wiederverwendung-Muster, das oben gezeigte weiterhin unterstützt wird, für die Abwärtskompatibilität, diesem neuen Muster besser ist als die Notwendigkeit, die null-Überprüfung in der Zelle entfällt.

Mit dem neuen Muster für eine Anwendung registriert, die Cell-Klasse oder eine Xib verwendet werden, durch Aufrufen von entweder `RegisterClassForCellReuse` oder `RegisterNibForCellReuse` im Konstruktor des Controllers. Dann, wenn dabei die Zelle in der `GetCell` -Methode einfach Aufruf `DequeueReusableCell` übergeben den Bezeichner, die Sie für die Cell-Klasse oder Xib und den Indexpfad registriert.

Der folgende Code registriert eine benutzerdefinierte Zellenklasse z. B. in einem UITableViewController an:

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

Mit der Klasse "MyCell" registriert ist, kann die Zelle entfernt werden die `GetCell` Methode der `UITableViewSource` ohne die Notwendigkeit, die zusätzliche Überprüfung auf null, siehe unten:

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

Beachten Sie, wenn das neue Wiederverwendung Muster für eine benutzerdefinierte Zellenklasse verwenden zu können, müssen Sie den Konstruktor implementieren, die eine `IntPtr`, wie im folgenden Codeausschnitt gezeigt wird, andernfalls Objective-C nicht möglich, eine Instanz der Zellenklasse erstellen:

```csharp
public class MyCell : UITableViewCell
{
  public MyCell (IntPtr p):base(p)
  {
  }
  ...
}
```

Sehen Sie Beispiele für die im oben erläuterten Themen der **BasicTable** verknüpfte in diesem Artikel.

<a name="Adding_an_Index" />

## <a name="adding-an-index"></a>Hinzufügen eines Indexes

Ein Index hilft dem Benutzer, die einen Bildlauf durch lange Listen, i. d. r. alphabetisch angeordnet werden, obwohl Sie indiziert werden können, von Ihnen ausgewählten Kriterien werden soll. Die **BasicTableIndex** Beispiel lädt eine viel größere Anzahl von Elementen aus einer Datei aus, um den Index zu veranschaulichen. Jedes Element im Index entspricht einer "Section" der Tabelle.

 [![](populating-a-table-with-data-images/image5.png "Die Index-Anzeige")](populating-a-table-with-data-images/image5.png#lightbox)

Zur Unterstützung von "Sections" müssen die Daten hinter der Tabelle gruppiert werden, damit das BasicTableIndex-Beispiel erstellt eine `Dictionary<>` aus dem Array von Zeichenfolgen, die den ersten Buchstaben jedes Elements als Wörterbuchschlüssel verwenden:

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

Die `UITableViewSource` Unterklasse benötigt klicken Sie dann die folgenden Methoden hinzugefügt oder geändert, um Sie verwenden die `Dictionary<>` :

-   **NumberOfSections** – diese Methode ist optional, standardmäßig die Tabelle setzt ein Abschnitt. Beim Anzeigen eines Indexes sollte diese Methode die Anzahl der Elemente zurück, in den Index (z. B. 26, wenn der Index alle Buchstaben des englischen Alphabets enthält).
-   **RowsInSection** – gibt die Anzahl der Zeilen in einem bestimmten Abschnitt zurück.
-   **SectionIndexTitles** – gibt das Array von Zeichenfolgen, die verwendet wird, um den Index anzuzeigen. Der Code gibt ein Array von Buchstaben zurück.


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

Indizes werden in der Regel nur mit einfachen Tabellenformat verwendet.


<a name="Adding_Headers_and_Footers" />

## <a name="adding-headers-and-footers"></a>Hinzufügen von Kopf- und Fußzeilen

Kopf- und Fußzeilen können verwendet werden, um Zeilen in einer Tabelle visuell zu gruppieren. Die Datenstruktur, die erforderlich ist sehr ähnlich, mit dem Hinzufügen eines Indexes – `Dictionary<>` funktioniert wirklich gut. Statt des Alphabets, um Zellen zu gruppieren, wird in diesem Beispiel wird die Gemüse nach botanischen Typ gruppieren.
Die Ausgabe sieht wie folgt aus:

 [![](populating-a-table-with-data-images/image6.png "Beispiel-Kopf- und Fußzeilen")](populating-a-table-with-data-images/image6.png#lightbox)

Zum Anzeigen von Kopf- und Fußzeilen der `UITableViewSource` Unterklasse erfordert diese zusätzlichen Methoden:

-   **TitleForHeader** – gibt den Text zurück, die als Header verwenden.
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

Sie können die Darstellung von Kopf- und Fußzeile mit einer Ansicht weiter anpassen Objekt, um mithilfe der `GetViewForHeader` und `GetViewForFooter` methodenüberschreibungen auf `UITableViewSource`.


## <a name="related-links"></a>Verwandte Links

- [WorkingWithTables (Beispiel)](https://developer.xamarin.com/samples/monotouch/WorkingWithTables)
