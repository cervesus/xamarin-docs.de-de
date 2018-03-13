---
title: Arbeiten mit Tabellensichten
description: Dieser Artikel umfasst das Entwerfen und Arbeiten mit Tabellensichten und View-Controller Tabelle innerhalb einer Xamarin.tvOS-app.
ms.topic: article
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: af562ac03f2cd5f293f99c7509000499ad5deaa4
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="working-with-table-views"></a>Arbeiten mit Tabellensichten

_Dieser Artikel umfasst das Entwerfen und Arbeiten mit Tabellensichten und View-Controller Tabelle innerhalb einer Xamarin.tvOS-app._

In tvos. außerdem wurden werden einer Tabellenansicht präsentiert, als eine einzelne Spalte des Bildlaufs an Zeilen, die optional in Gruppen oder Abschnitten organisiert werden können. Tabellenansichten sollte verwendet werden, wenn Sie eine große Menge von Daten für den Benutzer in einer deaktivieren, um zu verstehen, wie effizient anzeigen müssen.

Tabellenansichten werden in der Regel in 1-Seite angezeigt ein [geteilten Ansicht](~/ios/tvos/user-interface/split-views.md) als Navigation mit den Details des ausgewählten Elements in der entgegengesetzten Seite angezeigt:

[![](table-views-images/intro01.png "Beispiel-Tabellenansicht")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>Informationen zu Tabellensichten

Ein `UITableView` zeigt eine einzelne Spalte des bildlauffähigen Zeilen als eine hierarchische Liste von Informationen, die optional in Gruppen oder Abschnitten organisiert werden kann: 

[![](table-views-images/table01.png "Ein ausgewähltes Element")](table-views-images/table01.png#lightbox)

Apple hat die folgenden Vorschläge zum Arbeiten mit Tabellen:

- **Beachten Sie Breite werden** -versuchen, das richtige Gleichgewicht in Ihrer Tabellenbreiten zu erreichen. Wenn die Tabelle zu breit ist, kann schwierig sein, aus der Entfernung zu scannen und Weg von den verfügbaren Inhaltsbereichs dauert. Wenn die Tabelle nicht breit genug ist, kann kann es dazu führen, dass die Informationen an, die abgeschnitten werden oder Wrap, erneut dies für den Benutzer, aus dem gelesen werden soll, über den Raum schwierig.
- **Tabelle Inhalt schnell anzeigen** – für umfangreiche Listen der Daten verzögerte Laden des Inhalts und starten Sie die Informationen angezeigt, sobald die Tabelle, die dem Benutzer angezeigt wird. Wenn die Tabelle zu lange dauert, verliert der Benutzer möglicherweise Interesse in Ihrer app oder die Reaktionszeiten für sie gesperrt wird.
- **Informieren der lange Content Benutzerlasten** – Wenn es sich bei einer langen Tabelle-Ladezeit ist unvermeidbar, vorhanden eine [Statusanzeige oder eine Aktivität Indikator](~/ios/tvos/user-interface/progress-indicators.md) , damit sie wissen, dass die app abgestürzt noch nicht.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Tabellensichttypen Zelle

Ein `UITableViewCell` wird verwendet, um die einzelnen Datenzeilen in der Tabellenansicht darstellen. Apple hat mehrere standardmäßige Tabellentypen Zelle definiert:

- **Standard** – dieser Typ stellt eine Option Bild-auf der linken Seite der Zelle und linksbündig Titel auf der rechten Seite. 
- **Untertitel** – dieser Typ stellt ein Links ausgerichtete Titel auf der ersten Zeile und eine kleinere linksbündig Untertitel in der nächsten Zeile.
- **Wert 1** -dieser Typ stellt einen Titel links ausgerichtete mit einem Untertitel heller farbigen, rechtsbündig in derselben Zeile.
- **Der Wert 2** -dieser Typ stellt einen Titel rechtsbündig mit einem Untertitel heller farbigen, linksbündig ausgerichtet in derselben Zeile.

Alle von der Standardeinstellung Tabellensichttypen Zelle unterstützen auch grafische Elemente wie z. B. Offenlegung von Indikatoren oder Häkchen. 

Darüber hinaus können Sie definieren eine **benutzerdefinierte** Tabelle Zelle Ansichtstyp und präsentieren einer _Prototyp Zelle_, entweder in der Benutzeroberflächen-Designer oder über Code zu erstellen.

Apple hat die folgenden Vorschläge für die Arbeit mit Tabellenzellen-Ansicht:

- **Vermeiden Sie Text kürzen** -beibehalten kurzer einzelne Textzeilen, damit sie am Ende nicht abgeschnitten. Abgeschnittene Wörter oder Ausdrücke sind für den Benutzer beim Analysieren von über den Raum fest.
- **Betrachten Sie den Zustand "fokussiert Zeile"** : Da eine Zeile größer ist, mehrere gerundet wird Ecken im Fokus, müssen Sie die Darstellung der Zelle in allen Zuständen zu testen. Bilder oder Text möglicherweise abgeschnitten werden, oder suchen Sie in den Zustand "fokussiert" falsch.
- **Bearbeitbare Tabellen nur selten verwenden** -verschieben oder Löschen von Zeilen der Tabelle ist zeitaufwändiger für tvos. außerdem wurden als iOS. Sie müssen sorgfältig zu entscheiden, ob diese Funktion werden hinzugefügt oder aus Ihrer app tvos. außerdem wurden abgelenkt.
- **Erstellen benutzerdefinierter Zelle Typen, auf dem entsprechenden** - Testentwurfsmodus integrierte Zelle Tabellensichttypen hervorragend für die in vielen Situationen sollten Sie benutzerdefinierte Zellentypen nicht standardmäßige Informationen bieten eine bessere Kontrolle und eine bessere Leistung stellen die Informationen zum Erstellen der Benutzer.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Arbeiten mit Tabellensichten

Die einfachste Möglichkeit zum Arbeiten mit Tabellensichten in einer Xamarin.tvOS-app ist ihre Darstellung in der Benutzeroberflächen-Designer zu erstellen.

Um zu beginnen, machen Sie Folgendes:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)
    
1. In Visual Studio für Mac, starten Sie ein neues tvos. außerdem wurden-app-Projekt, und wählen **tvos. außerdem wurden** > **App** > **einzelne Ansicht App** , und klicken Sie auf die  **Nächste** Schaltfläche: 

    [![](table-views-images/table02.png "Wählen Sie einzelne Sicht App")](table-views-images/table02.png#lightbox)
1. Geben Sie einen **Namen** für die app, und klicken Sie auf **Weiter**: 

    [![](table-views-images/table03.png "Geben Sie einen Namen für die app")](table-views-images/table03.png#lightbox)
1. Passen Sie entweder die **Projektname** und **Projektmappenname** oder akzeptieren Sie die Standardeinstellungen, und klicken Sie auf die **erstellen** Schaltfläche, um die neue Projektmappe zu erstellen: 

    [![](table-views-images/table04.png "Der Projektname und einen Projektmappennamen an")](table-views-images/table04.png#lightbox)
1. In der **Lösung Pad**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie in der iOS-Designer zu öffnen: 

    [![](table-views-images/table05.png "Die Datei Main.storyboard")](table-views-images/table05.png#lightbox)
1. Wählen Sie aus, und löschen Sie die **Standard-View-Controller**: 

    [![](table-views-images/table06.png "Wählen Sie aus, und löschen Sie die Standard-View-Controller")](table-views-images/table06.png#lightbox)
1. Wählen Sie eine **Split-View-Controller** aus der **Toolbox** und ziehen Sie sie auf der Entwurfsoberfläche angezeigt.
1. Standardmäßig erhalten Sie eine [geteilten Ansicht](~/ios/tvos/user-interface/split-views.md) mit einer **Navigation-View-Controller** und ein **Tabelle-View-Controller** in der linken Seite und ein **View-Controller** in der rechten Seite. Dies ist die empfohlene Verwendung von Apple einer Tabellenansicht in tvos. außerdem wurden: 

    [![](table-views-images/table08.png "Hinzufügen einer geteilten Ansicht")](table-views-images/table08.png#lightbox)
1. Sie müssen jeden Teil der Tabellenansicht wählen und weisen sie eine benutzerdefinierte **Klassenname** in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer** , damit Sie später in c# zugreifen zu können Code. Z. B. die **Tabelle-View-Controller**: 

    [![](table-views-images/table09.png "Weisen Sie einen Klassennamen")](table-views-images/table09.png#lightbox)
1. Stellen Sie sicher, dass es sich bei der Erstellung einer benutzerdefinierten Klasse für die **Tabelle-View-Controller**, die **Tabellenansicht** und ein beliebiger **Prototyp Zellen**. Visual Studio für Mac werden die benutzerdefinierten Klassen der Projekt-Struktur hinzugefügt, wie sie erstellt werden: 

    [![](table-views-images/table10.png "Die benutzerdefinierten Klassen in der Projektstruktur")](table-views-images/table10.png#lightbox)
1. Klicken Sie dann wählen Sie der Tabellenansicht auf der Entwurfsoberfläche aus, und passen Sie bei Bedarf deren Eigenschaften. Z. B. die Anzahl der **Prototyp Zellen** und **Stil** (Plain oder gruppiert): 

    [![](table-views-images/table11.png "Die Registerkarte "Widget"")](table-views-images/table11.png#lightbox)
1. Für jede **Prototyp Zelle**, wählen Sie diese, und weisen Sie einen eindeutigen **Bezeichner** in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**. Dieser Schritt ist _sehr wichtig_ wie Sie diesen Bezeichner später benötigen, wenn Sie die Tabelle füllen. Z. B. `AttrCell`: 

    [![](table-views-images/table12.png "Die Registerkarte "Widget"")](table-views-images/table12.png#lightbox)
1. Sie können auch auswählen, an der Zelle als eines der [Standard Zelle Tabellensichttypen](#Table-View-Cell-Types) über die **Stil** Dropdownliste oder legen sie den **benutzerdefinierte** und die Entwurfsoberfläche zum Layout der Zelle verwenden durch Ziehen in anderen UI Widgets aus der **Toolbox**: 

    [![](table-views-images/table13.png "Das Zellenlayout")](table-views-images/table13.png#lightbox)
1. Weisen Sie einen eindeutigen **Namen** jedes Benutzeroberflächenelement beim Entwurf Prototyp Zelle in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer** , damit Sie später im C#-Code darauf zugreifen können: 

    [![](table-views-images/table14.png "Weisen Sie einen Namen")](table-views-images/table14.png#lightbox)
1. Wiederholen Sie den obigen Schritt für alle Zellen in der Tabellenansicht Prototyp.
1. Als Nächstes weisen Sie benutzerdefinierte Klassen, mit dem Rest der UI-Entwurf Layout der Ansicht und weisen eindeutige **Namen** auf jedes Element der Benutzeroberfläche in den Details anzeigen, damit sie sich auch in c# zugreifen können. Beispiel: 

    [![](table-views-images/table15.png "Das Benutzeroberflächenlayout")](table-views-images/table15.png#lightbox)
1. Speichern Sie die Änderungen auf das Storyboard.
    
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. Klicken Sie in Visual Studio ein neues tvos. außerdem wurden-app-Projekt starten, und wählen **tvos. außerdem wurden** > **einzelne Ansicht App** und geben Sie einen Namen für Ihre app. Klicken Sie auf die **Okay** Schaltfläche, um eine neue Projektmappe zu erstellen: 

    [![](table-views-images/table02-vs.png "Wählen Sie einzelne Sicht App")](table-views-images/table02-vs.png#lightbox)
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie in der iOS-Designer zu öffnen: 

    [![](table-views-images/table05-vs.png "Die Datei Main.storyboard")](table-views-images/table05-vs.png#lightbox)
1. Wählen Sie aus, und löschen Sie die **Standard-View-Controller**: 

    [![](table-views-images/table06-vs.png "Wählen Sie aus, und löschen Sie die Standard-View-Controller")](table-views-images/table06-vs.png#lightbox)
1. Wählen Sie eine **Split-View-Controller** aus der **Toolbox** und ziehen Sie sie auf der Entwurfsoberfläche angezeigt: 

    [![](table-views-images/table07-vs.png "Ein Split-View-Controller")](table-views-images/table07-vs.png#lightbox)
1. Standardmäßig erhalten Sie eine [geteilten Ansicht](~/ios/tvos/user-interface/split-views.md) mit einer **Navigation-View-Controller** und ein **Tabelle-View-Controller** in der linken Seite und ein **View-Controller** in der rechten Seite. Dies ist die empfohlene Verwendung von Apple einer Tabellenansicht in tvos. außerdem wurden: 

    [![](table-views-images/table08-vs.png "Layout der Benutzeroberfläche")](table-views-images/table08-vs.png#lightbox)
1. Sie müssen jeden Teil der Tabellenansicht wählen und weisen sie eine benutzerdefinierte **Klassenname** in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer** , damit Sie später in c# zugreifen zu können Code. Z. B. die **Tabelle-View-Controller**: 

    [![](table-views-images/table09-vs.png "Die Registerkarte "Widget"")](table-views-images/table09-vs.png#lightbox)
1. Stellen Sie sicher, dass es sich bei der Erstellung einer benutzerdefinierten Klasse für die **Tabelle-View-Controller**, die **Tabellenansicht** und ein beliebiger **Prototyp Zellen**. Visual Studio für Mac werden die benutzerdefinierten Klassen der Projekt-Struktur hinzugefügt, wie sie erstellt werden: 

    [![](table-views-images/table10-vs.png "Die benutzerdefinierten Klassen in der Projektstruktur")](table-views-images/table10-vs.png#lightbox)
1. Klicken Sie dann wählen Sie der Tabellenansicht auf der Entwurfsoberfläche aus, und passen Sie bei Bedarf deren Eigenschaften. Z. B. die Anzahl der **Prototyp Zellen** und **Stil** (Plain oder gruppiert): 

    [![](table-views-images/table11-vs.png "Die Registerkarte "Widget"")](table-views-images/table11-vs.png#lightbox)
1. Für jede **Prototyp Zelle**, wählen Sie diese, und weisen Sie einen eindeutigen **Bezeichner** in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer**. Dieser Schritt ist _sehr wichtig_ wie Sie diesen Bezeichner später benötigen, wenn Sie die Tabelle füllen. Z. B. `AttrCell`: 

    [![](table-views-images/table12-vs.png "Zuweisen eines Bezeichners")](table-views-images/table12-vs.png#lightbox)
1. Sie können auch auswählen, an der Zelle als eines der [Standard Zelle Tabellensichttypen](#Table-View-Cell-Types) über die **Stil** Dropdownliste oder legen sie den **benutzerdefinierte** und die Entwurfsoberfläche zum Layout der Zelle verwenden durch Ziehen in anderen UI Widgets aus der **Toolbox**: 

    [![](table-views-images/table13-vs.png "Der Stil-Dropdownliste")](table-views-images/table13-vs.png#lightbox)
1. Weisen Sie einen eindeutigen **Namen** jedes Benutzeroberflächenelement beim Entwurf Prototyp Zelle in der **Widget** auf der Registerkarte die **Eigenschaften-Explorer** , damit Sie später im C#-Code darauf zugreifen können: 

    [![](table-views-images/table14-vs.png "Die Registerkarte "Widget"")](table-views-images/table14-vs.png#lightbox)
1. Wiederholen Sie den obigen Schritt für alle Zellen in der Tabellenansicht Prototyp.
1. Als Nächstes weisen Sie benutzerdefinierte Klassen, mit dem Rest der UI-Entwurf Layout der Ansicht und weisen eindeutige **Namen** auf jedes Element der Benutzeroberfläche in den Details anzeigen, damit sie sich auch in c# zugreifen können. Beispiel: 

    [![](table-views-images/table15.png "Das Benutzeroberflächenlayout")](table-views-images/table15.png#lightbox)
1. Speichern Sie die Änderungen auf das Storyboard.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Ein Datenmodell

Arbeiten mit den Informationen vornehmen möchten, die einfachere der Tabellenansicht angezeigt werden und die Präsentation mit detaillierten Informationen (wie der Benutzer auswählt oder Zeilen in der Tabellenansicht hervorgehoben) vereinfachen, erstellen Sie eine benutzerdefinierte Klasse oder Klassen fungieren als Datenmodell für die Informationen angezeigt .

Nehmen Sie als Beispiel einer Reisen übernehmenden-App, die eine Liste von enthält **Städte**, jeder, der eine eindeutige Liste von enthält **Attraktionen** , die der Benutzer auswählen kann. Der Benutzer wird in der Lage, eine Attraktion als markieren eine *bevorzugten*abzurufenden Option *Richtungen* auf eine Attraktion und *Book einen Flug* mit einem angegebenen Ort.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

So erstellen das Datenmodell für eine **Attraktion**, mit der rechten Maustaste auf den Projektnamen in der **Lösung Pad** , und wählen Sie **hinzufügen** > **neue Datei...** . Geben Sie `AttractionInformation` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

[![](table-views-images/data01.png "Geben Sie ein AttractionInformation")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

So erstellen das Datenmodell für eine **Attraktion**, mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Element ...** . Wählen Sie **Klasse** , und geben Sie `AttractionInformation` für die **Namen** , und klicken Sie auf die **hinzufügen** Schaltfläche: 

[![](table-views-images/data01-vs.png "Wählen Sie Klasse aus, und geben Sie ein AttractionInformation")](table-views-images/data01-vs.png#lightbox)

-----

Bearbeiten der `AttractionInformation.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;

namespace tvTable
{
    public class AttractionInformation : NSObject
    {
        #region Computed Properties
        public CityInformation City { get; set;}
        public string Name { get; set;}
        public string Description { get; set;}
        public string ImageName { get; set;}
        public bool IsFavorite { get; set;}
        public bool AddDirections { get; set;}
        #endregion

        #region Constructors
        public AttractionInformation (string name, string description, string imageName)
        {
            // Initialize
            this.Name = name;
            this.Description = description;
            this.ImageName = imageName;
        }
        #endregion
    }
}
```

Diese Klasse stellt die Eigenschaften zum Speichern von Informationen zu einem bestimmten **Attraktion**.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio für Mac](#tab/vsmac)

Als Nächstes mit der rechten Maustaste auf den Projektnamen in der **Lösung Pad** erneut aus, und wählen Sie **hinzufügen** > **neue Datei...** . Geben Sie `CityInformation` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

[![](table-views-images/data02.png "Geben Sie ein CityInformation")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Als Nächstes mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer** erneut aus, und wählen Sie **hinzufügen** > **neues Element...** . Geben Sie `CityInformation` für die **Namen** , und klicken Sie auf die **hinzufügen** Schaltfläche: 

[![](table-views-images/data02-vs.png "Geben Sie ein CityInformation")](table-views-images/data02-vs.png#lightbox)

-----

Bearbeiten der `CityInformation.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using System.Collections.Generic;
using Foundation;

namespace tvTable
{
    public class CityInformation : NSObject
    {
        #region Computed Properties
        public string Name { get; set; }
        public List<AttractionInformation> Attractions { get; set;}
        public bool FlightBooked { get; set;}
        #endregion

        #region Constructors
        public CityInformation (string name)
        {
            // Initialize
            this.Name = name;
            this.Attractions = new List<AttractionInformation> ();
        }
        #endregion

        #region Public Methods
        public void AddAttraction (AttractionInformation attraction)
        {
            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }

        public void AddAttraction (string name, string description, string imageName)
        {
            // Create attraction
            var attraction = new AttractionInformation (name, description, imageName);

            // Mark as belonging to this city
            attraction.City = this;

            // Add to collection
            Attractions.Add (attraction);
        }
        #endregion
    }
}
```

Diese Klasse enthält alle Informationen über ein Ziel **City**, eine Auflistung von **Attraktionen** für diese Stadt und bietet zwei Hilfsmethoden (`AddAttraction`) erleichtern Attraktionen zum Hinzufügen der Stadt.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>Die Datenquelle der Tabelle anzeigen

Jede Tabellenansicht erfordert eine Datenquelle (`UITableViewDataSource`) geben die Daten für die Tabelle, und generieren die erforderlichen Zeilen als Tabellenansicht erforderlich.

Im obigen Beispiel mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer**Option **hinzufügen** > **neue Datei...**  und Aufruf der `AttractionTableDatasource` , und klicken Sie auf die **neu** Schaltfläche zu erstellen. Als Nächstes Bearbeiten der `AttractionTableDatasource.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDatasource : UITableViewDataSource
    {
        #region Constants
        const string CellID = "AttrCell";
        #endregion

        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        public List<CityInformation> Cities { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDatasource (AttractionTableViewController controller)
        {
            // Initialize
            this.Controller = controller;
            this.Cities = new List<CityInformation> ();
            PopulateCities ();
        }
        #endregion

        #region Public Methods
        public void PopulateCities ()
        {
            // Clear existing
            Cities.Clear ();

            // Define cities and attractions
            var Paris = new CityInformation ("Paris");
            Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
            Paris.AddAttraction ("Musée du Louvre", "is one of the world's largest museums and a historic monument in Paris, France.", "Louvre");
            Paris.AddAttraction ("Moulin Rouge", "French for 'Red Mill', is a cabaret in Paris, France.", "MoulinRouge");
            Paris.AddAttraction ("La Seine", "Is a 777-kilometre long river and an important commercial waterway within the Paris Basin.", "RiverSeine");
            Cities.Add (Paris);

            var SanFran = new CityInformation ("San Francisco");
            SanFran.AddAttraction ("Alcatraz Island", "Is located in the San Francisco Bay, 1.25 miles (2.01 km) offshore from San Francisco.", "Alcatraz");
            SanFran.AddAttraction ("Golden Gate Bridge", "Is a suspension bridge spanning the Golden Gate strait between San Francisco Bay and the Pacific Ocean", "GoldenGateBridge");
            SanFran.AddAttraction ("San Francisco", "Is the cultural, commercial, and financial center of Northern California.", "SanFrancisco");
            SanFran.AddAttraction ("Telegraph Hill", "Is primarily a residential area, much quieter than adjoining North Beach.", "TelegraphHill");
            Cities.Add (SanFran);

            var Houston = new CityInformation ("Houston");
            Houston.AddAttraction ("City Hall", "It was constructed in 1938-1939, and is located in Downtown Houston.", "CityHall");
            Houston.AddAttraction ("Houston", "Is the most populous city in Texas and the fourth most populous city in the US.", "Houston");
            Houston.AddAttraction ("Texas Longhorn", "Is a breed of cattle known for its characteristic horns, which can extend to over 6 ft.", "LonghornCattle");
            Houston.AddAttraction ("Saturn V Rocket", "was an American human-rated expendable rocket used by NASA between 1966 and 1973.", "Rocket");
            Cities.Add (Houston);
        }
        #endregion

        #region Override Methods
        public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Get cell
            var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

            // Populate cell
            cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

            // Return new cell
            return cell;
        }

        public override nint NumberOfSections (UITableView tableView)
        {
            // Return number of cities
            return Cities.Count;
        }

        public override nint RowsInSection (UITableView tableView, nint section)
        {
            // Return the number of attractions in the given city
            return Cities [(int)section].Attractions.Count;
        }

        public override string TitleForHeader (UITableView tableView, nint section)
        {
            // Get the name of the current city
            return Cities [(int)section].Name;
        }
        #endregion
    }
}
```

Werfen wir einen Blick auf einige Abschnitte der Klasse im Detail.

Wir haben zuerst definiert eine Konstante, um den eindeutigen Bezeichner der Prototyp Zelle speichern (Dies ist dieselbe ID zugewiesen, die in der oben genannten Benutzeroberflächen-Designer), eine Verknüpfung zurück zur Tabelle-View-Controller hinzugefügt und Speicher für unsere Daten erstellt:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Als Nächstes wir Speichern der Tabelle-View-Controller und erstellen und Auffüllen unsere Datenquelle (mithilfe der oben definierten Datenmodelle) bei der Erstellung der Klasse:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Beispiel der `PopulateCities` Methode erstellt einfach Datenmodellobjekte im Arbeitsspeicher, aber diese problemlos aus einer Datenbank oder Web-Dienst in eine wirkliche app gelesen werden konnte:

```csharp
public void PopulateCities ()
{
    // Clear existing
    Cities.Clear ();

    // Define cities and attractions
    var Paris = new CityInformation ("Paris");
    Paris.AddAttraction ("Eiffel Tower", "Is a wrought iron lattice tower on the Champ de Mars in Paris, France.", "EiffelTower");
    ...
}
```

Die `NumberOfSections` Methode gibt die Anzahl der Abschnitte in der Tabelle zurück:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Für **Plain** formatiert Tabellensichten, immer 1 zurück.

Die `RowsInSection` Methode gibt die Anzahl der Zeilen im aktuellen Abschnitt:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

In diesem Fall für **Plain** Tabellensichten, die Gesamtanzahl von Elementen in der Datenquelle zurückgeben.

Die `TitleForHeader` Methodenrückgabe den Titel für den angegebenen Abschnitt:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Für eine **Plain** Tabellenansicht eingeben, lassen Sie den Titel leer (`""`).

Erstellen Sie abschließend, wenn durch die Tabellenansicht angefordert, und füllen ein Prototyp Zelle mit dem `GetCell` Methode: 

```csharp
public override UITableViewCell GetCell (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Get cell
    var cell = tableView.DequeueReusableCell (CellID) as AttractionTableCell;

    // Populate cell
    cell.Attraction = Cities [indexPath.Section].Attractions [indexPath.Row];

    // Return new cell
    return cell;
}
```

Weitere Informationen zum Arbeiten mit einem `UITableViewDatasource`, finden Sie in der Apple- [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) Dokumentation.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Der Delegat der Tabelle anzeigen

Jede Ansicht Tabelle erfordert einen Delegaten (`UITableViewDelegate`) Reaktion auf Benutzerinteraktion oder andere Systemereignisse, die für die Tabelle.

Im obigen Beispiel mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer**Option **hinzufügen** > **neue Datei...**  und Aufruf der `AttractionTableDelegate` , und klicken Sie auf die **neu** Schaltfläche zu erstellen. Als Nächstes Bearbeiten der `AttractionTableDelegate.cs` Datei, und stellen sie wie folgt aussehen:

```csharp
using System;
using System.Collections.Generic;
using UIKit;

namespace tvTable
{
    public class AttractionTableDelegate : UITableViewDelegate
    {
        #region Computed Properties
        public AttractionTableViewController Controller { get; set;}
        #endregion

        #region Constructors
        public AttractionTableDelegate (AttractionTableViewController controller)
        {
            // Initializw
            this.Controller = controller;
        }
        #endregion

        #region Override Methods
        public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
            attraction.IsFavorite = (!attraction.IsFavorite);

            // Update UI
            Controller.TableView.ReloadData ();
        }

        public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
        {
            // Inform caller of highlight change
            RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
            return true;
        }
        #endregion

        #region Events
        public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
        public event AttractionHighlightedDelegate AttractionHighlighted;

        internal void RaiseAttractionHighlighted (AttractionInformation attraction)
        {
            // Inform caller
            if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
        }
        #endregion
    }
}
```

Werfen wir einen Blick auf einige Abschnitte dieser Klasse in den Details.

Zunächst erstellen wir eine Verknüpfung zu Tabelle-View-Controller auf, wenn die Klasse erstellt wird:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Klicken Sie dann, wenn eine Zeile ausgewählt ist (der Benutzer klickt auf die Touch-Oberfläche des Apple Remote) es markieren möchten die **Attraktion** durch die ausgewählte Zeile als einen Favoriten dargestellt:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Weiter, wenn der Benutzer eine Zeile (durch die Vergabe es den Fokus mit der Apple Remote-Touch-Oberfläche) hervorgehoben dargestellt werden sollen die Details der **Attraktion** durch diese Zeile im Detailbereich mit unserer Split-View-Controller dargestellt:

```csharp
public override bool CanFocusRow (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    // Inform caller of highlight change
    RaiseAttractionHighlighted (Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row]);
    return true;
}
...

public delegate void AttractionHighlightedDelegate (AttractionInformation attraction);
public event AttractionHighlightedDelegate AttractionHighlighted;

internal void RaiseAttractionHighlighted (AttractionInformation attraction)
{
    // Inform caller
    if (this.AttractionHighlighted != null) this.AttractionHighlighted (attraction);
}
```

Die `CanFocusRow` Methode wird aufgerufen, für jede Zeile, die Fokus in der Tabellenansicht abrufen. Zurückgeben `true` , wenn die Zeile den Fokus erhalten kann, andernfalls zurückgeben `false`. Bei diesem Beispiel haben wir eine benutzerdefinierte `AttractionHighlighted` Ereignis, das in jeder Zeile ausgelöst wird, wie es den Fokus erhält.

Weitere Informationen zum Arbeiten mit einem `UITableViewDelegate`, finden Sie in der Apple- [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) Dokumentation.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>Die Ansicht Tabellenzelle

Für jede Prototyp-Zelle, die die Tabellenansicht im Designer-Schnittstelle hinzugefügt wurden, haben Sie auch eine benutzerdefinierte Instanz der Tabellenzelle Ansicht erstellt (`UITableViewCell`) können Sie die neue Zelle (Zeile) aufgefüllt werden soll, wie es erstellt wird.

Der Beispiel-app, doppelklicken Sie auf die `AttractionTableCell.cs` Datei zur Bearbeitung öffnen, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableCell : UITableViewCell
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }
        #endregion

        #region Constructors
        public AttractionTableCell (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Private Methods
        private void UpdateUI ()
        {
            // Trap all errors
            try {
                Title.Text = Attraction.Name;
                Favorite.Hidden = (!Attraction.IsFavorite);
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion
    }
}
```

Diese Klasse stellt Speicher für das Datenmodell Attraktion-Objekt (`AttractionInformation` wie oben definiert) in der angegebenen Zeile angezeigt:

```csharp
private AttractionInformation _attraction = null;
...

public AttractionInformation Attraction {
    get { return _attraction; }
    set {
        _attraction = value;
        UpdateUI ();
    }
}
```

Die `UpdateUI` Methode füllt die UI-Widgets (die auf die Zelle Prototyp in der Benutzeroberflächen-Designer hinzugefügt wurden) nach Bedarf:

```csharp
private void UpdateUI ()
{
    // Trap all errors
    try {
        Title.Text = Attraction.Name;
        Favorite.Hidden = (!Attraction.IsFavorite);
    } catch {
        // Since the UI might not be fully loaded, ignore
        // all errors at this point
    }
}
```

Weitere Informationen zum Arbeiten mit einem `UITableViewCell`, finden Sie in der Apple- [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) Dokumentation.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Die Tabelle-View-Controller

Eine Tabelle-View-Controller (`UITableViewController`) verwaltet eine Tabelle anzeigen, die an einem Storyboard über den Benutzeroberflächen-Designer hinzugefügt wurde.

Der Beispiel-app, doppelklicken Sie auf die `AttractionTableViewController.cs` Datei zur Bearbeitung öffnen, und stellen sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionTableViewController : UITableViewController
    {
        #region Computed Properties
        public AttractionTableDatasource Datasource {
            get { return TableView.DataSource as AttractionTableDatasource; }
        }

        public AttractionTableDelegate TableDelegate {
            get { return TableView.Delegate as AttractionTableDelegate; }
        }
        #endregion

        #region Constructors
        public AttractionTableViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Setup table
            TableView.DataSource = new AttractionTableDatasource (this);
            TableView.Delegate = new AttractionTableDelegate (this);
            TableView.ReloadData ();
        }
        #endregion
    }
}
```

Werfen Sie näher auf diese Klasse an. Zuerst wurde erstellt, Verknüpfungen aus, um den Zugriff auf die Tabellenansicht vereinfachen `DataSource` und `TableDelegate`. Wir werden diese später verwenden, für die Kommunikation zwischen der Tabellenansicht auf der linken Seite der geteilten Ansicht und die Detailansicht in der rechten Ecke.

Wenn der Tabellenansicht in den Arbeitsspeicher geladen wird, wir erstellen Sie abschließend Instanzen von der `AttractionTableDatasource` und `AttractionTableDelegate` (beide oben erstellten) und fügen Sie sie an der Tabellenansicht.

Weitere Informationen zum Arbeiten mit einem `UITableViewController`, finden Sie in der Apple- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) Dokumentation.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Ziehen sie alle zusammen

Wie zu Beginn dieses Dokuments angegeben wird, werden die Tabellensichten in der Regel in einer Seite eines angezeigt ein [geteilten Ansicht](~/ios/tvos/user-interface/split-views.md) als Navigation mit den Details des ausgewählten Elements in der entgegengesetzten Seite angezeigt. Zum Beispiel: 

[![](table-views-images/intro01.png "Beispiel-app ausführen")](table-views-images/intro01.png#lightbox)

Da dies ein Standardmuster in tvos. außerdem wurden ist, sehen wir uns die abschließenden Schritte zum alles zusammenführen und die linken und rechten Seiten der geteilten Ansicht miteinander interagieren.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>Die Detailansicht

Für das Beispiel des Reise-app angezeigt, über eine benutzerdefinierte Klasse (`AttractionViewController`) für die standardmäßige View-Controller auf der rechten Seite der geteilten Ansicht als der Detailansicht angezeigt definiert ist:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class AttractionViewController : UIViewController
    {
        #region Private Variables
        private AttractionInformation _attraction = null;
        #endregion

        #region Computed Properties
        public AttractionInformation Attraction {
            get { return _attraction; }
            set {
                _attraction = value;
                UpdateUI ();
            }
        }

        public MasterSplitView SplitView { get; set;}
        #endregion

        #region Constructors
        public AttractionViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Public Methods
        public void UpdateUI ()
        {
            // Trap all errors
            try {
                City.Text = Attraction.City.Name;
                Title.Text = Attraction.Name;
                SubTitle.Text = Attraction.Description;

                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
                IsFavorite.Hidden = (!Attraction.IsFavorite);
                IsDirections.Hidden = (!Attraction.AddDirections);
                BackgroundImage.Image = UIImage.FromBundle (Attraction.ImageName);
                AttractionImage.Image = BackgroundImage.Image;
            } catch {
                // Since the UI might not be fully loaded, ignore
                // all errors at this point
            }
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear (bool animated)
        {
            base.ViewWillAppear (animated);

            // Ensure the UI Updates
            UpdateUI ();
        }
        #endregion

        #region Actions
        partial void BookFlight (NSObject sender)
        {
            // Ask user to book flight
            AlertViewController.PresentOKCancelAlert ("Book Flight",
                                                      string.Format ("Would you like to book a flight to {0}?", Attraction.City.Name),
                                                      this,
                                                      (ok) => {
                Attraction.City.FlightBooked = ok;
                IsFlighBooked.Hidden = (!Attraction.City.FlightBooked);
            });
        }

        partial void GetDirections (NSObject sender)
        {
            // Ask user to add directions
            AlertViewController.PresentOKCancelAlert ("Add Directions",
                                                     string.Format ("Would you like to add directions to {0} to you itinerary?", Attraction.Name),
                                                     this,
                                                     (ok) => {
                                                         Attraction.AddDirections = ok;
                                                         IsDirections.Hidden = (!Attraction.AddDirections);
                                                     });
        }

        partial void MarkFavorite (NSObject sender)
        {
            // Flip favorite state
            Attraction.IsFavorite = (!Attraction.IsFavorite);
            IsFavorite.Hidden = (!Attraction.IsFavorite);

            // Reload table
            SplitView.Master.TableController.TableView.ReloadData ();
        }
        #endregion
    }
}
```

Hier haben wir angegeben die **Attraktion** (`AttractionInformation`) als eine Eigenschaft angezeigt wird, und erstellt eine `UpdateUI` -Methode, die die UI-Widgets füllt die Ansicht in der Benutzeroberflächen-Designer hinzugefügt.

Wir haben auch eine Verknüpfung definiert, an der Split-View-Controller (`SplitView`), das wir zum Kommunizieren verwenden ändert die Tabellenansicht an (`AcctractionTableView`).

Schließlich wurden benutzerdefinierte Aktionen (Ereignisse) hinzugefügt, um die drei `UIButton` Instanzen in der Benutzeroberflächen-Designer erstellt wurden, mit denen den Benutzer eine Attraktion als Markieren einer _bevorzugten_, abrufen _Richtungen_ auf ein Attraktion und _Book einen Flug_ mit einem angegebenen Ort.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Der Navigationsbereich-View-Controller

Da die Tabelle-View-Controller in einem Navigation-View-Controller auf der linken Seite der geteilten Ansicht geschachtelt ist, wurde die Navigation-View-Controller eine benutzerdefinierte Klasse zugewiesen (`MasterNavigationController`) in der Benutzeroberflächen-Designer und wie folgt definiert:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterNavigationController : UINavigationController
    {
        #region Computed Properties
        public MasterSplitView SplitView { get; set;}
        public AttractionTableViewController TableController {
            get { return TopViewController as AttractionTableViewController; }
        }
        #endregion

        #region Constructors
        public MasterNavigationController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Diese Klasse definiert erneut aus, nur einige Tastenkombinationen, um über die beiden Seiten der Split-View-Controller kommunizieren vereinfachen:

* `SplitView` -Ist ein Link zu der Split-View-Controller (`MainSpiltViewController`), das die Navigation-View-Controller zu gehört.
* `TableController` -Ruft die Tabelle-View-Controller (`AttractionTableViewController`), die als die Top-Ansicht im Navigationsbereich-View-Controller dargestellt wird.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Der Split-View-Controller

Da der Split-View-Controller die Basis der Anwendung ist, erstellt es eine benutzerdefinierte Klasse (`MasterSplitViewController`) dafür in der Benutzeroberflächen-Designer, und es wie folgt definiert:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvTable
{
    public partial class MasterSplitView : UISplitViewController
    {
        #region Computed Properties
        public AttractionViewController Details {
            get { return ViewControllers [1] as AttractionViewController; }
        }

        public MasterNavigationController Master {
            get { return ViewControllers [0] as MasterNavigationController; }
        }
        #endregion

        #region Constructors
        public MasterSplitView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Initialize
            Master.SplitView = this;
            Details.SplitView = this;

            // Wire-up events
            Master.TableController.TableDelegate.AttractionHighlighted += (attraction) => {
                // Display new attraction
                Details.Attraction = attraction;
            };
        }
        #endregion
    }
}
```

Zuerst, wir Erstellen von Verknüpfungen zu der **Details** -Seite der geteilten Ansicht (`AttractionViewController`) und die **Master** Side (`MasterNavigationController`). Erneut, erleichtert dies für die Kommunikation zwischen den beiden Seiten später.

Als Nächstes, wenn der geteilten Ansicht in den Arbeitsspeicher geladen wird, es Anfügen der Split-View-Controller auf beiden Seiten der geteilten Ansicht und reagieren, die dem Benutzer eine Attraktion in der Tabellenansicht Hervorhebung (`AttractionHighlighted`) durch Anzeigen der neuen Attraktion in die **Details**  -Seite in der Ansicht teilen.

Finden Sie unter der [TvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) Beispiel-app für eine vollständige Implementierung der Tabellensichten innerhalb einer geteilten Ansicht.

## <a name="table-views-in-detail"></a>Tabellenansichten im Detail

Da tvos. außerdem wurden aus iOS basiert, Tabellensichten Tabelle-View-Controller dienen und in ähnlicher Weise Verhalten. Ausführlichere Informationen zum Arbeiten mit Tabellenansicht in einer Xamarin-app finden Sie unter unserer iOS [arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Tabellensichten innerhalb einer Xamarin.tvOS-app. Verfügt über ein Beispiel der Arbeit mit einer Tabellenansicht innerhalb einer geteilten Ansicht, die von einer Tabellenansicht in einer app für tvos. außerdem wurden die typische Nutzung darin angezeigt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos. außerdem wurden Handbücher für interaktive Workflowdienste-Schnittstelle](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos. außerdem wurden](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
