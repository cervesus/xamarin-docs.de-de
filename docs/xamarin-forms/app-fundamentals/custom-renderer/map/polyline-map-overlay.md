---
title: Markieren einer Route auf einer Karte
description: In diesem Artikel wird erläutert, wie einem Polyline-Overlay zu einer Karte hinzufügen. Einem Polyline-Overlay besteht aus einer Reihe verbundener Liniensegmente an, die in der Regel verwendet werden, zum Anzeigen einer Route auf einer Karte oder bilden eine beliebige Form, die erforderlich sind.
ms.prod: xamarin
ms.assetid: FBFDC715-1654-4188-82A0-FC522548BCFF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 786f050495d4682b719178f2723c482929544678
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998719"
---
# <a name="highlighting-a-route-on-a-map"></a>Markieren einer Route auf einer Karte

_In diesem Artikel wird erläutert, wie einem Polyline-Overlay zu einer Karte hinzufügen. Einem Polyline-Overlay besteht aus einer Reihe verbundener Liniensegmente an, die in der Regel verwendet werden, zum Anzeigen einer Route auf einer Karte oder bilden eine beliebige Form, die erforderlich sind._

## <a name="overview"></a>Übersicht

Eine Überlagerung ist eine überlappende Grafik auf einer Karte. Überlagerungen unterstützen grafischen Zeichnungsinhalt, die mit der Zuordnung skaliert werden soll, wie es vergrößert wird. Die folgenden Screenshots zeigen das Ergebnis der Addition von einem Polyline-Overlay zu einer Zuordnung an:

![](polyline-map-overlay-images/screenshots.png)

Wenn eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) -Steuerelements von einer Xamarin.Forms-Anwendung unter iOS die `MapRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Das Rendern zu erstellt werden kann nutzen plattformspezifische Zuordnung Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine `Map` auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Anpassen von](#Customizing_the_Map) der Zuordnung durch das Erstellen eines benutzerdefinierten Renderers für die Zuordnung auf jeder Plattform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informationen zum Anpassen einer Karte mithilfe eines benutzerdefinierten Renderers finden Sie unter [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Die benutzerdefinierte Karte erstellen

Erstellen Sie eine Unterklasse von der [ `Map` ](xref:Xamarin.Forms.Maps.Map) Klasse bereit, die Fügt eine `RouteCoordinates` Eigenschaft:

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
    customMap.RouteCoordinates.Add (new Position (37.785559, -122.396728));
    customMap.RouteCoordinates.Add (new Position (37.780624, -122.390541));
    customMap.RouteCoordinates.Add (new Position (37.777113, -122.394983));
    customMap.RouteCoordinates.Add (new Position (37.776831, -122.394627));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
  }
}
```

Diese Initialisierung gibt eine Reihe von Breiten- und Längengrad Koordinaten, um die Route definieren, auf der Karte hervorgehoben werden. Klicken Sie dann auf die Position der Map-Ansicht mit den [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode, die ändert sich die Position und die Zoomstufe der Karte durch das Erstellen einer [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) aus einer [ `Position` ](xref:Xamarin.Forms.Maps.Position) und [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Karte

Ein benutzerdefinierter Renderer muss jetzt jedem Projekt der Anwendung das Polyline-Overlay zur Karte hinzufügen hinzugefügt werden.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` Methode, um das Polyline-Overlay hinzuzufügen:

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

Diese Methode führt die folgende Konfiguration, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Die `MKMapView.OverlayRenderer` -Eigenschaftensatz auf einen entsprechenden Delegaten.
- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.RouteCoordinates` Eigenschaft und als ein Array von gespeichert `CLLocationCoordinate2D` Instanzen.
- Das Polyline-Objekt wird erstellt, durch Aufrufen der statischen `MKPolyline.FromCoordinates` -Methode, die den Breiten- und Längengrad der einzelnen Punkten angibt.
- Das Polyline-Objekt zur Karte hinzugefügt wird, indem die `MKMapView.AddOverlay` Methode.

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

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` und `OnMapReady` Methoden, um das Polyline-Overlay hinzuzufügen:

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

Die `OnElementChanged` Methode ruft die Auflistung der Breiten- und Längenkoordinaten aus der `CustomMap.RouteCoordinates` Eigenschaft und speichert sie in einer Membervariablen gespeichert. Es ruft dann die `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die an die Sicht gebunden ist, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Einmal die `GoogleMap` Instanz verfügbar ist, die `OnMapReady` Methode wird aufgerufen, und, wo das Polyline-Objekt erstellt wird, durch die Instanziierung einer `PolylineOptions` Objekt, das den Breiten- und Längengrad der einzelnen Punkten angibt. Das Polyline-Objekt wird dann zur Karte hinzugefügt, durch den Aufruf der `NativeMap.AddPolyline` Methode.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen den benutzerdefinierten Renderer für die universelle Windows-Plattform

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` Methode, um das Polyline-Overlay hinzuzufügen:

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

Diese Methode führt die folgenden Vorgänge, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.RouteCoordinates` Eigenschaft und in konvertiert eine `List` von `BasicGeoposition` Koordinaten.
- Das Polyline-Objekt wird erstellt, durch die Instanziierung einer `MapPolyline` Objekt. Die `MapPolygon` Klasse wird verwendet, um eine Zeile auf der Karte angezeigt, durch Festlegen seiner `Path` Eigenschaft, um eine `Geopath` -Objekt, das die zeilenkoordinaten enthält.
- Das Polyline-Objekt auf der Karte gerendert wird, indem sie zum Hinzufügen der `MapControl.MapElements` Auflistung.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie Hinzufügen von einem Polyline-Overlay zu einer Karte zum Anzeigen einer Route auf einer Karte oder bilden eine beliebige Form, die erforderlich sind.


## <a name="related-links"></a>Verwandte Links

- [Polylinie Zuordnung Ovlerlay (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polyline/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
