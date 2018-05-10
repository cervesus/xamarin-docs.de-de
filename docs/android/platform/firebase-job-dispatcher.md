---
title: Auftrag wurde vom Verteiler firebase
description: Dieses Handbuch erläutert, wie beim Planen der Verarbeitung im Hintergrund mithilfe der Auftrag wurde vom Verteiler Firebase-Bibliothek von Google.
ms.prod: xamarin
ms.assetid: 3DB9C7A3-D351-481D-90C5-BEC25D1B9910
ms.technology: xamarin-android
author: topgenorth
ms.author: toopge
ms.date: 05/08/2018
ms.openlocfilehash: a714ac55c3a49b91cb21e3ba1793b9bccd7d1be2
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/09/2018
---
# <a name="firebase-job-dispatcher"></a>Auftrag wurde vom Verteiler firebase

_Dieses Handbuch erläutert, wie beim Planen der Verarbeitung im Hintergrund mithilfe der Auftrag wurde vom Verteiler Firebase-Bibliothek von Google._

![Firebase Auftrag Verteiler in der Vorschau](~/media/shared/preview.png)

## <a name="overview"></a>Übersicht

Eine der geeignetsten Methoden eine Android-Anwendung für den Benutzer reaktionsfähig wird sichergestellt, dass komplexe oder lang ausgeführte Arbeit im Hintergrund ausgeführt wird. Allerdings ist es wichtig, dass die Verarbeitung im Hintergrund nicht negativ auf die benutzerfreundlichkeit mit dem Gerät auswirkt. 

Beispielsweise kann ein Hintergrundauftrag eine Website alle drei oder vier Minuten abzufragende für Änderungen an ein bestimmtes Dataset abrufen. Dies scheint keine Auswirkungen, aber es keine katastrophalen Auswirkungen auf die Akkulaufzeit haben würden. Die Anwendung wird wiederholt Reaktivieren des Geräts, erhöhen die CPU auf einer höheren Energiezustand aktiviert, schalten Sie die Sender, auf stellen die Anforderungen über das Netzwerk, und klicken Sie dann die Ergebnisse verarbeiten. Da das Gerät wird nicht sofort Herunterfahren und zurück in den Leerlaufzustand LP-herankommt schlechter. Schlecht geplanten Hintergrundarbeit möglicherweise versehentlich des Geräts in einem Zustand mit unnötiges und übermäßiges Stromverbrauch gewährleisten. Diese scheinbar harmlose Aktivität (eine Website abrufen) wird das Gerät in relativ kurzer Zeit unbrauchbar werden.

Android bietet die folgenden APIs unterstützen Sie beim Arbeiten im Hintergrund ausführen selbst sind allerdings nicht für intelligente auftragsplanung ausreichend. 

* **[Beabsichtigte Services](~/android/app-fundamentals/services/creating-a-service/intent-services.md)**  &ndash; Absicht Services sind hervorragend für die Arbeiten ausführen, jedoch keine Möglichkeit zum Planen der Arbeit angebotenen.
* **[AlarmManager](https://developer.android.com/reference/android/app/AlarmManager.html)**  &ndash; diese APIs ermöglichen nur Arbeit geplant werden, jedoch bietet keine Möglichkeit, die Aufgaben tatsächlich auszuführen. Außerdem ermöglicht das AlarmManager nur zeitbasierter Einschränkungen, d. h. ein Warnsignal auslösen, zu einem bestimmten Zeitpunkt oder nach ein bestimmten Zeitraum verstrichen ist. 
* **[JobScheduler](https://developer.android.com/reference/android/app/job/JobScheduler.html)**  &ndash; der JobSchedule ist eine hervorragende API, die mit dem Betriebssystem zum Planen von Aufträgen funktioniert. Es ist jedoch nur verfügbar für die Android-apps, die API-Ebene 21 ausgerichtet oder höher. 
* **[Broadcast Empfänger](~/android/app-fundamentals/broadcast-receivers.md)**  &ndash; ein Android-app kann broadcast Empfänger zum Ausführen von Aktionen als Reaktion auf eine systemweite Ereignisse oder Intents einrichten. Broadcast Empfänger stellen jedoch keine Kontrolle über die Ausführung des Auftrags. Änderungen in der Android-Betriebssystem werden eingeschränkt, auch wenn broadcast Empfänger funktioniert oder die Arbeitsschritte, auf die reagiert werden kann. 

Es sind zwei Schlüsselfunktionen effiziente Verarbeitung im Hintergrund ausführen (auch bezeichnet als eine _Hintergrundauftrag_ oder ein _Auftrag_):

1. **Intelligent Planen der Arbeit** &ndash; sind wichtig, wenn eine Anwendung im Hintergrund, die auf diese Weise ist dies der Fall als eine gute Bürger ist. Im Idealfall sollte die Anwendung nicht gefordert wird, dass ein Auftrag ausgeführt werden. Stattdessen sollte die Anwendung Bedingungen angeben, die für die erfüllt sein müssen, wenn der Auftrag ausführen kann, und Sie planen, dass diese Aufgabe ausführen, wenn die Bedingungen erfüllt sind. Dadurch können Android Intelligent Arbeit ausführen. Anforderungen über das Netzwerk können z. B. zusammengefasst werden, führen Sie alle zur selben Zeit maximale Mehraufwand bei der Platzierung Netzwerk nutzen.
2. **Kapselt die Arbeit** &ndash; sollte der Code die Verarbeitung im Hintergrund ausführen gekapselt werden, in einer Komponente, die ausgeführt werden kann, unabhängig von der Benutzeroberfläche und relativ einfach erneut geplant, wenn die Arbeit nicht abgeschlossen aus irgendeinem Grund.

Der Auftrag wurde vom Verteiler Firebase ist eine Bibliothek von Google, die eine fluent-API, um Planungsdaten Verarbeitung im Hintergrund zu vereinfachen. Es ist der Ersatz für Google Cloud-Manager werden soll. Der Auftrag wurde vom Verteiler Firebase umfasst die folgenden APIs:

* Ein `Firebase.JobDispatcher.JobService` ist eine abstrakte Klasse, die mit der Logik erweitert werden muss, die in den Hintergrundauftrag im ausgeführt wird.
* Ein `Firebase.JobDispatcher.JobTrigger` deklariert, wenn der Auftrag gestartet werden soll. Dies wird in der Regel als ein Zeitfenster, z. B., warten Sie mindestens 30 Sekunden, bevor der Auftrag gestartet wurde, aber führen Sie den Auftrag innerhalb von fünf Minuten.
* Ein `Firebase.JobDispatcher.RetryStrategy` enthält Informationen, was geschehen soll, wenn ein Auftrag nicht ordnungsgemäß ausgeführt wird. Gibt an, wie lange die wiederholungsstrategie warten, bevor Sie versuchen, den Auftrag erneut auszuführen. 
* Ein `Firebase.JobDispatcher.Constraint` ist ein optionaler Wert, der eine Bedingung, die erfüllt sein müssen beschreibt, bevor der Auftrag ausgeführt werden kann, wie z. B. ist das Gerät in einem Netzwerk unmetered oder geladen.
* Die `Firebase.JobDispatcher.Job` ist eine API, die die vorherigen APIs in einer Arbeitseinheit vereinheitlicht, die von geplant werden, kann die `JobDispatcher`. Die `Job.Builder` Klasse wird zum Instanziieren einer `Job`.
* Ein `Firebasee.JobDispatcher.JobDispatcher` verwendet die vorherigen drei APIs so planen Sie die Arbeit mit dem Betriebssystem und Möglichkeit zum Abbrechen der Aufträge, bei Bedarf bereitzustellen.

Informationen zum Planen von Arbeit mit der Dispatcher Firebase muss eine Anwendung Xamarin.Android den Code in einem Typ, der erweitert kapseln die `JobService` Klasse. `JobService` verfügt über drei Lifecycle-Methoden, die während der Lebensdauer des Auftrags aufgerufen werden kann:

* **`bool OnStartJob(IJobParameters parameters)`** &ndash; Diese Methode ist, in denen die Arbeit auftreten wird und immer implementiert werden sollte. Es wird auf der Haupt-Thread ausgeführt. Von dieser Methode zurückgegeben `true` Wenn Erfolg verbleiben, oder `false` , wenn die Arbeit abgeschlossen ist. 
* **`bool OnStopJob(IJobParameters parameters)`** &ndash; Wird aufgerufen, wenn der Auftrag aus irgendeinem Grund beendet wird. Es sollte zurückgeben `true` , wenn der Auftrag für die spätere neu geplant werden soll.
* **`JobFinished(IJobParameters parameters, bool needsReschedule)`** &ndash; Diese Methode wird aufgerufen, wenn die `JobService` asynchrone Arbeit abgeschlossen hat. 

Informationen zum Planen eines Auftrags die Anwendung instanziiert dann eine `JobDispatcher` Objekt. Anschließend wird eine `Job.Builder` dient zum Erstellen einer `Job` -Objekt, das bereitgestellt wird die `JobDispatcher` , versuchen Sie es zu planen der Ausführung des Auftrags.

Dieses Handbuch wird beschrieben, wie eine Anwendung Xamarin.Android der Dispatcher Firebase hinzu, und verwenden, um die Verarbeitung im Hintergrund zu planen.

## <a name="requirements"></a>Anforderungen

Der Auftrag wurde vom Verteiler Firebase ist die Android-API Level 9 oder höher erforderlich. Die Auftrag wurde vom Verteiler Firebase Bibliothek basiert auf einige Komponenten, die von Google Play-Dienste bereitgestellt; das Gerät muss über Google Play-Dienste installiert haben.

## <a name="using-the-firebase-job-dispatcher-library-in-xamarinandroid"></a>Mithilfe der Firebase Auftrag Dispatcher-Bibliothek in Xamarin.Android

Um mit der Dispatcher Firebase beginnen, zuerst Hinzufügen der [Xamarin.Firebase.JobDispatcher NuGet-Paket](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher) Xamarin.Android-Projekt. Suchen Sie das NuGet-Paket-Manager für die **Xamarin.Firebase.JobDispatcher** Paket (d. h. vor der Veröffentlichung).

Nach dem Hinzufügen der Auftrag wurde vom Verteiler Firebase-Bibliothek, erstellen Sie eine `JobService` Klasse, und Planen Sie mit einer Instanz der Ausführung der `FirebaseJobDispatcher`.

> [!NOTE]
> Das Ziel die aktuelle Bindung für den Firebase Auftrag Verteiler ist eine alte Version der Bibliothek. Das ist ein [bekannter Fehler [(https://bugzilla.xamarin.com/show_bug.cgi?id=59046)] verhindert die Bindung aus aktualisiert wird auf eine neuere Version des Verteilers Auftrag Firebase abzielen.


### <a name="creating-a-jobservice"></a>Erstellen eine JobService

Alle Arbeit, die von der Bibliothek Firebase Auftrag Verteiler muss erfolgen, in einem Typ, der erweitert die `Firebase.JobDispatcher.JobService` abstrakte Klasse. Erstellen einer `JobService` ähnelt dem Erstellen einer `Service` mit dem Android-Framework: 

1. Erweitern der `JobService` Klasse
2. Ergänzen Sie die Unterklasse mit der `ServiceAttribute`. Obwohl nicht zwingend erforderlich, es wird empfohlen, explizit festlegen der `Name` Parameter, um beim Debuggen hilfreich sein der `JobService`. 
3. Hinzufügen einer `IntentFilter` deklarieren die `JobService` in der **AndroidManifest.xml**. Dadurch wird auch die Firebase Auftrag Dispatcher-Bibliothek zu suchen, und rufen die `JobService`.

Der folgende Code ist ein Beispiel für die am einfachsten `JobService` für eine Anwendung mithilfe der TPL portierungsziel asynchron ausführen:

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

Bevor jede Arbeit geplant werden kann, ist es erforderlich, erstellen eine `Firebase.JobDispatcher.FirebaseJobDispatcher` Objekt. Die `FirebaseJobDispatcher` ist verantwortlich für die Planung einer `JobService`. Der folgende Codeausschnitt ist eine Möglichkeit zum Erstellen einer Instanz der `FirebaseJobDispatcher`: 
 
 ```csharp
// This is the "Java" way to create a FirebaseJobDispatcher object
IDriver driver = new GooglePlayDriver(context);
FirebaseJobDispatcher dispatcher = new FirebaseJobDispatcher(driver);
```

Im vorherigen Codeausschnitt die `GooglePlayDriver` ist die Klasse, die der Identifikation der `FirebaseJobDispatcher` interagiert mit einigen der scheduling-APIs in Google Play-Dienste auf dem Gerät. Der Parameter `context` stellt alle Android `Context`, wie etwa eine Aktivität. Derzeit die `GooglePlayDriver` ist die einzige `IDriver` Implementierung in der Bibliothek Firebase Auftrag wurde vom Verteiler. 

Die Xamarin.Android-Bindung für den Auftrag wurde vom Verteiler Firebase liefert eine Erweiterungsmethode zum Erstellen einer `FirebaseJobDispatcher` aus der `Context`: 

```csharp
FirebaseJobDispatcher dispatcher = context.CreateJobDispatcher();
```

Einmal die `FirebaseJobDispatcher` wurde instanziiert, es ist möglich, erstellen Sie eine `Job` und führen Sie den Code in der `JobService` Klasse. Die `Job` wird erstellt, indem ein `Job.Builder` Objekt, und wird im nächsten Abschnitt erläutert werden.

### <a name="creating-a-firebasejobdispatcherjob-with-the-jobbuilder"></a>Erstellen eine Firebase.JobDispatcher.Job mit der Job.Builder

Die `Firebase.JobDispatcher.Job` -Klasse ist verantwortlich für das Kapseln der Metadaten zum Ausführen einer `JobService`. Ein`Job` enthält Informationen, z. B. eine Einschränkung, die erfüllt sein muss, bevor der Auftrag ausgeführt werden kann, wenn die `Job` wiederholt wird, oder alle Trigger, das bewirkt den Auftrag ausgeführt werden.  Bare mindestens eine `Job` benötigen eine _Tag_ (eine eindeutige Zeichenfolge, die den Auftrag, identifiziert die `FirebaseJobDispatcher`) und den Typ des der `JobService` , die ausgeführt werden soll. Der Auftrag wurde vom Verteiler Firebase instanziiert dann die `JobService` wird er Zeit zum Ausführen des Auftrags.  Ein `Job` wird mit einer Instanz von erstellt die `Firebase.JobDispatcher.Job.JobBuilder` Klasse. 

Der folgende Codeausschnitt ist das einfachste Beispiel zum Erstellen einer `Job` mithilfe der Bindung Xamarin.Android:

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .Build();
```

Die `Job.Builder` führt einige grundlegende validierungsüberprüfungen für die Eingabewerte für den Auftrag. Eine Ausnahme wird ausgelöst, wenn es nicht möglich, dass die `Job.Builder` zum Erstellen einer `Job`.  Die `Job.Builder` erstellt eine `Job` mit den folgenden Standardeinstellungen:

* Ein `Job`des _Lebensdauer_ nur verwendet werden, bis das Gerät neu gestartet wird (wie lange es wird zur Ausführung geplant) &ndash; , sobald das Gerät neu gestartet der `Job` verloren gegangen ist.
* Ein `Job` wird nicht wiederholt &ndash; es wird nur einmal ausgeführt.
* Ein `Job` wird so bald wie möglich ausführen geplant werden.
* Die Standard-wiederholungsstrategie für eine `Job` ist die Verwendung einer _Exponentielles Backoff_ (auf Weitere Details unten im Abschnitt erläuterten [Festlegen einer RetryStrategy](#Setting_a_RetryStrategy))

### <a name="scheduling-a-job"></a>Planen eines Auftrags

Nach dem Erstellen der `Job`, muss geplant werden, mit der `FirebaseJobDispatcher` bevor es ausgeführt wird. Es gibt zwei Methoden für die Planung einer `Job`:

```csharp
// This will throw an exception if there was a problem scheduling the job
dispatcher.MustSchedule(myJob);

// This method will not throw an exception; an integer result value is returned
int scheduleResult = dispatcher.Schedule(myJob);
```

Der Rückgabewert von `FirebaseJobDispatcher.Schedule` wird eines der folgenden ganzzahligen Werte sein:

* `FirebaseJobDispatcher.ScheduleResultSuccess` &ndash; Die `Job` wurde erfolgreich geplant.
* `FirebaseJobDispatcher.ScheduleResultUnknownError` &ndash; Ein unbekanntes Problem ist aufgetreten, und kann nicht die `Job` geplant.
* `FirebaseJobDispatcher.ScheduleResultNoDriverAvailable` &ndash; Eine ungültige `IDriver` verwendet wurde oder die `IDriver` wurde aus irgendeinem Grund nicht verfügbar. 
* `FirebaseJobDispatcher.ScheduleResultUnsupportedTrigger` &ndash; Die `Trigger` wurde nicht unterstützt.
* `FirebaseJobDispatcher.ScheduleResultBadService` &ndash; Der Dienst ist nicht richtig konfiguriert oder ist nicht verfügbar.
 
### <a name="configuring-a-job"></a>Konfigurieren eines Auftrags

Es ist möglich, einen Auftrag anzupassen. Die Beispiele dafür, wie ein Auftrag angepasst werden kann:

* [Übergeben von Parametern zu einem Auftrag](#Passing_Parameters_to_a_Job) &ndash; ein `Job` möglicherweise zusätzliche Werte mit der Arbeit, z. B. das Herunterladen von Dateien.
* [Festlegen von Einschränkungen](#Setting_Constraints) &ndash; es möglicherweise erforderlich sein, nur ein Auftrag ausgeführt wird, wenn bestimmte Bedingungen erfüllt sind. Führen Sie z. B. nur eine `Job` Wenn das Gerät aufgeladen ist. 
* [Geben an, wann eine `Job` ausgeführt werden soll](#Setting_Job_Triggers) &ndash; der Firebase Auftrag wurde vom Verteiler ermöglicht Anwendungen, die eine Zeit angeben, der der Auftrag ausgeführt werden soll.  
* [Deklarieren Sie eine wiederholungsversuchstrategie für fehlgeschlagene Aufträge](#Setting_a_RetryStrategy) &ndash; ein _wiederholungsstrategie_ bietet Hinweise zum der `FirebaseJobDispatcher` auf Vorgehensweise mit `Jobs` , die nicht abgeschlossen. 

Jedes dieser Themen wird mehr in den folgenden Abschnitten erläutert werden.

<a name="Passing_Parameters_to_a_Job" />

#### <a name="passing-jarameters-to-a-job"></a>Übergeben von Jarameters an einen Auftrag

Parameter werden an einen Auftrag übergeben, durch das Erstellen einer `Bundle` , wird übergeben, zusammen mit der `Job.Builder.SetExtras` Methode:

```csharp
Bundle jobParameters = new Bundle();
jobParameters.PutInt(FibonacciCalculatorJob.FibonacciPositionKey, 25);

Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetExtras(jobParameters)
                      .Build();

```

Die `Bundle` erfolgt über die `IJobParameters.Extras` Eigenschaft auf die `OnStartJob` Methode:

```csharp
public override bool OnStartJob(IJobParameters jobParameters)
{
    int position = jobParameters.Extras.GetInt(FibonacciPositionKey, DEFAULT_VALUE);
    
    // rest of code omitted
} 
```

<a name="Setting_Constraints" />

#### <a name="setting-constraints"></a>Festlegen von Einschränkungen

Einschränkungen helfen reduziert die Kosten oder Akku Ausgleichsmodus auf dem Gerät. Die `Firebase.JobDispatcher.Constraint` Klasse diese Einschränkungen als ganzzahligen Werten definiert:

* `Constraint.OnUnmeteredNetwork` &ndash; Den Auftrag nur ausgeführt, wenn das Gerät mit einem unmetered Netzwerk verbunden ist. Dies ist hilfreich, um zu verhindern, dass den Benutzer Daten Gebühren anfallen.
* `Constraint.OnAnyNetwork` &ndash; Führt den Auftrag unabhängig Netzwerk mit das Gerät verbunden ist. Wenn zusammen mit angegebenen `Constraint.OnUnmeteredNetwork`, wird dieser Wert Vorrang.
* `Constraint.DeviceCharging` &ndash; Den Auftrag nur ausgeführt, wenn das Gerät aufgeladen ist.

Einschränkungen werden festgelegt, mit der `Job.Builder.SetConstraint` Methode: 

```csharp
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetConstraint(Constraint.DeviceCharging)
                      .Build();
```

<a name="Setting_Job_Triggers" />

Die `JobTrigger` bietet einen Leitfaden für das Betriebssystem zu, wenn der Auftrag beginnen soll. Ein `JobTrigger` verfügt über eine _Fenster ausführen_ , einen geplanten Zeitpunkt für den Fall definiert die `Job` ausgeführt werden soll. Das Fenster für die Ausführung verfügt über eine _Startfenster_ Wert und ein _endfenster_ Wert. Das Startfenster ist die Anzahl der Sekunden an, die das Gerät warten soll, bevor die Ausführung des Auftrags und der Endwert für das Fenster wird die maximale Anzahl von Sekunden vor dem Ausführen der `Job`. 

Ein `JobTrigger` können erstellt werden, mit der `Firebase.Jobdispatcher.Trigger.ExecutionWindow` Methode.  Z. B. `Trigger.ExecutionWindow(15,60)` bedeutet, der der Auftrag ausgeführt werden soll, zwischen 15 und 60 Sekunden aus, wenn sie geplant ist. Die `Job.Builder.SetTrigger` Methode wird verwendet, um 

```csharp
JobTrigger myTrigger = Trigger.ExecutionWindow(15,60);
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetTrigger(myTrigger)
                      .Build();
```

Die Standardeinstellung `JobTrigger` für ein Auftrag mit dem Wert dargestellt wird `Trigger.Now`, der angibt, dass ein Auftrag ausgeführt werden, so bald wie möglich nach geplant...

<a name="Setting_a_RetryStrategy" />

#### <a name="setting-a-retrystrategy"></a>Festlegen einer RetryStrategy

Die `Firebase.JobDispatcher.RetryStrategy` verwendet, um anzugeben, welcher Anteil des eine Verzögerung ein Gerät verwenden soll, bevor versucht einen fehlerhaften Auftrag erneut auszuführen. Ein `RetryStrategy` verfügt über eine _Richtlinie_, die definiert, welche Zeitbasis Algorithmus verwendet wird, um erneut zu planen, den fehlerhaften Auftrag und ein Fenster "Ausführung", die ein Fenster gibt an, in dem der Auftrag geplant werden soll. Dies _neu planen Fenster_ wird durch zwei Werte definiert. Der erste Wert ist die Anzahl der Sekunden vor dem durch das erneute Planen des Auftrags (die _anfängliche Wartezeit_ Wert), und die zweite Zahl ist die maximale Anzahl von Sekunden, bevor der Auftrag ausgeführt werden muss (der _fürmaximaleWartezeit_Wert). 
 
Die beiden Typen von wiederholungsrichtlinien werden durch diese Int Werte bestimmt:

* `RetryStrategy.RetryPolicyExponential` &ndash; Ein _Exponentielles Backoff_ erhöhen Richtlinie die anfängliche Wartezeit exponentiell nach jeder Fehler. Ersten Mal ein Auftrag ein Fehler auftritt, in die Bibliothek wartet der _initial Intervall aus, das angegeben wird, bevor Sie durch das erneute Planen des Auftrags &ndash; Beispiel 30 Sekunden. Das zweite Mal, das der Auftrag ein Fehler auftritt, das wartet die Bibliothek mindestens 60 Sekunden, bevor Sie versuchen, den Auftrag auszuführen. Nachdem die dritte fehlgeschlagenen Versuch die Bibliothek 120 Sekunden warten, und so weiter. Die Standardeinstellung `RetryStrategy` für den Auftrag wurde vom Verteiler Firebase-Bibliothek wird dargestellt, durch die `RetryStrategy.DefaultExponential` Objekt. Er verfügt über eine anfängliche Wartezeit von 30 Sekunden und eine maximale Wartezeit von 3600 Sekunden.
* `RetryStrategy.RetryPolicyLinear` &ndash; Diese Strategie ist eine _lineare Backoff_ legen Sie, dass der Auftrag neu geplant werden soll, zum Ausführen beim Intervalle (bis er erfolgreich abgeschlossen wurde). Lineare Backoff eignet sich am besten für die Arbeit, die so bald wie möglich ausgeführt werden muss oder für Probleme, die schnell selbst aufgelöst werden. Der Auftrag wurde vom Verteiler Firebase Library definiert eine `RetryStrategy.DefaultLinear` verfügt über ein Fenster neu Planen von mindestens 30 Sekunden und bis zu 3600 Sekunden.

Es ist möglich, ein benutzerdefiniertes `RetryStrategy` mit der `FirebaseJobDispatcher.NewRetryStrategy` Methode. Es akzeptiert drei Parameter:

1. `int policy` &ndash; Die _Richtlinie_ ist einer der vorherigen `RetryStrategy` Werte `RetryStrategy.RetryPolicyLinear`, oder `RetryStrategy.RetryPolicyExponential`.
2. `int intialBackoffSeconds` &ndash; Die _anfängliche Wartezeit_ ist eine Verzögerung in Sekunden an, der, die erforderlich sind, bevor Sie versuchen, den Auftrag erneut auszuführen. Der Standardwert für diese beträgt 30 Sekunden. 
3. `int maximumBackoffSeconds` &ndash; Die _für maximale Wartezeit_ Wert gibt die maximale Anzahl von Sekunden verzögert werden, bevor Sie versuchen, den Auftrag erneut auszuführen. Der Standardwert ist 3600 Sekunden. 

```csharp
RetryStrategy retry = dispatcher.NewRetryStrategy(RetryStrategy.RetryPolicyLinear, initialBackoffSeconds, maximumBackoffSet);

// Create a Job and set the RetryStrategy via the Job.Builder
Job myJob = dispatcher.NewJobBuilder()
                      .SetService<DemoJob>("demo-job-tag")
                      .SetRetryStrategy(retry)
                      .Build();
```

### <a name="cancelling-a-job"></a>Abbrechen eines Auftrags

Es ist möglich, alle Aufträge abzubrechen, die geplant haben, oder nur einen einzelnen Auftrag, indem die `FirebaseJobDispatcher.CancelAll()` Methode oder die `FirebaseJobDispatcher.Cancel(string)` Methode:

```csharp
int cancelResult = dispatcher.CancelAll(); 

// to cancel a single job:

int cancelResult = dispatcher.Cancel("unique-tag-for-job");
```

Einer dieser Methoden gibt einen ganzzahligen Wert zurück:

* `FirebaseJobDispatcher.CancelResultSuccess` &ndash; Der Auftrag wurde erfolgreich abgebrochen.
* `FirebaseJobDispatcher.CancelResultUnknownError` &ndash; Ein Fehler verhinderte, dass den Auftrag abgebrochen wird.
* `FirebaseJobDispatcher.CancelResult.NoDriverAvailable` &ndash; Die `FirebaseJobDispatcher` kann den Auftrag abgebrochen werden, da es keine gültige ist `IDriver` verfügbar.

## <a name="summary"></a>Zusammenfassung

Dieses Handbuch erläutert, wie mit, dass der Auftrag wurde vom Verteiler Firebase Intelligent arbeiten im Hintergrund ausgeführt wird. Er erläutert durchgeführt werden, da die Arbeit zu kapseln einer `JobService` sowie zum Verwenden der `FirebaseJobDispatcher` so planen Sie diese Aufgabe, die Angabe der Kriterien mit einer `JobTrigger` und wie Fehler mit behandelt werden sollen eine `RetryStrategy`.


## <a name="related-links"></a>Verwandte Links

- [Bindungen-Generator schlägt fehl mit Fehler Schwerwiegender AUSNAHMEFEHLER: System.ArgumentNullException: Wert darf nicht null sein.](https://bugzilla.xamarin.com/show_bug.cgi?id=59046)
- [Xamarin.Firebase.JobDispatcher für NuGet](https://www.nuget.org/packages/Xamarin.Firebase.JobDispatcher)
- [Firebase-Auftrag-Verteiler auf GitHub](https://github.com/firebase/firebase-jobdispatcher-android)
- [Xamarin.Firebase.JobDispatcher-Bindung](https://github.com/xamarin/XamarinComponents/tree/master/Android/FirebaseJobDispatcher)
- [Intelligent auftragsplanung](https://developer.android.com/topic/performance/scheduling.html)
- [Android Akku und Arbeitsspeicher Optimierungen - Google e/a-2016 (Video)](https://www.youtube.com/watch?v=VC2Hlb22mZM&feature=youtu.be)
