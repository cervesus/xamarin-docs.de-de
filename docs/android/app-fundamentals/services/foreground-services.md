---
title: Vordergrunddienste
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: df917896f901060a5518076afa859d34a03f4d6d
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50108352"
---
# <a name="foreground-services"></a>Vordergrunddienste

Ein Foreground-Dienst ist ein spezieller Typ von einer gebundenen oder einen Dienst wurde gestartet. Gelegentlich Services führt Aufgaben, die Benutzer aktiv kennen müssen, diese Dienste werden als bezeichnet _vordergrunddienste_. Ein Beispiel für einen Dienst Vordergrund ist eine app, die den Benutzer beim fahren oder gehen erfahren Sie, wie bereit ist. Auch wenn die app im Hintergrund ist, ist es dennoch wichtig, dass der Dienst über genügend Ressourcen ordnungsgemäß funktioniert hat und der Benutzer eine schnelle und praktische Möglichkeit zum Zugriff auf die app verfügt. Für eine Android-app, das bedeutet, dass ein Foreground-Dienst erhalten eine höheren Priorität als ein "regular" Dienst muss es sich um eine Bereitstellung eines Diensts Vordergrund einer `Notification` , in denen Android wird angezeigt, solange der Dienst ausgeführt wird.
 
Um ein Foreground-Dienst zu starten, muss die app einen Intent verteilen, die mitteilt, Android, die zum Starten des Diensts. Klicken Sie dann muss der Dienst selbst als Dienst Vordergrund mit Android registrieren. Apps, die unter Android 8.0 (oder höher) ausgeführt werden sollten verwenden die `Context.StartForegroundService` Methode, um den Dienst zu starten, während apps, die auf Geräten mit einer älteren Version von Android ausgeführt werden, verwenden sollten `Context.StartService`

Diese C#-Erweiterungsmethode ist ein Beispiel zum Starten eines Diensts Vordergrund. Unter Android 8.0 und höher verwendet die `StartForegroundService` Methode, die andernfalls die ältere `StartService` verwendet wird.  

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

## <a name="registering-as-a-foreground-service"></a>Registrieren als Dienst Vordergrund

Nachdem ein Foreground-Dienst gestartet wurde, müssen sie selbst mit Android registrieren, durch den Aufruf der [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/). Wenn der Dienst gestartet wird, mit der `Service.StartForegroundService` Methode aber nicht registriert, und klicken Sie dann für Android wird den Dienst beenden und die app als nicht reagierend gekennzeichnet.

`StartForeground` akzeptiert zwei Parameter, die beide erforderlich sind:
 
* Ein ganzzahliger Wert, der in der Anwendung zum Identifizieren des Diensts eindeutig ist.
* Ein `Notification` -Objekt, das in der Statusleiste für Android angezeigt werden, solange der Dienst ausgeführt wird.

Android wird in der Statusleiste für die Benachrichtigung angezeigt, solange der Dienst ausgeführt wird. Die Benachrichtigung mindestens bietet einen visuellen Hinweis für dem Benutzer, die der Dienst ausgeführt wird. Im Idealfall sollte die Benachrichtigung der Benutzer mit einer Verknüpfung zu der Anwendung oder möglicherweise einige Aktionsschaltflächen zur Steuerung der Anwendungs bereitstellen. Ein Beispiel hierfür ist ein Musikplayer &ndash; die Benachrichtigung, die angezeigt wird, handelt es sich möglicherweise um Schaltflächen zum Anhalten/Wiedergeben von Musik, auf die vorherige "Song" rewind oder zum nächsten Musiktitel zu überspringen. 

Dieser Codeausschnitt ist ein Beispiel für einen Dienst als Vordergrund Dienst zu registrieren:   

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

Die vorherige Benachrichtigung wird eine statusleistenbenachrichtigung angezeigt, die dem folgenden ähnelt:

![Bild, auf dem die Benachrichtigung in der Statusleiste](foreground-services-images/foreground-services-01.png "Bild, auf dem die Benachrichtigung in der Statusleiste")

Dieser Screenshot zeigt die erweiterte Benachrichtigung im Benachrichtigungsbereich mit zwei Aktionen, mit die den Benutzer den Dienst steuern können:

![Bild, auf dem die erweiterte Benachrichtigung](foreground-services-images/foreground-services-02.png "Bild, auf dem die erweiterte Benachrichtigung.")

Weitere Informationen zu Benachrichtigungen finden Sie in der [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md) Teil der [Android-Benachrichtigungen](~/android/app-fundamentals/notifications/index.md) Guide.

## <a name="unregistering-as-a-foreground-service"></a>Aufheben der Registrierung als Foreground-Dienst

Ein Dienst kann aufheben auflisten selbst als Dienst Vordergrund durch Aufrufen der Methode `StopForeground`. `StopForeground` den Dienst wird nicht beendet, aber es entfernt das Benachrichtigungssymbol und Signale Android, die dieser Dienst bei Bedarf heruntergefahren werden kann.

Statusleistenbenachrichtigung an, die angezeigt wird, kann ebenfalls entfernt werden, durch Übergabe `true` an die Methode: 

```csharp
StopForeground(true);
```

Wenn der Dienst, mit einem Aufruf von angehalten wird `StopSelf` oder `StopService`, die statusleistenbenachrichtigung entfernt werden.

## <a name="related-links"></a>Verwandte Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForeground](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
