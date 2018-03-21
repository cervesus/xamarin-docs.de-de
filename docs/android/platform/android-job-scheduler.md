---
title: Android Auftragsplaner
description: "Dieses Handbuch erläutert, wie mithilfe der Android-Auftrag-Zeitplan API Verarbeitung im Hintergrund zu planen."
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 03/19/2018
ms.openlocfilehash: dc72b7e4da330185b00541f923d9c4b64b91bc95
ms.sourcegitcommit: d450ae06065d8f8c80f3588bc5a614cfd97b5a67
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2018
---
# <a name="android-job-scheduler"></a>Android Auftragsplaner

_Dieses Handbuch erläutert, wie beim Planen der Verarbeitung im Hintergrund mithilfe der Android Auftrag Zeitplanungsmodul-API, die auf Android-Geräten mit Android 5.0.x (API-Ebene 21) verfügbar ist und höher._


## <a name="overview"></a>Übersicht 

Eine der geeignetsten Methoden eine Android-Anwendung für den Benutzer reaktionsfähig wird sichergestellt, dass komplexe oder lang ausgeführte Arbeit im Hintergrund ausgeführt wird. Allerdings ist es wichtig, dass die Verarbeitung im Hintergrund nicht negativ auf die benutzerfreundlichkeit mit dem Gerät auswirkt. 

Beispielsweise kann ein Hintergrundauftrag eine Website alle drei oder vier Minuten abzufragende für Änderungen an ein bestimmtes Dataset abrufen. Dies scheint keine Auswirkungen, aber es keine katastrophalen Auswirkungen auf die Akkulaufzeit haben würden. Die Anwendung wird wiederholt Reaktivieren des Geräts, erhöhen die CPU auf einer höheren Energiezustand aktiviert, schalten Sie die Sender, auf stellen die Anforderungen über das Netzwerk, und klicken Sie dann die Ergebnisse verarbeiten. Da das Gerät wird nicht sofort Herunterfahren und zurück in den Leerlaufzustand LP-herankommt schlechter. Schlecht geplanten Hintergrundarbeit möglicherweise versehentlich des Geräts in einem Zustand mit unnötiges und übermäßiges Stromverbrauch gewährleisten. Diese scheinbar harmlose Aktivität (eine Website abrufen) wird das Gerät in relativ kurzer Zeit unbrauchbar werden.

Android bietet die folgenden APIs unterstützen Sie beim Arbeiten im Hintergrund ausführen selbst sind allerdings nicht für intelligente auftragsplanung ausreichend. 

* **[Beabsichtigte Services](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; Absicht Services sind hervorragend für die Arbeiten ausführen, jedoch keine Möglichkeit zum Planen der Arbeit angebotenen.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; diese APIs ermöglichen nur Arbeit geplant werden, jedoch bietet keine Möglichkeit, die Aufgaben tatsächlich auszuführen. Außerdem ermöglicht das AlarmManager nur zeitbasierter Einschränkungen, d. h. ein Warnsignal auslösen, zu einem bestimmten Zeitpunkt oder nach ein bestimmten Zeitraum verstrichen ist. 
* **[Broadcast Empfänger](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; ein Android-app kann broadcast Empfänger zum Ausführen von Aktionen als Reaktion auf eine systemweite Ereignisse oder Intents einrichten. Broadcast Empfänger stellen jedoch keine Kontrolle über die Ausführung des Auftrags. Änderungen in der Android-Betriebssystem werden eingeschränkt, auch wenn broadcast Empfänger funktioniert oder die Arbeitsschritte, auf die reagiert werden kann. 

Es sind zwei Schlüsselfunktionen effiziente Verarbeitung im Hintergrund ausführen (auch bezeichnet als eine _Hintergrundauftrag_ oder ein _Auftrag_):

1. **Intelligent Planen der Arbeit** &ndash; sind wichtig, wenn eine Anwendung im Hintergrund, die auf diese Weise ist dies der Fall als eine gute Bürger ist. Im Idealfall sollte die Anwendung nicht gefordert wird, dass ein Auftrag ausgeführt werden. Stattdessen sollte die Anwendung Bedingungen angeben, die für die erfüllt sein müssen, wenn der Auftrag ausführen kann, und Planen Sie diesen Auftrag mit dem Betriebssystem, das die Arbeit ausgeführt wird, wenn die Bedingungen erfüllt sind. Dadurch können Android zum Ausführen des Auftrags, um sicherzustellen, dass maximalen Effizienz, auf dem Gerät. Anforderungen über das Netzwerk können z. B. zusammengefasst werden, führen Sie alle zur selben Zeit maximale Mehraufwand bei der Platzierung Netzwerk nutzen.
2. **Kapselt die Arbeit** &ndash; sollte der Code die Verarbeitung im Hintergrund ausführen gekapselt werden, in einer Komponente, die ausgeführt werden kann, unabhängig von der Benutzeroberfläche und relativ einfach erneut geplant, wenn die Arbeit nicht abgeschlossen aus irgendeinem Grund.

Der Auftragsplaner Android ist ein Framework integriert Android-Betriebssysteme, die eine fluent-API, um Planungsdaten Verarbeitung im Hintergrund zu vereinfachen.  Android Auftragsplaner umfasst die folgenden Typen:

* Die `Android.App.Job.JobScheduler` ist ein Systemdienst, der zum Planen und führen Sie bei Bedarf "Abbrechen", Aufträge im Auftrag einer Android-Anwendung verwendet wird.
* Ein `Android.App.Job.JobService` ist eine abstrakte Klasse, die mit der Logik erweitert werden muss, die der Auftrag auf den Hauptthread der Anwendung ausgeführt wird. Dies bedeutet, dass die `JobService` ist verantwortlich für die Arbeit an asynchron ausgeführt werden.
* Ein `Android.App.Job.JobInfo` -Objekt enthält die Kriterien für Android geführt, wenn der Auftrag ausgeführt werden soll.

Informationen zum Planen von Arbeit mit der Android Auftragsplaner muss eine Anwendung Xamarin.Android den Code in einer Klasse, die erweitert kapseln die `JobService` Klasse. `JobService` verfügt über drei Lifecycle-Methoden, die während der Lebensdauer des Auftrags aufgerufen werden kann:

* **Bool OnStartJob (JobParameters-Parameter)** &ndash; diese Methode wird aufgerufen, indem die `JobScheduler` auszuführenden Arbeiten und auf den Hauptthread der Anwendung ausgeführt wird. Es liegt in der Verantwortung des der `JobService` , die Aufgaben asynchron auszuführen und `true` Wenn Erfolg verbleiben, oder `false` , wenn die Arbeit abgeschlossen ist.
    
    Wenn die `JobScheduler` ruft diese Methode wird anfordern und Beibehalten einer Wakelock mit Android für die Dauer des Auftrags. Wenn der Auftrag abgeschlossen ist, ist es die Zuständigkeit für die `JobService` anzuweisen, die `JobScheduler` diesen Umstand durch Aufruf der `JobFinished` Methode (nachfolgend beschrieben).

* **JobFinished (JobParameters-Parameter, Bool NeedsReschedule)** &ndash; diese Methode muss aufgerufen werden, indem Sie die `JobService` anzuweisen, den `JobScheduler` , die die Arbeit durchgeführt wird. Wenn `JobFinished` nicht aufgerufen wird, die `JobScheduler` entfernt nicht die Wakelock verursachen unnötige Akku ausgleichen. 

* **Bool OnStopJob (JobParameters-Parameter)** &ndash; wird aufgerufen, wenn der Auftrag von Android vorzeitig beendet wird. Es sollte zurückgeben `true` , wenn der Auftrag neu geplant werden, sollten basierend auf den Wiederholung Kriterien (wie unten im Detail erläutert).

Es ist möglich, geben Sie _Einschränkungen_ oder _Trigger_ wird, die steuern, wenn ein Auftrag kann, oder ausgeführt werden soll. Beispielsweise ist es möglich, einen Auftrag zu beschränken, damit er nur ausgeführt wird, wenn das Gerät aufgeladen wird oder zum Starten eines Auftrags aus, wenn ein Bild stammt.

Dieser Anleitung wird erläutert ausführlich implementieren eine `JobService` Klasse, und Planen sie mit der `JobScheduler`.

## <a name="requirements"></a>Anforderungen

Der Auftragsplaner Android erfordert, Android-API-Ebene 21 (Android 5.0) oder höher. 

## <a name="using-the-android-job-scheduler"></a>Verwenden der auftragsplanung für Android

Es gibt drei Schritte zur Verwendung der Android JobScheduler-API:

1. Implementieren Sie einen JobService-Typ, um die Arbeit zu kapseln.
2. Verwenden einer `JobInfo.Builder` zu erstellenden Objekts die `JobInfo` -Objekt, das festhält, die Kriterien für die `JobScheduler` zum Ausführen des Auftrags. 
3. Planen Sie den Auftrag mit `JobScheduler.Schedule`.

### <a name="implement-a-jobservice"></a>Implementieren Sie eine JobService

Alle Arbeit, die von der Bibliothek für Android Auftragsplaner muss erfolgen, in einem Typ, der erweitert die `Android.App.Job.JobService` abstrakte Klasse. Erstellen einer `JobService` ähnelt dem Erstellen einer `Service` mit dem Android-Framework: 

1. Erweitern der `JobService` Klasse.
2. Ergänzen Sie die Unterklasse mit der `ServiceAttribute` und legen Sie die `Name` Parameter in eine Zeichenfolge, die aus dem Paketnamen und den Namen der Klasse besteht (Siehe das folgende Beispiel).
3. Legen Sie die `Permission` Eigenschaft auf die `ServiceAttribute` auf die Zeichenfolge `android.permission.BIND_JOB_SERVICE`.
4. Überschreiben Sie die `OnStartJob` -Methode, den Code zum Ausführen der Aktionen hinzufügen. Android wird diese Methode für den Hauptthread der Anwendung zum Ausführen des Auftrags aufgerufen. Arbeit, die länger dauern wird, dass einige Millisekunden in einem Thread, um zu verhindern, blockiert die Anwendung ausgeführt werden soll.
5. Wenn die Arbeit abgeschlossen ist, die `JobService` aufrufen, müssen die `JobFinished` Methode. Diese Methode ist wie `JobService` teilt die `JobScheduler` , dass die Arbeit erforderlich ist. Fehler beim Aufrufen `JobFinished` führt zu den `JobService` unnötige Anforderungen auf dem Gerät die Akkulaufzeit Verkürzung einfügen. 
6. Es ist eine gute Idee, überschreiben Sie auch die `OnStopJob` Methode. Diese Methode wird von Android aufgerufen, wenn der Auftrag heruntergefahren wird, bevor er abgeschlossen ist, und bietet die `JobService` eine Möglichkeit, alle Ressourcen ordnungsgemäß freigegeben. Diese Methode sollte zurückgeben `true` ist es erforderlich, um den Auftrag neu zu planen oder `false` ist er nicht desireable, um den Auftrag erneut auszuführen.
   

Der folgende Code ist ein Beispiel für die am einfachsten `JobService` für eine Anwendung mithilfe der TPL portierungsziel asynchron ausführen:

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Erstellen eine JobInfo zum Planen eines Auftrags

Xamarin.Android Anwendungen nicht instanziiert werden eine `JobService` direkt, stattdessen diese übergibt eine `JobInfo` -Objekt an die `JobScheduler`. Die `JobScheduler` der angeforderte instanziiert dann `JobService` Objekt, das Planung und Ausführung von der `JobService` gemäß der Metadaten in die `JobInfo`. Ein `JobInfo` Objekt muss die folgende Informationen enthalten:

* **"JobID"-Wert** &ndash; Dies ist ein `int` -Wert, der verwendet wird, um einen Auftrag zum Identifizieren der `JobScheduler`. Wiederverwenden von diesem Wert werden alle vorhandenen Aufträge aktualisiert. Der Wert muss für die Anwendung eindeutig sein. 
* **JobService** &ndash; dieser Parameter ist ein `ComponentName` , explizit den Typ identifiziert, die die `JobScheduler` sollte mit dem einen Auftrag ausgeführt. 
  

Diese Erweiterungsmethode veranschaulicht, wie eine `JobInfo.Builder` mit einem Android `Context`, wie etwa eine Aktivität:

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

// Sample usage - creates a JobBuilder for a DownloadJob andsets the Job ID to 1.
var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1);

var jobInfo = jobBuilder.Build();  // creats a JobInfo object.
```

Eine leistungsstarke Funktion von Android Auftragsplaner ist die Möglichkeit, die steuern, wann ein Auftrag ausgeführt wird oder die Bedingungen, die eines Auftrags ausführen kann. Die folgende Tabelle beschreibt einige der Methoden auf `JobInfo.Builder` , mit denen eine app, die beeinflussen, wenn ein Auftrag ausgeführt werden kann:  


|  Methode | Beschreibung   |
|---|---|
| `SetMinimumLatency`  | Gibt an, dass eine Verzögerung (in Millisekunden), die vor einem Auftrag beachtet werden sollten ausgeführt wird. |
| `SetOverridingDeadline`  | Deklariert, dass der Auftrag muss ausgeführt werden, bevor diese Zeit (in Millisekunden) verstrichen ist. |
| `SetRequiredNetworkType`  | Gibt die netzwerkanforderungen für einen Auftrag an. |
| `SetRequiresBatteryNotLow` | Der Auftrag kann nur ausgeführt, wenn das Gerät eine Warnung "niedriger Akkukapazität" nicht an den Benutzer angezeigt wird. |
| `SetRequiresCharging` | Der Auftrag kann nur ausgeführt, wenn die Batterie geladen wird. |
| `SetDeviceIdle` | Der Auftrag wird ausgeführt, wenn das Gerät ausgelastet ist. |
| `SetPeriodic` | Gibt an, dass der Auftrag regelmäßig ausgeführt werden soll. |
| `SetPersisted` | Der Auftrag sollte Perisist Neustart des Geräts. | 


Die `SetBackoffCriteria` enthält hilfreiche Informationen dazu, wie lange der `JobScheduler` warten soll, bevor Sie versuchen, einen Auftrag erneut auszuführen. Es gibt zwei Komponenten zu den Kriterien Backoff: eine Verzögerung in Millisekunden (Standardwert von 30 Sekunden) und den Typ der Backoff, die verwendet werden soll (mitunter als die _backoffrichtlinie_ oder die _wiederholungsrichtlinie_) . Die zwei Richtlinien gekapselt sind die `Android.App.Job.BackoffPolicy` Enum:

* `BackoffPolicy.Exponential` &ndash; Die anfängliche Wartezeit, eine exponentielle backoffrichtlinie wird nach jeder fehlerhaften exponentiell ansteigen. Ein Auftrag fehlschlägt, das zum ersten Mal wartet die Bibliothek das Erstintervall, die vor der durch das erneute Planen des Auftrags – Beispiel 30 Sekunden festgelegt ist. Das zweite Mal, das der Auftrag ein Fehler auftritt, das wartet die Bibliothek mindestens 60 Sekunden, bevor Sie versuchen, den Auftrag auszuführen. Nachdem die dritte fehlgeschlagenen Versuch die Bibliothek 120 Sekunden warten, und so weiter. Dies ist der Standardwert.
* `BackoffPolicy.Linear` &ndash; Diese Strategie ist eine lineare Wartezeit, die der Auftrag, neu geplant werden, sollten in festgelegten Intervallen ausgeführt wird, (bis er erfolgreich abgeschlossen wurde). Lineare Backoff eignet sich am besten für die Arbeit, die so bald wie möglich ausgeführt werden muss oder für Probleme, die schnell selbst aufgelöst werden. 

Weitere Informationen zum Erstellen einer `JobInfo` Objekt, lesen Sie [Google Dokumentation für die `JobInfo.Builder` Klasse](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Übergeben von Parametern zu einem Auftrag über die JobInfo

Parameter werden an einen Auftrag übergeben, durch das Erstellen einer `PersistableBundle` , wird übergeben, zusammen mit der `Job.Builder.SetExtras` Methode:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

Die `PersistableBundle` erfolgt über die `Android.App.Job.JobParameters.Extras` Eigenschaft in der `OnStartJob` Methode eine `JobService`:

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Informationen zum Planen eines Auftrags eine Xamarin.Android Anwendung Abrufen eines Verweises auf die `JobScheduler` -Systemdienst und rufen die `JobScheduler.Schedule` Methode mit der `JobInfo` -Objekt, das im vorherigen Schritt erstellt wurde. `JobScheduler.Schedule` wird sofort mit einem von zwei ganzzahligen Werten zurückgeben:

* **JobScheduler.ResultSuccess** &ndash; der Auftrag wurde erfolgreich geplant. 
* **JobScheduler.ResultFailure** &ndash; der Auftrag konnte nicht geplant werden. Dies wird normalerweise verursacht durch in Konflikt stehende `JobInfo` Parameter.

Dieser Code ist ein Beispiel ein Auftrags planen und Benachrichtigen des Benutzers der Ergebnisse des Versuchs des planen:

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

Es ist möglich, alle Aufträge abzubrechen, die geplant haben, oder nur einen einzelnen Auftrag, indem die `JobsScheduler.CancelAll()` Methode oder die `JobScheduler.Cancel(jobId)` Methode:

```csharp
// Cancel all jobs
jobSchduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert, wie mithilfe der Android Auftragsplaner Intelligent arbeiten im Hintergrund ausgeführt. Er erläutert durchgeführt werden, da die Arbeit zu kapseln einer `JobService` sowie zum Verwenden der `JobScheduler` so planen Sie diese Aufgabe, die Angabe der Kriterien mit einer `JobTrigger` und wie Fehler mit behandelt werden sollen eine `RetryStrategy`.

## <a name="related-links"></a>Verwandte Links

- [Intelligent auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler-API-Referenz](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Planen von Aufträgen wie ein Profi mit JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Android Akku und Arbeitsspeicher Optimierungen - Google e/a-2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler - René Ruppert - Xamarin University](https://www.youtube.com/watch?v=aSjBBPYjelE)