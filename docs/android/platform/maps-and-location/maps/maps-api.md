---
title: Verwenden das Google Maps-API in Ihrer Anwendung
description: Wie Sie Google Maps-API v2-Features in Ihrer Xamarin.Android-Anwendung zu implementieren.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 09/07/2018
ms.openlocfilehash: db442f45c615b680264e79262f62062eb6a6bfd5
ms.sourcegitcommit: f5fce8308b2e7c39c5b0c904e5f38a4ce2b55c87
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/04/2019
ms.locfileid: "54012294"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Verwenden die Google Maps-API in Ihrer Anwendung

Mithilfe der Maps-Anwendung eignet sich hervorragend, aber manchmal sollen Maps direkt in der Anwendung. Zusätzlich zu den integrierten ordnet Anwendung Google bietet auch eine [native Zuordnungs-API für Android](https://developers.google.com/maps/documentation/android-sdk/intro).
Die Maps-API eignet sich für Fälle, in denen mehr Kontrolle über die Benutzeroberfläche für die Zuordnung beibehalten werden sollen. Dinge, die mit der Maps-API möglich sind enthalten:

-  Ändern programmgesteuert den Standpunkt der Zuordnung.
-  Das Hinzufügen und Anpassen von Marker.
-  Hinzufügen einer Anmerkung zu einer Zuordnung mit überlagert.

Im Gegensatz zu der jetzt veralteten Google Maps Android-API-Version 1, Google Maps-Android-API v2 ist Teil der [Google Play Services](https://developers.google.com/android/guides/overview).
Eine Xamarin.Android-app muss einige obligatorischen Voraussetzungen erfüllen, bevor es möglich, die Google Maps Android-API zu verwenden ist.


## <a name="google-maps-api-prerequisites"></a>Voraussetzungen für Google Maps-API

Mehrere Schritte müssen ausgeführt werden, bevor Sie die Maps-API verwenden, können einschließlich:

-  [Abrufen eines Maps-API-Schlüssels](#obtain-maps-key)
-  [Die Google Play-Dienste SDK installieren](#install-gps-sdk)
-  [Installieren Sie das Xamarin.GooglePlayServices.Maps-Paket von NuGet](#install-gpsmaps-nuget)
-  [Geben Sie die erforderlichen Berechtigungen](#declare-permissions)
-  [Erstellen Sie optional einen Emulator mit den Google-APIs](#create-emulator-with-google-api)


### <a name="a-nameobtain-maps-key-obtain-a-google-maps-api-key"></a><a name="obtain-maps-key" />Erhalten Sie einen Google Maps-API-Schlüssel

Der erste Schritt ist, rufen Sie einen Google Maps-API-Schlüssel (Beachten Sie, dass Sie einen API-Schlüssel von den älteren Google Maps-v1-API wiederverwenden können nicht). Weitere Informationen zum Herunterladen und verwenden den API-Schlüssel, die mit Xamarin.Android, finden Sie unter [ein Google Maps-API-Schlüssel abrufen](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
 

### <a name="a-nameinstall-gps-sdk--install-the-google-play-services-sdk"></a><a name="install-gps-sdk" /> Die Google Play-Dienste SDK installieren

Google Play Services ist eine Technologie von Google, mit dem Android-Anwendungen auf verschiedenen Google-Features wie z. B. Google +, In-App-Abrechnung und Zuordnungen nutzen zu können. Diese Funktionen sind auf Android-Geräten zugegriffen werden kann, als Hintergrunddienste, die in enthalten sind die [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Android-Anwendungen interagieren Sie mit Google Play-Dienste über die Google Play Services-Clientbibliothek. Diese Bibliothek enthält die Schnittstellen und Klassen für die einzelnen Dienste wie Karten. Das folgende Diagramm zeigt die Beziehung zwischen einem Android-Anwendung und die Google Play-Dienste:

![Diagramm zur Veranschaulichung der Google Play Store, aktualisieren das Google Play Services-APK](maps-api-images/play-services-diagram.png)

Die Android Maps-API wird als Teil von Google Play Services bereitgestellt.
Bevor eine Xamarin.Android-Anwendung die Maps-API verwenden kann, die Google Play Services SDK muss installiert werden mithilfe der [Android SDK Manager](~/android/get-started/installation/android-sdk.md). Der folgende Screenshot zeigt, wo im Android SDK-Manager des Google Play Services-Clients gefunden werden kann:

![Google Play Services wird unter Extras im Android SDK-Manager angezeigt.](maps-api-images/image01.png)

> [!NOTE]
> Die Google Play-Dienste ist APK ein lizenziertes Produkt, das möglicherweise nicht auf allen Geräten vorhanden. Wenn es nicht installiert ist, klicken Sie dann funktioniert Google Maps auf dem Gerät nicht.

### <a name="a-nameinstall-gpsmaps-nuget--install-the-xamaringoogleplayservicesmaps-package-from-nuget"></a><a name="install-gpsmaps-nuget" /> Installieren Sie das Xamarin.GooglePlayServices.Maps-Paket von NuGet

Die [Xamarin.GooglePlayServices.Maps Paket](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps) enthält die Xamarin.Android-Bindungen für die Google Play Services Karten-API.
Um die Google Play Services Map-Paket hinzuzufügen, Informationen zu diesem mit der rechten Maustaste die **Verweise** Ordner des Projekts im Projektmappen-Explorer, und auf **NuGet-Pakete verwalten...** :

![Projektmappen-Explorer zeigt NuGet-Pakete verwalten-Kontextmenüelement unter Verweise](maps-api-images/image02.png)

Daraufhin wird die **NuGet Package Manager**. Klicken Sie auf **Durchsuchen** , und geben Sie **Xamarin Google Play Services Maps** in das Suchfeld ein. Wählen Sie **Xamarin.GooglePlayServices.Maps** , und klicken Sie auf **installieren**. (Wenn Sie dieses Paket bereits installiert wurde, klicken Sie auf **Update**.):

[![NuGet-Paket-Manager mit der ausgewählten Xamarin.GooglePlayServices.Maps-Paket](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Beachten Sie, dass die folgenden abhängigkeitspakete installiert werden:

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**

### <a name="a-namedeclare-permissions--specify-the-required-permissions"></a><a name="declare-permissions" /> Geben Sie die erforderlichen Berechtigungen

Apps müssen die Anforderungen Hardware und die Berechtigung, um die Google Maps-API verwenden identifizieren.  Einige Berechtigungen werden von Google Play Services SDK automatisch gewährt, und es ist nicht erforderlich für einen Entwickler explizit hinzugefügt **AndroidManfest.XML**:

-  **Zugriff auf den Netzwerkstatus** &ndash; die Maps-API muss in der Lage, überprüfen Sie, ob die Maps-Kacheln heruntergeladen werden kann.

-  **Zugriff auf das Internet** &ndash; Zugriff auf das Internet ist erforderlich, laden die Maps-Kacheln und die Kommunikation mit Google Play-Server für die API-Zugriff.

Die folgenden Berechtigungen und Funktionen angegeben werden der **"androidmanifest.xml"** für die Google Maps Android-API:

-  **OpenGL ES-v2** &ndash; die Anwendung muss die Notwendigkeit von OpenGL ES-v2 deklarieren.

-  **Google Maps-API-Schlüssel** &ndash; der API-Schlüssel wird verwendet, um sicherzustellen, dass die Anwendung registriert und autorisiert, Google Play-Dienste zu verwenden. Finden Sie unter [einen Google Maps-API-Schlüssel abrufen](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) ausführliche Informationen zu diesen Schlüssel.
   
- **Fordern Sie den Apache-HTTP-Legacyclient** &ndash; Apps für Android 9.0 (API-Ebene 28) oder höher müssen angeben, dass der ältere Apache HTTP Client eine optionale Bibliothek verwenden. 

-  **Zugriff auf die Google-Web-basierten Dienste** &ndash; die Anwendung benötigt Berechtigungen, um die Google Web-Dienste zugreifen, die die Android-API-Zuordnungen zu sichern.

-  **Berechtigungen für Google Play Services-Benachrichtigungen** &ndash; die Anwendung zum Empfangen von remotebenachrichtigungen aus Google Play Services-Berechtigung erteilt werden.

-  **Zugriff auf den Location-Anbieter** &ndash; Hierbei handelt es sich um optionale Berechtigungen.
   Sie können die `GoogleMap` Klasse, um den Speicherort des Geräts auf der Karte angezeigt.


> [!NOTE]
> Sehr alte Versionen von Google Play-SDK erforderlich sind, eine app zum Anfordern der `WRITE_EXTERNAL_STORAGE` Berechtigung. Diese Anforderung ist nicht mehr mit den aktuellen Xamarin-Bindungen für Google Play Services erforderlich.

Der folgende Codeausschnitt zeigt ein Beispiel für die Einstellungen, die hinzugefügt werden muss **"androidmanifest.xml"**:

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
    </application>
</manifest>
```

Zusätzlich zu die Berechtigungen anfordern **"androidmanifest.xml"**, eine app durchzuführenden auch Überprüfungen der Runtime-Berechtigung für die `ACCESS_COARSE_LOCATION` und `ACCESS_FINE_LOCATION` Berechtigungen. Finden Sie unter den [Xamarin.Android Berechtigungen](~/android/app-fundamentals/permissions.md) Handbuch für Weitere Informationen zum Durchführen von Überprüfungen der Runtime-Berechtigung.


### <a name="a-namecreate-emulator-with-google-api-create-an-emulator-with-google-apis"></a><a name="create-emulator-with-google-api" />Erstellen Sie einen Emulator mit Google-APIs

Falls bei der ein physisches Android-Gerät mit Google Play Services nicht installiert ist, ist es möglich, einen Emulator-Image für die Entwicklung zu erstellen. Weitere Informationen finden Sie unter den [-Geräte-Manager](~/android/get-started/installation/android-emulator/device-manager.md).


## <a name="the-googlemap-class"></a>Die GoogleMap-Klasse

Nachdem die Voraussetzungen erfüllt sind, ist es Zeit, mit der Entwicklung der Anwendungs beginnen, und verwenden die Android-API-Zuordnungen. Die [GoogleMap](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap) -Klasse ist die Haupt-API, die eine Xamarin.Android-Anwendung zum Anzeigen und interagieren mit einem Google Maps für Android verwenden. Diese Klasse verfügt über die folgenden Aufgaben:

-  Interagieren mit Google Play-Dienste zum Autorisieren der Anwendung mit dem Google-Webdienst.

-  Herunterladen, Zwischenspeichern und Anzeigen der Maps-Kacheln.

-  Anzeigen von UI-Steuerelemente wie z. B. Schwenken und Zoomen für den Benutzer.

-  Zeichnen Markierungen und geometrischen Formen auf Karten an.

Die `GoogleMap` wird eine Aktivität in einer von zwei Methoden hinzugefügt:

-  **MapFragment** : die [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) ist ein spezialisierter Fragment, das als Host fungiert die `GoogleMap` Objekt. Die `MapFragment` Android-API-Ebene 12 oder höher erforderlich ist.
   Ältere Versionen von Android können die [SupportMapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/SupportMapFragment).  Dieses Handbuch konzentriert sich auf die Verwendung der `MapFragment` Klasse.

-  **MapView** : die [MapView](https://developers.google.com/android/reference/com/google/android/gms/maps/MapView) ist eine spezielle Unterklasse von anzeigen, die als Host für fungieren kann eine `GoogleMap` Objekt. Benutzer dieser Klasse müssen die Lebenszyklusmethoden der Aktivität zum Weiterleiten der `MapView` Klasse.

Jeder dieser Container stellt eine `Map` -Eigenschaft, die eine Instanz der zurückgibt `GoogleMap`. Bevorzugte gegeben werden die [MapFragment](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment) Klasse, da es sich um eine einfachere API ist, die die Menge Standardcode reduziert, die ein Entwickler manuell implementieren müssen.

### <a name="adding-a-mapfragment-to-an-activity"></a>Eine Aktivität hinzugefügt eine MapFragment

Im folgende Screenshot ist ein Beispiel für eine einfache `MapFragment`:

[![Screenshot von einem Gerät ein Google-Map-Fragment anzeigen](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Ähnlich wie bei anderen fragmentklassen, stehen zwei Möglichkeiten zum Hinzufügen einer `MapFragment` auf eine Aktivität:

-   **Deklarativ** : die `MapFragment` können über die XML-Layout-Datei hinzugefügt werden, für die Aktivität. Der folgende XML-Ausschnitt zeigt ein Beispiel zur Verwendung der `fragment` Element:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **Programmgesteuertes** : die `MapFragment` instanziiert werden kann programmgesteuert mithilfe der [ `MapFragment.NewInstance` ](https://developers.google.com/android/reference/com/google/android/gms/maps/MapFragment.html#newInstance()) Methode, und klicken Sie dann auf eine Aktivität hinzugefügt. Dieser Codeausschnitt zeigt die einfachste Möglichkeit zum Instanziieren einer `MapFragment` -Objekt und fügen eine Aktivität hinzu:
    
    ```csharp
        var mapFrag = MapFragment.NewInstance();
        activity.FragmentManager.BeginTransaction()
                                .Add(Resource.Id.map_container, mapFrag, "map_fragment")
                                .Commit();

    ```

    Es ist möglich, konfigurieren Sie die `MapFragment` Objekt durch Übergabe einer [ `GoogleMapOptions` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) -Objekt `NewInstance`. Dies wird im Abschnitt erläutert [GoogleMap Eigenschaften](#googlemap_object) , das angezeigt wird später in diesem Handbuch.

Die `MapFragment.GetMapAsync` Methode dient zum Initialisieren der [ `GoogleMap` ](#googlemap_object) , die von dem Fragment gehostet wird, und erhalten einen Verweis auf das Map-Objekt, das von gehostet wird, die `MapFragment`. Diese Methode akzeptiert ein Objekt, implementiert die `IOnMapReadyCallback` Schnittstelle. 

Diese Schnittstelle verfügt über eine einzelne Methode, `IMapReadyCallback.OnMapReady(MapFragment map)` wird, die aufgerufen werden, wenn es möglich, dass die app für die Interaktion mit ist der `GoogleMap` Objekt. Der folgende Codeausschnitt zeigt, wie eine Android-Aktivität initialisieren kann eine `MapFragment` und Implementieren der `IOnMapReadyCallback` Schnittstelle:
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

### <a name="map-types"></a>Zuordnen von Typen

Es gibt fünf verschiedene Typen von Karten über die Google Maps-API verfügbar:

-  **Normale** – Dies ist der Standardtyp für die Zuordnung. Es zeigt Straßen und wichtige natürliche Funktionen zusammen mit einigen künstlichen Punkte von Interesse sind (z. B. Gebäuden und Bridges).

-  **Satelliten** -diese Code Map zeigt Satelliten Fotografie.

-  **Hybride** – diese Code Map zeigt Satelliten Fotografie und Road zugeordnet.

-  **Terrain** – Dies wird in erster Linie Topografie Features mit Nutzung einiger Straßen eingestuft.

-  **Keine** -diese Zuordnung wird keine Kacheln geladen, wird es als ein leeres Raster gerendert.


Die folgende Abbildung zeigt drei der verschiedenen Typen von Karten, die von links nach rechts (normal, Hybrid, Gelände):

[![Drei zuordnen Beispielscreenshots: Normal oder als Hybrid- bzw. Gelände](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Die `GoogleMap.MapType` Eigenschaft wird verwendet, um festzulegen oder zu ändern, welcher Typ von Karte angezeigt wird. Der folgende Codeausschnitt zeigt, wie Sie eine Satelliten-Zuordnung anzuzeigen.

```csharp
public void OnMapReady(GoogleMap map)
{
    map.MapType = GoogleMap.MapTypeHybrid;
}
```


### <a name="a-namegooglemapobject-googlemap-properties"></a><a name="googlemap_object" />GoogleMap-Eigenschaften

`GoogleMap` definiert mehrere Eigenschaften, die die Funktionalität und die Darstellung der Zuordnung steuern können. Eine Möglichkeit zum Konfigurieren des ursprünglichen Zustands der ein `GoogleMap` übergeben eine [GoogleMapOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMapOptions) Objekt beim Erstellen einer `MapFragment`. Der folgende Codeausschnitt ist ein Beispiel der Verwendung einer `GoogleMapOptions` Objekt beim Erstellen einer `MapFragment`:

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

Die andere Möglichkeit zum Konfigurieren einer `GoogleMap` wird durch das Bearbeiten von Eigenschaften auf der [UiSettings](https://developers.google.com/android/reference/com/google/android/gms/maps/UiSettings) von das Map-Objekt. Das nächste Codebeispiel zeigt, wie so konfigurieren Sie eine `GoogleMap` die Zoom-Steuerelemente und einen Kompass angezeigt:

```csharp
public void OnMapReady(GoogleMap map)
{
    map.UiSettings.ZoomControlsEnabled = true;
    map.UiSettings.CompassEnabled = true;
}
```

## <a name="interacting-with-the-googlemap"></a>Interaktion mit der GoogleMap

Die Android Maps-API enthält APIs, mit denen eine Aktivität zum Ändern des Standpunkts, Marker hinzufügen, platzieren der benutzerdefinierte Überlagerungen oder geometrische Formen zu zeichnen. Dieser Abschnitt wird erläutert, wie einige dieser Aufgaben in Xamarin.Android umgesetzt werden können.

### <a name="changing-the-viewpoint"></a>Ändern den Standpunkt

Zuordnungen sind auf dem Bildschirm, basierend auf die Mercator-Projektion als eine flache Ebene modelliert. In der kartensicht wird mit der eine *Kamera* direkt nach unten auf dieser Ebene suchen. Die Position der Kamera kann durch Ändern der Position, Zoom, Neigung und Einfluss gesteuert werden. Die [CameraUpdate](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdate) Klasse wird verwendet, um die Position der Kamera zu verschieben. `CameraUpdate` Objekte werden nicht direkt instanziiert werden, stattdessen die Maps-API bietet die [CameraUpdateFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/CameraUpdateFactory) Klasse.

Nach einem `CameraUpdate` Objekt erstellt wurde, die sie als Parameter übergeben wird, entweder die [GoogleMap.MoveCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#moveCamera(com.google.android.gms.maps.CameraUpdate)) oder [GoogleMap.AnimateCamera](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap#animateCamera(com.google.android.gms.maps.CameraUpdate)) Methoden. Die `MoveCamera` Methode aktualisiert die Zuordnung sofort bei der `AnimateCamera` Methode bietet einen reibungslosen, animierten Übergang.

Dieser Codeausschnitt ist ein einfaches Beispiel zur Verwendung der `CameraUpdateFactory` zum Erstellen einer `CameraUpdate` erhöht, die die Zoomstufe der Karte, indem Sie eine Zoomstufe:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...

public void OnMapReady(GoogleMap map)
{   
    map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Die Maps-API bietet eine [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) die alle möglichen Werte für die Kameraposition aggregiert. Eine Instanz dieser Klasse bereitgestellt werden kann die [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) Methode der zurückgegeben wird, wird eine `CameraUpdate` Objekt. Die Maps-API enthält auch die [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) Adapterklasse, die eine fluent-API zum Erstellen von `CameraPosition` Objekte.
Der folgende Codeausschnitt zeigt ein Beispiel zum Erstellen einer `CameraUpdate` aus einer `CameraPosition` und melde mich damit ändern Sie die Position (Kamera) auf eine `GoogleMap`:

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

Im vorherigen Codeausschnitt wird durch ein bestimmten Speicherort auf der Karte dargestellt die [LatLng](https://developers.google.com/android/reference/com/google/android/gms/maps/model/LatLng) Klasse. Die Zoomstufe wird auf 18 festgelegt, d.h. ein willkürliches Measure für den Zoomfaktor, die von Google Maps verwendet. Der Einfluss ist die Compass Messung im Uhrzeigersinn von Norden. Die Neigung-Eigenschaft steuert den Betrachtungswinkel und gibt einen Winkel von 25 Grad von der vertikalen. Der folgende Screenshot zeigt die `GoogleMap` nach dem Ausführen des vorangehenden Codes:

[![Beispiel für Google-Karte, die mit einer angegebenen Position mit einem Geneigter Betrachtungswinkel](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>Zeichnen auf der Karte

Die Android Maps-API bietet APIs zum Zeichnen der folgenden Elemente auf einer Karte:

-  **Marker** -Hierbei handelt es sich um spezielle Symbole, die verwendet werden, um einen einzelnen Speicherort auf einer Karte zu identifizieren.

-  **Überlagerungen** – Dies ist ein Bild, das verwendet werden kann, um eine Auflistung von Standorten oder auf der Karte zu identifizieren.

-  **Linien, Polygone und Kreise** – Dies sind APIs, die Aktivitäten zum Hinzufügen von Formen zu einer Zuordnung zu ermöglichen.


#### <a name="markers"></a>Marker

Die Maps-API bietet eine [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) Klasse, die alle Daten zu einer zentralen Stelle auf einer Zuordnung kapselt. Standardmäßig ist die Markierung verwendet die Klasse ein Standardsymbol, das von Google Maps bereitgestellt. Es ist möglich, um die Darstellung eines Markers anpassen und von Benutzern reagieren.


##### <a name="adding-a-marker"></a>Einen Marker hinzufügen

Um eine Markierung zu einer Zuordnung hinzuzufügen, ist es notwendig erstellen Sie ein neues [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) Objekt und rufen Sie dann die [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) Methode für eine `GoogleMap` Instanz. Diese Methode gibt eine [Marker](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker) Objekt.

```csharp
public void OnMapReady(GoogleMap map)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    
    map.AddMarker(markerOpt1);
}
```

Der Titel des Markers erscheint in einem *Fenster "Info"* Wenn der Benutzer tippt in der Markierung. Der folgende Screenshot zeigt, wie dieser Marker aussieht:

[![Beispiel für Google-Zuordnung mit einem Marker und ein Informationsfenster "für die Vimy Ridge](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>Einen Marker anpassen

Es ist möglich, Anpassen des Symbols, der vom Marker verwendet wird, durch den Aufruf der `MarkerOptions.InvokeIcon` Methode, wenn die Markierung zur Karte hinzufügen.
Diese Methode akzeptiert eine [BitmapDescriptor](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptor) -Objekt, das die Daten, die zum Rendern des Symbols enthält. Die [BitmapDescriptorFactory](https://developers.google.com/android/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory) Klasse enthält einige Hilfsmethoden zur Vereinfachung der Erstellung einer `BitmapDescriptor`. Die folgende Liste führt einige der folgenden Methoden:

-   `DefaultMarker(float colour)` &ndash; Verwenden Sie die Standard-Google Maps-Marker, aber ändern Sie die Farbe.

-   `FromAsset(string assetName)` &ndash; Verwenden Sie ein benutzerdefiniertes Symbol aus der angegebenen Datei im Ordner "Assets".

-   `FromBitmap(Bitmap image)` &ndash; Verwenden Sie die angegebene Bitmap als Symbol.

-   `FromFile(string fileName)` &ndash; Erstellen Sie das benutzerdefinierte Symbol aus der Datei unter dem angegebenen Pfad.

-   `FromResource(int resourceId)` &ndash; Erstellen Sie ein benutzerdefiniertes Symbol aus der angegebenen Ressource an.

Der folgende Codeausschnitt zeigt ein Beispiel für einen Marker Zyan Farbiger Standard erstellen:

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

#### <a name="info-windows"></a>Info-windows

*Info Windows* sind spezielle Windows dieses Popup auf Informationen, die dem Benutzer angezeigt wird, wenn sie eine bestimmte Markierung tippen. Standardmäßig wird das Fenster "Info" den Inhalt des Titels für den Marker angezeigt. Wenn der Titel nicht zugewiesen wurde, wird keine Fenster "Info" angezeigt. Nur ein Infofenster kann zu einem Zeitpunkt angezeigt werden.

Es ist möglich, passen das Fenster "Info" durch die Implementierung der [GoogleMap.IInfoWindowAdapter](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) Schnittstelle. Es gibt zwei wichtige Methoden für diese Schnittstelle:

-  `public View GetInfoWindow(Marker marker)` &ndash; Diese Methode wird aufgerufen, um ein Informationsfenster für die benutzerdefinierte für eine Markierung zu erhalten. Wenn sie zurückgibt `null` , und klicken Sie dann die Standardanzeige für Fenster verwendet wird. Wenn diese Methode eine Ansicht zurückgibt, wird diese Ansicht innerhalb der Info-Fensterrahmen platziert werden.

-  `public View GetInfoContents(Marker marker)` &ndash; Diese Methode wird nur aufgerufen werden, wenn GetInfoWindow zurückgibt `null` . Diese Methode kann Zurückgeben einer `null` Wert, wenn die standardmäßige Umsetzung von den Informationen im Fensterinhalt verwendet werden. Andernfalls gibt diese Methode eine Ansicht mit dem Inhalt des Fensters Informationen zurück.

Ein Fenster "Info" ist nicht mit einer live-Ansicht – stattdessen Android konvertieren Sie die Ansicht, um statische Bitmaps und anzuzeigen, die auf das Bild wird. Dies bedeutet, dass ein Fenster "Info" nicht auf das touchereignisse oder Gesten, noch wird es automatisch aktualisiert. Um ein Fenster "Info" zu aktualisieren, ist es notwendig, die [GoogleMap.ShowInfoWindow](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) Methode.

Die folgende Abbildung zeigt einige Beispiele für einige benutzerdefinierte Informationsfenster. Das Bild auf der linken Seite verfügt über ihre Inhalte angepasst werden, während das Image auf der rechten Seite das Fenster besitzt und die Inhalte, die mit abgerundeten Ecken angepasst:

![Beispiel für Marker Windows für Melbourne, einschließlich Symbol und Auffüllung. Im Rechte Fenster hat eine abgerundete Ecken.](maps-api-images/marker-infowindows.png)

#### <a name="groundoverlays"></a>GroundOverlays

Im Gegensatz zu den Marker ein, die einen bestimmten Speicherort auf einer Karte zu identifizieren, eine [GroundOverlay](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlay) ist ein Bild, das verwendet wird, um eine Auflistung von Standorten oder auf einen Bereich auf der Karte zu identifizieren.

##### <a name="adding-a-groundoverlay"></a>Hinzufügen einer GroundOverlay

Hinzufügen einer Überlagerung von Grund auf neu zu einer Karte ist vergleichbar mit dem eine Markierung zu einer Karte hinzufügen. Zunächst wird eine [GroundOverlayOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/GroundOverlayOptions) Objekt erstellt wird. Dieses Objekt wird dann als Parameter an übergeben die [ `GoogleMap.AddGroundOverlay` ](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addGroundOverlay(com.google.android.gms.maps.model.GroundOverlayOptions)) -Methode, die zurückgegeben wird, wird eine `GroundOverlay` Objekt. Dieser Codeausschnitt ist ein Beispiel für eine Überlagerung von Grund auf neu zu einer Karte hinzufügen:

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = googleMap.AddGroundOverlay(groundOverlayOptions);
```

Der folgende Screenshot zeigt dieses Overlay auf einer Karte:

[![Beispielzuordnung mit einem Bild überlagert, der ein Polardiagramm Beachten](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>Linien, Kreise und Polygone

Es gibt drei einfache Typen von geometrischen Abbildungen, die zu einer Zuordnung hinzugefügt werden können:

-  **Polylinie** – Dies ist eine Reihe verbundener Liniensegmente. Sie können einen Pfad auf einer Karte zu markieren, oder erstellen eine geometrische Form.

-  **Kreis** – Dies wird einen Kreis gezeichnet, auf der Karte.

-  **Polygon** – Dies ist eine geschlossene Form zum Markieren von Bereichen auf einer Karte.


##### <a name="polylines"></a>Polylinien

Ein [Polylinie](https://developers.google.com/android/reference/com/google/android/gms/maps/model/Polyline) ist eine Liste von aufeinander folgenden `LatLng` Objekte, die den Vertices für jedes Liniensegment angeben. Eine Polylinie wird erstellt, indem Sie zunächst eine `PolylineOptions` -Objekt und die Punkte hinzugefügt wird. Die `PolylineOption` Objekt wird dann zum Übergeben einer `GoogleMap` -Objekt durch Aufrufen der `AddPolyline` Methode.

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

Kreise werden erstellt, durch die ersten Instanziierung einer [CircleOption](https://developers.google.com/android/reference/com/google/android/gms/maps/model/CircleOptions) Objekt, das dem Mittelpunkt und den Radius des Kreises in Metern angeben. Der Kreis auf der Karte gezeichnet wird, durch den Aufruf [GoogleMap.AddCircle](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addCircle(com.google.android.gms.maps.model.CircleOptions)).
Der folgende Codeausschnitt zeigt, wie zum Zeichnen eines Kreises wird:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);

googleMap.AddCircle (circleOptions);
```


##### <a name="polygons"></a>Polygone

`Polygon`s sind ähnlich `Polyline`s, beendet wurde, jedoch nicht geöffnet werden. `Polygon`s sind eine geschlossene Schleife und deren innere ausgefüllt haben.
`Polygon`s werden erstellt, in genau dieselbe Weise wie eine `Polyline`, mit Ausnahme der [GoogleMap.AddPolygon](https://developers.google.com/android/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) aufgerufene Methode.

Im Gegensatz zu einem `Polyline`, `Polygon` selbstschließend ist. Das Polygon wird geschlossen deaktiviert die `AddPolygon` Methode durch Zeichnen einer Linie, der den ersten und letzten Punkt eine Verbindung herstellt. Der folgende Codeausschnitt erstellt ein solid Rechteck über der gleiche Bereich wie der vorherige Codeausschnitt in die `Polyline` Beispiel.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon

googleMap.AddPolygon(rectOptions);
```


## <a name="responding-to-user-events"></a>Reaktion auf Benutzerereignisse

Es gibt drei Arten von Aktivitäten, die ein Benutzer mit einer Karte haben kann:

-  **Marker auf** – der Benutzer klickt auf einen Marker.

-  **Ziehen Sie die Markierung** -Long-Wert-geklickt wurde auf einen Mparger

-  **Details-Fenster klicken** – der Benutzer auf ein Fenster "Info" geklickt hat.

Jedes dieser Ereignisse werden im folgenden ausführlicher erläutert.


### <a name="marker-click-events"></a>Marker click-Ereignisse

Die `MarkerClicked` Ereignis wird ausgelöst, wenn der Benutzer in einer Markierung tippt. Dieses Ereignis akzeptiert eine `GoogleMap.MarkerClickEventArgs` -Objekt als Parameter. Diese Klasse enthält zwei Eigenschaften:

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; Diese Eigenschaft muss festgelegt werden, um `true` um anzugeben, dass der Ereignishandler das Ereignis verarbeitet wurde. Wenn dies, um festgelegt ist `false` dann das Standardverhalten, zusätzlich zu das benutzerdefinierte Verhalten des ereignishandlers ausgeführt wird.

-  `Marker` &ndash; Diese Eigenschaft ist ein Verweis auf den Marker, der ausgelöst hat die `MarkerClick` Ereignis.


Dieser Codeausschnitt zeigt ein Beispiel für eine `MarkerClick` ändert, die an einem neuen Speicherort auf der Karte die Position (Kamera):

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


### <a name="marker-drag-events"></a>Ziehen Sie markerereignisse

Dieses Ereignis wird ausgelöst, wenn der Benutzer wünscht, dass den Marker zu ziehen. Marker sind standardmäßig nicht ziehbare. Ein Marker kann festgelegt werden wie ziehbare durch Festlegen der `Marker.Draggable` Eigenschaft `true` oder durch Aufrufen der `MarkerOptions.Draggable` -Methode mit `true` als Parameter.

Um die Markierung zu ziehen, der Benutzer muss zuerst lang, klicken Sie auf den Marker ein, und klicken Sie dann der Finger auf der Karte bleiben muss. Wenn dem Finger des Benutzers auf dem Bildschirm um gezogen wird, wechselt die Markierung. Wenn dem Finger des Benutzers außerhalb des Bildschirms abhebt, wird der Marker beibehalten.

Die folgende Liste beschreibt die verschiedenen Ereignisse, die für eine Markierung ziehbare ausgelöst wird:

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; Dieses Ereignis wird ausgelöst, wenn der Benutzer zuerst die Markierung zieht.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; Dieses Ereignis wird ausgelöst, wie der Marker gezogen wird.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; Dieses Ereignis wird ausgelöst, wenn der Benutzer abgeschlossen ist den Marker zu ziehen.

Jede der `EventArgs` enthält eine einzelne Eigenschaft namens `P0` , einen Verweis auf die `Marker` Objekt, das gezogen wird.


### <a name="info-window-click-events"></a>Info-Fensters klicken Sie auf Ereignisse

Nur ein Fenster "Info" kann zu einem Zeitpunkt angezeigt werden. Klickt der Benutzer auf ein Fenster "Info" in einer Zuordnung auf, kann das Map-Objekt löst ein `InfoWindowClick` Ereignis. Der folgende Codeausschnitt veranschaulicht, um einen Handler auf das Ereignis zu verknüpfen:

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

Denken Sie daran, dass ein Fenster "Info" eine statische ist `View` die wird als ein Bild auf die Karte gerendert. Keine Widgets, wie z. B. Schaltflächen, Kontrollkästchen oder Textansichten, die in das Fenster "Info" platziert werden werden statische und können nicht auf ihre ganzzahligen Benutzerereignisse zu reagieren.


## <a name="related-links"></a>Verwandte Links

- [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo)
- [Google Play-Dienste](https://developers.google.com/android/guides/overview)
- [Google ordnet der Android-API v2](https://developers.google.com/maps/documentation/android-sdk/intro)
- [Google Play APK-Dienste](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Einen Google Maps-API-Schlüssel abrufen](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
- [Bibliothek verwendet](https://developer.android.com/guide/topics/manifest/uses-library-element)
- [verwendet-Funktion](https://developer.android.com/guide/topics/manifest/uses-feature-element)
