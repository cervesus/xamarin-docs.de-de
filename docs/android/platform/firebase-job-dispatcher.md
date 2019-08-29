---
title: Firebase Job Dispatcher
description: In diesem Handbuch wird erläutert, wie Sie Hintergrund arbeiten mithilfe der Firebase-Auftrags Verteiler Bibliothek von Google planen.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: c0a35414cce6ff9981ad7825c8158a2f6f707585
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119493"
---
# <a name="firebase-job-dispatcher"></a>Firebase Job Dispatcher

_In diesem Handbuch wird erläutert, wie Sie Hintergrund arbeiten mithilfe der Firebase-Auftrags Verteiler Bibliothek von Google planen._


## <a name="overview"></a>Übersicht

Eine der besten Möglichkeiten, eine Android-Anwendung auf den Benutzer reaktionsfähig zu halten, besteht darin, sicherzustellen, dass komplexe oder lange laufende Arbeiten im Hintergrund ausgeführt werden. Allerdings ist es wichtig, dass sich Hintergrund arbeiten nicht negativ auf die Benutzererfahrung des Geräts auswirken. 

Ein Hintergrund Auftrag könnte z. b. alle drei oder vier Minuten eine Website Abfragen, um Änderungen an einem bestimmten Dataset abzufragen. Dies scheint unschädlich zu sein, würde jedoch eine katastrophale Auswirkung auf die Akku Lebensdauer haben. Die Anwendung wird das Gerät wiederholt reaktivieren, die CPU auf einen höheren Energiezustand herauf Stufen, die Radios einschalten, die Netzwerk Anforderungen durchführen und dann die Ergebnisse verarbeiten. Dies ist schlimmer, da das Gerät nicht sofort herunter Netz und wieder in den Energiespar Zustand wechselt. Eine schlecht geplante Hintergrundarbeit kann das Gerät versehentlich in einem Zustand mit unnötigen und übermäßigen Energieanforderungen halten. Diese scheinbar unschuldige Aktivität (das Abrufen einer Website) führt dazu, dass das Gerät in einem relativ kurzen Zeitraum unbrauchbar wird.

Android stellt die folgenden APIs bereit, um die Arbeit im Hintergrund zu unterstützen. Sie sind jedoch allein für die intelligente Auftragsplanung nicht ausreichend. 

- Beabsichtigte **[Dienste](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Beabsichtigte Dienste eignen sich hervorragend für die Arbeit, aber Sie bieten keine Möglichkeit, die Arbeit zu planen.
- **[Alarmmanager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; Diese APIs ermöglichen nur das Planen von Aufgaben, bieten aber keine Möglichkeit, die Arbeit tatsächlich auszuführen. Außerdem lässt der Alarmmanager zeitbasierte Einschränkungen nur zu, was bedeutet, dass ein Alarm zu einem bestimmten Zeitpunkt oder nach Ablauf einer bestimmten Zeitspanne ausgelöst wird. 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; Der JobSchedule ist eine großartige API, die mit dem Betriebssystem zusammenarbeitet, um Aufträge zu planen. Sie ist jedoch nur für Android-Apps verfügbar, die auf API-Ebene 21 oder höher ausgerichtet sind. 
- **[Broadcast Empfänger](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; Eine Android-App kann Broadcast Empfänger einrichten, um Aufgaben als Reaktion auf systemweite Ereignisse oder Intents auszuführen. Broadcast Empfänger bieten jedoch keine Kontrolle darüber, wann der Auftrag ausgeführt werden soll. Außerdem beschränken Änderungen im Android-Betriebssystem, wenn Broadcast Empfänger funktionieren, oder die Arten von arbeiten, auf die Sie reagieren können. 

Es gibt zwei wichtige Features für die effiziente Durchführung von Hintergrund arbeiten (manchmal auch als _Hintergrund Auftrag_ oder _Auftrag_bezeichnet):

1. **Intelligent Planen der Arbeit** &ndash; Es ist wichtig, dass eine Anwendung im Hintergrundaufgaben durchführt, die dies als guter Bürger bewirkt. Im Idealfall sollte die Anwendung nicht verlangen, dass ein Auftrag ausgeführt wird. Stattdessen sollte die Anwendung Bedingungen angeben, die erfüllt sein müssen, wenn der Auftrag ausgeführt werden kann, und dann die Ausführung planen, wenn die Bedingungen erfüllt sind. Dadurch kann Android Intelligent Arbeiten ausführen. Beispielsweise können Netzwerk Anforderungen in einem Batch zusammengefasst werden, um alle gleichzeitig auszuführen, um einen maximalen Aufwand für das Netzwerk zu ermöglichen.
2. **Kapseln der Arbeit** &ndash; Der Code zum Ausführen der Hintergrundarbeit sollte in einer diskreten Komponente gekapselt werden, die unabhängig von der Benutzeroberfläche ausgeführt werden kann und relativ einfach neu geplant werden kann, wenn die Arbeit aus irgendeinem Grund nicht vollständig ausgeführt werden kann.

Der Firebase Job Dispatcher ist eine Bibliothek von Google, die eine fließende API bereitstellt, um die Planung von Hintergrundaufgaben zu vereinfachen. Er soll als Ersatz für Google Cloud Manager dienen. Der Firebase-Auftrags Verteiler besteht aus den folgenden APIs:

- Eine `Firebase.JobDispatcher.JobService` ist eine abstrakte Klasse, die mit der Logik erweitert werden muss, die im Hintergrund Auftrag ausgeführt wird.
- Ein `Firebase.JobDispatcher.JobTrigger` deklariert, wann der Auftrag gestartet werden soll. Dies wird in der Regel als Zeitfenster ausgedrückt. warten Sie z. b. mindestens 30 Sekunden, bevor Sie den Auftrag starten, aber führen Sie den Auftrag innerhalb von 5 Minuten aus.
- Ein `Firebase.JobDispatcher.RetryStrategy` enthält Informationen zu den Aktionen, die durchgeführt werden sollten, wenn ein Auftrag nicht ordnungsgemäß ausgeführt wird. Die Wiederholungs Strategie gibt an, wie lange gewartet werden soll, bevor versucht wird, den Auftrag erneut auszuführen. 
- Ein `Firebase.JobDispatcher.Constraint` ist ein optionaler Wert, der eine Bedingung beschreibt, die erfüllt sein muss, bevor der Auftrag ausgeführt werden kann, z. b. wenn sich das Gerät in einem nicht gemessenen Netzwerk oder in einem gebührenpflichtigen Netzwerk befindet
- Ist eine API, die die vorherigen APIs in zu einer Arbeitseinheit vereinigt, die `JobDispatcher`vom geplant werden kann. `Firebase.JobDispatcher.Job` Die `Job.Builder` -Klasse wird verwendet, um eine `Job`zu instanziieren.
- Eine `Firebase.JobDispatcher.JobDispatcher` verwendet die vorherigen drei APIs, um die Arbeit mit dem Betriebssystem zu planen und um ggf. Aufträge abzubrechen.

Um die Arbeit mit dem Firebase-Auftrags Verteiler zu planen, muss eine xamarin. Android-Anwendung den Code in einem Typ Kapseln, `JobService` der die-Klasse erweitert. `JobService`verfügt über drei Lebenszyklus Methoden, die während der Lebensdauer des Auftrags aufgerufen werden können:

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; Diese Methode ist der Ort, an dem die Arbeit stattfindet und immer implementiert werden sollte. Es wird im Haupt Thread ausgeführt. Diese Methode gibt zurück `true` , wenn verbleibende Arbeit vorhanden ist `false` , oder, wenn die Arbeit erledigt ist. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; Dies wird aufgerufen, wenn der Auftrag aus irgendeinem Grund beendet wird. Er sollte zurück `true` geben, wenn der Auftrag für einen späteren Zeitpunkt neu geplant werden soll.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** Diese Methode wird aufgerufen, wenn `JobService` die asynchrone Arbeit abgeschlossen hat. &ndash; 

Zum Planen eines Auftrags wird ein `JobDispatcher` -Objekt von der Anwendung instanziiert. Anschließend wird ein `Job.Builder` -Objekt zum Erstellen eines `Job` -Objekts verwendet, das für den `JobDispatcher` bereitgestellt wird, der die Ausführung des Auftrags plant.

In diesem Handbuch wird erläutert, wie Sie den Firebase-Auftrags Verteiler einer xamarin. Android-Anwendung hinzufügen und ihn zum Planen der Hintergrundarbeit verwenden.

## <a name="requirements"></a>Anforderungen

Der Firebase-Auftrags Verteiler erfordert Android-API-Ebene 9 oder höher. Die Firebase-Auftrags Verteiler Bibliothek basiert auf einigen Komponenten, die von Google Play Services bereitgestellt werden. auf dem Gerät müssen Google Play Services installiert sein.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Verwenden der Firebase-Auftrags Verteiler Bibliothek in xamarin. Android

Fügen Sie zunächst das [nuget-Paket xamarin. Firebase. jobdispatcher](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) dem xamarin. Android-Projekt hinzu, um mit dem Firebase-Auftrags Verteiler loszulegen. Suchen Sie im nuget-Paket-Manager nach dem **xamarin. Firebase. jobdispatcher** -Paket (das sich noch in der Vorabversion befindet).

Nachdem Sie die Firebase-Auftrags Verteiler Bibliothek hinzugefügt haben `JobService` , erstellen Sie eine-Klasse, und planen Sie die Ausführung `FirebaseJobDispatcher`mit einer Instanz von.


### <a name="creating-a-jobservice"></a>Erstellen eines Jobservice

Alle Aufgaben, die von der Firebase-Auftrags Verteiler Bibliothek durchgeführt werden, müssen in einem Typ `Firebase.JobDispatcher.JobService` ausgeführt werden, der die abstrakte-Klasse erweitert. Das Erstellen `JobService` einer ähnelt sehr der Erstellung eines `Service` mit dem Android-Framework: 

1. Erweitern der `JobService` Klasse
2. Ergänzen Sie die-Unterklasse `ServiceAttribute`mit dem. Obwohl es nicht unbedingt erforderlich ist, wird empfohlen, den `Name` -Parameter explizit festzulegen, um beim `JobService`Debuggen von zu helfen. 
3. Fügen Sie hinzu `JobService`, um in der Datei "androidmanifest. xml" zu deklarieren `IntentFilter` . Dadurch wird auch die Firebase-Auftrags Verteiler Bibliothek dazu verwendet, die `JobService`zu finden und aufzurufen.

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

Bevor eine Arbeit geplant werden kann, muss ein `Firebase.JobDispatcher.FirebaseJobDispatcher` -Objekt erstellt werden. Der `FirebaseJobDispatcher` ist für die Planung einer `JobService`verantwortlich. Der folgende Code Ausschnitt ist eine Möglichkeit, eine Instanz von `FirebaseJobDispatcher`zu erstellen: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Im vorherigen Code Ausschnitt ist die `GooglePlayDriver` Klasse, die die `FirebaseJobDispatcher` Interaktion mit einigen der Planungs-APIs in Google Play Services auf dem Gerät unterstützt. Der Parameter `context` ist eine beliebige `Context`Android-Aktivität, z. b. eine Aktivität. Derzeit ist die einzige `IDriver` Implementierung in der Firebase-Auftrags Verteiler Bibliothek. `GooglePlayDriver` 

Die xamarin. Android-Bindung für den Firebase-Auftrags Verteiler stellt eine Erweiterungsmethode zum Erstellen `FirebaseJobDispatcher` eines aus `Context`der bereit: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Nachdem der `FirebaseJobDispatcher` instanziiert wurde, ist es möglich, einen `Job` zu erstellen und den Code in der `JobService` -Klasse auszuführen. Die `Job` wird von einem `Job.Builder` -Objekt erstellt und wird im nächsten Abschnitt erläutert.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Erstellen eines Firebase. jobdispatcher. Job mit "Job. Builder"

Die `Firebase.JobDispatcher.Job` -Klasse ist für das Kapseln der Metadaten, die zum Ausführen eines `JobService`erforderlich sind, verantwortlich. Ein`Job` enthält Informationen wie alle Einschränkungen, die erfüllt sein müssen, bevor der Auftrag ausgeführt werden kann, `Job` wenn wiederholt wird, oder alle Trigger, die dazu führen, dass der Auftrag ausgeführt wird.  Als Minimum muss `Job` ein- _Tag_ (eine eindeutige Zeichenfolge, die `FirebaseJobDispatcher`den Auftrag für identifiziert) und der Typ des `JobService` -Typs aufweisen, der ausgeführt werden soll. Der Firebase-Auftrags Verteiler instanziiert `JobService` , wann der Auftrag ausgeführt werden soll.  Ein `Job` wird mithilfe einer Instanz `Firebase.JobDispatcher.Job.JobBuilder` der-Klasse erstellt. 

Der folgende Code Ausschnitt ist das einfachste Beispiel für das Erstellen eines `Job` mithilfe der xamarin. Android-Bindung:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

`Job.Builder` Führt einige grundlegende Validierungs Überprüfungen für die Eingabewerte für den Auftrag aus. Eine-Ausnahme wird ausgelöst `Job.Builder` , wenn es nicht möglich ist, eine `Job`zu erstellen.  `Job.Builder` Erstellt eine`Job` mit den folgenden Standardwerten:

- Die Lebens _Dauer_ (wie lange die Ausführung geplant wird) erfolgt erst, &ndash; wenn das Gerät neu gestartet wird, nachdem das Gerät neu gestartet `Job` wurde. `Job`
- Ein `Job` wird nicht wieder &ndash; holt. er wird nur einmal ausgeführt.
- Eine `Job` wird so geplant, dass Sie so schnell wie möglich ausgeführt wird.
- Die Standard Wiederholungs Strategie für einen `Job` besteht in der Verwendung eines _exponentiellen Backoff_ (weitere Details finden Sie weiter unten im Abschnitt [Festlegen einer retrystrategy](#Setting_a_RetryStrategy)).

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Nachdem die `Job`erstellt wurde, muss Sie mit dem `FirebaseJobDispatcher` geplant werden, bevor Sie ausgeführt wird. Es gibt zwei Methoden zum Planen einer `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Der Wert, der `FirebaseJobDispatcher.Schedule` von zurückgegeben wird, ist einer der folgenden ganzzahligen Werte:

- `FirebaseJobDispatcher.ScheduleResultSuccess`&ndash; Das`Job` wurde erfolgreich geplant.
- `FirebaseJobDispatcher.ScheduleResultUnknownError`Es ist ein unbekanntes Problem aufgetreten, `Job` das verhindert hat, dass die geplant wurde. &ndash;
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable`Ein ungültiger `IDriver` wurde verwendet, `IDriver` oder der war nicht verfügbar. &ndash; 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger`&ndash; Der`Trigger` wurde nicht unterstützt.
- `FirebaseJobDispatcher.ScheduleResultBadService`&ndash; Der Dienst ist nicht ordnungsgemäß konfiguriert oder nicht verfügbar.
 
### <a name="configuring-a-job"></a>Konfigurieren eines Auftrags

Es ist möglich, einen Auftrag anzupassen. Beispiele für die Anpassung eines Auftrags:

- [Übergeben von Parametern an einen Auftrag](#Passing_Parameters_to_a_Job) &ndash; Eine`Job` benötigt möglicherweise zusätzliche Werte, um Ihre Arbeit auszuführen, z. b. das Herunterladen einer Datei.
- [Einschränkungen festlegen](#Setting_Constraints) &ndash; Es kann erforderlich sein, nur einen Auftrag auszuführen, wenn bestimmte Bedingungen erfüllt sind. Führen Sie z. b. `Job` nur eine aus, wenn das Gerät berechnet wird. 
- [Geben `Job` ](#Setting_Job_Triggers) Sie an, wann die von der Firebase-Auftrags Verteilung ausgeführt &ndash; werden soll, damit Anwendungen einen Zeitpunkt angeben können, zu dem der Auftrag ausgeführt werden soll.  
- [Deklarieren einer Wiederholungs Strategie für fehlgeschlagene Aufträge](#Setting_a_RetryStrategy) Eine Wiederholungs _Strategie_ bietet Anleitungen für die Vorgehens `FirebaseJobDispatcher` Weise, mit `Jobs` der Sie nicht fertiggestellt werden können. &ndash; 

Diese Themen werden in den folgenden Abschnitten ausführlicher erläutert.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Übergeben von Parametern an einen Auftrag

Parameter werden an einen Auftrag weitergegeben, indem `Bundle` ein erstellt wird, der zusammen `Job.Builder.SetExtras` mit der-Methode weitergegeben wird:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Der `Bundle` Zugriff auf die erfolgt `IJobParameters.Extras` über `OnStartJob` die-Eigenschaft der-Methode:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Festlegen von Einschränkungen

Einschränkungen können dabei helfen, die Kosten oder den Akku Abfluss auf dem Gerät zu verringern. Die `Firebase.JobDispatcher.Constraint` -Klasse definiert diese Einschränkungen als ganzzahlige Werte:

- `Constraint.OnUnmeteredNetwork`&ndash; Führen Sie den Auftrag nur aus, wenn das Gerät mit einem nicht gemessenen Netzwerk verbunden ist. Dies ist hilfreich, um zu verhindern, dass Benutzerdaten Gebühren verursachen.
- `Constraint.OnAnyNetwork`&ndash; Führen Sie den Auftrag in dem Netzwerk aus, mit dem das Gerät verbunden ist. Wenn dieser Wert zusammen `Constraint.OnUnmeteredNetwork`mit angegeben wird, hat dieser Wert Vorrang.
- `Constraint.DeviceCharging`&ndash; Führen Sie den Auftrag nur aus, wenn das Gerät berechnet wird.

Einschränkungen werden mit der `Job.Builder.SetConstraint` -Methode festgelegt: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

`JobTrigger` Bietet Anleitungen für das Betriebssystem, wann der Auftrag gestartet werden soll. Ein `JobTrigger` verfügt über ein _ausführendes Fenster_ , das einen geplanten Zeitpunkt für `Job` die Ausführung von definiert. Das Ausführungs Fenster verfügt über einen _Startfenster_ Wert und einen _Endfenster_ Wert. Das Startfenster gibt an, wie viele Sekunden das Gerät warten soll, bevor der Auftrag ausgeführt wird, und der Wert des endfensters ist die maximale Anzahl von Sekunden, `Job`die gewartet werden soll, bevor der ausgeführt wird. 

Mit `JobTrigger` der`Firebase.Jobdispatcher.Trigger.ExecutionWindow` -Methode kann ein erstellt werden.  Dies bedeutet `Trigger.ExecutionWindow(15,60)` beispielsweise, dass der Auftrag zwischen 15 und 60 Sekunden nach dem geplanten Zeitplan ausgeführt werden soll. Die `Job.Builder.SetTrigger` -Methode wird verwendet, um 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Der Standard `JobTrigger` Wert für einen Auftrag wird durch den- `Trigger.Now`Wert dargestellt, der angibt, dass ein Auftrag nach der Planung so schnell wie möglich ausgeführt wird.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Festlegen einer Wiederholungs Strategie

`Firebase.JobDispatcher.RetryStrategy` Wird verwendet, um anzugeben, wie lange ein Gerät verwendet werden soll, bevor versucht wird, einen fehlgeschlagenen Auftrag erneut auszuführen. Eine `RetryStrategy` verfügt über eine _Richtlinie_, mit der definiert wird, welcher Zeit Basis Algorithmus zum erneuten Planen des fehlerhaften Auftrags verwendet wird, und ein Ausführungs Fenster, das ein Fenster angibt, in dem der Auftrag geplant werden soll. Dieses _neuplanungs Fenster_ wird durch zwei Werte definiert. Der erste Wert ist die Anzahl der Sekunden, die gewartet wird, bevor der Auftrag neu geplant wird (der _anfängliche Backoff_ -Wert), und die zweite Zahl ist die maximale Anzahl von Sekunden, bevor der Auftrag ausgeführt werden muss (der _Maximale Backoff_ -Wert). 
 
Die zwei Arten von Wiederholungs Richtlinien werden durch diese int-Werte identifiziert:

- `RetryStrategy.RetryPolicyExponential`Eine _exponentielle Backoff_ -Richtlinie erhöht den anfänglichen Backoff-Wert exponentiell nach jedem Fehler. &ndash; Wenn ein Auftrag zum ersten Mal fehlschlägt, wartet die Bibliothek das _initial-Intervall, das vor der erneuten Planung &ndash; des Auftrags Beispiels 30 Sekunden angegeben wird. Wenn der Auftrag zum zweiten Mal fehlschlägt, wartet die Bibliothek mindestens 60 Sekunden, bevor versucht wird, den Auftrag auszuführen. Nach dem dritten fehlgeschlagenen Versuch wartet die Bibliothek 120 Sekunden usw. Der Standard `RetryStrategy` Wert für die Firebase-Auftrags Verteiler Bibliothek wird durch das `RetryStrategy.DefaultExponential` -Objekt dargestellt. Er verfügt über einen anfänglichen Backoff von 30 Sekunden und einen maximalen Backoff von 3600 Sekunden.
- `RetryStrategy.RetryPolicyLinear`Diese Strategie ist ein _lineares Backoff_ , dass der Auftrag für die Ausführung in festgelegten Intervallen neu geplant werden soll (bis er erfolgreich ausgeführt wird). &ndash; Der lineare Backoff eignet sich am besten für die Arbeit, die so bald wie möglich abgeschlossen werden muss, oder für Probleme, die sich schnell auflösen. Die Firebase-Auftrags Verteiler Bibliothek definiert einen `RetryStrategy.DefaultLinear` -Wert, der über ein Fenster für die Neuplanung von mindestens 30 Sekunden und bis zu 3600 Sekunden verfügt.

Es ist möglich, mit der `RetryStrategy` `FirebaseJobDispatcher.NewRetryStrategy` -Methode eine benutzerdefinierte zu definieren. Es werden drei Parameter benötigt:

1. `int policy``RetryStrategy.RetryPolicyLinear` `RetryStrategy.RetryPolicyExponential` `RetryStrategy` Die Richtlinie ist einer der vorherigen Werte, oder. &ndash;
2. `int initialBackoffSeconds`Der _anfängliche Backoff_ ist eine Verzögerung (in Sekunden), die erforderlich ist, bevor versucht wird, den Auftrag erneut auszuführen. &ndash; Der Standardwert hierfür ist 30 Sekunden. 
3. `int maximumBackoffSeconds`Der _Maximale Backoff_ -Wert deklariert die maximale Anzahl von Sekunden, die verzögert werden soll, bevor versucht wird, den Auftrag erneut auszuführen. &ndash; Der Standardwert ist 3600 Sekunden. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Abbrechen eines Auftrags

Es ist möglich, alle geplanten Aufträge oder nur einen einzelnen Auftrag mit der `FirebaseJobDispatcher.CancelAll()` -Methode oder der `FirebaseJobDispatcher.Cancel(string)` -Methode abzubrechen:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Beide Methoden geben einen ganzzahligen Wert zurück:

- `FirebaseJobDispatcher.CancelResultSuccess`&ndash; Der Auftrag wurde erfolgreich abgebrochen.
- `FirebaseJobDispatcher.CancelResultUnknownError`&ndash; Ein Fehler hat verhindert, dass der Auftrag abgebrochen wird.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable`Der kann den Auftrag nicht abbrechen, weil kein gültiger `IDriver` verfügbar ist. `FirebaseJobDispatcher` &ndash;

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wurde erläutert, wie der Firebase-Auftrags Verteiler verwendet wird, um im Hintergrund arbeiten Intelligent auszuführen. Es wurde erläutert, wie `JobService` die auszuführenden Aufgaben als und `FirebaseJobDispatcher` verwendet werden, um die Arbeit zu planen, die Kriterien mit einem `JobTrigger` anzugeben und zu bestimmen, wie Fehler mit einem `RetryStrategy`behandelt werden sollen.


## <a name="related-links"></a>Verwandte Links

- [Xamarin. Firebase. jobdispatcher für nuget](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [Firebase-Job-Dispatcher auf GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin. Firebase. jobdispatcher-Bindung](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Intelligente Auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [Optimierungen für Android-Akku und Arbeitsspeicher-Google I/O 2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
