---
title: Markieren eine Kreisbereiche auf einer Karte
description: In diesem Artikel wird erläutert, wie eine Karte, um eine zirkuläre Bereich der Karte zu markieren eine zirkuläre Überlagerung hinzugefügt.
ms.prod: xamarin
ms.assetid: 6FF8BD15-074E-4E6A-9522-F9E2BE32EF12
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: a70c8fdca457e386a1490ca974e1a1ea5da2f6db
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="highlighting-a-circular-area-on-a-map"></a>Markieren eine Kreisbereiche auf einer Karte

_In diesem Artikel wird erläutert, wie eine Karte, um eine zirkuläre Bereich der Karte zu markieren eine zirkuläre Überlagerung hinzugefügt._

## <a name="overview"></a>Übersicht

Overlay ist eine geschichteten Grafik auf einer Karte. Overlays unterstützen grafische Zeichnungsinhalt, die mit der Zuordnung skaliert werden, da es vergrößert wird. Die folgenden Screenshots zeigen das Ergebnis einer Zuordnung eine zirkuläre Überlagerung hinzugefügt:

![](circle-map-overlay-images/screenshots.png)

Wenn eine [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) einer Xamarin.Forms-Anwendung in iOS-Steuerelement gerendert wird die `MapRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf die universelle Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Des Renderingprozesses kann Vorteil ausgeführt werden, um Clientplattform-spezifische Zuordnung Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine `Map` auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Anpassen](#Customizing_the_Map) die Zuordnung durch das Erstellen eines benutzerdefinierten Renderers für die Zuordnung auf jeder Plattform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/") Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md)

Informationen zum Anpassen einer Karte, die mithilfe eines benutzerdefinierten Renderers finden Sie unter [Anpassen einer Karte Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Erstellen die benutzerdefinierte Karte

Erstellen einer `CustomCircle` Klasse, die verfügt `Position` und `Radius` Eigenschaften:

```csharp
public class CustomCircle
{
  public Position Position { get; set; }
  public double Radius { get; set; }
}
```

Erstellen Sie dann eine Unterklasse von der [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Klasse bereit, die eine Eigenschaft vom Typ fügt `CustomCircle`:

```csharp
public class CustomMap : Map
{
  public CustomCircle Circle { get; set; }
}
```

<a name="Consuming_the_Custom_Map" />

### <a name="consuming-the-custom-map"></a>Nutzen die benutzerdefinierte Karte

Nutzen der `CustomMap` Steuerelement durch Deklarieren einer Instanz des Zertifikats in der XAML-Seite-Instanz:

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

Alternativ können Sie nutzen die `CustomMap` Steuerelement durch Deklarieren einer Instanz des Zertifikats in der C#-Seite-Instanz:

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

Initialisieren der `CustomMap` nach Bedarf zu steuern:

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

Fügt diese Initialisierung [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) und `CustomCircle` Instanzen, die benutzerdefinierte Karte und die Kartenansicht mit positioniert das [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) -Methode, die die Position und Zoom ändert auf der Karte durch das Erstellen einer [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) aus einer [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) und ein [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Zuordnung

Ein benutzerdefinierter Renderer muss jetzt jedes Anwendungsprojekt zirkuläre Overlay hinzufügen, zu der Zuordnung hinzugefügt werden.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` Methode, um die zirkuläre Überlagerung hinzuzufügen:

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

Diese Methode führt die folgende Konfiguration, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Die `MKMapView.OverlayRenderer` Eigenschaft auf einen entsprechenden Delegaten festgelegt ist.
- Der Kreis wird erstellt, indem Sie einen statischen `MKCircle` Objekt, das den Mittelpunkt des Kreises und den Radius der Kreisradius in Metern angibt.
- Der Kreis wird zur Zuordnung hinzugefügt, indem die `MKMapView.AddOverlay` Methode.

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

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` und `OnMapReady` Methoden, um die zirkuläre Überlagerung hinzuzufügen:

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

Die `OnElementChanged` Methodenaufrufe der `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die an die Sicht gebunden ist, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Einmal die `GoogleMap` Instanz verfügbar ist, wird die `OnMapReady` Methode wird aufgerufen, wobei Kreises durch Instanziierung erstellt wird ein `CircleOptions` Objekt, das den Mittelpunkt des Kreises und den Radius der Kreisradius in Metern angibt. Der Kreis wird dann zur Zuordnung hinzugefügt, durch Aufrufen der `NativeMap.AddCircle` Methode.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen von benutzerdefinierten Renderers für universelle Windows-Plattform

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` Methode, um die zirkuläre Überlagerung hinzuzufügen:

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
        ...
    }
}
```

Diese Methode führt die folgenden Vorgänge aus, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Die Kreis-Position und den Radius werden abgerufen, von der `CustomMap.Circle` Eigenschaft und übergeben der `GenerateCircleCoordinates` -Methode, die Breiten- und Längengrad generiert Koordinaten für den Umfang des Kreises.
- Der Kreis Umkreis Koordinaten werden in konvertiert eine `List` von `BasicGeoposition` Koordinaten.
- Der Kreis wird erstellt, indem die Instanziierung einer `MapPolygon` Objekt. Die `MapPolygon` Klasse wird verwendet, um eine Form "multipunkteingabe" auf der Karte angezeigt, durch Festlegen seiner `Path` Eigenschaft, um eine `Geopath` Objekt, das die Koordinaten der Form "enthält.
- Das Polygon gerendert wird auf der Karte, hinzufügen zu der `MapControl.MapElements` Auflistung.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie einer Karte, um eine zirkuläre Bereich der Karte zu markieren eine zirkuläre Überlagerung hinzugefügt wird.


## <a name="related-links"></a>Verwandte Links

- [Zirkuläre Zuordnung Ovlerlay (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/circle/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
