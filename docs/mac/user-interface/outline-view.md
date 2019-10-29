---
title: Gliedern von Ansichten in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Gliederungs Ansichten in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: dd710f13d9324ce31e08641f214241e0433fe809
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73004345"
---
# <a name="outline-views-in-xamarinmac"></a>Gliedern von Ansichten in xamarin. Mac

_In diesem Artikel wird das Arbeiten mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Gliederungs Ansichten in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen._

Wenn Sie mit C# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf die gleichen Gliederungs Ansichten, die von einem Entwickler in " *Ziel-C* " und *Xcode* verwendet werden. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um Ihre Gliederungs Ansichten zu erstellen und zu verwalten C# (oder Sie optional direkt im Code zu erstellen).

Eine Gliederungs Ansicht ist ein Typ von Tabelle, der es dem Benutzer ermöglicht, Zeilen hierarchischer Daten zu erweitern oder zu reduzieren. Wie eine Tabellenansicht zeigt eine Gliederungs Ansicht Daten für eine Reihe verwandter Elemente an, wobei Zeilen die einzelnen Elemente und Spalten darstellen, die die Attribute dieser Elemente darstellen. Im Gegensatz zu einer Tabellenansicht befinden sich Elemente in einer Gliederungs Ansicht nicht in einer flachen Liste, Sie sind in einer Hierarchie angeordnet, wie z. b. Dateien und Ordner auf einer Festplatte.

[![](outline-view-images/populate03.png "An example app run")](outline-view-images/populate03.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , verwenden, da er wichtige Konzepte und Techniken behandelt, die wir in verwenden werden. Dieser Artikel.

Sie können sich auch den Abschnitt verfügbar machen von [ C# Klassen/Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) " ansehen. darin werden die`Register`und`Export`Befehle erläutert, die zum Verknüpfen der C# Klassen mit "Ziel-c" verwendet werden. Objekte und UI-Elemente.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Einführung in Gliederungs Ansichten

Eine Gliederungs Ansicht ist ein Typ von Tabelle, der es dem Benutzer ermöglicht, Zeilen hierarchischer Daten zu erweitern oder zu reduzieren. Wie eine Tabellenansicht zeigt eine Gliederungs Ansicht Daten für eine Reihe verwandter Elemente an, wobei Zeilen die einzelnen Elemente und Spalten darstellen, die die Attribute dieser Elemente darstellen. Im Gegensatz zu einer Tabellenansicht befinden sich Elemente in einer Gliederungs Ansicht nicht in einer flachen Liste, Sie sind in einer Hierarchie angeordnet, wie z. b. Dateien und Ordner auf einer Festplatte.

Wenn ein Element in einer Gliederungs Ansicht andere Elemente enthält, kann es vom Benutzer erweitert oder reduziert werden. Ein erweiterbares Element zeigt ein Offenlegungs Dreieck an, das nach rechts zeigt, wenn das Element reduziert wird, und zeigt, wenn das Element erweitert wird. Wenn Sie auf das Offenlegungs Dreieck klicken, wird das Element erweitert oder reduziert.

Die Gliederungs Ansicht (`NSOutlineView`) ist eine Unterklasse der Tabellenansicht (`NSTableView`) und erbt daher einen Großteil ihres Verhaltens von der übergeordneten Klasse. Infolgedessen werden viele Vorgänge, die von einer Tabellen Sicht unterstützt werden, z. b. das Auswählen von Zeilen oder Spalten, das Neupositionieren von Spalten durchziehen von Spalten Headern usw., auch von einer Gliederungs Ansicht unterstützt. Eine xamarin. Mac-Anwendung verfügt über die Kontrolle über diese Features und kann die Parameter der Gliederungs Ansicht (entweder im Code oder Interface Builder) so konfigurieren, dass bestimmte Vorgänge zugelassen oder verweigert werden.

In einer Gliederungs Ansicht werden die eigenen Daten nicht gespeichert. stattdessen wird eine Datenquelle (`NSOutlineViewDataSource`) verwendet, um sowohl die erforderlichen Zeilen als auch die Spalten bereitzustellen.

Das Verhalten einer Gliederungs Ansicht kann angepasst werden, indem eine Unterklasse des Gliederungs Ansichts Delegaten (`NSOutlineViewDelegate`) bereitgestellt wird, um die Gliederung der Spalten Verwaltung, den Typ der Auswahl, die Zeilenauswahl und die Bearbeitung, die benutzerdefinierte Nachverfolgung und benutzerdefinierte Ansichten und Zeilen.

Da eine Gliederungs Ansicht viel von Ihrem Verhalten und ihrer Funktionalität mit einer Tabellenansicht gemeinsam nutzt, sollten Sie die Dokumentation der [Tabellen Sichten](~/mac/user-interface/table-view.md) lesen, bevor Sie mit diesem Artikel fortfahren.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Erstellen und Verwalten von Gliederungs Ansichten in Xcode

Wenn Sie eine neue xamarin. Mac-Cocoa-Anwendung erstellen, erhalten Sie standardmäßig ein Standardmäßiges leeres Standardfenster. Dieses Fenster wird in einer `.storyboard` Datei definiert, die automatisch im Projekt enthalten ist. Um den Windows-Entwurf zu bearbeiten, doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei:

[![](outline-view-images/edit01.png "Selecting the main storyboard")](outline-view-images/edit01.png#lightbox)

Dadurch wird das Fensterdesign in der Interface Builder von Xcode geöffnet:

[![](outline-view-images/edit02.png "Editing the UI in Xcode")](outline-view-images/edit02.png#lightbox)

Geben Sie `outline` in das Suchfeld des **Bibliotheks Inspektors** ein, um die Suche nach den Gliederungs Ansicht-Steuerelementen zu vereinfachen:

[![](outline-view-images/edit03.png "Selecting an Outline View from the Library")](outline-view-images/edit03.png#lightbox)

Ziehen Sie eine Gliederungs Ansicht auf den Ansichts Controller im Schnittstellen-Editor, füllen Sie den Inhalts Bereich des Ansichts Controllers aus, und legen Sie ihn an der **Stelle**ab, an der er verkleinert wird und mit dem Fenster im Einschränkungs- **Editor**wächst:

[![](outline-view-images/edit04.png "Editing the constraints")](outline-view-images/edit04.png#lightbox)

Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![](outline-view-images/edit05.png "The Attribute Inspector")](outline-view-images/edit05.png#lightbox)

- Gliederungs **Spalte** : die Tabellenspalte, in der die hierarchischen Daten angezeigt werden.
- **Spalte für die automatische Speicherung** von Gliederung: Wenn `true`, wird die Gliederungs Spalte automatisch zwischen Anwendungs Ausführungen gespeichert und wieder hergestellt.
- **Einzug** : der Betrag für den Einzug von Spalten unter einem erweiterten Element.
- **Einzug folgt Zellen** : Wenn `true`, wird die Einzugs Markierung zusammen mit den Zellen eingerückt.
- **Erweiterte Elemente automatisch speichern** : Wenn `true`, wird der erweiterte/reduzierte Zustand der Elemente automatisch zwischen Anwendungs Ausführungen gespeichert und wieder hergestellt.
- **Inhalts Modus** : ermöglicht die Verwendung von Sichten (`NSView`) oder Zellen (`NSCell`), um die Daten in den Zeilen und Spalten anzuzeigen. Ab macOS 10,7 sollten Sie Ansichten verwenden.
- Gleit Komma **Gruppen Zeilen** : Wenn `true`, zeichnet die Tabellenansicht gruppierte Zellen so, als wären Sie unverankert.
- **Columns** : definiert die Anzahl der angezeigten Spalten.
- **Headers** : Wenn `true`, enthalten die Spaltenheader.
- **Neuanordnen** : Wenn `true`, kann der Benutzer die Reihenfolge der Spalten in der Tabelle verschieben.
- Größenänderung: Wenn `true`, kann der Benutzer **Spaltenüberschriften** ziehen, um die Größe der Spalten zu ändern.
- **Spaltengröße** : steuert die automatische Größenanpassung von Spalten durch die Tabelle.
- **Hervor** Hebung: steuert den Typ der Hervorhebung, die die Tabelle verwendet, wenn eine Zelle ausgewählt wird.
- **Alternative Zeilen** : Wenn `true`, hat eine beliebige andere Zeile eine andere Hintergrundfarbe.
- **Horizontales Raster** : wählt den Typ des Rahmens aus, der zwischen Zellen horizontal gezeichnet wird.
- **Vertikales Raster** : wählt den Typ des Rahmens aus, der zwischen Zellen vertikal gezeichnet wird.
- **Raster Farbe** : legt die Farbe für den Zell Rahmen fest.
- **Background** : legt die Hintergrundfarbe der Zelle fest.
- **Auswahl** : Hiermit können Sie steuern, wie der Benutzer Zellen in der Tabelle auswählen kann:
  - **Mehrfach** : Wenn `true`, kann der Benutzer mehrere Zeilen und Spalten auswählen.
  - **Spalte** : Wenn `true`, kann der Benutzer Spalten auswählen.
  - **Typ SELECT** -wenn `true`, kann der Benutzer ein Zeichen eingeben, um eine Zeile auszuwählen.
  - **Leer** : Wenn `true`, ist es nicht erforderlich, dass der Benutzer eine Zeile oder Spalte auswählt, da die Tabelle überhaupt keine Auswahl zulässt.
- **Autosave** : der Name, unter dem das Tabellenformat automatisch gespeichert wird.
- **Spalten Informationen** : Wenn `true`, werden die Reihenfolge und Breite der Spalten automatisch gespeichert.
- **Zeilenumbrüche** : Wählen Sie aus, wie die Zelle Zeilenumbrüche behandelt.
- Kürzt die **letzte sichtbare Zeile** : Wenn `true`, wird die Zelle abgeschnitten, wenn die Daten nicht innerhalb der Grenzen passen.

> [!IMPORTANT]
> Wenn Sie keine ältere xamarin. Mac-Anwendung verwalten, sollten Sie `NSView` basierten Gliederungs Ansichten in `NSCell` basierten Tabellen Sichten verwenden. `NSCell` gilt als Legacy und wird möglicherweise nicht weiter unterstützt.

Wählen Sie eine Tabellenspalte in der **Schnittstellen Hierarchie** aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![](outline-view-images/edit06.png "The Attribute Inspector")](outline-view-images/edit06.png#lightbox)

- **Title** : legt den Titel der Spalte fest.
- **Ausrichtung** : Legen Sie die Ausrichtung des Texts innerhalb der Zellen fest.
- **Titel Schriftart** : wählt die Schriftart für den Header Text der Zelle aus.
- **Sortierschlüssel** : der Schlüssel, der zum Sortieren der Daten in der Spalte verwendet wird. Lassen Sie das Feld leer, wenn der Benutzer diese Spalte nicht sortieren kann.
- **Selector** : ist die **Aktion** , die zum Ausführen der Sortierung verwendet wird. Lassen Sie das Feld leer, wenn der Benutzer diese Spalte nicht sortieren kann.
- **Order** : gibt die Sortierreihenfolge für die Spaltendaten an.
- **Ändern der Größe** : wählt den Typ der Größe der Größe für die Spalte aus.
- **Bearbeitbar** : Wenn `true`, kann der Benutzer Zellen in einer Zellen basierten Tabelle bearbeiten.
- **Hidden** : Wenn `true`, wird die Spalte ausgeblendet.

Sie können auch die Größe der Spalte ändern, indem Sie den Zieh Punkt (vertikal auf der rechten Seite der Spalte) nach links oder rechts ziehen.

Wählen Sie in der Tabellenansicht die einzelnen Spalten aus, und geben Sie der ersten Spalte einen **Titel** `Product` und der zweite `Details`.

Wählen Sie in der **Schnittstellen Hierarchie** eine Tabellenzellen Ansicht (`NSTableViewCell`) aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![](outline-view-images/edit07.png "The Attribute Inspector")](outline-view-images/edit07.png#lightbox)

Dabei handelt es sich um alle Eigenschaften einer Standardansicht. Sie haben auch die Möglichkeit, die Zeilen für diese Spalte hier zu ändern.

Wählen Sie eine Tabellen Ansichts Zelle (standardmäßig eine `NSTextField`) in der **Schnittstellen Hierarchie** aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![](outline-view-images/edit08.png "The Attribute Inspector")](outline-view-images/edit08.png#lightbox)

Sie verfügen über alle Eigenschaften eines Standard Textfelds, das hier festgelegt werden soll. Standardmäßig wird ein Standard Textfeld verwendet, um Daten für eine Zelle in einer Spalte anzuzeigen.

Wählen Sie in der **Schnittstellen Hierarchie** eine Tabellenzellen Ansicht (`NSTableFieldCell`) aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![](outline-view-images/edit09.png "The Attribute Inspector")](outline-view-images/edit09.png#lightbox)

Die wichtigsten Einstellungen finden Sie hier:

- **Layout** : Wählen Sie aus, wie Zellen in dieser Spalte angelegt werden.
- **Verwendet den Einzeilenmodus** -wenn `true`, ist die Zelle auf eine einzelne Zeile beschränkt.
- **Breite des ersten Lauf Zeit Layouts** : Wenn `true`, wird die Zelle für die Zelle (entweder manuell oder automatisch) bevorzugt, wenn Sie beim ersten Ausführen der Anwendung angezeigt wird.
- **Action** : steuert, wann die Bearbeitungs **Aktion** für die Zelle gesendet wird.
- **Behavior** : definiert, ob eine Zelle auswählbar ist oder bearbeitet werden kann.
- **Rich-Text** : Wenn `true`, kann die Zelle formatierten und formatierten Text anzeigen.
- **Rückgängig** : Wenn `true`, übernimmt die Zelle die Verantwortung für das rückgängig-Verhalten.

Wählen Sie die Tabellenzellen Ansicht (`NSTableFieldCell`) unten in einer Tabellenspalte in der **Schnittstellen Hierarchie**aus:

[![](outline-view-images/edit11.png "Selecting the table cell view")](outline-view-images/edit10.png#lightbox)

Dies ermöglicht es Ihnen, die Tabellenzellen Ansicht zu bearbeiten, die als Basis _Muster_ für alle Zellen verwendet wird, die für die jeweilige Spalte erstellt werden.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Hinzufügen von Aktionen und Outlets

Genau wie jedes andere Cocoa UI-Steuerelement müssen wir unsere Gliederungs Ansicht und ihre Spalten und Zellen für C# Code mithilfe von **Aktionen** und **Outlets** (basierend auf der erforderlichen Funktionalität) verfügbar machen.

Der Prozess ist für ein beliebiges Gliederungs Ansichts Element identisch, das wir verfügbar machen möchten:

1. Wechseln Sie zum **Assistenten-Editor** , und stellen Sie sicher, dass die Datei `ViewController.h` ausgewählt ist:

    [![](outline-view-images/edit11.png "Selecting the correct .h file")](outline-view-images/edit11.png#lightbox)
2. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie**aus, und ziehen Sie Sie in die `ViewController.h` Datei.
3. Erstellen Sie ein **Outlet** für die Gliederungs Ansicht namens `ProductOutline`:

    [![](outline-view-images/edit13.png "Configuring an Outlet")](outline-view-images/edit13.png#lightbox)
4. Erstellen Sie **Outlets** für die Tabellen Spalten, die auch `ProductColumn` und `DetailsColumn`genannt werden:

    [![](outline-view-images/edit14.png "Configuring an Outlet")](outline-view-images/edit14.png#lightbox)
5. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Als nächstes schreiben wir den Code, um einige Daten für die Kontur anzuzeigen, wenn die Anwendung ausgeführt wird.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Auffüllen der Gliederungs Ansicht

Wenn unsere Gliederungs Ansicht in Interface Builder entworfen und über ein **Outlet**verfügbar gemacht wird, müssen Sie C# als nächstes den Code erstellen, um ihn aufzufüllen.

Zunächst erstellen wir eine neue `Product`-Klasse, die die Informationen für die einzelnen Zeilen und Gruppen von unter Produkten enthält. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **neue Datei** **Hinzufügen** ... aus. Wählen Sie **Allgemein** > **leere Klasse**aus, geben Sie `Product` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** :

[![](outline-view-images/populate01.png "Creating an empty class")](outline-view-images/populate01.png#lightbox)

Erstellen Sie die `Product.cs` Datei wie folgt:

```csharp
using System;
using Foundation;
using System.Collections.Generic;

namespace MacOutlines
{
    public class Product : NSObject
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Computed Properties
        public string Title { get; set;} = "";
        public string Description { get; set;} = "";
        public bool IsProductGroup {
            get { return (Products.Count > 0); }
        }
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

Als nächstes müssen wir eine Unterklasse von `NSOutlineDataSource` erstellen, um die Daten für unsere Kontur anzugeben, wie Sie angefordert werden. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **neue Datei** **Hinzufügen** ... aus. Wählen Sie **Allgemein** > **leere Klasse**aus, geben Sie `ProductOutlineDataSource` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** .

Bearbeiten Sie die Datei `ProductTableDataSource.cs`, und führen Sie Sie wie folgt aus:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDataSource : NSOutlineViewDataSource
    {
        #region Public Variables
        public List<Product> Products = new List<Product>();
        #endregion

        #region Constructors
        public ProductOutlineDataSource ()
        {
        }
        #endregion

        #region Override Methods
        public override nint GetChildrenCount (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products.Count;
            } else {
                return ((Product)item).Products.Count;
            }

        }

        public override NSObject GetChild (NSOutlineView outlineView, nint childIndex, NSObject item)
        {
            if (item == null) {
                return Products [childIndex];
            } else {
                return ((Product)item).Products [childIndex];
            }

        }

        public override bool ItemExpandable (NSOutlineView outlineView, NSObject item)
        {
            if (item == null) {
                return Products [0].IsProductGroup;
            } else {
                return ((Product)item).IsProductGroup;
            }

        }
        #endregion
    }
}
```

Diese Klasse verfügt über Speicher für die Elemente unserer Gliederungs Ansicht und überschreibt die `GetChildrenCount`, um die Anzahl der Zeilen in der Tabelle zurückzugeben. Der `GetChild` gibt ein bestimmtes über-oder untergeordnetes Element zurück (wie von der Gliederungs Ansicht angefordert), und der `ItemExpandable` definiert das angegebene Element entweder als übergeordnetes Element oder als untergeordnetes Element.

Zum Schluss müssen wir eine Unterklasse von `NSOutlineDelegate` erstellen, um das Verhalten für unsere Gliederung bereitzustellen. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie > **neue Datei** **Hinzufügen** ... aus. Wählen Sie **Allgemein** > **leere Klasse**aus, geben Sie `ProductOutlineDelegate` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** .

Bearbeiten Sie die Datei `ProductOutlineDelegate.cs`, und führen Sie Sie wie folgt aus:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacOutlines
{
    public class ProductOutlineDelegate : NSOutlineViewDelegate
    {
        #region Constants
        private const string CellIdentifier = "ProdCell";
        #endregion

        #region Private Variables
        private ProductOutlineDataSource DataSource;
        #endregion

        #region Constructors
        public ProductOutlineDelegate (ProductOutlineDataSource datasource)
        {
            this.DataSource = datasource;
        }
        #endregion

        #region Override Methods

        public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
            // This pattern allows you reuse existing views when they are no-longer in use.
            // If the returned view is null, you instance up a new view
            // If a non-null view is returned, you modify it enough to reflect the new data
            NSTextField view = (NSTextField)outlineView.MakeView (CellIdentifier, this);
            if (view == null) {
                view = new NSTextField ();
                view.Identifier = CellIdentifier;
                view.BackgroundColor = NSColor.Clear;
                view.Bordered = false;
                view.Selectable = false;
                view.Editable = false;
            }

            // Cast item
            var product = item as Product;

            // Setup view based on the column selected
            switch (tableColumn.Title) {
            case "Product":
                view.StringValue = product.Title;
                break;
            case "Details":
                view.StringValue = product.Description;
                break;
            }

            return view;
        }
        #endregion
    }
}
```

Wenn wir eine Instanz des `ProductOutlineDelegate`erstellen, übergeben wir auch eine Instanz des `ProductOutlineDataSource`, die die Daten für die Kontur bereitstellt. Die `GetView`-Methode ist dafür verantwortlich, eine Ansicht (Daten) zurückzugeben, um die Zelle für eine Spalte und eine Zeile mit dem Wert anzuzeigen. Wenn möglich, wird eine vorhandene Ansicht wieder verwendet, um die Zelle anzuzeigen, wenn keine neue Ansicht erstellt werden muss.

Um die Gliederung aufzufüllen, müssen Sie die `MainWindow.cs` Datei bearbeiten und die `AwakeFromNib`-Methode wie folgt aussehen:

```csharp
public override void AwakeFromNib ()
{
    base.AwakeFromNib ();

    // Create data source and populate
    var DataSource = new ProductOutlineDataSource ();

    var Vegetables = new Product ("Vegetables", "Greens and Other Produce");
    Vegetables.Products.Add (new Product ("Cabbage", "Brassica oleracea - Leaves, axillary buds, stems, flowerheads"));
    Vegetables.Products.Add (new Product ("Turnip", "Brassica rapa - Tubers, leaves"));
    Vegetables.Products.Add (new Product ("Radish", "Raphanus sativus - Roots, leaves, seed pods, seed oil, sprouting"));
    Vegetables.Products.Add (new Product ("Carrot", "Daucus carota - Root tubers"));
    DataSource.Products.Add (Vegetables);

    var Fruits = new Product ("Fruits", "Fruit is a part of a flowering plant that derives from specific tissues of the flower");
    Fruits.Products.Add (new Product ("Grape", "True Berry"));
    Fruits.Products.Add (new Product ("Cucumber", "Pepo"));
    Fruits.Products.Add (new Product ("Orange", "Hesperidium"));
    Fruits.Products.Add (new Product ("Blackberry", "Aggregate fruit"));
    DataSource.Products.Add (Fruits);

    var Meats = new Product ("Meats", "Lean Cuts");
    Meats.Products.Add (new Product ("Beef", "Cow"));
    Meats.Products.Add (new Product ("Pork", "Pig"));
    Meats.Products.Add (new Product ("Veal", "Young Cow"));
    DataSource.Products.Add (Meats);

    // Populate the outline
    ProductOutline.DataSource = DataSource;
    ProductOutline.Delegate = new ProductOutlineDelegate (DataSource);

}
```

Wenn die Anwendung ausgeführt wird, wird Folgendes angezeigt:

[![](outline-view-images/populate02.png "The collapsed view")](outline-view-images/populate02.png#lightbox)

Wenn ein Knoten in der Gliederungs Ansicht erweitert wird, sieht er wie folgt aus:

[![](outline-view-images/populate03.png "The expanded view")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sortieren nach Spalte

Lassen Sie den Benutzer das Sortieren der Daten in der Gliederung zu, indem Sie auf eine Spaltenüberschrift klicken. Doppelklicken Sie zunächst auf die `Main.storyboard` Datei, um Sie für die Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Spalte `Product` aus, geben Sie `Title` für den **Sortierschlüssel**ein `compare:` für die **Auswahl** , und wählen Sie `Ascending` für die **Reihenfolge**aus:

[![](outline-view-images/sort01.png "Setting the sort key order")](outline-view-images/sort01.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Nun bearbeiten wir die `ProductOutlineDataSource.cs` Datei und fügen die folgenden Methoden hinzu:

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
    }
}

public override void SortDescriptorsChanged (NSOutlineView outlineView, NSSortDescriptor[] oldDescriptors)
{
    // Sort the data
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
    outlineView.ReloadData ();
}
```

Mit der `Sort`-Methode können wir die Daten in der Datenquelle auf der Grundlage eines bestimmten `Product` Klassen Felds in aufsteigender oder absteigender Reihenfolge sortieren. Die überschriebene `SortDescriptorsChanged` Methode wird jedes Mal aufgerufen, wenn die Verwendung auf eine Spaltenüberschrift klickt. Der **Schlüssel** Wert, den wir in Interface Builder festgelegt haben, und die Sortierreihenfolge für diese Spalte werden an Sie übermittelt.

Wenn Sie die Anwendung ausführen und in die Spaltenüberschriften klicken, werden die Zeilen nach dieser Spalte sortiert:

[![](outline-view-images/sort02.png "Example of sorted output")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zeilenauswahl

Wenn Sie dem Benutzer die Auswahl einer einzelnen Zeile gestatten möchten, doppelklicken Sie auf die `Main.storyboard` Datei, um Sie für die Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und deaktivieren Sie das Kontrollkästchen **mehrfach** im **Attribut Inspektor**:

[![](outline-view-images/select01.png "The Attribute Inspector")](outline-view-images/select01.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie als nächstes die Datei `ProductOutlineDelegate.cs`, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch kann der Benutzer eine beliebige einzelne Zeile in der Gliederungs Ansicht auswählen. Geben Sie `false` für die `ShouldSelectItem` für jedes Element zurück, das nicht vom Benutzer ausgewählt oder für jedes Element `false` werden soll, wenn der Benutzer nicht in der Lage sein soll, Elemente auszuwählen.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Auswahl mehrerer Zeilen

Wenn Sie zulassen möchten, dass der Benutzer mehrere Zeilen auswählt, doppelklicken Sie auf die `Main.storyboard` Datei, um Sie für die Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und aktivieren Sie das Kontrollkästchen **mehrfach** im **Attribut Inspektor**:

[![](outline-view-images/select02.png "The Attribute Inspector")](outline-view-images/select02.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie als nächstes die Datei `ProductOutlineDelegate.cs`, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch kann der Benutzer eine beliebige einzelne Zeile in der Gliederungs Ansicht auswählen. Geben Sie `false` für die `ShouldSelectRow` für jedes Element zurück, das nicht vom Benutzer ausgewählt oder für jedes Element `false` werden soll, wenn der Benutzer nicht in der Lage sein soll, Elemente auszuwählen.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Zum Auswählen der Zeile eingeben

Wenn Sie zulassen möchten, dass der Benutzer ein Zeichen mit ausgewählter Gliederungs Ansicht eingibt, und dann die erste Zeile mit diesem Zeichen auswählen, doppelklicken Sie auf die `Main.storyboard` Datei, um Sie für die Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und aktivieren Sie das Kontrollkästchen **Type Select** in the **Attribute Inspector**:

[![](outline-view-images/type01.png "Editing the row type")](outline-view-images/type01.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Nun bearbeiten wir die `ProductOutlineDelegate.cs` Datei und fügen die folgende Methode hinzu:

```csharp
public override NSObject GetNextTypeSelectMatch (NSOutlineView outlineView, NSObject startItem, NSObject endItem, string searchString)
{
    foreach(Product product in DataSource.Products) {
        if (product.Title.Contains (searchString)) {
            return product;
        }
    }

    // Not found
    return null;
}
```

Die `GetNextTypeSelectMatch`-Methode nimmt die angegebene `searchString` an und gibt das Element des ersten `Product` zurück, das diese Zeichenfolge enthält `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Neuordnen von Spalten

Wenn Sie zulassen möchten, dass der Benutzer die Neuanordnen von Spalten in der Gliederungs Ansicht zieht, doppelklicken Sie auf die `Main.storyboard` Datei, um Sie für die Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und aktivieren Sie das Kontrollkästchen **Neuanordnen** im **Attribut Inspektor**:

[![](outline-view-images/reorder01.png "The Attribute Inspector")](outline-view-images/reorder01.png#lightbox)

Wenn wir einen Wert für die Eigenschaft **Autosave** festlegen und das Feld **Spalten Informationen** aktivieren, werden alle Änderungen, die wir am Layout der Tabelle vornehmen, automatisch für uns gespeichert und beim nächsten Ausführen der Anwendung wieder hergestellt.

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Nun bearbeiten wir die `ProductOutlineDelegate.cs` Datei und fügen die folgende Methode hinzu:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Die `ShouldReorder`-Methode sollte `true` für jede Spalte zurückgeben, die in die `newColumnIndex`gezogen werden soll. andernfalls wird `false`zurückgegeben.

Wenn wir die Anwendung ausführen, können wir die Spaltenkopfzeilen verschieben, um die Spalten neu zu sortieren:

[![](outline-view-images/reorder02.png "Example of reordering columns")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Bearbeiten von Zellen

Wenn Sie zulassen möchten, dass der Benutzer die Werte für eine bestimmte Zelle bearbeitet, bearbeiten Sie die Datei `ProductOutlineDelegate.cs`, und ändern Sie die `GetViewForItem`-Methode wie folgt:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTextField view = (NSTextField)outlineView.MakeView (tableColumn.Title, this);
    if (view == null) {
        view = new NSTextField ();
        view.Identifier = tableColumn.Title;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.StringValue;
            break;
        case "Details":
            prod.Description = view.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.StringValue = product.Title;
        break;
    case "Details":
        view.StringValue = product.Description;
        break;
    }

    return view;
}
```

Wenn nun die Anwendung ausgeführt wird, kann der Benutzer die Zellen in der Tabellenansicht bearbeiten:

[![](outline-view-images/editing01.png "An example of editing cells")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Verwenden von Bildern in Gliederungs Ansichten

Wenn Sie ein Bild als Teil der Zelle in einem `NSOutlineView` einschließen möchten, müssen Sie ändern, wie die Daten von der `NSTableViewDelegate's` `GetView`-Methode der Gliederungs Ansicht zurückgegeben werden, um anstelle der typischen `NSTextField` einen `NSTableCellView` zu verwenden. Beispiel:

```csharp
public override NSView GetView (NSOutlineView outlineView, NSTableColumn tableColumn, NSObject item) {
    // Cast item
    var product = item as Product;

    // This pattern allows you reuse existing views when they are no-longer in use.
    // If the returned view is null, you instance up a new view
    // If a non-null view is returned, you modify it enough to reflect the new data
    NSTableCellView view = (NSTableCellView)outlineView.MakeView (tableColumn.Title, this);
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
        view.TextField.Editable = !product.IsProductGroup;
    }

    // Tag view
    view.TextField.Tag = outlineView.RowForItem (item);

    // Allow for edit
    view.TextField.EditingEnded += (sender, e) => {

        // Grab product
        var prod = outlineView.ItemAtRow(view.Tag) as Product;

        // Take action based on type
        switch(view.Identifier) {
        case "Product":
            prod.Title = view.TextField.StringValue;
            break;
        case "Details":
            prod.Description = view.TextField.StringValue;
            break;
        }
    };

    // Setup view based on the column selected
    switch (tableColumn.Title) {
    case "Product":
        view.ImageView.Image = NSImage.ImageNamed (product.IsProductGroup ? "tags.png" : "tag.png");
        view.TextField.StringValue = product.Title;
        break;
    case "Details":
        view.TextField.StringValue = product.Description;
        break;
    }

    return view;
}
```

Weitere Informationen finden Sie im Abschnitt [Verwenden von Bildern mit](~/mac/app-fundamentals/image.md) Gliederungs Ansichten in unserer Dokumentation zum [Arbeiten mit](~/mac/app-fundamentals/image.md) Images.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Daten Bindungs Gliederungs Sichten

Durch die Verwendung von Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken in ihrer xamarin. Mac-Anwendung können Sie die Menge des Codes, den Sie schreiben und verwalten müssen, erheblich verringern, um Benutzeroberflächen Elemente aufzufüllen und mit Ihnen zu arbeiten. Außerdem profitieren Sie von der weiteren Entkopplung ihrer Sicherungsdaten (_Datenmodell_) von der Front-End-Benutzeroberfläche (_Model-View-Controller_). Dies führt zu einer einfacheren Wartung und einem flexibleren Anwendungs Entwurf.

Key-Value Coding (KVC) ist ein Mechanismus für den indirekten Zugriff auf die Eigenschaften eines Objekts, indem Schlüssel (speziell formatierte Zeichen folgen) verwendet werden, um Eigenschaften zu identifizieren, anstatt über Instanzvariablen oder Accessormethoden (`get/set`) auf Sie zuzugreifen. Durch die Implementierung von Schlüssel-Wert-Codierungs kompatiblen Accessoren in ihrer xamarin. Mac-Anwendung erhalten Sie Zugriff auf andere macOS-Features, wie z. b. Key-Value-Beobachtungen (KVO), Datenbindung, Kerndaten, Cocoa-Bindungen und scriptbarkeit.

Weitere Informationen finden Sie im Abschnitt Gliederung der [Datenbindung](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) in unserer Datenbindung und in der Dokumentation zum [Schlüssel-Wert-Code](~/mac/app-fundamentals/databinding.md) .

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung ausführlich erläutert. Wir haben die verschiedenen Typen und Verwendungsmöglichkeiten von Gliederungs Ansichten gesehen, das Erstellen und Verwalten von Gliederungs Ansichten in Xcode-Interface Builder und das C# arbeiten mit Gliederungs Ansichten im Code.

## <a name="related-links"></a>Verwandte Links

- [Macskizziert (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [MacImages (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Quelllisten](~/mac/user-interface/source-list.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Gliederungs Ansichten](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [Nsoutlineview](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [Nsoutlineviewdatasource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [Nsoutlineviewdelegat](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
