---
title: Firebase Job Dispatcher
description: In diesem Leitfaden wird erläutert, wie Sie Hintergrundarbeit mit der Firebase Job Dispatcher-Bibliothek von Google planen.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 06/05/2018
ms.openlocfilehash: 280fe11f935db0a364f3342b22bb9544cdda1e6d
ms.sourcegitcommit: 9ee02a2c091ccb4a728944c1854312ebd51ca05b
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/10/2020
ms.locfileid: "73020244"
---
# <a name="firebase-job-dispatcher"></a>Firebase Job Dispatcher

_In diesem Leitfaden wird erläutert, wie Sie Hintergrundarbeit mit der Firebase Job Dispatcher-Bibliothek von Google planen._

## <a name="overview"></a>Übersicht

Eine der besten Möglichkeiten, die Reaktionsfähigkeit einer Android-Anwendung für einen Benutzer aufrechtzuerhalten, besteht darin, komplexe oder lange dauernde Arbeit im Hintergrund auszuführen. Allerdings ist es wichtig, dass sich Hintergrundarbeit nicht negativ auf die Erfahrung auswirkt, die der Benutzer mit dem Gerät macht. 

Eine Hintergrundaufgabe könnte beispielsweise alle drei oder vier Minuten eine Website abfragen, um auf Änderungen an einem bestimmten Dataset zu prüfen. Dies scheint harmlos zu sein, würde jedoch eine katastrophale Auswirkung auf die Akkulaufzeit haben. Die Anwendung wird das Gerät wiederholt reaktivieren, die CPU auf einen höheren Energiezustand bringen, die Sendegeräte einschalten, die Netzwerkanforderungen vornehmen und dann die Ergebnisse verarbeiten. Es wird noch schlimmer, da das Gerät nicht sofort ausgeschaltet und in den stromsparenden Leerlaufzustand zurückversetzt wird. Eine schlecht geplante Hintergrundarbeit kann unbeabsichtigterweise bewirken, dass das Gerät in einem Zustand mit unnötigem und übermäßigem Energiebedarf verbleibt. Diese scheinbar unschuldige Aktivität (das zyklische Abrufen einer Website) führt dazu, dass das Gerät in relativ kurzer Zeit unbrauchbar wird.

Android stellt die folgenden APIs bereit, die das Ausführen von Arbeit im Hintergrund unterstützen, aber diese allein reichen nicht für eine intelligente Auftragsplanung aus. 

- **[Intent-Dienste](~/android/app-fundamentals/services/creating-a-service/intent-services.md)** &ndash; Intent-Dienste eignen sich hervorragend zum Ausführen der Arbeit, aber sie bieten keine Möglichkeit, die Arbeit zu planen.
- **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)** &ndash; diese APIs ermöglichen nur das Planen von Arbeit, bieten aber keine Möglichkeit, die Arbeit tatsächlich auszuführen. Außerdem unterstützt AlarmManager nur zeitbasierte Einschränkungen, was bedeutet, dass ein Alarm zu einem bestimmten Zeitpunkt oder nach Ablauf einer bestimmten Zeitspanne ausgelöst wird. 
- **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)** &ndash; der JobScheduler ist eine großartige API, die mit dem Betriebssystem zusammenarbeitet, um Aufträge zu planen. Sie ist jedoch nur für Android-Apps verfügbar, die auf API-Ebene 21 oder höher ausgerichtet sind. 
- **[Broadcast Receiver](~/android/app-fundamentals/broadcast-receivers.md)** &ndash; über eine Android-App können Broadcast Receiver so eingerichtet werden, dass sie Arbeit als Reaktion auf systemweite Ereignisse oder Intent-Objekte ausführen. Mit Broadcast Receivern kann jedoch nicht gesteuert werden, wann der jeweilige Auftrag ausgeführt werden soll. Änderungen im Android-Betriebssystem beschränken außerdem, wann Broadcast Receiver funktionieren oder auf welche Arten von Arbeit sie reagieren können. 

Es gibt zwei Hauptaspekte für ein effizientes Ausführen von Hintergrundarbeit (manchmal auch als _Hintergrundauftrag_ oder _Auftrag_ bezeichnet):

1. **Intelligentes Planen der Arbeit** &ndash; beachten Sie unbedingt, dass eine Anwendung Arbeit im Hintergrund möglichst reibungslos ausführt. Im Idealfall sollte die Anwendung das Ausführen eines Auftrags nicht anfordern. Stattdessen sollte die Anwendung Bedingungen angeben, die erfüllt sein müssen, damit der Auftrag ausgeführt werden kann, und dann planen, dass diese Arbeit ausgeführt wird, wenn die Bedingungen erfüllt sind. So kann Android intelligent Arbeiten ausführen. Beispielsweise können Netzwerkanforderungen in einem Batch zusammengefasst werden, damit alle gleichzeitig ausgeführt werden und so der netzwerkbedingte Mehraufwand (Overhead) minimiert wird.
2. **Kapseln der Arbeit** &ndash; der Code zum Ausführen der Hintergrundarbeit sollte in einer diskreten Komponente gekapselt sein, die unabhängig von der Benutzeroberfläche ausgeführt werden kann und relativ einfach neu zu planen ist, wenn die Arbeit aus irgendeinem Grund nicht vollständig ausgeführt werden kann.

Der Firebase Job Dispatcher ist eine Bibliothek von Google, die eine Fluent-API bereitstellt, um die Planung von Hintergrundarbeit zu vereinfachen. Er soll als Ersatz für Google Cloud Manager dienen. Der Firebase Job Dispatcher besteht aus folgenden APIs:

- Ein `Firebase.JobDispatcher.JobService` ist eine abstrakte Klasse, die mit der Logik erweitert werden muss, die im Hintergrundauftrag ausgeführt wird.
- Ein `Firebase.JobDispatcher.JobTrigger` deklariert, wann der Auftrag gestartet werden soll. Dies wird in der Regel als Zeitfenster ausgedrückt. Warten Sie z. B. mindestens 30 Sekunden, bevor Sie den Auftrag starten, aber führen Sie den Auftrag innerhalb von 5 Minuten aus.
- Eine `Firebase.JobDispatcher.RetryStrategy` enthält Informationen zu den Aktionen, die durchgeführt werden sollten, wenn ein Auftrag nicht ordnungsgemäß ausgeführt wird. Die Wiederholungsstrategie gibt an, wie lange gewartet werden soll, bevor versucht wird, den Auftrag erneut auszuführen. 
- Ein `Firebase.JobDispatcher.Constraint` ist ein optionaler Wert, der eine Bedingung beschreibt, die erfüllt sein muss, bevor der Auftrag ausgeführt werden kann, z. B. wenn sich das Gerät in einem nicht getakteten Netzwerk befindet oder geladen wird.
- Der `Firebase.JobDispatcher.Job` ist eine API, die die vorherigen APIs in einer Arbeitseinheit vereinigt, die vom `JobDispatcher` geplant werden kann. Die `Job.Builder`-Klasse wird verwendet, um einen `Job` zu instanziieren.
- Ein `Firebase.JobDispatcher.JobDispatcher` verwendet die vorherigen drei APIs, um die Arbeit mit dem Betriebssystem zu planen und ggf. eine Möglichkeit zum Abbrechen von Aufträgen zu schaffen.

Um Arbeit mit dem Firebase Job Dispatcher planen zu können, muss der Code einer Xamarin.Android-Anwendung in einem Typ gekapselt sein, der die `JobService`-Klasse erweitert. `JobService` hat drei Lebenszyklusmethoden, die während der Lebensdauer des Auftrags aufgerufen werden können:

- **`bool OnStartJob(IJobParameters parameters)`** &ndash; in dieser Methode findet die Arbeit statt, und sie sollte immer implementiert werden. Sie wird im Hauptthread ausgeführt. Diese Methode gibt `true` zurück, wenn noch Arbeit übrig ist, oder `false`, wenn die Arbeit erledigt ist. 
- **`bool OnStopJob(IJobParameters parameters)`** &ndash; dies wird aufgerufen, wenn der Auftrag aus irgendeinem Grund beendet wird. Es sollte `true` zurückgegeben werden, wenn der Auftrag für einen späteren Zeitpunkt neu geplant werden soll.
- **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; diese Methode wird aufgerufen, wenn der `JobService` alle asynchronen Arbeiten abgeschlossen hat. 

Um einen Auftrag zu planen, instanziiert die Anwendung ein `JobDispatcher`-Objekt. Anschließend wird ein `Job.Builder` verwendet, um ein `Job`-Objekt zu erstellen, das dem `JobDispatcher` bereitgestellt wird, der versucht, die Ausführung des Auftrags zu planen.

In diesem Leitfaden wird erläutert, wie Sie den Firebase Job Dispatcher einer Xamarin.Android-Anwendung hinzufügen und ihn zum Planen der Hintergrundarbeit verwenden.

## <a name="requirements"></a>Anforderungen

Der Firebase Job Dispatcher setzt Android-API-Ebene 9 oder höher voraus. Die Firebase Job Dispatcher-Bibliothek basiert auf einigen Komponenten, die von Google Play Services bereitgestellt werden. Auf dem Gerät müssen Google Play Services installiert sein.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Verwenden der Firebase Job Dispatcher-Bibliothek in Xamarin.Android

Um mit dem Firebase Job Dispatcher zu beginnen, fügen Sie zunächst das [Xamarin.Firebase.JobDispatcher-NuGet-Paket](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) dem Xamarin.Android-Projekt hinzu. Suchen Sie im NuGet-Paket-Manager nach dem **Xamarin.Firebase.JobDispatcher**-Paket (das sich noch in der Vorabversion befindet).

Nachdem Sie die Firebase Job Dispatcher-Bibliothek hinzugefügt haben, erstellen Sie eine `JobService`-Klasse, und planen Sie die Ausführung mit einer Instanz des `FirebaseJobDispatcher`.

### <a name="creating-a-jobservice"></a>Erstellen eines JobService

Alle Aufgaben, die von der Firebase Job Dispatcher-Bibliothek ausgeführt werden, müssen in einem Typ ausgeführt werden, der die abstrakte `Firebase.JobDispatcher.JobService`-Klasse erweitert. Das Erstellen eines `JobService`-Objekts ist dem Erstellen eines `Service`-Objekts mit dem Android-Framework sehr ähnlich: 

1. Erweitern Sie die `JobService`-Klasse.
2. Ergänzen Sie die Unterklasse mit dem `ServiceAttribute`. Obwohl es nicht unbedingt erforderlich ist, sollten Sie den `Name`-Parameter explizit festlegen, um beim Debuggen des `JobService` zu helfen. 
3. Fügen Sie einen `IntentFilter` hinzu, um den `JobService` in der **AndroidManifest.xml**-Datei zu deklarieren. Dies unterstützt die Firebase Job Dispatcher-Bibliothek auch dabei, den `JobService` zu finden und aufzurufen.

Der folgende Code ist ein Beispiel für das einfachste `JobService`-Objekt für eine Anwendung, wobei die TPL verwendet wird, um einige Arbeit asynchron auszuführen:

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

### <a name="creating-a-firebasejobdispatcher"></a>Erstellen eines FirebaseJobDispatcher

Bevor eine Arbeit geplant werden kann, muss ein `Firebase.JobDispatcher.FirebaseJobDispatcher`-Objekt erstellt werden. Der `FirebaseJobDispatcher` ist für die Planung eines `JobService` verantwortlich. Der folgende Codeausschnitt zeigt eine Möglichkeit, eine Instanz des `FirebaseJobDispatcher` zu erstellen: 

 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Im vorherigen Codeausschnitt ist der `GooglePlayDriver` eine Klasse, die die `FirebaseJobDispatcher`-Interaktion mit einigen der Planungs-APIs in Google Play Services auf dem Gerät unterstützt. Der Parameter `context` ist ein beliebiger Android-`Context`, z. B. eine Aktivität. Derzeit ist der `GooglePlayDriver` die einzige `IDriver`-Implementierung in der Firebase Job Dispatcher-Bibliothek. 

Die Xamarin.Android-Bindung für den Firebase Job Dispatcher stellt eine Erweiterungsmethode zum Erstellen eines `FirebaseJobDispatcher` aus dem `Context` bereit: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Nachdem der `FirebaseJobDispatcher` instanziiert wurde, ist es möglich, einen `Job` zu erstellen und den Code in der `JobService`-Klasse auszuführen. Der `Job` wird von einem `Job.Builder`-Objekt erstellt und im nächsten Abschnitt erläutert.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Erstellen eines Firebase.JobDispatcher.Job mit dem Job.Builder

Die `Firebase.JobDispatcher.Job`-Klasse ist verantwortlich für das Kapseln der Metadaten, die erforderlich sind, um einen `JobService` auszuführen. Ein `Job` enthält Informationen wie jede Einschränkung, die erfüllt sein muss, bevor der Auftrag ausgeführt werden kann, wenn der `Job` wiederholt wird, oder alle Trigger, die dazu führen, dass der Auftrag ausgeführt wird.  Das absolute Minimum, worüber ein `Job` verfügen muss, ist ein _Tag_ (eine eindeutige Zeichenfolge, die den Auftrag für den `FirebaseJobDispatcher` identifiziert), und der Typ des `JobService`, der ausgeführt werden sollte. Der Firebase Job Dispatcher instanziiert den `JobService`, wenn es an der Zeit ist, den Auftrag auszuführen.  Ein `Job` wird mit einer Instanz der `Firebase.JobDispatcher.Job.JobBuilder`-Klasse erstellt. 

Der folgende Codeausschnitt ist das einfachste Beispiel für das Erstellen eines `Job` mithilfe der Xamarin.Android-Bindung:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

Der `Job.Builder` führt einige grundlegende Überprüfungen der Eingabewerte des Auftrags aus. Eine Ausnahme wird ausgelöst, wenn der `Job.Builder` keinen `Job` erstellen kann.  Der `Job.Builder` erstellt einen `Job` mit den folgenden Standardeinstellungen:

- Die _Lebensdauer_ (wie lange seine Ausführung geplant wird) eines `Job` endet beim Neustart des Geräts &ndash; sobald das Gerät neu gestartet wird, geht der `Job` verloren.
- Ein `Job` wird nicht wiederholt &ndash; er wird nur einmal ausgeführt.
- Ein `Job` wird so geplant, dass er so schnell wie möglich ausgeführt wird.
- Die Standardwiederholungsstrategie für einen `Job` ist die Verwendung eines _exponentiellen Backoffs_ (weitere Details finden Sie weiter unten im Abschnitt [Festlegen einer RetryStrategy](#Setting_a_RetryStrategy)).

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Nachdem Sie den `Job` erstellt haben, muss er mit dem `FirebaseJobDispatcher` geplant werden, bevor er ausgeführt wird. Es gibt zwei Methoden zum Planen eines `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Einer der folgenden ganzzahligen Werte wird von `FirebaseJobDispatcher.Schedule` zurückgegeben:

- `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; der `Job` wurde erfolgreich geplant.
- `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; ein unbekanntes Problem ist aufgetreten, das das Planen des `Job` verhindert hat.
- `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; ein ungültiger `IDriver` wurde verwendet, oder der `IDriver` war nicht verfügbar. 
- `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; der `Trigger` wurde nicht unterstützt.
- `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; der Dienst ist nicht ordnungsgemäß konfiguriert oder nicht verfügbar.

### <a name="configuring-a-job"></a>Konfigurieren eines Auftrags

Es ist möglich, einen Auftrag anzupassen. Ein Auftrag kann beispielsweise folgendermaßen angepasst werden:

- [Übergeben von Parametern an einen Auftrag](#Passing_Parameters_to_a_Job) &ndash; ein `Job` erfordert möglicherweise zusätzliche Werte, um die Arbeit auszuführen, z. B. das Herunterladen einer Datei.
- [Festlegen von Einschränkungen](#Setting_Constraints) &ndash; es ist möglicherweise notwendig, einen Auftrag nur auszuführen, wenn bestimmte Bedingungen erfüllt sind. Es kann z. B. sein, dass ein `Job` nur dann ausgeführt werden soll, wenn das Gerät geladen wird. 
- [Angeben des Zeitpunkts, zu dem ein `Job` ausgeführt werden soll](#Setting_Job_Triggers) &ndash; der Firebase Job Dispatcher ermöglicht Anwendungen, eine Uhrzeit anzugeben, zu der der Auftrag ausgeführt werden soll.  
- [Deklarieren einer Wiederholungsstrategie für Aufträge, bei denen ein Fehler aufgetreten ist](#Setting_a_RetryStrategy) &ndash; eine _Wiederholungsstrategie_ weist den `FirebaseJobDispatcher` an, was mit `Jobs` geschehen soll, die nicht abgeschlossen wurden. 

Diese Themen werden in den folgenden Abschnitten ausführlich behandelt.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-parameters-to-a-job"></a>Übergeben von Parametern an einen Auftrag

Parameter werden an einen Auftrag übergeben, indem ein `Bundle`-Objekt erstellt wird, das zusammen mit der `Job.Builder.SetExtras`-Methode übergeben wird:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Der Zugriff auf das `Bundle`-Objekt erfolgt aus der `IJobParameters.Extras`-Eigenschaft in der `OnStartJob`-Methode:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Festlegen von Einschränkungen

Einschränkungen können dabei helfen, die Kosten oder die Akkuentleerung des Geräts zu verringern. Die `Firebase.JobDispatcher.Constraint`-Klasse definiert diese Einschränkungen als ganzzahlige Werte:

- `Constraint.OnUnmeteredNetwork` &ndash; der Auftrag soll nur ausgeführt werden, wenn das Gerät mit einem nicht getakteten Netzwerk verbunden ist. Dies ist hilfreich, um zu verhindern, dass Benutzer Datengebühren verursachen.
- `Constraint.OnAnyNetwork` &ndash; der Auftrag soll in jedem Netzwerk ausgeführt werden, mit dem das Gerät verbunden ist. Bei Angabe zusammen mit `Constraint.OnUnmeteredNetwork` hat dieser Wert Vorrang.
- `Constraint.DeviceCharging` &ndash; der Auftrag soll nur ausgeführt werden, wenn für das Gerät Gebühren berechnet werden.

Einschränkungen werden mit der `Job.Builder.SetConstraint`-Methode festgelegt: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

Der `JobTrigger` weist das Betriebssystem an, wann der Auftrag gestartet werden soll. Ein `JobTrigger` verfügt über ein _Ausführungsfenster_, das einen geplanten Zeitpunkt für die Ausführung des `Job` definiert. Das Ausführungsfenster umfasst Werte für ein _Startfenster_ und ein _Endfenster_. Das Startfenster entspricht der Anzahl der Sekunden, die das Gerät warten soll, bevor der Auftrag ausgeführt wird, und der Wert des Endfensters entspricht der maximalen Anzahl von Sekunden, die gewartet wird, bevor der `Job` ausgeführt wird. 

Ein `JobTrigger` kann mit der `Firebase.Jobdispatcher.Trigger.ExecutionWindow`-Methode erstellt werden.  `Trigger.ExecutionWindow(15,60)` bedeutet beispielsweise, dass der Auftrag zwischen 15 und 60 Sekunden nach dem Zeitpunkt der Planung ausgeführt werden sollte. Die `Job.Builder.SetTrigger`-Methode dient zu folgendem Zweck: 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Der Wert `Trigger.Now` ist der standardmäßige `JobTrigger` für einen Auftrag, der angibt, dass ein Auftrag nach der Planung so schnell wie möglich ausgeführt werden sollte.

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Festlegen einer RetryStrategy

Mit der `Firebase.JobDispatcher.RetryStrategy` wird angegeben, nach welcher Verzögerung ein Gerät versuchen sollte, einen nicht gelungenen Auftrag erneut auszuführen. Zu einer `RetryStrategy` gibt es eine _Richtlinie_, die definiert, mit welchem Zeitbasisalgorithmus ein nicht gelungener Auftrag erneut geplant wird, und ein Ausführungsfenster, für das der Auftrag geplant werden sollte. Dieses _Neuplanungsfenster_ wird durch zwei Werte definiert. Der erste Wert ist die Wartezeit in Sekunden, bevor der Auftrag neu geplant wird (der _anfängliche Backoff_-Wert), und der zweite Wert ist die maximale Anzahl von Sekunden, bevor der Auftrag ausgeführt werden muss (der _maximale Backoff_-Wert). 

Die zwei Typen von Wiederholungsrichtlinien werden durch diese Int-Werte identifiziert:

- `RetryStrategy.RetryPolicyExponential` &ndash; Eine Richtlinie für _exponentielles Backoff_ erhöht den anfänglichen Backoffwert exponentiell nach jedem Fehler. Wenn bei einem Auftrag erstmals ein Fehler auftritt, wartet die Bibliothek solange mit dem erneuten Planen des Auftrags, bis das angegebene _initial-Intervall abgelaufen ist &ndash; Beispiel: 30 Sekunden. Wenn bei dem Auftrag zum zweiten Mal ein Fehler auftritt, wartet die Bibliothek mindestens 60 Sekunden, bevor sie versucht, den Auftrag auszuführen. Nach dem dritten Fehlversuch wartet die Bibliothek 120 Sekunden usw. Der Standard `RetryStrategy` für die Firebase Job Dispatcher-Bibliothek wird durch das `RetryStrategy.DefaultExponential`-Objekt dargestellt. Es verfügt über einen anfänglichen Backoff von 30 Sekunden und einen maximalen Backoff von 3.600 Sekunden.
- `RetryStrategy.RetryPolicyLinear` &ndash; Diese Strategie ist ein _lineares Backoff_, dass das Ausführen des Auftrags in festgelegten Intervallen neu geplant werden soll (bis er erfolgreich ausgeführt wird). Lineares Backoff eignet sich am besten für Arbeit, die so bald wie möglich abgeschlossen werden muss, oder für Probleme, die sich schnell von selbst lösen. Die Firebase Job Dispatcher-Bibliothek definiert eine `RetryStrategy.DefaultLinear`, die ein Neuplanungsfenster von mindestens 30 Sekunden und bis zu 3.600 Sekunden aufweist.

Es ist möglich, eine benutzerdefinierte `RetryStrategy` mit der `FirebaseJobDispatcher.NewRetryStrategy`-Methode zu definieren. Sie nimmt drei Parameter an:

1. `int policy` &ndash; die _Richtlinien_ ist einer der vorherigen `RetryStrategy`-Werte, `RetryStrategy.RetryPolicyLinear` oder `RetryStrategy.RetryPolicyExponential`.
2. `int initialBackoffSeconds` &ndash; das _anfängliche Backoff_ ist eine erforderliche Verzögerung (in Sekunden), bevor versucht wird, den Auftrag erneut auszuführen. Der Standardwert hierfür ist 30 Sekunden. 
3. `int maximumBackoffSeconds` &ndash; der _maximale Backoff_-Wert deklariert die maximale Anzahl von Sekunden der Verzögerung vor dem Versuch, den Auftrag erneut auszuführen. Der Standardwert ist 3.600 Sekunden. 

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

- `FirebaseJobDispatcher.CancelResultSuccess` &ndash; der Auftrag wurde erfolgreich beendet.
- `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; ein Fehler hat verhindert, dass der Auftrag abgebrochen wurde.
- `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; der `FirebaseJobDispatcher` kann den Auftrag nicht abbrechen, weil kein gültiger `IDriver` verfügbar ist.

## <a name="summary"></a>Zusammenfassung

In diesem Leitfaden wird erläutert, wie Sie den Firebase Job Dispatcher verwenden, um Arbeit intelligent im Hintergrund auszuführen. Es wird erläutert, wie die auszuführende Arbeit als `JobService`-Objekt gekapselt und wie der `FirebaseJobDispatcher`-Dienst verwendet wird, um diese Arbeit zu planen, indem die Kriterien mit einem `JobTrigger` angegeben werden, und wie Fehler mit einer `RetryStrategy` verarbeitet werden sollten.

## <a name="related-links"></a>Verwandte Links

- [Xamarin.Firebase.JobDispatcher auf NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [firebase-job-dispatcher auf GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher-Bindung](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Intelligente Auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [Android Battery and Memory Optimizations – Google I/O 2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
