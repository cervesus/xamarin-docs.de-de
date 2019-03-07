---
title: Xamarin.Forms-Karte
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms-Map-Klasse verwenden, mit der systemeigenen Zuordnungs-APIs auf jeder Plattform bereitstellen, dass eine vertraute Erfahrung für Benutzer zugeordnet wird.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/27/2018
ms.openlocfilehash: edba18eea3ea2b7b843dba70ff0b4b67cbab1ab1
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557115"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms-Karte

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/WorkingWithMaps/)

_Xamarin.Forms nutzt die native APIs-Zuordnung auf jeder Plattform._

Xamarin.Forms.Maps verwendet die systemeigenen Zuordnungs-APIs auf jeder Plattform. Dies bietet eine schnelle, vertraute Maps-Oberfläche für Benutzer, aber das bedeutet, dass einige Konfigurationsschritte erforderlich sind, um die einzelnen Plattformen-API-Anforderungen erfüllen.
Nach der Konfiguration, die `Map` funktionieren genauso wie alle anderen Xamarin.Forms-Elemente im gemeinsamen Code zu steuern.

Wurde das Map-Steuerelement verwendet die [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) Beispiel unten dargestellt ist.

 [![Karten in diesem Beispiel MobileCRM](map-images/maps-zoom-sml.png "Steuerelement Beispiel")](map-images/maps-zoom.png#lightbox "Map-Steuerelement-Beispiel")

Map-Funktionalität kann weiter verbessert werden, indem Sie erstellen eine [zuordnen benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Maps-Initialisierung

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

-  **iOS** -Datei "appdelegate.cs"-Datei, in der `FinishedLaunching` Methode.
-  **Android** -"mainactivity.cs"-Datei, in der `OnCreate` Methode.
-  **UWP** -Datei "MainPage.Xaml.cs"-Datei, in der `MainPage` Konstruktor.

Sobald das NuGet-Paket hinzugefügt wurde, und jede Anwendung, die Initialisierungsmethode aufgerufen `Xamarin.Forms.Maps` APIs können in der allgemeinen .NET Standard Library-Projekt oder ein freigegebenes Projekt Code verwendet werden.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Plattformkonfiguration

Auf einigen Plattformen sind zusätzliche Konfigurationsschritte erforderlich, bevor die Karte angezeigt werden.

### <a name="ios"></a>iOS

Für den Zugriff auf Standortdienste unter iOS, müssen Sie die folgenden Schlüssel in festlegen **"Info.plist"**:

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

Ohne einen gültigen API-Schlüssel wird der Maps-Steuerelement als ein graues Feld unter Android angezeigt.

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

Die beiden letzten sind erforderlich, da Anwendungen eine Netzwerkverbindung zum Herunterladen der Sitemap-Daten erforderlich sind. Erfahren Sie mehr über Android [Berechtigungen](http://developer.android.com/reference/android/Manifest.permission.html) um mehr zu erfahren.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Um die Zuordnungen für die universelle Windows-Plattform zu verwenden, müssen Sie ein Autorisierungstoken, das generieren. Weitere Informationen finden Sie unter [fordern Sie einen Maps-Authentifizierungsschlüssel](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) auf MSDN.

Das Authentifizierungstoken sollte dann angegeben werden, der `FormsMaps.Init("AUTHORIZATION_TOKEN")` -Methodenaufruf, um die app mit Bing Maps zu authentifizieren.

<a name="Using_Maps" />

## <a name="using-maps"></a>Verwenden von Zuordnungen

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

### <a name="map-type"></a>Zuordnungstyp

Inhalt der Zuordnungsdatei kann auch geändert werden, durch Festlegen der `MapType` Eigenschaft verwenden, um eine reguläre Straße und Zuordnung (Standard), Satelliten-Bilder oder eine Kombination aus beidem angezeigt.

```csharp
map.MapType == MapType.Street;
```

Gültige `MapType` Werte sind:

-  Hybrid
-  Satellit
-  Straße (Standard)

### <a name="map-region-and-mapspan"></a>Map-Region und MapSpan

Wie im obigen Codeausschnitt dargestellt, Angeben einer `MapSpan` Instanz für einen Map-Konstruktor legt die anfängliche Ansicht (Mittelpunkt und Zoomstufe) der Karte, wenn es geladen wird. Die `MoveToRegion` Methode für die Map-Klasse kann dann verwendet werden, um die Position oder Zoomen der Karte ändern. Es gibt zwei Möglichkeiten zum Erstellen eines neuen `MapSpan` Instanz:

-  **MapSpan.FromCenterAndRadius()** -statische Methode zum Erstellen von einem Bereich eine `Position` und Angeben einer `Distance` .
-  **neue MapSpan ()** -Konstruktor, der verwendet eine `Position` und den Grad der Breiten- und Längengrad angezeigt.


Um die Zoomstufe der Karte ändern, ohne den Speicherort ändern, erstellen Sie ein neues `MapSpan` mit der aktuellen Position aus der `VisibleRegion.Center` Eigenschaft das Map-Steuerelement. Ein `Slider` verwendet werden, um die Karte Zoomen wie folgt steuern (jedoch direkt in das Map-Steuerelement Zoomen derzeit den Wert des Schiebereglers update kann nicht):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Karten mit Zoom](map-images/maps-zoom-sml.png "Map-Steuerelement Zoom")](map-images/maps-zoom.png#lightbox "Zoom-Map-Steuerelement")

### <a name="map-pins"></a>Map-pins

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

 `PinType` kann auf eine der folgenden Werte an, festgelegt werden die die Möglichkeit, die die Pin (abhängig von der Plattform) gerendert wird, beeinträchtigen können:

-  Generisch
-  Ort
-  SavedPin
-  SearchResult

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Mithilfe von XAML

Maps können auch in XAML-Layouts positioniert werden, wie in diesem Codeausschnitt gezeigt.

```xaml
<?xml version="1.0" encoding="UTF-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="MapDemo.MapPage">
    <StackLayout VerticalOptions="StartAndExpand" Padding="30">
        <maps:Map WidthRequest="320" HeightRequest="200"
                  x:Name="MyMap"
                  IsShowingUser="true"
                  MapType="Hybrid" />
    </StackLayout>
</ContentPage>
```

> [!NOTE]
> Ein zusätzliches `xmlns` Namespacedefinition ist erforderlich, um die Xamarin.Forms.Maps-Steuerelemente verweisen.

Die `MapRegion` und `Pins` kann festgelegt werden, im Code mithilfe der `MyMap` -Verweis (oder was auch immer dem Namen der Zuordnung).

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="populating-a-map-with-data-using-data-binding"></a>Auffüllen einer Karte mit Daten, die mithilfe der Datenbindung

Die [ `Map` ](xref:Xamarin.Forms.Maps.Map) Klasse macht auch die folgenden Eigenschaften verfügbar:

- `ItemsSource` – Gibt die Auflistung der `IEnumerable` Elemente, die angezeigt werden.
- `ItemTemplate` – Gibt an, die [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) auf jedes Element in der Auflistung der angezeigten Elemente anwenden.

Aus diesem Grund eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) mit Daten gefüllt werden kann, um mithilfe der Datenbindung binden die `ItemsSource` Eigenschaft, um eine `IEnumerable` Auflistung:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:maps="clr-namespace:Xamarin.Forms.Maps;assembly=Xamarin.Forms.Maps"
             x:Class="WorkingWithMaps.PinItemsSourcePage">
    <Grid>
        ...
        <maps:Map x:Name="map"
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

Die `ItemsSource` Eigenschaftendaten bindet an die `Locations` Eigenschaft des verbundenen Ansichtsmodell, wodurch ein `ObservableCollection` von `Location` -Objekte, die einen benutzerdefinierten Typ handelt. Jede `Location` Objekt definiert `Address` und `Description` Eigenschaften vom Typ `string`, und ein `Position` -Eigenschaft vom Typ [ `Position` ](xref:Xamarin.Forms.Maps.Position).

Die Darstellung der einzelnen Elemente in der `IEnumerable` Auflistung wird definiert, indem die `ItemTemplate` Eigenschaft, um eine [ `DataTemplate` ](xref:Xamarin.Forms.DataTemplate) , enthält eine [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) Objekt, das an Daten gebunden entsprechende Eigenschaften.

Die folgenden Screenshots zeigen eine [ `Map` ](xref:Xamarin.Forms.Maps.Map) Anzeigen einer [ `Pin` ](xref:Xamarin.Forms.Maps.Pin) Auflistung, die mithilfe der Datenbindung:

[![Screenshot der Karte mit Daten gebunden, Pins, die unter iOS und Android](map-images/pins-itemssource.png "Stecknadeln mit Daten gebundenen")](map-images/pins-itemssource-large.png#lightbox "Stecknadeln mit Daten gebundenen")

## <a name="related-links"></a>Verwandte Links

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Zuordnen von benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
