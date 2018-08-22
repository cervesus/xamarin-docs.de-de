---
title: Ortungsdienste
description: Dieses Handbuch stellt Netzwerkadressinformationen in Android-Anwendungen, und es wird veranschaulicht, wie Sie den Standort des Benutzers mit der Android-API Speicherort sowie der fused Ortungsanbieter Speicherort die Google-API abrufen.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/22/2018
ms.openlocfilehash: 2cd49441ec9ba15cd7fd2647fa74b39b32ea8acd
ms.sourcegitcommit: e64c3c10d6a36b3b031d6d4dbff7af74ab2b7f21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/10/2018
ms.locfileid: "40251081"
---
# <a name="location-services"></a>Ortungsdienste

_Dieses Handbuch stellt Netzwerkadressinformationen in Android-Anwendungen, und es wird veranschaulicht, wie Sie den Standort des Benutzers mit der Android-API Speicherort sowie der fused Ortungsanbieter Speicherort die Google-API abrufen._

## <a name="location-services-overview"></a>Speicherort: Übersicht

Android bietet Zugriff auf verschiedenen Speicherort-Technologien wie z. B. die Position der Zelle Tower, Wi-Fi und GPS. Die Details zu jeder Technologie Standort werden über abstrahiert *Ortungsanbietern*, ermöglicht es Anwendungen, die Standorte auf die gleiche Weise, unabhängig von den verwendeten Anbieter zu erhalten. Dieser Leitfaden beschreibt die fused-Location-Anbieters, ein Teil der Google Play-Dienste, die auf intelligente Weise bestimmt die beste Möglichkeit zum Abrufen des Speicherort der Geräte basierend auf welche Anbieter verfügbar sind und wie das Gerät verwendet wird. Android-Ort-Service-API- und zeigt, wie für die Kommunikation mit dem Speicherort im Dateisystem-Dienst mithilfe einer `LocationManager`. Der zweite Teil des Handbuchs untersucht die Android Location Services-API mithilfe der `LocationManager`.
 
Als allgemeine Faustregel sollten Anwendungen die fused Location-Anbieters, das Fallback für die älteren Android Speicherort-API nur bei Bedarf verwenden möchten.

## <a name="location-fundamentals"></a>Speicherort-Grundlagen

In Android, unabhängig davon, welche API, die Sie auswählen, für die Arbeit mit Daten, bleiben einige Konzepte gleich. Dieser Abschnitt enthält die Location-Anbieter und entsprechende Berechtigungen.

### <a name="location-providers"></a>Location-Anbieter

Mehrere Technologien werden intern verwendet, um den Standort des Benutzers zu ermitteln. Der verwendeten Hardware abhängt, auf dem Typ des *Ortungsanbieter* für den Auftrag des Sammelns von Daten ausgewählt. Android verwendet drei Location-Anbieter:

-   **GPS-Anbieter** &ndash; erhält den genauesten Speicherort, verwendet das größte Leistungspotenzial und funktioniert am besten im freien GPS. Dieser Anbieter verwendet eine Kombination von GPS und persönlicher GPS ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), womit GPS-Daten, die von cellular Towers erfasst.

-   **Netzwerk-Anbieter** &ndash; stellt eine Kombination von Wi-Fi "und" Mobiltelefon Daten, einschließlich aGPS-Daten, die von der Zelle Towers erfasst. Weniger Energie als den GPS-Anbieter verwendet, sondern gibt Positionsdaten unterschiedlicher Genauigkeit.

-   **Passive Anbieter** &ndash; eine "piggyback" Option mithilfe von anderen Anwendungen oder Diensten angeforderten Anbieter zum Generieren von Daten in einer Anwendung. Dies ist eine weniger zuverlässige aber Energiesparoption Ideal für Anwendungen, die keine Konstante speicherortaktualisierungen Arbeit erforderlich ist.

Location-Anbieter sind nicht immer verfügbar. Beispielsweise wir möglicherweise GPS bei unserer Anwendung verwenden möchten, aber GPS kann in den Einstellungen deaktiviert werden, oder das Gerät möglicherweise nicht GPS überhaupt. Wenn ein bestimmter Anbieter nicht verfügbar ist, diesen Anbieter möglicherweise zurück auf die Schaltfläche `null`.

### <a name="location-permissions"></a>Berechtigungen für Positionsdaten

Eine standortbasierte Anwendung muss Zugriff eines Geräts hardwaresensoren um GPS, Wi-Fi und datenverbindungen zu erhalten. Zugriff wird über die entsprechenden Berechtigungen in der Anwendung Android-Manifest gesteuert.
Es gibt zwei Berechtigungen verfügbar &ndash; je nach den Anforderungen Ihrer Anwendung und Ihrer Wahl-API, möchten eine zulassen:

-   `ACCESS_FINE_LOCATION` &ndash; Ermöglicht eine Anwendung den Zugriff auf GPS.
    Erforderlich für die *GPS-Anbieter* und *passiven Anbieter* Optionen (*passiven Anbieter benötigt die Berechtigung zum Zugriff auf GPS-Daten gesammelt, die von einer anderen Anwendung oder Dienst*). Optionale Berechtigung für die *Netzwerkanbieter*.

-   `ACCESS_COARSE_LOCATION` &ndash; Ermöglicht eine Anwendung den Zugriff auf Mobilfunk- und Wi-Fi-Speicherort an. Erforderlich für *Netzwerkanbieter* Wenn `ACCESS_FINE_LOCATION` ist nicht festgelegt.

Für apps, die Ziel-API-Version 21 (Android 5.0 Lollipop) oder höher verwenden, können Sie aktivieren `ACCESS_FINE_LOCATION` und auf Geräten, die keine GPS-Hardware weiterhin ausgeführt. Wenn Ihre app GPS-Hardware erforderlich ist, sollten Sie explizit Hinzufügen einer `android.hardware.location.gps` `uses-feature` Element, das Android-Manifest. Weitere Informationen finden Sie im Android [verwendet-Feature](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) Elementverweis.

Um die Berechtigungen festgelegt haben, erweitern Sie die **Eigenschaften** Ordner in der **Lösungspad** und doppelklicken Sie auf **"androidmanifest.xml"**. Die Berechtigungen werden er unter **erforderliche Berechtigungen**:

[![Screenshot der Einstellungen für Android-Manifest erforderliche Berechtigungen](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Durch Festlegen eines dieser Berechtigungen teilt Android, dass Ihre Anwendung vom Benutzer die Berechtigung zum Zugriff auf die Location-Anbieter benötigt. Geräte, die API-Ebene 22 (Android 5.1) ausführen oder niedriger fordert den Benutzer zum gewähren dieser Berechtigungen jedes Mal, die die app installiert ist. Auf Geräten mit-API-Ebene 23 (Android 6.0) oder höher verwenden, sollte die app eine Laufzeit berechtigungsüberprüfung, bevor eine Anforderung des Ortungsanbieters ausführen. 

> [!NOTE]
>Hinweis: Das Festlegen `ACCESS_FINE_LOCATION` bedeutet den Zugriff auf beide Daten grob gehalten und ist in Ordnung. Sollte nie müssen Sie beide nur über die Berechtigungen, Festlegen der *minimale* Berechtigung der app arbeiten erforderlich.

Dieser Codeausschnitt ist ein Beispiel für So prüfen Sie, dass eine app-Berechtigung für die `ACCESS_FINE_LOCATION` Berechtigung:

```csharp
 if (ContextCompat.CheckSelfPermission(this, Manifest.Permission.AccessFineLocation) == Permission.Granted)
{
    StartRequestingLocationUpdates();
    isRequestingLocationUpdates = true;
}
else
{
    // The app does not have permission ACCESS_FINE_LOCATION 
}
```

Apps müssen fehlertoleranten des Szenarios, in denen der Benutzer wird die Berechtigung nicht gewährt (oder verfügt über die Berechtigung aufgehoben) und eine Möglichkeit, die mit dieser Situation ordnungsgemäß behandeln. Informieren Sie sich die [Handbuch Berechtigungen](~/android/app-fundamentals/permissions.md) prüft, ob weitere Informationen zum Implementieren der Laufzeit-Berechtigung in Xamarin.Android.


## <a name="using-the-fused-location-provider"></a>Verwenden die fused-Location-Anbieters

Die fused-Location-Anbieters ist die bevorzugte Methode für die Android-Anwendungen auf die Speicherort-Updates vom Gerät empfangen werden, weil sie effizient die Location-Anbieters während der Laufzeit, um die besten Informationen zum Speicherort auf Akku-effiziente Weise bereitzustellen ausgewählt wird. Ein Benutzer durchlaufen, um im freien ruft z. B. den besten Speicherort lesen mit GPS. Wenn der Benutzer in geschlossener Umgebung durchläuft, in denen GPS funktioniert nicht ordnungsgemäß (wenn überhaupt), die fused-Location-Anbieters kann automatisch zur WiFi, was in geschlossener Umgebung besser zu wechseln.
 
Die fused Location-Anbieter-API bietet eine Vielzahl von anderen Tools standortbezogenen Anwendungen, einschließlich Geofencing und aktivitätsüberwachung zu ermöglichen. In diesem Abschnitt werden wir den Fokus auf die Grundlagen zum Einrichten der `LocationClient`, Anbieter einrichten und den Standort des Benutzers abrufen.

Die fused Ortungsanbieter ist Teil des [Google Play Services](http://developer.android.com/google/play-services/index.html).
Das Google Play Services-Paket muss installiert und ordnungsgemäß konfiguriert werden, in der Anwendung für die fused Location-Anbieter-API arbeiten und das Gerät muss das Google Play Services APK installiert haben.

Bevor Sie eine xamarin.Android-Anwendung Anwendung können die fused-Location-Anbieters, es muss Hinzufügen der **Xamarin.GooglePlayServices.Maps** Paket dem Projekt. Zudem können die folgenden `using` Anweisungen, die auf die unten beschriebenen Klassen verweisen Quelldateien hinzugefügt werden sollen:

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Überprüft, ob es sich bei Google Play Services installiert ist

Eine Xamarin.Android-wird abstürzen, wenn versucht wird, die fused-Location-Anbieters verwenden, wenn es sich bei Google Play Services nicht installiert ist (oder veraltet) und dann eine Ausnahme zur Laufzeit würde auftreten.  Wenn Google Play Services nicht installiert ist, sollte dann die Anwendung auf der Android Speicherortdienst weiter oben erläuterten zurückgreifen. Wenn Google Play-Dienste nicht mehr aktuell ist, konnte die app eine Nachricht an den Benutzer auffordert, zum Aktualisieren der installierten Version von Google Play Services anzeigen.

Dieser Codeausschnitt ist ein Beispiel wie eine Android-Aktivität programmgesteuert überprüfen können, ob es sich bei Google Play Services installiert ist:

```csharp
bool IsGooglePlayServicesInstalled()
{
    var queryResult = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
    if (queryResult == ConnectionResult.Success)
    {
        Log.Info("MainActivity", "Google Play Services is installed on this device.");
        return true;
    }

    if (GoogleApiAvailability.Instance.IsUserResolvableError(queryResult))
    {
        // Check if there is a way the user can resolve the issue
        var errorString = GoogleApiAvailability.Instance.GetErrorString(queryResult);
        Log.Error("MainActivity", "There is a problem with Google Play Services on this device: {0} - {1}",
                  queryResult, errorString);
                  
        // Alternately, display the error to the user.
    }

    return false;
}
```

### <a name="fusedlocationproviderclient"></a>FusedLocationProviderClient

Zum interagieren mit der fused Ortungsanbieter benötigen eine Xamarin.Android-Anwendung eine Instanz von der `FusedLocationProviderClient`. Diese Klasse stellt die erforderlichen Methoden speicherortaktualisierungen abonnieren und den letzten bekannten Speicherort des Geräts abrufen.

Die `OnCreate` geht von einer Aktivität einer geeigneten Stelle zum Abrufen eines Verweises auf die `FusedLocationProviderClient`, wie im folgenden Codeausschnitt gezeigt:

```csharp
public class MainActivity: AppCompatActivity
{
    FusedLocationProviderClient fusedLocationProviderClient;

    protected override void OnCreate(Bundle bundle) 
    {
        fusedLocationProviderClient = LocationServices.GetFusedLocationProviderClient(this);
    }
}
```

### <a name="getting-the-last-known-location"></a>Abrufen der letzten bekannten Speicherort

Die `FusedLocationProviderClient.GetLastLocationAsync()` Methode bietet eine einfache, nicht blockierende-Methode für eine Xamarin.Android-Anwendung, um den letzten bekannten Speicherort des Geräts mit minimaler Codierung Mehraufwand schnell zu erhalten.

Dieser Codeausschnitt zeigt, wie die `GetLastLocationAsync` Methode, um den Speicherort des Geräts abzurufen:

```csharp
async Task GetLastLocationFromDevice()
{
    // This method assumes that the necessary run-time permission checks have succeeded.
    getLastLocationButton.SetText(Resource.String.getting_last_location);
    Android.Locations.Location location = await fusedLocationProviderClient.GetLastLocationAsync();

    if (location == null)
    {
        // Seldom happens, but should code that handles this scenario
    }
    else
    {
        // Do something with the location 
        Log.Debug("Sample", "The latitude is " + location.Latitude);
    }
}
```

### <a name="subscribing-to-location-updates"></a>Abonnieren von Speicherort aktualisiert

Eine Xamarin.Android-Anwendung abonnieren Sie speicherortaktualisierungen vom fused Ortungsanbieter mithilfe der `FusedLocationProviderClient.RequestLocationUpdatesAsync` Methode, wie im folgenden Codeausschnitt gezeigt:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
Diese Methode akzeptiert zwei Parameter:

-   **`Android.Gms.Location.LocationRequest`** &ndash; Ein `LocationRequest` Objekt ist, wie eine Xamarin.Android-Anwendung die Parameter zur Funktionsweise der fused-Location-Anbieters übergeben. Die `LocationRequest` enthält Informationen, die solche wie häufige Anforderungen gemacht werden sollen, oder wie wichtig eine Aktualisierung der genaue Speicherort sein. Beispielsweise bewirkt eine wichtiger Speicherort-Anforderung des Geräts zu bestimmen den Speicherort des GPS und daher mehr Leistung verwenden. Dieser Codeausschnitt zeigt, wie Sie erstellen eine `LocationRequest` für einen Standort mit hoher Genauigkeit, überprüfen etwa alle fünf Minuten eine Speicherort-Update (aber nicht früher als zwei Minuten zwischen den Anforderungen). Die fused-Location-Anbieters verwenden eine `LocationRequest` als Leitfaden für die Location-Anbieters mit, dass beim Versuch um der Standort des Geräts zu bestimmen:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Um speicherortaktualisierungen zu erhalten, muss eine Xamarin.Android-Anwendung als Unterklasse der `LocationProvider` abstrakte Klasse. Diese Klasse verfügbar gemacht werden zwei Methoden, die möglicherweise aufgerufen, durch die fused-Location-Anbieters aktualisieren die app mit Informationen zum Speicherort. Dies wird im folgenden ausführlicher erläutert werden.

Um eine Xamarin.Android-Anwendung eines Updates Speicherort zu benachrichtigen, die fused-Location-Anbieters wird aufgerufen, die `LocationCallBack.OnLocationResult(LocationResult result)`. Die `Android.Gms.Location.LocationResult` Parameter enthält die Informationen zum Update Speicherort.

Wenn die fused-Location-Anbieters eine Änderung in die Verfügbarkeit von Daten erkennt, ruft sie die `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` Methode. Wenn die `LocationAvailability.IsLocationAvailable` -Eigenschaft gibt `true`, wird davon ausgegangen werden kann, dass durch die speicherortergebnissen Gerät gemeldet `OnLocationResult` sind so genau, und wie auf dem neuesten Stand gemäß der `LocationRequest`. Wenn `IsLocationAvailable` ist "false" werden keine Ergebnisse zurück, indem `OnLocationResult`.

Dieser Codeausschnitt ist eine beispielimplementierung der `LocationCallback` Objekt:

```csharp
public class FusedLocationProviderCallback : LocationCallback
{
    readonly MainActivity activity;

    public FusedLocationProviderCallback(MainActivity activity)
    {
        this.activity = activity;
    }

    public override void OnLocationAvailability(LocationAvailability locationAvailability)
    {
        Log.Debug("FusedLocationProviderSample", "IsLocationAvailable: {0}",locationAvailability.IsLocationAvailable);
    }

    public override void OnLocationResult(LocationResult result)
    {
        if (result.Locations.Any())
        {
            var location = result.Locations.First();
            Log.Debug("Sample", "The latitude is :" + location.Latitude);
        }
        else
        {
            // No locations to work with.
        }
    }
}
```

## <a name="using-the-android-location-service-api"></a>Die Speicherort von Android-API verwenden

Der Speicherortdienst für Android ist eine ältere API für die Nutzung von Standortinformationen in Android. Standortdaten vom hardwaresensoren erfasst und erfasst, die von einem Systemdienst, die in der Anwendung mit zugegriffen wird eine `LocationManager` Klasse und ein `ILocationListener`.

Der Speicherortdienst eignet sich optimal für Anwendungen, die auf Geräten ausgeführt werden müssen, die keine Google Play Services installiert haben.

Der Speicherortdienst ist eine besondere Art von [Service](http://developer.android.com/guide/components/services.html) vom System verwaltet. Ein Systemdienst interagiert mit Hardware der Geräte, und es wird immer ausgeführt. Um speicherortaktualisierungen in unserer Anwendung zu nutzen, wir abonnieren speicherortaktualisierungen aus dem System Ortsdienst mithilfe einer `LocationManager` und `RequestLocationUpdates` aufrufen.

Um den Standort des Benutzers mithilfe von Android Ortsdienst erhalten die umfasst mehrere Schritte aus:

1.  Abrufen eines Verweises auf die `LocationManager` Service.
2.  Implementieren der `ILocationListener` -Schnittstelle und behandeln Ereignisse aus, wenn der Standort ändert.
3.  Verwenden der `LocationManager` Anforderung Standort Updates für einen angegebenen Anbieter. Die `ILocationListener` aus dem vorherigen Schritt verwendet, um Rückrufe von erhalten die `LocationManager`.
4.  Beenden Sie speicherortaktualisierungen, wenn die Anwendung nicht mehr Updates empfangen werden.

### <a name="location-manager"></a>Quellspeicherort-Manager

Wir erreichen die Speicherort-Systemdienst mit einer Instanz von der `LocationManager` Klasse. `LocationManager` ist eine spezielle Klasse, die uns bei der Interaktion mit dem Speicherort im Dateisystem Service Methoden aufrufen kann. Eine Anwendung erhalten einen Verweis auf die `LocationManager` durch Aufrufen von `GetSystemService` und übergeben einen Diensttyp, wie unten dargestellt:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` ist eine gute Möglichkeit zum Abrufen eines Verweises auf die `LocationManager`.
Es ist eine gute Idee, behalten Sie die `LocationManager` als eine Klassenvariable, damit wir sie an verschiedenen Punkten im Lebenszyklus von Aktivität aufrufen kann.

### <a name="request-location-updates-from-the-locationmanager"></a>Speicherortaktualisierungen von der LocationManager anfordern

Sobald die Anwendung einen Verweis auf verfügt die `LocationManager`, teilen muss die `LocationManager` welche Art von Informationen zum Speicherort, die erforderlich sind, und wie oft diese Informationen aktualisiert werden. Dies erfolgt durch Aufrufen `RequestionLocationUpdates` auf die `LocationManager` -Objekt, und übergeben den Kriterien für die Updates und einen Rückruf, der die Speicherort-Updates erhält. Dieser Rückruf ist ein Typ, der implementieren muss, die `ILocationListener` Schnittstelle (weiter unten in diesem Handbuch ausführlicher beschrieben).

Die `RequestionLocationUpdates` Methode teilt dem Speicherort im Dateisystem Dienst, dass Ihre Anwendung den Empfang von speicherortaktualisierungen starten möchten. Dieser Methode können Sie den Anbieter als auch Zeit und Entfernung Schwellenwerte zum Steuern der aktualisierungshäufigkeit angeben. Beispielsweise aktualisiert die Methode unten unterhalb Anforderungen vom GPS-Ortungsanbieter alle 2000 Millisekunden, und nur, wenn der Standort mehr als 1 Meter ändert:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Eine Anwendung sollte nur so oft wie erforderlich, damit die Anwendung aus, um gut zu funktionieren die speicherortaktualisierungen anfordern. Dies behält Akkuverbrauch gering zu halten und eine bessere Erfahrung für den Benutzer erstellt.

### <a name="responding-to-updates-from-the-locationmanager"></a>Reagieren auf Updates aus der LocationManager

Sobald eine Anwendung auf Updates angefordert hat die `LocationManager`, erhalten sie Informationen aus dem Dienst, durch die Implementierung der [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) Schnittstelle. Diese Schnittstelle bietet vier Möglichkeiten für das Lauschen auf den Speicherort-Dienst und der Ortungsanbieter `OnLocationChanged`. Das System ruft `OnLocationChanged` Wenn sich der Standort des Benutzers ändert genug, um als eine Änderung der Position gemäß den Kriterien festgelegt, wenn der Standort Updates anfordern zu qualifizieren. 

Der folgende Code zeigt die Methoden in der `ILocationListener` Schnittstelle:

```csharp
public class MainActivity : AppCompatActivity, ILocationListener
{
    TextView latitude;
    TextView longitude;
    
    public void OnLocationChanged (Location location)
    {
        // called when the location has been updated.
    }
    
    public OnProviderDisabled(string locationProvider)
    {
        // called when the user disables the provider
    }
    
    public OnProviderEnabled(string locationProvider)
    {
        // called when the user enables the provider
    }
    
    public OnStatusChanged(string locationProvider, Availability status, Bundle extras)
    {
        // called when the status of the provider changes (there are a variety of reasons for this)
    }
}
```

### <a name="unsubscribing-to-locationmanager-updates"></a>Kündigen des Abonnements LocationManager-Updates

Um Systemressourcen zu sparen, sollte eine Anwendung auf Standort Updates so bald wie möglich wieder abmelden. Die `RemoveUpdates` Methode teilt dem `LocationManager` zum Senden von Aktualisierungen an die Anwendung zu beenden.  Beispielsweise kann eine Aktivität aufrufen `RemoveUpdates` in die `OnPause` Methode, damit wir um Energie zu sparen, wenn eine Anwendung können benötigt keine speicherortaktualisierungen zwar die Aktivität nicht auf dem Bildschirm:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Wenn Ihre Anwendung auf die Speicherort-Updates im Hintergrund zu erhalten muss, sollten Sie einen benutzerdefinierten Dienst zu erstellen, mit dem System Ortsdienst abonniert. Finden Sie in der [Hintergrundverarbeitung mit Android-Dienste](~/android/app-fundamentals/services/index.md) Anleitung finden Sie weitere Informationen.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Bestimmen der besten Location-Anbieters für die LocationManager

Die Anwendung, die oben genannten legt GPS als der Ortungsanbieter fest. Allerdings können GPS nicht verfügbar in allen Fällen wie z. B., wenn das Gerät in geschlossenen Räumen oder verfügt nicht über ein GPS-Empfänger. Wenn dies der Fall ist, wird das Ergebnis ist eine `null` für den Anbieter zurück.

Rufen Sie Ihre app funktioniert, wenn GPS nicht verfügbar ist, die Sie mit der `GetBestProvider` Methode, um die beste verfügbar (unterstützte Geräte und Benutzer aktiviert) Location-Anbieters beim Anwendungsstart anfordern. Nicht in einen bestimmten Anbieter, Sie können feststellen, `GetBestProvider` die Anforderungen für Anbieter - z. B. Genauigkeit und Leistung – mit einem [ `Criteria` Objekt](https://developer.xamarin.com/api/type/Android.Locations.Criteria/). `GetBestProvider` besten-Anbieter für die angegebenen Kriterien zurückgegeben.

Der folgende Code zeigt, wie Sie den am besten verfügbaren Anbieter erhalten, und verwenden, wenn der Standort Updates anfordern:

```csharp
Criteria locationCriteria = new Criteria();   
locationCriteria.Accuracy = Accuracy.Coarse;
locationCriteria.PowerRequirement = Power.Medium;

locationProvider = locationManager.GetBestProvider(locationCriteria, true);

if(locationProvider != null)
{
    locationManager.RequestLocationUpdates (locationProvider, 2000, 1, this);
}
else
{
    Log.Info(tag, "No location providers available");
}
```

> [!NOTE]
>  Wenn der Benutzer alle Location-Anbieter deaktiviert hat `GetBestProvider` zurück `null`. Um anzuzeigen, wie dieser Code auf einem echten Gerät funktioniert, achten Sie darauf, GPS, Wi-Fi und Mobilfunknetze unter ermöglichen **Google-Einstellungen > Position > Modus** wie im folgenden Screenshot gezeigt:

[![Bildschirm "Einstellungen Positionsmodus" auf einem Android-Telefon](location-images/location-02.png)](location-images/location-02.png#lightbox)

Der folgende Screenshot zeigt den Speicherort Anwendung ausgeführten, indem `GetBestProvider`:

[![Anzeigen von Längen- und Breitengrad sowie Anbieter GetBestProvider-app](location-images/location-03.png)](location-images/location-03.png#lightbox)

Beachten Sie, dass `GetBestProvider` wird den Anbieter nicht dynamisch geändert. Stattdessen bestimmt es verfügbaren Anbieter einmal während der aktivitätslebenszyklus. Wenn der Anbieterstatus ändert, nachdem es festgelegt wurde, die Anwendung erfordert zusätzlichen Code in die `ILocationListener` Methoden &ndash; `OnProviderEnabled`, `OnProviderDisabled`, und `OnStatusChanged` &ndash; behandelt jede Möglichkeit, die im Zusammenhang mit der Anbieter-Schalter.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch behandelt die Position des Benutzers mit der Speicherortdienst für Android und die fused-Location-Anbieters von Google Location Services-API abrufen.


## <a name="related-links"></a>Verwandte Links

- [Speicherort (Beispiel)](https://developer.xamarin.com/samples/Location/)
- [FusedLocationProvider (Beispiel)](https://developer.xamarin.com/samples/FusedLocationProvider/)
- [Google Play-Dienste](http://developer.android.com/google/play-services/index.html)
- [Kriterien-Klasse](https://developer.xamarin.com/api/type/Android.Locations.Criteria/)
- [LocationManager-Klasse](https://developer.xamarin.com/api/type/Android.Locations.LocationManager/)
- [LocationListener-Klasse](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/)
- [LocationClient API](http://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener API](http://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
