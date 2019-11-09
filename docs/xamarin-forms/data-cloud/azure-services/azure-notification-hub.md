---
title: Senden und empfangen von Pushbenachrichtigungen mit Azure Notification Hubs und xamarin. Forms
description: In diesem Artikel wird erläutert, wie Sie Azure Notification Hubs verwenden, um plattformübergreifende Pushbenachrichtigungen an xamarin. Forms-Anwendungen zu senden.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/23/2019
ms.openlocfilehash: eafa5c8af8d93138ec6e2b9e2f25549d7ed006b0
ms.sourcegitcommit: bfe4327ef2e89dab095641860256eadb349ca62c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/08/2019
ms.locfileid: "73849838"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Senden und empfangen von Pushbenachrichtigungen mit Azure Notification Hubs und xamarin. Forms

[![herunterladen](~/media/shared/download.png)Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

Pushbenachrichtigungen liefern Informationen von einem Back-End-System an eine mobile Anwendung. Apple, Google und andere Plattformen verfügen jeweils über einen eigenen pushbenachrichtigungsdienst (PNS). Azure Notification Hubs ermöglichen Ihnen, Benachrichtigungen plattformübergreifend zu zentralisieren, damit Ihre Back-End-Anwendung mit einem einzelnen Hub kommunizieren kann, der die Verteilung der Benachrichtigungen an die einzelnen plattformspezifischen PNS übernimmt.

Integrieren Sie Azure Notification Hubs in Mobile Apps, indem Sie die folgenden Schritte ausführen:

1. [Richten Sie Push Notification Services und Azure Notification Hub](#set-up-push-notification-services-and-azure-notification-hub)ein.
1. Erfahren [Sie, wie Vorlagen und Tags verwendet](#register-templates-and-tags-with-the-azure-notification-hub)werden.
1. [Erstellen Sie eine plattformübergreifende xamarin. Forms-Anwendung](#xamarinforms-application-functionality).
1. [Konfigurieren Sie das Native Android-Projekt für Pushbenachrichtigungen](#configure-the-android-application-for-notifications).
1. [Konfigurieren Sie das Native IOS-Projekt für Pushbenachrichtigungen](#configure-ios-for-notifications).
1. [Testen Sie Benachrichtigungen mit dem Azure Notification Hub](#test-notifications-in-the-azure-portal).
1. Erstellen einer Back-End- [Anwendung zum Senden von Benachrichtigungen](#create-a-notification-dispatcher).

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Einrichten von Push-Notification Services und Azure Notification Hub

Die Integration von Azure Notification Hubs in xamarin. Forms Mobile App ähnelt der Integration von Azure Notification Hubs in eine native xamarin-Anwendung. Richten Sie eine **FCM-Anwendung** ein, indem Sie die Firebase-Konsolen Schritte in [Pushbenachrichtigungen an xamarin. Android mithilfe von Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging)befolgen. Führen Sie die folgenden Schritte mithilfe des xamarin. Android-Tutorials aus:

1. Definieren Sie einen Android-Paketnamen, wie z. b. `com.xamarin.notifysample`, der im Beispiel verwendet wird.
1. Laden Sie **Google-Services. JSON** über die Firebase-Konsole herunter. Sie fügen diese Datei in einem späteren Schritt zu Ihrer Android-Anwendung hinzu.
1. Erstellen Sie eine Azure Notification Hub-Instanz, und benennen Sie Sie. Dieser Artikel und dieses Beispiel verwenden `xdocsnotificationhub` als Hub-Name.
1. Kopieren Sie den FCM- **Server Schlüssel** , und speichern Sie ihn als **API-Schlüssel** unter **Google (GCM/FCM)** in Ihrem Azure Notification Hub.

Der folgende Screenshot zeigt die Konfiguration der Google-Plattform im Azure Notification Hub:

![Screenshot der Google-Konfiguration von Azure Notification Hub](azure-notification-hub-images/fcm-notification-hub-config.png "Google-Konfiguration für Azure Notification Hub")

Sie benötigen einen macOS-Computer, um das Setup für IOS-Geräte abzuschließen. Richten Sie APNs ein, indem Sie die ersten Schritte in [Pushbenachrichtigungen an xamarin. IOS mithilfe von Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file)befolgen. Führen Sie im xamarin. IOS-Tutorial die folgenden Schritte aus:

1. Hiermit wird eine IOS-Bündel-ID definiert. In diesem Artikel und Beispiel wird `com.xamarin.notifysample` als Bündel Bezeichner verwendet.
1. Erstellen Sie eine Zertifikat Signier Anforderung (Certificate Signing Request, CSR), und verwenden Sie Sie zum Generieren eines pushbenachrichtigungszertifikats.
1. Laden Sie das pushbenachrichtigungszertifikat unter **Apple (APNs)** in ihren Azure Notification Hub hoch.

Der folgende Screenshot zeigt die Konfiguration der Apple-Plattform im Azure Notification Hub:

![Screenshot der Apple-Konfiguration für Azure Notification Hub](azure-notification-hub-images/apns-notification-hub-config.png "Apple-Konfiguration für Azure Notification Hub")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Registrieren von Vorlagen und Tags mit dem Azure Notification Hub

Azure Notification Hub erfordert, dass Mobile Anwendungen beim Hub registriert werden, Vorlagen definiert und Tags abonniert werden. Die Registrierung verknüpft ein plattformspezifisches PNS-Handle mit einem Bezeichner im Azure Notification Hub. Weitere Informationen zu Registrierungen finden Sie unter [Registrierungs Verwaltung](/azure/notification-hubs/notification-hubs-push-notification-registration-management).

Mit Vorlagen können Geräte parametrisierte Nachrichten Vorlagen angeben. Eingehende Nachrichten können pro Gerät und Tag angepasst werden. Weitere Informationen zu Vorlagen finden Sie unter [Vorlagen](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).

Tags können verwendet werden, um Nachrichten Kategorien zu abonnieren, z. b. Nachrichten, Sport und Wetter. Der Einfachheit halber definiert die Beispielanwendung eine Standardvorlage mit einem einzelnen Parameter mit dem Namen `messageParam` und einem einzelnen Tag `default`. In komplexeren Systemen können benutzerspezifische Tags verwendet werden, um einen Benutzer über Geräte für personalisierte Benachrichtigungen zu benachrichtigen. Weitere Informationen zu Tags finden Sie unter [Routing-und tagausdrücke](/azure/notification-hubs/notification-hubs-tags-segment-push-message).

Zum erfolgreichen empfangen von Nachrichten muss jede native Anwendung die folgenden Schritte ausführen:

1. Rufen Sie ein PNS-handle oder-Token aus dem PNS der Plattform ab.
1. Registrieren Sie das PNS-Handle beim Azure Notification Hub.
1. Geben Sie eine Vorlage an, die die gleichen Parameter wie ausgehende Nachrichten enthält.
1. Abonnieren Sie das-Tag für ausgehende Nachrichten.

Diese Schritte werden für jede Plattform im Abschnitt [Konfigurieren der Android-Anwendung für Benachrichtigungen](#configure-the-android-application-for-notifications) und [Konfigurieren von IOS für Benachrichtigungen](#configure-ios-for-notifications) ausführlich beschrieben.

## <a name="xamarinforms-application-functionality"></a>Xamarin. Forms-Anwendungs Funktionalität

Die xamarin. Forms-Beispielanwendung zeigt eine Liste von pushbenachrichtigungsnachrichten an. Dies erfolgt mit der `AddMessage`-Methode, die der Benutzeroberfläche die angegebene pushbenachrichtigungsmeldung hinzufügt. Diese Methode verhindert außerdem, dass doppelte Nachrichten zur Benutzeroberfläche hinzugefügt werden, und wird im Haupt Thread ausgeführt, sodass Sie von jedem Thread aufgerufen werden kann. Der folgende Code veranschaulicht die `AddMessage`-Methode:

```csharp
public void AddMessage(string message)
{
    Device.BeginInvokeOnMainThread(() =>
    {
        if (messageDisplay.Children.OfType<Label>().Where(c => c.Text == message).Any())
        {
            // Do nothing, an identical message already exists
        }
        else
        {
            Label label = new Label()
            {
                Text = message,
                HorizontalOptions = LayoutOptions.CenterAndExpand,
                VerticalOptions = LayoutOptions.Start
            };
            messageDisplay.Children.Add(label);
        }
    });
}
```

Die Beispielanwendung enthält eine **AppConstants.cs** -Datei, die Eigenschaften definiert, die von den Platt Form Projekten verwendet werden. Diese Datei muss mit Werten aus Ihrem Azure Notification Hub angepasst werden. Der folgende Code zeigt die **AppConstants.cs** -Datei:

```csharp
public static class AppConstants
{
    public static string NotificationChannelName { get; set; } = "XamarinNotifyChannel";
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string ListenConnectionString { get; set; } = "< Insert your DefaultListenSharedAccessSignature >";
    public static string DebugTag { get; set; } = "XamarinNotify";
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string FCMTemplateBody { get; set; } = "{\"data\":{\"message\":\"$(messageParam)\"}}";
    public static string APNTemplateBody { get; set; } = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";
}
```

Passen Sie die folgenden Werte in `AppConstants` an, um die Beispielanwendung mit Ihrem Azure Notification Hub zu verbinden:

* `NotificationHubName`: Verwenden Sie den Namen des Azure Notification Hubs, den Sie in Ihrem Azure-Portal erstellt haben.
* `ListenConnectionString`: dieser Wert befindet sich im Azure Notification Hub unter **Zugriffsrichtlinien**.

Der folgende Screenshot zeigt, wo sich diese Werte in der Azure-Portal befinden:

![Screenshot der Azure Notification Hub-Zugriffs Richtlinie](azure-notification-hub-images/notification-hub-access-policy.png "Azure Notification Hub-Zugriffs Richtlinie")

## <a name="configure-the-android-application-for-notifications"></a>Konfigurieren der Android-Anwendung für Benachrichtigungen

Führen Sie die folgenden Schritte aus, um die Android-Anwendung für das empfangen und Verarbeiten von Benachrichtigungen zu konfigurieren:

1. Konfigurieren Sie den Android- **Paketnamen** so, dass er mit dem Paketnamen in der Firebase-Konsole
1. Installieren Sie die folgenden nuget-Pakete, um mit Google Play, Firebase und Azure Notification Hubs zu interagieren:
    1. Xamarin. googleplayservices. base.
    1. Xamarin. Firebase. Messaging.
    1. Xamarin. Azure. notificationhubs. Android.
1. Kopieren Sie die `google-services.json` Datei, die Sie beim FCM-Setup heruntergeladen haben, in das Projekt, und legen Sie die Buildaktion auf `GoogleServicesJson`
1. [Konfigurieren Sie "androidmanifest. xml" für die Kommunikation mit Firebase](#configure-android-manifest).
1. [Registrieren Sie die Anwendung bei Firebase und Azure Notification Hub mithilfe einer `FirebaseInstanceIdService`](#register-using-a-custom-firebaseinstanceidservice).
1. [Verarbeiten von Nachrichten mit einer `FirebaseMessagingService`](#process-messages-with-a-firebasemessagingservice).
1. [Fügen Sie der xamarin. Forms-Benutzeroberfläche eingehende Benachrichtigungen hinzu](#add-incoming-notifications-to-the-xamarinforms-ui).

> [!NOTE]
> Die **googleservicesjson** -Buildaktion ist Teil des nuget-Pakets **xamarin. googleplayservices. Base** . Visual Studio 2019 legt die verfügbaren Buildaktionen während des Starts fest. Wenn **googleservicesjson** nicht als Buildvorgang angezeigt wird, starten Sie Visual Studio 2019 neu, nachdem Sie die nuget-Pakete installiert haben.

### <a name="configure-android-manifest"></a>Konfigurieren des Android-Manifests

Die `receiver` Elemente innerhalb des `application` Elements ermöglichen der APP, mit Firebase zu kommunizieren. Die `uses-permission` Elemente ermöglichen der APP das Verarbeiten von Nachrichten und das registrieren beim Azure Notification Hub. Die gesamte Datei " **androidmanifest. XML** " sollte in etwa wie im folgenden Beispiel aussehen:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android" android:versionCode="1" android:versionName="1.0" package="YOUR_PACKAGE_NAME" android:installLocation="auto">
  <uses-sdk android:minSdkVersion="21" />
  <uses-permission android:name="android.permission.INTERNET" />
  <uses-permission android:name="com.google.android.c2dm.permission.RECEIVE" />
  <uses-permission android:name="android.permission.WAKE_LOCK" />
  <uses-permission android:name="android.permission.GET_ACCOUNTS"/>
  <application android:label="Notification Hub Sample">
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
    <receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
      <intent-filter>
        <action android:name="com.google.android.c2dm.intent.RECEIVE" />
        <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
        <category android:name="${applicationId}" />
      </intent-filter>
    </receiver>
  </application>
</manifest>
```

### <a name="register-using-a-custom-firebaseinstanceidservice"></a>Registrieren mithilfe eines benutzerdefinierten firebaseinstanceidservice

Firebase gibt Token aus, die ein Gerät auf dem PNS eindeutig identifizieren. Token haben eine lange Lebensdauer, werden jedoch gelegentlich aktualisiert. Wenn ein Token ausgestellt oder aktualisiert wird, muss die Anwendung das neue Token beim Azure Notification Hub registrieren. Die Registrierung wird von einer Instanz einer Klasse verarbeitet, die von `FirebaseInstanceIdService` abgeleitet ist.

In der Beispielanwendung erbt `FirebaseRegistrationService`-Klasse von `FirebaseInstanceIdService`. Diese Klasse verfügt über eine `IntentFilter`, die `com.google.firebase.INSTANCE_ID_EVENT`einschließt, sodass das Android-Betriebssystem automatisch `OnTokenRefresh` aufruft, wenn ein Token von Firebase ausgegeben wird.

Der folgende Code zeigt die benutzerdefinierte `FirebaseInstanceIdService` aus der Beispielanwendung:

```csharp
[Service]
[IntentFilter(new [] { "com.google.firebase.INSTANCE_ID_EVENT"})]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    public override void OnTokenRefresh()
    {
        string token = FirebaseInstanceId.Instance.Token;

        // NOTE: logging the token is not recommended in production but during
        // development it is useful to test messages directly from Firebase
        Log.Info(AppConstants.DebugTag, $"Token received: {token}");

        SendRegistrationToServer(token);
    }

    void SendRegistrationToServer(string token)
    {
        try
        {
            NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

            // register device with Azure Notification Hub using the token from FCM
            Registration reg = hub.Register(token, AppConstants.SubscriptionTags);

            // subscribe to the SubscriptionTags list with a simple template.
            string pnsHandle = reg.PNSHandle;
            var cats = string.Join(", ", reg.Tags);
            var temp = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
        }
        catch (Exception e)
        {
            Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
        }
    }
}
```

Die `SendRegistrationToServer`-Methode in der `FirebaseRegistrationClass` registriert das Gerät beim Azure Notification Hub und abonniert Tags mit einer Vorlage. Die Beispielanwendung definiert ein einzelnes Tag namens `default` und eine Vorlage mit einem einzelnen Parameter mit dem Namen `messageParam` in der Datei **AppConstants.cs** . Weitere Informationen zur Registrierung, zu Tags und Vorlagen finden Sie unter [Registrieren von Vorlagen und Tags mit dem Azure Notification Hub](#register-templates-and-tags-with-the-azure-notification-hub) .

### <a name="process-messages-with-a-firebasemessagingservice"></a>Verarbeiten von Nachrichten mit einem firebasemess agingservice

Eingehende Nachrichten werden an eine `FirebaseMessagingService`-Instanz weitergeleitet, wo Sie in eine lokale Benachrichtigung konvertiert werden können. Das Android-Projekt in der Beispielanwendung enthält eine Klasse mit dem Namen `FirebaseService`, die von `FirebaseMessagingService` erbt. Diese Klasse verfügt über eine `IntentFilter`, die `com.google.firebase.MESSAGING_EVENT`einschließt, sodass das Android-Betriebssystem automatisch `OnMessageReceived` aufruft, wenn eine pushbenachrichtigungsnachricht empfangen wird.

Das folgende Beispiel zeigt die `FirebaseService` aus der Beispielanwendung:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    public override void OnMessageReceived(RemoteMessage message)
    {
        base.OnMessageReceived(message);
        string messageBody = string.Empty;

        if (message.GetNotification() != null)
        {
            messageBody = message.GetNotification().Body;
        }

        // NOTE: test messages sent via the Azure portal will be received here
        else
        {
            messageBody = message.Data.Values.First();
        }

        // convert the incoming message to a local notification
        SendLocalNotification(messageBody);

        // send the incoming message directly to the MainPage
        SendMessageToMainPage(messageBody);
    }

    void SendLocalNotification(string body)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        intent.PutExtra("message", body);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetContentTitle("XamarinNotify Message")
            .SetSmallIcon(Resource.Drawable.ic_launcher)
            .SetContentText(body)
            .SetAutoCancel(true)
            .SetShowWhen(false)
            .SetContentIntent(pendingIntent);

        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            notificationBuilder.SetChannelId(AppConstants.NotificationChannelName);
        }

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }

    void SendMessageToMainPage(string body)
    {
        (App.Current.MainPage as MainPage)?.AddMessage(body);
    }
}
```

Eingehende Nachrichten werden mit der `SendLocalNotification`-Methode in eine lokale Benachrichtigung konvertiert. Diese Methode erstellt eine neue `Intent` und platziert den Nachrichten Inhalt als `string` `Extra` in der `Intent`. Wenn der Benutzer auf die lokale Benachrichtigung tippt, unabhängig davon, ob sich die APP im Vordergrund oder im Hintergrund befindet, wird der `MainActivity` gestartet und kann über das `Intent`-Objekt auf den Nachrichten Inhalt zugreifen.

Die lokale Benachrichtigung und das `Intent` Beispiel erfordern, dass der Benutzer die Aktion für das Tippen auf die Benachrichtigung durchführen muss. Dies ist wünschenswert, wenn der Benutzer vor der Änderung des Anwendungs Zustands Maßnahmen ergreifen soll. Möglicherweise möchten Sie jedoch auf die Nachrichten Daten zugreifen, ohne dass in einigen Fällen eine Benutzeraktion erforderlich ist. Im vorherigen Beispiel wird die Nachricht auch direkt an die aktuelle `MainPage` Instanz mit der `SendMessageToMainPage`-Methode gesendet. Wenn Sie in der Produktion beide Methoden für einen einzelnen Nachrichtentyp implementieren, erhält das `MainPage` Objekt doppelte Nachrichten, wenn der Benutzer auf die Benachrichtigung tippt.

> [!NOTE]
> Die Android-Anwendung empfängt nur Pushbenachrichtigungen, wenn Sie im Hintergrund oder im Vordergrund ausgeführt wird. Um Pushbenachrichtigungen zu erhalten, wenn der Haupt `Activity` nicht ausgeführt wird, müssen Sie einen Dienst implementieren, der über den Rahmen dieses Beispiels hinausgeht. Weitere Informationen finden Sie unter [Erstellen von Android-Diensten](/xamarin/android/app-fundamentals/services/) .

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Hinzufügen von eingehenden Benachrichtigungen zur xamarin. Forms-Benutzeroberfläche

Die `MainActivity`-Klasse muss die Berechtigung zum Verarbeiten von Benachrichtigungen und zum Verwalten eingehender Nachrichten Daten erhalten. Der folgende Code zeigt die gesamte `MainActivity` Implementierung:

```csharp
[Activity(Label = "NotificationHubSample", Icon = "@mipmap/icon", Theme = "@style/MainTheme", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation, LaunchMode = LaunchMode.SingleTop)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
{
    protected override void OnCreate(Bundle savedInstanceState)
    {
        TabLayoutResource = Resource.Layout.Tabbar;
        ToolbarResource = Resource.Layout.Toolbar;

        base.OnCreate(savedInstanceState);

        global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
        LoadApplication(new App());

        if (!IsPlayServiceAvailable())
        {
            throw new Exception("This device does not have Google Play Services and cannot receive push notifications.");
        }

        CreateNotificationChannel();
    }

    protected override void OnNewIntent(Intent intent)
    {
        if (intent.Extras != null)
        {
            var message = intent.GetStringExtra("message");
            (App.Current.MainPage as MainPage)?.AddMessage(message);
        }

        base.OnNewIntent(intent);
    }

    bool IsPlayServiceAvailable()
    {
        int resultCode = GoogleApiAvailability.Instance.IsGooglePlayServicesAvailable(this);
        if (resultCode != ConnectionResult.Success)
        {
            if (GoogleApiAvailability.Instance.IsUserResolvableError(resultCode))
                Log.Debug(AppConstants.DebugTag, GoogleApiAvailability.Instance.GetErrorString(resultCode));
            else
            {
                Log.Debug(AppConstants.DebugTag, "This device is not supported");
            }
            return false;
        }
        return true;
    }

    void CreateNotificationChannel()
    {
        // Notification channels are new as of "Oreo".
        // There is no need to create a notification channel on older versions of Android.
        if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
        {
            var channelName = AppConstants.NotificationChannelName;
            var channelDescription = String.Empty;
            var channel = new NotificationChannel(channelName, channelName, NotificationImportance.Default)
            {
                Description = channelDescription
            };

            var notificationManager = (NotificationManager)GetSystemService(NotificationService);
            notificationManager.CreateNotificationChannel(channel);
        }
    }
}
```

Das `Activity`-Attribut legt die Anwendungs `LaunchMode` auf `SingleTop` fest. Dieser Start Modus weist das Android-Betriebssystem an, nur eine einzelne Instanz dieser Aktivität zuzulassen. Bei diesem Start Modus werden eingehende `Intent` Daten an die `OnNewIntent`-Methode weitergeleitet, die Nachrichten Daten extrahiert und über die `AddMessage`-Methode an die `MainPage` Instanz sendet. Wenn Ihre Anwendung einen anderen Start Modus verwendet, muss `Intent` Daten unterschiedlich behandelt werden.

Die `OnCreate`-Methode verwendet eine Hilfsmethode namens `IsPlayServiceAvailable`, um sicherzustellen, dass das Gerät Google Play Dienst unterstützt. Emulatoren oder Geräte, die Google Play-Dienst nicht unterstützen, können keine Pushbenachrichtigungen von Firebase empfangen.

## <a name="configure-ios-for-notifications"></a>Konfigurieren von IOS für Benachrichtigungen

Der Vorgang zum Konfigurieren der IOS-Anwendung für den Empfang von Benachrichtigungen lautet wie folgt:

1. Konfigurieren Sie die **Bündel** -ID in der Datei " **Info. plist** " so, dass Sie mit dem im Bereitstellungs Profil verwendeten Wert identisch ist.
1. Fügen Sie die Option " **Pushbenachrichtigungen aktivieren** " zur Datei " **Berechtigungen. plist** " hinzu.
1. Fügen Sie dem Projekt das nuget-Paket **xamarin. Azure. notificationhubs. IOS** hinzu.
1. [Registrieren Sie sich für Benachrichtigungen mit APNs](#register-for-notifications-with-apns).
1. [Registrieren Sie die Anwendung beim Azure Notification Hub, und abonnieren Sie Tags](#register-with-azure-notification-hub-and-subscribe-to-tags).
1. [Fügen Sie die APNs-Benachrichtigungen zur xamarin. Forms-Benutzeroberfläche hinzu](#add-apns-notifications-to-xamarinforms-ui).

Der folgende Screenshot zeigt die Option **Pushbenachrichtigungen aktivieren** , die in der Datei " **Berechtigungen. plist** " in Visual Studio ausgewählt ist:

![Screenshot der pushbenachrichtigungsberechtigung](azure-notification-hub-images/push-notification-entitlement.png "Berechtigung für Pushbenachrichtigungen")

### <a name="register-for-notifications-with-apns"></a>Registrieren für Benachrichtigungen mit APNs

Die `FinishedLaunching`-Methode in der Datei **AppDelegate.cs** muss überschrieben werden, um sich für Remote Benachrichtigungen zu registrieren. Die Registrierung variiert abhängig von der IOS-Version, die auf dem Gerät verwendet wird. Das IOS-Projekt in der Beispielanwendung überschreibt die `FinishedLaunching`-Methode, um `RegisterForRemoteNotifications` aufzurufen, wie im folgenden Beispiel gezeigt:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();
    LoadApplication(new App());

    base.FinishedLaunching(app, options);

    RegisterForRemoteNotifications();

    return true;
}

void RegisterForRemoteNotifications()
{
    // register for remote notifications based on system version
    if (UIDevice.CurrentDevice.CheckSystemVersion(10, 0))
    {
        UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert |
            UNAuthorizationOptions.Sound |
            UNAuthorizationOptions.Sound,
            (granted, error) =>
            {
                if (granted)
                    InvokeOnMainThread(UIApplication.SharedApplication.RegisterForRemoteNotifications);
            });
    }
    else if (UIDevice.CurrentDevice.CheckSystemVersion(8, 0))
    {
        var pushSettings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
        new NSSet());

        UIApplication.SharedApplication.RegisterUserNotificationSettings(pushSettings);
        UIApplication.SharedApplication.RegisterForRemoteNotifications();
    }
    else
    {
        UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
        UIApplication.SharedApplication.RegisterForRemoteNotificationTypes(notificationTypes);
    }
}
```

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Registrieren beim Azure Notification Hub und Abonnieren von Tags

Wenn das Gerät während der `FinishedLaunching`-Methode erfolgreich für Remote Benachrichtigungen registriert wurde, ruft IOS die `RegisteredForRemoteNotifications`-Methode auf. Diese Methode sollte überschrieben werden, um die folgenden Aktionen auszuführen:

1. Instanziieren Sie die `SBNotificationHub`.
1. Aufheben der Registrierung vorhandener Registrierungen.
1. Registrieren Sie das Gerät beim Notification Hub.
1. Abonnieren Sie bestimmte Tags mit einer Vorlage.

Weitere Informationen zur Registrierung von Geräten, Vorlagen und Tags finden Sie unter Registrieren von [Vorlagen und Tags mit dem Azure Notification Hub](#register-templates-and-tags-with-the-azure-notification-hub). Der folgende Code veranschaulicht die Registrierung des Geräts und der Vorlagen:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAllAsync(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNativeAsync(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplateAsync(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                if (errorCallback != null)
                {
                    Debug.WriteLine($"RegisterTemplateAsync error: {errorCallback}");
                }
            }
        });
    });
}
```

> [!NOTE]
> Die Registrierung für Remote Benachrichtigungen kann in Situationen, in denen keine Netzwerkverbindung besteht, fehlschlagen. Sie können die `FailedToRegisterForRemoveNotifications`-Methode außer Kraft setzen, um Registrierungsfehler zu behandeln.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Hinzufügen von APNs-Benachrichtigungen zur xamarin. Forms-Benutzeroberfläche

Wenn ein Gerät eine Remote Benachrichtigung empfängt, ruft IOS die `ReceivedRemoteNotification`-Methode auf. Die JSON-Nachricht der eingehenden Nachricht wird in ein `NSDictionary` Objekt konvertiert, und die `ProcessNotification`-Methode extrahiert Werte aus dem Wörterbuch und sendet Sie an die `MainPage` Instanz von xamarin. Forms. Die `ReceivedRemoteNotifications`-Methode wird überschrieben, um `ProcessNotification` aufzurufen, wie im folgenden Code gezeigt:

```csharp
public override void ReceivedRemoteNotification(UIApplication application, NSDictionary userInfo)
{
    ProcessNotification(userInfo, false);
}

void ProcessNotification(NSDictionary options, bool fromFinishedLaunching)
{
    // make sure we have a payload
    if (options != null && options.ContainsKey(new NSString("aps")))
    {
        // get the APS dictionary and extract message payload. Message JSON will be converted
        // into a NSDictionary so more complex payloads may require more processing
        NSDictionary aps = options.ObjectForKey(new NSString("aps")) as NSDictionary;
        string payload = string.Empty;
        NSString payloadKey = new NSString("alert");
        if (aps.ContainsKey(payloadKey))
        {
            payload = aps[payloadKey].ToString();
        }

        if (!string.IsNullOrWhiteSpace(payload))
        {
            (App.Current.MainPage as MainPage)?.AddMessage(payload);
        }

    }
    else
    {
        Debug.WriteLine($"Received request to process notification but there was no payload.");
    }
}
```

## <a name="test-notifications-in-the-azure-portal"></a>Testen von Benachrichtigungen im Azure-Portal

Mit Azure Notification Hubs können Sie überprüfen, ob Ihre Anwendung Testnachrichten empfangen kann. Der Abschnitt " **Test send** " im Notification Hub ermöglicht es Ihnen, die Zielplattform auszuwählen und eine Nachricht zu senden. Wenn **Sie den Ausdruck Send to Tag** auf `default` festlegen, werden Nachrichten an Anwendungen gesendet, die eine Vorlage für das `default`-Tag registriert haben. Wenn Sie auf die Schaltfläche **senden** klicken, wird ein Bericht generiert, der die Anzahl der Geräte enthält, die mit der Meldung Der folgende Screenshot zeigt einen Android-Benachrichtigungs Test in der Azure-Portal:

![Screenshot einer Azure Notification Hub-Testnachricht](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure Notification Hub-Testnachricht")

### <a name="testing-tips"></a>Testtipps

1. Wenn Sie testen, ob eine Anwendung Pushbenachrichtigungen empfangen kann, müssen Sie ein physisches Gerät verwenden. Virtuelle Android-und IOS-Geräte sind möglicherweise nicht ordnungsgemäß für den Empfang von Pushbenachrichtigungen konfiguriert.
1. Die Android-Beispielanwendung registriert das Token und die Vorlagen einmal, wenn das Firebase-Token ausgegeben wird. Während des Tests müssen Sie möglicherweise ein neues Token anfordern und sich erneut beim Azure Notification Hub registrieren. Die beste Möglichkeit, dies zu erzwingen, besteht darin, das Projekt zu bereinigen, die `bin` und `obj` Ordner zu löschen und die Anwendung vor der Neuerstellung und Bereitstellung vom Gerät zu deinstallieren.
1. Viele Teile des pushbenachrichtigungsflows werden asynchron ausgeführt. Dies kann dazu führen, dass Breakpoints nicht in einer unerwarteten Reihenfolge getroffen werden oder nicht. Verwenden Sie die Geräte-oder Debugprotokollierung, um die Ausführung ohne Unterbrechung des Anwendungs Flusses Filtern Sie das Android-Geräte Protokoll mithilfe der in `Constants` angegebenen `DebugTag`.

## <a name="create-a-notification-dispatcher"></a>Erstellen eines Benachrichtigungs Verteilers

Azure Notification Hubs ermöglichen, dass Ihre Back-End-Anwendung Benachrichtigungen über Plattformen hinweg an Geräte verteilt. Im Beispiel wird die Benachrichtigungs Verteilung mit der **notificationdispatcher** -Konsolenanwendung veranschaulicht. Die Anwendung umfasst die Datei **DispatcherConstants.cs** , in der die folgenden Eigenschaften definiert sind:

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Sie müssen die **DispatcherConstants.cs** so konfigurieren, dass Sie Ihrer Azure Notification Hub-Konfiguration entspricht. Der Wert der `SubscriptionTags`-Eigenschaft sollte den Werten entsprechen, die in den Client-Apps verwendet werden. Die `NotificationHubName`-Eigenschaft ist der Name Ihrer Azure Notification Hub-Instanz. Die `FullAccessConnectionString`-Eigenschaft ist der Zugriffsschlüssel, der in ihren Notification Hub- **Zugriffsrichtlinien**gefunden wurde. Der folgende Screenshot zeigt den Speicherort der Eigenschaften `NotificationHubName` und `FullAccessConnectionString` in der Azure-Portal:

![Screenshot des Azure Notification Hub-namens und "fullaccessconnectionstring"](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure Notification Hub-Name und fullaccessconnectionstring")

Die Konsolenanwendung durchläuft die einzelnen `SubscriptionTags` Werte und sendet Benachrichtigungen mithilfe einer Instanz der `NotificationHubClient`-Klasse an Abonnenten. Der folgende Code zeigt die `Program` Klasse der Konsolenanwendung:

``` csharp
class Program
{
    static int messageCount = 0;

    static void Main(string[] args)
    {
        Console.WriteLine($"Press the spacebar to send a message to each tag in {string.Join(", ", DispatcherConstants.SubscriptionTags)}");
        WriteSeparator();
        while (Console.ReadKey().Key == ConsoleKey.Spacebar)
        {
            SendTemplateNotificationsAsync().GetAwaiter().GetResult();
        }
    }

    private static async Task SendTemplateNotificationsAsync()
    {
        NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(DispatcherConstants.FullAccessConnectionString, DispatcherConstants.NotificationHubName);
        Dictionary<string, string> templateParameters = new Dictionary<string, string>();

        messageCount++;

        // Send a template notification to each tag. This will go to any devices that
        // have subscribed to this tag with a template that includes "messageParam"
        // as a parameter
        foreach (var tag in DispatcherConstants.SubscriptionTags)
        {
            templateParameters["messageParam"] = $"Notification #{messageCount} to {tag} category subscribers!";
            try
            {
                await hub.SendTemplateNotificationAsync(templateParameters, tag);
                Console.WriteLine($"Sent message to {tag} subscribers.");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Failed to send template notification: {ex.Message}");
            }
        }

        Console.WriteLine($"Sent messages to {DispatcherConstants.SubscriptionTags.Length} tags.");
        WriteSeparator();
    }

    private static void WriteSeparator()
    {
        Console.WriteLine("==========================================================================");
    }
}
```

Wenn die Beispiel Konsolenanwendung ausgeführt wird, kann die Leertaste gedrückt werden, um Nachrichten zu senden. Geräte, auf denen die Client Anwendungen ausgeführt werden, sollten nummerierte Benachrichtigungen erhalten, sofern Sie ordnungsgemäß konfiguriert sind.

## <a name="related-links"></a>Verwandte Links

* [Pushbenachrichtigungsvorlagen](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).
* [Verwaltung von Geräte Registrierungen](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [Routing-und tagausdrücke](/azure/notification-hubs/notification-hubs-tags-segment-push-message).
* [Tutorial zu xamarin. Android in Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Tutorial zu xamarin. IOS in Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
