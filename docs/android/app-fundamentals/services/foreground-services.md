---
title: Vordergrunddienste
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: aaef03d68f4ec374a3bc706daffe636e575e42ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024758"
---
# <a name="foreground-services"></a>Vordergrunddienste

Ein Vordergrund Dienst ist ein spezieller Typ eines gebundenen Dienstanbieter oder ein gestarteter Dienst. Gelegentlich führen Dienste Aufgaben aus, die Benutzer aktiv kennen müssen. diese Dienste werden als _Vordergrund Dienste_bezeichnet. Ein Beispiel für einen Vordergrund-Dienst ist eine APP, die dem Benutzer während des Fahrens oder durch lauffahrens Anweisungen bereitstellt. Auch wenn sich die APP im Hintergrund befindet, ist es immer noch wichtig, dass der Dienst über genügend Ressourcen verfügt, um ordnungsgemäß zu funktionieren, und dass der Benutzer schnell und praktisch auf die App zugreifen kann. Bei einer Android-App bedeutet dies, dass ein Vordergrund Dienst eine höhere Priorität als ein regulärer Dienst erhalten sollte und ein Vordergrund Dienst eine `Notification` bereitstellen muss, die Android anzeigt, solange der Dienst ausgeführt wird.

Um einen Vordergrund Dienst zu starten, muss die APP eine Absicht verteilen, die Android anweist, den Dienst zu starten. Anschließend muss sich der Dienst als Vordergrund Dienst bei Android registrieren. Apps, die unter Android 8,0 (oder höher) ausgeführt werden, sollten den Dienst mit der `Context.StartForegroundService`-Methode starten, während apps, die auf Geräten mit einer älteren Version von Android ausgeführt werden, `Context.StartService`

Diese C# Erweiterungsmethode ist ein Beispiel für das Starten eines Vordergrund Dienstanbieter. Unter Android 8,0 und höher wird die `StartForegroundService`-Methode verwendet; andernfalls wird die ältere `StartService`-Methode verwendet.

```csharp
public static void StartForegroundServiceCompat<T>(this Context context, Bundle args = null) where T : Service
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

## <a name="registering-as-a-foreground-service"></a>Registrieren als Vordergrund Dienst

Sobald ein Vordergrund Dienst gestartet wurde, muss er sich bei Android registrieren, indem er den [`StartForeground`](xref:Android.App.Service.StartForeground*)aufruft. Wenn der Dienst mit der `Service.StartForegroundService`-Methode gestartet wird, sich aber nicht selbst registriert, hält Android den Dienst an, und die APP wird als nicht reagierend gekennzeichnet.

`StartForeground` benötigt zwei Parameter, die beide obligatorisch sind:

- Ein ganzzahliger Wert, der innerhalb der Anwendung eindeutig ist, um den Dienst zu identifizieren.
- Ein `Notification` Objekt, das von Android in der Statusleiste angezeigt wird, solange der Dienst ausgeführt wird.

Android zeigt die Benachrichtigung in der Statusleiste an, solange der Dienst ausgeführt wird. Die Benachrichtigung enthält mindestens einen visuellen Hinweis für den Benutzer, dass der Dienst ausgeführt wird. Im Idealfall sollte der Benutzer in der Benachrichtigung eine Verknüpfung zur Anwendung oder möglicherweise einige Aktions Schaltflächen zum Steuern der Anwendung bereitstellen. Ein Beispiel hierfür ist ein Musikplayer, &ndash; die angezeigte Benachrichtigung möglicherweise Schaltflächen zum Anhalten/Wiedergeben von Musik, zum Zurückspulen des vorherigen Titels oder zum Springen zum nächsten Song hat. 

Dieser Code Ausschnitt ist ein Beispiel für die Registrierung eines Dienstanbieter als Vordergrund Dienst:   

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

In der vorherigen Benachrichtigung wird eine Status leisten Benachrichtigung angezeigt, die der folgenden ähnelt:

![Bild, das die Benachrichtigung in der Statusleiste anzeigt](foreground-services-images/foreground-services-01.png "Bild, das die Benachrichtigung in der Statusleiste anzeigt")

Dieser Screenshot zeigt die erweiterte Benachrichtigung in der Benachrichtigungsleiste mit zwei Aktionen, die es dem Benutzer ermöglichen, den Dienst zu steuern:

![Das Bild zeigt die erweiterte Benachrichtigung.](foreground-services-images/foreground-services-02.png "Das Bild zeigt die erweiterte Benachrichtigung.")

Weitere Informationen zu Benachrichtigungen finden Sie im [Android-Benachrichtigungs](~/android/app-fundamentals/notifications/index.md) Handbuch im Abschnitt [lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md) .

## <a name="unregistering-as-a-foreground-service"></a>Aufheben der Registrierung als Vordergrund Dienst

Ein Dienst kann sich selbst als Vordergrund Dienst aufheben, indem er die-Methode `StopForeground` aufruft. `StopForeground` wird den Dienst nicht beenden, sondern entfernt das Benachrichtigungssymbol und signalisiert Android, dass dieser Dienst bei Bedarf heruntergefahren werden kann.

Die angezeigte Status leisten Benachrichtigung kann auch entfernt werden, indem `true` an die-Methode übergeben wird: 

```csharp
StopForeground(true);
```

Wenn der Dienst mit einem `StopSelf`-oder `StopService`-Rückruf angehalten wird, wird die Status leisten Benachrichtigung entfernt.

## <a name="related-links"></a>Verwandte Links

- [Android. app. Service](xref:Android.App.Service)
- [Android. app. Service. startvorder Grund](xref:Android.App.Service.StartForeground*)
- [Lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md)
- [Foregroundservicedemo (Beispiel)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-foregroundservicedemo)
