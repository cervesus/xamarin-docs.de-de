---
title: Verwenden der Google Maps-API in Ihrer Anwendung
description: 'Gewusst wie: Implementieren von Google Maps API v2-Features in ihrer xamarin. Android-Anwendung.'
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027154"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Verwenden der Google Maps-API in Ihrer Anwendung

Die Verwendung der Maps-Anwendung ist großartig, aber manchmal möchten Sie Zuordnungen direkt in Ihre Anwendung einschließen. Zusätzlich zur integrierten Maps-Anwendung bietet Google auch eine [native Mapping-API für Android](https://developers.google.com/maps/documentation/android-sdk/intro).
Die Maps-API eignet sich für Fälle, in denen Sie mehr Kontrolle über die Zuordnungs Darstellung behalten möchten. Mit der Maps-API können Sie Folgendes tun:

- Programm gesteuertes Ändern des Standpunkts der Karte.
- Hinzufügen und Anpassen von Markern.
- Kommentieren einer Karte mit Überlagerungen.

Anders als die mittlerweile veraltete Google Maps Android-API v1 ist Google Maps Android API v2 Teil [Google Play Services](https://developers.google.com/android/guides/overview).
Eine xamarin. Android-App muss einige erforderliche Voraussetzungen erfüllen, damit die Google Maps Android-API verwendet werden kann.

## <a name="google-maps-api-prerequisites"></a>Voraussetzungen für Google Maps-API

Bevor Sie die Maps-API verwenden können, müssen mehrere Schritte ausgeführt werden, darunter:

- [Abrufen eines Maps-API-Schlüssels](#obtain-maps-key)
- [Installieren des Google Play Services SDK](#install-gps-sdk)
- [Installieren des xamarin. googleplayservices. Maps-Pakets von nuget](#install-gpsmaps-nuget)
- [Geben Sie die erforderlichen Berechtigungen an.](#declare-permissions)
- [Erstellen Sie optional einen Emulator mit den Google-APIs.](#create-emulator-with-google-api)

### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Abrufen eines Google Maps-API-Schlüssels

Der erste Schritt besteht darin, einen Google Maps-API-Schlüssel zu erhalten (Beachten Sie, dass Sie keinen API-Schlüssel aus der Legacy-API von Google Maps v1 wieder verwenden können). Informationen dazu, wie Sie den API-Schlüssel mit xamarin. Android abrufen und verwenden, finden Sie unter Abrufen [eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Installieren des Google Play Services SDK

Google Play Services ist eine Technologie von Google, mit der Android-Anwendungen verschiedene Google-Features wie Google +, in-App-Abrechnung und Maps nutzen können. Diese Features sind auf Android-Geräten als Hintergrunddienste verfügbar, die im [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)enthalten sind.

Android-Anwendungen interagieren mit Google Play Services über die Google Play Services Client Bibliothek. Diese Bibliothek enthält die Schnittstellen und Klassen für die einzelnen Dienste, z. b. Maps. Das folgende Diagramm zeigt die Beziehung zwischen einer Android-Anwendung und Google Play Services:

![Diagramm zur Veranschaulichung der Google Play Store Aktualisieren des Google Play Services APK](maps-api-images/play-services-diagram.png)

Die Android Maps-API wird als Teil von Google Play Services bereitgestellt.
Bevor eine xamarin. Android-Anwendung die Maps-API verwenden kann, muss das Google Play Services SDK mithilfe des [Android SDK-Managers](~/android/get-started/installation/android-sdk.md)installiert werden. Der folgende Screenshot zeigt, wo sich der Google Play Services-Client im Android SDK-Manager befindet:

![Google Play Services im Android SDK Manager unter Extras angezeigt](maps-api-images/image01.png)

> [!NOTE]
> Das APK für die Google Play-Dienste ist ein lizenziertes Produkt, das möglicherweise nicht auf allen Geräten vorhanden ist. Wenn Sie nicht installiert ist, funktioniert Google Maps nicht auf dem Gerät.

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> Installieren des xamarin. googleplayservices. Maps-Pakets von nuget

Das [xamarin. googleplayservices. Maps-Paket](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) enthält die xamarin. Android-Bindungen für die Google Play Services Maps-API.
Klicken Sie zum Hinzufügen des Google Play Services Map-Pakets mit der rechten Maustaste auf den Ordner **Verweise** Ihres Projekts im Projektmappen-Explorer, und klicken Sie auf **nuget-Pakete verwalten...** :

![Projektmappen-Explorer Kontextmenü Element "nuget-Pakete verwalten" unter "Verweise" angezeigt](maps-api-images/image02.png)

Der **nuget-Paket-Manager**wird geöffnet. Klicken Sie auf **Durchsuchen** , und geben Sie im Suchfeld **xamarin Google Play Services Maps** ein. Wählen Sie **xamarin. googleplayservices. Maps** aus, und klicken Sie auf **Installieren**. (Wenn dieses Paket bereits installiert wurde, klicken Sie auf **Aktualisieren**.):

[![nuget-Paket-Manager mit ausgewähltem xamarin. googleplayservices. Maps-Paket](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Beachten Sie, dass die folgenden Abhängigkeits Pakete ebenfalls installiert sind:

- **Xamarin. googleplayservices. Base**
- **Xamarin. googleplayservices. Basement**
- **Xamarin. googleplayservices. Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a><a name="declare-permissions" /> geben Sie die erforderlichen Berechtigungen an.

Apps müssen die Hardware-und Berechtigungsanforderungen ermitteln, damit Sie die Google Maps-API verwenden können.  Einige Berechtigungen werden automatisch durch das Google Play Services SDK erteilt, und es ist nicht erforderlich, dass ein Entwickler diese explizit zu " **androidmanfest. XML**" hinzufügt:

- Der **Zugriff auf den Netzwerkstatus** &ndash; der Maps-API muss überprüfen können, ob die Karten Kacheln heruntergeladen werden können.

- **Internet Zugriff** &ndash; Internet Zugriff ist erforderlich, um die Karten Kacheln herunterzuladen und mit den Google Play Servern für den API-Zugriff zu kommunizieren.

Die folgenden Berechtigungen und Features müssen in der Datei " **androidmanifest. XML** " für die Google Maps Android-API angegeben werden:

- **OpenGL es v2** &ndash; die Anwendung muss die Anforderung für OpenGL es v2 deklarieren.

- **Google Maps-API-Schlüssel** &ndash; der API-Schlüssel verwendet wird, um zu bestätigen, dass die Anwendung registriert und für die Verwendung Google Play Services autorisiert ist. Weitere Informationen zu diesem Schlüssel finden Sie unter Abrufen [eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) .

- **Anfordern des Legacy-Apache HTTP-Clients** &ndash; apps, die auf Android 9,0 (API-Ebene 28) oder höher abzielen, müssen angeben, dass der Legacy-Apache-HTTP-Client eine optionale zu verwendende Bibliothek ist.

- Der **Zugriff auf die webbasierten Google-Dienste** &ndash; die Anwendung benötigt Berechtigungen für den Zugriff auf Google-Webdienste, die auf die Android Maps-API zurückgreifen.

- **Berechtigungen für Google Play Services Benachrichtigungen** &ndash; der Anwendung muss die Berechtigung zum Empfangen von Remote Benachrichtigungen von Google Play Services erteilt werden.

- Der **Zugriff auf standortanbieter** &ndash; diese optionalen Berechtigungen.
   Sie ermöglichen es der `GoogleMap` Klasse, den Speicherort des Geräts auf der Karte anzuzeigen.

Außerdem hat Android 9 die Apache HTTP-Client Bibliothek aus dem bootclasspath entfernt und ist daher für Anwendungen, die auf API 28 oder höher ausgerichtet sind, nicht verfügbar. Die folgende Zeile muss dem `application`-Knoten der Datei " **androidmanifest. XML** " hinzugefügt werden, um den Apache HTTP-Client in Anwendungen mit der Ziel-API 28 oder höher zu verwenden:

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> Bei sehr alten Versionen des Google Play SDK war eine APP erforderlich, um die `WRITE_EXTERNAL_STORAGE` Berechtigung anzufordern. Diese Anforderung ist mit den aktuellen xamarin-Bindungen für Google Play Services nicht mehr erforderlich.

Der folgende Code Ausschnitt ist ein Beispiel für die Einstellungen, die " **androidmanifest. XML**" hinzugefügt werden müssen:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="23" android:targetSdkVersion="28" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- Necessary for apps that target Android 9.0 or higher -->
    <uses-library android:name="org.apache.http.legacy" android:required="false" />

    <!-- Permission to receive remote notifications from Google Play Services -->
    <!-- Notice here that we have the package name of our application as a prefix on the permissions. -->
    <uses-permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" />
    <permission android:name="<PACKAGE NAME>.permission.MAPS_RECEIVE" android:protectionLevel="signature" />

    <!-- These are optional, but recommended. They will allow Maps to use the My Location provider. -->
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />

    <application android:label="@string/app_name">
        <!-- Put your Google Maps V2 API Key here. -->
        <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
        <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
        <!-- Necessary for apps that target Android 9.0 or higher -->
        <uses-library android:name="org.apache.http.legacy" android:required="false" />
    </application>
</manifest>
```

Zusätzlich zum Anfordern der Berechtigungen " **androidmanifest. XML**" muss eine APP auch Lauf Zeit Berechtigungs Prüfungen für die `ACCESS_COARSE_LOCATION` und die `ACCESS_FINE_LOCATION` Berechtigungen ausführen. Weitere Informationen zum Ausführen von Lauf Zeit Berechtigungs Prüfungen finden Sie im [xamarin. Android-Berechtigungs](~/android/app-fundamentals/permissions.md) Handbuch.

### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Erstellen eines Emulators mit Google-APIs

Wenn ein physisches Android-Gerät mit Google Play-Diensten nicht installiert ist, können Sie ein emulatorimage für die Entwicklung erstellen. Weitere Informationen finden Sie in der [Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).

## <a name="the-googlemap-class"></a>Die GoogleMap-Klasse

Nachdem die Voraussetzungen erfüllt sind, ist es an der Zeit, mit der Entwicklung der Anwendung zu beginnen und die Android Maps-API zu verwenden. Bei der [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) -Klasse handelt es sich um die Haupt-API, die eine xamarin. Android-Anwendung verwendet, um Google Maps für Android anzuzeigen und mit Ihnen zu interagieren. Diese Klasse hat die folgenden Zuständigkeiten:

- Interagieren mit Google Play Diensten, um die Anwendung mit dem Google-Webdienst zu autorisieren.

- Herunterladen, Zwischenspeichern und Anzeigen der Karten Kacheln.

- Anzeigen von UI-Steuerelementen wie Schwenken und Vergrößern des Benutzers.

- Zeichnen von Markern und geometrischen Formen in Maps.

Die `GoogleMap` wird einer Aktivität auf zwei Arten hinzugefügt:

- **Mapfragment** : das [mapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) ist ein spezielles Fragment, das als Host für das `GoogleMap` Objekt fungiert. Der `MapFragment` erfordert mindestens die Android-API-Ebene 12.
   Ältere Versionen von Android können das [supportmapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment)verwenden.  Dieser Leitfaden konzentriert sich auf die Verwendung der `MapFragment`-Klasse.

- **MapView** : [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) ist eine spezialisierte Ansichts Unterklasse, die als Host für ein `GoogleMap` Objekt fungieren kann. Benutzer dieser Klasse müssen alle Aktivitäts Lebenszyklus Methoden an die `MapView` Klasse weiterleiten.

Jeder dieser Container macht eine `Map`-Eigenschaft verfügbar, die eine Instanz von `GoogleMap`zurückgibt. Die zugriffsklasse sollte an die [mapfragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) -Klasse übergeben werden, da es sich um eine einfachere API handelt, die den Code für die Code Menge reduziert, den ein Entwickler manuell implementieren muss.

### <a name="adding-a-mapfragment-to-an-activity"></a>Hinzufügen eines mapfragments zu einer Aktivität

Der folgende Screenshot zeigt ein Beispiel für eine einfache `MapFragment`:

[![Screenshot eines Geräts, das ein Google Map-Fragment anzeigt](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Ähnlich wie bei anderen fragmentklassen gibt es zwei Möglichkeiten, einer Aktivität eine `MapFragment` hinzuzufügen:

- **Deklarativ** : der `MapFragment` kann über die XML-Layoutdatei für die-Aktivität hinzugefügt werden. Der folgende XML-Code Ausschnitt zeigt ein Beispiel für die Verwendung des `fragment`-Elements:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **Programm** gesteuert: der `MapFragment` kann Programm gesteuert mit der [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) -Methode instanziiert und dann zu einer-Aktivität hinzugefügt werden. Dieser Code Ausschnitt zeigt die einfachste Möglichkeit zum Instanziieren eines `MapFragment` Objekts und zum Hinzufügen zu einer Aktivität:

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    Es ist möglich, das `MapFragment` Objekt zu konfigurieren, indem Sie ein [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) Objekt an `NewInstance`übergeben. Dies wird im Abschnitt [GoogleMap-Eigenschaften](#googlemap_object) erläutert, die später in diesem Handbuch angezeigt werden.

Die `MapFragment.GetMapAsync`-Methode wird verwendet, um die vom Fragment gehostete [`GoogleMap`](#googlemap_object) zu initialisieren und einen Verweis auf das Map-Objekt zu erhalten, das vom `MapFragment`gehostet wird. Diese Methode nimmt ein Objekt an, das die `IOnMapReadyCallback`-Schnittstelle implementiert.

Diese Schnittstelle verfügt über eine einzelne Methode, `IMapReadyCallback.OnMapReady(MapFragment map)`, die aufgerufen wird, wenn es möglich ist, dass die APP mit dem `GoogleMap` Objekt interagiert. Der folgende Code Ausschnitt zeigt, wie eine Android-Aktivität eine `MapFragment` initialisieren und die `IOnMapReadyCallback`-Schnittstelle implementieren kann:

```csharp
public class MapWithMarkersActivity : AppCompatActivity, IOnMapReadyCallback
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        SetContentView(Resource.Layout.MapLayout);

        var mapFragment = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.map);
        mapFragment.GetMapAsync(this);

        // remainder of code omitted
    }

    public void OnMapReady(GoogleMap map)
    {
        // Do something with the map, i.e. add markers, move to a specific location, etc.
    }
}
```

### <a name="map-types"></a>Kartentypen

Es gibt fünf verschiedene Typen von Zuordnungen, die über die Google Maps-API verfügbar sind:

- **Normal** : Dies ist der Standard Kartentyp. Es zeigt Straßen und wichtige natürliche Features zusammen mit einigen künstlichen interessanten Punkten (z. b. Gebäude und Brücken).

- **Satellit** : diese Karte zeigt Satellitenfotos.

- **Hybrid** : diese Karte zeigt Satellitenfotos und Straßen Zuordnungen.

- **Gelände** : Dieses zeigt hauptsächlich topografische Features mit einigen Straßen an.

- **Keine** : diese Karte lädt keine Kacheln, sondern wird als leeres Raster gerendert.

Die Abbildung unten zeigt drei der verschiedenen Typen von Karten von links nach rechts (normal, Hybrid, Gelände):

[![drei Karten Beispiel-Screenshots: normal, Hybrid und Gelände](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Mit der `GoogleMap.MapType`-Eigenschaft wird festgelegt, welcher Kartentyp angezeigt wird. Der folgende Code Ausschnitt zeigt, wie eine Satellitenkarte angezeigt wird.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="a-namegooglemap_object-googlemap-properties"></a><a name="googlemap_object" />GoogleMap-Eigenschaften

`GoogleMap` definiert mehrere Eigenschaften, die die Funktionalität und Darstellung der Karte steuern können. Eine Möglichkeit, den Anfangszustand einer `GoogleMap` zu konfigurieren, besteht darin, ein [googlemapoptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) -Objekt zu übergeben, wenn ein `MapFragment`erstellt wird. Der folgende Code Ausschnitt ist ein Beispiel für die Verwendung eines `GoogleMapOptions` Objekts beim Erstellen eines `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, mapFragment, "map");
fragTx.Commit();
```

Die andere Möglichkeit, eine `GoogleMap` zu konfigurieren, besteht darin, die Eigenschaften der [uisettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) -Eigenschaft des Map-Objekts zu ändern. Im nächsten Codebeispiel wird gezeigt, wie ein `GoogleMap` konfiguriert wird, um die Zoom Steuerelemente und einen Kompass anzuzeigen:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>Interagieren mit der GoogleMap

Die Android Maps-API stellt APIs bereit, mit denen eine Aktivität den Standpunkt ändern, Marker hinzufügen, benutzerdefinierte Überlagerungen platzieren oder geometrische Formen zeichnen kann. In diesem Abschnitt wird erläutert, wie Sie einige dieser Aufgaben in xamarin. Android ausführen.

### <a name="changing-the-viewpoint"></a>Ändern des Standpunkts

Karten werden basierend auf der Mercator-Projektion als flache Ebene auf dem Bildschirm modelliert. Die Kartenansicht ist die einer *Kamera* , die auf dieser Ebene nach unten zeigt. Die Position der Kamera kann gesteuert werden, indem Sie den Speicherort, den Zoom, die Neigung und das-Verhalten ändern. Die [cameraupdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) -Klasse wird verwendet, um die Kameraposition zu verschieben. `CameraUpdate` Objekte nicht direkt instanziiert werden, stellt die Maps-API stattdessen die [cameraupdatefactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) -Klasse bereit.

Nachdem ein `CameraUpdate` Objekt erstellt wurde, wird es als Parameter an die [GoogleMap. muvecamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) -Methode oder die [GoogleMap. animatecamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) -Methode übergeben. Die `MoveCamera`-Methode aktualisiert die Zuordnung sofort, während die `AnimateCamera`-Methode einen reibungslosen, animierten Übergang bereitstellt.

Dieser Code Ausschnitt ist ein einfaches Beispiel für die Verwendung der `CameraUpdateFactory`, um eine `CameraUpdate` zu erstellen, mit der die Zoomstufe der Karte um eine Zoomstufe erhöht wird:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Die Maps-API stellt eine [cameraposition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) bereit, mit der alle möglichen Werte für die Kameraposition aggregiert werden. Eine Instanz dieser Klasse kann für die [cameraupdatefactory. newcameraposition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) -Methode bereitgestellt werden, die ein `CameraUpdate` Objekt zurückgibt. Die Maps-API enthält auch die Klasse " [cameraposition. Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) ", die eine fließende API zum Erstellen von `CameraPosition` Objekten bereitstellt.
Der folgende Code Ausschnitt zeigt ein Beispiel für das Erstellen eines `CameraUpdate` aus einem `CameraPosition` und dessen Verwendung zum Ändern der Kameraposition auf einem `GoogleMap`:

```csharp
public void OnMapReady(GoogleMap map)
{
    LatLng location = new LatLng(50.897778, 3.013333);

    CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
    builder.Target(location);
    builder.Zoom(18);
    builder.Bearing(155);
    builder.Tilt(65);

    CameraPosition cameraPosition = builder.Build();

    CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

    map.MoveCamera(cameraUpdate);
}
```

Im vorherigen Code Ausschnitt wird eine bestimmte Position auf der Karte durch die [latlng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) -Klasse dargestellt. Der Zoomfaktor ist auf 18 festgelegt, was ein beliebiges Measure von Zoom ist, das von Google Maps verwendet wird. Das-Verhalten ist die Kompass Messung im Uhrzeigersinn von North. Die Neigung-Eigenschaft steuert den Anzeige Winkel und gibt einen Winkel von 25 Grad aus der vertikalen an. Der folgende Screenshot zeigt die `GoogleMap` nach dem Ausführen des vorangehenden Codes:

[![Beispiel Google Map, das eine angegebene Position mit einem gekippten Anzeige Winkel anzeigt](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>Zeichnen auf der Karte

Die Android Maps-API stellt APIs zum Zeichnen der folgenden Elemente auf einer Karte bereit:

- **Marker** : Hierbei handelt es sich um besondere Symbole, die verwendet werden, um eine einzelne Position auf einer Karte zu identifizieren.

- Über **Lagerungen** : Dies ist ein Bild, das verwendet werden kann, um eine Auflistung von Standorten oder Bereichen in der Karte zu identifizieren.

- **Linien, Polygone und Kreise** : Hierbei handelt es sich um APIs, mit denen Aktivitäten einer Karte Formen hinzufügen können.

#### <a name="markers"></a>Marker

Die Maps-API stellt eine [markerklasse](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) bereit, die alle Daten zu einer einzelnen Position in einer Zuordnung kapselt. Standardmäßig verwendet die markerklasse ein Standard Symbol, das von Google Maps bereitgestellt wird. Es ist möglich, das Aussehen eines Markers anzupassen und auf Benutzer Klicks zu reagieren.

##### <a name="adding-a-marker"></a>Hinzufügen eines Markers

Um einer Karte einen Marker hinzuzufügen, müssen Sie ein neues [markerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) -Objekt erstellen und dann die [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) -Methode für eine `GoogleMap` Instanz aufzurufen. Diese Methode gibt ein [Markerobjekt](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) zurück.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    map.AddMarker(markerOpt1);
}
```

Der Titel des Markers wird in einem *Infofenster* angezeigt, wenn der Benutzer auf den Marker tippt. Der folgende Screenshot zeigt, wie dieser Marker aussieht:

[![Beispiel Google Map mit einem Marker und einem Infofenster für Vimy-Grat](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>Anpassen eines Markers

Es ist möglich, das Symbol anzupassen, das von der Markierung verwendet wird, indem Sie die `MarkerOptions.InvokeIcon`-Methode aufrufen, wenn Sie der Karte den Marker hinzufügen.
Diese Methode nimmt ein [bitmapdescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) -Objekt an, das die zum Rendering des Symbols erforderlichen Daten enthält. Die [bitmapdescriptorfactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) -Klasse stellt einige Hilfsmethoden bereit, um die Erstellung eines `BitmapDescriptor`zu vereinfachen. In der folgenden Liste werden einige dieser Methoden eingeführt:

- `DefaultMarker(float colour)` &ndash; den standardmäßigen Google Maps-Marker verwenden, aber die Farbe ändern.

- `FromAsset(string assetName)` &ndash; ein benutzerdefiniertes Symbol aus der angegebenen Datei im Ordner "Assets" verwenden.

- `FromBitmap(Bitmap image)` &ndash; die angegebene Bitmap als Symbol verwenden.

- `FromFile(string fileName)` &ndash; erstellen Sie das benutzerdefinierte Symbol aus der Datei im angegebenen Pfad.

- `FromResource(int resourceId)` &ndash; ein benutzerdefiniertes Symbol aus der angegebenen Ressource erstellen.

Der folgende Code Ausschnitt zeigt ein Beispiel für das Erstellen eines Zy-farbigen Standard Markers:

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");

    var bmDescriptor = BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan);
    markerOpt1.InvokeIcon(bmDescriptor);

    map.AddMarker(markerOpt1);
}
```

#### <a name="info-windows"></a>Info Fenster

*Info Fenster* sind spezielle Fenster, in denen dem Benutzerinformationen angezeigt werden, wenn diese auf einen bestimmten Marker tippen. Standardmäßig wird im Infofenster der Inhalt des markertitels angezeigt. Wenn der Titel nicht zugewiesen wurde, wird kein Infofenster angezeigt. Es kann jeweils nur ein Informationsfenster angezeigt werden.

Sie können das Infofenster anpassen, indem Sie die [GoogleMap. iinfowindowadapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) -Schnittstelle implementieren. Es gibt zwei wichtige Methoden für diese Schnittstelle:

- `public View GetInfoWindow(Marker marker)` &ndash; diese Methode aufgerufen wird, um ein benutzerdefiniertes Infofenster für einen Marker zu erhalten. Wenn `null` zurückgegeben wird, wird das Standardfenster Rendering verwendet. Wenn diese Methode eine Ansicht zurückgibt, wird diese Ansicht in den Rahmen des Info Fensters eingefügt.

- `public View GetInfoContents(Marker marker)` &ndash; diese Methode wird nur aufgerufen, wenn getinfowindow `null` zurückgibt. Diese Methode kann einen `null` Wert zurückgeben, wenn das standardmäßige Rendering des Inhalts des Informations Fensters verwendet werden soll. Andernfalls sollte diese Methode eine Ansicht mit dem Inhalt des Info Fensters zurückgeben.

Ein Infofenster ist keine Live Ansicht, stattdessen konvertiert Android die Ansicht in eine statische Bitmap und zeigt diese auf dem Bild an. Dies bedeutet, dass ein Infofenster weder auf Berührungs Ereignisse noch Gesten reagieren kann, und es wird nicht automatisch selbst aktualisiert. Zum Aktualisieren eines Info Fensters ist es erforderlich, die [GoogleMap. showinfowindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) -Methode aufzurufen.

Die folgende Abbildung zeigt einige Beispiele für einige angepasste Informationsfenster. Das Bild auf der linken Seite hat seinen Inhalt angepasst, während das Bild auf der rechten Seite das Fenster und die Inhalte mit abgerundeten Ecken angepasst hat:

![Beispiel-markerfenster für Melbourne, einschließlich Icon und Population. Das rechte Fenster hat abgerundete Ecken.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlay

Anders als Marker, die eine bestimmte Position auf einer Karte identifizieren, ist [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) ein Bild, das verwendet wird, um eine Auflistung von Standorten oder einen Bereich auf der Karte zu identifizieren.

##### <a name="adding-a-groundoverlay"></a>Hinzufügen von GroundOverlay

Das Hinzufügen einer Grund Überlagerung zu einer Karte ähnelt dem Hinzufügen eines Markers zu einer Zuordnung. Zuerst wird ein [groundoverlayoptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) -Objekt erstellt. Dieses Objekt wird dann als Parameter an die [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) -Methode übergeben, die ein `GroundOverlay`-Objekt zurückgibt. Dieser Code Ausschnitt ist ein Beispiel für das Hinzufügen einer Grund Überlagerung zu einer Karte:

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

Der folgende Screenshot zeigt diese Überlagerung auf einer Karte:

[![Beispiel Zuordnung mit einem über laerten Bild eines Polarbären](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>Linien, Kreise und Polygone

Es gibt drei einfache Typen von geometrischen Abbildungen, die einer Karte hinzugefügt werden können:

- **Polyline** : Hierbei handelt es sich um eine Reihe verbundener Liniensegmente. Es kann einen Pfad auf einer Karte markieren oder eine geometrische Form erstellen.

- **Circle** : Hiermit wird ein Kreis auf der Karte gezeichnet.

- **Polygon** : Dies ist eine geschlossene Form zum Markieren von Bereichen in einer Karte.

##### <a name="polylines"></a>Polylinien

Eine [Polylinie](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) ist eine Liste von aufeinander folgenden `LatLng` Objekten, die die Scheitel Punkte der einzelnen Zeilen Segmente angeben. Eine Polylinie wird erstellt, indem zuerst ein `PolylineOptions` Objekt erstellt und der Punkt hinzugefügt wird. Das `PolylineOption` Objekt wird dann durch Aufrufen der `AddPolyline`-Methode an ein `GoogleMap` Objekt weitergegeben.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.

googleMap.AddPolyline(rectOptions);
```

##### <a name="circles"></a>Kreise

Kreise werden erstellt, indem zuerst ein [circleoption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) -Objekt instanziiert wird, das den Mittelpunkt und den Radius des Kreises in Meter angibt. Der Kreis wird auf der Karte gezeichnet, indem [GoogleMap. addcircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions))aufgerufen wird.
Der folgende Code Ausschnitt zeigt, wie Sie einen Kreis zeichnen:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>Polygone

`Polygon`s ähneln `Polyline`s, Sie sind jedoch nicht geöffnet. `Polygon`s sind eine geschlossene Schleife, deren innere ausgefüllt ist.
`Polygon`s werden auf die gleiche Weise wie ein `Polyline`erstellt, mit der Ausnahme, dass die [GoogleMap. AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) -Methode aufgerufen wurde.

Im Gegensatz zu einem `Polyline`ist ein `Polygon` selbst geschlossen. Das Polygon wird durch die `AddPolygon`-Methode geschlossen, indem eine Linie gezeichnet wird, die den ersten und den letzten Punkt verbindet. Der folgende Code Ausschnitt erstellt ein voll solides Rechteck über demselben Bereich wie der vorherige Code Ausschnitt im `Polyline` Beispiel.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>Reagieren auf Benutzer Ereignisse

Es gibt drei Arten von Interaktionen, die ein Benutzer mit einer Karte haben kann:

- **Markerklick** : der Benutzer klickt auf einen Marker.

- **Markerdrag** -der Benutzer hat auf einen mparser lange geklickt

- **Infofenster Click** -der Benutzer hat auf ein Infofenster geklickt.

Diese Ereignisse werden im folgenden ausführlicher erläutert.

### <a name="marker-click-events"></a>Markerklickereignisse

Das `MarkerClicked`-Ereignis wird ausgelöst, wenn der Benutzer auf einen Marker tippt. Dieses Ereignis akzeptiert ein `GoogleMap.MarkerClickEventArgs`-Objekt als Parameter. Diese Klasse enthält zwei Eigenschaften:

- `GoogleMap.MarkerClickEventArgs.Handled` &ndash; diese Eigenschaft auf `true` festgelegt werden, um anzugeben, dass der Ereignishandler das Ereignis verarbeitet hat. Wenn dieser auf `false` festgelegt ist, wird das Standardverhalten zusätzlich zum benutzerdefinierten Verhalten des Ereignis Handlers angezeigt.

- `Marker` &ndash; diese Eigenschaft ist ein Verweis auf den Marker, der das Ereignis `MarkerClick` ausgelöst hat.

Dieser Code Ausschnitt zeigt ein Beispiel für eine `MarkerClick`, die die Kameraposition an eine neue Position auf der Karte ändert:

```csharp
void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;

    var marker = markerClickEventArgs.Marker;
    if (marker.Id.Equals(gotMauiMarkerId))
    {
        LatLng InMaui = new LatLng(20.72110, -156.44776);

        // Move the camera to look at Maui.
        PositionPolarBearGroundOverlay(InMaui);
        googleMap.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(InMaui, 13));
        gotMauiMarkerId = null;
        polarBearMarker.Remove();
        polarBearMarker = null;
    }
    else
    {
        Toast.MakeText(this, $"You clicked on Marker ID {marker.Id}", ToastLength.Short).Show();
    }
}
```

### <a name="marker-drag-events"></a>Marker-Zieh Ereignisse

Dieses Ereignis wird ausgelöst, wenn der Benutzer den Marker ziehen möchte. Standardmäßig sind Marker nicht dragfähig. Ein Marker kann als dragfähig festgelegt werden, indem die `Marker.Draggable`-Eigenschaft auf `true` festgelegt wird oder indem die `MarkerOptions.Draggable`-Methode mit `true` als Parameter aufgerufen wird.

Um den Marker zu ziehen, muss der Benutzer zuerst auf den Marker und dann auf die Karte klicken. Wenn der Finger des Benutzers auf dem Bildschirm bewegt wird, wird der Marker verschoben. Wenn sich der Finger des Benutzers auf dem Bildschirm befindet, bleibt der Marker vorhanden.

In der folgenden Liste werden die verschiedenen Ereignisse beschrieben, die für einen dragbaren Marker ausgelöst werden:

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; dieses Ereignis ausgelöst wird, wenn der Benutzer den Marker zum ersten Mal zieht.

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; dieses Ereignis ausgelöst wird, wenn der Marker gezogen wird.

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; dieses Ereignis ausgelöst wird, wenn der Benutzer das Ziehen des Markers abgeschlossen hat.

Jede der `EventArgs` enthält eine einzelne Eigenschaft mit dem Namen `P0`, die ein Verweis auf das `Marker` Objekt ist, das gezogen wird.

### <a name="info-window-click-events"></a>Info Fenster Click-Ereignisse

Es kann jeweils nur ein Infofenster angezeigt werden. Wenn der Benutzer auf ein Infofenster in einer Karte klickt, wird vom Map-Objekt ein `InfoWindowClick` Ereignis ausgegeben. Der folgende Code Ausschnitt zeigt, wie ein Handler an das-Ereignis gesendet wird:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.InfoWindowClick += MapOnInfoWindowClick;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.Marker;
    // Do something with marker.
}
```

Beachten Sie, dass ein Infofenster ein statisches `View` ist, das als Bild auf der Karte gerendert wird. Alle Widgets (z. b. Schaltflächen, Kontrollkästchen oder Text Ansichten), die im Infofenster abgelegt werden, sind insoweit und können auf keines ihrer ganzzahligen Benutzer Ereignisse reagieren.

## <a name="related-links"></a>Verwandte Links

- [Simplemapdemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play Services](https://developers.google.com/android/guides/overview)
- [Google Maps Android-API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Abrufen eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [verwendet-Library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [Verwendung-Feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
