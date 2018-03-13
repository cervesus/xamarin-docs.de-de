---
title: "Exemplarische Vorgehensweise – Hintergrund Speicherort verwenden"
ms.topic: article
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: b10894d6b18d78d682825000726c5ef2cbe5ba6b
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="walkthrough---using-background-location"></a>Exemplarische Vorgehensweise – Hintergrund Speicherort verwenden

In diesem Beispiel werden wir ein iOS Speicherort-Anwendung erstellen, die Informationen zu unseren aktuellen Speicherort ausgibt: Breite, Länge und andere Parameter auf dem Bildschirm. Diese Anwendung wird gezeigt, wie ordnungsgemäß Speicherort Updates ausführen, während die Anwendung entweder "aktiv" oder "Backgrounded ist.

In dieser exemplarischen Vorgehensweise wird erläutert, einige wichtige Konzepte, einschließlich Registrieren einer app als Hintergrund erforderliche Anwendung und Benutzeroberfläche Updates anhalten, wenn die app backgrounded ist arbeiten mit backgrounding der `WillEnterBackground` und `WillEnterForeground` `AppDelegate` Methoden .

## <a name="application-set-up"></a>Anwendung einrichten


1. Erstellen Sie zunächst ein neues **iOS > App > einzelne Ansicht-Anwendung (c#)**. Rufen sie _Speicherort_ und stellen Sie sicher, dass iPad und iPhone ausgewählt wurden.

1. Eine Anwendung Speicherort qualifiziert als in iOS-Anwendung im Hintergrund erforderlich. Registrieren Sie die Anwendung als Anwendung Speicherort durch Bearbeiten der **"Info.plist"** Datei für das Projekt.

    Im Projektmappen-Explorer, klicken Sie mit der Doppelklicken auf die **"Info.plist"** Datei geöffnet, und führen Sie einen Bildlauf zum Ende der Liste. Aktivieren Sie das Kontrollkästchen sowohl die **Aktivieren von Hintergrundmodi** und **Speicherort Updates** Kontrollkästchen.


    In Visual Studio für Mac sieht es wie etwa wie folgt aus:

    [![](location-walkthrough-images/image7.png "Aktivieren Sie das Kontrollkästchen der Hintergrundmodi aktivieren und die Kontrollkästchen für die Standort-Updates")](location-walkthrough-images/image7.png#lightbox)

    In Visual Studio **"Info.plist"** muss manuell aktualisiert werden, indem Sie die folgenden Schlüssel/Wert-Paar hinzufügen:

    ```xml
    <key>UIBackgroundModes</key>
    <array>
        <string>location</string>
    </array>
    ```

1. Nun, dass die Anwendung registriert ist, können sie Standortdaten vom Gerät abrufen. In iOS die `CLLocationManager` Klasse wird verwendet, um Standortinformationen zugreifen, und Ereignisse, die Updates Speicherort bereitstellen auslösen kann.

1. Erstellen Sie im Code wird eine neue Klasse namens `LocationManager` , die eine einzelne Stelle für verschiedene Bildschirme und Code zum Abonnieren von Speicherort Updates bereitstellt. In der `LocationManager` -Klasse verwenden, müssen Sie eine Instanz von der `CLLocationManager` aufgerufen `LocMgr`:

    ```csharp
    public class LocationManager
    {
        protected CLLocationManager locMgr;

        public LocationManager () {
            this.locMgr = new CLLocationManager();
            this.locMgr.PausesLocationUpdatesAutomatically = false;

            // iOS 8 has additional permissions requirements
            if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
                locMgr.RequestAlwaysAuthorization (); // works in background
                //locMgr.RequestWhenInUseAuthorization (); // only in foreground
            }

            if (UIDevice.CurrentDevice.CheckSystemVersion (9, 0)) {
                locMgr.AllowsBackgroundLocationUpdates = true;
            }
        }

        public CLLocationManager LocMgr {
            get { return this.locMgr; }
        }
    }
    ```

    Der obige Code legt eine Reihe von Eigenschaften und Berechtigungen auf die [CLLocationManager](https://developer.xamarin.com/api/type/CoreLocation.CLLocationManager/) Klasse:

    - `PausesLocationUpdatesAutomatically` – Dies ist ein boolescher Wert, der festgelegt werden kann, je nachdem, ob das System anhalten Speicherort Updates zugelassen wird. Auf manchen Geräten wird standardmäßig `true`, wodurch das Gerät überhaupt Hintergrund Speicherort Updates nach ca. 15 Minuten.
    - `RequestAlwaysAuthorization` – Sie sollten diese Methode, um die app-Benutzer die Möglichkeit, den Speicherort im Hintergrund auf zulassen gewähren übergeben. `RequestWhenInUseAuthorization` kann auch übergeben werden, wenn Sie dem Benutzer bieten die Möglichkeit, den Speicherort zugegriffen werden, nur, wenn die app im Vordergrund ist zulassen möchten.
    - `AllowsBackgroundLocationUpdates` – Dies ist eine boolesche Eigenschaft, die unter iOS 9, die festgelegt werden kann, können eine app Empfang von Updates Speicherort, wenn angehalten eingeführt.

    > [!IMPORTANT]
    > **Warnung**: iOS 8 (und höher) erfordert auch einen Eintrag in der **"Info.plist"** Datei, um dem Benutzer als Teil der autorisierungsanforderung anzuzeigen.

1. Fügen Sie einen Schlüssel `NSLocationAlwaysUsageDescription` oder `NSLocationWhenInUseUsageDescription` mit einer Zeichenfolge, die für den Benutzer in der Warnung angezeigt wird, von dem Speicherort Datenzugriff angefordert.

1. iOS 9 erfordert, dass die Verwendung von `AllowsBackgroundLocationUpdates` der **"Info.plist"** enthält die Schlüssel `UIBackgroundModes` mit dem Wert `location`. Wenn Sie abgeschlossen haben, Schritt 2 dieser exemplarischen Vorgehensweise, dies sollte bereits in der Datei "Info.plist" war.


1. Innerhalb der `LocationManager` Klasse, erstellen Sie eine Methode namens `StartLocationUpdates` durch den folgenden Code. Dieser Code zeigt, wie mit dem Empfang Speicherort Updates aus der `CLLocationManager`:

    ```csharp
    if (CLLocationManager.LocationServicesEnabled) {
        //set the desired accuracy, in meters
        LocMgr.DesiredAccuracy = 1;
        LocMgr.LocationsUpdated += (object sender, CLLocationsUpdatedEventArgs e) =>
        {
            // fire our custom Location Updated event
            LocationUpdated (this, new LocationUpdatedEventArgs (e.Locations [e.Locations.Length - 1]));
        };
        LocMgr.StartUpdatingLocation();
    }
    ```

    Es gibt einige wichtige Punkte, die in dieser Methode geschieht. Zuerst führen wir ein Häkchen, um festzustellen, ob die Anwendung auf dem Gerät den Zugriff auf Standortdaten hat. Wir überprüfen durch Aufrufen von `LocationServicesEnabled` auf die `CLLocationManager`. Von dieser Methode zurückgegeben **"false"** , wenn der Benutzer den Anwendungszugriff auf Standortinformationen verweigert wurde.

1. Als Nächstes weisen Sie wie oft der Quellspeicherort-Manager um zu aktualisieren. `CLLocationManager` bietet viele Optionen zum Filtern und Konfigurieren von Standortdaten, einschließlich der Häufigkeit von Updates an. In diesem Beispiel legen die `DesiredAccuracy` zu aktualisieren, sobald die Position von einem Messgerät ändert. Weitere Informationen zum Speicherort aktualisierungshäufigkeit und andere Einstellungen konfigurieren, finden Sie in der [CLLocationManager-Klassenreferenz](http://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) in der Apple-Dokumentation.

1. Rufen Sie zum Schluss `StartUpdatingLocation` auf die `CLLocationManager` Instanz. Dies teilt die Standort-Manager eine anfängliche Update auf den aktuellen Speicherort abgerufen und mit dem Senden von Updates begonnen

Bisher die Quellspeicherort-Manager erstellt wurde, konfiguriert mit den Arten von Daten, die wir erhalten, möchten und die ursprüngliche Position festgestellt hat. Nun muss der Code zum Rendern der Standortdaten und der Benutzeroberfläche. Wir dazu ein benutzerdefiniertes Ereignis aus, die akzeptiert eine `CLLocation` als Argument:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

Der nächste Schritt ist zum Abonnieren von Speicherort Updates aus der `CLLocationManager`, und lösen Sie das benutzerdefinierte `LocationUpdated` Ereignis aus, wenn der neue Speicherort Daten verfügbar, werden in den Speicherort als Argument übergeben. Zu diesem Zweck erstellen Sie eine neue Klasse **LocationUpdateEventArgs.cs**. Dieser Code ist in die Hauptassembly der Anwendung zugegriffen werden kann, und gibt die Gerätestandort zurück, wenn das Ereignis ausgelöst wird:

```csharp
public class LocationUpdatedEventArgs : EventArgs
{
    CLLocation location;

    public LocationUpdatedEventArgs(CLLocation location)
    {
       this.location = location;
    }

    public CLLocation Location
    {
       get { return location; }
    }
}
```

## <a name="user-interface"></a>Benutzeroberfläche

1. Verwenden Sie die iOS-Designer auf um den Bildschirm zu erstellen, der Informationen zum Speicherort angezeigt werden. Doppelklicken Sie auf die **Main.storyboard** Datei, um zu beginnen.

    Ziehen Sie auf das Storyboard mehrere Bezeichnungen auf dem Bildschirm, um als Platzhalter für Informationen zum Speicherort ein. In diesem Beispiel sind Bezeichnungen für die Breite, Länge, einsatzhöhe, Kurs und Geschwindigkeit.

    Das Layout sollte folgendermaßen aussehen:

    ![](location-walkthrough-images/image8.png "Ein Beispiellayout für die Benutzeroberfläche in der iOS-Designer")

1. Doppelklicken Sie in das Auffüllzeichen Lösung auf die `ViewController.cs` Datei, und bearbeiten, um eine neue Instanz der LocationManager und rufen erstellen `StartLocationUpdates`darauf.
  Ändern Sie den Code wie folgt aussehen:

    ```csharp
    #region Computed Properties
    public static bool UserInterfaceIdiomIsPhone {
                get { return UIDevice.CurrentDevice.UserInterfaceIdiom == UIUserInterfaceIdiom.Phone; }
            }

    public static LocationManager Manager { get; set;}
    #endregion

    #region Constructors
    public ViewController (IntPtr handle) : base (handle)
    {
    // As soon as the app is done launching, begin generating location updates in the location manager
        Manager = new LocationManager();
        Manager.StartLocationUpdates();
    }

    #endregion
    ```

    Dadurch wird der Speicherort-Updates beim Start der Anwendung gestartet, wird zwar keine Daten angezeigt.

1. Nun, dass der Speicherort Updates empfangen werden, aktualisieren Sie den Bildschirm mit Informationen zum Speicherort. Die folgende Methode ruft den Speicherort des unsere `LocationUpdated` Ereignis und zeigt sie in der Benutzeroberfläche:

    ```csharp
    #region Public Methods
    public void HandleLocationChanged (object sender, LocationUpdatedEventArgs e)
    {
        // Handle foreground updates
        CLLocation location = e.Location;

        LblAltitude.Text = location.Altitude + " meters";
        LblLongitude.Text = location.Coordinate.Longitude.ToString ();
        LblLatitude.Text = location.Coordinate.Latitude.ToString ();
        LblCourse.Text = location.Course.ToString ();
        LblSpeed.Text = location.Speed.ToString ();

        Console.WriteLine ("foreground updated");
    }
    #endregion
    ```

Wir benötigen Sie zum Abonnieren der `LocationUpdated` Ereignis in unserem AppDelegate, und rufen Sie die neue Methode, um die Benutzeroberfläche zu aktualisieren. Fügen Sie den folgenden Code in `ViewDidLoad,` direkt nach der `StartLocationUpdates` aufrufen:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


Wenn die Anwendung ausgeführt wird, sollte es etwa wie folgt aussehen:

[![](location-walkthrough-images/image5.png "Eine Beispiel-app ausführen")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Behandlung von Status "aktiv" und "Hintergrund"

1. Die Anwendung ist Speicherort Updates ausgeben, während es im Vordergrund und aktiv ist. Um zu zeigen, was geschieht, wenn die app wechselt in den Hintergrund, überschreiben die `AppDelegate` Methoden, die Anwendung verfolgen statusänderungen, damit die Anwendung in die Konsole schreibt, beim Übergang zwischen den Vordergrund und Hintergrund:

    ```csharp
    public override void DidEnterBackground (UIApplication application)
    {
        Console.WriteLine ("App entering background state.");
    }

    public override void WillEnterForeground (UIApplication application)
    {
        Console.WriteLine ("App will enter foreground");
    }
    ```

    Fügen Sie den folgenden Code in die `LocationManager` So drucken Sie den aktualisierten Speicherort kontinuierlich Daten an die Anwendungsausgabe, um zu überprüfen, ob die Informationen zum Speicherort ist weiterhin im Hintergrund zur Verfügung:

    ```csharp
    public class LocationManager
    {
        public LocationManager ()
        {
        ...
        LocationUpdated += PrintLocation;
        }
        ...

        //This will keep going in the background and the foreground
        public void PrintLocation (object sender, LocationUpdatedEventArgs e) {
        CLLocation location = e.Location;
        Console.WriteLine ("Altitude: " + location.Altitude + " meters");
        Console.WriteLine ("Longitude: " + location.Coordinate.Longitude);
        Console.WriteLine ("Latitude: " + location.Coordinate.Latitude);
        Console.WriteLine ("Course: " + location.Course);
        Console.WriteLine ("Speed: " + location.Speed);
        }
    }
    ```

1. Besteht eine verbleibenden Problem mit dem Code: beim Versuch, die Benutzeroberfläche zu aktualisieren, wenn die app backgrounded ist wird Ursache iOS wird beenden Sie sie. Wenn die app in den Hintergrund wechselt, muss der Code zum Abbestellen von Speicherort Updates und beenden Sie die Aktualisierung der Benutzeroberflächenautomatisierungs.

    iOS bietet uns Benachrichtigungen, wenn die app zu für den Übergang in eine andere Anwendung Status. In diesem Fall kann es abonnieren, die `ObserveDidEnterBackground` Benachrichtigung.

    Der folgende Codeausschnitt zeigt, wie auf eine Benachrichtigung zu verwenden, damit die Ansicht, die wissen, wann Updates UI angehalten wird. Dies geht `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Wenn die app ausgeführt wird, wird die Ausgabe etwa wie folgt aussehen:

    ![](location-walkthrough-images/image6.png "Beispiel für die Standort-Ausgabe in der Konsole")

1. Die Anwendung druckt Speicherort Updates auf dem Bildschirm, während des Betriebs in den Vordergrund, und zum Drucken von Daten in das Ausgabefenster für die Anwendung, und arbeiten Sie dabei im Hintergrund fortgesetzt.

Bleibt nur ein ausstehendes Problem: der Bildschirm UI-Updates beginnt, wenn die app zuerst geladen wird, verfügt aber über keine Möglichkeit, zu wissen, wann die app im Vordergrund erneut eingegeben hat. Wenn die backgrounded Anwendung wieder in den Vordergrund hinzugefügt wird, wird nicht UI Updates fortsetzen.

Schachteln Sie zur Beseitigung dieses Problems einen Aufruf von UI-Updates innerhalb einer anderen Benachrichtigung starten, das ausgelöst wird, wenn die Anwendung den aktiven Status wechselt:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Die Benutzeroberfläche beginnt aktualisieren, wenn die Anwendung zuerst gestartet wurde, und Resume jederzeit die app aktualisiert wieder in den Vordergrund jetzt.

In dieser exemplarischen Vorgehensweise erstellt es eine gut konzipierter, Hintergrund unterstützende iOS-Anwendung, die auf dem Bildschirm und die Anwendung Ausgabefenster Standortdaten ausgibt.


## <a name="related-links"></a>Verwandte Links

- [Speicherort (Teil 4) (Beispiel)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Core-Framework Speicherortverweis](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
