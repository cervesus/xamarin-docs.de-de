---
title: Arbeiten mit tvos-Tabellen Ansichten in xamarin
description: In diesem Artikel wird das Entwerfen und arbeiten mit Tabellen Sichten und Tabellen Ansichts Controllern in einer xamarin. tvos-App behandelt.
ms.prod: xamarin
ms.assetid: D8F80FA9-6400-4DB7-AFC9-A28A54AD04E8
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/16/2017
ms.openlocfilehash: 0d93978d6f7b3dff6d0d7ebf7c9f9afbe3572079
ms.sourcegitcommit: 1e3a0d853669dcc57d5dee0894d325d40c7d8009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2019
ms.locfileid: "70199908"
---
# <a name="working-with-tvos-table-views-in-xamarin"></a>Arbeiten mit tvos-Tabellen Ansichten in xamarin

_In diesem Artikel wird das Entwerfen und arbeiten mit Tabellen Sichten und Tabellen Ansichts Controllern in einer xamarin. tvos-App behandelt._

In tvos wird eine Tabellenansicht als einzelne Spalte mit scrollzeilen dargestellt, die optional in Gruppen oder Abschnitte organisiert werden können. Tabellen Sichten sollten verwendet werden, wenn Sie eine große Datenmenge effizient für den Benutzer anzeigen müssen, um eine klare Vorgehensweise zu verstehen.

Tabellen Sichten werden in der Regel auf einer Seite einer [geteilten Ansicht](~/ios/tvos/user-interface/split-views.md) als Navigation angezeigt, wobei die Details des ausgewählten Elements auf der gegenüberliegenden Seite angezeigt werden:

[![](table-views-images/intro01.png "Beispiel Tabellenansicht")](table-views-images/intro01.png#lightbox)

<a name="About-Table-Views" />

## <a name="about-table-views"></a>Informationen zu Tabellen Sichten

Eine `UITableView` zeigt eine einzelne Spalte mit scrollbaren Zeilen als hierarchische Liste von Informationen an, die optional in Gruppen oder Abschnitte organisiert werden können: 

[![](table-views-images/table01.png "Ein ausgewähltes Element")](table-views-images/table01.png#lightbox)

Apple hat die folgenden Vorschläge zum Arbeiten mit Tabellen:

- Beachten Sie die **Breite** : versuchen Sie, den richtigen Saldo der Tabellenbreite zu treffen. Wenn die Tabelle zu breit ist, kann es schwierig sein, aus einer Entfernung zu scannen und den verfügbaren Inhalts Bereich zu entfernen. Wenn die Tabelle zu schmal ist, kann dies dazu führen, dass die Informationen abgeschnitten oder gewrappt werden. Dies kann für den Benutzer im gesamten Raum schwierig zu lesen sein.
- **Schnelles Anzeigen von Tabellen Inhalten** : bei großen Daten Listen lädt der Inhalt verzögert und zeigt Informationen an, sobald die Tabelle dem Benutzer angezeigt wird. Wenn die Tabelle lange benötigt, um geladen zu werden, kann der Benutzer Ihr Interesse an Ihrer APP verlieren oder denken, dass er gesperrt ist.
- **Benutzer über lange Inhalts Belastungen informieren** : Wenn eine lange Tabellen Ladezeit unvermeidlich ist, stellen Sie eine [Statusanzeige oder einen Aktivitätsindikator](~/ios/tvos/user-interface/progress-indicators.md) dar, damit Sie wissen, dass die APP nicht gesperrt ist.

<a name="Table-Cell-Types" />

## <a name="table-view-cell-types"></a>Tabellen Sicht-Zelltypen

Eine `UITableViewCell` wird verwendet, um die einzelnen Daten Zeilen in der Tabellenansicht darzustellen. Apple hat mehrere standardmäßige Tabellenzellen Typen definiert:

- **Standard** : dieser Typ zeigt ein Options Bild auf der linken Seite der Zelle und den linksbündig ausgerichteten Titel auf der rechten Seite an. 
- Unter **Titel** : dieser Typ zeigt einen linksbündig ausgerichteten Titel in der ersten Zeile und einen kleineren linksbündig ausgerichteten Untertitel in der nächsten Zeile an.
- **Wert 1** : dieser Typ stellt einen links ausgerichteten Titel mit einem helleren, rechtsbündig ausgerichteten Titel in derselben Zeile dar.
- **Wert 2** : dieser Typ stellt einen rechts ausgerichteten Titel mit einem helleren, linksbündig ausgerichteten Titel in derselben Zeile dar.

Alle Zelltypen der Standard Tabellen Sicht unterstützen auch grafische Elemente, wie z. b. Offenlegungs Indikatoren oder Häkchen. 

Außerdem können Sie einen **benutzerdefinierten** Tabellen Ansichts Zellentyp definieren und eine _prototypzelle_darstellen, die Sie entweder im Schnittstellen-Designer oder über Code erstellen.

Apple hat die folgenden Vorschläge zum Arbeiten mit Tabellen Ansichts Zellen:

- **Vermeiden von Text Clipping** : halten Sie die einzelnen Textzeilen kurz, damit Sie nicht abgeschnitten werden. Abgeschnittene Wörter oder Ausdrücke sind schwer für den Benutzer, der über den Raum hinweg analysiert werden muss.
- **Sehen Sie sich den Status der fokussierten Zeile** an: da eine Zeile größer wird, und wenn Sie sich im Fokus befinden, müssen Sie die Darstellung ihrer Zelle in allen Zuständen testen. Bilder oder Text können abgeschnitten werden oder im Fokus Zustand falsch aussehen.
- **Verwenden bearbeitbarer Tabellen** , wenn Tabellenzeilen sparsam verschoben oder gelöscht werden, ist für tvos mehr zeitaufwändig als IOS. Sie müssen sorgfältig entscheiden, ob diese Funktion Ihre tvos-App hinzufügt oder von ihr ablenkt.
- Erstellen Sie ggf. **benutzerdefinierte Zelltypen** , während die integrierten Tabellenansicht-Zelltypen in vielen Situationen hervorragend sind. Erstellen Sie ggf. benutzerdefinierte Zelltypen für nicht standardmäßige Informationen, um eine bessere Kontrolle zu bieten und die Informationen für die Bedienungs.

<a name="Working-With-Table-Views" />

## <a name="working-with-table-views"></a>Arbeiten mit Tabellen Sichten

Die einfachste Möglichkeit zum Arbeiten mit Tabellen Sichten in einer xamarin. tvos-APP ist das Erstellen und Ändern ihrer Darstellung im Schnittstellen-Designer.

Um zu beginnen, machen Sie Folgendes:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1. Starten Sie in Visual Studio für Mac ein neues tvos-App-Projekt, wählen Sie **tvos** > **App** > **Single View App** aus, und klicken Sie auf die Schaltfläche **weiter** : 

    [![](table-views-images/table02.png "Einzelansicht-App auswählen")](table-views-images/table02.png#lightbox)
1. Geben Sie einen **Namen** für die APP ein, und klicken Sie auf **weiter**: 

    [![](table-views-images/table03.png "Geben Sie einen Namen für die APP ein.")](table-views-images/table03.png#lightbox)
1. Passen Sie entweder den **Projektnamen** und den Projektmappennamen an, oder übernehmen Sie die Standardeinstellungen, und klicken Sie auf die Schaltfläche **Erstellen** , um 

    [![](table-views-images/table04.png "Projektname und Projektmappenname")](table-views-images/table04.png#lightbox)
1. Doppelklicken Sie im **Lösungspad**auf die `Main.storyboard` Datei, um Sie im IOS-Designer zu öffnen: 

    [![](table-views-images/table05.png "Die Datei \"Main. Storyboard\"")](table-views-images/table05.png#lightbox)
1. **Standard Ansichts Controller**auswählen und löschen: 

    [![](table-views-images/table06.png "Auswählen und Löschen des Standard Ansichts Controllers")](table-views-images/table06.png#lightbox)
1. Wählen Sie einen **Split View Controller** aus der **Toolbox** aus, und ziehen Sie ihn auf den Designoberfläche.
1. Standardmäßig erhalten Sie eine [geteilte Ansicht](~/ios/tvos/user-interface/split-views.md) mit einem **Navigations Ansichts Controller** und einem **Tabellen Ansichts** Controller auf der linken Seite und einen **Ansichts Controller** auf der rechten Seite. Dies ist die empfohlene Verwendung einer Tabellenansicht in tvos in Apple: 

    [![](table-views-images/table08.png "Hinzufügen einer geteilten Ansicht")](table-views-images/table08.png#lightbox)
1. Sie müssen jeden Teil der Tabellenansicht auswählen und diesem im **Eigenschaften-Explorer** auf der Registerkarte C# " **Widget** " einen benutzerdefinierten **Klassennamen** zuweisen, damit Sie später im Code darauf zugreifen können. Beispielsweise kann der **Tabellen Ansichts Controller**Folgendes: 

    [![](table-views-images/table09.png "Zuweisen eines Klassen namens")](table-views-images/table09.png#lightbox)
1. Stellen Sie sicher, dass Sie eine benutzerdefinierte Klasse für den **Tabellen Ansichts Controller**, die **Tabellenansicht** und beliebige **prototypzellen**erstellen. Visual Studio für Mac werden die benutzerdefinierten Klassen der Projektstruktur hinzugefügt, wenn Sie erstellt werden: 

    [![](table-views-images/table10.png "Die benutzerdefinierten Klassen in der Projektstruktur")](table-views-images/table10.png#lightbox)
1. Wählen Sie als nächstes die Tabellenansicht in der Designoberfläche aus, und passen Sie die Eigenschaften nach Bedarf an. Z. b. die Anzahl der **prototypzellen** und der **Stil** (einfach oder gruppiert): 

    [![](table-views-images/table11.png "Die Widget-Registerkarte")](table-views-images/table11.png#lightbox)
1. Wählen Sie für jede **prototypzelle**diese aus, und weisen Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** einen eindeutigen Bezeichner zu. Dieser Schritt ist _sehr wichtig_ , da Sie diesen Bezeichner später benötigen, wenn Sie die Tabelle auffüllen. Beispiel `AttrCell`: 

    [![](table-views-images/table12.png "Die Widget-Registerkarte")](table-views-images/table12.png#lightbox)
1. Sie können auch auswählen, dass die Zelle als einer der [standardtabellenansichts-Zelltypen](#table-view-cell-types) über die Dropdown Liste Formatvorlagen angezeigt werden soll, oder Sie können den Designoberfläche verwenden, um die Zelle zu formatieren, indem Sie andere UI-Widgets aus der **Toolbox**ziehen: 

    [![](table-views-images/table13.png "Das Zellen Layout")](table-views-images/table13.png#lightbox)
1. Weisen Sie jedem Benutzeroberflächen Element im prototypzellentwurf auf der Registerkarte **Widget** im Eigenschaften- **Explorer** einen eindeutigen **Namen** zu, damit Sie später C# im Code darauf zugreifen können: 

    [![](table-views-images/table14.png "Zuweisen eines Namens")](table-views-images/table14.png#lightbox)
1. Wiederholen Sie den obigen Schritt für alle prototypzellen in der Tabellenansicht.
1. Weisen Sie anschließend dem restlichen Benutzeroberflächen Entwurf benutzerdefinierte Klassen zu, ordnen Sie die Detailansicht an, und weisen Sie jedem Benutzeroberflächen Element in der Detailansicht eindeutige **Namen** zu, sodass C# Sie auch in auf Sie zugreifen können. Beispiel: 

    [![](table-views-images/table15.png "Das Layout der Benutzeroberfläche")](table-views-images/table15.png#lightbox)
1. Speichern Sie die Änderungen am Storyboard.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1. Starten Sie in Visual Studio ein neues tvos-App-Projekt, und wählen Sie **tvos** > **Single View App** aus, und geben Sie einen Namen für Ihre APP ein. Klicken Sie auf die Schaltfläche OK, um eine neue Lösung zu erstellen: 

    [![](table-views-images/table02-vs.png "Einzelansicht-App auswählen")](table-views-images/table02-vs.png#lightbox)
1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Main.storyboard` Datei, um Sie im IOS-Designer zu öffnen: 

    [![](table-views-images/table05-vs.png "Die Datei \"Main. Storyboard\"")](table-views-images/table05-vs.png#lightbox)
1. **Standard Ansichts Controller**auswählen und löschen: 

    [![](table-views-images/table06-vs.png "Auswählen und Löschen des Standard Ansichts Controllers")](table-views-images/table06-vs.png#lightbox)
1. Wählen Sie einen **Split View Controller** aus der **Toolbox** aus, und ziehen Sie ihn auf den Designoberfläche: 

    [![](table-views-images/table07-vs.png "Einen geteilten Ansichts Controller")](table-views-images/table07-vs.png#lightbox)
1. Standardmäßig erhalten Sie eine [geteilte Ansicht](~/ios/tvos/user-interface/split-views.md) mit einem **Navigations Ansichts Controller** und einem **Tabellen Ansichts** Controller auf der linken Seite und einen **Ansichts Controller** auf der rechten Seite. Dies ist die empfohlene Verwendung einer Tabellenansicht in tvos in Apple: 

    [![](table-views-images/table08-vs.png "Layout der Benutzeroberfläche")](table-views-images/table08-vs.png#lightbox)
1. Sie müssen jeden Teil der Tabellenansicht auswählen und diesem im **Eigenschaften-Explorer** auf der Registerkarte C# " **Widget** " einen benutzerdefinierten **Klassennamen** zuweisen, damit Sie später im Code darauf zugreifen können. Beispielsweise kann der **Tabellen Ansichts Controller**Folgendes: 

    [![](table-views-images/table09-vs.png "Die Widget-Registerkarte")](table-views-images/table09-vs.png#lightbox)
1. Stellen Sie sicher, dass Sie eine benutzerdefinierte Klasse für den **Tabellen Ansichts Controller**, die **Tabellenansicht** und beliebige **prototypzellen**erstellen. Visual Studio für Mac werden die benutzerdefinierten Klassen der Projektstruktur hinzugefügt, wenn Sie erstellt werden: 

    [![](table-views-images/table10-vs.png "Die benutzerdefinierten Klassen in der Projektstruktur")](table-views-images/table10-vs.png#lightbox)
1. Wählen Sie als nächstes die Tabellenansicht in der Designoberfläche aus, und passen Sie die Eigenschaften nach Bedarf an. Z. b. die Anzahl der **prototypzellen** und der **Stil** (einfach oder gruppiert): 

    [![](table-views-images/table11-vs.png "Die Widget-Registerkarte")](table-views-images/table11-vs.png#lightbox)
1. Wählen Sie für jede **prototypzelle**diese aus, und weisen Sie im **Eigenschaften-Explorer**auf der Registerkarte **Widget** einen eindeutigen Bezeichner zu. Dieser Schritt ist _sehr wichtig_ , da Sie diesen Bezeichner später benötigen, wenn Sie die Tabelle auffüllen. Beispiel `AttrCell`: 

    [![](table-views-images/table12-vs.png "Zuweisen eines Bezeichners")](table-views-images/table12-vs.png#lightbox)
1. Sie können auch auswählen, dass die Zelle als einer der [standardtabellenansichts-Zelltypen](#table-view-cell-types) über die Dropdown Liste Formatvorlagen angezeigt werden soll, oder Sie können den Designoberfläche verwenden, um die Zelle zu formatieren, indem Sie andere UI-Widgets aus der **Toolbox**ziehen: 

    [![](table-views-images/table13-vs.png "Die Dropdown Liste \"Style\"")](table-views-images/table13-vs.png#lightbox)
1. Weisen Sie jedem Benutzeroberflächen Element im prototypzellentwurf auf der Registerkarte **Widget** im Eigenschaften- **Explorer** einen eindeutigen **Namen** zu, damit Sie später C# im Code darauf zugreifen können: 

    [![](table-views-images/table14-vs.png "Die Widget-Registerkarte")](table-views-images/table14-vs.png#lightbox)
1. Wiederholen Sie den obigen Schritt für alle prototypzellen in der Tabellenansicht.
1. Weisen Sie anschließend dem restlichen Benutzeroberflächen Entwurf benutzerdefinierte Klassen zu, ordnen Sie die Detailansicht an, und weisen Sie jedem Benutzeroberflächen Element in der Detailansicht eindeutige **Namen** zu, sodass C# Sie auch in auf Sie zugreifen können. Beispiel: 

    [![](table-views-images/table15.png "Das Layout der Benutzeroberfläche")](table-views-images/table15.png#lightbox)
1. Speichern Sie die Änderungen am Storyboard.

-----

<a name="Designing-a-Data-Model" />

## <a name="designing-a-data-model"></a>Entwerfen eines Datenmodells

Erstellen Sie eine benutzerdefinierte Klasse oder Klassen, die als Datenmodell für die dargestellten Informationen fungieren, um die Arbeit mit den Informationen zu vereinfachen, die in der Tabellenansicht einfacher angezeigt werden, und um die Darstellung detaillierter Informationen (beim auswählen oder hervorheben von Zeilen in der Tabellenansicht) zu vereinfachen. .

Sehen Sie sich das Beispiel für eine Reise Reservierungs-APP an, die eine Liste von **Städten**enthält, die jeweils eine eindeutige Liste von **Attraktionen** enthält, die der Benutzer auswählen kann. Der Benutzer ist in der Lage, eine Attraktion als *Favorit*zu markieren, die *Richtung* zu einer Attraktion zu bringen und *einen Flug* an eine bestimmte Stadt zu buchen.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Um das Datenmodell für eine **Attraktion**zu erstellen, klicken Sie im **Lösungspad** mit der rechten Maustaste auf den Projektnamen, und wählen Sie**neue Datei** **Hinzufügen** > ... aus. Geben `AttractionInformation` Sie als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** : 

[![](table-views-images/data01.png "Geben Sie \"attractioninformation\" für den Namen ein.")](table-views-images/data01.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Um das Datenmodell für eine **Attraktion**zu erstellen, klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf den Projektnamen, und wählen Sie**Neues Element** **Hinzufügen** > ... aus. Wählen Sie **Klasse** aus `AttractionInformation` , geben Sie für den **Namen** ein, und klicken Sie auf die Schaltfläche 

[![](table-views-images/data01-vs.png "Wählen Sie Klasse aus, und geben Sie \"attractioninformation\" als Namen")](table-views-images/data01-vs.png#lightbox)

-----

Bearbeiten Sie `AttractionInformation.cs` die Datei, und führen Sie Sie wie folgt aus:

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

Diese Klasse stellt die Eigenschaften zum Speichern der Informationen über eine bestimmte **Attraktion**bereit.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

Klicken Sie als nächstes mit der rechten Maustaste auf den Projektnamen im **Lösungspad** , und wählen Sie**neue Datei** **Hinzufügen** > ... aus. Geben `CityInformation` Sie als **Namen** ein, und klicken Sie auf die Schaltfläche **neu** : 

[![](table-views-images/data02.png "Geben Sie für den Namen cityinformation ein.")](table-views-images/data02.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Klicken Sie als nächstes mit der rechten Maustaste auf den Projektnamen im **Projektmappen-Explorer** , und wählen Sie**Neues Element** **Hinzufügen** > ... aus. Geben `CityInformation` Sie als **Namen** ein, und klicken Sie auf **Hinzufügen** : 

[![](table-views-images/data02-vs.png "Geben Sie für den Namen cityinformation ein.")](table-views-images/data02-vs.png#lightbox)

-----

Bearbeiten Sie `CityInformation.cs` die Datei, und führen Sie Sie wie folgt aus:

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

Diese Klasse enthält alle Informationen über eine Zielstadt,eine Sammlung von **Attraktionen** für diese Stadt und bietet zwei Hilfsmethoden (`AddAttraction`), um das Hinzufügen von Attraktionen zum Ort zu vereinfachen.

<a name="The-Table-Data-Source" />

## <a name="the-table-view-data-source"></a>Die Tabellen Ansichts Datenquelle

Jede Tabellen Sicht erfordert eine Datenquelle (`UITableViewDataSource`), um die Daten für die Tabelle bereitzustellen und die erforderlichen Zeilen gemäß der Tabellenansicht zu generieren.

Klicken Sie im obigen Beispiel mit der rechten Maustaste auf den Projektnamen im **Projektmappen-Explorer**, wählen Sie**neue Datei** **Hinzufügen** > aus, und rufen Sie `AttractionTableDatasource` Sie auf, und klicken Sie dann auf die Schaltfläche **neu** , um zu erstellen. Bearbeiten Sie anschließend die `AttractionTableDatasource.cs` Datei, und führen Sie Sie wie folgt aus:

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

Zuerst haben wir eine Konstante definiert, die den eindeutigen Bezeichner der prototypenzelle enthält (Dies ist derselbe Bezeichner, der im Schnittstellen-Designer oben zugewiesen wurde), eine Verknüpfung zum Tabellen Ansichts Controller hinzugefügt und Speicher für unsere Daten erstellt:

```csharp
const string CellID = "AttrCell";
public AttractionTableViewController Controller { get; set;}
public List<CityInformation> Cities { get; set;}
```

Als nächstes speichern wir den Tabellen Ansichts Controller und erstellen dann die Datenquelle (mit den oben definierten Datenmodellen), wenn die Klasse erstellt wird:

```csharp
public AttractionTableDatasource (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
    this.Cities = new List<CityInformation> ();
    PopulateCities ();
}
```

Zum Beispiel erstellt die-Methode einfach `PopulateCities` Datenmodell Objekte im Arbeitsspeicher, die jedoch problemlos von einer Datenbank oder einem Webdienst in einer echten App gelesen werden können:

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

Die `NumberOfSections` -Methode gibt die Anzahl der Abschnitte in der-Tabelle zurück:

```csharp
public override nint NumberOfSections (UITableView tableView)
{
    // Return number of cities
    return Cities.Count;
}
```

Geben Sie für Tabellen Sichten mit **einfacher** Formatierung immer 1 zurück.

Die `RowsInSection` -Methode gibt die Anzahl der Zeilen im aktuellen Abschnitt zurück:

```csharp
public override nint RowsInSection (UITableView tableView, nint section)
{
    // Return the number of attractions in the given city
    return Cities [(int)section].Attractions.Count;
}
```

Bei **einfachen** Tabellen Sichten wird die Gesamtzahl der Elemente in der Datenquelle zurückgegeben.

Die `TitleForHeader` -Methode gibt den Titel für den angegebenen Abschnitt zurück:

```csharp
public override string TitleForHeader (UITableView tableView, nint section)
{
    // Get the name of the current city
    return Cities [(int)section].Name;
}
```

Lassen Sie für einen **einfachen** Tabellen Ansichtstyp den Titel leer`""`().

Wenn Sie von der Tabellenansicht angefordert werden, erstellen Sie eine prototypzelle, und füllen `GetCell` Sie Sie mit der-Methode auf: 

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

Weitere Informationen zum Arbeiten mit einem `UITableViewDatasource`finden Sie in der Dokumentation zu [uitableviewdatasource](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40006941) von Apple.

<a name="The-Table-View-Delegate" />

## <a name="the-table-view-delegate"></a>Der Tabellen Ansichts Delegat

Jede Tabellen Sicht erfordert, dass ein`UITableViewDelegate`Delegat () auf Benutzerinteraktionen oder andere Systemereignisse in der Tabelle antwortet.

Klicken Sie im obigen Beispiel mit der rechten Maustaste auf den Projektnamen im **Projektmappen-Explorer**, wählen Sie**neue Datei** **Hinzufügen** > aus, und rufen Sie `AttractionTableDelegate` Sie auf, und klicken Sie dann auf die Schaltfläche **neu** , um zu erstellen. Bearbeiten Sie anschließend die `AttractionTableDelegate.cs` Datei, und führen Sie Sie wie folgt aus:

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

Werfen wir einen Blick auf einige Abschnitte dieser Klasse.

Zuerst erstellen wir eine Verknüpfung mit dem Tabellen Ansichts Controller, wenn die Klasse erstellt wird:

```csharp
public AttractionTableViewController Controller { get; set;}
...

public AttractionTableDelegate (AttractionTableViewController controller)
{
    // Initialize
    this.Controller = controller;
}
```

Wenn eine Zeile ausgewählt ist (der Benutzer klickt auf die Berührungs Oberfläche von Apple Remote), möchten wir die von der ausgewählten Zeile dargestellte **Attraktion** als Favorit markieren:

```csharp
public override void RowSelected (UITableView tableView, Foundation.NSIndexPath indexPath)
{
    var attraction = Controller.Datasource.Cities [indexPath.Section].Attractions [indexPath.Row];
    attraction.IsFavorite = (!attraction.IsFavorite);

    // Update UI
    Controller.TableView.ReloadData ();
}
```

Wenn der Benutzer eine Zeile hervorhebt (indem er den Fokus mithilfe der Apple-Remote Berührungs Oberfläche erhält), möchten wir die Details der von dieser Zeile dargestellten **Attraktion** im Detail Abschnitt des Split View Controller präsentieren:

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

Die `CanFocusRow` -Methode wird für jede Zeile aufgerufen, die den Fokus in der Tabellenansicht erhält. Gibt `true` zurück, wenn die Zeile den Fokus erhalten kann `false`, andernfalls. Im Fall dieses Beispiels haben wir ein benutzerdefiniertes `AttractionHighlighted` Ereignis erstellt, das für jede Zeile ausgelöst wird, wenn es den Fokus erhält.

Weitere Informationen zum Arbeiten mit einem `UITableViewDelegate`finden Sie in der Dokumentation zu [uitableviewdelegaten](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40006942) von Apple.

<a name="The-Table-View-Cell" />

## <a name="the-table-view-cell"></a>Die Tabellen Ansichts Zelle

Für jede prototypzelle, die Sie der Tabellenansicht im Schnittstellen-Designer hinzugefügt haben, haben Sie auch eine benutzerdefinierte Instanz der`UITableViewCell`Tabellen Ansichts Zelle () erstellt, damit Sie die neue Zelle (Zeile) während der Erstellung auffüllen können.

Doppelklicken Sie für die Beispiel-App auf `AttractionTableCell.cs` die Datei, um Sie zur Bearbeitung zu öffnen, und führen Sie Sie wie folgt aus:

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

Diese Klasse bietet Speicher für das Objekt "Anziehungs Datenmodell`AttractionInformation` " (wie oben definiert), das in der angegebenen Zeile angezeigt wird:

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

Die `UpdateUI` -Methode füllt die UI-Widgets (die dem Prototyp der Zelle im Schnittstellen-Designer hinzugefügt wurden) nach Bedarf auf:

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

Weitere Informationen zum Arbeiten mit einem `UITableViewCell`finden Sie in der Dokumentation zu [uitableviewcell](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewCell_Class/index.html#//apple_ref/doc/uid/TP40006938) von Apple.

<a name="The-Table-View-Controller" />

## <a name="the-table-view-controller"></a>Der Tabellen Ansichts Controller

Ein Tabellen Ansichts Controller`UITableViewController`() verwaltet eine Tabellenansicht, die einem Storyboard über den Schnittstellen-Designer hinzugefügt wurde.

Doppelklicken Sie für die Beispiel-App auf `AttractionTableViewController.cs` die Datei, um Sie zur Bearbeitung zu öffnen, und führen Sie Sie wie folgt aus:

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

Sehen wir uns diese Klasse genauer an. Zuerst haben wir Verknüpfungen erstellt, um den Zugriff auf die Tabellenansicht `DataSource` und `TableDelegate`zu vereinfachen. Wir verwenden diese später, um zwischen der Tabellenansicht auf der linken Seite der geteilten Ansicht und der Detailansicht auf der rechten Seite zu kommunizieren.

Wenn die Tabellen Sicht in den Arbeitsspeicher geladen wird, erstellen wir nun Instanzen der `AttractionTableDatasource` und `AttractionTableDelegate` (beide oben erstellt) und fügen diese an die Tabellenansicht an.

Weitere Informationen zum Arbeiten mit einem `UITableViewController`finden Sie in der Dokumentation zu [uitableviewcontroller](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523) von Apple.

<a name="Pulling-it-All-Together" />

## <a name="pulling-it-all-together"></a>Alles abrufen

Wie am Anfang dieses Dokuments angegeben, werden Tabellen Sichten in der Regel auf einer Seite einer [geteilten Ansicht](~/ios/tvos/user-interface/split-views.md) als Navigation angezeigt, wobei die Details des ausgewählten Elements auf der gegenüberliegenden Seite angezeigt werden. Beispiel: 

[![](table-views-images/intro01.png "Ausführen der Beispiel-App")](table-views-images/intro01.png#lightbox)

Da es sich hierbei um ein Standardmuster in tvos handelt, betrachten wir die abschließenden Schritte, um alles zusammenzuführen, und lassen Sie die linke und die Rechte Seite der geteilten Ansicht miteinander interagieren.

<a name="The-Detail-View" />

### <a name="the-detail-view"></a>Die Detail Ansicht

Für das Beispiel der oben dargestellten Reise-app wird eine benutzerdefinierte Klasse`AttractionViewController`() für den Standard Ansichts Controller definiert, der auf der rechten Seite der geteilten Ansicht als Detail Ansicht dargestellt wird:

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

Hier haben wir die " **Attraction** " (`AttractionInformation`) als Eigenschaft angezeigt und eine `UpdateUI` Methode erstellt, die die Benutzeroberflächen-Widgets auffüllt, die der Ansicht im Schnittstellen-Designer hinzugefügt wurden.

Wir haben auch eine Verknüpfung mit dem Split View Controller (`SplitView`) definiert, mit dem wir Änderungen an der Tabellenansicht (`AcctractionTableView`) zurückgibt.

Zum Schluss wurden benutzerdefinierte Aktionen (Ereignisse) zu den drei `UIButton` Instanzen hinzugefügt, die im Schnittstellen-Designer erstellt wurden, die es dem Benutzer ermöglichen, eine Attraktion als _Favorit_zu markieren, _Anweisungen_ zu einer Attraktion zu erhalten und _einen Flug_ zu einem bestimmten Stadt.

<a name="The-Navigation-View-Controller" />

### <a name="the-navigation-view-controller"></a>Navigations Ansichts Controller

Da der Tabellen Ansichts Controller in einem Navigations Ansichts Controller auf der linken Seite der geteilten Ansicht eingefügt ist, wurde dem Navigations Ansichts Controller eine benutzerdefinierte Klasse`MasterNavigationController`() im Schnittstellen-Designer zugewiesen und wie folgt definiert:

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

Diese Klasse definiert nur einige Verknüpfungen, um die Kommunikation über die beiden Seiten des Split View Controller zu vereinfachen:

- `SplitView`-Ist ein Link zum Split View Controller (`MainSpiltViewController`), zu dem der Navigations Ansichts Controller gehört.
- `TableController`-Ruft den Tabellen Ansichts Controller`AttractionTableViewController`() ab, der im Navigations Ansichts Controller als obere Ansicht dargestellt wird.

<a name="The-Split-View-Controller" />

### <a name="the-split-view-controller"></a>Der geteilte Ansichts Controller

Da der Split View Controller die Basis unserer Anwendung ist, haben wir eine benutzerdefinierte Klasse (`MasterSplitViewController`) für Sie im Schnittstellen-Designer erstellt und wie folgt definiert:

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

Zuerst erstellen wir Verknüpfungen zur **Detail** Seite der geteilten Ansicht (`AttractionViewController`) und zur **Master** Seite (`MasterNavigationController`). Dadurch wird die Kommunikation zwischen den beiden Seiten später vereinfacht.

Wenn die geteilte Ansicht in den Arbeitsspeicher geladen wird, fügen wir den Split View Controller an beide Seiten der geteilten Ansicht an und reagieren auf den Benutzer, der eine Attraktion in der Tabellenansicht`AttractionHighlighted`() hervorhebt, indem er die neue Attraktion auf der **Detail** Seite von anzeigt. die geteilte Ansicht.

Eine vollständige Implementierung von Tabellen Sichten in einer geteilten Ansicht finden Sie in der Beispiel-app " [tvtables](https://docs.microsoft.com/samples/xamarin/ios-samples/tvos-tvtable) ".

## <a name="table-views-in-detail"></a>Tabellen Sichten im Detail

Da tvos auf IOS basiert, sind Tabellen Sichten und Tabellen Ansichts Controller so konzipiert und Verhalten sich ähnlich. Ausführlichere Informationen zum Arbeiten mit der Tabellenansicht in einer xamarin-App finden Sie in der Dokumentation [Arbeiten mit Tabellen und Zellen](~/ios/user-interface/controls/tables/index.md) in ios.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das Entwerfen und arbeiten mit Tabellen Sichten in einer xamarin. tvos-App behandelt. Und bietet ein Beispiel für das Arbeiten mit einer Tabellenansicht in einer geteilten Ansicht, bei der es sich um die typische Verwendung einer Tabellenansicht in einer tvos-App handelt.



## <a name="related-links"></a>Verwandte Links

- [tvOS-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+tvOS)
- [UITableViewController](https://developer.apple.com/library/prerelease/tvos/documentation/UIKit/Reference/UITableViewController_Class/index.html#//apple_ref/doc/uid/TP40007523)
- [tvOS](https://developer.apple.com/tvos/)
- [tvos Human Interface Guides](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Leitfaden zur APP-Programmierung für tvos](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
