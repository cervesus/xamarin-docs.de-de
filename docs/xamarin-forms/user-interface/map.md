---
title: Xamarin.Forms-Karte
description: In diesem Artikel wird erläutert, wie die Xamarin.Forms-Map-Klasse verwenden, mit der systemeigenen Zuordnungs-APIs auf jeder Plattform bereitstellen, dass eine vertraute Erfahrung für Benutzer zugeordnet wird.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: f787adcba78a13f6d4cad3fb446350a65e960aca
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50123608"
---
# <a name="xamarinforms-map"></a>Xamarin.Forms-Karte

_Xamarin.Forms nutzt die native APIs-Zuordnung auf jeder Plattform._

Xamarin.Forms.Maps verwendet die systemeigenen Zuordnungs-APIs auf jeder Plattform. Dies bietet eine schnelle, vertraute Maps-Oberfläche für Benutzer, aber das bedeutet, dass einige Konfigurationsschritte erforderlich sind, um die einzelnen Plattformen spezifischen API-Anforderungen entsprechen.
Nach der Konfiguration, die `Map` funktionieren genauso wie alle anderen Xamarin.Forms-Elemente im gemeinsamen Code zu steuern.

* [Ordnet die Initialisierung](#Maps_Initialization) – mit `Map` erfordert zusätzlichen Initialisierungscode beim Start.
* [Plattformkonfiguration](#Platform_Configuration) -jede Plattform erfordert eine Konfiguration für Maps arbeiten.
* [Verwenden von Zuordnungen in c#](#Using_Maps) -Anzeige zugeordnet und heftet mithilfe von c#.
* [Verwenden von Zuordnungen in XAML](#Using_Xaml) -Anzeigen einer Karte mit XAML.

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

Sobald das NuGet-Paket hinzugefügt wurde und die Initialisierungsmethode aufgerufen wird, innerhalb jeder Applcation, `Xamarin.Forms.Maps` APIs können in der allgemeinen .NET Standard Library-Projekt oder ein freigegebenes Projekt Code verwendet werden.

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

### <a name="map-pins"></a>Map-Pins

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

 `PinType` können auf einen der folgenden Werte festgelegt werden, die Einfluss auf die möglicherweise die Pin in (abhängig von der Plattform) gerendert:

-  Generisch
-  Ort
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Mithilfe von Xaml

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
            MapType="Hybrid"
        />
    </StackLayout>
</ContentPage>
```

Die `MapRegion` und `Pins` kann festgelegt werden, im Code mithilfe der `MyMap` -Verweis (oder was auch immer dem Namen der Zuordnung). Beachten Sie, dass ein zusätzliches `xmlns` Namespacedefinition ist erforderlich, um die Xamarin.Forms.Maps-Steuerelemente verweisen.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Die Xamarin.Forms.Maps ist eine separate NuGet, die jedes Projekt in einer Xamarin.Forms-Projektmappe hinzugefügt werden muss. Zusätzlichen Initialisierungscode ist erforderlich, sowie einige Konfigurationsschritte für iOS-, Android- und UWP.

Nach der Konfiguration die Maps-API kann verwendet werden, um Zuordnungen Anheften von Markierungen in nur wenigen Codezeilen zu rendern. Maps können weiter verbessert werden, mit einem [benutzerdefinierter Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Verwandte Links

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Zuordnen von benutzerdefinierten Renderer](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
