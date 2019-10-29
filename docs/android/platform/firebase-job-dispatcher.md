---
title: Firebase Job Dispatcher
description: In diesem Handbuch wird erläutert, wie Sie Hintergrund arbeiten mithilfe der Firebase-Auftrags Verteiler Bibliothek von Google planen.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73020244"
---
# <a name="firebase-job-dispatcher"></a>Firebase Job Dispatcher

_In diesem Handbuch wird erläutert, wie Sie Hintergrund arbeiten mithilfe der Firebase-Auftrags Verteiler Bibliothek von Google planen._

## <a name="overview"></a>Übersicht

Eine der besten Möglichkeiten, eine Android-Anwendung auf den Benutzer reaktionsfähig zu halten, besteht darin, sicherzustellen, dass komplexe oder lange laufende Arbeiten im Hintergrund ausgeführt werden. Allerdings ist es wichtig, dass sich Hintergrund arbeiten nicht negativ auf die Benutzererfahrung des Geräts auswirken. 

Ein Hintergrund Auftrag könnte z. b. alle drei oder vier Minuten eine Website Abfragen, um Änderungen an einem bestimmten Dataset abzufragen. Dies scheint unschädlich zu sein, würde jedoch eine katastrophale Auswirkung auf die Akku Lebensdauer haben. Die Anwendung wird das Gerät wiederholt reaktivieren, die CPU auf einen höheren Energiezustand herauf Stufen, die Radios einschalten, die Netzwerk Anforderungen durchführen und dann die Ergebnisse verarbeiten. Dies ist schlimmer, da das Gerät nicht sofort herunter Netz und wieder in den Energiespar Zustand wechselt. Eine schlecht geplante Hintergrundarbeit kann das Gerät versehentlich in einem Zustand mit unnötigen und übermäßigen Energieanforderungen halten. Diese scheinbar unschuldige Aktivität (das Abrufen einer Website) führt dazu, dass das Gerät in einem relativ kurzen Zeitraum unbrauchbar wird.

Android stellt die folgenden APIs bereit, um die Arbeit im Hintergrund zu unterstützen. Sie sind jedoch allein für die intelligente Auftragsplanung nicht ausreichend. 

- Beabsichtigte **[Dienste](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Intent Services eignen sich hervorragend für die Arbeit, aber Sie bieten keine Möglichkeit, die Arbeit zu planen.
- **[Alarmmanager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; diese APIs ermöglichen nur die Planung von Aufgaben, bieten aber keine Möglichkeit, die Arbeit tatsächlich auszuführen. Außerdem lässt der Alarmmanager zeitbasierte Einschränkungen nur zu, was bedeutet, dass ein Alarm zu einem bestimmten Zeitpunkt oder nach Ablauf einer bestimmten Zeitspanne ausgelöst wird. 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; JobSchedule ist eine großartige API, die mit dem Betriebssystem zusammenarbeitet, um Aufträge zu planen. Sie ist jedoch nur für Android-Apps verfügbar, die auf API-Ebene 21 oder höher ausgerichtet sind. 
- **[Broadcast Empfänger](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; eine Android-App kann Broadcast Empfänger einrichten, um Aufgaben als Reaktion auf systemweite Ereignisse oder Intents auszuführen. Broadcast Empfänger bieten jedoch keine Kontrolle darüber, wann der Auftrag ausgeführt werden soll. Außerdem beschränken Änderungen im Android-Betriebssystem, wenn Broadcast Empfänger funktionieren, oder die Arten von arbeiten, auf die Sie reagieren können. 

Es gibt zwei wichtige Features für die effiziente Durchführung von Hintergrund arbeiten (manchmal auch als _Hintergrund Auftrag_ oder _Auftrag_bezeichnet):

1. **Intelligent Planen der Arbeit** &ndash; es wichtig, dass eine Anwendung im Hintergrundaufgaben durchführt, die dies als guter Bürger bewirkt. Im Idealfall sollte die Anwendung nicht verlangen, dass ein Auftrag ausgeführt wird. Stattdessen sollte die Anwendung Bedingungen angeben, die erfüllt sein müssen, wenn der Auftrag ausgeführt werden kann, und dann die Ausführung planen, wenn die Bedingungen erfüllt sind. Dadurch kann Android Intelligent Arbeiten ausführen. Beispielsweise können Netzwerk Anforderungen in einem Batch zusammengefasst werden, um alle gleichzeitig auszuführen, um einen maximalen Aufwand für das Netzwerk zu ermöglichen.
2. Das **Kapseln der Arbeit** &ndash; der Code zum Ausführen der Hintergrundarbeit sollte in einer diskreten Komponente gekapselt werden, die unabhängig von der Benutzeroberfläche ausgeführt werden kann und relativ einfach neu geplant werden kann, wenn die Arbeit für einige nicht vollständig ausgeführt wird. weshalb.

Der Firebase Job Dispatcher ist eine Bibliothek von Google, die eine fließende API bereitstellt, um die Planung von Hintergrundaufgaben zu vereinfachen. Er soll als Ersatz für Google Cloud Manager dienen. Der Firebase-Auftrags Verteiler besteht aus den folgenden APIs:

- Eine `Firebase.JobDispatcher.JobService` ist eine abstrakte Klasse, die mit der Logik erweitert werden muss, die im Hintergrund Auftrag ausgeführt wird.
- Ein-`Firebase.JobDispatcher.JobTrigger` deklariert, wann der Auftrag gestartet werden soll. Dies wird in der Regel als Zeitfenster ausgedrückt. warten Sie z. b. mindestens 30 Sekunden, bevor Sie den Auftrag starten, aber führen Sie den Auftrag innerhalb von 5 Minuten aus.
- Eine `Firebase.JobDispatcher.RetryStrategy` die Informationen zu den Aktionen enthält, die durchgeführt werden sollen, wenn ein Auftrag nicht ordnungsgemäß ausgeführt wird. Die Wiederholungs Strategie gibt an, wie lange gewartet werden soll, bevor versucht wird, den Auftrag erneut auszuführen. 
- Ein `Firebase.JobDispatcher.Constraint` ist ein optionaler Wert, der eine Bedingung beschreibt, die erfüllt sein muss, bevor der Auftrag ausgeführt werden kann, z. b. wenn sich das Gerät in einem nicht gemessenen Netzwerk oder in einem gebührenpflichtigen Netzwerk befindet
- Der `Firebase.JobDispatcher.Job` ist eine API, die die vorherigen APIs in zu einer Arbeitseinheit vereinigt, die vom `JobDispatcher`geplant werden kann. Die `Job.Builder`-Klasse wird verwendet, um eine `Job`zu instanziieren.
- In einem `Firebase.JobDispatcher.JobDispatcher` werden die vorherigen drei APIs verwendet, um die Arbeit mit dem Betriebssystem zu planen und um ggf. Aufträge abzubrechen.

Um die Arbeit mit dem Firebase-Auftrags Verteiler zu planen, muss eine xamarin. Android-Anwendung den Code in einem Typ Kapseln, der die `JobService` Klasse erweitert. `JobService` verfügt über drei Lebenszyklus Methoden, die während der Lebensdauer des Auftrags aufgerufen werden können:

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; diese Methode an der Stelle, an der die Arbeit stattfindet und immer implementiert werden sollte. Es wird im Haupt Thread ausgeführt. Diese Methode gibt `true` zurück, wenn die Arbeit noch vorhanden ist, oder `false`, wenn die Arbeit erledigt ist. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; wird dies aufgerufen, wenn der Auftrag aus irgendeinem Grund beendet wird. Es sollte `true` zurückgegeben werden, wenn der Auftrag zu einem späteren Zeitpunkt neu geplant werden soll.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; diese Methode aufgerufen wird, wenn die `JobService` alle asynchronen Arbeiten abgeschlossen hat. 

Um einen Auftrag zu planen, instanziiert die Anwendung ein `JobDispatcher` Objekt. Anschließend wird ein-`Job.Builder` verwendet, um ein `Job`-Objekt zu erstellen, das dem `JobDispatcher` bereitgestellt wird, das versucht, die Ausführung des Auftrags zu planen.

In diesem Handbuch wird erläutert, wie Sie den Firebase-Auftrags Verteiler einer xamarin. Android-Anwendung hinzufügen und ihn zum Planen der Hintergrundarbeit verwenden.

## <a name="requirements"></a>Anforderungen

Der Firebase-Auftrags Verteiler erfordert Android-API-Ebene 9 oder höher. Die Firebase-Auftrags Verteiler Bibliothek basiert auf einigen Komponenten, die von Google Play Services bereitgestellt werden. auf dem Gerät müssen Google Play Services installiert sein.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Verwenden der Firebase-Auftrags Verteiler Bibliothek in xamarin. Android

Fügen Sie zunächst das [nuget-Paket xamarin. Firebase. jobdispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) dem xamarin. Android-Projekt hinzu, um mit dem Firebase-Auftrags Verteiler loszulegen. Suchen Sie im nuget-Paket-Manager nach dem **xamarin. Firebase. jobdispatcher** -Paket (das sich noch in der Vorabversion befindet).

Nachdem Sie die Firebase-Auftrags Verteiler Bibliothek hinzugefügt haben, erstellen Sie eine `JobService`-Klasse, und planen Sie die Ausführung mit einer Instanz des `FirebaseJobDispatcher`.

### <a name="creating-a-jobservice"></a>Erstellen eines Jobservice

Alle Aufgaben, die von der Firebase-Auftrags Verteiler Bibliothek durchgeführt werden, müssen in einem Typ ausgeführt werden, der die `Firebase.JobDispatcher.JobService` abstrakte Klasse erweitert. Das Erstellen einer `JobService` ähnelt stark dem Erstellen einer `Service` mit dem Android-Framework: 

1. Erweitern der `JobService`-Klasse
2. Ergänzen Sie die-Unterklasse mit dem `ServiceAttribute`. Obwohl es nicht unbedingt erforderlich ist, wird empfohlen, den `Name`-Parameter explizit festzulegen, um beim Debuggen des `JobService`zu helfen. 
3. Fügen Sie eine `IntentFilter` hinzu, um die `JobService` in " **androidmanifest. XML**" zu deklarieren. Dadurch wird auch die Firebase-Auftrags Verteiler Bibliothek dazu verwendet, die `JobService`zu finden und aufzurufen.

Der folgende Code ist ein Beispiel für die einfachste `JobService` für eine Anwendung, wobei die TPL zum asynchronen Ausführen einiger Aufgaben verwendet wird:

```csharp
[Service(Name = "com.xamarin.fjdtestapp.DemoJob")]
[IntentFilter(new[] {FirebaseJobServiceIntent.Action})]
public class DemoJob : JobService
{
    static readonly string TAG = "X:DemoService";

    public override bool OnStartJob(IJobParameters jobParameters)
    {
        Task.Run(() =>
        {
            // Work is happening asynchronously (code omitted)
                       
        });

        // Return true because of the asynchronous work
        return true;  
    }

    public override bool OnStopJob(IJobParameters jobParameters)
    {
        Log.Debug(TAG, "DemoJob::OnStartJob");
        // nothing to do.
        return false;
    }
}
```

### <a name="creating-a-firebasejobdispatcher"></a>Erstellen eines firebasejobdispatcher-

Bevor eine Arbeit geplant werden kann, muss ein `Firebase.JobDispatcher.FirebaseJobDispatcher` Objekt erstellt werden. Der `FirebaseJobDispatcher` ist für die Planung einer `JobService`verantwortlich. Der folgende Code Ausschnitt ist eine Möglichkeit, eine Instanz des `FirebaseJobDispatcher`zu erstellen: 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Im vorherigen Code Ausschnitt ist die `GooglePlayDriver` eine Klasse, die die `FirebaseJobDispatcher` Interaktion mit einigen der Planungs-APIs in Google Play Services auf dem Gerät unterstützt. Der Parameter `context` ist eine beliebige Android-`Context`, z. b. eine-Aktivität. Derzeit ist die `GooglePlayDriver` die einzige `IDriver` Implementierung in der Firebase-Auftrags Verteiler Bibliothek. 

Die xamarin. Android-Bindung für den Firebase-Auftrags Verteiler stellt eine Erweiterungsmethode zum Erstellen eines `FirebaseJobDispatcher` aus dem `Context`bereit: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Nachdem der `FirebaseJobDispatcher` instanziiert wurde, ist es möglich, eine `Job` zu erstellen und den Code in der `JobService`-Klasse auszuführen. Der `Job` wird von einem `Job.Builder`-Objekt erstellt und wird im nächsten Abschnitt erläutert.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Erstellen eines Firebase. jobdispatcher. Job mit "Job. Builder"

Die `Firebase.JobDispatcher.Job`-Klasse ist verantwortlich für das Kapseln der Metadaten, die erforderlich sind, um eine `JobService`auszuführen. Ein @ no__t_0_ enthält Informationen wie jede Einschränkung, die erfüllt sein muss, bevor der Auftrag ausgeführt werden kann, wenn die `Job` wiederholt wird, oder alle Trigger, die dazu führen, dass der Auftrag ausgeführt wird.  Ein `Job` muss mindestens über ein _Tag_ (eine eindeutige Zeichenfolge, die den Auftrag für die `FirebaseJobDispatcher`identifiziert) und den Typ des `JobService` verfügen, der ausgeführt werden soll. Der Firebase-Auftrags Verteiler instanziiert die `JobService`, wenn es an der Zeit ist, den Auftrag auszuführen.  Eine `Job` wird mit einer Instanz der `Firebase.JobDispatcher.Job.JobBuilder`-Klasse erstellt. 

Der folgende Code Ausschnitt ist das einfachste Beispiel für das Erstellen einer `Job` mithilfe der xamarin. Android-Bindung:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

Der `Job.Builder` führt einige grundlegende Validierungs Überprüfungen für die Eingabewerte für den Auftrag aus. Eine Ausnahme wird ausgelöst, wenn das `Job.Builder` keine `Job`erstellen kann.  Der `Job.Builder` erstellt eine `Job` mit den folgenden Standardeinstellungen:

- Die _Lebensdauer_ eines `Job`(wie lange die Ausführung geplant wird) erfolgt erst, wenn das Gerät neu gestartet wird &ndash; sobald das Gerät neu `Job` gestartet wird.
- Eine `Job` wird nicht wiederholt, &ndash; Sie nur einmal ausgeführt wird.
- Eine `Job` wird so geplant, dass Sie so schnell wie möglich ausgeführt wird.
- Die Standard Wiederholungs Strategie für eine `Job` ist die Verwendung eines _exponentiellen Backoff_ (weitere Details finden Sie weiter unten im Abschnitt [Festlegen einer retrystrategy](#Setting_a_RetryStrategy)).

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Nachdem Sie die `Job`erstellt haben, muss Sie mit der `FirebaseJobDispatcher` geplant werden, bevor Sie ausgeführt wird. Es gibt zwei Methoden zum Planen einer `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Der von `FirebaseJobDispatcher.Schedule` zurückgegebene Wert ist einer der folgenden ganzzahligen Werte:

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; die `Job` erfolgreich geplant wurde.
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; ein unbekanntes Problem aufgetreten ist, das das geplante `Job` verhindert hat.
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; eine ungültige `IDriver` wurde verwendet, oder die `IDriver` war nicht verfügbar. 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; der `Trigger` nicht unterstützt.
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; der Dienst nicht ordnungsgemäß konfiguriert ist oder nicht verfügbar ist.

### <a name="configuring-a-job"></a>Konfigurieren eines Auftrags

Es ist möglich, einen Auftrag anzupassen. Beispiele für die Anpassung eines Auftrags:

- Das [übergeben von Parametern an einen Auftrag](#Passing_Parameters_to_a_Job) &ndash; ein `Job` möglicherweise zusätzliche Werte zum Ausführen der Aufgaben, z. b. zum Herunterladen einer Datei, erforderlich.
- [Legen Sie Einschränkungen fest](#Setting_Constraints) &ndash; möglicherweise ist es erforderlich, einen Auftrag nur auszuführen, wenn bestimmte Bedingungen erfüllt sind. Führen Sie z. b. nur eine `Job` aus, wenn das Gerät berechnet wird. 
- Geben Sie an, [wann eine `Job` ausgeführt werden soll](#Setting_Job_Triggers) &ndash; der Firebase-Auftrags Verteiler ermöglicht es Anwendungen, eine Uhrzeit anzugeben, zu der der Auftrag ausgeführt werden soll.  
- [Deklarieren Sie eine Wiederholungs Strategie für fehlgeschlagene Aufträge](#Setting_a_RetryStrategy) &ndash; eine _Wiederholungs Strategie_ bietet Anleitungen für die `FirebaseJobDispatcher`, wie `Jobs`, die nicht beendet werden sollen. 

Diese Themen werden in den folgenden Abschnitten ausführlicher erläutert.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Übergeben von Parametern an einen Auftrag

Parameter werden an einen Auftrag übermittelt, indem eine `Bundle` erstellt wird, die zusammen mit der `Job.Builder.SetExtras`-Methode weitergegeben wird:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Der Zugriff auf die `Bundle` erfolgt über die `IJobParameters.Extras`-Eigenschaft der `OnStartJob`-Methode:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Festlegen von Einschränkungen

Einschränkungen können dabei helfen, die Kosten oder den Akku Abfluss auf dem Gerät zu verringern. Die `Firebase.JobDispatcher.Constraint`-Klasse definiert diese Einschränkungen als ganzzahlige Werte:

- `Constraint.OnUnmeteredNetwork` &ndash; den Auftrag nur ausführen, wenn das Gerät mit einem nicht gemessenen Netzwerk verbunden ist. Dies ist hilfreich, um zu verhindern, dass Benutzerdaten Gebühren verursachen.
- `Constraint.OnAnyNetwork` &ndash; den Auftrag in jedem Netzwerk ausführen, mit dem das Gerät verbunden ist. Wenn Sie zusammen mit `Constraint.OnUnmeteredNetwork`angegeben wird, hat dieser Wert Vorrang.
- `Constraint.DeviceCharging` &ndash; den Auftrag nur dann ausführen, wenn für das Gerät ein Ladevorgang ausgeführt wird.

Einschränkungen werden mit der `Job.Builder.SetConstraint`-Methode festgelegt: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

Der `JobTrigger` enthält Anleitungen für das Betriebssystem, wann der Auftrag gestartet werden soll. Ein `JobTrigger` verfügt über ein _ausführendes Fenster_ , das einen geplanten Zeitpunkt für die Ausführung des `Job` definiert. Das Ausführungs Fenster verfügt über einen _Startfenster_ Wert und einen _Endfenster_ Wert. Das Startfenster gibt an, wie viele Sekunden das Gerät warten soll, bevor der Auftrag ausgeführt wird, und der Wert des endfensters ist die maximale Anzahl von Sekunden, die gewartet wird, bevor die `Job`ausgeführt wird. 

Eine `JobTrigger` kann mit der `Firebase.Jobdispatcher.Trigger.ExecutionWindow`-Methode erstellt werden.  `Trigger.ExecutionWindow(15,60)` bedeutet beispielsweise, dass der Auftrag zwischen 15 und 60 Sekunden nach der geplanten Ausführung ausgeführt werden soll. Die `Job.Builder.SetTrigger`-Methode wird verwendet, um 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Der Standard `JobTrigger` für einen Auftrag wird durch den Wert `Trigger.Now`dargestellt, der angibt, dass ein Auftrag nach der Planung so schnell wie möglich ausgeführt wird.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Festlegen einer Wiederholungs Strategie

Der `Firebase.JobDispatcher.RetryStrategy` wird verwendet, um anzugeben, wie lange ein Gerät verwendet werden soll, bevor versucht wird, einen fehlgeschlagenen Auftrag erneut auszuführen. Ein `RetryStrategy` verfügt über eine _Richtlinie_, die definiert, welcher Zeit Basis Algorithmus zum erneuten Planen des fehlgeschlagenen Auftrags verwendet wird, und ein Ausführungs Fenster, das ein Fenster angibt, in dem der Auftrag geplant werden soll. Dieses _neuplanungs Fenster_ wird durch zwei Werte definiert. Der erste Wert ist die Anzahl der Sekunden, die gewartet wird, bevor der Auftrag neu geplant wird (der _anfängliche Backoff_ -Wert), und die zweite Zahl ist die maximale Anzahl von Sekunden, bevor der Auftrag ausgeführt werden muss (der _Maximale Backoff_ -Wert). 

Die zwei Arten von Wiederholungs Richtlinien werden durch diese int-Werte identifiziert:

- `RetryStrategy.RetryPolicyExponential` &ndash; einer _exponentiellen Backoff_ -Richtlinie erhöht sich der anfängliche Backoff-Wert exponentiell nach jedem Fehler. Wenn ein Auftrag zum ersten Mal fehlschlägt, wartet die Bibliothek das _initial-Intervall, das vor der erneuten Planung des Auftrags angegeben wird, &ndash; beispielsweise 30 Sekunden. Wenn der Auftrag zum zweiten Mal fehlschlägt, wartet die Bibliothek mindestens 60 Sekunden, bevor versucht wird, den Auftrag auszuführen. Nach dem dritten fehlgeschlagenen Versuch wartet die Bibliothek 120 Sekunden usw. Der Standard `RetryStrategy` für die Firebase-Auftrags Verteiler Bibliothek wird durch das `RetryStrategy.DefaultExponential`-Objekt dargestellt. Er verfügt über einen anfänglichen Backoff von 30 Sekunden und einen maximalen Backoff von 3600 Sekunden.
- `RetryStrategy.RetryPolicyLinear` &ndash; diese Strategie ein _lineares Backoff_ , dass der Auftrag für die Ausführung in festgelegten Intervallen neu geplant werden soll (bis er erfolgreich ausgeführt wird). Der lineare Backoff eignet sich am besten für die Arbeit, die so bald wie möglich abgeschlossen werden muss, oder für Probleme, die sich schnell auflösen. Die Firebase Job Dispatcher Library definiert eine `RetryStrategy.DefaultLinear`, die ein Fenster mit einer Neuplanung von mindestens 30 Sekunden und bis zu 3600 Sekunden aufweist.

Es ist möglich, eine benutzerdefinierte `RetryStrategy` mit der `FirebaseJobDispatcher.NewRetryStrategy`-Methode zu definieren. Es werden drei Parameter benötigt:

1. `int policy` &ndash; die _Richtlinie_ einer der vorherigen `RetryStrategy` Werte, `RetryStrategy.RetryPolicyLinear`oder `RetryStrategy.RetryPolicyExponential`ist.
2. `int initialBackoffSeconds` &ndash; der _anfängliche Backoff_ eine Verzögerung (in Sekunden), die erforderlich ist, bevor versucht wird, den Auftrag erneut auszuführen. Der Standardwert hierfür ist 30 Sekunden. 
3. `int maximumBackoffSeconds` &ndash; der _Maximale Backoff_ -Wert die maximale Anzahl von Sekunden, die verzögert werden soll, bevor versucht wird, den Auftrag erneut auszuführen. Der Standardwert ist 3600 Sekunden. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Abbrechen eines Auftrags

Es ist möglich, alle geplanten Aufträge oder nur einen einzelnen Auftrag mit der `FirebaseJobDispatcher.CancelAll()`-Methode oder der `FirebaseJobDispatcher.Cancel(string)`-Methode abzubrechen:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Beide Methoden geben einen ganzzahligen Wert zurück:

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; der Auftrag erfolgreich abgebrochen wurde.
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; ein Fehler verhindert, dass der Auftrag abgebrochen wird.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; der `FirebaseJobDispatcher` nicht in der Lage ist, den Auftrag abzubrechen, weil keine gültige `IDriver` verfügbar ist.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie der Firebase-Auftrags Verteiler verwendet wird, um im Hintergrund arbeiten Intelligent auszuführen. Es wurde erläutert, wie die auszuführenden Aufgaben als `JobService` gekapselter werden, und wie die `FirebaseJobDispatcher` verwendet wird, um die Arbeit zu planen, die Kriterien mit einem `JobTrigger` anzugeben und zu erfahren, wie Fehler mit einer `RetryStrategy`behandelt werden müssen.

## <a name="related-links"></a>Verwandte Links

- [Xamarin. Firebase. jobdispatcher für nuget](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [Firebase-Job-Dispatcher auf GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin. Firebase. jobdispatcher-Bindung](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Intelligente Auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [Optimierungen für Android-Akku und Arbeitsspeicher-Google I/O 2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
