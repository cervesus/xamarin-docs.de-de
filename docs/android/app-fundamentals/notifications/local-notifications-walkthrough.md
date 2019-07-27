---
title: 'Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in xamarin. Android'
description: In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie lokale Benachrichtigungen in xamarin. Android-Anwendungen verwendet werden. Es veranschaulicht die Grundlagen der Erstellung und Veröffentlichung einer lokalen Benachrichtigung. Wenn der Benutzer im Benachrichtigungsbereich auf die Benachrichtigung klickt, startet er eine zweite Aktivität.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/16/2018
ms.openlocfilehash: fb70ea126216642af513036211f7dd2a86fd9559
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/26/2019
ms.locfileid: "68509537"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in xamarin. Android

_In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie lokale Benachrichtigungen in xamarin. Android-Anwendungen verwendet werden. Es veranschaulicht die Grundlagen der Erstellung und Veröffentlichung einer lokalen Benachrichtigung. Wenn der Benutzer im Benachrichtigungsbereich auf die Benachrichtigung klickt, startet er eine zweite Aktivität._


## <a name="overview"></a>Übersicht

In dieser exemplarischen Vorgehensweise erstellen wir eine Android-Anwendung, die eine Benachrichtigung auslöst, wenn der Benutzer auf eine Schaltfläche in einer Aktivität klickt. Wenn der Benutzer auf die Benachrichtigung klickt, wird eine zweite Aktivität gestartet, die anzeigt, wie oft der Benutzer auf die Schaltfläche in der ersten Aktivität geklickt hat.

Die folgenden Screenshots veranschaulichen einige Beispiele für diese Anwendung:

[![Beispiel-Screenshots mit Benachrichtigung](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> Dieser Leitfaden konzentriert sich auf die [notificationcompat-APIs](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) aus der [Android-Unterstützungs Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Diese APIs stellen eine maximale Abwärtskompatibilität mit Android 4,0 (API-Ebene 14) sicher.

## <a name="creating-the-project"></a>Erstellen des Projekts

Erstellen Sie zunächst ein neues Android-Projekt mithilfe der Android- **App** -Vorlage. Wir nennen dieses Projekt **localnotification**. (Wenn Sie mit dem Erstellen von xamarin. Android-Projekten nicht vertraut sind, finden Sie weitere Informationen unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

Bearbeiten Sie die Ressourcen Datei **Values/Strings. XML** so, dass Sie zwei zusätzliche Zeichen folgen Ressourcen enthält, die beim Erstellen des Benachrichtigungs Kanals verwendet werden:


```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Hinzufügen des nuget-Pakets "Android. Support. v4"

In dieser exemplarischen Vorgehensweise verwenden `NotificationCompat.Builder` wir, um unsere lokale Benachrichtigung zu erstellen. Wie in [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md)erläutert, müssen wir die [Android-Unterstützungs Bibliothek V4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) nuget in unser Projekt einschließen, `NotificationCompat.Builder`um zu verwenden.

Als nächstes bearbeiten wir **MainActivity.cs** und fügen die folgende `using` Anweisung hinzu, damit die Typen in `Android.Support.V4.App` für den Code verfügbar sind:

```csharp
using Android.Support.V4.App;
```

Außerdem müssen wir dem Compiler klar machen, dass die `Android.Support.V4.App` Version von `TaskStackBuilder` anstelle der `Android.App` -Version verwendet wird. Fügen Sie die `using` folgende-Anweisung hinzu, um Mehrdeutigkeit zu beheben:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>Erstellen des Benachrichtigungs Kanals

Fügen Sie als nächstes eine Methode `MainActivity` hinzu, die einen Benachrichtigungs Kanal erstellt (falls erforderlich):

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

    var name = Resources.GetString(Resource.String.channel_name);
    var description = GetString(Resource.String.channel_description);
    var channel = new NotificationChannel(CHANNEL_ID, name, NotificationImportance.Default)
                  {
                      Description = description
                  };

    var notificationManager = (NotificationManager) GetSystemService(NotificationService);
    notificationManager.CreateNotificationChannel(channel);
}
```

Aktualisieren Sie `OnCreate` die-Methode, um diese neue Methode aufzurufen:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```


### <a name="define-the-notification-id"></a>Festlegen der Benachrichtigungs-ID

Wir benötigen eine eindeutige ID für unsere Benachrichtigungs-und Benachrichtigungs Kanäle. Bearbeiten Sie **MainActivity.cs** , `MainActivity` und fügen Sie der-Klasse die folgende statische Instanzvariable hinzu:

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>Hinzufügen von Code zum Generieren der Benachrichtigung

Als nächstes müssen wir einen neuen Ereignishandler für das Schalt `Click` Flächen Ereignis erstellen. Fügen Sie die folgende Methode `MainActivity`zu hinzu:

```csharp
void ButtonOnClick(object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    var valuesForActivity = new Bundle();
    valuesForActivity.PutInt(COUNT_KEY, count);

    // When the user clicks the notification, SecondActivity will start up.
    var resultIntent = new Intent(this, typeof(SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras(valuesForActivity);

    // Construct a back stack for cross-task navigation:
    var stackBuilder = TaskStackBuilder.Create(this);
    stackBuilder.AddParentStack(Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent(resultIntent);

    // Create the PendingIntent with the back stack:
    var resultPendingIntent = stackBuilder.GetPendingIntent(0, (int) PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    var builder = new NotificationCompat.Builder(this, CHANNEL_ID)
                  .SetAutoCancel(true) // Dismiss the notification from the notification area when the user clicks on it
                  .SetContentIntent(resultPendingIntent) // Start up this activity when the user clicks the intent.
                  .SetContentTitle("Button Clicked") // Set the title
                  .SetNumber(count) // Display the count in the Content Info
                  .SetSmallIcon(Resource.Drawable.ic_stat_button_click) // This is the icon to display
                  .SetContentText($"The button has been clicked {count} times."); // the message to display.

    // Finally, publish the notification:
    var notificationManager = NotificationManagerCompat.From(this);
    notificationManager.Notify(NOTIFICATION_ID, builder.Build());

    // Increment the button press count:
    count++;
}
```

Die `OnCreate` mainactivity-Methode muss den Aufruf zum Erstellen des Benachrichtigungs Kanals ausführen und die `ButtonOnClick` Methode dem- `Click` Ereignis der Schaltfläche zuweisen (ersetzen Sie den delegatereignishandler, der von der Vorlage bereitgestellt wird):

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();

    // Display the "Hello World, Click Me!" button and register its event handler:
    var button = FindViewById<Button>(Resource.Id.MyButton);
    button.Click += ButtonOnClick;
}
```


### <a name="create-a-second-activity"></a>Erstellen Sie eine zweite Aktivität.

Nun müssen wir eine weitere Aktivität erstellen, die von Android angezeigt wird, wenn der Benutzer auf die Benachrichtigung klickt. Fügen Sie dem Projekt eine weitere Android-Aktivität namens **secondactivity**hinzu. Öffnen Sie **SecondActivity.cs** , und ersetzen Sie seinen Inhalt durch den folgenden Code:

```csharp
using System;
using Android.App;
using Android.OS;
using Android.Widget;

namespace LocalNotifications
{
    [Activity(Label = "Second Activity")]
    public class SecondActivity : Activity
    {
        protected override void OnCreate(Bundle bundle)
        {
            base.OnCreate(bundle);

            // Get the count value passed to us from MainActivity:
            var count = Intent.Extras.GetInt(MainActivity.COUNT_KEY, -1);

            // No count was passed? Then just return.
            if (count <= 0)
            {
                return;
            }

            // Display the count sent from the first activity:
            SetContentView(Resource.Layout.Second);
            var txtView = FindViewById<TextView>(Resource.Id.textView1);
            txtView.Text = $"You clicked the button {count} times in the previous activity.";
        }
    }
}
```

Außerdem muss ein Ressourcen Layout für **secondactivity**erstellt werden. Fügen Sie dem Projekt eine neue **Android-Layoutdatei** mit dem Namen **Second. axml**hinzu. Bearbeiten Sie **Second. axml** , und fügen Sie den folgenden Layoutcode ein:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
    android:minWidth="25px"
    android:minHeight="25px">
    <TextView
        android:text=""
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:id="@+id/textView1" />
</LinearLayout>
```


### <a name="add-a-notification-icon"></a>Benachrichtigungssymbol hinzufügen

Fügen Sie abschließend ein kleines Symbol hinzu, das im Benachrichtigungsbereich angezeigt wird, wenn die Benachrichtigung gestartet wird. Sie können [dieses Symbol](local-notifications-walkthrough-images/ic-stat-button-click.png) in Ihr Projekt kopieren oder ein eigenes benutzerdefiniertes Symbol erstellen. Benennen Sie die Symbol **Datei\_IC\_stat\_Button Click. png** , und kopieren Sie Sie in den Ordner **Resources/drawable** . Denken Sie daran, **> vorhandenes Element hinzufügen...** zu verwenden, um diese Symbol Datei in das Projekt einzuschließen.


### <a name="run-the-application"></a>Ausführen der Anwendung

Erstellen Sie die Anwendung, und führen Sie sie aus. Die erste Aktivität sollte wie im folgenden Screenshot dargestellt werden:

[![Screenshot der ersten Aktivität](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Wenn Sie auf die Schaltfläche klicken, sollten Sie feststellen, dass das kleine Symbol für die Benachrichtigung im Benachrichtigungsbereich angezeigt wird:

[![Benachrichtigungssymbol wird angezeigt](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Wenn Sie die Benachrichtigungsleiste schwenken und die Benachrichtigungsleiste verfügbar machen, sollte die Benachrichtigung angezeigt werden:

[![Benachrichtigungs Meldung](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Wenn Sie auf die Benachrichtigung klicken, sollte Sie verschwinden, und die andere Aktivität sollte &ndash; in etwa wie im folgenden Screenshot aussehen:

[![Screenshot der zweiten Aktivität](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Herzlichen Glückwunsch! An diesem Punkt haben Sie die exemplarische Vorgehensweise für die lokale Android-Benachrichtigung abgeschlossen, und Sie haben ein funktionierendes Beispiel, auf das Sie verweisen können. Es gibt noch viel mehr zu Benachrichtigungen, als wir hier gezeigt haben. Wenn Sie weitere Informationen benötigen, sehen Sie sich [die Google-Dokumentation zu Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)an.


## <a name="summary"></a>Zusammenfassung

Diese exemplarische Vorgehens `NotificationCompat.Builder` Weise wird zum Erstellen und Anzeigen von Benachrichtigungen verwendet. Es wurde ein einfaches Beispiel gezeigt, wie Sie eine zweite Aktivität als Reaktion auf die Benutzerinteraktion mit der Benachrichtigung starten und die Übertragung von Daten von der ersten Aktivität zur zweiten Aktivität veranschaulichen.


## <a name="related-links"></a>Verwandte Links

- [Localbenachrichtigungen (Beispiel)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Android Oreo-Benachrichtigungs Kanäle](https://blog.xamarin.com/android-oreo-notification-channels/)
- [Benachrichtigungen](xref:Android.App.Notification)
- [NotificationManager](xref:Android.App.NotificationManager)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](xref:Android.App.PendingIntent)
