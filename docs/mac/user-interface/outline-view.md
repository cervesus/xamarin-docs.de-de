---
title: Gliedern von Ansichten in xamarin. Mac
description: In diesem Artikel wird das Arbeiten mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Gliederungs Ansichten in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: davidortinau
ms.author: daortin
ms.date: 03/14/2017
ms.openlocfilehash: 7f1ae2ecfa7d6dbed56b8009593fc172615fd051
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86928795"
---
# <a name="outline-views-in-xamarinmac"></a>Gliedern von Ansichten in xamarin. Mac

_In diesem Artikel wird das Arbeiten mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es beschreibt das Erstellen und Verwalten von Gliederungs Ansichten in Xcode und Interface Builder und Programm gesteuertes arbeiten mit Ihnen._

Wenn Sie mit c# und .net in einer xamarin. Mac-Anwendung arbeiten, haben Sie Zugriff auf die gleichen Gliederungs Ansichten, die ein Entwickler in *Ziel-C* und *Xcode* verwendet. Da xamarin. Mac direkt in Xcode integriert ist, können Sie die _Interface Builder_ von Xcode verwenden, um Ihre Gliederungs Ansichten zu erstellen und zu verwalten (oder Sie optional direkt in c#-Code zu erstellen).

Eine Gliederungs Ansicht ist ein Typ von Tabelle, der es dem Benutzer ermöglicht, Zeilen hierarchischer Daten zu erweitern oder zu reduzieren. Wie eine Tabellenansicht zeigt eine Gliederungs Ansicht Daten für eine Reihe verwandter Elemente an, wobei Zeilen die einzelnen Elemente und Spalten darstellen, die die Attribute dieser Elemente darstellen. Im Gegensatz zu einer Tabellenansicht befinden sich Elemente in einer Gliederungs Ansicht nicht in einer flachen Liste, Sie sind in einer Hierarchie angeordnet, wie z. b. Dateien und Ordner auf einer Festplatte.

[![Ein Beispiel für eine APP-Laufzeit](outline-view-images/populate03.png)](outline-view-images/populate03.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung behandelt. Es wird dringend empfohlen, dass Sie zunächst den Artikel [Hello, Mac](~/mac/get-started/hello-mac.md) , insbesondere die [Einführung in Xcode und die Abschnitte zu Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und Outlets und [Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) , durcharbeiten, da er wichtige Konzepte und Techniken behandelt, die wir in diesem Artikel verwenden werden.

Lesen Sie ggf. den Abschnitt verfügbar machen von [c#-Klassen/-Methoden zu "Ziel-c](~/mac/internals/how-it-works.md) " im Dokument " [xamarin. Mac](~/mac/internals/how-it-works.md) ". darin werden die `Register` -und-Befehle erläutert, die `Export` zum Verknüpfen ihrer c#-Klassen mit Ziel-c-Objekten und Benutzeroberflächen Elementen verwendet werden.

<a name="Introduction_to_Outline_Views"></a>

## <a name="introduction-to-outline-views"></a>Einführung in Gliederungs Ansichten

Eine Gliederungs Ansicht ist ein Typ von Tabelle, der es dem Benutzer ermöglicht, Zeilen hierarchischer Daten zu erweitern oder zu reduzieren. Wie eine Tabellenansicht zeigt eine Gliederungs Ansicht Daten für eine Reihe verwandter Elemente an, wobei Zeilen die einzelnen Elemente und Spalten darstellen, die die Attribute dieser Elemente darstellen. Im Gegensatz zu einer Tabellenansicht befinden sich Elemente in einer Gliederungs Ansicht nicht in einer flachen Liste, Sie sind in einer Hierarchie angeordnet, wie z. b. Dateien und Ordner auf einer Festplatte.

Wenn ein Element in einer Gliederungs Ansicht andere Elemente enthält, kann es vom Benutzer erweitert oder reduziert werden. Ein erweiterbares Element zeigt ein Offenlegungs Dreieck an, das nach rechts zeigt, wenn das Element reduziert wird, und zeigt, wenn das Element erweitert wird. Wenn Sie auf das Offenlegungs Dreieck klicken, wird das Element erweitert oder reduziert.

Die `NSOutlineView` Gliederungs Ansicht () ist eine Unterklasse der Tabellenansicht ( `NSTableView` ) und erbt daher viel von Ihrem Verhalten von der übergeordneten Klasse. Infolgedessen werden viele Vorgänge, die von einer Tabellen Sicht unterstützt werden, z. b. das Auswählen von Zeilen oder Spalten, das Neupositionieren von Spalten durchziehen von Spalten Headern usw., auch von einer Gliederungs Ansicht unterstützt. Eine xamarin. Mac-Anwendung verfügt über die Kontrolle über diese Features und kann die Parameter der Gliederungs Ansicht (entweder im Code oder Interface Builder) so konfigurieren, dass bestimmte Vorgänge zugelassen oder verweigert werden.

In einer Gliederungs Ansicht werden die eigenen Daten nicht gespeichert. stattdessen wird eine Datenquelle ( `NSOutlineViewDataSource` ) verwendet, um sowohl die erforderlichen Zeilen als auch die Spalten bereitzustellen.

Das Verhalten einer Gliederungs Ansicht kann angepasst werden, indem eine Unterklasse des Gliederungs Ansichts Delegaten () bereitgestellt wird, um die Gliederungs `NSOutlineViewDelegate` Spalten Verwaltung, den Typ zur Auswahl von Funktionen, die Zeilenauswahl und-Bearbeitung, die benutzerdefinierte Überwachung und benutzerdefinierte Ansichten für einzelne Spalten

Da eine Gliederungs Ansicht viel von Ihrem Verhalten und ihrer Funktionalität mit einer Tabellenansicht gemeinsam nutzt, sollten Sie die Dokumentation der [Tabellen Sichten](~/mac/user-interface/table-view.md) lesen, bevor Sie mit diesem Artikel fortfahren.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode"></a>

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Erstellen und Verwalten von Gliederungs Ansichten in Xcode

Wenn Sie eine neue xamarin. Mac-Cocoa-Anwendung erstellen, erhalten Sie standardmäßig ein Standardmäßiges leeres Standardfenster. Dieses Fenster wird in einer Datei definiert, `.storyboard` die automatisch im Projekt enthalten ist. Um den Windows-Entwurf zu bearbeiten, doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei:

[![Auswählen des Haupt Storyboards](outline-view-images/edit01.png)](outline-view-images/edit01.png#lightbox)

Dadurch wird das Fensterdesign in der Interface Builder von Xcode geöffnet:

[![Bearbeiten der Benutzeroberfläche in Xcode](outline-view-images/edit02.png)](outline-view-images/edit02.png#lightbox)

Geben `outline` Sie in das Suchfeld des **Bibliotheks Inspektors** ein, um die Suche nach den Gliederungs Ansicht-Steuerelementen zu vereinfachen:

[![Auswählen einer Gliederungs Ansicht aus der Bibliothek](outline-view-images/edit03.png)](outline-view-images/edit03.png#lightbox)

Ziehen Sie eine Gliederungs Ansicht auf den Ansichts Controller im Schnittstellen-Editor, füllen Sie den Inhalts Bereich des Ansichts Controllers aus, und legen Sie ihn an der **Stelle**ab, an der er verkleinert wird und mit dem Fenster im Einschränkungs- **Editor**wächst:

[![Bearbeiten der Einschränkungen](outline-view-images/edit04.png)](outline-view-images/edit04.png#lightbox)

Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![Der Attribut Inspektor](outline-view-images/edit05.png)](outline-view-images/edit05.png#lightbox)

- Gliederungs **Spalte** : die Tabellenspalte, in der die hierarchischen Daten angezeigt werden.
- **Autosave** -Gliederungs Spalte: Wenn `true` , wird die Gliederungs Spalte automatisch zwischen Anwendungs Läufen gespeichert und wieder hergestellt.
- **Einzug** : der Betrag für den Einzug von Spalten unter einem erweiterten Element.
- Der **Einzug folgt auf Zellen** : Wenn `true` , wird die Einzugs Markierung zusammen mit den Zellen eingerückt.
- **Erweiterte Elemente automatisch speichern** : Wenn `true` , wird der erweiterte/reduzierte Zustand der Elemente automatisch zwischen Anwendungs Ausführungen gespeichert und wieder hergestellt.
- **Inhalts Modus** : ermöglicht es Ihnen, `NSView` `NSCell` die Daten in den Zeilen und Spalten entweder mithilfe von Sichten () oder Zellen () anzuzeigen. Ab macOS 10,7 sollten Sie Ansichten verwenden.
- Gleit Komma **Gruppen Zeilen** : Wenn `true` , werden in der Tabellenansicht gruppierte Zellen so gezeichnet, als wären Sie unverankert.
- **Columns** : definiert die Anzahl der angezeigten Spalten.
- **Headers** : Wenn `true` , enthalten die Spaltenheader.
- **Neuanordnen** : Wenn `true` , kann der Benutzer die Reihenfolge der Spalten in der Tabelle verschieben.
- Größen **Änderung:** Wenn `true` , kann der Benutzer Spaltenüberschriften ziehen, um die Größe der Spalten zu ändern.
- **Spaltengröße** : steuert die automatische Größenanpassung von Spalten durch die Tabelle.
- **Hervor** Hebung: steuert den Typ der Hervorhebung, die die Tabelle verwendet, wenn eine Zelle ausgewählt wird.
- **Alternative Zeilen** : Wenn `true` , hat eine andere Zeile eine andere Hintergrundfarbe.
- **Horizontales Raster** : wählt den Typ des Rahmens aus, der zwischen Zellen horizontal gezeichnet wird.
- **Vertikales Raster** : wählt den Typ des Rahmens aus, der zwischen Zellen vertikal gezeichnet wird.
- **Raster Farbe** : legt die Farbe für den Zell Rahmen fest.
- **Background** : legt die Hintergrundfarbe der Zelle fest.
- **Auswahl** : Hiermit können Sie steuern, wie der Benutzer Zellen in der Tabelle auswählen kann:
  - **Multiple** -if `true` , der Benutzer kann mehrere Zeilen und Spalten auswählen.
  - **Spalte** : Wenn `true` , kann der Benutzer Spalten auswählen.
  - **Typ SELECT** -if `true` , der Benutzer kann ein Zeichen eingeben, um eine Zeile auszuwählen.
  - **Leer** : Wenn `true` , der Benutzer ist nicht erforderlich, um eine Zeile oder Spalte auszuwählen, kann die Tabelle überhaupt keine Auswahl treffen.
- **Autosave** : der Name, unter dem das Tabellenformat automatisch gespeichert wird.
- **Spalten Informationen** : Wenn `true` , werden die Reihenfolge und Breite der Spalten automatisch gespeichert.
- **Zeilenumbrüche** : Wählen Sie aus, wie die Zelle Zeilenumbrüche behandelt.
- **Abgeschnitten der letzten sichtbaren Zeile** : Wenn `true` , wird die Zelle in den Daten abgeschnitten und kann nicht in die Grenzen passen.

> [!IMPORTANT]
> Wenn Sie keine ältere xamarin. Mac-Anwendung verwalten, sollten Sie auf den basierten `NSView` Tabellen Sichten basierende Gliederungs Ansichten verwenden `NSCell` . `NSCell`wird als Legacy angesehen und wird möglicherweise nicht weiter unterstützt.

Wählen Sie eine Tabellenspalte in der **Schnittstellen Hierarchie** aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![Der Attribut Inspektor](outline-view-images/edit06.png)](outline-view-images/edit06.png#lightbox)

- **Title** : legt den Titel der Spalte fest.
- **Ausrichtung** : Legen Sie die Ausrichtung des Texts innerhalb der Zellen fest.
- **Titel Schriftart** : wählt die Schriftart für den Header Text der Zelle aus.
- **Sortierschlüssel** : der Schlüssel, der zum Sortieren der Daten in der Spalte verwendet wird. Lassen Sie das Feld leer, wenn der Benutzer diese Spalte nicht sortieren kann.
- **Selector** : ist die **Aktion** , die zum Ausführen der Sortierung verwendet wird. Lassen Sie das Feld leer, wenn der Benutzer diese Spalte nicht sortieren kann.
- **Order** : gibt die Sortierreihenfolge für die Spaltendaten an.
- **Ändern der Größe** : wählt den Typ der Größe der Größe für die Spalte aus.
- **Editable** -if `true` , der Benutzer kann Zellen in einer Zellen basierten Tabelle bearbeiten.
- **Hidden** : Wenn `true` , wird die Spalte ausgeblendet.

Sie können auch die Größe der Spalte ändern, indem Sie den Zieh Punkt (vertikal auf der rechten Seite der Spalte) nach links oder rechts ziehen.

Wählen Sie in der Tabellenansicht die einzelnen Spalten aus, und geben Sie der ersten Spalte den **Titel** `Product` und die zweite Spalte `Details` .

Wählen Sie `NSTableViewCell` in der **Schnittstellen Hierarchie** eine Tabellenzellen Ansicht () aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![Der Attribut Inspektor](outline-view-images/edit07.png)](outline-view-images/edit07.png#lightbox)

Dabei handelt es sich um alle Eigenschaften einer Standardansicht. Sie haben auch die Möglichkeit, die Zeilen für diese Spalte hier zu ändern.

Wählen Sie eine Tabellen Ansichts Zelle (standardmäßig eine `NSTextField` ) in der **Schnittstellen Hierarchie** aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![Der Attribut Inspektor](outline-view-images/edit08.png)](outline-view-images/edit08.png#lightbox)

Sie verfügen über alle Eigenschaften eines Standard Textfelds, das hier festgelegt werden soll. Standardmäßig wird ein Standard Textfeld verwendet, um Daten für eine Zelle in einer Spalte anzuzeigen.

Wählen Sie `NSTableFieldCell` in der **Schnittstellen Hierarchie** eine Tabellenzellen Ansicht () aus, und die folgenden Eigenschaften sind im **Attribut Inspektor**verfügbar:

[![Der Attribut Inspektor](outline-view-images/edit09.png)](outline-view-images/edit09.png#lightbox)

Die wichtigsten Einstellungen finden Sie hier:

- **Layout** : Wählen Sie aus, wie Zellen in dieser Spalte angelegt werden.
- **Verwendet den Einzeilenmodus** : Wenn `true` der Wert ist, ist die Zelle auf eine einzelne Zeile beschränkt.
- **Breite des ersten Lauf Zeit Layouts** : Wenn `true` , wird die Zelle die für Sie festgelegte Breite (entweder manuell oder automatisch) bevorzugen, wenn Sie beim ersten Ausführen der Anwendung angezeigt wird.
- **Action** : steuert, wann die Bearbeitungs **Aktion** für die Zelle gesendet wird.
- **Behavior** : definiert, ob eine Zelle auswählbar ist oder bearbeitet werden kann.
- **Rich-Text** : Wenn `true` , kann die Zelle formatierten und formatierten Text anzeigen.
- **Rückgängig** -Wenn `true` , übernimmt die Zelle die Verantwortung für das rückgängig-Verhalten.

Wählen Sie die Tabellenzellen Ansicht ( `NSTableFieldCell` ) unten in einer Tabellenspalte in der **Schnittstellen Hierarchie**aus:

[![Auswählen der Tabellenzellen Ansicht](outline-view-images/edit11.png)](outline-view-images/edit10.png#lightbox)

Dies ermöglicht es Ihnen, die Tabellenzellen Ansicht zu bearbeiten, die als Basis _Muster_ für alle Zellen verwendet wird, die für die jeweilige Spalte erstellt werden.

<a name="Adding_Actions_and_Outlets"></a>

### <a name="adding-actions-and-outlets"></a>Hinzufügen von Aktionen und Outlets

Genau wie jedes andere Cocoa UI-Steuerelement müssen wir unsere Gliederungs Ansicht und ihre Spalten und Zellen in c#-Code mithilfe von **Aktionen** und **Outlets** (basierend auf der erforderlichen Funktionalität) verfügbar machen.

Der Prozess ist für ein beliebiges Gliederungs Ansichts Element identisch, das wir verfügbar machen möchten:

1. Wechseln Sie zum **Assistenten-Editor** , und stellen Sie sicher, dass die `ViewController.h` Datei ausgewählt ist:

    [![Auswählen der richtigen h-Datei](outline-view-images/edit11.png)](outline-view-images/edit11.png#lightbox)
2. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie**aus, und ziehen Sie Sie in die `ViewController.h` Datei.
3. Erstellen Sie ein **Outlet** für die Gliederungs Ansicht mit dem Namen `ProductOutline` :

    [![Konfigurieren eines Outlets](outline-view-images/edit13.png)](outline-view-images/edit13.png#lightbox)
4. Erstellen Sie **Outlets** für die Tabellen Spalten ebenso wie `ProductColumn` und `DetailsColumn` :

    [![Konfigurieren eines Outlets](outline-view-images/edit14.png)](outline-view-images/edit14.png#lightbox)
5. Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Als nächstes schreiben wir den Code, um einige Daten für die Kontur anzuzeigen, wenn die Anwendung ausgeführt wird.

<a name="Populating_the_Table_View"></a>

## <a name="populating-the-outline-view"></a>Auffüllen der Gliederungs Ansicht

Wenn unsere Gliederungs Ansicht in Interface Builder entworfen und über ein **Outlet**verfügbar gemacht wird, müssen Sie als nächstes den c#-Code erstellen, um ihn aufzufüllen.

Zunächst erstellen wir eine neue Klasse, `Product` die die Informationen für die einzelnen Zeilen und Gruppen von unter Produkten enthält. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie neue Datei **Hinzufügen**  >  **... aus.** Wählen Sie **Allgemeine**  >  **leere Klasse**aus, geben Sie `Product` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** :

[![Erstellen einer leeren Klasse](outline-view-images/populate01.png)](outline-view-images/populate01.png#lightbox)

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

Als nächstes müssen wir eine Unterklasse von erstellen `NSOutlineDataSource` , um die Daten für den gewünschten Umriss bereitzustellen. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie neue Datei **Hinzufügen**  >  **... aus.** Wählen Sie **Allgemeine**  >  **leere Klasse**aus, geben Sie `ProductOutlineDataSource` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** .

Bearbeiten `ProductTableDataSource.cs` Sie die Datei, und führen Sie Sie wie folgt aus:

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

Diese Klasse verfügt über Speicher für die Elemente unserer Gliederungs Ansicht und überschreibt die `GetChildrenCount` , um die Anzahl der Zeilen in der Tabelle zurückzugeben. `GetChild`Gibt ein bestimmtes über-oder untergeordnetes Element zurück (wie von der Gliederungs Ansicht angefordert), und `ItemExpandable` definiert das angegebene Element entweder als übergeordnetes Element oder als untergeordnetes Element.

Zum Schluss muss eine Unterklasse von erstellt werden `NSOutlineDelegate` , um das Verhalten für unseren Umriss bereitzustellen. Klicken Sie im **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie neue Datei **Hinzufügen**  >  **... aus.** Wählen Sie **Allgemeine**  >  **leere Klasse**aus, geben Sie `ProductOutlineDelegate` als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** .

Bearbeiten `ProductOutlineDelegate.cs` Sie die Datei, und führen Sie Sie wie folgt aus:

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

Wenn Sie eine Instanz von erstellen `ProductOutlineDelegate` , übergeben wir auch eine Instanz von `ProductOutlineDataSource` , die die Daten für die Kontur bereitstellt. Die- `GetView` Methode ist dafür verantwortlich, eine Ansicht (Daten) zurückzugeben, um die Zelle für eine Spalte und eine Zeile mit dem Wert anzuzeigen. Wenn möglich, wird eine vorhandene Ansicht wieder verwendet, um die Zelle anzuzeigen, wenn keine neue Ansicht erstellt werden muss.

Um die Gliederung aufzufüllen, bearbeiten Sie die `MainWindow.cs` Datei, und führen Sie die `AwakeFromNib` Methode wie folgt aus:

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

[![Die reduzierte Ansicht](outline-view-images/populate02.png)](outline-view-images/populate02.png#lightbox)

Wenn ein Knoten in der Gliederungs Ansicht erweitert wird, sieht er wie folgt aus:

[![Die erweiterte Ansicht](outline-view-images/populate03.png)](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column"></a>

## <a name="sorting-by-column"></a>Sortieren nach Spalte

Lassen Sie den Benutzer das Sortieren der Daten in der Gliederung zu, indem Sie auf eine Spaltenüberschrift klicken. Doppelklicken Sie zunächst auf die `Main.storyboard` Datei, um Sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie die `Product` Spalte aus, geben Sie für den `Title` **Sortierschlüssel** `compare:` für die **Auswahl** ein, und wählen Sie `Ascending` für die **Reihenfolge**aus:

[![Festlegen der Sortierschlüssel Reihenfolge](outline-view-images/sort01.png)](outline-view-images/sort01.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie nun die `ProductOutlineDataSource.cs` Datei, und fügen Sie die folgenden Methoden hinzu:

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

Mit der- `Sort` Methode können wir die Daten in der Datenquelle auf der Grundlage eines angegebenen `Product` Klassen Felds in aufsteigender oder absteigender Reihenfolge sortieren. Die überschriebene- `SortDescriptorsChanged` Methode wird jedes Mal aufgerufen, wenn die Verwendung auf eine Spaltenüberschrift klickt. Der **Schlüssel** Wert, den wir in Interface Builder festgelegt haben, und die Sortierreihenfolge für diese Spalte werden an Sie übermittelt.

Wenn Sie die Anwendung ausführen und in die Spaltenüberschriften klicken, werden die Zeilen nach dieser Spalte sortiert:

[![Beispiel für eine sortierte Ausgabe](outline-view-images/sort02.png)](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection"></a>

## <a name="row-selection"></a>Zeilenauswahl

Wenn Sie dem Benutzer die Auswahl einer einzelnen Zeile gestatten möchten, doppelklicken Sie auf die `Main.storyboard` Datei, um Sie für die Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und deaktivieren Sie das Kontrollkästchen **mehrfach** im **Attribut Inspektor**:

[![Der Attribut Inspektor](outline-view-images/select01.png)](outline-view-images/select01.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie anschließend die `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch kann der Benutzer eine beliebige einzelne Zeile in der Gliederungs Ansicht auswählen. Geben Sie `false` für jedes `ShouldSelectItem` Element zurück, das nicht vom Benutzer ausgewählt werden kann, oder `false` für jedes Element, wenn Sie nicht möchten, dass der Benutzer Elemente auswählen kann.

<a name="Multiple_Row_Selection"></a>

## <a name="multiple-row-selection"></a>Auswahl mehrerer Zeilen

Wenn Sie zulassen möchten, dass der Benutzer mehrere Zeilen auswählt, doppelklicken Sie `Main.storyboard` auf die Datei, um Sie für die Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und aktivieren Sie das Kontrollkästchen **mehrfach** im **Attribut Inspektor**:

[![Der Attribut Inspektor](outline-view-images/select02.png)](outline-view-images/select02.png#lightbox)

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Bearbeiten Sie anschließend die `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch kann der Benutzer eine beliebige einzelne Zeile in der Gliederungs Ansicht auswählen. Geben Sie `false` für jedes `ShouldSelectRow` Element zurück, das nicht vom Benutzer ausgewählt werden kann, oder `false` für jedes Element, wenn Sie nicht möchten, dass der Benutzer Elemente auswählen kann.

<a name="Type_to_Select_Row"></a>

## <a name="type-to-select-row"></a>Zum Auswählen der Zeile eingeben

Wenn Sie es dem Benutzer gestatten möchten, ein Zeichen mit ausgewählter Gliederungs Ansicht einzugeben, und die erste Zeile mit diesem Zeichen auszuwählen, doppelklicken Sie `Main.storyboard` auf die Datei, um Sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und aktivieren Sie das Kontrollkästchen **Type Select** in the **Attribute Inspector**:

[![Bearbeiten des Zeilen Typs](outline-view-images/type01.png)](outline-view-images/type01.png#lightbox)

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

Die `GetNextTypeSelectMatch` -Methode nimmt die angegebene `searchString` an und gibt das Element des ersten zurück `Product` , das diese Zeichenfolge enthält `Title` .

<a name="Reordering_Columns"></a>

## <a name="reordering-columns"></a>Neuordnen von Spalten

Wenn Sie zulassen möchten, dass der Benutzer die Neuanordnen von Spalten in der Gliederungs Ansicht zieht, doppelklicken Sie `Main.storyboard` auf die Datei, um Sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie die Gliederungs Ansicht in der **Schnittstellen Hierarchie** aus, und aktivieren Sie das Kontrollkästchen **Neuanordnen** im **Attribut Inspektor**:

[![Der Attribut Inspektor](outline-view-images/reorder01.png)](outline-view-images/reorder01.png#lightbox)

Wenn wir einen Wert für die Eigenschaft **Autosave** festlegen und das Feld **Spalten Informationen** aktivieren, werden alle Änderungen, die wir am Layout der Tabelle vornehmen, automatisch für uns gespeichert und beim nächsten Ausführen der Anwendung wieder hergestellt.

Speichern Sie die Änderungen, und kehren Sie zu Visual Studio für Mac zurück, um mit Xcode zu synchronisieren.

Nun bearbeiten wir die `ProductOutlineDelegate.cs` Datei und fügen die folgende Methode hinzu:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Die- `ShouldReorder` Methode sollte `true` für jede Spalte zurückgeben, für die das ziehen in das reorderstream erfolgen soll, andernfalls `newColumnIndex` `false` .

Wenn wir die Anwendung ausführen, können wir die Spaltenkopfzeilen verschieben, um die Spalten neu zu sortieren:

[![Beispiel für das Neuordnen von Spalten](outline-view-images/reorder02.png)](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells"></a>

## <a name="editing-cells"></a>Bearbeiten von Zellen

Wenn Sie zulassen möchten, dass der Benutzer die Werte für eine bestimmte Zelle bearbeitet, bearbeiten Sie die `ProductOutlineDelegate.cs` Datei, und ändern Sie die `GetViewForItem` Methode wie folgt:

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

[![Beispiel für das Bearbeiten von Zellen](outline-view-images/editing01.png)](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views"></a>

## <a name="using-images-in-outline-views"></a>Verwenden von Bildern in Gliederungs Ansichten

Wenn Sie ein Bild als Teil der Zelle in einem einschließen möchten `NSOutlineView` , müssen Sie ändern, wie die Daten von der-Methode der Gliederungs Ansicht zurückgegeben werden, `NSTableViewDelegate's` `GetView` um anstelle der typischen zu verwenden `NSTableCellView` `NSTextField` . Beispiel:

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

<a name="Data_Binding_Outline_Views"></a>

## <a name="data-binding-outline-views"></a>Daten Bindungs Gliederungs Sichten

Durch die Verwendung von Schlüssel-Wert-Codierungs-und Daten Bindungs Techniken in ihrer xamarin. Mac-Anwendung können Sie die Menge des Codes, den Sie schreiben und verwalten müssen, erheblich verringern, um Benutzeroberflächen Elemente aufzufüllen und mit Ihnen zu arbeiten. Außerdem profitieren Sie von der weiteren Entkopplung ihrer Sicherungsdaten (_Datenmodell_) von der Front-End-Benutzeroberfläche (_Model-View-Controller_). Dies führt zu einer einfacheren Wartung und einem flexibleren Anwendungs Entwurf.

Key-Value Coding (KVC) ist ein Mechanismus für den indirekten Zugriff auf die Eigenschaften eines Objekts, indem Schlüssel (speziell formatierte Zeichen folgen) verwendet werden, um Eigenschaften zu identifizieren, anstatt über Instanzvariablen oder Zugriffsmethoden () auf Sie zuzugreifen `get/set` . Durch die Implementierung von Schlüssel-Wert-Codierungs kompatiblen Accessoren in ihrer xamarin. Mac-Anwendung erhalten Sie Zugriff auf andere macOS-Features, wie z. b. Key-Value-Beobachtungen (KVO), Datenbindung, Kerndaten, Cocoa-Bindungen und scriptbarkeit.

Weitere Informationen finden Sie im Abschnitt Gliederung der [Datenbindung](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) in unserer Datenbindung und in der Dokumentation zum [Schlüssel-Wert-Code](~/mac/app-fundamentals/databinding.md) .

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde die Arbeit mit Gliederungs Ansichten in einer xamarin. Mac-Anwendung ausführlich erläutert. Wir haben die verschiedenen Typen und Verwendungsmöglichkeiten von Gliederungs Ansichten gesehen, das Erstellen und Verwalten von Gliederungs Ansichten in Xcode-Interface Builder und das Arbeiten mit Gliederungs Ansichten in c#-Code.

## <a name="related-links"></a>Ähnliche Themen

- [Macskizziert (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macoutlines)
- [MacImages (Beispiel)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Hello, Mac (Hallo Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Quelllisten](~/mac/user-interface/source-list.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in Gliederungs Ansichten](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [Nsoutlineview](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [Nsoutlineviewdatasource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [Nsoutlineviewdelegat](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
