---
title: Lokale Benachrichtigungen von Xamarin.Forms
description: In diesem Artikel wird das Senden und Empfangen lokaler Benachrichtigungen in Xamarin.Forms erläutert.
ms.prod: xamarin
ms.assetid: 60460F57-63C6-4916-BBB5-A870F1DF53D7
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 10/10/2019
ms.openlocfilehash: ef2ef004378212fac593179d7aa38b3688fa82c3
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "72371538"
---
# <a name="local-notifications-in-xamarinforms"></a>Lokale Benachrichtigungen in Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/local-notifications)

Lokale Benachrichtigungen sind Warnungen, die von Anwendungen gesendet werden, die auf einem mobilen Gerät installiert sind. Lokale Benachrichtigungen werden häufig für Funktionen wie die folgenden verwendet:

- Kalenderereignisse
- Erinnerungen
- Standortbasierte Auslöser

Jede Plattform behandelt die Erstellung, Anzeige und Verwendung lokaler Benachrichtigungen anders. In diesem Artikel wird erläutert, wie Sie eine plattformübergreifende Abstraktion erstellen, um lokale Benachrichtigungen mit Xamarin.Forms zu senden und zu empfangen.

[![Anwendung für lokale Benachrichtigungen unter iOS und Android](local-notifications-images/local-notifications-msg-cropped.png)](local-notifications-images/local-notifications-msg.png#lightbox)

## <a name="create-a-cross-platform-interface"></a>Erstellen einer plattformübergreifenden Schnittstelle

Die Xamarin.Forms-Anwendung sollte unabhängig von den zugrunde liegenden Plattformimplementierungen Benachrichtigungen erstellen und nutzen. Die folgende `INotificationManager`-Schnittstelle ist in der freigegebenen Codebibliothek implementiert und definiert eine plattformübergreifende API, die von der Anwendung verwendet werden kann, um mit Benachrichtigungen zu interagieren:

```csharp
public interface INotificationManager
{
    event EventHandler NotificationReceived;

    void Initialize();

    int ScheduleNotification(string title, string message);

    void ReceiveNotification(string title, string message);
}
```

Diese Schnittstelle wird in jedem Plattformprojekt implementiert. Das `NotificationReceived`-Ereignis ermöglicht der Anwendung, eingehende Benachrichtigungen zu verarbeiten. Die `Initialize`-Methode sollte eine beliebige native Plattformlogik ausführen, die zum Vorbereiten des Benachrichtigungssystems erforderlich ist. Die `ScheduleNotification`-Methode sollte eine Benachrichtigung senden. Die `ReceiveNotification`-Methode sollte von der zugrunde liegenden Plattform aufgerufen werden, wenn eine Nachricht empfangen wird.

## <a name="consume-the-interface-in-xamarinforms"></a>Verwenden der Schnittstelle in Xamarin.Forms

Nachdem eine Schnittstelle erstellt wurde, kann sie im freigegebenen Xamarin.Forms-Projekt verwendet werden, obwohl noch keine Plattformimplementierungen erstellt wurden. Die Beispielanwendung enthält eine `ContentPage` mit dem Namen **MainPage.xaml** mit folgendem Inhalt:

```xaml
<StackLayout Margin="0,35,0,0"
             x:Name="stackLayout">
    <Label Text="Click the button to create a local notification."
           TextColor="Red"
           HorizontalOptions="Center"
           VerticalOptions="Start" />
    <Button Text="Create Notification"
            HorizontalOptions="Center"
            VerticalOptions="Start"
            Clicked="OnScheduleClick"/>
</StackLayout>
```

Das Layout enthält ein `Label`-Element mit Anweisungen für den Benutzer und einen `Button`, der eine Benachrichtigung planen sollte, wenn er angetippt wird.

Der `MainPage`-Klasse- CodeBehind verarbeitet das Senden und Empfangen von Benachrichtigungen:

```csharp
public partial class MainPage : ContentPage
{
    INotificationManager notificationManager;
    int notificationNumber = 0;

    public MainPage()
    {
        InitializeComponent();

        notificationManager = DependencyService.Get<INotificationManager>();
        notificationManager.NotificationReceived += (sender, eventArgs) =>
        {
            var evtData = (NotificationEventArgs)eventArgs;
            ShowNotification(evtData.Title, evtData.Message);
        };
    }

    void OnScheduleClick(object sender, EventArgs e)
    {
        notificationNumber++;
        string title = $"Local Notification #{notificationNumber}";
        string message = $"You have now received {notificationNumber} notifications!";
        notificationManager.ScheduleNotification(title, message);
    }

    void ShowNotification(string title, string message)
    {
        Device.BeginInvokeOnMainThread(() =>
        {
            var msg = new Label()
            {
                Text = $"Notification Received:\nTitle: {title}\nMessage: {message}"
            };
            stackLayout.Children.Add(msg);
        });
    }
}
```

Der `MainPage`-Klassenkonstruktor verwendet den Xamarin.Forms-`DependencyService`, um eine plattformspezifische Instanz des `INotificationManager` abzurufen. Die `OnScheduleClicked`-Methode verwendet die `INotificationManager`-Instanz, um eine neue Benachrichtigung zu planen. Die `ShowNotification`-Methode wird vom Ereignishandler aufgerufen, der an das `NotificationReceived`-Ereignis angefügt ist, und fügt ein neues `Label` in die Seite ein, wenn das Ereignis aufgerufen wird.

Weitere Informationen zum Xamarin.Forms-`DependencyService` finden Sie unter [Xamarin.Forms Dependency.Service](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md).

## <a name="create-the-android-interface-implementation"></a>Erstellen der Android-Schnittstellenimplementierung

Damit die Xamarin.Forms-Anwendung Benachrichtigungen unter Android senden und empfangen kann, muss die Anwendung eine Implementierung der `INotificationManager`-Schnittstelle bereitstellen.

### <a name="create-the-androidnotificationmanager-class"></a>Erstellen der AndroidNotificationManager-Klasse

Die `AndroidNotificationManager`-Klasse implementiert die `INotificationManager`-Schnittstelle:

```csharp
using Android.Support.V4.App;
using Xamarin.Forms;
using AndroidApp = Android.App.Application;

[assembly: Dependency(typeof(LocalNotifications.Droid.AndroidNotificationManager))]
namespace LocalNotifications.Droid
{
    public class AndroidNotificationManager : INotificationManager
    {
        const string channelId = "default";
        const string channelName = "Default";
        const string channelDescription = "The default channel for notifications.";
        const int pendingIntentId = 0;

        public const string TitleKey = "title";
        public const string MessageKey = "message";

        bool channelInitialized = false;
        int messageId = -1;
        NotificationManager manager;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            CreateNotificationChannel();
        }

        public int ScheduleNotification(string title, string message)
        {
            if (!channelInitialized)
            {
                CreateNotificationChannel();
            }

            messageId++;

            Intent intent = new Intent(AndroidApp.Context, typeof(MainActivity));
            intent.PutExtra(TitleKey, title);
            intent.PutExtra(MessageKey, message);

            PendingIntent pendingIntent = PendingIntent.GetActivity(AndroidApp.Context, pendingIntentId, intent, PendingIntentFlags.OneShot);

            NotificationCompat.Builder builder = new NotificationCompat.Builder(AndroidApp.Context, channelId)
                .SetContentIntent(pendingIntent)
                .SetContentTitle(title)
                .SetContentText(message)
                .SetLargeIcon(BitmapFactory.DecodeResource(AndroidApp.Context.Resources, Resource.Drawable.xamagonBlue))
                .SetSmallIcon(Resource.Drawable.xamagonBlue)
                .SetDefaults((int)NotificationDefaults.Sound | (int)NotificationDefaults.Vibrate);

            var notification = builder.Build();
            manager.Notify(messageId, notification);

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message,
            };
            NotificationReceived?.Invoke(null, args);
        }

        void CreateNotificationChannel()
        {
            manager = (NotificationManager)AndroidApp.Context.GetSystemService(AndroidApp.NotificationService);

            if (Build.VERSION.SdkInt >= BuildVersionCodes.O)
            {
                var channelNameJava = new Java.Lang.String(channelName);
                var channel = new NotificationChannel(channelId, channelNameJava, NotificationImportance.Default)
                {
                    Description = channelDescription
                };
                manager.CreateNotificationChannel(channel);
            }

            channelInitialized = true;
        }
    }
}
```

Das `assembly`-Attribut oberhalb des Namespace registriert die `INotificationManager`-Schnittstellenimplementierung beim `DependencyService`.

Android ermöglicht Anwendungen, mehrere Kanäle für Benachrichtigungen zu definieren. Mit der `Initialize`-Methode wird ein einfacher Kanal erstellt, den die Beispielanwendung zum Senden von Benachrichtigungen verwendet. Die `ScheduleNotification`-Methode definiert die plattformspezifische Logik, die erforderlich ist, um eine Benachrichtigung zu erstellen und zu senden. Zum Schluss wird die `ReceiveNotification`-Methode vom Android-Betriebssystem aufgerufen, wenn eine Nachricht empfangen wird, und der Ereignishandler wird aufgerufen.

> [!NOTE]
> Die `Application`-Klasse wird sowohl im `Xamarin.Forms`- als auch im `Android.App`-Namespace definiert, sodass der `AndroidApp`-Alias in den `using`-Anweisungen definiert ist, um die beiden zu unterscheiden.

### <a name="handle-incoming-notifications-on-android"></a>Verarbeiten eingehender Benachrichtigungen unter Android

Die `MainActivity`-Klasse muss eingehende Benachrichtigungen erkennen und die `AndroidNotificationManager`-Instanz benachrichtigen. Das `Activity`-Attribut der `MainActivity`-Klasse sollte einen `LaunchMode`-Wert von `LaunchMode.SingleTop` angeben:

```csharp
[Activity(
        //...
        LaunchMode = LaunchMode.SingleTop]
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsAppCompatActivity
    {
        // ...
    }
```

Der `SingleTop`-Modus verhindert, dass mehrere Instanzen einer `Activity` gestartet werden, während sich die Anwendung im Vordergrund befindet. Dieser `LaunchMode` eignet sich möglicherweise nicht für Anwendungen, die in komplexeren Benachrichtigungsszenarios mehrere Aktivitäten starten. Weitere Informationen zu `LaunchMode`-Enumerationswerten finden Sie unter [Android Activity LaunchMode](https://developer.android.com/guide/topics/manifest/activity-element#lmode) (Startmodus von Android-Aktivitäten).

In der `MainActivity`-Klasse wird Folgendes geändert, um eingehende Benachrichtigungen zu empfangen:

```csharp
protected override void OnCreate(Bundle savedInstanceState)
{
    // ...

    global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
    LoadApplication(new App());
    CreateNotificationFromIntent(Intent);
}

protected override void OnNewIntent(Intent intent)
{
    CreateNotificationFromIntent(intent);
}

void CreateNotificationFromIntent(Intent intent)
{
    if (intent?.Extras != null)
    {
        string title = intent.Extras.GetString(AndroidNotificationManager.TitleKey);
        string message = intent.Extras.GetString(AndroidNotificationManager.MessageKey);
        DependencyService.Get<INotificationManager>().ReceiveNotification(title, message);
    }
}
```

Die `CreateNotificationFromIntent`-Methode extrahiert Benachrichtigungsdaten aus dem `intent`-Argument und stellt sie mithilfe der `ReceiveNotification`-Methode für den `AndroidNotificationManager` bereit. Die `CreateNotificationFromIntent`-Methode wird sowohl von der `OnCreate`-Methode als auch der `OnNewIntent`-Methode aufgerufen:

- Wenn die Anwendung von Benachrichtigungsdaten gestartet wird, werden die `Intent`-Daten an die `OnCreate`-Methode übergeben.
- Wenn die Anwendung sich bereits im Vordergrund befindet, werden die `Intent`-Daten an die `OnNewIntent`-Methode übergeben.

Android bietet viele erweiterte Optionen für Benachrichtigungen. Weitere Informationen finden Sie unter [Benachrichtigungen in Xamarin.Android](~/android/app-fundamentals/notifications/index.md).

## <a name="create-the-ios-interface-implementation"></a>Erstellen der iOS-Schnittstellenimplementierung

Damit die Xamarin.Forms-Anwendung Benachrichtigungen unter iOS senden und empfangen kann, muss die Anwendung eine Implementierung des `INotificationManager` bereitstellen.

### <a name="create-the-iosnotificationmanager-class"></a>Erstellen der iOSNotificationManager-Klasse

Die `iOSNotificationManager`-Klasse implementiert die `INotificationManager`-Schnittstelle:

```csharp
[assembly: Dependency(typeof(LocalNotifications.iOS.iOSNotificationManager))]
namespace LocalNotifications.iOS
{
    public class iOSNotificationManager : INotificationManager
    {
        int messageId = -1;

        bool hasNotificationsPermission;

        public event EventHandler NotificationReceived;

        public void Initialize()
        {
            // request the permission to use local notifications
            UNUserNotificationCenter.Current.RequestAuthorization(UNAuthorizationOptions.Alert, (approved, err) =>
            {
                hasNotificationsPermission = approved;
            });
        }

        public int ScheduleNotification(string title, string message)
        {
            // EARLY OUT: app doesn't have permissions
            if(!hasNotificationsPermission)
            {
                return -1;
            }

            messageId++;

            var content = new UNMutableNotificationContent()
            {
                Title = title,
                Subtitle = "",
                Body = message,
                Badge = 1
            };

            // Local notifications can be time or location based
            // Create a time-based trigger, interval is in seconds and must be greater than 0
            var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger(0.25, false);

            var request = UNNotificationRequest.FromIdentifier(messageId.ToString(), content, trigger);
            UNUserNotificationCenter.Current.AddNotificationRequest(request, (err) =>
            {
                if (err != null)
                {
                    throw new Exception($"Failed to schedule notification: {err}");
                }
            });

            return messageId;
        }

        public void ReceiveNotification(string title, string message)
        {
            var args = new NotificationEventArgs()
            {
                Title = title,
                Message = message
            };
            NotificationReceived?.Invoke(null, args);
        }
    }
}
```

Das `assembly`-Attribut oberhalb des Namespace registriert die `INotificationManager`-Schnittstellenimplementierung beim `DependencyService`.

Unter iOS müssen Sie die Berechtigung zum Verwenden von Benachrichtigungen anfordern, bevor Sie versuchen, eine Benachrichtigung zu planen. Die `Initialize`-Methode fordert die Autorisierung zur Verwendung lokaler Benachrichtigungen an. Die `ScheduleNotification`-Methode definiert die erforderliche Logik, um eine Benachrichtigung zu erstellen und zu senden. Zum Schluss wird die `ReceiveNotification`-Methode von iOS aufgerufen, wenn eine Nachricht empfangen wird, und der Ereignishandler wird aufgerufen.

### <a name="handle-incoming-notifications-on-ios"></a>Verarbeiten eingehender Benachrichtigungen unter iOS

Unter iOS müssen Sie einen Delegaten erstellen, der die Unterklasse `UNUserNotificationCenterDelegate` bildet, um eingehende Nachrichten zu verarbeiten. Die Beispielanwendung definiert eine `iOSNotificationReceiver`-Klasse:

```csharp
public class iOSNotificationReceiver : UNUserNotificationCenterDelegate
{
    public override void WillPresentNotification(UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
    {
        DependencyService.Get<INotificationManager>().ReceiveNotification(notification.Request.Content.Title, notification.Request.Content.Body);

        // alerts are always shown for demonstration but this can be set to "None"
        // to avoid showing alerts if the app is in the foreground
        completionHandler(UNNotificationPresentationOptions.Alert);
    }
}
```

Diese Klasse verwendet den `DependencyService`, um eine Instanz der `iOSNotificationManager`-Klasse zu erhalten, und stellt eingehende Benachrichtigungsdaten für die `ReceiveNotification`-Methode bereit.

Die `AppDelegate`-Klasse muss den benutzerdefinierten Delegaten beim Starten der Anwendung angeben. Die `AppDelegate`-Klasse muss ein `iOSNotificationReceiver`-Objekt als `UNUserNotificationCenter`-Delegaten beim Starten der Anwendung angeben. Dies tritt in der `FinishedLaunching`-Methode auf:

```csharp
public override bool FinishedLaunching(UIApplication app, NSDictionary options)
{
    global::Xamarin.Forms.Forms.Init();

    UNUserNotificationCenter.Current.Delegate = new iOSNotificationReceiver();

    LoadApplication(new App());
    return base.FinishedLaunching(app, options);
}
```

iOS bietet viele erweiterte Optionen für Benachrichtigungen. Weitere Informationen finden Sie unter [Benachrichtigungen in Xamarin.iOS](~/ios/platform/user-notifications/index.md).

## <a name="test-the-application"></a>Testen der Anwendung

Sobald die Plattformprojekte eine registrierte Implementierung der `INotificationManager`-Schnittstelle enthalten, kann die Anwendung auf beiden Plattformen getestet werden. Führen Sie die Anwendung aus, und klicken Sie auf die Schaltfläche **Planungshinweis**, um eine Benachrichtigung zu erstellen.

Unter Android wird die Benachrichtigung im Benachrichtigungsbereich angezeigt. Wenn auf die Benachrichtigung getippt wird, empfängt die Anwendung die Benachrichtigung und zeigt eine Meldung unterhalb der Schaltfläche **Planungshinweis** an:

![Lokale Benachrichtigungen unter Android](local-notifications-images/local-notifications-android.png)

Unter iOS werden eingehende Benachrichtigungen automatisch von der Anwendung empfangen, ohne dass eine Benutzereingabe erforderlich ist. Die Anwendung empfängt die Benachrichtigung und zeigt eine Meldung unterhalb der Schaltfläche **Planungshinweis** an:

![Lokale Benachrichtigungen unter iOS](local-notifications-images/local-notifications-ios.png)

## <a name="related-links"></a>Verwandte Links

- [Beispielprojekt](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/local-notifications)
- [Benachrichtigungen in Xamarin.Android](~/android/app-fundamentals/notifications/index.md)
- [Benachrichtigungen in Xamarin.iOS](~/ios/platform/user-notifications/index.md)
- [Xamarin.Forms Dependency.Service](~/xamarin-forms/app-fundamentals/dependency-service/introduction.md)
