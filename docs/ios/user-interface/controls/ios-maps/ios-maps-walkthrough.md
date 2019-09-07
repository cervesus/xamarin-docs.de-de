---
title: Anmerkungen und Überlagerungen in xamarin. IOS
description: Dieser Artikel enthält eine schrittweise exemplarische Vorgehensweise, die zeigt, wie Sie mit der Anmerkung und den Überlagerungs Features von Map Kit arbeiten. Es wird gezeigt, wie einer Anwendung, die eine Anmerkung und Überlagerung am Speicherort der xamarin Evolve 2013-Konferenz anzeigt, eine Zuordnung hinzugefügt wird.
ms.prod: xamarin
ms.assetid: 1BC4F7FC-AE3C-46D7-A4D3-18E142F55B8E
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/21/2017
ms.openlocfilehash: 2091e710352b25167b740e409955787ffec99e1c
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768965"
---
# <a name="annotations-and-overlays-in-xamarinios"></a>Anmerkungen und Überlagerungen in xamarin. IOS

Die Anwendung, die in dieser exemplarischen Vorgehensweise erstellt wird, ist unten dargestellt:

 [![](ios-maps-walkthrough-images/00-map-overlay.png "Ein Beispiel für eine MapKit-App")](ios-maps-walkthrough-images/00-map-overlay.png#lightbox)

Den vollständigen Code finden Sie im [Beispiel zu Maps](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)Exemplarische Vorgehensweise.

Erstellen Sie zunächst ein neues **leeres IOS-Projekt**, und geben Sie ihm einen relevanten Namen. Wir beginnen mit dem Hinzufügen von Code zum Ansichts Controller, um MapView anzuzeigen, und erstellen dann neue Klassen für den mapdelegaten und die benutzerdefinierten Anmerkungen. Führenden Sie dazu die folgenden Schritte aus:

## <a name="viewcontroller"></a>ViewController

1. Fügen Sie die folgenden Namespaces `ViewController`hinzu:

    ```csharp
    using MapKit;
    using CoreLocation;
    using UIKit
    using CoreGraphics
    ```

1. Fügen Sie `MKMapView` der-Klasse eine Instanzvariable zusammen mit `MapDelegate` einer-Instanz hinzu. Wir erstellen die `MapDelegate` in Kürze:

    ```csharp
    public partial class ViewController : UIViewController
    {
        MKMapView map;
        MapDelegate mapDelegate;
        ...
    ```

1. Fügen Sie in der `LoadView` -Methode des Controllers `MKMapView` ein hinzu, und legen `View` Sie es auf die-Eigenschaft des Controllers fest:

    ```csharp
    public override void LoadView ()
    {
        map = new MKMapView (UIScreen.MainScreen.Bounds);
        View = map;
    }
    ```

    Als Nächstes fügen Sie Code hinzu, um die Zuordnung in der viewDidLoad-Methode zu initialisieren.

1. Zeigen `ViewDidLoad` Sie unter Code zum Festlegen des Karten Typs hinzufügen den Benutzer Speicherort an, und lassen Sie das Zoomen und Schwenken zu:

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

1. Erstellen Sie eine neue Instanz `MapDelegate` von, und weisen Sie `Delegate` Sie dem `MKMapView`von zu. Auch hier wird der `MapDelegate` Vorgang in Kürze durchführbar:

    ```csharp
    mapDelegate = new MapDelegate ();
    map.Delegate = mapDelegate;
    ```

1. Ab IOS 8 sollten Sie die Autorisierung Ihres Benutzers anfordern, um seinen Speicherort zu verwenden. wir fügen das nun unserem Beispiel hinzu. Definieren Sie zunächst eine `CLLocationManager` Variable auf Klassenebene:

    ```csharp
    CLLocationManager locationManager = new CLLocationManager();
    ```

1. In der `ViewDidLoad` -Methode möchten wir überprüfen, ob das Gerät, auf dem die Anwendung ausgeführt wird, IOS 8 verwendet. wenn es ist, wird die Autorisierung angefordert, wenn die APP verwendet wird:

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

1. Wir verwenden eine benutzerdefinierte Klasse für die Anmerkung, die aufgerufen `ConferenceAnnotation`wird. Fügen Sie dem Projekt die folgende Klasse hinzu:

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

1. Mit der `ConferenceAnnotation` Stelle können wir Sie der Karte hinzufügen. Fügen Sie in `ViewDidLoad` der-Methode `ViewController`von die-Anmerkung in der mittelkoordinate der Karte hinzu:

    ```csharp
    map.AddAnnotations (new ConferenceAnnotation ("Evolve Conference", mapCenter));
    ```

1. Wir möchten auch über eine Überlagerung des Hotels verfügen. Fügen Sie den folgenden Code hinzu, `MKPolygon` um das mithilfe der Koordinaten für das bereitgestellte Hotel zu erstellen, und fügen Sie `AddOverlay`es der Zuordnung nach dem folgenden Befehl hinzu:

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

Dadurch wird der Code in `ViewDidLoad`abgeschlossen. Nun müssen wir unsere `MapDelegate` Klasse implementieren, um das Erstellen der Anmerkung und der Überlagerungs Sichten zu verarbeiten.

## <a name="mapdelegate"></a>MapDelegate

1. Erstellen Sie eine Klasse `MapDelegate` mit dem Namen, `MKMapViewDelegate` die von erbt und eine `annotationId` Variable enthält, die als Verwendungs Bezeichner für die Anmerkung verwendet werden soll:

    ```csharp
    class MapDelegate : MKMapViewDelegate
    {
        static string annotationId = "ConferenceAnnotation";
        ...
    }
    ```

    Wir haben hier nur eine Anmerkung, sodass der wiederzuverwendenden Code nicht unbedingt erforderlich ist, aber es wird empfohlen, ihn einzuschließen.

1. Implementieren `ConferenceAnnotation` Sie `GetViewForAnnotation` die-Methode, um mithilfe des in dieser exemplarischen Vorgehensweise enthaltenen " **Conference. png** "-Bilds eine Ansicht für zurückzugeben

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

1. Wenn der Benutzer auf die Anmerkung tippt, soll ein Bild angezeigt werden, das die Stadt Austin anzeigt. Fügen Sie die folgenden Variablen `MapDelegate` für das Bild und die Ansicht hinzu, um es anzuzeigen:

    ```csharp
    UIImageView venueView;
    UIImage venueImage;
    ```

1. Um das Bild anzuzeigen, wenn die Anmerkung getippt wird, implementieren Sie die `DidSelectAnnotation` -Methode wie folgt:

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

1. Um das Bild auszublenden, wenn der Benutzer die Anmerkung durch Tippen auf eine andere Stelle auf der Karte deaktiviert, `DidSelectAnnotationView` implementieren Sie die Methode wie folgt:

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

    Der Code für die Anmerkung ist nun vorhanden. Zum Erstellen der Ansicht für die `MapDelegate` Überlagerung des Hotels müssen Sie nur noch Code hinzufügen.

1. Fügen Sie die folgende Implementierung `GetViewForOverlay` von `MapDelegate`hinzu:

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

 [![](ios-maps-walkthrough-images/01-map-image.png "Tippen Sie auf die Anmerkung, und das Bild von Austin wird angezeigt.")](ios-maps-walkthrough-images/01-map-image.png#lightbox)

## <a name="summary"></a>Zusammenfassung

In diesem Artikel haben wir uns mit dem Hinzufügen einer Anmerkung zu einer Zuordnung und dem Hinzufügen einer Überlagerung für ein bestimmtes Polygon beschäftigt. Wir haben außerdem gezeigt, wie Sie der Anmerkung die Berührungs Unterstützung hinzufügen, um ein Bild über eine Karte zu animieren.

## <a name="related-links"></a>Verwandte Links

- [Exemplarische Vorgehensweise für Maps](https://docs.microsoft.com/samples/xamarin/ios-samples/mapswalkthrough)
- [Beispiel für eine Karten Demo](https://docs.microsoft.com/samples/xamarin/ios-samples/mapdemo)
- [Maps in Xamarin.iOS (Karten in Xamarin.iOS)](~/ios/user-interface/controls/ios-maps/index.md)
