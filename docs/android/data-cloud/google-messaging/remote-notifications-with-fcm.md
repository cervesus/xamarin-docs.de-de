---
title: Remote Benachrichtigungen mit Firebase Cloud Messaging
description: Diese exemplarische Vorgehensweise enthält eine schrittweise Erläuterung zur Verwendung von Firebase Cloud Messaging zum Implementieren von Remote Benachrichtigungen (auch als Pushbenachrichtigungen bezeichnet) in einer xamarin. Android-Anwendung. Es wird veranschaulicht, wie die verschiedenen Klassen implementiert werden, die für die Kommunikation mit Firebase Cloud Messaging (FCM) benötigt werden, und es werden Beispiele für die Konfiguration des Android-Manifests für den Zugriff auf den FCM und das downstreammessaging mithilfe von Firebase veranschaulicht. TS.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: 3837e28fa657764312cdbe379ba66caf9ccf18a4
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/30/2019
ms.locfileid: "68644204"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Remote Benachrichtigungen mit Firebase Cloud Messaging

_Diese exemplarische Vorgehensweise enthält eine schrittweise Erläuterung zur Verwendung von Firebase Cloud Messaging zum Implementieren von Remote Benachrichtigungen (auch als Pushbenachrichtigungen bezeichnet) in einer xamarin. Android-Anwendung. Es wird veranschaulicht, wie die verschiedenen Klassen implementiert werden, die für die Kommunikation mit Firebase Cloud Messaging (FCM) benötigt werden, und es werden Beispiele für die Konfiguration des Android-Manifests für den Zugriff auf den FCM und das downstreammessaging mithilfe von Firebase veranschaulicht. TS._

## <a name="fcm-notifications-overview"></a>Übersicht über die Übersicht über die Übersicht

In dieser exemplarischen Vorgehensweise wird eine einfache APP mit dem Namen **fcmclient** erstellt, um die Grundlagen von FCM-Messaging zu veranschaulichen. **Fcmclient** prüft, ob Google Play Services vorhanden ist, empfängt Registrierungs Token von FCM, zeigt Remote Benachrichtigungen an, die Sie über die Firebase-Konsole senden, und abonniert Themen Meldungen:

[![Screenshot der APP mit Beispiel](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Die folgenden Themenbereiche werden untersucht:

1.  Hintergrund Benachrichtigungen

2.  Themen Meldungen

3.  Vordergrund Benachrichtigungen

In dieser exemplarischen Vorgehensweise fügen Sie dem **fcmclient** inkrementelle Funktionen hinzu und führen Sie auf einem Gerät oder Emulator aus, um zu verstehen, wie es mit FCM interagiert. Sie verwenden die Protokollierung, um Live-App-Transaktionen mit FCM-Servern zu Zeugen, und Sie beobachten, wie Benachrichtigungen von FCM-Nachrichten generiert werden, die Sie in die Benutzeroberfläche der Firebase-Konsolen Benachrichtigungen eingeben.

## <a name="requirements"></a>Anforderungen


Es ist hilfreich, sich mit den [verschiedenen Arten von Nachrichten](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) vertraut zu machen, die von Firebase Cloud Messaging gesendet werden können. Die Nutzlast der Nachricht bestimmt, wie die Nachricht von einer Client-App empfangen und verarbeitet wird.

Bevor Sie mit dieser exemplarischen Vorgehensweise fortfahren können, müssen Sie die erforderlichen Anmelde Informationen für die Verwendung der Google-FCM-Server erwerben. Dieser Prozess wird in [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm)erläutert.
Insbesondere müssen Sie die Datei **Google-Services. JSON** herunterladen, die mit dem in dieser exemplarischen Vorgehensweise dargestellten Beispielcode verwendet werden kann. Wenn Sie noch kein Projekt in der Firebase-Konsole erstellt haben (oder die Datei " **Google-Services. JSON** " noch nicht heruntergeladen haben), lesen Sie [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

Zum Ausführen der Beispiel-App benötigen Sie ein Android-Testgerät oder einen Emulator, das mit Firebase kompatibel ist. Firebase Cloud Messaging unterstützt Clients, die unter Android 4,0 oder höher ausgeführt werden, und auf diesen Geräten muss außerdem die Google Play Store-App installiert sein (Google Play Services 9.2.1 oder höher erforderlich). Wenn Sie die Google Play Store-App noch nicht auf Ihrem Gerät installiert haben, besuchen Sie die [Google Play](https://support.google.com/googleplay) Website, um sie herunterzuladen und zu installieren. Alternativ können Sie den Android SDK Emulator mit installierter Google Play Services anstelle eines Testgeräts verwenden (Sie müssen die Google Play Store nicht installieren, wenn Sie den Android SDK Emulator verwenden).

## <a name="start-an-app-project"></a>Starten eines App-Projekts

Erstellen Sie zunächst ein neues leeres xamarin. Android-Projekt mit dem Namen **fcmclient**. Wenn Sie mit dem Erstellen von xamarin. Android-Projekten nicht vertraut sind, finden Sie weitere Informationen unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).
Nachdem die neue App erstellt wurde, besteht der nächste Schritt darin, den Paketnamen festzulegen und mehrere nuget-Pakete zu installieren, die für die Kommunikation mit FCM verwendet werden.

### <a name="set-the-package-name"></a>Festlegen des Paket namens

In [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)haben Sie einen Paketnamen für die APP mit aktiviertem SCM angegeben. Dieser Paketname dient auch als [*Anwendungs-ID*](./firebase-cloud-messaging.md#fcm-in-action-app-id) , die dem [API-Schlüssel](firebase-cloud-messaging.md#fcm-in-action-api-key)zugeordnet ist. Konfigurieren Sie die APP für die Verwendung dieses Paket namens:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Öffnen Sie die Eigenschaften für das Projekt " **f-Client** ".

2.  Legen Sie auf der Seite **Android-Manifest** den Paketnamen fest.

Im folgenden Beispiel wird der Paketname auf `com.xamarin.fcmexample`festgelegt:

[![Festlegen des Paket namens](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Wenn Sie das Android- **Manifest**aktualisieren, sollten Sie auch überprüfen, ob `Internet` die Berechtigung aktiviert ist.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Öffnen Sie die Eigenschaften für das Projekt " **f-Client** ".

2.  Legen Sie auf der Seite **Android-Anwendung** den Paketnamen fest.

Im folgenden Beispiel wird der Paketname auf `com.xamarin.fcmexample`festgelegt:

[![Festlegen des Paket namens](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Wenn Sie das Android- **Manifest**aktualisieren, sollten Sie auch überprüfen, ob `INTERNET` die Berechtigung aktiviert ist (unter **erforderliche Berechtigungen**).

-----

> [!IMPORTANT]
> Die Client-App kann kein Registrierungs Token von FCM empfangen, wenn dieser Paketname nicht *exakt* mit dem Paketnamen übereinstimmt, der in die Firebase-Konsole eingegeben wurde.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Hinzufügen des xamarin Google Play Services Basispakets

Da Firebase Cloud Messaging von Google Play Services abhängig ist, muss dem xamarin. Android-Projekt das [xamarin Google Play Services-Base-](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) nuget-Paket hinzugefügt werden. Sie benötigen Version 29.0.0.2 oder höher.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Klicken Sie in Visual Studio mit der rechten Maustaste auf **Verweise > nuget-Pakete verwalten...** .

2.  Klicken Sie auf die Registerkarte **Durchsuchen** , und suchen Sie nach **xamarin. googleplayservices. Base**.

3.  Installieren Sie dieses Paket im Projekt " **f cmclient** ":

    [![Installieren von Google Play Services Basis](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...** .

2.  Suchen Sie nach **xamarin. googleplayservices. Base**.

3.  Installieren Sie dieses Paket im Projekt " **f cmclient** ":

    [![Installieren von Google Play Services Basis](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

Wenn bei der Installation des nuget-Fehlers ein Fehler auftritt, schließen Sie das Projekt " **f** ", öffnen Sie es erneut, und wiederholen Sie die Installation von nuget.

Wenn Sie **xamarin. googleplayservices. Base**installieren, werden alle notwendigen Abhängigkeiten ebenfalls installiert. Bearbeiten Sie **MainActivity.cs** , und fügen `using` Sie die folgende Anweisung hinzu:

```csharp
using Android.Gms.Common;
```

Diese Anweisung macht die `GoogleApiAvailability` -Klasse in **xamarin. googleplayservices. Base** für den **fcmclient** -Code verfügbar.
`GoogleApiAvailability`wird verwendet, um zu überprüfen, ob Google Play Services vorhanden ist.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Hinzufügen des xamarin Firebase-Messaging Pakets

Zum Empfangen von Nachrichten von FCM muss das nuget [-Paket xamarin Firebase-Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) dem App-Projekt hinzugefügt werden. Ohne dieses Paket kann eine Android-Anwendung keine Nachrichten von einem Server empfangen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Klicken Sie in Visual Studio mit der rechten Maustaste auf **Verweise > nuget-Pakete verwalten...** .

2. Suchen Sie nach **xamarin. Firebase. Messaging**.

3.  Installieren Sie dieses Paket im Projekt " **f cmclient** ":

    [![Installieren von xamarin Firebase-Messaging](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Klicken Sie in Visual Studio für Mac mit der rechten Maustaste auf **Pakete > Pakete hinzufügen...** .

2.  Suchen Sie nach **xamarin. Firebase. Messaging**.

3.  Installieren Sie dieses Paket im Projekt " **f cmclient** ":

    [![Installieren von xamarin Firebase-Messaging](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

Wenn Sie **xamarin. Firebase. Messaging**installieren, werden alle notwendigen Abhängigkeiten ebenfalls installiert.

Bearbeiten Sie als nächstes **MainActivity.cs** , und fügen `using` Sie die folgenden Anweisungen hinzu:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Die ersten beiden-Anweisungen machen Typen im **xamarin. Firebase. Messaging** -nuget-Paket für den **fcmclient** -Code verfügbar. **Android. util** fügt Protokollierungsfunktionen hinzu, die zum Beobachten von Transaktionen mit FMS verwendet werden.

### <a name="add-googleplayservices-json"></a>Hinzufügen der JSON-Datei für Google-Dienste

Der nächste Schritt besteht darin, die Datei " **Google-Services. JSON** " dem Stammverzeichnis des Projekts hinzuzufügen:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Kopieren Sie die **Datei Google-Services. JSON** in den Projektordner.

2.  Fügen Sie **Google-Services. JSON** dem App-Projekt hinzu (Klicken Sie im **Projektmappen-Explorer**auf **alle Dateien anzeigen** , klicken Sie mit der rechten Maustaste auf **Google-Services. JSON**, und wählen Sie dann **in Projekt einschließen aus**.)

3.  Wählen Sie im Fenster **Projektmappen-Explorer** die Option **Google-Services. JSON** aus.

4.  Legen Sie im Bereich **Eigenschaften** die Buildaktion auf **googleservicesjson**fest:

    [![Festlegen der Buildaktion auf googleservicesjson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

    > [!NOTE] 
    > Wenn die Buildaktion **googleservicesjson** nicht angezeigt wird, speichern und schließen Sie die Projekt Mappe, und öffnen Sie Sie erneut.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Kopieren Sie die **Datei Google-Services. JSON** in den Projektordner.

2.  Fügen Sie dem App-Projekt die **Datei Google-Services. JSON** hinzu.

3.  Klicken Sie mit der rechten Maustaste auf **Google-Services. JSON**.

4.  Legen Sie die Buildaktion auf **googleservicesjson**fest:

    [![Festlegen der Buildaktion auf googleservicesjson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

Wenn " **Google-Services. JSON** " zum Projekt hinzugefügt wird (und die **googleservicesjson** -Buildaktion festgelegt ist), extrahiert der Buildprozess die Client-ID und den [API-Schlüssel](./firebase-cloud-messaging.md#fcm-in-action-api-key) und fügt diese Anmelde Informationen dann dem zusammengeführten/generierten  **"Androidmanifest. XML** " befindet sich in " **obj/Debug/Android/androidmanifest. XML**". Bei diesem Mergeprozess werden automatisch alle Berechtigungen und anderen FCM-Elemente hinzugefügt, die für die Verbindung mit FCM-Servern erforderlich sind.


## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Überprüfen auf Google Play Services und Erstellen eines Benachrichtigungs Kanals

Google empfiehlt, dass Android-Apps vor dem Zugriff auf Google Play Services Features auf das vorhanden sein des Google Play Services APK überprüfen (Weitere Informationen finden [Sie unter Überprüfen auf Google Play Services](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)).

Zuerst wird ein erstes Layout für die Benutzeroberfläche der App erstellt. Bearbeiten Sie **Resources/Layout/Main. axml** , und ersetzen Sie den Inhalt durch den folgenden XML-Code:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="10dp">
    <TextView
        android:text=" "
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:id="@+id/msgText"
        android:textAppearance="?android:attr/textAppearanceMedium"
        android:padding="10dp" />
</LinearLayout>
```

Diese `TextView` wird verwendet, um Meldungen anzuzeigen, die angeben, ob Google Play Services installiert ist. Speichern Sie die Änderungen in " **Main. axml**".


Bearbeiten Sie **MainActivity.cs** , `MainActivity` und fügen Sie der-Klasse die folgenden-Instanzvariablen hinzu:

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

Die Variablen `CHANNEL_ID` und `NOTIFICATION_ID` werden in der-Methode verwendet [`CreateNotificationChannel`](#create-notification-channel-code) , die `MainActivity` später in dieser exemplarischen Vorgehensweise hinzugefügt wird.


Im folgenden Beispiel überprüft die `OnCreate` -Methode, ob Google Play Services verfügbar ist, bevor die APP versucht, FCM-Dienste zu verwenden.
Fügen Sie der- `MainActivity` Klasse die folgende Methode hinzu:

```csharp
public bool IsPlayServicesAvailable ()
{
    int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable (this);
    if (resultCode != ConnectionResult.Success)
    {
        if (GoogleApiAvailability.Instance.IsUserResolvableError (resultCode))
            msgText.Text = GoogleApiAvailability.Instance.GetErrorString (resultCode);
        else
        {
            msgText.Text = "This device is not supported";
            Finish ();
        }
        return false;
    }
    else
    {
        msgText.Text = "Google Play Services is available.";
        return true;
    }
}
```

Dieser Code überprüft das Gerät, um festzustellen, ob das Google Play Services APK installiert ist. Wenn Sie nicht installiert ist, wird in der eine Meldung angezeigt `TextBox` , in der der Benutzer angewiesen wird, ein APK vom Google Play Store herunterzuladen (oder in den Systemeinstellungen des Geräts zu aktivieren).

<a name="create-notification-channel-code"></a>Apps, die unter Android 8,0 (API-Ebene 26) oder höher ausgeführt werden, müssen einen [_Benachrichtigungs Kanal_](~/android/app-fundamentals/notifications/local-notifications.md) zum Veröffentlichen Ihrer Benachrichtigungen erstellen.  Fügen Sie der- `MainActivity` Klasse die folgende Methode hinzu, die den Benachrichtigungs Kanal erstellt (falls erforderlich):

```csharp
void CreateNotificationChannel()
{
    if (Build.VERSION.SdkInt < BuildVersionCodes.O)
    {
        // Notification channels are new in API 26 (and not a part of the
        // support library). There is no need to create a notification
        // channel on older versions of Android.
        return;
    }

    var channel = new NotificationChannel(CHANNEL_ID,
                                          "FCM Notifications",
                                          NotificationImportance.Default)
                  {

                      Description = "Firebase Cloud Messages appear in this channel"
                  };

    var notificationManager = (NotificationManager)GetSystemService(Android.Content.Context.NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Ersetzen Sie die `OnCreate`-Methode durch folgenden Code:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);
    SetContentView (Resource.Layout.Main);
    msgText = FindViewById<TextView> (Resource.Id.msgText);

    IsPlayServicesAvailable ();

    CreateNotificationChannel();
}
```

`IsPlayServicesAvailable`wird am Ende von `OnCreate` aufgerufen, sodass die Google Play Services Prüfung bei jedem Start der app ausgeführt wird. Die- `CreateNotificationChannel` Methode wird aufgerufen, um sicherzustellen, dass ein Benachrichtigungs Kanal für Geräte mit Android 8 oder höher vorhanden ist. Wenn Ihre APP über eine `OnResume` -Methode verfügt, sollte `IsPlayServicesAvailable` Sie `OnResume` auch von aufgerufen werden. Vollständiges Neuerstellen und Ausführen der app. Wenn alles ordnungsgemäß konfiguriert ist, sollte ein Bildschirm angezeigt werden, der dem folgenden Screenshot ähnelt:

[![App gibt an, dass Google Play Services verfügbar ist.](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Wenn Sie dieses Ergebnis nicht erhalten, vergewissern Sie sich, dass das Google Play Services APK auf Ihrem Gerät installiert ist (Weitere Informationen finden Sie unter [einrichten Google Play Services](https://developers.google.com/android/guides/setup)).
Vergewissern Sie sich außerdem, dass Sie das **xamarin. Google. Play. Services. Base** -Paket zum **fcmclient** -Projekt hinzugefügt haben, wie bereits erläutert.


## <a name="add-the-instance-id-receiver"></a>Instanz-ID-Empfänger hinzufügen

Der nächste Schritt besteht darin, einen Dienst hinzuzufügen `FirebaseInstanceIdService` , der erweitert, um das Erstellen, drehen und Aktualisieren von [Firebase-Registrierungs Token](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token)zu verarbeiten. Der `FirebaseInstanceIdService` Dienst ist erforderlich, damit FCM Nachrichten an das Gerät senden kann. Wenn der `FirebaseInstanceIdService` Dienst der Client-app hinzugefügt wird, empfängt die APP automatisch FCM-Nachrichten und zeigt Sie immer dann als Benachrichtigungen an, wenn die APP sich im Hintergrund befindet.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Deklarieren des Empfängers im Android-Manifest

Bearbeiten Sie " **androidmanifest. XML** ", `<receiver>` und fügen Sie `<application>` die folgenden Elemente in den Abschnitt ein:

```xml
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver"
    android:exported="false" />
<receiver
    android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver"
    android:exported="true"
    android:permission="com.google.android.c2dm.permission.SEND">
    <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
    </intent-filter>
</receiver>
```

Dieser XML-Code führt folgende Schritte aus:

-   Deklariert eine `FirebaseInstanceIdReceiver` -Implementierung, die einen [eindeutigen Bezeichner](https://developers.google.com/instance-id/) für jede app-Instanz bereitstellt. Dieser Empfänger ist außerdem authentifiziert und autorisiert Aktionen.

-   Deklariert eine interne `FirebaseInstanceIdInternalReceiver` -Implementierung, die verwendet wird, um Dienste sicher zu starten.

-   Die [App-ID](./firebase-cloud-messaging.md#fcm-in-action-app-id) wird in der Datei " **Google-Services. JSON** " gespeichert, die [dem Projekt hinzugefügt](#add-googleplayservices-json)wurde. Die xamarin. Android-Firebase-Bindungen ersetzen das `${applicationId}` Token durch die APP-ID. es ist kein zusätzlicher Code für die Client-App erforderlich, um die APP-ID bereitzustellen.

`FirebaseInstanceIdService` `FirebaseInstanceId` Ist eine `WakefulBroadcastReceiver` , die-und `FirebaseMessaging` -Ereignisse empfängt und Sie an die Klasse übergibt, von der Sie abgeleitet sind. `FirebaseInstanceIdReceiver`

### <a name="implement-the-firebase-instance-id-service"></a>Implementieren Sie den Firebase-Instanz-ID-Dienst.

Der von Ihnen bereitgestellte benutzerdefinierte `FirebaseInstanceIdService` Dienst wird durch die Registrierung der Anwendung bei der Verwendung von ficm behandelt.
`FirebaseInstanceIdService`führt die folgenden Schritte aus:

1.  Verwendet die [Instanz-ID-API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) zum Generieren von Sicherheits Token, die die Client-App für den Zugriff auf FCM und den App-Server autorisieren. In der Rückgabe erhält die APP ein [Registrierungs Token](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) von einem anderen Dienst.

2.  Leitet das Registrierungs Token an den App-Server weiter, wenn es vom App-Server benötigt wird.

Fügen Sie eine neue Datei mit dem Namen **MyFirebaseIIDService.cs** hinzu, und ersetzen Sie den Vorlagen Code durch Folgendes:

```csharp
using System;
using Android.App;
using Firebase.Iid;
using Android.Util;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
    public class MyFirebaseIIDService : FirebaseInstanceIdService
    {
        const string TAG = "MyFirebaseIIDService";
        public override void OnTokenRefresh()
        {
            var refreshedToken = FirebaseInstanceId.Instance.Token;
            Log.Debug(TAG, "Refreshed token: " + refreshedToken);
            SendRegistrationToServer(refreshedToken);
        }
        void SendRegistrationToServer(string token)
        {
            // Add custom implementation, as needed.
        }
    }
}
```

Dieser Dienst implementiert eine `OnTokenRefresh` Methode, die aufgerufen wird, wenn das Registrierungs Token anfänglich erstellt oder geändert wird. Wenn `OnTokenRefresh` ausgeführt wird, wird das neueste Token aus der- `FirebaseInstanceId.Instance.Token` Eigenschaft abgerufen (die asynchron von der Vollversion von vollständig aktualisiert wird). In diesem Beispiel wird das aktualisierte Token protokolliert, sodass es im Ausgabefenster angezeigt werden kann:

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh`wird nur selten aufgerufen: Sie wird verwendet, um das Token unter den folgenden Umständen zu aktualisieren:

-   Wenn die APP installiert oder deinstalliert wird.

-   Wenn der Benutzer App-Daten löscht.

-   Wenn die APP die Instanz-ID löscht.

-   Wenn die Sicherheit des Tokens kompromittiert wurde.

Gemäß der Google- [Instanz-ID](https://developers.google.com/instance-id/guides/android-implementation) -Dokumentation fordert der FCM-Instanz-ID-Dienst an, dass die APP das Token regelmäßig aktualisiert (in der Regel alle 6 Monate).

`OnTokenRefresh`Außerdem wird `SendRegistrationToAppServer` von aufgerufen, um dem serverseitigen Konto (sofern vorhanden), das von der Anwendung verwaltet wird, das Registrierungs Token des Benutzers zuzuordnen:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Da diese Implementierung vom Entwurf des App-Servers abhängt, wird in diesem Beispiel ein leerer Methoden Text bereitgestellt. Wenn Ihr App-Server FCM-Registrierungsinformationen erfordert `SendRegistrationToAppServer` , ändern Sie, um das FCM-Instanz-ID-Token des Benutzers einem beliebigen serverseitigen Konto zuzuordnen, das von Ihrer APP verwaltet wird. (Beachten Sie, dass das Token für die Client-App nicht transparent ist.)

Wenn ein Token an den App-Server gesendet wird `SendRegistrationToAppServer` , muss einen booleschen Wert aufbewahren, um anzugeben, ob das Token an den Server gesendet wurde. Wenn dieser boolesche Wert false ist `SendRegistrationToAppServer` , sendet das Token an den APP &ndash; -Server, andernfalls wurde das Token bereits in einem vorherigen-Befehl an den App-Server gesendet. In einigen Fällen (z. b `FCMClient` . in diesem Beispiel) benötigt der App-Server das Token nicht. Daher ist diese Methode für dieses Beispiel nicht erforderlich.

## <a name="implement-client-app-code"></a>Implementieren von Client-App-Code

Nachdem die Empfänger Dienste eingerichtet sind, kann der Client-App-Code geschrieben werden, um diese Dienste zu nutzen. In den folgenden Abschnitten wird der Benutzeroberfläche eine Schaltfläche hinzugefügt, um das Registrierungs Token (auch als *Instanz-ID-Token*bezeichnet) zu protokollieren, `MainActivity` und es wird mehr Code hinzugefügt, um Informationen anzuzeigen `Intent` , wenn die APP aus einer Benachrichtigung gestartet wird:

[![Schaltfläche zum Hinzufügen des Protokolls zum App-Bildschirm](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>Protokoll Token

Der in diesem Schritt hinzugefügte Code ist nur für Demonstrations &ndash; Zwecke gedacht. eine Produktions Client-App wäre nicht zum Protokollieren von Registrierungs Token erforderlich. Bearbeiten Sie **Resources/Layout/Main. axml** , und fügen `Button` Sie die folgende Deklaration direkt nach dem `TextView` -Element hinzu:

```xml
<Button
  android:id="@+id/logTokenButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:text="Log Token" />
```

Fügen Sie den folgenden Code am Ende der `MainActivity.OnCreate`-Methode hinzu:

```csharp
var logTokenButton = FindViewById<Button>(Resource.Id.logTokenButton);
logTokenButton.Click += delegate {
    Log.Debug(TAG, "InstanceID token: " + FirebaseInstanceId.Instance.Token);
};
```

Mit diesem Code wird das aktuelle Token im Ausgabefenster protokolliert, wenn die Schaltfläche **Protokoll Token** getippt wird.

### <a name="handle-notification-intents"></a>Behandeln von Benachrichtigungs Intents

Wenn der Benutzer auf eine von **fcmclient**ausgegebene Benachrichtigung tippt, werden alle Daten, die mit dieser Benachrichtigungs Meldung einhergehen, in `Intent` Extras zur Verfügung gestellt. Bearbeiten Sie **MainActivity.cs** , und fügen Sie den folgenden Code am Anfang `OnCreate` der Methode ( `IsPlayServicesAvailable`vor dem-Befehl) ein:

```csharp
if (Intent.Extras != null)
{
    foreach (var key in Intent.Extras.KeySet())
    {
        var value = Intent.Extras.GetString(key);
        Log.Debug(TAG, "Key: {0} Value: {1}", key, value);
    }
}
```

Das Start Programm der `Intent` APP wird ausgelöst, wenn der Benutzer `Intent` auf seine Benachrichtigungs Meldung tippt, sodass dieser Code alle zugehörigen Daten im in das Ausgabefenster protokolliert. Wenn ein anderes `Intent` ausgelöst werden muss, muss `click_action` das Feld der Benachrichtigungs Meldung `Intent` auf dieses `Intent` festgelegt werden (das Start Programm wird verwendet `click_action` , wenn kein angegeben ist).


## <a name="background-notifications"></a>Hintergrund Benachrichtigungen

Erstellen Sie die app " **f** ", und führen Sie Sie aus. Die Schaltfläche **Protokoll Token** wird angezeigt:

[![Schaltfläche "Protokoll Token" wird angezeigt](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Tippen Sie auf die Schaltfläche **Protokoll Token** . Im Fenster IDE-Ausgabe sollte eine Meldung wie die folgende angezeigt werden:

[![Im Ausgabefenster angezeigtes Instanz-ID-Token](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

Die lange Zeichenfolge mit **Token** ist das Instanz-ID-Token, das Sie in die Firebase &ndash; -Konsole einfügen, wählen Sie diese Zeichenfolge in die Zwischenablage kopieren aus. Wenn ein Instanz-ID-Token nicht angezeigt wird, fügen Sie die folgende Zeile am Anfang `OnCreate` der Methode hinzu, um zu überprüfen, ob " **Google-Services. JSON** " ordnungsgemäß analysiert wurde:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

Der `google_app_id` im Ausgabefenster protokollierte Wert sollte mit dem `mobilesdk_app_id` in **Google-Services. JSON**aufgezeichneten Wert identisch sein.

### <a name="send-a-message"></a>Nachricht senden

Melden Sie sich bei der [Firebase-Konsole](https://console.firebase.google.com)an, wählen Sie Ihr Projekt aus, klicken Sie auf **Benachrichtigungen**, und klicken Sie auf **Nachricht senden**:

[![Schaltfläche "erste Nachricht senden"](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Geben Sie auf der Seite **Nachricht verfassen** den Nachrichtentext ein, und wählen Sie **einzelnes Gerät**aus. Kopieren Sie das Instanz-ID-Token aus dem IDE-Ausgabefenster, und fügen Sie es in das Feld "BCM- **Registrierungs Token** " der Firebase-Konsole:

[![Dialogfeld Nachricht verfassen](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Im Hintergrund der APP auf dem Android-Gerät (oder dem Emulator) klicken Sie auf die Schaltfläche Android **Overview (Übersicht über** Android) und berühren den Startbildschirm. Wenn das Gerät bereit ist, klicken Sie in der Firebase-Konsole auf **Nachricht senden** :

[![Schaltfläche Nachricht senden](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Wenn das Dialogfeld **Nachricht überprüfen** angezeigt wird, klicken Sie auf **senden**.
Das Benachrichtigungssymbol sollte im Benachrichtigungsbereich des Geräts (oder Emulators) angezeigt werden:

[![Das Benachrichtigungssymbol wird angezeigt.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

Öffnen Sie das Benachrichtigungssymbol, um die Meldung anzuzeigen. Die Benachrichtigungs Meldung sollte genau so eingegeben werden, wie Sie in das Feld **Nachrichtentext** der Firebase-Konsole eingegeben wurde:

[![Die Benachrichtigungs Meldung wird auf dem Gerät angezeigt.](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Tippen Sie auf das Benachrichtigungssymbol, um die **fcmclient** -APP zu starten. Die `Intent` an **fcmclient** gesendeten Extras werden im Fenster IDE-Ausgabe aufgeführt:

[![Beabsichtigte Extras Listen aus Key, Message ID und Collapse Key](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

In diesem Beispiel wird der **from** -Schlüssel auf die Firebase-Projekt Nummer der APP (in diesem Beispiel `41590732`) festgelegt, und **collapse_key** wird auf den zugehörigen Paketnamen (**com. xamarin. fcmexample**) festgelegt.
Wenn Sie keine Meldung erhalten, löschen Sie die Anwendung " **f** " auf dem Gerät (oder dem Emulator), und wiederholen Sie die obigen Schritte.


> [!NOTE]
> Wenn Sie das Schließen der APP erzwingen, wird die Übermittlung von Benachrichtigungen durch den ficm beendet. Android verhindert, dass Hintergrunddienst Übertragungen versehentlich oder unnötig Komponenten von beendeten Anwendungen starten. (Weitere Informationen zu diesem Verhalten finden Sie unter [Starten von Steuerelementen](https://developer.android.com/about/versions/android-3.1.html#launchcontrols)für beendete Anwendungen.) Aus diesem Grund ist es erforderlich, die APP jedes Mal manuell zu deinstallieren, wenn Sie Sie ausführen, und Sie aus &ndash; einer Debugsitzung zu entfernen. dadurch erzwingt FCM, dass ein neues Token generiert wird, damit Nachrichten weiterhin empfangen werden.

### <a name="add-a-custom-default-notification-icon"></a>Hinzufügen eines benutzerdefinierten Standard Benachrichtigungs Symbols

Im vorherigen Beispiel wird das Benachrichtigungssymbol auf das Anwendungssymbol festgelegt. Mit dem folgenden XML-Code wird ein benutzerdefiniertes Standard Symbol für Benachrichtigungen konfiguriert. Android zeigt dieses benutzerdefinierte Standard Symbol für alle Benachrichtigungs Meldungen an, bei denen das Benachrichtigungssymbol nicht explizit festgelegt ist.

Fügen Sie zum Hinzufügen eines benutzerdefinierten Standard Benachrichtigungs Symbols das Symbol zum Verzeichnis **Resources/drawable** hinzu, bearbeiten Sie die Datei " **androidmanifest. XML**" `<application>` , und fügen Sie das folgende `<meta-data>` Element in den Abschnitt ein:

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

In diesem Beispiel wird das Benachrichtigungssymbol, das sich unter **Resources/drawable\_/\_IC\_stat IC Notification. png** befindet, als benutzerdefiniertes Standard Benachrichtigungssymbol verwendet. Wenn ein benutzerdefiniertes Standard Symbol in " **androidmanifest. XML** " nicht konfiguriert ist und in der Benachrichtigungs Nutzlast kein Symbol festgelegt ist, verwendet Android das Anwendungssymbol als Benachrichtigungssymbol (wie im obigen Benachrichtigungssymbol Screenshot zu sehen).

## <a name="handle-topic-messages"></a>Behandeln von Themen Meldungen

Der bisher geschriebene Code behandelt Registrierungs Token und fügt der APP Remote Benachrichtigungsfunktionen hinzu. Im nächsten Beispiel wird Code hinzugefügt, der nach *Thema Nachrichten* lauscht und Sie als Remote Benachrichtigungen an den Benutzer weiterleitet. Themen Nachrichten sind FCM-Nachrichten, die an ein oder mehrere Geräte gesendet werden, die ein bestimmtes Thema abonnieren. Weitere Informationen zu Thema Nachrichten finden Sie unter [Topic Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

### <a name="subscribe-to-a-topic"></a>Abonnieren eines Themas

Bearbeiten Sie **Resources/Layout/Main. axml** , und fügen `Button` Sie die folgende Deklaration `Button` direkt nach dem vorherigen Element hinzu:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Mit diesem XML-Code wird dem Layout die Schaltfläche **Benachrichtigung abonnieren** hinzugefügt.
Bearbeiten Sie **MainActivity.cs** , und fügen Sie den folgenden Code am Ende `OnCreate` der-Methode hinzu:

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Mit diesem Code wird die Schaltfläche " **Abonnieren von Benachrichtigungen** " im Layout angezeigt, und dem Code, der `FirebaseMessaging.Instance.SubscribeToTopic`aufruft, wird der Click-Handler _zugewiesen._ Wenn der Benutzer auf die Schaltfläche **abonnieren** tippt, abonniert die APP das _Nachrichten_ Thema. Im folgenden Abschnitt wird eine Nachricht zu einem _Nachrichten_ Thema von der Firebase-Konsolen Benachrichtigungs-GUI gesendet.

### <a name="send-a-topic-message"></a>Themen Nachricht senden

Deinstallieren Sie die APP, erstellen Sie Sie neu, und führen Sie Sie erneut aus. Klicken Sie auf die Schaltfläche **Benachrichtigungen abonnieren** :

[![Schaltfläche zum Abonnieren von Benachrichtigungen](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Wenn die APP erfolgreich abonniert wurde, sollte im Fenster IDE-Ausgabe das **Thema Sync** erfolgreich angezeigt werden:

[![Ausgabefenster zeigt die Meldung zum Thema Synchronisierung erfolgreich an](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Gehen Sie folgendermaßen vor, um eine Themen Nachricht zu senden:

1.  Klicken Sie in der Firebase-Konsole auf **neue Nachricht**.

2.  Geben Sie auf der Seite **Verfassen der Nachricht** den Meldungs Text ein, und wählen Sie **Topic**aus.

3.  Wählen Sie im Dropdown Menü **Thema** das integrierte Thema **Neuigkeiten**:

    [![Auswählen des Themas "News"](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Im Hintergrund der APP auf dem Android-Gerät (oder dem Emulator) klicken Sie auf die Schaltfläche Android **Overview (Übersicht über** Android) und berühren den Startbildschirm.

5.  Wenn das Gerät bereit ist, klicken Sie in der Firebase-Konsole auf **Nachricht senden** .

6.  Überprüfen Sie das Fenster IDE-Ausgabe, um **/topics/News** in der Protokoll Ausgabe anzuzeigen:

    [![Die Meldung von/Topic/News wird angezeigt.](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Wenn diese Meldung im Fenster Ausgabe angezeigt wird, sollte das Benachrichtigungssymbol auch im Benachrichtigungsbereich auf dem Android-Gerät angezeigt werden. Öffnen Sie das Benachrichtigungssymbol, um die Themen Meldung anzuzeigen:

[![Die Themen Meldung wird als Benachrichtigung angezeigt.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Wenn Sie keine Meldung erhalten, löschen Sie die Anwendung " **f** " auf dem Gerät (oder dem Emulator), und wiederholen Sie die obigen Schritte.

## <a name="foreground-notifications"></a>Vordergrund Benachrichtigungen

Zum Empfangen von Benachrichtigungen in Vordergrund-apps müssen Sie `FirebaseMessagingService`implementieren. Dieser Dienst wird auch zum Empfangen von Daten Nutzlasten und zum Senden von upstreamnachrichten benötigt. In den folgenden Beispielen wird veranschaulicht, wie ein Dienst implementiert `FirebaseMessagingService` wird, der die resultierende App erweitert &ndash; , damit Remote Benachrichtigungen verarbeitet werden können, während Sie im Vordergrund ausgeführt wird.

### <a name="implement-firebasemessagingservice"></a>Firebasemess agingservice implementieren

Der `FirebaseMessagingService` Dienst ist für das empfangen und Verarbeiten der Nachrichten von Firebase verantwortlich. Jede APP muss diesen Typ Unterklassen und `OnMessageReceived` überschreiben, um eine eingehende Nachricht zu verarbeiten. Wenn eine APP im Vordergrund steht, verarbeitet `OnMessageReceived` der Rückruf immer die Nachricht.

> [!NOTE]
> Apps verfügen nur über 10 Sekunden, in denen eine eingehende Firebase-Cloud-Nachricht verarbeitet werden soll. Jede Arbeit, die länger dauert, sollte für die Ausführung im Hintergrund mit einer Bibliothek wie dem [Android-Auftrags Planer](~/android/platform/android-job-scheduler.md) oder dem [Firebase-Auftrags](~/android/platform/firebase-job-dispatcher.md)Verteiler geplant werden.

Fügen Sie eine neue Datei mit dem Namen **MyFirebaseMessagingService.cs** hinzu, und ersetzen Sie den Vorlagen Code durch Folgendes:

```csharp
using System;
using Android.App;
using Android.Content;
using Android.Media;
using Android.Util;
using Firebase.Messaging;

namespace FCMClient
{
    [Service]
    [IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
    public class MyFirebaseMessagingService : FirebaseMessagingService
    {
        const string TAG = "MyFirebaseMsgService";
        public override void OnMessageReceived(RemoteMessage message)
        {
            Log.Debug(TAG, "From: " + message.From);
            Log.Debug(TAG, "Notification Message Body: " + message.GetNotification().Body);
        }
    }
}
```

Beachten Sie, `MESSAGING_EVENT` dass der Intent-Filter deklariert werden muss, damit neue FCM- `MyFirebaseMessagingService`Nachrichten an Folgendes weitergeleitet werden:

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Wenn die Client-App eine Nachricht von FCM empfängt `OnMessageReceived` , extrahiert den Nachrichten Inhalt aus dem bestandenen `RemoteMessage` Objekt, indem die `GetNotification` zugehörige-Methode aufgerufen wird. Als nächstes protokolliert Sie den Nachrichten Inhalt, sodass er im Fenster IDE-Ausgabe angezeigt werden kann:

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> Wenn Sie in Haltepunkte festlegen `FirebaseMessagingService`, kann die Debugsitzung diese Haltepunkte aufgrund der Art und Weise, in der die Nachrichten von SMS übermittelt werden, nicht erreichen.


### <a name="send-another-message"></a>Eine andere Nachricht senden

Deinstallieren Sie die APP, erstellen Sie Sie erneut, und führen Sie die folgenden Schritte aus, um eine weitere Nachricht zu senden:

1.  Klicken Sie in der Firebase-Konsole auf **neue Nachricht**.

2.  Geben Sie auf der Seite **Nachricht verfassen** den Nachrichtentext ein, und wählen Sie **einzelnes Gerät**aus.

3.  Kopieren Sie die Tokenzeichenfolge aus dem Fenster der IDE-Ausgabe, und fügen Sie Sie in das Feld "BCM- **Registrierungs Token** " der Firebase-Konsole ein

4.  Stellen Sie sicher, dass die APP im Vordergrund ausgeführt wird, und klicken Sie dann in der Firebase-Konsole auf **Nachricht senden** :

    [![Senden einer anderen Nachricht über die Konsole](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Wenn das Dialogfeld **Nachricht überprüfen** angezeigt wird, klicken Sie auf **senden**.

6.  Die eingehende Nachricht wird im Fenster IDE-Ausgabe protokolliert:

    [![Im Ausgabefenster gedruckter Nachrichtentext](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notification-sender"></a>Hinzufügen eines lokalen Benachrichtigungs Absenders

In diesem verbleibenden Beispiel wird die eingehende FCM-Nachricht in eine lokale Benachrichtigung konvertiert, die gestartet wird, während die APP im Vordergrund ausgeführt wird. Bearbeiten Sie **MyFirebaseMessageService.cs** , und fügen `using` Sie die folgenden Anweisungen hinzu:

```csharp
using FCMClient;
using System.Collections.Generic;
```

Fügen Sie die folgende Methode `MyFirebaseMessagingService`zu hinzu:

<a name="sendnotification-method"></a>

```csharp
void SendNotification(string messageBody, IDictionary<string, string> data)
{
    var intent = new Intent(this, typeof(MainActivity));
    intent.AddFlags(ActivityFlags.ClearTop);
    foreach (var key in data.Keys)
    {
        intent.PutExtra(key, data[key]);
    }

    var pendingIntent = PendingIntent.GetActivity(this,
                                                  MainActivity.NOTIFICATION_ID,
                                                  intent,
                                                  PendingIntentFlags.OneShot);

    var notificationBuilder = new  NotificationCompat.Builder(this, MainActivity.CHANNEL_ID)
                              .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
                              .SetContentTitle("FCM Message")
                              .SetContentText(messageBody)
                              .SetAutoCancel(true)
                              .SetContentIntent(pendingIntent);

    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(MainActivity.NOTIFICATION_ID, notificationBuilder.Build());
}
```

Um diese Benachrichtigung von Hintergrund Benachrichtigungen zu unterscheiden, markiert dieser Code Benachrichtigungen mit einem Symbol, das vom Anwendungssymbol abweicht. Fügen Sie die Datei " [\_IC\_stat\_IC Notification. png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) " zu **Ressourcen/drawable** hinzu, und schließen Sie Sie in das **fcmclient** -Projekt ein.

Die `SendNotification` -Methode `NotificationCompat.Builder` verwendet, um die Benachrichtigung zu `NotificationManagerCompat` erstellen, und wird zum Starten der Benachrichtigung verwendet. Die Benachrichtigung enthält eine `PendingIntent` , mit der der Benutzer die APP öffnen und den Inhalt der Zeichenfolge anzeigen kann, die `messageBody`an übermittelt wird. Weitere Informationen zu `NotificationCompat.Builder`finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

Wenden Sie `SendNotification` die-Methode am Ende `OnMessageReceived` der-Methode an:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

Infolge dieser Änderungen wird immer dann ausgeführt `SendNotification` , wenn eine Benachrichtigung empfangen wird, während sich die APP im Vordergrund befindet, und die Benachrichtigung wird im Benachrichtigungsbereich angezeigt.

Wenn eine APP im Hintergrund ist, bestimmt die [Nutzlast der Nachricht](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) , wie die Nachricht behandelt wird:

* **Benachrichtigung** Nachrichten werden an die Task **Leiste gesendet.** &ndash; Dort wird eine lokale Benachrichtigung angezeigt. Wenn der Benutzer auf die Benachrichtigung tippt, wird die APP gestartet.
* **Daten** Nachrichten werden von `OnMessageReceived`verarbeitet. &ndash;
* **Beides** &ndash; Nachrichten, die über eine Benachrichtigung und eine Daten Nutzlast verfügen, werden an die Taskleiste übermittelt. Wenn die APP gestartet wird, wird die Daten Nutzlast in `Extras` `Intent` der der angezeigt, die zum Starten der APP verwendet wurde.

In diesem Beispiel `SendNotification` wird ausgeführt, wenn die APP mit einer Daten Nutzlast übereinstimmt. Andernfalls wird eine Hintergrund Benachrichtigung (weiter oben in dieser exemplarischen Vorgehensweise veranschaulicht) gestartet.

### <a name="send-the-last-message"></a>Letzte Nachricht senden

Deinstallieren Sie die APP, erstellen Sie Sie erneut, und führen Sie dann die folgenden Schritte aus, um die letzte Nachricht zu senden:

1.  Klicken Sie in der Firebase-Konsole auf **neue Nachricht**.

2.  Geben Sie auf der Seite **Nachricht verfassen** den Nachrichtentext ein, und wählen Sie **einzelnes Gerät**aus.

3.  Kopieren Sie die Tokenzeichenfolge aus dem Fenster der IDE-Ausgabe, und fügen Sie Sie in das Feld "BCM- **Registrierungs Token** " der Firebase-Konsole ein

4.  Stellen Sie sicher, dass die APP im Vordergrund ausgeführt wird, und klicken Sie dann in der Firebase-Konsole auf **Nachricht senden** :

    [![Senden der Vordergrund Meldung](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Dieses Mal wird die Meldung, die im Ausgabefenster protokolliert wurde, auch in eine neue Benachrichtigung &ndash; gepackt. das Benachrichtigungssymbol wird in der Info Leiste angezeigt, während die APP im Vordergrund ausgeführt wird:

[![Benachrichtigungssymbol für Vordergrund Meldung](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Wenn Sie die Benachrichtigung öffnen, sollte die letzte Meldung angezeigt werden, die über die Firebase-Konsolen Benachrichtigungs-GUI gesendet wurde:

[![Vordergrund-Benachrichtigung mit Vordergrund Symbol](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>Trennen der Verbindung mit dem "f"

Um das Abonnement eines Themas aufzuheben, rufen Sie die [unabonnementfromtopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) -Methode für die [firebasemess Aging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) -Klasse auf. Wenn Sie z. b. das Abonnement abonnieren möchten, das zuvor abonniert wurde, könnte dem Layout eine Schaltfläche " **Abmelden** " mit dem folgenden Handlercode hinzugefügt werden:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

Um die Registrierung des Geräts bei FCM vollständig aufzuheben, löschen Sie die Instanz-ID, indem Sie die Methode " [Delta einstanceid](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) " für die Klasse " [firebaseinstanceid](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) " aufrufen. Beispiel:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Dieser Methoden aufzurufen löscht die Instanz-ID und die zugehörigen Daten. Folglich wird das regelmäßige Senden von FCM-Daten an das Gerät angehalten.


## <a name="troubleshooting"></a>Problembehandlung

Im folgenden werden Probleme und Problem Umgehungen beschrieben, die bei Verwendung von Firebase Cloud Messaging mit xamarin. Android auftreten können.

### <a name="firebaseapp-is-not-initialized"></a>Firebaseapp wurde nicht initialisiert.

In einigen Fällen wird möglicherweise folgende Fehlermeldung angezeigt:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Dies ist ein bekanntes Problem, das Sie umgehen können, indem Sie die Lösung bereinigen und das Projekt neu**erstellen (Build > Clean Solution**, **Build > Rebuild Solution**).

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise werden die Schritte zum Implementieren von Firebase Cloud Messaging-Remote Benachrichtigungen in einer xamarin. Android-Anwendung beschrieben. Es wurde beschrieben, wie die für die FCM-Kommunikation erforderlichen erforderlichen Pakete installiert werden, und es wurde erläutert, wie das Android-Manifest für den Zugriff auf FCM-Server konfiguriert wird. Es wurde Beispielcode bereitgestellt, der veranschaulicht, wie Google Play Services vorhanden ist. Es wurde gezeigt, wie Sie einen Instanz-ID-Listenerdienst implementieren, der mit FCM für ein Registrierungs Token aushandiert. Außerdem wurde erläutert, wie dieser Code Hintergrund Benachrichtigungen erstellt, während die APP sich im Hintergrund befindet. Es wurde erläutert, wie Topic-Meldungen abonniert werden, und es wurde eine Beispiel Implementierung eines nachrichtenlistener-Dienstanbieter bereitgestellt, der zum empfangen und Anzeigen von Remote Benachrichtigungen verwendet wird, während die APP im Vordergrund ausgeführt wird.


## <a name="related-links"></a>Verwandte Links

- ["F" (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/firebase-fcmnotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [Informationen zu Nachrichten](https://firebase.google.com/docs/cloud-messaging/concept-options)
