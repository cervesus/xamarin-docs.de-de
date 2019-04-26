---
title: Neue Features in MapKit auf iOS 11
description: 'Dieses Dokument beschreibt die neuen MapKit-Features in iOS 11: Gruppieren von markern, die Schaltfläche "Compass", der Skalierungsansicht und die Schaltfläche "Benutzer-nachverfolgung".'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 08/30/2017
ms.openlocfilehash: 3c1b29a4aba03ffe8a3131625ef29cf64766bb6c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61342351"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Neue Features in MapKit auf iOS 11

iOS 11 hinzugefügt MapKit die folgenden neuen Features:

- [Anmerkung Clustering](#clustering)
- [Schaltfläche "Compass"](#compass)
- [Skalierungsansicht](#scale)
- [Schaltfläche "Benutzer-Überwachung"](#user-tracking)

![Eine Zuordnung, die gruppierten Marker anzeigt und Compass-Schaltfläche](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Automatisch Gruppierung Marker beim Zoomen

Das Beispiel [MapKit-Beispiel "Tandm"](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) zeigt, wie Sie die neue iOS 11-Anmerkung, die Failoverclustering-Features zu implementieren.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Erstellen Sie eine `MKPointAnnotation` Unterklasse

Der Punkt Annotation-Klasse stellt die Marker auf der Karte dar. Sie können einzeln mit hinzugefügt werden `MapView.AddAnnotation()` oder ein Array mit `MapView.AddAnnotations()`.

Point-Anmerkung-Klassen müssen sich nicht auf eine visuelle Darstellung, sie werden nur benötigt, um die Darstellung der Daten, die dem Marker zugeordnet (vor allem die `Coordinate` Eigenschaft, die die Breiten- und Längengrad auf der Karte ist), und benutzerdefinierte Eigenschaften:

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Erstellen Sie eine `MKMarkerAnnotationView` -Unterklasse für die einzelnen Markierungen

Die Markierung Anmerkung Ansicht die visuelle Darstellung der jede Anmerkung, und ist ein Format, z. B. mithilfe von Eigenschaften:

- **MarkerTintColor** – die Farbe für den Marker.
- **GlyphText** – Text in der Marker angezeigt.
- **GlyphImage** : Legt das Bild, das in der Marker angezeigt wird.
- **DisplayPriority** – bestimmt die Z-Reihenfolge (Stapeln Verhalten) Wenn die Zuordnung mit markern überfüllt ist. Verwenden Sie eine der `Required`, `DefaultHigh`, oder `DefaultLow`.

Um automatische clustering zu unterstützen, müssen Sie auch Folgendes festlegen:

- **ClusteringIdentifier** – Dies steuert, welche Markierungen zusammen gruppiert. Sie können den gleichen Bezeichner für alle Ihre Marker verwenden oder verwenden verschiedene Bezeichner zur steuern, wie sie zusammen gruppiert werden.

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

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Erstellen Sie eine `MKAnnotationView` zur Darstellung der Cluster von markern

Während die Anmerkung-Ansicht, die einen Cluster von markern darstellt _konnte_ können Sie ein einfaches Image, Benutzer erwarten, dass die app zu optische Hinweise zur wie viele Marker gruppiert wurden.

Die [Beispielcode](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) CoreGraphics verwendet, um die Anzahl von Markierungen in den Cluster als auch einen Circle-grafikdarstellung den Anteil von jedem Markertyp zu rendern.

Sie sollen ebenfalls festlegen:

- **DisplayPriority** – bestimmt die Z-Reihenfolge (Stapeln Verhalten) Wenn die Zuordnung mit markern überfüllt ist. Verwenden Sie eine der `Required`, `DefaultHigh`, oder `DefaultLow`.
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

### <a name="4-register-the-view-classes"></a>4. Registrieren der Klassen anzeigen

Wenn die Map-Steuerelement erstellt wird, und eine Sicht hinzugefügt wurde, registrieren Sie die Ansichtstypen Anmerkung, um das automatische clustering Verhalten zu aktivieren, wie die Karte ein-und vergrößert wird:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Die Karte zu rendern.

Wenn die Karte gerendert wird, werden anmerkungsmarker gruppiert oder abhängig vom Zoomfaktor gerendert werden. Wenn der Zoomfaktor geändert wird, animieren Marker und Cluster an.

![Gruppierte Marker auf der Karte mit Simulator](mapkit-images/cyclemap-sml.png)

Finden Sie in der [ordnet Abschnitt](~/ios/user-interface/controls/ios-maps/index.md) Weitere Informationen zum Anzeigen von Daten mit MapKit.

<a name="compass" />

## <a name="compass-button"></a>Schaltfläche "Compass"

iOS 11 fügt die Möglichkeit, der Kompass verkleinern Sie die Übersicht angezeigt, und es an anderer Stelle in der Ansicht zu rendern. Finden Sie unter den [Tandm-Beispiel-app](https://developer.xamarin.com/samples/monotouch/ios11/MapKitSample/) verdeutlicht.

Erstellen Sie eine Schaltfläche, die einen Kompass (einschließlich live-Animation aus, wenn die Ausrichtung der Zuordnung geändert wird), sieht und rendert ihn für ein anderes Steuerelement.

![Compass-Schaltfläche in der Navigationsleiste](mapkit-images/compass-sml.png)

Der folgende Code erstellt eine Compass-Schaltfläche und rendert ihn auf der Navigationsleiste auf:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

Die `ShowsCompass` Eigenschaft kann zum Steuern der Sichtbarkeit des Kompasses standardmäßig in der Kartenansicht verwendet werden.

<a name="scale" />

## <a name="scale-view"></a>Skalierungsansicht

Hinzufügen des Scale an anderer Stelle in der Ansicht mit den `MKScaleView.FromMapView()` -Methode zum Abrufen einer Instanz der Skalierungsansicht an anderer Stelle in der Hierarchie von Inhaltsansichten hinzufügen.

![Skalierungsansicht als Überlagerung auf einer Karte](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

Die `ShowsScale` Eigenschaft kann zum Steuern der Sichtbarkeit des Kompasses standardmäßig in der Kartenansicht verwendet werden.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Schaltfläche "Benutzer-Überwachung"

Die Schaltfläche "Benutzer-nachverfolgung" konzentriert sich die Karte auf aktuellen Standort des Benutzers. Verwenden der `MKUserTrackingButton.FromMapView()` Methode rufen Sie eine Instanz der Schaltfläche und Anwenden von Formatierungsänderungen an anderer Stelle in der Hierarchie von Inhaltsansichten hinzufügen.

![Schaltfläche der Benutzer-Speicherort als Überlagerung auf einer Karte](mapkit-images/user-location-sml.png)

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
- [Was ist neu in der MapKit (WWDC) (video)](https://developer.apple.com/videos/play/wwdc2017/237/)
