---
title: Exemplarische Vorgehensweise – Hintergrundposition in Xamarin.iOS
description: Dieses Dokument enthält eine exemplarische Vorgehensweise zur Verwendung von Standortinformationen in einer backgrounded Xamarin.iOS-Anwendung. Es beschreibt die erforderlichen Setup, Benutzeroberfläche und Anwendungsstatus.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: fa8a48e165764a449af4bc5414d2e66aecea8269
ms.sourcegitcommit: 495680e74c72e7c570e68cde95d3d3643b1fcc8a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/02/2019
ms.locfileid: "58870143"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>Exemplarische Vorgehensweise – Hintergrundposition in Xamarin.iOS

In diesem Beispiel werden wir ein iOS Speicherort-Anwendung erstellen, die Informationen über unsere aktuellen Speicherort ausgibt: Längen- und Breitengrad sowie andere Parameter auf dem Bildschirm. Diese Anwendung zeigen, wie speicherortaktualisierungen ordnungsgemäß durchführen, während die Anwendung entweder "aktiv" oder "Backgrounded ist.

In dieser exemplarischen Vorgehensweise wird erläutert, einige wichtige Konzepte, wie z.B. das Registrieren einer app als Hintergrund-Bedarf-Anwendung und Aktualisierungen der Benutzeroberfläche anhalten, wenn die app in diesem Fall arbeiten mit hintergrundverarbeitung der `WillEnterBackground` und `WillEnterForeground` `AppDelegate` Methoden .

## <a name="application-set-up"></a>Eingerichtete Anwendung


1. Erstellen Sie zunächst ein neues **iOS > App > Einzelansicht-Anwendungsprojekt (C#)**. Rufen sie _Speicherort_ und stellen Sie sicher, dass sowohl iPad und iPhone ausgewählt wurden.

1. Eine Standort-Anwendung kann als Hintergrund-Bedarf-Anwendung unter iOS. Registrieren Sie die Anwendung als Anwendung Speicherort durch Bearbeiten der **"Info.plist"** Datei für das Projekt.

    Im Projektmappen-Explorer einen Doppelklick auf die **"Info.plist"** Datei zu öffnen, und führen Sie einen Bildlauf zum unteren Rand der Liste. Ein Häkchen zu platzieren, indem Sie sowohl die **Hintergrundmodi aktivieren** und **Speicherortaktualisierungen** Kontrollkästchen.

    In Visual Studio für Mac sieht dies wie etwa wie folgt aus:

    [![](location-walkthrough-images/image7.png "Aktivieren Sie das Kontrollkästchen von den Hintergrundmodi aktivieren und die Kontrollkästchen für Speicherortaktualisierungen")](location-walkthrough-images/image7.png#lightbox)

    In Visual Studio **"Info.plist"** muss manuell aktualisiert werden, indem Sie das folgende Schlüssel/Wert-Paar hinzufügen:

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. Nun, da die Anwendung registriert ist, können sie Daten vom Gerät abrufen. Bei iOS der `CLLocationManager` Klasse wird verwendet, um auf Standortinformationen zugreifen und können Ereignisse auslösen, die Positionsupdates bereitzustellen.

1. In den Code, erstellen Sie eine neue Klasse namens `LocationManager` , die einen einzigen Ort für verschiedene Bildschirme und Code zum Abonnieren von speicherortaktualisierungen bereitstellt. In der `LocationManager` Klasse, die eine Instanz des der `CLLocationManager` namens `LocMgr`:

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

    Der obige Code legt eine Reihe von Eigenschaften und Berechtigungen auf die [CLLocationManager](xref:CoreLocation.CLLocationManager) Klasse:

    - `PausesLocationUpdatesAutomatically` – Dies ist ein boolescher Wert, der festgelegt werden kann, je nachdem, ob das System speicherortaktualisierungen anhalten darf. Auf manchen Geräten wird standardmäßig `true`, wodurch das Gerät nicht mehr erhalten Hintergrund speicherortaktualisierungen nach ca. 15 Minuten.
    - `RequestAlwaysAuthorization` – Sie sollten diese Methode, um den Speicherort im Hintergrund erfolgen darf, geben Sie dem app-Benutzer übergeben. `RequestWhenInUseAuthorization` kann auch übergeben werden, wenn Sie möchten die Benutzer bieten die Möglichkeit, können den Speicherort zugegriffen werden kann, nur, wenn die app im Vordergrund ist.
    - `AllowsBackgroundLocationUpdates` – Dies ist eine boolesche Eigenschaft, die in iOS 9, die festgelegt werden kann, können eine app zum Empfangen von speicherortaktualisierungen angehalten eingeführt.

    > [!IMPORTANT]
    > iOS 8 (und höher) erfordert auch einen Eintrag in der **"Info.plist"** Datei, die dem Benutzer angezeigt wird, als Teil der autorisierungsanforderung.

1. Fügen Sie einen Schlüssel `NSLocationAlwaysUsageDescription` oder `NSLocationWhenInUseUsageDescription` mit einer Zeichenfolge, die für den Benutzer in der Warnung angezeigt wird, die Zugriff auf den Standort-Daten anfordert.

1. iOS 9 erfordert, dass die Verwendung von `AllowsBackgroundLocationUpdates` der **"Info.plist"** enthält den Schlüssel `UIBackgroundModes` mit dem Wert `location`. Wenn Sie abgeschlossen haben, Schritt 2 dieser exemplarischen Vorgehensweise, dies sollte bereits in der Datei "Info.plist" wurden.


1. In der `LocationManager` Klasse, erstellen Sie eine Methode namens `StartLocationUpdates` durch den folgenden Code. Dieser Code zeigt, wie mit dem Empfang von Updates der Speicherort von der `CLLocationManager`:

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

    Es gibt einige wichtige Punkte, die in dieser Methode geschieht. Führen Sie es zuerst überprüft, ob die Anwendung den Zugriff auf Daten auf dem Gerät. Wir überprüfen dies durch den Aufruf `LocationServicesEnabled` auf die `CLLocationManager`. Diese Methode gibt **"false"** , wenn der Benutzer den Anwendungszugriff auf Standortinformationen verweigert hat.

1. Danach fest, dass der Quellspeicherort-Manager für Häufigkeit aktualisiert. `CLLocationManager` bietet viele Optionen zum Filtern und Daten, einschließlich der Häufigkeit von Updates zu konfigurieren. In diesem Beispiel legen Sie die `DesiredAccuracy` zum Aktualisieren von einer Änderung die Position. Weitere Informationen zum Konfigurieren der aktualisierungshäufigkeit der Speicherort und andere Einstellungen finden Sie in der [CLLocationManager Class Reference](https://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) in der Apple-Dokumentation.

1. Rufen Sie zum Schluss `StartUpdatingLocation` auf die `CLLocationManager` Instanz. Dies weist die Quellspeicherort-Manager an eine erste Lösung auf der aktuellen Position und Senden von Updates zu starten

Bisher die Quellspeicherort-Manager erstellt wurde, konfiguriert die Arten von Daten, die wir erhalten, möchten und die ursprüngliche Position festgestellt. Jetzt muss der Code die Positionsdaten, die Benutzeroberfläche zu rendern. Wir dazu ein benutzerdefiniertes Ereignis aus, die akzeptiert eine `CLLocation` als Argument:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

Der nächste Schritt besteht zum Abonnieren von speicherortaktualisierungen aus der `CLLocationManager`, und lösen Sie das benutzerdefinierte `LocationUpdated` Ereignis aus, wenn neue Daten verfügbar ist, werden der Speicherort als Argument übergeben. Zu diesem Zweck erstellen Sie eine neue Klasse **LocationUpdateEventArgs.cs**. Dieser Code ist die Hauptassembly der Anwendung zugegriffen werden kann und der Standort des Geräts zurückgegeben, wenn das Ereignis ausgelöst wird:

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

1. Verwenden Sie den iOS-Designer, um den Bildschirm zu erstellen, der Informationen zum Speicherort angezeigt werden. Doppelklicken Sie auf die **"Main.Storyboard"** Datei, um zu beginnen.

    Ziehen Sie auf dem Storyboard mehrere Bezeichnungen auf dem Bildschirm, das als Platzhalter für Informationen zum Speicherort der fungiert. In diesem Beispiel sind Bezeichnungen für Latitude, Longitude, Höhe, Kurs und Geschwindigkeit.

    Das Layout sollte folgendermaßen aussehen:

    ![](location-walkthrough-images/image8.png "Eine Beispiel-UI-Layout, in der iOS-Designer")

1. Doppelklicken Sie im Pad "Projektmappe", auf die `ViewController.cs` Datei, und bearbeiten können, um eine neue Instanz der LocationManager und rufen `StartLocationUpdates`darauf.
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

    Dadurch wird beim Start der Anwendung, die Speicherort-Updates gestartet, obwohl keine Daten angezeigt werden.

1. Nun, dass die Speicherort-Updates empfangen werden, den Bildschirm mit Informationen zum Speicherort der zu aktualisieren. Die folgende Methode ruft den Speicherort aus unserer `LocationUpdated` Ereignis und zeigt sie in der Benutzeroberfläche:

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

Wir benötigen zum Abonnieren der `LocationUpdated` Ereignis in unserem AppDelegate ein, und rufen Sie die neue Methode zur Aktualisierung der Benutzeroberfläche. Fügen Sie den folgenden Code in `ViewDidLoad,` direkt nach der `StartLocationUpdates` aufrufen:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```


Nun, wenn die Anwendung ausgeführt wird, sollte es etwa wie folgt aussehen:

[![](location-walkthrough-images/image5.png "Eine Beispiel-app-Ausführung")](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Behandeln von aktiv "und" Hintergrund "

1. Die Anwendung ist speicherortaktualisierungen ausgeben, während es im Vordergrund und aktiv ist. Um zu zeigen, was geschieht, wenn die Anwendung in den Hintergrund wechselt, überschreiben die `AppDelegate` Methoden, die Anwendung nachverfolgen-statusänderungen, damit die Anwendung in die Konsole schreibt, beim Übergang zwischen dem Vordergrund und Hintergrund:

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

    Fügen Sie den folgenden Code in die `LocationManager` ständig aktualisierten Speicherort drucken Daten in der Ausgabe der Anwendung, um zu überprüfen, ob die Informationen zum Speicherort im Hintergrund weiterhin verfügbar ist:

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

1. Ein verbleibenden mögliches Problem mit dem Code: Es wird versucht, die Benutzeroberfläche zu aktualisieren, wenn die app in diesem Fall werden Ursache iOS wird beenden Sie sie. Wenn die app in den Hintergrund wechselt, muss der Code speicherortaktualisierungen abbestellen, und beenden Sie die Aktualisierung der Benutzeroberflächenautomatisierungs.

    iOS bietet uns Benachrichtigungen, wenn die app für den Übergang in eine andere Anwendung geht Zustände. In diesem Fall können wir zum Abonnieren der `ObserveDidEnterBackground` Benachrichtigung.

    Der folgende Codeausschnitt veranschaulicht eine Benachrichtigung zu verwenden, um die Ansicht, die wissen, wann Aktualisierungen der Benutzeroberfläche anhalten zu ermöglichen. Dies geht `ViewDidLoad`:

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Wenn die app ausgeführt wird, wird die Ausgabe etwa wie folgt aussehen:

    ![](location-walkthrough-images/image6.png "Beispiel für den Ausgabespeicherort in der Konsole")

1. Die Anwendung speicherortaktualisierungen auf dem Bildschirm ausgegeben, wenn im Vordergrund ausgeführt, und weiterhin Daten an das Ausgabefenster für die Anwendung im Hintergrund drucken.

Nur ein ungelöstes Problem bleibt: der Bildschirm beginnt Aktualisierungen der Benutzeroberfläche aus, wenn die app zuerst geladen wird, aber sie hat keine Möglichkeit festzustellen, wann die app im Vordergrund erneut eingegeben hat. Aktualisierungen der Benutzeroberfläche und wenn die backgrounded Anwendung im Vordergrund wiederhergestellt wird, wird nicht fortgesetzt.

Um dieses Problem zu beheben, Schachteln Sie, einen Aufruf von Aktualisierungen der Benutzeroberfläche in eine weitere Benachrichtigung, starten Sie das ausgelöst wird, wenn die Anwendung den aktiven Zustand eintritt:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Jetzt wird die Benutzeroberfläche beginnen aktualisieren, wenn die Anwendung zuerst gestartet wird und fortsetzen, die jedes Mal die app aktualisiert zurück in den Vordergrund dann.

In dieser exemplarischen Vorgehensweise entwickelten wir eine kaum, Hintergrund-fähigen iOS-Anwendung, die sowohl auf dem Bildschirm im Ausgabefenster der Anwendung auf Daten ausgibt.


## <a name="related-links"></a>Verwandte Links

- [Speicherort (Teil 4) (Beispiel)](https://developer.xamarin.com/samples/monotouch/Location/)
- [Core-Framework Speicherortverweis](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
