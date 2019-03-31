---
title: Arbeiten mit TvOS Tabellenansichten in Xamarin
description: Dieser Artikel behandelt das Entwerfen von und Arbeiten mit Tabellenansichten "und" Table View Controllers, in ein Xamarin.tvOS-app.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 3e7fc3d627b5d7a1dc73caa395a9181efb0b5f08
ms.sourcegitcommit: 946ce514fd6575aa6b93ff24181e02a60b24b106
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/30/2019
ms.locfileid: "58678001"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>Arbeiten mit TvOS Tabellenansichten in Xamarin

_Dieser Artikel behandelt das Entwerfen von und Arbeiten mit Tabellenansichten "und" Table View Controllers, in ein Xamarin.tvOS-app._

In TvOS wird eine Tabellenansicht angezeigt, als eine einzelne Spalte des Bildlaufs an Zeilen, die optional in Gruppen oder Abschnitten organisiert werden können. Tabellenansichten sollte verwendet werden, wenn eine große Menge von Daten effizient für den Benutzer in ein eindeutiges verständliche Weise angezeigt werden müssen.

Tabellenansichten in der Regel in einer Seite angezeigt werden eine [geteilte Ansicht](~/ios/tvos/user-interface/split-views.md) wie Navigation, mit den Details des ausgewählten Elements in der entgegengesetzten Seite angezeigt:

[![](table-views-images/intro01.png "Beispiel-Tabellenansicht")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>Über Tabellenansichten

Ein `UITableView` zeigt von einer einzelnen Spalte des bildlauffähigen Zeilen als eine hierarchische Liste von Informationen, die optional in Gruppen oder Abschnitten organisiert werden kann: 

[![](table-views-images/table01.png "Ein ausgewähltes Element")](table-views-images/table01.png#lightbox)

Apple hat die folgenden Vorschläge für die Arbeit mit Tabellen:

- **Kennen der Breite werden** -versuchen Sie es, das richtige Gleichgewicht in Ihrer Tabellenbreiten zu finden. Wenn die Tabelle zu breit ist, kann es kann schwierig sein, die aus der Entfernung zu überprüfen und von den verfügbaren Inhaltsbereich. Wenn die Tabelle nicht breit genug ist, kann kann es dazu führen, dass die Informationen an, die abgeschnitten werden oder Zeilenumbruch, erneut dies für den Benutzer zum Lesen aus durch das Zimmer schwierig.
- **Tabelle Inhalte schnell anzeigen** – für umfangreiche Listen der Daten, verzögerten Laden des Inhalts und starten Sie die Informationen angezeigt, sobald die Tabelle, die dem Benutzer angezeigt wird. Wenn die Tabelle laden zu lange dauert, der Benutzer verlieren möglicherweise Interesse an Ihrer app oder stellen Sie sich, die es gesperrt wird.
- **Informieren Sie Benutzer von langen Inhalt lädt** : bei einer langen Tabelle-Ladezeit unvermeidbar, vorhanden ist eine [Statusanzeige oder Aktivitätsanzeige](~/ios/tvos/user-interface/progress-indicators.md) , damit sie wissen, dass die app noch nicht gesperrt.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Tabellensichttypen Zelle

Ein `UITableViewCell` wird verwendet, um die einzelnen Zeilen von Daten in der Tabellenansicht darstellen. Apple hat mehrere standardmäßige Tabellentypen Zelle definiert:

- **Standard** – dieser Typ stellt eine Option Bild auf der linken Seite der Zelle und linksbündig Titel auf der rechten Seite. 
- **Untertitel** – dieser Typ stellt ein Links ausgerichtete Titel auf der ersten Zeile und eine kleinere linksbündig Untertitel in der nächsten Zeile.
- **Der Wert 1** -dieser Typ stellt einen Titel links ausgerichtete mit einem Untertitel heller farbigen, rechtsbündig in derselben Zeile.
- **Der Wert 2** -dieser Typ stellt einen Titel rechtsbündig ausgerichtet, mit einem Untertitel heller farbigen, linksbündig in derselben Zeile.

Außerdem unterstützen alle die Standard-Zelle für Tabellensichttypen grafische Elemente wie z. B. Offenlegung von Indikatoren oder Häkchen. 

Darüber hinaus können Sie definieren eine **benutzerdefinierte** Tabellenansicht Zelle und vorhanden eine _Prototyp Zelle_, entweder im Benutzeroberflächen-Designer oder über den Code zu erstellen.

Apple hat die folgenden Vorschläge für die Arbeit mit Zellen der Tabelle anzeigen:

- **Vermeiden Sie Text zu kürzen** -beibehalten kurzer einzelnen Textzeilen, damit sie letztlich nicht abgeschnitten. Abgeschnittene Wörter oder Ausdrücke sind für den Benutzer zum Analysieren von durch das Zimmer schwierig.
- **Betrachten Sie den Zustand "fokussiert Zeile"** : Da Sie eine Zeile größer ist, mit mehr gerundet wird Ecken im Fokus, müssen Sie Ihre zellendarstellung des in allen Zuständen zu testen. Bilder oder Text möglicherweise abgeschnitten werden, oder suchen Sie in den Zustand "fokussiert" falsch.
- **Bearbeitbare Tabellen nur selten verwenden** -verschieben oder Löschen von Zeilen ist zeitaufwändiger unter TvOS als iOS. Sie müssen sorgfältig entscheiden, ob dieses Feature führt zum Hinzufügen oder von TvOS-app abgelenkt werden.
- **Erstellen Sie benutzerdefinierte Zelle Typen, in dem entsprechenden** – zwar integrierte Zelle Tabellensichttypen in vielen Situationen geeignet sind sollten Sie benutzerdefinierte Zellentypen nicht standardmäßige Informationen bieten eine bessere Kontrolle und bessere stellen die Informationen zum Erstellen der Benutzer.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Arbeiten mit Tabellenansichten

Die einfachste Möglichkeit zum Arbeiten mit Tabellenansichten in einer Xamarin.tvOS-app ist ihre Darstellung im Benutzeroberflächen-Designer zu erstellen.

Um zu beginnen, machen Sie Folgendes:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)
    
1. Klicken Sie in Visual Studio für Mac starten ein neues TvOS app-Projekt, und wählen **TvOS** > **App** > **Einzelansicht-App** , und klicken Sie auf die  **Nächste** Schaltfläche: 

    [![](table-views-images/table02.png "Ein Einzelansicht-App auswählen")](table-views-images/table02.png#lightbox)
1. Geben Sie einen **Namen** für die app und klicken Sie auf **Weiter**: 

    [![](table-views-images/table03.png "Geben Sie einen Namen für die app")](table-views-images/table03.png#lightbox)
1. Passen Sie entweder die **Projektname** und **Projektmappenname** oder übernehmen Sie die Standardeinstellungen, und klicken Sie auf die **erstellen** klicken, um die neue Projektmappe zu erstellen: 

    [![](table-views-images/table04.png "Der Projektname und die Namen der Projektmappe")](table-views-images/table04.png#lightbox)
1. In der **Lösungspad**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie in der iOS-Designer zu öffnen: 

    [![](table-views-images/table05.png "Die Datei \"Main.storyboard\"")](table-views-images/table05.png#lightbox)
1. Wählen Sie aus, und löschen Sie die **Standard-View-Controller**: 

    [![](table-views-images/table06.png "Wählen Sie aus, und löschen Sie den Standard-View-Controller")](table-views-images/table06.png#lightbox)
1. Wählen Sie eine **Controller für geteilte Ansicht** aus der **Toolbox** und ziehen Sie es auf die Entwurfsoberfläche.
1. Standardmäßig erhalten Sie eine [geteilte Ansicht](~/ios/tvos/user-interface/split-views.md) mit einer **Navigation-View-Controller** und ein **Tabellenansichtscontroller** in der linken Seite und einen **Ansichtscontroller** in der rechten Seite. Dies ist die empfohlene Verwendung von Apple, von einer Tabellensicht in TvOS: 

    [![](table-views-images/table08.png "Fügen Sie eine geteilte Ansicht")](table-views-images/table08.png#lightbox)
1. Sie benötigen, wählen Sie jeden Teil der Tabellenansicht und weisen Sie sie ein benutzerdefiniertes **Klassenname** in die **Widget** Registerkarte die **Eigenschaften-Explorer** , damit Sie später in C#Code. Z. B. die **Tabellenansichtscontroller**: 

    [![](table-views-images/table09.png "Weisen Sie einen Klassennamen")](table-views-images/table09.png#lightbox)
1. Stellen Sie sicher, dass Sie eine benutzerdefinierte Klasse zum Erstellen der **Tabellenansichtscontroller**, **Tabellenansicht** sowie **Prototyp Zellen**. Visual Studio für Mac wird von der benutzerdefinierten Klassen der Projektstruktur hinzugefügt, wie sie erstellt werden: 

    [![](table-views-images/table10.png "Die benutzerdefinierten Klassen in der Projektstruktur")](table-views-images/table10.png#lightbox)
1. Klicken Sie dann wählen Sie der Tabellenansicht auf der Entwurfsoberfläche aus, und passen Sie dessen Eigenschaften nach Bedarf. Wie die Anzahl der **Prototyp Zellen** und **Stil** ("Plain" oder "Group"): 

    [![](table-views-images/table11.png "Die Registerkarte \"Widget\"")](table-views-images/table11.png#lightbox)
1. Für jede **Prototyp Zelle**, wählen Sie ihn, und weist eine eindeutige **Bezeichner** in die **Widget** Registerkarte die **Eigenschaften-Explorer**. Dieser Schritt ist _sehr wichtig_ , wie Sie diesen Bezeichner später benötigen, wenn Sie füllen Sie die Tabelle. Z. B. `AttrCell`: 

    [![](table-views-images/table12.png "Die Registerkarte \"Widget\"")](table-views-images/table12.png#lightbox)
1. Sie können auch auswählen, zur Darstellung der Zelle als eines der [Standard Zelle Tabellensichttypen](#table-view-cell-types) über die **Stil** Dropdownliste oder legen ihn auf **benutzerdefinierte** und verwenden Sie die Entwurfsoberfläche, das Layout der Zelle durch Ziehen in andere UI-Widgets aus dem **Toolbox**: 

    [![](table-views-images/table13.png "Das Zellenlayout")](table-views-images/table13.png#lightbox)
1. Weist eine eindeutige **Namen** auf jedes Element der Benutzeroberfläche beim Entwurf Prototyp Zelle in der **Widget** Registerkarte die **Eigenschaften-Explorer** damit Sie sie später unter zugreifen können C# Code: 

    [![](table-views-images/table14.png "Weisen Sie einen Namen")](table-views-images/table14.png#lightbox)
1. Wiederholen Sie die oben genannten Schritte für alle Zellen in der Tabellenansicht Prototyp aus.
1. Als Nächstes weisen Sie benutzerdefinierte Klassen für den Rest der Entwurf von Benutzeroberflächen, Layout der Detailansicht und weisen eindeutige **Namen** auf jedes Element der Benutzeroberfläche in den Details anzeigen, damit Sie diese im zugreifen können C# ebenfalls. Beispiel: 

    [![](table-views-images/table15.png "Das UI-layout")](table-views-images/table15.png#lightbox)
1. Speichern Sie die Änderungen auf das Storyboard an.
    
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)
    
1. Klicken Sie in Visual Studio starten ein neues TvOS app-Projekt, und wählen **TvOS** > **Einzelansicht-App** , und geben Sie einen Namen für Ihre app. Klicken Sie auf die **OK** klicken, um eine neue Projektmappe zu erstellen: 

    [![](table-views-images/table02-vs.png "Ein Einzelansicht-App auswählen")](table-views-images/table02-vs.png#lightbox)
1. In der **Projektmappen-Explorer**, doppelklicken Sie auf die `Main.storyboard` Datei, die sie in der iOS-Designer zu öffnen: 

    [![](table-views-images/table05-vs.png "Die Datei \"Main.storyboard\"")](table-views-images/table05-vs.png#lightbox)
1. Wählen Sie aus, und löschen Sie die **Standard-View-Controller**: 

    [![](table-views-images/table06-vs.png "Wählen Sie aus, und löschen Sie den Standard-View-Controller")](table-views-images/table06-vs.png#lightbox)
1. Wählen Sie eine **Controller für geteilte Ansicht** aus der **Toolbox** und ziehen Sie es auf die Entwurfsoberfläche: 

    [![](table-views-images/table07-vs.png "Ein Controller für geteilte Ansicht")](table-views-images/table07-vs.png#lightbox)
1. Standardmäßig erhalten Sie eine [geteilte Ansicht](~/ios/tvos/user-interface/split-views.md) mit einer **Navigation-View-Controller** und ein **Tabellenansichtscontroller** in der linken Seite und einen **Ansichtscontroller** in der rechten Seite. Dies ist die empfohlene Verwendung von Apple, von einer Tabellensicht in TvOS: 

    [![](table-views-images/table08-vs.png "Layout der Benutzeroberfläche")](table-views-images/table08-vs.png#lightbox)
1. Sie benötigen, wählen Sie jeden Teil der Tabellenansicht und weisen Sie sie ein benutzerdefiniertes **Klassenname** in die **Widget** Registerkarte die **Eigenschaften-Explorer** , damit Sie später in C#Code. Z. B. die **Tabellenansichtscontroller**: 

    [![](table-views-images/table09-vs.png "Die Registerkarte \"Widget\"")](table-views-images/table09-vs.png#lightbox)
1. Stellen Sie sicher, dass Sie eine benutzerdefinierte Klasse zum Erstellen der **Tabellenansichtscontroller**, **Tabellenansicht** sowie **Prototyp Zellen**. Visual Studio für Mac wird von der benutzerdefinierten Klassen der Projektstruktur hinzugefügt, wie sie erstellt werden: 

    [![](table-views-images/table10-vs.png "Die benutzerdefinierten Klassen in der Projektstruktur")](table-views-images/table10-vs.png#lightbox)
1. Klicken Sie dann wählen Sie der Tabellenansicht auf der Entwurfsoberfläche aus, und passen Sie dessen Eigenschaften nach Bedarf. Wie die Anzahl der **Prototyp Zellen** und **Stil** ("Plain" oder "Group"): 

    [![](table-views-images/table11-vs.png "Die Registerkarte \"Widget\"")](table-views-images/table11-vs.png#lightbox)
1. Für jede **Prototyp Zelle**, wählen Sie ihn, und weist eine eindeutige **Bezeichner** in die **Widget** Registerkarte die **Eigenschaften-Explorer**. Dieser Schritt ist _sehr wichtig_ , wie Sie diesen Bezeichner später benötigen, wenn Sie füllen Sie die Tabelle. Z. B. `AttrCell`: 

    [![](table-views-images/table12-vs.png "Zuweisen eines Bezeichners")](table-views-images/table12-vs.png#lightbox)
1. Sie können auch auswählen, zur Darstellung der Zelle als eines der [Standard Zelle Tabellensichttypen](#table-view-cell-types) über die **Stil** Dropdownliste oder legen ihn auf **benutzerdefinierte** und verwenden Sie die Entwurfsoberfläche, das Layout der Zelle durch Ziehen in andere UI-Widgets aus dem **Toolbox**: 

    [![](table-views-images/table13-vs.png "Der stildropdownliste")](table-views-images/table13-vs.png#lightbox)
1. Weist eine eindeutige **Namen** auf jedes Element der Benutzeroberfläche beim Entwurf Prototyp Zelle in der **Widget** Registerkarte die **Eigenschaften-Explorer** damit Sie sie später unter zugreifen können C# Code: 

    [![](table-views-images/table14-vs.png "Die Registerkarte \"Widget\"")](table-views-images/table14-vs.png#lightbox)
1. Wiederholen Sie die oben genannten Schritte für alle Zellen in der Tabellenansicht Prototyp aus.
1. Als Nächstes weisen Sie benutzerdefinierte Klassen für den Rest der Entwurf von Benutzeroberflächen, Layout der Detailansicht und weisen eindeutige **Namen** auf jedes Element der Benutzeroberfläche in den Details anzeigen, damit Sie diese im zugreifen können C# ebenfalls. Beispiel: 

    [![](table-views-images/table15.png "Das UI-Layout")](table-views-images/table15.png#lightbox)
1. Speichern Sie die Änderungen auf das Storyboard an.
    
-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Entwerfen von Datenmodellen

Arbeiten mit den Informationen zu gestalten, der die Tabellenansicht einfacher angezeigt wird und die Präsentation mit detaillierten Informationen (wie der Benutzer wählt oder hebt der Zeilen in der Tabellenansicht) zu vereinfachen, erstellen Sie eine benutzerdefinierte Klasse oder Klassen fungieren als Datenmodell für die Informationen angezeigt .

Betrachten Sie beispielsweise eine Buchung Reise-app, die eine Liste der enthält **Städte**, jeder, der eine eindeutige Liste von enthält **Attraktionen** , die der Benutzer auswählen kann. Der Benutzer wird in der Lage, markieren Sie ein Anziehungskraft als eine *bevorzugten*, wählen Sie diese Option erhalten *erfahren Sie, wie* auf eine Anziehungskraft und *Buchen ein Flugs* mit einem angegebenen Ort.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Das Datenmodell zum Erstellen einer **Anziehungskraft**, mit der rechten Maustaste auf den Projektnamen in der **Lösungspad** , und wählen Sie **hinzufügen** > **neue Datei...** . Geben Sie `AttractionInformation` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

[![](table-views-images/data01.png "Geben Sie ein AttractionInformation")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Das Datenmodell zum Erstellen einer **Anziehungskraft**, mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer** , und wählen Sie **hinzufügen** > **neues Element ...** . Wählen Sie **Klasse** , und geben Sie `AttractionInformation` für die **Namen** , und klicken Sie auf die **hinzufügen** Schaltfläche: 

[![](table-views-images/data01-vs.png "Wählen Sie die Klasse und geben AttractionInformation für den Namen ein")](table-views-images/data01-vs.png#lightbox)

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

Diese Klasse stellt die Eigenschaften zum Speichern der Informationen zu einem bestimmten **Anziehungskraft**.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Als Nächstes mit der rechten Maustaste auf den Projektnamen in der **Lösungspad** erneut aus, und wählen Sie **hinzufügen** > **neue Datei...** . Geben Sie `CityInformation` für die **Namen** , und klicken Sie auf die **neu** Schaltfläche: 

[![](table-views-images/data02.png "Geben Sie ein CityInformation")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

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

Diese Klasse enthält alle Informationen zu einem Ziel **City**, eine Auflistung von **Attraktionen** für diese Stadt und enthält zwei Hilfsmethoden (`AddAttraction`) zu hinzuzufügenden Attraktionen zum Vereinfachen der Stadt.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>Die Datenquelle der Tabelle anzeigen

Jede Tabellenansicht erfordert eine Datenquelle (`UITableViewDataSource`) geben die Daten für die Tabelle, und generieren die erforderlichen Zeilen als die Tabellenansicht erforderlich.

Im Beispiel oben angegebenen, mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer**Option **hinzufügen** > **neue Datei...**  und nennen Sie sie `AttractionTableDatasource` , und klicken Sie auf die **neu** zu erstellen. Als Nächstes bearbeiten Sie die `AttractionTableDatasource.cs` Datei, und stellen sie wie folgt aussehen:

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

Wir haben zunächst definiert eine Konstante, um den eindeutigen Bezeichner der Prototyp Zelle zu speichern (Dies ist der gleiche Bezeichner im Benutzeroberflächen-Designer zugewiesen ist), eine Verknüpfung zurück auf den Tabelle-View-Controller hinzugefügt und Speicher für unsere Daten erstellt:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Als Nächstes wir speichern die Tabellenansichtscontroller, anschließendes Erstellen und Auffüllen der Datenquelle (mithilfe der oben definierte Datenmodelle) bei der Erstellung der Klasse:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Als Beispiel das `PopulateCities` Methode erstellt einfach Datenmodellobjekte im Arbeitsspeicher, aber diese problemlos aus einer Datenbank oder Web-Dienst in einer echten Anwendung gelesen werden konnte:

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

Die `NumberOfSections` Methode die Anzahl der Abschnitte in der Tabelle zurückgegeben:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Für **einfache** gestaltetem Tabellenansichten, immer 1 zurück.

Die `RowsInSection` Methode wird die Anzahl der Zeilen im aktuellen Abschnitt:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

In diesem Fall für **einfache** Tabellenansichten, die Gesamtanzahl von Elementen zurückgeben, in der Datenquelle.

Die `TitleForHeader` Methode gibt den Titel aus, für den angegebenen Abschnitt:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Für eine **einfache** Tabellenansicht eingeben, lassen Sie den Titel leer (`""`).

Erstellen Sie schließlich beim der Tabellenansicht anfordert, und füllen ein Prototyp-Zelle mit dem `GetCell` Methode: 

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

Weitere Informationen zum Arbeiten mit einem `UITableViewDatasource`, informieren Sie sich von Apple [UITableViewDatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) Dokumentation.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Der Delegat der Tabelle anzeigen

Jede Ansicht der Tabelle ein Delegat erforderlich ist (`UITableViewDelegate`) zur Reaktion auf Benutzerinteraktionen oder andere Systemereignisse in der Tabelle.

Im Beispiel oben angegebenen, mit der rechten Maustaste auf den Projektnamen in der **Projektmappen-Explorer**Option **hinzufügen** > **neue Datei...**  und nennen Sie sie `AttractionTableDelegate` , und klicken Sie auf die **neu** zu erstellen. Als Nächstes bearbeiten Sie die `AttractionTableDelegate.cs` Datei, und stellen sie wie folgt aussehen:

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

Werfen wir einen Blick auf mehrere Abschnitte dieser Klasse in den Details.

Zunächst erstellen wir eine Verknüpfung mit der Table-View-Controller auf, wenn die Klasse erstellt wird:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Klicken Sie dann, wenn eine Zeile ausgewählt ist (der Benutzer klickt auf der Oberfläche Touch, der das Apple-Remote) wir markieren möchten die **Anziehungskraft** durch die ausgewählte Zeile als Favorit dargestellt:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Als Nächstes, wenn der Benutzer eine Zeile hervorgehoben (indem Sie Ihren Fokus mit der Apple Remote-Touch-Oberfläche) dargestellt werden sollen die Details der **Anziehungskraft** durch diese Zeile im Detailabschnitt der unser Controller für geteilte Ansicht dargestellt wird:

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

Die `CanFocusRow` Methode wird aufgerufen, für jede Zeile, die Fokus in der Tabellenansicht zu erhalten. Zurückgeben `true` bei die Zeile den Fokus erhalten, zurückgegeben. andernfalls `false`. In diesem Beispiel haben wir eine benutzerdefinierte `AttractionHighlighted` Ereignis, das für jede Zeile ausgelöst wird, wie es den Fokus erhält.

Weitere Informationen zum Arbeiten mit einem `UITableViewDelegate`, informieren Sie sich von Apple [UITableViewDelegate](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) Dokumentation.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>Die Tabellenansichtszelle

Für jede Prototyp-Zelle, die die Tabellenansicht im Benutzeroberflächen-Designer hinzugefügt, erstellt Sie auch eine benutzerdefinierte Instanz von der Tabellenansichtszelle (`UITableViewCell`) es Ihnen ermöglichen, die neue Zelle (Zeile) zu füllen, wie sie erstellt wird.

Für die Beispiel-app, doppelklicken Sie auf die `AttractionTableCell.cs` Datei zur Bearbeitung öffnen, und stellen sie wie folgt aussehen:

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

Diese Klasse stellt Speicher für die Anziehungskraft datenmodellobjekt (`AttractionInformation` wie oben definiert) in der angegebenen Zeile angezeigt:

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

Die `UpdateUI` Methode füllt den UI-Widgets (die in der Zelle Prototyp im Benutzeroberflächen-Designer hinzugefügt wurden), nach Bedarf:

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

Weitere Informationen zum Arbeiten mit einem `UITableViewCell`, informieren Sie sich von Apple [UITableViewCell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) Dokumentation.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Die Tabellenansichtscontroller

Ein Tabellenansichtscontroller (`UITableViewController`) verwaltet eine Tabellenansicht, die auf ein Storyboard über den Benutzeroberflächen-Designer hinzugefügt wurde.

Für die Beispiel-app, doppelklicken Sie auf die `AttractionTableViewController.cs` Datei zur Bearbeitung öffnen, und stellen sie wie folgt aussehen:

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

Werfen Sie einen genaueren Blick auf diese Klasse an. Wir haben zunächst erstellt Verknüpfungen zum Vereinfachen der Tabellenansicht auf `DataSource` und `TableDelegate`. Diese verwenden später wir für die Kommunikation zwischen der Tabellenansicht auf der linken Seite der geteilten Ansicht und der Detailansicht, in der rechten Seite.

Wenn die Tabellenansicht in den Arbeitsspeicher geladen wird, wir erstellen Sie abschließend Instanzen von der `AttractionTableDatasource` und `AttractionTableDelegate` (beide oben erstellt haben) und fügen Sie sie an der Tabellenansicht.

Weitere Informationen zum Arbeiten mit einem `UITableViewController`, informieren Sie sich von Apple [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) Dokumentation.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Ziehen sie alle zusammen

Wie am Anfang dieses Dokuments angegeben wird, werden Tabellenansichten in der Regel in einer Seite angezeigt ein [geteilte Ansicht](~/ios/tvos/user-interface/split-views.md) wie Navigation, mit den Details des ausgewählten Elements in der entgegengesetzten Seite angezeigt. Zum Beispiel: 

[![](table-views-images/intro01.png "Beispielapp-Ausführung")](table-views-images/intro01.png#lightbox)

Da dies ein Standardmuster in TvOS ist, sehen wir uns die abschließenden Schritte aus, um alles vereinen zu können, und die linke und rechte Seite der geteilten Ansicht miteinander interagieren.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>Die Detailansicht

Im Beispiel des Reise-app angezeigt, über eine benutzerdefinierte Klasse (`AttractionViewController`) wird definiert, für den standard Ansichtscontroller, die in der rechten Seite der geteilten Ansicht als die Detailansicht dargestellt:

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

Wir haben hier angegeben die **Anziehungskraft** (`AttractionInformation`) als Eigenschaft angezeigt wird, und erstellt eine `UpdateUI` -Methode, die füllt die Benutzeroberflächen-Widgets hinzugefügt wird, an die Ansicht im Benutzeroberflächen-Designer.

Wir haben auch eine Verknüpfung definiert, an den Controller für geteilte Ansicht (`SplitView`), dass wir für die Kommunikation verwenden wechselt zurück in die Tabelle (`AcctractionTableView`).

Schließlich wurden benutzerdefinierte Aktionen (Ereignisse) hinzugefügt, um die drei `UIButton` Instanzen im Benutzeroberflächen-Designer erstellt wurden, bei denen der Benutzer eine Anziehungskraft als Markieren einer _bevorzugten_, erhalten _erfahren Sie, wie_ auf ein Anziehungskraft und _Buchen ein Flugs_ mit einem angegebenen Ort.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Der Navigationscontroller-Ansicht

Da die Tabellenansichtscontroller in einen Navigationscontroller anzeigen, auf der linken Seite der geteilten Ansicht geschachtelt ist, wurde der Navigation-View-Controller eine benutzerdefinierte Klasse zugewiesen (`MasterNavigationController`) im Benutzeroberflächen-Designer und wie folgt definiert:

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

Diese Klasse definiert in diesem Fall nur ein paar Tastenkombinationen, um die Kommunikation über die beiden Seiten der der Controller für geteilte Ansicht zu vereinfachen:

* `SplitView` -Ist ein Link zu den Controller für geteilte Ansicht (`MainSpiltViewController`), zu der die Navigation-View-Controller gehört.
* `TableController` -Ruft die Tabellenansichtscontroller (`AttractionTableViewController`), die als die Top-Ansicht in der Ansicht Navigationscontroller dargestellt wird.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Der Controller für geteilte Ansicht

Da der Controller für geteilte Ansicht auf der Basis der Anwendung ist, erstellt es eine benutzerdefinierte Klasse (`MasterSplitViewController`) dafür im Benutzeroberflächen-Designer, und sie wie folgt definiert:

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

Zunächst erstellen wir von Verknüpfungen zu den **Details** -Seite der geteilten Ansicht (`AttractionViewController`) und die **Master** Seite (`MasterNavigationController`). In diesem Fall erleichtert dies die Kommunikation zwischen den beiden Seiten später noch mal.

Als Nächstes bei der geteilten Ansicht in den Arbeitsspeicher geladen wird, wir fügen Sie den Controller für geteilte Ansicht auf beiden Seiten der geteilten Ansicht und reagieren, die dem Benutzer ein Anziehungskraft in der Tabellenansicht hervorheben (`AttractionHighlighted`) durch Anzeigen der neuen Anziehungskraft in die **Details**  -Seite der geteilten Ansicht.

Informieren Sie sich die [TvTables](https://developer.xamarin.com/samples/monotouch/tvos/tvTable/) Beispiel-app für eine vollständige Implementierung der Tabellenansichten in einer geteilten Ansicht.

## <a name="table-views-in-detail"></a>Tabellenansichten im Detail

Da TvOS auf iOS basiert, Tabellenansichten Table View Controllers dienen und sich auf ähnliche Weise Verhalten. Ausführlichere Informationen zum Arbeiten mit Tabelle anzeigen, in einer Xamarin-app finden Sie unter unserem iOS [arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md) Dokumentation.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden behandelt, Entwerfen und Arbeiten mit Tabellenansichten in ein Xamarin.tvOS-app. Ist ein Beispiel für die Arbeit mit einer Tabellenansicht in einer geteilten Ansicht an, die Regel von einer Tabellenansicht in einer TvOS-app wird angezeigt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://developer.xamarin.com/samples/tvos/all/)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [TvOS Human Interface-Handbücher](https://developer.apple.com/tvos/human-interface-guidelines/)
- [App-Programmierhandbuch für tvos verwendet.](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
