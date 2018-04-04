---
title: Senden von Pushbenachrichtigungen von Azure Mobile Apps
description: Azure Benachrichtigungshubs stellen eine skalierbare pushinfrastruktur für das Senden von Pushbenachrichtigungen mobile von beliebigen Back-Ends an beliebige mobile Plattformen die Komplexität von einem Back-End-müssen für die Kommunikation mit verschiedenen plattformbenachrichtigungssysteme Geschäftslogikkomponenten bei. In diesem Artikel wird erläutert, wie Azure-Benachrichtigungshubs zum Senden von Pushbenachrichtigungen von einer Instanz von Azure Mobile Apps zu einer Xamarin.Forms-Anwendung verwendet wird.
ms.prod: xamarin
ms.assetid: A1EF400F-73F4-43E9-A0C3-1569A0F34A3B
ms.technology: xamarin-forms
ms.custom: xamu-video
author: davidbritch
ms.author: dabritch
ms.date: 11/02/2017
ms.openlocfilehash: cff0b85514d2e5995d09735d6ad99b7909bfacb4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="sending-push-notifications-from-azure-mobile-apps"></a>Senden von Pushbenachrichtigungen von Azure Mobile Apps

_Azure Benachrichtigungshubs stellen eine skalierbare pushinfrastruktur für das Senden von Pushbenachrichtigungen mobile von beliebigen Back-Ends an beliebige mobile Plattformen die Komplexität von einem Back-End-müssen für die Kommunikation mit verschiedenen plattformbenachrichtigungssysteme Geschäftslogikkomponenten bei. In diesem Artikel wird erläutert, wie Azure-Benachrichtigungshubs zum Senden von Pushbenachrichtigungen von einer Instanz von Azure Mobile Apps zu einer Xamarin.Forms-Anwendung verwendet wird._

> [!VIDEO https://youtube.com/embed/le2lDY22xwM]

**Azure Push von Benachrichtigungs-Hub und Xamarin.Forms, [Xamarin University](https://university.xamarin.com/)**

Eine Pushbenachrichtigung wird verwendet, um Informationen, z. B. eine Nachricht von einem Back-End-System zu einer Anwendung auf einem mobilen Gerät auf die Anwendung Engagement und Nutzung erhöhen zu übermitteln. Die Benachrichtigung kann jederzeit, am gesendet werden, selbst wenn der Benutzer nicht die entsprechende Anwendung aktiv verwenden.

Back-End-Systeme senden Pushbenachrichtigungen an mobile Geräte über Platform Notification Systems (PNS), wie im folgenden Diagramm dargestellt:

[![](azure-images/pns.png "Plattformbenachrichtigungssysteme")](azure-images/pns-large.png#lightbox "Plattformbenachrichtigungssysteme")

Um eine Pushbenachrichtigung zu senden, kontaktiert das Back-End-System das plattformspezifische PNS zum Senden einer Benachrichtigung an einen Client-Anwendungsinstanz. Hierdurch werden erheblich die Komplexität der Back-End beim plattformübergreifende Pushbenachrichtigungen erforderlich sind, da der Back-End-jede Clientplattform-spezifische PNS-API und Protokoll benötigt.

Azure Notification Hubs vermeiden diese Komplexität, indem Sie dabei die Details der verschiedenen plattformbenachrichtigungssysteme ermöglicht eine plattformübergreifende Benachrichtigung mit einem einzigen API-Aufruf gesendet wird, wie im folgenden Diagramm dargestellt:

[![](azure-images/notification-hub.png)](azure-images/notification-hub-large.png#lightbox)

Zum Senden einer Pushbenachrichtigung Codieren der Back-End-System nur Kontakte Azure Notification Hub, der wiederum mit der verschiedenen plattformbenachrichtigungssysteme kommuniziert, verringern daher die Komplexität der Back-End, sendet Pushbenachrichtigungen an.

Azure Mobile Apps haben integrierten Unterstützung für die Verwendung von Notification Hubs Pushbenachrichtigungen. Der Vorgang zum Senden einer Pushbenachrichtigung aus einer Instanz von Azure Mobile Apps zu einer Xamarin.Forms-Anwendung lautet wie folgt:

1. Mit dem PNS, die ein Handle zurückgibt, wird die Xamarin.Forms-Anwendung registriert.
1. Die Instanz von Azure Mobile Apps sendet eine Benachrichtigung mit der Azure-Benachrichtigungs-Hub, die den Handle des Geräts zu verwendenden frameworkprofils an.
1. Der Azure-Benachrichtigungs-Hub sendet die Benachrichtigung an die entsprechenden PNS für das Gerät.
1. Das PNS sendet die Benachrichtigung an das angegebene Gerät.
1. Die Xamarin.Forms-Anwendung verarbeitet die Benachrichtigung und angezeigt.

Die beispielanwendung veranschaulicht eine Todo List-Anwendung, deren Daten in einer Azure-Mobile Apps-Instanz gespeichert ist. Jedes Mal, wenn der Azure-Mobile Apps-Instanz ein neues Element hinzugefügt wird, wird eine Pushbenachrichtigung an die Xamarin.Forms-Anwendung gesendet. Die folgenden Screenshots zeigen jede Plattform, die die empfangene Pushbenachrichtigung anzeigen:

[![](azure-images/screenshots.png "Beispiel-Anwendung empfängt eine Pushbenachrichtigung")](azure-images/screenshots-large.png#lightbox "Beispielanwendung, die eine Pushbenachrichtigung empfangen")

Weitere Informationen zu Azure Notification Hubs, finden Sie unter [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/) und [Hinzufügen von Pushbenachrichtigungen an Ihre app Xamarin.Forms](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push/).

## <a name="azure-and-platform-notification-system-setup"></a>Azure und Platform Notification System-Setup

Der Prozess zum Integrieren eines Azure-Benachrichtigungs-Hub in eine Instanz von Azure Mobile Apps ist wie folgt:

1. Erstellen Sie eine Azure Mobile Apps-Instanz. Weitere Informationen finden Sie unter [einer Azure-Mobile-App nutzen](~/xamarin-forms/data-cloud/consuming/azure.md).
1. Konfigurieren eines benachrichtigungs-Hubs. Weitere Informationen finden Sie unter [konfigurieren ein benachrichtigungs-Hubs](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#create-hub).
1. Aktualisieren Sie die Azure-Mobile Apps-Instanz, um Pushbenachrichtigungen zu senden. Weitere Informationen finden Sie unter [aktualisieren Sie das Serverprojekt, um das Senden von Pushbenachrichtigungen](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#update-the-server-project-to-send-push-notifications).
1. Bei jeder PNS registrieren.
1. Konfigurieren Sie benachrichtigungs-Hub für die Kommunikation mit jeder PNS.

Die folgenden Abschnitte enthalten weitere installationsanweisungen für jede Plattform.

### <a name="ios"></a>iOS

Die folgenden zusätzlichen Schritte müssen durchgeführt werden, mithilfe von Apple Push Notification Service (APNS) aus einem Azure-Benachrichtigungs-Hub:

1. Generieren Sie eine zertifikatsignieranforderung für das Push-Zertifikat mit dem Tool Schlüsselbundverwaltung. Weitere Informationen finden Sie unter [generieren Sie die Zertifikatsignieranforderung-Datei für das Push-Zertifikat](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#generate-the-certificate-signing-request-file-for-the-push-certificate) auf den Azure Documentation Center.
1. Registrieren Sie das Xamarin.Forms-Anwendung für Push Notification-Unterstützung auf der Apple Developer Center. Weitere Informationen finden Sie unter [Registrieren Ihrer app für Pushbenachrichtigungen](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-app-for-push-notifications) auf den Azure Documentation Center.
1. Erstellen Sie ein Bereitstellungsprofil Push, Benachrichtigungen aktiviert, für die Xamarin.Forms-Anwendung auf der Apple Developer Center. Weitere Informationen finden Sie unter [erstellen Sie ein Bereitstellungsprofil für die app](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#create-a-provisioning-profile-for-the-app) auf den Azure Documentation Center.
1. Konfigurieren der benachrichtigungshub zur Nutzung bei APNS zu kommunizieren. Weitere Informationen finden Sie unter [konfigurieren Sie den Notification Hub für APNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-apns).
1. Konfigurieren Sie die Xamarin.Forms-Anwendung auf die neue App-ID und ein Bereitstellungsprofil verwenden. Weitere Informationen finden Sie unter [Konfigurieren von iOS-Projekt in Xamarin Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-xamarin-studio) oder [konfigurieren die iOS-Projekt in Visual Studio](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configuring-the-ios-project-in-visual-studio) auf den Azure Documentation Center.

### <a name="android"></a>Android

Die folgenden zusätzlichen Schritte müssen durchgeführt werden, Firebase Cloud Messaging (FCM) aus einem Azure-Benachrichtigungs-Hub verwenden:

1. Für FCM registriert werden. Ein Server-API-Schlüssel und eine Client-ID werden automatisch generiert, und verpackt einer `google-services.json` -Datei, die heruntergeladen wird. Weitere Informationen finden Sie unter [aktivieren Firebase Cloud Messaging (FCM)](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#enable-firebase-cloud-messaging-fcm).
1. Konfigurieren Sie benachrichtigungs-Hub für die Kommunikation mit FCM. Weitere Informationen finden Sie unter [konfigurieren, die die Mobile Apps wieder zu beenden, um Anfragen FCM mit Push](/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started-push#configure-the-mobile-apps-back-end-to-send-push-requests-by-using-fcm).

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Die folgenden zusätzlichen Schritte müssen durchgeführt werden, verwenden Sie den Windows Notification Service (WNS) aus einem Azure-Benachrichtigungs-Hub:

1. Für den Windows Notification Service (WNS) registriert werden. Weitere Informationen finden Sie unter [Registrieren Ihrer Windows-app für Pushbenachrichtigungen bei WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#register-your-windows-app-for-push-notifications-with-wns) auf den Azure Documentation Center.
1. Konfigurieren Sie benachrichtigungs-Hub für die Kommunikation mit WNS. Weitere Informationen finden Sie unter [konfigurieren Sie den Notification Hub für WNS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/#configure-the-notification-hub-for-wns) auf den Azure Documentation Center.

## <a name="adding-push-notification-support-to-the-xamarinforms-application"></a>Hinzufügen von Push-Benachrichtigung-Support, um die Xamarin.Forms-Anwendung

In den folgenden Abschnitten erläutern die Implementierung, die in jedem Projekt plattformspezifischen erforderlich sind, um Pushbenachrichtigungen zu unterstützen.

### <a name="ios"></a>iOS

Der Vorgang zum Implementieren von Push Notification-Unterstützung in einer iOS-Anwendung lautet wie folgt:

1. Registrieren Sie mit den Apple Push Notification Service (APNS) in der `AppDelegate.FinishedLaunching` Methode. Weitere Informationen finden Sie unter [registrieren mit dem Apple Push Notification System](#ios_register).
1. Implementieren der `AppDelegate.RegisteredForRemoteNotifications` Methode, um die Antwort auf die Registrierung zu behandeln. Weitere Informationen finden Sie unter [Behandlung der Antwort auf die Registrierung von](#ios_registration_response).
1. Implementieren der `AppDelegate.DidReceiveRemoteNotification` Methode zum Verarbeiten von eingehenden Pushbenachrichtigungen. Weitere Informationen finden Sie unter [verarbeiten eingehende Pushbenachrichtigungen](#ios_process_incoming).

<a name="ios_register" />

### <a name="registering-with-the-apple-push-notification-service"></a>Mit dem Apple Push Notification Service registrieren

Bevor eine iOS-Anwendung Pushbenachrichtigungen empfangen kann, müssen sie mit dem Apple Push Notification Service (APNS) registrieren die eindeutige Geräte-Token generieren und an die Anwendung zurückgegeben wird. Registrierung wird aufgerufen, der `FinishedLaunching` außer Kraft setzen, der `AppDelegate` Klasse:

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

Bei der Registrierung von iOS-Anwendung mit APNS müssen sie die Typen von Pushbenachrichtigungen angeben, die sie erhalten sollen. Die `RegisterUserNotificationSettings` Methode registriert die Arten von Benachrichtigungen, die die Anwendung erhalten kann, mit der `RegisterForRemoteNotifications` Methode, die zum Empfangen von Pushbenachrichtigungen von APNS registrieren.

> [!NOTE]
> Wegen eines Fehlers beim Aufrufen der `RegisterUserNotificationSettings` Methode führt dazu, im Push-Benachrichtigungen, die automatisch von der Anwendung empfangen werden.

<a name="ios_registration_response" />

### <a name="handling-the-registration-response"></a>Behandlung von der Antwort auf die Registrierung

Die Anforderung des APNs-Registrierung erfolgt im Hintergrund. IOS wird aufgerufen, wenn die Antwort empfangen wird, die `RegisteredForRemoteNotifications` außer Kraft setzen, der `AppDelegate` Klasse:

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

Diese Methode erstellt eine einfache Benachrichtigungsvorlage des Meldung als JSON und das Gerät, um die Vorlage für Benachrichtigungen über den Notification Hub registriert.

> [!NOTE]
> Die `FailedToRegisterForRemoteNotifications` Außerkraftsetzung sollten implementiert werden, um Situationen, z. B. keine Netzwerkverbindung zu bewältigen. Dies ist wichtig, da der Benutzer die Anwendung beim start möglicherweise offline.

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

Die `userInfo` Wörterbuch enthält die `aps` Schlüssel, dessen Wert ist die `alert` Wörterbuch mit den übrigen Benachrichtigungsdaten. Dieses Wörterbuch wird abgerufen, mit der `string` benachrichtigungsmeldung in einem Dialogfeld angezeigt wird.

> [!NOTE]
> Wenn eine Anwendung nicht ausgeführt wird, wenn eine Pushbenachrichtigung eingeht, wird die Anwendung gestartet, aber die `DidReceiveRemoteNotification` -Methode die Benachrichtigung wird nicht verarbeitet werden. Rufen Sie stattdessen die benachrichtigungsnutzlast und entsprechend reagieren, aus der `WillFinishLaunching` oder `FinishedLaunching` überschreibt.

Weitere Informationen zu APNS, finden Sie unter [Pushbenachrichtigungen in iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md).

### <a name="android"></a>Android

Der Vorgang zum Implementieren von Push Notification-Unterstützung in einer Android-Anwendung lautet wie folgt:

1. Hinzufügen der [Xamarin.Firebase.Messaging](https://www.nuget.org/packages/Xamarin.Firebase.Messaging/) NuGet-Pakets mit der Android-Projekt, und legen Sie die Anwendung Zielversion auf Android 7.0 oder höher.
1. Hinzufügen der `google-services.json` Datei, über die Konsole Firebase heruntergeladen, in das Stammverzeichnis der Android-Projekts, und legen seine Buildaktion auf **GoogleServicesJson**. Weitere Informationen finden Sie unter [fügen Sie die JSON-Datei von Google Services](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).
1. Register mit Firebase Cloud Messaging (FCM) durch deklarieren einen Empfänger in der Android-Manifest-Datei, und durch Implementieren der `FirebaseRegistrationService.OnTokenRefresh` Methode. Weitere Informationen finden Sie unter [registrieren bei Firebase Cloud Messaging](#android_register_fcm).
1. Registrieren Sie mit der Azure-Benachrichtigungs-Hub in der `AzureNotificationHubService.RegisterAsync` Methode. Weitere Informationen finden Sie unter [registrieren bei Azure Notification Hub](#android_register_azure).
1. Implementieren der `FirebaseNotificationService.OnMessageReceived` Methode zum Verarbeiten von eingehenden Pushbenachrichtigungen. Weitere Informationen finden Sie unter [Anzeigen des Inhalts von einer Pushbenachrichtigung](#android_displaying_notification).

Weitere Informationen zu Firebase Cloud Messaging, finden Sie unter [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md) und [Remote Benachrichtigungen mit Firebase Cloud Messaging](~/android/data-cloud/google-messaging/remote-notifications-with-fcm.md).

<a name="android_register_fcm" />

#### <a name="registering-with-firebase-cloud-messaging"></a>Registrieren bei Firebase Cloud-Messaging

Bevor eine Android-Anwendung Pushbenachrichtigungen empfangen kann, müssen sie beim FCM, registrieren, die Registrierung-Token generieren und an die Anwendung zurückgegeben wird. Weitere Informationen zu Registrierungstoken, finden Sie unter [Registrierung mit FCM](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md#registration).

Dies erfolgt durch:

- Deklarieren Sie einen Empfänger in der Android-Manifest. Weitere Informationen finden Sie unter [Deklarieren des Empfängers in der Android-Manifest](#declaring_a_receiver).
- Implementieren der Firebase Instanz-ID-Dienst an. Weitere Informationen finden Sie unter [Firebase Instanz-ID-Dienst implementieren](#implementing-firebase-instance-id-service).

<a name="declaring_a_receiver" />

##### <a name="declaring-the-receiver-in-the-android-manifest"></a>Deklarieren Sie die Empfänger in der Android-Manifest

Bearbeiten Sie **AndroidManifest.xml** , und fügen Sie die folgenden `<receiver>` Elemente in der `<application>` Element:

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

Durch dieses XML werden die folgenden Vorgänge ausgeführt:

- Deklariert eine interne `FirebaseInstanceIdInternalReceiver` Implementierung, die beim Starten der Dienste sicher verwendet wird.
- Deklariert eine `FirebaseInstanceIdReceiver` Implementierung, die einen eindeutigen Bezeichner für jede Instanz für die app bereitstellt. Dieser Empfänger ist außerdem authentifiziert und autorisiert Aktionen.

Die `FirebaseInstanceIdReceiver` empfängt `FirebaseInstanceId` und `FirebaseMessaging` Ereignisse und übermittelt sie an die Klasse, die abgeleitet ist `FirebasesInstanceIdService`.

<a name="implementing-firebase-instance-id-service" />

##### <a name="implementing-the-firebase-instance-id-service"></a>Implementieren des Firebase Instanz-ID-Diensts

Registrieren die Anwendung mit FCM erfolgt durch Ableiten einer Klasse von der `FirebaseInstanceIdService` Klasse. Diese Klasse dient zum Generieren von Sicherheitstoken, die die Clientanwendung FCM Zugriff auf Autorisieren. In der beispielanwendung der `FirebaseRegistrationService` Klasse leitet sich von der `FirebaseInstanceIdService` -Klasse und wird im folgenden Codebeispiel gezeigt:

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

Die `OnTokenRefresh` Methode wird aufgerufen, wenn die Anwendung ein Token für die Registrierung von FCM empfängt. Die Methode ruft ab, das Token aus der `FirebaseInstanceId.Instance.Token` -Eigenschaft, die asynchron durch FCM aktualisiert wird. Die `OnTokenRefresh` Methode ist nur selten aufgerufen, da das Token nur aktualisiert werden, wenn die Anwendung installiert oder deinstalliert, wenn der Benutzer die Anwendungsdaten, löscht, wenn die Anwendung die Instanz-ID gelöscht wird oder wenn die Sicherheit des Tokens wurde gefährdet. Darüber hinaus fordert der FCM Instanz-ID-Dienst, dass die Anwendung die Token in regelmäßigen Abständen, in der Regel alle sechs Monate aktualisiert.

Die `OnTokenRefresh` Methode ruft außerdem die `SendRegistrationTokenToAzureNotificationHub` -Methode, die verwendet wird, der Azure-Benachrichtigungs-Hub-Registrierung das Benutzertoken zugeordnet werden soll.

<a name="android_register_azure" />

#### <a name="registering-with-the-azure-notification-hub"></a>Registrieren bei Azure Notification Hub

Die `AzureNotificationHubService` -Klasse stellt die `RegisterAsync` -Methode, die Azure Notification Hub-Registrierung das Benutzertoken zuordnet. Das folgende Codebeispiel zeigt die `RegisterAsync` -Methode, die aufgerufen wird, indem Sie die `FirebaseRegistrationService` Klasse bei der Registrierung das Benutzertoken ändert:

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

Diese Methode erstellt eine einfache Benachrichtigungsvorlage des Meldung als JSON und Register Vorlage für Benachrichtigungen von der benachrichtigungs-Hub, mit dem Firebase Registrierung Token empfangen. Dadurch wird sichergestellt, dass von der Azure-Benachrichtigungs-Hub gesendete Benachrichtigungen das Gerät durch die Registrierung-Token dargestellt wird Ziel werden.

<a name="android_displaying_notification" />

#### <a name="displaying-the-contents-of-a-push-notification"></a>Anzeigen des Inhalts von einer Pushbenachrichtigung fest.

Anzeigen des Inhalts von einer Pushbenachrichtigung erfolgt durch Ableiten einer Klasse von der `FirebaseMessagingService` Klasse. Diese Klasse enthält die überschreibbare `OnMessageReceived` -Methode, die aufgerufen wird, wenn die Anwendung eine Benachrichtigung aus FCM empfängt, angegeben, dass die Anwendung im Vordergrund ausgeführt wird. In der beispielanwendung der `FirebaseNotificationService` Klasse leitet sich von der `FirebaseMessagingService` Klasse, und wird im folgenden Codebeispiel gezeigt:

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

Wenn die Anwendung eine Benachrichtigung aus FCM, empfängt der `OnMessageReceived` Methode den Nachrichteninhalt extrahiert, und ruft die `SendNotification` Methode. Diese Methode konvertiert den Inhalt von Nachrichten in eine lokale Benachrichtigung, die gestartet wird, während die Anwendung ausgeführt wird, bei der Benachrichtigung im Infobereich angezeigt wird.

##### <a name="handling-notification-intents"></a>Behandlung von Benachrichtigung Intents

Wenn ein Benutzer eine Benachrichtigung tippt, alle Daten, die zugehörige Benachrichtigung zur Verfügung gestellt werden in der `Intent` Extras. Diese Daten können durch den folgenden Code extrahiert werden:

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

Die Anwendung Startprogramm `Intent` wird ausgelöst, wenn der Benutzer die Benachrichtigung tippt, damit dieser Code alle zugehörigen Daten, in protokolliert werden der `Intent` im Ausgabefenster.

### <a name="universal-windows-platform"></a>Universelle Windows-Plattform

Bevor Sie eine universelle Windows-Plattform (UWP) kann Anwendung Pushbenachrichtigungen empfangen, die sie mit den Windows Notification Service (WNS) registrieren muss einen Benachrichtigungskanal zurückgegeben wird. Registrierung wird aufgerufen, indem die `InitNotificationsAsync` Methode in der `App` Klasse:

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

Diese Methode ruft den Push-Benachrichtigungskanal, erstellt eine Benachrichtigungsvorlage für die Nachricht als JSON und registriert das Gerät, um die Vorlage für Benachrichtigungen von Notification Hub empfangen.

Die `InitNotificationsAsync` Methode wird aufgerufen, aus der `OnLaunched` außer Kraft setzen, der `App` Klasse:

```csharp
protected override async void OnLaunched(LaunchActivatedEventArgs e)
{
    ...
    await InitNotificationsAsync();
}
```

Dadurch wird sichergestellt, dass die Registrierung des pushbenachrichtigungsdiensts erstellt oder aktualisiert jedes Mal, wenn die Anwendung gestartet wird, daher sicherstellen, dass die WNS-Kanal Push immer aktiv ist.

Beim Empfang einer Pushbenachrichtigung wird automatisch angezeigt, wie eine *Toast* – ein nicht modales Fenster, die die Nachricht enthält.

## <a name="summary"></a>Zusammenfassung

Dieser Artikel veranschaulicht, wie Azure-Benachrichtigungshubs zum Senden von Pushbenachrichtigungen von einer Instanz von Azure Mobile Apps zu einer Xamarin.Forms-Anwendung verwenden. Azure Benachrichtigungshubs stellen eine skalierbare pushinfrastruktur für das Senden von Pushbenachrichtigungen mobile von beliebigen Back-Ends an beliebige mobile Plattformen die Komplexität von einem Back-End-müssen für die Kommunikation mit verschiedenen plattformbenachrichtigungssysteme Geschäftslogikkomponenten bei.


## <a name="related-links"></a>Verwandte Links

- [Verwenden einer mobilen Anwendung für Azure](~/xamarin-forms/data-cloud/consuming/azure.md)
- [Azure Notification Hubs](https://azure.microsoft.com/documentation/articles/notification-hubs-push-notification-overview/)
- [Hinzufügen von Pushbenachrichtigungen an Ihre app Xamarin.Forms](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-forms-get-started-push/)
- [Pushbenachrichtigungen in iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md)
- [Firebase Cloud Messaging](~/android/data-cloud/google-messaging/firebase-cloud-messaging.md)
- [TodoAzurePush (sample)](https://developer.xamarin.com/samples/xamarin-forms/WebServices/TodoAzurePush/)
- [Azure Mobile Client SDK](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
