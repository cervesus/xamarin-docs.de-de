---
title: Verwenden die Google Maps-API in der Anwendung
description: Wie Google Maps-API v2-Funktionen in Ihrer Anwendung Xamarin.Android implementiert.
ms.prod: xamarin
ms.assetid: C0589878-2D04-180E-A5B9-BB41D5AF6E02
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 06/25/2018
ms.openlocfilehash: a0e010a8300eb4b4452737e34d2f55a35ab95428
ms.sourcegitcommit: 26033c087f49873243751deded8037d2da701655
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/25/2018
ms.locfileid: "36935138"
---
# <a name="using-the-google-maps-api-in-your-application"></a>Die Google Maps-API verwenden in der Anwendung.

Mithilfe der Maps-Anwendung eignet sich hervorragend, aber in einigen Fällen sollen Maps direkt in der Anwendung. Zusätzlich zu den integrierten ordnet Anwendung Google bietet außerdem eine [native Zuordnungs-API für Android](https://developers.google.com/maps/documentation/android/).
Die Maps-API eignet sich für Fälle, in denen steuerungsmöglichkeiten für die Zuordnung beibehalten werden sollen. Aufgaben, die mit der Maps-API möglich sind enthalten:

-  Ändern programmgesteuert Standpunkt der Zuordnung.
-  Hinzufügen und Anpassen von Marker.
-  Hinzufügen einer Anmerkung zu einer Karte mit überlagert.

Im Gegensatz zu der nun veraltete Google Maps Android-API v1 Google Maps Android-API v2 ist Bestandteil des [Google Play-Dienste](http://developer.android.com/google/play-services/index.html).
Aus diesem Grund ist es erforderlich, einige obligatorischen Voraussetzungen erfüllt sein, damit es möglich ist, verwenden Sie die Google Maps Android-API in einer Anwendung Xamarin.Android.


## <a name="google-maps-api-prerequisites"></a>Google ordnet API-Komponenten

Mehrere Elemente müssen konfiguriert werden, bevor Sie die Maps-API verwenden können einschließlich:

-  Die Google Play-Dienste-SDK installieren
-  Erstellen Sie einen Emulator mit Google-APIs
-  Besorgen Sie sich einen Maps-API-Schlüssel
-  Geben Sie die erforderlichen Berechtigungen



### <a name="install-the-google-play-services-sdk"></a>Die Google Play-Dienste-SDK installieren

Google Play-Dienste ist eine Technologie von Google, mit dem Android-Anwendungen, z. B. Google +, -App-Abrechnung und Zuordnungen verschiedene Google-Funktionen nutzen kann. Diese Funktionen auf Android-Geräten zugegriffen werden, als Hintergrunddienste, die in enthaltenen der [Google wiedergeben Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en).

Android-Anwendungen interagieren mit Google Play-Dienste über die Google Play-Dienste-Client-Bibliothek. Diese Bibliothek enthält die Schnittstellen und Klassen für die einzelnen Dienste, z. B. Zuordnungen. Das folgende Diagramm zeigt die Beziehung zwischen einem Android-Anwendung und Google Play-Dienste:

![Diagramm zur Veranschaulichung den Google Play Store, Google wiedergeben Services-APK aktualisieren](maps-api-images/play-services-diagram.png)

Die Android Maps-API wird als Teil von Google Play-Dienste bereitgestellt.
Bevor eine Anwendung Xamarin.Android die Maps-API verwenden kann, muss das Google wiedergeben Services SDK installiert und gebunden werden. Das Google wiedergeben Services SDK ist über den Android SDK Manager installiert. Der folgende Screenshot zeigt Where in dem Android SDK Manager der Google Play-Dienstclient gefunden werden kann:

![Google Play-Dienste unter Extras im Android SDK Manager angezeigt wird](maps-api-images/image01.png)

> [!NOTE]
> Die Google Play-Dienste, APK ist ein lizenziertes Produkt, das möglicherweise nicht auf allen Geräten vorhanden. Wenn es nicht installiert ist, funktioniert Google Maps nicht auf dem Gerät.


#### <a name="binding-google-play-services"></a>Bindung Google Play-Dienste

Sobald die Clientbibliothek Google Play-Dienste installiert ist, muss es von einer Bindung Xamarin.Android Java-Bibliothek gebunden werden. Es gibt zwei Möglichkeiten, dies zu erreichen:

-  **Das Google wiedergeben Services Maps NuGet-Paket** -Dies ist der einfachste Ansatz (nur in Xamarin.Android 4.8 oder höher verfügbar).
   Installieren der [Xamarin Google wiedergeben Services Maps NuGet](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Maps); Hierbei werden auch alle abhängigkeitspakete von Google Play-Dienste installiert.
   Der Rest dieses Handbuchs konzentriert sich auf diesen Ansatz.

-  **Die Google Play-Dienste-Clientbibliothek manuell binden** -Dies ist von einem komplexeren Ansatz und ist die einzige Möglichkeit für Xamarin.Android 4.4 oder Xamarin.Android 4.6 Google wiedergeben Services SDK zu binden.
   Binden manuell von der Clientbibliothek Google Play-Dienste ist nicht Gegenstand dieses Dokuments, aber ein Beispiel dazu finden Sie der [Karten und Speicherort Demo v3-Beispiel](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3) auf Github.


#### <a name="adding-the-google-play-services-map-package"></a>Hinzufügen von dem Google Play Services Map-Paket

Um das Paket Google wiedergeben Services Zuordnung hinzufügen, mit der rechten Maustaste klicken Sie auf die **Verweise** -Ordner des Projekts im Projektmappen-Explorer, und auf **NuGet-Pakete verwalten...** :

![Projektmappen-Explorer zeigt NuGet-Pakete verwalten-Kontextmenüelement unter Verweise](maps-api-images/image02.png)

Daraufhin wird die **NuGet Package Manager**. Klicken Sie auf **Durchsuchen** , und geben Sie **Xamarin Google wiedergeben Services Maps** in das Suchfeld ein. Wählen Sie **Xamarin.GooglePlayServices.Maps** , und klicken Sie auf **installieren**. (Wenn dieses Paket bereits installiert wurde, klicken Sie auf **Update**.):

[![NuGet-Paket-Manager mit der ausgewählten Xamarin.GooglePlayServices.Maps-Paket](maps-api-images/image03-sml.png)](maps-api-images/image03.png#lightbox)

Beachten Sie, dass die folgenden abhängigkeitspakete installiert werden:

-   **Xamarin.GooglePlayServices.Base**
-   **Xamarin.GooglePlayServices.Basement**
-   **Xamarin.GooglePlayServices.Tasks**



### <a name="create-an-emulator-with-google-apis"></a>Erstellen Sie einen Emulator mit Google-APIs

Obwohl dies nicht empfohlen wird, ist es möglich, den Emulator aus, um die Android Maps-API unterstützen einrichten. Der Emulator konfiguriert werden, damit der Google-API-Ebene 17 (Android 4.2.2) als Ziel oder höher. Im folgenden Screenshot ist eine Emulator-Image für API-Ebene 19 konfiguriert: 

![Android Emulator Manager mit einer AVD für API-Ebene 19 konfiguriert](maps-api-images/image04.png)



### <a name="obtain-a-google-maps-api-key"></a>Abrufen einer Google Maps-API-Schlüssel

Der letzte Schritt ist zum Abrufen eines Google Maps-API-Schlüssels (Beachten Sie, dass Sie einen API-Schlüssel von älteren Google Maps v1 wiederverwenden können nicht). Informationen zum Verwenden des API-Schlüssels mit Xamarin.Android zu erhalten, finden Sie unter [ein Google Maps-API-Schlüssel erhalten](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).
 


### <a name="specify-the-required-permissions"></a>Geben Sie die erforderlichen Berechtigungen

Die folgenden Berechtigungen müssen angegeben werden, der **AndroidManifest.XML** für die Google Maps Android-API:

-  **Der Zugriff auf das Netzwerk Status** &ndash; die Maps-API muss in der Lage, überprüfen Sie, ob die Maps-Kacheln heruntergeladen werden kann.

-  **Zugriff auf das Internet** &ndash; Zugang zum Internet wird zum Herunterladen der Maps-Kacheln und kommunizieren mit Google Wiedergeben von Servern für API-Zugriff erforderlich sind.

-  **OpenGL ES-v2** &ndash; die Anwendung muss die Notwendigkeit von OpenGL ES-v2 deklarieren.

-  **Google Maps-API-Schlüssel** &ndash; der API-Schlüssel wird verwendet, um zu bestätigen, dass die Anwendung registriert und autorisiert, Google Play-Dienste zu verwenden. Finden Sie unter [erhalten eine Google Maps-API-Schlüssel](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md) ausführliche Informationen über diesen Schlüssel.

-  **In einem Externspeicher schreiben** &ndash; den Google Maps Android-API-werden heruntergeladene Kacheln in einem externen Speicher zwischengespeichert.

-  **Zugriff auf die Google-Web-basierte Dienste** &ndash; Zugriffsberechtigungen für Google-Webdienste, die die Android Maps-API Sichern in der Anwendung benötigt.

-  **Berechtigungen für Google wiedergeben Services Benachrichtigungen** &ndash; die Anwendung muss die Berechtigung zum Empfangen von Benachrichtigungen remote aus Google Play-Dienste erteilt werden.

-  **Zugriff auf den Speicherort Anbieter** &ndash; Dies sind optionale Berechtigungen.
   Sie können die `GoogleMap` Klasse, um den Speicherort des Geräts auf der Karte angezeigt.


Der folgende Codeausschnitt zeigt ein Beispiel für die Einstellungen, die hinzugefügt werden müssen **AndroidManifest.XML**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionName="4.5" package="com.xamarin.docs.android.mapsandlocationdemo2" android:versionCode="6">
    <uses-sdk android:minSdkVersion="14" android:targetSdkVersion="17" />

    <!-- Google Maps for Android v2 requires OpenGL ES v2 -->
    <uses-feature android:glEsVersion="0x00020000" android:required="true" />

    <!-- We need to be able to download map tiles and access Google Play Services-->
    <uses-permission android:name="android.permission.INTERNET" />

    <!-- Allow the application to access Google web-based services. -->
    <uses-permission android:name="com.google.android.providers.gsf.permission.READ_GSERVICES" />

    <!-- Google Maps for Android v2 will cache map tiles on external storage -->
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

    <!-- Google Maps for Android v2 needs this permission so that it may check the connection state as it must download data -->
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />

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


## <a name="the-googlemap-class"></a>Die GoogleMap-Klasse

Nachdem die erforderlichen Komponenten durchgeführt werden, ist es Zeit, starten Sie die Anwendung entwickeln und Verwenden von Android Maps-API. Die [GoogleMap](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap) Klasse ist die Haupt-API, die eine Anwendung Xamarin.Android zum Anzeigen und interagieren mit einem Google Maps für Android verwenden. Diese Klasse verfügt über die folgenden Verantwortungen:

-  Interaktion mit Google Play-Dienste zum Autorisieren der Anwendung mit dem Google-Webdienst.

-  Herunterladen, das Zwischenspeichern und Anzeigen von Maps-Kacheln.

-  Anzeigen von Benutzeroberflächen-Steuerelemente wie z. B. Schwenken und Zoomen für den Benutzer.

-  Zeichnen Marker und geometrische Formen auf Karten an.

Die `GoogleMap` einer Aktivität auf zwei Arten hinzugefügt wird:

-  **MapFragment** – der [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) ist eine spezielle Fragment, das als Host fungiert der `GoogleMap` Objekt. Die `MapFragment` Android-API-Ebene, 12 oder höher erforderlich ist.
   Ältere Versionen von Android können die [SupportMapFragment](http://developer.android.com/reference/com/google/android/gms/maps/SupportMapFragment.html).

-  **MapView** – der [MapView](https://developer.xamarin.com/api/type/Android.GoogleMaps.MapView/) ist eine spezielle Sicht-Unterklasse, die als Host für fungieren kann ein `GoogleMap` Objekt. Benutzer dieser Klasse müssen der Lebenszyklusmethoden Aktivität zum Weiterleiten der `MapView` Klasse.

Jeder dieser Container verfügbar machen eine `Map` -Eigenschaft, die eine Instanz zurückgegeben `GoogleMap`. Bevorzugt sollte zugewiesen werden, um die [MapFragment](http://developer.android.com/reference/com/google/android/gms/maps/MapFragment.html) -Klasse zu verwenden, wie es sich um eine einfachere API handelt, die den Standardcode Menge reduziert, die ein Entwickler manuell implementieren müssen.


### <a name="adding-a-mapfragment-to-an-activity"></a>Hinzufügen einer MapFragment zu einer Aktivität

Der folgende Screenshot ist ein Beispiel für eine sehr einfache `MapFragment`:

[![Screenshot eines Geräts ein Fragment Karte anzeigen](maps-api-images/image05-sml.png)](maps-api-images/image05.png#lightbox)

Ähnlich wie bei anderen Klassen Fragment, werden auf zweierlei Weise hinzugefügt dies `MapFragment` an eine Aktivität:

-   **Deklarativ** : die `MapFragment` für die Aktivität über die XML-Layout-Datei hinzugefügt werden können. Der folgende XML-Ausschnitt zeigt ein Beispiel zum Verwenden der `fragment` Element:

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <fragment xmlns:android="http://schemas.android.com/apk/res/android"
              android:id="@+id/map"
              android:layout_width="match_parent"
              android:layout_height="match_parent"
              class="com.google.android.gms.maps.MapFragment" />
    ```

-   **Programmgesteuertes** : die `MapFragment` wie nachfolgend beschrieben, programmgesteuert hinzugefügt werden.

Programmgesteuert hinzufügen einer `MapFragment`, muss Ihre Aktivität implementiert die `IOnMapReadyCallback` Schnittstelle. Da die Initialisierung einer `GoogleMap` Objekt dauert einige Zeit in Anspruch wie Google Play API kommuniziert, müssen Sie einen Rückruf, der app bereitstellen bei der `GoogleMap` bereit ist.

Fügen Sie zunächst `IOnMapReadyCallback` auf die `Activity` -Klassendeklaration.
Zum Beispiel:

```csharp
public class MapWithMarkersActivity : Activity, IOnMapReadyCallback
```

Im nächsten Schritt in der `OnCreate` -Methode, Hinzufügen der `MapFragment` wie im folgenden Codebeispiel gezeigt (die `GoogleMapOptions` Klasse wird weiter unten in diesem Handbuch erläutert):

```csharp
_mapFragment = FragmentManager.FindFragmentByTag("map") as MapFragment;
if (_mapFragment == null)
{
    GoogleMapOptions mapOptions = new GoogleMapOptions()
        .InvokeMapType(GoogleMap.MapTypeSatellite)
        .InvokeZoomControlsEnabled(false)
        .InvokeCompassEnabled(true);

    FragmentTransaction fragTx = FragmentManager.BeginTransaction();
    _mapFragment = MapFragment.NewInstance(mapOptions);
    fragTx.Add(Resource.Id.map, _mapFragment, "map");
    fragTx.Commit();
}
_mapFragment.GetMapAsync(this);
```

Ein `GoogleMap` muss abgerufen werden, mithilfe von `GetMapAsync`, wie am Ende der im vorangehenden Codebeispiel gezeigt &ndash; der Maps-System und die Ansicht automatisch initialisiert. (Beachten Sie, die diese Methode nicht verwendet `await` / `async` Semantik &ndash; die `Async` Verhalten von Android implementiert wird.) Wenn die `GoogleMap` Objekt ist bereit, Android ruft Ihre app `OnMapReady` Methode (die Sie implementieren müssen, als Teil der `IOnMapReadyCallback` Schnittstelle). Zum Beispiel:

```csharp
public void OnMapReady (GoogleMap map)
{
    _map = map;
}
```

Im obigen Beispiel die `OnMapReady` Rückruf initialisiert die `_map` auf die erstellte Variable `GoogleMap` Objekt.

Als Beispiel zur Verwendung dieses Ergebnis beim `OnResume` wird aufgerufen, er kann überprüfen `_map` ungleich Null ist. Wenn `_map` festgelegt ist, um eine `GoogleMap` -Objekt, `OnResume` kann darauf hinzufügen Marker und verschieben ihre Kamera, um eine angegebene Längen- und Breitengrad Methoden aufrufen. Ein vollständiges Codebeispiel finden Sie unter [SimpleMapDemo](https://github.com/xamarin/monodroid-samples/tree/master/MapsAndLocationDemo_v3/SimpleMapDemo).



### <a name="map-types"></a>Kartentypen

Es gibt fünf verschiedene Typen von Karten aus dem Google Maps-API verfügbar:

-  **Normal** -Dies ist der Standardtyp für die Karte. Es zeigt Straßen und wichtige natürliche Funktionen zusammen mit einigen menschliches interessenschwerpunkte (z. B. Gebäude und Brücken).

-  **Satellit** -diese Übersicht zeigt Satellit Fotos.

-  **Hybride** – diese Übersicht zeigt Satellit Fotos und Road zugeordnet.

-  **Terrain** -zeigt dies hauptsächlich Topografie Funktionen mit einigen Straßen.

-  **Keine** -diese Zuordnung wird keine Kacheln geladen, wird es als ein leeres Raster gerendert.


Die folgende Abbildung zeigt drei der verschiedenen Typen von Zuordnungen, von links nach rechts (normal, Hybrid, Terrain):

[![Ordnen Sie drei Beispiel Screenshots: Normal, Hybrid und Terrain](maps-api-images/map-types-sml.png)](maps-api-images/map-types.png#lightbox)

Die `GoogleMap.MapType` Eigenschaft wird verwendet, um festzulegen oder zu ändern, welcher Typ von Karte angezeigt wird. Der folgende Codeausschnitt zeigt, wie eine Satelliten-Zuordnung angezeigt wird.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MapType = GoogleMap.MapTypeSatellite;
}
```


### <a name="googlemap-properties"></a>GoogleMap-Eigenschaften

`GoogleMap` definiert verschiedene Eigenschaften, die die Funktionalität und die Darstellung der Karte kontrollieren können. Eine Möglichkeit zum Konfigurieren des Anfangszustand eine `GoogleMap` ist die Übergabe einer [GoogleMapOptions](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMapOptions.html) Objekt beim Erstellen einer `MapFragment`. Der folgende Codeausschnitt ist ein Beispiel der Verwendung einer `GoogleMapOptions` Objekt beim Erstellen einer `MapFragment`:

```csharp
GoogleMapOptions mapOptions = new GoogleMapOptions()
    .InvokeMapType(GoogleMap.MapTypeSatellite)
    .InvokeZoomControlsEnabled(false)
    .InvokeCompassEnabled(true);

FragmentTransaction fragTx = FragmentManager.BeginTransaction();
_mapFragment = MapFragment.NewInstance(mapOptions);
fragTx.Add(Resource.Id.map, _mapFragment, "map");
fragTx.Commit();
```

Die Möglichkeit zum Konfigurieren einer `GoogleMap` Objekt wird durch Festlegen von Werten der [UiSettings](http://developer.android.com/reference/com/google/android/gms/maps/UiSettings.html) Eigenschaft der Map-Objekt. Im nächste Codebeispiel wird gezeigt, wie so konfigurieren Sie eine `GoogleMap` die Zoomsteuerelemente und eine Kompass angezeigt:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.UiSettings.ZoomControlsEnabled = true;
    _map.UiSettings.CompassEnabled = true;
}
```


## <a name="interacting-with-the-map"></a>Mit der entsprechenden Karte interagieren

Die Android Maps-API bietet APIs, mit denen eine Aktivität zum Ändern der Sicht, Marker hinzufügen, benutzerdefinierte Overlays platzieren oder geometrische Formen gezeichnet werden soll. In diesem Abschnitt wird erläutert, wie einige dieser Aufgaben in Xamarin.Android ausgeführt erläutert.

### <a name="changing-the-viewpoint"></a>Ändern der Sicht

Zuordnungen werden als eine flache Ebene auf dem Bildschirm, basierend auf die Mercator-Projektion modelliert. Die Kartenansicht wird, die von einem *Kamera* gerade nach unten auf dieser Ebene suchen. Die Kameraposition kann durch Ändern der Position, Zoom, Neigung und Einfluss gesteuert werden. Die [CameraUpdate](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdate) Klasse wird verwendet, um den Speicherort der Kamera zu verschieben. `CameraUpdate` Objekte werden nicht direkt instanziiert werden, stattdessen die Maps-API bietet die [CameraUpdateFactory](http://developer.android.com/reference/com/google/android/gms/maps/CameraUpdateFactory.html) Klasse.

Einmal eine `CameraUpdate` Objekt erstellt wurde, es wird als Parameter übergeben, entweder die [GoogleMap.MoveCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#moveCamera%28com.google.maps.CameraUpdate%29) oder [GoogleMap.AnimateCamera](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.html#animateCamera%28com.google.maps.CameraUpdate%29) Methoden. Die `MoveCamera` Methode aktualisiert die Zuordnung während der sofort die `AnimateCamera` Methode stellt einen Übergang smooth, animierten.

Dieser Codeausschnitt ist ein einfaches Beispiel zum Verwenden der `CameraUpdateFactory` zum Erstellen einer `CameraUpdate` , die die Zoomstufe der Karte um eins erhöht:

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(CameraUpdateFactory.ZoomIn());
}
```

Die Maps-API bietet eine [CameraPosition](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.html) aggregiert die alle möglichen Werte für die Kameraposition. Eine Instanz dieser Klasse kann angegeben werden, um die [CameraUpdateFactory.NewCameraPosition](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/CameraUpdateFactory#newCameraPosition%28com.google.android.gms.maps.model.CameraPosition%29) Methode zurückgegeben wird, wird ein `CameraUpdate` Objekt. Die Maps-API enthält auch die [CameraPosition.Builder](http://developer.android.com/reference/com/google/android/gms/maps/model/CameraPosition.Builder.html) -Klasse, die zum Erstellen eine fluent-API bietet `CameraPosition` Objekte.
Der folgende Codeausschnitt zeigt ein Beispiel zum Erstellen einer `CameraUpdate` aus einem `CameraPosition` und verwenden, um die Kameraposition zu ändern einer `GoogleMap`:

```csharp
LatLng location = new LatLng(50.897778, 3.013333);
CameraPosition.Builder builder = CameraPosition.InvokeBuilder();
builder.Target(location);
builder.Zoom(18);
builder.Bearing(155);
builder.Tilt(65);
CameraPosition cameraPosition = builder.Build();
CameraUpdate cameraUpdate = CameraUpdateFactory.NewCameraPosition(cameraPosition);

MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    _map.MoveCamera(cameraUpdate);
}
```

Im vorherigen Codeausschnitt ist ein bestimmten Speicherort auf der Karte dargestellt, durch die eine [LatLng](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/LatLng) Klasse. Der Zoomfaktor wird 18 festgelegt. Der Einfluss wird im Uhrzeigersinn aus North Compass Messung. Die Neigung Eigenschaft steuert den Betrachtungswinkel und gibt einen Winkel von 25 Grad aus der vertikalen. Der folgende Screenshot zeigt die `GoogleMap` nach der Ausführung des vorangehenden Codes:

[![Beispiel für Google-Zuordnung mit einem angegebenen Speicherort mit einem Geneigter Betrachtungswinkel](maps-api-images/image06-sml.png)](maps-api-images/image06.png#lightbox)


### <a name="drawing-on-the-map"></a>Zeichnen auf der Karte

Die Android Maps-API bietet APIs für das Zeichnen die folgenden Elemente auf einer Karte:

-  **Marker** -Hierbei handelt es sich um mit besonderen Symbolen, die verwendet werden, um einen einzelnen Speicherort auf einer Karte zu identifizieren.

-  **Overlays** -Dies ist ein Bild, das verwendet werden kann, um eine Auflistung von Positionen oder auf der Karte zu identifizieren.

-  **Linien, Polygone und Kreise** – Dies sind die APIs, die Aktivitäten zum Hinzufügen von Formen zu einer Zuordnung zu ermöglichen.


#### <a name="markers"></a>Marker

Die Maps-API bietet eine [Marker](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) Klasse, die alle Daten über einen einzelnen Standort auf einer Karte kapselt. Verwenden sie standardmäßig ein Standardsymbol von Google Maps bereitgestellt. Es ist möglich, um die Darstellung einer Markierung anzupassen und So reagieren Sie auf die benutzeraufrufen.


##### <a name="adding-a-marker"></a>Einen Marker hinzufügen

Um eine Zuordnung einen Marker hinzugefügt haben, ist es notwendig erstellen Sie ein neues [MarkerOptions](https://developers.google.com/android/reference/com/google/android/gms/maps/model/MarkerOptions) Objekt, und rufen Sie anschließend die [AddMarker](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addMarker%28com.google.android.gms.maps.model.MarkerOptions%29) Methode auf eine `GoogleMap` Instanz. Diese Methode zurückgegeben wird ein [Marker](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Marker) Objekt.

```csharp
MapFragment mapFrag = (MapFragment) FragmentManager.FindFragmentById(Resource.Id.my_mapfragment_container);
mapFrag.GetMapAsync(this);
...
if (_map != null) {
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    _map.AddMarker(markerOpt1);
}
```

Der Titel des Markers wird angezeigt, eine *Fenster "Info"* Wenn der Benutzer tippt, auf dem Marker. Der folgende Screenshot zeigt, wie diese Marke aussieht:

[![Durch einen Marker und ein Fenster "Info" für Vimy Ridge Beispiel Google-Zuordnung](maps-api-images/image07-sml.png)](maps-api-images/image07.png#lightbox)


##### <a name="customizing-a-marker"></a>Einen Marker anpassen

Es ist möglich, anpassen, das Symbol ", der vom Marker verwendet wird, durch Aufrufen der `MarkerOptions.InvokeIcon` -Methode auf, wenn die Markierung der Karte hinzufügen.
Diese Methode nimmt einen [BitmapDescriptor](http://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptor.html) Objekt, das zum Rendern der Symbol erforderlichen Daten enthält. Die [BitmapDescriptorFactory](https://developer.android.com/reference/com/google/android/gms/maps/model/BitmapDescriptorFactory.html) -Klasse bietet einige Hilfsmethoden zum Vereinfachen der Erstellung einer `BitmapDescriptor`. Die folgende Liste führt einige der folgenden Methoden:

-   `DefaultMarker(float colour)` &ndash; Verwenden Sie den Standardwert Google Maps Marker, aber ändern Sie die Farbe.

-   `FromAsset(string assetName)` &ndash; Verwenden Sie ein benutzerdefiniertes Symbol aus der angegebenen Datei im Ordner "Assets".

-   `FromBitmap(Bitmap image)` &ndash; Verwenden Sie die angegebene Bitmap als Symbol.

-   `FromFile(string fileName)` &ndash; Erstellen Sie das benutzerdefinierte Symbol aus der Datei im angegebenen Pfad.

-   `FromResource(int resourceId)` &ndash; Erstellen Sie ein benutzerdefiniertes Symbol aus der angegebenen Ressource an.

Der folgende Codeausschnitt zeigt ein Beispiel für ein Marker Zyan gefärbt standardmäßig erstellen:

```csharp
mapFrag.GetMapAsync(this);
...
if (_map != null)
{
    MarkerOptions markerOpt1 = new MarkerOptions();
    markerOpt1.SetPosition(new LatLng(50.379444, 2.773611));
    markerOpt1.SetTitle("Vimy Ridge");
    markerOpt1.InvokeIcon(BitmapDescriptorFactory.DefaultMarker (BitmapDescriptorFactory.HueCyan));
    _map.AddMarker(markerOpt1);
}
```


#### <a name="info-windows"></a>Info-Windows

*Info Windows* sind spezielle Windows diese Popup Informationen für den Benutzer angezeigt, wenn sie eine bestimmte Marke Tippen Sie auf. Standardmäßig wird das Informationsfenster den Inhalt des Titels für die Markierung angezeigt. Wenn der Titel nicht zugewiesen wurde, wird kein Infofenster angezeigt. Nur ein Fenster "Info" möglicherweise zu einem Zeitpunkt angezeigt werden.

Es ist möglich, durch die Implementierung der Informationsfenster Anpassen der [GoogleMap.IInfoWindowAdapter](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap.InfoWindowAdapter) Schnittstelle. Es gibt zwei wichtige Methoden für diese Schnittstelle:

-  `public View GetInfoWindow(Marker marker)` &ndash; Diese Methode wird aufgerufen, um ein Informationsfenster benutzerdefinierte für einen Marker zu erhalten. Wenn zurückgegeben `null` , und klicken Sie dann das Standardrendering zurückgreifen Fenster verwendet werden soll. Wenn diese Methode eine Sicht zurückgibt, wird dieser Ansicht innerhalb der Fensterrahmen Info platziert werden.

-  `public View GetInfoContents(Marker marker)` &ndash; Diese Methode wird nur aufgerufen werden, wenn GetInfoWindow zurückgibt `null` . Diese Methode kann Zurückgeben einer `null` -Wert, wenn das Standardrendering von Inhalt im Befehlsfenster "Info" verwendet wird. Andernfalls sollte diese Methode eine Sicht mit dem Inhalt des Fensters Informationen zurückgeben.

Ein Informationsfenster ist nicht für eine live-Ansicht - stattdessen Android konvertieren Sie die Ansicht in einer statischen Bitmap und auf das Bild angezeigt wird. Dies bedeutet, dass ein Infofenster nicht auf das Berührungsereignisse kein(e) Gesten, noch wird es automatisch aktualisiert. Um ein Infofenster zu aktualisieren, ist es notwendig, die [GoogleMap.ShowInfoWindow](http://developer.android.com/reference/com/google/android/gms/maps/model/Marker.html#showInfoWindow()) Methode.

Die folgende Abbildung zeigt einige Beispiele für einige angepasste Informationsfenster. Das Bild auf der linken Seite hat, dessen Inhalt angepasst werden, während das Bild auf der rechten Seite des Fensters und Inhalt angepasst hat:

![Beispiel Marker Windows für Melbourne, einschließlich Symbol und Auffüllung. Im Rechte Fenster abgerundeten Ecken.](maps-api-images/marker-infowindows.png)


#### <a name="ground-overlays"></a>Grund Overlays

Im Gegensatz zu den Marker, die einen bestimmten Speicherort auf einer Karte zu identifizieren, eine [GroundOverlay](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlay.html) ist ein Bild, das verwendet wird, um eine Auflistung von Positionen oder auf einen Bereich auf der Karte zu identifizieren.


##### <a name="adding-a-groundoverlay"></a>Hinzufügen einer GroundOverlay

Hinzufügen von einem Ground Overlay zu einer Zuordnung ähnelt einen Marker zu einer Zuordnung hinzufügen. Zuerst wird eine [GroundOverlayOptions](http://developer.android.com/reference/com/google/android/gms/maps/model/GroundOverlayOptions.html) Objekt erstellt wird. Dieses Objekt wird dann als Parameter an übergeben der `GoogleMap.AddGroundOverlay` -Methode, die zurückgegeben wird ein `GroundOverlay` Objekt. Dieser Codeausschnitt ist ein Beispiel für eine Zuordnung eine Ground Überlagerung hinzugefügt:

```csharp
BitmapDescriptor image = BitmapDescriptorFactory.FromResource(Resource.Drawable.polarbear);
GroundOverlayOptions groundOverlayOptions = new GroundOverlayOptions()
    .Position(position, 150, 200)
    .InvokeImage(image);
GroundOverlay myOverlay = _map.AddGroundOverlay(groundOverlayOptions);
```

Der folgende Screenshot zeigt diese Overlay auf einer Karte:

[![Beispiel-Zuordnung mit einem Rasterquadrat Image von einem Polardiagramm tragen](maps-api-images/image09-sml.png)](maps-api-images/image09.png#lightbox)


#### <a name="lines-circles-and-polygons"></a>Linien, Kreise und Polygone

Es gibt drei einfache Typen der geometrischen Formen, die zu einer Zuordnung hinzugefügt werden können:

-  **Polylinie** -Dies ist eine Reihe von Liniensegmenten verbunden. Sie können einen Pfad auf einer Karte zu markieren oder bilden eine beliebige Form erforderlich sind.

-  **Polygon** -Dies ist eine geschlossene Form zum Markieren von Bereichen auf einer Karte.

-  **Kreis** – Dies wird einen Kreis gezeichnet, in der Zuordnung.



##### <a name="polylines"></a>Polylinien

Ein [Polylinie](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/Polyline) ist eine Liste von aufeinander folgenden `LatLng` -Objekte, die die Eckpunkte des jedes Liniensegment angeben. Eine Polylinie wird erstellt, indem zunächst erstellt eine `PolylineOptions` -Objekt und die Punkte hinzugefügt. Die `PolylineOption` Objekt wird dann zum Übergeben einer `GoogleMap` Objekt durch Aufrufen der `AddPolyline` Methode.

```csharp
PolylineOption rectOptions = new PolylineOption();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
rectOptions.Add(new LatLng(37.35, -122.0)); // close the polyline - this makes a rectangle.
myMap.AddPolyline(rectOptions);
```


##### <a name="polygons"></a>Polygone

`Polygon`s sind sehr ähnlich `Polyline`s, beendet wurde, jedoch nicht geöffnet werden. `Polygon`s sind eine geschlossene Schleife und deren innere ausgefüllt.
`Polygon`s werden erstellt, auf genau dieselbe Weise als ein `Polyline`, mit Ausnahme der [GoogleMap.AddPolygon](http://developer.android.com/reference/com/google/android/gms/maps/GoogleMap.html#addPolygon(com.google.android.gms.maps.model.PolygonOptions)) aufgerufene Methode.

Im Gegensatz zu einer `Polyline`ein `Polygon` selbst geschlossen wird. Wenn `AddPolygon` aufgerufen wird, wird die Methode wird automatisch geschlossen deaktiviert das Polygon durch Zeichnen einer Linie, der das erste und letzte Punkt eine Verbindung herstellt. Der folgende Codeausschnitt erstellt ein solid Rechteck über der gleiche Bereich wie im vorherigen Codeausschnitt in der `Polyline` Beispiel.

```csharp
PolygonOptions rectOptions = new PolygonOptions();
rectOptions.Add(new LatLng(37.35, -122.0));
rectOptions.Add(new LatLng(37.45, -122.0));
rectOptions.Add(new LatLng(37.45, -122.2));
rectOptions.Add(new LatLng(37.35, -122.2));
// notice we don't need to close off the polygon
myMap.AddPolygon(rectOptions);
```


##### <a name="circles"></a>Kreise

Kreise werden erstellt, indem die ersten Instanziieren einer [CircleOption](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/model/CircleOptions) Objekt, das dem Mittelpunkt und den Radius der Kreisradius in Metern angeben. Der Kreis wird auf der Karte gezeichnet, durch Aufrufen [GoogleMap.AddCircle](https://developers.google.com/maps/documentation/android/reference/com/google/android/gms/maps/GoogleMap#addCircle(com.google.android.gms.maps.model.CircleOptions)).
Der folgende Codeausschnitt veranschaulicht das Zeichnen eines Kreises:

```csharp
CircleOptions circleOptions = new CircleOptions ();
circleOptions.InvokeCenter (new LatLng(37.4, -122.1));
circleOptions.InvokeRadius (1000);
_map.AddCircle (circleOptions);
```


## <a name="responding-to-events"></a>Reagieren auf Ereignisse

Es gibt drei Arten von Aktivitäten, die ein Benutzer möglicherweise durch eine Zuordnung:

-  **Marker auf** – der Benutzer klickt auf einen Marker.

-  **Ziehen Sie die Markierung** -Long-geklickt wurde auf eine Mparger

-  **Info über Fenster klicken** -geklickt wurde für ein Infofenster.

Jedes dieser Ereignisse werden im folgenden ausführlicher erläutert.


### <a name="marker-click-events"></a>Marker Click-Ereignisse

Wenn der Benutzer klickt auf einen Marker der `MarkerClick` Ereignis wird ausgelöst, und ein `GoogleMap.MarkerClickEventArgs` übergeben. Diese Klasse enthält zwei Eigenschaften:

-  `GoogleMap.MarkerClickEventArgs.Handled` &ndash; Diese Eigenschaft sollte festgelegt werden, um `true` um anzugeben, dass der Ereignishandler das Ereignis verarbeitet wurde. Wenn dies, um festgelegt ist `false` dann zusätzlich zu das benutzerdefinierte Verhalten des ereignishandlers standardmäßig ausgeführt wird.

-  `P0` &ndash; Diese schlecht Name-Parameter ist ein Verweis auf den Marker, die ausgelöst der `MarkerClick` Ereignis.


Dieser Codeausschnitt zeigt ein Beispiel für eine `MarkerClick` ändert, die an einem neuen Speicherort auf der Karte die Kameraposition:

```csharp
private void MapOnMarkerClick(object sender, GoogleMap.MarkerClickEventArgs markerClickEventArgs)
{
    markerClickEventArgs.Handled = true;
    Marker marker = markerClickEventArgs.P0;
    if (marker.Id.Equals(MyMarkerId)) // The ID of a specific marker the user clicked on.
    {
        _map.AnimateCamera(CameraUpdateFactory.NewLatLngZoom(new LatLng(20.72110, -156.44776), 13));
    }
    else
    {
        Toast.MakeText(this, String.Format("You clicked on Marker ID {0}", marker.Id), ToastLength.Short).Show();
    }
}
```


### <a name="marker-drag-events"></a>Ziehen Sie Methodenmarkerereignisse

Dieses Ereignis wird ausgelöst, wenn der Benutzer möchte die Marke ziehen. Marker sind standardmäßig nicht verschiebbare. Ein Marker kann festgelegt werden wie draggable durch Festlegen der `Marker.Draggable` Eigenschaft `true` oder durch Aufrufen der `MarkerOptions.Draggable` Methode mit `true` als Parameter.

Um zuerst die Markierung zu ziehen, der Benutzer muss Long-darauf klicken und halten ihre Finger auf der Karte. Wenn sie ihre Finger auf dem Bildschirm ziehen, wird die Markierung verschoben. Bei den Finger wieder an den Benutzer außerhalb des Bildschirms abhebt, bleibt die Markierung vorhanden.

Die folgende Liste beschreibt die verschiedenen Ereignisse, die für eine verschiebbare Marker ausgelöst werden:

-   `GoogleMap.MarkerDragStart(object sender, GoogleMap.MarkerDragStartEventArgs e)` &ndash; Dieses Ereignis wird ausgelöst, wenn der Benutzer zuerst die Markierung zieht.

-   `GoogleMap.MarkerDrag(object sender, GoogleMap.MarkerDragEventArgs e)` &ndash; Dieses Ereignis wird ausgelöst, wenn die Markierung gezogen wird.

-   `GoogleMap.MarkerDragEnd(object sender, GoogleMap.MarkerDragEndEventArgs e)` &ndash; Dieses Ereignis wird ausgelöst, wenn die When der Benutzer ist abgeschlossen. Ziehen die Markierung.

Jede der `EventArgs` enthält eine einzelne Eigenschaft mit dem Namen `P0` , einen Verweis auf die `Marker` Objekt gezogen wird.


### <a name="info-window-click-events"></a>Fenster "Info" Click-Ereignisse

Nur ein Informationsfenster kann zu einem Zeitpunkt angezeigt werden. Klickt der Benutzer auf ein Fenster "Info" in einer Zuordnung, löst die Map-Objekt ein `InfoWindowClick` Ereignis. Der folgende Codeausschnitt veranschaulicht, um einen Handler an das Ereignis zu verknüpfen:

```csharp
private bool SetupMapIfNeeded()
{
    if (_map == null)
    {
        _map = _mapFragment.Map;
        if (_map != null)
        {
            _map.InfoWindowClick += MapOnInfoWindowClick;
            return true;
        }
        return false;
    }
    return true;
}

private void MapOnInfoWindowClick (object sender, GoogleMap.InfoWindowClickEventArgs e)
{
    Marker myMarker = e.P0;
    // Do something with marker.
}
```

Beachten Sie, dass ein Infofenster einen statischen ist `View` der wird als ein Bild auf der Karte gerendert. Alle Widgets wie Schaltflächen, Kontrollkästchen oder Textansichten, die in das Informationsfenster platziert werden werden statische und können nicht auf alle ihre ganzzahlige Benutzerereignisse zu reagieren.



## <a name="related-links"></a>Verwandte Links

- [Google Play-Dienste](http://developer.android.com/google/play-services/index.html)
- [Google ordnet Android-API v2](https://developers.google.com/maps/documentation/android/)
- [Google Play Services APK](https://play.google.com/store/apps/details?id=com.google.android.gms&hl=en)
- [Abrufen eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md)
