---
title: Markieren einer Route auf einer Karte
description: In diesem Artikel wird erläutert, wie Sie eine Polylinienüberlagerung zu einer Karte hinzufügen. Eine Polylinienüberlagerung ist eine Reihe verbundener Liniensegmente, die in der Regel verwendet werden, um eine Route auf einer Karte anzuzeigen oder eine andere erforderliche Form zu bilden.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 184aa18ac8c0f27ce92a23b06b9dd0364f977abc
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53050883"
---
# <a name="highlighting-a-route-on-a-map"></a>Markieren einer Route auf einer Karte

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)

_In diesem Artikel wird erläutert, wie Sie eine Polylinienüberlagerung zu einer Karte hinzufügen. Eine Polylinienüberlagerung ist eine Reihe verbundener Liniensegmente, die in der Regel verwendet werden, um eine Route auf einer Karte anzuzeigen oder eine andere erforderliche Form zu bilden._

## <a name="overview"></a>Übersicht

Eine Überlagerung ist eine überlappende Grafik auf einer Karte. Überlagerungen unterstützen das Zeichnen grafischer Inhalte, die beim Zoomen mit der Karte skaliert werden. Die folgenden Screenshots zeigen Polylinienüberlagerungen, die zu einer Karte hinzugefügt wurden:

![](polyline-map-overlay-images/screenshots.png)

Beim Rendern eines [`Map`](xref:Xamarin.Forms.Maps.Map)-Steuerelements durch eine Xamarin.Forms-App wird in iOS die `MapRenderer`-Klasse instanziiert, wodurch wiederum ein natives `MKMapView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `MapRenderer`-Klasse ein natives `MapView`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `MapRenderer`-Klasse eine native `MapControl`-Klasse. Der Renderingprozess kann genutzt werden, um plattformspezifische Kartenanpassungen zu implementieren, indem für eine `Map`-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#Creating_the_Custom_Map) Sie eine benutzerdefinierte Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) Sie die benutzerdefinierte Karte über Xamarin.Forms.
1. [Passen Sie](#Customizing_the_Map) die Karte durch Erstellen eines benutzerdefinierten Renderers für die Karte auf jeder Plattform an.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) muss vor der Verwendung initialisiert und konfiguriert werden. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Weitere Informationen zum Anpassen einer Karte mit einem benutzerdefinierten Renderer finden Sie unter [Customizing a Map Pin (Anpassen einer Kartenstecknadel)](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Erstellen der benutzerdefinierten Karte

Erstellen Sie eine Unterklasse der [`Map`](xref:Xamarin.Forms.Maps.Map)-Klasse, die eine `RouteCoordinates`-Eigenschaft hinzufügt:

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

Die `RouteCoordinates`-Eigenschaft speichert eine Sammlung von Koordinaten, die die hervorzuhebende Route definieren.

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Durch diese Initialisierung werden eine Reihe von Breiten- und Längenkoordinaten bestimmt, um die auf der Karte hervorzuhebende Route zu definieren. Dann wird die Kartenansicht mit der [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)-Methode positioniert, wodurch sich die Position und der Zoomfaktor der Karte ändern, indem eine [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)-Klasse aus einer [`Position`](xref:Xamarin.Forms.Maps.Position)- und einer [`Distance`](xref:Xamarin.Forms.Maps.Distance)-Struktur erstellt wird.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Karte

Ein benutzerdefinierter Renderer muss nun zu jedem Projekt der Anwendung hinzugefügt werden, um die Polylinienüberlagerung zur Karte hinzuzufügen.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren `OnElementChanged`-Methode, um die Polylinienüberlagerung hinzuzufügen:

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

Diese Methode führt die folgende Konfiguration durch, vorausgesetzt der benutzerdefinierte Renderer wurde an ein neues Xamarin.Forms-Element angefügt:

- Die Eigenschaft `MKMapView.OverlayRenderer` wird auf einen entsprechenden Delegaten festgelegt.
- Die Sammlung von Breiten- und Längenkoordinaten wird aus der `CustomMap.RouteCoordinates`-Eigenschaft abgerufen und als Array von `CLLocationCoordinate2D`-Instanzen gespeichert.
- Die Polylinie wird durch Aufrufen der statischen `MKPolyline.FromCoordinates`-Methode erstellt, wodurch die Breite und Länge jedes Punkts angegeben wird.
- Die Polylinie wird durch Aufrufen der `MKMapView.AddOverlay`-Methode der Karte hinzugefügt.

Implementieren Sie dann die Methode `GetOverlayRenderer`, um das Rendering der Überlagerung anzupassen:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren Methoden `OnElementChanged` und `OnMapReady`, um die Polylinienüberlagerung hinzuzufügen:

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

Die `OnElementChanged`-Methode ruft die Sammlung der Breiten- und Längenkoordinaten aus der `CustomMap.RouteCoordinates`-Eigenschaft ab und speichert sie in einer Membervariablen. Dann ruft sie die Methode `MapView.GetMapAsync` auf, die die zugrunde liegende `GoogleMap`-Klasse abruft, die an die Ansicht gebunden ist, sofern der benutzerdefinierte Renderer einem neuen Xamarin.Forms-Element angefügt wurde. Sobald die `GoogleMap`-Instanz verfügbar ist, wird die `OnMapReady`-Methode aufgerufen, wobei die Polylinie durch Instanziieren eines `PolylineOptions`-Objekts erstellt wird, das die Breite und Länge jedes Punkts angibt. Die Polylinie wird dann durch Aufrufen der `NativeMap.AddPolyline`-Methode der Karte hinzugefügt.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen des benutzerdefinierten Renderers auf der Universellen Windows-Plattform

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren `OnElementChanged`-Methode, um die Polylinienüberlagerung hinzuzufügen:

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

Diese Methode führt die folgenden Vorgänge durch, vorausgesetzt der benutzerdefinierte Renderer wurde an ein neues Xamarin.Forms-Element angefügt:

- Die Sammlung von Breiten- und Längenkoordinaten wird aus der `CustomMap.RouteCoordinates`-Eigenschaft abgerufen und in eine `List` von `BasicGeoposition`-Koordinaten konvertiert.
- Die Polylinie wird durch Instanziieren eines `MapPolyline`-Objekts erstellt. Die `MapPolygon`-Klasse wird zum Anzeigen einer Linie auf der Karte verwendet, indem deren `Path`-Eigenschaft auf ein `Geopath`-Objekt festgelegt wird, das die Linienkoordinaten enthält.
- Die Polylinie wird auf der Karte durch Hinzufügen zur `MapControl.MapElements`-Collection gerendert.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Sie eine Polylinienüberlagerung einer Karte hinzufügen, um eine Route auf einer Karte anzuzeigen oder eine andere erforderliche Form zu bilden.


## <a name="related-links"></a>Verwandte Links

- [Polyline Map Overla (Polylinienkartenüberlagerung (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps Namespace (Xamarin.Forms.Maps-Namespace)](xref:Xamarin.Forms.Maps)
