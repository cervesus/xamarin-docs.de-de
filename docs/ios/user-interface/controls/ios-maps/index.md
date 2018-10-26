---
title: Karten in Xamarin.iOS
description: Dieses Dokument beschreibt das iOS-MapKit-Framework und wie er mit Xamarin.iOS verwendet wird. Es wird erläutert, wie eine Karte, Stil, schwenken und zoomen, arbeiten mit Benutzerstandort, Hinzufügen von Anmerkungen, zusammenarbeiten, Legenden und für Überlagerungen und lokale Suche hinzufügen.
ms.prod: xamarin
ms.assetid: 5DD8E56D-51C1-4AFA-B387-79B5734698ED
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: d83470db23b1376d18fa36c52c1daabaf68cfe0b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50117758"
---
# <a name="maps-in-xamarinios"></a>Karten in Xamarin.iOS

Zuordnungen sind ein Feature in allen modernen mobilen Betriebssystemen. iOS bietet Unterstützung systemintern über das Map-Kit-Framework. Map Kit können Anwendungen problemlos interaktive Karten hinzufügen. Diese Zuordnungen können in einer Vielzahl von Möglichkeiten, wie etwa das Hinzufügen von Anmerkungen, um die Speicherorte auf einer Karte zu markieren und Überlagern von Grafiken von willkürlichen Formen angepasst werden. Map Kit verfügt auch über integrierte Unterstützung für die aktuelle Position eines Geräts angezeigt.

## <a name="adding-a-map"></a>Hinzufügen einer Karte

Hinzufügen einer Zuordnung zu einer Anwendung erfolgt durch Hinzufügen einer `MKMapView` Instanz in der Hierarchie anzeigen, wie unten dargestellt:

```csharp
// map is an MKMapView declared as a class variable
map = new MKMapView (UIScreen.MainScreen.Bounds);
View = map;
```

 `MKMapView` ist eine `UIView` Unterklasse, die eine Zuordnung anzeigt. Hinzufügen der Zuordnung mit dem obigen Code wird eine interaktive Karte:

 ![](images/00-map.png "Eine Beispiel-Karte")

## <a name="map-style"></a>Map-Stil

 `MKMapView` 3 verschiedenen Formate von Zuordnungen unterstützt werden. Wenn ein Format anwenden möchten, legen Sie einfach die `MapType` Eigenschaft auf einen Wert aus der `MKMapType` Enumeration:
 ```
map.MapType = MKMapType.Standard; //road map
map.MapType = MKMapType.Satellite;
map.MapType = MKMapType.Hybrid;
 ```
  Der folgende Screenshot zeigt es sich um andere Zuordnung Stile an, die verfügbar sind:

 ![](images/01-mapstyles.png "Dieser Screenshot zeigt die unterschiedlichen Zuordnung Stile an, die verfügbar sind")

## <a name="panning-and-zooming"></a>Schwenken und Zoomen

 `MKMapView` Unterstützung für die Zuordnung Interaktivitätsfunktionen enthält wie z. B.:

-  Über eine zusammendrückbewegung Zoomen
-  Über eine Geste für ein Pan Schwenken


Diese Features können aktiviert oder deaktiviert, indem einfach die `ZoomEnabled` und `ScrollEnabled` Eigenschaften der `MKMapView` Instanz, in dem der Standardwert für beide "true" ist. Beispielsweise um einer statischen Karte anzuzeigen, legen Sie einfach die entsprechenden Eigenschaften auf "false":

```csharp
map.ZoomEnabled = false;
map.ScrollEnabled = false;
```

## <a name="user-location"></a>Benutzerstandort

Zusätzlich zur Benutzerinteraktion `MKMapView` verfügt auch über integrierte Unterstützung für den Standort des Geräts angezeigt. Dies geschieht mithilfe der *zentralen Standort* Framework. Bevor Sie den Standort des Benutzers zugreifen können, müssen Sie den Benutzer auffordern. Zu diesem Zweck erstellen Sie eine Instanz von `CLLocationManager` , und rufen Sie `RequestWhenInUseAuthorization`.

```csharp
CLLocationManager locationManager = new CLLocationManager();
locationManager.RequestWhenInUseAuthorization();
//locationManager.RequestAlwaysAuthorization(); //requests permission for access to location data while running in the background
```

Beachten Sie, dass in Versionen von iOS 8.0, es wird versucht, rufen Sie vor der `RequestWhenInUseAuthorization` führt zu einem Fehler. Stellen Sie sicher, überprüfen Sie die iOS-Version vor dieser Aufruf verwenden, wenn Sie beabsichtigen, 8-Versionen unterstützt.

Erfordert den Zugriff auf den Standort des Benutzers auch Änderungen an **"Info.plist"**. Die folgenden Schlüssel, die mit den Standortdaten zusammenhängen, sollten festgelegt werden:

- **NSLocationWhenInUseUsageDescription**: Für den Zugriff auf den Standort des Benutzers während dieser mit Ihrer App interagiert.
- **NSLocationAlwaysUsageDescription**: Für den Zugriff Ihrer App auf den Standort des Benutzers aus dem Hintergrund.

Sie können diese Schlüssel hinzufügen, indem Sie Sie öffnen **"Info.plist"** und *Quelle* am unteren Rand der Editor.

Nachdem Sie aktualisiert haben **"Info.plist"** aufgefordert, den Benutzer für die Berechtigung zum Zugriff auf deren Speicherort, und Sie können den Standort des Benutzers auf der Karte anzeigen, indem Sie die Einstellung der `ShowsUserLocation` Eigenschaft auf "true":

```csharp
map.ShowsUserLocation = true;
```

 ![](images/02-location-alert.png "Warnung Zugriff Standort zulassen")
 
## <a name="annotations"></a>Anmerkungen

 `MKMapView` unterstützt auch das Anzeigen von Bildern, Anmerkungen, die auf einer Karte genannt. Dies können entweder benutzerdefinierte Images oder systemdefinierte Pins verschiedener Farben sein. Der folgende Screenshot zeigt beispielsweise eine Karte mit einem sowohl eine Pin und einem benutzerdefinierten Image:

 ![](images/03-annotations.png "Dieser Screenshot zeigt eine Zuordnung zu einem sowohl eine Pin und einem benutzerdefinierten Image")

### <a name="adding-an-annotation"></a>Hinzufügen einer Anmerkung

Eine Anmerkung selbst besteht aus zwei Teilen:

-  Die `MKAnnotation` -Objekt, das Daten des Modells über die Anmerkung, z. B. den Titel und die Position der Anmerkung enthält.
-  Die `MKAnnotationView` , enthält das Image für die Anzeige und optional eine Legende, die angezeigt wird, wenn der Benutzer auf die Anmerkung tippt.


Map Kit verwendet das iOS-delegationsmuster zum Hinzufügen von Anmerkungen zu einer Zuordnung, in denen die `Delegate` Eigenschaft der `MKMapView` festgelegt ist, mit einer Instanz von einer `MKMapViewDelegate`. Es handelt sich des Delegaten-Implementierung, die verantwortlich für die Rückgabe ist die `MKAnnotationView` für eine Anmerkung.

Um eine Anmerkung hinzuzufügen, zuerst die-Anmerkung wird hinzugefügt, durch den Aufruf `AddAnnotations` auf die `MKMapView` Instanz:

```csharp
// add an annotation
map.AddAnnotations (new MKPointAnnotation (){
    Title="MyAnnotation",
    Coordinate = new CLLocationCoordinate2D (42.364260, -71.120824)
});
```

Bei der die Position der Anmerkung sichtbar ist, in der Zuordnung wird die `MKMapView` wird Aufrufen des Delegaten `GetViewForAnnotation` -Methode zum Abrufen der `MKAnnotationView` angezeigt.

Der folgende Code gibt z. B. eine vom System bereitgestellte `MKPinAnnotationView`:

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

Um Arbeitsspeicher zu sparen `MKMapView` können Anmerkung anzeigen, für die Wiederverwendung, ähnlich wie die in einem Pool zusammengefasst werden Zellen der Tabelle werden wiederverwendet. Eine Anmerkung-Ansicht aus dem Pool abrufen erfolgt durch einen Aufruf von `DequeueReusableAnnotation`:

```csharp
MKAnnotationView pinView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);
```

#### <a name="showing-callouts"></a>Anzeigen von Legenden

Wie bereits erwähnt, kann eine Anmerkung optional eine Legende angezeigt. Legen Sie eine Legende einfach anzuzeigenden `CanShowCallout` auf "true", auf die `MKAnnotationView`. Dadurch wird in der Anmerkung Titel angezeigt wird, wenn die Anmerkung angetippt wird, wie gezeigt:

 ![](images/04-callout.png "Die Anmerkungen des Titels angezeigt wird")

### <a name="customizing-the-callout"></a>Anpassen der Legende

Die Legende kann auch angepasst werden, um Ansichten für Links und rechts Zubehör, zeigen, wie unten dargestellt:

```csharp
pinView.RightCalloutAccessoryView = UIButton.FromType (UIButtonType.DetailDisclosure);
pinView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile ("monkey.png"));
```

Dieser Code führt in der folgenden Beschriftung:

 ![](images/05-callout-accessories.png "Eine Legende mit einem Beispiel")

Um die richtige Zugriffsmethode tippen des Benutzers zu behandeln, implementieren Sie einfach die `CalloutAccessoryControlTapped` -Methode in der die `MKMapViewDelegate`:

```csharp
public override void CalloutAccessoryControlTapped (MKMapView mapView, MKAnnotationView view, UIControl control)
{
    ...
}
```

### <a name="overlays"></a>Überlagerungen

Eine weitere Möglichkeit zum Layer-Grafiken auf einer Karte verwendet überlagert. Überlagerungen unterstützen grafischen Zeichnungsinhalt, die mit der Zuordnung skaliert werden soll, wie es vergrößert wird. iOS bietet Unterstützung für mehrere Typen von Überlagerungen, einschließlich:

-  Polygone - wird häufig verwendet, um eine Region auf einer Karte zu markieren.
-  Polylinien - häufig angezeigt wird, wenn eine Route angezeigt.
-  Kreise - zum Markieren einer runden Fläche einer Zuordnung.


Darüber hinaus können benutzerdefinierte Overlays erstellt werden, um beliebige Geometrien mit präzisen, benutzerdefinierten Zeichnungscode anzuzeigen. Beispielsweise wäre Wetter-Netz ein guter Kandidat für eine benutzerdefinierte Überlagerung.

#### <a name="adding-an-overlay"></a>Hinzufügen einer Überlagerung

Ähnlich wie bei Anmerkungen Hinzufügen einer Überlagerung umfasst 2 Teile:

-  Erstellen ein Modellobjekt für die Überlagerung und zum Hinzufügen, die `MKMapView` .
-  Erstellen einer Ansicht für den Vordergrund in der `MKMapViewDelegate` .


Das Modell für die Überlagerung möglich `MKShape` Unterklasse. Xamarin.iOS enthält `MKShape` Unterklassen für Polygone, Polylinien und Kreise, über die `MKPolygon`, `MKPolyline` und `MKCircle` bzw. Klassen.

Z. B. der folgende Code wird zum Hinzufügen einer `MKCircle`:

```csharp
var circleOverlay = MKCircle.Circle (mapCenter, 1000);
map.AddOverlay (circleOverlay);
```

Die Ansicht für eine Überlagerung ist ein `MKOverlayView` -Instanz, die von zurückgegeben wird die `GetViewForOverlay` in die `MKMapViewDelegate`. Jede `MKShape` verfügt über eine entsprechende `MKOverlayView` , die weiß, wie die angegebene Form anzuzeigen. Für `MKPolygon` besteht `MKPolygonView`. Auf ähnliche Weise `MKPolyline` entspricht `MKPolylineView`, und für `MKCircle` besteht `MKCircleView`.

Im folgenden Codebeispiel gibt ein `MKCircleView` für eine `MKCircle`:

```csharp
public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
{
    var circleOverlay = overlay as MKCircle;
    var circleView = new MKCircleView (circleOverlay);
    circleView.FillColor = UIColor.Blue;
    return circleView;
}
```

Dies zeigt einen Kreis auf der Karte, wie dargestellt:

 ![](images/06-circle-overlay.png "Ein Kreis, der auf der Karte angezeigt")

## <a name="local-search"></a>Lokale Suche

iOS enthält eine lokale Suche-API mit Map-Kit, das asynchrone Suchen nach Punkten Interesse an einer angegebenen geografischen Region möglich.

Um eine lokale Suche durchzuführen, muss eine Anwendung die folgenden Schritte ausführen:

1.  Erstellen Sie `MKLocalSearchRequest` Objekt.
1.  Erstellen Sie eine `MKLocalSearch` -Objekt aus der `MKLocalSearchRequest` .
1.  Rufen Sie die `Start` Methode für die `MKLocalSearch` Objekt.
1.  Abrufen der `MKLocalSearchResponse` Objekt in einem Rückruf.


Die lokale Suche-API selbst stellt keine Benutzeroberfläche bereit. Es erfordert keine auch eine Zuordnung verwendet werden. Um die praktische Verwendung von lokalen Search vornehmen zu können, muss eine Anwendung eine Methode zum Angeben einer Abfrage und die Ergebnisse werden angezeigt. Darüber hinaus, da die Ergebnisse Daten enthält, wird es häufig damit diese auf einer Karte angezeigt sinnvoll.

<a name="Adding_a_Local_Search_UI"/>

### <a name="adding-a-local-search-ui"></a>Hinzufügen einer lokalen Suche Benutzeroberfläche

Eine Möglichkeit zur Suche Eingabe akzeptieren wird mit einer `UISearchBar`, die im Lieferumfang von einer `UISearchController` und zeigt Ergebnisse in einer Tabelle.

Der folgende Code fügt die `UISearchController` (diese hat es sich um einer sucheigenschaftenliste eine Leiste) in der `ViewDidLoad` -Methode der `MapViewController`:

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

### <a name="updating-the-search-results"></a>Aktualisieren die Ergebnisse der Suche

Die `SearchResultsUpdater` fungiert als Vermittler zwischen der `searchController`der Suchleiste und den Suchergebnissen angezeigt. 

In diesem Beispiel müssen wir zuerst erstellen Sie die Search-Methode in der `SearchResultsViewController`. Zu diesem Zweck wir erstellen eine `MKLocalSearch` Objekt aus, und verwenden, um eine Suche nach Ausgeben einer `MKLocalSearchRequest`, einen Rückruf übergeben wird, um die Ergebnisse abgerufen werden die `Start` -Methode der der `MKLocalSearch` Objekt. Die Ergebnisse werden dann zurückgegeben, eine `MKLocalSearchResponse` Objekt mit einem Array von `MKMapItem` Objekte:

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

Klicken Sie auf unserer `MapViewController` wir erstellen eine benutzerdefinierte Implementierung der `UISearchResultsUpdating`, zugewiesen die `SearchResultsUpdater` Eigenschaft unserer `searchController` in der [Hinzufügen einer lokalen Suchbenutzeroberfläche](#Adding_a_Local_Search_UI) Abschnitt:

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

Die Implementierung oben hinzugefügt eine Anmerkung zur Zuordnung, wenn ein Element, in den Ergebnissen ausgewählt ist wie unten dargestellt:

 ![](images/08-search-results.png "Eine Anmerkung zur Karte hinzugefügt wird, wenn ein Element, aus den Ergebnissen ausgewählt ist")
 
> [!IMPORTANT]
> `UISearchController` IOS 8 wurde implementiert werden. Wenn Sie Geräte vor dies unterstützen möchten, müssen Sie verwenden `UISearchDisplayController`.



## <a name="summary"></a>Zusammenfassung

In diesem Artikel untersucht die *Zuordnung* *Kit* Framework für iOS. Zunächst er erläutert, wie die `MKMapView` Klasse ermöglicht es interaktive Zuordnungen, die in einer Anwendung enthalten sein. Klicken Sie dann es wurde veranschaulicht, wie Sie Zuordnungen mit Anmerkungen und Überlagerungen weiter anpassen. Schließlich wurde die lokale Suchfunktionen, die Map Kit mit iOS 6.1, hinzugefügt wurden, zur Verwendung positionsbasierten Abfragen nach interessanten Punkten ausführen und eine Karte hinzufügen.

## <a name="related-links"></a>Verwandte Links

- [SearchController](https://github.com/xamarin/recipes/tree/master/Recipes/ios/content_controls/search-controller)
- [MapDemo (Beispiel)](https://developer.xamarin.com/samples/monotouch/MapDemo)
