---
title: Anmerkungen und Überlagerungen in xamarin. IOS
description: Dieser Artikel enthält eine schrittweise exemplarische Vorgehensweise, die zeigt, wie Sie mit der Anmerkung und den Überlagerungs Features von Map Kit arbeiten. Es wird gezeigt, wie einer Anwendung, die eine Anmerkung und Überlagerung am Speicherort der xamarin Evolve 2013-Konferenz anzeigt, eine Zuordnung hinzugefügt wird.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 404483bb0c2c405fb810ebcd3a8007692219f522
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73022000"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>Anmerkungen und Überlagerungen in xamarin. IOS

Die Anwendung, die in dieser exemplarischen Vorgehensweise erstellt wird, ist unten dargestellt:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "An example MapKit app")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)

Den vollständigen Code finden Sie im [Beispiel zu Maps](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)Exemplarische Vorgehensweise.

Erstellen Sie zunächst ein neues **leeres IOS-Projekt**, und geben Sie ihm einen relevanten Namen. Wir beginnen mit dem Hinzufügen von Code zum Ansichts Controller, um MapView anzuzeigen, und erstellen dann neue Klassen für den mapdelegaten und die benutzerdefinierten Anmerkungen. Führenden Sie dazu die folgenden Schritte aus:

## <a name="viewcontroller"></a>ViewController

1. Fügen Sie dem `ViewController`die folgenden Namespaces hinzu:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Fügen Sie der-Klasse eine `MKMapView`-Instanzvariable zusammen mit einer `MapDelegate`-Instanz hinzu. Wir erstellen die `MapDelegate` in Kürze:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. Fügen Sie in der `LoadView`-Methode des Controllers ein `MKMapView` hinzu, und legen Sie es auf die `View`-Eigenschaft des Controllers fest:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Als Nächstes fügen Sie Code hinzu, um die Zuordnung in der viewDidLoad-Methode zu initialisieren.

1. Zeigen Sie in `ViewDidLoad` Hinzufügen von Code zum Festlegen des Karten Typs den Benutzer Speicherort an, und lassen Sie das Zoomen und Schwenken zu:

    ```csharp
    // change map type, show user location and allow zooming and panning
    map.MapType = MKMapType.Standard;
    map.ShowsUserLocation = true;
    map.ZoomEnabled = true;
    map.ScrollEnabled = true;

    ```

1. Fügen Sie als nächstes Code hinzu, um die Karte zu zentrieren und die Region festzulegen:

    ```csharp
    double lat = 30.2652233534254;
    double lon = -97.73815460962083;
    CLLocationCoordinate2D mapCenter = new CLLocationCoordinate2D (lat, lon);
    MKCoordinateRegion mapRegion = MKCoordinateRegion.FromDistance (mapCenter, 100, 100);
    map.CenterCoordinate = mapCenter;
    map.Region = mapRegion;

    ```

1. Erstellen Sie eine neue Instanz von `MapDelegate` und weisen Sie Sie dem `Delegate` des `MKMapView`zu. Auch hier wird der `MapDelegate` in Kürze:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;
    ```

1. Ab IOS 8 sollten Sie die Autorisierung Ihres Benutzers anfordern, um seinen Speicherort zu verwenden. wir fügen das nun unserem Beispiel hinzu. Definieren Sie zunächst eine `CLLocationManager` auf Klassenebene:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. In der `ViewDidLoad`-Methode möchten wir überprüfen, ob das Gerät, auf dem die Anwendung ausgeführt wird, IOS 8 verwendet. wenn dies der Fall ist, wird die Autorisierung angefordert, wenn die APP verwendet wird:

    ```csharp
    if (UIDevice.CurrentDevice.CheckSystemVersion(8,0)){
        locationManager.RequestWhenInUseAuthorization ();
    }
    ```

1. Schließlich müssen wir die Datei " **Info. plist** " Bearbeiten, um Benutzer über den Grund für die Anforderung Ihres Standorts zu informieren. Fügen Sie im **quellmenü** der Datei " **Info. plist**" den folgenden Schlüssel hinzu:

    `NSLocationWhenInUseUsageDescription`

    und Zeichenfolge:

    `Maps Walkthrough Docs Sample`

## <a name="conferenceannotationcs--a-class-for-custom-annotations"></a>ConferenceAnnotation.cs – eine Klasse für benutzerdefinierte Anmerkungen

1. Wir verwenden eine benutzerdefinierte Klasse für die Anmerkung namens "`ConferenceAnnotation`". Fügen Sie dem Projekt die folgende Klasse hinzu:

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

## <a name="viewcontroller---adding-the-annotation-and-overlay"></a>ViewController-hinzufügen der Anmerkung und der Überlagerung

1. Wenn die `ConferenceAnnotation` vorhanden sind, können wir Sie der Karte hinzufügen. Fügen Sie in der `ViewDidLoad`-Methode der `ViewController`die-Anmerkung in der mittelkoordinate der Karte hinzu:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter));
    ```

1. Wir möchten auch über eine Überlagerung des Hotels verfügen. Fügen Sie den folgenden Code hinzu, um die `MKPolygon` mithilfe der Koordinaten für das angegebene Hotel zu erstellen, und fügen Sie Sie dem Map by-Aufruf`AddOverlay`hinzu:

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

Dadurch wird der Code in `ViewDidLoad`abgeschlossen. Nun müssen wir unsere `MapDelegate`-Klasse implementieren, um das Erstellen der Anmerkung und der Überlagerungs Sichten zu verarbeiten.

## <a name="mapdelegate"></a>Mapdelegat

1. Erstellen Sie eine Klasse mit dem Namen `MapDelegate`, die von `MKMapViewDelegate` erbt, und fügen Sie eine `annotationId` Variable ein, die als Verwendungs Bezeichner für die Anmerkung verwendet werden soll:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```

    Wir haben hier nur eine Anmerkung, sodass der wiederzuverwendenden Code nicht unbedingt erforderlich ist, aber es wird empfohlen, ihn einzuschließen.

1. Implementieren Sie die `GetViewForAnnotation`-Methode, um mithilfe des in dieser exemplarischen Vorgehensweise enthaltenen **Konferenz. png** -Bilds eine Ansicht für die `ConferenceAnnotation` zurückzugeben:

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

1. Wenn der Benutzer auf die Anmerkung tippt, soll ein Bild angezeigt werden, das die Stadt Austin anzeigt. Fügen Sie die folgenden Variablen der `MapDelegate` für das Bild und die Ansicht hinzu, um Sie anzuzeigen:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Um das Bild beim Tippen der Anmerkung anzuzeigen, müssen Sie die `DidSelectAnnotation`-Methode wie folgt implementieren:

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

1. Um das Bild auszublenden, wenn der Benutzer die Anmerkung deaktiviert, indem Sie auf eine beliebige andere Stelle auf der Karte tippen, implementieren Sie die `DidSelectAnnotationView`-Methode wie folgt:

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

    Der Code für die Anmerkung ist nun vorhanden. Sie müssen dem `MapDelegate` nur noch Code hinzufügen, um die Ansicht für die Überlagerung des Hotels zu erstellen.

1. Fügen Sie der `MapDelegate`die folgende Implementierung von `GetViewForOverlay` hinzu:

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

Führen Sie die Anwendung aus. Wir verfügen jetzt über eine interaktive Karte mit einer benutzerdefinierten Anmerkung und einem Overlay! Tippen Sie auf die Anmerkung, und das Bild von Austin wird angezeigt, wie unten dargestellt:

 [![](ios-maps-walkthrough-images/01-map-image.png "Tap on the annotation and the image of Austin is displayed")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir uns mit dem Hinzufügen einer Anmerkung zu einer Zuordnung und dem Hinzufügen einer Überlagerung für ein bestimmtes Polygon beschäftigt. Wir haben außerdem gezeigt, wie Sie der Anmerkung die Berührungs Unterstützung hinzufügen, um ein Bild über eine Karte zu animieren.

## <a name="related-links"></a>Verwandte Links

- [Exemplarische Vorgehensweise für Maps](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)
- [Beispiel für eine Karten Demo](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
- [Maps in Xamarin.iOS (Karten in Xamarin.iOS)](~/ios/user-interface/controls/ios-maps/index.md)
