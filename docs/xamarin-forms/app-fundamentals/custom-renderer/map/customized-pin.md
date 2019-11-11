---
title: Anpassen einer Kartenstecknadel
description: In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für das Map-Steuerelement erstellen, der eine native Karte mit einer benutzerdefinierten Stecknadel und einer benutzerdefinierten Ansicht der Stecknadeldaten auf jeder Plattform anzeigt.
ms.prod: xamarin
ms.assetid: C5481D86-80E9-4E3D-9FB6-57B0F93711A6
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/06/2019
ms.openlocfilehash: dfb7f12affc8b0b41ec56cd17894c0f0a4b5fc6e
ms.sourcegitcommit: 283810340de5310f63ef7c3e4b266fe9dc2ffcaf
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/06/2019
ms.locfileid: "73662349"
---
# <a name="customizing-a-map-pin"></a>Anpassen einer Kartenstecknadel

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-pin)

_In diesem Artikel wird veranschaulicht, wie Sie einen benutzerdefinierten Renderer für das Map-Steuerelement erstellen, der eine native Karte mit einer benutzerdefinierten Stecknadel und einer benutzerdefinierten Ansicht der Stecknadeldaten auf jeder Plattform anzeigt._

Jede Xamarin.Forms-Ansicht verfügt über einen entsprechenden Renderer für jede Plattform, der eine Instanz eines nativen Steuerelements erstellt. Beim Rendern eines [`Map`](xref:Xamarin.Forms.Maps.Map)-Objekts durch eine Xamarin.Forms-Anwendung wird in iOS die `MapRenderer`-Klasse instanziiert, wodurch wiederum ein natives `MKMapView`-Steuerelement instanziiert wird. Auf der Android-Plattform instanziiert die `MapRenderer`-Klasse ein natives `MapView`-Steuerelement. Auf der Universellen Windows-Plattform (UWP) instanziiert die `MapRenderer`-Klasse eine native `MapControl`-Klasse. Weitere Informationen zu den Renderern und Klassen nativer Steuerelemente, auf die Xamarin.Forms-Steuerelemente verweisen, finden Sie unter [Renderer Base Classes and Native Controls (Rendererbasisklassen und native Steuerelemente)](~/xamarin-forms/app-fundamentals/custom-renderer/renderers.md).

Das folgende Diagramm veranschaulicht die Beziehungen zwischen dem [`Map`](xref:Xamarin.Forms.Maps.Map)-Objekt und den entsprechenden nativen Steuerelementen, die dieses implementieren:

![](customized-pin-images/map-classes.png "Relationship Between the Map Control and the Implementing Native Controls")

Der Renderingprozess kann genutzt werden, um plattformspezifische Anpassungen zu implementieren, indem für eine [`Map`](xref:Xamarin.Forms.Maps.Map)-Klasse auf jeder Plattform ein benutzerdefinierter Renderer erstellt wird. Gehen Sie hierfür folgendermaßen vor:

1. [Erstellen](#Creating_the_Custom_Map) Sie eine benutzerdefinierte Xamarin.Forms-Karte.
1. [Nutzen](#Consuming_the_Custom_Map) Sie die benutzerdefinierte Karte über Xamarin.Forms.
1. [Erstellen](#Creating_the_Custom_Renderer_on_each_Platform) Sie einen benutzerdefinierten Renderer für die Karte auf jeder Plattform.

Die Elemente werden im Folgenden erläutert, um einen `CustomMap`-Renderer zu implementieren, der eine native Karte mit einer benutzerdefinierten Stecknadel und eine benutzerdefinierte Ansicht der Stecknadeldaten für jede Plattform anzeigt.

> [!NOTE]
> [`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps) muss vor der Verwendung initialisiert und konfiguriert werden. Weitere Informationen finden Sie unter [`Maps Control`](~/xamarin-forms/user-interface/map/index.md).

<a name="Creating_the_Custom_Map" />

## <a name="creating-the-custom-map"></a>Erstellen der benutzerdefinierten Karte

Das benutzerdefinierte Kartensteuerelement kann erstellt werden, indem Sie die [`Map`](xref:Xamarin.Forms.Maps.Map)-Klasse wie in folgendem Codebeispiel als Unterklasse verwenden:

```csharp
public class CustomMap : Map
{
    public List<CustomPin> CustomPins { get; set; }
}
```

Das Steuerelement `CustomMap` wird im .NET Standard-Bibliotheksprojekt erstellt und definiert die API für die benutzerdefinierte Klasse. Die benutzerdefinierte Karte macht die `CustomPins`-Eigenschaft verfügbar, die die Collection der `CustomPin`-Objekte darstellt, die vom nativen Kartensteuerelement auf jeder Plattform gerendert wird. Die `CustomPin`-Klasse wird im folgenden Codebeispiel veranschaulicht.

```csharp
public class CustomPin : Pin
{
    public string Name { get; set; }
    public string Url { get; set; }
}
```

Hiermit wird eine `CustomPin`-Klasse definiert, die die Eigenschaften der [`Pin`](xref:Xamarin.Forms.Maps.Pin)-Klasse erben soll. Darüber hinaus werden die Eigenschaften `Name` und `Url` hinzugefügt.

<a name="Consuming_the_Custom_Map" />

## <a name="consuming-the-custom-map"></a>Nutzen der benutzerdefinierten Karte

Sie können auf das Steuerelement `CustomMap` in XAML im .NET Standard-Bibliotheksprojekt verweisen, indem Sie einen Namespace für seine Position deklarieren und das Namespacepräfix für das benutzerdefinierte Kartensteuerelement verwenden. Das folgende Codebeispiel veranschaulicht, wie das Steuerelement `CustomMap` von der XAML-Seite genutzt werden kann:

```xaml
<ContentPage ...
                   xmlns:local="clr-namespace:CustomRenderer;assembly=CustomRenderer">
    <local:CustomMap x:Name="customMap"
                   MapType="Street" />
</ContentPage>

```

Das `local`-Namespacepräfix kann beliebig benannt werden. Die Werte `clr-namespace` und `assembly` müssen mit den Angaben der benutzerdefinierten Karte übereinstimmen. Wenn der Namespace deklariert wurde, wird das Präfix verwendet, um auf die benutzerdefinierte Karte zu verweisen.

Das folgende Codebeispiel veranschaulicht, wie das benutzerdefinierte Steuerelement `CustomMap` von einer C#-Seite genutzt werden kann:

```csharp
public class MapPageCS : ContentPage
{
    public MapPageCS()
    {
        CustomMap customMap = new CustomMap
        {
            MapType = MapType.Street
        };
        // ...
        Content = customMap;
    }
}
```

Die `CustomMap`-Instanz wird verwendet, um eine native Karte auf jeder Plattform anzuzeigen. Die [`MapType`](xref:Xamarin.Forms.Maps.Map.MapType)-Eigenschaft der Instanz legt das Anzeigeformat von [`Map`](xref:Xamarin.Forms.Maps.Map) fest. Die möglichen Werte werden hierbei in der [`MapType`](xref:Xamarin.Forms.Maps.MapType)-Enumeration definiert.

Die Position der Karte und die enthaltenen Stecknadeln werden wie im folgenden Codebeispiel dargestellt initialisiert:

```csharp
public MapPage()
{
    // ...
    CustomPin pin = new CustomPin
    {
        Type = PinType.Place,
        Position = new Position(37.79752, -122.40183),
        Label = "Xamarin San Francisco Office",
        Address = "394 Pacific Ave, San Francisco CA",
        Name = "Xamarin",
        Url = "http://xamarin.com/about/"
    };
    customMap.CustomPins = new List<CustomPin> { pin };
    customMap.Pins.Add(pin);
    customMap.MoveToRegion(MapSpan.FromCenterAndRadius(new Position(37.79752, -122.40183), Distance.FromMiles(1.0)));
}
```

Diese Initialisierung fügt über die [`MoveToRegion`](xref:Xamarin.Forms.Maps.Map.MoveToRegion*)-Methode eine benutzerdefinierte Stecknadel hinzu und positioniert die Kartenansicht, wodurch sich die Position und der Zoomfaktor der Karte ändern, indem eine [`MapSpan`](xref:Xamarin.Forms.Maps.MapSpan)-Klasse aus einer [`Position`](xref:Xamarin.Forms.Maps.Position)- und einer [`Distance`](xref:Xamarin.Forms.Maps.Distance)-Struktur erstellt wird.

Ein benutzerdefinierter Renderer kann nun zu jedem Anwendungsprojekt hinzugefügt werden, um die nativen Kartensteuerelemente anzupassen.

<a name="Creating_the_Custom_Renderer_on_each_Platform" />

## <a name="creating-the-custom-renderer-on-each-platform"></a>Erstellen des benutzerdefinierten Renderers auf jeder Plattform

Gehen Sie folgendermaßen vor, um eine Klasse für einen benutzerdefinierten Renderer zu erstellen:

1. Erstellen Sie eine Unterklasse der `MapRenderer`-Klasse, die die benutzerdefinierte Karte rendert.
1. Überschreiben Sie die `OnElementChanged`-Methode, die die benutzerdefinierte Karte rendert, und schreiben Sie Logik, um diese anzupassen. Die Methode wird aufgerufen, wenn die entsprechende benutzerdefinierte Xamarin.Forms-Karte erstellt wird.
1. Fügen Sie der Klasse des benutzerdefinierten Renderers ein `ExportRenderer`-Attribut hinzu, um anzugeben, dass sie zum Rendern der benutzerdefinierten Xamarin.Forms-Karte verwendet werden soll. Dieses Attribut wird verwendet, um den benutzerdefinierten Renderer bei Xamarin.Forms zu registrieren.

> [!NOTE]
> Optional kann ein benutzerdefinierter Renderer in jedem Plattformprojekt bereitgestellt werden. Wenn kein benutzerdefinierter Renderer registriert wurde, wird der Standardrenderer für die Basisklasse des Steuerelements verwendet.

Das folgende Diagramm veranschaulicht die Zuständigkeiten jedes Projekts in der Beispielanwendung sowie deren Beziehungen zueinander:

![](customized-pin-images/solution-structure.png "CustomMap Custom Renderer Project Responsibilities")

Das Steuerelement `CustomMap` wird von plattformspezifischen Rendererklassen gerendert, die von der `MapRenderer`-Klasse für jede Plattform abgeleitet werden. Das führt dazu, dass jedes `CustomMap`-Steuerelement mit plattformspezifischen Steuerelementen gerendert wird. Dies wird in folgenden Screenshots veranschaulicht:

![](customized-pin-images/screenshots.png "CustomMap on each Platform")

Die `MapRenderer`-Klasse stellt die `OnElementChanged`-Methode zur Verfügung, die bei der Erstellung der benutzerdefinierten Xamarin.Forms-Karte aufgerufen wird, um das entsprechende native Steuerelement zu rendern. Diese Methode akzeptiert einen `ElementChangedEventArgs`-Parameter, der die Eigenschaften `OldElement` und `NewElement` enthält. Diese Eigenschaften stellen jeweils das Xamarin.Forms-Element dar, an das der Renderer angefügt *war*, und das Xamarin.Forms-Element, an das der Renderer angefügt *ist*. In der Beispielanwendung ist die `OldElement`-Eigenschaft `null`, und die `NewElement`-Eigenschaft enthält einen Verweis auf die `CustomMap`-Instanz.

Das native Steuerelement sollte in der überschriebenen Version der `OnElementChanged`-Methode in jeder plattformspezifischen Rendererklasse angepasst werden. Über die `Control`-Eigenschaft können Sie auf einen typisierten Verweis auf das native Steuerelement zugreifen, das auf der Plattform verwendet wird. Zusätzlich kann ein Verweis auf das Xamarin.Forms-Steuerelement, das gerendert wird, über die `Element`-Eigenschaft abgerufen werden.

Sie müssen vorsichtig sein, wenn Sie Ereignishandler in der `OnElementChanged`-Methode abonnieren. Sehen Sie sich dazu das folgende Codebeispiel an:

```csharp
protected override void OnElementChanged (ElementChangedEventArgs<Xamarin.Forms.View> e)
{
  base.OnElementChanged (e);

  if (e.OldElement != null)
  {
      // Unsubscribe from event handlers
  }

  if (e.NewElement != null)
  {
      // Configure the native control and subscribe to event handlers
  }
}
```

Das native Steuerelement sollte nur dann konfiguriert und für Ereignishandler abonniert werden, wenn der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt wird. Gleichermaßen sollte das Abonnement für Ereignishandler nur dann gekündigt werden, wenn sich das Element ändert, an das der Renderer angefügt wurde. Mit diesem Ansatz kann ein benutzerdefinierter Renderer erstellt werden, der nicht durch Speicherverluste beeinträchtigt wird.

Jede benutzerdefinierte Rendererklasse ist mit einem `ExportRenderer`-Attribut versehen, das den Renderer bei Xamarin.Forms registriert. Das Attribut benötigt zwei Parameter: den Typnamen des zu rendernden benutzerdefinierten Xamarin.Forms-Steuerelements und den Typnamen des benutzerdefinierten Renderers. Das Präfix `assembly` für das Attribut gibt an, dass das Attribut für die gesamte Assembly gilt.

In den folgenden Abschnitten wird die Implementierung jeder plattformspezifischen, benutzerdefinierten Rendererklasse erläutert.

### <a name="creating-the-custom-renderer-on-ios"></a>Erstellen des benutzerdefinierten Renderers unter iOS

Auf den folgenden Screenshots ist die Karte vor und nach der Anpassung zu sehen:

![](customized-pin-images/map-layout-ios.png "Map Control Before and After Customization")

Unter iOS wird die Stecknadel als *Anmerkung* bezeichnet und kann entweder aus einem benutzerdefinierten Bild oder aus einer systemdefinierten Stecknadel in verschiedenen Farben erstellt werden. Anmerkungen können optional eine *Legende* anzeigen. Diese wird angezeigt, wenn der Benutzer eine Anmerkung auswählt. Die Legende zeigt die `Label`- und `Address`-Eigenschaften der `Pin`-Instanz mit optionalen zusätzlichen Ansichten auf der linken oder rechten Seite an. Auf dem obigen Screenshot wird auf der linken Seite zusätzlich das Bild eines Affen angezeigt, während auf der rechten Seite eine *Informationsschaltfläche* angezeigt wird.

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die iOS-Plattform veranschaulicht:

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

            if (e.OldElement != null)
            {
                var nativeMap = Control as MKMapView;
                if (nativeMap != null)
                {
                    nativeMap.RemoveAnnotations(nativeMap.Annotations);
                    nativeMap.GetViewForAnnotation = null;
                    nativeMap.CalloutAccessoryControlTapped -= OnCalloutAccessoryControlTapped;
                    nativeMap.DidSelectAnnotationView -= OnDidSelectAnnotationView;
                    nativeMap.DidDeselectAnnotationView -= OnDidDeselectAnnotationView;
                }
            }

            if (e.NewElement != null)
            {
                var formsMap = (CustomMap)e.NewElement;
                var nativeMap = Control as MKMapView;
                customPins = formsMap.CustomPins;

                nativeMap.GetViewForAnnotation = GetViewForAnnotation;
                nativeMap.CalloutAccessoryControlTapped += OnCalloutAccessoryControlTapped;
                nativeMap.DidSelectAnnotationView += OnDidSelectAnnotationView;
                nativeMap.DidDeselectAnnotationView += OnDidDeselectAnnotationView;
            }
        }
        // ...
    }
}
```

Die `OnElementChanged`-Methode führt die folgende Konfiguration der [`MKMapView`](xref:MapKit.MKMapView)-Instanz durch, wenn der benutzerdefinierte Renderer einem neuen Xamarin.Forms-Element angefügt wurde:

- Die [`GetViewForAnnotation`](xref:MapKit.MKMapView.GetViewForAnnotation*)-Eigenschaft wird auf die `GetViewForAnnotation`-Methode festgelegt. Diese Methode wird aufgerufen, wenn die [Position der Anmerkung auf der Karte angezeigt wird](#Displaying_the_Annotation). Sie wird dann verwendet, um die Anmerkung anzupassen, bevor sie angezeigt wird.
- Ereignishandler für die Ereignisse `CalloutAccessoryControlTapped`, `DidSelectAnnotationView` und `DidDeselectAnnotationView` werden registriert. Diese Ereignisse werden ausgelöst, wenn der Benutzer auf das [rechte zusätzliche Element in der Legende tippt](#Tapping_on_the_Right_Callout_Accessory_View) und wenn der Benutzer die Anmerkung [auswählt](#Selecting_the_Annotation) bzw. die [Auswahl aufhebt](#Deselecting_the_Annotation). Das Abonnement der Ereignisse werden nur gekündigt, wenn das Element, dem der Renderer angefügt ist, geändert wird.

<a name="Displaying_the_Annotation" />

#### <a name="displaying-the-annotation"></a>Anzeigen der Anmerkung

Die `GetViewForAnnotation`-Methode wird aufgerufen, wenn die Position der Anmerkung auf der Karte angezeigt wird. Sie wird dann verwendet, um die Anmerkung anzupassen, bevor sie angezeigt wird. Eine Anmerkung besteht aus zwei Teilen:

- `MkAnnotation`: enthält den Titel, Untertitel und die Position der Anmerkung
- `MkAnnotationView`: enthält das Bild, das für die Darstellung der Anmerkung verwendet wird, und optional eine Legende, die angezeigt wird, wenn der Benutzer auf die Anmerkung tippt

Die `GetViewForAnnotation`-Methode akzeptiert eine `IMKAnnotation`-Schnittstelle, die die Daten der Anmerkung enthält und eine `MKAnnotationView`-Klasse für die Darstellung der Karte zurückgibt. Dies wird im folgenden Codebeispiel veranschaulicht:

```csharp
protected override MKAnnotationView GetViewForAnnotation(MKMapView mapView, IMKAnnotation annotation)
{
    MKAnnotationView annotationView = null;

    if (annotation is MKUserLocation)
        return null;

    var customPin = GetCustomPin(annotation as MKPointAnnotation);
    if (customPin == null)
    {
        throw new Exception("Custom pin not found");
    }

    annotationView = mapView.DequeueReusableAnnotation(customPin.Name);
    if (annotationView == null)
    {
        annotationView = new CustomMKAnnotationView(annotation, customPin.Name);
        annotationView.Image = UIImage.FromFile("pin.png");
        annotationView.CalloutOffset = new CGPoint(0, 0);
        annotationView.LeftCalloutAccessoryView = new UIImageView(UIImage.FromFile("monkey.png"));
        annotationView.RightCalloutAccessoryView = UIButton.FromType(UIButtonType.DetailDisclosure);
        ((CustomMKAnnotationView)annotationView).Name = customPin.Name;
        ((CustomMKAnnotationView)annotationView).Url = customPin.Url;
    }
    annotationView.CanShowCallout = true;

    return annotationView;
}
```

Durch diese Methode wird sichergestellt, dass die Anmerkung als benutzerdefiniertes Bild statt als systemdefinierte Stecknadel angezeigt wird und dass eine Legende angezeigt wird, wenn der Benutzer auf die Anmerkung tippt, die zusätzliche Inhalte rechts und links vom Titel und der Adresse der Anmerkung anzeigt. Dies kann folgendermaßen erreicht werden:

1. Die `GetCustomPin`-Methode wird aufgerufen, um die benutzerdefinierten Stecknadeldaten für die Anmerkung zurückzugeben.
1. Die Ansicht der Anmerkung wird für die Wiederverwendung mit einem Aufruf von [`DequeueReusableAnnotation`](xref:MapKit.MKMapView.DequeueReusableAnnotation*) gepoolt, um Arbeitsspeicher zu sparen.
1. Die `CustomMKAnnotationView`-Klasse erweitert die `MKAnnotationView`-Klasse mit `Name`- und `Url`-Eigenschaften, die den identischen Eigenschaften in der `CustomPin`-Instanz entsprechen. Eine neue Instanz von `CustomMKAnnotationView` wird erstellt, wenn die Anmerkung `null` ist:
    - Die `CustomMKAnnotationView.Image`-Eigenschaft wird auf das Bild festgelegt, das die Anmerkung auf der Karte darstellen soll.
    - Die `CustomMKAnnotationView.CalloutOffset`-Eigenschaft wird auf eine `CGPoint`-Struktur festgelegt, die angibt, dass die Legende über der Anmerkung zentriert wird.
    - Die `CustomMKAnnotationView.LeftCalloutAccessoryView`-Eigenschaft wird auf ein Bild von einem Affen festgelegt, der links von Titel und Adresse der Anmerkung angezeigt wird.
    - Die `CustomMKAnnotationView.RightCalloutAccessoryView`-Eigenschaft wird auf eine *Informationsschaltfläche* festgelegt, die rechts von Titel und Adresse der Anmerkung angezeigt wird.
    - Die `CustomMKAnnotationView.Name`-Eigenschaft wird auf die `CustomPin.Name`-Eigenschaft festgelegt, die von der `GetCustomPin`-Methode zurückgegeben wird. Dadurch kann die Anmerkung identifiziert und die [Legende weiter angepasst](#Selecting_the_Annotation) werden (falls gewünscht).
    - Die `CustomMKAnnotationView.Url`-Eigenschaft wird auf die `CustomPin.Url`-Eigenschaft festgelegt, die von der `GetCustomPin`-Methode zurückgegeben wird. Zu dieser URL wird navigiert, wenn der Benutzer [auf die Schaltfläche tippt, die in der rechten zusätzlichen Ansicht der Legende angezeigt wird](#Tapping_on_the_Right_Callout_Accessory_View).
1. Die [`MKAnnotationView.CanShowCallout`](xref:MapKit.MKAnnotationView.CanShowCallout*)-Eigenschaft wird auf `true` festgelegt, sodass die Legende angezeigt wird, wenn die Anmerkung angetippt wird.
1. Die Anmerkung wird zurückgegeben, um auf der Karte angezeigt werden zu können.

<a name="Selecting_the_Annotation" />

#### <a name="selecting-the-annotation"></a>Auswählen der Anmerkung

Wenn der Benutzer auf die Anmerkung tippt, wird das `DidSelectAnnotationView`-Ereignis ausgelöst, das dann die `OnDidSelectAnnotationView`-Methode ausgeführt:

```csharp
void OnDidSelectAnnotationView(object sender, MKAnnotationViewEventArgs e)
{
    CustomMKAnnotationView customView = e.View as CustomMKAnnotationView;
    customPinView = new UIView();

    if (customView.Name.Equals("Xamarin"))
    {
        customPinView.Frame = new CGRect(0, 0, 200, 84);
        var image = new UIImageView(new CGRect(0, 0, 200, 84));
        image.Image = UIImage.FromFile("xamarin.png");
        customPinView.AddSubview(image);
        customPinView.Center = new CGPoint(0, -(e.View.Frame.Height + 75));
        e.View.AddSubview(customPinView);
    }
}
```

Diese Methode erweitert die vorhandene Legende (die zusätzliche Ansichten auf der rechten und linken Seite enthält), indem eine `UIView`-Instanz hinzugefügt wird, die ein Bild des Xamarin-Logos enthält, sofern die `Name`-Eigenschaft der Anmerkung auf `Xamarin` festgelegt wurde. Dadurch können unterschiedliche Legenden für unterschiedliche Anmerkungen angezeigt werden. Die `UIView`-Instanz wird zentriert über der vorhandenen Legende angezeigt.

<a name="Tapping_on_the_Right_Callout_Accessory_View" />

#### <a name="tapping-on-the-right-callout-accessory-view"></a>Tippen auf die rechte zusätzliche Ansicht der Legende

Wenn der Benutzer auf die *Informationsschaltfläche* der rechten zusätzlichen Ansicht der Legende tippt, wird das `CalloutAccessoryControlTapped`-Ereignis ausgelöst, das dann die `OnCalloutAccessoryControlTapped`-Methode ausführt:

```csharp
void OnCalloutAccessoryControlTapped(object sender, MKMapViewAccessoryTappedEventArgs e)
{
    CustomMKAnnotationView customView = e.View as CustomMKAnnotationView;
    if (!string.IsNullOrWhiteSpace(customView.Url))
    {
        UIApplication.SharedApplication.OpenUrl(new Foundation.NSUrl(customView.Url));
    }
}
```

Diese Methode öffnet einen Webbrowser und navigiert zu der Adresse, die in der `CustomMKAnnotationView.Url`-Eigenschaft gespeichert ist. Beachten Sie, dass die Adresse definiert wurde, als die `CustomPin`-Collection im .NET Standard-Bibliotheksprojekt erstellt wurde.

<a name="Deselecting_the_Annotation" />

#### <a name="deselecting-the-annotation"></a>Aufheben der ausgewählten Anmerkung

Wenn die Anmerkung angezeigt wird und der Benutzer auf die Karte tippt, wird das `DidDeselectAnnotationView`-Ereignis ausgelöst, das dann die `OnDidDeselectAnnotationView`-Methode ausgeführt:

```csharp
void OnDidDeselectAnnotationView(object sender, MKAnnotationViewEventArgs e)
{
    if (!e.View.Selected)
    {
        customPinView.RemoveFromSuperview();
        customPinView.Dispose();
        customPinView = null;
    }
}
```

Durch diese Methode wird sichergestellt, dass die vorhandene Legende nicht ausgewählt ist, der erweiterte Bereich der Legende (das Xamarin-Logo) nicht mehr angezeigt wird und die Ressourcen freigegeben werden.

Weitere Informationen zum Anpassen von einer `MKMapView`-Instanz finden Sie unter [Maps in Xamarin.iOS (Karten in Xamarin.iOS)](~/ios/user-interface/controls/ios-maps/index.md).

### <a name="creating-the-custom-renderer-on-android"></a>Erstellen des benutzerdefinierten Renderers unter Android

Auf den folgenden Screenshots ist die Karte vor und nach der Anpassung zu sehen:

![](customized-pin-images/map-layout-android.png "Map Control Before and After Customization")

Unter Android wird die Stecknadel als *Markierung* bezeichnet und kann entweder aus einem benutzerdefinierten Bild oder aus einer systemdefinierten Markierung in verschiedenen Farben erstellt werden. Markierungen können ein *Infofenster* anzeigen, wenn der Benutzer darauf tippt. Das Infofenster zeigt die `Label`- und `Address`-Eigenschaften der `Pin`-Instanz an und kann auch auf andere Inhalte angepasst werden. Es kann jedoch nur ein Infofenster gleichzeitig angezeigt werden.

Im folgenden Codebeispiel wird der benutzerdefinierte Renderer für die Android-Plattform veranschaulicht:

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

Die `OnElementChanged`-Methode ruft die Liste benutzerdefinierter Stecknadeln vom Steuerelement ab, sofern der benutzerdefinierte Renderer an ein neues Xamarin.Forms-Element angefügt ist. Sobald die `GoogleMap`-Instanz verfügbar ist, wird die `OnMapReady`-Überschreibung aufgerufen. Diese Methode registriert einen Ereignishandler für das `InfoWindowClick`-Ereignis, das ausgelöst wird, wenn [auf das Infofenster geklickt wird](#Clicking_on_the_Info_Window). Das Abonnement für das Ereignis wird nur gekündigt, wenn das Element, dem der Renderer angefügt ist, sich ändert. Die `OnMapReady`-Überschreibung ruft außerdem die `SetInfoWindowAdapter`-Methode auf, um festzulegen, dass die `CustomMapRenderer`-Klasseninstanz die Methoden bereitstellt, die zur Anpassung des Infofensters erforderlich sind.

Die `CustomMapRenderer`-Klasse implementiert die `GoogleMap.IInfoWindowAdapter`-Schnittstelle, um [das Infofenster anzupassen](#Customizing_the_Info_Window). Diese Schnittstelle gibt an, dass folgende Methoden implementiert werden müssen:

- `public Android.Views.View GetInfoWindow(Marker marker)`: Diese Methode wird aufgerufen, um ein benutzerdefiniertes Infofenster für eine Markierung zurückzugeben. Wenn `null` zurückgegeben wird, wird das Fenster standardmäßig gerendert. Wenn `View` zurückgegeben wird, wird `View` im Rahmen des Infofensters platziert.
- `public Android.Views.View GetInfoContents(Marker marker)`: Diese Methode wird aufgerufen, um eine `View`-Klasse zurückzugeben, die den Inhalt des Infofensters enthält. Diese wird nur aufgerufen, wenn die `GetInfoWindow`-Methode `null` zurückgibt. Wenn `null` zurückgegeben wird, wird der Inhalt des Infofensters standardmäßig gerendert.

In der Beispielanwendung wird nur der Inhalt des Infofensters angepasst. Hierfür gibt die `GetInfoWindow`-Methode `null` zurück.

#### <a name="customizing-the-marker"></a>Anpassen der Markierung

Das Symbol, das zur Darstellung einer Markierung verwendet wird, kann durch den Aufruf der `MarkerOptions.SetIcon`-Methode angepasst werden. Hierfür müssen Sie die `CreateMarker`-Methode überschreiben, die für jedes `Pin`-Element aufgerufen wird, das zur Karte hinzugefügt wird:

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

Diese Methode erstellt eine neue `MarkerOption`-Instanz für jede `Pin`-Instanz. Nachdem Sie die Position, die Bezeichnung und die Adresse der Markierung festgelegt haben, wird das Symbol mit der `SetIcon`-Methode festgelegt. Diese Methode verwendet ein `BitmapDescriptor`-Objekt, das die Daten enthält, die zum Rendern des Symbols erforderlich sind, mit der `BitmapDescriptorFactory`-Klasse, die Hilfsmethoden bereitstellt, um die Erstellung von `BitmapDescriptor` zu vereinfachen. Weitere Informationen zur Verwendung der `BitmapDescriptorFactory`-Klasse für das Anpassen einer Markierung finden Sie unter [Customizing a Marker (Anpassen einer Markierung)](~/android/platform/maps-and-location/maps/maps-api.md).

> [!NOTE]
> Falls erforderlich kann die `GetMarkerForPin`-Methode im Renderer der Karte aufgerufen werden, um `Marker` von `Pin` abzurufen.

<a name="Customizing_the_Info_Window" />

#### <a name="customizing-the-info-window"></a>Anpassen des Infofensters

Wenn ein Benutzer auf die Markierung tippt, wird die `GetInfoContents`-Methode ausgeführt, sofern die `GetInfoWindow`-Methode `null` zurückgibt. Die `GetInfoContents`-Methode wird in folgendem Codebeispiel veranschaulicht:

```csharp
public Android.Views.View GetInfoContents(Marker marker)
{
    var inflater = Android.App.Application.Context.GetSystemService(Context.LayoutInflaterService) as Android.Views.LayoutInflater;
    if (inflater != null)
    {
        Android.Views.View view;

        var customPin = GetCustomPin(marker);
        if (customPin == null)
        {
            throw new Exception("Custom pin not found");
        }

        if (customPin.Name.Equals("Xamarin"))
        {
            view = inflater.Inflate(Resource.Layout.XamarinMapInfoWindow, null);
        }
        else
        {
            view = inflater.Inflate(Resource.Layout.MapInfoWindow, null);
        }

        var infoTitle = view.FindViewById<TextView>(Resource.Id.InfoWindowTitle);
        var infoSubtitle = view.FindViewById<TextView>(Resource.Id.InfoWindowSubtitle);

        if (infoTitle != null)
        {
            infoTitle.Text = marker.Title;
        }
        if (infoSubtitle != null)
        {
            infoSubtitle.Text = marker.Snippet;
        }

        return view;
    }
    return null;
}
```

Diese Methode gibt eine `View`-Klasse zurück, die den Inhalt des Infofensters enthält. Dies kann folgendermaßen erreicht werden:

- Eine `LayoutInflater`-Instanz wird abgerufen. Diese wird verwendet, um eine XML-Layoutdatei in die entsprechende `View`-Klasse zu instanziieren.
- Die `GetCustomPin`-Methode wird aufgerufen, um die benutzerdefinierten Stecknadeldaten für das Infofenster zurückzugeben.
- Das `XamarinMapInfoWindow`-Layout wird vergrößert, wenn die `CustomPin.Name`-Eigenschaft `Xamarin` entspricht. Andernfalls wird das `MapInfoWindow`-Layout vergrößert. Dadurch können unterschiedliche Infofensterlayouts für unterschiedliche Markierungen angezeigt werden.
- Die `InfoWindowTitle`- und `InfoWindowSubtitle`-Ressourcen werden vom vergrößerten Layout abgerufen, und ihre `Text`-Eigenschaften werden auf die entsprechenden Daten der `Marker`-Instanz festgelegt, sofern die Ressourcen nicht den Wert `null` aufweisen.
- Die `View`-Instanz wird zurückgegeben, um auf der Karte angezeigt werden zu können.

> [!NOTE]
> Bei einem Infofenster handelt es sich nicht um eine `View`-Liveklasse. Android konvertiert die `View`-Klasse stattdessen in eine statische Bitmap und zeigt diese als Bild an. Das bedeutet, dass ein Infofenster zwar auf Klicken reagieren kann, aber nicht auf Tippen oder Gesten. Zudem können die einzelnen Steuerelemente des Infofensters nicht auf ihre eigenen Klickereignisse reagieren.

<a name="Clicking_on_the_Info_Window" />

#### <a name="clicking-on-the-info-window"></a>Klicken auf das Infofenster

Wenn der Benutzer auf das Infofenster klickt, wird das `InfoWindowClick`-Ereignis ausgelöst, das dann die `OnInfoWindowClick`-Methode ausgeführt:

```csharp
void OnInfoWindowClick(object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    var customPin = GetCustomPin(e.Marker);
    if (customPin == null)
    {
        throw new Exception("Custom pin not found");
    }

    if (!string.IsNullOrWhiteSpace(customPin.Url))
    {
        var url = Android.Net.Uri.Parse(customPin.Url);
        var intent = new Intent(Intent.ActionView, url);
        intent.AddFlags(ActivityFlags.NewTask);
        Android.App.Application.Context.StartActivity(intent);
    }
}
```

Diese Methode öffnet einen Webbrowser und navigiert zu der Adresse, die in der `Url`-Eigenschaft der abgerufenen `CustomPin`-Instanz für `Marker` gespeichert ist. Beachten Sie, dass die Adresse definiert wurde, als die `CustomPin`-Collection im .NET Standard-Bibliotheksprojekt erstellt wurde.

Weitere Informationen zum Anpassen von einer `MapView`-Instanz finden Sie unter [Using the Google Maps API (Verwenden der Google Maps-API)](~/android/platform/maps-and-location/maps/maps-api.md).

### <a name="creating-the-custom-renderer-on-the-universal-windows-platform"></a>Erstellen des benutzerdefinierten Renderers auf der Universellen Windows-Plattform

Auf den folgenden Screenshots ist die Karte vor und nach der Anpassung zu sehen:

![](customized-pin-images/map-layout-uwp.png "Map Control Before and After Customization")

Auf der Universellen Windows-Plattform wird die Stecknadel als *Kartensymbol* bezeichnet und kann aus einem benutzerdefinierten Bild oder einem systemdefinierten Standardbild erstellt werden. Ein Kartensymbol kann eine `UserControl`-Klasse anzeigen, wenn der Benutzer darauf tippt. Die `UserControl`-Klasse kann sämtliche Inhalte darstellen, einschließlich den `Label`- und `Address`-Eigenschaften der `Pin`-Instanz.

Im folgenden Codebeispiel wird der benutzerdefinierte UWP-Renderer veranschaulicht:

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

Die `OnElementChanged`-Methode führt die folgenden Vorgänge durch, vorausgesetzt der benutzerdefinierte Renderer wurde an ein neues Xamarin.Forms-Element angefügt:

- Dadurch wird die `MapControl.Children`-Collection gelöscht, um alle vorhandenen Benutzeroberflächenelemente von der Karte zu löschen, bevor ein Ereignishandler für das `MapElementClick`-Ereignis registriert wird. Dieses Ereignis wird ausgelöst, wenn der Benutzer auf `MapElement` in `MapControl` tippt oder klickt, und das Abonnement für das Ereignis wird nur gekündigt, wenn das Element, dem der Renderer angefügt ist, sich ändert.
- Jede Stecknadel in der `customPins`-Collection wird folgendermaßen als der richtige geografische Standort auf der Karte angezeigt:
  - Die Position der Stecknadel wird als `Geopoint`-Instanz erstellt.
  - Eine `MapIcon`-Instanz wird erstellt, um die Stecknadel darzustellen.
  - Das Bild verwendet, um das `MapIcon`-Element darzustellen, das über die `MapIcon.Image`-Eigenschaft festgelegt wird. Das Bild des Kartensymbols wird jedoch nicht immer angezeigt. Möglicherweise wird es von anderen Elementen auf der Karte verdeckt. Legen Sie die `CollisionBehaviorDesired`-Eigenschaft des Kartensymbols auf `MapElementCollisionBehavior.RemainVisible` fest, damit es sichtbar bleibt.
  - Die Position von `MapIcon` wird über die `MapIcon.Location`-Eigenschaft festgelegt.
  - Die `MapIcon.NormalizedAnchorPoint`-Eigenschaft wird auf die ungefähre Position des Zeigers auf dem Bild festgelegt. Wenn die Eigenschaft ihren Standardwert von (0,0) beibehält, der der oberen linken Ecke des Bilds entspricht, kann das Ändern des Zoomfaktors der Karte dazu führen, dass das Bild eine andere Position darstellt.
  - Die `MapIcon`-Instanz wird zur `MapControl.MapElements`-Collection hinzugefügt. Dadurch wird das Kartensymbol auf dem `MapControl`-Element angezeigt.

> [!NOTE]
> Wenn Sie das gleiche Bild für mehrere Kartensymbole verwenden, sollte die `RandomAccessStreamReference`-Instanz auf Seiten- oder Anwendungsebene deklariert werden, um eine optimale Leistung zu erzielen.

#### <a name="displaying-the-usercontrol"></a>Anzeigen des UserControl-Elements

Wenn ein Benutzer auf das Kartensymbol tippt, wird die `OnMapElementClick`-Methode ausgeführt. Im folgenden Codebeispiel wird diese Methode veranschaulicht:

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

            if (customPin.Name.Equals("Xamarin"))
            {
                if (mapOverlay == null)
                {
                    mapOverlay = new XamarinMapOverlay(customPin);
                }

                var snPosition = new BasicGeoposition { Latitude = customPin.Position.Latitude, Longitude = customPin.Position.Longitude };
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

Die Methode erstellt eine `UserControl`-Instanz, die Informationen über die Stecknadel anzeigt. Dies kann folgendermaßen erreicht werden:

- Die `MapIcon`-Instanz wird abgerufen.
- Die `GetCustomPin`-Methode wird aufgerufen, um die benutzerdefinierten Stecknadeldaten zurückzugeben, die angezeigt werden.
- Eine `XamarinMapOverlay`-Instanz wird erstellt, um die benutzerdefinierten Stecknadeldaten anzuzeigen. Diese Klasse ist ein Benutzersteuerelement.
- Der geografische Standort, an dem die `XamarinMapOverlay`-Instanz auf dem `MapControl`-Element angezeigt werden soll, wird als `Geopoint`-Instanz erstellt.
- Die `XamarinMapOverlay`-Instanz wird zur `MapControl.Children`-Collection hinzugefügt. Diese Collection enthält XAML-Benutzeroberflächenelemente, die auf der Karte angezeigt werden.
- Der geografische Standort der `XamarinMapOverlay`-Instanz auf der Karte wird durch einen Aufruf der `SetLocation`-Methode festgelegt.
- Die relative Position auf der `XamarinMapOverlay`-Instanz, die der angegebenen Position entspricht, wird durch einen Aufruf der `SetNormalizedAnchorPoint`-Methode festgelegt. Dadurch wird sichergestellt, dass die `XamarinMapOverlay`-Instanz immer an der richtigen Position angezeigt wird, auch wenn der Zoomfaktor der Karte geändert wird.

Alternativ entfernt das Tippen auf der Karte die `XamarinMapOverlay`-Instanz aus der `MapControl.Children`-Collection, wenn Informationen über die Stecknadel bereits auf der Karte angezeigt werden.

#### <a name="tapping-on-the-information-button"></a>Tippen auf die Informationsschaltfläche

Wenn der Benutzer auf die *Informationsschaltfläche* des `XamarinMapOverlay`-Benutzersteuerelements tippt, wird das `Tapped`-Ereignis ausgelöst, das dann die `OnInfoButtonTapped`-Methode ausführt:

```csharp
private async void OnInfoButtonTapped(object sender, TappedRoutedEventArgs e)
{
    await Launcher.LaunchUriAsync(new Uri(customPin.Url));
}
```

Diese Methode öffnet einen Webbrowser und navigiert zu der Adresse, die in der `Url`-Eigenschaft der `CustomPin`-Instanz gespeichert ist. Beachten Sie, dass die Adresse definiert wurde, als die `CustomPin`-Collection im .NET Standard-Bibliotheksprojekt erstellt wurde.

Weitere Informationen zum Anpassen von einer `MapControl`-Instanz finden Sie auf MSDN unter [Übersicht über Karten und Position](https://msdn.microsoft.com/library/windows/apps/mt219699.aspx).

## <a name="related-links"></a>Verwandte Links

- [Maps Control (Kartensteuerelement)](~/xamarin-forms/user-interface/map/index.md)
- [Maps in Xamarin.iOS (Karten in Xamarin.iOS)](~/ios/user-interface/controls/ios-maps/index.md)
- [Karten-API](~/android/platform/maps-and-location/maps/maps-api.md)
- [Customized Pin (Benutzerdefinierte Stecknadel (Beispiel))](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/customrenderers-map-pin)
