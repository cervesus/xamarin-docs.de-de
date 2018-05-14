---
title: Anpassen einer Karte-Pin
description: Dieser Artikel veranschaulicht, wie ein benutzerdefiniertes Renderers für das Kartensteuerelement, dem eine systemeigene Darstellung mit einer angepassten Pin "und" eine angepasste Ansicht der Pin-Daten auf jeder Plattform angezeigt.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/29/2017
ms.openlocfilehash: 04d3d99a5d85dd77c93e9b926e8952cc3d8a771e
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="customizing-a-map-pin"></a>Anpassen einer Karte-Pin

_Dieser Artikel veranschaulicht, wie ein benutzerdefiniertes Renderers für das Kartensteuerelement, dem eine systemeigene Darstellung mit einer angepassten Pin "und" eine angepasste Ansicht der Pin-Daten auf jeder Plattform angezeigt._

Jede Ansicht Xamarin.Forms verfügt über eine begleitende Renderer für jede Plattform, die eine Instanz eines systemeigenen Steuerelements erstellt. Wenn eine [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) einer Xamarin.Forms-Anwendung in iOS, gerendert wird die `MapRenderer` Klasse instanziiert, die instanziiert wiederum ein systemeigenes `MKMapView` Steuerelement. Auf der Android-Plattform die `MapRenderer` Klasse instanziiert ein systemeigenes `MapView` Steuerelement. Auf die universelle Windows-Plattform (UWP), die `MapRenderer` Klasse instanziiert ein systemeigenes `MapControl`. Weitere Informationen zu den Renderer und systemeigene Steuerelementklassen, die Xamarin.Forms-Steuerelemente zuordnen, finden Sie unter [Renderer-Basisklassen und systemeigenen Steuerelementen](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehung zwischen der [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) und das entsprechende systemeigene Steuerelemente, die ihn implementieren:

![](customized-pin-images/map-classes.png "Beziehung zwischen das Kartensteuerelement und die implementierende Native Steuerelemente")

Des Renderingprozesses kann verwendet werden, um plattformspezifische Anpassungen zu implementieren, durch das Erstellen eines benutzerdefinierten Renderers für eine [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) auf jeder Plattform. Die Verfahrensweise für diesen Vorgang lautet wie folgt:

1. [Erstellen Sie](#Creating_the_Custom_Map) einer benutzerdefinierten Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) die benutzerdefinierte Zuordnung von Xamarin.Forms.
1. [Erstellen Sie](#Creating_the_Custom_Renderer_on_each_Platform) der benutzerdefinierten Renderers für die Zuordnung auf jeder Plattform.

Jedes Element wird nun wiederum zum Implementieren der besprochen werden eine `CustomMap` Renderer an, der eine systemeigene Darstellung mit einer angepassten Pin "und" eine angepasste Ansicht der Pin-Daten auf jeder Plattform angezeigt.

> [!NOTE]
> [`Xamarin.Forms.Maps`](https://developer.xamarin.com/api/namespace/Xamarin.Forms.Maps/) Initialisiert und vor der Verwendung konfiguriert werden müssen. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Erstellen die benutzerdefinierte Karte

Ein benutzerdefiniertes Steuerelement erstellt werden, indem Unterklassen der [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/) Klasse, wie im folgenden Codebeispiel gezeigt:

```csharp
public class CustomMap : Map
{
  public List<CustomPin> CustomPins { get; set; }
}
```

Die `CustomMap` Steuerelement wird in das .NET Standard-Steuerelementbibliothek-Projekt erstellt und definiert die API für die benutzerdefinierte Karte. Die benutzerdefinierte Karte macht die `CustomPins` Eigenschaft, die die Auflistung von darstellt `CustomPin` Objekte, die von der systemeigenen Kartensteuerelement auf jeder Plattform gerendert werden. Die `CustomPin` ist im folgenden Codebeispiel dargestellt:

```csharp
public class CustomPin : Pin
{
  public string Url { get; set; }
}
```

Diese Klasse definiert ein `CustomPin` als erben die Eigenschaften der [ `Pin` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Pin/) Klasse und das Hinzufügen von einer `Url` Eigenschaft.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Nutzen die benutzerdefinierte Karte

Die `CustomMap` Steuerelement kann verwiesen werden in XAML in .NET Standard-Bibliotheksprojekt durch deklarieren einen Namespace für den Speicherort der und verwenden das Namespacepräfix für das benutzerdefinierte Kartensteuerelement. Im folgenden Codebeispiel wird veranschaulicht wie die `CustomMap` Steuerelement genutzt werden kann, durch die entsprechende Verwendung von XAML-Seite:

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

Die `local` nichts Namespacepräfix kann benannt werden. Allerdings die `clr-namespace` und `assembly` Werte müssen die Details der benutzerdefinierten Zuordnung übereinstimmen. Sobald der Namespace deklariert ist, ist das Präfix verwendet, auf die benutzerdefinierte Karte verweisen.

Im folgenden Codebeispiel wird veranschaulicht wie die `CustomMap` Steuerelement genutzt werden kann, um eine C#-Seite:

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

Die `CustomMap` Instanz wird verwendet, um die systemeigenen Zuordnung auf jeder Plattform anzuzeigen. Es wurde [ `MapType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Maps.Map.MapType/) Eigenschaftensätze das Anzeigenformat des der [ `Map` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Map/), mit den möglichen Werten definiert wird, der [ `MapType` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapType/) Enumeration. Für iOS und Android, die Breite und Höhe der Karte durch Eigenschaften festgelegt ist die `App` -Klasse, die in den plattformspezifischen Projekten initialisiert werden.

Der Speicherort der Karte und die Pins enthält, werden initialisiert, wie im folgenden Codebeispiel gezeigt:

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

Diese Initialisierung Fügt eine benutzerdefinierte Pin und positioniert die Kartenansicht mit der [ `MoveToRegion` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Maps.Map.MoveToRegion(Xamarin.Forms.Maps.MapSpan)/) -Methode, die ändert die Position und die Zoomstufe der Karte durch das Erstellen einer [ `MapSpan` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.MapSpan/) aus einem [ `Position` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Position/) und ein [ `Distance` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Maps.Distance/).

Ein benutzerdefinierter Renderer kann jetzt jedes Anwendungsprojekt zum Anpassen der systemeigenen Zuordnung Steuerelementen hinzugefügt werden.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen benutzerdefinierten Renderer auf jeder Plattform

Der Prozess zum Erstellen der benutzerdefinierten Rendererklasse lautet wie folgt:

1. Erstellen Sie eine Unterklasse von der `MapRenderer` -Klasse, die die benutzerdefinierte Karte rendert.
1. Überschreiben Sie die `OnElementChanged` -Methode, rendert die benutzerdefinierte Karte und Write-Logik, um ihn anzupassen. Diese Methode wird aufgerufen, wenn die entsprechende benutzerdefinierte Karte mit Xamarin.Forms erstellt wird.
1. Hinzufügen einer `ExportRenderer` -Attribut auf die benutzerdefinierten Renderer-Klasse, um anzugeben, dass es zum Rendern der benutzerdefinierte Zuordnung mit Xamarin.Forms verwendet werden. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer mit Xamarin.Forms zu registrieren.

> [!NOTE]
> Ist er optional einen benutzerdefinierten Renderer in jedem plattformprojekt bereitstellen. Wenn ein benutzerdefinierter Renderer nicht registriert ist, wird der Standardrenderer für die Basisklasse für das Steuerelement verwendet werden.

Das folgende Diagramm veranschaulicht die Zuständigkeiten aller Projekte in der beispielanwendung, sowie die Beziehungen zwischen ihnen:

![](customized-pin-images/solution-structure.png "CustomMap benutzerdefinierten Renderer Projekt Zuständigkeiten")

Die `CustomMap` plattformspezifischen Renderingklassen, die Ableitung Steuerelement gerendert wird die `MapRenderer` Klasse für jede Plattform. Dies führt in den einzelnen `CustomMap` Steuerelements plattformspezifischen Steuerelemente gerendert wird, wie in den folgenden Screenshots dargestellt:

![](customized-pin-images/screenshots.png "CustomMap auf jeder Plattform")

Die `MapRenderer` -Klasse verfügbar macht die `OnElementChanged` -Methode, die aufgerufen wird, wenn die benutzerdefinierte Karte mit Xamarin.Forms erstellt wird, um das entsprechende systemeigene Steuerelement rendern. Diese Methode nimmt ein `ElementChangedEventArgs` Parameter, enthält `OldElement` und `NewElement` Eigenschaften. Diese Eigenschaften repräsentieren die Xamarin.Forms-Element, den Renderer *wurde* angefügt, und das Xamarin.Forms-Element, den Renderer *ist* angefügt sind, bzw. In der beispielanwendung der `OldElement` -Eigenschaft `null` und `NewElement` Eigenschaft enthält einen Verweis auf die `CustomMap` Instanz.

Eine überschriebene Version von den `OnElementChanged` Methode in jeder Rendererklasse plattformspezifischen bietet die Möglichkeit zum Durchführen der systemeigenen-Steuerelement. Durch ein typisierter Verweis auf das systemeigene Steuerelement verwendet wird, auf der Plattform zugegriffen werden kann die `Control` Eigenschaft. Darüber hinaus kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird abgerufen werden, über die `Element` Eigenschaft.

Muss darauf geachtet werden beim Abonnieren von Ereignishandlern in der `OnElementChanged` Methode, wie im folgenden Codebeispiel gezeigt:

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

Das systemeigene Steuerelement sollte konfiguriert werden, und Ereignishandler abonniert nur, wenn die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist. Ebenso sollten alle Ereignishandler, die abonniert wurden gekündigt werden nur, wenn das Element, dem der Renderer angefügt ist ändert. Dieser Ansatz hilft ein benutzerdefiniertes Renderers erstellen, das von Arbeitsspeicherverlusten nicht beeinträchtigt werden.

Jede Klasse benutzerdefinierter Renderer mit ergänzt wird ein `ExportRenderer` -Attribut, das den Renderer mit Xamarin.Forms registriert. Das Attribut nimmt zwei Parameter: der Typname, der das benutzerdefinierte Xamarin.Forms-Steuerelement gerendert wird, und der Typname der benutzerdefinierten Renderer. Die `assembly` Präfix für das Attribut gibt an, dass das Attribut für die gesamte Assembly angewendet wird.

In den folgenden Abschnitten erläutern die Implementierung der einzelnen benutzerdefinierten Renderers Clientplattform-spezifische Klassen.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen die benutzerdefinierten Renderers für iOS

Die folgenden Screenshots zeigen die Zuordnung vor und nach dem anpassen:

![](customized-pin-images/map-layout-ios.png "Kartensteuerelement vor und nach der Anpassung")

Auf iOS-Pin heißt ein *Anmerkung*, und kann ein benutzerdefiniertes Bild oder eine Pin systemdefinierte verschiedener Farben. Anmerkungen können optional anzeigen, eine *Legende*, wird die Reaktion auf den Benutzer, der die Anmerkung auswählen angezeigt. Die Legende zeigt die `Label` und `Address` Eigenschaften der `Pin` -Instanz, mit optionalen links und rechts Zubehörs Sichten. Im oben dargestellten Screenshot die linke Zubehörs Ansicht wird für das Bild von einem Affe mit der rechts Zubehörs anzeigen, die *Informationen* Schaltfläche.

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

Die `OnElementChanged` -Methode führt die folgende Konfiguration von der [ `MKMapView` ](https://developer.xamarin.com/api/type/MapKit.MKMapView/) Instanz, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Die [ `GetViewForAnnotation` ](https://developer.xamarin.com/api/property/MapKit.MKMapView.GetViewForAnnotation/) -Eigenschaftensatz auf die `GetViewForAnnotation` Methode. Diese Methode wird aufgerufen, wenn die [die Position der Anmerkung wird auf der Karte sichtbar](#Displaying_the_Annotation), und wird verwendet, um die Anmerkung vor der Anzeige anpassen.
- Ereignishandler für das `CalloutAccessoryControlTapped`, `DidSelectAnnotationView`, und `DidDeselectAnnotationView` Ereignisse registriert werden. Diese Ereignisse werden ausgelöst, wenn der Benutzer [tippt in der Legende der richtige Zubehör](#Tapping_on_the_Right_Callout_Accessory_View), und wenn der Benutzer [wählt](#Selecting_the_Annotation) und [hebt die Auswahl](#Deselecting_the_Annotation) die Anmerkung bzw. Die Ereignisse sind gekündigt, nur, wenn das Element den Renderer an Änderungen angefügt ist.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Die Anmerkung anzeigen

Die `GetViewForAnnotation` Methode wird aufgerufen, wenn die Position der Anmerkung auf der Karte sichtbar wird und verwendet wird, um die Anmerkung vor der Anzeige anpassen. Eine Anmerkung besteht aus zwei Teilen:

- `MkAnnotation` – Titel, einen Untertitel und die Position der Anmerkung enthält.
- `MkAnnotationView` – enthält das Bild darstellt, die Anmerkung, und optional eine Legende, die angezeigt wird, wenn der Benutzer die Anmerkung tippt.

Die `GetViewForAnnotation` Methode akzeptiert eine `IMKAnnotation` , die die Anmerkung Daten enthält, und gibt eine `MKAnnotationView` für die Anzeige auf der Karte und wird im folgenden Codebeispiel gezeigt:

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

Diese Methode wird sichergestellt, dass die Anmerkung wird als ein benutzerdefiniertes Bild angezeigt, statt als systemdefinierte Pin und, wenn die Anmerkung abgerufen werden eine Legende angezeigt, die zusätzliche Inhalte, links und rechts neben der Anmerkungstitel und die Adresse enthält . Dies geschieht wie folgt:

1. Die `GetCustomPin` Methode wird aufgerufen, um die benutzerdefinierte Pin-Daten für die Anmerkung zurückzugeben.
1. Um Speicherplatz zu sparen, die Anmerkung Sicht ist in einem Pool zusammengefasste zur Wiederverwendung durch den Aufruf [ `DequeueReusableAnnotation` ](https://developer.xamarin.com/api/member/MapKit.MKMapView.DequeueReusableAnnotation/(System.String)/).
1. Die `CustomMKAnnotationView` -Klasse erweitert die `MKAnnotationView` -Klasse mit `Id` und `Url` Eigenschaften, die in identischen Eigenschaften entsprechen, den `CustomPin` Instanz. Eine neue Instanz der dem `CustomMKAnnotationView` erstellt wurde, vorausgesetzt, dass die Anmerkung ist `null`:
  - Die `CustomMKAnnotationView.Image` Eigenschaft festgelegt ist, auf das Abbild, die die Anmerkung in der Zuordnung darstellen.
  - Die `CustomMKAnnotationView.CalloutOffset` -Eigenschaftensatz auf eine `CGPoint` , der angibt, dass die Legende oberhalb der Anmerkung zentriert werden soll.
  - Die `CustomMKAnnotationView.LeftCalloutAccessoryView` Eigenschaftensatz zu einem Bild von einer Affe, die auf der linken Seite der Anmerkungstitel und Adresse angezeigt werden.
  - Die `CustomMKAnnotationView.RightCalloutAccessoryView` -Eigenschaftensatz auf eine *Informationen* Schaltfläche, die rechts neben dem Anmerkungstitel und der Adresse angezeigt werden.
  - Die `CustomMKAnnotationView.Id` -Eigenschaftensatz auf die `CustomPin.Id` von zurückgegebene Eigenschaft der `GetCustomPin` Methode. Dadurch können die Anmerkung identifiziert werden, sodass sie [Legende kann weiter angepasst](#Selecting_the_Annotation), falls gewünscht.
  - Die `CustomMKAnnotationView.Url` -Eigenschaftensatz auf die `CustomPin.Url` von zurückgegebene Eigenschaft der `GetCustomPin` Methode. Der URL wird, navigiert werden, wenn der Benutzer [die in der Legende rechts Zubehörs Ansicht angezeigte Schaltfläche tippt](#Tapping_on_the_Right_Callout_Accessory_View).
1. Die [ `MKAnnotationView.CanShowCallout` ](https://developer.xamarin.com/api/property/MapKit.MKAnnotationView.CanShowCallout/) -Eigenschaftensatz auf `true` , damit die Legende angezeigt wird, wenn die Anmerkung abgerufen werden.
1. Die Anmerkung wird für die Anzeige auf der Karte zurückgegeben.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Die Anmerkung auswählen

Beim Tippen auf die Anmerkung, die `DidSelectAnnotationView` Ereignis wird ausgelöst, die wiederum führt die `OnDidSelectAnnotationView` Methode:

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

Diese Methode erweitert die vorhandene Legende (die linke und rechten Zubehörs Ansichten enthält) durch Hinzufügen einer `UIView` Instanz darauf, ein Bild des Logos Xamarin enthält, vorausgesetzt, dass die ausgewählte Anmerkung verfügt über, seine `Id` -Eigenschaftengruppe`Xamarin`. Dies ermöglicht Szenarien, in denen unterschiedliche Legenden für andere Anmerkungen angezeigt werden können. Die `UIView` Instanz zentriert oberhalb der vorhandenen Legende angezeigt werden.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Tippen auf die richtige Legende Zubehör-Sicht

Wenn der Benutzer tippt, auf die *Informationen* Schaltfläche in der Legende rechts Zubehörs Ansicht der `CalloutAccessoryControlTapped` Ereignis wird ausgelöst, die wiederum führt die `OnCalloutAccessoryControlTapped` Methode:

```csharp
void OnCalloutAccessoryControlTapped (object sender, MKMapViewAccessoryTappedEventArgs e)
{
  var customView = e.View as CustomMKAnnotationView;
  if (!string.IsNullOrWhiteSpace (customView.Url)) {
    UIApplication.SharedApplication.OpenUrl (new Foundation.NSUrl (customView.Url));
  }
}
```

Diese Methode wird einen Webbrowser geöffnet und navigiert zu der Adresse gespeichert, der `CustomMKAnnotationView.Url` Eigenschaft. Beachten Sie, dass die Adresse, beim Erstellen definiert wurde der `CustomPin` Auflistung in das .NET Standard-Steuerelementbibliothek-Projekt.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Deaktivieren die Anmerkung

Wenn die Anmerkung angezeigt wird und der Benutzer in der Zuordnung tippt der `DidDeselectAnnotationView` Ereignis wird ausgelöst, die wiederum führt die `OnDidDeselectAnnotationView` Methode:

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

Diese Methode wird sichergestellt, dass die vorhandene Legende nicht ausgewählt ist, der erweiterten Teil der Legende (das Bild des Logos Xamarin) beendet darüber hinaus wird angezeigt und seine Ressourcen werden freigegeben werden.

Weitere Informationen zum Anpassen einer `MKMapView` Instanz ist, finden Sie unter [iOS Maps](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen von benutzerdefinierten Renderers für Android

Die folgenden Screenshots zeigen die Zuordnung vor und nach dem anpassen:

![](customized-pin-images/map-layout-android.png "Kartensteuerelement vor und nach der Anpassung")

Auf Android-Geräten wird die Pin bezeichnet eine *Marker*, und kann entweder ein benutzerdefiniertes Image oder ein Marker systemdefinierte verschiedener Farben. Marker anzeigen können eine *Fenster "Info"*, in der Antwort an den Benutzer Tippen auf die Markierung angezeigt. Zeigt das Informationsfenster der `Label` und `Address` Eigenschaften der `Pin` -Instanz und können angepasst werden, um andere Inhalte einschließt. Allerdings kann nur ein Informationsfenster gleichzeitig angezeigt werden.

Das folgende Codebeispiel zeigt die benutzerdefinierten Renderers für das Android-Plattform:

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

Vorausgesetzt, dass ein neues Xamarin.Forms-Element, der benutzerdefinierte Renderer angefügt ist die `OnElementChanged` Methodenaufrufe der `MapView.GetMapAsync` -Methode, die die zugrunde liegende ruft `GoogleMap` , die auf die Sicht gebunden ist. Einmal die `GoogleMap` Instanz verfügbar ist, wird die `OnMapReady` Außerkraftsetzung aufgerufen wird. Diese Methode registriert einen Ereignishandler für das `InfoWindowClick` Ereignis, das ausgelöst, wenn wird die [Fenster "Info" geklickt wird](#Clicking_on_the_Info_Window), und von abgemeldet wird, nur, wenn das Element den Renderer an Änderungen angefügt ist. Die `OnMapReady` überschreiben auch Aufrufe der `SetInfoWindowAdapter` Methode, um anzugeben, dass die `CustomMapRenderer` Klasseninstanz gebe die Methoden, um das Informationsfenster anpassen.

Die `CustomMapRenderer` -Klasse implementiert die `GoogleMap.IInfoWindowAdapter` -Schnittstelle [anpassen Informationsfenster](#Customizing_the_Info_Window). Diese Schnittstelle gibt an, dass die folgenden Methoden implementiert werden müssen:

- `public Android.Views.View GetInfoWindow(Marker marker)` – Diese Methode wird aufgerufen, um ein Informationsfenster benutzerdefinierte für einen Marker zurückzugeben. Wenn zurückgegeben `null`, und klicken Sie dann das Standardrendering zurückgreifen Fenster verwendet werden soll. Wenn zurückgegeben wird ein `View`, klicken Sie dann, `View` wird die Info-Fensterrahmens platziert werden.
- `public Android.Views.View GetInfoContents(Marker marker)` – Diese Methode wird aufgerufen, um zurückzukehren eine `View` mit dem Inhalt des Fensters Info und wird nur aufgerufen werden, wenn die `GetInfoWindow` -Methode zurückkehrt `null`. Wenn zurückgegeben `null`, und klicken Sie dann das standardmäßige Rendern des Inhalts im Fenster "Info" verwendet werden soll.

In der beispielanwendung nur die Informationen im Fensterinhalt angepasst wird, muss das `GetInfoWindow` -Methode zurückkehrt `null` aktivieren.

#### <a name="customizing-the-marker"></a>Die Markierung anpassen

Das Symbol zur Darstellung von eines Markers kann angepasst werden, durch Aufrufen der `MarkerOptions.SetIcon` Methode. Dies geschieht durch Überschreiben der `CreateMarker` -Methode, die aufgerufen wird, für die einzelnen `Pin` , die Zuordnung hinzugefügt wird:

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

Diese Methode erstellt ein neues `MarkerOption` für jede Instanz `Pin` Instanz. Nach dem Festlegen der Position, die Bezeichnung und die Adresse des Markers, dessen Symbol "festgelegt ist, mit der `SetIcon` Methode. Diese Methode nimmt einen `BitmapDescriptor` Objekt, das zum Rendern des Symbol mit erforderlichen Daten enthält die `BitmapDescriptorFactory` Klasse, die Hilfsmethoden zum Vereinfachen der Erstellung der Bereitstellung der `BitmapDescriptor`.

Weitere Informationen zum Verwenden der `BitmapDescriptorFactory` Klasse eine Markierung anpassen, finden Sie unter [Anpassen einen Marker](~/android/platform/maps-and-location/maps/maps-api.md).

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Das Informationsfenster anpassen

Wenn ein Benutzer auf den Marker fest, tippt der `GetInfoContents` Methode ausgeführt wird, werden bereitgestellt, die die `GetInfoWindow` -Methode zurückkehrt `null`. Das folgende Codebeispiel zeigt die `GetInfoContents` Methode:

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

Diese Methode gibt ein `View` mit dem Inhalt des Fensters Informationen. Dies geschieht wie folgt:

- Ein `LayoutInflater` Instanz abgerufen wird. Wird verwendet, um ein Layout-XML-Datei in das entsprechende instanziieren `View`.
- Die `GetCustomPin` Methode wird aufgerufen, um die benutzerdefinierte Pin-Daten für das Informationsfenster zurückzugeben.
- Die `XamarinMapInfoWindow` Layout wird vergrößert, wenn die `CustomPin.Id` -Eigenschaft gleich `Xamarin`. Andernfalls die `MapInfoWindow` Layout vergrößert wird. Dies ermöglicht Szenarien, in anderen Informationen Fensterlayouts für verschiedene Marker angezeigt werden können.
- Die `InfoWindowTitle` und `InfoWindowSubtitle` Ressourcen werden aus dem vergrößerte Layout abgerufen und ihre `Text` Eigenschaften werden festgelegt, um die entsprechenden Daten aus der `Marker` Instanz, vorausgesetzt, dass die Ressourcen nicht sind `null`.
- Die `View` Instanz wird für die Anzeige auf der Karte zurückgegeben.

> [!NOTE]
> Ein Informationsfenster ist keine live `View`. Stattdessen Android wandelt die `View` auf eine statische bitmap und anzuzeigen, die als Bild. Das bedeutet, die bei einem Informationsfenster auf einen Click-Ereignis reagieren kann, es auf alle Berührungsereignisse oder Gesten nicht reagieren, und die einzelnen Steuerelemente in das Informationsfenster nicht auf ihre eigenen click-Ereignisse.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Durch Klicken auf das Informationsfenster

Klickt der Benutzer auf das Informationsfenster der `InfoWindowClick` Ereignis wird ausgelöst, die wiederum führt die `OnInfoWindowClick` Methode:

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

Diese Methode wird einen Webbrowser geöffnet und navigiert zu der Adresse, die in gespeicherten der `Url` Eigenschaft der abgerufenen `CustomPin` Instanz, für die `Marker`. Beachten Sie, dass die Adresse, beim Erstellen definiert wurde der `CustomPin` Auflistung in das .NET Standard-Steuerelementbibliothek-Projekt.

Weitere Informationen zum Anpassen einer `MapView` Instanz ist, finden Sie unter [Maps-API](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen von benutzerdefinierten Renderers für universelle Windows-Plattform

Die folgenden Screenshots zeigen die Zuordnung vor und nach dem anpassen:

![](customized-pin-images/map-layout-uwp.png "Kartensteuerelement vor und nach der Anpassung")

Für universelle Windows-Plattform die Pin aufgerufen wird eine *Kartensymbol*, und ein benutzerdefiniertes Image oder das System definierte Standardbild sein. Ein Symbol "Zuordnung" anzeigen kann eine `UserControl`, angezeigt, in der Antwort an den Benutzer auf das Symbol "Zuordnung" tippen. Die `UserControl` können alle Inhalte anzeigen, einschließlich der `Label` und `Address` Eigenschaften des der `Pin` Instanz.

Im folgenden Codebeispiel wird veranschaulicht, den benutzerdefinierten Renderer für universelle Windows-Plattform:

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

Die `OnElementChanged` -Methode führt die folgenden Vorgänge aus, vorausgesetzt, dass die benutzerdefinierten Renderers ein neues Xamarin.Forms-Element zugeordnet ist:

- Löscht die `MapControl.Children` -Auflistung, um alle vorhandenen Elemente der Benutzeroberfläche aus der Zuordnung entfernen, bevor Sie registrieren einen Ereignishandler für das `MapElementClick` Ereignis. Dieses Ereignis wird ausgelöst, wenn der Benutzer tippt, oder klicken Sie auf eine `MapElement` auf die `MapControl`, und von abgemeldet wird, nur, wenn das Element den Renderer an Änderungen angefügt ist.
- Jede Pin in der `customPins` Sammlung wird wie folgt an den richtigen geografischen Standorten auf der Karte angezeigt:
  - Der Speicherort für die Pin wird als erstellt eine `Geopoint` Instanz.
  - Ein `MapIcon` Instanz erstellt, um die Pin darstellen.
  - Das Bild zur Darstellung der `MapIcon` wird angegeben, indem die `MapIcon.Image` Eigenschaft. Allerdings ist Symbolbild die Zuordnung ist nicht immer gewährleistet angezeigt werden, wie es von anderen Elementen auf der Karte verdeckt werden kann. Aus diesem Grund die Zuordnung des Symbols `CollisionBehaviorDesired` -Eigenschaftensatz auf `MapElementCollisionBehavior.RemainVisible`, um sicherzustellen, dass diese sichtbar bleiben.
  - Der Speicherort des der `MapIcon` wird angegeben, indem die `MapIcon.Location` Eigenschaft.
  - Die `MapIcon.NormalizedAnchorPoint` Eigenschaft auf die ungefähre Position des Zeigers auf das Image festgelegt ist. Wenn diese Eigenschaft den Standardwert (0,0), der der oberen linken Ecke des Bilds darstellt beibehält, möglicherweise die Änderungen in die Zoomstufe der Karte in der Abbildung, die auf einem anderen Speicherort verweist.
  - Die `MapIcon` Instanz wird hinzugefügt, um die `MapControl.MapElements` Auflistung. Dies führt zu dem Kartensymbol angezeigt wird, auf die `MapControl`.

> [!NOTE]
> Bei Verwendung des gleichen Images für mehrere Zuordnung Symbole der `RandomAccessStreamReference` Instanz sollte für eine optimale Leistung auf der Seite oder Anwendungsebene deklariert werden.

#### <a name="displaying-the-usercontrol"></a>Anzeigen von der UserControl

Wenn ein Benutzer auf das Symbol "Zuordnung" tippt der `OnMapElementClick` Methode ausgeführt wird. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

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

Diese Methode erstellt eine `UserControl` -Instanz, die Informationen zu den Pin angezeigt. Dies geschieht wie folgt:

- Die `MapIcon` Instanz abgerufen wird.
- Die `GetCustomPin` Methode wird aufgerufen, um die benutzerdefinierte Pin-Daten zurückgeben, die angezeigt werden sollen.
- Ein `XamarinMapOverlay` Instanz erstellt, um die benutzerdefinierte Pin-Daten anzuzeigen. Diese Klasse ist ein benutzerdefiniertes Steuerelement an.
- Den geografischen Standort die Anzeige an die `XamarinMapOverlay` Instanz, auf die `MapControl` wird als erstellt eine `Geopoint` Instanz.
- Die `XamarinMapOverlay` Instanz wird hinzugefügt, um die `MapControl.Children` Auflistung. Diese Auflistung enthält die Verwendung von XAML-Elemente der Benutzeroberfläche, die auf der Karte angezeigt werden.
- Der geografische Standort des der `XamarinMapOverlay` Instanz auf der Karte wird festgelegt, durch Aufrufen der `SetLocation` Methode.
- Die relative Position auf der `XamarinMapOverlay` Instanz, die der angegebenen Position entspricht, wird festgelegt, durch Aufrufen der `SetNormalizedAnchorPoint` Methode. Dadurch wird sichergestellt, die Änderungen in die Zoomstufe des Ergebnisses Zuordnung in der `XamarinMapOverlay` Instanz immer am richtigen Speicherort angezeigt wird.

Alternativ können Sie Informationen über die Pin bereits auf der Karte angezeigt wird, tippen Sie auf der Karte entfernt die `XamarinMapOverlay` -Instanz von der `MapControl.Children` Auflistung.

#### <a name="tapping-on-the-information-button"></a>Tippen auf die Schaltfläche "Information"

Wenn der Benutzer tippt auf die *Informationen* Schaltfläche der `XamarinMapOverlay` Benutzersteuerelement, das `Tapped` Ereignis wird ausgelöst, die wiederum führt die `OnInfoButtonTapped` Methode:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Diese Methode wird einen Webbrowser geöffnet und navigiert zu der Adresse, die in gespeicherten der `Url` Eigenschaft von der `CustomPin` Instanz. Beachten Sie, dass die Adresse, beim Erstellen definiert wurde der `CustomPin` Auflistung in das .NET Standard-Steuerelementbibliothek-Projekt.

Weitere Informationen zum Anpassen einer `MapControl` Instanz ist, finden Sie unter [Zuordnungen und Speicherort (Übersicht)](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx) auf MSDN.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie zum Erstellen eines benutzerdefinierten Renderers für das `Map` -Steuerelement, und Entwickler können das Standardrendering zurückgreifen systemeigene mit ihren eigenen plattformspezifische Anpassungen zu überschreiben. Xamarin.Forms.Maps bietet eine plattformübergreifende Abstraktion für die Anzeige von Zuordnungen, die systemeigene Zuordnung auf jeder Plattform-APIs zum bieten einer schnelle und vertrauten Zuordnung Erfahrung für Benutzer verwenden.


## <a name="related-links"></a>Verwandte Links

- [Karten-Steuerelement](~/xamarin-forms/user-interface/map.md)
- [iOS-Zuordnungen](~/ios/user-interface/controls/ios-maps/index.md)
- [Karten-API](~/android/platform/maps-and-location/maps/maps-api.md)
- [Benutzerdefinierte Pin (Beispiel)](https://developer.xamarin.com/samples/xamarin-forms/customrenderers/map/pin/)
