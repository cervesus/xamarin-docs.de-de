---
title: Vordergrund-Dienste
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: 3088fa4b5cfa21ac57533ef331ffcc15414e14b4
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="foreground-services"></a>Vordergrund-Dienste

Ein Foreground-Dienst ist ein besonderer Typ eines gebundenen Diensts oder einem gestarteten Dienst. Gelegentlich Services führt Aufgaben, die Benutzer aktiv kennen müssen, diese Dienste werden als bezeichnet _Vordergrund Services_. Ein Beispiel eines Diensts Vordergrund ist eine app, die den Benutzer beim autostrecke oder Fußweg Richtungen gewährt wird. Auch wenn die app im Hintergrund ist, ist es wichtig, dass der Dienst über ausreichende Ressourcen ordnungsgemäß funktioniert und dass der Benutzer eine schnelle und praktische Möglichkeit zum Zugriff auf die app verfügt. Für Android-app, dies bedeutet, dass Hintergrunddienst Vordergrund sollten höheren Priorität als eine "normale" Dienst erhalten ein Vordergrund-Dienst bereitstellen muss eine `Notification` , in denen Android wird angezeigt, solange der Dienst ausgeführt wird.
 
Um einen Vordergrund-Dienst zu starten, muss die app Priorität verteilen, die zum Starten des Diensts Android informiert. Dann muss der Dienst selbst als Dienst Vordergrund mit Android registrieren. Apps, die unter Android 8.0 (oder höher) ausgeführt werden, sollten verwenden die `Context.StartForegroundService` Methode, um den Dienst zu starten, obwohl verwendet werden apps, die auf Geräten mit einer älteren Version von Android ausgeführt werden soll `Context.StartService`

Diese C#-Erweiterungsmethode ist ein Beispiel zum Starten eines Diensts Vordergrund. Auf Android 8.0 und höher verwendet die `StartForegroundService` -Methode, andernfalls das ältere `StartService` verwendet wird.  

```csharp
public static void StartForegroundServiceComapt<T>(this Context context, Bundle args = null) where T : Service
{
    var intent = new Intent(context, typeof(T));
    if (args != null) 
    {
        intent.PutExtras(args);
    }

    if (Android.OS.Build.VERSION.SdkInt >= Android.OS.BuildVersionCodes.O)
    {
        context.StartForegroundService(intent);
    }
    else
    {
        context.StartService(intent);
    }
}
```

## <a name="registering-as-a-foreground-service"></a>Registrieren als Hintergrunddienst Vordergrund

Nachdem ein Foreground-Dienst gestartet wurde, müssen sie selbst mit Android registrieren, durch den Aufruf der [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Wenn der Dienst gestartet wurde, mit der `Service.StartForegroundService` Methode registriert aber nicht selbst dann Android beenden Sie den Dienst und die app als nicht reagierend gekennzeichnet wird.

`StartForeground` verwendet zwei Parameter, die beide erforderlich sind:
 
* Ein Ganzzahlwert, der in der Anwendung zum Identifizieren des Diensts eindeutig ist.
* Ein `Notification` -Objekt, das in der Statusleiste für Android angezeigt werden, solange der Dienst ausgeführt wird.

Android wird in der Statusleiste für die Benachrichtigung angezeigt, solange der Dienst ausgeführt wird. Die Benachrichtigung, zumindest, stellt einen visuellen Hinweis für dem Benutzer bereit, die der Dienst ausgeführt wird. Im Idealfall sollte die Benachrichtigung der Benutzer mit einer Verknüpfung zu der Anwendung oder möglicherweise einige Aktionsschaltflächen, steuern Sie die Anwendung bereitstellen. Ein Beispiel hierfür ist, ein Musikplayer &ndash; die Benachrichtigung, die angezeigt wird, ist möglicherweise Schaltflächen zum Anhalten/Musik, um auf den vorherigen Titel rewind oder zum nächsten Musiktitel überspringen wiedergeben. 

Dieser Codeausschnitt ist ein Beispiel für einen Dienst als Vordergrund Dienst registrieren:   

```csharp
// This is any integer value unique to the application.
public const int SERVICE_RUNNING_NOTIFICATION_ID = 10000;

public override StartCommandResult OnStartCommand(Intent intent, StartCommandFlags flags, int startId)
{
    // Code not directly related to publishing the notification has been omitted for clarity.
    // Normally, this method would hold the code to be run when the service is started.
    
    var notification = new Notification.Builder(this)
        .SetContentTitle(Resources.GetString(Resource.String.app_name))
        .SetContentText(Resources.GetString(Resource.String.notification_text))
        .SetSmallIcon(Resource.Drawable.ic_stat_name)
        .SetContentIntent(BuildIntentToShowMainActivity())
        .SetOngoing(true)
        .AddAction(BuildRestartTimerAction())
        .AddAction(BuildStopServiceAction())
        .Build();

    // Enlist this instance of the service as a foreground service
    StartForeground(SERVICE_RUNNING_NOTIFICATION_ID, notification);
}
```

Die vorherige Benachrichtigung zeigt eine statusleistenbenachrichtigung, ähnlich der folgenden ist:

![Darstellung die Benachrichtigung in der Statusleiste des](foreground-services-images/foreground-services-01.png "Bild zeigt die Benachrichtigung in der Statusleiste")

Diese bildschirmabbildung zeigt die erweiterte Benachrichtigung im Benachrichtigungsbereich mit zwei Aktionen, die den Benutzer zum Steuern des Diensts zu ermöglichen:

![Darstellung des erweiterte Benachrichtigung](foreground-services-images/foreground-services-02.png "Darstellung des erweiterte Benachrichtigung.")

Weitere Informationen zu Benachrichtigungen finden Sie in der [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md) Teil der [Android-Benachrichtigungen](~/android/app-fundamentals/notifications/index.md) Handbuch.

## <a name="unregistering-as-a-foreground-service"></a>Aufheben der Registrierung als Hintergrunddienst Vordergrund

Ein Dienst kann deserialisiert auflisten selbst als Dienst Vordergrund durch Aufrufen der Methode `StopForeground`. `StopForeground` den Dienst wird nicht beendet, doch er entfernt die Benachrichtigungssymbol und signalisiert Android, die diesen Dienst bei Bedarf heruntergefahren werden kann.

Statusleistenbenachrichtigung an, die angezeigt wird, kann auch durch Übergabe entfernt werden `true` an die Methode: 

```csharp
StopForeground(true);
```

Wenn der Dienst, mit einem Aufruf von angehalten wird `StopSelf` oder `StopService`, die statusleistenbenachrichtigung entfernt werden.

## <a name="related-links"></a>Verwandte Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
