---
title: Gliederung-Ansichten
description: In diesem Artikel wird das Arbeiten mit Gliederung Sichten in einer Anwendung Xamarin.Mac behandelt. Erstellen und Verwalten von Gliederung Sichten in Xcode und Benutzeroberflächen-Generator und Arbeiten mit diesen programmgesteuerten beschrieben.
ms.prod: xamarin
ms.assetid: 043248EE-11DA-4E96-83A3-08824A4F2E01
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 82cb3afadf7615fdd92476371e9ab80cd1228b02
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="outline-views"></a>Gliederung-Ansichten

_In diesem Artikel wird das Arbeiten mit Gliederung Sichten in einer Anwendung Xamarin.Mac behandelt. Erstellen und Verwalten von Gliederung Sichten in Xcode und Benutzeroberflächen-Generator und Arbeiten mit diesen programmgesteuerten beschrieben._

Bei der Arbeit mit c# und .NET in einer Anwendung Xamarin.Mac haben Sie Zugriff auf die gleiche Gliederung anzeigt, die ein Entwickler arbeiten in unter *Objective-C* und *Xcode* verfügt. Da Xamarin.Mac direkt mit Xcode integriert ist, können Sie die Xcode _Schnittstelle-Generator_ erstellen und verwalten Ihre Ansichten Gliederung (oder erstellen sie optional direkt im C#-Code).

Einer Gliederungsansicht ist ein Typ der Tabelle, die dem Benutzer ermöglicht erweitern oder reduzieren Zeilen mit hierarchischen Daten. Wie einer Tabellenansicht zeigt einer Gliederungsansicht Daten für einen Satz verwandter Elemente mit Zeilen, die einzelne Elemente und Spalten, die Attribute dieser Elemente darstellen, darstellt. Im Gegensatz zu einer Tabellenansicht Elemente in einer Gliederungsansicht sind nicht in einer flachen Liste, sie sind in einer Hierarchie, z. B. Dateien und Ordnern auf einer Festplatte organisiert.

[![](outline-view-images/populate03.png "Eine Beispiel-app ausführen")](outline-view-images/populate03.png#lightbox)

In diesem Artikel werden die Grundlagen der Arbeit mit Gliederung Sichten in einer Anwendung Xamarin.Mac eingegangen. Wird mit hoher vorgeschlagen, dass Sie über arbeiten die [Hello, Mac](~/mac/get-started/hello-mac.md) Artikel zuerst, insbesondere die [Einführung in Xcode und Benutzeroberflächen-Generator](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) und [Steckdosen und Aktionen](~/mac/get-started/hello-mac.md#Outlets_and_Actions) Abschnitte, wie sie behandelt wichtige Konzepte und Techniken, die in diesem Artikel verwendet werden.

Sie möchten einen Blick auf die [Verfügbarmachen von C#-Klassen / Methoden für Objective-C-](~/mac/internals/how-it-works.md) Teil der [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) dokumentieren, es wird erläutert, die `Register` und `Export` Befehle verwendet, um über das Netzwerk Ihrer C#-Klassen für Objective-C-Objekte und Elemente der Benutzeroberfläche von Volltextkatalogen.

<a name="Introduction_to_Outline_Views" />

## <a name="introduction-to-outline-views"></a>Einführung in die Ansichten werden kann

Einer Gliederungsansicht ist ein Typ der Tabelle, die dem Benutzer ermöglicht erweitern oder reduzieren Zeilen mit hierarchischen Daten. Wie einer Tabellenansicht zeigt einer Gliederungsansicht Daten für einen Satz verwandter Elemente mit Zeilen, die einzelne Elemente und Spalten, die Attribute dieser Elemente darstellen, darstellt. Im Gegensatz zu einer Tabellenansicht Elemente in einer Gliederungsansicht sind nicht in einer flachen Liste, sie sind in einer Hierarchie, z. B. Dateien und Ordnern auf einer Festplatte organisiert.

Wenn ein Element in einer Gliederungsansicht Weitere Elemente enthält, können Sie erweitert oder vom Benutzer reduziert werden. Ein erweiterbares Element zeigt ein Dreieck Offenlegung von, die auf der rechten Seite verweist, wenn das Element reduziert ist, und zeigt nach unten, wenn das Element erweitert wird. Durch Klicken auf das Dreieck Offenlegung bewirkt, dass das Element erweitert oder reduziert.

Die Gliederungsansicht (`NSOutlineView`) ist eine Unterklasse der Tabellenansicht (`NSTableView`) und der Großteil des Verhaltens daher von der übergeordneten Klasse erbt. Daher werden viele Vorgänge, die von einer Tabellenansicht, wie z. B. das Auswählen von Zeilen oder Spalten, Neuanordnen von Spalten durch Ziehen von Spaltenüberschriften usw. unterstützt auch von einer Gliederungsansicht unterstützt. Eine Anwendung Xamarin.Mac hat die Kontrolle über diese Funktionen und die Gliederungsansicht-Parameter (entweder im Code oder Schnittstelle-Generator) konfigurieren, um gestattet oder verweigert bestimmte Vorgänge.

Einer Gliederungsansicht seinem eigenen Daten nicht gespeichert, er verlässt sich stattdessen auf einer Datenquelle (`NSOutlineViewDataSource`), die Zeilen und Spalten, die erforderlich sind, klicken Sie auf einen Bedarf bereitzustellen.

Verhalten einer Gliederungsansicht kann angepasst werden, durch die Bereitstellung einer Unterklasse des Delegaten Gliederung anzeigen (`NSOutlineViewDelegate`) um Gliederung Spalte Management zu unterstützen, geben Sie zum Auswählen von Funktionen, Zeilenauswahl und Bearbeiten von benutzerdefinierten Nachverfolgungsdatensatzes und benutzerdefinierte Ansichten für einzelne Spalten und Zeilen.

Da eine Gliederungsansicht Großteil der Verhalten und die Funktionen mit einer Tabellenansicht gemeinsam verwendet wird, möchten Sie möglicherweise durchlaufen unsere [Tabellensichten](~/mac/user-interface/table-view.md) Dokumentation vor dem Fortfahren mit der in diesem Artikel.

<a name="Creating_and_Maintaining_Outline_Views_in_Xcode" />

## <a name="creating-and-maintaining-outline-views-in-xcode"></a>Erstellen und Verwalten von Ansichten der Kontur in Xcode

Wenn Sie eine neue Xamarin.Mac Kakao-Anwendung erstellen, erhalten Sie Standardfensters leer ist, wird standardmäßig an. Dieses Windows wird definiert, einem `.storyboard` automatisch im Projekt enthaltene Datei. So bearbeiten Sie die Windows-Design in der **Projektmappen-Explorer**, doppelklicken klicken Sie auf die `Main.storyboard` Datei:

[![](outline-view-images/edit01.png "Die Haupt-Storyboard auswählen")](outline-view-images/edit01.png#lightbox)

Dies öffnet das Fenster Design in Xcodes Benutzeroberflächen-Generator:

[![](outline-view-images/edit02.png "Bearbeiten die Benutzeroberfläche in Xcode")](outline-view-images/edit02.png#lightbox)

Typ `outline` in der **Bibliothek Inspektors** Suchfeld zum Suchen die Gliederungsansicht Steuerelemente erleichtern:

[![](outline-view-images/edit03.png "Auswählen einer Gliederungsansicht aus der Bibliothek")](outline-view-images/edit03.png#lightbox)

Ziehen Sie die View-Controller in einer Gliederungsansicht der **Benutzeroberflächen-Editors**, unbedingt Ausfüllen des Inhaltsbereichs des View-Controller, und legen Sie dafür, wo es verkleinert und vergrößert wird, mit dem Fenster in der **Einschränkung Editor**:

[![](outline-view-images/edit04.png "Die Einschränkungen bearbeiten")](outline-view-images/edit04.png#lightbox)

Wählen Sie in der Gliederungsansicht der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](outline-view-images/edit05.png "Die Attribut-Inspektor")](outline-view-images/edit05.png#lightbox)

- **Zeigen Sie die Spalte** -die Spalte der Tabelle, die in dem die hierarchischen Daten angezeigt werden.
- **AutoSpeichern Gliederung Spalte** – Wenn `true`, die Gliederung-Spalte wird automatisch gespeichert und wiederhergestellt, die zwischen der Anwendung ausgeführt wird.
- **Einzug** -der Betrag, um den Einzug Spalten unter eines erweiterten Elements.
- **Einzug folgt Zellen** – Wenn `true`, die Markierung Einzug mit Einzug dargestellt wird zusammen mit den Zellen.
- **AutoSpeichern erweitert Elemente** – Wenn `true`, der Status erweitert/reduziert, der die Elemente werden automatisch gespeichert und wiederhergestellt, die zwischen der Anwendung ausgeführt wird.
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
> Wenn Sie eine ältere Xamarin.Mac-Anwendung verwaltet werden `NSView` basierende Gliederung Ansichten sollte verwendet werden, über `NSCell` Tabellensichten Basis. `NSCell` ältere gilt und möglicherweise nicht zukünftig unterstützt werden.

Wählen Sie eine Tabellenspalte in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](outline-view-images/edit06.png "Die Attribut-Inspektor")](outline-view-images/edit06.png#lightbox)

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

[![](outline-view-images/edit07.png "Die Attribut-Inspektor")](outline-view-images/edit07.png#lightbox)

Hierbei handelt es sich um alle Eigenschaften einer Standardsicht. Sie haben auch die Möglichkeit zum Ändern der Größe der Zeilen für diese Spalte hier.

Wählen Sie eine Zelle der Tabelle anzeigen (Standardmäßig ist dies ein `NSTextField`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](outline-view-images/edit08.png "Die Attribut-Inspektor")](outline-view-images/edit08.png#lightbox)

Sie müssen alle Eigenschaften eines standard-Text-Felds hier festlegen. Standardmäßig wird ein standard-Text-Feld verwendet, Daten für eine Zelle in einer Spalte angezeigt.

Wählen Sie eine Zelle Tabellensicht (`NSTableFieldCell`) in der **Schnittstellenhierarchie** und die folgenden Eigenschaften stehen in der **Attribut Inspektor**:

[![](outline-view-images/edit09.png "Die Attribut-Inspektor")](outline-view-images/edit09.png#lightbox)

Im folgenden die wichtigsten Einstellungen sind:

- **Layout** – aus, wie die Zellen in dieser Spalte angeordnet sind.
- **Verwendet den Modus für einzelne Zeile** – Wenn `true`, die Zelle zu einer einzelnen Zeile beschränkt ist.
- **Erste Runtime Layoutbreite** – Wenn `true`, bevorzugt die Zelle die Breite festgelegt (entweder manuell oder automatisch) beim ersten Mal die Anwendung ausgeführt wird angezeigt.
- **Aktion** -steuert, wann die Bearbeitung **Aktion** wird für die Zelle gesendet.
- **Verhalten** -definiert werden, wenn eine Zelle ausgewählt oder bearbeitbar ist.
- **Rich-Text** – Wenn `true`, die Zelle kann formatiert und formatierten Text anzeigen.
- **Rückgängig machen** – Wenn `true`, die Zelle übernimmt die Verantwortung für ihn ist Verhalten rückgängig zu machen.

Wählen Sie die Zelle Tabellenansicht (`NSTableFieldCell`) am unteren Rand einer Tabellenspalte, in der **Schnittstellenhierarchie**:

[![](outline-view-images/edit11.png "Auswählen der Zelle Tabellenansicht")](outline-view-images/edit10.png#lightbox)

Dadurch können Sie so bearbeiten Sie die Zelle Tabellenansicht als Basis verwendet _Muster_ für alle Zellen, die für die angegebene Spalte erstellt.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Hinzufügen von Aktionen und Ausgänge

Nur wie jedes andere Kakao UI-Steuerelement, müssen wir unsere Gliederungsansicht verfügbar zu machen und wird für Spalten und C#-Code mit Zellen **Aktionen** und **Steckdosen** (basierend auf die erforderliche Funktionalität).

Der Prozess ist für jedes Gliederungsansicht-Element, das wir verfügbar machen möchten:

1. Wechseln Sie zu der **Assistant Editor** und sicherstellen, dass die `ViewController.h` Datei ausgewählt ist: 

    [![](outline-view-images/edit11.png "Auswählen der richtigen .h-Datei")](outline-view-images/edit11.png#lightbox)
2. Wählen Sie die Gliederungsansicht aus der **Schnittstellenhierarchie**, Steuerelement klicken und ziehen Sie in der `ViewController.h` Datei.
3. Erstellen einer **Nachrichtenplattform** für die Gliederungsansicht aufgerufen `ProductOutline`: 

    [![](outline-view-images/edit13.png "Konfigurieren von einer Steckdose")](outline-view-images/edit13.png#lightbox)
4. Erstellen Sie **Steckdosen** für die Spalten der Tabellen ebenfalls aufgerufen `ProductColumn` und `DetailsColumn`: 

    [![](outline-view-images/edit14.png "Konfigurieren von einer Steckdose")](outline-view-images/edit14.png#lightbox)
5. Speichern Sie die Änderungen und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Als Nächstes wird der Anzeige von Code schreiben einige Daten für die Kontur, wenn die Anwendung ausgeführt wird.

<a name="Populating_the_Table_View" />

## <a name="populating-the-outline-view"></a>Die Gliederungsansicht Auffüllen

Mit unseren Gliederungsansicht vorgesehen, in der Benutzeroberflächen-Generator und über verfügbar gemachte ein **Nachrichtenplattform**, als Nächstes müssen wir erstellen den C#-Code, um es zu füllen.

Zunächst erstellen wir ein neues `Product` Klasse, die Informationen für die einzelnen Zeilen und die Gruppen von Produkten Sub aufnimmt. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `Product` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche:

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

Wir als Nächstes erstellen Sie eine Unterklasse von `NSOutlineDataSource` um die Daten für unsere Gliederung bereitzustellen, wie angefordert. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductOutlineDataSource` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

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

Diese Klasse ist der Speicher für unsere Gliederungsansicht Elemente und überschreibt die `GetChildrenCount` die Anzahl der Zeilen in der Tabelle zurückgegeben. Die `GetChild` gibt einen bestimmten über- oder untergeordneten Elements (wie durch die Gliederungsansicht angefordert) und die `ItemExpandable` definiert das angegebene Element als übergeordnetes Element oder ein untergeordnetes Element.

Schließlich müssen, erstellen Sie eine Unterklasse von `NSOutlineDelegate` unsere Gliederung des Verhaltens bereit. In der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **hinzufügen** > **neue Datei...** Wählen Sie **allgemeine** > **leere Klasse**, geben Sie `ProductOutlineDelegate` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche.

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

Wenn wir erstellen eine Instanz von der `ProductOutlineDelegate`, wir auch in einer Instanz von übergeben der `ProductOutlineDataSource` , die Daten für die Gliederung bereitstellt. Die `GetView` Methode ist verantwortlich für die Rückgabe einer Ansicht (Daten), um die Zelle für einen bestimmten Spalte und Zeile anzuzeigen. Wenn möglich, eine vorhandene Sicht wird wiederverwendet werden, um die Zelle anzuzeigen, wenn keine neue Ansicht erstellt werden muss.

Um den Umriss aufzufüllen, ermöglicht das Bearbeiten der `MainWindow.cs` Datei, und stellen die `AwakeFromNib` Methode aussehen wie folgt:

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

Wenn wir einen Knoten in der Gliederungsansicht erweitern, wird es wie folgt aussehen:

[![](outline-view-images/populate03.png "Der erweiterten Ansicht")](outline-view-images/populate03.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Nach Spalte sortieren

Wir können Benutzer die Daten in der Gliederung durch Klicken auf eine Spaltenüberschrift sortieren. Doppelklicken Sie zuerst auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie die `Product` Spalte Geben Sie `Title` für die **Sortierschlüssel**, `compare:` für die **Selektor** , und wählen Sie `Ascending` für die **Reihenfolge**:

[![](outline-view-images/sort01.png "Festlegen der Sortierreihenfolge Schlüssel")](outline-view-images/sort01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

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

Die `Sort` Methode ermöglichen es uns zum Sortieren der Daten in der Datenquelle basierend auf einer bestimmten `Product` Klassenfeld in aufsteigender oder absteigender Reihenfolge. Die überschriebene `SortDescriptorsChanged` Methode wird aufgerufen, jedes Mal, wenn die Verwendung auf eine Spaltenüberschrift klickt. Sie übergeben die **Schlüssel** wir im Schnittstelle-Generator und die Sortierreihenfolge für diese Spalte festgelegte Wert.

Wenn wir führen Sie die Anwendung, und klicken Sie in den Spaltenüberschriften, werden die Zeilen nach dieser Spalte sortiert werden:

[![](outline-view-images/sort02.png "Beispiel für die sortierte Ausgabe")](outline-view-images/sort02.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Zeilenauswahl

Wenn Sie möchten zulassen, dass die Benutzer wählen Sie eine einzelne Zeile aus, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Gliederungsansicht der **Schnittstellenhierarchie** und deaktivieren Sie die **mehrere** Kontrollkästchen in der **Attribut Inspektor**:

[![](outline-view-images/select01.png "Die Attribut-Inspektor")](outline-view-images/select01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.


Als Nächstes Bearbeiten der `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch kann den Benutzer eine einzelne Zeile in der Gliederungsansicht auswählen. Zurückgeben `false` für die `ShouldSelectItem` für ein, das Element nicht die Benutzer können ausgewählt werden soll oder `false` für jedes Element, wenn Sie nicht, dass die Benutzer können keine Elemente ausgewählt werden möchten.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Mehrere Zeilenauswahl

Wenn Sie zulassen möchten, den Benutzer eine mehrere Zeilen auswählen, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Gliederungsansicht der **Schnittstellenhierarchie** und überprüfen Sie die **mehrere** Kontrollkästchen in der **Attribut Inspektor**:

[![](outline-view-images/select02.png "Die Attribut-Inspektor")](outline-view-images/select02.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.


Als Nächstes Bearbeiten der `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldSelectItem (NSOutlineView outlineView, NSObject item)
{
    // Don't select product groups
    return !((Product)item).IsProductGroup;
}
```

Dadurch kann den Benutzer eine einzelne Zeile in der Gliederungsansicht auswählen. Zurückgeben `false` für die `ShouldSelectRow` für ein, das Element nicht die Benutzer können ausgewählt werden soll oder `false` für jedes Element, wenn Sie nicht, dass die Benutzer können keine Elemente ausgewählt werden möchten.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Typ, um die Zeile auszuwählen.

Wenn Sie möchten ermöglicht dem Benutzer die Gliederungsansicht ausgewählt und geben Sie ein Zeichen, und wählen Sie die erste Zeile, der das angegebenen Zeichen hat, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Gliederungsansicht der **Schnittstellenhierarchie** und überprüfen Sie die **Typ auswählen** Kontrollkästchen in der **Attribut Inspektor**:

[![](outline-view-images/type01.png "Der Zeilentyp bearbeiten")](outline-view-images/type01.png#lightbox)

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

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

Die `GetNextTypeSelectMatch` -Methode übernimmt die angegebenen `searchString` und gibt das Element der ersten `Product` hat, die diese Zeichenfolge in der `Title`.

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Neuanordnen von Spalten

Wenn Sie den Benutzer, ziehen zulassen möchten, Neuanordnen von Spalten in der Gliederungsansicht, doppelklicken Sie auf die `Main.storyboard` Datei, um ihn zur Bearbeitung in der Benutzeroberflächen-Generator zu öffnen. Wählen Sie in der Gliederungsansicht der **Schnittstellenhierarchie** und überprüfen Sie die **Reordering** Kontrollkästchen in der **Attribut Inspektor**:

[![](outline-view-images/reorder01.png "Die Attribut-Inspektor")](outline-view-images/reorder01.png#lightbox)

Wenn wir geben Sie einen Wert für die **AutoSpeichern** -Eigenschaft, und Überprüfen der **Spalteninformationen** Feld, alle Änderungen, die wir zum Layout der Tabelle ändern, werden automatisch für uns gespeichert und wiederhergestellt das nächste Mal die Anwendung wird ausgeführt.

Die Änderungen zu speichern und zurück zu Visual Studio für Mac mit Xcode zu synchronisieren.

Jetzt ermöglicht das Bearbeiten der `ProductOutlineDelegate.cs` Datei, und fügen Sie die folgende Methode hinzu:

```csharp
public override bool ShouldReorder (NSOutlineView outlineView, nint columnIndex, nint newColumnIndex)
{
    return true;
}
```

Die `ShouldReorder` -Methode zurückgeben sollte `true` für jede Spalte, die es ermöglichen, werden möchten ziehen neu angeordnet, in der `newColumnIndex`, ansonsten return `false`;

Wenn wir die Anwendung ausführen, können wir Spaltenüberschriften, um unsere Spalten neu anordnen ziehen:

[![](outline-view-images/reorder02.png "Beispiel für die Spalten neu anordnen")](outline-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Bearbeiten von Zellen

Wenn Sie zulassen, dass der Benutzer die Werte für eine bestimmte Zelle bearbeiten, bearbeiten möchten die `ProductOutlineDelegate.cs` Datei und ändern Sie die `GetViewForItem` Methode wie folgt:

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

Wenn wir die Anwendung ausführen, kann der Benutzer jetzt die Zellen in der Tabellenansicht bearbeiten:

[![](outline-view-images/editing01.png "Ein Beispiel für die Bearbeitung von Zellen")](outline-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Outline_Views" />

## <a name="using-images-in-outline-views"></a>Verwenden von Images in den Ansichten der Kontur.

Um ein Bild enthalten, als Teil der Zelle in einer `NSOutlineView`, müssen Sie dagegen ändern, wie die Daten über die Gliederungsansicht zurückgegeben werden `NSTableViewDelegate's` `GetView` zu verwendende Methode eine `NSTableCellView` anstelle der normalen `NSTextField`. Zum Beispiel:

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

Weitere Informationen finden Sie unter der [mithilfe von Bildern mit Gliederung Ansichten](~/mac/app-fundamentals/image.md) Teil unserer [arbeiten mit Images](~/mac/app-fundamentals/image.md) Dokumentation.

<a name="Data_Binding_Outline_Views" />

## <a name="data-binding-outline-views"></a>Gliederung von Datenansichten-Bindung

Mithilfe von Techniken von Schlüssel-Wert zu codieren, und Binden von Daten in Ihrer Anwendung Xamarin.Mac, können Sie den Umfang des Codes, die Sie schreiben und verwalten, die zum Auffüllen von und Arbeiten mit UI-Elemente, erheblich verringern. Sie haben außerdem den Vorteil, dass weitere Entkopplung Ihre Daten sichern (_Datenmodell_) von der Vorderseite enden Benutzeroberfläche (_Model-View-Controller_), was zu einfacher zu verwalten, eine flexiblere Anwendung Entwurf.

Schlüssel-Wert-Codierung (KVM) ist ein Mechanismus für den Zugriff auf Eigenschaften eines Objekts ist indirekt, mithilfe von Schlüsseln (speziell formatierte Zeichenfolgen) zum Identifizieren von Eigenschaften, anstatt über Nachrichteninstanzvariablen auf Sie zuzugreifen oder Methoden des Eigenschaftenaccessors (`get/set`). Durch die Implementierung von Schlüssel-Wert-Codierung kompatibel Accessoren in Ihrer Anwendung Xamarin.Mac können haben Sie Zugriff auf andere MacOS-Funktionen, z. B. beobachten von Schlüssel-Wert (KVO), zum Binden von Daten, Core Daten, Kakao Bindungen und Scriptability.

Weitere Informationen finden Sie unter der [Gliederung Anzeigen einer Datenbindung](~/mac/app-fundamentals/databinding.md#Outline_View_Data_Binding) Teil unserer [zum Binden von Daten und Schlüssel-Wert-Codierung](~/mac/app-fundamentals/databinding.md) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Dieser Artikel hat eine ausführliche Übersicht über das Arbeiten mit Gliederung Sichten in einer Anwendung Xamarin.Mac übernommen. Wir gesehen haben die verschiedenen Typen und der Gliederung Ansichten, wie zum Erstellen und Verwalten von Gliederung Ansichten in Xcodes Benutzeroberflächen-Generator und zum Arbeiten mit Sichten Gliederung in C#-Code verwendet.

## <a name="related-links"></a>Verwandte Links

- [MacOutlines (Beispiel)](https://developer.xamarin.com/samples/mac/MacOutlines/)
- [MacImages (Beispiel)](https://developer.xamarin.com/samples/mac/MacImages/)
- [Hello, Mac (Hallo, Mac)](~/mac/get-started/hello-mac.md)
- [Tabellenansichten](~/mac/user-interface/table-view.md)
- [Quelllisten](~/mac/user-interface/source-list.md)
- [Datenbindung und Schlüssel/Wert-Codierung](~/mac/app-fundamentals/databinding.md)
- [Eingaberichtlinien für OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Einführung in die Ansichten werden kann](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/OutlineView/OutlineView.html#//apple_ref/doc/uid/10000023i)
- [NSOutlineView](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSOutlineView_Class/index.html#//apple_ref/doc/uid/TP40004079)
- [NSOutlineViewDataSource](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSOutlineViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004175)
- [NSOutlineViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSOutlineViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008609)
