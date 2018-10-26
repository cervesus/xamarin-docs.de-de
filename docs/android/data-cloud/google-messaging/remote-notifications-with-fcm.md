---
title: Remotebenachrichtigungen mit Firebase Cloud Messaging
description: Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Firebase Cloud Messaging verwenden, um remotebenachrichtigungen (auch als "Pushbenachrichtigungen" bezeichnet) implementieren in einer Xamarin.Android-Anwendung. Es wird veranschaulicht, wie die verschiedenen Klassen implementieren, die erforderlich sind, für die Kommunikation mit Firebase Cloud Messaging (FCM) und enthält Beispiele zum Konfigurieren der Android-Manifest für den Zugriff auf FCM veranschaulicht downstream-messaging über die Firebase Konsole.
ms.prod: xamarin
ms.assetid: 4D7C5F46-C997-49F6-AFDA-6763E68CDC90
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/31/2018
ms.openlocfilehash: de0e2c5ff10de9136c4cb5987c80ce22c7b18c4d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105544"
---
# <a name="remote-notifications-with-firebase-cloud-messaging"></a>Remotebenachrichtigungen mit Firebase Cloud Messaging

_Diese exemplarische Vorgehensweise enthält eine schrittweise Erklärung der Vorgehensweise Firebase Cloud Messaging verwenden, um remotebenachrichtigungen (auch als "Pushbenachrichtigungen" bezeichnet) implementieren in einer Xamarin.Android-Anwendung. Es wird veranschaulicht, wie die verschiedenen Klassen implementieren, die erforderlich sind, für die Kommunikation mit Firebase Cloud Messaging (FCM) und enthält Beispiele zum Konfigurieren der Android-Manifest für den Zugriff auf FCM veranschaulicht downstream-messaging über die Firebase Konsole._

## <a name="fcm-notifications-overview"></a>Übersicht über die FCM-Benachrichtigungen

In dieser exemplarischen Vorgehensweise eine einfache app aufgerufen **FCMClient** erstellt wird, um die Grundlagen von FCM-messaging veranschaulichen. **FCMClient** überprüft das Vorhandensein von Google Play Services, Registrierungstoken von FCM empfängt, zeigt Sie remotebenachrichtigungen, die Sie über die Firebase-Konsole zu senden und abonniert themanachrichten gesendet werden:

[![Beispielhafter Screenshot der app](remote-notifications-with-fcm-images/00-app-example-sml.png)](remote-notifications-with-fcm-images/00-app-example.png#lightbox)

Die folgenden Themenbereiche werden untersucht werden:

1.  Hintergrund-Benachrichtigungen

2.  Themanachrichten gesendet werden

3.  Vordergrund-Benachrichtigungen

Im Verlauf dieser exemplarischen Vorgehensweise fügen Sie inkrementell Funktionalität **FCMClient** und führen Sie es auf einem Gerät oder Emulator aus, um zu verstehen, wie sie mit FCM interagiert. Verwenden Sie Protokollierung, um live-app-Transaktionen mit FCM-Servern Zeuge und bemerken Sie, wie Benachrichtigungen von FCM-Nachrichten generiert werden, die Sie in der grafischen Benutzeroberfläche Benachrichtigungen von Firebase-Konsole eingeben.

## <a name="requirements"></a>Anforderungen


Es wird hilfreich sein, sich mit den Grundlagen der [verschiedene Nachrichtentypen](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) , die von Firebase Cloud Messaging gesendet werden können. Die Nutzlast der Nachricht wird bestimmt, wie eine Client-app empfängt und verarbeitet die Nachricht.

Bevor Sie mit dieser exemplarischen Vorgehensweise fortfahren können, müssen Sie die erforderlichen Anmeldeinformationen zur Verwendung von FCM Googles-Servern abrufen; Dieser Prozess wird erläutert, [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#setup_fcm).
Sie müssen insbesondere Herunterladen der **Google-services.json** Datei für die Verwendung durch den Beispielcode in dieser exemplarischen Vorgehensweise angezeigt. Wenn Sie nicht noch ein Projekt in der Firebase-Konsole erstellt haben (oder wenn Sie noch nicht heruntergeladen haben die **Google-services.json** Datei), finden Sie unter [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

Zum Ausführen der Beispiel-app benötigen Sie ein Android-Tests-Gerät oder einen Emulator, die kompatibel mit Firebase ist. Firebase Cloud Messaging unterstützt Clients, die auf Android 4.0 oder höher ausgeführt wird, und diese Geräte müssen auch die Google Play Store-app installiert haben (Google Play Services 9.2.1 oder höher ist erforderlich). Wenn Sie noch nicht über die Google Play Store-app auf Ihrem Gerät installierten verfügen, besuchen Sie die [Google Play](https://support.google.com/googleplay) -Website herunterladen und installieren. Alternativ können Sie den Android SDK-Emulator mit Google Play Services installiert wird, anstatt ein Testgerät (Sie müssen keinen der Google Play Store zu installieren, wenn Sie Android SDK-Emulator verwenden).

## <a name="start-an-app-project"></a>Starten Sie ein app-Projekt

Erstellen Sie zunächst ein neues leeres Xamarin.Android-Projekt namens **FCMClient**. Wenn Sie nicht mit dem Erstellen von Xamarin.Android-Projekte vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/hello-android-quickstart.md).
Nachdem die neue app erstellt wurde, werden im nächste Schritt legen Sie den Paketnamen an und installieren mehrere NuGet-Pakete, die für die Kommunikation mit FCM verwendet werden.

### <a name="set-the-package-name"></a>Legen Sie den Paketnamen

In [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md), Sie einen Paketnamen für das FCM-fähige Anwendung angegeben. Dieses Paketname dient auch als die [ *Anwendungs-ID* ](./firebase-cloud-messaging.md#fcm-in-action-app-id) zugeordnete der [API-Schlüssel](firebase-cloud-messaging.md#fcm-in-action-api-key). Konfigurieren Sie die app zur Verwendung dieser Paketname:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Öffnen Sie die Eigenschaften für die **FCMClient** Projekt.

2.  In der **Android-Manifest** Seite, legen Sie den Paketnamen ein.

Im folgenden Beispiel wird der Paketname festgelegt `com.xamarin.fcmexample`:

[![Festlegen der Paketname](remote-notifications-with-fcm-images/01-package-name-vs-sml.png)](remote-notifications-with-fcm-images/01-package-name-vs.png#lightbox)

Während Sie aktualisieren die **Android-Manifest**, überprüfen Sie auch, um sicherzustellen, dass die `Internet` Berechtigung aktiviert ist.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Öffnen Sie die Eigenschaften für die **FCMClient** Projekt.

2.  In der **Android-Anwendung** Seite, legen Sie den Paketnamen ein.

Im folgenden Beispiel wird der Paketname festgelegt `com.xamarin.fcmexample`:

[![Festlegen der Paketname](remote-notifications-with-fcm-images/01-package-name-xs-sml.png)](remote-notifications-with-fcm-images/01-package-name-xs.png#lightbox)

Während Sie aktualisieren die **Android-Manifest**, überprüfen Sie auch, um sicherzustellen, dass der `INTERNET` Berechtigung aktiviert ist (unter **erforderliche Berechtigungen**).

-----

> [!IMPORTANT]
> Die Client-app wird in der Lage, ein Registrierungstoken von FCM zu erhalten, wenn dieser Paketname nicht der Fall ist *genau* entsprechen den Paketnamen an, die in der Firebase-Konsole eingegeben wurde.

### <a name="add-the-xamarin-google-play-services-base-package"></a>Fügen Sie das Base für Xamarin Google Play Services-Paket

Da Google Play-Dienste, Firebase Cloud Messaging abhängt, die [Xamarin Google Play Services - Basis](https://www.nuget.org/packages/Xamarin.GooglePlayServices.Base/) NuGet-Paket muss das Xamarin.Android-Projekt hinzugefügt werden. Sie benötigen Version 29.0.0.2 oder höher.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** .

2.  Klicken Sie auf die **Durchsuchen** Registerkarte und suchen Sie nach **Xamarin.GooglePlayServices.Base**.

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt:

    [![Installieren von Google Play Services-Basis](remote-notifications-with-fcm-images/02-google-play-services-vs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  In Visual Studio für Mac, mit der Maustaste **Pakete > Pakete hinzufügen...** .

2.  Suchen Sie nach **Xamarin.GooglePlayServices.Base**.

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt:

    [![Installieren von Google Play Services-Basis](remote-notifications-with-fcm-images/02-google-play-services-xs-sml.png)](remote-notifications-with-fcm-images/02-google-play-services-xs.png#lightbox)

-----

Wenn Sie während der Installation des NuGet eine Fehlermeldung erhalten, schließen Sie die **FCMClient** Projekt, öffnen Sie es erneut, und wiederholen Sie die NuGet-Installation.

Bei der Installation **Xamarin.GooglePlayServices.Base**, alle erforderlichen Abhängigkeiten werden ebenfalls installiert. Bearbeiten Sie **"mainactivity.cs"** und fügen Sie die folgenden `using` Anweisung:

```csharp
using Android.Gms.Common;
```

Diese Anweisung wird die `GoogleApiAvailability` -Klasse im **Xamarin.GooglePlayServices.Base** zur **FCMClient** Code.
`GoogleApiAvailability` wird verwendet, um das Vorhandensein von Google Play-Dienste zu überprüfen.

### <a name="add-the-xamarin-firebase-messaging-package"></a>Fügen Sie das Xamarin Firebase-Messaging-Paket

Zum Empfangen von Nachrichten von FCM, die [Xamarin Firebase - Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet-Paket muss das app-Projekt hinzugefügt werden. Ohne dieses Paket kann keine Android-Anwendung die Nachrichten von FCM-Servern empfangen.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  In Visual Studio mit der Maustaste **Verweise > NuGet-Pakete verwalten...** .

2. Suchen Sie nach **Xamarin.Firebase.Messaging**.

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt:

    [![Installieren von Xamarin Firebase-Messaging](remote-notifications-with-fcm-images/03-firebase-messaging-vs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-vs.png#lightbox)

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  In Visual Studio für Mac, mit der Maustaste **Pakete > Pakete hinzufügen...** .

2.  Überprüfen Sie **vorabversionspakete anzeigen** und suchen Sie nach **Xamarin.Firebase.Messaging**.

3.  Installieren Sie dieses Paket in der **FCMClient** Projekt:

    [![Installieren von Xamarin Firebase-Messaging](remote-notifications-with-fcm-images/03-firebase-messaging-xs-sml.png)](remote-notifications-with-fcm-images/03-firebase-messaging-xs.png#lightbox)

-----

Bei der Installation **Xamarin.Firebase.Messaging**, alle erforderlichen Abhängigkeiten werden ebenfalls installiert.

Als Nächstes bearbeiten **"mainactivity.cs"** und fügen Sie die folgenden `using` Anweisungen:

```csharp
using Firebase.Messaging;
using Firebase.Iid;
using Android.Util;
```

Die ersten beiden Anweisungen stellen die Typen in der **Xamarin.Firebase.Messaging** NuGet-Paket zur **FCMClient** Code. **Android.Util** fügt die Protokollierungsfunktion bereit, die verwendet wird, um Transaktionen mit FMS zu beobachten.

### <a name="add-googleplayservices-json"></a>Hinzufügen der Google Services JSON-Datei

Der nächste Schritt besteht, zum Hinzufügen der **Google-services.json** Datei in das Stammverzeichnis des Projekts:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

1.  Kopie **Google-services.json** in den Projektordner.

2.  Hinzufügen **Google-services.json** mit der app-Projekt (klicken Sie auf **alle Dateien anzeigen** in die **Projektmappen-Explorer**, klicken Sie mit der rechten Maustaste auf **Google-services.json**, und wählen Sie dann **zu Projekt**).

3.  Wählen Sie **Google-services.json** in die **Projektmappen-Explorer** Fenster.

4.  In der **Eigenschaften** legen Sie im Bereich der **Buildvorgang** zu **GoogleServicesJson**:

    [![Die Buildaktion festlegen auf GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-vs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-vs.png#lightbox)

    > [!NOTE] 
    > Wenn die **GoogleServicesJson** Buildaktion wird nicht angezeigt werden, speichern und schließen Sie die Projektmappe, und öffnen Sie es erneut.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio für Mac](#tab/macos)

1.  Kopie **Google-services.json** in den Projektordner.

2.  Hinzufügen **Google-services.json** mit der app-Projekt.

3.  Mit der rechten Maustaste **Google-services.json**.

4.  Legen Sie die **Buildvorgang** zu **GoogleServicesJson**:

    [![Die Buildaktion festlegen auf GoogleServicesJson](remote-notifications-with-fcm-images/04-google-services-json-xs-sml.png)](remote-notifications-with-fcm-images/04-google-services-json-xs.png#lightbox)

-----

Wenn **Google-services.json** wird dem Projekt hinzugefügt (und die **GoogleServicesJson** Buildvorgang festgelegt ist), des Buildprozesses extrahiert die Client-ID und [API-Schlüssel](./firebase-cloud-messaging.md#fcm-in-action-api-key) und klicken Sie dann Fügt diese Anmeldeinformationen, die zusammengeführt/generiert **"androidmanifest.xml"** , die sich unter befindet **obj/Debug/android/AndroidManifest.xml**. Dieser Mergeprozess fügt automatisch alle Berechtigungen und andere FCM-Elemente, die für die Verbindung zu FCM-Servern erforderlich sind.


## <a name="check-for-google-play-services-and-create-a-notification-channel"></a>Google Play-Dienste überprüfen Sie und erstellen Sie einen Benachrichtigungskanal

Google empfiehlt, dass Android-apps auf das Vorhandensein des Google Play Services APK überprüfen, bevor Sie den Zugriff auf Google Play Services-Funktionen (Weitere Informationen finden Sie unter [für Google Play-Dienste überprüfen](https://firebase.google.com/docs/cloud-messaging/android/client#sample-play)).

Eine anfängliche Layout für die Benutzeroberfläche der app wird zuerst erstellt werden. Bearbeiten Sie **Resources/layout/Main.axml** und Ersetzen Sie den Inhalt durch folgendes XML:

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

Dies `TextView` verwendet werden, um Nachrichten anzuzeigen, die angibt, ob es sich bei Google Play Services installiert ist. Speichern Sie die Änderungen an **Main.axml**.


Bearbeiten Sie **"mainactivity.cs"** und fügen Sie die folgenden Instanzvariablen auf dem `MainActivity` Klasse:

```csharp
public class MainActivity : AppCompatActivity
{
    static readonly string TAG = "MainActivity";

    internal static readonly string CHANNEL_ID = "my_notification_channel";
    internal static readonly int NOTIFICATION_ID = 100;

    TextView msgText;
```

Die Variablen `CHANNEL_ID` und `NOTIFICATION_ID` verwendet werden, in der Methode [ `CreateNotificationChannel` ](#create-notification-channel-code) hinzugefügt werden sollen `MainActivity` später in dieser exemplarischen Vorgehensweise.


Im folgenden Beispiel die `OnCreate` Methode überprüft, dass Google Play Services verfügbar ist, bevor die app versucht, FCM-Dienste verwenden.
Fügen Sie die folgende Methode der `MainActivity` Klasse:

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

Dieser Code überprüft das Gerät aus, um festzustellen, ob die Google Play Services-APK installiert ist. Wenn es nicht installiert ist, wird eine Meldung angezeigt, der `TextBox` , die der Benutzer ein APK aus dem Google Play Store herunterladen (oder in den Systemeinstellungen des Geräts aktivieren) wird angewiesen.

<a name="create-notification-channel-code"></a>Erstellen Sie Apps, die unter Android 8.0 (API-Ebene 26) oder höher ausgeführt werden müssen eine [ _Benachrichtigungskanal_ ](~/android/app-fundamentals/notifications/local-notifications.md) für die Veröffentlichung ihrer Benachrichtigungen.  Fügen Sie die folgende Methode der `MainActivity` Klasse, die den Benachrichtigungskanal erstellt (falls erforderlich):

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

`IsPlayServicesAvailable` wird aufgerufen, am Ende der `OnCreate` , damit die Google Play-Dienste überprüfen führt jedes Mal die app wird gestartet. Die Methode `CreateNotificationChannel` wird aufgerufen, um sicherzustellen, dass ein Benachrichtigungskanal für Geräte unter Android 8 vorhanden ist oder höher. Wenn Ihre app verfügt über eine `OnResume` -Methode, die sie aufrufen sollten `IsPlayServicesAvailable` aus `OnResume` ebenfalls. Vollständig neu zu erstellen und Ausführen der app. Wenn alle ordnungsgemäß konfiguriert ist, sollte einen Bildschirm angezeigt werden, der wie im folgenden Screenshot aussieht:

[![App gibt an, dass Google Play Services verfügbar ist.](remote-notifications-with-fcm-images/05-gps-available-sml.png)](remote-notifications-with-fcm-images/05-gps-available.png#lightbox)

Wenn Sie dieses Ergebnis nicht erhalten, stellen Sie sicher, dass die Google Play Services-APK auf Ihrem Gerät installiert ist (Weitere Informationen finden Sie unter [-Einstellung Sie Google Play Services](https://developers.google.com/android/guides/setup)).
Auch überprüfen, ob Sie hinzugefügt haben, die **Xamarin.Google.Play.Services.Base** -Paket zu Ihrem **FCMClient** Projekt wie bereits erwähnt.


## <a name="add-the-instance-id-receiver"></a>Die Instanz-ID-Empfänger hinzufügen

Der nächste Schritt zum Hinzufügen eines Diensts, der erweitert wird `FirebaseInstanceIdService` behandelt die Erstellung, Drehung, und Aktualisieren der [Firebase-Registrierungstoken](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token). Die `FirebaseInstanceIdService` -Dienst ist erforderlich, damit FCM, um Nachrichten an das Gerät senden zu können. Wenn die `FirebaseInstanceIdService` Dienst wird die app automatisch FCM-Nachrichten empfängt und diese als Benachrichtigungen anzuzeigen, wenn die app in diesem Fall an den Client-app hinzugefügt.

### <a name="declare-the-receiver-in-the-android-manifest"></a>Den Empfänger im Android-Manifest deklariert

Bearbeiten Sie **"androidmanifest.xml"** und fügen Sie die folgenden `<receiver>` Elemente in der `<application>` Abschnitt:

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

Dieser XML-Code führt Folgendes aus:

-   Deklariert eine `FirebaseInstanceIdReceiver` -Implementierung, die bietet eine [Eindeutiger Bezeichner](https://developers.google.com/instance-id/) für jede app-Instanz. Dieser Empfänger ist außerdem authentifiziert und autorisiert Aktionen.

-   Deklariert eine interne `FirebaseInstanceIdInternalReceiver` -Implementierung, die verwendet wird, um Dienste sicher zu starten.

-   Die [app-ID](./firebase-cloud-messaging.md#fcm-in-action-app-id) befindet sich in der **Google-services.json** Datei [dem Projekt hinzugefügt,](#add-googleplayservices-json). Die Xamarin.Android-Firebase-Bindungen ersetzt das Token `${applicationId}` mit der app-ID, ohne zusätzlicher Code ist erforderlich, durch die Client-app die app-ID angeben.

Die `FirebaseInstanceIdReceiver` ist eine `WakefulBroadcastReceiver` , empfängt `FirebaseInstanceId` und `FirebaseMessaging` Ereignisse und übermittelt diese an die Klasse, die Sie eine Ableitung aus `FirebaseInstanceIdService`.

### <a name="implement-the-firebase-instance-id-service"></a>Implementieren der Diensts Firebase Instance ID

Die Registrierung der Anwendung bei FCM Arbeit erfolgt durch den benutzerdefinierten `FirebaseInstanceIdService` Dienst, den Sie bereitstellen.
`FirebaseInstanceIdService` führt die folgenden Schritte aus:

1.  Verwendet die [Instanz-ID-API](https://developers.google.com/android/reference/com/google/android/gms/iid/InstanceID) um Sicherheitstoken zu generieren, die die Client-app den Zugriff auf FCM und der app-Server zu autorisieren. Im Gegenzug erhält die app zurück eine [Registrierungstoken](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#fcm-in-action-registration-token) von FCM.

2.  Das Registrierungstoken für die app-Server weitergeleitet, wenn der app-Server erforderlich ist.

Fügen Sie eine neue Datei namens **MyFirebaseIIDService.cs** und Ersetzen Sie den Vorlagencode durch Folgendes:

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

Dieser Dienst implementiert einen `OnTokenRefresh` -Methode, die aufgerufen wird, wenn das Registrierungstoken anfänglich erstellt oder geändert wird. Wenn `OnTokenRefresh` ausgeführt wird, ruft es ab, das aktuelle Token aus der `FirebaseInstanceId.Instance.Token` -Eigenschaft (die von FCM asynchron aktualisiert wird). In diesem Beispiel wird das aktualisierte Token protokolliert, sodass sie im Ausgabefenster angezeigt werden kann:

```csharp
var refreshedToken = FirebaseInstanceId.Instance.Token;
Log.Debug(TAG, "Refreshed token: " + refreshedToken);
```

`OnTokenRefresh` wird nur selten aufgerufen werden kann: Es wird verwendet, um das Token unter den folgenden Umständen aktualisieren:

-   Wenn die app installiert oder deinstalliert wird.

-   Wenn der Benutzer Anwendungsdaten löscht.

-   Wenn die app löscht die Instanz-ID.

-   Wenn die Sicherheit des Tokens gefährdet ist.

Anhand von Google [Instanz-ID](https://developers.google.com/instance-id/guides/android-implementation) Dokumentation, dass die Anwendung das Token in regelmäßigen Abständen (in der Regel alle 6 Monate) zu aktualisieren, wird der Dienst FCM Instance ID anfordern.

`OnTokenRefresh` Ruft auch `SendRegistrationToAppServer` Zuordnen des Benutzers Registrierung token mit dem serverseitigen-Konto (falls vorhanden) verwaltet wird von der Anwendung:

```csharp
void SendRegistrationToAppServer (string token)
{
    // Add custom implementation here as needed.
}
```

Da diese Implementierung für den Entwurf des app-Servers abhängig ist, wird ein leere Methodentext in diesem Beispiel bereitgestellt. Wenn Ihr app-Server FCM-Registrierungsinformationen erfordert, ändern Sie `SendRegistrationToAppServer` alle serverseitigen-Konten, die von Ihrer app verwaltet FCM-Instanz-ID-Token des Benutzers zugeordnet werden soll. (Beachten Sie, dass das Token für die Client-app nicht transparent ist.)

Wenn ein Token an der app-Server gesendet wird `SendRegistrationToAppServer` sollten beibehalten, ein boolescher Wert, um anzugeben, ob das Token an den Server gesendet wurde. Wenn dieser Wert auf "false" ist `SendRegistrationToAppServer` sendet das Token an die Anwendungsserver &ndash; , andernfalls das Token bereits an der app-Server in einem vorherigen Aufruf gesendet wurde. In einigen Fällen (wie diese `FCMClient` Beispiel), ist das Token von der app-Server nicht erforderlich; diese Methode ist daher nicht für dieses Beispiel erforderlich.

## <a name="implement-client-app-code"></a>Implementieren Sie die Client-app-code

Nun, dass der Empfänger Dienste vorhanden sind, kann Client-app-Code geschrieben werden, um diese Dienste nutzen. In den folgenden Abschnitten wird eine Schaltfläche an der Benutzeroberfläche das Registrierungstoken anmelden hinzugefügt (so genannte der *Instanz-ID-Token*), und mehr Code hinzugefügt `MainActivity` anzeigen `Intent` Informationen dar, wenn die app gestartet wird, aus einer Benachrichtigung:

[![Schaltfläche-Token "Protokoll" app-Bildschirm hinzugefügt](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

### <a name="log-tokens"></a>Log-Token

In diesem Schritt hinzugefügte Code dient nur zu Demonstrationszwecken &ndash; eine Produktions-Client-app würde keine müssen Registrierungstoken anmelden. Bearbeiten Sie **Resources/layout/Main.axml** und fügen Sie die folgenden `Button` Deklaration direkt nach der `TextView` Element:

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

Dieser Code protokolliert das aktuelle Token in das Fenster "Ausgabe" bei der **Log Token** Schaltfläche getippt wird.

### <a name="handle-notification-intents"></a>Benachrichtigung Intents verarbeiten

Wenn der Benutzer tippt auf eine Benachrichtigung von ausgegebenen **FCMClient**, alle Daten, die diese Benachrichtigung zu Nachricht im verfügbar gemacht wurde `Intent` Extras. Bearbeiten Sie **"mainactivity.cs"** und fügen Sie den folgenden Code am Anfang der `OnCreate` Methode (vor dem Aufruf von `IsPlayServicesAvailable`):

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

Der app-Startfeld `Intent` wird ausgelöst, wenn der Benutzer die Benachrichtigung tippt, damit dieser Code alle zugehörigen Daten Sie sich melden werden die `Intent` im Ausgabefenster. Wenn Sie ein anderes `Intent` ausgelöst werden muss, die `click_action` Feld der Benachrichtigung muss festgelegt werden, mit dem `Intent` (das Startprogramm `Intent` wird verwendet, wenn keine `click_action` angegeben ist).


## <a name="background-notifications"></a>Hintergrund-Benachrichtigungen

Erstellen und Ausführen der **FCMClient** app. Die **Log Token** -Schaltfläche angezeigt wird:

[![Schaltfläche "Protokoll-Token" wird angezeigt.](remote-notifications-with-fcm-images/06-log-token-sml.png)](remote-notifications-with-fcm-images/06-log-token.png#lightbox)

Tippen Sie auf die **Log Token** Schaltfläche. Sinngemäß die folgende Meldung sollte im Ausgabefenster IDE angezeigt werden:

[![Instanz-ID-Token im Fenster "Ausgabe" angezeigt](remote-notifications-with-fcm-images/07-token-received-sml.png)](remote-notifications-with-fcm-images/07-token-received.png#lightbox)

Die lange Zeichenfolge mit der Bezeichnung **token** ist die Instanz-ID-Token, die Sie in der Firebase-Konsole einfügen werden &ndash; auswählen und diese Zeichenfolge in die Zwischenablage zu kopieren. Wenn Sie eine Instanz-ID-Token nicht angezeigt werden, fügen Sie die folgende Zeile am Anfang der `OnCreate` Methode, um zu überprüfen, ob **Google-services.json** ordnungsgemäß analysiert wurde:

```csharp
Log.Debug(TAG, "google app id: " + GetString(Resource.String.google_app_id));
```

Die `google_app_id` im Ausgabefenster protokollierte Wert übereinstimmen sollte die `mobilesdk_app_id` aufgezeichnete Wert im **Google-services.json**.

### <a name="send-a-message"></a>Senden einer Nachricht

Melden Sie sich bei der [Firebase-Konsole](https://console.firebase.google.com), wählen Sie Ihr Projekt, klicken Sie auf **Benachrichtigungen**, und klicken Sie auf **der ersten Nachricht senden**:

[![Ihre erste Meldung-Schaltfläche "Senden"](remote-notifications-with-fcm-images/08-first-notification-sml.png)](remote-notifications-with-fcm-images/08-first-notification.png#lightbox)

Auf der **Compose Nachricht** Seite Geben Sie den Nachrichtentext, und wählen Sie **Einzelgerät**. Die Instanz-ID-Token aus dem IDE-Ausgabefenster kopieren und fügen Sie ihn in das **FCM-Registrierungstoken** Feld der Firebase-Konsole:

[![Das Dialogfeld mit Compose](remote-notifications-with-fcm-images/09-compose-message-sml.png)](remote-notifications-with-fcm-images/09-compose-message.png#lightbox)

Auf dem Android-Gerät (oder Emulator), die app im Hintergrund durch Tippen auf die Android **Übersicht** Schaltfläche und die home-Bildschirm zu berühren. Wenn das Gerät bereit ist, klicken Sie auf **SEND MESSAGE** in der Firebase-Konsole:

[![Message-Schaltfläche "Senden"](remote-notifications-with-fcm-images/10-send-message-sml.png)](remote-notifications-with-fcm-images/10-send-message.png#lightbox)

Wenn die **Review Nachricht** Dialogfeld wird angezeigt, klicken Sie auf **senden**.
Das Symbol "Benachrichtigung" sollte im Infobereich des Geräts (oder -Emulator) angezeigt werden:

[![Symbol "Benachrichtigung" wird angezeigt.](remote-notifications-with-fcm-images/11-notification-icon-sml.png)](remote-notifications-with-fcm-images/11-notification-icon.png#lightbox)

Öffnen Sie das Benachrichtigungssymbol, um die Nachricht anzuzeigen. Die Nachricht sollte werden genau wie in eingegeben wurde die **Meldungstext** Feld der Firebase-Konsole:

[![Nachricht wird auf dem Gerät angezeigt.](remote-notifications-with-fcm-images/12-notification-sml.png)](remote-notifications-with-fcm-images/12-notification.png#lightbox)

Tippen Sie auf das Benachrichtigungssymbol zum Starten der **FCMClient** app. Die `Intent` Extras gesendet, um **FCMClient** finden Sie im Ausgabefenster IDE:

[![Beabsichtigte zusätzliche Daten enthält, aus Schlüssel Meldungs-ID und Zuklappen-Taste](remote-notifications-with-fcm-images/13-intent-extras-sml.png)](remote-notifications-with-fcm-images/13-intent-extras.png#lightbox)

In diesem Beispiel die **aus** Schlüssel festgelegt ist, auf die Anzahl der Firebase-Projekt der app (in diesem Beispiel `41590732`), und die **Collapse_key** an den Paketnamen festgelegt ist ( **com.xamarin.fcmexample**).
Versuchen Sie es löschen, wenn Sie eine Nachricht nicht erhalten, die **FCMClient** app auf dem Gerät (oder -Emulator), und wiederholen Sie die oben genannten Schritte.


> [!NOTE]
> Wenn Sie erzwingen die app schließen, hält FCM die Übermittlung von Benachrichtigungen an. Android verhindert, dass Hintergrund Service Broadcasts versehentlich oder unnötig, starten Komponenten der beendete Anwendungen. (Weitere Informationen zu diesem Verhalten finden Sie unter [Steuerelemente auf die beendete Anwendungen starten](https://developer.android.com/about/versions/android-3.1.html#launchcontrols).) Aus diesem Grund ist es erforderlich, manuell die app jedes Mal deinstallieren Sie ausführen, und beenden Sie diesen aus einer Debugsitzung &ndash; FCM, um ein neues Token zu generieren, damit Sie weiterhin Nachrichten empfangen werden muss.

### <a name="add-a-custom-default-notification-icon"></a>Fügen Sie eine benutzerdefinierte standardbenachrichtigungssymbol hinzu.

Im vorherigen Beispiel wird das Symbol "Benachrichtigung" auf das Symbol der Anwendung festgelegt. Das folgende XML wird ein benutzerdefinierter Standardsymbol für Benachrichtigungen konfiguriert. Android zeigt dieser benutzerdefinierte Standardsymbol für alle benachrichtigungsmeldungen, in dem das Symbol "Benachrichtigungen" nicht explizit festgelegt wurde.

Ein benutzerdefinierter Standardwert Symbol hinzuzufügen, fügen Ihre Symbol, um die **Ressourcen/drawable** Verzeichnis bearbeiten **"androidmanifest.xml"**, und fügen Sie die folgenden `<meta-data>` Element in der `<application>`Abschnitt:

```xml
<meta-data
    android:name="com.google.firebase.messaging.default_notification_icon"
    android:resource="@drawable/ic_stat_ic_notification" />
```

In diesem Beispiel ist das Benachrichtigungssymbol, die befindet sich am **Ressourcen/drawable/ic\_Stat\_ic\_notification.png** wird als das Benachrichtigungssymbol benutzerdefinierter Standardwert verwendet. Wenn ein benutzerdefinierter Standardsymbol nicht, im konfiguriert ist **"androidmanifest.xml"** Symbol "keine" festgelegt ist, in die Nutzlast der Benachrichtigung und Android verwendet das Anwendungssymbol wie das Benachrichtigungssymbol, (wie in der Benachrichtigung Symbol oben abgebildeter Screenshot dargestellt).

## <a name="handle-topic-messages"></a>Verarbeitung von Nachrichten durch Thema

Bisher geschriebene Code Registrierungstoken behandelt und funktionell remote-Benachrichtigung an die app. Im nächste Beispiel fügt den Code, der überwacht *themanachrichten gesendet* und leitet diese weiter an den Benutzer als remotebenachrichtigungen. Themanachrichten gesendet werden, FCM-Nachrichten, die auf einem oder mehreren Geräten gesendet werden, die ein bestimmtes Thema abonnieren. Weitere Informationen zu themanachrichten gesendet werden, finden Sie unter [Thema Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md).

### <a name="subscribe-to-a-topic"></a>Abonnieren eines Themas

Bearbeiten Sie **Resources/layout/Main.axml** und fügen Sie die folgenden `Button` Deklaration sofort nach der letzten `Button` Element:

```xml
<Button
  android:id="@+id/subscribeButton"
  android:layout_width="match_parent"
  android:layout_height="wrap_content"
  android:layout_gravity="center_horizontal"
  android:layout_marginTop="20dp"
  android:text="Subscribe to Notifications" />
```

Dieser XML-Code fügt eine **Benachrichtigung abonnieren** Schaltfläche, um das Layout.
Bearbeiten Sie **"mainactivity.cs"** und fügen Sie den folgenden Code am Ende der `OnCreate` Methode:

```csharp
var subscribeButton = FindViewById<Button>(Resource.Id.subscribeButton);
subscribeButton.Click += delegate {
    FirebaseMessaging.Instance.SubscribeToTopic("news");
    Log.Debug(TAG, "Subscribed to remote notifications");
};
```

Dieser Code sucht nach der **Benachrichtigung abonnieren** Schaltfläche im Layout und weist Sie den Klickhandler für Code, der Aufrufe `FirebaseMessaging.Instance.SubscribeToTopic`, und übergeben Sie das Thema abonnierten _Nachrichten_. Wenn der Benutzer tippt der **abonnieren** Schaltfläche, die app abonniert die _News_ Thema. Im folgenden Abschnitt eine _News_ Thema Nachricht wird über die grafische Benutzeroberfläche Benachrichtigungen von Firebase-Konsole gesendet.

### <a name="send-a-topic-message"></a>Senden einer Nachricht Thema

Deinstallieren Sie die app, neu erstellen Sie und führen sie erneut aus. Klicken Sie auf die **Abonnieren von Benachrichtigungen** Schaltfläche:

[![Abonnieren Sie die Schaltfläche "Benachrichtigungen"](remote-notifications-with-fcm-images/14-subscribe-sml.png)](remote-notifications-with-fcm-images/14-subscribe.png#lightbox)

Wenn die app erfolgreich abonniert hat, sollten Sie sehen **Thema Sync erfolgreich** Ausgabefenster in der IDE:

[![Fenster "Ausgabe" zeigt die erfolgreiche Synchronisierung themennachricht](remote-notifications-with-fcm-images/15-topic-sync-sml.png)](remote-notifications-with-fcm-images/15-topic-sync.png#lightbox)

Verwenden Sie zum Senden einer Nachricht Thema die folgenden Schritte aus:

1.  Klicken Sie in der Firebase-Konsole auf **neue Nachricht**.

2.  Auf der **Compose Nachricht** Seite Geben Sie den Nachrichtentext, und wählen Sie **Thema**.

3.  In der **Thema** Pulldown-Menü Wählen Sie im Thema integrierte **News**:

    [![Wählen die News-Thema](remote-notifications-with-fcm-images/16-topic-message-sml.png)](remote-notifications-with-fcm-images/16-topic-message.png#lightbox)

4.  Auf dem Android-Gerät (oder Emulator), die app im Hintergrund durch Tippen auf die Android **Übersicht** Schaltfläche und die home-Bildschirm zu berühren.

5.  Wenn das Gerät bereit ist, klicken Sie auf **SEND MESSAGE** in der Firebase-Konsole.

6.  Überprüfen Sie die IDE-Ausgabefenster, finden Sie unter **/Themen/News** in die Ausgabe des Protokolls:

    [![Nachricht von /topic/news wird angezeigt.](remote-notifications-with-fcm-images/17-message-arrived-sml.png)](remote-notifications-with-fcm-images/17-message-arrived.png#lightbox)

Wenn diese Meldung im Ausgabefenster angezeigt wird, sollte das Symbol "Benachrichtigungen" im Infobereich der Taskleiste auf dem Android-Gerät auch angezeigt werden. Öffnen Sie das Benachrichtigungssymbol, um die themanachricht anzuzeigen:

[![Die themanachricht wird als eine Benachrichtigung angezeigt.](remote-notifications-with-fcm-images/18-other-news-sml.png)](remote-notifications-with-fcm-images/18-other-news.png#lightbox)

Versuchen Sie es löschen, wenn Sie eine Nachricht nicht erhalten, die **FCMClient** app auf dem Gerät (oder -Emulator), und wiederholen Sie die oben genannten Schritte.

## <a name="foreground-notifications"></a>Vordergrund-Benachrichtigungen

Sie müssen zum Empfangen von Benachrichtigungen in badgewert apps implementieren `FirebaseMessagingService`. Dieser Dienst ist auch erforderlich, für den Empfang von datennutzlasten und zum Senden von upstream-Nachrichten. Die folgenden Beispiele veranschaulichen, wie Sie einen Dienst implementieren, die erweitert `FirebaseMessagingService` &ndash; die resultierende app wird in der Lage, remotebenachrichtigungen zu verarbeiten, während der Ausführung im Vordergrund.

### <a name="implement-firebasemessagingservice"></a>Implementieren von FirebaseMessagingService

Die `FirebaseMessagingService` Dienst ist für das empfangen und Verarbeiten der Nachrichten von Firebase zuständig. Jede app muss als Unterklasse verwenden diesen Typ und die Außerkraftsetzung der `OnMessageReceived` eine eingehende Nachricht zu verarbeiten. Wenn eine app im Vordergrund ist die `OnMessageReceived` Rückruf wird immer die Meldung zu behandeln.

> [!NOTE]
> Apps müssen nur 10 Sekunden, in dem eine eingehende Nachricht von Firebase Cloud zu behandeln. Jede Arbeit, die dauert länger, als dies geplant werden soll, für die Ausführung der Hintergrund mithilfe einer Bibliothek wie z. B. die [Android-Auftragsplaner](~/android/platform/android-job-scheduler.md) oder [Firebase Auftrag Dispatcher](~/android/platform/firebase-job-dispatcher.md).

Fügen Sie eine neue Datei namens **MyFirebaseMessagingService.cs** und Ersetzen Sie den Vorlagencode durch Folgendes:

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

Beachten Sie, dass die `MESSAGING_EVENT` Zielfilter muss deklariert werden, sodass neue FCM-Nachrichten an das weitergeleitet werden `MyFirebaseMessagingService`:

```csharp
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
```

Wenn die Client-app eine Nachricht von FCM empfängt `OnMessageReceived` extrahiert den Nachrichteninhalt aus dem übergebenen `RemoteMessage` -Objekt durch Aufrufen der `GetNotification` Methode. Als Nächstes protokolliert den Nachrichteninhalt, damit sie in der IDE-Ausgabefenster angezeigt werden kann:

```csharp
var body = message.GetNotification().Body;
Log.Debug(TAG, "Notification Message Body: " + body);
```

> [!NOTE]
> Wenn Sie in Haltepunkte `FirebaseMessagingService`, Ihrer Debugsitzung kann oder möglicherweise diese Haltepunkte nicht erreicht, da wie FCM Nachrichten übermittelt.


### <a name="send-another-message"></a>Eine weitere Nachricht senden

Deinstallieren Sie die app, neu erstellen Sie, führen sie erneut aus und befolgen Sie diese Schritte aus, um eine weitere Nachricht senden:

1.  Klicken Sie in der Firebase-Konsole auf **neue Nachricht**.

2.  Auf der **Compose Nachricht** Seite Geben Sie den Nachrichtentext, und wählen Sie **Einzelgerät**.

3.  Kopieren Sie die Tokenzeichenfolge aus dem IDE-Ausgabefenster aus, und fügen Sie ihn in das **FCM-Registrierungstoken** Feld der Firebase-Konsole wie zuvor.

4.  Stellen Sie sicher, dass die app im Vordergrund ausgeführt wird, und klicken Sie dann **SEND MESSAGE** in der Firebase-Konsole:

    [![Senden eine andere Nachricht in der Konsole](remote-notifications-with-fcm-images/19-hello-again-sml.png)](remote-notifications-with-fcm-images/19-hello-again.png#lightbox)

5.  Wenn die **Review Nachricht** Dialogfeld wird angezeigt, klicken Sie auf **senden**.

6.  Die eingehende Nachricht wird in der IDE-Ausgabefenster protokolliert:

    [![Nachrichtentext im Ausgabefenster drucken](remote-notifications-with-fcm-images/20-logged-message.png)](remote-notifications-with-fcm-images/20-logged-message.png#lightbox)


### <a name="add-a-local-notification-sender"></a>Fügen Sie eine lokale Benachrichtigung Absender hinzu.

In diesem Beispiel verbleibenden wird die eingehende Nachricht von FCM in eine lokale Benachrichtigung konvertiert werden, die gestartet wird, während die app im Vordergrund ausgeführt wird. Bearbeiten Sie **MyFirebaseMessageService.cs** und fügen Sie die folgenden `using` Anweisungen:

```csharp
using FCMClient;
using System.Collections.Generic;
```

Fügen Sie die folgende Methode `MyFirebaseMessagingService`:

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

Um diese Benachrichtigung von Hintergrund-Benachrichtigungen zu unterscheiden, markiert diesen Code Benachrichtigungen mit einem Symbol, das das Symbol der Anwendung unterscheidet. Fügen Sie die Datei [ic\_Stat\_ic\_notification.png](remote-notifications-with-fcm-images/ic-stat-ic-notification.png) zu **Ressourcen/drawable** und nehmen Sie diese in die **FCMClient** Projekt .

Die `SendNotification` -Methode verwendet ` NotificationCompat.Builder` So erstellen Sie die Benachrichtigung und `NotificationManagerCompat` verwendet, um die Benachrichtigung zu starten. Die Benachrichtigung enthält eine `PendingIntent` ermöglichen, die der Benutzer, öffnen Sie die app, und zeigen Sie den Inhalt der übergebenen Zeichenfolge `messageBody`. Weitere Informationen zu `NotificationCompat.Builder`, finden Sie unter [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md).

Rufen Sie die `SendNotification` Methode am Ende der `OnMessageReceived` Methode:

```csharp
public override void OnMessageReceived(RemoteMessage message)
{
    Log.Debug(TAG, "From: " + message.From);

    var body = message.GetNotification().Body;
    Log.Debug(TAG, "Notification Message Body: " + body);
    SendNotification(body, message.Data);
}
```

Aufgrund dieser Änderungen `SendNotification` wird ausgeführt, wenn eine Benachrichtigung empfangen wird, während die app im Vordergrund, und die Benachrichtigung im Infobereich der Taskleiste angezeigt wird.

Wenn eine app im Hintergrund ist die [Nutzlast der Nachricht](https://firebase.google.com/docs/cloud-messaging/concept-options#notifications_and_data_messages) wird bestimmt, wie die Nachricht behandelt wird:

* **Benachrichtigung** &ndash; Nachrichten werden gesendet werden, um die **Taskleiste**. Eine lokale Benachrichtigung wird dort angezeigt. Wenn der Benutzer auf die Benachrichtigung tippt, wird die app gestartet.
* **Daten** &ndash; Nachrichten werden vom behandelt `OnMessageReceived`.
* **Beide** &ndash; Nachrichten, die Nutzlast für eine Benachrichtigung und die Daten werden an die Taskleiste übermittelt. Wenn die app gestartet wird, erscheint in die Datennutzlast der `Extras` von der `Intent` , die zum Starten der app verwendet wurde.

In diesem Beispiel, wenn in diesem die app Fall `SendNotification` wird ausgeführt, wenn die Nachricht eine Datennutzlast enthält. Andernfalls wird eine Hintergrund-Benachrichtigung (die weiter oben in dieser exemplarischen Vorgehensweise dargestellt werden) gestartet.

### <a name="send-the-last-message"></a>Senden Sie die letzte Meldung

Deinstallieren Sie die app, neu erstellen Sie, führen sie erneut aus, und verwenden Sie die folgenden Schritte aus, um die letzte Nachricht zu senden:

1.  Klicken Sie in der Firebase-Konsole auf **neue Nachricht**.

2.  Auf der **Compose Nachricht** Seite Geben Sie den Nachrichtentext, und wählen Sie **Einzelgerät**.

3.  Kopieren Sie die Tokenzeichenfolge aus dem IDE-Ausgabefenster aus, und fügen Sie ihn in das **FCM-Registrierungstoken** Feld der Firebase-Konsole wie zuvor.

4.  Stellen Sie sicher, dass die app im Vordergrund ausgeführt wird, und klicken Sie dann **SEND MESSAGE** in der Firebase-Konsole:

    [![Die Vordergrund-Nachricht senden](remote-notifications-with-fcm-images/21-console-fg-msg-sml.png)](remote-notifications-with-fcm-images/21-console-fg-msg.png#lightbox)

Dieses Mal die Meldung, die im Ausgabefenster protokolliert wurde auch in eine neue Benachrichtigung verpackt &ndash; das Benachrichtigungssymbol wird im Benachrichtigungsbereich angezeigt, während die app im Vordergrund ausgeführt wird:

[![Symbol "Benachrichtigung" für die Vordergrund-Nachricht](remote-notifications-with-fcm-images/22-foreground-icon-sml.png)](remote-notifications-with-fcm-images/22-foreground-icon.png#lightbox)

Wenn Sie die Benachrichtigung öffnen, sehen Sie die letzte Meldung, die über die grafische Benutzeroberfläche Benachrichtigungen von Firebase-Konsole gesendet wurde:

[![Vordergrund-Benachrichtigung mit der Vordergrund-Symbol angezeigt](remote-notifications-with-fcm-images/23-foreground-msg-sml.png)](remote-notifications-with-fcm-images/23-foreground-msg.png#lightbox)


## <a name="disconnecting-from-fcm"></a>Trennen von FCM

Rufen Sie zum Abbestellen von Benachrichtigungen von einem Thema der [UnsubscribeFromTopic](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging.html#unsubscribeFromTopic%28java.lang.String%29) Methode für die [FirebaseMessaging](https://firebase.google.com/docs/reference/android/com/google/firebase/messaging/FirebaseMessaging) Klasse. Beispielsweise, um das Abonnement kündigen die _News_ Thema bereits abonniert ein **Unsubscribe** Schaltfläche konnte das Layout mit dem folgenden Handlercode hinzugefügt werden:

```csharp
var unSubscribeButton = FindViewById<Button>(Resource.Id.unsubscribeButton);
unSubscribeButton.Click += delegate {
    FirebaseMessaging.Instance.UnsubscribeFromTopic("news");
    Log.Debug(TAG, "Unsubscribed from remote notifications");
};
```

Löschen Sie zum Aufheben der Registrierung des Geräts FCM vollständig aus die Instanz-ID durch Aufrufen der [DeleteInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId.html#deleteInstanceId%28%29) Methode für die [FirebaseInstanceId](https://firebase.google.com/docs/reference/android/com/google/firebase/iid/FirebaseInstanceId) Klasse. Zum Beispiel:

```csharp
FirebaseInstanceId.Instance.DeleteInstanceId();
```

Dieser Methodenaufruf löscht die Instanz-ID und die damit verbundenen Daten. Das regelmäßige Senden von FCM-Daten an das Gerät wird daher angehalten.


## <a name="troubleshooting"></a>Problembehandlung

Die folgende Beschreibung Probleme und problemumgehungen, die auftreten können, wenn Sie mithilfe von Firebase Cloud Messaging, die mit Xamarin.Android.

### <a name="firebaseapp-is-not-initialized"></a>FirebaseApp ist nicht initialisiert.

In einigen Fällen können Sie diese Fehlermeldung angezeigt:

```shell
Java.Lang.IllegalStateException: Default FirebaseApp is not initialized in this process
Make sure to call FirebaseApp.initializeApp(Context) first.
```

Dies ist ein bekanntes Problem, die Sie umgehen können, indem Sie die Projektmappe zu bereinigen und das Projekt neu erstellt (**erstellen > Projektmappe bereinigen**, **erstellen > Projektmappe neu erstellen**). Weitere Informationen finden Sie in diesem [Forumsdiskussion](https://forums.xamarin.com/discussion/96263/default-firebaseapp-is-not-initialized-in-this-process).


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise beschrieben, die Schritte zur Implementierung von remotebenachrichtigungen Firebase Cloud Messaging in einer Xamarin.Android-Anwendung. Es wurde beschrieben, wie Sie die Installation der erforderlichen Pakete für die FCM-Kommunikation benötigt, und es wurde erklärt, wie das Android-Manifest für den Zugriff auf FCM-Servern konfigurieren. Es bot Beispielcode, der veranschaulicht, wie Sie auf das Vorhandensein von Google Play-Dienste überprüfen. Es wurde veranschaulicht, wie einen Instanz-ID-Listenerdienst implementieren, der für ein Registrierungstoken bei FCM handelt, und er erläutert, wie dieser Code Hintergrund Benachrichtigungen erstellt, während die app in diesem Fall. Es wurde erklärt, wie zum Thema Nachrichten abonnieren und anschließend eine beispielimplementierung eines Message-Listener-Diensts, die verwendet wird, empfangen und remotebenachrichtigungen angezeigt, während die app im Vordergrund ausgeführt wird.


## <a name="related-links"></a>Verwandte Links

- [FCMNotifications (Beispiel)](https://developer.xamarin.com/samples/monodroid/Firebase/FCMNotifications)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [Informationen zu FCM-Nachrichten](https://firebase.google.com/docs/cloud-messaging/concept-options)
