---
title: Markieren einer Region auf einer Karte
description: In diesem Artikel wird erläutert, wie eine Karte, um einen Bereich auf der Karte markieren eine Polygon-Überlagerung hinzugefügt. Polygone werden eine geschlossene Form und ihre Innenbereiche ausgefüllt haben.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 0a11e9c25922531727ad2fee3bbed9c8d4e2b80c
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998133"
---
# <a name="highlighting-a-region-on-a-map"></a>Markieren einer Region auf einer Karte

_In diesem Artikel wurde erläutert, wie eine Karte, um einen Bereich auf der Karte markieren eine Polygon-Überlagerung hinzugefügt. Polygone werden eine geschlossene Form und ihre Innenbereiche ausgefüllt haben._

## <a name="overview"></a>Übersicht

Eine Überlagerung ist eine überlappende Grafik auf einer Karte. Überlagerungen unterstützen grafischen Zeichnungsinhalt, die mit der Zuordnung skaliert werden soll, wie es vergrößert wird. Die folgenden Screenshots zeigen das Ergebnis der Addition einer Polygon-Überlagerung zu einer Zuordnung an:

![](polygon-map-overlay-images/screenshots.png)

Wenn eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) -Steuerelements von einer Xamarin.Forms-Anwendung unter iOS die `MapRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Das Rendern zu erstellt werden kann nutzen plattformspezifische Zuordnung Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine `Map` auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Anpassen von](#Customizing_the_Map) der Zuordnung durch das Erstellen eines benutzerdefinierten Renderers für die Zuordnung auf jeder Plattform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informationen zum Anpassen einer Karte mithilfe eines benutzerdefinierten Renderers finden Sie unter [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Die benutzerdefinierte Karte erstellen

Erstellen Sie eine Unterklasse von der [ `Map` ](xref:Xamarin.Forms.Maps.Map) Klasse bereit, die Fügt eine `ShapeCoordinates` Eigenschaft:

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

Die `ShapeCoordinates` Eigenschaft speichert eine Auflistung von Koordinaten zur Definition der Region hervorgehoben werden.

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Diese Initialisierung gibt eine Reihe von Breiten- und Längengrad Koordinaten, definieren Sie die Region der Karte hervorgehoben werden. Klicken Sie dann auf die Position der Map-Ansicht mit den [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode, die ändert sich die Position und die Zoomstufe der Karte durch das Erstellen einer [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) aus einer [ `Position` ](xref:Xamarin.Forms.Maps.Position) und [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Karte

Ein benutzerdefinierter Renderer muss jetzt jedem Projekt der Anwendung auf das Overlay Polygon zur Karte hinzufügen hinzugefügt werden.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` Methode, um die Polygon-Überlagerung hinzuzufügen:

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

Diese Methode führt die folgende Konfiguration, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Die `MKMapView.OverlayRenderer` -Eigenschaftensatz auf einen entsprechenden Delegaten.
- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.ShapeCoordinates` Eigenschaft und als ein Array von gespeichert `CLLocationCoordinate2D` Instanzen.
- Das Polygon wird erstellt, durch Aufrufen der statischen `MKPolygon.FromCoordinates` -Methode, die den Breiten- und Längengrad der einzelnen Punkten angibt.
- Das Polygon zur Karte hinzugefügt wird, durch Aufrufen der `MKMapView.AddOverlay` Methode. Diese Methode schließt automatisch das Polygon durch Zeichnen einer Linie, die die ersten und letzten Punkte verbindet.

Implementieren Sie anschließend die `GetOverlayRenderer` Methode, um die Wiedergabe der Überlagerung anzupassen:

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

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` und `OnMapReady` Methoden, um die Polygon-Überlagerung hinzuzufügen:

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

Die `OnElementChanged` Methode ruft die Auflistung der Breiten- und Längenkoordinaten aus der `CustomMap.ShapeCoordinates` Eigenschaft und speichert sie in einer Membervariablen gespeichert. Es ruft dann die `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die an die Sicht gebunden ist, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Nach der `GoogleMap` Instanz verfügbar ist, die `OnMapReady` Methode wird aufgerufen, und, wo das Polygon durch Instanziierung erstellt wird eine `PolygonOptions` Objekt, das den Breiten- und Längengrad der einzelnen Punkten angibt. Das Polygon wird dann zur Karte hinzugefügt, durch den Aufruf der `NativeMap.AddPolygon` Methode. Diese Methode schließt automatisch das Polygon durch Zeichnen einer Linie, die die ersten und letzten Punkte verbindet.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen den benutzerdefinierten Renderer für die universelle Windows-Plattform

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben seine `OnElementChanged` Methode, um die Polygon-Überlagerung hinzuzufügen:

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

Diese Methode führt die folgenden Vorgänge, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.ShapeCoordinates` Eigenschaft und in konvertiert eine `List` von `BasicGeoposition` Koordinaten.
- Das Polygon wird erstellt, durch die Instanziierung einer `MapPolygon` Objekt. Die `MapPolygon` Klasse wird verwendet, um eine Form mit mehreren Punkten auf der Karte angezeigt, durch Festlegen der `Path` Eigenschaft, um eine `Geopath` -Objekt, das die Koordinaten der Form enthält.
- Das Polygon auf der Karte gerendert wird, indem sie zum Hinzufügen der `MapControl.MapElements` Auflistung. Beachten Sie, dass das Polygon automatisch geschlossen wird, indem eine Linie, die die ersten und letzten Punkte verbindet.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie eine Karte, um einen Bereich der Karte markieren eine Polygon-Überlagerung hinzugefügt. Polygone werden eine geschlossene Form und ihre Innenbereiche ausgefüllt haben.


## <a name="related-links"></a>Verwandte Links

- [Polygon Zuordnung überlagern (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](xref:Xamarin.Forms.Maps)
