---
title: Android-Auftragsplaner
description: In diesem Leitfaden wird erläutert, wie Hintergrundarbeit mit der Android-Auftragsplaner-API geplant wird.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/19/2018
ms.openlocfilehash: 10d2ae6ac35f02d75ef6e04a0531ec3f5dafd668
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/13/2020
ms.locfileid: "76940820"
---
# <a name="android-job-scheduler"></a>Android-Auftragsplaner

_In diesem Leitfaden wird erläutert, wie Hintergrundarbeit mit der Android-Auftragsplaner-API geplant wird. Diese API ist auf Android-Geräten mit Android 5.0 (API-Ebene 21) und höher verfügbar._

## <a name="overview"></a>Übersicht 

Eine der besten Möglichkeiten, die Reaktionsfähigkeit einer Android-Anwendung für einen Benutzer aufrechtzuerhalten, besteht darin, komplexe oder zeitintensive Arbeit im Hintergrund auszuführen. Allerdings ist es wichtig, dass sich Hintergrundarbeit nicht negativ auf die Erfahrung auswirkt, die der Benutzer mit dem Gerät macht. 

Eine Hintergrundaufgabe könnte beispielsweise alle drei oder vier Minuten eine Website abfragen, um auf Änderungen an einem bestimmten Dataset zu prüfen. Dies scheint harmlos zu sein, würde jedoch eine katastrophale Auswirkung auf die Akkulaufzeit haben. Die Anwendung wird das Gerät wiederholt reaktivieren, die CPU auf einen höheren Energiezustand bringen, die Sendegeräte einschalten, die Netzwerkanforderungen vornehmen und dann die Ergebnisse verarbeiten. Es wird noch schlimmer, da das Gerät nicht sofort ausgeschaltet und in den stromsparenden Leerlaufzustand zurückversetzt wird. Eine schlecht geplante Hintergrundarbeit kann unbeabsichtigterweise bewirken, dass das Gerät in einem Zustand mit unnötigem und übermäßigem Energiebedarf verbleibt. Diese scheinbar unschuldige Aktivität (das zyklische Abrufen einer Website) führt dazu, dass das Gerät in relativ kurzer Zeit unbrauchbar wird.

Android stellt die folgenden APIs bereit, die das Ausführen von Arbeit im Hintergrund unterstützen, aber diese allein reichen nicht für eine intelligente Auftragsplanung aus. 

- **[Intent-Dienste](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Intent-Dienste eignen sich hervorragend zum Ausführen der Arbeit, aber sie bieten keine Möglichkeit, die Arbeit zu planen.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; diese APIs ermöglichen nur das Planen von Arbeit, bieten aber keine Möglichkeit, die Arbeit tatsächlich auszuführen. Außerdem unterstützt AlarmManager nur zeitbasierte Einschränkungen, was bedeutet, dass ein Alarm zu einem bestimmten Zeitpunkt oder nach Ablauf einer bestimmten Zeitspanne ausgelöst wird. 
- **[Broadcast Receiver](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Über eine Android-App können Broadcast Receiver so eingerichtet werden, dass sie Arbeit als Reaktion auf systemweite Ereignisse oder Intent-Objekte ausführen. Mit Broadcast Receivern kann jedoch nicht gesteuert werden, wann der jeweilige Auftrag ausgeführt werden soll. Änderungen im Android-Betriebssystem beschränken außerdem, wann Broadcast Receiver funktionieren oder auf welche Arten von Arbeit sie reagieren können. 

Es gibt zwei Hauptaspekte für ein effizientes Ausführen von Hintergrundarbeit (manchmal auch als _Hintergrundauftrag_ oder _Auftrag_ bezeichnet):

1. **Intelligentes Planen der Arbeit** &ndash; beachten Sie unbedingt, dass eine Anwendung Arbeit im Hintergrund möglichst reibungslos ausführt. Im Idealfall sollte die Anwendung das Ausführen eines Auftrags nicht anfordern. Stattdessen sollte die Anwendung Bedingungen angeben, die erfüllt sein müssen, damit der Auftrag ausgeführt werden kann, und dann diesen Auftrag mit dem Betriebssystem abstimmen, das die Arbeit ausführt, wenn die Bedingungen erfüllt sind. Dadurch kann Android den Auftrag so ausführen, dass eine maximale Effizienz auf dem Gerät sichergestellt ist. Beispielsweise können Netzwerkanforderungen in einem Batch zusammengefasst werden, damit alle gleichzeitig ausgeführt werden und so der netzwerkbedingte Mehraufwand (Overhead) minimiert wird.
2. **Kapseln der Arbeit** &ndash; Der Code zum Ausführen der Hintergrundarbeit sollte in einer diskreten Komponente gekapselt sein, die unabhängig von der Benutzeroberfläche ausgeführt werden kann und relativ einfach neu zu planen ist, wenn die Arbeit aus irgendeinem Grund nicht vollständig ausgeführt werden kann.

Der Android-Auftragsplaner (Job Scheduler) ist ein Framework, das in das Android-Betriebssystem integriert ist und eine flexible API bereitstellt, um das Planen von Hintergrundarbeit zu vereinfachen.  Der Android-Auftragsplaner besteht aus den folgenden Typen:

- `Android.App.Job.JobScheduler` ist ein Systemdienst, über den Aufträge im Namen einer Android-Anwendung geplant, ausgeführt und ggf. abgebrochen werden.
- `Android.App.Job.JobService` ist eine abstrakte Klasse, die um die Logik erweitert werden muss, gemäß der der Auftrag im Hauptthread der Anwendung ausgeführt wird. Dies bedeutet, dass das jeweilige `JobService`-Objekt dafür verantwortlich ist, wie die Arbeit asynchron auszuführen ist.
- Ein `Android.App.Job.JobInfo`-Objekt enthält die Kriterien, über die für Android gesteuert wird, wann es den Auftrag ausführen soll.

Um Arbeit mit dem Android-Auftragsplaner planen zu können, muss der Code einer Xamarin.Android-Anwendung in einer Klasse gekapselt sein, die die `JobService`-Klasse erweitert. `JobService` hat drei Lebenszyklusmethoden, die während der Lebensdauer des Auftrags aufgerufen werden können:

- **bool OnStartJob(JobParameters-Parameter)** &ndash; Diese Methode wird vom `JobScheduler`-Dienst aufgerufen, um Arbeit auszuführen, und wird im Hauptthread der Anwendung ausgeführt. Das `JobService`-Objekt ist dafür zuständig, die Arbeit asynchron auszuführen und `true` zurückzugeben, wenn noch Arbeit verblieben ist, oder `false`, wenn die Arbeit erledigt ist.
    
    Wenn der `JobScheduler`-Dienst diese Methode aufruft, fordert er bei Android eine Aufwachsperre an, die für die Dauer des Auftrags beibehalten wird. Wenn der Auftrag abgeschlossen ist, muss das `JobService`-Objekt dies dem `JobScheduler`-Dienst mitteilen, indem es die `JobFinished`-Methode (nachstehend beschrieben) aufruft.

- **JobFinished(JobParameters-Parameter, bool needsReschedule)** &ndash; Diese Methode muss vom `JobService`-Objekt aufgerufen werden, um dem `JobScheduler`-Dienst mitzuteilen, dass die Arbeit abgeschlossen ist. Wird `JobFinished` nicht aufgerufen, entfernt der `JobScheduler`-Dienst die Aufwachsperre nicht, wodurch eine unnötige Akkuentleerung verursacht wird. 

- **bool OnStopJob(JobParameters-Parameter)** &ndash; Diese Methode wird aufgerufen, wenn der Auftrag vorzeitig von Android beendet wird. Sie sollte `true` zurückgeben, wenn der Auftrag anhand der Wiederholungskriterien neu geplant werden muss (weiter unten ausführlicher erläutert).

Es können _Einschränkungen_ oder _Trigger_ angegeben werden, mit denen gesteuert wird, wann ein Auftrag ausgeführt werden kann oder sollte. Beispielsweise ist es möglich, einen Auftrag so einzuschränken, dass er nur ausgeführt wird, wenn das Gerät geladen wird, oder einen Auftrag zu starten, wenn ein Foto gemacht wird.

In diesem Leitfaden wird ausführlich erläutert, wie eine `JobService`-Klasse implementiert und mit dem `JobScheduler`-Dienst geplant wird.

## <a name="requirements"></a>Anforderungen

Der Android-Auftragsplaner erfordert Android-API-Ebene 21 (Android 5.0) oder höher. 

## <a name="using-the-android-job-scheduler"></a>Verwenden des Android-Auftragsplaners (Job Scheduler)

Es gibt drei Schritte für die Verwendung der Android JobScheduler-API:

1. Implementieren Sie einen „JobService“-Typ, um die Arbeit zu kapseln.
2. Verwenden Sie ein `JobInfo.Builder`-Objekt, um das `JobInfo`-Objekt zu erstellen, das die Kriterien enthält, gemäß denen der `JobScheduler`-Dienst den Auftrag ausführt. 
3. Planen Sie den Auftrag mit `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implementieren eines „JobService“-Typs

Alle Aufgaben, die von der Android-Auftragsplaner-Bibliothek ausgeführt werden, müssen in einem Typ ausgeführt werden, der die abstrakte `Android.App.Job.JobService`-Klasse erweitert. Das Erstellen eines `JobService`-Objekts ist dem Erstellen eines `Service`-Objekts mit dem Android-Framework sehr ähnlich: 

1. Erweitern Sie die `JobService`-Klasse.
2. Ergänzen Sie die Unterklasse mit dem `ServiceAttribute`, und legen Sie den `Name`-Parameter auf eine Zeichenfolge fest, die aus dem Paketnamen und dem Namen der Klasse besteht (siehe folgendes Beispiel).
3. Legen Sie die `Permission`-Eigenschaft von `ServiceAttribute` auf die Zeichenfolge `android.permission.BIND_JOB_SERVICE` fest.
4. Überschreiben Sie die `OnStartJob`-Methode, und fügen Sie den Code hinzu, mit dem die Arbeit ausgeführt wird. Diese Methode wird von Android auf im Hauptthread der Anwendung aufgerufen, um den Auftrag auszuführen. Arbeit, die länger dauert als ein paar Millisekunden, sollte in einem Thread ausgeführt werden, um ein Blockieren der Anwendung zu vermeiden.
5. Wenn die Arbeit erledigt ist, muss im `JobService`-Objekt die `JobFinished`-Methode aufgerufen werden. Diese Methode gibt an, wie `JobService` dem `JobScheduler`-Dienst mitteilt, dass die Arbeit erledigt ist. Nicht ordnungsgemäßes Aufrufen von `JobFinished` führt dazu, dass im `JobService`-Objekt unnötige Anforderungen an das Gerät ausgelöst werden, wodurch die Akkulaufzeit verkürzt wird. 
6. Es empfiehlt sich, auch die `OnStopJob`-Methode zu überschreiben. Diese Methode wird von Android aufgerufen, wenn der Auftrag beendet wird, bevor er abgeschlossen ist, und bietet dem `JobService`-Objekt die Möglichkeit, sämtliche Ressourcen ordnungsgemäß freizugeben. Diese Methode sollte `true` zurückgeben, wenn es erforderlich ist, den Auftrag neu zu planen, oder `false`, wenn es nicht wünschenswert ist, den Auftrag erneut auszuführen.

Der folgende Code ist ein Beispiel für das einfachste `JobService`-Objekt für eine Anwendung, wobei die TPL verwendet wird, um einige Arbeit asynchron auszuführen:

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Erstellen eines „JobInfo“-Objekts zum Planen eines Auftrags

Xamarin.Android-Anwendungen instanziieren ein `JobService`-Objekt nicht direkt, sondern übergeben ein `JobInfo`-Objekt an den `JobScheduler`-Dienst. Der `JobScheduler`-Dienst instanziiert das angeforderte `JobService`-Objekt, wozu das Planen und Ausführen des `JobService`-Objekts entsprechend den Metadaten im `JobInfo`-Objekt vorgenommen werden. Ein `JobInfo`-Objekt muss die folgenden Informationen enthalten:

- **JobId** &ndash; Dies ist ein `int`-Wert, der dazu verwendet wird, einen Auftrag für den `JobScheduler`-Dienst zu kennzeichnen. Durch erneute Verwendung dieses Werts werden alle vorhandenen Aufträge aktualisiert. Der Wert muss für die Anwendung eindeutig sein. 
- **JobService** &ndash; Dieser Parameter ist ein `ComponentName`, der explizit den Typ kennzeichnet, den der `JobScheduler`-Dienst verwenden soll, um einen Auftrag auszuführen. 

Diese Erweiterungsmethode veranschaulicht, wie ein `JobInfo.Builder`-Objekt mit einem Android-`Context`, z. B. einer Aktivität, erstellt wird:

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

Eine leistungsstarke Funktion des Android-Auftragsplaners ist die Möglichkeit, zu steuern, wann ein Auftrag ausgeführt wird oder unter welchen Bedingungen ein Auftrag ausgeführt werden kann. In der folgenden Tabelle sind einige der Methoden von `JobInfo.Builder` beschrieben, mit denen sich in einer App beeinflussen lässt, wann ein Auftrag ausgeführt werden kann:  

|  Methode | Beschreibung   |
|---|---|
| `SetMinimumLatency`  | Gibt eine Verzögerung (in Millisekunden) an, die berücksichtigt werden soll, bevor ein Auftrag ausgeführt wird. |
| `SetOverridingDeadline`  | Deklariert, dass der Auftrag ausgeführt sein muss, bevor diese Zeit (in Millisekunden) abgelaufen ist. |
| `SetRequiredNetworkType`  | Gibt die Netzwerkanforderungen für einen Auftrag an. |
| `SetRequiresBatteryNotLow` | Der Auftrag kann nur ausgeführt werden, wenn das Gerät keine „geringe Akkukapazität“-Warnung anzeigt. |
| `SetRequiresCharging` | Der Auftrag kann nur ausgeführt werden, wenn der Akku geladen wird. |
| `SetDeviceIdle` | Der Auftrag wird ausgeführt, wenn das Gerät ausgelastet ist. |
| `SetPeriodic` | Gibt an, dass der Auftrag regelmäßig ausgeführt werden soll. |
| `SetPersisted` | Der Auftrag soll über Geräteneustarts hinweg erhalten bleiben. | 

Die `SetBackoffCriteria`-Methode bietet gewisse Anleitung dazu, wie lange der `JobScheduler`-Dienst warten soll, bevor versucht wird, einen Auftrag erneut auszuführen. Es gibt zwei Teile der Backoffkriterien: eine Verzögerung in Millisekunden (Standardwert von 30 Sekunden) und den Typ des zu verwendenden Backoffwerts (manchmal auch als _Backoffrichtlinie_ oder _Wiederholungsrichtlinie_ bezeichnet). Die beiden Richtlinien sind in der `Android.App.Job.BackoffPolicy`-Aufzählung gekapselt:

- `BackoffPolicy.Exponential` &ndash; Eine exponentielle Backoffrichtlinie erhöht den anfänglichen Backoffwert exponentiell nach jedem Fehler. Wenn ein Auftrag erstmals fehlschlägt, wartet die Bibliothek solange mit dem erneuten Planen des Auftrags, bis das angegebene anfängliche Intervall abgelaufen ist. Beispiel: 30 Sekunden. Wenn der Auftrag zum zweiten Mal fehlschlägt, wartet die Bibliothek mindestens 60 Sekunden, bevor sie versucht, den Auftrag auszuführen. Nach dem dritten fehlgeschlagenen Versuch wartet die Bibliothek 120 Sekunden usw. Dies ist der Standardwert.
- `BackoffPolicy.Linear` &ndash; Diese Strategie ist ein lineares Backoff, dass der Auftrag in festgelegten Intervallen neu für Ausführen geplant werden soll (bis er erfolgreich ausgeführt wird). Lineares Backoff eignet sich am besten für Arbeit, die so bald wie möglich abgeschlossen werden muss, oder für Probleme, die sich schnell von selbst lösen. 

Weitere Informationen zum Erstellen eines `JobInfo`-Objekts finden Sie in der [Google-Dokumentation für die `JobInfo.Builder`-Klasse](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Übergeben von Parametern an einen Auftrag über ein „JobInfo“-Objekt

Parameter werden an einen Auftrag übergeben, indem ein `PersistableBundle`-Objekt erstellt wird, das zusammen mit der `Job.Builder.SetExtras`-Methode übergeben wird:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

Der Zugriff auf das `PersistableBundle`-Objekt erfolgt aus der `Android.App.Job.JobParameters.Extras`-Eigenschaft in der `OnStartJob`-Methode eines `JobService`-Objekts:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Um einen Auftrag zu planen, erhält eine Xamarin.Android-Anwendung einen Verweis auf den `JobScheduler`-Systemdienst und ruft die Anwendung die `JobScheduler.Schedule`-Methode mit dem `JobInfo`-Objekt auf, das im vorherigen Schritt erstellt wurde. `JobScheduler.Schedule` gibt sofort einen von zwei ganzzahligen Werten zurück:

- **JobScheduler.ResultSuccess** &ndash; Der Auftrag wurde erfolgreich geplant. 
- **JobScheduler.ResultFailure** &ndash; Der Auftrag konnte nicht geplant werden. Dies wird in der Regel durch widersprüchliche `JobInfo`-Parameter verursacht.

Dieser Code ist ein Beispiel, wie ein Auftrag geplant und der Benutzer über die Ergebnisse des Planungsversuchs benachrichtigt wird:

```csharp
var jobScheduler = (JobScheduler)GetSystemService(JobSchedulerService);
var scheduleResult = jobScheduler.Schedule(jobInfo);

if (JobScheduler.ResultSuccess == scheduleResult)
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_success, Snackbar.LengthShort);
    snackBar.Show();
}
else
{
    var snackBar = Snackbar.Make(FindViewById(Android.Resource.Id.Content), Resource.String.jobscheduled_failure, Snackbar.LengthShort);
    snackBar.Show();
}
```

### <a name="cancelling-a-job"></a>Abbrechen eines Auftrags

Es ist möglich, alle geplanten Aufträge oder nur einen einzelnen Auftrag mit der `JobsScheduler.CancelAll()`-Methode oder der `JobScheduler.Cancel(jobId)`-Methode abzubrechen:

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wird erläutert, wie Sie den Android-Auftragsplaner verwenden, um Arbeit intelligent im Hintergrund auszuführen. Es wird erläutert, wie die auszuführende Arbeit als `JobService`-Objekt gekapselt und wie der `JobScheduler`-Dienst verwendet wird, um diese Arbeit zu planen, indem die Kriterien mit einem `JobTrigger` angegeben werden, und wie Fehler mit einer `RetryStrategy` verarbeitet werden sollten.

## <a name="related-links"></a>Verwandte Links

- [Intelligente Auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler-API-Referenz](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Scheduling jobs like a pro with JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android Battery and Memory Optimizations – Google I/O 2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler – René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
