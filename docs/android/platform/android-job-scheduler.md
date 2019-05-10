---
title: Android-Auftragsplaner
description: Dieser Leitfaden erläutert die Vorgehensweise beim Planen der Verarbeitung im Hintergrund mithilfe der Android-Auftrag-Scheduler-API.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: d69a96ee55b09ef9fcf1485ec34d986dd40e7662
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977973"
---
# <a name="android-job-scheduler"></a>Android-Auftragsplaner

_Dieser Leitfaden erläutert, wie zum Planen der Verarbeitung im Hintergrund mithilfe der Android-Auftrag Scheduler-API, die auf Android-Geräten mit Android 5.0 (API Level 21) verfügbar ist und höher._


## <a name="overview"></a>Übersicht 

Eine der besten Möglichkeiten, eine Android-Anwendung, die dem Benutzer Reaktionsfähigkeit wird sichergestellt, dass komplexe oder lang ausgeführte Arbeit im Hintergrund ausgeführt wird. Allerdings ist es wichtig, dass die Verarbeitung im Hintergrund nicht negativ auf die benutzererfahrung mit dem Gerät beeinträchtigt wird. 

Beispielsweise kann ein Hintergrundauftrag eine Website alle drei oder vier Minuten an die Abfrage für Änderungen an ein bestimmtes Dataset abrufen. Dies scheint harmlos, jedoch eine verheerende Auswirkungen auf die Akkulaufzeit werden müssten. Die Anwendung wird wiederholt reaktiviert, das Gerät erhöhen die CPU auf einer höheren Energiezustand aktiviert, Einschalten auf den Funkempfang gestattet, stellen die netzwerkanforderungen, und klicken Sie dann die Ergebnisse verarbeiten. Es verschlimmert sich, da das Gerät wird nicht sofort schließen und zurück in den energiesparenden Leerlauf. Schlecht geplanten Hintergrundarbeit möglicherweise versehentlich des Geräts in einem Zustand mit unnötiges und übermäßiges Energieprofil gewährleisten. Diese scheinbar harmlose Aktivität (eine Website abrufen), wird das Gerät in relativ kurzer Zeit unbrauchbar werden.

Android bietet die folgenden APIs unterstützen Sie beim Arbeiten im Hintergrund ausführen, aber selbst keine ausreichend intelligent auftragsplanung. 

* **[Intent-Dienste](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; Intent-Dienste eignen sich hervorragend für die Arbeit auszuführen, jedoch keine Möglichkeit zum Planen der Arbeit bieten.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; diese APIs können nur die Arbeit geplant werden, jedoch bietet keine Möglichkeit, die die Arbeit zu erledigen. Außerdem ermöglicht der AlarmManager nur zeitbasierte Einschränkungen, was bedeutet einen Alarm auslösen, zu einem bestimmten Zeitpunkt oder nach eine bestimmten Zeitspanne verstrichen ist. 
* **[Broadcast Receiver](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; ein Android-app kann zum Ausführen von Aufgaben als Reaktion auf eine systemweite Ereignisse oder Intents übertragungsempfänger einrichten. Übertragungsempfänger bieten jedoch keine keine Kontrolle über, wenn der Auftrag ausgeführt werden soll. Änderungen in der Android-Betriebssystem schränkt auch wenn übertragungsempfänger funktionieren, oder die Arten von Arbeitsschritten, die sie reagieren können. 

Es gibt zwei wichtige Features, die effiziente Durchführung von Hintergrundarbeit (auch bezeichnet als eine _Hintergrundauftrag_ oder _Auftrag_):

1. **Planen die Arbeit auf intelligente Weise** &ndash; es ist wichtig, wenn eine Anwendung im Hintergrund, die Arbeit ist dies der Fall als ein "guter Bürger" ist. Im Idealfall sollte die Anwendung nicht gefordert wird, dass ein Auftrag ausgeführt werden. Stattdessen sollte die Anwendung Bedingungen angeben, die für die erfüllt sein müssen, wenn der Auftrag ausführen kann, und planen diesen Auftrag mit dem Betriebssystem, das die Arbeit ausgeführt werden, wenn die Bedingungen erfüllt sind. Dadurch können Android zum Ausführen des Auftrags, um sicherzustellen, dass maximale Effizienz bei auf dem Gerät. Beispielsweise können netzwerkanforderungen zusammengefasst werden, führen Sie alle zur gleichen Zeit maximale Mehraufwand bei der Netzwerke nutzen.
2. **Kapseln die Arbeit** &ndash; sollte der Code die Verarbeitung im Hintergrund ausführen gekapselt werden, in eine diskrete Komponente, die ausgeführt werden kann, unabhängig von der Benutzeroberfläche und relativ einfach, plant, wenn die Arbeit nicht abgeschlossen aus irgendeinem Grund.

Die Android-Auftragsplaner ist ein Framework, die direkt in der Android-Betriebssystem, das eine fluent-API zur Vereinfachung der zeitplanung Verarbeitung im Hintergrund bereitstellt.  Die Android-Auftragsplaner besteht aus folgenden Typen:

* Die `Android.App.Job.JobScheduler` ist ein Systemdienst, der zum Planen, ausführen und ggf. Abbrechen, Aufträge im Auftrag von einer Android-Anwendung verwendet wird.
* Ein `Android.App.Job.JobService` ist eine abstrakte Klasse, die mit der Logik, die der Auftrag auf dem Hauptthread der Anwendung ausgeführt wird, erweitert werden muss. Dies bedeutet, dass die `JobService` muss dafür, wie die Arbeit wird asynchron ausgeführt werden.
* Ein `Android.App.Job.JobInfo` -Objekt enthält die Kriterien für die Android-Anleitung, wenn der Auftrag ausgeführt werden soll.

Um mit der Android-Auftragsplaner zu planen, eine Xamarin.Android-Anwendung muss den Code in einer Klasse, die erweitert kapseln die `JobService` Klasse. `JobService` verfügt über drei Methoden, die während der Lebensdauer des Auftrags aufgerufen werden können:

* **"bool" OnStartJob (JobParameters-Parameter)** &ndash; diese Methode wird aufgerufen, indem die `JobScheduler` auszuführende Arbeit, und klicken Sie auf den Hauptthread der Anwendung ausgeführt wird. Es ist Aufgabe von der `JobService` um die Arbeit asynchron auszuführen und `true` liegt arbeiten verbleibenden, oder `false` , wenn die Arbeit erfolgt.
    
    Wenn die `JobScheduler` ruft diese Methode auf, wird anfordern und eine Wakelock von Android für die Dauer des Auftrags beibehalten. Wenn der Auftrag abgeschlossen ist, ist es die Verantwortung für die `JobService` informieren die `JobScheduler` diesen Umstand durch Aufruf der `JobFinished` Methode, die (als Nächstes beschrieben).

* **JobFinished (JobParameters-Parameter, "bool" NeedsReschedule)** &ndash; diese Methode muss aufgerufen werden, indem die `JobService` informieren die `JobScheduler` , die die Arbeit erfolgt. Wenn `JobFinished` nicht aufgerufen wird, die `JobScheduler` der Wakelock, wodurch unnötige akkuentleerung werden nicht entfernt. 

* **"bool" OnStopJob (JobParameters-Parameter)** &ndash; wird aufgerufen, wenn der Auftrag vom Android vorzeitig beendet wird. Es sollte zurückgeben `true` , wenn der Auftrag basierend auf "Wiederholen" (weiter unten ausführlicher erläutert) neu geplant werden soll.

Es ist möglich, geben Sie _Einschränkungen_ oder _Trigger_ wird, die steuern, wann ein Auftrag kann, oder ausgeführt werden soll. Beispielsweise ist es möglich, einen Auftrag zu beschränken, sodass sie nur ausgeführt werden soll, wenn das Gerät geladen wird, oder Starten eines Auftrags aus, wenn ein Bild stammt.

Dieses Handbuch wird erläutert im Detail implementieren eine `JobService` Klasse, und Planen sie mit der `JobScheduler`.

## <a name="requirements"></a>Anforderungen

Die Android-Auftragsplaner benötigt Android-API-Ebene 21 (Android 5.0) oder höher. 

## <a name="using-the-android-job-scheduler"></a>Verwenden die Android-Auftragsplaner

Es gibt drei Schritte für die Verwendung der Android-API-JobScheduler:

1. Implementieren Sie einen JobService-Typ, um die Arbeit zu kapseln.
2. Verwenden einer `JobInfo.Builder` zu erstellenden Objekts der `JobInfo` Objekt, das die Kriterien zum aufnehmen, wird die `JobScheduler` zum Ausführen des Auftrags. 
3. Planen Sie den Auftrag mithilfe `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implementieren einer JobService

Alle Aufgaben, die von der Android-Auftragsplaner-Bibliothek muss ausgeführt werden, in einem Typ, der erweitert die `Android.App.Job.JobService` abstrakte Klasse. Erstellen einer `JobService` ähnelt sehr dem Erstellen einer `Service` mit dem Android-Framework: 

1. Erweitern Sie die `JobService` Klasse.
2. Ergänzen Sie die Unterklasse mit der `ServiceAttribute` und legen Sie die `Name` Parameter, um eine Zeichenfolge, die aus dem Paketnamen und den Namen der Klasse besteht (siehe folgendes Beispiel).
3. Legen Sie die `Permission` Eigenschaft für die `ServiceAttribute` auf die Zeichenfolge `android.permission.BIND_JOB_SERVICE`.
4. Überschreiben der `OnStartJob` -Methode Code hinzufügen, um die Arbeit zu erledigen. Android wird diese Methode auf dem Hauptthread der Anwendung für die auftragsausführung aufgerufen. Aufgaben, die länger dauern wird, dass ein paar Millisekunden in einem Thread aus, um zu vermeiden, blockiert die Anwendung ausgeführt werden soll.
5. Wenn die Arbeit abgeschlossen ist, die `JobService` aufrufen, müssen die `JobFinished` Methode. Diese Methode ist wie `JobService` weist die `JobScheduler` , dass die Arbeit abgeschlossen ist. Fehler beim Aufrufen `JobFinished` führt dazu, dass die `JobService` unnötige Anforderungen einfügen, auf dem Gerät die Akkulaufzeit zu verkürzen. 
6. Es ist eine gute Idee, außer Kraft setzen der `OnStopJob` Methode. Diese Methode wird von Android aufgerufen, wenn der Auftrag beendet ist, bevor er abgeschlossen ist, und bietet die `JobService` Möglichkeit, alle Ressourcen ordnungsgemäß freigegeben. Diese Methode sollte zurückgeben `true` ist es erforderlich, um den Auftrag neu zu planen oder `false` ist dies nicht erwünscht ist, um den Auftrag erneut auszuführen.
   

Der folgende Code ist ein Beispiel für den einfachsten `JobService` verwenden Sie für eine Anwendung, die TPL können einige Vorgänge asynchron:

```csharp
[Service(Name = "com.xamarin.samples.downloadscheduler.DownloadJob", 
         Permission = "android.permission.BIND_JOB_SERVICE")]
public class DownloadJob : JobService
{
    public override bool OnStartJob(JobParameters jobParams)
    {            
        Task.Run(() =>
        {
            // Work is happening asynchronously
                      
            // Have to tell the JobScheduler the work is done. 
            JobFinished(jobParams, false);
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(JobParameters jobParams)
    {
        // we don't want to reschedule the job if it is stopped or cancelled.
        return false; 
    }
}
```

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Erstellen eine JobInfo um einen Auftrag zu planen.

Xamarin.Android-Anwendungen nicht instanziiert werden eine `JobService` direkt, stattdessen diese übergibt eine `JobInfo` -Objekt an die `JobScheduler`. Die `JobScheduler` instanziiert die angeforderte `JobService` -Objekt, Planung und Ausführung der `JobService` entsprechend den Metadaten in die `JobInfo`. Ein `JobInfo` Objekt muss die folgende Informationen enthalten:

* **"JobID"** &ndash; Dies ist ein `int` -Wert, der verwendet wird, um einen Auftrag zum Identifizieren der `JobScheduler`. Wiederverwenden von dieser Wert aktualisiert vorhandene Aufträge. Der Wert muss für die Anwendung eindeutig sein. 
* **JobService** &ndash; dieser Parameter ist ein `ComponentName` identifiziert, die explizit den Typ, der die `JobScheduler` sollten verwenden, um einen Auftrag auszuführen. 
  

Diese Erweiterungsmethode veranschaulicht, wie eine `JobInfo.Builder` mit Android `Context`, z. B. eine Aktivität:

```csharp
public static class JobSchedulerHelpers
{
    public static JobInfo.Builder CreateJobBuilderUsingJobId<T>(this Context context, int jobId) where T:JobService
    {
        var javaClass = Java.Lang.Class.FromType(typeof(T));
        var componentName = new ComponentName(context, javaClass);
        return new JobInfo.Builder(jobId, componentName);
    }
}

// Sample usage - creates a JobBuilder for a DownloadJob and sets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creates a JobInfo object.
```

Eine leistungsstarke Funktion von der Android-Auftragsplaner ist die Möglichkeit, die steuern, wann ein Auftrag ausgeführt wird, oder die Bedingungen, eines Auftrags ausführen kann. Die folgende Tabelle beschreibt einige der Methoden auf `JobInfo.Builder` , mit denen eine app, die beeinflussen, wenn ein Auftrag ausgeführt werden kann:  


|  Methode | Beschreibung   |
|---|---|
| `SetMinimumLatency`  | Gibt an, dass eine Verzögerung (in Millisekunden), die vor einem Auftrag beachtet werden soll, ausgeführt wird. |
| `SetOverridingDeadline`  | Deklariert, dass der Auftrag muss ausgeführt werden, bevor diese Zeit (in Millisekunden) verstrichen ist. |
| `SetRequiredNetworkType`  | Gibt die netzwerkanforderungen für einen Auftrag an. |
| `SetRequiresBatteryNotLow` | Der Auftrag kann nur ausgeführt, wenn das Gerät eine Warnung "Akku" nicht für den Benutzer angezeigt wird. |
| `SetRequiresCharging` | Der Auftrag kann nur ausgeführt, wenn der Akku geladen wird. |
| `SetDeviceIdle` | Der Auftrag ausgeführt wird, wenn das Gerät ausgelastet ist. |
| `SetPeriodic` | Gibt an, dass der Auftrag regelmäßig ausgeführt werden soll. |
| `SetPersisted` | Der Auftrag sollte Perisist Gerät nach einem Neustart beibehalten. | 


Die `SetBackoffCriteria` enthält hilfreiche Informationen dazu, wie lange der `JobScheduler` warten soll, bevor Sie versuchen, einen Auftrag erneut ausführen. Umfasst zwei Teile, die den Kriterien Backoff: eine Verzögerung in Millisekunden (Standardwert von 30 Sekunden) und den Typ der Backoff, die verwendet werden soll (auch bezeichnet als die _backoffrichtlinie_ oder _wiederholungsrichtlinie_) . Die beiden Richtlinien in gekapselt werden die `Android.App.Job.BackoffPolicy` Enumeration:

* `BackoffPolicy.Exponential` &ndash; Eine exponentielle backoffrichtlinie wird den Wert für die anfängliche Wartezeit nach jedem Fehler exponentiell erhöhen. Ein Auftrag fehlschlägt, das zum ersten Mal wird die Bibliothek das anfängliche Intervall warten, das angegeben wird, bevor Sie durch das erneute Planen des Auftrags – Beispiel für 30 Sekunden. Der zweite Start des Auftrags ein Fehler auftritt, wird die Bibliothek mindestens 60 Sekunden, bevor Sie versuchen, die zum Ausführen des Auftrags gewartet. Nach der dritten Versuch, wird die Bibliothek 120 Sekunden warten, und so weiter. Dies ist der Standardwert.
* `BackoffPolicy.Linear` &ndash; Diese Strategie ist eine lineare Backoff, die der Auftrag zu neu geplant werden, sollten in festgelegten Intervallen ausgeführt werden, (bis er erfolgreich abgeschlossen wurde). Lineare Backoff eignet sich am besten für die Arbeit, die so bald wie möglich ausgeführt werden muss oder für Probleme, die schnell selbst aufgelöst werden. 

Ausführliche Informationen zum Erstellen einer `JobInfo` -Objekt, lesen Sie [Google Dokumentation für die `JobInfo.Builder` Klasse](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Übergeben von Parametern zu einem Auftrag über die JobInfo

Parameter werden an einen Auftrag übergeben, durch das Erstellen einer `PersistableBundle` , übergeben wird, zusammen mit den `Job.Builder.SetExtras` Methode:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

Die `PersistableBundle` erfolgt über die `Android.App.Job.JobParameters.Extras` -Eigenschaft in der `OnStartJob` Methode eine `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Um einen Auftrag zu planen, eine Xamarin.Android-Anwendung Abrufen eines Verweises auf die `JobScheduler` -Systemdienst und der Aufruf der `JobScheduler.Schedule` -Methode mit der `JobInfo` -Objekt, das im vorherigen Schritt erstellt wurde. `JobScheduler.Schedule` mit einem von zwei ganzzahligen Werten wird sofort zurückgegeben werden:

* **JobScheduler.ResultSuccess** &ndash; der Auftrag wurde erfolgreich geplant. 
* **JobScheduler.ResultFailure** &ndash; der Auftrag konnte nicht geplant werden. Dies wird normalerweise verursacht durch in Konflikt stehende `JobInfo` Parameter.

Dieser Code ist ein Beispiel für einen Auftrag planen und benachrichtigt den Benutzer über die Ergebnisse des Versuchs planen:

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
}
else
{
    Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
}
```
 
### <a name="cancelling-a-job"></a>Abbrechen eines Auftrags

Es ist möglich, um alle Aufträge abzubrechen, die geplant wurden, oder nur einen einzelnen Auftrag, indem die `JobsScheduler.CancelAll()` Methode oder der `JobScheduler.Cancel(jobId)` Methode:

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, wie Sie mit der Android-Auftragsplaner auf intelligente Weise Aufgaben im Hintergrund. Er erläutert, wie die Arbeit als zu kapseln eine `JobService` und zum Verwenden der `JobScheduler` so planen Sie die Arbeit, Angeben der Kriterien mit einer `JobTrigger` und wie Fehler mit behandelt werden sollen eine `RetryStrategy`.

## <a name="related-links"></a>Verwandte Links

- [Intelligente auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler-API-Referenz](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Planen von Aufträgen wie die Profis mit JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android Akku und Arbeitsspeicheroptimierung – Google e/a-2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
