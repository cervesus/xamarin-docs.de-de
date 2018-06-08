---
title: Hervorheben eines Bereichs auf einer Karte
description: In diesem Artikel wurde erläutert, wie einer Karte, markieren Sie eine Region auf der Karte eine Polygon-Überlagerung hinzugefügt wird. Polygone sind eine geschlossene Form, und ihre Innenbereiche ausgefüllt haben.
ms.prod: xamarin
ms.assetid: E79EB2CF-8DD6-44A8-B47D-5F0A94FB0A63
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: e0ffa1948bb7dd0996dd21793237df550a32aa70
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848329"
---
# <a name="highlighting-a-region-on-a-map"></a>Hervorheben eines Bereichs auf einer Karte

_In diesem Artikel wurde erläutert, wie einer Karte, markieren Sie eine Region auf der Karte eine Polygon-Überlagerung hinzugefügt wird. Polygone sind eine geschlossene Form, und ihre Innenbereiche ausgefüllt haben._

## <a name="overview"></a>Übersicht

Overlay ist eine geschichteten Grafik auf einer Karte. Overlays unterstützen grafische Zeichnungsinhalt, die mit der Zuordnung skaliert werden, da es vergrößert wird. Die folgenden Screenshots zeigen das Ergebnis der Addition einer Polygon-Überlagerung zu einer Zuordnung:

![](polygon-map-overlay-images/screenshots.png)

Wenn eine [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) einer Xamarin.Forms-Anwendung in iOS-Steuerelement gerendert wird die `MapRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf die universelle Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Des Renderingprozesses kann Vorteil ausgeführt werden, um Clientplattform-spezifische Zuordnung Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine `Map` auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Anpassen](#Customizing_the_Map) die Zuordnung durch das Erstellen eines benutzerdefinierten Renderers für die Zuordnung auf jeder Plattform.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

Informationen zum Anpassen einer Karte, die mithilfe eines benutzerdefinierten Renderers finden Sie unter [Anpassen einer Karte Pin](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md).

<a name="Creating_the_Custom_Map" />

### <a name="creating-the-custom-map"></a>Erstellen die benutzerdefinierte Karte

Erstellen Sie eine Unterklasse von der [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Klasse bereit, die Fügt eine `ShapeCoordinates` Eigenschaft:

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
    customMap.ShapeCoordinates.Add (new Position (37.797513, -122.402058));
    customMap.ShapeCoordinates.Add (new Position (37.798433, -122.402256));
    customMap.ShapeCoordinates.Add (new Position (37.798582, -122.401071));
    customMap.ShapeCoordinates.Add (new Position (37.797658, -122.400888));

    customMap.MoveToRegion (MapSpan.FromCenterAndRadius (new Position (37.79752, -122.40183), Distance.FromMiles (0.1)));
  }
}
```

Diese Initialisierung gibt eine Reihe von Breiten- und Längengrad Koordinaten, definieren Sie den Bereich der Karte, die hervorgehoben werden. Klicken Sie dann auf die Position der Kartenansicht mit der [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) -Methode, die ändert die Position und die Zoomstufe der Karte durch das Erstellen einer [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) aus einem [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) und ein [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

<a name="Customizing_the_Map" />

### <a name="customizing-the-map"></a>Anpassen der Zuordnung

Ein benutzerdefinierter Renderer muss jetzt jedes Anwendungsprojekt hinzufügen die Polygon-Überlagerung zu der Zuordnung hinzugefügt werden.

#### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` Methode, um das Polygon Overlay hinzuzufügen:

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

Diese Methode führt die folgende Konfiguration, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Die `MKMapView.OverlayRenderer` Eigenschaft auf einen entsprechenden Delegaten festgelegt ist.
- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.ShapeCoordinates` Eigenschaft und gespeichert als ein Array von `CLLocationCoordinate2D` Instanzen.
- Das Polygon wird erstellt, durch Aufrufen der statischen `MKPolygon.FromCoordinates` -Methode, die Breiten- und Längengrad der einzelnen Punkt angibt.
- Das Polygon ist an der Zuordnung hinzugefügt, indem die `MKMapView.AddOverlay` Methode. Diese Methode schließt automatisch das Polygon durch Zeichnen einer Linie, die die erste und letzte Punkt eine Verbindung herstellt.

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

#### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` und `OnMapReady` Methoden, um das Polygon Overlay hinzuzufügen:

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

Die `OnElementChanged` Methode ruft die Auflistung der Breiten- und Längenkoordinaten aus der `CustomMap.ShapeCoordinates` Eigenschaft und speichert sie in einer Membervariablen gespeichert. Er ruft dann die `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die an die Sicht gebunden ist, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Einmal die `GoogleMap` Instanz verfügbar ist, wird der `OnMapReady` Methode wird aufgerufen, wo das Polygon durch Instanziierung erstellt wird eine `PolygonOptions` Objekt, das die Breiten- und Längengrad der einzelnen Punkt angibt. Das Polygon wird dann zur Zuordnung hinzugefügt, durch Aufrufen der `NativeMap.AddPolygon` Methode. Diese Methode schließt automatisch das Polygon durch Zeichnen einer Linie, die die erste und letzte Punkt eine Verbindung herstellt.

#### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen von benutzerdefinierten Renderers für universelle Windows-Plattform

Erstellen Sie eine Unterklasse von der `MapRenderer` Klasse, und überschreiben die `OnElementChanged` Methode, um das Polygon Overlay hinzuzufügen:

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

Diese Methode führt die folgenden Vorgänge aus, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Die Auflistung der Breiten-und Längenkoordinaten werden abgerufen, von der `CustomMap.ShapeCoordinates` Eigenschaft und konvertierte in eine `List` von `BasicGeoposition` Koordinaten.
- Das Polygon durch Instanziierung erstellt eine `MapPolygon` Objekt. Die `MapPolygon` Klasse wird verwendet, um eine Form "multipunkteingabe" auf der Karte angezeigt, durch Festlegen seiner `Path` Eigenschaft, um eine `Geopath` Objekt, das die Koordinaten der Form "enthält.
- Das Polygon gerendert wird auf der Karte, hinzufügen zu der `MapControl.MapElements` Auflistung. Beachten Sie, dass das Polygon automatisch geschlossen wird, indem eine Linie, die die erste und letzte Punkt eine Verbindung herstellt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde erläutert, wie einer Karte, markieren Sie eine Region der Karte eine Polygon-Überlagerung hinzugefügt wird. Polygone sind eine geschlossene Form, und ihre Innenbereiche ausgefüllt haben.


## <a name="related-links"></a>Verwandte Links

- [Polygon Zuordnung Overlay (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/polygon/)
- [Anpassen einer Kartennadel](~/xamarin-forms/app-fundamentals/custom-renderer/map/customized-pin.md)
- [Xamarin.Forms.Maps](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/)
