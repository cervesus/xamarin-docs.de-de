---
title: Tabellenansichten
description: In diesem Artikel wird das Arbeiten mit Tabellensichten in einer Anwendung Xamarin.Mac behandelt. Erstellen von Tabellensichten in Xcode und Benutzeroberflächen-Generator und interagieren mit diesen im Code beschrieben.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: c274405613f079cb61ad9c96497a9effdc7173f5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="table-views"></a>Tabellenansichten

_In diesem Artikel wird das Arbeiten mit Tabellensichten in einer Anwendung Xamarin.Mac behandelt. Erstellen von Tabellensichten in Xcode und Benutzeroberflächen-Generator und interagieren mit diesen im Code beschrieben._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleiche Tabelle anzeigt, die ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ zu erstellen und Verwalten Ihrer Tabellensichten (oder erstellen sie optional direkt im C#-Code).

Eine Tabellensicht zeigt Daten in einem tabellarischen Format, das eine oder mehrere Spalten mit Informationen aus mehreren Zeilen enthält. Basierend auf den Typ der Tabellenansicht erstellt wird, kann der Benutzer nach Spalte sortieren, neu organisieren von Spalten, Spalten hinzufügen, Entfernen von Spalten oder in der Tabelle enthaltenen Daten bearbeiten.

[![](table-view-images/intro01.png "Beispieltabelle")](table-view-images/intro01.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Tabellensichten in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Einführung in die Tabellenansichten

Eine Tabellensicht zeigt Daten in einem tabellarischen Format, das eine oder mehrere Spalten mit Informationen aus mehreren Zeilen enthält. Tabellensichten in Scroll Ansichten angezeigt werden (`NSScrollView`) und MacOS 10.7 ab, Sie können alle `NSView` anstelle von Zellen (`NSCell`), Zeilen und Spalten angezeigt. Dies bedeutet, dass Sie weiterhin verwenden können `NSCell` jedoch, Sie müssen in der Regel Unterklasse `NSTableCellView` und Erstellen von benutzerdefinierten Zeilen und Spalten.

Eine Tabellensicht seinem eigenen Daten nicht gespeichert, er verlässt sich stattdessen auf einer Datenquelle (`NSTableViewDataSource`), die Zeilen und Spalten, die erforderlich sind, klicken Sie auf einen Bedarf bereitzustellen.

Verhalten einer Tabellenansicht kann angepasst werden, durch die Bereitstellung einer Unterklasse des Delegaten Tabelle anzeigen (`NSTableViewDelegate`) um die Verwaltung der Spalte zu unterstützen, geben Sie zum Auswählen von Funktionen, Zeilenauswahl und Bearbeiten von benutzerdefinierten Nachverfolgungsdatensatzes und benutzerdefinierte Ansichten für einzelne Spalten und Zeilen.

Beim Erstellen von Tabellensichten schlägt Apple Folgendes vor:

* Ermöglicht dem Benutzer, die Tabelle sortieren, indem Sie auf einen Spaltenheader.
* Erstellen Sie die Spaltenüberschriften, die-Nomen verwenden oder kurze nominale Ausdrücke, die beschreiben, die Daten in dieser Spalte angezeigt wird.

Weitere Informationen finden Sie unter der [Content Ansichten](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) Abschnitt der Apple [OS X-Richtlinien für menschliche Benutzeroberfläche](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Erstellen und Verwalten von Tabellensichten in Xcode

Wenn Sie eine neue Xamarin.Mac Kakao-Anwendung erstellen, erhalten Sie Standardfensters leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` automatisch im Projekt enthaltene Datei. So bearbeiten Sie die Windows-Design in der **Projektmappen-Explorer**, doppelklicken klicken Sie auf die `Main.storyboard` Datei:

[![](table-view-images/edit01.png "Die Haupt-Storyboard auswählen")](table-view-images/edit01.png#lightbox)

Dies öffnet das Fenster Design in Xcodes Benutzeroberflächen-Generator:

[![](table-view-images/edit02.png "Bearbeiten die Benutzeroberfläche in Xcode")](table-view-images/edit02.png#lightbox)

Typ `table` in der **Bibliothek Inspektors** Suchfeld der Tabellenansicht Steuerelemente erleichtern:

[![](table-view-images/edit03.png "Wählen eine Tabellensicht aus der Bibliothek")](table-view-images/edit03.png#lightbox)

Ziehen Sie eine Tabellensicht auf die View-Controller in der **Benutzeroberflächen-Editors**, unbedingt Ausfüllen des Inhaltsbereichs des View-Controller, und legen Sie dafür, wo es verkleinert und vergrößert wird, mit dem Fenster in der **Einschränkung Editor**:

[![](table-view-images/edit04.png "Bearbeiten von Einschränkungen")](table-view-images/edit04.png#lightbox)

Wählen Sie in der Tabellenansicht der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](table-view-images/edit05.png "Die Attribut-Inspektor")](table-view-images/edit05.png#lightbox)

- **Inhalts-Modus** -können Sie mit beiden Ansichten (`NSView`) oder Zellen (`NSCell`) zum Anzeigen der Daten in den Zeilen und Spalten. Mit MacOS 10.7 beginnen, sollten Sie die Sichten verwenden.
- **Gleitet Gruppenzeilen** – Wenn `true`, Tabellenansicht wird gruppierte Zellen gezeichnet, als ob sie unverankert sind.
- **Spalten** -definiert die Anzahl der angezeigten Spalten.
- **Header** – Wenn `true`, die Spalten müssen Header.
- **Neuanordnen von** – Wenn `true`, der Benutzer wird in der Lage, ziehen die Spalten in der Tabelle neu anordnen.
- **Ändern der Größe von** – Wenn `true`, der Benutzer wird in der Lage, Spaltenheader, um die Größe der Spalten ziehen.
- **Größenanpassung von Spalten** -steuert, wie die Tabelle Spalten automatisch wird.
- **Markieren Sie** -Steuerelemente, die der Typ des Hervorhebung der Tabelle verwendet wird, wenn eine Zelle ausgewählt ist.
- **Alternative Zeilen** – Wenn `true`, jemals andere Zeile hat eine andere Hintergrundfarbe.
- **Horizontale Raster** -wählt die Art des Rahmens, der zwischen den Zellen horizontal gezeichnet.
- **Vertikale Raster** -wählt die Art des Rahmens, der zwischen den Zellen vertikal gezeichnet.
- **Rasterfarbe** -legt die Rahmenfarbe der Zelle.
- **Hintergrund** -legt die Hintergrundfarbe der Zelle fest.
- **Auswahl** -können Sie steuern, wie der Benutzer die Zellen in der Tabelle als auswählen kann:
    - **Mehrere** – Wenn `true`, der Benutzer kann mehrere Zeilen und Spalten auswählen.
    - **Spalte** – Wenn `true`, der Benutzer kann Spalten auswählen.
    - **Geben Sie Select** – Wenn `true`, die Benutzer kann ein Zeichen, das Auswählen einer Zeile eingeben.
    - **Leere** – Wenn `true`, der Benutzer ist nicht erforderlich, um eine Zeile oder Spalte auszuwählen, die Tabelle kann für keine Auswahl getroffen wurde überhaupt.
- **AutoSpeichern** -der Name, das das Format Tabellen automatisch unter zu speichern.
- **Spalteninformationen** – Wenn `true`, Reihenfolge und Breite der Spalten werden automatisch gespeichert.
- **Zeilenumbrüche** – aus, wie die Zelle Zeilenumbrüche behandelt.
- **Schneidet die letzte sichtbare Zeile** – Wenn `true`, wird die Zelle in die Daten können nicht innerhalb der Begrenzung passt abgeschnitten.

> [!IMPORTANT]
> Wenn Sie eine ältere Xamarin.Mac-Anwendung verwaltet werden `NSView` basierend Tabellensichten sollte verwendet werden, über `NSCell` Tabellensichten Basis. `NSCell` ältere gilt und möglicherweise nicht zukünftig unterstützt werden.

Wählen Sie eine Tabellenspalte in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](table-view-images/edit06.png "Die Attribut-Inspektor")](table-view-images/edit06.png#lightbox)

- **Titel** -definiert den Titel der Spalte.
- **Ausrichtung** -legen Sie die Ausrichtung des Texts innerhalb der Zellen.
- **Titel der Schriftart** -wählt die Schriftart für die Zelle Headertext aus.
- **Schlüssel sortieren** -ist der Schlüssel, um Daten in der Spalte zu sortieren. Lassen Sie leer, wenn der Benutzer in dieser Spalte sortieren kann.
- **Selektor** -ist der **Aktion** verwendet, um die sortiert werden. Lassen Sie leer, wenn der Benutzer in dieser Spalte sortieren kann.
- **Reihenfolge** -wird die Sortierreihenfolge für die Spaltendaten.
- **Ändern der Größe von** -wählt die Art der größenanpassung für die Spalte.
- **Bearbeitbare** – Wenn `true`, der Benutzer die Zellen in einer basierend Zellentabelle bearbeiten kann.
- **Ausgeblendete** – Wenn `true`, die Spalte ausgeblendet ist.

Sie können auch die Größe der Spalte ändern, durch dessen Handle (vertikal zentriert rechts neben der Spalte) nach links oder rechts ziehen.

Wir wählen Sie die einzelnen Spalten in unserer Tabelle aus, und geben Sie die erste Spalte einer **Titel** von `Product` und der zweite `Details`.

Wählen Sie eine Zelle Tabellensicht (`NSTableViewCell`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](table-view-images/edit07.png "Die Attribut-Inspektor")](table-view-images/edit07.png#lightbox)

Hierbei handelt es sich um alle Eigenschaften einer Standardsicht. Sie haben auch die Möglichkeit zum Ändern der Größe der Zeilen für diese Spalte hier.

Wählen Sie eine Zelle der Tabelle anzeigen (Standardmäßig ist dies ein `NSTextField`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](table-view-images/edit08.png "Die Attribut-Inspektor")](table-view-images/edit08.png#lightbox)

Sie müssen alle Eigenschaften eines Felds Standardtext hier festlegen. Standardmäßig wird ein standard-Text-Feld verwendet, Daten für eine Zelle in einer Spalte angezeigt.

Wählen Sie eine Zelle Tabellensicht (`NSTableFieldCell`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](table-view-images/edit09.png "Die Attribut-Inspektor")](table-view-images/edit09.png#lightbox)

Im folgenden die wichtigsten Einstellungen sind:

- **Layout** – aus, wie die Zellen in dieser Spalte angeordnet sind.
- **Verwendet den Modus für einzelne Zeile** – Wenn `true`, die Zelle zu einer einzelnen Zeile beschränkt ist.
- **Erste Runtime Layoutbreite** – Wenn `true`, bevorzugt die Zelle die Breite festgelegt (entweder manuell oder automatisch) beim ersten Mal die Anwendung ausgeführt wird angezeigt.
- **Aktion** -steuert, wann die Bearbeitung **Aktion** wird für die Zelle gesendet.
- **Verhalten** -definiert werden, wenn eine Zelle ausgewählt oder bearbeitbar ist.
- **Rich-Text** – Wenn `true`, die Zelle kann formatiert und formatierten Text anzeigen.
- **Rückgängig machen** – Wenn `true`, die Zelle übernimmt die Verantwortung für ihn ist Verhalten rückgängig zu machen.

Wählen Sie die Zelle Tabellenansicht (`NSTableFieldCell`) am unteren Rand einer Tabellenspalte, in der **Schnittstellenhierarchie**:

[![](table-view-images/edit10.png "Auswählen der Zelle Tabellenansicht")](table-view-images/edit10.png#lightbox)

Dadurch können Sie so bearbeiten Sie die Zelle Tabellenansicht als Basis verwendet _Muster_ für alle Zellen, die für die angegebene Spalte erstellt.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Hinzufügen von Aktionen und Ausgänge

Nur wie jedes andere Kakao UI-Steuerelement, müssen wir unsere Tabellenansicht verfügbar zu machen und wird für Spalten und C#-Code mit Zellen **Aktionen** und **Steckdosen** (basierend auf die erforderliche Funktionalität).

Der Prozess ist für jedes Tabellenansicht-Element, das wir verfügbar machen möchten:

1. Wechseln Sie zu der **Assistant Editor** und sicherstellen, dass die `ViewController.h` Datei ausgewählt ist: 

    [![](table-view-images/edit11.png "Der Assistent-Editor")](table-view-images/edit11.png#lightbox)
2. Wählen Sie aus der Tabellenansicht der **Schnittstellenhierarchie**, Steuerelement klicken und ziehen Sie in der `ViewController.h` Datei.
3. Erstellen einer **Nachrichtenplattform** für Tabellenansicht aufgerufen `ProductTable`: 

    [![](table-view-images/edit13.png "Konfigurieren von einer Steckdose")](table-view-images/edit13.png#lightbox)
4. Erstellen Sie **Steckdosen** für die Spalten der Tabellen ebenfalls aufgerufen `ProductColumn` und `DetailsColumn`: 

    [![](table-view-images/edit14.png "Konfigurieren von einer Steckdose")](table-view-images/edit14.png#lightbox)
5. Speichern Sie die Änderungen und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Als Nächstes wird der Anzeige von Code schreiben einige Daten für die Tabelle, wenn die Anwendung ausgeführt wird.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Auffüllen der Tabellenansicht

Mit unserer Tabellenansicht vorgesehen, in der Benutzeroberflächen-Generator und über verfügbar gemachte ein **Nachrichtenplattform**, als Nächstes müssen wir erstellen den C#-Code, um es zu füllen.

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

Wir als Nächstes erstellen Sie eine Unterklasse von `NSTableDataSource` unserer Tabelle die Daten bereit, wie angefordert. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductTableDataSource` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

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

Diese Klasse wurde von Speicher für unsere Tabellenansicht Elemente und überschreibt die `GetRowCount` die Anzahl der Zeilen in der Tabelle zurückgegeben.

Schließlich müssen, erstellen Sie eine Unterklasse von `NSTableDelegate` um das Verhalten für unsere Tabelle bereitzustellen. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductTableDelegate` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

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

Wenn wir erstellen eine Instanz von der `ProductTableDelegate`, wir auch in einer Instanz von übergeben der `ProductTableDataSource` , die die Daten für die Tabelle bereitstellt. Die `GetViewForItem` Methode ist verantwortlich für die Rückgabe einer Ansicht (Daten), um die Zelle für einen bestimmten Spalte und Zeile anzuzeigen. Wenn möglich, eine vorhandene Sicht wird wiederverwendet werden, um die Zelle anzuzeigen, wenn keine neue Ansicht erstellt werden muss.

Um die Tabelle zu füllen, ermöglicht das Bearbeiten der `ViewController.cs` Datei, und stellen die `AwakeFromNib` Methode aussehen wie folgt:

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

[![](table-view-images/populate02.png "Eine Beispiel-app ausführen")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Nach Spalte sortieren

Wir können Benutzer die Daten in der Tabelle durch Klicken auf eine Spaltenüberschrift sortieren. Doppelklicken Sie zuerst auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie die `Product` Spalte Geben Sie `Title` für die **Sortierschlüssel**, `compare:` für die **Selektor** , und wählen Sie `Ascending` für die **Reihenfolge**:

[![](table-view-images/sort01.png "Der Sortierschlüssel festlegen")](table-view-images/sort01.png#lightbox)

Wählen Sie die `Details` Spalte Geben Sie `Description` für die **Sortierschlüssel**, `compare:` für die **Selektor** , und wählen Sie `Ascending` für die **Reihenfolge**:

[![](table-view-images/sort02.png "Der Sortierschlüssel festlegen")](table-view-images/sort02.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

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

Die `Sort` Methode ermöglichen es uns zum Sortieren der Daten in der Datenquelle basierend auf einer bestimmten `Product` Klassenfeld in aufsteigender oder absteigender Reihenfolge. Die überschriebene `SortDescriptorsChanged` Methode wird aufgerufen, jedes Mal, wenn die Verwendung auf eine Spaltenüberschrift klickt. Sie übergeben die **Schlüssel** wir im Schnittstelle-Generator und die Sortierreihenfolge für diese Spalte festgelegte Wert.

Wenn wir führen Sie die Anwendung, und klicken Sie in den Spaltenüberschriften, werden die Zeilen nach dieser Spalte sortiert werden:

[![](table-view-images/sort03.png "Eine Beispiel-app ausführen")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zeilenauswahl

Wenn Sie möchten zulassen, dass die Benutzer wählen Sie eine einzelne Zeile aus, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Tabellenansicht der **Schnittstellenhierarchie** und deaktivieren Sie die **mehrere** Kontrollkästchen in der **Attribut Inspektor**:

[![](table-view-images/select01.png "Die Attribut-Inspektor")](table-view-images/select01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.


Als Nächstes Bearbeiten der `ProductTableDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Dadurch kann den Benutzer eine einzelne Zeile in der Tabellenansicht auswählen. Zurückgeben `false` für die `ShouldSelectRow` für jede, die Zeile nicht die Benutzer können ausgewählt werden soll oder `false` für jede Zeile, wenn Sie nicht, dass die Benutzer können alle Zeilen ausgewählt werden möchten.

Die Tabellenansicht (`NSTableView`) enthält die folgenden Methoden zum Arbeiten mit Zeilenauswahl:

- `DeselectRow(nint)` -Deaktiviert die angegebene Zeile in der Tabelle.
- `SelectRow(nint,bool)` -Die angegebene Zeile ausgewählt. Übergeben Sie `false` für den zweiten Parameter, jeweils nur eine Zeile auswählen.
- `SelectedRow` -Gibt die aktuelle Zeile in der Tabelle ausgewählt.
- `IsRowSelected(nint)` -Gibt `true` , wenn die angegebene Zeile ausgewählt ist.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Mehrere Zeilenauswahl

Wenn Sie zulassen möchten, den Benutzer eine mehrere Zeilen auswählen, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Tabellenansicht der **Schnittstellenhierarchie** und überprüfen Sie die **mehrere** Kontrollkästchen in der **Attribut Inspektor**:

[![](table-view-images/select02.png "Die Attribut-Inspektor")](table-view-images/select02.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.


Als Nächstes Bearbeiten der `ProductTableDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
    return true;
}
```

Dadurch kann den Benutzer eine einzelne Zeile in der Tabellenansicht auswählen. Zurückgeben `false` für die `ShouldSelectRow` für jede, die Zeile nicht die Benutzer können ausgewählt werden soll oder `false` für jede Zeile, wenn Sie nicht, dass die Benutzer können alle Zeilen ausgewählt werden möchten.

Die Tabellenansicht (`NSTableView`) enthält die folgenden Methoden zum Arbeiten mit Zeilenauswahl:

- `DeselectAll(NSObject)` – Hebt die Auswahl aller Zeilen in der Tabelle. Verwendung `this` für den ersten Parameter in dem Objekt, das auf diese Weise die Auswahl zu senden. 
- `DeselectRow(nint)` -Deaktiviert die angegebene Zeile in der Tabelle.
- `SelectAll(NSobject)` -Wählt alle Zeilen in der Tabelle an. Verwendung `this` für den ersten Parameter in dem Objekt, das auf diese Weise die Auswahl zu senden.
- `SelectRow(nint,bool)` -Die angegebene Zeile ausgewählt. Übergeben Sie `false` für den zweiten Parameter heben Sie die Auswahl, und wählen Sie nur eine einzelne Zeile, übergeben `true` zum Erweitern der Auswahl, und diese Zeile enthalten.
- `SelectRows(NSIndexSet,bool)` – Wählt aus den angegebenen Satz von Zeilen. Übergeben `false` für den zweiten Parameter, heben Sie die Auswahl, und wählen Sie nur eine dieser Zeilen übergeben `true` zum Erweitern der Auswahl, und diese Zeilen enthalten.
- `SelectedRow` -Gibt die aktuelle Zeile in der Tabelle ausgewählt.
- `SelectedRows` -Gibt ein `NSIndexSet` , die die Indizes der ausgewählten Zeilen enthält.
- `SelectedRowCount` -Gibt die Anzahl der ausgewählten Zeilen zurück.
- `IsRowSelected(nint)` -Gibt `true` , wenn die angegebene Zeile ausgewählt ist.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ, um die Zeile auszuwählen.

Wenn Sie möchten ermöglicht dem Benutzer ein Zeichentyp mit der Tabellenansicht ausgewählt, und wählen Sie die erste Zeile, der das angegebenen Zeichen hat, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Tabellenansicht der **Schnittstellenhierarchie** und überprüfen Sie die **Typ auswählen** Kontrollkästchen in der **Attribut Inspektor**:

[![](table-view-images/type01.png "Die Auswahl Einstellungstyp")](table-view-images/type01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

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

Die `GetNextTypeSelectMatch` -Methode übernimmt die angegebenen `searchString` und gibt die Zeile der ersten `Product` hat, die diese Zeichenfolge in der `Title`.

Wenn wir führen Sie die Anwendung, und geben Sie ein Zeichen, ist eine Zeile ausgewählt:

[![](table-view-images/type02.png "Eine Beispiel-app ausführen")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Neuanordnen von Spalten

Wenn Sie den Benutzer, ziehen zulassen möchten, Neuanordnen von Spalten in der Tabellenansicht, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Tabellenansicht der **Schnittstellenhierarchie** und überprüfen Sie die **Reordering** Kontrollkästchen in der **Attribut Inspektor**:

[![](table-view-images/reorder01.png "Die Attribut-Inspektor")](table-view-images/reorder01.png#lightbox)

Wenn wir geben Sie einen Wert für die **AutoSpeichern** -Eigenschaft, und Überprüfen der **Spalteninformationen** Feld, alle Änderungen, die wir zum Layout der Tabelle ändern, werden automatisch für uns gespeichert und wiederhergestellt das nächste Mal die Anwendung wird ausgeführt.

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Jetzt ermöglicht das Bearbeiten der `ProductTableDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Die `ShouldReorder` -Methode zurückgeben sollte `true` für jede Spalte, die es ermöglichen, werden möchten ziehen neu angeordnet, in der `newColumnIndex`, ansonsten return `false`;

Wenn wir die Anwendung ausführen, können wir Spaltenüberschriften, um unsere Spalten neu anordnen ziehen:

[![](table-view-images/reorder02.png "Ein Beispiel für die neu angeordneten Spalten")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Bearbeiten von Zellen

Wenn Sie zulassen, dass der Benutzer die Werte für eine bestimmte Zelle bearbeiten, bearbeiten möchten die `ProductTableDelegate.cs` Datei und ändern Sie die `GetViewForItem` Methode wie folgt:

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

Wenn wir die Anwendung ausführen, kann der Benutzer jetzt die Zellen in der Tabellenansicht bearbeiten:

[![](table-view-images/editing01.png "Ein Beispiel für die Bearbeitung einer Zelle")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Verwenden von Bildern in Tabellensichten

Um ein Bild als Teil der Zelle enthalten eine `NSTableView`, müssen Sie dagegen ändern, wie die Daten von der Tabellenansicht zurückgegeben werden `NSTableViewDelegate's` `GetViewForItem` zu verwendende Methode eine `NSTableCellView` anstelle der normalen `NSTextField`. Zum Beispiel:

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

Weitere Informationen finden Sie unter der [mithilfe von Bildern mit Tabellensichten](~/mac/app-fundamentals/image.md) Teil unserer [arbeiten mit Images](~/mac/app-fundamentals/image.md) Dokumentation.

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Hinzufügen einer Schaltfläche "löschen" an einer Zeile

Basierend auf den Anforderungen der app, es gibt möglicherweise Situationen, in denen Sie auf die Aktionsschaltfläche für jede Zeile in der Tabelle angeben müssen. Als ein Beispiel dafür, schauen wir uns die einzuschließende oben erstellten Tabelle anzeigen eines Beispiels eine **löschen** auf jede Zeile auf die Schaltfläche.

Bearbeiten Sie zuerst die `Main.storyboard` in Xcodes Benutzeroberflächen-Generator, wählen Sie die Ansicht der Tabelle, und erhöhen Sie die Anzahl der Spalten, die drei (3). Als Nächstes ändern Sie die **Titel** der neuen Spalte auf `Action`:

[![](table-view-images/delete01.png "Bearbeiten den Namen der Spalte")](table-view-images/delete01.png#lightbox)

Speichern Sie die Änderungen auf das Storyboard und zurück zu Visual Studio für Mac Änderungen synchronisiert.

Als Nächstes Bearbeiten der `ViewController.cs` Datei, und fügen Sie die folgende öffentliche Methode hinzu:

```csharp
public void ReloadTable ()
{
    ProductTable.ReloadData ();
}
```

Ändern Sie in der gleichen Datei, die Erstellung der neuen Tabelle Ansicht Delegaten innerhalb eines `ViewDidLoad` Methode wie folgt:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Bearbeiten Sie nun die `ProductTableDelegate.cs` Datei um eine private Verbindung mit der View-Controller einzuschließen und an den Controller als Parameter, die beim Erstellen einer neuen Instanz des Delegaten:

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

Als Nächstes fügen Sie die folgenden neuen private Methode der Klasse hinzu:

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

Dies führt der Textansicht Konfigurationen, die zuvor in durchgeführt wurden die `GetViewForItem` Methode und speichert sie an einem einzelnen aufgerufen werden kann (da die letzte Spalte der Tabelle nicht angezeigt, aber eine Schaltfläche enthält).

Bearbeiten Sie schließlich die `GetViewForItem` Methode, und stellen sie wie folgt aussehen:

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
                var btw = sender as NSButton;
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
            var btw = subview as NSButton;
            if (btw != null) {
                btn.Tag = row;
            }
        }
        break;
    }

    return view;
}
```

Sehen wir uns auf mehrere Abschnitte dieses Codes im Detail. Erste, wenn ein neuer `NSTableViewCell` ist erstellende Aktion erfolgt basierend auf den Namen der Spalte. Für die ersten beiden Spalten (**Produkt** und **Details**), die neue `ConfigureTextField` -Methode aufgerufen wird.

Für die **Aktion** Spalte wird eine neue `NSButton` erstellt und der Zelle als Sub Sicht hinzugefügt wird:

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

Der Schaltfläche `Tag` Eigenschaft wird verwendet, um die Nummer der Zeile aufzunehmen, die gerade verarbeitet wird. Diese Zahl wird später verwendet werden, wenn der Benutzer eine Zeile in der Schaltfläche gelöscht anfordert `Activated` Ereignis:

```csharp
// Wireup events
button.Activated += (sender, e) => {
    // Get button and product
    var btw = sender as NSButton;
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

Zu Beginn des ereignishandlers erhalten wir die Schaltfläche und das Produkt, das in der angegebenen Tabellenzeile befindet. Anschließend wird eine Warnung für den Benutzer bestätigen das Löschen der Zeile dargestellt wird. Wenn der Benutzer entscheidet, die Zeile zu löschen, wird die angegebene Zeile aus der Datenquelle entfernt und die Tabelle wird erneut geladen:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Schließlich Ansicht Tabellenzelle wiederverwendet werden wird, anstatt neu erstellt wird, konfiguriert der folgende Code basierend auf der Spalte verarbeitet wird:

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
        var btw = subview as NSButton;
        if (btw != null) {
            btn.Tag = row;
        }
    }
    break;
}

```

Für die **Aktion** Spalte alle Sub-Ansichten werden gescannt, bis die `NSButton` gefunden wird, ist es `Tag` -Eigenschaft aktualisiert, um in der aktuellen Zeile zeigen.

Mit diesen Änderungen vorhanden, wenn die Anwendung ausgeführt wird jede Zeile haben eine **löschen** Schaltfläche:

[![](table-view-images/delete02.png "Die Tabellenansicht mit den Schaltflächen löschen")](table-view-images/delete02.png#lightbox)

Wenn der Benutzer klickt ein **löschen** Schaltfläche, eine Warnung wird angezeigt, und sie gebeten, die angegebene Zeile zu löschen:

[![](table-view-images/delete03.png "Eine Delete-Zeile-Warnung")](table-view-images/delete03.png#lightbox)

Bei Einmaliger Betätigung löschen die Zeile entfernt werden und wird in der Tabelle neu gezeichnet werden:

[![](table-view-images/delete04.png "Die Tabelle, nachdem die Zeile gelöscht wird")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Datenansichten Bindung-Tabelle

Mithilfe von Techniken von Schlüssel-Wert zu codieren, und Binden von Daten in Ihrer Anwendung Xamarin.Mac, können Sie den Umfang des Codes, die Sie schreiben und verwalten, die zum Auffüllen von und Arbeiten mit UI-Elemente, erheblich verringern. Sie haben außerdem den Vorteil, dass weitere Entkopplung Ihre Daten sichern (_Datenmodell_) von der Vorderseite enden Benutzeroberfläche (_Model-View-Controller_), was zu einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

Schlüssel-Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, anstatt über Nachrichteninstanzvariablen auf Sie zuzugreifen oder Methoden des Eigenschaftenaccessors (`get/set`). Durch die Implementierung von Schlüssel-Wert-Codierung kompatibel Accessoren in Ihrer Anwendung Xamarin.Mac können haben Sie Zugriff auf andere MacOS-Funktionen, z. B. beobachten von Schlüssel-Wert (KVO), zum Binden von Daten, Core Daten, Kakao Bindungen und Scriptability.

Weitere Informationen finden Sie unter der [Tabelle anzeigen einer Datenbindung](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) Teil unserer [zum Binden von Daten und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Tabellensichten in einer Anwendung Xamarin.Mac übernommen. Wir gesehen haben die verschiedenen Typen und Tabellensichten, zum Erstellen und Verwalten von Tabellensichten in Xcodes Benutzeroberflächen-Generator und zum Arbeiten mit Tabellensichten in C#-Code verwendet.

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
