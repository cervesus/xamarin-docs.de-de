---
title: "Exemplarische Vorgehensweise – mit lokalen Benachrichtigungen in Xamarin.Android"
description: "In dieser exemplarischen Vorgehensweise veranschaulicht, wie lokale Benachrichtigungen in Xamarin.Android Anwendungen verwendet wird. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine lokale Benachrichtigung. Wenn der Benutzer die Benachrichtigung im Infobereich klickt, wird er eine zweite Aktivität gestartet."
ms.topic: article
ms.prod: xamarin
ms.assetid: D8C6C9E2-3282-49D1-A2F6-78A4F3306E29
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 01/30/2018
ms.openlocfilehash: 4728b50446033c02d33ccf8273f1dc2e50d66906
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="walkthrough---using-local-notifications-in-xamarinandroid"></a>Exemplarische Vorgehensweise – mit lokalen Benachrichtigungen in Xamarin.Android

_In dieser exemplarischen Vorgehensweise veranschaulicht, wie lokale Benachrichtigungen in Xamarin.Android Anwendungen verwendet wird. Es veranschaulicht die Grundlagen zum Erstellen und veröffentlichen eine lokale Benachrichtigung. Wenn der Benutzer die Benachrichtigung im Infobereich klickt, wird er eine zweite Aktivität gestartet._

<a name="overview" />

## <a name="overview"></a>Übersicht

In dieser exemplarischen Vorgehensweise erstellen wir eine Android-Anwendung, die eine Benachrichtigung auslöst, klickt der Benutzer eine Schaltfläche in einer Aktivität. Klickt der Benutzer die Benachrichtigung, startet er eine zweite Aktivität, die die Anzahl der Male zeigt an, der Benutzer die Schaltfläche in der ersten Aktivität geklickt hatte.

Die folgenden Screenshots veranschaulicht einige Beispiele für diese Anwendung:

[![Beispiel-Screenshots mit der Benachrichtigung](local-notifications-walkthrough-images/1-overview-sml.png)](local-notifications-walkthrough-images/1-overview.png)


<a name="walkthrough" />

## <a name="walkthrough"></a>Exemplarische Vorgehensweise

Zunächst sehen wir erstellen ein neues Android-Projekt mit der **Android-App** Vorlage. Wir nennen Sie dieses Projekt **LocalNotifications**. (Wenn Sie nicht mit dem Erstellen von Projekten Xamarin.Android vertraut sind, finden Sie unter [Hello, Android](~/android/get-started/hello-android/hello-android-quickstart.md).)

<a name="add-v4-support" />

### <a name="add-the-androidsupportv4app-component"></a>Fügen Sie der Android.Support.V4.App-Komponente hinzu

In dieser exemplarischen Vorgehensweise verwenden wir `NotificationCompat.Builder` unserer lokalen Benachrichtigung erstellen. Wie in beschrieben [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md), schließen wir die [Android Unterstützungsbibliothek v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/) NuGet in unserem Projekt verwenden `NotificationCompat.Builder`.

Als Nächstes bearbeiten wir **MainActivity.cs** und fügen Sie die folgenden `using` Anweisung, damit die Typen in `Android.Support.V4.App` stehen getesteten Codes:

```csharp
using Android.Support.V4.App;
```

Darüber hinaus wir müssen unbedingt klar, auf dem Compiler, die wir verwenden die `Android.Support.V4.App` Version `TaskStackBuilder` statt über das `Android.App` Version. Fügen Sie die folgenden `using` Anweisung um eine Mehrdeutigkeit zu beheben:

```csharp
using TaskStackBuilder = Android.Support.V4.App.TaskStackBuilder;
```

<a name="define-id" />

### <a name="define-the-notification-id"></a>Definieren Sie die Benachrichtigungs-ID

Wir benötigen eine eindeutige ID für unsere Benachrichtigung. Ermöglicht das Bearbeiten **MainActivity.cs** und fügen Sie die folgenden statische Instanz-Variable, um die `MainActivity` Klasse:

```csharp
private static readonly int ButtonClickNotificationId = 1000;
```

<a name="add-code" />

### <a name="add-code-to-generate-the-notification"></a>Fügen Sie Code aus, um die Benachrichtigung generiert wird, hinzu.

Wir als Nächstes erstellen Sie einen neuen Ereignishandler für die Schaltfläche `Click` Ereignis. Fügen Sie die folgende Methode hinzu `MainActivity`:

```csharp
private void ButtonOnClick (object sender, EventArgs eventArgs)
{
    // Pass the current button press count value to the next activity:
    Bundle valuesForActivity = new Bundle();
    valuesForActivity.PutInt ("count", count);

    // When the user clicks the notification, SecondActivity will start up.
    Intent resultIntent = new Intent(this, typeof (SecondActivity));

    // Pass some values to SecondActivity:
    resultIntent.PutExtras (valuesForActivity);

    // Construct a back stack for cross-task navigation:
    TaskStackBuilder stackBuilder = TaskStackBuilder.Create (this);
    stackBuilder.AddParentStack (Java.Lang.Class.FromType(typeof(SecondActivity)));
    stackBuilder.AddNextIntent (resultIntent);

    // Create the PendingIntent with the back stack:            
    PendingIntent resultPendingIntent =
        stackBuilder.GetPendingIntent (0, (int)PendingIntentFlags.UpdateCurrent);

    // Build the notification:
    NotificationCompat.Builder builder = new NotificationCompat.Builder (this)
        .SetAutoCancel (true)                    // Dismiss from the notif. area when clicked
        .SetContentIntent (resultPendingIntent)  // Start 2nd activity when the intent is clicked.
        .SetContentTitle ("Button Clicked")      // Set its title
        .SetNumber (count)                       // Display the count in the Content Info
        .SetSmallIcon(Resource.Drawable.ic_stat_button_click)  // Display this icon
        .SetContentText (String.Format(
            "The button has been clicked {0} times.", count)); // The message to display.

    // Finally, publish the notification:
    NotificationManager notificationManager =
        (NotificationManager)GetSystemService(Context.NotificationService);
    notificationManager.Notify(ButtonClickNotificationId, builder.Build());

    // Increment the button press count:
    count++;
}
```

In der `OnCreate` -Methode, weisen Sie dieses `ButtonOnClick` Methode, um die `Click` -Ereignis der Schaltfläche (ersetzen Sie die Ereignishandler des Delegaten, die von der Vorlage bereitgestellten):

```csharp
button.Click += ButtonOnClick;
```

<a name="second-activity" />

### <a name="create-a-second-activity"></a>Erstellen Sie eine zweite Aktivität

Nun müssen wir eine andere Aktivität erstellen, in denen Android klickt der Benutzer unserer Benachrichtigung angezeigt wird. Hinzufügen einer anderen Android Aktivität zu Ihrem Projekt namens **SecondActivity**. Open **SecondActivity.cs** und Ersetzen Sie den Inhalt durch den folgenden Code:

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
        protected override void OnCreate (Bundle bundle)
        {
            base.OnCreate (bundle);

            // Get the count value passed to us from MainActivity:
            int count = Intent.Extras.GetInt ("count", -1);

            // No count was passed? Then just return.
            if (count <= 0)
                return;

            // Display the count sent from the first activity:
            SetContentView (Resource.Layout.Second);
            TextView textView = FindViewById<TextView>(Resource.Id.textView);
            textView.Text = String.Format (
                "You clicked the button {0} times in the previous activity.", count);
        }
    }
}
```

Wir müssen auch erstellen, ein Layout Ressource für **SecondActivity**. Fügen Sie einen neuen **Android Layout** Datei in Ihrem Projekt namens **Second.axml**. Bearbeiten Sie **Second.axml** und fügen Sie in den folgenden Layoutcode:

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
        android:id="@+id/textView" />
</LinearLayout>
```

<a name="add-icon" />

### <a name="add-a-notification-icon"></a>Fügen Sie einem Symbol hinzu.

Zum Schluss fügen Sie ein kleines Symbol, das im Infobereich angezeigt wird, wenn unsere Benachrichtigung gestartet wird. Sie können kopieren [dieses Symbol](local-notifications-walkthrough-images/ic-stat-button-click.png) zu Ihrem Projekt oder ein eigene benutzerdefinierte Symbol "erstellen". Den Namen der Symboldatei **ic\_Stat\_Schaltfläche\_click.png** und kopieren Sie sie in der **Ressourcen und Ausgaben möglich** Ordner. Denken Sie daran, verwenden Sie **hinzufügen > Vorhandenes Element...**  auf dieses Symboldatei im Projekt enthalten.

<a name="run-app" />

### <a name="run-the-application"></a>Ausführen der Anwendung

Wir erstellen, und führen Sie die Anwendung. Die erste Aktivität, die mit dem folgenden Screenshot vergleichbarer sollte angezeigt werden:

[ ![Screenshot der ersten Aktivität](local-notifications-walkthrough-images/2-start-screen-sml.png)](local-notifications-walkthrough-images/2-start-screen.png)

Wie Sie die Schaltfläche klicken, sollten Sie das kleine Symbol für die Benachrichtigung im Infobereich angezeigt feststellen:

[ ![Symbol "Benachrichtigung" angezeigt wird.](local-notifications-walkthrough-images/3-notification-icon-sml.png)](local-notifications-walkthrough-images/3-notification-icon.png)

Wenn Sie nach unten Streifen und verfügbar öffnen Sie die Benachrichtigung machen, sollte die Benachrichtigung angezeigt werden:

[ ![Benachrichtigungsmeldung](local-notifications-walkthrough-images/4-notifications-sml.png)](local-notifications-walkthrough-images/4-notifications.png)

Wenn Sie klicken Sie auf die Benachrichtigung, es verschwinden sollte und andere Aktivität gestartet werden sollte &ndash; in etwa dem folgenden Screenshot suchen:

[ ![Screenshot der zweiten Aktivität](local-notifications-walkthrough-images/5-second-activity-sml.png)](local-notifications-walkthrough-images/5-second-activity.png)

Herzlichen Glückwunsch! An diesem Punkt auf der Sie Android lokalen Benachrichtigung dieser exemplarischen Vorgehensweise abgeschlossen haben, und Ihnen ein funktionierendes Beispiel, das Sie verwenden können. Es gibt viele weitere Benachrichtigungen als wir hier gezeigt haben daher ggf. Weitere Informationen sehen Sie sich [Google Dokumentation auf Benachrichtigungen](http://developer.android.com/guide/topics/ui/notifiers/notifications.html) und das Android [Benachrichtigungen](http://developer.android.com/design/patterns/notifications.html) -Entwurfshandbuch.


<a name="summary" />

## <a name="summary"></a>Zusammenfassung

In dieser exemplarischen Vorgehensweise wird `NotificationCompat.Builder` zum Erstellen und Anzeigen von Benachrichtigungen. Ein einfaches Beispiel zum Starten einer zweiten Aktivität als eine Möglichkeit für die Reaktion auf eine Benutzerinteraktion bei der Benachrichtigung wurde erläutert, und wir die Übertragung von Daten aus der ersten Aktivität gezeigt haben, um die zweite Aktivität.


## <a name="related-links"></a>Verwandte Links

- [LocalNotifications (Beispiel)](https://developer.xamarin.com/samples/monodroid/LocalNotifications/)
- [Handbuch Entwurfsmuster für Benachrichtigungen](http://developer.android.com/design/patterns/notifications.html)
- [Benachrichtigung](https://developer.xamarin.com/api/type/Android.App.Notification/)
- [NotificationManager](https://developer.xamarin.com/api/type/Android.App.NotificationManager/)
- [NotificationCompat.Builder](https://developer.android.com/reference/android/support/v4/app/NotificationCompat.Builder.html)
- [PendingIntent](https://developer.xamarin.com/api/type/Android.App.PendingIntent/)
