---
title: Markieren eine Route auf einer Karte
description: In diesem Artikel wird erläutert, wie auf einer Karte eine Polylinie Überlagerung hinzugefügt wird. Eine Polylinie Overlay besteht aus einer Reihe von verbundenen Liniensegmenten, die normalerweise verwendet werden, bilden eine Form, die erforderlich ist, oder eine Route auf einer Karte anzeigen.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 8b80f9569e9377ca76798911cda64d8c0ee28d93
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34846642"
---
# <a name="highlighting-a-route-on-a-map"></a>Markieren eine Route auf einer Karte

_In diesem Artikel wird erläutert, wie auf einer Karte eine Polylinie Überlagerung hinzugefügt wird. Eine Polylinie Overlay besteht aus einer Reihe von verbundenen Liniensegmenten, die normalerweise verwendet werden, bilden eine Form, die erforderlich ist, oder eine Route auf einer Karte anzeigen._

## <a name="overview"></a>Übersicht

Overlay ist eine geschichteten Grafik auf einer Karte. Overlays unterstützen grafische Zeichnungsinhalt, die mit der Zuordnung skaliert werden, da es vergrößert wird. Die folgenden Screenshots zeigen das Ergebnis der Addition von einem Polylinie Overlay zu einer Zuordnung:

![](polyline-map-overlay-images/screenshots.png)

Wenn eine [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) einer Xamarin.Forms-Anwendung in iOS-Steuerelement gerendert wird die `MapRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf die universelle Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Des Renderingprozesses kann Vorteil ausgeführt werden, um Clientplattform-spezifische Zuordnung Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine `Map` auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Anpassen](#Customizing_the_Map) die Zuordnung durch das Erstellen eines benutzerdefinierten Renderers für die Zuordnung auf jeder Plattform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informationen zum Anpassen einer Karte, die mithilfe eines benutzerdefinierten Renderers finden Sie unter [Anpassen einer Karte Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Erstellen die benutzerdefinierte Karte

Erstellen Sie eine Unterklasse von der [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Klasse bereit, die Fügt eine `RouteCoordinates` Eigenschaft:

```csharp
public class CustomMap : Map
{
  public List<Position> RouteCoordinates { get; set; }

  public CustomMap ()
  {
    RouteCoordinates = new List<Position> ();
  }
}
```

Die `RouteCoordinates` Eigenschaft speichert eine Auflistung von Koordinaten zur Definition der Route hervorgehoben werden.

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Diese Initialisierung gibt eine Reihe von Breiten- und Längengrad Koordinaten, definieren Sie die Route auf der Karte hervorgehoben werden. Klicken Sie dann auf die Position der Kartenansicht mit der [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) -Methode, die ändert die Position und die Zoomstufe der Karte durch das Erstellen einer [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) aus einem [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) und ein [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Zuordnung

Ein benutzerdefinierter Renderer muss nun jede-Anwendungsprojekt auf der Karte die Polylinie Überlagerung hinzugefügt hinzugefügt werden.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` Methode, um die Überlagerung Polylinie hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolylineRenderer polylineRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polylineRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.RouteCoordinates.Count];
                int index = 0;
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var routeOverlay = MKPolyline.FromCoordinates(coords);
                nativeMap.AddOverlay(routeOverlay);
            }
        }
        ...
    }
}

```

Diese Methode führt die folgende Konfiguration, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Die `MKMapView.OverlayRenderer` Eigenschaft auf einen entsprechenden Delegaten festgelegt ist.
- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.RouteCoordinates` Eigenschaft und gespeichert als ein Array von `CLLocationCoordinate2D` Instanzen.
- Die Polylinie wird erstellt, durch Aufrufen der statischen `MKPolyline.FromCoordinates` -Methode, die Breiten- und Längengrad der einzelnen Punkt angibt.
- Die Polylinie wird zur Zuordnung hinzugefügt, indem die `MKMapView.AddOverlay` Methode.

Implementieren Sie anschließend die `GetOverlayRenderer` Methode, um die Wiedergabe der Überlagerung anzupassen:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolylineRenderer polylineRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polylineRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polylineRenderer = new MKPolylineRenderer(overlay as MKPolyline) {
              FillColor = UIColor.Blue,
              StrokeColor = UIColor.Red,
              LineWidth = 3,
              Alpha = 0.4f
          };
      }
      return polylineRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` und `OnMapReady` Methoden, um die Überlagerung Polylinie hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> routeCoordinates;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                // Unsubscribe
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                routeCoordinates = formsMap.RouteCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polylineOptions = new PolylineOptions();
            polylineOptions.InvokeColor(0x66FF0000);

            foreach (var position in routeCoordinates)
            {
                polylineOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }

            NativeMap.AddPolyline(polylineOptions);
        }
    }
}
```

Die `OnElementChanged` Methode ruft die Auflistung der Breiten- und Längenkoordinaten aus der `CustomMap.RouteCoordinates` Eigenschaft und speichert sie in einer Membervariablen gespeichert. Er ruft dann die `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die an die Sicht gebunden ist, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Einmal die `GoogleMap` Instanz verfügbar ist, wird der `OnMapReady` Methode wird aufgerufen, wo die Polylinie durch Instanziierung erstellt wird eine `PolylineOptions` Objekt, das die Breiten- und Längengrad der einzelnen Punkt angibt. Die Polylinie wird dann zur Zuordnung hinzugefügt, durch Aufrufen der `NativeMap.AddPolyline` Methode.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen von benutzerdefinierten Renderers für universelle Windows-Plattform

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` Methode, um die Überlagerung Polylinie hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
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

                var coordinates = new List<BasicGeoposition>();
                foreach (var position in formsMap.RouteCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polyline = new MapPolyline();
                polyline.StrokeColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polyline.StrokeThickness = 5;
                polyline.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polyline);
            }
        }
    }
}
```

Diese Methode führt die folgenden Vorgänge aus, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.RouteCoordinates` Eigenschaft und konvertierte in eine `List` von `BasicGeoposition` Koordinaten.
- Die Polylinie wird erstellt, durch die Instanziierung einer `MapPolyline` Objekt. Die `MapPolygon` Klasse wird verwendet, um eine Zeile auf der Karte angezeigt werden, durch Festlegen seiner `Path` Eigenschaft, um eine `Geopath` Objekt, das die Zeile Koordinaten enthält.
- Die Polylinie wird auf der Karte gerendert, indem sie zum Hinzufügen der `MapControl.MapElements` Auflistung.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wird erläutert, wie eine Karte, um eine Route auf einer Karte anzeigen oder bilden eine Form, die erforderlich ist, eine Polylinie Überlagerung hinzugefügt.


## <a name="related-links"></a>Verwandte Links

- [Polylinie Zuordnung Ovlerlay (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
