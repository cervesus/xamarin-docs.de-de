---
title: Gliederungsansichten in Xamarin.Mac
description: Dieser Artikel behandelt die Arbeit mit Gliederungsansichten in einer Xamarin.Mac-Anwendung. Es wird beschrieben, erstellen und verwalten Gliederungsansichten in Xcode und Interface Builder und Programmgesteuertes Arbeiten mit ihnen.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7228a22eeae3dfdb354ba3693c277251fd2cd821
ms.sourcegitcommit: 47709db4d115d221e97f18bc8111c95723f6cb9b
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/13/2018
ms.locfileid: "40251059"
---
# <a name="outline-views-in-xamarinmac"></a>Gliederungsansichten in Xamarin.Mac

_Dieser Artikel behandelt die Arbeit mit Gliederungsansichten in einer Xamarin.Mac-Anwendung. Es wird beschrieben, erstellen und verwalten Gliederungsansichten in Xcode und Interface Builder und Programmgesteuertes Arbeiten mit ihnen._

Bei der Arbeit mit c# und .NET in einer Xamarin.Mac-Anwendung haben Sie Zugriff auf die gleichen Gliederung anzeigt, die ein Entwickler *Objective-C-* und *Xcode* ist. Da Xamarin.Mac direkt in Xcode integriert ist, können Sie von Xcode _Interface Builder_ erstellen und verwalten Ihre Gliederungsansichten (oder erstellen sie optional direkt in c#-Code).

Eine Gliederungsansicht wird einer Tabelle, die dem Benutzer ermöglicht erweitern oder Reduzieren von Zeilen von hierarchischen Daten. Wie eine Tabellenansicht zeigt eine Gliederung-Ansicht Daten für einen Satz verwandter Elemente, Zeilen stellen dabei einzelne Elemente und Spalten, die die Attribute dieser Elemente darstellt. Im Gegensatz zu einer Tabellenansicht Elemente in einer Gliederungsansicht nicht in einer flachen Liste, deren Organisation in einer Hierarchie, wie Dateien und Ordner auf einer Festplatte.

[![](outline-view-images/populate03.png "Eine Beispiel-app-Ausführung")](outline-view-images/populate03.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Gliederungsansichten in einer Xamarin.Mac-Anwendung beschrieben. Es wird dringend empfohlen, dass Sie über arbeiten die [Hallo, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) und [Outlets und Aktionen](~/mac/get-started/hello-mac.md#outlets-and-actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die wir in diesem Artikel verwenden.

Empfiehlt es sich um einen Blick auf die [Verfügbarmachen von c#-Klassen / Methoden mit Objective-C](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac-Interna](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um Ihre Klassen in c# für Objective-C-Objekte und Elemente der Benutzeroberfläche anschließen.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Einführung in die Ansichten beschrieben.

Eine Gliederungsansicht wird einer Tabelle, die dem Benutzer ermöglicht erweitern oder Reduzieren von Zeilen von hierarchischen Daten. Wie eine Tabellenansicht zeigt eine Gliederung-Ansicht Daten für einen Satz verwandter Elemente, Zeilen stellen dabei einzelne Elemente und Spalten, die die Attribute dieser Elemente darstellt. Im Gegensatz zu einer Tabellenansicht Elemente in einer Gliederungsansicht nicht in einer flachen Liste, deren Organisation in einer Hierarchie, wie Dateien und Ordner auf einer Festplatte.

Wenn ein Element in einer Gliederungsansicht andere Elemente enthält, kann er erweitert oder reduziert, die vom Benutzer sein. Ein Element erweiterbar zeigt ein Dreieck Offenlegung, die auf der rechten Seite verweist, wenn das Element reduziert, und zeigt nach unten, wenn das Element erweitert wird. Durch Klicken auf das Dreieck Offenlegung bewirkt, dass das Element erweitert oder reduziert.

Gliederungsansicht (`NSOutlineView`) ist eine Unterklasse der Tabellenansicht (`NSTableView`) und der Großteil des Verhaltens, von der übergeordneten Klasse erbt. Daher werden viele Vorgänge, die von einer Tabellenansicht, z. B. das Auswählen von Zeilen oder Spalten, Neupositionieren von Spalten durch Ziehen der Spaltenüberschriften usw. unterstützt auch von einer Gliederungsansicht unterstützt. Eine Xamarin.Mac-Anwendung hat die Kontrolle über diese Features, und Sie können die Gliederungsansicht-Parameter (entweder im Code oder in Interface Builder) zum Zulassen oder verbieten des bestimmte Vorgänge konfigurieren.

Eine Gliederungsansicht speichert keine eigenen Daten, die stattdessen greift Sie auf eine Datenquelle (`NSOutlineViewDataSource`) um die Zeilen und Spalten, die erforderlich sind, klicken Sie auf einen Bedarf bereitzustellen.

Verhalten von einer Gliederungsansicht kann angepasst werden, durch die Bereitstellung einer Unterklasse des Delegaten Gliederung anzeigen (`NSOutlineViewDelegate`) um Gliederung Spalte Management unterstützen, geben Sie zum Auswählen von Funktionen, Zeilenauswahl, und bearbeiten, benutzerdefinierte nachverfolgung und benutzerdefinierte Ansichten für einzelne Spalten und Zeilen.

Da es sich bei einer Gliederungsansicht Großteil dessen Verhalten und die Funktionen für eine Tabellenansicht freigegeben hat, möchten Sie möglicherweise durchlaufen unsere [Tabellenansichten](~/mac/user-interface/table-view.md) Dokumentation, bevor Sie mit diesem Artikel fortfahren.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Erstellen und verwalten Gliederungsansichten in Xcode

Wenn Sie eine neue Xamarin.Mac-Cocoa-Anwendung erstellen, erhalten Sie ein Standardfenster leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` Datei automatisch im Projekt enthalten ist. So bearbeiten Sie Ihre Windows-Design, in der **Projektmappen-Explorer**, Doppelklick der `Main.storyboard` Datei:

[![](outline-view-images/edit01.png "Wählen die Haupt-storyboard")](outline-view-images/edit01.png#lightbox)

Dies öffnet das Fenster Design in Interface Builder von Xcode:

[![](outline-view-images/edit02.png "Bearbeiten die Benutzeroberfläche in Xcode")](outline-view-images/edit02.png#lightbox)

Typ `outline` in die **der Bibliotheksinspektor** Suchfeld zum Vereinfachen der Gliederungsansicht an Steuerelemente:

[![](outline-view-images/edit03.png "Auswählen einer Gliederungsansicht aus der Bibliothek")](outline-view-images/edit03.png#lightbox)

Die View-Controller in einer Gliederungsansicht ziehen die **Schnittstellen-Editor**, können sie den Inhaltsbereich des Ansichtscontrollers füllen, und legen Sie dafür, wo sie verkleinert und vergrößert werden, mit dem Fenster in der **Einschränkung Editor**:

[![](outline-view-images/edit04.png "Bearbeiten die Einschränkungen")](outline-view-images/edit04.png#lightbox)

Wählen Sie in der Gliederungsansicht an die **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attributinspektor**:

[![](outline-view-images/edit05.png "Attribute Inspector")](outline-view-images/edit05.png#lightbox)

- **Beschreiben Sie die Spalte** -die Spalte der Tabelle, die in der hierarchischen Daten werden angezeigt.
- **AutoSave Gliederung Spalte** – Wenn `true`, die Gliederung-Spalte wird automatisch gespeichert und wiederhergestellt werden zwischen der Anwendung ausgeführt wird.
- **Einzug** – der Betrag, um die Spalten, die unter einem erweiterten Element Einzug.
- **Einzug folgt Zellen** – Wenn `true`, die Markierung für die Einzüge mit Einzug dargestellt wird zusammen mit den Zellen.
- **Automatisches Speichern erweitert Elemente** – Wenn `true`, der Status aufgeklappt/Zugeklappt, der die Elemente werden automatisch gespeichert und wiederhergestellt werden zwischen der Anwendung ausgeführt wird.
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
> Wenn Sie eine ältere Xamarin.Mac-Anwendung verwalten `NSView` basierend Gliederungsansichten verwendet werden soll, über `NSCell` Tabellenansichten basiert. `NSCell` ältere gilt und möglicherweise nicht in Zukunft unterstützt werden.

Wählen Sie eine Tabellenspalte in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attributinspektor**:

[![](outline-view-images/edit06.png "Attribute Inspector")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "Attribute Inspector")](outline-view-images/edit07.png#lightbox)

Hierbei handelt es sich um alle Eigenschaften einer Standardsicht. Sie können auch das Ändern der Größe der Zeilen für diese Kolumne.

Wählen Sie eine Zelle der Tabelle anzeigen (Standardmäßig ist dies ein `NSTextField`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen im der **Attributinspektor**:

[![](outline-view-images/edit08.png "Attribute Inspector")](outline-view-images/edit08.png#lightbox)

Sie müssen alle Eigenschaften des ein standard-Text-Feld, hier festzulegen. Standardmäßig wird ein standard-Text-Feld verwendet, um Daten für eine Zelle in einer Spalte anzuzeigen.

Wählen Sie eine Tabellenansicht der Zelle (`NSTableFieldCell`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attributinspektor**:

[![](outline-view-images/edit09.png "Attribute Inspector")](outline-view-images/edit09.png#lightbox)

Hier die wichtigsten Einstellungen sind:

- **Layout** – aus, wie Zellen in dieser Spalte angeordnet sind.
- **Verwendet einmaliges Zeilenmodus** – Wenn `true`, die Zelle ist eine einzelne Zeile auf.
- **Erste Runtime Layoutbreite** – Wenn `true`, die Zelle am liebsten der Breite festgelegt (entweder manuell oder automatisch) Wenn beim ersten der Anwendung ausführen wird angezeigt.
- **Aktion** -steuert, wann die Bearbeitung **Aktion** wird für die Zelle gesendet.
- **Verhalten** -definiert werden, wenn eine Zelle ausgewählt oder bearbeitet werden.
- **Rich-Text** – Wenn `true`, die Zelle kann formatiert und formatierten Text angezeigt.
- **Rückgängig machen** – Wenn `true`, die Zelle übernimmt die Verantwortung für ihn ist Verhalten rückgängig zu machen.

Wählen Sie die Zelle Tabellenansicht (`NSTableFieldCell`) am unteren Rand einer Tabellenspalte in der **Schnittstellenhierarchie**:

[![](outline-view-images/edit11.png "Die Tabellenansicht der Zelle auswählen")](outline-view-images/edit10.png#lightbox)

Dadurch können Sie so bearbeiten Sie die Tabelle Zelle anzeigen, die als Basis verwendet _Muster_ für alle Zellen, die für die angegebene Spalte erstellt.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Hinzufügen von Aktionen und Ergebnisdaten

Nur wie jedes andere Cocoa-UI-Steuerelement, müssen wir unsere Gliederungsansicht verfügbar zu machen und dabei handelt es sich Spalten für C#-Code mit Zellen **Aktionen** und **Outlets** (basierend auf die erforderliche Funktionalität).

Der Prozess ist für alle Gliederungsansicht-Elemente, die verfügbar gemacht werden soll:

1. Wechseln Sie zu der **Assistant Editor** und sicherstellen, dass die `ViewController.h` Datei ausgewählt ist: 

    [![](outline-view-images/edit11.png "Auswählen der richtigen h-Datei")](outline-view-images/edit11.png#lightbox)
2. Wählen Sie die Gliederungsansicht aus der **Schnittstellenhierarchie**, Strg + Klick und ziehen Sie in der `ViewController.h` Datei.
3. Erstellen Sie eine **Outlet** für die Gliederungsansicht aufgerufen `ProductOutline`: 

    [![](outline-view-images/edit13.png "Konfigurieren eines Outlets")](outline-view-images/edit13.png#lightbox)
4. Erstellen Sie **Outlets** wird aufgerufen, für die Tabellenspalten sowie `ProductColumn` und `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Konfigurieren eines Outlets")](outline-view-images/edit14.png#lightbox)
5. Speichern Sie die Änderungen und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Als Nächstes Schreiben der Anzeige von Code wir einige Daten für den Umriss, wenn die Anwendung ausgeführt wird.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Auffüllen der Gliederungsansicht an

Mit unserer Gliederungsansicht Interface Builder entwickelt und über verfügbar gemachte ein **Outlet**, als Nächstes müssen wir die C#-Code, um es zu füllen zu erstellen.

Zunächst erstellen wir ein neues `Product` Klasse, die die Informationen für die einzelnen Zeilen und die Gruppen von untergeordneten Produkten enthält. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `Product` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche:

[![](outline-view-images/populate01.png "Erstellen eine leere Klasse")](outline-view-images/populate01.png#lightbox)

Stellen Sie die `Product.cs` Datei aussehen wie folgt:

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

Als Nächstes müssen wir erstellen eine Unterklasse von `NSOutlineDataSource` unsere Gliederung die Daten bereit, wie angefordert. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductOutlineDataSource` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

Bearbeiten der `ProductTableDataSource.cs` Datei, und stellen sie wie folgt aussehen:

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

Diese Klasse verfügt über Speicher für unsere Gliederungsansicht Elemente und überschreibt die `GetChildrenCount` um die Anzahl der Zeilen in der Tabelle zurückzugeben. Die `GetChild` ein bestimmtes übergeordneten oder untergeordneten Elements zurückgibt, (wie durch die Dokumentgliederung Ansicht angefordert) und die `ItemExpandable` definiert das angegebene Element als übergeordnetes Element oder ein untergeordnetes Element.

Abschließend müssen wir erstellen eine Unterklasse von `NSOutlineDelegate` unsere Gliederung des Verhaltens bereit. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductOutlineDelegate` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

Bearbeiten der `ProductOutlineDelegate.cs` Datei, und stellen sie wie folgt aussehen:

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

Wenn wir erstellen eine Instanz von der `ProductOutlineDelegate`, übergeben wir auch in einer Instanz von der `ProductOutlineDataSource` , das die Daten für den Umriss bereitstellt. Die `GetView` Methode ist verantwortlich für die Rückgabe einer Ansicht (Daten), um die Zelle für die einer bestimmten Spalte und Zeile anzuzeigen. Wenn möglich, eine vorhandene Sicht wird wiederverwendet werden, um die Zelle anzuzeigen, wenn keine neue Ansicht erstellt werden muss.

Zum Auffüllen der Gliederung ermöglicht das Bearbeiten der `MainWindow.cs` Datei, und stellen die `AwakeFromNib` Methode sehen wie folgt:

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

Wenn wir die Anwendung ausführen, wird Folgendes angezeigt:

[![](outline-view-images/populate02.png "Die reduzierte Ansicht")](outline-view-images/populate02.png#lightbox)

Wenn es sich um einen Knoten in der Gliederungsansicht an erweitern, wird es wie folgt aussehen:

[![](outline-view-images/populate03.png "Der erweiterten Ansicht")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Sortieren nach Spalte

Wir können Benutzer die Daten in der Gliederung durch Klicken auf eine Spaltenüberschrift sortieren. Doppelklicken Sie zunächst auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie die `Product` Spalte Geben Sie `Title` für die **Sortierschlüssel**, `compare:` für die **Selektor** , und wählen Sie `Ascending` für die **Reihenfolge**:

[![](outline-view-images/sort01.png "Festlegen der Sortierreihenfolge für den Schlüssel")](outline-view-images/sort01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Jetzt ermöglicht das Bearbeiten der `ProductOutlineDataSource.cs` Datei, und fügen Sie die folgenden Methoden:

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

Die `Sort` Methode erlauben Sie die zum Sortieren der Daten in der Datenquelle, die basierend auf einer bestimmten `Product` Klassenfeld in aufsteigender oder absteigender Reihenfolge. Die überschriebene `SortDescriptorsChanged` aufgerufene Methode jedes Mal, wenn die Verwendung einer Spaltenüberschrift klickt. Sie übergeben die **Schlüssel** Wert an, die wir in Interface Builder und die Sortierreihenfolge für diese Spalte festgelegt.

Wenn wir die Anwendung auszuführen und in den Spaltenüberschriften klicken, werden die Zeilen nach dieser Spalte sortiert werden:

[![](outline-view-images/sort02.png "Beispiel für die sortierte Ausgabe")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zeilenauswahl

Wenn Sie zulassen möchten, den Benutzer, wählen Sie eine einzelne Zeile aus, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Gliederungsansicht an die **Schnittstellenhierarchie** , und deaktivieren Sie die **mehrere** Kontrollkästchen in der **Attributinspektor**:

[![](outline-view-images/select01.png "Attribute Inspector")](outline-view-images/select01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.


Als Nächstes bearbeiten Sie die `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch wird der Benutzer die Auswahl eine einzelne Zeile in der Gliederungsansicht an. Zurückgeben `false` für die `ShouldSelectItem` für ein, das Element nicht die Benutzer können ausgewählt werden soll oder `false` für jedes Element, wenn Sie nicht, dass den Benutzer, um alle Elemente auswählen zu können möchten.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Mehrere Zeilenauswahl

Wenn Sie zulassen möchten, den Benutzer einen mehrere Zeilen auswählen, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Gliederungsansicht an die **Schnittstellenhierarchie** und überprüfen Sie die **mehrere** Kontrollkästchen in der **Attributinspektor**:

[![](outline-view-images/select02.png "Attribute Inspector")](outline-view-images/select02.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.


Als Nächstes bearbeiten Sie die `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch wird der Benutzer die Auswahl eine einzelne Zeile in der Gliederungsansicht an. Zurückgeben `false` für die `ShouldSelectRow` für ein, das Element nicht die Benutzer können ausgewählt werden soll oder `false` für jedes Element, wenn Sie nicht, dass den Benutzer, um alle Elemente auswählen zu können möchten.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ Zeile auswählen

Wenn Sie möchten ermöglicht dem Benutzer, geben Sie ein Zeichen mit der Gliederungsansicht ausgewählt, und wählen Sie die erste Zeile, das Zeichen, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Gliederungsansicht an die **Schnittstellenhierarchie** und überprüfen Sie die **Tasksequenztyps** Kontrollkästchen in der **Attributinspektor**:

[![](outline-view-images/type01.png "Den Zeilentyp bearbeiten")](outline-view-images/type01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Jetzt ermöglicht das Bearbeiten der `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

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

Die `GetNextTypeSelectMatch` -Methode übernimmt die angegebenen `searchString` und gibt das Element der ersten `Product` , die diese Zeichenfolge ist, in dessen `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Neuanordnen von Spalten

Wenn Sie zulassen möchten, ziehen den Benutzer Neuanordnen von Spalten in der Gliederungsansicht, doppelklicken Sie auf die `Main.storyboard` Datei, die sie zur Bearbeitung in Interface Builder zu öffnen. Wählen Sie in der Gliederungsansicht an die **Schnittstellenhierarchie** und überprüfen Sie die **Neuanordnen** Kontrollkästchen in der **Attributinspektor**:

[![](outline-view-images/reorder01.png "Attribute Inspector")](outline-view-images/reorder01.png#lightbox)

Wenn wir geben Sie einen Wert für die **Autosave** -Eigenschaft, und überprüfen Sie die **Spalteninformationen** Feld wir, um das Layout der Tabelle die treffen Änderungen werden automatisch für uns gespeichert und wiederhergestellt das nächste Mal die Anwendung wird ausgeführt.

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode synchronisiert.

Jetzt ermöglicht das Bearbeiten der `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Die `ShouldReorder` Methode zurückgeben soll `true` für jede Spalte, die sie zulassen möchten, dass sein ziehen neu angeordnet, in der `newColumnIndex`, ansonsten return `false`;

Wenn wir die Anwendung ausführen, können wir Spaltenüberschriften zu ziehen, um unsere Spalten aber leicht neu anzuordnen:

[![](outline-view-images/reorder02.png "Beispiel für Neuanordnen von Spalten")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Bearbeiten von Zellen

Wenn Sie zulassen möchten, den Benutzer die Werte für eine bestimmte Zelle bearbeiten, bearbeiten Sie die `ProductOutlineDelegate.cs` und Ändern der `GetViewForItem` -Methode wie folgt:

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

Wenn wir die Anwendung ausführen, kann der Benutzer jetzt auch die Zellen in der Tabellenansicht bearbeiten:

[![](outline-view-images/editing01.png "Ein Beispiel für Zellen bearbeiten")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Verwendung von Images in Gliederungsansichten

Um ein Bild enthalten, im Rahmen der Zelle in einer `NSOutlineView`, müssen Sie ändern, wie die Daten durch die Dokumentgliederung-Sicht zurückgegeben werden `NSTableViewDelegate's` `GetView` zu verwendende Methode eine `NSTableCellView` statt der normalen `NSTextField`. Zum Beispiel:

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

Weitere Informationen finden Sie unter den [mithilfe von Bildern mit Gliederungsansichten](~/mac/app-fundamentals/image.md) Teil unserer [arbeiten mit Images](~/mac/app-fundamentals/image.md) Dokumentation.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Gliederungsansichten für Daten-Bindung

Techniken für Schlüssel / Wert-Codierung und Binden von Daten in Ihre Xamarin.Mac-Anwendung verwenden, können Sie die Menge des Codes, die Sie schreiben und verwalten, die zum Auffüllen und zum Arbeiten mit UI-Elemente, erheblich reduzieren. Sie haben außerdem den Vorteil, dass weitere entkoppeln Ihre Daten sichern (_Datenmodell_) aus Ihrem Front end-Benutzeroberfläche (_Model-View-Controller_), wodurch einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

Schlüssel-Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf die Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, statt den Zugriff über Instanzvariablen oder Methoden der eigenschaftenzugriffsmethode (`get/set`). Durch die Implementierung von Schlüssel / Wert-Codierung kompatibel Accessors in Ihre Xamarin.Mac-Anwendung, erhalten Sie Zugriff auf andere MacOS-Funktionen wie z. B. das beobachten von Schlüssel-Wert (KVO), Datenbindung, Kerndaten, Cocoa-Bindungen und Scriptability.

Weitere Informationen finden Sie unter den [Gliederung Anzeigen einer Datenbindung](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) Teil unserer [Datenbindung und Schlüssel / Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird eine ausführliche Übersicht über das Arbeiten mit Gliederungsansichten in einer Xamarin.Mac-Anwendung verwendet. Wir haben gesehen, die verschiedenen Typen und verwendet Gliederungsansichten, das Erstellen und verwalten Gliederungsansichten in Interface Builder von Xcode und wie Sie mit Gliederungsansichten in C#-Code zu arbeiten.

## <a name="related-links"></a>Verwandte Links

- [MacOutlines (Beispiel)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Quelllisten](~/mac/user-interface/source-list.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in die Ansichten beschrieben.](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
