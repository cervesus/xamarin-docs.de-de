---
title: Neue Funktionen in MapKit auf iOS 11
description: 'Dieses Dokument beschreibt die neuen MapKit-Funktionen in iOS 11: Marker, die Schaltfläche "Compass" der Skala anzeigen und die Benutzer-Schaltfläche "Überwachung" gruppieren.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: f73078a2dcbaeefeb5608ce7ec1e2c12b261acad
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34787405"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Neue Funktionen in MapKit auf iOS 11

iOS 11 hinzugefügt MapKit die folgenden neuen Funktionen:

- [Clustering-Anmerkung](#clustering)
- [Kompass Schaltfläche](#compass)
- [Skala anzeigen](#scale)
- [Schaltfläche der Benutzer-Überwachung](#user-tracking)

![Zuordnung mit einem Beispiel gruppierten Marker und Kompass Schaltfläche](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Automatisch Gruppierung Marker beim Vergrößern/Verkleinern

Im Beispiel [MapKit-Beispiel "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) wird gezeigt, wie die neue iOS 11-Anmerkung, die Failoverclustering-Feature zu implementieren.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Erstellen einer `MKPointAnnotation` Unterklasse

Der Punkt Annotation-Klasse stellt einzelnen Marker auf der Karte dar. Einzeln mit hinzugefügt werden können `MapView.AddAnnotation()` oder über ein Array mit `MapView.AddAnnotations()`.

Point-Anmerkung-Klassen verfügen nicht über eine visuelle Darstellung, sie sind nur erforderlich, die dem Marker zugeordneten Daten darzustellen (am wichtigsten ist, die `Coordinate` -Eigenschaft die Längen- und Breitenkoordinaten auf der Karte ist), und benutzerdefinierte Eigenschaften:

```csharp
public class Bike : MKPointAnnotation
{
  public BikeType Type { get; set; } = BikeType.Tricycle;
  public Bike(){}
  public Bike(NSNumber lat, NSNumber lgn, NSNumber type)
  {
    Coordinate = new CLLocationCoordinate2D(lat.NFloatValue, lgn.NFloatValue);
    switch(type.NUIntValue) {
      case 0:
        Type = BikeType.Unicycle;
        break;
      case 1:
        Type = BikeType.Tricycle;
        break;
    }
  }
}
```

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Erstellen Sie eine `MKMarkerAnnotationView` -Unterklasse für einzelne Marker

Die Markierung Anmerkung Ansicht die visuelle Darstellung der einzelnen Anmerkung, und wird mithilfe von Eigenschaften wie z. B. formatiert:

- **MarkerTintColor** – die Farbe für den Marker fest.
- **GlyphText** – Text in die Markierung angezeigt.
- **GlyphImage** – legt das Bild, das in den Marker angezeigt wird.
- **DisplayPriority** – bestimmt die Z-Reihenfolge (Stapeln Verhalten) Wenn die Zuordnung mit Datenpunkten überfüllt ist. Verwenden Sie eine der `Required`, `DefaultHigh`, oder `DefaultLow`.

Um automatische clustering zu unterstützen, müssen Sie auch Folgendes festlegen:

- **ClusteringIdentifier** – Dies steuert, welche Marker gruppiert abrufen. Sie können verwenden den gleichen Bezeichner für alle Ihre Marker oder Google verwenden verschiedene Bezeichner um zu steuern, wie sie gruppiert sind.

```csharp
[Register("BikeView")]
public class BikeView : MKMarkerAnnotationView
{
  public static UIColor UnicycleColor = UIColor.FromRGB(254, 122, 36);
  public static UIColor TricycleColor = UIColor.FromRGB(153, 180, 44);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;

      var bike = value as Bike;
      if (bike != null){
        ClusteringIdentifier = "bike";
        switch(bike.Type){
          case BikeType.Unicycle:
            MarkerTintColor = UnicycleColor;
            GlyphImage = UIImage.FromBundle("Unicycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultLow;
            break;
          case BikeType.Tricycle:
            MarkerTintColor = TricycleColor;
            GlyphImage = UIImage.FromBundle("Tricycle");
            DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
            break;
        }
      }
    }
  }
```

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Erstellen einer `MKAnnotationView` zur Darstellung von Clustern von Datenpunkten

Die Anmerkung-Sicht, die einen Cluster von markern darstellt, während _konnte_ ein einfaches Abbilds zu sein, Benutzer erwarten, dass die app bereitstellen optische Hinweise zur wie viele Datenpunkte gruppiert wurden.

Die [Beispielcode](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) CoreGraphics verwendet, um die Anzahl von Datenpunkten in dem Cluster sowie ein Kreis-grafikdarstellung der Anteil der einzelnen Markertyp zu rendern.

Sie sollten auch Folgendes festlegen:

- **DisplayPriority** – bestimmt die Z-Reihenfolge (Stapeln Verhalten) Wenn die Zuordnung mit Datenpunkten überfüllt ist. Verwenden Sie eine der `Required`, `DefaultHigh`, oder `DefaultLow`.
- **CollisionMode** – `Circle` oder `Rectangle`.

```csharp
[Register("ClusterView")]
public class ClusterView : MKAnnotationView
{
  public static UIColor ClusterColor = UIColor.FromRGB(202, 150, 38);
  public override IMKAnnotation Annotation
  {
    get {
      return base.Annotation;
    }
    set {
      base.Annotation = value;
      var cluster = MKAnnotationWrapperExtensions.UnwrapClusterAnnotation(value);
      if (cluster != null)
      {
        var renderer = new UIGraphicsImageRenderer(new CGSize(40, 40));
        var count = cluster.MemberAnnotations.Length;
        var unicycleCount = CountBikeType(cluster.MemberAnnotations, BikeType.Unicycle);

        Image = renderer.CreateImage((context) => {
          // Fill full circle with tricycle color
          BikeView.TricycleColor.SetFill();
          UIBezierPath.FromOval(new CGRect(0, 0, 40, 40)).Fill();
          // Fill pie with unicycle color
          BikeView.UnicycleColor.SetFill();
          var piePath = new UIBezierPath();
          piePath.AddArc(new CGPoint(20,20), 20, 0, (nfloat)(Math.PI * 2.0 * unicycleCount / count), true);
          piePath.AddLineTo(new CGPoint(20, 20));
          piePath.ClosePath();
          piePath.Fill();
          // Fill inner circle with white color
          UIColor.White.SetFill();
          UIBezierPath.FromOval(new CGRect(8, 8, 24, 24)).Fill();
          // Finally draw count text vertically and horizontally centered
          var attributes = new UIStringAttributes() {
            ForegroundColor = UIColor.Black,
            Font = UIFont.BoldSystemFontOfSize(20)
          };
          var text = new NSString($"{count}");
          var size = text.GetSizeUsingAttributes(attributes);
          var rect = new CGRect(20 - size.Width / 2, 20 - size.Height / 2, size.Width, size.Height);
          text.DrawString(rect, attributes);
        });
      }
    }
  }
  public ClusterView(){}
  public ClusterView(MKAnnotation annotation, string reuseIdentifier) : base(annotation, reuseIdentifier)
  {
    DisplayPriority = MKFeatureDisplayPriority.DefaultHigh;
    CollisionMode = MKAnnotationViewCollisionMode.Circle;
    // Offset center point to animate better with marker annotations
    CenterOffset = new CoreGraphics.CGPoint(0, -10);
  }
  private nuint CountBikeType(IMKAnnotation[] members, BikeType type) {
    nuint count = 0;
    foreach(Bike member in members){
      if (member.Type == type) ++count;
    }
    return count;
  }
}
```

### <a name="4-register-the-view-classes"></a>4. Registrieren der Ansichtsklassen

Wenn das Kartensteuerelement für die Sicht erstellt wird, und eine Ansicht hinzugefügt werden, registrieren Sie die Anmerkung Ansichtstypen, um das automatische clustering Verhalten zu aktivieren, wie die Karte ein-und vergrößert wird:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Die Zuordnung zu rendern.

Beim Rendern der Zuordnung werden Anmerkung Marker gruppiert oder abhängig von der Zoomstufe gerendert. Wie ändert sich die Zoomstufe, animieren Marker und Clustern.

![Simulator gruppierten Marker auf der Karte angezeigt](mapkit-images/cyclemap-sml.png)

Finden Sie in der [ordnet Abschnitt](~/ios/user-interface/controls/ios-maps/index.md) für Weitere Informationen zum Anzeigen von Daten mit MapKit.

<a name="compass" />

## <a name="compass-button"></a>Kompass Schaltfläche

iOS 11 fügt die Möglichkeit, holen Kompass aus der Zuordnung, und es an anderer Stelle in der Ansicht zu rendern. Finden Sie unter der [Tandm Beispiel-app](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) ein Beispiel.

Erstellen Sie eine Schaltfläche, die sieht wie ein Kompass (einschließlich live Animation aus, wenn die Ausrichtung der Zuordnung geändert wird), und es auf ein anderes Steuerelement rendert.

![Schaltfläche "Compass" auf der Navigationsleiste](mapkit-images/compass-sml.png)

Der folgende Code erstellt eine Compass-Schaltfläche und rendert ihn auf der Navigationsleiste:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

Die `ShowsCompass` Eigenschaft kann verwendet werden, um die Sichtbarkeit von der Standardeinstellung Kompass innerhalb der Kartenansicht zu steuern.

<a name="scale" />

## <a name="scale-view"></a>Skala anzeigen

Fügen Sie die Skala an anderer Stelle in der Ansicht mit der `MKScaleView.FromMapView()` Methode zum Abrufen einer Instanz der Skalierungsansicht, die an anderer Stelle in der Ansichtshierarchie hinzufügen.

![Skalierungsansicht Überlagerung auf einer Karte](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

Die `ShowsScale` Eigenschaft kann verwendet werden, um die Sichtbarkeit von der Standardeinstellung Kompass innerhalb der Kartenansicht zu steuern.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Schaltfläche der Benutzer-Überwachung

Die Schaltfläche "Benutzer Tracking" Zentriert die Karte auf den aktuellen Ort des Benutzers. Verwenden der `MKUserTrackingButton.FromMapView()` Methode zum Abrufen einer Instanz der Schaltfläche, Formatierungsänderungen anwenden und an anderer Stelle in der Ansichtshierarchie hinzufügen.

![Schaltfläche der Benutzer-Speicherort auf einer Karte Überlagerung](mapkit-images/user-location-sml.png)

```csharp
var button = MKUserTrackingButton.FromMapView(MapView);
button.Layer.BackgroundColor = UIColor.FromRGBA(255,255,255,80).CGColor;
button.Layer.BorderColor = UIColor.White.CGColor;
button.Layer.BorderWidth = 1;
button.Layer.CornerRadius = 5;
button.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(button); // constraints omitted for simplicity
```


## <a name="related-links"></a>Verwandte Links

- [MapKit-Beispiel "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Neuigkeiten in der MapKit (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/237/)
