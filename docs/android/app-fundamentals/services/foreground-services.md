---
title: Vordergrund-Dienste
ms.topic: article
ms.prod: xamarin
ms.assetid: C10FD999-7A91-4708-B642-0C1B0901BD24
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 857c7fe47ad5fa0f50fce95672bc8ffbf6771dc9
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="foreground-services"></a>Vordergrund-Dienste

Einige Dienste Ausführen einiger Aufgaben, die Benutzer aktiv zur Kenntnis genommen haben, werden diese Dienste als bezeichnet _Vordergrund Services_. Ein Beispiel eines Diensts Vordergrund ist eine app, die den Benutzer beim autostrecke oder Fußweg Richtungen gewährt wird. Auch wenn die app im Hintergrund ist, ist es wichtig, dass der Dienst über ausreichende Ressourcen ordnungsgemäß funktioniert und dass der Benutzer eine schnelle und praktische Möglichkeit zum Zugriff auf die app verfügt. Für Android-app, dies bedeutet, dass Hintergrunddienst Vordergrund sollten höheren Priorität als eine "normale" Dienst erhalten ein Vordergrund-Dienst bereitstellen muss eine `Notification` , in denen Android wird angezeigt, solange der Dienst ausgeführt wird.
 
Ein Foreground-Dienst erstellt und ebenso wie jeden anderen Dienst gestartet. Wenn der Dienst gestartet wird, werden selbst mit Android als Vordergrund-Dienst registriert.
 
Dieses Handbuch werden die zusätzlichen Schritte erläutert, die ausgeführt werden muss, um einer Vordergrund-Dienst zu registrieren und zum Beenden des Diensts, wenn sie danach.

## <a name="registering-as-a-foreground-service"></a>Registrieren als Hintergrunddienst Vordergrund

Ein Foreground-Dienst ist ein besonderer Typ eines gebundenen Diensts oder einem gestarteten Dienst. Der Dienst, nachdem es gestartet wurde, ruft der [ `StartForeground` ](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/) Methode, um sich selbst bei Android als Vordergrund Dienst registrieren.   

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

Wenn der Dienst, mit einem Aufruf von angehalten wird `StopSelf` oder `StopService`, und klicken Sie dann den Status Leiste Benachrichtigung ebenso entfernt wird.


## <a name="related-links"></a>Verwandte Links

- [Android.App.Service](https://developer.xamarin.com/api/type/Android.App.Service/)
- [Android.App.Service.StartForegrond](https://developer.xamarin.com/api/member/Android.App.Service.StartForeground/p/System.Int32/Android.App.Notification/)
- [Lokale Benachrichtigungen](~/android/app-fundamentals/notifications/local-notifications.md)
- [ForegroundServiceDemo (Beispiel)](https://developer.xamarin.com/samples/monodroid/ApplicationFundamentals/ServiceSamples/ForegroundServiceDemo/)
