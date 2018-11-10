---
title: Tabellensichten in Xamarin.Mac
description: Dieser Artikel behandelt die Arbeit mit Tabellenansichten in einer Xamarin.Mac-Anwendung. Erstellen von Tabellenansichten in Xcode und Interface Builder und mit ihnen interagieren, im Code beschrieben.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: lobrien
ms.author: laobri
ms.date: 03/14/2017
ms.openlocfilehash: 9a8c55c8b4ff3fbd515aad8bf45c52a0b549af9f
ms.sourcegitcommit: b60a37587aad8a0bfa8a522d88d22fa672002443
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285598"
---
# <a name="table-views-in-xamarinmac"></a>Tabellensichten in Xamarin.Mac

_Dieser Artikel behandelt die Arbeit mit Tabellenansichten in einer Xamarin.Mac-Anwendung. Erstellen von Tabellenansichten in Xcode und Interface Builder und mit ihnen interagieren, im Code beschrieben._

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleiche Tabelle, die ein Entwickler Ansichten *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und verwalten Ihre Ansichten für die Tabelle (oder erstellen sie optional direkt in c#-Code).

Eine Tabellenansicht zeigt Daten in einem Tabellenformat mit einer oder mehreren Spalten der Informationen in mehreren Zeilen. Basierend auf den Typ der Tabelle anzeigen, die erstellt wird, der Benutzer kann nach Spalte sortieren, reorganize Spalten, Spalten hinzufügen, Entfernen von Spalten oder zum Bearbeiten der Daten in der Tabelle enthalten sind.

[![](table-view-images/intro01.png "Beispieltabelle")](table-view-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Tabellenansichten in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Einführung in die Tabellenansichten

Eine Tabellenansicht zeigt Daten in einem Tabellenformat mit einer oder mehreren Spalten der Informationen in mehreren Zeilen. Tabellenansichten in Scroll-Ansichten angezeigt werden (`NSScrollView`) und MacOS 10.7 ab, Sie können eine `NSView` anstelle von Zellen (`NSCell`) zum Anzeigen von Zeilen und Spalten. Allerdings können Sie trotzdem verwenden `NSCell` jedoch müssen Sie in der Regel Unterklasse `NSTableCellView` erstellen und die benutzerdefinierte Zeilen und Spalten.

Eine Tabellenansicht eigenen Daten nicht gespeichert, stattdessen greift Sie auf eine Datenquelle (`NSTableViewDataSource`) um die Zeilen und Spalten, die erforderlich sind, klicken Sie auf einen Bedarf bereitzustellen.

Eine Tabellenansicht Verhalten kann angepasst werden, durch die Bereitstellung einer Unterklasse des Delegaten Tabelle anzeigen (`NSTableViewDelegate`) um die Verwaltung der Spalte zu unterstützen, geben Sie zum Auswählen von Funktionen, Zeilenauswahl, und bearbeiten, benutzerdefinierte nachverfolgung und benutzerdefinierte Ansichten für einzelne Spalten und Zeilen.

Beim Erstellen von Ansichten der Tabelle hat Apple die folgenden Empfehlungen:

* Ermöglicht dem Benutzer zum Sortieren der Tabelle, indem Sie auf einer Spaltenüberschrift klicken.
* Erstellen Sie die Spaltenüberschriften, die Nomen oder kurze nominale Ausdrücke, die beschreiben, die Daten in dieser Spalte angezeigt wird.

Weitere Informationen finden Sie unter den [Inhaltsanzeigen](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Abschnitt des Apple [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Erstellen und verwalten Tabellenansichten in Xcode

Wenn Sie eine neue Xamarin.Mac-Cocoa-Anwendung erstellen, erhalten Sie ein Standardfenster leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` Datei automatisch im Projekt enthalten ist. So bearbeiten Sie Ihre Windows-Design, in der **Projektmappen-Explorer**, Doppelklick der `Main.storyboard` Datei:

[![](table-view-images/edit01.png "Wählen die Haupt-storyboard")](table-view-images/edit01.png#lightbox)

Dies öffnet das Fenster Design in Interface Builder von Xcode:

[![](table-view-images/edit02.png "Bearbeiten die Benutzeroberfläche in Xcode")](table-view-images/edit02.png#lightbox)

Typ `table` in die **der Bibliotheksinspektor** Suchfeld zum Vereinfachen der Tabellenansicht Steuerelemente:

[![](table-view-images/edit03.png "Wählen eine Tabellensicht aus der Bibliothek")](table-view-images/edit03.png#lightbox)

Ziehen Sie eine Tabellensicht auf den Ansichtscontroller im der **Schnittstellen-Editor**, können sie den Inhaltsbereich des Ansichtscontrollers füllen, und legen Sie dafür, wo sie verkleinert und vergrößert werden, mit dem Fenster in der **Einschränkung Editor**:

[![](table-view-images/edit04.png "Bearbeiten von Einschränkungen")](table-view-images/edit04.png#lightbox)

Wählen Sie in der Tabelle anzeigen der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attributinspektor**:

[![](table-view-images/edit05.png "Attribute Inspector")](table-view-images/edit05.png#lightbox)

- **Inhalt Modus** -können Sie mit beiden Ansichten (`NSView`) oder Zellen (`NSCell`) zum Anzeigen der Daten in den Zeilen und Spalten. Beginnen mit MacOS 10.7, sollten Sie die Ansichten verwenden.
- **Gruppieren von Zeilen gleitet** – Wenn `true`, die Tabellenansicht wird gruppierte Zellen gezeichnet, als ob sie bewegliche sind.
- **Spalten** -definiert die Anzahl der angezeigten Spalten.
- **Header** – Wenn `true`, die Spalten müssen die Header.
- **Neuanordnen von** – Wenn `true`, der Benutzer wird in der Lage, ziehen die Spalten in der Tabelle neu anordnen.
- **Ändern der Größe** – Wenn `true`, der Benutzer wird in der Lage, Spaltenheader, um die Spaltenbreite zu ziehen.
- **Größenanpassung von Spalten** -steuert, wie in der Tabelle die Spaltengröße auto ist.
- **Markieren Sie** -Steuerelemente, die den Typ der Hervorhebung in der Tabelle verwendet, wenn eine Zelle ausgewählt wird.
- **Alternative Zeilen** – Wenn `true`, jemals andere Zeile hat eine andere Hintergrundfarbe.
- **Horizontale Raster** -wählt den Typ des Rahmens zwischen den Zellen gezeichnet, horizontal.
- **Vertikales Raster** -wählt den Typ des Rahmens, der vertikal zwischen den Zellen gezeichnet.
- **Rasterfarbe** -legt die Rahmenfarbe der Zelle.
- **Hintergrund** -legt die Hintergrundfarbe der Zelle fest.
- **Auswahl** -können Sie steuern, wie der Benutzer die Zellen in der Tabelle als auswählen kann:
    - **Mehrere** – Wenn `true`, der Benutzer kann mehrere Zeilen und Spalten auswählen.
    - **Spalte** – Wenn `true`, kann der Benutzer Spalten auswählen.
    - **Geben Sie die Option** – Wenn `true`, der Benutzer kann ein Zeichen zum Auswählen einer Zeile eingeben.
    - **Leere** – Wenn `true`, der Benutzer ist nicht erforderlich, um eine Zeile oder Spalte auszuwählen, in der Tabelle ermöglicht keine Auswahl überhaupt.
- **Automatisches Speichern** – den Namen, das das Format für die Tabellen automatisch zu speichern, klicken Sie unter.
- **Spalteninformationen** – Wenn `true`, die Reihenfolge und Breite der Spalten werden automatisch gespeichert.
- **Zeilenumbrüche** – aus, wie sich die Zelle Zeilenumbrüche behandelt.
- **Schneidet die letzte sichtbare Zeile** – Wenn `true`, die Zelle in der Daten können nicht innerhalb der Grenzen passen abgeschnitten.

> [!IMPORTANT]
> Wenn Sie eine ältere Xamarin.Mac-Anwendung verwalten `NSView` basierend Tabellenansichten verwendet werden soll, über `NSCell` Tabellenansichten basiert. `NSCell` ältere gilt und möglicherweise nicht in Zukunft unterstützt werden.

Wählen Sie eine Tabellenspalte in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attributinspektor**:

[![](table-view-images/edit06.png "Attribute Inspector")](table-view-images/edit06.png#lightbox)

- **Titel** -definiert den Titel der Spalte.
- **Ausrichtung** -legen Sie die Ausrichtung des Texts in den Zellen.
- **Schriftart des Titels** – wählt die Schriftart für den Headertext für der Zelle ab.
- **Schlüssel sortieren** -ist der Schlüssel verwendet, um Daten in der Spalte zu sortieren. Lassen Sie leer, wenn der Benutzer kann nicht in dieser Spalte sortieren.
- **Selektor** -ist der **Aktion** verwendet, um die Sortierung durchzuführen. Lassen Sie leer, wenn der Benutzer kann nicht in dieser Spalte sortieren.
- **Reihenfolge** -stimmt die Sortierreihenfolge für die Spaltendaten.
- **Ändern der Größe** -wählt den Typ des Ändern der Größe für die Spalte.
- **Bearbeitbare** – Wenn `true`, der Benutzer kann Zellen in einer Zelle basierend Tabelle bearbeiten.
- **Ausgeblendete** – Wenn `true`, die Spalte ausgeblendet ist.

Sie können auch die Größe der Spalte ändern, ziehen Sie dessen Handle (vertikal zentriert rechts in der Spalte ") links oder rechts.

Wir wählen Sie die einzelnen Spalten in unserer Tabelle aus, und geben Sie die erste Spalte einer **Titel** von `Product` und der zweite `Details`.

Wählen Sie eine Tabellenansicht der Zelle (`NSTableViewCell`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attributinspektor**:

[![](table-view-images/edit07.png "Attribute Inspector")](table-view-images/edit07.png#lightbox)

Hierbei handelt es sich um alle Eigenschaften einer Standardsicht. Sie können auch das Ändern der Größe der Zeilen für diese Kolumne.

Wählen Sie eine Zelle der Tabelle anzeigen (Standardmäßig ist dies ein `NSTextField`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen im der **Attributinspektor**:

[![](table-view-images/edit08.png "Attribute Inspector")](table-view-images/edit08.png#lightbox)

Sie müssen alle Eigenschaften des ein standard-Text-Feld, hier festzulegen. Standardmäßig wird ein standard-Text-Feld verwendet, um Daten für eine Zelle in einer Spalte anzuzeigen.

Wählen Sie eine Tabellenansicht der Zelle (`NSTableFieldCell`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attributinspektor**:

[![](table-view-images/edit09.png "Attribute Inspector")](table-view-images/edit09.png#lightbox)

Hier die wichtigsten Einstellungen sind:

- **Layout** – aus, wie Zellen in dieser Spalte angeordnet sind.
- **Verwendet einmaliges Zeilenmodus** – Wenn `true`, die Zelle ist eine einzelne Zeile auf.
- **Erste Runtime Layoutbreite** – Wenn `true`, die Zelle am liebsten der Breite festgelegt (entweder manuell oder automatisch) Wenn beim ersten der Anwendung ausführen wird angezeigt.
- **Aktion** -steuert, wann die Bearbeitung **Aktion** wird für die Zelle gesendet.
- **Verhalten** -definiert werden, wenn eine Zelle ausgewählt oder bearbeitet werden.
- **Rich-Text** – Wenn `true`, die Zelle kann formatiert und formatierten Text angezeigt.
- **Rückgängig machen** – Wenn `true`, die Zelle übernimmt die Verantwortung für ihn ist Verhalten rückgängig zu machen.

Wählen Sie die Zelle Tabellenansicht (`NSTableFieldCell`) am unteren Rand einer Tabellenspalte in der **Schnittstellenhierarchie**:

[![](table-view-images/edit10.png "Die Tabellenansicht der Zelle auswählen")](table-view-images/edit10.png#lightbox)

Dadurch können Sie so bearbeiten Sie die Tabelle Zelle anzeigen, die als Basis verwendet _Muster_ für alle Zellen, die für die angegebene Spalte erstellt.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Hinzufügen von Aktionen und Ergebnisdaten

Nur wie jedes andere Cocoa-UI-Steuerelement, müssen wir unsere Tabellenansicht verfügbar zu machen und dabei handelt es sich Spalten für C#-Code mit Zellen **Aktionen** und **Outlets** (basierend auf die erforderliche Funktionalität).

Der Prozess ist für alle Table View-Elemente, die verfügbar gemacht werden soll:

1. Wechseln Sie zu der **Assistant Editor** und sicherstellen, dass die `ViewController.h` Datei ausgewählt ist: 

    [![](table-view-images/edit11.png "Die Assistenten-Editor")](table-view-images/edit11.png#lightbox)
2. Wählen Sie die Ansicht der Tabelle in der **Schnittstellenhierarchie**, Strg + Klick und ziehen Sie in der `ViewController.h` Datei.
3. Erstellen Sie eine **Outlet** für die Ansicht der Tabelle mit der Bezeichnung `ProductTable`: 

    [![](table-view-images/edit13.png "Konfigurieren eines Outlets")](table-view-images/edit13.png#lightbox)
4. Erstellen Sie **Outlets** wird aufgerufen, für die Tabellenspalten sowie `ProductColumn` und `DetailsColumn`: 

    [![](table-view-images/edit14.png "Konfigurieren eines Outlets")](table-view-images/edit14.png#lightbox)
5. Speichern Sie die Änderungen und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Als Nächstes Schreiben der Anzeige von Code wir einige Daten für die Tabelle, wenn die Anwendung ausgeführt wird.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Auffüllen der Tabellenansicht

Mit unserer Tabellenansicht Interface Builder entwickelt und über verfügbar gemachte ein **Outlet**, als Nächstes müssen wir die C#-Code, um es zu füllen zu erstellen.

Zunächst erstellen wir ein neues `Product` Klasse, die die Informationen für die einzelnen Zeilen enthält. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `Product` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche:

[![](table-view-images/populate01.png "Erstellen eine leere Klasse")](table-view-images/populate01.png#lightbox)

Stellen Sie die `Product.cs` Datei aussehen wie folgt:

```csharp
using System;

namespace MacTables
{
    public class Product
    {
        #region Computed Propoperties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        #endregion

        #region Constructors
        public Product ()
        {
        }

        public Product (string title, string description)
        {
            this.Title = title;
            this.Description = description;
        }
        #endregion
    }
}

```

Als Nächstes müssen wir erstellen eine Unterklasse von `NSTableDataSource` die Daten der Tabelle bereit, wie angefordert. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductTableDataSource` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

Bearbeiten der `ProductTableDataSource.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDataSource : NSTableViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductTableDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetRowCount (NSTableView tableView)
        {
            return Products.Count;
        }
        #endregion
    }
}

```

Diese Klasse verfügt über Speicher für unsere Tabellenansicht Elemente und überschreibt die `GetRowCount` um die Anzahl der Zeilen in der Tabelle zurückzugeben.

Abschließend müssen wir erstellen eine Unterklasse von `NSTableDelegate` auf das Verhalten für unsere Tabelle bereitzustellen. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductTableDelegate` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

Bearbeiten der `ProductTableDelegate.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
    public class ProductTableDelegate: NSTableViewDelegate
    {
        #region Constants 
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductTableDataSource DataSource;
        #endregion

        #region Constructors
        public ProductTableDelegate (ProductTableDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods
        public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
        {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = DataSource.Products [(int)row].Title;
                break;
            case "Details":
                view.StringValue = DataSource.Products [(int)row].Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Wenn wir erstellen eine Instanz von der `ProductTableDelegate`, übergeben wir auch in einer Instanz von der `ProductTableDataSource` , das die Daten für die Tabelle bereitstellt. Die `GetViewForItem` Methode ist verantwortlich für die Rückgabe einer Ansicht (Daten), um die Zelle für die einer bestimmten Spalte und Zeile anzuzeigen. Wenn möglich, eine vorhandene Sicht wird wiederverwendet werden, um die Zelle anzuzeigen, wenn keine neue Ansicht erstellt werden muss.

Um die Tabelle zu füllen, ermöglicht das Bearbeiten der `ViewController.cs` Datei, und stellen die `AwakeFromNib` Methode sehen wie folgt:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create the Product Table Data Source and populate it
    var DataSource = new ProductTableDataSource ();
    DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
    DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

    // Populate the Product Table
    ProductTable.DataSource = DataSource;
    ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

Wenn wir die Anwendung ausführen, wird Folgendes angezeigt:

[![](table-view-images/populate02.png "Führen Sie eine Beispiel-app")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sortieren nach Spalte

Wir können Benutzer die Daten in der Tabelle durch Klicken auf eine Spaltenüberschrift sortieren. Doppelklicken Sie zunächst auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie die `Product` Spalte Geben Sie `Title` für die **Sortierschlüssel**, `compare:` für die **Selektor** , und wählen Sie `Ascending` für die **Reihenfolge**:

[![](table-view-images/sort01.png "Festlegen der Schlüssel für die Sortierung")](table-view-images/sort01.png#lightbox)

Wählen Sie die `Details` Spalte Geben Sie `Description` für die **Sortierschlüssel**, `compare:` für die **Selektor** , und wählen Sie `Ascending` für die **Reihenfolge**:

[![](table-view-images/sort02.png "Festlegen der Schlüssel für die Sortierung")](table-view-images/sort02.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Jetzt ermöglicht das Bearbeiten der `ProductTableDataSource.cs` Datei, und fügen Sie die folgenden Methoden:

```csharp
public void Sort(string key, bool ascending) {

    // Take action based on key
    switch (key) {
    case "Title":
        if (ascending) {
            Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
        } else {
            Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
        }
        break;
    case "Description":
        if (ascending) {
            Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
        } else {
            Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
        }
        break;
    }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    if (oldDescriptors.Length > 0) {
        // Update sort
        Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    } else {
        // Grab current descriptors and update sort
        NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
        Sort (tbSort[0].Key, tbSort[0].Ascending); 
    }
            
    // Refresh table
    tableView.ReloadData ();
}
```

Die `Sort` Methode erlauben Sie die zum Sortieren der Daten in der Datenquelle, die basierend auf einer bestimmten `Product` Klassenfeld in aufsteigender oder absteigender Reihenfolge. Die überschriebene `SortDescriptorsChanged` aufgerufene Methode jedes Mal, wenn die Verwendung einer Spaltenüberschrift klickt. Sie übergeben die **Schlüssel** Wert an, die wir in Interface Builder und die Sortierreihenfolge für diese Spalte festgelegt.

Wenn wir die Anwendung auszuführen und in den Spaltenüberschriften klicken, werden die Zeilen nach dieser Spalte sortiert werden:

[![](table-view-images/sort03.png "Eine Beispiel-app-Ausführung")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zeilenauswahl

Wenn Sie zulassen möchten, den Benutzer, wählen Sie eine einzelne Zeile aus, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Tabelle anzeigen der **Schnittstellenhierarchie** , und deaktivieren Sie die **mehrere** Kontrollkästchen in der **Attributinspektor**:

[![](table-view-images/select01.png "Attribute Inspector")](table-view-images/select01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.


Als Nächstes bearbeiten Sie die `ProductTableDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Dadurch wird der Benutzer die Auswahl eine einzelne Zeile in der Tabellenansicht. Zurückgeben `false` für die `ShouldSelectRow` für jede, die Zeile nicht die Benutzer können ausgewählt werden soll oder `false` für jede Zeile aus, wenn Sie nicht, dass die Benutzer können keine Zeilen ausgewählt werden möchten.

Die Tabellenansicht (`NSTableView`) enthält die folgenden Methoden für die Arbeit mit Zeilenauswahl:

- `DeselectRow(nint)` – Deaktiviert die angegebene Zeile in der Tabelle.
- `SelectRow(nint,bool)` : Wählt die angegebene Zeile. Übergeben Sie `false` für den zweiten Parameter um jeweils nur eine Zeile auswählen.
- `SelectedRow` -Gibt die aktuelle Zeile in der Tabelle ausgewählt.
- `IsRowSelected(nint)` -Gibt `true` , wenn die angegebene Zeile ausgewählt ist.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Mehrere Zeilenauswahl

Wenn Sie zulassen möchten, den Benutzer einen mehrere Zeilen auswählen, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Tabelle anzeigen der **Schnittstellenhierarchie** und überprüfen Sie die **mehrere** Kontrollkästchen in der **Attributinspektor**:

[![](table-view-images/select02.png "Attribute Inspector")](table-view-images/select02.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.


Als Nächstes bearbeiten Sie die `ProductTableDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Dadurch wird der Benutzer die Auswahl eine einzelne Zeile in der Tabellenansicht. Zurückgeben `false` für die `ShouldSelectRow` für jede, die Zeile nicht die Benutzer können ausgewählt werden soll oder `false` für jede Zeile aus, wenn Sie nicht, dass die Benutzer können keine Zeilen ausgewählt werden möchten.

Die Tabellenansicht (`NSTableView`) enthält die folgenden Methoden für die Arbeit mit Zeilenauswahl:

- `DeselectAll(NSObject)` – Hebt die Auswahl aller Zeilen in der Tabelle. Verwendung `this` für den ersten Parameter zum Senden von in das Objekt ausführen, die Sie auswählen. 
- `DeselectRow(nint)` – Deaktiviert die angegebene Zeile in der Tabelle.
- `SelectAll(NSobject)` : Wählt alle Zeilen in der Tabelle an. Verwendung `this` für den ersten Parameter zum Senden von in das Objekt ausführen, die Sie auswählen.
- `SelectRow(nint,bool)` : Wählt die angegebene Zeile. Übergeben Sie `false` heben Sie die Auswahl für den zweiten Parameter, und wählen Sie nur eine einzelne Zeile, übergeben `true` zum Erweitern der Auswahl, und diese Zeile enthalten.
- `SelectRows(NSIndexSet,bool)` : Wählt die angegebene Gruppe von Zeilen. Übergeben Sie `false` für den zweiten Parameter heben Sie die Auswahl, und wählen Sie nur eine dieser Zeilen übergeben `true` zum Erweitern der Auswahl, und diese Zeilen enthalten.
- `SelectedRow` -Gibt die aktuelle Zeile in der Tabelle ausgewählt.
- `SelectedRows` -Gibt ein `NSIndexSet` , die die Indizes der ausgewählten Zeilen enthält.
- `SelectedRowCount` -Gibt die Anzahl der ausgewählten Zeilen.
- `IsRowSelected(nint)` -Gibt `true` , wenn die angegebene Zeile ausgewählt ist.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ Zeile auswählen

Wenn Sie zulassen möchten, des Benutzers ein Zeichen und geben Sie die ausgewählte Tabellenansicht und wählen Sie die erste Zeile, das Zeichen, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Tabelle anzeigen der **Schnittstellenhierarchie** und überprüfen Sie die **Tasksequenztyps** Kontrollkästchen in der **Attributinspektor**:

[![](table-view-images/type01.png "Festlegen des Auswahltyps")](table-view-images/type01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Jetzt ermöglicht das Bearbeiten der `ProductTableDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
    nint row = 0;
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains(searchString)) return row;

        // Increment row counter
        ++row;
    }

    // If not found select the first row
    return 0;
}
```

Die `GetNextTypeSelectMatch` -Methode übernimmt die angegebenen `searchString` und gibt die Zeile des ersten `Product` , die diese Zeichenfolge ist, in dessen `Title`.

Wenn wir die Anwendung auszuführen, und geben Sie ein Zeichen, ist eine Zeile ausgewählt:

[![](table-view-images/type02.png "Führen Sie eine Beispiel-app")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Neuanordnen von Spalten

Wenn Sie zulassen möchten, ziehen den Benutzer Neuanordnen von Spalten in der Tabellenansicht, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Tabelle anzeigen der **Schnittstellenhierarchie** und überprüfen Sie die **Neuanordnen** Kontrollkästchen in der **Attributinspektor**:

[![](table-view-images/reorder01.png "Attribute Inspector")](table-view-images/reorder01.png#lightbox)

Wenn wir geben Sie einen Wert für die **Autosave** -Eigenschaft, und überprüfen Sie die **Spalteninformationen** Feld wir, um das Layout der Tabelle die treffen Änderungen werden automatisch für uns gespeichert und wiederhergestellt das nächste Mal die Anwendung wird ausgeführt.

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Jetzt ermöglicht das Bearbeiten der `ProductTableDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Die `ShouldReorder` Methode zurückgeben soll `true` für jede Spalte, die sie zulassen möchten, dass sein ziehen neu angeordnet, in der `newColumnIndex`, ansonsten return `false`;

Wenn wir die Anwendung ausführen, können wir Spaltenüberschriften zu ziehen, um unsere Spalten aber leicht neu anzuordnen:

[![](table-view-images/reorder02.png "Ein Beispiel für die neu angeordnete Spalten")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Bearbeiten von Zellen

Wenn Sie zulassen möchten, den Benutzer die Werte für eine bestimmte Zelle bearbeiten, bearbeiten Sie die `ProductTableDelegate.cs` und Ändern der `GetViewForItem` -Methode wie folgt:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = true;

        view.EditingEnded += (sender, e) => {
                    
            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.Tag].Title = view.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.Tag].Description = view.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Wenn wir die Anwendung ausführen, kann der Benutzer jetzt auch die Zellen in der Tabellenansicht bearbeiten:

[![](table-view-images/editing01.png "Ein Beispiel für die Bearbeitung einer Zelle")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Verwendung von Images in Tabellenansichten

Um ein Bild im Rahmen der Zelle im enthalten eine `NSTableView`, müssen Sie ändern, wie die Daten zurückgegeben werden, von der `NSTableViewDelegate's` `GetViewForItem` zu verwendende Methode eine `NSTableCellView` statt der normalen `NSTextField`. Zum Beispiel:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();
        if (tableColumn.Title == "Product") {
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
        } else {
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
        }
        view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
        view.AddSubview (view.TextField);
        view.Identifier = tableColumn.Title;
        view.TextField.BackgroundColor = NSColor.Clear;
        view.TextField.Bordered = false;
        view.TextField.Selectable = false;
        view.TextField.Editable = true;

        view.TextField.EditingEnded += (sender, e) => {

            // Take action based on type
            switch(view.Identifier) {
            case "Product":
                DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
                break;
            case "Details":
                DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
                break; 
            }
        };
    }

    // Tag view
    view.TextField.Tag = row;

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tags.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        break;
    }

    return view;
}
```

Weitere Informationen finden Sie unter den [mithilfe von Bildern mit Tabellenansichten](~/mac/app-fundamentals/image.md) Teil unserer [arbeiten mit Images](~/mac/app-fundamentals/image.md) Dokumentation.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Hinzufügen einer Schaltfläche Löschen einer Zeile

Je nach den Anforderungen Ihrer App möglicherweise vorkommen, Sie eine Schaltfläche "Aktion" für jede Zeile in der Tabelle angeben müssen. Als dieses Beispiel erweitern wir nun die Tabellenansicht Beispiel oben erstellten enthalten eine **löschen** auf jede Zeile auf die Schaltfläche.

Bearbeiten Sie zuerst die `Main.storyboard` in Interface Builder von Xcode, wählen Sie die Ansicht der Tabelle, und erhöhen Sie die Anzahl der Spalten, die drei (3). Ändern Sie anschließend die **Titel** der neuen Spalte, die `Action`:

[![](table-view-images/delete01.png "Bearbeiten den Namen der Spalte")](table-view-images/delete01.png#lightbox)

Speichern Sie die Änderungen auf das Storyboard und zurück zu Visual Studio für Mac, um die Änderungen zu synchronisieren.

Als Nächstes bearbeiten Sie die `ViewController.cs` Datei, und fügen Sie die folgende öffentliche Methode hinzu:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

Ändern Sie in der gleichen Datei, die Erstellung der neuen Tabelle Ansicht Delegat innerhalb des `ViewDidLoad` -Methode wie folgt:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Bearbeiten Sie nun die `ProductTableDelegate.cs` Datei um eine private Verbindung mit dem View Controller einzuschließen und an den Controller als Parameter, die beim Erstellen einer neuen Instanz des Delegaten:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
    this.Controller = controller;
    this.DataSource = datasource;
}
#endregion
```

Als Nächstes fügen Sie die folgende neue private Methode zur Klasse hinzu:

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
    // Add to view
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);

    // Configure
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    // Wireup events
    view.TextField.EditingEnded += (sender, e) => {

        // Take action based on type
        switch (view.Identifier) {
        case "Product":
            DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
            break;
        case "Details":
            DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
            break;
        }
    };

    // Tag view
    view.TextField.Tag = row;
}
```

Dabei werden alle von der Textansicht-Konfigurationen, die zuvor in getan wird der `GetViewForItem` Methode und in einem aufrufbaren Standort (seit die letzte Spalte der Tabelle nicht über eine Textansicht, aber eine Schaltfläche beinhaltet).

Bearbeiten Sie abschließend die `GetViewForItem` Methode und legen Sie ihn wie folgt aussehen:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTableCellView ();

        // Configure the view
        view.Identifier = tableColumn.Title;

        // Take action based on title
        switch (tableColumn.Title) {
        case "Product":
            view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
            view.AddSubview (view.ImageView);
            view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Details":
            view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
            ConfigureTextField (view, row);
            break;
        case "Action":
            // Create new button
            var button = new NSButton (new CGRect (0, 0, 81, 16));
            button.SetButtonType (NSButtonType.MomentaryPushIn);
            button.Title = "Delete";
            button.Tag = row;

            // Wireup events
            button.Activated += (sender, e) => {
                // Get button and product
                var btn = sender as NSButton;
                var product = DataSource.Products [(int)btn.Tag];

                // Configure alert
                var alert = new NSAlert () {
                    AlertStyle = NSAlertStyle.Informational,
                    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
                    MessageText = $"Delete {product.Title}?",
                };
                alert.AddButton ("Cancel");
                alert.AddButton ("Delete");
                alert.BeginSheetForResponse (Controller.View.Window, (result) => {
                    // Should we delete the requested row?
                    if (result == 1001) {
                        // Remove the given row from the dataset
                        DataSource.Products.RemoveAt((int)btn.Tag);
                        Controller.ReloadTable ();
                    }
                });
            };

            // Add to view
            view.AddSubview (button);
            break;
        }

    }

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed ("tag.png");
        view.TextField.StringValue = DataSource.Products [(int)row].Title;
        view.TextField.Tag = row;
        break;
    case "Details":
        view.TextField.StringValue = DataSource.Products [(int)row].Description;
        view.TextField.Tag = row;
        break;
    case "Action":
        foreach (NSView subview in view.Subviews) {
            var btn = subview as NSButton;
            if (btn != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Betrachten wir nun mehrere Abschnitte des Codes im Detail. Erstens, wenn ein neues `NSTableViewCell` ist erstellende Aktion wird durchgeführt wird, auf den Namen der Spalte. Für die ersten beiden Spalten (**Produkt** und **Details**), die neue `ConfigureTextField` Methode wird aufgerufen.

Für die **Aktion** Spalte, wird ein neuer `NSButton` wird erstellt und auf die Zelle als eine untergeordnete Ansicht hinzugefügt:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

Der Schaltfläche `Tag` Eigenschaft wird verwendet, um die Nummer der Zeile zu speichern, die gerade verarbeitet wird. Diese Zahl wird später verwendet werden, wenn der Benutzer eine Zeile in der Schaltfläche gelöscht werden, fordert `Activated` Ereignis:

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btn = sender as NSButton;
    var product = DataSource.Products [(int)btn.Tag];

    // Configure alert
    var alert = new NSAlert () {
        AlertStyle = NSAlertStyle.Informational,
        InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
        MessageText = $"Delete {product.Title}?",
    };
    alert.AddButton ("Cancel");
    alert.AddButton ("Delete");
    alert.BeginSheetForResponse (Controller.View.Window, (result) => {
        // Should we delete the requested row?
        if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
        }
    });
};
```

An den Anfang des ereignishandlers rufen wir die Schaltfläche und das Produkt, das in der angegebenen Tabellenzeile befindet. Anschließend wird eine Warnung für den Benutzer bestätigt das Löschen der Zeile angezeigt. Wenn der Benutzer auswählt, die Zeile zu löschen, entfernt die angegebene Zeile aus der Datenquelle, und in der Tabelle wird erneut geladen:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Zum Schluss die Tabellenansichtszelle wiederverwendet werden wird, anstatt neu erstellt wird, konfiguriert der folgende Code auf Grundlage der Spalte verarbeitet wird:

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
case "Action":
    foreach (NSView subview in view.Subviews) {
        var btn = subview as NSButton;
        if (btn != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

Für die **Aktion** Spalte alle untergeordneten Ansichten werden überprüft, bis die `NSButton` gefunden wird, ist es `Tag` -Eigenschaft aktualisiert, um auf die aktuelle Zeile verweist.

Mit diesen Änderungen vorhanden, wenn die app ausgeführt wird jede Zeile hat ein **löschen** Schaltfläche:

[![](table-view-images/delete02.png "Die Tabellenansicht mit Löschen von Schaltflächen")](table-view-images/delete02.png#lightbox)

Klickt der Benutzer eine **löschen** Schaltfläche, eine Warnung angezeigt, die gebeten, die die angegebene Zeile zu löschen:

[![](table-view-images/delete03.png "Eine Delete-Zeile-Warnung")](table-view-images/delete03.png#lightbox)

Wenn der Benutzer löschen, die Zeile entfernt werden und wird in der Tabelle neu gezeichnet werden:

[![](table-view-images/delete04.png "In der Tabelle nach dem Löschen der zeilenupdates")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Datenansichten Bindung-Tabelle

Techniken für Schlüssel / Wert-Codierung und Binden von Daten in Ihre Xamarin.Mac-Anwendung verwenden, können Sie die Menge des Codes, die Sie schreiben und verwalten, die zum Auffüllen und zum Arbeiten mit UI-Elemente, erheblich reduzieren. Sie haben außerdem den Vorteil, dass weitere entkoppeln Ihre Daten sichern (_Datenmodell_) aus Ihrem Front end-Benutzeroberfläche (_Model-View-Controller_), wodurch einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

Schlüssel-Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf die Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, statt den Zugriff über Instanzvariablen oder Methoden der eigenschaftenzugriffsmethode (`get/set`). Durch die Implementierung von Schlüssel / Wert-Codierung kompatibel Accessors in Ihre Xamarin.Mac-Anwendung, erhalten Sie Zugriff auf andere MacOS-Funktionen wie z. B. das beobachten von Schlüssel-Wert (KVO), Datenbindung, Kerndaten, Cocoa-Bindungen und Scriptability.

Weitere Informationen finden Sie unter den [Tabelle anzeigen einer Datenbindung](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) Teil unserer [Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine ausführliche Übersicht über das Arbeiten mit Tabellenansichten in einer Xamarin.Mac-Anwendung verwendet. Wir haben gesehen, die unterschiedlichen Typen und Tabellenansichten, das Erstellen und verwalten Tabellenansichten in Interface Builder von Xcode und arbeiten Sie mit Tabellenansichten in C#-Code verwendet.

## <a name="related-links"></a>Verwandte Links

- [MacTables (Beispiel)](https://developer.xamarin.com/samples/mac/MacTables/)
- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Gliederungsansichten](~/mac/user-interface/outline-view.md)
- [Quelllisten](~/mac/user-interface/source-list.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [NSTableView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [NSTableViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
