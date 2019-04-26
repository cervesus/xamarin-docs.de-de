---
title: Anmerkungen und Überlagerungen in Xamarin.iOS
description: 'Dieser Artikel enthält eine schrittweise exemplarische Vorgehensweise zeigt, wie in der Anmerkung und Überlagerung von Map Kit funktioniert. Gewusst wie: hinzufügen eine Zuordnung zu einer Anwendung, in dem eine Anmerkung und Überlagerung am Speicherort des der Xamarin-weiterentwickeln 2013-Konferenz angezeigt werden.'
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 445661513b0cf79df99d54ed0bb4b0261dd75c2a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61381485"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>Anmerkungen und Überlagerungen in Xamarin.iOS

Die Anwendung, die wir in dieser exemplarischen Vorgehensweise erstellen wollen, ist unten dargestellt:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Ein Beispiel-app MapKit")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)
 
Sie finden den vollständigen Code in die [Maps Exemplarische Vorgehensweise Beispiel](https://developer.xamarin.com/samples/monotouch/MapsWalkthrough/).

Beginnen wir durch Erstellen eines neuen **iOS leeres Projekt**, und ihm einen entsprechenden Namen. Wir beginnen mit dem Hinzufügen von Code zu unserer View-Controller die MapView angezeigt und erstellt dann neue Klassen für unsere MapDelegate und der benutzerdefinierten Anmerkungen. Führenden Sie dazu die folgenden Schritte aus:

## <a name="viewcontroller"></a>ViewController


1. Fügen Sie die folgenden Namespaces, die `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Hinzufügen einer `MKMapView` Instanzvariable der-Klasse zusammen mit einem `MapDelegate` Instanz. Wir erstellen den `MapDelegate` in Kürze:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. In des Controllers `LoadView` -Methode, Hinzufügen einer `MKMapView` und legen Sie dafür die `View` -Eigenschaft des Controllers:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Als Nächstes fügen wir Code zum Initialisieren der Zuordnung in der ' ViewDidLoad'' Methode.

1. In `ViewDidLoad` Code hinzufügen, um den Map-Typ festgelegt, ermöglichen Zoomen und Schwenken und zeigen den Speicherort der Benutzer:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;
    
    ```

1. Als Nächstes fügen Sie Code aus, um die Karte zentrieren, und legen Sie dessen Region hinzu:

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;
    
    ```

1. Erstellen Sie eine neue Instanz der `MapDelegate` und weisen sie Sie der `Delegate` von der `MKMapView`. In diesem Fall Implcodeent werden wir die `MapDelegate` in Kürze:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. Ab iOS 8 sollten Fragen Sie Autorisierung aus Ihrem Benutzer zu ihrem Standort verwenden, fügen Sie Folgendes wir in unserem Beispiel. Definieren Sie zuerst eine `CLLocationManager` Klassenvariablen:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. In der `ViewDidLoad` -Methode, wir möchten Sie überprüfen, ob das Gerät, das Ausführen der Anwendung von iOS 8 mithilfe wird und ist dies wir Autorisierung anfordern werden, wenn die app verwendet wird:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Abschließend müssen wir bearbeiten die **"Info.plist"** Datei Nichtbeachtung des Grunds für das Anfordern von ihrem Speicherort. In der **Quelle** im Menü der **"Info.plist"**, fügen Sie den folgenden Schlüssel:
    
    `NSLocationWhenInUseUsageDescription` 
    
    und die Zeichenfolge: 

    `Maps Walkthrough Docs Sample`.


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – eine Klasse für den benutzerdefinierten Anmerkungen


1. Wir werden eine benutzerdefinierte Klasse zu verwenden, für die Anmerkung wird aufgerufen, `ConferenceAnnotation`. Fügen Sie dem Projekt die folgende Klasse hinzu:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapsWalkthrough
    {
        public class ConferenceAnnotation : MKAnnotation
        {
            string title;
            CLLocationCoordinate2D coord;
    
            public ConferenceAnnotation (string title,
            CLLocationCoordinate2D coord)
            {
                this.title = title;
                this.coord = coord;
            }
    
            public override string Title {
                get {
                    return title;
                }
            }
    
            public override CLLocationCoordinate2D Coordinate {
                get {
                    return coord;
                }
            }
        }
    }   
    ```

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController: Hinzufügen der-Anmerkung und die Überlagerung

1. Mit der `ConferenceAnnotation` direktes können wir es zur Karte hinzufügen. In der `ViewDidLoad` -Methode der der `ViewController`, fügen Sie die Anmerkung am Center-Koordinate der Karte hinzu:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. Wir möchten auch eine Überlagerung des Hotels. Fügen Sie folgenden Code zum Erstellen der `MKPolygon` verwenden Sie die Koordinaten für das Hotel, bereitgestellt und Hinzufügen der Zuordnung durch Aufruf `AddOverlay`:

    ```csharp
    // add an overlay of the hotel
    MKPolygon hotelOverlay = MKPolygon.FromCoordinates(
        new CLLocationCoordinate2D[]{
        new CLLocationCoordinate2D(30.2649977168594, -97.73863627705),
        new CLLocationCoordinate2D(30.2648461170005, -97.7381627734755),
        new CLLocationCoordinate2D(30.2648355402574, -97.7381750192576),
        new CLLocationCoordinate2D(30.2647791309417, -97.7379872505988),
        new CLLocationCoordinate2D(30.2654525150319, -97.7377341711021),
        new CLLocationCoordinate2D(30.2654807195004, -97.7377994819399),
        new CLLocationCoordinate2D(30.2655089239607, -97.7377994819399),
        new CLLocationCoordinate2D(30.2656428950368, -97.738346460207),
        new CLLocationCoordinate2D(30.2650364981811, -97.7385709662122),
        new CLLocationCoordinate2D(30.2650470749025, -97.7386199493406)
    });
    
    map.AddOverlay (hotelOverlay);  
    ```
Dies schließt den Code in `ViewDidLoad`. Nun wir implementieren müssen unsere `MapDelegate` -Klasse zum Verarbeiten, erstellen die Anmerkung und Ansichten bzw. überlagern.


## <a name="mapdelegate"></a>MapDelegate

1. Erstellen Sie eine Klasse namens `MapDelegate` von erbt `MKMapViewDelegate` und enthalten eine `annotationId` Variable, die für die Anmerkung als Wiederverwendung Bezeichner zu verwenden:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Wir haben nur eine Anmerkung hier also der Wiederverwendung von Code ist nicht zwingend erforderlich, aber es empfiehlt sich, die es enthalten ist.

1. Implementieren der `GetViewForAnnotation` -Methode zur Rückgabe einer Ansicht für die `ConferenceAnnotation` mithilfe der **conference.png** Image in dieser exemplarischen Vorgehensweise enthalten:

    ```csharp
    public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
    {
        MKAnnotationView annotationView = null;
    
        if (annotation is MKUserLocation)
            return null; 
    
        if (annotation is ConferenceAnnotation) {
    
            // show conference annotation
            annotationView = mapView.DequeueReusableAnnotation (annotationId);
    
            if (annotationView == null)
                annotationView = new MKAnnotationView (annotation, annotationId);
        
            annotationView.Image = UIImage.FromFile ("images/conference.png");
            annotationView.CanShowCallout = true;
        } 
    
        return annotationView;
    }
    ```

1. Wenn der Benutzer auf die Anmerkung tippt, möchten wir ein Bild, auf dem die City Austin anzeigen. Fügen Sie die folgenden Variablen auf die `MapDelegate` für das Bild und der Ansicht angezeigt:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Als Nächstes zum Anzeigen des Bilds, wenn die Anmerkung angetippt wird, implementieren die `DidSelectAnnotation` -Methode wie folgt:

    ```csharp
    public override void DidSelectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // show an image view when the conference annotation view is selected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView = new UIImageView ();
            venueView.ContentMode = UIViewContentMode.ScaleAspectFit;
            venueImage = UIImage.FromFile ("image/venue.png");
            venueView.Image = venueImage;
            view.AddSubview (venueView);
    
            UIView.Animate (0.4, () => {
            venueView.Frame = new CGRect (-75, -75, 200, 200); });
        }
    }
    ```

1. Um das Bild auszublenden, wenn der Benutzer die Anmerkung deaktiviert, indem Sie auf der Stelle auf der Karte tippen, implementieren die `DidSelectAnnotationView` -Methode wie folgt:

    ```csharp
    public override void DidDeselectAnnotationView (MKMapView mapView, MKAnnotationView view)
    {
        // remove the image view when the conference annotation is deselected
        if (view.Annotation is ConferenceAnnotation) {
    
            venueView.RemoveFromSuperview ();
            venueView.Dispose ();
            venueView = null;
        }
    }
    ```
    Wir haben jetzt den Code für die Anmerkung vorhanden. Noch ist, Hinzufügen von Code, der `MapDelegate` auf die Ansicht für die Hotel-Überlagerung zu erstellen.

1. Fügen Sie die folgende Implementierung des `GetViewForOverlay` auf die `MapDelegate`:

    ```csharp
    public override MKOverlayView GetViewForOverlay (MKMapView mapView, NSObject overlay)
    {
        // return a view for the polygon
        MKPolygon polygon = overlay as MKPolygon;
        MKPolygonView polygonView = new MKPolygonView (polygon);
        polygonView.FillColor = UIColor.Blue;
        polygonView.StrokeColor = UIColor.Red;
        return polygonView;
    }
    ```

Führen Sie die Anwendung aus. Wir verfügen jetzt über eine interaktive Karte mit einer benutzerdefinierten Anmerkung und einer Überlagerung! Tippen Sie auf die Anmerkung, und das Bild der Austin verwendet wird, wie unten dargestellt angezeigt:

 [![](ios-maps-walkthrough-images/01-map-image.png "Tippen Sie auf die Anmerkung, und das Bild der Austin wird angezeigt.")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert, wie zum Hinzufügen einer Anmerkung zu einer Zuordnung als auch eine Überlagerung für einen angegebenen Polygon hinzufügen. Wir haben außerdem die Multitouch-Unterstützung für die Anmerkung animiert ein Bild auf einer Karte hinzufügen.


## <a name="related-links"></a>Verwandte Links

- [Beispiel für Maps Exemplarische Vorgehensweise](https://developer.xamarin.com/samples/monotouch/MapsWalkthrough/)
- [Demobeispiel zuordnen](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [Maps in Xamarin.iOS (Karten in Xamarin.iOS)](~/ios/user-interface/controls/ios-maps/index.md)
