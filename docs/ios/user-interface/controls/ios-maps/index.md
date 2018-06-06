---
title: Karten in Xamarin.iOS
description: Dieses Dokument beschreibt das iOS-MapKit-Framework und wie er mit Xamarin.iOS verwendet wird. Es wird erläutert, wie zum Hinzufügen einer Zuordnung, Stil, schwenken und zoomen, arbeiten mit Benutzerstandort einstellen, Hinzufügen von Anmerkungen, arbeiten mit Legenden und für Überlagerungen und lokale Suche durchführen.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 3649c8eb9c8c1a82940b8e2eece7d2bfd005d024
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790229"
---
# <a name="maps-in-xamarinios"></a>Karten in Xamarin.iOS

Karten sind ein gängiges Feature in allen modernen mobilen Betriebssystemen. iOS bietet Unterstützung systemintern über das Map-Kit-Framework. Map-Kit können Anwendungen problemlos umfassende, interaktive Karten hinzufügen. Diese Zuordnungen können in einer Vielzahl von Möglichkeiten, wie z. B. Hinzufügen von Anmerkungen zum Kennzeichnen von Standorten auf einer Karte und Überlagern von Grafiken der willkürlichen Formen angepasst werden. Map-Kit hat auch integrierten Unterstützung für die aktuelle Position eines Geräts angezeigt.

## <a name="adding-a-map"></a>Hinzufügen einer Karte

Hinzufügen einer Zuordnung zu einer Anwendung geschieht durch Hinzufügen einer `MKMapView` -Instanz, auf die Hierarchie anzeigen, wie unten dargestellt:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` ist eine `UIView` Unterklasse, die eine Zuordnung zeigt. Hinzufügen der Zuordnung mithilfe des obigen Codes erzeugt eine interaktive Zuordnung:

 ![](images/00-map.png "Eine Beispiel-Karte")

## <a name="map-style"></a>Map-Stil

 `MKMapView` unterstützt die 3 verschiedene Formaten von Zuordnungen. Um einen Stil für die Zuordnung zu anzuwenden, legen Sie einfach die `MapType` Eigenschaft auf einen Wert aus der `MKMapType` Enumeration:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Der folgende Screenshot zeigen die unterschiedlichen Zuordnung Stile, die verfügbar sind:

 ![](images/01-mapstyles.png "Diesem Screenshot werden die unterschiedlichen Zuordnung-Formatvorlagen, die verfügbar sind")

## <a name="panning-and-zooming"></a>Schwenken und Zoomen

 `MKMapView` Unterstützung für die Zuordnung Interaktivitätsfunktionen enthält wie z. B.:

-  Vergrößern/verkleinern, über eine zwei-Finger-Geste
-  Über eine Geste Schwenken Schwenken


Diese Features können aktiviert oder deaktiviert, indem einfach die `ZoomEnabled` und `ScrollEnabled` Eigenschaften der `MKMapView` Instanz, in dem der Standardwert für beide "true" ist. Z. B. zum Anzeigen einer statischen Zuordnung einfach die entsprechenden Eigenschaften auf False festgelegt:

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Benutzerstandort einstellen

Zusätzlich zu den Benutzereingriff `MKMapView` verfügt auch über integrierte Unterstützung für den Speicherort des Geräts angezeigt. Dies geschieht mithilfe der *zentralen Standort* Framework. Bevor Sie den Standort des Benutzers zugreifen können, muss den Benutzer aufgefordert werden. Zu diesem Zweck erstellen Sie eine Instanz des `CLLocationManager` , und rufen Sie `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Beachten Sie, dass in Versionen von iOS 8.0, bei dem Versuch, rufen Sie vor der `RequestWhenInUseAuthorization` führt zu einem Fehler. Vergewissern Sie sich die iOS-Version vornehmen, von denen aufgerufen, wenn Sie, zur Unterstützung von Vorgängerversionen von 8 beabsichtigen.

Erfordert den Zugriff auf den Standort des Benutzers auch Änderungen an **"Info.plist"**. Die folgenden Schlüssel, die mit den Standortdaten zusammenhängen, sollten festgelegt werden:

- **NSLocationWhenInUseUsageDescription**: Für den Zugriff auf den Standort des Benutzers während dieser mit Ihrer App interagiert.
- **NSLocationAlwaysUsageDescription**: Für den Zugriff Ihrer App auf den Standort des Benutzers aus dem Hintergrund.

Sie können diese Schlüssel hinzufügen, indem Sie öffnen **"Info.plist"** auswählen und *Quelle* am unteren Rand der Editor.

Nachdem Sie aktualisiert haben **"Info.plist"** und aufgefordert, die Benutzer die Berechtigung zum Zugriff auf ihren Speicherort, können Sie den Standort des Benutzers auf der Karte anzeigen, durch Festlegen der `ShowsUserLocation` Eigenschaft auf "true":

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Warnung Zugriff Speicherort zulassen")
 
## <a name="annotations"></a>Anmerkungen

 `MKMapView` unterstützt auch anzeigen von Bildern, die als Anmerkungen, auf einer Karte bezeichnet. Dabei kann es sich um benutzerdefinierte Bilder oder systemdefinierte Pins verschiedener Farben handeln. Das folgende Bildschirmfoto zeigt z. B. eine Zuordnung mit einem sowohl eine Pin und ein benutzerdefiniertes Image:

 ![](images/03-annotations.png "Diese bildschirmabbildung zeigt eine Zuordnung mit einem sowohl eine Pin und eines benutzerdefinierten Abbilds")

### <a name="adding-an-annotation"></a>Hinzufügen einer Anmerkung

Eine Anmerkung selbst besteht aus zwei Teilen:

-  Die `MKAnnotation` -Objekt, das Daten des Modells über die Anmerkung, z. B. den Titel und den Speicherort der Anmerkung enthält.
-  Die `MKAnnotationView` , das das Bild, das Anzeigen und optional eine Legende, die angezeigt wird, wenn der Benutzer die Anmerkung tippt enthält.


Zuordnung Kit verwendet die iOS-delegationsmuster zum Hinzufügen von Anmerkungen zu einer Zuordnung, in dem die `Delegate` Eigenschaft von der `MKMapView` festgelegt ist, mit einer Instanz von ein `MKMapViewDelegate`. Es handelt sich dieser Delegat-Implementierung, die verantwortlich für die Rückgabe der `MKAnnotationView` für eine Anmerkung.

Um eine Anmerkung hinzuzufügen, zuerst die-Anmerkung wird hinzugefügt, durch den Aufruf `AddAnnotations` auf die `MKMapView` Instanz:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Wenn die Position der Anmerkung auf der Karte angezeigt wird der `MKMapView` wird Aufrufen des Delegaten `GetViewForAnnotation` Methode zum Abrufen der `MKAnnotationView` angezeigt.

Der folgende Code gibt z. B. eine vom System vorgegebene `MKPinAnnotationView`:

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

### <a name="reusing-annotations"></a>Wiederverwenden von Anmerkungen

Um Speicher zu sparen `MKMapView` Anmerkung Ansicht's, für die Wiederverwendung, ähnlich wie bei "zusammengelegt" werden Tabellenzellen wiederverwendet werden können. Abrufen einer Anmerkung-Sicht aus dem Pool erfolgt mit einem Aufruf von `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Anzeigen von Legenden

Wie bereits erwähnt, kann eine Anmerkung optional eine Legende angezeigt. Legen Sie eine Legende einfach anzuzeigende `CanShowCallout` auf "true", auf die `MKAnnotationView`. Dadurch wird die Anmerkung Titel angezeigt wird, wenn die Anmerkung abgerufen werden, wie gezeigt:

 ![](images/04-callout.png "Die Anmerkungen Titel angezeigt wird")

### <a name="customizing-the-callout"></a>Anpassen der Legende

Die Legende kann auch angepasst werden, um Links und rechts Zubehörs Ansichten anzuzeigen, wie unten dargestellt:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Dieser Code führt in der folgenden Beschriftung:

 ![](images/05-callout-accessories.png "Ein Beispiel-Legende")

Um den Benutzer Tippen auf die richtige Zubehör zu behandeln, implementieren Sie einfach die `CalloutAccessoryControlTapped` Methode in der `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Überlagert

Eine weitere Möglichkeit zum Ebene Grafiken auf einer Karte verwendeten überlagert. Overlays unterstützen grafische Zeichnungsinhalt, die mit der Zuordnung skaliert werden, da es vergrößert wird. iOS bietet Unterstützung für mehrere Typen von Überlagerungen, einschließlich:

-  Polygone – häufig verwendet, um einen Bereich auf einer Karte zu markieren.
-  Polylinien - häufig angezeigt wird, wenn eine Route angezeigt.
-  Kreise - verwendet, um eine kreisbereiche einer Zuordnung zu markieren.


Darüber hinaus können benutzerdefinierte Overlays erstellt werden, um beliebige Geometrien mit präzise, benutzerdefinierte Zeichnung Code anzuzeigen. Beispielsweise wäre Weather Netz ein guter Kandidat für eine benutzerdefinierte Überlagerung.

#### <a name="adding-an-overlay"></a>Hinzufügen einer Überlagerung

Mit Anmerkungen vergleichbar, Hinzufügen einer Überlagerung umfasst 2 Teile:

-  Erstellen ein Modellobjekt für die Überlagerung und das Hinzufügen zu den `MKMapView` .
-  Erstellen einer Ansicht für die Überlagerung in die `MKMapViewDelegate` .


Das Modell für die Überlagerung möglich `MKShape` Unterklasse. Enthält Xamarin.iOS `MKShape` Unterklassen für Polygone, Polylinien und Kreise, über die `MKPolygon`, `MKPolyline` und `MKCircle` bzw. Klassen.

Der folgende Code z. B. dient zum Hinzufügen einer `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Die Ansicht für die Überlagerung wird ein `MKOverlayView` -Instanz, die von zurückgegeben wird das `GetViewForOverlay` in der `MKMapViewDelegate`. Jede `MKShape` verfügt über eine entsprechende `MKOverlayView` , die weiß, wie die angegebene Form angezeigt. Für `MKPolygon` besteht `MKPolygonView`. Auf ähnliche Weise `MKPolyline` entspricht `MKPolylineView`, und für `MKCircle` besteht `MKCircleView`.

Im folgenden code gibt beispielsweise eine `MKCircleView` für eine `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Dies zeigt einen Kreis auf der Karte an, wie dargestellt:

 ![](images/06-circle-overlay.png "Ein Kreis, der auf der Karte angezeigt")

## <a name="local-search"></a>Lokale Suche

iOS enthält eine lokale Such-API Zuordnung, mit dem asynchronen sucht in einer angegebenen geografischen Region relevante Punkte können.

Um eine lokale Suche auszuführen, muss eine Anwendung die folgenden Schritte ausführen:

1.  Erstellen Sie `MKLocalSearchRequest` Objekt.
1.  Erstellen einer `MKLocalSearch` -Objekt aus der `MKLocalSearchRequest` .
1.  Rufen Sie die `Start` Methode für die `MKLocalSearch` Objekt.
1.  Abrufen der `MKLocalSearchResponse` Objekt in einem Rückruf.


Der lokale Such-API selbst stellt keine Benutzeroberfläche bereit. Nicht selbst eine Karte verwendet werden müssen. Um lokale Suche praktische zu nutzen, muss eine Anwendung stellt einige Möglichkeit zum Angeben einer Suchabfrage und die Ergebnisse werden angezeigt. Darüber hinaus, da die Ergebnisse Standortdaten enthält, wird oft sinnvoll, diese auf einer Karte darzustellen, erleichtern.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Hinzufügen einer lokalen Suche UI

Eine Möglichkeit zur Suche von Benutzereingaben wird mit einer `UISearchBar`, die im Lieferumfang von einer `UISearchController` und die Ergebnisse in einer Tabelle angezeigt.

Der folgende Code fügt der `UISearchController` (die hat einer Leiste Sucheigenschaft) in der `ViewDidLoad` Methode `MapViewController`:

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

```csharp
Note that you are responsible for incorporating the search bar object into the user interface. In this example, we assigned it to the TitleView of the navigation bar, but if you do not use a navigation controller in your application you will have to find another place to display it.

In this code snippet, we created another custom view controller – `searchResultsController` –  that displays the search results and then we used this object to create our search controller object. We also created a new search updater, which becomes active when the user interacts with the search bar. It receives notifications about searches with each keystroke and is responsible for updating the UI.
We will take a look at how to implement both the `searchResultsController` and the `searchResultsUpdater` later in this guide.

This results in a search bar displayed over the map as shown below:

 ![](images/07-searchbar.png "A search bar displayed over the map")
 


### Displaying the Search Results

To display search results, we need to create a custom View Controller; normally a `UITableViewController`. As shown above, the `searchResultsController` is passed to the constructor of the `searchController` when it is being created.
The following code is an example of how to create this custom View Controller:

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

### <a name="updating-the-search-results"></a>Aktualisieren die Suchergebnisse

Die `SearchResultsUpdater` dient als Vermittler zwischen der `searchController`des Suchleiste und Suchergebnisse. 

In diesem Beispiel wird, müssen Sie zuerst erstellen Sie die Search-Methode in der `SearchResultsViewController`. Zu diesem Zweck wir erstellen ein `MKLocalSearch` -Objekt und verwenden, um eine Suche nach Ausgeben einer `MKLocalSearchRequest`, einen Rückruf übergeben wird, um die Ergebnisse abgerufen werden die `Start` Methode der `MKLocalSearch` Objekt. Die Ergebnisse werden dann zurückgegeben, eine `MKLocalSearchResponse` Objekt ein Array von `MKMapItem` Objekte:

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

Klicken Sie auf unserer `MapViewController` wir erstellen eine benutzerdefinierte Implementierung von `UISearchResultsUpdating`, zugewiesen der `SearchResultsUpdater` Eigenschaft unsere `searchController` in der [Hinzufügen einer lokalen Suche Benutzeroberfläche](#Adding_a_Local_Search_UI) Abschnitt:

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

Die oben genannten Implementierung wird eine Anmerkung zur Zuordnung, beim aus den Ergebnissen ein Element ausgewählt ist wie unten dargestellt:

 ![](images/08-search-results.png "Eine Anmerkung zur Zuordnung hinzugefügt werden, wenn ein Element, aus den Ergebnissen ausgewählt ist")
 
> [!IMPORTANT]
> `UISearchController` wurde in iOS 8 implementiert. Wenn Sie älter als diese Geräte unterstützen möchten, müssen Sie verwenden `UISearchDisplayController`.



## <a name="summary"></a>Zusammenfassung

In diesem Artikel untersucht die *Zuordnung* *Kit* Framework für iOS. Erstens es erläutert, wie die `MKMapView` Klasse ermöglicht die interaktive Karten, die in einer Anwendung eingeschlossen werden. Klicken Sie dann zur Demonstration Maps mithilfe der Anmerkungen und für Überlagerungen weiter anpassen. Schließlich untersucht es die lokalen Suchfunktionen, die Zuordnung Kit mit iOS 6.1, hinzugefügt wurden, zur Verwendung Position basierend Abfragen für relevante Punkte ausführen und einer Zuordnung hinzugefügt werden.

## <a name="related-links"></a>Verwandte Links

- [SearchController](https://developer.xamarin.com/recipes/ios/content_controls/search-controller/)
- [MapDemo (Beispiel)](https://developer.xamarin.com/samples/monotouch/MapDemo)
