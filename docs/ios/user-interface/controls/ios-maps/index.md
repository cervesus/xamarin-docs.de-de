---
title: Zuordnungen in xamarin. IOS
description: In diesem Dokument wird das IOS MapKit-Framework und seine Verwendung mit xamarin. IOS beschrieben. Darin wird erläutert, wie Sie eine Karte hinzufügen, Sie formatieren, Schwenken und vergrößern, mit der Benutzer Position arbeiten, Anmerkungen hinzufügen, mit aufrufen und Überlagerungen arbeiten und eine lokale Suche durchführen.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 157f797ebb19de1ae00a00328a9c63b051c7224f
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226450"
---
# <a name="maps-in-xamarinios"></a>Zuordnungen in xamarin. IOS

Maps sind ein gängiges Feature in allen modernen mobilen Betriebssystemen. IOS bietet eine systemeigene Zuordnungs Unterstützung durch das Map Kit-Framework. Mit dem Map-Kit können Anwendungen problemlos umfangreiche, interaktive Karten hinzufügen. Diese Zuordnungen können auf verschiedene Arten angepasst werden, z. b. das Hinzufügen von Anmerkungen zum Markieren von Positionen auf einer Karte und das Überlagern von Grafiken beliebiger Formen. Map Kit verfügt auch über integrierte Unterstützung für die Anzeige des aktuellen Speicher Orts eines Geräts.

## <a name="adding-a-map"></a>Hinzufügen einer Karte

Das Hinzufügen einer Zuordnung zu einer Anwendung erfolgt durch hinzu `MKMapView` fügen einer Instanz zur Ansichts Hierarchie, wie unten dargestellt:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

`MKMapView`ist eine `UIView` Unterklasse, die eine Karte anzeigt. Durch einfaches Hinzufügen der Zuordnung mit dem obigen Code wird eine interaktive Karte erzeugt:

![](images/00-map.png "Eine Beispiel Zuordnung")

## <a name="map-style"></a>Karten Stil

`MKMapView`unterstützt drei verschiedene Arten von Karten. Um einen Karten Stil anzuwenden, legen Sie einfach `MapType` die-Eigenschaft auf einen Wert `MKMapType` aus der-Enumeration fest:

```csharp
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
```

Der folgende Screenshot zeigt die verschiedenen verfügbaren Karten Stile:

![](images/01-mapstyles.png "Dieser Screenshot zeigt die verschiedenen verfügbaren Karten Stile.")

## <a name="panning-and-zooming"></a>Schwenken und Zoomen

`MKMapView`bietet Unterstützung für Funktionen der Karten Interaktivität wie z. b.:

- Zoomen über eine Pinsel Bewegung
- Schwenken über eine schwenken-Geste


Diese Funktionen können aktiviert oder deaktiviert werden, indem einfach die `ZoomEnabled` - `ScrollEnabled` Eigenschaft und die `MKMapView` -Eigenschaft der-Instanz festgelegt werden, wobei der Standardwert für beide "true" ist. Wenn Sie z. b. eine statische Karte anzeigen möchten, legen Sie einfach die entsprechenden Eigenschaften auf false fest:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Benutzer Standort

Zusätzlich zur Benutzerinteraktion `MKMapView` verfügt auch über eine integrierte Unterstützung für das Anzeigen des Speicher Orts des Geräts. Hierfür wird das *Core Location* Framework verwendet. Bevor Sie auf den Speicherort des Benutzers zugreifen können, müssen Sie den Benutzer zur Eingabe auffordern. Erstellen Sie hierzu eine Instanz von `CLLocationManager` , und rufen `RequestWhenInUseAuthorization`Sie auf.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Beachten Sie, dass in ios-Versionen vor 8,0 der Versuch, `RequestWhenInUseAuthorization` einen aufzurufen, zu einem Fehler führt. Stellen Sie sicher, dass Sie die Version von IOS überprüfen, bevor Sie diesen anrufen, wenn Sie beabsichtigen, Versionen vor 8 zu unterstützen.

Der Zugriff auf den Speicherort des Benutzers erfordert auch Änderungen an **Info. plist**. Die folgenden Schlüssel, die mit den Standortdaten zusammenhängen, sollten festgelegt werden:

- **NSLocationWhenInUseUsageDescription**: Für den Zugriff auf den Standort des Benutzers während dieser mit Ihrer App interagiert.
- **NSLocationAlwaysUsageDescription**: Für den Zugriff Ihrer App auf den Standort des Benutzers aus dem Hintergrund.

Sie können diese Schlüssel hinzufügen, indem Sie " **Info. plist** " öffnen und unten im Editor die Option *Quelle* auswählen.

Nachdem Sie " **Info. plist** " aktualisiert und den Benutzer zur Eingabe der Berechtigung für den Zugriff auf seinen Speicherort aufgefordert haben, können Sie den Speicherort des `ShowsUserLocation` Benutzers auf der Karte anzeigen, indem Sie die Eigenschaft auf "true" festlegen

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Die Warnung \"Speicherort Zugriff zulassen\"")

## <a name="annotations"></a>Anmerkungen

 `MKMapView`unterstützt auch das Anzeigen von Bildern, die als Anmerkungen bezeichnet werden, auf einer Karte. Dabei kann es sich entweder um benutzerdefinierte Images oder um System definierte Pins verschiedener Farben handeln. Der folgende Screenshot zeigt z. b. eine Karte mit einer PIN und einem benutzerdefinierten Bild:

 ![](images/03-annotations.png "Dieser Screenshot zeigt eine Karte mit einer PIN und einem benutzerdefinierten Bild.")

### <a name="adding-an-annotation"></a>Hinzufügen einer Anmerkung

Eine Anmerkung selbst besteht aus zwei Teilen:

- Das `MKAnnotation` -Objekt, das Modelldaten zur Anmerkung enthält, z. b. den Titel und den Speicherort der Anmerkung.
- Der `MKAnnotationView` , der das anzuzeigende Bild und optional eine Legende enthält, die angezeigt wird, wenn der Benutzer auf die Anmerkung tippt.


Das Map-Kit verwendet das IOS-Delegierungs Muster, um Anmerkungen zu einer Zuordnung `Delegate` hinzuzufügen, `MKMapView` wobei die-Eigenschaft des `MKMapViewDelegate`auf eine Instanz von festgelegt ist. Es handelt sich um die Implementierung dieses Delegaten, die für `MKAnnotationView` die Rückgabe von für eine Anmerkung zuständig ist.

Zum Hinzufügen einer Anmerkung wird zuerst die-Anmerkung durch Aufrufen `AddAnnotations` von für die `MKMapView` -Instanz hinzugefügt:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Wenn der Speicherort der Anmerkung in der Zuordnung sichtbar wird, ruft die `MKMapView` die- `GetViewForAnnotation` Methode des Delegaten auf, um `MKAnnotationView` die anzuzeigende anzuzeigen.

Der folgende Code gibt z. b. ein vom System `MKPinAnnotationView`bereitgestelltes zurück:

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

Um Speicherplatz zu `MKMapView` sparen, kann die Ansicht der Anmerkung für die Wiederverwendung in einem Pool zusammengefasst werden, ähnlich wie bei der Wiederverwendung von Tabellenzellen. Das Abrufen `DequeueReusableAnnotation`einer Anmerkung-Ansicht aus dem Pool erfolgt mit einem-Befehl:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Zeigen von Legenden

Wie bereits erwähnt, kann eine Anmerkung optional eine Legende anzeigen. Um eine Legende anzuzeigen, legen Sie `CanShowCallout` einfach `MKAnnotationView`auf true fest. Dies führt dazu, dass der Titel der Anmerkung angezeigt wird, wenn die Anmerkung abgetippt wird, wie hier gezeigt:

 ![](images/04-callout.png "Der angezeigte Anmerkungen Titel")

### <a name="customizing-the-callout"></a>Anpassen der Legende

Die Legende kann auch so angepasst werden, dass Sie Links-und Rechte Zubehör Sichten anzeigt, wie unten dargestellt:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Dieser Code führt zu folgender Legende:

 ![](images/05-callout-accessories.png "Beispiel Legende")

Um den Benutzer zu behandeln, der auf das richtige Zubehör tippt `CalloutAccessoryControlTapped` , implementieren Sie `MKMapViewDelegate`einfach die-Methode in:

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

- Erstellen eines Modell Objekts für das Overlay und Hinzufügen des `MKMapView` Objekts zum.
- Erstellen einer Ansicht für die Überlagerung in `MKMapViewDelegate` .


Das Modell für die Überlagerung kann eine `MKShape` beliebige Unterklasse sein. Xamarin. IOS enthält `MKShape` Unterklassen für Polygone, Polylinien und Kreise über die `MKPolygon`Klassen, `MKPolyline` und `MKCircle` .

Der folgende Code wird z. b. zum Hinzufügen `MKCircle`eines verwendet:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Die Ansicht für eine Überlagerung ist `MKOverlayView` eine-Instanz, die von `GetViewForOverlay` der in `MKMapViewDelegate`der zurückgegeben wird. Jede `MKShape` verfügt über eine `MKOverlayView` entsprechende, die weiß, wie die angegebene Form angezeigt wird. `MKPolygon` Hierfür ist`MKPolygonView`. Entsprechend `MKPolyline` `MKCircle` entspricht, und ist`MKCircleView`für vorhanden. `MKPolylineView`

Der folgende Code gibt z. b. `MKCircleView` einen für `MKCircle`eine zurück:

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

 ![](images/06-circle-overlay.png "Ein Kreis, der auf der Karte angezeigt wird.")

## <a name="local-search"></a>Lokale Suche

IOS enthält eine lokale Such-API mit dem Map-Kit, mit der asynchrone Such Punkte in einer bestimmten geografischen Region gesucht werden können.

Um eine lokale Suche auszuführen, muss eine Anwendung die folgenden Schritte ausführen:

1. Erstellen `MKLocalSearchRequest` Sie ein Objekt.
1. Erstellen Sie `MKLocalSearch` ein-Objekt `MKLocalSearchRequest` aus der.
1. Ruft die `Start` -Methode für `MKLocalSearch` das-Objekt auf.
1. Rufen Sie `MKLocalSearchResponse` das-Objekt in einem Rückruf ab.


Die lokale Such-API selbst stellt keine Benutzeroberfläche bereit. Es ist nicht einmal erforderlich, eine Karte zu verwenden. Um jedoch eine praktische Verwendung der lokalen Suche zu ermöglichen, muss eine Anwendung eine bestimmte Möglichkeit bieten, eine Suchabfrage anzugeben und Ergebnisse anzuzeigen. Da die Ergebnisse auch Speicherort Daten enthalten, ist es häufig sinnvoll, Sie auf einer Karte anzuzeigen.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Hinzufügen einer lokalen Such Benutzeroberfläche

Eine Möglichkeit, eine Sucheingabe zu akzeptieren, `UISearchBar`ist eine, die von `UISearchController` einem bereitgestellt wird und Ergebnisse in einer Tabelle anzeigt.

Der folgende Code fügt in `UISearchController` der `ViewDidLoad` -Methode von `MapViewController`die (die eine Suchleisten Eigenschaft enthält) hinzu:

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

In diesem Code Ausschnitt haben wir einen weiteren benutzerdefinierten Ansichts Controller `searchResultsController` – – erstellt, in dem die Suchergebnisse angezeigt werden. Anschließend haben wir dieses Objekt verwendet, um das Search Controller-Objekt zu erstellen. Wir haben auch einen neuen Suchvorgang erstellt, der aktiv wird, wenn der Benutzer mit der Suchleiste interagiert. Er empfängt Benachrichtigungen zu Such Vorgängen mit den einzelnen Tastatureingaben und ist für die Aktualisierung der Benutzeroberfläche verantwortlich.
Wir sehen uns nun an, wie Sie `searchResultsController` `searchResultsUpdater` und später in diesem Handbuch implementieren.

Dies führt dazu, dass eine Suchleiste über der Karte angezeigt wird, wie unten dargestellt:

 ![](images/07-searchbar.png "Eine Suchleiste, die über der Karte angezeigt wird.")



### <a name="displaying-the-search-results"></a>Anzeigen der Suchergebnisse

Zum Anzeigen von Suchergebnissen müssen wir einen benutzerdefinierten Ansichts Controller erstellen. normalerweise `UITableViewController`ein. Wie oben gezeigt, wird `searchResultsController` der an den Konstruktor `searchController` von übergeben, wenn er erstellt wird.
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

Fungiert als Vermittler zwischen der Suchleiste `searchController`und den Suchergebnissen von. `SearchResultsUpdater`

In diesem Beispiel müssen Sie zuerst die Suchmethode in `SearchResultsViewController`erstellen. Zu diesem `MKLocalSearch` Zweck müssen wir ein-Objekt erstellen und es verwenden, um eine Suche nach einem `MKLocalSearchRequest`auszugeben. die Ergebnisse werden in einem Rückruf abgerufen, `Start` der `MKLocalSearch` an die-Methode des-Objekts übermittelt wird. Die Ergebnisse werden dann in einem `MKLocalSearchResponse` -Objekt zurückgegeben, das ein Array von `MKMapItem` -Objekten enthält:

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

Anschließend erstellen wir in unserem `MapViewController` eine benutzerdefinierte Implementierung von `UISearchResultsUpdating`, die der Eigenschaft `SearchResultsUpdater` von `searchController` im Abschnitt [Hinzufügen einer lokalen Suchbenutzeroberfläche](#Adding_a_Local_Search_UI) zugeordnet ist.

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

 ![](images/08-search-results.png "Eine Anmerkung, die der Karte hinzugefügt wird, wenn ein Element aus den Ergebnissen ausgewählt wird.")

> [!IMPORTANT]
> `UISearchController`wurde in ios 8 implementiert. Wenn Sie Geräte vor diesem Zeitpunkt unterstützen möchten, müssen Sie verwenden `UISearchDisplayController`.



## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde das *map* *Kit* -Framework für IOS untersucht. Zuerst wurde erläutert, wie die- `MKMapView` Klasse das einschließen interaktiver Zuordnungen in eine Anwendung zulässt. Anschließend wurde erläutert, wie Karten mithilfe von Anmerkungen und Überlagerungen angepasst werden. Schließlich wurden die lokalen Suchfunktionen untersucht, die dem Map-Kit mit IOS 6,1 hinzugefügt wurden, und es wird gezeigt, wie Sie Standortbasierte Abfragen für relevante Punkte verwenden und zu einer Karte hinzufügen.

## <a name="related-links"></a>Verwandte Links

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [Mapdemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
