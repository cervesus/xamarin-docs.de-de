---
title: Markieren einer runden Fläche auf einer Karte
description: In diesem Artikel wird erläutert, wie eine Zuordnung, zum Markieren einer runden Fläche der Karte eine zirkuläre Überlagerung hinzugefügt. Zwar iOS und Android APIs für das zirkuläre Overlay zur Karte hinzufügen, wird das Overlay auf UWP als ein Polygon gerendert.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 3064296d4c78a3342fb27afc971c37a029987e5e
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998556"
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Markieren einer runden Fläche auf einer Karte

_In diesem Artikel wird erläutert, wie eine Zuordnung, zum Markieren einer runden Fläche der Karte eine zirkuläre Überlagerung hinzugefügt._

## <a name="overview"></a>Übersicht

Eine Überlagerung ist eine überlappende Grafik auf einer Karte. Überlagerungen unterstützen grafischen Zeichnungsinhalt, die mit der Zuordnung skaliert werden soll, wie es vergrößert wird. Die folgenden Screenshots zeigen das Ergebnis der Addition von einem kreisförmigen Overlay zu einer Zuordnung an:

![](circle-map-overlay-images/screenshots.png)

Wenn eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) -Steuerelements von einer Xamarin.Forms-Anwendung unter iOS die `MapRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Das Rendern zu erstellt werden kann nutzen plattformspezifische Zuordnung Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine `Map` auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Anpassen von](#Customizing_the_Map) der Zuordnung durch das Erstellen eines benutzerdefinierten Renderers für die Zuordnung auf jeder Plattform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Informationen zum Anpassen einer Karte mithilfe eines benutzerdefinierten Renderers finden Sie unter [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Die benutzerdefinierte Karte erstellen

Erstellen Sie eine `CustomCircle` Klasse, die verfügt `Position` und `Radius` Eigenschaften:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Erstellen Sie dann auf eine Unterklasse von der [ `Map` ](xref:Xamarin.Forms.Maps.Map) Klasse bereit, die eine Eigenschaft des Typs fügt `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Nutzen die benutzerdefinierte Karte

Nutzen der `CustomMap` Steuerelement durch deklarieren eine Instanz davon in der XAML-Seite-Instanz:

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

Verwenden Sie alternativ die `CustomMap` Steuerelement durch deklarieren eine Instanz davon in der C#-Seite-Instanz:

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

Initialisieren der `CustomMap` nach Bedarf steuern:

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

Fügt diese Initialisierung [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) und `CustomCircle` Instanzen, die benutzerdefinierte Karte und positioniert die Map-Ansicht mit den [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode, die ändert sich die Position und Zoom auf der Karte durch das Erstellen einer [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) aus einer [ `Position` ](xref:Xamarin.Forms.Maps.Position) und [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Karte

Ein benutzerdefinierter Renderer muss jedes Anwendungsprojekt aus, um die zirkuläre Überlagerung zur Karte hinzufügen jetzt hinzugefügt werden.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` Methode, um die zirkuläre Überlagerung hinzuzufügen:

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

Diese Methode führt die folgende Konfiguration, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Die `MKMapView.OverlayRenderer` -Eigenschaftensatz auf einen entsprechenden Delegaten.
- Der Kreis wird durch Festlegen einer statischen erstellt `MKCircle` -Objekt, das den Mittelpunkt des Kreises und den Radius des Kreises in Metern angibt.
- Der Kreis wird zur Karte hinzugefügt, durch Aufrufen der `MKMapView.AddOverlay` Methode.

Implementieren Sie anschließend die `GetOverlayRenderer` Methode, um die Wiedergabe der Überlagerung anzupassen:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` und `OnMapReady` Methoden, um die zirkuläre Überlagerung hinzuzufügen:

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
                Control.GetMapAsync(this);
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

Die `OnElementChanged` Methodenaufrufe der `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die an die Sicht gebunden ist, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Einmal die `GoogleMap` Instanz verfügbar ist, die `OnMapReady` Methode wird aufgerufen, und, in der Kreis erstellt wird, durch die Instanziierung einer `CircleOptions` -Objekt, das den Mittelpunkt des Kreises und den Radius des Kreises in Metern angibt. Der Kreis wird dann zur Karte hinzugefügt, durch den Aufruf der `NativeMap.AddCircle` Methode.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen den benutzerdefinierten Renderer für die universelle Windows-Plattform

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` Methode, um die zirkuläre Überlagerung hinzuzufügen:

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

Diese Methode führt die folgenden Vorgänge, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Die Kreis-Position und die RADIUS-werden abgerufen, von der `CustomMap.Circle` Eigenschaft und an die `GenerateCircleCoordinates` -Methode, die Breiten- und Längengrad generiert Koordinaten für den Umfang des Kreises. Der Code für diese Hilfsmethode ist unten dargestellt.
- Die Koordinaten des Kreises Umkreis werden in konvertiert eine `List` von `BasicGeoposition` Koordinaten.
- Der Kreis wird erstellt, durch die Instanziierung einer `MapPolygon` Objekt. Die `MapPolygon` Klasse wird verwendet, um eine Form mit mehreren Punkten auf der Karte angezeigt, durch Festlegen der `Path` Eigenschaft, um eine `Geopath` -Objekt, das die Koordinaten der Form enthält.
- Das Polygon auf der Karte gerendert wird, indem sie zum Hinzufügen der `MapControl.MapElements` Auflistung.


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

In diesem Artikel wurde erläutert, wie eine zirkuläre Überlagerung auf einer Karte zum Markieren einer runden Fläche der Karte hinzufügen.


## <a name="related-links"></a>Verwandte Links

- [Zirkuläre Zuordnung Ovlerlay (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
