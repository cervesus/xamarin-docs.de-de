---
title: Xamarin. Forms-Zuordnungs Initialisierung und-Konfiguration
description: In diesem Artikel wird erläutert, wie Sie die xamarin. Forms-Zuordnungs Klasse verwenden, um die nativen Karten-APIs auf jeder Plattform zu verwenden, um Benutzern eine vertraute Kartendarstellung bereitzustellen.
ms.prod: xamarin
ms.assetid: 59CD1344-8248-406C-9144-0C8A67141E5B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/07/2019
ms.openlocfilehash: d9b1b93b0667415281bb04e2c4264f03be19bd83
ms.sourcegitcommit: 21d8be9571a2fa89fb7d8ff0787ff4f957de0985
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "72697710"
---
# <a name="xamarinforms-map-initialization-and-configuration"></a>Xamarin. Forms-Zuordnungs Initialisierung und-Konfiguration

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

_Xamarin. Forms verwendet die nativen Map-APIs auf jeder Plattform._

Xamarin. Forms. Maps verwendet die nativen Map-APIs auf jeder Plattform. Dies bietet Benutzern eine schnelle, vertraute Kartendarstellung, bedeutet jedoch, dass einige Konfigurationsschritte erforderlich sind, um die API-Anforderungen der einzelnen Plattformen einzuhalten.
Nach der Konfiguration funktioniert das `Map`-Steuerelement genauso wie jedes andere xamarin. Forms-Element in Common Code.

Das Map-Steuerelement wurde im [mapssample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps) -Beispiel verwendet, das unten dargestellt ist.

 [![Karten im mobilecrm-Beispiel](map-images/maps-zoom-sml.png "Kartensteuerelement Beispiel")](map-images/maps-zoom.png#lightbox "Kartensteuerelement Beispiel")

Map-Funktionen können durch Erstellen eines [benutzerdefinierten Renderers der Karte](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)erweitert werden.

<a name="Maps_Initialization" />

## <a name="map-initialization"></a>Zuordnungs Initialisierung

Beim Hinzufügen von Karten zu einer xamarin. Forms-Anwendung ist **xamarin. Forms. Maps** ein separates nuget-Paket, das Sie jedem Projekt in der Projekt Mappe hinzufügen sollten.
Unter Android hat dies auch eine Abhängigkeit von googleplayservices (ein weiteres nuget), das automatisch heruntergeladen wird, wenn Sie xamarin. Forms. Maps hinzufügen.

Nach der Installation des nuget-Pakets ist in jedem Anwendungsprojekt *nach* dem `Xamarin.Forms.Forms.Init`-Methoden aufrufen ein bestimmter Initialisierungs Code erforderlich. Verwenden Sie für IOS den folgenden Code:

```csharp
Xamarin.FormsMaps.Init();
```

Unter Android müssen Sie dieselben Parameter wie `Forms.Init` übergeben:

```csharp
Xamarin.FormsMaps.Init(this, bundle);
```

Verwenden Sie für die universelle Windows-Plattform (UWP) den folgenden Code:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Fügen Sie diesen-Befehl in den folgenden Dateien für jede Plattform hinzu:

- **IOS** -AppDelegate.cs-Datei in der `FinishedLaunching`-Methode.
- **Android** -MainActivity.cs-Datei in der `OnCreate`-Methode.
- **UWP** -MainPage.XAML.cs-Datei im `MainPage`-Konstruktor.

Nachdem das nuget-Paket hinzugefügt und die Initialisierungs Methode in jeder Anwendung aufgerufen wurde, können `Xamarin.Forms.Maps`-APIs im Allgemeinen .NET Standard Bibliotheksprojekt oder im freigegebenen Projekt Code verwendet werden.

<a name="Platform_Configuration" />

## <a name="platform-configuration"></a>Platt Form Konfiguration

Auf einigen Plattformen sind zusätzliche Konfigurationsschritte erforderlich, bevor die Karte angezeigt wird.

### <a name="ios"></a>iOS

Für den Zugriff auf die Standortdienste unter IOS müssen Sie die folgenden Schlüssel in " **Info. plist**" festlegen:

- iOS 11
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – für die Verwendung von Location Services, wenn die APP verwendet wird.
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/corelocation/choosing_the_authorization_level_for_location_services/requesting_always_authorization?language=objc) – für die jederzeit Verwendung von Location Services
- IOS 10 und früher
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26) – für die Verwendung von Location Services, wenn die APP verwendet wird.
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18) – für die jederzeit Verwendung von Location Services    

Zur Unterstützung von IOS 11 und früher können Sie alle drei Schlüssel einschließen: `NSLocationWhenInUseUsageDescription`, `NSLocationAlwaysAndWhenInUseUsageDescription` und `NSLocationAlwaysUsageDescription`.

Die XML-Darstellung für diese Schlüssel in " **Info. plist** " ist unten dargestellt. Aktualisieren Sie die `string` Werte, um widerzuspiegeln, wie Ihre Anwendung die Standortinformationen verwendet:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your app is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

Die **Info. plist** -Einträge können auch beim Bearbeiten der Datei " **Info. plist** " in der **Quell** Ansicht hinzugefügt werden:

![Info. plist für IOS 8](map-images/ios8-map-permissions.png "IOS 8 erforderliche Info. plist-Einträge")

### <a name="android"></a>Android

Wenn Sie die [Google Maps-API v2](https://developers.google.com/maps/documentation/android/) unter Android verwenden möchten, müssen Sie einen API-Schlüssel generieren und ihn Ihrem Android-Projekt hinzufügen.
Befolgen Sie die Anweisungen im xamarin-Dokument, um [einen Google Maps-API v2-Schlüssel zu erhalten](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
Fügen Sie den API-Schlüssel in die Datei " **Properties/androidmanifest. XML** " ein, nachdem Sie diese Anweisungen befolgen (Quelle anzeigen und das folgende Element suchen/aktualisieren):

```xml
<application ...>
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
</application>
```

Ohne einen gültigen API-Schlüssel wird das Karten-Steuerelement als graues Feld unter Android angezeigt.

> [!NOTE]
> Beachten Sie, dass Sie, damit Ihr APK auf Google Maps zugreifen kann, SHA-1-Fingerabdrücke und Paketnamen für jeden Keystore (Debug und Release) einschließen müssen, mit dem Sie Ihr APK signieren. Wenn Sie z. b. einen Computer für das Debuggen und einen anderen Computer zum Erstellen des Release-APK verwenden, sollten Sie den SHA-1-Zertifikat Fingerabdruck aus dem Debug-keystore des ersten Computers und dem SHA-1-Zertifikat Fingerabdruck aus dem releasekeystore von der zweite Computer. Beachten Sie auch, dass die Schlüssel Anmelde Informationen bearbeitet werden, wenn sich der **Paketname** der app ändert. Weitere Informationen finden Sie unter Abrufen [eines Google Maps API v2-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Sie müssen auch die entsprechenden Berechtigungen aktivieren, indem Sie mit der rechten Maustaste auf das Android-Projekt klicken und **Optionen > Erstellen > Android-Anwendung** auswählen und folgendes tippen:

- `AccessCoarseLocation`
- `AccessFineLocation`
- `AccessLocationExtraCommands`
- `AccessMockLocation`
- `AccessNetworkState`
- `AccessWifiState`
- `Internet`

Einige davon sind im folgenden Screenshot zu sehen:

![Erforderliche Berechtigungen für Android](map-images/android-map-permissions.png "Erforderliche Berechtigungen für Android")

Die letzten beiden sind erforderlich, da Anwendungen eine Netzwerkverbindung zum Herunterladen von Kartendaten benötigen. Weitere Informationen finden Sie unter Android- [Berechtigungen](https://developer.android.com/reference/android/Manifest.permission.html) .

Außerdem hat Android 9 die Apache HTTP-Client Bibliothek aus dem bootclasspath entfernt und ist daher für Anwendungen, die auf API 28 oder höher ausgerichtet sind, nicht verfügbar. Die folgende Zeile muss dem `application`-Knoten der Datei " **androidmanifest. XML** " hinzugefügt werden, um den Apache HTTP-Client in Anwendungen mit der Ziel-API 28 oder höher zu verwenden:

```xml
<application ...>
    ...
    <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Um Karten auf dem universelle Windows-Plattform zu verwenden, müssen Sie ein Autorisierungs Token generieren. Weitere Informationen finden Sie unter [Anfordern eines Karten Authentifizierungs Schlüssels](https://msdn.microsoft.com/library/windows/apps/mt219694.aspx) auf der MSDN-Website.

Das Authentifizierungs Token sollte dann im `FormsMaps.Init("AUTHORIZATION_TOKEN")`-Methoden Aufrufes angegeben werden, um die APP mit dem Namen von "admaps" zu authentifizieren.

<a name="Using_Maps" />

## <a name="map-configuration"></a>Zuordnungs Konfiguration

Ein Beispiel dafür, wie das Karten Steuerelement im Code verwendet werden kann, finden Sie unter [MapPage.cs](https://github.com/xamarin/xamarin-forms-samples/blob/master/MobileCRM/MobileCRM.Shared/Pages/MapPage.cs) im mobilecrm-Beispiel. Eine einfache `MapPage` Klasse könnte wie folgt aussehen: Beachten Sie, dass eine neue `MapSpan` erstellt wird, um die Ansicht der Karte zu positionieren:

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

Der Karteninhalt kann auch geändert werden, indem Sie die `MapType`-Eigenschaft festlegen, um eine reguläre Straße (Standard), Satellitenbilder oder eine Kombination aus beidem anzuzeigen.

```csharp
map.MapType = MapType.Street;
```

Gültige `MapType` Werte sind:

- `Hybrid`
- `Satellite`
- `Street` (Standard)

### <a name="map-region-and-mapspan"></a>Kartenbereich und Zuordnungs Spanne

Wie im obigen Code Ausschnitt gezeigt, wird durch das Bereitstellen einer `MapSpan`-Instanz an einen Zuordnungs Konstruktor die anfängliche Ansicht (Mittelpunkt und Zoomstufe) der Karte beim Laden festgelegt. Es gibt zwei Möglichkeiten, eine neue `MapSpan` Instanz zu erstellen:

- **Mapspan. fromcenterandradius ()** -statische Methode zum Erstellen einer Spanne aus einer `Position` und Angeben einer `Distance`.
- **neuer mapspan ()** -Konstruktor, der eine `Position` und den Grad der Breite und Längengrad der Anzeige verwendet.

Die `MoveToRegion`-Methode der Map-Klasse kann dann verwendet werden, um die Position oder Zoomstufe der Karte zu ändern. Um den Zoomfaktor der Karte zu ändern, ohne den Speicherort zu ändern, erstellen Sie eine neue `MapSpan` mithilfe der aktuellen Position aus der `VisibleRegion.Center`-Eigenschaft des Karten Steuer Elements. Eine `Slider` kann verwendet werden, um den Zuordnungs Zoom wie folgt zu steuern (der Wert des Schiebereglers kann jedoch zurzeit direkt im Karten Steuerelement vergrößert werden):

```csharp
Slider slider = new Slider (1, 18, 1);
slider.ValueChanged += (sender, e) =>
{
    var zoomLevel = e.NewValue; // between 1 and 18
    var latlongdegrees = 360 / (Math.Pow(2, zoomLevel));
    map.MoveToRegion(new MapSpan (map.VisibleRegion.Center, latlongdegrees, latlongdegrees));
};
```

[![Zuordnungen mit Zoom](map-images/maps-zoom-sml.png "Kartensteuerelement Zoom")](map-images/maps-zoom.png#lightbox "Kartensteuerelement Zoom")

Außerdem verfügt die [`Map`](xref:Xamarin.Forms.Maps.Map) -Klasse über eine `MoveToLastRegionOnLayoutChange`-Eigenschaft des Typs `bool`, die durch eine bindbare Eigenschaft unterstützt wird. Standardmäßig ist diese Eigenschaft `true`. Dies bedeutet, dass der angezeigte Kartenbereich von seinem aktuellen Bereich in den zuvor festgelegten Bereich wechselt, wenn eine Layoutänderung auftritt, z. b. bei der Geräte Rotation. Wenn diese Eigenschaft auf `false` festgelegt ist, bleibt der angezeigte Kartenbereich zentriert, wenn eine Layoutänderung auftritt. Das folgende Beispiel zeigt, wie Sie diese Eigenschaft festlegen:

```csharp
map.MoveToLastRegionOnLayoutChange = false;
```

### <a name="map-clicks"></a>Karten Klicks

`Map` definiert ein `MapClicked` Ereignis, das ausgelöst wird, wenn die Karte abgetippt wird. Das `MapClickedEventArgs` Objekt, das das `MapClicked` Ereignis begleitet, verfügt über eine einzelne Eigenschaft mit dem Namen `Position` vom Typ `Position`. Wenn das Ereignis ausgelöst wird, wird der Wert der `Position`-Eigenschaft auf den Zuordnungs Speicherort festgelegt, der abgetippt wurde.

Das folgende Codebeispiel zeigt einen Ereignishandler für das `MapClicked`-Ereignis:

```csharp
map.MapClicked += OnMapClicked;

void OnMapClicked(object sender, MapClickedEventArgs e)
{
    System.Diagnostics.Debug.WriteLine($"MapClick: {e.Position.Latitude}, {e.Position.Longitude}");
}
```

In diesem Beispiel gibt der `OnMapClicked`-Ereignishandler den breiten-und Längengrad aus, der den gezapften Zuordnungs Speicherort darstellt.

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
> Eine zusätzliche `xmlns`-Namespace Definition ist erforderlich, um auf die xamarin. Forms. Maps-Steuerelemente zu verweisen. Im vorherigen Beispiel wird auf den `Xamarin.Forms.Maps` Namespace über das `maps`-Schlüsselwort verwiesen.

Der `MapRegion` kann im Code mithilfe des benannten Verweises für die `Map` festgelegt werden:

```csharp
MyMap.MoveToRegion(
    MapSpan.FromCenterAndRadius(
        new Position(37,-122), Distance.FromMiles(1)));
```

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin. Forms. Maps-Pins](~/xamarin-forms/user-interface/map/pins.md).
- [Karten-API](xref:Xamarin.Forms.Maps)
- [Zuordnen eines benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map/index.md)
