---
title: Senden von Pushbenachrichtigungen von Azure Mobile Apps
description: In diesem Artikel wird erläutert, wie Sie mit Azure Notification Hubs zum Senden von Pushbenachrichtigungen aus einer Azure Mobile Apps-Instanz zu einer Xamarin.Forms-Anwendung.
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: 92c068ceb3d382ed4612318dc987d950ec7e7ef2
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61327473"
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Senden von Pushbenachrichtigungen von Azure Mobile Apps

[![Beispiel herunterladen](~/media/shared/download.png) Herunterladen des Beispiels](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)

_Azure Notification Hubs bietet eine skalierbare pushinfrastruktur für mobile Pushbenachrichtigungen von jedem Back-End an beliebige mobile Plattformen, sodass Sie die Komplexität eines Back-Ends müssen für die Kommunikation mit verschiedenen plattformbenachrichtigungssysteme senden. In diesem Artikel wird erläutert, wie Sie mit Azure Notification Hubs zum Senden von Pushbenachrichtigungen aus einer Azure Mobile Apps-Instanz zu einer Xamarin.Forms-Anwendung._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure Notification Hubs und Push Xamarin.Forms durch [Xamarin University](https://university.xamarin.com/)**

Eine Pushbenachrichtigung wird zum Übermitteln von Informationen, z. B. eine Nachricht von einem Back-End-System zu einer Anwendung auf einem mobilen Gerät auf die Anwendung Engagement und Nutzung zu erhöhen. Die Benachrichtigung kann jederzeit und an gesendet werden, selbst wenn der Benutzer die entsprechende Anwendung nicht aktiv verwendet wird.

Back-End-Systeme Senden von Pushbenachrichtigungen an mobile Geräte über Plattform Notification Systems (PNS), wie im folgenden Diagramm dargestellt:

[![](azure-images/pns.png "Plattform-Benachrichtigungssysteme")](azure-images/pns-large.png#lightbox "Plattform Notification Systems")

Um eine Pushbenachrichtigung zu senden, kontaktiert das Back-End-System den plattformspezifischen PNS zum Senden einer benachrichtigungs an eine Instanz des Client-Anwendung. Dadurch erhöht sich deutlich die Komplexität der Back-End bei Cross-Platform-Push-Benachrichtigungen erforderlich sind, da das Back-End jedes plattformspezifischen PNS-API und Protokoll verwendet werden muss.

Azure Notification Hubs Eliminieren dieser Komplexität, indem Sie dabei die Details der verschiedenen plattformbenachrichtigungssysteme können eine plattformübergreifende-Benachrichtigung mit einem einzigen API-Aufruf gesendet werden, wie im folgenden Diagramm dargestellt:

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

Um eine Pushbenachrichtigung zu senden, codieren die Back-End-System nur Kontakte Azure Notification Hub, der wiederum mit der verschiedenen plattformbenachrichtigungssysteme kommuniziert, daher verringern die Komplexität der Back-End, erste Schritte mit Pushbenachrichtigungen sendet.

Azure Mobile Apps haben integrierten Unterstützung für Pushbenachrichtigungen mit Notification Hubs. Der Vorgang zum Senden einer Pushbenachrichtigung aus einer Azure Mobile Apps-Instanz zu einer Xamarin.Forms-Anwendung lautet wie folgt aus:

1. Die Xamarin.Forms-Anwendung registriert, mit dem PNS auf, die ein Handle zurückgibt.
1. Die Azure Mobile Apps-Instanz sendet eine Benachrichtigung an die Azure Notification Hub,-, die den Handle des Geräts bereitgestellt werden soll.
1. Azure Notification Hub sendet die Benachrichtigung an den entsprechenden PNS für das Gerät an.
1. Das PNS sendet die Benachrichtigung an das angegebene Gerät.
1. Die Xamarin.Forms-Anwendung verarbeitet die Benachrichtigung und zeigt ihn an.

Die beispielanwendung zeigt eine Todolist-Anwendung, deren Daten in einer Azure Mobile Apps-Instanz gespeichert werden. Jedes Mal, wenn der Azure Mobile Apps-Instanz ein neues Element hinzugefügt wird, wird eine Pushbenachrichtigung an die Xamarin.Forms-Anwendung gesendet. Die folgenden Screenshots zeigen, jede Plattform, die die erhaltene Pushbenachrichtigung angezeigt wird:

[![](azure-images/screenshots.png "Beispielanwendung, die eine Pushbenachrichtigung empfangen")](azure-images/screenshots-large.png#lightbox "Beispielanwendung, die eine Pushbenachrichtigung empfangen")

Weitere Informationen zu Azure Notification Hubs, finden Sie unter [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) und [Hinzufügen von Pushbenachrichtigungen zur Xamarin.Forms-app](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure und Plattform Notification System-Setup

Der Prozess für die Integration von Azure Notification Hub in eine Azure Mobile Apps-Instanz lautet wie folgt aus:

1. Erstellen Sie eine Azure Mobile Apps-Instanz. Weitere Informationen finden Sie unter [Nutzung von Azure Mobile Apps](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Konfigurieren eines Notification Hubs. Weitere Informationen finden Sie unter [Konfigurieren eines Notification Hubs](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-hub).
1. Aktualisieren Sie die Azure Mobile Apps-Instanz, um Pushbenachrichtigungen zu senden. Weitere Informationen finden Sie unter [Aktualisieren des Serverprojekts zum Senden von Pushbenachrichtigungen](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Registrieren Sie bei einzelnen Pushbenachrichtigungsdienste.
1. Konfigurieren Sie den Notification Hub für die Kommunikation mit einzelnen Pushbenachrichtigungsdienste.

Die folgenden Abschnitte enthalten weitere installationsanweisungen für jede Plattform.

### <a name="ios"></a>iOS

Die folgenden zusätzlichen Schritte müssen durchgeführt werden, mit der Apple Push Notification Service (APNS) aus einer Azure Notification Hub:

1. Generieren Sie eine zertifikatsignieranforderung für das pushzertifikat mit dem Tool "Schlüsselbundverwaltung". Weitere Informationen finden Sie unter [Generieren der zertifikatsignieranforderungsdatei für das pushzertifikat](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) auf das Azure-Dokumentationscenter.
1. Registrieren Sie die Xamarin.Forms-Anwendung für die Unterstützung von Pushbenachrichtigungen im Apple Developer Center. Weitere Informationen finden Sie unter [Registrieren Ihrer app für Pushbenachrichtigungen](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) auf das Azure-Dokumentationscenter.
1. Erstellen Sie ein Bereitstellungsprofil Push, Benachrichtigungen aktiviert, für die Xamarin.Forms-Anwendung im Apple Developer Center. Weitere Informationen finden Sie unter [Erstellen eines bereitstellungsprofils für die app](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) auf das Azure-Dokumentationscenter.
1. Konfigurieren Sie den Notification Hub für die Kommunikation mit APNS. Weitere Informationen finden Sie unter [Konfigurieren des Notification Hubs für APNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Konfigurieren Sie die Xamarin.Forms-Anwendung, die neue App-ID und das Bereitstellungsprofil verwenden. Weitere Informationen finden Sie unter [Konfigurieren des iOS-Projekts in Xamarin Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) oder [konfigurieren das iOS-Projekt in Visual Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) auf das Azure-Dokumentationscenter.

### <a name="android"></a>Android

Die folgenden zusätzlichen Schritte müssen zur Verwendung von Firebase Cloud Messaging (FCM) aus einer Azure Notification Hub durchgeführt werden:

1. Registrieren Sie sich für FCM. Ein Server-API-Schlüssel und eine Client-ID werden automatisch erstellt und verpackt einem `google-services.json` -Datei, die heruntergeladen wird. Weitere Informationen finden Sie unter [aktivieren Firebase Cloud Messaging (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Konfigurieren Sie den Notification Hub für die Kommunikation mit FCM. Weitere Informationen finden Sie unter [konfigurieren, die das Mobile Apps-back-end zum Senden von pushanforderungen per FCM mit](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Die folgenden zusätzlichen Schritte müssen durchgeführt werden, um den Windows Notification Service (WNS) aus einer Azure Notification Hub verwenden:

1. Registrieren Sie sich für den Windows-Benachrichtigungsdienst (WNS). Weitere Informationen finden Sie unter [registrieren Sie Ihre Windows-app für Pushbenachrichtigungen mit WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) auf das Azure-Dokumentationscenter.
1. Konfigurieren Sie den Notification Hub für die Kommunikation mit WNS. Weitere Informationen finden Sie unter [Konfigurieren des Notification Hubs für WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) auf das Azure-Dokumentationscenter.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Unterstützung von Pushbenachrichtigungen zur Xamarin.Forms-Anwendung hinzufügen

Den folgenden Abschnitten werden die Implementierung, die zur Unterstützung von Pushbenachrichtigungen in den einzelnen plattformspezifischen Projekten erforderlich.

### <a name="ios"></a>iOS

Der Prozess zum Implementieren der Unterstützung von Pushbenachrichtigungen in iOS-Anwendungen lautet wie folgt aus:

1. Registrieren Sie mit der Apple Push Notification Service (APNS) in der `AppDelegate.FinishedLaunching` Methode. Weitere Informationen finden Sie unter [registrieren mit dem Apple Push Notification System](#ios_register).
1. Implementieren der `AppDelegate.RegisteredForRemoteNotifications` Methode, um die Antwort zu verarbeiten. Weitere Informationen finden Sie unter [Behandlung der Registrierungsantwort](#ios_registration_response).
1. Implementieren der `AppDelegate.DidReceiveRemoteNotification` Methode zum Verarbeiten von eingehenden Pushbenachrichtigungen. Weitere Informationen finden Sie unter [verarbeitet eingehende Pushbenachrichtigungen](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Mit dem Apple Push Notification Service registrieren

Bevor eine iOS-App Pushbenachrichtigungen empfangen kann, müssen sie mit der Apple Push Notification Service (APNS), registrieren, die eine eindeutige Geräte-Token generieren und an die Anwendung zurückgeben. Registrierung wird aufgerufen, der `FinishedLaunching` außer Kraft setzen, der `AppDelegate` Klasse:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    ...
    var settings = UIUserNotificationSettings.GetSettingsForTypes(
        UIUserNotificationType.Alert, new NSSet());

    UIApplication.SharedApplication.RegisterUserNotificationSettings(settings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications();
    ...
}
```

Bei der Registrierung von iOS-Anwendungen mit APNS müssen sie die Typen der Push-Benachrichtigungen angeben, die sie empfangen möchten. Die `RegisterUserNotificationSettings` Methode registriert die Arten von Benachrichtigungen, die die Anwendung erhalten, mit der `RegisterForRemoteNotifications` Methode, die zum Empfangen von Pushbenachrichtigungen von APNS registriert.

> [!NOTE]
> Fehler beim Aufrufen der `RegisterUserNotificationSettings` Methode führt dazu, in Pushbenachrichtigungen im Hintergrund von der Anwendung empfangen werden.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Verarbeiten der Antwort auf die Registrierung

Die Anforderung der APNS-Registrierung erfolgt im Hintergrund. IOS wird aufgerufen, wenn die Antwort empfangen wird, die `RegisteredForRemoteNotifications` außer Kraft setzen, der `AppDelegate` Klasse:

```csharp
public override void RegisteredForRemoteNotifications(UIApplication application, NSData deviceToken)
{
    const string templateBodyAPNS = "{\"aps\":{\"alert\":\"$(messageParam)\"}}";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyAPNS}
    };

    // Register for push with the Azure mobile app
    Push push = TodoItemManager.DefaultManager.CurrentClient.GetPush();
    push.RegisterAsync(deviceToken, templates);
}
```

Diese Methode erstellt eine einfache benachrichtigungsmeldungsvorlage als JSON-Code und des Geräts und zum Empfangen von vorlagenbenachrichtigungen vom Notification Hub registriert.

> [!NOTE]
> Die `FailedToRegisterForRemoteNotifications` außer Kraft setzen muss implementiert werden, um die Behandlung von Situationen, z. B. keine Netzwerkverbindung besteht. Dies ist wichtig, da der Benutzer die Anwendung beim Starten möglicherweise offline.

<a name="ios_process_incoming" />

### <a name="processing-incoming-push-notifications"></a>Verarbeiten von eingehenden Pushbenachrichtigungen

Die `DidReceiveRemoteNotification` außer Kraft setzen, der `AppDelegate` Klasse wird verwendet, um eingehende Pushbenachrichtigungen zu verarbeiten, wenn die Anwendung ausgeführt wird, und wird aufgerufen, wenn eine Benachrichtigung empfangen wird:

```csharp
public override void DidReceiveRemoteNotification(
    UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
    NSDictionary aps = userInfo.ObjectForKey(new NSString("aps")) as NSDictionary;

    string alert = string.Empty;
    if (aps.ContainsKey(new NSString("alert")))
        alert = (aps[new NSString("alert")] as NSString).ToString();

    // Show alert
    if (!string.IsNullOrEmpty(alert))
    {
      var notificationAlert = UIAlertController.Create("Notification", alert, UIAlertControllerStyle.Alert);
      notificationAlert.AddAction(UIAlertAction.Create("OK", UIAlertActionStyle.Cancel, null));
      UIApplication.SharedApplication.KeyWindow.RootViewController.PresentViewController(notificationAlert, true, null);
    }
}
```

Die `userInfo` Wörterbuch enthält die `aps` Schlüssel, dessen Wert die `alert` Wörterbuch mit den übrigen Benachrichtigungsdaten. Dieses Wörterbuch wird abgerufen, mit der `string` benachrichtigungsmeldung in einem Dialogfeld angezeigt wird.

> [!NOTE]
> Wenn eine Anwendung nicht ausgeführt wird, wenn eine Pushbenachrichtigung eingeht, wird die Anwendung gestartet werden, aber die `DidReceiveRemoteNotification` Methode die Benachrichtigung wird nicht verarbeiten kann. Rufen Sie stattdessen die Nutzlast der Benachrichtigung und entsprechend reagieren, aus der `WillFinishLaunching` oder `FinishedLaunching` überschreibt.

Weitere Informationen zu APNS, finden Sie unter [Pushbenachrichtigungen in iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Der Prozess zum Implementieren der Unterstützung von Pushbenachrichtigungen in einer Android-Anwendung lautet wie folgt aus:

1. Hinzufügen der [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet-Paket zum Android-Projekt, und legen Sie die Zielversion in der Anwendung auf Android 7.0 oder höher.
1. Hinzufügen der `google-services.json` Datei, aus der Firebase-Konsole heruntergeladen wird, in das Stammverzeichnis des Android-Projekt, und legen Sie seine Buildaktion auf **GoogleServicesJson**. Weitere Informationen finden Sie unter [Hinzufügen der Google Services JSON-Datei](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Registrieren mit Firebase Cloud Messaging (FCM) durch das Deklarieren eines Empfängers im Android-Manifest-Datei und die Implementierung von der `FirebaseRegistrationService.OnTokenRefresh` Methode. Weitere Informationen finden Sie unter [registrieren bei Firebase Cloud Messaging](#android_register_fcm).
1. Registrieren bei Azure Notification Hub in der `AzureNotificationHubService.RegisterAsync` Methode. Weitere Informationen finden Sie unter [registrieren beim Azure Notification Hub](#android_register_azure).
1. Implementieren der `FirebaseNotificationService.OnMessageReceived` Methode zum Verarbeiten von eingehenden Pushbenachrichtigungen. Weitere Informationen finden Sie unter [zeigt den Inhalt der Pushbenachrichtigung](#android_displaying_notification).

Weitere Informationen zu Firebase Cloud Messaging, finden Sie unter [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) und [Remotebenachrichtigungen mit Firebase Cloud Messaging](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Registrieren bei Firebase Cloud Messaging

Bevor Sie eine Android-Anwendung Pushbenachrichtigungen empfangen kann, müssen sie bei FCM, registrieren, Registrierungstoken generieren und an die Anwendung zurückgeben. Weitere Informationen zu Registrierungstoken, finden Sie unter [Registrierung bei FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

Dies erfolgt durch:

- Deklarieren Sie einen Empfänger im Android-Manifest. Weitere Informationen finden Sie unter [Deklarieren des Empfängers im Android-Manifest](#declaring_a_receiver).
- Implementieren der Diensts Firebase Instance ID an. Weitere Informationen finden Sie unter [implementieren die Diensts Firebase Instance ID](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Den Empfänger im Android-Manifest deklariert

Bearbeiten Sie **"androidmanifest.xml"** und fügen Sie die folgenden `<receiver>` Elemente in der `<application>` Element:

```xml
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdInternalReceiver" android:exported="false" />
<receiver android:name="com.google.firebase.iid.FirebaseInstanceIdReceiver" android:exported="true" android:permission="com.google.android.c2dm.permission.SEND">
  <intent-filter>
    <action android:name="com.google.android.c2dm.intent.RECEIVE" />
    <action android:name="com.google.android.c2dm.intent.REGISTRATION" />
    <category android:name="${applicationId}" />
  </intent-filter>
</receiver>
```

Dieses XML werden die folgenden Vorgänge ausgeführt:

- Deklariert eine interne `FirebaseInstanceIdInternalReceiver` -Implementierung, die verwendet wird, um Dienste sicher zu starten.
- Deklariert eine `FirebaseInstanceIdReceiver` -Implementierung, die einen eindeutigen Bezeichner für jede app-Instanz bereitstellt. Dieser Empfänger ist außerdem authentifiziert und autorisiert Aktionen.

Die `FirebaseInstanceIdReceiver` empfängt `FirebaseInstanceId` und `FirebaseMessaging` Ereignisse und übermittelt diese an die abgeleitete Klasse, von `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Implementieren der Diensts Firebase Instance ID

Registrieren der Anwendung bei FCM erfolgt durch Ableiten einer Klasse von der `FirebaseInstanceIdService` Klasse. Diese Klasse ist zuständig für das Generieren von Sicherheitstoken, die die Clientanwendung, auf FCM zuzugreifen zu autorisieren. In diesem Beispiel die `FirebaseRegistrationService` Klasse leitet sich von der `FirebaseInstanceIdService` Klasse und wird im folgenden Codebeispiel dargestellt:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.INSTANCE_ID_EVENT" })]
public class FirebaseRegistrationService : FirebaseInstanceIdService
{
    const string TAG = "FirebaseRegistrationService";

    public override void OnTokenRefresh()
    {
        var refreshedToken = FirebaseInstanceId.Instance.Token;
        Log.Debug(TAG, "Refreshed token: " + refreshedToken);
        SendRegistrationTokenToAzureNotificationHub(refreshedToken);
    }

    void SendRegistrationTokenToAzureNotificationHub(string token)
    {
        // Update notification hub registration
        Task.Run(async () =>
        {
            await AzureNotificationHubService.RegisterAsync(TodoItemManager.DefaultManager.CurrentClient.GetPush(), token);
        });
    }
}
```

Die `OnTokenRefresh` Methode wird aufgerufen, wenn die Anwendung ein Registrierungstoken von FCM empfängt. Die Methode ruft ab, das Token aus dem `FirebaseInstanceId.Instance.Token` -Eigenschaft, die von FCM asynchron aktualisiert wird. Die `OnTokenRefresh` Methode ist nur selten aufgerufen, da das Token nur dann aktualisiert wird, wenn die Anwendung installiert oder deinstalliert, wenn der Benutzer Anwendungsdaten löscht, wenn die Anwendung die Instanz-ID löscht oder die Sicherheit des Tokens wurde gefährdet. Darüber hinaus fordert der Dienst FCM Instance ID, dass die Anwendung das Token in regelmäßigen Abständen in der Regel alle sechs Monate aktualisiert.

Die `OnTokenRefresh` -Methode ruft auch die `SendRegistrationTokenToAzureNotificationHub` -Methode, die verwendet wird, den Azure Notification Hub-Registrierung-Token des Benutzers zugeordnet werden soll.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Registrieren beim Azure Notification Hub

Die `AzureNotificationHubService` -Klasse stellt die `RegisterAsync` -Methode, die Registrierungstoken des Benutzers mit Azure Notification Hub verknüpft. Das folgende Codebeispiel zeigt die `RegisterAsync` -Methode, die aufgerufen wird, durch die `FirebaseRegistrationService` Klasse bei der Registrierung-Token des Benutzers ändert:

```csharp
public class AzureNotificationHubService
{
    const string TAG = "AzureNotificationHubService";

    public static async Task RegisterAsync(Push push, string token)
    {
        try
        {
            const string templateBody = "{\"data\":{\"message\":\"$(messageParam)\"}}";
            JObject templates = new JObject();
            templates["genericMessage"] = new JObject
            {
                {"body", templateBody}
            };

            await push.RegisterAsync(token, templates);
            Log.Info("Push Installation Id: ", push.InstallationId.ToString());
        }
        catch (Exception ex)
        {
            Log.Error(TAG, "Could not register with Notification Hub: " + ex.Message);
        }
    }
}
```

Diese Methode erstellt eine einfache benachrichtigungsmeldungsvorlage als JSON, und registriert, zum Empfangen von vorlagenbenachrichtigungen vom Notification Hub, das Firebase-Registrierungstoken verwenden. Dadurch wird sichergestellt, dass alle von Azure Notification Hub gesendeten Benachrichtigungen das Gerät, das durch das Registrierungstoken dargestellt abzielen.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Anzeigen des Inhalts einer Pushbenachrichtigung

Anzeigen des Inhalts einer Pushbenachrichtigung erfolgt durch Ableiten einer Klasse von der `FirebaseMessagingService` Klasse. Diese Klasse enthält die überschreibbare `OnMessageReceived` -Methode, die aufgerufen wird, wenn die Anwendung eine Benachrichtigung von FCM empfängt, angegeben, dass die Anwendung im Vordergrund ausgeführt wird. In diesem Beispiel die `FirebaseNotificationService` Klasse leitet sich von der `FirebaseMessagingService` Klasse, und wird im folgenden Codebeispiel dargestellt:

```csharp
[Service]
[IntentFilter(new[] { "com.google.firebase.MESSAGING_EVENT" })]
public class FirebaseNotificationService : FirebaseMessagingService
{
    const string TAG = "FirebaseNotificationService";

    public override void OnMessageReceived(RemoteMessage message)
    {
        Log.Debug(TAG, "From: " + message.From);

        // Pull message body out of the template
        var messageBody = message.Data["message"];
        if (string.IsNullOrWhiteSpace(messageBody))
            return;

        Log.Debug(TAG, "Notification message body: " + messageBody);
        SendNotification(messageBody);
    }

    void SendNotification(string messageBody)
    {
        var intent = new Intent(this, typeof(MainActivity));
        intent.AddFlags(ActivityFlags.ClearTop);
        var pendingIntent = PendingIntent.GetActivity(this, 0, intent, PendingIntentFlags.OneShot);

        var notificationBuilder = new NotificationCompat.Builder(this)
            .SetSmallIcon(Resource.Drawable.ic_stat_ic_notification)
            .SetContentTitle("New Todo Item")
            .SetContentText(messageBody)
            .SetContentIntent(pendingIntent)
            .SetSound(RingtoneManager.GetDefaultUri(RingtoneType.Notification))
            .SetAutoCancel(true);

        var notificationManager = NotificationManager.FromContext(this);
        notificationManager.Notify(0, notificationBuilder.Build());
    }
}
```

Wenn die Anwendung eine Benachrichtigung von FCM empfängt die `OnMessageReceived` Methode extrahiert den Nachrichteninhalt und ruft die `SendNotification` Methode. Diese Methode konvertiert den Inhalt der Nachricht in eine lokale Benachrichtigung, die gestartet wird, während die Anwendung ausgeführt wird, mit der Benachrichtigung im Infobereich der Taskleiste angezeigt wird.

##### <a name="handling-notification-intents"></a>Verarbeiten von Benachrichtigungen Intents

Wenn ein Benutzer eine Benachrichtigung tippt, alle Daten, die zu die Nachricht zur Verfügung gestellt werden in der `Intent` Extras. Diese Daten können mit dem folgenden Code extrahiert werden:

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

Der Anwendung Startprogramm `Intent` wird ausgelöst, wenn der Benutzer die Benachrichtigung tippt, damit dieser Code alle zugehörigen Daten Sie sich melden werden die `Intent` im Ausgabefenster.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Bevor Sie eine universelle Windows-Plattform (UWP) kann Anwendung Pushbenachrichtigungen empfangen, die sie mit den Windows Notification Service (WNS) registrieren muss, durch den einen Benachrichtigungskanal zurückgegeben wird. Registrierung wird aufgerufen, indem die `InitNotificationsAsync` -Methode in der die `App` Klasse:

```csharp
private async Task InitNotificationsAsync()
{
    var channel = await PushNotificationChannelManager
          .CreatePushNotificationChannelForApplicationAsync();

    const string templateBodyWNS =
        "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";

    JObject headers = new JObject();
    headers["X-WNS-Type"] = "wns/toast";

    JObject templates = new JObject();
    templates["genericMessage"] = new JObject
    {
        {"body", templateBodyWNS},
        {"headers", headers} // Needed for WNS.
    };

    await TodoItemManager.DefaultManager.CurrentClient.GetPush()
          .RegisterAsync(channel.Uri, templates);
}
```

Diese Methode ruft den pushbenachrichtigungskanal ab, erstellt eine Benachrichtigungsvorlage im JSON-Format und des Geräts und zum Empfangen von vorlagenbenachrichtigungen vom Notification Hub registriert.

Die `InitNotificationsAsync` Methode wird aufgerufen, von der `OnLaunched` außer Kraft setzen, der `App` Klasse:

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Dadurch wird sichergestellt, dass die Registrierung der Pushbenachrichtigung erstellt oder aktualisiert jedes Mal, wenn die Anwendung gestartet wird, damit stellen Sie sicher, dass der WNS-pushkanal immer aktiv ist, ist.

Wenn eine Pushbenachrichtigung empfangen wird automatisch angezeigt wird als eine *Toast* – ein nicht modales Fenster mit der Meldung.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurde veranschaulicht, wie Azure Notification Hubs zum Senden von Pushbenachrichtigungen aus einer Azure Mobile Apps-Instanz zu einer Xamarin.Forms-Anwendung verwenden. Azure Notification Hubs bietet eine skalierbare pushinfrastruktur für mobile Pushbenachrichtigungen von jedem Back-End an beliebige mobile Plattformen, sodass Sie die Komplexität eines Back-Ends müssen für die Kommunikation mit verschiedenen plattformbenachrichtigungssysteme senden.


## <a name="related-links"></a>Verwandte Links

- [Verwenden einer Azure-Mobile-App](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Hinzufügen von Pushbenachrichtigungen zur Xamarin.Forms-app](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [Senden von Pushbenachrichtigungen in iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Azure Mobile Client-SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
