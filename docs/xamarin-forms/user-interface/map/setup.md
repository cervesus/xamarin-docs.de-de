---
Title: " Xamarin.Forms map Initialization and Configuration" Description: "The Xamarin.Forms . Das Karten-nuget-Paket ist erforderlich, um Maps-Funktionen in einer Anwendung zu verwenden. Außerdem müssen für den Zugriff auf den Speicherort des Benutzers Standort Berechtigungen für die Anwendung erteilt worden sein.
ms. Prod: ms. assetid: ms. Technology: Author: ms. Author: ms. Date: NO-LOC:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="xamarinforms-map-initialization-and-configuration"></a>Xamarin.FormsZuordnungs Initialisierung und-Konfiguration

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Das- [`Map`](xref:Xamarin.Forms.Maps.Map) Steuerelement verwendet das Native Karten Steuerelement auf jeder Plattform. Dies bietet Benutzern eine schnelle, vertraute Kartendarstellung, bedeutet jedoch, dass einige Konfigurationsschritte erforderlich sind, um die API-Anforderungen der einzelnen Plattformen einzuhalten.

## <a name="map-initialization"></a>Zuordnungs Initialisierung

Das- [`Map`](xref:Xamarin.Forms.Maps.Map) Steuerelement wird von bereitgestellt [ Xamarin.Forms . Ordnet das nuget](https://www.nuget.org/packages/Xamarin.Forms.Maps/) -Paket zu, das jedem Projekt in der Projekt Mappe hinzugefügt werden sollte.

Nach der Installation von [ Xamarin.Forms . Maps](https://www.nuget.org/packages/Xamarin.Forms.Maps/) : das nuget-Paket muss in jedem Platt Form Projekt initialisiert werden.

Unter IOS sollte dies in **AppDelegate.cs** erfolgen, indem die- `Xamarin.FormsMaps.Init` Methode *nach* der-Methode aufgerufen wird `Xamarin.Forms.Forms.Init` :

```csharp
Xamarin.FormsMaps.Init();
```

Unter Android sollte dies in **MainActivity.cs** erfolgen, indem die- `Xamarin.FormsMaps.Init` Methode *nach* der-Methode aufgerufen wird `Xamarin.Forms.Forms.Init` :

```csharp
Xamarin.FormsMaps.Init(this, savedInstanceState);
```

Beim universelle Windows-Plattform (UWP) sollte dies in **MainPage.XAML.cs** erfolgen, indem die- `Xamarin.FormsMaps.Init` Methode aus dem- `MainPage` Konstruktor aufgerufen wird:

```csharp
Xamarin.FormsMaps.Init("INSERT_AUTHENTICATION_TOKEN_HERE");
```

Informationen zu dem für UWP erforderlichen Authentifizierungs Token finden Sie unter [universelle Windows-Plattform](#universal-windows-platform).

Nachdem das nuget-Paket hinzugefügt und die Initialisierungs Methode in jeder Anwendung aufgerufen wurde, `Xamarin.Forms.Maps` können APIs im freigegebenen Code Projekt verwendet werden.

## <a name="platform-configuration"></a>Platt Form Konfiguration

Für Android und die universelle Windows-Plattform (UWP) ist eine zusätzliche Konfiguration erforderlich, bevor die Karte angezeigt wird. Außerdem erfordert das Zugreifen auf den Speicherort des Benutzers unter IOS, Android und UWP das Erteilen von Standort Berechtigungen für die Anwendung.

### <a name="ios"></a>iOS

Das anzeigen und interagieren mit einer Karte unter IOS erfordert keine zusätzliche Konfiguration. Für den Zugriff auf die Standortdienste müssen Sie jedoch die folgenden Schlüssel in " **Info. plist**" festlegen:

- IOS 11 und höher
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26)– für die Verwendung von Location Services, wenn die Anwendung verwendet wird
  - [`NSLocationAlwaysAndWhenInUseUsageDescription`](https://developer.apple.com/documentation/bundleresources/information_property_list/nslocationalwaysandwheninuseusagedescription)– für die jederzeit Verwendung von Location Services
- IOS 10 und früher
  - [`NSLocationWhenInUseUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW26)– für die Verwendung von Location Services, wenn die Anwendung verwendet wird
  - [`NSLocationAlwaysUsageDescription`](https://developer.apple.com/library/ios/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW18)– für die jederzeit Verwendung von Location Services    

Zur Unterstützung von IOS 11 und früher können Sie alle drei Schlüssel einschließen: `NSLocationWhenInUseUsageDescription` , `NSLocationAlwaysAndWhenInUseUsageDescription` und `NSLocationAlwaysUsageDescription` .

Die XML-Darstellung für diese Schlüssel in " **Info. plist** " ist unten dargestellt. Sie sollten die `string` Werte aktualisieren, um die Verwendung der Standortinformationen durch Ihre Anwendung widerzuspiegeln:

```xml
<key>NSLocationAlwaysUsageDescription</key>
<string>Can we use your location at all times?</string>
<key>NSLocationWhenInUseUsageDescription</key>
<string>Can we use your location when your application is being used?</string>
<key>NSLocationAlwaysAndWhenInUseUsageDescription</key>
<string>Can we use your location at all times?</string>
```

Die **Info. plist** -Einträge können auch beim Bearbeiten der Datei " **Info. plist** " in der **Quell** Ansicht hinzugefügt werden:

![Info. plist für IOS 8](setup-images/ios8-map-permissions.png "IOS 8 erforderliche Info. plist-Einträge")

Daraufhin wird eine Aufforderung angezeigt, wenn die Anwendung versucht, auf den Speicherort des Benutzers zuzugreifen und Zugriff anzufordern:

[![Screenshot der Standort Berechtigungs Anforderung unter IOS](setup-images/permission-ios.png "IOS-Berechtigungs Anforderung")](setup-images/permission-ios-large.png#lightbox "IOS-Berechtigungs Anforderung")

### <a name="android"></a>Android

Der Konfigurations Vorgang zum Anzeigen und interagieren mit einer Karte unter Android lautet:

1. Verschaffen Sie sich einen Google Maps-API-Schlüssel, und fügen Sie ihn dem Manifest hinzu.
1. Geben Sie die Google Play Services-Versionsnummer im Manifest an.
1. Geben Sie im Manifest die Anforderung für die Apache HTTP-Legacy Bibliothek an.
1. optionale Geben Sie die WRITE_EXTERNAL_STORAGE-Berechtigung im Manifest an.
1. optionale Geben Sie die Speicherort Berechtigungen im Manifest an.
1. optionale Berechtigungen für Lauf Zeit Speicherort in der- `MainActivity` Klasse anfordern.

Ein Beispiel für eine ordnungsgemäß konfigurierte Manifest-Datei finden Sie unter " [androidmanifest. XML](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/WorkingWithMaps.Android/Properties/AndroidManifest.xml) " aus der Beispielanwendung.

#### <a name="get-a-google-maps-api-key"></a>Einen Google Maps-API-Schlüssel erhalten

Um die [Google Maps-API](https://developers.google.com/maps/documentation/android/) unter Android verwenden zu können, müssen Sie einen API-Schlüssel generieren. Befolgen Sie hierzu die Anweisungen unter Abrufen [eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

Nachdem Sie einen API-Schlüssel abgerufen haben, muss er innerhalb des `<application>` -Elements der Datei " **Properties/androidmanifest. XML** " hinzugefügt werden:

```xml
<application ...>
    <meta-data android:name="com.google.android.geo.API_KEY" android:value="PASTE-YOUR-API-KEY-HERE" />
</application>
```

Dadurch wird der API-Schlüssel in das Manifest eingebettet. Ohne einen gültigen API-Schlüssel [`Map`](xref:Xamarin.Forms.Maps.Map) zeigt das Steuerelement ein leeres Raster an.

> [!NOTE]
> `com.google.android.geo.API_KEY`der empfohlene Metadatenname für den API-Schlüssel. Aus Gründen der Abwärtskompatibilität `com.google.android.maps.v2.API_KEY` kann der Metadatenname verwendet werden, aber nur die Authentifizierung für die Android Maps-API v2.

Damit Ihr APK auf Google Maps zugreifen kann, müssen Sie SHA-1-Fingerabdrücke und Paketnamen für jeden Keystore (Debug und Release) einschließen, mit dem Sie Ihr APK signieren. Wenn Sie z. b. einen Computer für das Debuggen und einen anderen Computer zum Erstellen des Release-APK verwenden, sollten Sie den SHA-1-Zertifikat Fingerabdruck aus dem Debug-keystore des ersten Computers und dem SHA-1-Zertifikat Fingerabdruck aus dem releasekeystore des zweiten Computers einschließen. Beachten Sie auch, dass die Schlüssel Anmelde Informationen bearbeitet werden, wenn sich der **Paketname** der app ändert. Siehe Abrufen [eines Google Maps-API-Schlüssels](~/android/platform/maps-and-location/maps/obtaining-a-google-maps-api-key.md).

#### <a name="specify-the-google-play-services-version-number"></a>Angeben der Google Play Services-Versionsnummer

Fügen Sie die folgende Deklaration im- `<application>` Element von " **androidmanifest. XML**" hinzu:

```xml
<meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
```

Dadurch wird die Version von Google Play Diensten, mit denen die Anwendung kompiliert wurde, in das Manifest eingebettet.

#### <a name="specify-the-requirement-for-the-apache-http-legacy-library"></a>Angeben der Anforderung für die Apache HTTP-Legacy Bibliothek

Wenn Ihre Xamarin.Forms Anwendung auf API 28 oder höher ausgerichtet ist, müssen Sie die folgende Deklaration im- `<application>` Element von " **androidmanifest. XML**" hinzufügen:

```xml
<uses-library android:name="org.apache.http.legacy" android:required="false" />    
```

Dadurch wird die Anwendung aufgefordert, die Apache HTTP-Client Bibliothek zu verwenden, die `bootclasspath` in Android 9 aus entfernt wurde.

#### <a name="specify-the-write_external_storage-permission"></a>WRITE_EXTERNAL_STORAGE Berechtigung angeben

Wenn Ihre Anwendung auf API 22 oder niedriger ausgerichtet ist, kann es erforderlich sein, dem `WRITE_EXTERNAL_STORAGE` Manifest die Berechtigung als untergeordnetes Element des- `<manifest>` Elements hinzuzufügen:

```xml
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
```

Dies ist nicht erforderlich, wenn Ihre Anwendung auf API 23 oder höher ausgerichtet ist.

#### <a name="specify-location-permissions"></a>Speicherort Berechtigungen angeben

Wenn die Anwendung auf den Speicherort des Benutzers zugreifen muss, müssen Sie die Berechtigung anfordern, indem Sie dem `ACCESS_COARSE_LOCATION` `ACCESS_FINE_LOCATION` Manifest (oder beiden) die Berechtigungen oder als untergeordnetes Element des-Elements hinzufügen `<manifest>` :

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="com.companyname.myapp">
  ...
  <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
  <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
</manifest>
```

Mithilfe der `ACCESS_COARSE_LOCATION` Berechtigung kann die API den Speicherort des Geräts mithilfe von WiFi-oder mobilen Daten (oder beides) bestimmen. Mit den `ACCESS_FINE_LOCATION` Berechtigungen kann die API die GPS (GPS), WiFi oder Mobile Daten verwenden, um einen präzisen Speicherort wie möglich zu bestimmen.

Alternativ können diese Berechtigungen aktiviert werden, indem Sie mit dem Manifest-Editor die folgenden Berechtigungen hinzufügen:

- `AccessCoarseLocation`
- `AccessFineLocation`

Diese werden im folgenden Screenshot gezeigt:

![Erforderliche Berechtigungen für Android](setup-images/android-map-permissions.png "Erforderliche Berechtigungen für Android")

#### <a name="request-runtime-location-permissions"></a>Berechtigungen für Lauf Zeit Speicherort anfordern

Wenn Ihre Anwendung auf API 23 oder höher abzielt und auf den Speicherort des Benutzers zugreifen muss, muss Sie überprüfen, ob Sie zur Laufzeit über die erforderliche Berechtigung verfügt, und Sie anfordern, wenn Sie nicht vorhanden ist. Dies kann folgendermaßen erfüllt werden:

1. Fügen Sie in der- `MainActivity` Klasse die folgenden Felder hinzu:

    ```csharp
    const int RequestLocationId = 0;

    readonly string[] LocationPermissions =
    {
        Manifest.Permission.AccessCoarseLocation,
        Manifest.Permission.AccessFineLocation
    };
    ```

1. Fügen Sie in der- `MainActivity` Klasse die folgende `OnStart` außer Kraft Setzung hinzu:

    ```csharp
    protected override void OnStart()
    {
        base.OnStart();

        if ((int)Build.VERSION.SdkInt >= 23)
        {
            if (CheckSelfPermission(Manifest.Permission.AccessFineLocation) != Permission.Granted)
            {
                RequestPermissions(LocationPermissions, RequestLocationId);
            }
            else
            {
                // Permissions already granted - display a message.
            }
        }
    }
    ```

    Vorausgesetzt, dass die Anwendung auf API 23 oder höher ausgerichtet ist, führt dieser Code eine Lauf Zeit Berechtigungsüberprüfung für die `AccessFineLocation` Berechtigung aus. Wenn die Berechtigung nicht erteilt wurde, erfolgt eine Berechtigungs Anforderung durch Aufrufen der- `RequestPermissions` Methode.

1. Fügen Sie in der- `MainActivity` Klasse die folgende `OnRequestPermissionsResult` außer Kraft Setzung hinzu:

    ```csharp
    public override void OnRequestPermissionsResult(int requestCode, string[] permissions, [GeneratedEnum] Permission[] grantResults)
    {
        if (requestCode == RequestLocationId)
        {
            if ((grantResults.Length == 1) && (grantResults[0] == (int)Permission.Granted))
                // Permissions granted - display a message.
            else
                // Permissions denied - display a message.
        }
        else
        {
            base.OnRequestPermissionsResult(requestCode, permissions, grantResults);
        }
    }
    ```

    Diese außer Kraft Setzung behandelt das Ergebnis der Berechtigungs Anforderung.

Der Gesamteffekt dieses Codes besteht darin, dass der folgende Dialog angezeigt wird, wenn die Anwendung den Speicherort des Benutzers anfordert:

[![Screenshot der Standort Berechtigungs Anforderung unter Android](setup-images/permission-android.png "Android-Berechtigungs Anforderung")](setup-images/permission-android-large.png#lightbox "Android-Berechtigungs Anforderung")

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Bei UWP muss Ihre Anwendung authentifiziert werden, bevor eine Zuordnung angezeigt und Kartendienste genutzt werden können. Um Ihre Anwendung zu authentifizieren, müssen Sie einen Maps-Authentifizierungsschlüssel angeben. Weitere Informationen finden Sie unter [Anfordern eines Zuordnungs Authentifizierungs Schlüssels](/windows/uwp/maps-and-location/authentication-key). Das Authentifizierungs Token sollte dann im-Methoden-Befehl angegeben werden `FormsMaps.Init("AUTHORIZATION_TOKEN")` , um die Anwendung mit dem Namen von "admaps" zu authentifizieren.

> [!NOTE]
> Um Kartendienste wie z. b. die Geocodierung verwenden zu können, müssen Sie die- `MapService.ServiceToken` Eigenschaft auf den Wert des Authentifizierungs Schlüssels festlegen. Dies kann mit der folgenden Codezeile erreicht werden: `Windows.Services.Maps.MapService.ServiceToken = "INSERT_AUTH_TOKEN_HERE";` .

Außerdem müssen Sie, wenn Ihre Anwendung auf den Speicherort des Benutzers zugreifen muss, die Standort Funktion im Paket Manifest aktivieren. Dies kann folgendermaßen erfüllt werden:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf **package.appxmanifest**, und wählen Sie die Registerkarte **Funktionen** aus.
1. Aktivieren Sie in der Liste der **Funktionen** das Kontrollkästchen für **Speicherort**. Dadurch wird die `location` Geräte Funktion der Paket Manifest-Datei hinzugefügt.

    ```xml
    <Capabilities>
      <!-- DeviceCapability elements must follow Capability elements (if present) -->
      <DeviceCapability Name="location"/>
    </Capabilities>
    ```

#### <a name="release-builds"></a>Releasebuilds

UWP-Releasebuilds verwenden die .net Native-Kompilierung, um die Anwendung direkt in systemeigenen Code zu kompilieren Eine Folge davon ist jedoch, dass der Renderer für das [`Map`](xref:Xamarin.Forms.Maps.Map) Steuerelement in der UWP möglicherweise aus der ausführbaren Datei verknüpft ist. Dies kann mithilfe einer UWP-spezifischen Überladung der- `Forms.Init` Methode in **app.XAML.cs**korrigiert werden:

```csharp
var assembliesToInclude = new [] { typeof(Xamarin.Forms.Maps.UWP.MapRenderer).GetTypeInfo().Assembly };
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
```

Dieser Code übergibt die Assembly, in der sich die- `Xamarin.Forms.Maps.UWP.MapRenderer` Klasse befindet, an die- `Forms.Init` Methode. Dadurch wird sichergestellt, dass die Assembly durch den .net Native-Kompilierungsprozess nicht aus der ausführbaren Datei verknüpft ist.

> [!IMPORTANT]
> Wenn dies nicht der Fall ist, wird das [`Map`](xref:Xamarin.Forms.Maps.Map) Steuerelement beim Ausführen eines Releasebuilds nicht angezeigt.

## <a name="related-links"></a>Verwandte Links

- [Maps-Beispiel](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.Forms. Ordnet Pins](~/xamarin-forms/user-interface/map/pins.md)zu.
- [Karten-API](xref:Xamarin.Forms.Maps)
- [Zuordnen eines benutzerdefinierten Renderers](~/xamarin-forms/app-fundamentals/custom-renderer/map-pin.md)
