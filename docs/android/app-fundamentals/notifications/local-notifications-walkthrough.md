---
title: 'Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in Xamarin.Android'
description: Diese exemplarische Vorgehensweise veranschaulicht, wie Sie lokale Benachrichtigungen in Xamarin.Android-Anwendungen zu verwenden. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine lokale Benachrichtigung. Wenn der Benutzer die Benachrichtigung im Infobereich klickt, wird es einer zweiten Aktivität gestartet.
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 08/16/2018
ms.openlocfilehash: e60ed6cc49921fc7b6e8e2616a6b0bf6f8abb401
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61026842"
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Exemplarische Vorgehensweise: Verwenden von lokalen Benachrichtigungen in Xamarin.Android

_Diese exemplarische Vorgehensweise veranschaulicht, wie Sie lokale Benachrichtigungen in Xamarin.Android-Anwendungen zu verwenden. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine lokale Benachrichtigung. Wenn der Benutzer die Benachrichtigung im Infobereich klickt, wird es einer zweiten Aktivität gestartet._


## <a name="overview"></a>Übersicht

In dieser exemplarischen Vorgehensweise erstellen wir eine Android-Anwendung, die eine Benachrichtigung auslöst, klickt der Benutzer eine Schaltfläche in einer Aktivität. Wenn der Benutzer die Benachrichtigung klickt, wird eine zweite Aktivität, die die Anzahl der anzeigt, der Benutzer die Schaltfläche in der ersten Aktivität geklickt hätte, gestartet.

Die folgenden Screenshots zeigen einige Beispiele für diese Anwendung:

[![Beispiel-Screenshot mit der Benachrichtigung](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png#lightbox)

> [!NOTE]
> Dieser Leitfaden konzentriert sich auf die [NotificationCompat APIs](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.html) aus der [Android Support-Bibliothek](https://www.nuget.org/packages/Xamarin.Android.Support.v4/). Diese APIs werden maximal Abwärtskompatibilität mit Android 4.0 (API-Ebene 14) sichergestellt.

## <a name="creating-the-project"></a>Erstellen des Projekts

Wir erstellen Sie zunächst ein neues Android-Projekt mit der **Android-App** Vorlage. Wir nennen dieses Projekt **LocalNotifications**. (Wenn Sie nicht mit dem Erstellen von Xamarin.Android-Projekte vertraut sind, finden Sie unter [Hallo, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

Die Ressourcendatei bearbeiten **values/Strings.xml** , damit sie zwei zusätzliche Zeichenfolgenressourcen enthält, die verwendet wird, wenn es den Benachrichtigungskanal erstellt wird:


```xml
<?xml version="1.0" encoding="utf-8"?>

<resources>
  <string name="Hello">Hello World, Click Me!</string>
  <string name="ApplicationName">Notifications</string>

  <string name="channel_name">Local Notifications</string>
  <string name="channel_description">The count from MainActivity.</string>
</resources>
```

### <a name="add-the-androidsupportv4-nuget-package"></a>Fügen Sie das Android.Support. v4-NuGet-Paket hinzu.

In dieser exemplarischen Vorgehensweise verwenden wir `NotificationCompat.Builder` unsere lokale Benachrichtigung zu erstellen. Siehe [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md), schließen wir die [Android Support-Bibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet in Ihrem Projekt mit `NotificationCompat.Builder`.

Als Nächstes ermöglicht das Bearbeiten von **"mainactivity.cs"** und fügen Sie die folgenden `using` Anweisung so, dass die Typen in `Android.Support.V4.App` sind für Code verfügbar:

```csharp
using Android.Support.V4.App;
```

Darüber hinaus wir müssen erleichtern Sie dem Compiler, die wir verwenden die `Android.Support.V4.App` Version `TaskStackBuilder` anstelle der `Android.App` Version. Fügen Sie die folgenden `using` Anweisung Mehrdeutigkeiten auflösen:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

### <a name="create-the-notification-channel"></a>Erstellen Sie den Benachrichtigungskanal

Fügen Sie eine Methode, um `MainActivity` , die erstellt eines Kanals (falls erforderlich):

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

Update der `OnCreate` Methode, um diese neue Methode aufzurufen:

```csharp
protected override void OnCreate(Bundle bundle)
{
    base.OnCreate(bundle);
    SetContentView(Resource.Layout.Main);

    CreateNotificationChannel();
}
```


### <a name="define-the-notification-id"></a>Definieren Sie die benachrichtigungs-ID

Wir benötigen eine eindeutige ID für unsere Benachrichtigung und den Benachrichtigungskanal. Ermöglicht das Bearbeiten von **"mainactivity.cs"** und fügen Sie die folgenden statischen Instanzvariable zu den `MainActivity` Klasse:

```csharp
static readonly int NOTIFICATION_ID = 1000;
static readonly string CHANNEL_ID = "location_notification";
internal static readonly string COUNT_KEY = "count";
```

### <a name="add-code-to-generate-the-notification"></a>Hinzufügen von Code zum Generieren der benachrichtigungs

Als Nächstes müssen wir erstellen einen neuen Ereignishandler für die Schaltfläche `Click` Ereignis. Fügen Sie die folgende Methode `MainActivity`:

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

Die `OnCreate` -Methode der MainActivity muss den Aufruf den Benachrichtigungskanal erstellen und zuweisen, die `ButtonOnClick` Methode, um die `Click` -Ereignis der Schaltfläche (ersetzen Sie die Ereignishandler des Delegaten, die von der Vorlage bereitgestellten):

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


### <a name="create-a-second-activity"></a>Erstellen Sie eine zweite Aktivität

Jetzt müssen wir eine andere Aktivität zu erstellen, die Android angezeigt werden, wenn der Benutzer auf unsere Benachrichtigung klickt. Hinzufügen von einer anderen Android-Aktivität zu Ihrem Projekt namens **SecondActivity**. Open **SecondActivity.cs** und Ersetzen Sie den Inhalt durch den folgenden Code:

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

Wir müssen auch erstellen, ein ressourcenlayout für **SecondActivity**. Fügen Sie einen neuen **Android-Layout** Datei in Ihrem Projekt namens **Second.axml**. Bearbeiten Sie **Second.axml** und fügen Sie in den folgenden Layoutcode:

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


### <a name="add-a-notification-icon"></a>Fügen Sie ein Symbol hinzu.

Fügen Sie schließlich ein kleines Symbol, das im Infobereich der Taskleiste angezeigt wird, wenn die Benachrichtigung gestartet wird. Sie können kopieren [dieses Symbol](local-notifications-walkthrough-images/ic-stat-button-click.png) zu Ihrem Projekt, oder erstellen Sie Ihr eigenes benutzerdefinierte Symbol. Benennen Sie die Symboldatei **ic\_Stat\_Schaltfläche\_click.png** und kopieren Sie sie in der **Ressourcen/drawable** Ordner. Denken Sie daran, verwenden Sie **hinzufügen > Vorhandenes Element...**  auf dieses Symboldatei in Ihr Projekt einbinden.


### <a name="run-the-application"></a>Ausführen der Anwendung

Erstellen Sie die Anwendung, und führen Sie sie aus. Ihnen sollte die erste Aktivität, die ähnlich wie im folgenden Screenshot angezeigt werden:

[![Screenshot der ersten Aktivität](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png#lightbox)

Wie Sie die Schaltfläche klicken, sollten Sie feststellen, dass das kleine Symbol für die Benachrichtigung im Infobereich der Taskleiste angezeigt wird:

[![Symbol "Benachrichtigung" angezeigt wird.](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png#lightbox)

Wenn Sie streichen Sie nach unten, und machen die Benachrichtigung, sollte die Benachrichtigung angezeigt werden:

[![Benachrichtigungsmeldung](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png#lightbox)

Wenn Sie klicken Sie auf die Benachrichtigung, es nicht mehr angezeigt, und unsere anderen Aktivität gestartet werden sollte &ndash; etwas wie im folgenden Screenshot zu suchen:

[![Screenshot der zweiten Aktivität](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png#lightbox)

Herzlichen Glückwunsch! An diesem Punkt haben Sie die exemplarische Vorgehensweise für Android lokale Benachrichtigung, und Sie haben ein funktionierendes Beispiel, das Sie verwenden können. Es gibt viele weitere Benachrichtigungen als wir hier gezeigt haben also wenn Sie weitere Informationen benötigen, sehen Sie sich [Google-Dokumentation zur Benachrichtigungen](https://developer.android.com/guide/topics/ui/notifiers/notifications.html).


## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise verwendet `NotificationCompat.Builder` erstellen und Anzeigen von Benachrichtigungen. Es wurde ein einfaches Beispiel, um eine zweite Aktivität als eine mögliche Reaktion auf eine Benutzerinteraktion mit der Benachrichtigung zu starten, und es veranschaulicht die Übertragung von Daten von der ersten Aktivität aus, für die zweite Aktivität.


## <a name="related-links"></a>Verwandte Links

- [LocalNotifications (Beispiel)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Benachrichtigungskanäle für Android Oreo](https://blog.xamarin.com/android-oreo-notification-channels/)
- [Benachrichtigung](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
