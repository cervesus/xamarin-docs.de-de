---
title: Hervorheben eines kreisförmigen Bereiches auf einer Karte
description: In diesem Artikel wird beschrieben, wie Sie einer Karte eine kreisförmige Überlagerung hinzufügen, um einen kreisförmigen Bereich auf der Karte hervorzuheben. Die Betriebssysteme iOS und Android stellen APIs bereit, um die kreisförmige Überlagerung der Karte hinzuzufügen, während die Überlagerung auf der Universellen Windows-Plattform als Polygon gerendert wird.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 1fe2611e26d357d910cc85800355b42d11e1104b
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "72697183"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Hervorheben eines kreisförmigen Bereiches auf einer Karte

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-circle)

_In diesem Artikel wird beschrieben, wie Sie einer Karte eine kreisförmige Überlagerung hinzufügen, um einen kreisförmigen Bereich auf der Karte hervorzuheben._

## <a name="overview"></a>Übersicht

Eine Überlagerung ist eine überlappende Grafik auf einer Karte. Überlagerungen unterstützen das Zeichnen grafischer Inhalte, die beim Zoomen mit der Karte skaliert werden. Die folgenden Screenshots zeigen eine kreisförmige Überlagerung, die zu einer Karte hinzugefügt wurden:

![](circle-map-overlay-images/screenshots.png)

Beim Rendern eines [`Map`](xref:Xamarin.Forms.Maps.Map)-Steuerelements durch eine Xamarin.Forms-App wird in iOS die `MapRenderer`-Klasse instanziiert, wodurch wiederum ein natives `MKMapView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `MapRenderer`-Klasse ein natives `MapView`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `MapRenderer`-Klasse eine native `MapControl`-Klasse. Der Renderingprozess kann genutzt werden, um plattformspezifische Kartenanpassungen zu implementieren, indem für eine `Map`-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#Creating_the_Custom_Map) Sie eine benutzerdefinierte Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) Sie die benutzerdefinierte Karte über Xamarin.Forms.
1. [Passen Sie](#Customizing_the_Map) die Karte durch Erstellen eines benutzerdefinierten Renderers für die Karte auf jeder Plattform an.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) muss vor der Verwendung initialisiert und konfiguriert werden. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map/index.md).

Weitere Informationen zum Anpassen einer Karte mit einem benutzerdefinierten Renderer finden Sie unter [Customizing a Map Pin (Anpassen einer Kartenstecknadel)](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Erstellen der benutzerdefinierten Karte

Erstellen Sie eine `CustomCircle`-Klasse, die über die Eigenschaften `Position` und `Radius` verfügt:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Erstellen Sie anschließend eine Unterklasse der [`Map`](xref:Xamarin.Forms.Maps.Map)-Klasse, die eine `CustomCircle`-Eigenschaft hinzufügt:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Nutzen der benutzerdefinierten Karte

Nutzen Sie das `CustomMap`-Steuerelement, indem Sie eine Instanz davon in der XAML-Seiteninstanz deklarieren:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:local="clr-namespace:MapOverlay;assembly=MapOverlay"
             x:Class="MapOverlay.MapPage">
    <ContentPage.Content>
        <local:CustomMap x:Name="customMap" MapType="Street" WidthRequest="{x:Static local:App.ScreenWidth}" HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Alternativ können Sie das `CustomMap`-Steuerelement nutzen, indem Sie eine Instanz davon in der C#-Seiteninstanz deklarieren:

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS ()
    {
        var customMap = new CustomMap {
            MapType = MapType.Street,
            WidthRequest = App.ScreenWidth,
            HeightRequest = App.ScreenHeight
        };
        ...
        Content = customMap;
    }
}
```

Initialisieren Sie das `CustomMap`-Steuerelement nach Bedarf:

```csharp
public partial class MapPage : ContentPage
{
  public MapPage ()
  {
    ...
    var pin = new Pin {
      Type = PinType.Place,
      Position = new Position (37.79752, -122.40183),
      Label = "Xamarin San Francisco Office",
      Address = "394 Pacific Ave, San Francisco CA"
    };

    var position = new Position (37.79752, -122.40183);
    customMap.Circle = new CustomCircle {
      Position = position,
      Radius = 1000
    };

    customMap.Pins.Add (pin);
    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (position, Distance.FromMiles (1.0)));
  }
}
```

Diese Initialisierung fügt die [`Pin`](xref:Xamarin.Forms.Maps.Pin)- und `CustomCircle`-Instanzen der benutzerdefinierten Karte hinzu und positioniert die Kartenansicht mit der [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)-Methode, wodurch sich die Position und der Zoomfaktor der Karte ändern, indem eine [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)-Klasse aus einer [`Position`](xref:Xamarin.Forms.Maps.Position)- und einer [`Distance`](xref:Xamarin.Forms.Maps.Distance)-Struktur erstellt wird.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Karte

Ein benutzerdefinierter Renderer muss nun zu jedem Projekt der Anwendung hinzugefügt werden, um die kreisförmige Überlagerung zur Karte hinzuzufügen.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren Methode `OnElementChanged`, um die kreisförmige Überlagerung hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKCircleRenderer circleRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    circleRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                var circle = formsMap.Circle;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                var circleOverlay = MKCircle.Circle(new CoreLocation.CLLocationCoordinate2D(circle.Position.Latitude, circle.Position.Longitude), circle.Radius);
                nativeMap.AddOverlay(circleOverlay);
            }
        }
        ...
    }
}

```

Diese Methode führt die folgende Konfiguration durch, vorausgesetzt der benutzerdefinierte Renderer wurde an ein neues Xamarin.Forms-Element angefügt:

- Die Eigenschaft `MKMapView.OverlayRenderer` wird auf einen entsprechenden Delegaten festgelegt.
- Der Kreis wird erstellt, indem ein statisches `MKCircle`-Objekt festgelegt wird, das die Mitte und den Radius des Kreises (in Metern) bestimmt.
- Der Kreis wird durch Aufrufen der Methode `MKMapView.AddOverlay` der Karte hinzugefügt.

Implementieren Sie dann die Methode `GetOverlayRenderer`, um das Rendering der Überlagerung anzupassen:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKCircleRenderer circleRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (circleRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          circleRenderer = new MKCircleRenderer(overlay as MKCircle) {
              FillColor = UIColor.Red,
              Alpha = 0.4f
          };
      }
      return circleRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren Methoden `OnElementChanged` und `OnMapReady`, um die kreisförmige Überlagerung hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        CustomCircle circle;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Xamarin.Forms.Maps.Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                circle = formsMap.Circle;
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var circleOptions = new CircleOptions();
            circleOptions.InvokeCenter(new LatLng(circle.Position.Latitude, circle.Position.Longitude));
            circleOptions.InvokeRadius(circle.Radius);
            circleOptions.InvokeFillColor(0X66FF0000);
            circleOptions.InvokeStrokeColor(0X66FF0000);
            circleOptions.InvokeStrokeWidth(0);

            NativeMap.AddCircle(circleOptions);
        }
    }
}
```

Die `OnElementChanged`-Methode ruft die benutzerdefinierten Kreisdaten ab, sofern der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Sobald die `GoogleMap`-Instanz verfügbar ist, wird die `OnMapReady`-Methode aufgerufen, wobei der Kreis durch Instanziieren eines `CircleOptions`-Objekts erstellt wird, das die Mitte und den Radius des Kreises (in Metern) bestimmt. Der Kreis wird anschließend durch Aufrufen der Methode `NativeMap.AddCircle` der Karte hinzugefügt.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen des benutzerdefinierten Renderers auf der Universellen Windows-Plattform

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren Methode `OnElementChanged`, um die kreisförmige Überlagerung hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        const int EarthRadiusInMeteres = 6371000;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }
            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MapControl;
                var circle = formsMap.Circle;

                var coordinates = new List<BasicGeoposition>();
                var positions = GenerateCircleCoordinates(circle.Position, circle.Radius);
                foreach (var position in positions)
                {
                    coordinates.Add(new BasicGeoposition { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
        // GenerateCircleCoordinates helper method (below)
    }
}
```

Diese Methode führt die folgenden Vorgänge durch, vorausgesetzt der benutzerdefinierte Renderer wurde an ein neues Xamarin.Forms-Element angefügt:

- Die Position und der Radius des Kreises werden von der Eigenschaft `CustomMap.Circle` abgerufen und an die Methode `GenerateCircleCoordinates` übergeben, die die Koordinaten für Breiten- und Längengrad des Kreisumfangs erstellt. Der Code für diese Hilfsprogrammmethode ist hier dargestellt:
- Die Koordinaten des Kreisumfangs werden in eine `List`-Klasse mit `BasicGeoposition`-Koordinaten konvertiert.
- Der Kreis wird durch Instanziieren eines `MapPolygon`-Objekts erstellt. Die `MapPolygon`-Klasse wird zum Anzeigen einer Multipunktform auf der Karte verwendet, indem deren `Path`-Eigenschaft auf ein `Geopath`-Objekt festgelegt wird, das die Koordinaten der Form enthält.
- Der Kreis wird auf der Karte durch Hinzufügen zur Sammlung `MapControl.MapElements` gerendert.

```
List<Position> GenerateCircleCoordinates(Position position, double radius)
{
    double latitude = position.Latitude.ToRadians();
    double longitude = position.Longitude.ToRadians();
    double distance = radius / EarthRadiusInMeteres;
    var positions = new List<Position>();

    for (int angle = 0; angle <=360; angle++)
    {
        double angleInRadians = ((double)angle).ToRadians();
        double latitudeInRadians = Math.Asin(Math.Sin(latitude) * Math.Cos(distance) + Math.Cos(latitude) * Math.Sin(distance) * Math.Cos(angleInRadians));
        double longitudeInRadians = longitude + Math.Atan2(Math.Sin(angleInRadians) * Math.Sin(distance) * Math.Cos(latitude), Math.Cos(distance) - Math.Sin(latitude) * Math.Sin(latitudeInRadians));

        var pos = new Position(latitudeInRadians.ToDegrees(), longitudeInRadians.ToDegrees());
        positions.Add(pos);
    }

    return positions;
}
```

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie einer Karte eine kreisförmige Überlagerung hinzufügen, um einen kreisförmigen Bereich auf der Karte hervorzuheben.

## <a name="related-links"></a>Verwandte Links

- [Circular Map Ovlerlay (sample) (Kreisförmige Kartenüberlagerung (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-circle)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps Namespace (Xamarin.Forms.Maps-Namespace)](xref:Xamarin.Forms.Maps)
