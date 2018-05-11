---
title: Zuordnung
description: Xamarin.Forms nutzt die systemeigene APIs-Zuordnung auf jeder Plattform.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/27/2016
ms.openlocfilehash: 6063732e08680974b8d4a2358bfd85b176b36aec
ms.sourcegitcommit: b0a1c3969ab2a7b7fe961f4f470d1aa57b1ff2c6
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/10/2018
---
# <a name="map"></a>Zuordnung

_Xamarin.Forms nutzt die systemeigene APIs-Zuordnung auf jeder Plattform._

Xamarin.Forms.Maps verwendet die systemeigene APIs-Zuordnung auf jeder Plattform. Dies bietet eine schnelle, vertraut Maps-Erfahrung für Benutzer jedoch bedeutet, dass einige Konfigurationsschritte erforderlich sind, jede Plattformen bestimmten API-Anforderungen entsprechen.
Nachdem es konfiguriert wurde, die `Map` funktioniert genau wie jedes andere Xamarin.Forms-Element im gemeinsamen Code zu steuern.

* [Ordnet Initialisierung](#Maps_Initialization) – mit `Map` erfordert zusätzlichen Initialisierungscode beim Start.
* [Plattformkonfiguration](#Platform_Configuration) -jede Plattform erfordert etwas Konfiguration für Maps arbeiten.
* [Verwenden von Zuordnungen in c#](#Using_Maps) -Anzeige zugeordnet und pins mithilfe von c#.
* [Verwenden von Zuordnungen in XAML](#Using_Xaml) -Anzeigen einer Karte mit XAML.

Das Kartensteuerelement wurde in verwendet das [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/) Beispiel, das im unten dargestellt wird.

 [![Zuordnungen in der Stichprobe MobileCRM](map-images/maps-zoom-sml.png "Zuordnung Steuerelement Beispiel")](map-images/maps-zoom.png#lightbox "Zuordnung Steuerelement-Beispiel")

Kartenfunktion kann weiter verbessert werden, indem Sie erstellen eine [Zuordnen von benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).

<a name="Maps_Initialization" />

## <a name="maps-initialization"></a>Maps-Initialisierung

Beim Hinzufügen von Zuordnungen zu einer Anwendung Xamarin.Forms **Xamarin.Forms.Maps** ist eine separate NuGet-Paket, die auf jedes Projekt in der Projektmappe hinzugefügt werden soll.
Auf Android-Geräten dies verfügt ebenfalls über eine Abhängigkeit für GooglePlayServices (einen anderen NuGet) automatisch heruntergeladen wird, wenn Sie Xamarin.Forms.Maps hinzufügen.

Nach der Installation der NuGet-Paket ist erforderlich, einige Initialisierungscode in jedem Anwendungsprojekt *nach* der `Xamarin.Forms.Forms.Init` -Methodenaufruf. Verwenden Sie den folgenden Code für iOS:

```csharp
Xamarin.FormsMaps.Init();
```

Auf Android-Geräten müssen Sie die gleichen Parameter wie übergeben `Forms.Init`:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Verwenden Sie für die universelle Windows-Plattform (UWP) den folgenden Code:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Fügen Sie diesen Aufruf in den folgenden Dateien für jede Plattform hinzu:

-  **iOS** -AppDelegate.cs-Datei, in der `FinishedLaunching` Methode.
-  **Android** -MainActivity.cs-Datei, in der `OnCreate` Methode.
-  **Universelle Windows-Plattform** -Datei "MainPage.Xaml.cs"-Datei, in der `MainPage` Konstruktor.

Sobald das NuGet-Paket hinzugefügt wurde und die Initialisierungsmethode innerhalb jedes Applcation aufgerufen `Xamarin.Forms.Maps` APIs im allgemeine .NET Standard-Bibliotheksprojekt oder freigegebenes Projekt Code verwendet werden können.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Plattformkonfiguration

Auf manchen Plattformen sind zusätzliche Konfigurationsschritte erforderlich, bevor die Karte angezeigt werden.

### <a name="ios"></a>iOS

Für den Zugriff auf iOS-Standortdiensten auf, müssen Sie die folgenden Schlüssel in festlegen **"Info.plist"**:

- iOS 11
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – für die Verwendung von Standortdiensten, wenn die app verwendet wird
    - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – für die Verwendung von Standortdiensten jederzeit
- iOS 10 und früher
    - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – für die Verwendung von Standortdiensten, wenn die app verwendet wird
    - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – für die Verwendung von Standortdiensten jederzeit    
    
Um die iOS 11 und früheren Versionen zu unterstützen, können Sie alle drei Schlüssel einschließen: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription`, und `NSLocationAlwaysUsageDescription`.

Die XML-Darstellung für diese Schlüssel im **"Info.plist"** wird unten gezeigt. Aktualisieren Sie die `string` Werte widerspiegeln, wie Ihre Anwendung die Speicherortinformationen verwendet:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

Die **"Info.plist"** Einträge können auch hinzugefügt werden, **Quelle** Sicht beim Bearbeiten der **"Info.plist"** Datei:

![Datei "Info.plist" für iOS 8](map-images/ios8-map-permissions.png "iOS 8 erforderliche Datei "Info.plist" Einträge")


### <a name="android"></a>Android

Verwenden der [Google Maps-API v2](https://developers.google.com/maps/documentation/android/) auf Android-Geräten müssen Sie einen API-Schlüssel generieren und Ihre Android-Projekt hinzugefügt.
Befolgen Sie die Anweisungen in die Xamarin-Dokumentkommentaren auf [eine Google Maps-API v2 Schlüssel](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Befolgen Sie diese Anweisungen verwenden, fügen Sie den API-Schlüssel in der **Properties/AndroidManifest.xml** -Datei (Quelle anzeigen und mit dem folgenden Element suchen/aktualisieren):

```xml
<meta-data
        android:name="com.google.android.geo.API_KEY"
        android:value="YOUR_API_KEY"/>
```

Ohne einen gültigen API-Schlüssel wird der Maps-Steuerelement als graues Feld unter Android angezeigt.

> [!NOTE]
> Denken Sie daran, generieren Sie einen anderen Schlüssel mithilfe der Keystore-Datei, die verwendet wird, wie die endgültigen Produktversion von jeder Anwendung, die hochgeladen wird zum Google Play Store. Die schlüsselgenerierung für die Entwicklung und Debuggen funktioniert nicht, und die app von Google Play heruntergeladen werden Kartenanzeige zerstört. Beachten Sie dabei außerdem die Taste, wenn der app erneut generieren **Paketname** ändert.

Sie müssen auch auf die entsprechende Berechtigungen zu aktivieren, indem Sie mit der rechten Maustaste auf das Android-Projekt, und wählen **Optionen > Erstellen > Android-Anwendung** und zu laufen Folgendes:

* `AccessCoarseLocation`
* `AccessFineLocation`
* `AccessLocationExtraCommands`
* `AccessMockLocation`
* `AccessNetworkState`
* `AccessWifiState`
* `Internet`

Einige davon werden im folgenden Screenshot gezeigt:

![Erforderliche Berechtigungen für Android](map-images/android-map-permissions.png "erforderliche Berechtigungen für Android")

Die letzten zwei sind erforderlich, da Anwendungen eine Netzwerkverbindung zum Herunterladen von Kartendaten erfordern. Erfahren Sie, bis Android [Berechtigungen](http://developer.android.com/reference/android/Manifest.permission.html) um mehr zu erfahren.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Um die Zuordnungen für die universelle Windows-Plattform verwenden, müssen Sie ein Autorisierungstoken generieren. Weitere Informationen finden Sie unter [anfordern einen Maps Authentifizierungsschlüssel](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) auf MSDN.

Das Authentifizierungstoken sollte dann angegeben werden, der `FormsMaps.Init("AUTHORIZATION_TOKEN")` -Methodenaufruf, um die app mit Bing Maps zu authentifizieren.

<a name="Using_Maps" />

## <a name="using-maps"></a>Verwenden von Zuordnungen

Finden Sie unter der [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) im MobileCRM Beispiel für ein Beispiel, wie das Kartensteuerelement im Code verwendet werden kann. Eine einfache `MapPage` Klasse dieses - Hinweises aussehen könnte, eine neue `MapSpan` wird erstellt, um positionieren Sie die Karte anzeigen:

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

### <a name="map-type"></a>Map-Typ

Karteninhalts kann auch geändert werden, indem die `MapType` -Eigenschaft, um eine reguläre Straße Karte (Standard), Satelliten bereitzustellen oder eine Kombination aus beidem anzuzeigen.

```csharp
map.MapType == MapType.Street;
```

Gültige `MapType` Werte sind:

-  Hybrid
-  Satellitenassemblys
-  Straße (Standard)


### <a name="map-region-and-mapspan"></a>Region der Karte und MapSpan

Wie im obigen Codeausschnitt gezeigt wird, Angeben einer `MapSpan` Instanz an einen Map-Konstruktor legt die anfängliche Ansicht (Mittelpunkt und Zoomstufe) der Karte, wenn es geladen wird. Die `MoveToRegion` Methode auf der Map-Klasse kann dann verwendet werden, die Ebene Position und Zoomen der Karte geändert. Es gibt zwei Möglichkeiten zum Erstellen eines neuen `MapSpan` Instanz:

-  **MapSpan.FromCenterAndRadius()** -statische Methode zum Erstellen von einem Bereich eine `Position` und Angeben einer `Distance` .
-  **neue MapSpan ()** -Konstruktor, der verwendet eine `Position` und den Grad der Breiten- und Längengrad angezeigt.


Um die Zoomstufe der Karte zu ändern, ohne den Speicherort zu ändern, erstellen Sie ein neues `MapSpan` mithilfe der aktuellen Position aus der `VisibleRegion.Center` Eigenschaft des Kartensteuerelements. Ein `Slider` kann verwendet werden, um die Karte Zoom wie folgt steuern (jedoch den Wert des Schiebereglers vergrößern/verkleinern, direkt in das Kartensteuerelement derzeit aktualisieren kann):

```csharp
var slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) => {
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

 [![Zuordnungen mit Zoom](map-images/maps-zoom-sml.png "Zuordnung Steuerelement Zoom")](map-images/maps-zoom.png#lightbox "Zuordnung Steuerelement Zoom")

### <a name="map-pins"></a>Map-Pins

Speicherorte können gekennzeichnet werden, auf der Karte mit `Pin` Objekte.

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

 `PinType` kann auf einen der folgenden Werte festgelegt werden, die Einfluss auf die möglicherweise die Pin in (abhängig von der Plattform) gerendert:

-  Generisch
-  Ort
-  SavedPin
-  SearchResult


<a name="Using_Xaml" />

## <a name="using-xaml"></a>Verwendung von Xaml

Karten können auch in XAML-Layouts positioniert werden, wie in diesem Codeausschnitt gezeigt.

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

Die `MapRegion` und `Pins` kann festgelegt werden, im Code mithilfe der `MyMap` Verweis (oder alle von Ihnen gewünschten dem Namen der Zuordnung). Beachten Sie, dass ein zusätzliches `xmlns` Namespacedefinition ist erforderlich, um die Xamarin.Forms.Maps-Steuerelemente verweisen.

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

Die Xamarin.Forms.Maps ist eine separate NuGet, die auf jedes Projekt in einer Xamarin.Forms-Projektmappe hinzugefügt werden muss. Zusätzlichen Initialisierungscode ist erforderlich, als auch einige Konfigurationsschritte für iOS, Android und universelle Windows-Plattform.

Nach der Konfiguration der Maps-API kann verwendet werden, um Zuordnungen mit Pin-Marker in nur wenigen Codezeilen gerendert werden soll. Zuordnungen können weiter verbessert werden, mit einem [benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md).


## <a name="related-links"></a>Verwandte Links

- [MapsSample](https://developer.xamarin.com/samples/WorkingWithMaps/)
- [Zuordnen von benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
