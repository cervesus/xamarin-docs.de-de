---
title: Verwenden der Google Maps-API in Ihrer Anwendung
description: 'Gewusst wie: Implementieren von Google Maps API v2-Features in ihrer Xamarin.Android-Anwendung.'
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 09/07/2018
ms.openlocfilehash: adcfb1457742d343f87a602885566107cf327e2d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027154"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Verwenden der Google Maps-API in Ihrer Anwendung

Die Maps-Anwendung bietet viele nützliche Verwendungsmöglichkeiten. Manchmal kann es aber wünschenswert sein, Karten direkt in Ihre Anwendung einzufügen. Zusätzlich zur integrierten Kartenanwendung bietet Google auch eine [native Karten-API für Android](https://developers.google.com/maps/documentation/android-sdk/intro).
Die Maps-API eignet sich für Fälle, in denen Sie mehr Kontrolle über die Kartendarstellung erhalten möchten. Die Maps-API bietet u. a. folgende Möglichkeiten:

- Programmgesteuerte Änderung des Standpunkts der Karte.
- Hinzufügen und Anpassen von Markern.
- Versehen einer Karte mit Anmerkungen durch Überlagerungen.

Im Gegensatz zur mittlerweile veralteten Google Maps Android-API v1 ist Google Maps Android API v2 Bestandteil von [Google Play Services](https://developers.google.com/android/guides/overview).
Eine Xamarin.Android-App muss einige erforderliche Voraussetzungen erfüllen, damit die Google Maps-API für Android verwendet werden kann.

## <a name="google-maps-api-prerequisites"></a>Voraussetzungen für die Google Maps-API

Damit Sie die Maps-API verwenden können, müssen mehrere Schritte ausgeführt werden, z. B.:

- [Abrufen eines Maps-API-Schlüssels](#obtain-maps-key)
- [Installieren des Google Play Services SDK](#install-gps-sdk)
- [Installieren des Xamarin.GooglePlayServices.Maps-Pakets von NuGet](#install-gpsmaps-nuget)
- [Angeben der erforderlichen Berechtigungen](#declare-permissions)
- [Erstellen eines Emulators mit den Google-APIs (optional)](#create-emulator-with-google-api)

### <a name="obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Abrufen eines Google Maps-API-Schlüssels

Der erste Schritt besteht darin, einen Google Maps-API-Schlüssel anzufordern. (Sie können einen Schlüssel der alten Google Maps v1-API nicht verwenden.) Informationen zum Abrufen und Verwenden des API-Schlüssels mit Xamarin.Android finden Sie unter [Abrufen eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

### <a name="install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Installieren des Google Play Services SDK

Google Play Services ist eine Technologie von Google, mit der Android-Anwendungen verschiedene Google-Features wie Google+, In-App-Abrechnung und Maps nutzen können. Diese Features sind auf Android-Geräten als Hintergrunddienste verfügbar, die im [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en) enthalten sind.

Android-Anwendungen interagieren über die Google Play Services-Clientbibliothek mit Google Play Services. Diese Bibliothek enthält die Schnittstellen und Klassen für die einzelnen Dienste, z. B. Maps. Das folgende Diagramm veranschaulicht die Beziehung zwischen einer Android-Anwendung und Google Play Services:

![Diagramm zur Veranschaulichung der Aktualisierung des Google Play Services APK vom Google Play Store](maps-api-images/play-services-diagram.png)

Die Android Maps-API wird im Rahmen von Google Play Services bereitgestellt.
Damit eine Xamarin.Android-Anwendung die Maps-API verwenden kann, muss das Google Play Services SDK mit dem [Android SDK-Manager](~/android/get-started/installation/android-sdk.md) installiert werden. Der folgende Screenshot zeigt, wo sich der Google Play Services-Client im Android SDK-Manager befindet:

![Anzeige von Google Play Services im Android SDK-Manager unter „Extras“](maps-api-images/image01.png)

> [!NOTE]
> Das Google Play Services APK ist ein lizenziertes Produkt, das möglicherweise nicht auf allen Geräten vorhanden ist. Wenn es nicht installiert ist, funktioniert Google Maps nicht auf dem Gerät.

### <a name="install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> Installieren des Xamarin.GooglePlayServices.Maps-Pakets von NuGet

Das [Xamarin.GooglePlayServices.Maps-Paket](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) enthält die Xamarin.Android-Bindungen für die Google Play Services Maps-API.
Klicken Sie mit der rechten Maustaste auf den Ordner **Verweise** Ihres Projekts im Projektmappen-Explorer, um das Google Play Services Maps-Paket hinzuzufügen, und klicken Sie auf **NuGet -Pakete verwalten...** .

![Projektmappen-Explorer mit Anzeige des Kontextmenüelements „NuGet-Pakete verwalten“ unter „Verweise“](maps-api-images/image02.png)

Daraufhin wird der **NuGet Paket-Manager** geöffnet. Klicken Sie auf **Durchsuchen**, und geben Sie **Xamarin Google Play Services Maps** im Suchfeld ein. Wählen Sie **Xamarin.GooglePlayServices.Maps** aus und klicken Sie auf **Installieren**. (Klicken Sie auf **Aktualisieren**, wenn dieses Paket bereits installiert wurde.)

[![NuGet-Paket-Manager mit ausgewähltem Xamarin.GooglePlayServices.Maps-Paket](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Beachten Sie, dass die folgenden Abhängigkeitspakete ebenfalls installiert werden:

- **Xamarin.GooglePlayServices.Base**
- **Xamarin.GooglePlayServices.Basement**
- **Xamarin.GooglePlayServices.Tasks**

### <a name="specify-the-required-permissions"></a><a name="declare-permissions" /> Angeben der erforderlichen Berechtigungen

Apps müssen die Hardware- und Berechtigungsanforderungen erfüllen, damit die Google Maps-API verwendet werden kann.  Einige Berechtigungen werden automatisch durch das Google Play Services SDK erteilt, sodass ein Entwickler diese nicht explizit zu **AndroidManifest.XML** hinzufügen muss:

- **Zugriff auf den Netzwerkzustand** &ndash; Die Maps-API muss überprüfen können, ob die Kartenkacheln heruntergeladen werden können.

- **Internetzugang** &ndash; Damit die Kartenkacheln heruntergeladen werden können und eine Kommunikation mit den Google Play-Servern für den API-Zugriff möglich ist, ist ein Internetzugang erforderlich.

Die folgenden Berechtigungen und Features müssen in der **AndroidManifest.XML** für die Google Maps Android-API angegeben werden:

- **OpenGL ES v2** &ndash; Die Anwendung muss die Anforderung für OpenGL ES v2 deklarieren.

- **Google Maps-API-Schlüssel** &ndash; Der API-Schlüssel wird verwendet, um zu bestätigen, dass die Anwendung registriert und für die Verwendung von Google Play Services autorisiert ist. Weitere Informationen zu diesem Schlüssel finden Sie unter [Abrufen eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

- **Anforderung des alten Apache HTTP-Clients** &ndash; Apps für Android 9.0 (API-Ebene 28) oder höher müssen angeben, dass der alte Apache HTTP-Client eine optionale Bibliothek ist, die verwendet werden soll.

- **Zugriff auf die webbasierten Google-Dienste** &ndash; Die Anwendung benötigt Berechtigungen für den Zugriff auf Google-Webdienste, die die Android Maps-API unterstützen.

- **Berechtigungen für Google Play Services Benachrichtigungen** &ndash; Der Anwendung muss die Berechtigung zum Empfangen von Remotebenachrichtigungen von Google Play Services erteilt werden.

- **Zugriff auf Standortanbieter** &ndash; Diese Berechtigungen sind optional.
   Sie ermöglichen es der `GoogleMap`-Klasse, den Standort des Geräts auf der Karte anzuzeigen.

Außerdem wurde die Apache HTTP-Clientbibliothek Android 9 aus „bootclasspath“ entfernt und ist daher für Anwendungen, die auf API 28 oder höher ausgerichtet sind, nicht verfügbar. Damit der Apache HTTP-Client weiterhin in Anwendungen mit der API-Zielebene 28 oder höher verwendet werden kann, muss die folgende Zeile zum Knoten `application` der Datei **AndroidManifest.XML** hinzugefügt werden:

```xml
<application ...>
   ...
   <uses-library android:name="org.apache.http.legacy" android:required="false" />    
</application>
```

> [!NOTE]
> Bei sehr alten Versionen des Google Play SDK musste eine App die `WRITE_EXTERNAL_STORAGE`-Berechtigung anfordern. Diese Anforderung gilt bei den aktuellen Xamarin-Bindungen für Google Play Services nicht mehr.

Der folgende Codeausschnitt ist ein Beispiel für die Einstellungen, die zu **AndroidManifest.XML** hinzugefügt werden müssen:

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

Zusätzlich zum Anfordern der Berechtigungen in **AndroidManifest.XML** muss eine App auch Laufzeitberechtigungsprüfungen für die Berechtigungen `ACCESS_COARSE_LOCATION` und `ACCESS_FINE_LOCATION` ausführen. Weitere Informationen zum Ausführen von Laufzeitberechtigungsprüfungen finden Sie in der Anleitung zu [Xamarin.Android-Berechtigungen](~/android/app-fundamentals/permissions.md).

### <a name="create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Erstellen eines Emulators mit Google-APIs

Falls kein physisches Android-Gerät mit Google Play-Diensten installiert ist, können Sie ein Emulatorimage für die Entwicklung erstellen. Weitere Informationen finden Sie unter [Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).

## <a name="the-googlemap-class"></a>Die GoogleMap-Klasse

Wenn die Voraussetzungen erfüllt sind, kann die Entwicklung der Anwendung unter Verwendung der Android Maps-API beginnen. Die [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap)-Klasse ist die Haupt-API, die eine Xamarin.Android-Anwendung für die Anzeige und Interaktion mit Google Maps für Android verwendet. Diese Klasse erfüllt folgende Aufgaben:

- Interagieren mit Google Play-Diensten, um die Anwendung beim Google-Webdienst zu autorisieren.

- Herunterladen, Zwischenspeichern und Anzeigen der Kartenkacheln.

- Anzeigen von Steuerelementen wie Schwenken und Vergrößern auf der Benutzeroberfläche.

- Zeichnen von Markern und geometrischen Formen auf Karten.

Es gibt zwei Möglichkeiten, um `GoogleMap` zu einer Aktivität hinzuzufügen:

- **MapFragment** – [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) ist ein spezielles Fragment, das als Host für das `GoogleMap`-Objekt fungiert. Für `MapFragment` ist die Android API-Ebene 12 oder höher erforderlich.
   Ältere Android-Versionen können [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment) verwenden.  Diese Anleitung konzentriert sich auf die Verwendung der `MapFragment`-Klasse.

- **MapView** – [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) ist eine spezielle View-Unterklasse, die als Host für ein `GoogleMap`-Objekt fungieren kann. Benutzer dieser Klasse müssen alle Lebenszyklusmethoden einer Aktivität an die `MapView`-Klasse weiterleiten.

Alle diese Container machen eine `Map`-Eigenschaft verfügbar, die eine Instanz von `GoogleMap` zurückgibt. Vorzugsweise sollte die [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment)-Klasse verwendet werden, da es sich um eine einfachere API handelt, die den Umfang von Boilerplate-Code reduziert, den ein Entwickler manuell implementieren muss.

### <a name="adding-a-mapfragment-to-an-activity"></a>Hinzufügen von MapFragment zu einer Aktivität

Der folgende Screenshot zeigt ein Beispiel für ein einfaches `MapFragment`:

[![Screenshot für ein Gerät, auf dem ein Google-Kartenfragment angezeigt wird](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Ähnlich wie bei anderen Fragment-Klassen gibt es zwei Möglichkeiten, ein `MapFragment` zu einer Aktivität hinzuzufügen:

- **Deklarativ** – `MapFragment` kann über die XML-Layoutdatei für die Aktivität hinzugefügt werden. Der folgende XML-Codeausschnitt zeigt ein Beispiel für die Verwendung des `fragment`-Elements:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

- **Programmgesteuert** – `MapFragment` kann programmgesteuert mit der [`MapFragment.NewInstance`](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance())-Methode instanziiert und dann zu einer Aktivität hinzugefügt werden. Dieser Codeausschnitt zeigt die einfachste Möglichkeit, um ein `MapFragment`-Objekt zu instanziieren und zu einer Aktivität hinzuzufügen:

    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    Es ist möglich, das `MapFragment`-Objekt zu konfigurieren, indem ein [`GoogleMapOptions`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions)-Objekt an `NewInstance` übergeben wird. Dies wird im Abschnitt [GoogleMap-Eigenschaften](#googlemap_object) weiter unten in dieser Anleitung erläutert.

Die `MapFragment.GetMapAsync`-Methode wird verwendet, um die [`GoogleMap`](#googlemap_object) zu initialisieren, die vom Fragment gehostet wird, und um einen Verweis auf das Map-Objekt zu erhalten, das von `MapFragment` gehostet wird. Diese Methode übernimmt ein Objekt, das die `IOnMapReadyCallback`-Schnittstelle implementiert.

Diese Schnittstelle verfügt über eine einzelne Methode, `IMapReadyCallback.OnMapReady(MapFragment map)`, die aufgerufen wird, wenn eine Interaktion der App mit dem `GoogleMap`-Objekt möglich ist. Der folgende Codeausschnitt zeigt, wie eine Android-Aktivität ein `MapFragment` initialisieren und die `IOnMapReadyCallback`-Schnittstelle implementieren kann:

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

Es gibt fünf verschiedenartige Karten, die über die Google Maps-API verfügbar sind:

- **Normal** – Dies ist der Standardkartentyp. Auf dieser Karte werden Straßen und wichtige Landschaftsstrukturen zusammen mit einigen Sehenswürdigkeiten (z. B. Gebäude und Brücken) angezeigt.

- **Satellite** – Auf dieser Karte werden Satellitenfotos angezeigt.

- **Hybrid** – Auf dieser Karte werden Satellitenfotos und Straßenkarten angezeigt.

- **Terrain** – Auf dieser Karte werden hauptsächlich topografische Strukturen mit einigen Straßen angezeigt.

- **None** – Diese Karte lädt keine Kacheln, sondern wird als leeres Raster gerendert.

Die folgende Abbildung zeigt drei der verschiedenen Kartentypen (von links nach rechts: Normal, Hybrid, Terrain):

[![Drei Beispielscreenshots für Karten: Normal, Hybrid und Terrain](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Mit der `GoogleMap.MapType`-Eigenschaft wird der angezeigte Kartentyp festgelegt oder geändert. Der folgende Codeausschnitt veranschaulicht, wie eine Satellitenkarte angezeigt wird:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```

### <a name="googlemap-properties"></a><a name="googlemap_object" />GoogleMap-Eigenschaften

`GoogleMap` definiert mehrere Eigenschaften, die die Funktionalität und Darstellung der Karte steuern können. Eine Möglichkeit, den Anfangszustand einer `GoogleMap` zu konfigurieren, besteht darin, beim Erstellen von `MapFragment` ein [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions)-Objekt zu übergeben. Der folgende Codeausschnitt ist ein Beispiel für die Verwendung eines `GoogleMapOptions`-Objekts beim Erstellen von `MapFragment`:

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

Die andere Möglichkeit, eine `GoogleMap` zu konfigurieren, besteht darin, die Eigenschaften in den [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) des Map-Objekts zu ändern. Das nächste Codebeispiel zeigt, wie eine `GoogleMap` konfiguriert wird, um die Zoomsteuerelemente und einen Kompass anzuzeigen:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>Interaktion mit der GoogleMap

Die Android Maps-API umfasst APIs, mit denen eine Aktivität den Standpunkt ändern, Marker hinzufügen, benutzerdefinierte Überlagerungen einfügen oder geometrische Formen zeichnen kann. In diesem Abschnitt wird erläutert, wie einige dieser Aufgaben in Xamarin.Android bewerkstelligt werden.

### <a name="changing-the-viewpoint"></a>Ändern des Standpunkts

Karten werden basierend auf der Mercator-Projektion als flache Ebene auf dem Bildschirm modelliert. Die Kartenansicht ist die Sicht einer *Kamera*, die direkt nach unten auf diese Ebene gerichtet ist. Die Position der Kamera kann gesteuert werden, indem Position, Zoom, Kippwinkel und Richtung geändert werden. Die [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate)-Klasse wird verwendet, um die Kameraposition zu verschieben. `CameraUpdate`-Objekte werden nicht direkt instanziiert, stattdessen stellt die Maps-API die [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory)-Klasse bereit.

Nachdem ein `CameraUpdate`-Objekt erstellt wurde, wird es als Parameter an die [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate))- oder [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate))-Methode übergeben. Die `MoveCamera`-Methode aktualisiert die Karte sofort, während die `AnimateCamera`-Methode einen glatten, animierten Übergang ermöglicht.

Dieser Codeausschnitt ist ein einfaches Beispiel für die Verwendung von `CameraUpdateFactory`, um ein `CameraUpdate` zu erstellen, mit dem der Zoomfaktor der Karte um eins erhöht wird:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Die Maps-API stellt eine [CameraPosition](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) bereit, die alle möglichen Werte für die Kameraposition aggregiert. Eine Instanz dieser Klasse kann an die [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29)-Methode übergeben werden, die ein `CameraUpdate`-Objekt zurückgibt. Die Maps-API enthält auch die [CameraPosition.Builder](https://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html)-Klasse, die eine Fluent-API zum Erstellen von `CameraPosition`-Objekten bereitstellt.
Der folgende Codeausschnitt zeigt ein Beispiel für das Erstellen von `CameraUpdate` aus einer `CameraPosition` und dessen Verwendung zum Ändern der Kameraposition in einer `GoogleMap`:

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

Im vorherigen Codeausschnitt wird eine bestimmte Position auf der Karte durch die [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng)-Klasse dargestellt. Der Zoomfaktor wird auf 18 festgelegt (ein willkürlicher Zoomfaktor, der von Google Maps verwendet wird). Die Bearing-Richtung ist der Kompasswert im Uhrzeigersinn ausgehend von der Nordrichtung. Die Tilt-Eigenschaft steuert den Sichtwinkel und legt einen Winkel von 25 Grad zur Senkrechten fest. Der folgende Screenshot zeigt die `GoogleMap` nach Ausführung des obigen Codes:

[![Google Map-Beispiel mit einer angegebenen Position mit einem geneigten Sichtwinkel](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)

### <a name="drawing-on-the-map"></a>Zeichnen auf der Karte

Die Android Maps-API umfasst APIs zum Zeichnen folgender Elemente auf einer Karte:

- **Marker** – Hierbei handelt es sich um spezielle Symbole, mit denen eine einzelne Position auf der Karte gekennzeichnet wird.

- **Überlagerungen** – Hierbei handelt es sich um ein Bild, mit dem mehrere Positionen oder Bereiche auf der Karte gekennzeichnet werden können.

- **Linien, Polygone und Kreise** – Diese APIs bieten Aktivitäten die Möglichkeit, Formen zu einer Karte hinzuzufügen.

#### <a name="markers"></a>Marker

Die Maps-API umfasst eine [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker)-Klasse, die alle Daten einer einzelnen Position auf einer Karte kapselt. Die Marker-Klasse verwendet standardmäßig ein Standardsymbol, das von Google Maps bereitgestellt wird. Es ist möglich, das Aussehen eines Markers anzupassen und auf Benutzerklicks zu reagieren.

##### <a name="adding-a-marker"></a>Hinzufügen eines Markers

Um einen Marker zu einer Karte hinzuzufügen, müssen Sie ein neues [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions)-Objekt erstellen und dann die [AddMarker](https://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29)-Methode für eine `GoogleMap`-Instanz aufzurufen. Diese Methode gibt ein [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker)-Objekt zurück.

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

[![Google Map-Beispiel mit einem Marker und einem Infofenster für Vimy Ridge](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)

##### <a name="customizing-a-marker"></a>Anpassen eines Markers

Es ist möglich, das vom Marker verwendete Symbol anzupassen, indem die `MarkerOptions.InvokeIcon`-Methode beim Hinzufügen des Markers zur Karte aufgerufen wird.
Diese Methode übernimmt ein [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor)-Objekt, das die zum Rendern des Symbols erforderlichen Daten enthält. Die [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory)-Klasse stellt einige Hilfsmethoden bereit, um die `BitmapDescriptor`-Erstellung zu vereinfachen. In der folgenden Liste sind einige dieser Methoden aufgeführt:

- `DefaultMarker(float colour)` &ndash; Verwendet den Standardmarker von Google Maps, ändert aber die Farbe.

- `FromAsset(string assetName)` &ndash; Verwendet ein benutzerdefiniertes Symbol aus der angegebenen Datei im Ordner für Assets.

- `FromBitmap(Bitmap image)` &ndash; Verwendet die angegebene Bitmap als Symbol.

- `FromFile(string fileName)` &ndash; Erstellt das benutzerdefinierte Symbol aus der Datei im angegebenen Pfad.

- `FromResource(int resourceId)` &ndash; Erstellt ein benutzerdefiniertes Symbol aus der angegebenen Ressource.

Der folgende Codeausschnitt zeigt ein Beispiel für das Erstellen eines blaugrünen Standardmarkers:

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

#### <a name="info-windows"></a>Infofenster

*Infofenster* sind spezielle Popupfenster, in denen Informationen angezeigt werden, wenn der Benutzer auf einen bestimmten Marker tippt. Standardmäßig wird im Infofenster der Inhalt des Markertitels angezeigt. Wenn kein Titel zugewiesen wurde, wird kein Infofenster angezeigt. Es kann jeweils nur ein Infofenster angezeigt werden.

Sie können das Infofenster anpassen, indem Sie die [GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter)-Schnittstelle implementieren. Es gibt zwei wichtige Methoden in dieser Schnittstelle:

- `public View GetInfoWindow(Marker marker)` &ndash; – Diese Methode wird aufgerufen, um ein benutzerdefiniertes Infofenster für einen Marker anzuzeigen. Wird `null` zurückgegeben, wird das Standardfensterrendering verwendet. Wenn die Methode eine Ansicht zurückgibt, wird die betreffende Ansicht im Rahmen des Infofensters platziert.

- `public View GetInfoContents(Marker marker)` &ndash; – Diese Methode wird nur aufgerufen, wenn GetInfoWindow `null` zurückgibt. Diese Methode kann einen `null`-Wert zurückgeben, wenn das Standardrendering des Infofensterinhaltes verwendet werden soll. Andernfalls sollte diese Methode eine Ansicht mit dem Inhalt des Infofensters zurückgeben.

Ein Infofenster ist keine Liveansicht, Android konvertiert die Ansicht stattdessen in eine statische Bitmap und zeigt diese im Bild an. Dies bedeutet, dass ein Infofenster nicht auf Berührungsereignisse oder Berührungsgesten reagieren kann und nicht automatisch aktualisiert wird. Zum Aktualisieren eines Infofensters muss die [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow())-Methode aufgerufen werden.

Die folgende Abbildung veranschaulicht einige Beispiele für angepasste Infofenster. Das Bild auf der linken Seite umfasst angepasste Inhalte, während im Bild auf der rechten Seite Fenster und Inhalt mit abgerundeten Ecken angepasst wurden:

![Beispielmarkerfenster für Melbourne mit Symbol und Bevölkerung. Das rechte Fenster weist abgerundete Ecken auf.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>Geländeüberlagerungen

Anders als Marker, die eine bestimmte Position auf einer Karte kennzeichnen, ist [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) ein Bild, das zum Kennzeichnen mehrerer Positionen oder eines Bereichs auf der Karte verwendet wird.

##### <a name="adding-a-groundoverlay"></a>Hinzufügen einer Geländeüberlagerung

Das Hinzufügen einer Geländeüberlagerung zu einer Karte ähnelt dem Hinzufügen eines Markers zu einer Karte. Zuerst wird ein [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions)-Objekt erstellt. Dieses Objekt wird dann als Parameter an die [`GoogleMap.AddGroundOverlay`](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions))-Methode übergeben, die ein `GroundOverlay`-Objekt zurückgibt. Dieser Codeausschnitt ist ein Beispiel für das Hinzufügen einer Geländeüberlagerung zu einer Karte:

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

Der folgende Screenshot zeigt diese Überlagerung auf einer Karte:

[![Beispielkarte mit dem überlagerten Bild eines Eisbären](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)

#### <a name="lines-circles-and-polygons"></a>Linien, Kreise und Polygone

Es gibt drei verschiedene geometrischen Figuren, die zu einer Karte hinzugefügt werden können:

- **Polyline** – Hierbei handelt es sich um eine Reihe verbundener Liniensegmente. Damit kann ein Weg auf einer Karte markiert oder eine geometrische Form erstellt werden.

- **Circle** – Zeichnet einen Kreis auf der Karte.

- **Polygon** – Hierbei handelt es sich um eine geschlossene Form zum Markieren von Bereichen in einer Karte.

##### <a name="polylines"></a>Polylinien

Bei einer [Polylinie](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) handelt es sich um eine Liste von aufeinander folgenden `LatLng`-Objekten, die Eckpunkte der einzelnen Liniensegmente angeben. Eine Polylinie wird erstellt, indem zuerst ein `PolylineOptions`-Objekt erstellt wird und dann Punkte hinzugefügt werden. Das `PolylineOption`-Objekt wird dann durch Aufrufen der `AddPolyline`-Methode an ein `GoogleMap`-Objekt übergeben.

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

Kreise werden erstellt, indem zuerst ein [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions)-Objekt instanziiert wird, das den Mittelpunkt und den Radius des Kreises in Metern angibt. Der Kreis wird auf der Karte gezeichnet, indem [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions)) aufgerufen wird.
Der folgende Codeausschnitt veranschaulicht, wie ein Kreis gezeichnet wird:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```

##### <a name="polygons"></a>Polygone

`Polygon`e weisen Ähnlichkeit mit `Polyline`-Linien auf, sind jedoch geschlossen. `Polygon`e bilden eine geschlossene Schleife mit einem gefüllten Innenbereich.
`Polygon`e werden genauso wie eine `Polyline` erstellt, außer dass die [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions))-Methode aufgerufen wird.

Im Gegensatz zu einer `Polyline` ist ein `Polygon` selbstschließend. Das Polygon wird automatisch geschlossen, indem die `AddPolygon`-Methode eine Verbindungslinie zwischen dem ersten und dem letzten Punkt zeichnet. Der folgende Codeausschnitt erstellt ein massives Rechteck über demselben Bereich wie der obige Codeausschnitt im `Polyline`-Beispiel.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```

## <a name="responding-to-user-events"></a>Reagieren auf Benutzerereignisse

Es gibt drei verschiedenartige Benutzerinteraktionen für eine Karte:

- **MarkerClick** – Der Benutzer klickt auf einen Marker.

- **MarkerDrag** – Der Benutzer zieht einen Marker.

- **InfoWindowClick** – Der Benutzer klickt auf ein Infofenster.

Diese Ereignisse werden nachstehend ausführlicher erläutert.

### <a name="marker-click-events"></a>MarkerClick-Ereignisse

Das `MarkerClicked`-Ereignis wird ausgelöst, wenn der Benutzer auf einen Marker tippt. Dieses Ereignis akzeptiert ein `GoogleMap.MarkerClickEventArgs`-Objekt als Parameter. Diese Klasse enthält zwei Eigenschaften:

- `GoogleMap.MarkerClickEventArgs.Handled` &ndash; – Diese Eigenschaft sollte auf `true` festgelegt werden, um anzugeben, dass der Ereignishandler das Ereignis verarbeitet hat. Bei Festlegung auf `false` tritt das Standardverhalten zusätzlich zum benutzerdefinierten Verhalten des Ereignishandlers auf.

- `Marker` &ndash; – Diese Eigenschaft ist ein Verweis auf den Marker, der das `MarkerClick`-Ereignis ausgelöst hat.

Dieser Codeausschnitt zeigt ein Beispiel für ein `MarkerClick`, das die Kameraposition in eine neue Position auf der Karte ändert:

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

### <a name="marker-drag-events"></a>MarkerDrag-Ereignisse

Dieses Ereignis wird ausgelöst, wenn der Benutzer den Marker ziehen möchte. Standardmäßig können Marker nicht gezogen werden. Ein Marker kann als ziehbar festgelegt werden, indem die `Marker.Draggable`-Eigenschaft auf `true` festgelegt wird oder die `MarkerOptions.Draggable`-Methode mit `true` als Parameter aufgerufen wird.

Um den Marker zu ziehen, muss der Benutzer zuerst länger auf den Marker klicken und der Finger muss danach auf der Karte bleiben. Wird der Finger des Benutzers auf dem Bildschirm gezogen, wird der Marker bewegt. Hebt der Benutzer den Finger vom Bildschirm, bleibt der Marker an der betreffenden Stelle stehen.

Die folgende Liste enthält eine Beschreibung der verschiedenen Ereignisse, die für einen ziehbaren Marker ausgelöst werden:

- `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; – Dieses Ereignis wird ausgelöst, wenn der Benutzer länger auf den Marker klickt.

- `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; – Dieses Ereignis wird ausgelöst, wenn der Marker gezogen wird.

- `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; – Dieses Ereignis wird ausgelöst, wenn der Benutzer das Ziehen des Markers beendet.

Alle `EventArgs` enthalten eine einzelne Eigenschaft namens `P0`, die auf das gezogene `Marker`-Objekt verweist.

### <a name="info-window-click-events"></a>InfoWindowClick-Ereignisse

Es kann jeweils nur ein Infofenster angezeigt werden. Wenn der Benutzer auf ein Infofenster in einer Karte klickt, wird vom Map-Objekt ein `InfoWindowClick`-Ereignis ausgelöst. Der folgende Codeausschnitt zeigt, wie ein Handler mit dem Ereignis verbunden wird:

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

Beachten Sie, dass ein Infofenster eine statische `View` ist, die als Bild auf der Karte gerendert wird. Alle Widgets wie Schaltflächen, Kontrollkästchen oder Textansichten, die im Infofenster platziert werden, sind inaktiv und können nicht auf integrierte Benutzerereignisse reagieren.

## <a name="related-links"></a>Verwandte Links

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play Services](https://developers.google.com/android/guides/overview)
- [Google Maps Android API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Abrufen eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [uses-library](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element)
