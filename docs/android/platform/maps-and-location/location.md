---
title: Standortdienste unter Android
description: Dieser Leitfaden bietet eine Einführung in die Standortfunktionen in Android-Anwendungen und veranschaulicht, wie sich über die Standortdienst-API von Android sowie über den in der Standortdienste-API von Google verfügbaren Fused Location Provider der Standort eines Benutzers abrufen lässt.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/22/2018
ms.openlocfilehash: e027d41e98c26ef1659c27ab05df3052e19cc670
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73027137"
---
# <a name="location-services-on-android"></a>Standortdienste unter Android

_Dieser Leitfaden bietet eine Einführung in die Standortfunktionen in Android-Anwendungen und veranschaulicht, wie sich über die Standortdienst-API von Android sowie über den in der Standortdienste-API von Google verfügbaren Fused Location Provider der Standort eines Benutzers abrufen lässt._

Android bietet Zugriff auf verschiedene Technologien zur Standortbestimmung, z. B. auf die Funkturmtriangulation, die WLAN-basierte Ortung sowie die GPS-Ortung. Die Details der einzelnen Ortungstechnologien werden durch *Standortanbieter* abstrahiert, sodass Anwendungen unabhängig vom verwendeten Anbieter auf dieselbe Weise Standorte abrufen können. In diesem Leitfaden wird der Fused Location Provider vorgestellt. Diese Komponente von Google Play Services legt anhand der verfügbaren Anbieter und des Nutzungszwecks des Geräts auf intelligente Weise fest, wie der Standort am besten abgerufen wird. Des Weiteren wird die Standortdienst-API von Android eingeführt und erläutert, wie Sie mithilfe von `LocationManager` mit dem Standortdienst des Systems kommunizieren. Im zweiten Teil des Leitfadens wird die Standortdienst-API von Android mithilfe von `LocationManager` untersucht.

Die allgemeine Faustregel lautet, dass Anwendungen bevorzugt den Fused Location Provider verwenden und nur bei Bedarf auf die ältere Standortdienst-API von Android zurückfallen sollen.

## <a name="location-fundamentals"></a>Grundlagen zu den Standortdiensten

Unabhängig von der API, über die Sie unter Android mit Standortdaten arbeiten, gibt es einige Konzepte, die immer gleich bleiben. In diesem Abschnitt werden Standortanbieter und standortbezogene Berechtigungen vorgestellt.

### <a name="location-providers"></a>Standortanbieter

Intern werden mehrere Technologien verwendet, um den Standort eines Benutzers zu ermitteln. Welche Hardware verwendet wird, hängt vom Typ des *Standortanbieters* ab, der für das Erfassen der Daten ausgewählt wurde. Unter Android werden die folgenden drei Standortanbieter eingesetzt:

- **GPS-Anbieter:** GPS ist die genaueste Methode für die Standortbestimmung, beansprucht die meiste Akkuleistung und funktioniert im Freien am zuverlässigsten. Dieser Anbieter kombiniert normales und unterstütztes GPS ([assisted GPS, aGPS](https://en.wikipedia.org/wiki/Assisted_GPS)). Letzteres gibt die von den Funktürmen erfassten GPS-Daten zurück.

- **Netzwerkanbieter:** Dieser Anbieter kombiniert WLAN- und Mobilfunkdaten, einschließlich der von Funktürmen erfassten aGPS-Daten. Der Netzwerkanbieter verbraucht weniger Strom als der GPS-Anbieter, gibt aber Standortdaten von unterschiedlicher Genauigkeit zurück.

- **Passiver Anbieter:** Diese Option nutzt Anbieter, die von anderen Anwendungen oder Diensten angefordert wurden, um Standortdaten in einer Anwendung zu generieren. Diese weniger zuverlässige, aber energiesparende Option eignet sich ideal für Anwendungen, für die keine konstanten Standortupdates erforderlich sind.

Standortanbieter sind nicht immer verfügbar. Selbst wenn Ihre Anwendung GPS verwenden soll, ist es möglich, dass die GPS-Funktion in den Einstellungen deaktiviert oder das Gerät nicht GPS-fähig ist. Wenn ein bestimmter Anbieter nicht verfügbar ist, wird nach dem Auswählen dieses Anbieters möglicherweise `null` zurückgegeben.

### <a name="location-permissions"></a>Standortberechtigungen

Eine ortungsfähige Anwendung benötigt Zugriff auf die Hardwaresensoren eines Geräts, um GPS-, WLAN- und Mobilfunkdaten zu erhalten. Der Zugriff wird über die entsprechenden Berechtigungen im Android-Manifest der Anwendung gesteuert.
Es sind zwei Berechtigungen verfügbar. In Abhängigkeit von den Anforderungen Ihrer Anwendung und der ausgewählten API sollten Sie eine aktivieren:

- `ACCESS_FINE_LOCATION`: Erteilt einer Anwendung Zugriff auf die GPS-Funktion.
    Diese Berechtigung ist für den *GPS-Anbieter* und den *passiven Anbieter* erforderlich (*der passive Anbieter benötigt eine Berechtigung, um auf GPS-Daten zugreifen zu können, die von einer anderen Anwendung oder einem anderen Dienst gesammelt wurden*). Die Berechtigung ist optional für den *Netzwerkanbieter*.

- `ACCESS_COARSE_LOCATION`: Erteilt einer Anwendung Zugriff auf Mobilfunk- und WLAN-Standortdaten. Die Berechtigung ist für den *Netzwerkanbieter* erforderlich, wenn `ACCESS_FINE_LOCATION` nicht festgelegt ist.

Bei Apps, die auf API-Version 21 (Android 5.0 Lollipop) oder höher ausgerichtet sind, können Sie `ACCESS_FINE_LOCATION` aktivieren und die Apps weiterhin auf Geräten ohne GPS-Hardware ausführen. Wenn Ihre App GPS-Hardware erfordert, sollten Sie dem Android-Manifest explizit ein `android.hardware.location.gps` `uses-feature`-Element hinzufügen. Weitere Informationen finden Sie in der Referenz zum Android-Element [uses-feature](https://developer.android.com/guide/topics/manifest/uses-feature-element.html).

Erweitern Sie zum Festlegen der Berechtigungen den Ordner **Eigenschaften** im **Lösungspad**, und doppelklicken Sie auf **AndroidManifest.xml**. Die Berechtigungen werden unter **Erforderliche Berechtigungen** aufgeführt:

[![Screenshot: erforderliche Berechtigungseinstellungen für das Android-Manifest](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Wenn Sie eine dieser Berechtigungen festlegen, teilen Sie Android dadurch mit, dass Ihre Anwendung die Berechtigung des Benutzers benötigt, um auf die Standortanbieter zugreifen zu können. Auf Geräten, die API-Ebene 22 (Android 5.1) oder niedriger ausführen, werden Benutzer bei jeder Installation der App dazu aufgefordert, diese Berechtigungen zu erteilen. Auf Geräten, auf denen API-Ebene 23 (Android 6.0) oder höher ausgeführt wird, muss die App eine Berechtigungsüberprüfung zur Laufzeit durchführen, bevor Sie eine Anforderung an den Standortanbieter sendet. 

> [!NOTE]
>Hinweis: Die Festlegung von `ACCESS_FINE_LOCATION` impliziert den Zugriff auf grobe und genaue Standortdaten. Sie sollten niemals beide Berechtigungen festlegen müssen, sondern nur die *minimale* Berechtigung, die Ihre App zum Funktionieren benötigt.

Der folgende Codeausschnitt zeigt beispielhaft, wie Sie überprüfen können, ob eine App über die Berechtigung für die Berechtigung `ACCESS_FINE_LOCATION` verfügt:

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

Apps müssen auch auf das Szenario ausgelegt sein, dass der Benutzer keine Berechtigungen erteilt (oder die Berechtigung widerruft), und mit dieser Situation umgehen können. Weitere Informationen zu Laufzeitüberprüfungen von Berechtigungen in Xamarin.Android finden Sie unter [Berechtigungen in Xamarin.Android](~/android/app-fundamentals/permissions.md).

## <a name="using-the-fused-location-provider"></a>Verwenden des Fused Location Providers

Der Fused Location Provider ist die bevorzugte Methode für die Bereitstellung von Standortupdates für Android-Anwendungen auf einem Gerät. Der Grund ist, dass der Standortanbieter effizient zur Laufzeit ausgewählt wird, um die besten Standortinformationen auf batterieeffiziente Weise bereitzustellen. Ein Benutzer, der im Freien unterwegs ist, erhält beispielsweise die zuverlässigsten Standortdaten per GPS. Wenn der Benutzer dann ein Gebäude betritt, wo GPS (wenn überhaupt) nur schlecht funktioniert, kann der Fused Location Provider automatisch zur WLAN-basierten Ortung wechseln, da diese in geschlossenen Räumen zuverlässiger ist.

Die Fused Location Provider-API enthält eine Vielzahl anderer Tools wie Geofencing und Aktivitätsüberwachung, die ortungsfähige Anwendungen optimieren. In diesem Abschnitt geht es um die Grundlagen der Einrichtung von `LocationClient` und von Anbietern sowie um das Abrufen des Benutzerstandorts.

Der Fused Location Provider ist Teil von [Google Play Services](https://developer.android.com/google/play-services/index.html).
Das Google Play Services-Paket muss in der Anwendung installiert und ordnungsgemäß konfiguriert sein, damit die Fused Location Provider-API funktioniert. Außerdem muss auf dem Gerät das Google Play Services-APK installiert sein.

Bevor eine Xamarin.Android-Anwendung den Fused Location Provider verwenden kann, muss sie dem Projekt das Paket **Xamarin.GooglePlayServices.Maps** hinzufügen. Außerdem sollten allen Quelldateien, die auf die nachfolgend beschriebenen Klassen verweisen, die folgenden `using`-Anweisungen hinzugefügt werden:

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Überprüfen der Installation von Google Play Services

Eine Xamarin.Android-Anwendung stürzt ab, wenn sie versucht, den Fused Location Provider zu verwenden, obwohl Google Play Services nicht installiert (oder veraltet) ist. In diesem Fall tritt eine Runtimeausnahme auf.  Wenn Google Play Services nicht installiert ist, sollte die Anwendung ein Fallback auf den zuvor erwähnten Android-Standortdienst durchführen. Wenn die installierte Version von Google Play Services veraltet ist, kann die App dem Benutzer eine Meldung mit der Aufforderung anzeigen, die installierte Version von Google Play Services zu aktualisieren.

Der folgende Codeausschnitt zeigt beispielhaft, wie eine Activity-Klasse unter Android programmgesteuert überprüfen kann, ob Google Play Services installiert ist:

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

Eine Xamarin.Android-Anwendung muss über eine Instanz von `FusedLocationProviderClient` verfügen, um mit dem Fused Location Provider interagieren zu können. Diese Klasse macht die erforderlichen Methoden zum Abonnieren von Standortupdates und zum Abrufen des letzten bekannten Gerätestandorts verfügbar.

Die Methode `OnCreate` der Activity-Klasse ist die geeignete Stelle, um einen Verweis auf `FusedLocationProviderClient` abzurufen. Dies wird im nachfolgenden Codeausschnitt veranschaulicht:

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

### <a name="getting-the-last-known-location"></a>Abrufen des zuletzt bekannten Standorts

Die Methode `FusedLocationProviderClient.GetLastLocationAsync()` bietet eine einfache, nicht blockierende Methode für Xamarin.Android-Anwendungen dar, den letzten bekannten Gerätestandort mit minimalem Programmieraufwand schnell abzurufen.

In diesem Codeausschnitt wird gezeigt, wie Sie mit der Methode `GetLastLocationAsync` den Standort eines Geräts abrufen:

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

### <a name="subscribing-to-location-updates"></a>Abonnieren von Standortupdates

Eine Xamarin.Android-Anwendung kann mithilfe der Methode `FusedLocationProviderClient.RequestLocationUpdatesAsync` ebenfalls Standortupdates beim Fused Location Provider abonnieren. Dies wird im folgenden Codeausschnitt veranschaulicht:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```

Diese Methode nimmt zwei Parameter:

- **`Android.Gms.Location.LocationRequest`** : In `LocationRequest`-Objekten übergeben Xamarin.Android-Anwendungen die Parameter zur Funktionsweise des Fused Location Providers. `LocationRequest` enthält z. B. Informationen dazu, wie häufig Anforderungen gesendet werden sollten oder wie wichtig ein genaues Standortupdate sein soll. Für ein wichtiges Standortupdate verwendet das Gerät beispielsweise GPS und verbraucht folglich auch mehr Akkuleistung beim Bestimmen des Standorts. In diesem Codeausschnitt wird gezeigt, wie Sie ein `LocationRequest`-Objekt für einen Standort mit hoher Genauigkeit erstellen, das etwa alle fünf Minuten auf ein Standortupdate prüft (aber nicht, bevor nicht mindestens zwei Minuten zwischen Anforderungen vergangen sind). Der Fused Location Provider wählt beim Ermitteln des Gerätestandorts anhand eines `LocationRequest`-Objekts den Standortanbieter aus:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```

- **`Android.Gms.Location.LocationCallback`** : Zum Empfangen von Standortupdates muss eine Xamarin.Android-Anwendung eine Unterklasse der abstrakten Klasse `LocationProvider` erstellen. Diese Klasse macht zwei Methoden verfügbar, die vom Fused Location Provider aufgerufen werden können, um die App mit neuen Standortinformationen zu versorgen. Dies wird in später in diesem Artikel ausführlicher erläutert.

Der Fused Location Provider ruft `LocationCallBack.OnLocationResult(LocationResult result)` auf, um ein Standortupdate an eine Xamarin.Android-Anwendung weiterzugeben. Der Parameter `Android.Gms.Location.LocationResult` enthält die Informationen zum aktualisierten Standort.

Wenn der Fused Location Provider eine Änderung bei der Verfügbarkeit von Standortdaten erkennt, wird die Methode `LocationProvider.OnLocationAvailability(LocationAvailability
locationAvailability)` aufgerufen. Wenn die Eigenschaft `LocationAvailability.IsLocationAvailable` den Wert `true` zurückgibt, kann davon ausgegangen werden, dass die von `OnLocationResult` gemeldeten Gerätestandortergebnisse so genau und aktuell sind, wie vom `LocationRequest`-Objekt gefordert. Wenn `IsLocationAvailable` FALSE ist, werden von `OnLocationResult` keine Standortergebnisse zurückgegeben.

Dieser Codeausschnitt zeigt eine Beispielimplementierung des `LocationCallback`-Objekts:

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

## <a name="using-the-android-location-service-api"></a>Verwenden der Standortdienst-API von Android

Der Android-Standortdienst ist eine ältere API für die Verwendung von Standortinformationen unter Android. Standortdaten werden von Hardwaresensoren erfasst und von einem Systemdienst gesammelt, auf den in der Anwendung mit einer `LocationManager`-Klasse und einem `ILocationListener` zugegriffen wird.

Der Standortdienst eignet sich am besten für Anwendungen, die auf Geräten ausgeführt werden müssen, auf denen Google Play Services nicht installiert ist.

Der Standortdienst ist eine besondere Art von [Dienst](https://developer.android.com/guide/components/services.html), der vom System verwaltet wird. Ein Systemdienst interagiert mit der Gerätehardware und wird immer ausgeführt. Damit Ihre Anwendung Standortupdates abrufen kann, müssen Sie Standortsupdates mithilfe eines `LocationManager`- und eines `RequestLocationUpdates`-Aufrufs beim Standortdienst des Systems abonnieren.

Das Abrufen eines Benutzerstandorts mithilfe des Android-Standortdiensts umfasst mehrere Schritte:

1. Rufen Sie einen Verweis auf den `LocationManager`-Dienst ab.
2. Implementieren Sie die Schnittstelle `ILocationListener`, und lassen Sie Ereignisse verarbeiten, wenn sich der Standort ändert.
3. Fordern Sie mit `LocationManager` Standortupdates für einen angegebenen Anbieter an. `ILocationListener` aus dem vorherigen Schritt wird dazu verwendet, Rückrufe von `LocationManager` zu empfangen.
4. Beenden Sie die Standortupdates, wenn die Anwendung nicht mehr für den Empfang von Updates geeignet ist.

### <a name="location-manager"></a>LocationManager

Der Zugriff auf den Standortdienst des Systems ist mit einer Instanz der Klasse `LocationManager` möglich. `LocationManager` ist eine besondere Klasse, die die Interaktion mit dem Standortdienst des Systems und das Aufrufen Methoden für den Standortdienst ermöglicht. Eine Anwendung kann einen Verweis auf `LocationManager` abrufen, indem sie `GetSystemService` aufruft und so wie unten dargestellt einen Diensttyp übergibt:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` ist die geeignete Methode, um einen Verweis auf `LocationManager` abzurufen.
Es empfiehlt sich, `LocationManager` als Klassenvariable beizubehalten, damit Sie sie an verschiedenen Stellen im Activity-Lebenszyklus abrufen können.

### <a name="request-location-updates-from-the-locationmanager"></a>Anfordern von Standortupdates von LocationManager

Sobald die Anwendung über einen Verweis auf `LocationManager`verfügt, muss sie `LocationManager` darüber informieren, welche Art von Standortinformationen erforderlich sind und wie oft diese Informationen aktualisiert werden sollen. Rufen Sie hierzu `RequestLocationUpdates` für das `LocationManager`-Objekt auf, und übergeben Sie einige Updatekriterien und einen Rückruf, der die Standortupdates empfängt. Dieser Rückruf ist ein Typ, der die Schnittstelle `ILocationListener` implementieren muss (wird weiter unten in diesem Leitfaden ausführlicher beschrieben).

Die Methode `RequestLocationUpdates` teilt dem Standortdienst des Systems mit, dass Ihre Anwendung bereit ist, Standortupdates zu empfangen. Mit dieser Methode können Sie den Anbieter sowie Zeit- und Entfernungsschwellenwerte angeben, um die Updatehäufigkeit zu steuern. Die nachfolgende Methode fordert beispielsweise alle 2.000 Millisekunden Standortupdates vom GPS-Standortanbieter an, und zwar nur dann, wenn sich der Standort um mehr als einen Meter ändert:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Eine Anwendung sollte Standortupdates nur so oft anfordern, wie dies für eine gute Anwendungsleistung erforderlich ist. Dies schont die Akkulaufzeit und sorgt für ein besseres Benutzererlebnis.

### <a name="responding-to-updates-from-the-locationmanager"></a>Antworten auf Updates von LocationManager

Nachdem eine Anwendung Updates von `LocationManager` angefordert hat, kann sie Informationen vom Dienst empfangen, indem sie die Schnittstelle [`ILocationListener`](xref:Android.Locations.ILocationListener) implementiert. Diese Schnittstelle stellt vier Methoden für das Lauschen am Standortdienst und am Standortanbieter (`OnLocationChanged`) bereit. Das System ruft `OnLocationChanged` auf, wenn sich der Standort des Benutzers so deutlich ändert, dass gemäß dem Kriterienset für das Anfordern von Standortupdates eine Standortänderung vorliegt. 

Der folgende Code zeigt die Methoden in der Schnittstelle `ILocationListener`:

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

### <a name="unsubscribing-to-locationmanager-updates"></a>Abbestellen von LocationManager-Updates

Zum Einsparen von Systemressourcen sollte eine Anwendung Standortupdates so bald wie möglich abbestellen. Die Methode `RemoveUpdates` informiert `LocationManager` darüber, das keine Updates mehr an Ihre Anwendung gesendet werden sollen.  Ein Activity-Objekt kann `RemoveUpdates` in der Methode `OnPause` aufrufen, damit Akkulaufzeit eingespart werden kann, wenn eine Anwendung keine Standortupdates erfordert, während ihr Activity-Objekt nicht auf dem Display angezeigt wird:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Wenn Ihre Anwendung Standortupdates abrufen muss, während sie im Hintergrund ausgeführt wird, sollten Sie einen benutzerdefinierten Dienst erstellen, der den Standortdienst des Systems abonniert. Weitere Informationen finden Sie im Leitfaden [Erstellen von Android-Diensten](~/android/app-fundamentals/services/index.md).

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Ermitteln des optimalen Standortanbieters für LocationManager

Die zuvor beschriebene Anwendung legt GPS als Standortanbieter fest. Das GPS-Signal ist jedoch möglicherweise nicht in allen Situationen verfügbar, z. B. wenn sich das Gerät in geschlossenen Räumen befindet oder nicht über einen GPS-Empfänger verfügt. Wenn dies der Fall ist, wird `null` für den Anbieter zurückgegeben.

Damit Ihre App auch dann funktioniert, wenn kein GPS-Signal verfügbar ist, müssen Sie mit der Methode `GetBestProvider` beim Anwendungsstart den besten verfügbaren Standortanbieter abrufen (der vom Gerät unterstützt wird und vom Benutzer aktiviert wurde). Anstatt einen bestimmten Anbieter zu übergeben, können Sie in der Methode `GetBestProvider` mit einem [`Criteria`-Objekt](xref:Android.Locations.Criteria) die Anforderungen für den Anbieter angeben, z. B. Genauigkeit und Stromverbrauch. `GetBestProvider` gibt den besten Anbieter für die angegebenen Kriterien zurück.

Der folgende Code zeigt, wie Sie den besten verfügbaren Anbieter abrufen können und ihn bei der Anforderung von Standortupdates verwenden:

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
> Wenn der Benutzer alle Standortanbieter deaktiviert hat, gibt `GetBestProvider` den Wert `null` zurück. Stellen Sie sicher, dass Sie die GPS-, WLAN- und Mobilfunknetzwerk-Funkionen wie in dem folgenden Screenshot gezeigt unter **Google Settings > Location > Mode** (Google-Einstellungen > Standort > Modus) aktiviert haben, um zu sehen, wie der Code auf einem echten Gerät funktioniert:
>
> [![Display mit den Einstellungen für den Standortmodus auf einem Android-Handy](location-images/location-02.png)](location-images/location-02.png#lightbox)
>
> Im folgenden Screenshot wird die Standortanwendung gezeigt, die mit `GetBestProvider` ausgeführt wird:
>
> [![GetBestProvider-App, die den Breitengrad, den Längengrad und den Anbieter anzeigt](location-images/location-03.png)](location-images/location-03.png#lightbox)
>
> Beachten Sie, dass `GetBestProvider` den Anbieter nicht dynamisch ändert. Stattdessen wird der beste verfügbare Anbieter einmal während des Activity-Lebenszyklus bestimmt. Wenn sich der Anbieterstatus ändert, nachdem er festgelegt wurde, benötigt die Anwendung zusätzlichen Code in den Methoden von `ILocationListener` (`OnProviderEnabled`, `OnProviderDisabled` und `OnStatusChanged`), um auf jedes mögliche Szenario im Zusammenhang mit dem Anbieterwechsel vorbereitet zu sein.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde das Abrufen von Benutzerstandorten mit dem Android-Standortdienst und dem Fused Location Provider aus der Standortdienste-API von Google behandelt.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Android – Beispiel für Android-Standortdienste](https://docs.microsoft.com/samples/xamarin/monodroid-samples/location)
- [Xamarin.Android – Beispiel für FusedLocationProvider](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fusedlocationprovider)
- [Überblick über Google Play Services](https://developer.android.com/google/play-services/index.html)
- [Criteria-Klasse](xref:Android.Locations.Criteria)
- [LocationManager-Klasse](xref:Android.Locations.LocationManager)
- [ILocationListener-Schnittstelle](xref:Android.Locations.ILocationListener)
- [LocationClient-API](https://developer.android.com/reference/com/google/android/gms/location/LocationClient.html)
- [LocationListener-API](https://developer.android.com/reference/com/google/android/gms/location/LocationListener.html)
- [LocationRequest-API](https://developer.android.com/reference/com/google/android/gms/location/LocationRequest.html)
