---
title: Location Services unter Android
description: In diesem Leitfaden wird das Standort Bewusstsein in Android-Anwendungen vorgestellt, und es wird veranschaulicht, wie Sie den Speicherort des Benutzers mithilfe der Android Location Service-API und des mit dem Google-Ortungsdienste-API verfügbaren Dienstanbieters für den Fused-Standort
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: e027d41e98c26ef1659c27ab05df3052e19cc670
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027137"
---
# <a name="location-services-on-android"></a>Location Services unter Android

_In diesem Leitfaden wird das Standort Bewusstsein in Android-Anwendungen vorgestellt, und es wird veranschaulicht, wie Sie den Speicherort des Benutzers mithilfe der Android Location Service-API und des mit dem Google-Ortungsdienste-API verfügbaren Dienstanbieters für den Fused-Standort_

Android ermöglicht den Zugriff auf verschiedene Location-Technologien, wie z. b. Cell Tower Location, Wi-Fi und GPS. Die Details der einzelnen Standorttechnologien werden durch *standortanbieter*abstrahiert, sodass Anwendungen unabhängig vom verwendeten Anbieter auf dieselbe Weise Standorte abrufen können. In diesem Leitfaden wird der Anbieter für den zusammen gezierten Standort vorgestellt, ein Teil des Google Play Services, der Intelligent festlegt, welche Art von Geräten Sie basierend auf den verfügbaren Anbietern und wie das Gerät verwendet wird. Android Location Service-API und zeigt, wie mit dem System Location Service mithilfe eines `LocationManager`kommuniziert werden kann. Im zweiten Teil des Handbuchs werden die Android-Ortungsdienste-API mithilfe der `LocationManager`untersucht.

Als allgemeine Faustregel empfiehlt es sich, den Anbieter für die Verbindung mit der Anwendung zu verwenden. dabei wird die ältere Android Location Service-API nur bei Bedarf zurückfallen.

## <a name="location-fundamentals"></a>Grundlagen des Orts

Unabhängig von der API, die Sie für die Arbeit mit Standortdaten ausgewählt haben, bleiben verschiedene Konzepte gleich. In diesem Abschnitt werden standortanbieter und standortbezogene Berechtigungen vorgestellt.

### <a name="location-providers"></a>Location-Anbieter

Mehrere Technologien werden intern verwendet, um den Speicherort des Benutzers zu ermitteln. Welche Hardware verwendet wird, hängt vom Typ des *Orts Anbieters* ab, der für den Auftrag zum Sammeln von Daten ausgewählt wurde. Android verwendet drei Speicherort Anbieter:

- **GPS-Anbieter** &ndash; GPS gibt den genauesten Speicherort an, beansprucht die meiste Stromversorgung und funktioniert am besten im Außenbereich. Dieser Anbieter verwendet eine Kombination aus GPS und unterstütztem GPS ([aGPS](https://en.wikipedia.org/wiki/Assisted_GPS)), die GPS-Daten zurückgibt, die von Mobil Funktürmen gesammelt werden.

- Der **Netzwerkanbieter** &ndash; bietet eine Kombination aus WiFi-und Mobilfunk-Daten, einschließlich aGPS-Daten, die von zelltürmen gesammelt werden. Es verwendet weniger Energie als der GPS-Anbieter, gibt aber Positionsdaten mit abweichender Genauigkeit zurück.

- **Passiver Anbieter** &ndash; eine Option "Piggyback" mithilfe von Anbietern, die von anderen Anwendungen oder Diensten angefordert werden, um Standortdaten in einer Anwendung zu generieren. Diese Option ist weniger zuverlässig, aber die Energiespar Option eignet sich ideal für Anwendungen, für die keine Konstanten Speicherort Aktualisierungen erforderlich sind.

Standortanbieter sind nicht immer verfügbar. Beispielsweise können wir GPS für unsere Anwendung verwenden, aber GPS ist möglicherweise in den Einstellungen ausgeschaltet, oder das Gerät verfügt möglicherweise nicht über GPS. Wenn ein bestimmter Anbieter nicht verfügbar ist, wird bei Auswahl dieses Anbieters möglicherweise `null`zurückgegeben.

### <a name="location-permissions"></a>Speicherort Berechtigungen

Eine Standort orientierte Anwendung benötigt Zugriff auf die Hardware Sensoren eines Geräts, um GPS-, Wi-Fi-und Mobilfunk-Daten zu erhalten. Der Zugriff wird über die entsprechenden Berechtigungen im Android-Manifest der Anwendung gesteuert.
Es sind zwei Berechtigungen verfügbar &ndash; je nach den Anforderungen Ihrer Anwendung und ihrer Auswahl der API sollten Sie Folgendes zulassen:

- `ACCESS_FINE_LOCATION` &ndash; ermöglicht einer Anwendung den Zugriff auf GPS.
    Erforderlich für den *GPS-Anbieter* und *passive Anbieter* Optionen (*passiver Anbieter benötigt Zugriffsberechtigungen für GPS-Daten, die von einer anderen Anwendung oder einem anderen Dienst gesammelt werden*). Optionale Berechtigung für den *Netzwerkanbieter*.

- `ACCESS_COARSE_LOCATION` &ndash; ermöglicht einem Anwendungs Zugriff auf Mobilfunk-und WLAN-Speicherort. Erforderlich für den *Netzwerkanbieter* , wenn `ACCESS_FINE_LOCATION` nicht festgelegt ist.

Für apps, die auf API-Version 21 (Android 5,0 Lollipop) oder höher ausgerichtet sind, können Sie `ACCESS_FINE_LOCATION` aktivieren und weiterhin auf Geräten ohne GPS-Hardware ausgeführt werden. Wenn Ihre APP GPS-Hardware erfordert, sollten Sie dem Android-manifest explizit ein `android.hardware.location.gps` `uses-feature`-Element hinzufügen. Weitere Informationen finden Sie in der Referenz zum [Funktions](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) Element von Android.

Um die Berechtigungen festzulegen, erweitern Sie den Ordner **Properties** im **Lösungspad** , und doppelklicken Sie auf **androidmanifest. XML**. Die Berechtigungen werden unter **erforderliche Berechtigungen**aufgeführt:

[![Screenshot der erforderlichen Berechtigungseinstellungen für das Android-Manifest](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Wenn Sie eine dieser Berechtigungen festlegen, wird Android mitgeteilt, dass Ihre Anwendung die Berechtigung für den Benutzer benötigt, damit Sie auf die standortanbieter zugreifen können. Bei Geräten, auf denen API Level 22 (Android 5,1) oder niedriger ausgeführt wird, wird der Benutzer aufgefordert, diese Berechtigungen bei jeder Installation der APP zu erteilen. Auf Geräten, auf denen API-Ebene 23 (Android 6,0) oder höher ausgeführt wird, sollte die APP eine Lauf Zeit Berechtigungsüberprüfung durchführen, bevor Sie eine Anforderung an den standortanbieter sendet. 

> [!NOTE]
>Hinweis: durch das Festlegen von `ACCESS_FINE_LOCATION` wird der Zugriff auf grobe und feine Positionsdaten impliziert. Sie sollten niemals beide Berechtigungen festlegen, sondern nur die *minimale* Berechtigung, die Ihre APP benötigt, um zu funktionieren.

Dieser Code Ausschnitt ist ein Beispiel dafür, wie Sie überprüfen, ob eine APP über Berechtigungen für die `ACCESS_FINE_LOCATION`-Berechtigung verfügt:

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

Apps müssen für das Szenario tolerant sein, in dem der Benutzer keine Berechtigungen erteilt (oder die Berechtigung widerrufen hat) und eine Möglichkeit hat, diese Situation ordnungsgemäß zu behandeln. Weitere Informationen zum Implementieren von Lauf Zeit Berechtigungs Überprüfungen in xamarin. Android finden Sie im [Berechtigungs Handbuch](~/android/app-fundamentals/permissions.md) .

## <a name="using-the-fused-location-provider"></a>Verwenden des Anbieters für die Zwischenspeicher Orte

Der Anbieter für die standortübergreifende Bereitstellung ist die bevorzugte Methode für Android-Anwendungen, um Standort Updates vom Gerät zu empfangen, da der Anbieter für den Standort während der Laufzeit effizient ausgewählt wird, um die optimalen Standortinformationen auf Akku-effiziente Weise bereitzustellen. Beispielsweise erhält ein Benutzer, der die Umgebung durchläuft, den besten Ort, der sich mit GPS ausliest. Wenn der Benutzer dann in den einzelnen Bereichen wechselt, in denen GPS schlecht funktioniert (wenn überhaupt), wechselt der Anbieter für den zusammen gezierten Standort möglicherweise automatisch zu WiFi, was besser im Inneren funktioniert.

Die Anbieter-API für den Fused-Speicherort bietet eine Vielzahl anderer Tools, mit denen standortabhängige Anwendungen wie Geofencing und Aktivitäts Überwachung unterstützt werden können. In diesem Abschnitt befassen wir uns mit den Grundlagen der Einrichtung der `LocationClient`, dem Einrichten von Anbietern und der Beschaffung des Benutzer Standorts.

Der Anbieter für den Fused-Speicherort ist Teil [Google Play Services](https://developer.android.com/google/play-services/index.html).
Das Google Play Services Paket muss in der Anwendung installiert und ordnungsgemäß konfiguriert sein, damit die Anbieter-API für den Fused Location funktioniert, und auf dem Gerät muss das Google Play Services APK installiert sein.

Bevor eine xamarin. Android-Anwendung den Anbieter für den Fused-Speicherort verwenden kann, muss Sie dem Projekt das **xamarin. googleplayservices. Maps** -Paket hinzufügen. Außerdem sollten den Quelldateien, die auf die unten beschriebenen Klassen verweisen, die folgenden `using`-Anweisungen hinzugefügt werden:

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Überprüfen, ob Google Play Services installiert ist

Ein xamarin. Android stürzt ab, wenn versucht wird, den Anbieter für den Fused-Speicherort zu verwenden, wenn Google Play Services nicht installiert (oder veraltet) ist, dann tritt eine Lauf Zeit Ausnahme auf.  Wenn Google Play Services nicht installiert ist, sollte die Anwendung auf den zuvor beschriebenen Android Location Service zurückgreifen. Wenn Google Play Services veraltet ist, könnte die APP dem Benutzer eine Meldung anzeigen, in der Sie aufgefordert werden, die installierte Version von Google Play Services zu aktualisieren.

Dieser Code Ausschnitt ist ein Beispiel dafür, wie eine Android-Aktivität Programm gesteuert überprüfen kann, ob Google Play Services installiert ist:

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

### <a name="fusedlocationproviderclient"></a>Fusedlocationproviderclient

Eine xamarin. Android-Anwendung muss über eine Instanz des `FusedLocationProviderClient`verfügen, um mit dem Anbieter für die Zusammenstellung von Adressen interagieren zu können. Diese Klasse macht die erforderlichen Methoden zum Abonnieren von Standort Aktualisierungen und zum Abrufen des letzten bekannten Speicher Orts des Geräts verfügbar.

Die `OnCreate`-Methode einer Aktivität ist ein geeigneter Speicherort, um einen Verweis auf die `FusedLocationProviderClient`zu erhalten, wie im folgenden Code Ausschnitt gezeigt:

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

### <a name="getting-the-last-known-location"></a>Der letzte bekannte Speicherort wird erhalten.

Die `FusedLocationProviderClient.GetLastLocationAsync()`-Methode bietet eine einfache, nicht blockierende Methode, mit der eine xamarin. Android-Anwendung den letzten bekannten Speicherort des Geräts mit minimalem Programmieraufwand schnell abrufen kann.

In diesem Code Ausschnitt wird gezeigt, wie Sie die `GetLastLocationAsync`-Methode verwenden, um den Speicherort des Geräts abzurufen:

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

### <a name="subscribing-to-location-updates"></a>Abonnieren von Standort Aktualisierungen

Eine xamarin. Android-Anwendung kann auch Location-Updates vom Anbieter für verknüpfter Speicherorte abonnieren, indem Sie die `FusedLocationProviderClient.RequestLocationUpdatesAsync`-Methode verwendet, wie im folgenden Code Ausschnitt gezeigt:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

Diese Methode nimmt zwei Parameter an:

- **`Android.Gms.Location.LocationRequest`** &ndash; ein `LocationRequest` Objekt gibt an, wie eine xamarin. Android-Anwendung die Parameter für die Funktionsweise des Dienstanbieters für den Zusammenführungen übergibt. Der `LocationRequest` enthält Informationen, z. b. wie häufig Anforderungen erfolgen sollten oder wie wichtig eine genaue Speicherort Aktualisierung sein sollte. Beispielsweise bewirkt eine wichtige Standort Anforderung, dass das Gerät das GPS verwendet und folglich mehr Energie, wenn der Speicherort bestimmt wird. In diesem Code Ausschnitt wird gezeigt, wie Sie eine `LocationRequest` für einen Speicherort mit hoher Genauigkeit erstellen und etwa alle fünf Minuten auf eine Standort Aktualisierung überprüfen (aber nicht früher als zwei Minuten zwischen Anforderungen). Der Anbieter für die Verbindungs Speicherort verwendet einen `LocationRequest` als Leitfaden für den standortanbieter, der beim Ermitteln des Gerätespeicher Orts verwendet werden soll:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- **`Android.Gms.Location.LocationCallback`** &ndash;, um Speicherort Aktualisierungen zu erhalten, muss eine xamarin. Android-Anwendung die `LocationProvider` abstrakte Klasse Unterklassen. Diese Klasse stellt zwei Methoden zur Verfügung, die möglicherweise vom Anbieter für den Fused-Location aufgerufen werden, um die APP mit Standortinformationen zu aktualisieren Dies wird im folgenden ausführlicher erläutert.

Um eine xamarin. Android-Anwendung über eine Speicherort Aktualisierung zu benachrichtigen, ruft der Anbieter für den Fused-Ort den `LocationCallBack.OnLocationResult(LocationResult result)`auf. Der `Android.Gms.Location.LocationResult`-Parameter enthält die Informationen zum Update Speicherort.

Wenn der Anbieter für den Fused-Speicherort eine Änderung der Verfügbarkeit von Standortdaten erkennt, wird die `LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)`-Methode aufgerufen. Wenn die `LocationAvailability.IsLocationAvailable`-Eigenschaft `true`zurückgibt, kann davon ausgegangen werden, dass die von `OnLocationResult` gemeldeten Geräte Speicherort Ergebnisse so genau und aktuell wie für die `LocationRequest`erforderlich sind. Wenn `IsLocationAvailable` false ist, werden von `OnLocationResult`keine Speicherort Ergebnisse zurückgegeben.

Dieser Code Ausschnitt ist eine Beispiel Implementierung des `LocationCallback` Objekts:

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

## <a name="using-the-android-location-service-api"></a>Verwenden der Android Location Service-API

Der Android Location Service ist eine ältere API für die Verwendung von Standortinformationen unter Android. Standortdaten werden von Hardware Sensoren gesammelt und von einem-Systemdienst gesammelt, auf den in der Anwendung mit einer `LocationManager`-Klasse und einem-`ILocationListener`zugegriffen wird.

Der Location-Dienst eignet sich am besten für Anwendungen, die auf Geräten ausgeführt werden müssen, auf denen Google Play Services nicht installiert ist.

Der Location-Dienst ist ein spezieller [Diensttyp](https://developer.android.com/guide/components/services.html) , der vom System verwaltet wird. Ein-System Dienst interagiert mit der Gerätehardware und wird immer ausgeführt. Zum Abrufen von Location-Updates in unserer Anwendung abonnieren wir Standort Updates vom System Location Service mithilfe eines `LocationManager` und eines `RequestLocationUpdates`-Aufrufes.

Um den Speicherort des Benutzers mithilfe von Android Location Service abzurufen, sind mehrere Schritte erforderlich:

1. Sie erhalten einen Verweis auf den `LocationManager`-Dienst.
2. Implementieren der `ILocationListener`-Schnittstelle und behandeln von Ereignissen, wenn sich der Speicherort ändert.
3. Verwenden Sie den `LocationManager`, um Location-Updates für einen angegebenen Anbieter anzufordern. Der `ILocationListener` aus dem vorherigen Schritt wird verwendet, um Rückrufe vom `LocationManager`zu empfangen.
4. Beenden Sie Standort Aktualisierungen, wenn die Anwendung nicht mehr für den Empfang von Updates geeignet ist.

### <a name="location-manager"></a>Location Manager

Wir können auf den System Location-Dienst mit einer Instanz der `LocationManager`-Klasse zugreifen. `LocationManager` ist eine spezielle Klasse, mit der wir mit dem System Positions Dienst interagieren und Methoden für ihn abrufen können. Eine Anwendung kann einen Verweis auf die `LocationManager` abrufen, indem Sie `GetSystemService` aufgerufen und einen Diensttyp übergibt, wie unten dargestellt:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` ist ein guter Ort, um einen Verweis auf die `LocationManager`zu erhalten.
Es empfiehlt sich, die `LocationManager` als Klassen Variable beizubehalten, damit wir Sie an verschiedenen Punkten im Aktivitäts Lebenszyklus abrufen können.

### <a name="request-location-updates-from-the-locationmanager"></a>Anfordern von Standort Aktualisierungen vom Locationmanager

Wenn die Anwendung über einen Verweis auf die `LocationManager`verfügt, muss Sie die `LocationManager` angeben, welche Art von Standortinformationen erforderlich sind und wie oft diese Informationen aktualisiert werden sollen. Rufen Sie hierzu `RequestLocationUpdates` für das `LocationManager`-Objekt auf, und übergeben Sie einige Kriterien für Updates und einen Rückruf, der die Speicherort Aktualisierungen empfängt. Dieser Rückruf ist ein Typ, der die `ILocationListener`-Schnittstelle implementieren muss (wird weiter unten in diesem Handbuch ausführlicher beschrieben).

Die `RequestLocationUpdates`-Methode teilt dem System Positions Dienst mit, dass Ihre Anwendung mit dem empfangen von Standort Aktualisierungen beginnen soll. Mit dieser Methode können Sie den Anbieter sowie Zeit-und Entfernungs Schwellenwerte angeben, um die Aktualisierungshäufigkeit zu steuern. Beispielsweise fordert die nachfolgende Methode alle 2000 Millisekunden Standort Aktualisierungen vom GPS-Speicherort Anbieter an, und zwar nur dann, wenn sich der Standort um mehr als 1 Meter ändert:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Eine Anwendung sollte Standort Aktualisierungen nur so oft anfordern, wie dies für die Ausführung der Anwendung erforderlich ist. Dies behält die Akku Lebensdauer bei und sorgt für eine bessere Benutzer Leistung.

### <a name="responding-to-updates-from-the-locationmanager"></a>Reagieren auf Updates vom Locationmanager

Nachdem eine Anwendung Updates vom `LocationManager`angefordert hat, kann Sie Informationen vom Dienst empfangen, indem Sie die [`ILocationListener`](xref:Android.Locations.ILocationListener) -Schnittstelle implementiert. Diese Schnittstelle bietet vier Methoden zum lauschen am Location-Dienst und dem Orts Anbieter `OnLocationChanged`. Das System ruft `OnLocationChanged` auf, wenn der Speicherort des Benutzers so geändert wird, dass es sich je nach den Kriterien, die beim Anfordern von Standort Aktualisierungen festgelegt wurden, als Standortänderung qualifiziert 

Der folgende Code zeigt die Methoden in der `ILocationListener`-Schnittstelle:

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

### <a name="unsubscribing-to-locationmanager-updates"></a>Kündigen von Locationmanager-Updates

Um Systemressourcen zu sparen, sollte eine Anwendung so bald wie möglich Updates für Standort Updates kündigen. Die `RemoveUpdates`-Methode weist das `LocationManager` an, keine Updates mehr an die Anwendung zu senden.  Beispielsweise kann eine Aktivität `RemoveUpdates` in der `OnPause`-Methode anrufen, damit wir Energie sparen können, wenn eine Anwendung keine Standort Aktualisierungen benötigt, während sich Ihre Aktivität nicht auf dem Bildschirm befindet:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Wenn Ihre Anwendung im Hintergrund Speicherort Updates erhalten muss, sollten Sie einen benutzerdefinierten Dienst erstellen, der den System Location Service abonniert. Weitere Informationen finden Sie im Handbuch für die [backerden mit Android-Diensten](~/android/app-fundamentals/services/index.md) .

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Ermitteln des optimalen Orts Anbieters für Locationmanager

Die obige Anwendung legt GPS als Speicherort Anbieter fest. GPS ist jedoch möglicherweise nicht in allen Fällen verfügbar, z. b. wenn das Gerät im Inneren oder nicht über einen GPS-Empfänger verfügt. Wenn dies der Fall ist, ist das Ergebnis eine `null` Rückgabe für den Anbieter.

Damit Ihre APP funktioniert, wenn GPS nicht verfügbar ist, verwenden Sie die `GetBestProvider`-Methode, um beim Anwendungsstart den am besten verfügbaren (Geräte gestützten und Benutzer fähigen) Speicherort Anbieter anzufordern. Anstatt einen bestimmten Anbieter zu übergeben, können Sie `GetBestProvider` die Anforderungen für den Anbieter angeben, z. b. Genauigkeit und Stromversorgung mit einem [`Criteria`-Objekt](xref:Android.Locations.Criteria). `GetBestProvider` gibt den besten Anbieter für die angegebenen Kriterien zurück.

Der folgende Code zeigt, wie Sie den besten verfügbaren Anbieter abrufen und bei der Anforderung von Standort Aktualisierungen verwenden können:

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
> Wenn der Benutzer alle standortanbieter deaktiviert hat, wird `GetBestProvider` `null`zurückgeben. Um zu sehen, wie dieser Code auf einem echten Gerät funktioniert, stellen Sie sicher, dass Sie GPS, Wi-Fi und Mobilfunk-Netzwerke unter **Google Settings > Location >-Modus** aktivieren, wie in diesem Screenshot gezeigt:
>
> [Bildschirm "Speicherort Modus![Einstellungen" auf einem Android-Telefon](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> Der folgende Screenshot zeigt die Standort Anwendung, die mit `GetBestProvider`ausgeführt wird:
>
> [![getbestprovider-APP, die Breitengrad, Längengrad und Anbieter anzeigt](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> Beachten Sie, dass `GetBestProvider` den Anbieter nicht dynamisch ändert. Stattdessen wird der am besten verfügbare Anbieter während des Aktivitäts Lebenszyklus einmal bestimmt. Wenn sich der Anbieter Status ändert, nachdem er festgelegt wurde, benötigt die Anwendung zusätzlichen Code in den `ILocationListener` Methoden &ndash; `OnProviderEnabled`, `OnProviderDisabled`und `OnStatusChanged` &ndash;, um jede Möglichkeit im Zusammenhang mit dem Anbieterwechsel zu verarbeiten.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde das Abrufen des Speicher Orts des Benutzers mit dem Android-Speicherort Dienst und dem Anbieter für den Fused-Ort von Google Ortungsdienste-API behandelt.

## <a name="related-links"></a>Verwandte Links

- [Speicherort (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/location)
- [Fusedlocationprovider (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Google Play Services](https://developer.android.com/google/play-services/index.html)
- [Criteria-Klasse](xref:Android.Locations.Criteria)
- [Locationmanager-Klasse](xref:Android.Locations.LocationManager)
- [Locationlistener-Klasse](xref:Android.Locations.ILocationListener)
- [Locationclient-API](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [Locationlistener-API](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [Locationrequest-API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
