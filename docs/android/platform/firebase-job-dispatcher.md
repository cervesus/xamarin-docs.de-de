---
title: Firebase-Auftrag-Verteiler
description: Dieser Leitfaden erläutert die Vorgehensweise beim Planen der Verarbeitung im Hintergrund mithilfe der Firebase-Auftrag-Dispatcher-Bibliothek von Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/05/2018
ms.openlocfilehash: 4ae1fb71209f8116b17ee7e2cb44318ef790d831
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50116166"
---
# <a name="firebase-job-dispatcher"></a>Firebase-Auftrag-Verteiler

_Dieser Leitfaden erläutert die Vorgehensweise beim Planen der Verarbeitung im Hintergrund mithilfe der Firebase-Auftrag-Dispatcher-Bibliothek von Google._


## <a name="overview"></a>Übersicht

Eine der besten Möglichkeiten, eine Android-Anwendung, die dem Benutzer Reaktionsfähigkeit wird sichergestellt, dass komplexe oder lang ausgeführte Arbeit im Hintergrund ausgeführt wird. Allerdings ist es wichtig, dass die Verarbeitung im Hintergrund nicht negativ auf die benutzererfahrung mit dem Gerät beeinträchtigt wird. 

Beispielsweise kann ein Hintergrundauftrag eine Website alle drei oder vier Minuten an die Abfrage für Änderungen an ein bestimmtes Dataset abrufen. Dies scheint harmlos, jedoch eine verheerende Auswirkungen auf die Akkulaufzeit werden müssten. Die Anwendung wird wiederholt reaktiviert, das Gerät erhöhen die CPU auf einer höheren Energiezustand aktiviert, Einschalten auf den Funkempfang gestattet, stellen die netzwerkanforderungen, und klicken Sie dann die Ergebnisse verarbeiten. Es verschlimmert sich, da das Gerät wird nicht sofort schließen und zurück in den energiesparenden Leerlauf. Schlecht geplanten Hintergrundarbeit möglicherweise versehentlich des Geräts in einem Zustand mit unnötiges und übermäßiges Energieprofil gewährleisten. Diese scheinbar harmlose Aktivität (eine Website abrufen), wird das Gerät in relativ kurzer Zeit unbrauchbar werden.

Android bietet die folgenden APIs unterstützen Sie beim Arbeiten im Hintergrund ausführen, aber selbst keine ausreichend intelligent auftragsplanung. 

* **[Intent-Dienste](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; Intent-Dienste eignen sich hervorragend für die Arbeit auszuführen, jedoch keine Möglichkeit zum Planen der Arbeit bieten.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; diese APIs können nur die Arbeit geplant werden, jedoch bietet keine Möglichkeit, die die Arbeit zu erledigen. Außerdem ermöglicht der AlarmManager nur zeitbasierte Einschränkungen, was bedeutet einen Alarm auslösen, zu einem bestimmten Zeitpunkt oder nach eine bestimmten Zeitspanne verstrichen ist. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; der JobSchedule ist eine gute API, die mit dem Betriebssystem, zum Planen von Aufträgen funktioniert. Es ist jedoch nur verfügbar, für die Android-apps, die Ziel-API-Ebene 21 oder höher. 
* **[Broadcast Receiver](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; ein Android-app kann zum Ausführen von Aufgaben als Reaktion auf eine systemweite Ereignisse oder Intents übertragungsempfänger einrichten. Übertragungsempfänger bieten jedoch keine keine Kontrolle über, wenn der Auftrag ausgeführt werden soll. Änderungen in der Android-Betriebssystem schränkt auch wenn übertragungsempfänger funktionieren, oder die Arten von Arbeitsschritten, die sie reagieren können. 

Es gibt zwei wichtige Features, die effiziente Durchführung von Hintergrundarbeit (auch bezeichnet als eine _Hintergrundauftrag_ oder _Auftrag_):

1. **Planen die Arbeit auf intelligente Weise** &ndash; es ist wichtig, wenn eine Anwendung im Hintergrund, die Arbeit ist dies der Fall als ein "guter Bürger" ist. Im Idealfall sollte die Anwendung nicht gefordert wird, dass ein Auftrag ausgeführt werden. Stattdessen sollte die Anwendung Bedingungen angeben, die für die erfüllt sein müssen, wenn der Auftrag ausführen kann, und Planen Sie diese Aufgabe ausführen, wenn die Bedingungen erfüllt sind. Dadurch können Android zum Arbeitsaufgaben auf intelligente Weise ausführen. Beispielsweise können netzwerkanforderungen zusammengefasst werden, führen Sie alle zur gleichen Zeit maximale Mehraufwand bei der Netzwerke nutzen.
2. **Kapseln die Arbeit** &ndash; sollte der Code die Verarbeitung im Hintergrund ausführen gekapselt werden, in eine diskrete Komponente, die ausgeführt werden kann, unabhängig von der Benutzeroberfläche und relativ einfach, plant, wenn die Arbeit nicht abgeschlossen aus irgendeinem Grund.

Der Firebase-Dispatcher ist eine Bibliothek von Google, das eine fluent-API zur Vereinfachung der zeitplanung Verarbeitung im Hintergrund bereitstellt. Es richtet sich an den Ersatz für Google Cloud-Manager. Der Firebase-Dispatcher umfasst die folgenden APIs:

* Ein `Firebase.JobDispatcher.JobService` ist eine abstrakte Klasse, die mit der Logik erweitert werden muss, die im Hintergrundauftrag ausgeführt werden.
* Ein `Firebase.JobDispatcher.JobTrigger` deklariert werden, wenn der Auftrag gestartet werden soll. Dies wird in der Regel als ein Zeitfenster, z. B., warten Sie mindestens 30 Sekunden, bevor der Auftrag wird gestartet, aber führen Sie den Auftrag innerhalb von 5 Minuten.
* Ein `Firebase.JobDispatcher.RetryStrategy` enthält Informationen, was geschehen soll, wenn ein Auftrag nicht korrekt ausgeführt. Die wiederholungsstrategie gibt an, wie lange warten, bevor Sie versuchen, den Auftrag erneut ausführen. 
* Ein `Firebase.JobDispatcher.Constraint` Akkuladestufe oder ist ein optionaler Wert, der eine Bedingung, die erfüllt sein müssen beschreibt, bevor der Auftrag ausgeführt werden kann, wie z. B., wenn das Gerät in einem unmetered Netzwerk befindet.
* Die `Firebase.JobDispatcher.Job` ist eine API, das die vorherigen APIs in einer Arbeitseinheit vereint, die vom geplant werden, können die `JobDispatcher`. Die `Job.Builder` Klasse wird zum Instanziieren einer `Job`.
* Ein `Firebasee.JobDispatcher.JobDispatcher` die vorherigen drei APIs verwendet, um die Arbeit mit dem Betriebssystem zu planen und eine Möglichkeit zum Abbrechen von Aufträgen, bei Bedarf bereitzustellen.

Um mit der Firebase-Dispatcher zu planen, muss eine Xamarin.Android-Anwendung den Code in einem Typ, der erweitert kapseln die `JobService` Klasse. `JobService` verfügt über drei Methoden, die während der Lebensdauer des Auftrags aufgerufen werden kann:

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Diese Methode ist, in denen die Arbeit erfolgt und sollte immer implementiert werden. Er wird im Hauptthread ausgeführt. Diese Methode gibt `true` liegt arbeiten verbleibenden, oder `false` , wenn die Arbeit erfolgt. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Wird aufgerufen, wenn der Auftrag aus irgendeinem Grund beendet wird. Es sollte zurückgeben `true` , wenn der Auftrag für die spätere Verwendung neu geplant werden soll.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Diese Methode wird aufgerufen, wenn die `JobService` ist irgendwelche asynchronen Vorgänge abgeschlossen. 

Um einen Auftrag zu planen, die Anwendung instanziiert ein `JobDispatcher` Objekt. Danach wird ein `Job.Builder` dient zum Erstellen einer `Job` -Objekt, das bereitgestellt wird die `JobDispatcher` der versuchen Sie es zu Ausführung des Auftrags zu planen.

Dieses Handbuch wird beschrieben, wie eine Xamarin.Android-Anwendung der Firebase-Dispatcher hinzugefügt, und verwenden, um die Verarbeitung im Hintergrund planen.

## <a name="requirements"></a>Anforderungen

Der Firebase-Dispatcher ist Android-API-Ebene, 9 oder höher erforderlich. Die Firebase-Auftrag-Dispatcher-Bibliothek basiert auf einige Komponenten, die von Google Play Services bereitgestellten; das Gerät muss es sich um Google Play Services installiert haben.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Mithilfe der Firebase-Auftrag-Dispatcher-Bibliothek in Xamarin.Android

Um den ersten Schritten mit der Firebase-Dispatcher Hinzufügen der [Xamarin.Firebase.JobDispatcher NuGet-Paket](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) dem Xamarin.Android-Projekt. Suchen Sie den NuGet-Paket-Manager für die **Xamarin.Firebase.JobDispatcher** Paket (die vor der Veröffentlichung).

Erstellen Sie nach dem Hinzufügen der Firebase-Auftrag-Dispatcher-Bibliothek, eine `JobService` Klasse, und Planen Sie mit einer Instanz der Ausführung der `FirebaseJobDispatcher`.


### <a name="creating-a-jobservice"></a>Erstellen eine JobService

Alle Aufgaben, die von der Firebase-Auftrag-Dispatcher-Bibliothek muss ausgeführt werden, in einem Typ, der erweitert die `Firebase.JobDispatcher.JobService` abstrakte Klasse. Erstellen einer `JobService` ähnelt sehr dem Erstellen einer `Service` mit dem Android-Framework: 

1. Erweitern Sie die `JobService` Klasse
2. Ergänzen Sie die Unterklasse mit der `ServiceAttribute`. Obwohl nicht zwingend erforderlich, es wird empfohlen, legen Sie explizit die `Name` Parameter, um beim Debuggen hilfreich sein der `JobService`. 
3. Hinzufügen einer `IntentFilter` zum Deklarieren der `JobService` in die **"androidmanifest.xml"**. Dadurch wird auch die Firebase-Auftrag-Dispatcher-Bibliothek suchen, und rufen Sie die `JobService`.

Der folgende Code ist ein Beispiel für den einfachsten `JobService` verwenden Sie für eine Anwendung, die TPL können einige Vorgänge asynchron:

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

### <a name="creating-a-firebasejobdispatcher"></a>Erstellen eine FirebaseJobDispatcher

Bevor keine Arbeit geplant werden kann, ist es erforderlich, erstellen eine `Firebase.JobDispatcher.FirebaseJobDispatcher` Objekt. Die `FirebaseJobDispatcher` ist verantwortlich für die Planung einer `JobService`. Der folgende Codeausschnitt ist eine Möglichkeit zum Erstellen einer Instanz von der `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Im vorherigen Codeausschnitt der `GooglePlayDriver` ist Klasse, die dabei hilft die `FirebaseJobDispatcher` einige Planung APIs in Google Play-Dienste auf dem Gerät interagieren. Der Parameter `context` stellt alle Android `Context`, z. B. eine Aktivität. Derzeit den `GooglePlayDriver` ist die einzige `IDriver` Implementierung in der Firebase-Auftrag-Dispatcher-Bibliothek. 

Die Xamarin.Android-Bindung für den Verteiler der Firebase-Projekt enthält eine Erweiterungsmethode zum Erstellen einer `FirebaseJobDispatcher` aus der `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Nach der `FirebaseJobDispatcher` wurde instanziiert, es ist möglich, erstellen Sie eine `Job` und führen Sie den Code in die `JobService` Klasse. Die `Job` wird erstellt, indem eine `Job.Builder` Objekt aus, und wird im nächsten Abschnitt erläutert werden.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Erstellen eine Firebase.JobDispatcher.Job mit der Job.Builder

Die `Firebase.JobDispatcher.Job` -Klasse ist verantwortlich für das Kapseln der Metadaten zum Ausführen einer `JobService`. Ein`Job` enthält Informationen wie z. B. alle Einschränkungen, die erfüllt sein muss, bevor der Auftrag ausgeführt werden kann, wenn die `Job` wird wiederholt, oder alle Trigger, der bewirkt den Auftrag ausgeführt werden.  Als absolutes Minimum eine `Job` benötigen eine _Tag_ (eine eindeutige Zeichenfolge, die den Auftrag identifiziert die `FirebaseJobDispatcher`) und den Typ des der `JobService` , die ausgeführt werden soll. Der Firebase-Dispatcher instanziiert die `JobService` wann Zeit zum Ausführen des Auftrags ist.  Ein `Job` wird mit einer Instanz von erstellt die `Firebase.JobDispatcher.Job.JobBuilder` Klasse. 

Der folgende Codeausschnitt ist das einfachste Beispiel zum Erstellen einer `Job` die Xamarin.Android-Bindung verwenden:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

Die `Job.Builder` führt einige einfache Überprüfungen auf der Eingabewerten für den Auftrag. Eine Ausnahme wird ausgelöst, wenn es nicht möglich, dass die `Job.Builder` zum Erstellen einer `Job`.  Die `Job.Builder` erstellt eine `Job` mit den folgenden Standardeinstellungen:

* Ein `Job`des _Lebensdauer_ nur solange, bis das Gerät neu gestartet wird (wie lange es geplant wird ausgeführt) &ndash; , sobald das Gerät neu gestartet der `Job` geht verloren.
* Ein `Job` wird nicht wiederholt &ndash; es wird nur einmal ausgeführt.
* Ein `Job` wird geplant werden, und führen Sie so bald wie möglich.
* Standardwiederholungsstrategie für eine `Job` ist die Verwendung einer _exponentiell ansteigende Wartezeiten_ (auf Weitere Details unten im Abschnitt erläuterten [Festlegen einer RetryStrategy](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Nach dem Erstellen der `Job`, muss geplant werden, mit der `FirebaseJobDispatcher` bevor er ausgeführt wird. Es gibt zwei Methoden für die Planung einer `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Der Rückgabewert von `FirebaseJobDispatcher.Schedule` einem der folgenden ganzzahligen Werte:

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; Die `Job` wurde erfolgreich geplant.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Ein unbekanntes Problem ist aufgetreten, und kann nicht die `Job` geplant.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Ein ungültiger `IDriver` verwendet wurde oder die `IDriver` aus irgendeinem Grund nicht verfügbar war. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; Die `Trigger` wurde nicht unterstützt.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Der Dienst ist nicht ordnungsgemäß konfiguriert oder nicht verfügbar ist.
 
### <a name="configuring-a-job"></a>Konfigurieren eines Auftrags

Es ist möglich, einen Auftrag anpassen. Die folgenden: Beispiele, wie ein Auftrag angepasst werden kann

* [Übergeben von Parametern zu einem Auftrag](#Passing_Parameters_to_a_Job) &ndash; ein `Job` unter Umständen weitere Werte mit der Arbeit, z. B. eine Datei herunterlädt.
* [Legen Sie Einschränkungen](#Setting_Constraints) &ndash; muss möglicherweise nur einen Auftrag ausgeführt wird, wenn bestimmte Bedingungen erfüllt sind. Führen Sie beispielsweise nur eine `Job` Wenn das Gerät wird geladen. 
* [Angeben, wann eine `Job` ausgeführt werden soll](#Setting_Job_Triggers) &ndash; der Firebase-Auftrag Verteiler ermöglicht Anwendungen, eine Zeit anzugeben, wann der Auftrag ausgeführt werden sollte.  
* [Deklarieren Sie eine wiederholungsstrategie für fehlerhafte Aufträge](#Setting_a_RetryStrategy) &ndash; ein _wiederholungsstrategie_ enthält Informationen zu den `FirebaseJobDispatcher` dazu, was mit `Jobs` , die nicht abgeschlossen. 

Jeder der folgenden Themen werden mehr in den folgenden Abschnitten erläutert werden.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-jarameters-to-a-job"></a>Übergeben von Jarameters an einen Auftrag

Parameter werden an einen Auftrag übergeben, durch das Erstellen einer `Bundle` , übergeben wird, zusammen mit den `Job.Builder.SetExtras` Methode:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Die `Bundle` erfolgt über die `IJobParameters.Extras` Eigenschaft für die `OnStartJob` Methode:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Festlegen von Einschränkungen

Einschränkungen können reduziert Kosten oder Akku Ausgleichsmodus auf dem Gerät. Die `Firebase.JobDispatcher.Constraint` Klasse definiert diese Einschränkungen als ganzzahlige Werte:

* `Constraint.OnUnmeteredNetwork` &ndash; Den Auftrag nur ausgeführt, wenn das Gerät mit einem unmetered-Netzwerk verbunden ist. Dies ist nützlich, um zu verhindern, dass den Benutzer Gebühren für Daten.
* `Constraint.OnAnyNetwork` &ndash; Führen Sie den Auftrag unabhängig Netzwerk mit das Gerät verbunden ist. Wenn zusammen mit angegebenen `Constraint.OnUnmeteredNetwork`, dieser Wert wird eine höhere Priorität.
* `Constraint.DeviceCharging` &ndash; Führen Sie den Auftrag nur dann, wenn das Gerät geladen wird.

Einschränkungen werden festgelegt, mit der `Job.Builder.SetConstraint` Methode: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

Die `JobTrigger` enthält Anleitungen für das Betriebssystem zu, wenn der Auftrag gestartet werden soll. Ein `JobTrigger` verfügt über eine _Fenster ausführen_ , einen geplanten Zeitpunkt für den Fall definiert die `Job` ausgeführt werden soll. Das Fenster verfügt über eine _Startfenster_ Wert und einem _endfenster_ Wert. Das Startfenster ist die Anzahl der Sekunden an, die das Gerät warten soll, bevor die Ausführung des Auftrags und den Wert für das Ende ist die maximale Anzahl von Sekunden vor dem Ausführen der `Job`. 

Ein `JobTrigger` können erstellt werden, mit der `Firebase.Jobdispatcher.Trigger.ExecutionWindow` Methode.  Z. B. `Trigger.ExecutionWindow(15,60)` bedeutet, die der Auftrag ausgeführt werden soll, zwischen 15 und 60 Sekunden aus, wenn er eingeplant ist. Die `Job.Builder.SetTrigger` Methode wird verwendet, um 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Der Standardwert `JobTrigger` für ein Auftrag mit dem Wert entspricht `Trigger.Now`, der angibt, dass ein Auftrag ausgeführt werden, so bald wie möglich nach dem geplant...

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Festlegen einer RetryStrategy

Die `Firebase.JobDispatcher.RetryStrategy` verwendet, um anzugeben, welcher Anteil einer Verzögerung kommen, ein Gerät verwenden soll, bevor Sie versuchen, einen fehlerhaften Auftrag erneut ausführen. Ein `RetryStrategy` verfügt über eine _Richtlinie_, die definiert welcher-Basis-Algorithmus verwendet wird, um erneut zu planen, den fehlerhaften Auftrag, und ein Fenster, das ein Fenster gibt an, in dem der Auftrag geplant werden soll. Dies _neu planen Fenster_ wird durch zwei Werte definiert. Der erste Wert ist die Anzahl der Sekunden vor dem durch das erneute Planen des Auftrags (der _anfängliche Backoff_ Wert), und die zweite Zahl ist die maximale Anzahl von Sekunden, bevor der Auftrag ausgeführt werden muss (der _fürmaximaleWartezeit_Wert). 
 
Die beiden Arten von wiederholungsrichtlinien werden durch diese Int-Werte bestimmt:

* `RetryStrategy.RetryPolicyExponential` &ndash; Ein _exponentiell ansteigende Wartezeiten_ Richtlinie erhöht sich der Wert für die anfängliche Wartezeit exponentiell nach jedem Fehler. Beim ersten ein Auftrag fehlschlägt, in die Bibliothek wartet das _ursprünglicher-Intervall, der angegeben wird, bevor Sie durch das erneute Planen des Auftrags &ndash; Beispiel 30 Sekunden. Der zweite Start des Auftrags ein Fehler auftritt, wird die Bibliothek mindestens 60 Sekunden, bevor Sie versuchen, die zum Ausführen des Auftrags gewartet. Nach der dritten Versuch, wird die Bibliothek 120 Sekunden warten, und so weiter. Der Standardwert `RetryStrategy` für der Firebase-Dispatcher-Bibliothek wird dargestellt, durch die `RetryStrategy.DefaultExponential` Objekt. Es weist eine anfängliche Wartezeit von 30 Sekunden und eine maximale Wartezeit von 3600 Sekunden.
* `RetryStrategy.RetryPolicyLinear` &ndash; Diese Strategie ist eine _lineare Backoff_ , dass der Auftrag neu geplant werden soll, führen Sie auf festgelegten Abständen (bis er erfolgreich abgeschlossen wurde). Lineare Backoff eignet sich am besten für die Arbeit, die so bald wie möglich ausgeführt werden muss oder für Probleme, die schnell selbst aufgelöst werden. Die Verteiler der Firebase-Auftrag-Bibliothek definiert einen `RetryStrategy.DefaultLinear` IValidator.h ein neu planen Fenster von mindestens 30 Sekunden und bis zu 3.600 Sekunden.

Es ist möglich, ein benutzerdefiniertes `RetryStrategy` mit der `FirebaseJobDispatcher.NewRetryStrategy` Methode. Es verwendet drei Parameter:

1. `int policy` &ndash; Die _Richtlinie_ ist einer der vorherigen `RetryStrategy` Werte `RetryStrategy.RetryPolicyLinear`, oder `RetryStrategy.RetryPolicyExponential`.
2. `int intialBackoffSeconds` &ndash; Die _anfängliche Backoff_ eine Verzögerung, in Sekunden, die erforderlich sind, bevor Sie versuchen, den Auftrag erneut ausführen. Der Standardwert für diese ist 30 Sekunden. 
3. `int maximumBackoffSeconds` &ndash; Die _für maximale Wartezeit_ Wert gibt die maximale Anzahl von Sekunden verzögert werden, bevor Sie versuchen, den Auftrag erneut ausführen. Der Standardwert beträgt 3600 Sekunden. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Abbrechen eines Auftrags

Es ist möglich, um alle Aufträge abzubrechen, die geplant wurden, oder nur einen einzelnen Auftrag, indem die `FirebaseJobDispatcher.CancelAll()` Methode oder der `FirebaseJobDispatcher.Cancel(string)` Methode:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Einer dieser Methoden gibt einen ganzzahligen Wert zurück:

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; Der Auftrag wurde erfolgreich abgebrochen.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Ein Fehler verhinderte, dass den Auftrag wird abgebrochen.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; Die `FirebaseJobDispatcher` kann nicht für das Abbrechen des Auftrags, wie es keine gültige ist `IDriver` verfügbar.

## <a name="summary"></a>Zusammenfassung

Dieser Leitfaden erläutert, wie Sie mit, dass der Firebase-Dispatcher auf intelligente Weise Aufgaben im Hintergrund. Er erläutert, wie die Arbeit als zu kapseln eine `JobService` und zum Verwenden der `FirebaseJobDispatcher` so planen Sie die Arbeit, Angeben der Kriterien mit einer `JobTrigger` und wie Fehler mit behandelt werden sollen eine `RetryStrategy`.


## <a name="related-links"></a>Verwandte Links

- [Xamarin.Firebase.JobDispatcher auf NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [Firebase-Auftrag-Dispatcher auf GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher-Bindung](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Intelligente auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [Android Akku und Arbeitsspeicheroptimierung – Google e/a-2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
