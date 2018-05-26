---
title: Ortsbezogene Dienste
description: Dieses Handbuch stellt Netzwerkadressinformationen in Android-Anwendungen, und es wird veranschaulicht, wie der Standort des Benutzers verwenden die Android-API der Speicherort sowie die verfügbaren Speicherortanbieter mit mit der Google-Speicherort-Dienste-API zu erhalten.
ms.prod: xamarin
ms.assetid: 0008682B-6CEF-0C1D-3200-56ECF58F5D3C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 05/22/2018
ms.openlocfilehash: b509f6892b27afa053a6ee913826d913d7ad54a8
ms.sourcegitcommit: 4f646dc5c51db975b2936169547d625c78a22b30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/25/2018
---
# <a name="location-services"></a>Ortsbezogene Dienste

_Dieses Handbuch stellt Netzwerkadressinformationen in Android-Anwendungen, und es wird veranschaulicht, wie der Standort des Benutzers verwenden die Android-API der Speicherort sowie die verfügbaren Speicherortanbieter mit mit der Google-Speicherort-Dienste-API zu erhalten._

## <a name="location-services-overview"></a>Speicherort (Übersicht)

Android bietet Zugriff auf verschiedene Speicherort-Technologien wie Zelle Tower Speicherort Wi-Fi und GPS. Die Details der jeweiligen Technologie Speicherort über abstrahiert werden *Speicherort Anbieter*, Speicherorte auf die gleiche Weise, unabhängig von den verwendeten Anbieter Abrufen von Anwendungen gestatten. Dieses Handbuch enthält, den mit Standort-Anbieter einen Teil der Google Play-Dienste, die auf intelligente Weise bestimmt die beste Möglichkeit zum Abrufen des Speicherort der Geräte basierend auf welche Anbieter verfügbar sind und wie das Gerät verwendet wird. Android-API-Dienst von Standort und gezeigt, wie für die Kommunikation mit dem Speicherort im Dateisystem-Dienst mithilfe einer `LocationManager`. Der zweite Teil des Handbuchs untersucht die Android-API-Dienste der Speicherort mithilfe der `LocationManager`.
 
Als allgemeine Faustregel sollten Anwendungen mit Sicherung Speicherortanbieter, der älteren Android Speicherort Dienst-API nur bei Bedarf für Fallback verwenden möchten.

## <a name="location-fundamentals"></a>Speicherort-Grundlagen

Android-Geräten keine Rolle spielt, welche API, die Sie auswählen, für die Arbeit mit Standortdaten, einige Konzepte bleiben unverändert. Dieser Abschnitt enthält Speicherort Anbieter und entsprechende Berechtigungen.

### <a name="location-providers"></a>Speicherort-Anbieter

Mehrere Technologien werden intern verwendet, um den Standort des Benutzers zu ermitteln. Die Hardware verwendet, hängt vom Typ der *Speicherortanbieter* für den Auftrag des Sammelns von Daten ausgewählt. Android verwendet drei Speicherort Anbieter:

-   **GPS-Anbieter** &ndash; GPS erhält die genauesten Speicherort, verwendet die meisten Power und funktioniert am besten im freien. Dieser Anbieter verwendet eine Kombination von GPS und unterstützte GPS ([aGPS](http://en.wikipedia.org/wiki/Assisted_GPS)), womit GPS-Daten vom Mobiltelefon Towers gesammelt.

-   **Netzwerk-Anbieter** &ndash; bietet eine Kombination von WiFi und Mobilfunk-Daten, einschließlich der Zelle Towers erhobene aGPS-Daten. Weniger Energie als der GPS-Anbieter verwendet, aber gibt die Standortdaten von unterschiedlichen Genauigkeit zurück.

-   **Passive Anbieter** &ndash; eine "piggyback" Option mithilfe von Anbietern, die von anderen Anwendungen oder Dienste angeforderte um Standortdaten in einer Anwendung zu generieren. Dies ist eine weniger zuverlässige jedoch energiesparende Option Ideal für Anwendungen, die keine Konstante Speicherort Updates zu erfordern.

Speicherort Anbieter sind nicht immer verfügbar. Wir möglicherweise GPS für die Anwendung verwenden möchten, aber GPS kann in den Einstellungen deaktiviert oder das Gerät möglicherweise keinen GPS überhaupt. Wenn Sie ein bestimmten Anbieter nicht verfügbar ist, Auswählen dieses Anbieters möglicherweise zurück `null`.

### <a name="location-permissions"></a>Speicherort-Berechtigungen

Eine Anwendung mit Standortbestimmung muss ein Gerät Hardware Sensoren zum Empfangen von GPS-, WLAN- und mobilfunkdaten zugreifen. Der Zugriff wird über die entsprechenden Berechtigungen in der Anwendungsverzeichnis Android Manifest gesteuert.
Es gibt zwei Berechtigungen verfügbar &ndash; je nach den Anforderungen der Anwendung und Ihrer Wahl der API, soll eine zulassen:

-   `ACCESS_FINE_LOCATION` &ndash; Ermöglicht einer Anwendung auf GPS an.
    Erforderlich für die *GPS-Anbieter* und *passiven Anbieter* Optionen (*passiven Anbieter benötigt die Berechtigung zum Datenzugriff GPS von einer anderen Anwendung oder den Dienst gesammelten*). Optional über die Berechtigung für die *Netzwerkanbieter*.

-   `ACCESS_COARSE_LOCATION` &ndash; Können einen Anwendungszugriff auf Mobiltelefon und Wi-Fi-Speicherort. Erforderlich für *Netzwerkanbieter* Wenn `ACCESS_FINE_LOCATION` ist nicht festgelegt.

Für apps, die als API 21 (Android 5.0 Lollipop)-Version Ziel oder höher können Sie aktivieren `ACCESS_FINE_LOCATION` und auf Geräten, die über keine GPS-Hardware weiterhin ausgeführt. Wenn Ihre app GPS-Hardware benötigt, sollten Sie explizit Hinzufügen einer `android.hardware.location.gps` `uses-feature` Element, das Android-Manifest. Weitere Informationen finden Sie in der Android [verwendet-Feature](https://developer.android.com/guide/topics/manifest/uses-feature-element.html) Elementverweis.

Um die Berechtigungen festgelegt haben, erweitern Sie die **Eigenschaften** Ordner in der **Lösung Pad** und doppelklicken Sie auf **AndroidManifest.xml**. Die Berechtigungen aufgelistet **Required Permissions**:

[![Screenshot der Einstellungen für Android-Manifest erforderliche Berechtigungen](location-images/location-01-xs.png)](location-images/location-01-xs.png#lightbox)

Durch Festlegen dieser Berechtigungen weist Android an, dass Ihre Anwendung die Erlaubnis des Benutzers, um den Zugriff auf die Ort-Anbieter erforderlich. Geräte, die API-Ebene 22 (Android 5.1) ausführen oder niedriger fordert den Benutzer zum gewähren dieser Berechtigungen jedes Mal die app installiert ist. Auf Geräten mit-API-Ebene 23 (Android 6.0) oder höher, sollten die app eine Laufzeit-berechtigungsüberprüfung, bevor eine Anforderung des Anbieters Speicherort ausführen. 

> [!NOTE]
>Hinweis: Einstellung `ACCESS_FINE_LOCATION` Zugriff auf beide grob und gut Standortdaten impliziert. Sollte nie festgelegt werden müssen, beide Berechtigungen nur die *minimale* Berechtigung, die Ihre app benötigt, um zu arbeiten.

Dieser Codeausschnitt ist ein Beispiel zum Überprüfen Sie, ob eine app-Berechtigung für die `ACCESS_FINE_LOCATION` Berechtigung:

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

Apps müssen werden fehlertolerante des Szenarios, in denen der Benutzer wird die Berechtigung nicht gewährt (oder die Berechtigung wurde widerrufen) und haben eine Möglichkeit, dieser Situation ordnungsgemäß zu verarbeiten. Finden Sie unter der [Berechtigungen Handbuch](~/android/app-fundamentals/permissions.md) prüft, ob weitere Informationen zum Implementieren der Berechtigung zur Laufzeit in Xamarin.Android.


## <a name="using-the-fused-location-provider"></a>Mithilfe des Anbieters mit Sicherung Speicherort

Der mit Standort-Anbieter ist die bevorzugte Methode für Android-Anwendungen zum Speicherort Updates vom Gerät empfangen werden, weil sie den Speicherortanbieter während der Laufzeit, um die beste Speicherortinformationen auf Akku effiziente Weise bereitzustellen effizient ausgewählt werden. Beispielsweise ruft ein Benutzer durchlaufen, um im freien den besten Ort lesen mit GPS ab. Wenn der Benutzer in geschlossener Umgebung durchläuft, in dem GPS arbeitet schlecht (wenn überhaupt), der Speicherortanbieter mit Sicherung möglicherweise automatisch zur WiFi, welche Works geschlossenen Räume besser wechseln.
 
Der mit Standort-Anbieter-API bietet eine Vielzahl von anderen Tools mit Standortbestimmung Anwendungen, einschließlich Geofencing und aktivitätsüberwachung zu ergänzen. In diesem Abschnitt werden wir den Fokus auf die Grundlagen der Einrichtung der `LocationClient`, Einrichten von Anbietern und vom Standort des Benutzers abrufen.

Der mit Standort-Anbieter ist Teil des [Google Play-Dienste](http://developer.android.com/google/play-services/index.html).
Das Google Play-Dienste-Paket muss installiert und ordnungsgemäß konfiguriert, in der Anwendung für den Speicherortanbieter mit Sicherung API zu arbeiten, und das Gerät muss die Google wiedergeben Services APK installiert haben.

Vor einem Xamarin.Android Anwendung kann den Speicherortanbieter mit Sicherung verwenden, es muss Hinzufügen der **Xamarin.GooglePlayServices.Maps** Paket zum Projekt. Zudem können die folgenden `using` Anweisungen, die die Klassen, die unten beschriebenen verweisen Quelldateien hinzugefügt werden sollen:

```csharp
using Android.Gms.Common;
using Android.Gms.Location;
```

### <a name="checking-if-google-play-services-is-installed"></a>Überprüft, ob es sich bei Google Play-Dienste installiert ist

Eine Xamarin.Android wird einem Absturz, wenn versucht wird, der mit Standort-Anbieter verwenden, wenn Sie Google Play-Dienste nicht installiert ist (oder nicht mehr aktuell) und dann eine Laufzeitausnahme kommt.  Google Play-Dienste nicht installiert ist, sollte die Anwendung wieder an den Android Speicherortdienst weiter oben erläuterten fallen. Wenn Google Play-Dienste nicht mehr aktuell ist, konnte die app eine Nachricht an den Benutzer bitten, aktualisieren Sie die installierte Version von Google Play-Dienste anzeigen.

Dieser Codeausschnitt ist ein Beispiel, wie eine Aktivität Android programmgesteuert überprüfen können, falls es sich bei Google Play-Dienste installiert ist:

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

Für die Interaktion mit dem Speicherortanbieter mit Sicherung benötigen eine Xamarin.Android-Anwendung eine Instanz von der `FusedLocationProviderClient`. Diese Klasse stellt die erforderlichen Methoden Speicherort Updates abonnieren und den letzten bekannten Speicherort des Geräts abgerufen.

Die `OnCreate` Methode einer Aktivität ist einer geeigneten Stelle zum Abrufen eines Verweises auf die `FusedLocationProviderClient`, wie im folgenden Codeausschnitt gezeigt:

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

Die `FusedLocationProviderClient.GetLastLocationAsync()` Methode bietet eine einfache, nicht blockierende Methode für eine Anwendung Xamarin.Android schnell die letzten bekannten Speicherort des Geräts mit minimalem Aufwand Codierung abgerufen.

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

Eine Xamarin.Android Anwendung kann auch abonnieren Speicherort Updates aus der mit Standort-Anbieter mithilfe der `FusedLocationProviderClient.RequestLocationUpdatesAsync` Methode, wie in diesem Codeausschnitt gezeigt:

```csharp
await fusedLocationProviderClient.RequestLocationUpdatesAsync(locationRequest, locationCallback);
```
Diese Methode akzeptiert zwei Parameter:

-   **`Android.Gms.Location.LocationRequest`** &ndash; Ein `LocationRequest` Objekt ist wie eine Anwendung Xamarin.Android übergibt die Parameter zur Funktionsweise des Anbieters mit Sicherung Speicherort. Die `LocationRequest` enthält Informationen, die solche wie häufige anfordern gemacht werden sollen, oder von wichtigen ein Update der genaue Speicherort sein sollten. Beispielsweise verursacht eine wichtiger Speicherort-Anforderung des Geräts bestimmen den Speicherort der GPS und daher mehr Leistung verwenden. Diesem Codeausschnitt wird veranschaulicht, wie Sie erstellen eine `LocationRequest` für einen Standort mit hoher Genauigkeit, Überprüfung etwa alle fünf Minuten für Speicherort Update (aber nicht früher als zwei Minuten zwischen den Anfragen). Der Anbieter mit Sicherung Speicherort eine `LocationRequest` als Leitfaden für welche zu verwendenden Speicherortanbieter beim Versuch, um das Gerät Position zu bestimmen:

    ```csharp
    LocationRequest locationRequest = new LocationRequest()
                                      .SetPriority(LocationRequest.PriorityHighAccuracy)
                                      .SetInterval(60 * 1000 * 5)
                                      .SetFastestInterval(60 * 1000 * 2);
    ```
                                          

-   **`Android.Gms.Location.LocationCallback`** &ndash; Um Speicherort Updates zu erhalten, muss eine Anwendung Xamarin.Android Unterklasse der `LocationProvider` abstrakte Klasse. Diese Klasse verfügbar gemacht werden zwei Methoden, die vielleicht aufgerufen, von einem Speicherortanbieter mit Sicherung mit Informationen zum Speicherort die app zu aktualisieren. Dies wird im folgenden ausführlicher erläutert.

Um eine Xamarin.Android Anwendung eines Updates Speicherort zu benachrichtigen, der mit Standort-Anbieter rufen die `LocationCallBack.OnLocationResult(LocationResult result)`. Die `Android.Gms.Location.LocationResult` Parameter werden die Informationen zum Speicherort der Update enthalten.

Wenn der Speicherortanbieter mit Sicherung eine Änderung an der Verfügbarkeit Standortdaten erkennt, ruft er die `LocationProvider.OnLocationAvaibility(LocationAvailability
locationAvailability)` Methode. Wenn die `LocationAvailability.IsLocationAvailable` -Eigenschaft gibt `true`, wird davon ausgegangen werden kann, dass es sich bei die speicherortergebnissen Gerät gemeldet `OnLocationResult` sind als genau und wie auf dem neuesten Stand, gemäß der `LocationRequest`. Wenn `IsLocationAvailable` ist "false", und klicken Sie dann keine speicherortergebnissen von zurückgegeben werden, werden `OnLocationResult`.

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

## <a name="using-the-android-location-service-api"></a>Mithilfe der Android-Speicherort-Dienst-API

Der Speicherortdienst für Android ist eine ältere API für die Verwendung von Standortinformationen für Android. Standortdaten vom Hardware-Sensoren erfasst und gesammelt, die von einem Systemdienst, die mit in der Anwendung zugegriffen wird eine `LocationManager` Klasse und ein `ILocationListener`.

Der Speicherortdienst eignet sich optimal für Anwendungen, die auf Geräten ausgeführt werden müssen, die keine von Google Play-Dienste installiert sind.

Der Speicherort-Dienst ist eine besondere Art von [Service](http://developer.android.com/guide/components/services.html) vom System verwaltet. Ein System-Dienst interagiert mit der Gerätehardware und wird immer ausgeführt. In den Speicherort von Updates in der vorliegenden Anwendung abgerufen werden sollen, wir abonnieren Speicherort Updates aus dem System Location-Dienst mithilfe einer `LocationManager` und ein `RequestLocationUpdates` aufrufen.

Schließt mehrere Schritte, um den Standort des Benutzers mit Android Location-Dienst zu erhalten:

1.  Abrufen eines Verweises auf die `LocationManager` Dienst.
2.  Implementieren der `ILocationListener` Schnittstelle und behandeln Ereignisse aus, wenn der Speicherort geändert.
3.  Verwenden der `LocationManager` Anforderung Speicherort Updates für einen angegebenen Anbieter. Die `ILocationListener` aus dem vorherigen Schritt wird verwendet, um Rückrufe aus empfangen die `LocationManager`.
4.  Beenden Sie Speicherort Updates aus, wenn die Anwendung nicht mehr Updates empfangen werden.

### <a name="location-manager"></a>Standort-Manager

Wir erreichen der Speicherortdienst System mit einer Instanz von der `LocationManager` Klasse. `LocationManager` ist eine besondere Klasse, mit dem uns mit dem Speicherort im Dateisystem Dienst interagieren und Methoden dafür aufrufen. Eine Anwendung kann einen Verweis zum Abrufen der `LocationManager` durch Aufrufen von `GetSystemService` und übergeben eines Diensttyps wie unten dargestellt:

```csharp
LocationManager locationManager = (LocationManager) GetSystemService(Context.LocationService);
```

`OnCreate` ist eine gute zum Abrufen eines Verweises auf die `LocationManager`.
Es ist ratsam, behalten die `LocationManager` als Klassenvariable, damit wir sie an verschiedenen Punkten im Lebenszyklus Aktivität aufrufen können.

### <a name="request-location-updates-from-the-locationmanager"></a>Die LocationManager Speicherort Updates anfordern

Sobald die Anwendung einen Verweis auf die ist der `LocationManager`, teilen Sie muss die `LocationManager` welche Art von Informationen zum Speicherort, die erforderlich sind, und wie oft diese Informationen aktualisiert werden. Dazu rufen `RequestionLocationUpdates` auf die `LocationManager` Objekt und das Übergeben von in einige Kriterien für Updates und einen Rückruf, der den Speicherort Updates erhalten sollen. Dieser Rückruf wird ein Typ, der implementiert werden muss die `ILocationListener` Schnittstelle (ausführlicher weiter unten in diesem Handbuch beschrieben).

Die `RequestionLocationUpdates` Methode teilt dem Speicherort im Dateisystem Dienst, dass die Anwendung den Empfang von Updates Speicherort starten möchten. Diese Methode ermöglicht Ihnen die Angabe der Anbieter als auch Zeit und Entfernung Schwellenwerte um aktualisierungshäufigkeit zu steuern. Beispielsweise aktualisiert die Methode unter unten Anforderungen Speicherort vom Anbieter GPS-Speicherort alle 2000 Millisekunden, und nur, wenn der Speicherort mehr als 1 Meter geändert:

```csharp
// For this example, this method is part of a class that implements ILocationListener, described below
locationManager.RequestLocationUpdates(LocationManager.GpsProvider, 2000, 1, this);
```

Eine Anwendung sollte nur so oft wie erforderlich, damit die Anwendung auch ausgeführt die Speicherort Updates anfordern. Dies behält Akkulaufzeit und Benutzerfreundlicher für den Benutzer erstellt.

### <a name="responding-to-updates-from-the-locationmanager"></a>Reagieren auf Updates aus der LocationManager

Sobald eine Anwendung von Updates angefordert hat die `LocationManager`, erhalten sie Informationen aus dem Dienst, durch die Implementierung der [ `ILocationListener` ](https://developer.xamarin.com/api/type/Android.Locations.ILocationListener/) Schnittstelle. Diese Schnittstelle bietet vier Methoden für den Empfang von den Speicherort-Dienst und der Speicherortanbieter `OnLocationChanged`. Das System ruft `OnLocationChanged` Wenn vom Standort des Benutzers ändert genug, um Sie als Speicherort Änderung entsprechend den festgelegten Speicherort Updates zum Anfordern von Kriterien zu qualifizieren. 

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

### <a name="unsubscribing-to-locationmanager-updates"></a>Kündigung LocationManager Updates

Um Systemressourcen zu sparen, sollte eine Anwendung um Speicherort Updates so bald wie möglich zu kündigen. Die `RemoveUpdates` Methode teilt die `LocationManager` Senden von Updates zu unseren Anwendung beendet werden soll.  Beispielsweise kann eine Aktivität aufrufen `RemoveUpdates` in die `OnPause` Methode, damit wir Energie zu sparen, wenn eine Anwendung können benötigt keinen Speicherort Updates während der Aktivität nicht auf dem Bildschirm ist:

```csharp
protected override void OnPause ()
{
    base.OnPause ();
    locationManager.RemoveUpdates (this);
}
```

Wenn Ihre Anwendung Speicherort Updates im Hintergrund zu erhalten muss, sollten Sie einen benutzerdefinierten Dienst zum Erstellen, mit dem System Location-Dienst abonniert. Finden Sie in der [mit Android Services Backgrounding](~/android/app-fundamentals/services/index.md) Weitere Informationen.

### <a name="determining-the-best-location-provider-for-the-locationmanager"></a>Bestimmen des besten Ortungsanbieters für die LocationManager

Die Anwendung, die oben genannten legt als Speicherortanbieter GPS fest. Jedoch möglicherweise GPS nicht verfügbar in allen Fällen, z. B., wenn das Gerät in geschlossener Umgebung oder verfügt nicht über ein GPS-Empfänger. Wenn dies der Fall ist, ist das Ergebnis einer `null` für den Anbieter zurück.

Zum Abrufen Ihrer app verwenden, wenn GPS nicht verfügbar ist, verwenden Sie die `GetBestProvider` Methode für den besten Speicherort (Gerät unterstützt, und der Benutzer aktiviert)-Anbieter beim Anwendungsstart um Unterstützung bitten. Statt einen bestimmten Anbieter übergeben, Sie können feststellen, `GetBestProvider` die Anforderungen für Anbieter - z. B. Genauigkeit "und" Power "- mit einem [ `Criteria` Objekt](https://developer.xamarin.com/api/type/Android.Locations.Criteria/). `GetBestProvider` best-Anbieter für die angegebenen Kriterien zurückgegeben.

Der folgende Code zeigt, wie Sie den am besten Anbieter erhalten, und verwenden, wenn der Speicherort Updates anfordern:

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
>  Wenn der Benutzer alle Speicherort Anbieter deaktiviert hat `GetBestProvider` zurück `null`. Um zu sehen, wie dieser Code auf einem echten Gerät funktioniert, müssen Sie GPS, Wi-Fi und Mobilfunknetzen unter ermöglichen **Google-Einstellungen > Speicherort > Modus** wie in diesem Screenshot gezeigt:

[![Bildschirm "Einstellungen Positionsmodus" auf einem Android-Telefon](location-images/location-02.png)](location-images/location-02.png#lightbox)

Der folgende Screenshot zeigt, dem Speicherort Anwendung ausgeführten über `GetBestProvider`:

[![Anzeigen von Breiten- und Längengrad Anbieter GetBestProvider-app](location-images/location-03.png)](location-images/location-03.png#lightbox)

Beachten Sie, dass `GetBestProvider` wird den Anbieter nicht dynamisch geändert. Stattdessen bestimmt die am besten verfügbaren Anbieter einmal während des Lebenszyklus der Aktivität. Ändert die Anbieterstatus, nachdem es festgelegt wurde, wird die Anwendung erfordert zusätzlichen Code in der `ILocationListener` Methoden &ndash; `OnProviderEnabled`, `OnProviderDisabled`, und `OnStatusChanged` &ndash; , behandeln jede Möglichkeit, die im Zusammenhang mit der Anbieter-Schalter.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch behandelt den Standort des Benutzers mit Android Location-Dienst und den Speicherortanbieter mit Sicherung aus Google Speicherort Dienste-API abrufen.


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
