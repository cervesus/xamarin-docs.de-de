---
title: Anpassen einer Kartennadel
description: In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für das Map-Steuerelement zu erstellen, die eine systemeigene Darstellung mit einer benutzerdefinierten Pin und eine angepasste Ansicht der Pin-Daten auf jeder Plattform angezeigt wird.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 4fee67f08e86c40709aa226c40c0f7721dc26800
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998303"
---
# <a name="customizing-a-map-pin"></a>Anpassen einer Kartennadel

_In diesem Artikel wird veranschaulicht, wie einen benutzerdefinierten Renderer für das Map-Steuerelement zu erstellen, die eine systemeigene Darstellung mit einer benutzerdefinierten Pin und eine angepasste Ansicht der Pin-Daten auf jeder Plattform angezeigt wird._

Alle Xamarin.Forms-Ansicht ist eine begleitende-Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) einer Xamarin.Forms-Anwendung unter iOS gerendert wird die `MapRenderer` Klasse instanziiert wird, die instanziiert wiederum eines systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf der universellen Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Weitere Informationen zu den Renderer und nativen Steuerelements-Klassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und Native Steuerelemente](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `Map` ](xref:Xamarin.Forms.Maps.Map) und die entsprechenden systemeigenen Steuerelemente, die sie implementieren:

![](customized-pin-images/map-classes.png "Beziehung zwischen dem Kartensteuerelement und die implementierenden Native Steuerelemente")

Das Rendern zu kann verwendet werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) auf jeder Plattform. Der Prozess hierfür lautet wie folgt aus:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierte Renderer für die Zuordnung auf jeder Plattform.

Jedes Element jetzt erläutert wiederum zum Implementieren einer `CustomMap` Renderer an, der eine systemeigene Darstellung mit einer benutzerdefinierten Pin und eine angepasste Ansicht der Pin-Daten auf jeder Plattform angezeigt.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Die benutzerdefinierte Karte erstellen

Eine benutzerdefinierte Karte-Steuerelement kann erstellt werden, indem Unterklassen der [ `Map` ](xref:Xamarin.Forms.Maps.Map) Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

Die `CustomMap` Steuerelement wird in .NET Standard Library-Projekt erstellt und definiert die API für die benutzerdefinierte Karte. Die benutzerdefinierte Karte stellt die `CustomPins` -Eigenschaft, die die Auflistung darstellt `CustomPin` Objekte, die von der systemeigenen Map-Steuerelement auf jeder Plattform gerendert werden. Die `CustomPin` ist im folgenden Codebeispiel dargestellt:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Diese Klasse definiert eine `CustomPin` als erben die Eigenschaften der [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) -Klasse und das Hinzufügen einer `Url` Eigenschaft.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Nutzen die benutzerdefinierte Karte

Die `CustomMap` Steuerelement kann verwiesen werden in XAML in .NET Standard Library-Projekt durch deklarieren einen Namespace für den Speicherort der und verwenden das Namespacepräfix für das Steuerelement benutzerdefinierte Karte. Das folgende Codebeispiel zeigt die `CustomMap` Steuerelement kann von einer XAML-Seite verwendet werden:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <ContentPage.Content>
        <local:CustomMap x:Name="myMap" MapType="Street"
          WidthRequest="{x:Static local:App.ScreenWidth}"
          HeightRequest="{x:Static local:App.ScreenHeight}" />
    </ContentPage.Content>
</ContentPage>
```

Die `local` Namespacepräfix kann eine beliebige Bezeichnung. Allerdings die `clr-namespace` und `assembly` Werte müssen die Details der benutzerdefinierten Zuordnung übereinstimmen. Nach der Namespace deklariert wird, ist das Präfix verwendet, um die benutzerdefinierte Zuordnung verweisen.

Das folgende Codebeispiel zeigt die `CustomMap` Steuerelement durch einen C# -Code genutzt werden kann:

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

Die `CustomMap` Instanz wird verwendet, um die native Zuordnung auf jeder Plattform anzuzeigen. Sie verfügt über [ `MapType` ](xref:Xamarin.Forms.Maps.Map.MapType) Eigenschaftensätze den Anzeigestil der der [ `Map` ](xref:Xamarin.Forms.Maps.Map), mit den möglichen Werten definiert wird, der [ `MapType` ](xref:Xamarin.Forms.Maps.MapType) Enumeration. Für iOS und Android, die Breite und Höhe der Karte wird festgelegt durch Eigenschaften der `App` -Klasse, die in den plattformspezifischen Projekten initialisiert werden.

Der Speicherort der Zuordnung und die Pins enthält, werden initialisiert, wie im folgenden Codebeispiel gezeigt:

```csharp
public MapPage ()
{
  ...
  var pin = new CustomPin {
    Type = PinType.Place,
    Position = new Position (37.79752, -122.40183),
    Label = "Xamarin San Francisco Office",
    Address = "394 Pacific Ave, San Francisco CA",
    Id = "Xamarin",
    Url = "http://xamarin.com/about/"
  };

  customMap.CustomPins = new List<CustomPin> { pin };
  customMap.Pins.Add (pin);
  customMap.MoveToRegion (MapSpan.FromCenterAndRadius (
    new Position (37.79752, -122.40183), Distance.FromMiles (1.0)));
}
```

Diese Initialisierung Fügt eine benutzerdefinierte Pin und positioniert die Map-Ansicht mit den [ `MoveToRegion` ](xref:Xamarin.Forms.Maps.Map.MoveToRegion*) -Methode, die ändert sich die Position und die Zoomstufe der Karte durch das Erstellen einer [ `MapSpan` ](xref:Xamarin.Forms.Maps.MapSpan) aus einem [ `Position` ](xref:Xamarin.Forms.Maps.Position) und [ `Distance` ](xref:Xamarin.Forms.Maps.Distance).

Ein benutzerdefinierter Renderer kann jetzt jedem Projekt der Anwendung zum Anpassen der systemeigenen kartensteuerelemente hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Renderer-Klasse sieht folgendermaßen aus:

1. Erstellen Sie eine Unterklasse von der `MapRenderer` -Klasse, die die benutzerdefinierte Karte gerendert wird.
1. Überschreiben der `OnElementChanged` -Methode, die benutzerdefinierte Karte und schreiblogik zur Anpassung rendert. Diese Methode wird aufgerufen, wenn die entsprechende benutzerdefinierte Xamarin.Forms-Karte erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut der benutzerdefinierten Renderer-Klasse, um anzugeben, dass er zum Rendern der benutzerdefinierten Xamarin.Forms-Zuordnung verwendet wird. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Dies ist optional, um einen benutzerdefinierten Renderer in jedem plattformprojekt bereitzustellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standard-Renderer für die Basisklasse des Steuerelements verwendet werden.

Das folgende Diagramm veranschaulicht die Verantwortlichkeiten der einzelnen Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](customized-pin-images/solution-structure.png "Zuständigkeiten von CustomMap benutzerdefinierten Renderer-Projekt")

Die `CustomMap` -Steuerelements durch plattformspezifische Renderingklassen, die `MapRenderer` -Klasse für jede Plattform. Dies führt in jeder `CustomMap` Steuerelements plattformspezifische Steuerelemente gerendert wird, wie in den folgenden Screenshots dargestellt:

![](customized-pin-images/screenshots.png "CustomMap auf jeder Plattform")

Die `MapRenderer` -Klasse macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn die benutzerdefinierte Xamarin.Forms-Karte erstellt wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert eine `ElementChangedEventArgs` Parameter mit `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, die den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, die den Renderer *ist* angefügt wird, bzw. In diesem Beispiel die `OldElement` Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `CustomMap` Instanz.

Eine außer Kraft gesetzte Version von der `OnElementChanged` -Methode, in den einzelnen plattformspezifischen rendererklassen an, ist der Ort für die Anpassung von systemeigenen Steuerelementen führen. Ein typisierter Verweis auf das native Steuerelement verwendet wird, auf der Plattform möglich über die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

Vorsicht beim Abonnieren von Ereignishandlern in die `OnElementChanged` -Methode, wie im folgenden Codebeispiel gezeigt:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.ListView> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null) {
    // Unsubscribe from event handlers
  }

  if (e.NewElement != null) {
    // Configure the native control and subscribe to event handlers
  }
}
```

Das native Steuerelement konfiguriert werden, und Ereignishandler abonniert werden, nur, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Ebenso sollten alle Ereignishandler, die abonniert wurden gekündigt werden nur, wenn das Element, dem der Renderer angefügt ist ändert. Diese Vorgehensweise können Sie einen benutzerdefinierten Renderer erstellen, der durch Speicherverluste beeinträchtigt nicht.

Mit jeder benutzerdefinierten Renderer-Klasse ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das-Attribut nimmt zwei Parameter: den Typnamen des benutzerdefinierten Xamarin.Forms-Steuerelements gerendert wird, und der Typname des benutzerdefinierten Renderers. Die `assembly` Präfix, das das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

Den folgenden Abschnitten werden die Implementierung der einzelnen plattformspezifischen benutzerdefinierten Renderer-Klasse.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen den benutzerdefinierten Renderer für iOS

Die folgenden Screenshots zeigen die Zuordnung, vor und nach der Anpassung:

![](customized-pin-images/map-layout-ios.png "Map-Steuerelement vor und nach der Anpassung")

Unter iOS die Pin wird aufgerufen, eine *Anmerkung*, und kann entweder ein benutzerdefiniertes Image oder eine Pin systemdefinierte verschiedener Farben. Anmerkungen können optional Anzeigen einer *Legende*, die in der Antwort an den Benutzer, der die Anmerkung auswählen angezeigt. Die Legende zeigt die `Label` und `Address` Eigenschaften der `Pin` -Instanz, mit optionalen links und rechts Zubehör Ansichten. Im obigen Screenshot ist die Ansicht von linke Zubehör das Abbild des Monkey-Objekt, mit der rechten Zubehör anzeigen, die *Informationen* Schaltfläche.

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die iOS-Plattform:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.iOS
{
    public class CustomMapRenderer : MapRenderer
    {
        UIView customPinView;
        List<CustomPin> customPins;

        protected override void OnElementChanged(ElementChangedEventArgs<View> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null) {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null) {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null) {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        ...
    }
}
```

Die `OnElementChanged` Methode führt die folgende Konfiguration des der [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) -Instanz, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Die [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) -Eigenschaftensatz auf die `GetViewForAnnotation` Methode. Diese Methode wird aufgerufen, wenn die [wird auf der Karte angezeigt, die Position der Anmerkung](#Displaying_the_Annotation), und wird verwendet, um die Anmerkung vor der Anzeige anpassen.
- Ereignishandler für die `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, und `DidDeselectAnnotationView` Ereignisse registriert werden. Diese Ereignisse werden ausgelöst, wenn der Benutzer [tippt auf den richtigen Zubehör in der Legende](#Tapping_on_the_Right_Callout_Accessory_View), und wenn der Benutzer [wählt](#Selecting_the_Annotation) und [hebt die Auswahl](#Deselecting_the_Annotation) die Anmerkung wird bzw. Die Ereignisse sind nicht mehr abonniert, nur, wenn das Element der Renderer Änderungen zugeordnet ist.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Die Anmerkung anzeigen

Die `GetViewForAnnotation` Methode wird aufgerufen, wenn die Position der Anmerkung auf der Karte angezeigt wird, und verwendet wird, um die Anmerkung vor der Anzeige anpassen. Eine Anmerkung besteht aus zwei Teilen:

- `MkAnnotation` : enthält den Titel, Untertitel und die Position der Anmerkung.
- `MkAnnotationView` – enthält das Bild zur Darstellung der Anmerkung, und optional eine Legende, die angezeigt wird, wenn der Benutzer auf die Anmerkung tippt.

Die `GetViewForAnnotation` -Methode akzeptiert eine `IMKAnnotation` , die der Anmerkung Daten enthält, und gibt eine `MKAnnotationView` für die Anzeige auf der Karte, und wird im folgenden Codebeispiel wird angezeigt:

```csharp
MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null) {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Id.ToString());
    if (annotationView == null) {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Id.ToString());
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Id = customPin.Id.ToString();
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Diese Methode wird sichergestellt, dass die Anmerkung wird als benutzerdefiniertes Image angezeigt werden, anstatt als systemdefinierte Pin, und dass die Anmerkung getippt wird eine Legende angezeigt, die zusätzliche Inhalte, links und rechts von der Anmerkungstitel und die Adresse enthält. . Dies wird wie folgt erreicht:

1. Die `GetCustomPin` aufgerufen, um die benutzerdefinierte Pin-Daten für die Anmerkung zurück.
1. Um Arbeitsspeicher zu sparen, wird der Anmerkung Ansicht für die Wiederverwendung durch den Aufruf von zusammengefasst [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. Die `CustomMKAnnotationView` -Klasse erweitert die `MKAnnotationView` Klasse mit `Id` und `Url` Eigenschaften, die identische Eigenschaften entsprechen, den `CustomPin` Instanz. Eine neue Instanz der dem `CustomMKAnnotationView` wird erstellt, vorausgesetzt, dass die Anmerkung ist `null`:
  - Die `CustomMKAnnotationView.Image` -Eigenschaftensatz auf das Bild, das die Anmerkung in der Zuordnung dargestellt wird.
  - Die `CustomMKAnnotationView.CalloutOffset` -Eigenschaftensatz auf eine `CGPoint` , der angibt, dass die Legende oberhalb der Anmerkung zentriert werden soll.
  - Die `CustomMKAnnotationView.LeftCalloutAccessoryView` -Eigenschaftensatz auf ein Abbild von einem Monkey-Objekt, das auf der linken Seite der Anmerkungstitel und Adresse angezeigt wird.
  - Die `CustomMKAnnotationView.RightCalloutAccessoryView` -Eigenschaftensatz auf eine *Informationen* Schaltfläche, die rechts neben den Anmerkungstitel und die Adresse angezeigt werden.
  - Die `CustomMKAnnotationView.Id` -Eigenschaftensatz auf die `CustomPin.Id` von zurückgegebene Eigenschaft der `GetCustomPin` Methode. Dies ermöglicht die Anmerkung identifiziert werden, sodass sie [Legende kann angepasst werden, weitere](#Selecting_the_Annotation), falls gewünscht.
  - Die `CustomMKAnnotationView.Url` -Eigenschaftensatz auf die `CustomPin.Url` von zurückgegebene Eigenschaft der `GetCustomPin` Methode. Der URL wird, navigiert werden, wenn der Benutzer [tippt auf die Schaltfläche wird angezeigt, in der richtigen Legende Zubehör Ansicht](#Tapping_on_the_Right_Callout_Accessory_View).
1. Die [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) -Eigenschaftensatz auf `true` , damit die Legende angezeigt wird, wenn die Anmerkung angetippt wird.
1. Die Anmerkung wird auf der Karte für die Anzeige zurückgegeben.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Die Anmerkung auswählen

Wenn der Benutzer, auf die Anmerkung tippt, die `DidSelectAnnotationView` Ereignis wird ausgelöst, das wiederum führt die `OnDidSelectAnnotationView` Methode:

```csharp
void OnDidSelectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  customPinView = new UIView ();

  if (customView.Id == "Xamarin") {
    customPinView.Frame = new CGRect (0, 0, 200, 84);
    var image = new UIImageView (new CGRect (0, 0, 200, 84));
    image.Image = UIImage.FromFile ("xamarin.png");
    customPinView.AddSubview (image);
    customPinView.Center = new CGPoint (0, -(e.View.Frame.Height + 75));
    e.View.AddSubview (customPinView);
  }
}
```

Diese Methode erweitert die vorhandene Legende (der linke und rechten Zubehör Ansichten enthält) durch das Hinzufügen einer `UIView` -Instanz darin, ein Abbild von der Xamarin-Logo, enthält, vorausgesetzt, dass die ausgewählte Anmerkung verfügt über, seine `Id` -Eigenschaft auf `Xamarin`. Dies ermöglicht Szenarien, in denen unterschiedliche Legenden für andere Anmerkungen angezeigt werden können. Die `UIView` Instanz wird über die vorhandenen Legende zentriert angezeigt werden.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Tippen in der Ansicht rechts Callout-Zubehör

Wenn der Benutzer tippt in der *Informationen* Schaltfläche in der richtigen Legende Zubehör Ansicht der `CalloutAccessoryControlTapped` Ereignis wird ausgelöst, das wiederum führt die `OnCalloutAccessoryControlTapped` Methode:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Diese Methode öffnet einen Webbrowser und navigiert zu der Adresse gespeichert, der `CustomMKAnnotationView.Url` Eigenschaft. Beachten Sie, dass die Adresse, beim Erstellen definiert wurde der `CustomPin` Collection in .NET Standard Library-Projekt.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Deaktivieren die Anmerkung

Wenn die Anmerkung angezeigt wird, und der Benutzer in der Zuordnung tippt der `DidDeselectAnnotationView` Ereignis wird ausgelöst, das wiederum führt die `OnDidDeselectAnnotationView` Methode:

```csharp
void OnDidDeselectAnnotationView (object sender, MKAnnotationViewEventArgs e)
{
  if (!e.View.Selected) {
    customPinView.RemoveFromSuperview ();
    customPinView.Dispose ();
    customPinView = null;
  }
}
```

Diese Methode wird sichergestellt, wenn die vorhandene Legende nicht ausgewählt ist, der erweiterten Teil der Legende (das Bild des Logos Xamarin) wird auch beendet angezeigt wird und seine Ressourcen freigegeben werden werden.

Weitere Informationen zum Anpassen einer `MKMapView` Instanz ist, finden Sie unter [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen den benutzerdefinierten Renderer für Android

Die folgenden Screenshots zeigen die Zuordnung, vor und nach der Anpassung:

![](customized-pin-images/map-layout-android.png "Map-Steuerelement vor und nach der Anpassung")

Unter Android die Pin wird aufgerufen, eine *Marker*, und kann entweder ein benutzerdefiniertes Image oder ein Marker systemdefinierte verschiedener Farben. Marker Anzeigen einer *Infofenster*, die in der Antwort an den Benutzer Tippen auf dem Marker angezeigt. Zeigt das Fenster "Info" der `Label` und `Address` Eigenschaften der `Pin` Serverinstanz aus, und kann angepasst werden, um andere Inhalte einzuschließen. Gleichzeitig kann jedoch nur ein Fenster "Info" angezeigt werden.

Das folgende Codebeispiel zeigt den benutzerdefinierten Renderer für die Android-Plattform:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.Droid
{
    public class CustomMapRenderer : MapRenderer, GoogleMap.IInfoWindowAdapter
    {
        List<CustomPin> customPins;

        public CustomMapRenderer(Context context) : base(context)
        {
        }

        protected override void OnElementChanged(Xamarin.Forms.Platform.Android.ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                NativeMap.InfoWindowClick -= OnInfoWindowClick;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                customPins = formsMap.CustomPins;
                Control.GetMapAsync(this);
            }
        }

        protected override void OnMapReady(GoogleMap map)
        {
            base.OnMapReady(map);

            NativeMap.InfoWindowClick += OnInfoWindowClick;
            NativeMap.SetInfoWindowAdapter(this);
        }
        ...
    }
}
```

Vorausgesetzt, dass der benutzerdefinierte Renderer, um ein neues Xamarin.Forms-Element angefügt ist, das `OnElementChanged` Methodenaufrufe der `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die an die Sicht gebunden ist. Nach der `GoogleMap` Instanz verfügbar ist, die `OnMapReady` Außerkraftsetzung aufgerufen werden. Diese Methode registriert einen Ereignishandler für die `InfoWindowClick` Ereignis, das ausgelöst, wenn wird die [Fenster "Info" geklickt wird](#Clicking_on_the_Info_Window), und aus Abonnement gekündigt wird nur verwendet werden, wenn das Element der Renderer an Änderungen angefügt ist. Die `OnMapReady` überschreiben auch die Aufrufe der `SetInfoWindowAdapter` Methode, um anzugeben, dass die `CustomMapRenderer` Klasseninstanz bieten die Methoden, um das Fenster "Info" anpassen.

Die `CustomMapRenderer` -Klasse implementiert die `GoogleMap.IInfoWindowAdapter` -Schnittstelle [passen Sie das Fenster "Info"](#Customizing_the_Info_Window). Diese Schnittstelle gibt an, dass die folgenden Methoden implementiert werden müssen:

- `public Android.Views.View GetInfoWindow(Marker marker)` – Diese Methode wird aufgerufen, um ein Informationsfenster benutzerdefinierte für einen Marker zurück. Wenn sie zurückgibt `null`, und klicken Sie dann die Standardanzeige für Fenster verwendet wird. Wenn zurückgegeben wird ein `View`, klicken Sie dann, `View` innerhalb des Info-Fensterrahmens platziert werden.
- `public Android.Views.View GetInfoContents(Marker marker)` – Diese Methode wird aufgerufen, um zurückzukehren eine `View` , die den Inhalt des Fensters Informationen enthält, und wird nur aufgerufen werden, wenn die `GetInfoWindow` Methodenrückgabe `null`. Wenn sie zurückgibt `null`, und klicken Sie dann die standarddarstellung des Inhalts der Info-Fensters verwendet wird.

In der beispielanwendung, nur die Informationen im Fensterinhalt angepasst wird, muss das `GetInfoWindow` Methodenrückgabe `null` um dies zu ermöglichen.

#### <a name="customizing-the-marker"></a>Die Markierung anpassen

Das Symbol zur Darstellung der Marker kann angepasst werden, durch den Aufruf der `MarkerOptions.SetIcon` Methode. Dies kann erreicht werden, durch Überschreiben der `CreateMarker` -Methode, die aufgerufen wird, für die einzelnen `Pin` zur Karte hinzugefügt wird:

```csharp
protected override MarkerOptions CreateMarker(Pin pin)
{
    var marker = new MarkerOptions();
    marker.SetPosition(new LatLng(pin.Position.Latitude, pin.Position.Longitude));
    marker.SetTitle(pin.Label);
    marker.SetSnippet(pin.Address);
    marker.SetIcon(BitmapDescriptorFactory.FromResource(Resource.Drawable.pin));
    return marker;
}
```

Diese Methode erstellt ein neues `MarkerOption` Instanz für jedes `Pin` Instanz. Nach dem Festlegen der Position, die Bezeichnung und die Adresse des Markers an, das entsprechende Symbol festgelegt ist, mit der `SetIcon` Methode. Diese Methode verwendet eine `BitmapDescriptor` Objekt mit den Daten, die zum Rendern des Symbols mit der `BitmapDescriptorFactory` Klasse Hilfsmethoden zur Vereinfachung der Erstellung der Bereitstellung der `BitmapDescriptor`.

Weitere Informationen zur Verwendung der `BitmapDescriptorFactory` Klasse, um eine Markierung anpassen, finden Sie unter [Anpassen einen Marker](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Anpassen der Fenster "Info"

Wenn ein Benutzer, auf den Marker fest tippt, die `GetInfoContents` Methode ausgeführt wird, werden bereitgestellt, die die `GetInfoWindow` Methodenrückgabe `null`. Das folgende Codebeispiel zeigt die `GetInfoContents` Methode:

```csharp
public Android.Views.View GetInfoContents (Marker marker)
{
  var inflater = Android.App.Application.Context.GetSystemService (Context.LayoutInflaterService) as Android.Views.LayoutInflater;
  if (inflater != null) {
    Android.Views.View view;

    var customPin = GetCustomPin (marker);
    if (customPin == null) {
      throw new Exception ("Custom pin not found");
    }

    if (customPin.Id.ToString() == "Xamarin") {
      view = inflater.Inflate (Resource.Layout.XamarinMapInfoWindow, null);
    } else {
      view = inflater.Inflate (Resource.Layout.MapInfoWindow, null);
    }

    var infoTitle = view.FindViewById<TextView> (Resource.Id.InfoWindowTitle);
    var infoSubtitle = view.FindViewById<TextView> (Resource.Id.InfoWindowSubtitle);

    if (infoTitle != null) {
      infoTitle.Text = marker.Title;
    }
    if (infoSubtitle != null) {
      infoSubtitle.Text = marker.Snippet;
    }

    return view;
  }
  return null;
}
```

Diese Methode gibt eine `View` mit dem Inhalt des Fensters Informationen. Dies wird wie folgt erreicht:

- Ein `LayoutInflater` Instanz abgerufen wird. Wird verwendet, um eine Layout-XML-Datei in das entsprechende instanziieren `View`.
- Die `GetCustomPin` aufgerufen, um die benutzerdefinierte Pin-Daten für das Fenster "Info" zurückgeben.
- Die `XamarinMapInfoWindow` Layout wird vergrößert, wenn die `CustomPin.Id` Eigenschaft `Xamarin`. Andernfalls die `MapInfoWindow` Layout vergrößert wird. Dies ermöglicht Szenarien, in dem anderen Informationen Fensterlayouts für verschiedene Marker angezeigt werden können.
- Die `InfoWindowTitle` und `InfoWindowSubtitle` Ressourcen werden abgerufen, aus dem Layout der zunehmen, und ihre `Text` Eigenschaften festgelegt werden, um die entsprechenden Daten aus der `Marker` -Instanz, vorausgesetzt, dass die Ressourcen nicht sind `null`.
- Die `View` Instanz wird für die Anzeige zurückgegeben, auf der Karte.

> [!NOTE]
> Ein Fenster "Info" ist nicht live `View`. Stattdessen wird Android konvertiert die `View` auf ein statisches bitmap und anzuzeigen, die als Image. Dies bedeutet, die bei der ein Fenster "Info" auf ein Click-Ereignis reagieren können, reagiert er kann nicht auf alle Touch-Ereignissen oder Gesten, und die einzelnen Steuerelemente in das Fenster "Info" können nicht mit ihrer eigenen Antworten click-Ereignisse.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Durch Klicken auf das Fenster "Info"

Klickt der Benutzer die Informationen im Fenster der `InfoWindowClick` Ereignis wird ausgelöst, das wiederum führt die `OnInfoWindowClick` Methode:

```csharp
void OnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
  var customPin = GetCustomPin (e.Marker);
  if (customPin == null) {
    throw new Exception ("Custom pin not found");
  }

  if (!string.IsNullOrWhiteSpace (customPin.Url)) {
    var url = Android.Net.Uri.Parse (customPin.Url);
    var intent = new Intent (Intent.ActionView, url);
    intent.AddFlags (ActivityFlags.NewTask);
    Android.App.Application.Context.StartActivity (intent);
  }
}
```

Diese Methode öffnet einen Webbrowser und navigiert zu die Adresse, die in gespeicherten der `Url` Eigenschaft des abgerufenen `CustomPin` Instanz, für die `Marker`. Beachten Sie, dass die Adresse, beim Erstellen definiert wurde der `CustomPin` Collection in .NET Standard Library-Projekt.

Weitere Informationen zum Anpassen einer `MapView` Instanz ist, finden Sie unter [Maps-API](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen den benutzerdefinierten Renderer für die universelle Windows-Plattform

Die folgenden Screenshots zeigen die Zuordnung, vor und nach der Anpassung:

![](customized-pin-images/map-layout-uwp.png "Map-Steuerelement vor und nach der Anpassung")

In UWP heißt die Pin ein *Kartensymbol*, und kann entweder ein benutzerdefiniertes Image oder systemdefinierte Standardbilds sein. Ein Symbol zuordnen kann anzeigen, eine `UserControl`, die in der Antwort an den Benutzer Tippen auf das Symbol "Zuordnung" angezeigt. Die `UserControl` können Inhalte anzeigen, einschließlich der `Label` und `Address` Eigenschaften der `Pin` Instanz.

Im folgenden Codebeispiel wird veranschaulicht, den benutzerdefinierten Renderer für UWP:

```csharp
[assembly: ExportRenderer(typeof(CustomMap), typeof(CustomMapRenderer))]
namespace CustomRenderer.UWP
{
    public class CustomMapRenderer : MapRenderer
    {
        MapControl nativeMap;
        List<CustomPin> customPins;
        XamarinMapOverlay mapOverlay;
        bool xamarinOverlayShown = false;

        protected override void OnElementChanged(ElementChangedEventArgs<Map> e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null)
            {
                nativeMap.MapElementClick -= OnMapElementClick;
                nativeMap.Children.Clear();
                mapOverlay = null;
                nativeMap = null;
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                nativeMap = Control as MapControl;
                customPins = formsMap.CustomPins;

                nativeMap.Children.Clear();
                nativeMap.MapElementClick += OnMapElementClick;

                foreach (var pin in customPins)
                {
                    var snPosition = new BasicGeoposition { Latitude = pin.Pin.Position.Latitude, Longitude = pin.Pin.Position.Longitude };
                    var snPoint = new Geopoint(snPosition);

                    var mapIcon = new MapIcon();
                    mapIcon.Image = RandomAccessStreamReference.CreateFromUri(new Uri("ms-appx:///pin.png"));
                    mapIcon.CollisionBehaviorDesired = MapElementCollisionBehavior.RemainVisible;
                    mapIcon.Location = snPoint;
                    mapIcon.NormalizedAnchorPoint = new Windows.Foundation.Point(0.5, 1.0);

                    nativeMap.MapElements.Add(mapIcon);                    
                }
            }
        }
        ...
    }
}
```

Die `OnElementChanged` Methode führt die folgenden Vorgänge aus, vorausgesetzt, dass der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist:

- Löscht die `MapControl.Children` -Auflistung, um alle vorhandenen Elemente der Benutzeroberfläche aus der Zuordnung zu entfernen, bevor Sie die Registrierung eines ereignishandlers für die `MapElementClick` Ereignis. Dieses Ereignis wird ausgelöst, wenn der Benutzer tippt oder klickt auf eine `MapElement` auf die `MapControl`, und aus Abonnement gekündigt wird nur verwendet werden, wenn das Element der Renderer an Änderungen angefügt ist.
- Jeder Pin in der `customPins` Auflistung wie folgt an den richtigen geografischen Standort auf der Karte angezeigt wird:
  - Der Speicherort für die Pin wird als erstellt eine `Geopoint` Instanz.
  - Ein `MapIcon` Instanz wird erstellt, um die Pin darstellen.
  - Das Bild darstellen, die `MapIcon` wird angegeben, indem die `MapIcon.Image` Eigenschaft. Allerdings ist die Map-Symbol nicht unbedingt immer angezeigt werden, wie es von anderen Elementen auf der Karte verdeckt werden. Aus diesem Grund die Zuordnung des Symbols `CollisionBehaviorDesired` -Eigenschaftensatz auf `MapElementCollisionBehavior.RemainVisible`, um sicherzustellen, dass es sichtbar bleibt.
  - Den Speicherort der `MapIcon` wird angegeben, indem die `MapIcon.Location` Eigenschaft.
  - Die `MapIcon.NormalizedAnchorPoint` -Eigenschaftensatz auf die ungefähre Position des Zeigers auf das Bild. Wenn diese Eigenschaft den Standardwert von (0,0), der oben links des Bilds darstellt behält, möglicherweise Änderungen an die Zoomstufe der Karte in der Abbildung, die auf einem anderen Speicherort verweist.
  - Die `MapIcon` Instanz wird hinzugefügt, um die `MapControl.MapElements` Auflistung. Dies führt zu dem Kartensymbol angezeigt wird, auf die `MapControl`.

> [!NOTE]
> Bei Verwendung desselben Images für mehrere Map-Symbole, die `RandomAccessStreamReference` Instanz sollte für eine optimale Leistung auf der Seite oder Anwendung deklariert werden.

#### <a name="displaying-the-usercontrol"></a>Anzeigen von UserControl

Wenn ein Benutzer auf das Symbol "Zuordnung" tippt der `OnMapElementClick` Methode ausgeführt wird. Das folgende Codebeispiel zeigt diese Methode:

```csharp
private void OnMapElementClick(MapControl sender, MapElementClickEventArgs args)
{
    var mapIcon = args.MapElements.FirstOrDefault(x => x is MapIcon) as MapIcon;
    if (mapIcon != null)
    {
        if (!xamarinOverlayShown)
        {
            var customPin = GetCustomPin(mapIcon.Location.Position);
            if (customPin == null)
            {
                throw new Exception("Custom pin not found");
            }

            if (customPin.Id.ToString() == "Xamarin")
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Pin.Position.Latitude, Longitude = customPin.Pin.Position.Longitude };
                var snPoint = new Geopoint(snPosition);

                nativeMap.Children.Add(mapOverlay);
                MapControl.SetLocation(mapOverlay, snPoint);
                MapControl.SetNormalizedAnchorPoint(mapOverlay, new Windows.Foundation.Point(0.5, 1.0));
                xamarinOverlayShown = true;
            }
        }
        else
        {
            nativeMap.Children.Remove(mapOverlay);
            xamarinOverlayShown = false;
        }
    }
}
```

Diese Methode erstellt eine `UserControl` -Instanz, die Informationen über die Pin angezeigt. Dies wird wie folgt erreicht:

- Die `MapIcon` Instanz abgerufen wird.
- Die `GetCustomPin` aufgerufen, um die benutzerdefinierte Pin-Daten zurückgeben, die angezeigt werden.
- Ein `XamarinMapOverlay` Instanz wird erstellt, um die benutzerdefinierte Pin-Daten anzuzeigen. Diese Klasse ist ein Benutzersteuerelement.
- Den geografischen Standort an, die angezeigt werden sollen die `XamarinMapOverlay` Instanz in der `MapControl` wird als erstellt eine `Geopoint` Instanz.
- Die `XamarinMapOverlay` Instanz wird hinzugefügt, um die `MapControl.Children` Auflistung. Diese Sammlung enthält XAML-Benutzeroberflächenelemente, die auf der Karte angezeigt werden.
- Der geografische Standort der `XamarinMapOverlay` Instanz, auf der Karte wird festgelegt, durch den Aufruf der `SetLocation` Methode.
- Der relative Speicherort in der `XamarinMapOverlay` Instanz, die der angegebenen Position entspricht, wird festgelegt, durch den Aufruf der `SetNormalizedAnchorPoint` Methode. Dadurch wird sichergestellt, die Änderungen in den Zoomfaktor der das Ergebnis der Zuordnung in der `XamarinMapOverlay` Instanz immer am richtigen Speicherort angezeigt wird.

Auch wenn Informationen über die Pin bereits auf der Karte angezeigt wird, tippen Sie auf der Karte entfernt die `XamarinMapOverlay` -Instanz aus der `MapControl.Children` Auflistung.

#### <a name="tapping-on-the-information-button"></a>Tippen auf die Schaltfläche "Information"

Wenn der Benutzer tippt auf die *Informationen* Schaltfläche der `XamarinMapOverlay` Benutzersteuerelement, das `Tapped` Ereignis wird ausgelöst, das wiederum führt die `OnInfoButtonTapped` Methode:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Diese Methode öffnet einen Webbrowser und navigiert zu der Adresse gespeichert, der `Url` Eigenschaft der `CustomPin` Instanz. Beachten Sie, dass die Adresse, beim Erstellen definiert wurde der `CustomPin` Collection in .NET Standard Library-Projekt.

Weitere Informationen zum Anpassen einer `MapControl` Instanz ist, finden Sie unter [Karten und Übersicht über die Position](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) auf MSDN.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das `Map` -Steuerelement ermöglicht es Entwicklern, native standarddarstellung mit ihren eigenen plattformspezifische Anpassungen zu überschreiben. Xamarin.Forms.Maps bietet eine plattformübergreifende Abstraktion für die Anzeige von Karten, die die native Zuordnung APIs auf jeder Plattform zu einer Karte schnell und vertraute Erfahrung für Benutzer verwenden.


## <a name="related-links"></a>Verwandte Links

- [Maps-Steuerelement](~/xamarin-forms/user-interface/map.md)
- [iOS-Zuordnungen](~/ios/user-interface/controls/ios-maps/index.md)
- [Karten-API](~/android/platform/maps-and-location/maps/maps-api.md)
- [Benutzerdefinierte Pin (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
