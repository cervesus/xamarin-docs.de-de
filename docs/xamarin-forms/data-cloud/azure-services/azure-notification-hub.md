---
title: Senden und Empfangen von Pushbenachrichtigungen mit Azure Notification Hubs und Xamarin.Forms
description: In diesem Artikel wird erläutert, wie Sie mit Azure Notification Hubs zum Senden von plattformübergreifenden Pushbenachrichtigungen an Xamarin.Forms-Anwendungen.
ms.prod: xamarin
ms.assetid: 07D13195-3A0D-4C95-ACF0-143A9084973C
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 05/23/2019
ms.openlocfilehash: fb2f108ba115690ca181738486fd8310f26bb909
ms.sourcegitcommit: 58d8bbc19ead3eb535fb8248710d93ba0892e05d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/09/2019
ms.locfileid: "67674516"
---
# <a name="send-and-receive-push-notifications-with-azure-notification-hubs-and-xamarinforms"></a>Senden und Empfangen von Pushbenachrichtigungen mit Azure Notification Hubs und Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png)Herunterladen des Beispiels](https://github.com/xamarin/xamarin-forms-samples/tree/master/WebServices/AzureNotificationHub)

Übertragen Sie Benachrichtigungen liefern Informationen von einem Back-End-System, auf einer mobilen Anwendung. Apple, Google und anderen Plattformen jeder haben ihre eigenen Push Notification Service (PNS). Azure Notification Hubs können Sie Benachrichtigungen über Plattformen hinweg zentralisieren, damit die Back-End-Anwendung mit einem einzelnen Hub kommunizieren kann, die übernimmt die Benachrichtigungen an den einzelnen plattformspezifischen PNS zu verteilen.

Integrieren Sie Azure Notification Hubs in mobile apps, indem Sie folgende Schritte:

1. [Richten Sie Push Notification Services und Azure Notification Hub](#set-up-push-notification-services-and-azure-notification-hub).
1. [Grundlegendes zur Verwendung von Tags und Vorlagen](#register-templates-and-tags-with-the-azure-notification-hub).
1. [Erstellen Sie eine plattformübergreifende Xamarin.Forms-Anwendung](#xamarinforms-application-functionality).
1. [Konfigurieren Sie das native Android-Projekt für Pushbenachrichtigungen](#configure-the-android-application-for-notifications).
1. [Konfigurieren Sie das native iOS-Projekt für Pushbenachrichtigungen](#configure-ios-for-notifications).
1. [Testen Sie die Benachrichtigungen mithilfe von Azure Notification Hub](#test-notifications-in-the-azure-portal).
1. [Erstellen Sie eine Back-End-Anwendung zum Senden von Benachrichtigungen](#create-a-notification-dispatcher).

## <a name="set-up-push-notification-services-and-azure-notification-hub"></a>Richten Sie Push Notification Services und Azure Notification Hub

Die Integration von Azure Notification Hubs mit einer mobilen Xamarin.Forms-app ähnelt der Integration von Azure Notification Hubs mit einer nativen Xamarin-Anwendung. Richten Sie eine **FCM-Anwendung** indem Sie die folgenden Schritte der Firebase-Konsole im [von Pushbenachrichtigungen an Xamarin.Android, die mit Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm#create-a-firebase-project-and-enable-firebase-cloud-messaging). Führen Sie die folgenden Schritte aus, die mit dem Xamarin.Android-Lernprogramm:

1. Definieren Sie einen Namen für die Android-Paket wie z. B. `com.xamarin.notifysample`, die im Beispiel verwendet wird.
1. Herunterladen **Google-services.json** über die Firebase-Konsole. Sie fügen diese Datei auf Ihrem Android-Anwendung in einem späteren Schritt.
1. Erstellen Sie eine Azure Notification Hub-Instanz, und geben sie einen Namen ein. Diese Artikel und Beispiel-Verwendung `xdocsnotificationhub` als Hub-Name.
1. Kopieren Sie den FCM **Serverschlüssel** und speichern Sie ihn der **API-Schlüssel** unter **Google (GCM/FCM)** in Ihrem Azure Notification Hub.

Der folgende Screenshot zeigt die Konfiguration der Google-Plattform in der Azure Notification Hub:

![Screenshot, der Konfiguration der Azure Notification Hub Google](azure-notification-hub-images/fcm-notification-hub-config.png "Azure Notification Hub Google-Konfiguration")

Sie benötigen einen MacOS-Computer, auf den Abschluss des Setups für iOS-Geräte. Richten Sie APNS, indem die ersten im Schritte folgenden [von Pushbenachrichtigungen an Xamarin.iOS, die mit Azure Notification Hubs](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started#generate-the-certificate-signing-request-file). Führen Sie die folgenden Schritte aus, die mit dem Xamarin.iOS-Lernprogramm:

1. Definieren Sie einen iOS-Bundle-Bezeichner. Diese Artikel und Beispiel-Verwendung `com.xamarin.notifysample` als der Bundle-Bezeichner.
1. Erstellen Sie eine Zertifikatsignieranforderung (CSR)-Datei, und verwenden Sie, um einen Push Notification-Zertifikat zu generieren.
1. Unter den Push Notification-Zertifikat hochladen **Apple (APNS)** in Ihrem Azure Notification Hub.

Der folgende Screenshot zeigt die Apple-Plattform-Konfiguration in Azure Notification Hub:

![Screenshot, der Konfiguration der Azure Notification Hub Apple](azure-notification-hub-images/apns-notification-hub-config.png "Azure Notification Hub Apple Configurator")

## <a name="register-templates-and-tags-with-the-azure-notification-hub"></a>Registrieren von Tags und Vorlagen mit Azure Notification Hub

Azure Notification Hub ist erforderlich, mobile Anwendungen mit dem Hub registrieren, definieren Vorlagen und Tags abonnieren. Registrierung verknüpft einen plattformspezifischen PNS-Handle in einen Bezeichner in der Azure Notification Hub. Weitere Informationen zu Registrierungen finden Sie unter [Registrierungsverwaltung](/azure/notification-hubs/notification-hubs-push-notification-registration-management).

Vorlagen können Geräte, die parametrisierte Meldungsvorlagen angeben. Eingehende Nachrichten können pro Gerät pro Tag angepasst werden. Weitere Informationen zu Vorlagen finden Sie unter [Vorlagen](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).

Tags können zum Abonnieren von Meldungskategorien wie z. B. Nachrichten, Sport und Wetter verwendet werden. Die beispielanwendung definiert eine Standardvorlage der Einfachheit halber mit einem einzigen Parameter namens `messageParam` und ein einzelnes Tag namens `default`. In komplexeren Systemen können benutzerdefinierte Tags einen Benutzer auf Geräten für personalisierte Benachrichtigungen Nachricht verwendet werden. Weitere Informationen zu Tags finden Sie unter [Weiterleitung und tagausdrücke](/azure/notification-hubs/notification-hubs-tags-segment-push-message).

Um Nachrichten erfolgreich empfangen zu können, muss jede systemeigene Anwendung diese Schritte ausführen:

1. Rufen Sie ein PNS-Handle oder Token, von dem Plattform-PNS.
1. Registrieren Sie das PNS-Handle, mit Azure Notification Hub.
1. Geben Sie eine Vorlage, die die gleichen Parameter wie ausgehender Nachrichten enthält.
1. Abonnieren Sie das Tag für die ausgehende Nachrichten.

Diese Schritte werden ausführlicher für jede Plattform in beschrieben die [konfigurieren die Android-Anwendung für Benachrichtigungen](#configure-the-android-application-for-notifications) und [iOS-Benachrichtigungen konfigurieren](#configure-ios-for-notifications) Abschnitte.

## <a name="xamarinforms-application-functionality"></a>Die Funktionalität der Xamarin.Forms-Anwendung

Die Xamarin.Forms-beispielanwendung zeigt eine Liste von Pushbenachrichtigungen an. Dies wird erreicht, mit der `AddMessage` -Methode, die die Benutzeroberfläche der angegebenen pushbenachrichtigungsmeldung hinzufügt. Diese Methode wird auch verhindert, dass doppelte Nachrichten an die Benutzeroberfläche hinzugefügt und im Hauptthread ausgeführt wird, damit sie von jedem Thread aufgerufen werden kann. Der folgende Code veranschaulicht die `AddMessage`-Methode:

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

Die beispielanwendung enthält ein **AppConstants.cs** Datei, die definiert Eigenschaften, die von der Plattform-Projekten verwendet wird. Diese Datei muss mit Werten aus Ihrem Azure-Benachrichtigungshub angepasst werden. Der folgende code zeigt die **AppConstants.cs** Datei:

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

Passen Sie die folgenden Werte in `AppConstants` verbinden Sie die beispielanwendung zum Azure Notification Hub:

* `NotificationHubName`: Verwenden Sie den Namen des Azure Notification Hubs Sie in Ihrem Azure-Portal erstellt haben.
* `ListenConnectionString`: Dieser Wert befindet sich in der Azure Notification Hub unter **Zugriffsrichtlinien**.

Der folgende Screenshot zeigt, wo sich diese Werte im Azure-Portal befinden:

![Screenshot der Azure Notification Hub-Zugriffsrichtlinie](azure-notification-hub-images/notification-hub-access-policy.png "Azure Notification Hub-Zugriffsrichtlinie")

## <a name="configure-the-android-application-for-notifications"></a>Konfigurieren der Android-Anwendung für Benachrichtigungen

Führen Sie die folgenden Schritte aus, um die Android-Anwendung zum Empfangen und Verarbeiten von Benachrichtigungen zu konfigurieren:

1. Konfigurieren Sie die Android **Paketname** entsprechend den Paketnamen in der Firebase-Konsole.
1. Installieren Sie die folgenden NuGet-Pakete für die Interaktion mit Google Play, Firebase und Azure Notification Hubs:
    1. Xamarin.GooglePlayServices.Base.
    1. Xamarin.Firebase.Messaging.
    1. Xamarin.Azure.NotificationHubs.Android.
1. Kopieren der `google-services.json` -Datei, die Sie während des Setups von FCM auf das Projekt heruntergeladen, und legen Sie die Buildaktion auf `GoogleServicesJson`.
1. [Konfigurieren Sie die Datei "androidmanifest.xml", für die Kommunikation mit Firebase](#configure-android-manifest).
1. [Registrieren Sie die Anwendung mit Firebase und Azure Notification Hub mithilfe einer `FirebaseInstanceIdService` ](#register-using-a-custom-firebaseinstanceidservice).
1. [Verarbeiten von Nachrichten mit einem `FirebaseMessagingService` ](#process-messages-with-a-firebasemessagingservice).
1. [Hinzufügen von eingehende Benachrichtigungen zu Xamarin.Forms-UI](#add-incoming-notifications-to-the-xamarinforms-ui).

> [!NOTE]
> Die **GoogleServicesJson** Buildvorgang ist Teil der **Xamarin.GooglePlayServices.Base** NuGet-Paket. Visual Studio-2019 legt die verfügbaren erstellen Aktionen während des Starts fest. Wenn Sie nicht sehen **GoogleServicesJson** als Aktion zu erstellen, neu starten 2019 für Visual Studio nach der Installation der NuGet-Pakete.

### <a name="configure-android-manifest"></a>Konfigurieren von Android-Manifest.

Die `receiver` Elemente innerhalb der `application` Element kann die app für die Kommunikation mit Firebase. Die `uses-permission` die app Nachrichten behandeln, und registrieren beim Azure Notification Hub zu ermöglichen. Die vollständige **"androidmanifest.xml"** sollte etwa wie im folgenden Beispiel aussehen:

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

### <a name="register-using-a-custom-firebaseinstanceidservice"></a>Registrieren Sie mithilfe einer benutzerdefinierten FirebaseInstanceIdService

Firebase stellt Token, die ein Gerät auf das PNS eindeutig zu identifizieren. Token haben eine lange Lebensdauer, jedoch werden gelegentlich aktualisiert. Wenn ein Token ausgestellt oder aktualisiert wird, muss die Anwendung der neue Token mit dem Azure Notification Hub registrieren. Registrierung erfolgt durch eine Instanz einer Klasse, die von abgeleitet `FirebaseInstanceIdService`.

In der beispielanwendung `FirebaseRegistrationService` Klasse erbt von `FirebaseInstanceIdService`. Diese Klasse verfügt über eine `IntentFilter` , umfasst `com.google.firebase.INSTANCE_ID_EVENT`, Android-Betriebssystems automatisch aufrufen, sodass `OnTokenRefresh` Wenn ein Token von Firebase ausgestellt wird.

Der folgende Code zeigt die benutzerdefinierte `FirebaseInstanceIdService` aus der beispielanwendung:

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

Die `SendRegistrationToServer` -Methode in der die `FirebaseRegistrationClass` registriert das Gerät bei Azure Notification Hub und Tags mit einer Vorlage abonniert. Die beispielanwendung definiert ein einzelnes Tag namens `default` und eine Vorlage mit einem einzigen Parameter namens `messageParam` in die **AppConstants.cs** Datei. Weitere Informationen zur Registrierung, Tags und Vorlagen finden Sie unter [Tags und Vorlagen mit Azure Notification Hub registrieren](#register-templates-and-tags-with-the-azure-notification-hub)

### <a name="process-messages-with-a-firebasemessagingservice"></a>Verarbeiten von Nachrichten mit einem FirebaseMessagingService

Eingehende Nachrichten weitergeleitet werden, um eine `FirebaseMessagingService` -Instanz, in dem sie in eine lokale Benachrichtigung konvertiert werden können. Das Android-Projekt in der beispielanwendung enthält eine Klasse namens `FirebaseService` von erbt `FirebaseMessagingService`. Diese Klasse verfügt über eine `IntentFilter` , umfasst `com.google.firebase.MESSAGING_EVENT`, Android-Betriebssystems automatisch aufrufen, sodass `OnMessageReceived` Wenn pushbenachrichtigungsmeldung empfangen wird.

Das folgende Beispiel zeigt die `FirebaseService` aus der beispielanwendung:

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

Eingehende Nachrichten werden in eine lokale Benachrichtigung mit konvertiert die `SendLocalNotification` Methode. Diese Methode erstellt ein neues `Intent` und legt den Nachrichteninhalt in das `Intent` als eine `string` `Extra`. Wenn der Benutzer auf die lokale Benachrichtigung tippt, ob die app im Vorder- oder Hintergrund ist die `MainActivity` wird gestartet und hat Zugriff auf den Inhalt der Nachricht durch die `Intent` Objekt.

Der lokale Benachrichtigung und `Intent` Beispiel muss der Benutzer eine Aktion Tippen auf die Benachrichtigung. Dies ist wünschenswert, wenn der Benutzer vor der Anwendung ändert sich ergreifen soll. Möglicherweise möchten jedoch die Nachrichtendaten zuzugreifen, ohne dass eine Benutzeraktion in einigen Fällen. Im vorherige Beispiel sendet die Nachricht auch direkt mit dem aktuellen `MainPage` -Instanz mit der `SendMessageToMainPage` Methode. In der Produktion, wenn Sie nur einen Nachrichtentyp, beide Methoden implementieren die `MainPage` Objekt erhalten doppelte Nachrichten, wenn der Benutzer auf die Benachrichtigung tippt.

> [!NOTE]
> Die Android-Anwendung erhalten nur Push-Benachrichtigungen, wenn sie in den Hintergrund oder den Vordergrund ausgeführt wird. Zum Empfangen von Pushbenachrichtigungen bei der Hauptseite `Activity` wird nicht ausgeführt wird, müssen Sie einen Dienst, der den Rahmen dieses Beispiels sprengen implementieren. Weitere Informationen finden Sie unter [Erstellen von Android-Dienste](/xamarin/android/app-fundamentals/services/)

### <a name="add-incoming-notifications-to-the-xamarinforms-ui"></a>Hinzufügen von eingehende Benachrichtigungen zu der Xamarin.Forms-UI

Die `MainActivity` Klasse muss über die Berechtigung zum Verarbeiten von Benachrichtigungen und Verwalten von Daten der eingehenden Nachricht zu erhalten. Der folgende Code zeigt die vollständige `MainActivity` Implementierung:

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

        if (IsPlayServiceAvailable() == false)
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

Die `Activity` Attribut legt fest, die Anwendung `LaunchMode` zu `SingleTop`. Dieser Startmodus weist das Android-Betriebssystem zu, dass nur eine einzelne Instanz dieser Aktivität. In diesem Startmodus, die eingehenden `Intent` Daten weitergeleitet werden, um die `OnNewIntent` -Methode, die Nachrichtendaten extrahiert und sendet sie an der `MainPage` -Instanz über die `AddMessage` Methode. Wenn Ihre Anwendung auf einen anderen Startmodus verwendet, müssen sie behandeln `Intent` Daten anders.

Die `OnCreate` Methode verwendet eine Hilfsmethode namens `IsPlayServiceAvailable` um sicherzustellen, dass das Gerät unterstützt Google Play-Dienst. -Emulatoren oder-Geräten, die Google Play-Dienst nicht unterstützen können nicht von Pushbenachrichtigungen von Firebase erhalten.

## <a name="configure-ios-for-notifications"></a>IOS-Benachrichtigungen konfigurieren

Der Prozess für die Konfiguration der iOS-Anwendung zum Empfangen von Benachrichtigungen ist:

1. Konfigurieren der **Bündel-ID** in die **"Info.plist"** Datei, die mit dem Wert, der verwendet wird, im Bereitstellungsprofil überein.
1. Hinzufügen der **Pushbenachrichtigungen aktivieren** die Möglichkeit, die **"Entitlements.plist"** Datei.
1. Hinzufügen der **Xamarin.Azure.NotificationHubs.iOS** NuGet-Paket Ihrem Projekt.
1. [Registrieren für Benachrichtigungen mit APNS](#register-for-notifications-with-apns).
1. [Registrieren Sie die Anwendung mit Azure Notification Hub, und Abonnieren von Tags](#register-with-azure-notification-hub-and-subscribe-to-tags).
1. [Hinzufügen von APNS-Benachrichtigungen zu Xamarin.Forms-UI](#add-apns-notifications-to-xamarinforms-ui).

Der folgende Screenshot zeigt die **Pushbenachrichtigungen aktivieren** gewählten Option in der **"Entitlements.plist"** Datei in Visual Studio:

![Screenshot der Push Notifications-Berechtigung](azure-notification-hub-images/push-notification-entitlement.png "Push Notifications-Berechtigung")

### <a name="register-for-notifications-with-apns"></a>Registrieren für Benachrichtigungen mit APNS

Die `FinishedLaunching` -Methode in der die **Datei "appdelegate.cs"** Datei überschrieben werden muss, um remotebenachrichtigungen zu registrieren. Registrierung unterscheidet sich je nach der iOS-Version auf dem Gerät verwendet wird. Das iOS-Projekt in der beispielanwendung überschreibt die `FinishedLaunching` aufzurufende Methode `RegisterForRemoteNotifications` wie im folgenden Beispiel gezeigt:

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

### <a name="register-with-azure-notification-hub-and-subscribe-to-tags"></a>Mit Azure Notification Hub registrieren, und Abonnieren von tags

Wenn hat das Gerät erfolgreich registriert von remotebenachrichtigungen während der `FinishedLaunching` -Methode, die iOS aufrufen wird die `RegisteredForRemoteNotifications` Methode. Diese Methode sollte überschrieben werden, um die folgenden Aktionen ausführen:

1. Instanziieren der `SBNotificationHub`.
1. Aufheben der Registrierung alle vorhandenen Registrierungen.
1. Registrieren Sie das Gerät bei Ihrem Notification Hub.
1. Abonnieren Sie bestimmte Tags mit einer Vorlage aus.

Weitere Informationen zur Registrierung des Geräts, Vorlagen, und Tags finden Sie unter [Tags und Vorlagen mit Azure Notification Hub registrieren](#register-templates-and-tags-with-the-azure-notification-hub). Der folgende Code veranschaulicht die Registrierung des Geräts und Vorlagen:

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
> Registrieren von remotebenachrichtigungen kann in Situationen, z. B. keine Netzwerkverbindung fehlschlagen. Sie können auch außer Kraft setzen der `FailedToRegisterForRemoveNotifications` Methode zum Behandeln von Registrierungsfehler.

### <a name="add-apns-notifications-to-xamarinforms-ui"></a>Hinzufügen von APNS-Benachrichtigungen zu Xamarin.Forms-UI

Wenn ein Gerät empfängt eine remotebenachrichtigung iOS Aufrufe der `ReceivedRemoteNotification` Methode. Eingehende Nachricht, die in JSON konvertiert eine `NSDictionary` -Objekt, und die `ProcessNotification` Methode extrahiert Werte aus dem Wörterbuch und sendet sie an der Xamarin.Forms `MainPage` Instanz. Die `ReceivedRemoteNotifications` Methode wird überschrieben, um Aufrufen `ProcessNotification` wie im folgenden Code gezeigt:

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

## <a name="test-notifications-in-the-azure-portal"></a>Testbenachrichtigungen im Azure-portal

Azure Notification Hubs können Sie überprüfen, dass Ihre Anwendung Testnachrichten empfangen kann. Die **Testsendevorgang** Abschnitt im Notification Hub können Sie wählen die Zielplattform aus, und Senden einer Nachricht. Festlegen der **senden an tagausdruck** zu `default` sendet Nachrichten an eine Vorlage für registrierte Anwendungen die `default` Tag. Klicken auf die **senden** Schaltfläche generiert einen Bericht, der die Anzahl von Geräten erreicht, mit der Meldung enthält. Der folgende Screenshot zeigt einen Android-benachrichtigungs-Test im Azure-Portal:

![Bildschirmabbildung von einer Azure Notification Hub-Testnachricht](azure-notification-hub-images/azure-notification-hub-test-send.png "Azure Notification Hub-Testnachricht")

### <a name="testing-tips"></a>Tipps für das Testen

1. Wenn Sie testen, dass eine Anwendung Pushbenachrichtigungen empfangen kann, müssen Sie ein physisches Gerät verwenden. Virtuelle Android und iOS-Geräte möglicherweise nicht ordnungsgemäß konfiguriert werden, um Pushbenachrichtigungen empfangen.
1. Die Beispiel-Android-Anwendung registriert die Token und die Vorlagen, sobald bei der Firebase-Token ausgestellt wird. Während des Tests müssen Sie ein neues Token anfordern und eine erneute Registrierung mit Azure Notification Hub. Die beste Möglichkeit, dies ist Ihr Projekt zu bereinigen, löschen Sie die `bin` und `obj` Ordner und deinstallieren Sie die Anwendung auf dem Gerät, bevor Sie neu zu erstellen und bereitstellen.
1. Viele Teile der pushbenachrichtigungs-Flows werden asynchron ausgeführt. Dies möglicherweise Haltepunkte nicht wird drücken oder in einer unerwarteten Reihenfolge erreicht wird. Verwenden Sie Gerät "oder" Debug-Protokollierung zur Ablaufverfolgung der codeausführung ohne Unterbrechung des Anwendungsflusses. Filtern Sie die Android-Gerät-Protokoll verwenden die `DebugTag` im angegebenen `Constants`.

## <a name="create-a-notification-dispatcher"></a>Erstellen Sie einen Benachrichtigung-Verteiler

Azure Notification Hubs ermöglichen, Ihre Back-End-Anwendung zum Senden von Benachrichtigungen an Geräte auf Plattformen. Das Beispiel zeigt die Verteilung der Benachrichtigung mit der **NotificationDispatcher** Konsolenanwendung. Die Anwendung enthält die **DispatcherConstants.cs** Datei, die die folgenden Eigenschaften definiert:

```csharp
public static class DispatcherConstants
{
    public static string[] SubscriptionTags { get; set; } = { "default" };
    public static string NotificationHubName { get; set; } = "< Insert your Azure Notification Hub name >";
    public static string FullAccessConnectionString { get; set; } = "< Insert your DefaultFullSharedAccessSignature >";
}
```

Sie müssen konfigurieren, die **DispatcherConstants.cs** entsprechend die Konfiguration von Azure Notification Hub. Der Wert des der `SubscriptionTags` Eigenschaft sollte mit den Werten übereinstimmen, die in die Client-apps verwendet. Die `NotificationHubName` Eigenschaft ist der Name der Azure Notification Hub-Instanz. Die `FullAccessConnectionString` -Eigenschaft ist der Zugriffsschlüssel finden Sie in Ihrem Notification Hub **Zugriffsrichtlinien**. Der folgende Screenshot zeigt den Speicherort der `NotificationHubName` und `FullAccessConnectionString` Eigenschaften im Azure-Portal:

![Screenshot der Azure Notification Hub-Namen und FullAccessConnectionString](azure-notification-hub-images/notification-hub-full-access-policy.png "Azure Notification Hub-Namen und FullAccessConnectionString")

Die Konsolenanwendung durchläuft alle `SubscriptionTags` Wert ein, und sendet Benachrichtigungen für Abonnenten mit einer Instanz von der `NotificationHubClient` Klasse. Der folgende Code zeigt die Konsolenanwendung `Program` Klasse:

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

Wenn die Beispiel einer Konsolenanwendung ausgeführt wird, kann die LEERTASTE gedrückt werden, um Nachrichten zu senden. Geräte mit die Client-Anwendungen erhalten sollen nummerierten Benachrichtigungen bereitgestellt, dass sie ordnungsgemäß konfiguriert sind.

## <a name="related-links"></a>Verwandte Links

* [Mithilfe von Push übertragen Benachrichtigungsvorlagen](/azure/notification-hubs/notification-hubs-templates-cross-platform-push-messages).
* [Registrierung der Geräteverwaltung](/azure/notification-hubs/notification-hubs-push-notification-registration-management).
* [Weiterleitung und Tagausdrücke](/azure/notification-hubs/notification-hubs-tags-segment-push-message).
* [Xamarin.Android Azure Notification Hubs-Tutorial](/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm).
* [Xamarin.iOS Azure Notification Hubs-Tutorial](/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started).
