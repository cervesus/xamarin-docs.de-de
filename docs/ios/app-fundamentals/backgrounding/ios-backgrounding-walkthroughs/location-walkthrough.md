---
title: 'Exemplarische Vorgehensweise: Hintergrund Speicherort in xamarin. IOS'
description: Dieses Dokument enthält eine exemplarische Vorgehensweise zur Verwendung von Standortinformationen in einer umgekehrten xamarin. IOS-Anwendung. Es werden die erforderlichen Setup-, Benutzeroberflächen-und Anwendungs Zustände beschrieben.
ms.prod: xamarin
ms.assetid: F8EEA0FD-5614-47FE-ADAC-80A5BCA6EB5F
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: bbb0dfbc9a6bf1396c8d517cc2c3289e2857a836
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86938397"
---
# <a name="walkthrough---background-location-in-xamarinios"></a>Exemplarische Vorgehensweise: Hintergrund Speicherort in xamarin. IOS

In diesem Beispiel erstellen wir eine IOS-Location-Anwendung, die Informationen zum aktuellen Speicherort ausgibt: Breitengrad, Längengrad und andere Parameter auf dem Bildschirm. Diese Anwendung veranschaulicht, wie Standort Aktualisierungen ordnungsgemäß durchgeführt werden, während die Anwendung aktiv oder mit einem Hintergrund ausgeführt wird.

In dieser exemplarischen Vorgehensweise werden einige wichtige Grundkonzepte erläutert, darunter das Registrieren einer App als Hintergrund erforderliche Anwendung, das Anhalten von Aktualisierungen der Benutzeroberfläche, wenn die APP sich im Hintergrund befindet, und das Arbeiten mit den `WillEnterBackground` `WillEnterForeground` `AppDelegate` Methoden und.

## <a name="application-set-up"></a>Anwendungs Einrichtung

1. Erstellen Sie zunächst eine neue **IOS-> App > Single View Application (c#)**. Nennen Sie ihn _Location_ , und stellen Sie sicher, dass sowohl iPad als auch iPhone ausgewählt wurden.

1. Eine Location-Anwendung ist als Hintergrund erforderliche Anwendung in ios qualifiziert. Registrieren Sie die Anwendung als Location-Anwendung, indem Sie die **Info. plist** -Datei für das Projekt bearbeiten.

    Doppelklicken Sie unter Projektmappen-Explorer auf die Datei " **Info. plist** ", um Sie zu öffnen, und Scrollen Sie zum Ende der Liste. Aktivieren Sie die Kontrollkästchen **hintergrundmodi aktivieren** und **Speicherort Updates** .

    In Visual Studio für Mac sieht es in etwa wie folgt aus:

    [![Aktivieren Sie die Kontrollkästchen hintergrundmodi aktivieren und Speicherort Updates.](location-walkthrough-images/image7.png)](location-walkthrough-images/image7.png#lightbox)

    In Visual Studio muss " **Info. plist** " manuell aktualisiert werden, indem das folgende Schlüssel-Wert-Paar hinzugefügt wird:

    ```xml
    <key>UIBackgroundModes</key>
    <array>
      <string>location</string>
    </array>
    ```

1. Nachdem die Anwendung registriert wurde, können Sie Standortdaten vom Gerät erhalten. In IOS wird die `CLLocationManager` -Klasse für den Zugriff auf Standortinformationen verwendet, und es können Ereignisse mit Standort Aktualisierungen bereitgestellt werden.

1. Erstellen Sie im Code eine neue Klasse mit dem Namen `LocationManager` , die eine einzige Stelle für verschiedene Bildschirme und Code zum Abonnieren von Standort Aktualisierungen bereitstellt. Erstellen Sie in der- `LocationManager` Klasse eine Instanz von mit dem `CLLocationManager` Namen `LocMgr` :

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

    Der obige Code legt eine Reihe von Eigenschaften und Berechtigungen für die Klasse [cllocationmanager](xref:CoreLocation.CLLocationManager) fest:

    - `PausesLocationUpdatesAutomatically`– Dies ist ein boolescher Wert, der abhängig davon festgelegt werden kann, ob das Systemstandort Aktualisierungen anhalten darf. Auf einigen Geräten wird standardmäßig auf festgelegt `true` , was dazu führen kann, dass das Gerät nach ungefähr 15 Minuten keine Updates für den Hintergrund Speicherort mehr erhält.
    - `RequestAlwaysAuthorization`-Sie sollten diese Methode übergeben, um dem App-Benutzer die Möglichkeit zu geben, den Zugriff auf den Speicherort im Hintergrund zuzulassen. `RequestWhenInUseAuthorization`kann auch übergeben werden, wenn Sie dem Benutzer die Möglichkeit geben möchten, den Zugriff auf den Speicherort nur zuzulassen, wenn sich die APP im Vordergrund befindet.
    - `AllowsBackgroundLocationUpdates`– Dies ist eine boolesche Eigenschaft, die in ios 9 eingeführt wurde und so festgelegt werden kann, dass eine APP bei angehaltenem Speicherort Updates empfangen kann.

    > [!IMPORTANT]
    > IOS 8 (und höher) erfordert auch einen Eintrag in der **Info. plist** -Datei, um den Benutzer als Teil der Autorisierungs Anforderung anzuzeigen.

1. Hinzufügen von " **Info. plist** "-Schlüsseln für die Berechtigungs Typen die APP benötigt – `NSLocationAlwaysUsageDescription` , `NSLocationWhenInUseUsageDescription` und/oder `NSLocationAlwaysAndWhenInUseUsageDescription` – mit einer Zeichenfolge, die dem Benutzer in der Warnung angezeigt wird, die den Zugriff auf Standortdaten anfordert.

1. IOS 9 erfordert, dass bei Verwendung `AllowsBackgroundLocationUpdates` von " **Info. plist** " der Schlüssel `UIBackgroundModes` mit dem Wert enthält `location` . Wenn Sie Schritt 2 dieser exemplarischen Vorgehensweise abgeschlossen haben, sollte diese bereits in der Datei "Info. plist" vorhanden sein.

1. Erstellen Sie in der- `LocationManager` Klasse eine Methode `StartLocationUpdates` mit dem Namen mit dem folgenden Code. Dieser Code zeigt, wie Sie mit dem empfangen von Speicherort Updates von beginnen `CLLocationManager` :

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

    Bei dieser Methode gibt es einige wichtige Dinge. Zuerst führen wir eine Prüfung durch, um festzustellen, ob die Anwendung Zugriff auf Standortdaten auf dem Gerät hat. Wir überprüfen dies durch Aufrufen von `LocationServicesEnabled` für `CLLocationManager` . Diese Methode gibt **false** zurück, wenn der Benutzer den Anwendungs Zugriff auf Standortinformationen verweigert hat.

1. Geben Sie als nächstes den Location Manager an, wie häufig aktualisiert werden soll. `CLLocationManager`bietet viele Optionen zum Filtern und Konfigurieren von Standortdaten, einschließlich der Häufigkeit von Updates. Legen Sie in diesem Beispiel den so fest, dass `DesiredAccuracy` immer dann aktualisiert wird, wenn sich der Speicherort um eine Meter Weitere Informationen zum Konfigurieren von Speicherort Aktualisierungshäufigkeit und anderen Einstellungen finden Sie in der [Referenz zur cllocationmanager-Klasse](https://developer.apple.com/library/ios/#documentation/CoreLocation/Reference/CLLocationManager_Class/CLLocationManager/CLLocationManager.html) in der Apple-Dokumentation.

1. Zum Schluss wird `StartUpdatingLocation` für die- `CLLocationManager` Instanz aufgerufen. Dadurch wird der Location Manager aufgefordert, eine anfängliche Korrektur am aktuellen Speicherort zu erhalten und mit dem Senden von Updates zu beginnen.

Bisher wurde der Location Manager erstellt, mit den Datentypen konfiguriert, die wir empfangen möchten, und hat den ursprünglichen Speicherort festgelegt. Der Code muss nun die Speicherort Daten auf der Benutzeroberfläche darstellen. Hierfür können Sie ein benutzerdefiniertes Ereignis verwenden, das ein `CLLocation` als Argument annimmt:

```csharp
// event for the location changing
public event EventHandler<LocationUpdatedEventArgs>LocationUpdated = delegate { };
```

Der nächste Schritt besteht darin, Location-Updates aus dem zu abonnieren `CLLocationManager` und das benutzerdefinierte `LocationUpdated` Ereignis zu erhöhen, wenn neue Standortdaten verfügbar werden, wobei der Speicherort als Argument übergeben wird. Erstellen Sie zu diesem Zweck eine neue Klasse **LocationUpdateEventArgs.cs**. Dieser Code ist innerhalb der Hauptanwendung zugänglich und gibt den Speicherort des Geräts zurück, wenn das Ereignis ausgelöst wird:

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

1. Verwenden Sie den IOS-Designer, um den Bildschirm zu erstellen, der Standortinformationen anzeigt. Doppelklicken Sie auf die Datei " **Main. Storyboard** ", um zu beginnen.

    Ziehen Sie auf dem Storyboard mehrere Bezeichnungen auf den Bildschirm, um als Platzhalter für die Speicherort Informationen zu fungieren. In diesem Beispiel gibt es Bezeichnungen für Breitengrad, Längengrad, Höhe, Kurs und Geschwindigkeit.

    Das Layout sollte etwa wie folgt aussehen:

    ![Ein Beispiel für ein Benutzeroberflächen Layout im IOS-Designer](location-walkthrough-images/image8.png)

1. Doppelklicken Sie im Lösungspad auf die `ViewController.cs` Datei, und bearbeiten Sie Sie, um eine neue Instanz von Locationmanager zu erstellen, und rufen Sie Sie `StartLocationUpdates` auf.
  Ändern Sie den Code so, dass er wie folgt aussieht:

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

    Dadurch werden die Standort Aktualisierungen beim Anwendungsstart gestartet, obwohl keine Daten angezeigt werden.

1. Nachdem Sie die Standort Aktualisierungen empfangen haben, aktualisieren Sie den Bildschirm mit den Informationen zum Speicherort. Mit der folgenden Methode wird der Speicherort aus dem `LocationUpdated` Ereignis abgerufen und in der Benutzeroberfläche angezeigt:

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

Wir müssen das `LocationUpdated` Ereignis weiterhin in unserem appdelegaten abonnieren und die neue Methode zum Aktualisieren der Benutzeroberfläche aufzurufen. Fügen Sie den folgenden Code `ViewDidLoad,` direkt nach dem-Befehl ein `StartLocationUpdates` :

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // It is better to handle this with notifications, so that the UI updates
    // resume when the application re-enters the foreground!
    Manager.LocationUpdated += HandleLocationChanged;

}
```

Wenn die Anwendung ausgeführt wird, sollte Sie nun etwa wie folgt aussehen:

[![Ein Beispiel für eine APP-Laufzeit](location-walkthrough-images/image5.png)](location-walkthrough-images/image5.png#lightbox)

## <a name="handling-active-and-background-states"></a>Behandeln von aktiven und Hintergrund Zuständen

1. Von der Anwendung werden Standort Aktualisierungen ausgegeben, während Sie sich im Vordergrund befinden und aktiv sind. Um zu veranschaulichen, was geschieht, wenn die app in den Hintergrund wechselt, überschreiben `AppDelegate` Sie die Methoden, die Änderungen des Anwendungs Zustands nachverfolgen, sodass die Anwendung in die Konsole schreibt, wenn Sie zwischen dem Vordergrund und dem Hintergrund übergeht:

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

    Fügen Sie den folgenden Code im hinzu `LocationManager` , um aktualisierte Positionsdaten fortlaufend in der Anwendungs Ausgabe auszugeben, um zu überprüfen, ob die Standortinformationen weiterhin im Hintergrund verfügbar sind:

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

1. Es gibt noch ein Problem mit dem Code: Wenn Sie versuchen, die Benutzeroberfläche zu aktualisieren, wenn die APP auf eine andere Weise erstellt wird, wird Sie von IOS beendet. Wenn die app in den Hintergrund wechselt, muss der Code die Standort Aktualisierungen kündigen und die Aktualisierung der Benutzeroberfläche beenden.

    IOS gibt uns Benachrichtigungen, wenn die APP im Begriff ist, zu einem anderen Anwendungs Status zu wechseln. In diesem Fall können wir die `ObserveDidEnterBackground` Benachrichtigung abonnieren.

    Der folgende Code Ausschnitt zeigt, wie Sie eine Benachrichtigung verwenden, um anzuzeigen, wann Aktualisierungen der Benutzeroberfläche angehalten werden sollen. Dies erfolgt in `ViewDidLoad` :

    ```csharp
    UIApplication.Notifications.ObserveDidEnterBackground ((sender, args) => {
        Manager.LocationUpdated -= HandleLocationChanged;
    });
    ```

    Wenn die app ausgeführt wird, sieht die Ausgabe in etwa wie folgt aus:

    ![Beispiel für die Ausgabe des Speicher Orts in der Konsole](location-walkthrough-images/image6.png)

1. Die Anwendung druckt Standort Aktualisierungen auf dem Bildschirm, wenn Sie im Vordergrund ausgeführt wird, und druckt weiterhin Daten im Anwendungs Ausgabefenster, während Sie im Hintergrund arbeiten.

Es bleibt nur ein ausstehendes Problem bestehen: auf dem Bildschirm werden Aktualisierungen der Benutzeroberfläche gestartet, wenn die APP zum ersten Mal geladen wird. Sie können jedoch nicht wissen, wann die APP den Vordergrund erneut eingegeben hat. Wenn die Back-End-Anwendung wieder in den Vordergrund gestellt wird, werden Aktualisierungen der Benutzeroberfläche nicht fortgesetzt.

Um dieses Problem zu beheben, Schachteln Sie einen Befehl zum Starten von UI-Updates in einer anderen Benachrichtigung, die ausgelöst wird, wenn die Anwendung in den aktiven Zustand wechselt:

```csharp
UIApplication.Notifications.ObserveDidBecomeActive ((sender, args) => {
  Manager.LocationUpdated += HandleLocationChanged;
});
```

Nun wird die Benutzeroberfläche aktualisiert, wenn die Anwendung zum ersten Mal gestartet wird, und die Aktualisierung fortsetzen, sobald die APP wieder im Vordergrund steht.

In dieser exemplarischen Vorgehensweise haben wir eine gut verhaltene, Hintergrund fähige IOS-Anwendung erstellt, die Standortdaten sowohl auf dem Bildschirm als auch im Anwendungs Ausgabefenster ausgibt.

## <a name="related-links"></a>Verwandte Links

- [Standort (Teil 4) (Beispiel)](https://docs.microsoft.com/samples/xamarin/ios-samples/location)
- [Core Location Framework-Referenz](https://developer.apple.com/library/ios/documentation/CoreLocation/Reference/CoreLocation_Framework/_index.html)
