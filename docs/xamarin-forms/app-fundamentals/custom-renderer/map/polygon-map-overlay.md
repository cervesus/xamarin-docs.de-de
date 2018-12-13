---
title: Hervorheben eines Bereichs auf einer Karte
description: In diesem Artikel wird beschrieben, wie Sie einer Karte eine polygonale Überlagerung hinzufügen, um einen Bereich auf der Karte hervorzuheben. Polygone sind geschlossene ausgefüllte Formen.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998133"
---
# <a name="highlighting-a-region-on-a-map"></a>Hervorheben eines Bereichs auf einer Karte

_In diesem Artikel wurde beschrieben, wie Sie einer Karte eine polygonale Überlagerung hinzufügen, um einen Bereich auf der Karte hervorzuheben. Polygone sind geschlossene ausgefüllte Formen._

## <a name="overview"></a>Übersicht

Eine Überlagerung ist eine überlappende Grafik auf einer Karte. Überlagerungen unterstützen das Zeichnen grafischer Inhalte, die beim Zoomen mit der Karte skaliert werden. Die folgenden Screenshots zeigen polygonale Überlagerungen, die zu einer Karte hinzugefügt wurden:

![](polygon-map-overlay-images/screenshots.png)

Beim Rendern eines [`Map`](xref:Xamarin.Forms.Maps.Map)-Steuerelements durch eine Xamarin.Forms-App wird in iOS die `MapRenderer`-Klasse instanziiert, wodurch wiederum ein natives `MKMapView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `MapRenderer`-Klasse ein natives `MapView`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `MapRenderer`-Klasse eine native `MapControl`-Klasse. Der Renderingprozess kann genutzt werden, um plattformspezifische Kartenanpassungen zu implementieren, indem für eine `Map`-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#Creating_the_Custom_Map) Sie eine benutzerdefinierte Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) Sie die benutzerdefinierte Karte über Xamarin.Forms.
1. [Passen Sie](#Customizing_the_Map) die Karte durch Erstellen eines benutzerdefinierten Renderers für die Karte auf jeder Plattform an.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) muss vor der Verwendung initialisiert und konfiguriert werden. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Weitere Informationen zum Anpassen einer Karte mit einem benutzerdefinierten Renderer finden Sie unter [Customizing a Map Pin (Anpassen einer Kartenstecknadel)](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Erstellen der benutzerdefinierten Karte

Erstellen Sie eine Unterklasse der [`Map`](xref:Xamarin.Forms.Maps.Map)-Klasse, die eine `ShapeCoordinates`-Eigenschaft hinzufügt:

```csharp
public class CustomMap : Map
{
  public List<Position> ShapeCoordinates { get; set; }

  public CustomMap ()
  {
    ShapeCoordinates = new List<Position> ();
  }
}
```

Die `ShapeCoordinates`-Eigenschaft speichert eine Sammlung von Koordinaten, die den hervorzuhebenden Bereich definieren.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Durch diese Initialisierung werden eine Reihe von Breiten- und Längenkoordinaten bestimmt, um den auf der Karte hervorzuhebenden Bereich zu definieren. Dann wird die Kartenansicht mit der [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)-Methode positioniert, wodurch sich die Position und der Zoomfaktor der Karte ändern, indem eine [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)-Klasse aus einer [`Position`](xref:Xamarin.Forms.Maps.Position)- und einer [`Distance`](xref:Xamarin.Forms.Maps.Distance)-Struktur erstellt wird.

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Karte

Ein benutzerdefinierter Renderer muss nun zu jedem Projekt der Anwendung hinzugefügt werden, um die polygonale Überlagerung zur Karte hinzuzufügen.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren `OnElementChanged`-Methode, um die polygonale Überlagerung hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        MKPolygonRenderer polygonRenderer;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveOverlays(nativeMap.Overlays);
                    nativeMap.OverlayRenderer = null;
                    polygonRenderer = null;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;

                nativeMap.OverlayRenderer = GetOverlayRenderer;

                CLLocationCoordinate2D[] coords = new CLLocationCoordinate2D[formsMap.ShapeCoordinates.Count];

                int index = 0;
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coords[index] = new CLLocationCoordinate2D(position.Latitude, position.Longitude);
                    index++;
                }

                var blockOverlay = MKPolygon.FromCoordinates(coords);
                nativeMap.AddOverlay(blockOverlay);
            }
        }
        ...
    }
}

```

Diese Methode führt die folgende Konfiguration durch, vorausgesetzt der benutzerdefinierte Renderer wurde an ein neues Xamarin.Forms-Element angefügt:

- Die Eigenschaft `MKMapView.OverlayRenderer` wird auf einen entsprechenden Delegaten festgelegt.
- Die Sammlung von Breiten- und Längenkoordinaten wird aus der Eigenschaft `CustomMap.ShapeCoordinates` abgerufen und als Array von `CLLocationCoordinate2D`-Instanzen gespeichert.
- Das Polygon wird durch Aufrufen der statischen Methode `MKPolygon.FromCoordinates` erstellt, wodurch die Breite und Länge jedes Punkts angegeben wird.
- Das Polygon wird durch Aufrufen der Methode `MKMapView.AddOverlay` der Karte hinzugefügt. Diese Methode schließt das Polygon automatisch, indem eine Linie gezogen wird, die den ersten und den letzten Punkt miteinander verbindet.

Implementieren Sie dann die Methode `GetOverlayRenderer`, um das Rendering der Überlagerung anzupassen:

```csharp
public class CustomMapRenderer : MapRenderer
{
  MKPolygonRenderer polygonRenderer;
  ...

  MKOverlayRenderer GetOverlayRenderer(MKMapView mapView, IMKOverlay overlayWrapper)
  {
      if (polygonRenderer == null && !Equals(overlayWrapper, null)) {
          var overlay = Runtime.GetNSObject(overlayWrapper.Handle) as IMKOverlay;
          polygonRenderer = new MKPolygonRenderer(overlay as MKPolygon) {
              FillColor = UIColor.Red,
              StrokeColor = UIColor.Blue,
              Alpha = 0.4f,
              LineWidth = 9
          };
      }
      return polygonRenderer;
  }
}
```

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren Methoden `OnElementChanged` und `OnMapReady`, um die polygonale Überlagerung hinzuzufügen:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace MapOverlay.Droid
{
    public class CustomMapRenderer : MapRenderer
    {
        List<Position> shapeCoordinates;

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
                shapeCoordinates = formsMap.ShapeCoordinates;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(Android.Gms.Maps.GoogleMap map)
        {
            base.OnMapReady(map);

            var polygonOptions = new PolygonOptions();
            polygonOptions.InvokeFillColor(0x66FF0000);
            polygonOptions.InvokeStrokeColor(0x660000FF);
            polygonOptions.InvokeStrokeWidth(30.0f);

            foreach (var position in shapeCoordinates)
            {
                polygonOptions.Add(new LatLng(position.Latitude, position.Longitude));
            }
            NativeMap.AddPolygon(polygonOptions);
        }
    }
}
```

Die `OnElementChanged`-Methode ruft die Sammlung der Breiten- und Längenkoordinaten aus der Eigenschaft `CustomMap.ShapeCoordinates` ab und speichert sie in einer Membervariablen. Dann ruft sie die Methode `MapView.GetMapAsync` auf, die die zugrunde liegende `GoogleMap`-Klasse abruft, die an die Ansicht gebunden ist, sofern der benutzerdefinierte Renderer einem neuen Xamarin.Forms-Element angefügt wurde. Sobald die `GoogleMap`-Instanz verfügbar ist, wird die `OnMapReady`-Methode aufgerufen, wobei das Polygon durch Instanziieren eines `PolygonOptions`-Objekts erstellt wird, das die Breite und Länge jedes Punkts angibt. Das Polygon wird anschließend durch Aufrufen der Methode `NativeMap.AddPolygon` der Karte hinzugefügt. Diese Methode schließt das Polygon automatisch, indem eine Linie gezogen wird, die den ersten und den letzten Punkt miteinander verbindet.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen des benutzerdefinierten Renderers auf der Universellen Windows-Plattform

Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, und überschreiben Sie deren `OnElementChanged`-Methode, um die polygonale Überlagerung hinzuzufügen:

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
                foreach (var position in formsMap.ShapeCoordinates)
                {
                    coordinates.Add(new BasicGeoposition() { Latitude = position.Latitude, Longitude = position.Longitude });
                }

                var polygon = new MapPolygon();
                polygon.FillColor = Windows.UI.Color.FromArgb(128, 255, 0, 0);
                polygon.StrokeColor = Windows.UI.Color.FromArgb(128, 0, 0, 255);
                polygon.StrokeThickness = 5;
                polygon.Path = new Geopath(coordinates);
                nativeMap.MapElements.Add(polygon);
            }
        }
    }
}
```

Diese Methode führt die folgenden Vorgänge durch, vorausgesetzt der benutzerdefinierte Renderer wurde an ein neues Xamarin.Forms-Element angefügt:

- Die Sammlung von Breiten- und Längenkoordinaten wird aus der Eigenschaft `CustomMap.ShapeCoordinates` abgerufen und in eine `List`-Klasse mit `BasicGeoposition`-Koordinaten konvertiert.
- Das Polygon wird durch Instanziieren eines `MapPolygon`-Objekts erstellt. Die `MapPolygon`-Klasse wird zum Anzeigen einer Multipunktform auf der Karte verwendet, indem deren `Path`-Eigenschaft auf ein `Geopath`-Objekt festgelegt wird, das die Koordinaten der Form enthält.
- Der Kreis wird auf der Karte durch Hinzufügen zur Sammlung `MapControl.MapElements` gerendert. Das Polygon wird automatisch geschlossen, indem eine Linie gezogen wird, die den ersten und den letzten Punkt miteinander verbindet.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde beschrieben, wie Sie einer Karte eine polygonale Überlagerung hinzufügen, um einen Bereich auf der Karte hervorzuheben. Polygone sind geschlossene ausgefüllte Formen.


## <a name="related-links"></a>Verwandte Links

- [Polygon Map Overlay (sample) (Polygonale Kartenüberlagerung (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps Namespace (Xamarin.Forms.Maps-Namespace)](xref:Xamarin.Forms.Maps)
