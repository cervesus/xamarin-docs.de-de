---
title: Android-Auftragsplaner
description: In diesem Handbuch wird erläutert, wie Sie Hintergrundarbeit mithilfe der Android-Auftrags Planer-API planen.
ms.prod: xamarin
ms.assetid: 673BB8C3-C5CC-43EC-BA8F-758F15D986C9
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/19/2018
ms.openlocfilehash: 95d4194e0ed1a1da435a233e40a74f506c49b539
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119876"
---
# <a name="android-job-scheduler"></a>Android-Auftragsplaner

_In dieser Anleitung wird erläutert, wie Sie Hintergrund arbeiten mithilfe der Android-Auftrags Planer-API planen, die auf Android-Geräten mit Android 5,0 (API-Ebene 21) und höher verfügbar ist._


## <a name="overview"></a>Übersicht 

Eine der besten Möglichkeiten, eine Android-Anwendung auf den Benutzer reaktionsfähig zu halten, besteht darin, sicherzustellen, dass komplexe oder lange laufende Arbeiten im Hintergrund ausgeführt werden. Allerdings ist es wichtig, dass sich Hintergrund arbeiten nicht negativ auf die Benutzererfahrung des Geräts auswirken. 

Ein Hintergrund Auftrag könnte z. b. alle drei oder vier Minuten eine Website Abfragen, um Änderungen an einem bestimmten Dataset abzufragen. Dies scheint unschädlich zu sein, würde jedoch eine katastrophale Auswirkung auf die Akku Lebensdauer haben. Die Anwendung wird das Gerät wiederholt reaktivieren, die CPU auf einen höheren Energiezustand herauf Stufen, die Radios einschalten, die Netzwerk Anforderungen durchführen und dann die Ergebnisse verarbeiten. Dies ist schlimmer, da das Gerät nicht sofort herunter Netz und wieder in den Energiespar Zustand wechselt. Eine schlecht geplante Hintergrundarbeit kann das Gerät versehentlich in einem Zustand mit unnötigen und übermäßigen Energieanforderungen halten. Diese scheinbar unschuldige Aktivität (das Abrufen einer Website) führt dazu, dass das Gerät in einem relativ kurzen Zeitraum unbrauchbar wird.

Android stellt die folgenden APIs bereit, um die Arbeit im Hintergrund zu unterstützen. Sie sind jedoch allein für die intelligente Auftragsplanung nicht ausreichend. 

- Beabsichtigte **[Dienste](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Beabsichtigte Dienste eignen sich hervorragend für die Arbeit, aber Sie bieten keine Möglichkeit, die Arbeit zu planen.
- **[Alarmmanager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; Diese APIs ermöglichen nur das Planen von Aufgaben, bieten aber keine Möglichkeit, die Arbeit tatsächlich auszuführen. Außerdem lässt der Alarmmanager zeitbasierte Einschränkungen nur zu, was bedeutet, dass ein Alarm zu einem bestimmten Zeitpunkt oder nach Ablauf einer bestimmten Zeitspanne ausgelöst wird. 
- **[Broadcast Empfänger](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Eine Android-App kann Broadcast Empfänger einrichten, um Aufgaben als Reaktion auf systemweite Ereignisse oder Intents auszuführen. Broadcast Empfänger bieten jedoch keine Kontrolle darüber, wann der Auftrag ausgeführt werden soll. Außerdem beschränken Änderungen im Android-Betriebssystem, wenn Broadcast Empfänger funktionieren, oder die Arten von arbeiten, auf die Sie reagieren können. 

Es gibt zwei wichtige Features für die effiziente Durchführung von Hintergrund arbeiten (manchmal auch als _Hintergrund Auftrag_ oder _Auftrag_bezeichnet):

1. **Intelligent Planen der Arbeit** &ndash; Es ist wichtig, dass eine Anwendung im Hintergrundaufgaben durchführt, die dies als guter Bürger bewirkt. Im Idealfall sollte die Anwendung nicht verlangen, dass ein Auftrag ausgeführt wird. Stattdessen sollte die Anwendung Bedingungen angeben, die erfüllt sein müssen, wenn der Auftrag ausgeführt werden kann. Anschließend wird der Auftrag mit dem Betriebssystem geplant, von dem die Arbeit ausgeführt wird, wenn die Bedingungen erfüllt sind. Dadurch kann Android den Auftrag ausführen, um eine maximale Effizienz auf dem Gerät sicherzustellen. Beispielsweise können Netzwerk Anforderungen in einem Batch zusammengefasst werden, um alle gleichzeitig auszuführen, um einen maximalen Aufwand für das Netzwerk zu ermöglichen.
2. **Kapseln der Arbeit** &ndash; Der Code zum Ausführen der Hintergrundarbeit sollte in einer diskreten Komponente gekapselt werden, die unabhängig von der Benutzeroberfläche ausgeführt werden kann und relativ einfach neu geplant werden kann, wenn die Arbeit aus irgendeinem Grund nicht vollständig ausgeführt werden kann.

Der Android-Auftrags Planer ist ein Framework, das in das Android-Betriebssystem integriert ist, das eine fließende API bereitstellt, um die Planung der Hintergrundarbeit zu vereinfachen.  Der Android-Auftrags Planer besteht aus den folgenden Typen:

- Bei `Android.App.Job.JobScheduler` handelt es sich um einen-Systemdienst, der zum Planen, ausführen und ggf. Abbrechen von Aufträgen im Auftrag einer Android-Anwendung verwendet wird.
- Eine `Android.App.Job.JobService` ist eine abstrakte Klasse, die mit der Logik erweitert werden muss, die den Auftrag im Haupt Thread der Anwendung ausgeführt wird. Dies bedeutet, dass `JobService` die für die asynchrone Ausführung der Arbeit verantwortlich ist.
- Ein `Android.App.Job.JobInfo` -Objekt enthält die Kriterien für Android, wenn der Auftrag ausgeführt werden soll.

Um die Arbeit mit dem Android-Auftrags Planer zu planen, muss eine xamarin. Android-Anwendung den Code in einer Klasse Kapseln, `JobService` die die Klasse erweitert. `JobService`verfügt über drei Lebenszyklus Methoden, die während der Lebensdauer des Auftrags aufgerufen werden können:

- **bool onstartjob (jobparameters-Parameter)** Diese Methode wird `JobScheduler` von aufgerufen, um Arbeit auszuführen, und wird im Haupt Thread der Anwendung ausgeführt. &ndash; Es liegt `JobService` in der Verantwortung der, die `true` Arbeit asynchron auszuführen, wenn noch verbleibende Arbeit vorliegt oder `false` wenn die Arbeit erledigt ist.
    
    Wenn diese Methode aufruft, wird für die Dauer des Auftrags ein wakelock von Android angefordert und beibehalten. `JobScheduler` Wenn der Auftrag abgeschlossen ist, liegt es `JobService` in der Verantwortung des, den `JobFinished` dieser `JobScheduler` Tatsache durch aufzurufen der-Methode mitzuteilen (siehe weiter unten).

- **Jobabgeschlossene (jobparameters-Parameter, bool needsreschedule)** Diese Methode muss `JobService` von aufgerufen werden, um dem `JobScheduler` mitzuteilen, dass die Arbeit erledigt ist. &ndash; Wenn `JobFinished` nicht aufgerufen wird, entfernt `JobScheduler` das wakelock nicht, was unnötige Akku Abladung verursacht. 

- **bool onstopjob (jobparameters-Parameter)** &ndash; Dies wird aufgerufen, wenn der Auftrag von Android vorzeitig beendet wird. Er sollte zurück `true` geben, wenn der Auftrag basierend auf den Wiederholungs Kriterien neu geplant werden soll (im folgenden ausführlicher erläutert).

Es ist möglich, _Einschränkungen_ oder _Trigger_ anzugeben, die Steuern, wann ein Auftrag ausgeführt werden kann oder sollte. Beispielsweise ist es möglich, einen Auftrag so einzuschränken, dass er nur ausgeführt wird, wenn das Gerät belastet wird, oder einen Auftrag zu starten, wenn ein Bild erstellt wird.

In diesem Leitfaden wird ausführlich erläutert, wie eine `JobService` -Klasse implementiert und mit dem `JobScheduler`geplant wird.

## <a name="requirements"></a>Anforderungen

Der Android-Auftrags Planer erfordert Android-API-Ebene 21 (Android 5,0) oder höher. 

## <a name="using-the-android-job-scheduler"></a>Verwenden des Android-Auftrags Planers

Es gibt drei Schritte für die Verwendung der Android JobScheduler-API:

1. Implementieren Sie einen Jobservice-Typ, um die Arbeit zu kapseln.
2. Verwenden Sie `JobInfo.Builder` ein-Objekt, `JobInfo` um das Objekt zu erstellen, das die `JobScheduler` Kriterien für die zum Ausführen des Auftrags enthalten soll. 
3. Planen Sie den Auftrag `JobScheduler.Schedule`mithilfe von.

### <a name="implement-a-jobservice"></a>Implementieren eines Jobservice

Alle Aufgaben, die von der Android-Auftrags Planer-Bibliothek ausgeführt werden, müssen in einem Typ `Android.App.Job.JobService` ausgeführt werden, der die abstrakte-Klasse erweitert. Das Erstellen `JobService` einer ähnelt sehr der Erstellung eines `Service` mit dem Android-Framework: 

1. Erweitern Sie `JobService` die-Klasse.
2. Ergänzen Sie die-Unterklasse `ServiceAttribute` mit dem, `Name` und legen Sie den-Parameter auf eine Zeichenfolge fest, die aus dem Paketnamen und dem Namen der Klasse besteht (siehe folgendes Beispiel).
3. Legen `ServiceAttribute` Sie `Permission` die-Eigenschaft für den auf `android.permission.BIND_JOB_SERVICE`die Zeichenfolge fest.
4. Überschreiben `OnStartJob` Sie die-Methode, und fügen Sie den Code zum Ausführen der Arbeit hinzu. Diese Methode wird von Android auf dem Haupt Thread der Anwendung aufgerufen, um den Auftrag auszuführen. Die Arbeit dauert länger, bis ein paar Millisekunden in einem Thread ausgeführt werden, um eine Blockierung der Anwendung zu vermeiden.
5. Wenn die Arbeit erledigt ist, muss `JobService` die die `JobFinished` -Methode aufruft. Diese Methode `JobService` teilt dem mit, `JobScheduler` dass die Arbeit erledigt ist. Wenn Sie nicht `JobFinished` aufrufen, führt dies `JobService` dazu, dass nicht benötigte Anforderungen auf das Gerät gestellt werden, sodass die Akku Lebensdauer verkürzt wird. 
6. Es empfiehlt sich, auch die `OnStopJob` -Methode zu überschreiben. Diese Methode wird von Android aufgerufen, wenn der Auftrag beendet wird, bevor er abgeschlossen ist, und bietet `JobService` die Möglichkeit, sämtliche Ressourcen ordnungsgemäß zu löschen. Diese Methode sollte zurück `true` geben, wenn es erforderlich ist, den Auftrag neu zu `false` planen, oder, wenn es nicht wünschenswert ist, den Auftrag erneut auszuführen.
   

Der folgende Code ist ein Beispiel für die einfachste `JobService` für eine Anwendung, wobei die TPL zum asynchronen Ausführen einiger Aufgaben verwendet wird:

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

### <a name="creating-a-jobinfo-to-schedule-a-job"></a>Erstellen einer Jobinfo zum Planen eines Auftrags

Xamarin. Android-Anwendungen instanziieren `JobService` keine direkt, sondern übergeben ein `JobInfo` -Objekt an `JobScheduler`. Der `JobScheduler` instanziiert das angeforderte `JobService` Objekt, plant und führt den `JobService` gemäß den Metadaten in aus `JobInfo`. Ein `JobInfo` -Objekt muss die folgenden Informationen enthalten:

- **JobID** Dies ist ein `int` -Wert, der verwendet wird, um einen Auftrag `JobScheduler`für die zu identifizieren. &ndash; Durch die erneute Verwendung dieses Werts werden alle vorhandenen Aufträge aktualisiert. Der Wert muss für die Anwendung eindeutig sein. 
- **Jobservice** Dieser Parameter ist ein `ComponentName` , der den Typ explizit identifiziert, `JobScheduler` den der zum Ausführen eines Auftrags verwenden soll. &ndash; 
  

Diese Erweiterungsmethode veranschaulicht, wie eine `JobInfo.Builder` mit einem Android `Context`erstellt wird, z. b. eine-Aktivität:

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

Ein leistungsfähiges Feature des Android-Auftrags Planers ist die Möglichkeit, zu steuern, wann ein Auftrag ausgeführt wird oder unter welchen Bedingungen ein Auftrag ausgeführt werden kann. In der folgenden Tabelle werden einige der Methoden für `JobInfo.Builder` beschrieben, mit denen eine APP beeinflussen kann, wann ein Auftrag ausgeführt werden kann:  


|  Methode | Beschreibung   |
|---|---|
| `SetMinimumLatency`  | Gibt an, dass eine Verzögerung (in Millisekunden) vor dem Ausführen eines Auftrags festgestellt werden soll. |
| `SetOverridingDeadline`  | Deklariert, dass der Auftrag vor diesem Zeitpunkt (in Millisekunden) ausgeführt werden muss. |
| `SetRequiredNetworkType`  | Gibt die Netzwerk Anforderungen für einen Auftrag an. |
| `SetRequiresBatteryNotLow` | Der Auftrag kann nur ausgeführt werden, wenn dem Benutzer keine Warnung vom Typ "geringe Akkukapazität" angezeigt wird. |
| `SetRequiresCharging` | Der Auftrag kann nur ausgeführt werden, wenn der Akku belastet wird. |
| `SetDeviceIdle` | Der Auftrag wird ausgeführt, wenn das Gerät ausgelastet ist. |
| `SetPeriodic` | Gibt an, dass der Auftrag regelmäßig ausgeführt werden soll. |
| `SetPersisted` | Der Auftrag sollte über Geräte Neustarts hinweg durchgesetzt werden. | 


Bietet eine Anleitung dazu, wie `JobScheduler` lange gewartet werden soll, bevor versucht wird, einen Auftrag erneut auszuführen. `SetBackoffCriteria` Es gibt zwei Teile der Backoff-Kriterien: eine Verzögerung in Millisekunden (Standardwert von 30 Sekunden) und den Rückgabetyp, der verwendet werden soll (manchmal auch als _Backoff-Richtlinie_ oder _Wiederholungs Richtlinie_bezeichnet). Die beiden Richtlinien werden in der `Android.App.Job.BackoffPolicy` -Aufzählung gekapselt:

- `BackoffPolicy.Exponential`&ndash; Eine exponentielle Backoff-Richtlinie erhöht den anfänglichen Backoff-Wert exponentiell nach jedem Fehler. Wenn ein Auftrag zum ersten Mal fehlschlägt, wartet die Bibliothek das Anfangs Intervall, das vor dem erneuten Planen des Auftrags angegeben wird – Beispiel: 30 Sekunden. Wenn der Auftrag zum zweiten Mal fehlschlägt, wartet die Bibliothek mindestens 60 Sekunden, bevor versucht wird, den Auftrag auszuführen. Nach dem dritten fehlgeschlagenen Versuch wartet die Bibliothek 120 Sekunden usw. Dies ist der Standardwert.
- `BackoffPolicy.Linear`&ndash; Diese Strategie ist ein lineares Backoff, dass der Auftrag für die Ausführung in festgelegten Intervallen neu geplant werden soll (bis er erfolgreich ausgeführt wird). Der lineare Backoff eignet sich am besten für die Arbeit, die so bald wie möglich abgeschlossen werden muss, oder für Probleme, die sich schnell auflösen. 

Weitere Informationen zum Erstellen eines `JobInfo` -Objekts finden Sie in [der Google-Dokumentation für `JobInfo.Builder` die-Klasse](https://developer.android.com/reference/android/app/job/JobInfo.Builder.html).

#### <a name="passing-parameters-to-a-job-via-the-jobinfo"></a>Übergeben von Parametern an einen Auftrag über die Jobinfo

Parameter werden an einen Auftrag weitergegeben, indem `PersistableBundle` ein erstellt wird, der zusammen `Job.Builder.SetExtras` mit der-Methode weitergegeben wird:

```csharp
var jobParameters = new PersistableBundle();
jobParameters.PutInt("LoopCount", 11);

var jobBuilder = this.CreateJobBuilderUsingJobId<DownloadJob>(1)
                     .SetExtras(jobParameters)
                     .Build();
```

Der `PersistableBundle` `Android.App.Job.JobParameters.Extras` Zugriff aufdie`OnStartJob` erfolgt über die-Eigenschaft in der-Methode eines: `JobService`

```csharp
public override bool OnStartJob(JobParameters jobParameters)
{
    var loopCount = jobParams.Extras.GetInt("LoopCount", 10);
    
    // rest of code omitted
} 
```

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Zum Planen eines Auftrags erhält eine xamarin. Android-Anwendung einen Verweis auf den `JobScheduler` -Systemdienst und ruft die `JobScheduler.Schedule` -Methode mit dem `JobInfo` -Objekt auf, das im vorherigen Schritt erstellt wurde. `JobScheduler.Schedule`wird sofort mit einem von zwei ganzzahligen Werten zurückgegeben:

- **Jobscheduler. resultsuccess** &ndash; der Auftrag wurde erfolgreich geplant. 
- **Jobscheduler. resultfailure** &ndash; der Auftrag konnte nicht geplant werden. Dies wird in der Regel durch `JobInfo` widersprüchliche Parameter verursacht.

Dieser Code ist ein Beispiel für die Planung eines Auftrags und die Benachrichtigung des Benutzers über die Ergebnisse des Planungs Versuchs:

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

Es ist möglich, alle geplanten Aufträge oder nur einen einzelnen Auftrag mit der `JobsScheduler.CancelAll()` -Methode oder der `JobScheduler.Cancel(jobId)` -Methode abzubrechen:

```csharp
// Cancel all jobs
jobScheduler.CancelAll(); 

// to cancel a job with jobID = 1
jobScheduler.Cancel(1)
```
  
## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie Sie den Android-Auftrags Planer verwenden, um im Hintergrund arbeiten Intelligent auszuführen. Es wurde erläutert, wie `JobService` die auszuführenden Aufgaben als und `JobScheduler` verwendet werden, um die Arbeit zu planen, die Kriterien mit einem `JobTrigger` anzugeben und zu bestimmen, wie Fehler mit einem `RetryStrategy`behandelt werden sollen.

## <a name="related-links"></a>Verwandte Links

- [Intelligente Auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [JobScheduler-API-Referenz](https://developer.android.com/reference/android/app/job/JobScheduler.html)
- [Planen von Aufträgen wie einem pro mit JobScheduler](https://medium.com/google-developers/scheduling-jobs-like-a-pro-with-jobscheduler-286ef8510129)
- [Optimierungen für Android-Akku und Arbeitsspeicher-Google I/O 2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
- [Android JobScheduler-René Ruppert](https://www.youtube.com/watch?v=aSjBBPYjelE)
