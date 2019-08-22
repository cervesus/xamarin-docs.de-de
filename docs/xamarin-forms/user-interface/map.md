---
title: Xamarin.Forms-Karte
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms-Map-Klasse verwenden, mit der systemeigenen Zuordnungs-APIs auf jeder Plattform bereitstellen, dass eine vertraute Erfahrung für Benutzer zugeordnet wird.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/18/2019
ms.openlocfilehash: 4cfedad6ccf87dfef819b677233be1edb2d2c62d
ms.sourcegitcommit: 5f972a757030a1f17f99177127b4b853816a1173
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/21/2019
ms.locfileid: "69887963"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms-Karte

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

_Xamarin.Forms nutzt die native APIs-Zuordnung auf jeder Plattform._

Xamarin.Forms.Maps verwendet die systemeigenen Zuordnungs-APIs auf jeder Plattform. Dies bietet Benutzern eine schnelle, vertraute Kartendarstellung, bedeutet jedoch, dass einige Konfigurationsschritte erforderlich sind, um die API-Anforderungen der einzelnen Plattformen einzuhalten.
Nach der Konfiguration, die `Map` funktionieren genauso wie alle anderen Xamarin.Forms-Elemente im gemeinsamen Code zu steuern.

Wurde das Map-Steuerelement verwendet die [MapsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps) Beispiel unten dargestellt ist.

 [![Karten in diesem Beispiel MobileCRM](map-images/maps-zoom-sml.png "Steuerelement Beispiel")](map-images/maps-zoom.png#lightbox "Map-Steuerelement-Beispiel")

Map-Funktionalität kann weiter verbessert werden, indem Sie erstellen eine [zuordnen benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="map-initialization"></a>Zuordnungs Initialisierung

Beim Hinzufügen von Zuordnungen zu einer Xamarin.Forms-Anwendung, **Xamarin.Forms.Maps** ist ein separates NuGet-Paket, die Sie für jedes Projekt in der Projektmappe hinzufügen sollten.
Unter Android bietet dies eine Abhängigkeit auf GooglePlayServices (eine andere NuGet) automatisch heruntergeladen wird, wenn Sie Xamarin.Forms.Maps hinzufügen.

Nach der Installation des NuGet-Pakets muss in jedem Anwendungsprojekt etwas Initialisierungscode *nach* der `Xamarin.Forms.Forms.Init` Methodenaufruf. Verwenden Sie für iOS den folgenden Code ein:

```csharp
Xamarin.FormsMaps.Init();
```

Für Android müssen Sie die gleichen Parameter wie übergeben `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Verwenden Sie für die universelle Windows-Plattform (UWP) den folgenden Code:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Fügen Sie diesen Aufruf in den folgenden Dateien für jede Plattform hinzu:

- **iOS** -Datei "appdelegate.cs"-Datei, in der `FinishedLaunching` Methode.
- **Android** -"mainactivity.cs"-Datei, in der `OnCreate` Methode.
- **UWP** -Datei "MainPage.Xaml.cs"-Datei, in der `MainPage` Konstruktor.

Nachdem das nuget-Paket hinzugefügt und die Initialisierungs Methode in jeder Anwendung aufgerufen wurde `Xamarin.Forms.Maps` , können APIs im Allgemeinen .NET Standard Bibliotheksprojekt oder im freigegebenen Projekt Code verwendet werden.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Platt Form Konfiguration

Auf einigen Plattformen sind zusätzliche Konfigurationsschritte erforderlich, bevor die Karte angezeigt werden.

### <a name="ios"></a>iOS

Für den Zugriff auf Standortdienste unter iOS, müssen Sie die folgenden Schlüssel in festlegen **"Info.plist"** :

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – für die Verwendung von ortungsdienste, wenn die app verwendet wird
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – für die Verwendung der Standortdienste jederzeit
- iOS 10 und früher
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – für die Verwendung von ortungsdienste, wenn die app verwendet wird
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – für die Verwendung der Standortdienste jederzeit    

Um iOS 11 und früheren zu unterstützen, können Sie alle drei Product Keys einschließen: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, und `NSLocationAlwaysUsageDescription`.

Die XML-Darstellung für diese Schlüssel im **"Info.plist"** ist unten dargestellt. Aktualisieren Sie die `string` Werte, um anzugeben, wie Ihre Anwendung die Informationen zum Speicherort verwendet:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

Die **"Info.plist"** Einträge können auch hinzugefügt werden, **Quelle** anzeigen, während der Bearbeitung der **"Info.plist"** Datei:

![Datei "Info.plist" für iOS 8](map-images/ios8-map-permissions.png "iOS 8 erforderliche Datei \"Info.plist\" Einträge")

### <a name="android"></a>Android

Verwenden der [Google Maps-API v2](https://developers.google.com/maps/documentation/android/) unter Android müssen Sie einen API-Schlüssel generieren und fügen Sie es auf Ihrem Android-Projekt hinzu.
Befolgen Sie die Anweisungen im Dokument Xamarin [ein Google Maps-API v2 Schlüssel](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Fügen Sie nach dem Ausführen dieser Anweisungen, die die API-Schlüssel in der **Properties/Androidmanifest.XML** -Datei (Quelltext anzeigen und mit dem folgenden Element suchen/aktualisieren):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Ohne einen gültigen API-Schlüssel wird das Karten-Steuerelement als graues Feld unter Android angezeigt.

> [!NOTE]
> Beachten Sie, dass in der Reihenfolge für das APK Zugriff auf Google Maps, müssen Sie SHA1-Fingerabdrücke gehören und Packen Namen für jede Keystore (Debug und Release), die Sie verwenden, um das APK zu signieren. Z. B. Wenn Sie einen Computer für Debug- und einem anderen Computer zum Generieren des Release-APK verwenden, sollten Sie den SHA-1-Fingerabdruck des Zertifikats aus dem debugkeystore des ersten Computers und der SHA-1-Fingerabdruck des Zertifikats aus dem Keystore Version der einschließen der zweite Computer. Beachten Sie außerdem die schlüsselanmeldeinformationen bearbeiten, wenn der app **Paketname** Änderungen. Finden Sie unter [ein Google Maps-API v2 Schlüssel](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Sie müssen außerdem Berechtigungen zu aktivieren, indem Sie mit der rechten Maustaste auf das Android-Projekt, und wählen **Optionen > Erstellen > Android-Anwendung** und an zu laufen, Folgendes:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Einige davon werden im folgenden Screenshot dargestellt:

![Die erforderlichen Berechtigungen für Android](map-images/android-map-permissions.png "erforderliche Berechtigungen für Android")

Die beiden letzten sind erforderlich, da Anwendungen eine Netzwerkverbindung zum Herunterladen der Sitemap-Daten erforderlich sind. Erfahren Sie mehr über Android [Berechtigungen](https://developer.android.com/reference/android/Manifest.permission.html) um mehr zu erfahren.

Außerdem hat Android 9 die Apache HTTP-Client Bibliothek aus dem bootclasspath entfernt und ist daher für Anwendungen, die auf API 28 oder höher ausgerichtet sind, nicht verfügbar. Die folgende Zeile muss dem- `application` Knoten der Datei " **androidmanifest. XML** " hinzugefügt werden, um den Apache HTTP-Client in Anwendungen mit der Ziel-API 28 oder höher zu verwenden:

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Um die Zuordnungen für die universelle Windows-Plattform zu verwenden, müssen Sie ein Autorisierungstoken, das generieren. Weitere Informationen finden Sie unter [fordern Sie einen Maps-Authentifizierungsschlüssel](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) auf MSDN.

Das Authentifizierungstoken sollte dann angegeben werden, der `FormsMaps.Init("AUTHORIZATION_TOKEN")` -Methodenaufruf, um die app mit Bing Maps zu authentifizieren.

<a name="Using_Maps" />

## <a name="map-configuration"></a>Zuordnungs Konfiguration

Finden Sie unter den [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) im MobileCRM Beispiel verdeutlicht, wie das Map-Steuerelement im Code verwendet werden kann. Eine einfache `MapPage` Klasse dieses - Hinweises aussehen könnte, ein neues `MapSpan` wird erstellt, um die position der Karte anzeigen:

```csharp
public class MapPage : ContentPage {
    public MapPage() {
        var map = new Map(
            MapSpan.FromCenterAndRadius(
                    new Position(37,-122), Distance.FromMiles(0.3))) {
                IsShowingUser = true,
                HeightRequest = 100,
                WidthRequest = 960,
                VerticalOptions = LayoutOptions.FillAndExpand
            };
        var stack = new StackLayout { Spacing = 0 };
        stack.Children.Add(map);
        Content = stack;
    }
}
```

### <a name="map-type"></a>Kartentyp

Inhalt der Zuordnungsdatei kann auch geändert werden, durch Festlegen der `MapType` Eigenschaft verwenden, um eine reguläre Straße und Zuordnung (Standard), Satelliten-Bilder oder eine Kombination aus beidem angezeigt.

```csharp
map.MapType = MapType.Street;
```

Gültige `MapType` Werte sind:

- `Hybrid`
- `Satellite`
- `Street` (Standard)

### <a name="map-region-and-mapspan"></a>Kartenbereich und Zuordnungs Spanne

Wie im obigen Codeausschnitt dargestellt, Angeben einer `MapSpan` Instanz für einen Map-Konstruktor legt die anfängliche Ansicht (Mittelpunkt und Zoomstufe) der Karte, wenn es geladen wird. Es gibt zwei Möglichkeiten zum Erstellen eines neuen `MapSpan` Instanz:

- **MapSpan.FromCenterAndRadius()** -statische Methode zum Erstellen von einem Bereich eine `Position` und Angeben einer `Distance` .
- **neue MapSpan ()** -Konstruktor, der verwendet eine `Position` und den Grad der Breiten- und Längengrad angezeigt.

Die `MoveToRegion` Methode für die Map-Klasse kann dann verwendet werden, um die Position oder Zoomen der Karte ändern. Um die Zoomstufe der Karte ändern, ohne den Speicherort ändern, erstellen Sie ein neues `MapSpan` mit der aktuellen Position aus der `VisibleRegion.Center` Eigenschaft das Map-Steuerelement. Ein `Slider` kann verwendet werden, um den Zuordnungs Zoom wie folgt zu steuern (es kann jedoch sein, dass der Wert des Schiebereglers durch Zoomen direkt im Karten Steuerelement nicht aktualisiert wird):

```csharp
Slider slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) =>
{
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

[![Karten mit Zoom](map-images/maps-zoom-sml.png "Map-Steuerelement Zoom")](map-images/maps-zoom.png#lightbox "Zoom-Map-Steuerelement")

Außerdem verfügt die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse über eine `MoveToLastRegionOnLayoutChange` Eigenschaft vom Typ `bool`, die durch eine bindbare Eigenschaft unterstützt wird. Standardmäßig ist `true`diese Eigenschaft. Dies bedeutet, dass der angezeigte Zuordnungs Bereich beim Auftreten einer Layoutänderung von seinem aktuellen Bereich in den zuvor festgelegten Bereich wechselt, z. b. bei der Geräte Rotation. Wenn diese Eigenschaft auf `false`festgelegt ist, bleibt der angezeigte Kartenbereich zentriert, wenn eine Layoutänderung auftritt. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

```csharp
map.MoveToLastRegionOnLayoutChange = false;
```

### <a name="map-pins"></a>Karten Pins

Standorte können markiert werden, auf der Karte mit `Pin` Objekte.

```csharp
var position = new Position(37,-122); // Latitude, Longitude
var pin = new Pin {
            Type = PinType.Place,
            Position = position,
            Label = "custom pin",
            Address = "custom detail info"
        };
map.Pins.Add(pin);
```

`PinType`kann auf einen der folgenden Werte festgelegt werden. Dies kann sich auf die Art und Weise auswirken, in der die PIN gerendert wird (je nach Plattform):

- Generisch
- Ort
- SavedPin
- SearchResult

### <a name="map-clicks"></a>Karten Klicks

`Map`definiert ein `MapClicked` -Ereignis, das ausgelöst wird, wenn die Karte abgetippt wird. Das `MapClickedEventArgs` Objekt, das das `MapClicked` Ereignis begleitet, verfügt über eine `Position`einzelne Eigenschaft namens `Position`vom Typ. Wenn das Ereignis ausgelöst wird, wird der Wert `Position` der-Eigenschaft auf den Zuordnungs Speicherort festgelegt, der abgetippt wurde.

Das folgende Codebeispiel zeigt einen Ereignishandler für das `MapClicked` -Ereignis:

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

In diesem Beispiel gibt der `OnMapClicked` -Ereignishandler die breiten-und Längengrade aus, die den angegeppten Zuordnungs Speicherort darstellen.

<a name="Using_Xaml" />

### <a name="create-a-map-in-xaml"></a>Erstellen einer Karte in XAML

Zuordnungen können auch in XAML erstellt werden, wie im folgenden Beispiel gezeigt:

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map x:Name="MyMap"
                  Clicked="OnMapClicked"
                  WidthRequest="320"
                  HeightRequest="200"                  
                  IsShowingUser="true"
                  MapType="Hybrid" />
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> Eine zusätzliche `xmlns` Namespace Definition ist erforderlich, um auf die xamarin. Forms. Maps-Steuerelemente zu verweisen.

Und können im Code mithilfe des benannten Verweises für das `Map`festgelegt werden: `Pins` `MapRegion`

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="populate-a-map-with-data-using-data-binding"></a>Auffüllen einer Zuordnung mit Daten mithilfe der Datenbindung

Die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse macht auch die folgenden Eigenschaften verfügbar:

- `ItemsSource`– Gibt die Sammlung von `IEnumerable` Elementen an, die angezeigt werden sollen.
- `ItemTemplate`– Gibt das [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) an, das auf jedes Element in der Auflistung der angezeigten Elemente angewendet werden soll.
- `ItemTemplateSelector`– Gibt das [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector) an, das zum Auswählen eines [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) für ein Element zur Laufzeit verwendet wird.

> [!NOTE]
> Die `ItemTemplate` -Eigenschaft hat Vorrang, wenn `ItemTemplate` die `ItemTemplateSelector` -Eigenschaft und die-Eigenschaft festgelegt sind.

Ein [`Map`](xref:Xamarin.Forms.Maps.Map) -Objekt kann mit Daten aufgefüllt werden, indem die-Eigenschaft `ItemsSource` mithilfe der Daten `IEnumerable` Bindung an eine-Auflistung gebunden wird:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
                  MoveToLastRegionOnLayoutChange="false"
                  ItemsSource="{Binding Locations}">
            <maps:Map.ItemTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </maps:Map.ItemTemplate>
        </maps:Map>
        ...
    </Grid>
</ContentPage>
```

Die `ItemsSource` Eigenschaften Daten werden an die `Locations` -Eigenschaft des verbundenen Ansichts Modells gebunden, das `ObservableCollection` eine `Location` von-Objekten zurückgibt, bei der es sich um einen benutzerdefinierten Typ handelt. Jedes `Location` -Objekt `Address` definiert `Description` die-Eigenschaft und `string`die-Eigenschaft `Position` vom Typ und eine [`Position`](xref:Xamarin.Forms.Maps.Position)-Eigenschaft vom Typ.

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung wird durch Festlegen der `ItemTemplate` -Eigenschaft auf eine [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) definiert, die [`Pin`](xref:Xamarin.Forms.Maps.Pin) ein-Objekt enthält, das von Daten an die entsprechenden Eigenschaften gebunden wird.

Die folgenden Screenshots zeigen [`Map`](xref:Xamarin.Forms.Maps.Map) , wie eine Auflistung mit Datenbindung [`Pin`](xref:Xamarin.Forms.Maps.Pin) angezeigt wird:

[ ![Screenshot der Karte mit Daten gebundenen Pins, auf IOS-und Android-](map-images/pins-itemssource.png "Karten mit Daten gebundenen Pins") ] Zuordnung (map-images/pins-itemssource-large.png#lightbox "mit Daten gebundenen Pins")

### <a name="choose-item-appearance-at-runtime"></a>Auswählen der Element Darstellung zur Laufzeit

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung kann zur Laufzeit basierend auf dem Elementwert ausgewählt werden, indem die `ItemTemplateSelector` -Eigenschaft auf [`DataTemplateSelector`](xref:Xamarin.Forms.DataTemplateSelector)festgelegt wird:

```xaml
<ContentPage ...
             xmlns:local="clr-namespace:WorkingWithMaps"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps">
    <ContentPage.Resources>
        <local:MapItemTemplateSelector x:Key="MapItemTemplateSelector">
            <local:MapItemTemplateSelector.DefaultTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="{Binding Description}" />
                </DataTemplate>
            </local:MapItemTemplateSelector.DefaultTemplate>
            <local:MapItemTemplateSelector.XamarinTemplate>
                <DataTemplate>
                    <maps:Pin Position="{Binding Position}"
                              Address="{Binding Address}"
                              Label="Xamarin!" />
                </DataTemplate>
            </local:MapItemTemplateSelector.XamarinTemplate>    
        </local:MapItemTemplateSelector>
    </ContentPage.Resources>

    <Grid>
        ...
        <maps:Map x:Name="map"
                  ItemsSource="{Binding Locations}"
                  ItemTemplateSelector="{StaticResource MapItemTemplateSelector}" />
        ...
    </Grid>
</ContentPage>
```

Das folgende Beispiel zeigt die `MapItemTemplateSelector` -Klasse:

```csharp
public class MapItemTemplateSelector : DataTemplateSelector
{
    public DataTemplate DefaultTemplate { get; set; }
    public DataTemplate XamarinTemplate { get; set; }

    protected override DataTemplate OnSelectTemplate(object item, BindableObject container)
    {
        return ((Location)item).Address.Contains("San Francisco") ? XamarinTemplate : DefaultTemplate;
    }
}
```

Die `MapItemTemplateSelector` -Klasse `DefaultTemplate` definiert `XamarinTemplate` die Eigenschaften und [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) , die auf unterschiedliche Datenvorlagen festgelegt sind. Die `OnSelectTemplate` -Methode gibt `XamarinTemplate`den zurück, der "xamarin" als Bezeichnung anzeigt, `Pin` wenn ein getippt wird, wenn das Element über eine Adresse verfügt, die "San Francisco" enthält. Wenn das Element nicht über eine Adresse verfügt, die "San Francisco" enthält `OnSelectTemplate` , `DefaultTemplate`gibt die Methode zurück.

Weitere Informationen zu Datenvorlagen-Selektoren finden Sie unter [Erstellen eines xamarin. Forms DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md).

## <a name="related-links"></a>Verwandte Links

- [MapsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Zuordnen von benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.Forms)
- [Erstellen eines xamarin. Forms-DataTemplateSelector](~/xamarin-forms/app-fundamentals/templates/data-templates/selector.md)
- [Karten-API](xref:Xamarin.Forms.Maps)
