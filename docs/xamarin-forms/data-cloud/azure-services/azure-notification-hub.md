---
title: Senden und Empfangen von Pushbenachrichtigungen mit Azure Notification Hubs und Xamarin.Forms
description: In diesem Artikel wird erklärt, wie Sie mithilfe von Azure Notification Hubs plattformübergreifende Pushbenachrichtigungen an eine Xamarin.Forms-Anwendung senden.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 11/27/2019
ms.openlocfilehash: 778f56ec844e2802c1e1bc783824d55218678761
ms.sourcegitcommit: e9d88587aafc912124b87732d81c3910247ad811
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/05/2020
ms.locfileid: "78337291"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Senden und Empfangen von Pushbenachrichtigungen mit Azure Notification Hubs und Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-azurenotificationhub/)

Pushbenachrichtigungen liefern Informationen von einem Back-End-System an eine mobile Anwendung. Apple, Google und andere Plattformen verfügen jeweils über einen eigenen Pushbenachrichtigungsdienst (PNS). Azure Notification Hubs ermöglichen Ihnen, Benachrichtigungen plattformübergreifend zu zentralisieren, damit Ihre Back-End-Anwendung mit einem einzelnen Hub kommunizieren kann, der die Verteilung der Benachrichtigungen an die einzelnen plattformspezifischen PNS übernimmt.

Integrieren Sie Azure Notification Hubs in mobile Apps, indem Sie die folgenden Schritte ausführen:

1. [Einrichten von Pushbenachrichtigungsdiensten und Azure Notification Hub](#set-up-push-notification-services-and-azure-notification-hub).
1. [Verstehen, wie Sie Vorlagen und Tags verwenden](#register-templates-and-tags-with-the-azure-notification-hub).
1. [Erstellen einer plattformübergreifenden Xamarin.Forms-Anwendung](#xamarinforms-application-functionality).
1. [Konfigurieren des nativen Android-Projekts für Pushbenachrichtigungen](#configure-the-android-application-for-notifications).
1. [Konfigurieren des nativen iOS-Projekts für Pushbenachrichtigungen](#configure-ios-for-notifications).
1. [Testen von Benachrichtigungen mithilfe des Azure Notification Hub](#test-notifications-in-the-azure-portal).
1. [Erstellen einer Back-End-Anwendung zum Senden von Benachrichtigungen](#create-a-notification-dispatcher).

> [!NOTE]
> Wenn Sie kein [Azure-Abonnement](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing) besitzen, erstellen Sie ein [kostenloses Konto](https://aka.ms/azfree-docs-mobileapps), bevor Sie beginnen.

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Einrichten von Pushbenachrichtigungsdiensten und Azure Notification Hub

Das Integrieren von Azure Notification Hubs in eine mobile Xamarin.Forms-App ähnelt der Integration von Azure Notification Hubs in eine native Xamarin-Anwendung. Richten Sie eine **FCM-Anwendung** ein, indem Sie die Schritte für die Firebase-Konsole unter [Pushen von Benachrichtigungen an Xamarin.Android mithilfe von Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging) ausführen. Führen Sie folgende Schritte mithilfe des Xamarin.Android-Tutorials aus:

1. Definieren Sie einen Android-Paketnamen, z. B. `com.xamarin.notifysample`, der im Beispiel verwendet wird.
1. Laden Sie die Datei **google-services.json** über die Firebase-Konsole herunter. Diese Datei fügen Sie in einem späteren Schritt zu Ihrer Android-Anwendung hinzu.
1. Erstellen Sie eine Azure Notification Hub-Instanz, und benennen Sie diese. In diesem Artikel und Beispiel wird `xdocsnotificationhub` als Hubname verwendet.
1. Kopieren Sie den FCM-**Serverschlüssel**, und speichern Sie ihn als **API-Schlüssel** unter **Google (GCM/FCM)** in Ihrem Azure Notification Hub.

Der folgende Screenshot zeigt die Google-Plattformkonfigurationen im Azure Notification Hub:

![Screenshot der Google-Konfiguration in Azure Notification Hub](azure-notification-hub-images/fcm-notification-hub-config.png "Google-Konfiguration in Azure Notification Hub")

Sie benötigen einen macOS-Computer, um das Setup für iOS-Geräte abzuschließen. Richten Sie APNS ein, indem Sie die anfänglichen Schritte unter [Pushen von Benachrichtigungen an Xamarin.iOS mithilfe von Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file) ausführen. Führen Sie folgende Schritte mithilfe des Xamarin.iOS-Tutorials aus:

1. Definieren Sie eine iOS-Bündel-ID. In diesem Artikel und Beispiel wird `com.xamarin.notifysample` als Bündel-ID verwendet.
1. Erstellen Sie eine Zertifikatsignieranforderungs-Datei (CSR-Datei), und verwenden Sie diese, um ein Pushbenachrichtigungszertifikat zu generieren.
1. Laden Sie das Pushbenachrichtigungszertifikat unter **Apple (APNS)** in Ihren Azure Notification Hub hoch.

Der folgende Screenshot zeigt die Apple-Plattformkonfigurationen im Azure Notification Hub:

![Screenshot der Apple-Konfiguration in Azure Notification Hub](azure-notification-hub-images/apns-notification-hub-config.png "Apple-Konfiguration in Azure Notification Hub")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Registrieren von Vorlagen und Tags bei Azure Notification Hub

Azure Notification Hub erfordert, dass mobile Anwendungen sich beim Hub registrieren, Vorlagen definieren und Tags abonnieren. Durch die Registrierung wird ein plattformspezifisches PNS-Handle mit einem Bezeichner im Azure Notification Hub verknüpft. Weitere Informationen zu Registrierungen finden Sie unter [Registrierungsverwaltung](/azure/notification-hubs/notification-hubs-push-notification-registration-management).

Mit Vorlagen können Geräte parametrisierte Nachrichtenvorlagen angeben. Eingehende Nachrichten können pro Gerät und Tag angepasst werden. Weitere Informationen zu Vorlagen finden Sie unter [Vorlagen](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).

Tags können verwendet werden, um Nachrichtenkategorien zu abonnieren, z. B. Nachrichten, Sport und Wetter. Der Einfachheit halber definiert die Beispielanwendung eine Standardvorlage mit einem einzelnen Parameter mit dem Namen `messageParam` und einem einzelnen Tag namens `default`. In komplexeren Systemen können benutzerspezifische Tags verwendet werden, um einen Benutzer über Geräte hinweg mit personalisierten Benachrichtigungen zu informieren. Weitere Informationen zu Tags finden Sie unter [Weiterleitung und Tagausdrücke](/azure/notification-hubs/notification-hubs-tags-segment-push-message).

Um Nachrichten erfolgreich zu empfangen, muss jede native Anwendung diese Schritte ausführen:

1. Abrufen eines PNS-Handles oder -Tokens vom Plattform-PNS.
1. Registrieren des PNS-Handles bei Azure Notification Hub.
1. Angeben einer Vorlage, die dieselben Parameter wie ausgehende Nachrichten enthält.
1. Abonnieren des Tag, das Ziel für ausgehende Nachrichten ist.

Diese Schritte werden für jede Plattform in den Abschnitten [Konfigurieren der Android-Anwendung für Benachrichtigungen](#configure-the-android-application-for-notifications) und [Konfigurieren von iOS für Benachrichtigungen](#configure-ios-for-notifications) ausführlicher beschrieben.

## <a name="xamarinforms-application-functionality"></a>Funktionalität der Xamarin.Forms-Anwendung

Die Xamarin.Forms-Beispielanwendung zeigt eine Liste mit Pushbenachrichtigungsmeldungen an. Dies wird mithilfe der `AddMessage`-Methode erzielt, die die angegebene Pushbenachrichtigungsmeldung der Benutzeroberfläche hinzufügt. Diese Methode verhindert außerdem, dass doppelte Nachrichten zur Benutzeroberfläche hinzugefügt werden, und wird im Hauptthread ausgeführt, sodass Sie von jedem Thread aufgerufen werden kann. Der folgende Code veranschaulicht die `AddMessage`-Methode:

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

Die Beispielanwendung enthält eine **AppConstants.cs**-Datei, die Eigenschaften definiert, die von den Plattformprojekten verwendet werden. Diese Datei muss mit Werten aus Ihrem Azure Notification Hub angepasst werden. Der folgende Code zeigt die **AppConstants.cs**-Datei:

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

* `NotificationHubName`: Verwenden Sie den Namen des Azure Notification Hub, den Sie in Ihrem Azure-Portal erstellt haben.
* `ListenConnectionString`: Diesen Wert finden Sie im Azure Notification Hub unter **Zugriffsrichtlinien**.

Der folgende Screenshot zeigt, wo sich diese Werte im Azure-Portal befinden:

![Screenshot der Zugriffsrichtlinie von Azure Notification Hub](azure-notification-hub-images/notification-hub-access-policy.png "Zugriffsrichtlinie von Azure Notification Hub")

## <a name="configure-the-android-application-for-notifications"></a>Konfigurieren der Android-Anwendung für Benachrichtigungen

Führen Sie die folgenden Schritte aus, um die Android-Anwendung für das Empfangen und Verarbeiten von Benachrichtigungen zu konfigurieren:

1. Konfigurieren Sie den **Namen des Android-Pakets** so, dass er mit dem Paketnamen in der Firebase-Konsole übereinstimmt.
1. Installieren Sie die folgenden NuGet-Pakete, um mit Google Play, Firebase und Azure Notification Hubs zu interagieren:
    1. Xamarin.GooglePlayServices.Base.
    1. Xamarin.Firebase.Messaging.
    1. Xamarin.Azure.NotificationHubs.Android.
1. Kopieren Sie die `google-services.json`-Datei, die Sie während des FCM-Setups heruntergeladen haben, in das Projekt, und legen Sie die Buildaktion auf `GoogleServicesJson` fest.
1. [Konfigurieren Sie „AndroidManifest.xml“ für die Kommunikation mit Firebase](#configure-android-manifest).
1. [Setzen Sie FirebaseMessagingService außer Kraft, um Nachrichten zu verarbeiten](#override-firebasemessagingservice-to-handle-messages).
1. [Fügen Sie eingehende Benachrichtigungen zur Xamarin.Forms-Benutzeroberfläche hinzu](#add-incoming-notifications-to-the-xamarinforms-ui).

> [!NOTE]
> Die Buildaktion **GoogleServicesJson** ist Teil des NuGet-Pakets **Xamarin.GooglePlayServices.Base**. Visual Studio 2019 legt die verfügbaren Buildaktionen während des Starts fest. Wenn **GoogleServicesJson** nicht als Buildaktion angezeigt wird, starten Sie Visual Studio 2019 nach der Installation der NuGet-Pakete neu.

### <a name="configure-android-manifest"></a>Konfigurieren des Android-Manifests

Die `receiver`-Elemente innerhalb des `application`-Elements ermöglichen der App, mit Firebase zu kommunizieren. Die `uses-permission`-Elemente ermöglichen der App das Verarbeiten von Nachrichten und das Registrieren bei Azure Notification Hub. Die vollständige **AndroidManifest.xml** sollte dem nachstehenden Beispiel ähneln:

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

### <a name="override-firebasemessagingservice-to-handle-messages"></a>Außerkraftsetzen von FirebaseMessagingService, um Nachrichten zu verarbeiten

Um sich bei Firebase zu registrieren und Nachrichten zu verarbeiten, erstellen Sie Unterklassen für die `FirebaseMessagingService`-Klasse. Dieselbe Anwendung definiert eine `FirebaseService`-Klasse, die Unterklassen von `FirebaseMessagingService` erstellt. Diese Klasse ist mit einem `IntentFilter`-Attribut gekennzeichnet, das den `com.google.firebase.MESSAGING_EVENT`-Filter enthält. Dieser Filter ermöglicht Android, eingehende Nachrichten zur Verarbeitung an diese Klasse zu übergeben:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseService : FirebaseMessagingService
{
    // ...
}

```

Wenn die Anwendung gestartet wird, fordert das Firebase SDK automatisch einen eindeutigen Tokenbezeichner vom Firebase-Server an. Bei einer erfolgreich durchgeführten Anforderung wird die `OnNewToken`-Methode für die `FirebaseService`-Klasse aufgerufen. Das Beispielprojekt setzt diese Methode außer Kraft und registriert das Token bei Azure Notification Hubs:

```csharp
public override void OnNewToken(string token)
{
    // NOTE: save token instance locally, or log if desired

    SendRegistrationToServer(token);
}

void SendRegistrationToServer(string token)
{
    try
    {
        NotificationHub hub = new NotificationHub(AppConstants.NotificationHubName, AppConstants.ListenConnectionString, this);

        // register device with Azure Notification Hub using the token from FCM
        Registration registration = hub.Register(token, AppConstants.SubscriptionTags);

        // subscribe to the SubscriptionTags list with a simple template.
        string pnsHandle = registration.PNSHandle;
        TemplateRegistration templateReg = hub.RegisterTemplate(pnsHandle, "defaultTemplate", AppConstants.FCMTemplateBody, AppConstants.SubscriptionTags);
    }
    catch (Exception e)
    {
        Log.Error(AppConstants.DebugTag, $"Error registering device: {e.Message}");
    }
}
```

Die `SendRegistrationToServer`-Methode registriert das Gerät bei Azure Notification Hub und abonniert Tags mit einer Vorlage. Die Beispielanwendung definiert ein einzelnes Tag namens `default` und eine Vorlage mit einem einzelnen Parameter namens `messageParam` in der Datei **AppConstants.cs**. Weitere Informationen zur Registrierung, zu Tags und Vorlagen finden Sie unter [Registrieren von Vorlagen und Tags bei Azure Notification Hub](#register-templates-and-tags-with-the-azure-notification-hub).

Wenn eine Nachricht empfangen wird, wird die `OnMessageReceived`-Methode für die `FirebaseService`-Klasse aufgerufen:

```csharp
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
    
    //Unique request code to avoid PendingIntent collision.
    var requestCode = new Random().Next();
    var pendingIntent = PendingIntent.GetActivity(this, requestCode, intent, PendingIntentFlags.OneShot);

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
```

Eingehende Nachrichten werden mit der `SendLocalNotification`-Methode in eine lokale Benachrichtigung konvertiert. Diese Methode erstellt ein neues `Intent` und legt den Nachrichteninhalt in dem `Intent` als `string` `Extra` ab. Wenn der Benutzer auf die lokale Benachrichtigung tippt, unabhängig davon, ob sich die App im Vorder- oder Hintergrund befindet, wird die `MainActivity` gestartet und erhält über das `Intent`-Objekt Zugriff auf den Nachrichteninhalt.

Die lokale Benachrichtigung und das `Intent`-Beispiel erfordern, dass der Benutzer die Aktion des Tippens auf die Benachrichtigung durchführt. Dies ist wünschenswert, wenn der Benutzer vor der Änderung des Anwendungszustands Maßnahmen ergreifen soll. Möglicherweise möchten Sie jedoch auf die Nachrichtendaten zugreifen, ohne dass in manchen Fällen eine Benutzeraktion erforderlich ist. Im vorherigen Beispiel wird die Nachricht mit der `SendMessageToMainPage`-Methode auch direkt an die aktuelle `MainPage`-Instanz gesendet. Wenn Sie in der Produktion beide Methoden für einen einzelnen Nachrichtentyp implementieren, erhält das `MainPage`-Objekt doppelte Nachrichten, wenn der Benutzer auf die Benachrichtigung tippt.

> [!NOTE]
> Die Android-Anwendung empfängt nur Pushbenachrichtigungen, wenn sie entweder im Hintergrund oder im Vordergrund ausgeführt wird. Um Pushbenachrichtigungen zu empfangen, wenn die Haupt-`Activity` nicht ausgeführt wird, müssen Sie einen Dienst implementieren, der den Rahmen dieses Beispiels sprengt. Weitere Informationen finden Sie unter [Erstellen von Android-Diensten](/xamarin/android/app-fundamentals/services/).

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Hinzufügen eingehender Benachrichtigungen zur Xamarin.Forms-Benutzeroberfläche

Die `MainActivity`-Klasse muss die Berechtigung zum Verarbeiten von Benachrichtigungen und zum Verwalten eingehender Nachrichtendaten erhalten. Der folgende Code zeigt die vollständige `MainActivity`-Implementierung:

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

Das `Activity`-Attribut legt die Anwendung `LaunchMode` auf `SingleTop` fest. Dieser Startmodus weist das Android-Betriebssystem an, nur eine einzelne Instanz dieser Aktivität zuzulassen. Bei diesem Startmodus werden eingehende `Intent`-Daten an die `OnNewIntent`-Methode weitergeleitet, die Nachrichtendaten extrahiert und über die `AddMessage`-Methode an die `MainPage`-Instanz sendet. Wenn Ihre Anwendung einen anderen Startmodus verwendet, muss sie `Intent`-Daten anders verarbeiten.

Die `OnCreate`-Methode verwendet eine Hilfsmethode namens `IsPlayServiceAvailable`, um sicherzustellen, dass das Gerät den Google Play-Dienst unterstützt. Emulatoren oder Geräte, die den Google Play-Dienst nicht unterstützen, können keine Pushbenachrichtigungen von Firebase empfangen.

## <a name="configure-ios-for-notifications"></a>Konfigurieren von iOS für Benachrichtigungen

Der Vorgang zum Konfigurieren der iOS-Anwendung zum Empfangen von Benachrichtigungen lautet wie folgt:

1. Konfigurieren Sie die **Bündel-ID** in der Datei **Info.plist** so, dass sie dem im Bereitstellungsprofil verwendeten Wert entspricht.
1. Fügen Sie die Option **Pushbenachrichtigungen aktivieren** zu der Datei **Entitlements.plist** hinzu.
1. Fügen Sie das NuGet-Paket **Xamarin.Azure.NotificationHubs.iOS** zu Ihrem Projekt hinzu.
1. [Registrieren Sie für Benachrichtigungen mit APNS](#register-for-notifications-with-apns).
1. [Registrieren Sie die Anwendung bei Azure Notification Hub, und abonnieren Sie Tags](#register-with-azure-notification-hub-and-subscribe-to-tags).
1. [Fügen Sie APNS-Benachrichtigungen zur Xamarin.Forms-Benutzeroberfläche hinzu](#add-apns-notifications-to-xamarinforms-ui).

Der folgende Screenshot zeigt die ausgewählte Option **Pushbenachrichtigungen aktivieren** in der Datei **Entitlements.plist** in Visual Studio:

![Screenshot der Berechtigung für Pushbenachrichtigungen](azure-notification-hub-images/push-notification-entitlement.png "Berechtigung für Pushbenachrichtigungen")

### <a name="register-for-notifications-with-apns"></a>Registrieren für Benachrichtigungen mit APNS

Die `FinishedLaunching`-Methode in der Datei **AppDelegate.cs** muss außer Kraft gesetzt werden, um sich für Remotebenachrichtigungen zu registrieren. Die Registrierung variiert abhängig von der iOS-Version, die auf dem Gerät verwendet wird. Das iOS-Projekt in der Beispielanwendung setzt die `FinishedLaunching`-Methode außer Kraft, damit `RegisterForRemoteNotifications` aufgerufen wird, wie im folgenden Beispiel gezeigt:

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

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Registrieren bei Azure Notification Hub und Abonnieren von Tags

Wenn das Gerät während der `FinishedLaunching`-Methode erfolgreich für Remotebenachrichtigungen registriert wurde, ruft iOS die `RegisteredForRemoteNotifications`-Methode auf. Diese Methode sollte außer Kraft gesetzt werden, damit folgende Aktionen ausgeführt werden:

1. Instanziieren von `SBNotificationHub`.
1. Aufheben aller vorhandenen Registrierungen.
1. Registrieren des Geräts bei Ihrem Notification Hub.
1. Abonnieren bestimmter Tags mit einer Vorlage.

Weitere Informationen zur Registrierung des Geräts, von Vorlagen und Tags finden Sie unter [Registrieren von Vorlagen und Tags bei Azure Notification Hub](#register-templates-and-tags-with-the-azure-notification-hub). Der folgende Code veranschaulicht die Registrierung des Geräts und der Vorlagen:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    Hub = new SBNotificationHub(AppConstants.ListenConnectionString, AppConstants.NotificationHubName);

    // update registration with Azure Notification Hub
    Hub.UnregisterAll(deviceToken, (error) =>
    {
        if (error != null)
        {
            Debug.WriteLine($"Unable to call unregister {error}");
            return;
        }

        var tags = new NSSet(AppConstants.SubscriptionTags.ToArray());
        Hub.RegisterNative(deviceToken, tags, (errorCallback) =>
        {
            if (errorCallback != null)
            {
                Debug.WriteLine($"RegisterNativeAsync error: {errorCallback}");
            }
        });

        var templateExpiration = DateTime.Now.AddDays(120).ToString(System.Globalization.CultureInfo.CreateSpecificCulture("en-US"));
        Hub.RegisterTemplate(deviceToken, "defaultTemplate", AppConstants.APNTemplateBody, templateExpiration, tags, (errorCallback) =>
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
> Die Registrierung für Remotebenachrichtigungen kann in Situationen, in denen keine Netzwerkverbindung besteht, fehlschlagen. Sie können die `FailedToRegisterForRemoveNotifications`-Methode außer Kraft setzen, um Registrierungsfehler zu behandeln.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Hinzufügen von APNS-Benachrichtigungen zur Xamarin.Forms-Benutzeroberfläche

Wenn ein Gerät eine Remotebenachrichtigung empfängt, ruft iOS die `ReceivedRemoteNotification`-Methode auf. Eingehender Nachrichten-JSON-Code wird in ein `NSDictionary`-Objekt konvertiert, und die `ProcessNotification`-Methode extrahiert Werte aus dem Wörterbuch und sendet diese an die `MainPage`-Instanz von Xamarin.Forms. Die `ReceivedRemoteNotifications`-Methode wird außer Kraft gesetzt, damit `ProcessNotification` aufgerufen wird, wie im folgenden Code gezeigt:

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

Mit Azure Notification Hubs können Sie überprüfen, ob Ihre Anwendung Testnachrichten empfangen kann. Im Abschnitt **Test senden** des Notification Hubs können Sie die Zielplattform auswählen und eine Nachricht senden. Wenn Sie **Senden an Tagausdruck** auf `default` festlegen, werden Nachrichten an Anwendungen gesendet, die eine Vorlage für das `default`-Tag registriert haben. Wenn Sie auf die Schaltfläche **Senden** klicken, wird ein Bericht generiert, der die Anzahl der mit der Nachricht erreichten Geräte enthält. Der folgende Screenshot zeigt einen Android-Benachrichtigungstest im Azure-Portal:

![Screenshot einer Testnachricht in Azure Notification Hub](azure-notification-hub-images/azure-notification-hub-test-send.png "Testnachricht in Azure Notification Hub")

### <a name="testing-tips"></a>Tipps zum Testen

1. Wenn Sie testen, ob eine Anwendung Pushbenachrichtigungen empfangen kann, müssen Sie ein physisches Gerät verwenden. Virtuelle Android- und iOS-Geräte sind möglicherweise nicht ordnungsgemäß für den Empfang von Pushbenachrichtigungen konfiguriert.
1. Die Android-Beispielanwendung registriert ihr Token und ihre Vorlagen einmalig, wenn das Firebase-Token ausgegeben wird. Während des Testens müssen Sie möglicherweise ein neues Token anfordern und sich erneut beim Azure Notification Hub registrieren. Die beste Möglichkeit, dies zu erzwingen, besteht darin, Ihr Projekt zu bereinigen, die Ordner `bin` und `obj` zu löschen und die Anwendung vor der Neuerstellung und Bereitstellung vom Gerät zu deinstallieren.
1. Viele Teile des Pushbenachrichtigungsflusses werden asynchron ausgeführt. Dies kann dazu führen, dass Breakpoints in einer unerwarteten Reihenfolge oder gar nicht getroffen werden. Verwenden Sie die Geräte- oder Debugprotokollierung, um den Ablauf der Ausführung ohne Unterbrechung des Anwendungsflusses zu verfolgen. Filtern Sie das Android-Geräteprotokoll mithilfe des in `Constants` angegebenen `DebugTag`.
1. Wenn das Debuggen in Visual Studio beendet wird, wird das Schließen der App erzwungen. Alle Nachrichtenempfänger oder andere Dienste, die im Rahmen des Debugprozesses gestartet werden, werden geschlossen und reagieren nicht auf Nachrichtenereignisse.

## <a name="create-a-notification-dispatcher"></a>Erstellen eines Benachrichtigungsdispatchers

Azure Notification Hubs ermöglicht, dass Ihre Back-End-Anwendung Benachrichtigungen über Plattformen hinweg an Geräte verteilt. Im Beispiel wird die Benachrichtigungsverteilung mit der Konsolenanwendung **NotificationDispatcher** veranschaulicht. Die Anwendung enthält die Datei **DispatcherConstants.cs**, in der die folgenden Eigenschaften definiert sind:

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Sie müssen die Datei **DispatcherConstants.cs-** so konfigurieren, dass sie Ihrer Azure Notification Hub-Konfiguration entspricht. Der Wert der `SubscriptionTags`-Eigenschaft sollte den in den Client-Apps verwendeten Werten entsprechen. Die `NotificationHubName`-Eigenschaft ist der Name Ihrer Azure Notification Hub-Instanz. Die `FullAccessConnectionString`-Eigenschaft ist der Zugriffsschlüssel, der sich in den **Zugriffsrichtlinien** Ihres Notification Hubs befindet. Der folgende Screenshot zeigt die Positionen der Eigenschaften `NotificationHubName` und `FullAccessConnectionString` im Azure-Portal:

![Screenshot von Name und FullAccessConnectionString von Azure Notification Hub](azure-notification-hub-images/notification-hub-full-access-policy.png "Name und FullAccessConnectionString von Azure Notification Hub")

Die Konsolenanwendung durchläuft die einzelnen `SubscriptionTags`-Werte und sendet mithilfe einer Instanz der `NotificationHubClient`-Klasse Benachrichtigungen an Abonnenten. Der folgende Code zeigt die `Program`-Klasse der Konsolenanwendung:

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

Wenn die Beispielkonsolenanwendung ausgeführt wird, kann die Leertaste gedrückt werden, um Nachrichten zu senden. Geräte, auf denen die Clientanwendungen ausgeführt werden, sollten nummerierte Benachrichtigungen empfangen, sofern sie ordnungsgemäß konfiguriert sind.

## <a name="related-links"></a>Verwandte Links

* [Pushbenachrichtigungsvorlagen](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).
* [Geräteregistrierungsverwaltung](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [Weiterleitung und Tagausdrücke](/azure/notification-hubs/notification-hubs-tags-segment-push-message).
* [Tutorial zu Xamarin.Android Azure in Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Tutorial zu Xamarin.iOS in Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
