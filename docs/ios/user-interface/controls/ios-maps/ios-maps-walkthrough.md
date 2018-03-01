---
title: "Anmerkungen und für Überlagerungen"
description: "Dieser Artikel stellt eine schrittweise exemplarische Vorgehensweisen für das Arbeiten mit der Anmerkung und Overlay Funktionen der Karte Kit. Es wird gezeigt, wie eine Karte zu einer Anwendung hinzufügen, die eine Anmerkung und Overlay an der Position von der Xamarin entwickeln 2013-Konferenz anzeigt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/21/2017
ms.openlocfilehash: 7eea7231ec4300a368e4612cbed2ba4ebc044a26
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="annotations-and-overlays--walkthrough"></a>Anmerkungen und für Überlagerungen – Exemplarische Vorgehensweise

Die Anwendung, die wir in dieser exemplarischen Vorgehensweise erstellen möchten, wird unten gezeigt:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Eine Beispiel-app MapKit")](ios-maps-walkthrough-images/00-map-overlay.png)
 
Finden Sie in den fertigen Code die **MapsWalkthroughComplete** Ordner innerhalb der [Zuordnung Demobeispiel](https://developer.xamarin.com/samples/monotouch/MapDemo/).

Fangen wir durch Erstellen eines neuen **iOS leeres Projekt**, und es einen entsprechenden Namen. Wir beginnen mit unserer View-Controller die MapView anzuzeigenden Code hinzugefügt, und erstellen anschließend neue Klassen für unsere MapDelegate und die benutzerdefinierte Anmerkungen. Führenden Sie dazu die folgenden Schritte aus:

## <a name="viewcontroller"></a>ViewController


1. Fügen Sie die folgenden Namespaces auf die `ViewController`:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Hinzufügen einer `MKMapView` Instanzvariable auf die Klasse zusammen mit einem `MapDelegate` Instanz. Erstellen wir die `MapDelegate` in Kürze:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. In des Controllers `LoadView` -Methode, Hinzufügen einer `MKMapView` und legen Sie dafür die `View` Eigenschaft des Controllers:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Als Nächstes fügen wir Code zum Initialisieren der Zuordnung in der ' ViewDidLoad'' Methode.

1. In `ViewDidLoad` Code hinzufügen, um legen Sie den Kartentyp, zeigen den Speicherort und Zoomen und Schwenken zulassen:

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

1. Erstellen Sie eine neue Instanz der `MapDelegate` und weisen Sie ihn der `Delegate` von der `MKMapView`. Erneut Implcodeent wir die `MapDelegate` in Kürze:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;     
    ```

1. Seit iOS 8 sollte die angeforderte werden Autorisierung von Ihrer Benutzer zu ihrem Speicherort verwenden, fügen Sie Folgendes wir also in diesem Beispiel. Definieren Sie zuerst eine `CLLocationManager` auf Klassenebene Variablen:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. In der `ViewDidLoad` -Methode, möchten wir prüfen Sie, ob das Gerät, das Ausführen der Anwendung iOS 8 verwendet, und es wird jedoch wir Autorisierung anfordern werden, wenn die app verwendet wird:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
                    locationManager.RequestWhenInUseAuthorization ();
                }
    ```

1. Schließlich müssen so bearbeiten Sie die **"Info.plist"** Datei, die Benutzer über den Grund für die Anforderung von ihrem Speicherort informieren. In der **Quelle** Menü der **"Info.plist"**, fügen Sie den folgenden Schlüssel hinzu:
    
    `NSLocationWhenInUseUsageDescription` 
    
    und Zeichenfolge: 

    `Maps Demo`


## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – eine Klasse für benutzerdefinierte Anmerkungen


1. Verwenden Sie eine benutzerdefinierte Klasse für die Anmerkung wird aufgerufen, wir werden `ConferenceAnnotation`. Fügen Sie dem Projekt die folgende Klasse hinzu:

    ```csharp
    using System;
    using CoreLocation;
    using MapKit;
    
    namespace MapDemo
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

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController - die Anmerkung und Overlay hinzufügen

1. Mit der `ConferenceAnnotation` an Stelle können wir ihn zur Zuordnung hinzufügen. In der `ViewDidLoad` Methode der `ViewController`, auf die Karte Center Koordinate der Anmerkung hinzufügen:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter)); 
    ```

1. Wir möchten auch eine Überlagerung des Hotels haben. Fügen Sie den folgenden Code zum Erstellen der `MKPolygon` unter Verwendung der Koordinaten für die Hotel bereitgestellt, und fügen Sie es zur Zuordnung durch Aufruf `AddOverlay`:

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
Dies schließt den Code in `ViewDidLoad`. Nun wir implementieren müssen unsere `MapDelegate` Klasse, überlagern Sie die Ansichten bzw. und verarbeiten, erstellen die Anmerkung.


## <a name="mapdelegate"></a>MapDelegate

1. Erstellen Sie eine Klasse namens `MapDelegate` von erbt `MKMapViewDelegate` und enthalten eine `annotationId` Variable für die Verwendung als Bezeichner Wiederverwendung für die Anmerkung:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```
    Wir haben nur eine Anmerkung hier daher die Wiederverwendung von Code ist nicht unbedingt erforderlich, aber es empfohlen wird, diesen zu übernehmen.

1. Implementieren der `GetViewForAnnotation` -Methode zur Rückgabe einer Ansicht für die `ConferenceAnnotation` mithilfe der **conference.png** Bild enthalten, die mit dieser exemplarischen Vorgehensweise:

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

1. Beim Tippen auf die Anmerkung, die wir ein Image mit der Stadt Austin anzeigen möchten. Fügen Sie die folgenden Variablen, die `MapDelegate` für das Abbild und die Ansicht, um diese anzuzeigen:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Als Nächstes implementieren Sie zum Anzeigen des Bilds, wenn die Anmerkung abgerufen werden, die `DidSelectAnnotation` Methode wie folgt:

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

1. Um das Bild auszublenden, wenn der Benutzer die Anmerkung hebt durch Tippen auf eine andere auf der Karte Stelle, implementieren die `DidSelectAnnotationView` Methode wie folgt:

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
    Wir haben jetzt den Code für die Anmerkung an. Wird lediglich zum Hinzufügen von Code, um die `MapDelegate` auf die Ansicht für die Überlagerung Hotel zu erstellen.

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

Führen Sie die Anwendung aus. Wir haben jetzt eine interaktive Karten mit einer benutzerdefinierten Anmerkung und Overlay! Tippen Sie auf die Anmerkung, und das Abbild des Austin dargestellt, wie unten dargestellt:

 [![](ios-maps-walkthrough-images/01-map-image.png "Tippen Sie auf die Anmerkung, und das Abbild des Austin wird angezeigt.")](ios-maps-walkthrough-images/01-map-image.png)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel erläutert wir, wie das Hinzufügen einer Anmerkung zu einer Zuordnung als auch zum Hinzufügen einer Überlagerung für einen angegebenen Polygon. Es wird veranschaulicht, wie Touch-Unterstützung für die Anmerkung zum Animieren ein Bild über eine Zuordnung hinzufügen.


## <a name="related-links"></a>Verwandte Links

- [Map-Demo (Beispiel)](https://developer.xamarin.com/samples/monotouch/MapDemo/)
- [iOS-Zuordnungen](~/ios/user-interface/controls/ios-maps/index.md)
