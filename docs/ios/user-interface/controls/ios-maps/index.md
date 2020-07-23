---
title: Zuordnungen in xamarin. IOS
description: In diesem Dokument wird das IOS MapKit-Framework und seine Verwendung mit xamarin. IOS beschrieben. Darin wird erläutert, wie Sie eine Karte hinzufügen, Sie formatieren, Schwenken und vergrößern, mit der Benutzer Position arbeiten, Anmerkungen hinzufügen, mit aufrufen und Überlagerungen arbeiten und eine lokale Suche durchführen.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 177701b8b50edea965e97da225265912f1f0c198
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932326"
---
# <a name="maps-in-xamarinios"></a>Zuordnungen in xamarin. IOS

Maps sind ein gängiges Feature in allen modernen mobilen Betriebssystemen. IOS bietet eine systemeigene Zuordnungs Unterstützung durch das Map Kit-Framework. Mit dem Map-Kit können Anwendungen problemlos umfangreiche, interaktive Karten hinzufügen. Diese Zuordnungen können auf verschiedene Arten angepasst werden, z. b. das Hinzufügen von Anmerkungen zum Markieren von Positionen auf einer Karte und das Überlagern von Grafiken beliebiger Formen. Map Kit verfügt auch über integrierte Unterstützung für die Anzeige des aktuellen Speicher Orts eines Geräts.

## <a name="adding-a-map"></a>Hinzufügen einer Karte

Das Hinzufügen einer Zuordnung zu einer Anwendung erfolgt durch Hinzufügen einer `MKMapView` Instanz zur Ansichts Hierarchie, wie unten dargestellt:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

`MKMapView`ist eine `UIView` Unterklasse, die eine Karte anzeigt. Durch einfaches Hinzufügen der Zuordnung mit dem obigen Code wird eine interaktive Karte erzeugt:

![Eine Beispiel Zuordnung](images/00-map.png)

## <a name="map-style"></a>Karten Stil

`MKMapView`unterstützt drei verschiedene Arten von Karten. Um einen Karten Stil anzuwenden, legen Sie einfach die- `MapType` Eigenschaft auf einen Wert aus der- `MKMapType` Enumeration fest:

```csharp
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
```

Der folgende Screenshot zeigt die verschiedenen verfügbaren Karten Stile:

![Dieser Screenshot zeigt die verschiedenen verfügbaren Karten Stile.](images/01-mapstyles.png)

## <a name="panning-and-zooming"></a>Schwenken und Zoomen

`MKMapView`bietet Unterstützung für Funktionen der Karten Interaktivität wie z. b.:

- Zoomen über eine Pinsel Bewegung
- Schwenken über eine schwenken-Geste

Diese Funktionen können aktiviert oder deaktiviert werden, indem einfach die `ZoomEnabled` -Eigenschaft und die-Eigenschaft der-Instanz festgelegt werden `ScrollEnabled` `MKMapView` , wobei der Standardwert für beide "true" ist. Wenn Sie z. b. eine statische Karte anzeigen möchten, legen Sie einfach die entsprechenden Eigenschaften auf false fest:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Benutzerstandort

Zusätzlich zur Benutzerinteraktion `MKMapView` verfügt auch über eine integrierte Unterstützung für das Anzeigen des Speicher Orts des Geräts. Hierfür wird das *Core Location* Framework verwendet. Bevor Sie auf den Speicherort des Benutzers zugreifen können, müssen Sie den Benutzer zur Eingabe auffordern. Erstellen Sie hierzu eine Instanz von, `CLLocationManager` und rufen Sie auf `RequestWhenInUseAuthorization` .

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Beachten Sie, dass in ios-Versionen vor 8,0 der Versuch, einen aufzurufen, zu `RequestWhenInUseAuthorization` einem Fehler führt. Stellen Sie sicher, dass Sie die Version von IOS überprüfen, bevor Sie diesen anrufen, wenn Sie beabsichtigen, Versionen vor 8 zu unterstützen.

Der Zugriff auf den Speicherort des Benutzers erfordert auch Änderungen an **Info. plist**. Die folgenden Schlüssel, die mit den Standortdaten zusammenhängen, sollten festgelegt werden:

- **NSLocationWhenInUseUsageDescription**: Für den Zugriff auf den Standort des Benutzers während dieser mit Ihrer App interagiert.
- **NSLocationAlwaysUsageDescription**: Für den Zugriff Ihrer App auf den Standort des Benutzers aus dem Hintergrund.

Sie können diese Schlüssel hinzufügen, indem Sie " **Info. plist** " öffnen und unten im Editor die Option *Quelle* auswählen.

Nachdem Sie " **Info. plist** " aktualisiert und den Benutzer zur Eingabe der Berechtigung für den Zugriff auf seinen Speicherort aufgefordert haben, können Sie den Speicherort des Benutzers auf der Karte anzeigen, indem Sie die `ShowsUserLocation` Eigenschaft auf "true" festlegen

```csharp
map.ShowsUserLocation = true;
```

 ![Die Warnung "Speicherort Zugriff zulassen"](images/02-location-alert.png)

## <a name="annotations"></a>Anmerkungen

 `MKMapView`unterstützt auch das Anzeigen von Bildern, die als Anmerkungen bezeichnet werden, auf einer Karte. Dabei kann es sich entweder um benutzerdefinierte Images oder um System definierte Pins verschiedener Farben handeln. Der folgende Screenshot zeigt z. b. eine Karte mit einer PIN und einem benutzerdefinierten Bild:

 ![Dieser Screenshot zeigt eine Karte mit einer PIN und einem benutzerdefinierten Bild.](images/03-annotations.png)

### <a name="adding-an-annotation"></a>Hinzufügen einer Anmerkung

Eine Anmerkung selbst besteht aus zwei Teilen:

- Das- `MKAnnotation` Objekt, das Modelldaten zur Anmerkung enthält, z. b. den Titel und den Speicherort der Anmerkung.
- Der `MKAnnotationView` , der das anzuzeigende Bild und optional eine Legende enthält, die angezeigt wird, wenn der Benutzer auf die Anmerkung tippt.

Das Map-Kit verwendet das IOS-Delegierungs Muster, um Anmerkungen zu einer Zuordnung hinzuzufügen, wobei die- `Delegate` Eigenschaft des `MKMapView` auf eine Instanz von festgelegt ist `MKMapViewDelegate` . Es handelt sich um die Implementierung dieses Delegaten, die für die Rückgabe von `MKAnnotationView` für eine Anmerkung zuständig ist.

Zum Hinzufügen einer Anmerkung wird zuerst die-Anmerkung durch Aufrufen von `AddAnnotations` für die- `MKMapView` Instanz hinzugefügt:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Wenn der Speicherort der Anmerkung in der Zuordnung sichtbar wird, ruft die die-Methode des Delegaten auf, `MKMapView` `GetViewForAnnotation` um die `MKAnnotationView` anzuzeigende anzuzeigen.

Der folgende Code gibt z. b. ein vom System bereitgestelltes zurück `MKPinAnnotationView` :

```csharp
string pId = "PinAnnotation";

public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
{
    if (annotation is MKUserLocation)
        return null;

    // create pin annotation view
    MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

    if (pinView == null)
        pinView = new MKPinAnnotationView (annotation, pId);

    ((MKPinAnnotationView)pinView).PinColor = MKPinAnnotationColor.Red;
    pinView.CanShowCallout = true;

    return pinView;
}
```

### <a name="reusing-annotations"></a>Wieder verwenden von Anmerkungen

Um Speicherplatz zu sparen, `MKMapView` kann die Ansicht der Anmerkung für die Wiederverwendung in einem Pool zusammengefasst werden, ähnlich wie bei der Wiederverwendung von Tabellenzellen. Das Abrufen einer Anmerkung-Ansicht aus dem Pool erfolgt mit einem-Befehl `DequeueReusableAnnotation` :

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Zeigen von Legenden

Wie bereits erwähnt, kann eine Anmerkung optional eine Legende anzeigen. Um eine Legende anzuzeigen, legen Sie einfach `CanShowCallout` auf true fest `MKAnnotationView` . Dies führt dazu, dass der Titel der Anmerkung angezeigt wird, wenn die Anmerkung abgetippt wird, wie hier gezeigt:

 ![Der angezeigte Anmerkungen Titel](images/04-callout.png)

### <a name="customizing-the-callout"></a>Anpassen der Legende

Die Legende kann auch so angepasst werden, dass Sie Links-und Rechte Zubehör Sichten anzeigt, wie unten dargestellt:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Dieser Code führt zu folgender Legende:

 ![Beispiel Legende](images/05-callout-accessories.png)

Um den Benutzer zu behandeln, der auf das richtige Zubehör tippt, implementieren Sie einfach die- `CalloutAccessoryControlTapped` Methode in `MKMapViewDelegate` :

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Überlagerungen

Eine andere Möglichkeit zur ebenengrafik auf einer Karte ist die Verwendung von Überlagerungen. Überlagerungen unterstützen das Zeichnen grafischer Inhalte, die beim Zoomen mit der Karte skaliert werden. IOS bietet Unterstützung für verschiedene Arten von Überlagerungen, einschließlich:

- Polygone: wird häufig verwendet, um einen Bereich auf einer Karte hervorzuheben.
- Polylines: wird häufig angezeigt, wenn eine Route angezeigt wird.
- Kreise: wird verwendet, um einen kreisförmigen Bereich einer Karte hervorzuheben.

Darüber hinaus können benutzerdefinierte Überlagerungen erstellt werden, um beliebige Geometrien mit granulartem, angepassten Zeichencode anzuzeigen. Beispielsweise wäre das Wetterradar ein guter Kandidat für eine benutzerdefinierte Überlagerung.

#### <a name="adding-an-overlay"></a>Hinzufügen einer Überlagerung

Ähnlich wie bei Anmerkungen umfasst das Hinzufügen einer Überlagerung zwei Teile:

- Erstellen eines Modell Objekts für das Overlay und Hinzufügen des Objekts zum `MKMapView` .
- Erstellen einer Ansicht für die Überlagerung in `MKMapViewDelegate` .

Das Modell für die Überlagerung kann eine beliebige `MKShape` Unterklasse sein. Xamarin. IOS enthält `MKShape` Unterklassen für Polygone, Polylinien und Kreise über die `MKPolygon` Klassen, `MKPolyline` und `MKCircle` .

Der folgende Code wird z. b. zum Hinzufügen eines verwendet `MKCircle` :

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Die Ansicht für eine Überlagerung ist eine- `MKOverlayView` Instanz, die von der in der zurückgegeben wird `GetViewForOverlay` `MKMapViewDelegate` . Jede `MKShape` verfügt über eine entsprechende `MKOverlayView` , die weiß, wie die angegebene Form angezeigt wird. Hierfür `MKPolygon` ist `MKPolygonView` . Entsprechend `MKPolyline` entspricht `MKPolylineView` , und `MKCircle` ist für vorhanden `MKCircleView` .

Der folgende Code gibt z. b. einen `MKCircleView` für eine zurück `MKCircle` :

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Dadurch wird ein Kreis auf der Karte angezeigt, wie hier gezeigt:

 ![Ein Kreis, der auf der Karte angezeigt wird.](images/06-circle-overlay.png)

## <a name="local-search"></a>Lokale Suche

IOS enthält eine lokale Such-API mit dem Map-Kit, mit der asynchrone Such Punkte in einer bestimmten geografischen Region gesucht werden können.

Um eine lokale Suche auszuführen, muss eine Anwendung die folgenden Schritte ausführen:

1. Erstellen Sie ein `MKLocalSearchRequest` Objekt.
1. Erstellen Sie ein- `MKLocalSearch` Objekt aus der `MKLocalSearchRequest` .
1. Ruft die- `Start` Methode für das- `MKLocalSearch` Objekt auf.
1. Rufen Sie das- `MKLocalSearchResponse` Objekt in einem Rückruf ab.

Die lokale Such-API selbst stellt keine Benutzeroberfläche bereit. Es ist nicht einmal erforderlich, eine Karte zu verwenden. Um jedoch eine praktische Verwendung der lokalen Suche zu ermöglichen, muss eine Anwendung eine bestimmte Möglichkeit bieten, eine Suchabfrage anzugeben und Ergebnisse anzuzeigen. Da die Ergebnisse auch Speicherort Daten enthalten, ist es häufig sinnvoll, Sie auf einer Karte anzuzeigen.

<a name="Adding_a_Local_Search_UI"></a>

### <a name="adding-a-local-search-ui"></a>Hinzufügen einer lokalen Such Benutzeroberfläche

Eine Möglichkeit, eine Sucheingabe zu akzeptieren, ist eine `UISearchBar` , die von einem bereitgestellt wird `UISearchController` und Ergebnisse in einer Tabelle anzeigt.

Der folgende Code fügt `UISearchController` in der-Methode von die (die eine Suchleisten Eigenschaft enthält) hinzu `ViewDidLoad` `MapViewController` :

```csharp
//Creates an instance of a custom View Controller that holds the results
var searchResultsController = new SearchResultsViewController (map);

//Creates a search controller updater
var searchUpdater = new SearchResultsUpdator ();
searchUpdater.UpdateSearchResults += searchResultsController.Search;

//add the search controller
searchController = new UISearchController (searchResultsController) {
                SearchResultsUpdater = searchUpdater
            };

//format the search bar
searchController.SearchBar.SizeToFit ();
searchController.SearchBar.SearchBarStyle = UISearchBarStyle.Minimal;
searchController.SearchBar.Placeholder = "Enter a search query";

//the search bar is contained in the navigation bar, so it should be visible
searchController.HidesNavigationBarDuringPresentation = false;

//Ensure the searchResultsController is presented in the current View Controller
DefinesPresentationContext = true;

//Set the search bar in the navigation bar
NavigationItem.TitleView = searchController.SearchBar;
```

Beachten Sie, dass Sie dafür verantwortlich sind, das Suchleisten Objekt in die Benutzeroberfläche einzubinden. In diesem Beispiel haben wir es der TitleView der Navigationsleiste zugewiesen, aber wenn Sie in Ihrer Anwendung keinen Navigations Controller verwenden, müssen Sie einen anderen Speicherort für die Anzeige finden.

In diesem Code Ausschnitt haben wir einen weiteren benutzerdefinierten Ansichts Controller – `searchResultsController` – erstellt, in dem die Suchergebnisse angezeigt werden. Anschließend haben wir dieses Objekt verwendet, um das Search Controller-Objekt zu erstellen. Wir haben auch einen neuen Suchvorgang erstellt, der aktiv wird, wenn der Benutzer mit der Suchleiste interagiert. Er empfängt Benachrichtigungen zu Such Vorgängen mit den einzelnen Tastatureingaben und ist für die Aktualisierung der Benutzeroberfläche verantwortlich.
Wir sehen uns nun an, wie Sie `searchResultsController` und `searchResultsUpdater` später in diesem Handbuch implementieren.

Dies führt dazu, dass eine Suchleiste über der Karte angezeigt wird, wie unten dargestellt:

 ![Eine Suchleiste, die über der Karte angezeigt wird.](images/07-searchbar.png)

### <a name="displaying-the-search-results"></a>Anzeigen der Suchergebnisse

Zum Anzeigen von Suchergebnissen müssen wir einen benutzerdefinierten Ansichts Controller erstellen. normalerweise ein `UITableViewController` . Wie oben gezeigt, `searchResultsController` wird der an den Konstruktor von übergeben, `searchController` Wenn er erstellt wird.
Der folgende Code ist ein Beispiel für die Erstellung dieses benutzerdefinierten Ansichts Controllers:

```csharp
public class SearchResultsViewController : UITableViewController
{
    static readonly string mapItemCellId = "mapItemCellId";
    MKMapView map;

    public List<MKMapItem> MapItems { get; set; }

    public SearchResultsViewController (MKMapView map)
    {
        this.map = map;

        MapItems = new List<MKMapItem> ();
    }

    public override nint RowsInSection (UITableView tableView, nint section)
    {
        return MapItems.Count;
    }

    public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath)
    {
        var cell = tableView.DequeueReusableCell (mapItemCellId);

        if (cell == null)
            cell = new UITableViewCell ();

        cell.TextLabel.Text = MapItems [indexPath.Row].Name;
        return cell;
    }

    public override void RowSelected (UITableView tableView, NSIndexPath indexPath)
    {
        // add item to map
        CLLocationCoordinate2D coord = MapItems [indexPath.Row].Placemark.Location.Coordinate;
        map.AddAnnotations (new MKPointAnnotation () {
            Title = MapItems [indexPath.Row].Name,
            Coordinate = coord
        });

        map.SetCenterCoordinate (coord, true);

        DismissViewController (false, null);
    }

    public void Search (string forSearchString)
    {
        // create search request
        var searchRequest = new MKLocalSearchRequest ();
        searchRequest.NaturalLanguageQuery = forSearchString;
        searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

        // perform search
        var localSearch = new MKLocalSearch (searchRequest);

        localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
            if (response != null && error == null) {
                this.MapItems = response.MapItems.ToList ();
                this.TableView.ReloadData ();
            } else {
                Console.WriteLine ("local search error: {0}", error);
            }
        });

    }
}
```

### <a name="updating-the-search-results"></a>Aktualisieren der Suchergebnisse

`SearchResultsUpdater`Fungiert als Vermittler zwischen der `searchController` Suchleiste und den Suchergebnissen von.

In diesem Beispiel müssen Sie zuerst die Suchmethode in Erstellen `SearchResultsViewController` . Zu diesem Zweck müssen wir ein `MKLocalSearch` -Objekt erstellen und es verwenden, um eine Suche nach einem auszugeben `MKLocalSearchRequest` . die Ergebnisse werden in einem Rückruf abgerufen, der an die-Methode des-Objekts übermittelt wird `Start` `MKLocalSearch` . Die Ergebnisse werden dann in einem-Objekt zurückgegeben, das `MKLocalSearchResponse` ein Array von- `MKMapItem` Objekten enthält:

```csharp
public void Search (string forSearchString)
{
    // create search request
    var searchRequest = new MKLocalSearchRequest ();
    searchRequest.NaturalLanguageQuery = forSearchString;
    searchRequest.Region = new MKCoordinateRegion (map.UserLocation.Coordinate, new MKCoordinateSpan (0.25, 0.25));

    // perform search
    var localSearch = new MKLocalSearch (searchRequest);

    localSearch.Start (delegate (MKLocalSearchResponse response, NSError error) {
        if (response != null && error == null) {
            this.MapItems = response.MapItems.ToList ();
            this.TableView.ReloadData ();
        } else {
            Console.WriteLine ("local search error: {0}", error);
        }
    });

}
```

Anschließend `MapViewController` erstellen wir eine benutzerdefinierte Implementierung von `UISearchResultsUpdating` , die der- `SearchResultsUpdater` Eigenschaft des `searchController` im Abschnitt [Hinzufügen einer lokalen Such Benutzeroberfläche](#Adding_a_Local_Search_UI) zugewiesen wird:

```csharp
public class SearchResultsUpdator : UISearchResultsUpdating
{
    public event Action<string> UpdateSearchResults = delegate {};

    public override void UpdateSearchResultsForSearchController (UISearchController searchController)
    {
        this.UpdateSearchResults (searchController.SearchBar.Text);
    }
}
```

Die obige Implementierung fügt der Karte eine Anmerkung hinzu, wenn ein Element aus den Ergebnissen ausgewählt wird, wie unten dargestellt:

 ![Eine Anmerkung, die der Karte hinzugefügt wird, wenn ein Element aus den Ergebnissen ausgewählt wird.](images/08-search-results.png)

> [!IMPORTANT]
> `UISearchController`wurde in ios 8 implementiert. Wenn Sie Geräte vor diesem Zeitpunkt unterstützen möchten, müssen Sie verwenden `UISearchDisplayController` .

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das *map* *Kit* -Framework für IOS untersucht. Zuerst wurde erläutert, wie die- `MKMapView` Klasse das einschließen interaktiver Zuordnungen in eine Anwendung zulässt. Anschließend wurde erläutert, wie Karten mithilfe von Anmerkungen und Überlagerungen angepasst werden. Schließlich wurden die lokalen Suchfunktionen untersucht, die dem Map-Kit mit IOS 6,1 hinzugefügt wurden, und es wird gezeigt, wie Sie Standortbasierte Abfragen für relevante Punkte verwenden und zu einer Karte hinzufügen.

## <a name="related-links"></a>Ähnliche Themen

- [Searchcontroller](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [Mapdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
