---
title: Neue Features in MapKit unter IOS 11
description: 'In diesem Dokument werden die neuen MapKit-Features in ios 11 beschrieben: Gruppierungs Marker, die Kompass Schaltfläche, die Skalierungs Ansicht und die Benutzer nach Verfolgungs Schaltfläche.'
ms.prod: xamarin
ms.assetid: 304AE5A3-518F-422F-BE24-92D62CE30F34
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 08/30/2017
ms.openlocfilehash: c194f2c9f8ea974bf3d6a8798f8a12e246c7d75b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286685"
---
# <a name="new-features-in-mapkit-on-ios-11"></a>Neue Features in MapKit unter IOS 11

IOS 11 fügt MapKit die folgenden neuen Funktionen hinzu:

- [Annotation-Clustering](#clustering)
- [Kompass Schaltfläche](#compass)
- [Skalierungs Ansicht](#scale)
- [Schaltfläche "Benutzer Verfolgung](#user-tracking)

![Karte mit geclusterten Markern und Kompass Schaltfläche](mapkit-images/cyclemap-heading.png)

<a name="clustering" />

## <a name="automatically-grouping-markers-while-zooming"></a>Automatisches Gruppieren von Markern beim Zoomen

Das Beispiel ["tandm" für das MapKit](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample) zeigt, wie das neue Feature "IOS 11-Anmerkung-Clustering" implementiert wird.

### <a name="1-create-an-mkpointannotation-subclass"></a>1. Erstellen einer `MKPointAnnotation` Unterklasse

Die Point-Anmerkung-Klasse stellt die einzelnen Marker auf der Karte dar. Sie können mithilfe von `MapView.AddAnnotation()` oder mithilfe von `MapView.AddAnnotations()`einzeln hinzugefügt werden.

Pointannotation-Klassen verfügen über keine visuelle Darstellung, Sie sind nur erforderlich, um die dem Marker zugeordneten Daten darzustellen (am wichtigsten `Coordinate` ist dies die Eigenschaft, bei der es sich um den breiten-und Längengrad der Zuordnung handelt) und alle benutzerdefinierten Eigenschaften:

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

### <a name="2-create-an-mkmarkerannotationview-subclass-for-single-markers"></a>2. Erstellen einer `MKMarkerAnnotationView` Unterklasse für einzelne Marker

Die Ansicht der markeranmerkung ist die visuelle Darstellung jeder Anmerkung und wird mit Eigenschaften wie z. b. formatiert:

- **Markertintcolor** – die Farbe für den Marker.
- **Glyphtext** – im Marker angezeigter Text.
- **Glyphimage** – legt das Bild fest, das in der Markierung angezeigt wird.
- **Displaypriority** – bestimmt die z-Reihenfolge (Stapel Verhalten), wenn die Zuordnung mit Markern überfüllt ist. Verwenden Sie eine `Required`der `DefaultHigh`-, `DefaultLow`-oder-.

Um das automatische Clustering zu unterstützen, müssen Sie auch Folgendes festlegen:

- **Clusteringidentifier** – Hiermit wird gesteuert, welche Marker gruppiert werden. Sie können den gleichen Bezeichner für alle Marker verwenden oder andere Bezeichner verwenden, um die Art und Weise zu steuern, wie Sie gruppiert werden.

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

### <a name="3-create-an-mkannotationview-to-represent-clusters-of-markers"></a>3. Erstellen eines `MKAnnotationView` zum Darstellen von markerclustern

Obwohl es sich bei der Anmerkung-Ansicht, die einen Cluster von Markern darstellt, um ein einfaches Bild handeln _kann_ , erwarten Benutzer, dass die APP visuelle Hinweise dazu bereitstellt, wie viele Marker gruppiert wurden.

Im [Beispielcode](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample) wird CoreGraphics verwendet, um die Anzahl der Marker im Cluster und eine Kreis Diagramm Darstellung des Anteils der einzelnen Markertypen darzustellen.

Sie sollten auch Folgendes festlegen:

- **Displaypriority** – bestimmt die z-Reihenfolge (Stapel Verhalten), wenn die Zuordnung mit Markern überfüllt ist. Verwenden Sie eine `Required`der `DefaultHigh`-, `DefaultLow`-oder-.
- **Collisionmode** – `Circle` oder `Rectangle`.

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

### <a name="4-register-the-view-classes"></a>4. Registrieren der Ansichts Klassen

Wenn das Kartenansicht-Steuerelement erstellt und zu einer Ansicht hinzugefügt wird, registrieren Sie die Anmerkung-Ansichts Typen, um das automatische clusteringverhalten zu aktivieren, wenn die Zuordnung vergrößert und ausgehend wird:

```csharp
MapView.Register(typeof(BikeView), MKMapViewDefault.AnnotationViewReuseIdentifier);
MapView.Register(typeof(ClusterView), MKMapViewDefault.ClusterAnnotationViewReuseIdentifier);
```

### <a name="5-render-the-map"></a>5. Rendering der Karte!

Wenn die Zuordnung gerendert wird, werden die Anmerkung-Marker abhängig von der Zoomstufe gruppiert oder gerendert. Wenn sich die Zoomstufe ändert, animieren Marker in und aus Clustern.

![Simulator, der gruppierte Marker auf der Karte anzeigt](mapkit-images/cyclemap-sml.png)

Weitere Informationen zum Anzeigen von Daten mit MapKit finden Sie im [Abschnitt Maps](~/ios/user-interface/controls/ios-maps/index.md) .

<a name="compass" />

## <a name="compass-button"></a>Kompass Schaltfläche

IOS 11 bietet die Möglichkeit, den Kompass aus der Zuordnung zu übertragen und an anderer Stelle in der Ansicht zu rendereren. Ein Beispiel finden Sie in der Beispiel- [App für tandm](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample) .

Erstellen Sie eine Schaltfläche, die wie ein Kompass aussieht (einschließlich Live Animation, wenn die Karten Ausrichtung geändert wird), und rendert Sie in einem anderen Steuerelement.

![Kompass Schaltfläche in der Navigationsleiste](mapkit-images/compass-sml.png)

Der folgende Code erstellt eine Kompass Schaltfläche und rendert Sie auf der Navigationsleiste:

```csharp
var compass = MKCompassButton.FromMapView(MapView);
compass.CompassVisibility = MKFeatureVisibility.Visible;
NavigationItem.RightBarButtonItem = new UIBarButtonItem(compass);
MapView.ShowsCompass = false; // so we don't have two compasses!
```

Die `ShowsCompass` -Eigenschaft kann verwendet werden, um die Sichtbarkeit des standardkompass in der Kartenansicht zu steuern.

<a name="scale" />

## <a name="scale-view"></a>Skalierungs Ansicht

Fügen Sie die Skala an anderer Stelle in der `MKScaleView.FromMapView()` Sicht hinzu, indem Sie die-Methode verwenden, um eine Instanz der Skalierungs Ansicht zu erhalten, die an anderer Stelle in der

![Skalierungs Ansicht überlagert auf einer Karte](mapkit-images/scale-sml.png)

```csharp
var scale = MKScaleView.FromMapView(MapView);
scale.LegendAlignment = MKScaleViewAlignment.Trailing;
scale.TranslatesAutoresizingMaskIntoConstraints = false;
View.AddSubview(scale); // constraints omitted for simplicity
MapView.ShowsScale = false; // so we don't have two scale displays!
```

Die `ShowsScale` -Eigenschaft kann verwendet werden, um die Sichtbarkeit des standardkompass in der Kartenansicht zu steuern.

<a name="user-tracking" />

## <a name="user-tracking-button"></a>Schaltfläche "Benutzer Verfolgung

Die Schaltfläche Benutzer Nachverfolgung zentriert die Karte auf den aktuellen Speicherort des Benutzers. Verwenden Sie `MKUserTrackingButton.FromMapView()` die-Methode, um eine Instanz der Schaltfläche zu erhalten, Formatierungs Änderungen anzuwenden und an anderer Stelle in der Ansichts Hierarchie hinzuzufügen.

![Benutzer Positions Schaltfläche auf einer Karte überlagert](mapkit-images/user-location-sml.png)

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

- [MapKit-Beispiel ' tandm '](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-mapkitsample)
- [MKCompassButton](https://developer.apple.com/documentation/mapkit/mkcompassbutton)
- [Neues in MapKit (WWDC) (Video)](https://developer.apple.com/videos/play/wwdc2017/237/)
