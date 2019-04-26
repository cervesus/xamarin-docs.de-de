---
title: WatchOS-Trainings-Apps in Xamarin
description: Dieser Artikel behandelt die Verbesserungen Apple Trainings-Apps in WatchOS 3 und wie sie in Xamarin implementiert wurde.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/17/2017
ms.openlocfilehash: 02db7dce6ba38b6c1e943ff189ff69efb7cc1c08
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61084020"
---
# <a name="watchos-workout-apps-in-xamarin"></a>WatchOS-Trainings-Apps in Xamarin

_Dieser Artikel behandelt die Verbesserungen Apple Trainings-Apps in WatchOS 3 und wie sie in Xamarin implementiert wurde._


Neue WatchOS 3 Trainings im Zusammenhang mit apps die Möglichkeit, im Hintergrund ausgeführt wird, klicken Sie auf der Apple Watch und HealthKit Daten zuzugreifen. Die übergeordnete iOS 10-basierten app verfügt auch über die Möglichkeit zum Starten der WatchOS 3-basierten app ohne Eingreifen des Benutzers.

Die folgenden Themen werden im Detail behandelt:

## <a name="about-workout-apps"></a>Trainings-Apps

Benutzer, der Eignung und Trainings-App möglich hoch dedizierte macht mehrere Stunden des Tages auf ihre persönlichen Ziele. Daher erwarten, dass sie reaktionsfähig sind leicht zu bedienende apps, die genau erfassen und Anzeigen von Daten und nahtlose Integration in die Integrität von Apple.

Eine wohlgeformte Fitness oder Trainings-app hilft Benutzern, die "Diagrammeigenschaften" ihre Aktivitäten, um ihre Eignung Ziele zu erreichen. Mit der Apple Watch, Eignung und Trainings-apps verfügen über den sofortigen Zugriff auf Herz, Kalorie brennen und aktivitätserkennung.

[![](workout-apps-images/workout01.png "Eignung und Trainings-Beispiel-app")](workout-apps-images/workout01.png#lightbox)

Watchos 3, neue _Hintergrund ausgeführten_ bietet Trainings bezogene apps die Möglichkeit, im Hintergrund ausgeführt wird, klicken Sie auf der Apple Watch und HealthKit Daten zuzugreifen.

In diesem Dokument das Feature Hintergrund ausgeführt wird, behandelt den Trainings-app-Lebenszyklus vor und zeigen, wie eine Trainings-app des Benutzers beitragen kann _Aktivität Ringe_ auf der Apple Watch.

## <a name="about-workout-sessions"></a>Informationen zu den Trainings-Sitzungen

Das Herzstück jedes Trainings-App ist eine _Trainings Sitzung_ (`HKWorkoutSession`), die der Benutzer starten und beenden kann. Das Trainings-Sitzungs-API ist einfach zu implementieren und eine Trainings-app bietet verschiedene Vorteile wie z. B. enthält:

- Brennen während der Übertragung und Kalorie Erkennung basierend auf dem Typ der Aktivität.
- Automatische Beitrag an die Aktivität Ringe des Benutzers.
- Klicken Sie in einer Sitzung wird die app automatisch angezeigt, wenn der Benutzer das Gerät aus (entweder durch Auslösen von ihren Finger oder der Interaktion mit der Apple Watch) aktiviert.

## <a name="about-background-running"></a>Zum Hintergrund ausführen

Wie bereits erwähnt, kann bei WatchOS 3 eine Trainings-app festgelegt werden, im Hintergrund ausgeführt werden. Mit Hintergrund Ausführung einer Trainings-app kann Daten von der Apple Watch-Sensoren verarbeiten, während der Ausführung im Hintergrund. Beispielsweise kann eine app weiterhin Herz des Benutzers, überwachen, obwohl es nicht mehr auf dem Bildschirm angezeigt wird.

Hintergrund ausgeführt wird, bietet auch die Möglichkeit bieten, live Feedback der Benutzer zu einem beliebigen Zeitpunkt während einer aktiven Trainings-Sitzung, wie z. B. das Senden einer Übermitteln von haptischem Warnung um ihren aktuellen Benutzer zu informieren.

Darüber hinaus ermöglicht die Ausführung der Hintergrund die app die Benutzeroberfläche schnell aktualisieren, damit der Benutzer die neuesten Daten verfügt, wenn sie auf ihrer Apple Watch-einen schnellen Überblick.

Um hohe Leistung für Apple Watch zu gewährleisten, sollte eine Watch-app mithilfe von Hintergrund ausgeführt wird die Verarbeitung im Hintergrund Akku zu sparen begrenzen. Wenn eine app einen hohen CPU im Hintergrund verwendet wird, kann es von WatchOS angehalten werden.

### <a name="enabling-background-running"></a>Aktivieren der Hintergrund ausführen

Zum Ausführen von Hintergrund zu aktivieren, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf der Watch-Erweiterung Companion iPhone-app die `Info.plist` Datei, die sie für die Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen: 

    [![](workout-apps-images/plist01.png "Die Datenquellensicht")](workout-apps-images/plist01.png#lightbox)
3. Fügen Sie einen neuen Schlüssel namens `WKBackgroundModes` und legen Sie die **Typ** zu `Array`: 

    [![](workout-apps-images/plist02.png "Fügen Sie einen neuen Schlüssel namens WKBackgroundModes hinzu.")](workout-apps-images/plist02.png#lightbox)
4. Hinzufügen eines neuen Elements in das Array mit den **Typ** von `String` und den Wert `workout-processing`: 

    [![](workout-apps-images/plist03.png "Hinzufügen eines neuen Elements in das Array mit dem Typ der Zeichenfolge und einem Wert des Trainings-Verarbeitung")](workout-apps-images/plist03.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

## <a name="starting-a-workout-session"></a>Trainings-Sitzung starten

Es gibt drei grundlegende Schritte für das Starten einer Sitzung Trainings:

[![](workout-apps-images/workout02.png "Die drei wichtigsten Schritte zum Starten einer Sitzung Trainings")](workout-apps-images/workout02.png#lightbox)

1. Die app muss die Autorisierung für den Zugriff auf Daten in HealthKit anfordern.
2. Erstellen Sie ein Trainings-Konfigurationsobjekt für den Typ des Trainings gestartet wird.
3. Erstellen Sie und starten Sie eine Trainings-Sitzung mit der neu erstellten Trainings-Konfiguration.

### <a name="requesting-authorization"></a>Eine Autorisierung anfordert

Damit eine app die Daten des Benutzers HealthKit zugreifen kann, müssen sie anfordern und Empfangen von Autorisierung des Benutzers. Je nach Art der Trainings-app können sie die folgenden Typen von Anforderungen stellen:

- Autorisierung zum Schreiben von Daten:
    - Training
- Autorisierung zum Lesen von Daten:
    - Verschwendeten Energie entfällt
    - Entfernung
    - Herz  

Bevor eine app mit Autorisierung anfordern kann, muss sie für den Zugriff auf HealthKit konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Entitlements.plist`, um sie zur Bearbeitung zu öffnen.
2. Scrollen Sie nach unten, und überprüfen Sie **HealthKit aktivieren**: 

    [![](workout-apps-images/auth01.png "Aktivieren von HealthKit aktivieren")](workout-apps-images/auth01.png#lightbox)
3. Speichern Sie die Änderungen in der Datei.
4. Befolgen Sie die Anweisungen in der [explizite App-ID und das Bereitstellungsprofil](~/ios/platform/healthkit.md) und [Zuordnen der App-ID und den Provisioning-Profil mit Ihrer Xamarin.iOS-App](~/ios/platform/healthkit.md) Teile der [– Einführung HealthKit](~/ios/platform/healthkit.md) Artikel, um die app richtig bereitstellen.
5. Verwenden Sie abschließend die Anweisungen in der [Programmierung Integrität Kit](~/ios/platform/healthkit.md) und [Berechtigung aus der der Benutzer anfordern](~/ios/platform/healthkit.md) Teile der [Einführung in HealthKit](~/ios/platform/healthkit.md) Artikel auf Anforderung die Autorisierung des Benutzers HealthKit Datenspeicher auf.

### <a name="setting-the-workout-configuration"></a>Festlegen der Konfiguration des Trainings

Trainings-Sitzungen werden erstellt, die mit einem Trainings-Konfigurationsobjekt (`HKWorkoutConfiguration`) den Trainings-Typ angibt (z. B. `HKWorkoutActivityType.Running`) und den Speicherort des Trainings (z. B. `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Erstellen eines Trainings Sitzung Delegaten 

Um die Ereignisse behandeln, die während einer Sitzung Trainings auftreten können, müssen die app eine Delegatinstanz von Trainings-Sitzung zu erstellen. Das Projekt eine neue Klasse hinzu, und basierend auf den `HKWorkoutSessionDelegate` Klasse. Das Beispiel, für eine outdoor ausführen können sie wie folgt aussehen:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }
        #endregion
    }
}
```

Diese Klasse erstellt mehrere Ereignisse, die ausgelöst werden, wenn sich der Zustand des Trainings Sitzung ändert (`DidChangeToState`) und das Trainings-Sitzung ein Fehler auftritt (`DidFail`). 

### <a name="creating-a-workout-session"></a>Erstellen einer Sitzung Trainings

Mit dem Trainings-Konfiguration und Trainings Sitzung Delegat erstellt, erstellen Sie eine neue Trainings-Sitzung, und starten Sie ihn anhand des Benutzers HealthKit Standardspeicher über:

```csharp
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...

private void StartOutdoorRun ()
{
    // Create a workout configuration
    var configuration = new HKWorkoutConfiguration () {
        ActivityType = HKWorkoutActivityType.Running,
        LocationType = HKWorkoutSessionLocationType.Outdoor
    };

    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (configuration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Wenn die app gestartet, diese Sitzung Trainings wird und der Benutzer wieder zu deren Zifferblatt Ihrer Apple Watch wechselt, wird ein kleine grüne "running Man" Symbol oben das Gesicht angezeigt:

[![](workout-apps-images/workout03.png "Ein kleine grüne ausgeführten Man Symbol über das Gesicht angezeigt")](workout-apps-images/workout03.png#lightbox)

Wenn der Benutzer dieses Symbol tippt, wird die zurück an die app verwendet werden.

## <a name="data-collection-and-control"></a>Die Datensammlung und -Steuerelement

Sobald eine Sitzung Trainings konfiguriert und gestartet wurde, benötigt die app zum Sammeln von Daten über die Sitzung (z. B. "Herzfrequenz von des Benutzers) und Steuerung des Status der Sitzung:

[![](workout-apps-images/workout04.png "Die Datensammlung und Steuerelement-Diagramm")](workout-apps-images/workout04.png#lightbox)

1. **Beobachten die Beispiele** – die app zum Abrufen von Informationen von HealthKit, die eine Aktion ausgeführt und dem Benutzer angezeigt werden muss.
2. **Überwachen von Ereignissen** – die app auf Ereignisse reagieren, die HealthKit oder über die Benutzeroberfläche der app (z. B. der Benutzer das Anhalten der Trainings) generiert werden muss.
3. **Geben Sie Ausführungsstatus** -Sitzung gestartet wurde und derzeit ausgeführt wird.
4. **Geben Sie die Status "angehalten"** – der Benutzer wurde die aktuelle Trainings Sitzung angehalten und kann es zu einem späteren Zeitpunkt neu starten. Der Benutzer kann zwischen den Zuständen ausgeführten und angehaltenen mehrmals in einer einzelnen Sitzung des Trainings wechseln.
5. **Trainings-Sitzung beenden** – zu einem beliebigen Zeitpunkt kann der Benutzer die Trainings-Sitzung beenden oder ablaufen und selbst bei getakteten Thema (z. B. eine Ausführung zwei Meilen) enden.

Im letzte Schritt werden die Ergebnisse der Sitzung Trainings in des Benutzers HealthKit Datenspeicher zu speichern.

### <a name="observing-healthkit-samples"></a>Beobachten von HealthKit-Beispiele

Öffnen Sie die app muss ein _Anker Objektabfrage_ für jede HealthKit, zeigt es ist interessant sind, wie z. B. Herzfrequenz oder aktive Energie gebrannt. Für jeden Datenpunkt, der überwacht wird müssen ein updatehandler erstellt werden, um neue Daten zu erfassen, wie sie an die app gesendet wird.

Die app kann über diese Datenpunkte Summen (z. B. die gesamte Ausführung Entfernung) sammeln und -Benutzeroberfläche nach Bedarf zu aktualisieren. Darüber hinaus kann die app den Benutzer benachrichtigen, wenn sie ein bestimmtes Ziel oder die Leistung wird z. B. Abschluss einer Ausführung der nächsten Meilen erreicht haben.

Sehen Sie sich im folgenden Beispielcode:

```csharp
private void ObserveHealthKitSamples ()
{
    // Get the starting date of the required samples
    var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

    // Get data from the local device
    var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
    var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

    // Assemble compound predicate
    var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

    // Get ActiveEnergyBurned
    var queryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
        // Valid?
        if (error == null) {
            // Yes, process all returned samples
            foreach (HKSample sample in addedObjects) {
                var quantitySample = sample as HKQuantitySample;
                ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);
            }
            
            // Update User Interface
            ...
        }
    });

    // Start Query
    HealthStore.ExecuteQuery (queryActiveEnergyBurned);
                                          
}
```

Erstellt ein Prädikat, das Startdatum festzulegen, der zum Abrufen von Daten für die Verwendung von sollen die `GetPredicateForSamples` Methode. Erstellt eine Gruppe von Geräten, die zum Abrufen von HealthKit Informationen aus der Verwendung der `GetPredicateForObjectsFromDevices` -Methode, in diesem Fall nur die lokalen Apple Watch (`HKDevice.LocalDevice`). Die beiden Prädikate werden in einer Verbundanweisung Prädikat kombiniert (`NSCompoundPredicate`) mithilfe der `CreateAndPredicate` Methode.

Ein neues `HKAnchoredObjectQuery` wird für den Datenpunkt mit dem gewünschten erstellt (in diesem Fall `HKQuantityTypeIdentifier.ActiveEnergyBurned` für den Datenpunkt Active Energie gebrannt), wird keine Beschränkung für die Menge der zurückgegebenen Daten (`HKSampleQuery.NoLimit`) und ein updatehandler zum Behandeln von Daten werden zurück an die app definiert ist von HealthKit. 

Vom updatehandler wird jedes Mal aufgerufen werden, die neue Daten an die app für den angegebenen Datenpunkt übermittelt werden. Wenn kein Fehler zurückgegeben wird, kann die app sicher lesen Sie die Daten aller erforderlichen Berechnungen und durch die Benutzeroberfläche nach Bedarf zu aktualisieren.

Der Code durchläuft alle Beispiele (`HKSample`) zurückgegeben, der `addedObjects` array und wandelt sie um ein Beispiel für die Menge (`HKQuantitySample`). Daraufhin wird den double-Wert des Beispiels als eine Joule (`HKUnit.Joule`) und akkumuliert diese in die gesamte ausgeführte aktive verschwendeten Energie entfällt für die Trainings und die Benutzeroberfläche aktualisiert.

### <a name="achieved-goal-notification"></a>Erzielte Ziel-Benachrichtigung

Wie oben erwähnt Wenn der Benutzer eine erzielt in das Trainings-app (z. B. Sie haben die erste Meile dar, der eine Ausführung), können sie übermitteln von haptischem Feedback für den Benutzer über die Taptic-Engine senden. Die app sollte auch dessen Benutzeroberfläche an diesem Punkt aktualisiert werden, weil der Benutzer sehr wahrscheinlich ihre Handgelenk, um das Ereignis, das das Feedback dazu aufgefordert werden, finden Sie unter ausgelöst wird.

Um das Übermitteln von haptischem Feedback wiederzugeben, verwenden Sie den folgenden Code:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Überwachen von Ereignissen

Ereignisse sind Zeitstempel definieren, die die app verwenden können, um bestimmte Punkte während des Trainings für den Benutzer zu verdeutlichen. Einige Ereignisse werden von der app erstellt und in das Thema gespeichert, und einige Ereignisse werden von HealthKit automatisch erstellt.

Um Ereignisse zu beobachten, die von HealthKit erstellt werden, die app überschreibt der `DidGenerateEvent` Methode der `HKWorkoutSessionDelegate`:

```csharp
using System.Collections.Generic;
...

public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Save HealthKit generated event
    WorkoutEvents.Add (@event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Lap:
        break;
    case HKWorkoutEventType.Marker:
        break;
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

Apple hat die folgenden neuen Ereignistypen in WatchOS 3 hinzugefügt:

- `HKWorkoutEventType.Lap` – Verwenden für Ereignisse, die jeweils denselben Abstand Teile der Trainings unterbrechen. Beispielsweise markieren eine Vorstellung von einer Spur während der Ausführung.
- `HKWorkoutEventType.Marker` -In das Thema für den beliebigen Stellen von Interesse sind. Erreichen z. B. einem bestimmten Zeitpunkt auf der Route einen outdoor-Lauf.

Diese neuen Typen von der app erstellt, und klicken Sie in das Thema für die spätere Verwendung bei der Erstellung von Diagrammen und Statistiken gespeichert werden können.

Um ein Ereignis für die Markierung zu erstellen, führen Sie folgende Schritte aus:

```csharp
using System.Collections.Generic;
...

public float MilesRun { get; set; }
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

public void ReachedNextMile ()
{
    // Create and save marker event
    var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
    WorkoutEvents.Add (markerEvent);

    // Notify user
    NotifyUserOfReachedMileGoal (++MilesRun);
}
```

Dieser Code erstellt eine neue Instanz eines Ereignisses Marker (`HKWorkoutEvent`) und speichert es in eine private Sammlung von Ereignissen (die weiter unten in der Sitzung Trainings geschrieben wird), und benachrichtigt den Benutzer über das Ereignis über Haptics.

### <a name="pausing-and-resuming-workouts"></a>Anhalten und Fortsetzen der Fitnessaktivitäten

Zu jedem Zeitpunkt in einer Sitzung Trainings kann der Benutzer vorübergehend anhalten der Trainings und es zu einem späteren Zeitpunkt fortsetzen. Beispielsweise kann ein ruhiges führen Sie einen wichtigen Aufruf und die Ausführung fortsetzen, nachdem der Aufruf abgeschlossen wurde unterbrochen.

Benutzeroberfläches der app sollte die Möglichkeit, Anhalten und Fortsetzen der Trainings (durch den Aufruf in HealthKit), so, dass die Apple Watch können sowohl Daten als auch Power Speicherplatz sparen, während der Benutzer ihre Aktivität angehalten hat. Darüber hinaus sollte die app alle neuen Datenpunkte ignorieren, die empfangen werden können, wenn die Trainings-Sitzung in einem angehaltenen Zustand ist.

HealthKit reagiert zum Anhalten und Fortsetzen der Aufrufe durch Anhalten und Fortsetzen von Ereignissen generieren. Während der Sitzung Trainings angehalten wird, werden keine neuen Ereignisse oder Daten werden an die app gesendet von HealthKit, bis die Sitzung fortgesetzt wird.

Verwenden Sie den folgenden Code zum Anhalten und Fortsetzen der Sitzung Trainings:

```csharp
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public HKWorkoutSession WorkoutSession { get; set;}
...

public void PauseWorkout ()
{
    // Pause the current workout
    HealthStore.PauseWorkoutSession (WorkoutSession);
}

public void ResumeWorkout ()
{
    // Pause the current workout
    HealthStore.ResumeWorkoutSession (WorkoutSession);
}
```

Das Anhalten und fortsetzen-Ereignisse, die von HealthKit generiert werden, können bearbeitet werden, durch Überschreiben der `DidGenerateEvent` Methode der `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);

    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.Pause:
        break;
    case HKWorkoutEventType.Resume:
        break;
    }
}
```

### <a name="motion-events"></a>Motion-Ereignisse

Auch neue watchos 3, werden die während der Übertragung angehalten (`HKWorkoutEventType.MotionPaused`) und während der Übertragung fortgesetzt (`HKWorkoutEventType.MotionResumed`) Ereignisse. Diese Ereignisse werden automatisch von HealthKit während einer laufenden Herausforderung ausgelöst, wenn der Benutzer beginnt, und nicht mehr bewegt.

Wenn die app ein Ereignis während der Übertragung angehalten empfängt, sollte sie anhalten, das Sammeln von Daten, bis der Benutzer wird, während der Übertragung fortgesetzt und die Bewegung fortgesetzt-Ereignis empfangen wird. App anhalten soll nicht über die Trainings-Sitzung in Reaktion auf ein Ereignis während der Übertragung angehalten.

> [!IMPORTANT]
> Die Bewegung angehalten und während der Übertragung Resume-Ereignisse werden nur unterstützt, für Aktivitätstyps RunningWorkout (`HKWorkoutActivityType.Running`).

In diesem Fall können diese Ereignisse behandelt werden, durch Überschreiben der `DidGenerateEvent` Methode der `HKWorkoutSessionDelegate`:

```csharp
public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
{
    base.DidGenerateEvent (workoutSession, @event);
    
    // Take action based on the type of event
    switch (@event.Type) {
    case HKWorkoutEventType.MotionPaused:
        break;
    case HKWorkoutEventType.MotionResumed:
        break;
    }
}

```

## <a name="ending-and-saving-the-workout-session"></a>Beendet, und speichern die Trainings-Sitzung

Wenn der Benutzer ihre Trainings abgeschlossen ist, müssen die app Beenden der aktuellen Trainings und speichern Sie sie in der Datenbank HealthKit. Fitnessaktivitäten HealthKit gespeichert werden automatisch in der Liste des Trainings-Aktivität angezeigt werden.

Neue IOS 10, die Aktivitätsliste Trainings Liste des Benutzers iPhone sowie dazu. Daher wird das Thema, selbst wenn der Apple Watch nicht in der Nähe befindet, auf dem Telefon angezeigt.

Fitnessaktivitäten, die Energie Samplings werden verschieben-Ring in der app für die Aktivitäten des Benutzers aktualisiert, damit 3rd Party-apps jetzt zu des Benutzers tägliche verschieben Ziele beitragen können.

Die folgenden Schritte sind erforderlich, beendet, und speichern eine Sitzung Trainings:

[![](workout-apps-images/workout05.png "Beendet, und speichern Sie das Diagramm des Trainings-Sitzung")](workout-apps-images/workout05.png#lightbox)

1. Zunächst müssen die app die Trainings-Sitzung zu beenden.
2. HealthKit wird die Trainings-Sitzung gespeichert.
3. Beispiele (z. B. verschwendeten Energie entfällt oder Entfernung) und der gespeicherten Trainings-Sitzung hinzugefügt.

### <a name="ending-the-session"></a>Beenden der Sitzung

Um die Trainings-Sitzung zu beenden, rufen die `EndWorkoutSession` -Methode der der `HKHealthStore` übergebe die `HKWorkoutSession`:

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
...

public void EndOutdoorRun ()
{
    // End the current workout session
    HealthStore.EndWorkoutSession (WorkoutSession);
}
```

Dadurch wird die Sensoren Geräte auf ihre normalen Modus zurückgesetzt. Wenn HealthKit abgeschlossen ist, endet die Trainings, erhält er einen Rückruf, der die `DidChangeToState` Methode der `HKWorkoutSessionDelegate`:

```csharp
public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
{
    // Take action based on the change in state
    switch (toState) {
    ...
    case HKWorkoutSessionState.Ended:
        StopObservingHealthKitSamples ();
        RaiseEnded ();
        break;
    }

}
```

### <a name="saving-the-session"></a>Speichern die Sitzung

Sobald die app die Trainings-Sitzung beendet wurde, müssen sie zum Thema zu erstellen (`HKWorkout`) und speichern Sie sie (zusammen mit einem Ereignisse) mit dem Datenspeicher HealthKit (`HKHealthStore`):

```csharp
public HKHealthStore HealthStore { get; private set; }
public HKWorkoutSession WorkoutSession { get; private set;}
public float MilesRun { get; set; }
public double ActiveEnergyBurned { get; set;}
public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
...

private void SaveWorkoutSession ()
{
    // Build required workout quantities 
    var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
    var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

    // Create any required metadata
    var metadata = new NSMutableDictionary ();
    metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

    // Create workout
    var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                    WorkoutSession.StartDate, 
                                    NSDate.Now, 
                                    WorkoutEvents.ToArray (), 
                                    energyBurned, 
                                    distance, 
                                    metadata);

    // Save to HealthKit
    HealthStore.SaveObject (workout, (successful, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (successful) {

            }
        } else {
            // Report error
        }
    });

}
```

Dieser Code erstellt die Gesamtmenge an Energie gebrannt und Abstand für den Trainings als erfordern `HKQuantity` Objekte. Ein Wörterbuch mit Metadaten zur Definition der Trainings wird erstellt, und der Speicherort der Trainings angegeben ist:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Ein neues `HKWorkout` -Objekt wird erstellt, mit dem gleichen `HKWorkoutActivityType` als die `HKWorkoutSession`, die Start- und Enddatum Datumsangaben, die Liste der Ereignisse (wird aus den oben aufgeführten Abschnitten akkumuliert), die verschwendeten Energie entfällt, gesamt, Abstand und die Metadaten-Wörterbuch. Dieses Objekt wird in der Health Store gespeichert, und alle Fehler behandelt.  

### <a name="adding-samples"></a>Beispiele hinzufügen

Wenn die app eine Reihe von Beispielen zum Thema speichert, generiert HealthKit eine Verbindung zwischen der Beispiele und das Thema, selbst, sodass die app zu einem späteren Zeitpunkt für alle Beispiele, die einem bestimmten Thema zugeordnet HealthKit abgefragt werden kann. Mithilfe dieser Informationen kann die app Generieren von Diagrammen aus den Daten des Trainings und für eine Zeitachse Trainings zu zeichnen.

Für eine app zum mitwirken an der Activity-app verschieben Ring muss Energy-Beispiele mit der gespeicherten Trainings enthalten sein. Darüber hinaus müssen die Gesamtwerte für Entfernung und Energieverbrauch die Summe der Beispiele übereinstimmen, die die app gespeicherten Thema zuordnet.

Um Beispiele für gespeicherte Thema hinzuzufügen, führen Sie folgende Schritte aus:

```csharp
using System.Collections.Generic;
using WatchKit;
using HealthKit;
...

public HKHealthStore HealthStore { get; private set; }
public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
...


private void SaveWorkoutSamples (HKWorkout workout)
{
    // Add samples to saved workout
    HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
        // Handle any errors
        if (error == null) {
            // Was the save successful
            if (success) {

            }
        } else {
            // Report error
        }
    });
}
```

Optional kann die app zu berechnen und erstellen eine kleinere Teilmenge von Beispiele oder nur ein Mega Beispiel (auf den gesamten Bereich der den Trainings), die dann mit der gespeicherten Trainings verknüpft abruft.

## <a name="workouts-and-ios-10"></a>Training und iOS 10

Jede WatchOS 3 Trainings-app besitzt eine übergeordnete iOS 10 basierend Trainings-app und für iOS-10, neue dieses iOS-app kann verwendet werden, um eine Herausforderung zu starten, platzieren die Apple Watch im Trainings-Modus (ohne Benutzereingriff) und WatchOS-app im Hintergrund ausführen ausgeführt wird (finden Sie unter [zu im Hintergrund ausgeführten](#about-background-running) oben Weitere Details).

Während die WatchOS-app ausgeführt wird, können sie WatchConnectivity für messaging und Kommunikation mit der übergeordneten iOS-app verwenden.

Sehen Sie sich die Funktionsweise dieses Prozesses:

[![](workout-apps-images/workout06.png "iPhone und ein Diagramm der Apple Watch-Kommunikation")](workout-apps-images/workout06.png#lightbox)

1. Die iPhone-app erstellt ein `HKWorkoutConfiguration` -Objekt und legt die Trainings-Typ und Speicherort fest.
2. Die `HKWorkoutConfiguration` Objekt wird gesendet, die Apple Watch-Version der app und, wenn es nicht bereits ausgeführt wird, wird es vom System gestartet.
3. Verwenden das übergebene in Trainings-Konfiguration, die WatchOS 3-app startet eine neue Trainings-Sitzung (`HKWorkoutSession`).

> [!IMPORTANT]
> In der Reihenfolge für die übergeordnete iPhone-app zu einer Herausforderung auf der Apple Watch muss die app WatchOS 3 Hintergrund ausgeführt wird, aktiviert sein. Informieren Sie sich [Hintergrund aktivieren ausgeführt wird.](#enabling-background-running) über weitere Details.

Dieser Prozess ist sehr ähnlich ist, an den Prozess des Trainings-Sitzung direkt in der WatchOS 3-app zu starten. Verwenden Sie auf dem iPhone den folgenden Code:

```csharp
using System;
using HealthKit;
using WatchConnectivity;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
#endregion
...

private void StartOutdoorRun ()
{
    // Can the app communicate with the watchOS version of the app?
    if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
        // Create a workout configuration
        var configuration = new HKWorkoutConfiguration () {
            ActivityType = HKWorkoutActivityType.Running,
            LocationType = HKWorkoutSessionLocationType.Outdoor
        };

        // Start watch app
        HealthStore.StartWatchApp (configuration, (success, error) => {
            // Handle any errors
            if (error == null) {
                // Was the save successful
                if (success) {
                    ...
                }
            } else {
                // Report error
                ...
            }
        });
    }
}
```

Dieser Code stellt sicher, dass die WatchOS-Version der app installiert ist und die iPhone-Version kann zunächst eine Verbindung her:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

Erstellt. anschließend eine `HKWorkoutConfiguration` wie gewohnt und verwendet die `StartWatchApp` Methode der `HKHealthStore` senden Sie sie an der Apple Watch und starten Sie die app und die Trainings-Sitzung.

An der watchos-app, mit dem folgenden Code in die `WKExtensionDelegate`:

```csharp
using WatchKit;
using HealthKit;
...

#region Computed Properties
public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
public OutdoorRunDelegate RunDelegate { get; set; }
#endregion
...


public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
{
    // Create workout session
    // Start workout session
    NSError error = null;
    var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

    // Successful?
    if (error != null) {
        // Report error to user and return
        return;
    }

    // Create workout session delegate and wire-up events
    RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

    RunDelegate.Failed += () => {
        // Handle the session failing
    };

    RunDelegate.Paused += () => {
        // Handle the session being paused
    };

    RunDelegate.Running += () => {
        // Handle the session running
    };

    RunDelegate.Ended += () => {
        // Handle the session ending
    };

    // Start session
    HealthStore.StartWorkoutSession (workoutSession);
}
```

Es dauert die `HKWorkoutConfiguration` und erstellt einen neuen `HKWorkoutSession` und fügt eine Instanz des benutzerdefinierten `HKWorkoutSessionDelegate`. Die Trainings-Sitzung wird anhand des Benutzers HealthKit Health Store gestartet.

## <a name="bringing-all-the-pieces-together"></a>Vereint alle der Teile

Nehmen alle Informationen in diesem Dokument dargestellt, können eine WatchOS 3-Basis Trainings-app und die übergeordnete iOS 10-Basis Trainings-app die folgenden Teile enthalten:

1. **iOS 10 `ViewController.cs`**  -behandelt das Starten einer Sitzung Watch-Konnektivität und Thema auf der Apple Watch.
2. **WatchOS 3 `ExtensionDelegate.cs`**  – behandelt die WatchOS 3-Version der Trainings-app.
3. **WatchOS 3 `OutdoorRunDelegate.cs`**  -eine benutzerdefinierte `HKWorkoutSessionDelegate` Ereignisse für das Thema behandelt.

> [!IMPORTANT]
> Der Code in den folgenden Abschnitten enthält nur die Teile, die zum Implementieren der neuen, erweiterten Funktionen bereitgestellt, um Trainings-apps in WatchOS 3 erforderlich. Alle zugrunde liegenden Code und den Code und die Benutzeroberfläche aktualisiert nicht enthalten ist, jedoch können problemlos anhand der übrigen WatchOS-Dokumentation erstellt werden.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

Die `ViewController.cs` Datei in der übergeordneten iOS 10-Version der Trainings-app würde den folgenden Code enthalten:

```csharp
using System;
using HealthKit;
using UIKit;
using WatchConnectivity;

namespace MonkeyWorkout
{
    public partial class ViewController : UIViewController
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set; } = new HKHealthStore ();
        public WCSession ConnectivitySession { get; set; } = WCSession.DefaultSession;
        #endregion

        #region Constructors
        protected ViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Private Methods
        private void InitializeWatchConnectivity ()
        {
            // Is Watch Connectivity supported?
            if (!WCSession.IsSupported) {
                // No, abort
                return;
            }

            // Is the session already active?
            if (ConnectivitySession.ActivationState != WCSessionActivationState.Activated) {
                // No, start session
                ConnectivitySession.ActivateSession ();
            }
        }

        private void StartOutdoorRun ()
        {
            // Can the app communicate with the watchOS version of the app?
            if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
                // Create a workout configuration
                var configuration = new HKWorkoutConfiguration () {
                    ActivityType = HKWorkoutActivityType.Running,
                    LocationType = HKWorkoutSessionLocationType.Outdoor
                };

                // Start watch app
                HealthStore.StartWatchApp (configuration, (success, error) => {
                    // Handle any errors
                    if (error == null) {
                        // Was the save successful
                        if (success) {
                            ...
                        }
                    } else {
                        // Report error
                        ...
                    }
                });
            }
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Start Watch Connectivity
            InitializeWatchConnectivity ();
        }
        #endregion
    }
}
```

### <a name="extensiondelegatecs"></a>ExtensionDelegate.cs

Die `ExtensionDelegate.cs` Datei in der WatchOS 3-Version der Trainings-app würde den folgenden Code enthalten:

```csharp
using System;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public HKHealthStore HealthStore { get; set;} = new HKHealthStore ();
        public OutdoorRunDelegate RunDelegate { get; set; }
        #endregion

        #region Constructors
        public ExtensionDelegate ()
        {
            
        }
        #endregion

        #region Private Methods
        private void StartWorkoutSession (HKWorkoutConfiguration workoutConfiguration)
        {
            // Create workout session
            // Start workout session
            NSError error = null;
            var workoutSession = new HKWorkoutSession (workoutConfiguration, out error);

            // Successful?
            if (error != null) {
                // Report error to user and return
                return;
            }

            // Create workout session delegate and wire-up events
            RunDelegate = new OutdoorRunDelegate (HealthStore, workoutSession);

            RunDelegate.Failed += () => {
                // Handle the session failing
                ...
            };

            RunDelegate.Paused += () => {
                // Handle the session being paused
                ...
            };

            RunDelegate.Running += () => {
                // Handle the session running
                ...
            };

            RunDelegate.Ended += () => {
                // Handle the session ending
                ...
            };
            
            RunDelegate.ReachedMileGoal += (miles) => {
                // Handle the reaching a session goal
                ...
            };

            RunDelegate.HealthKitSamplesUpdated += () => {
                // Update UI as required
                ...
            };

            // Start session
            HealthStore.StartWorkoutSession (workoutSession);
        }

        private void StartOutdoorRun ()
        {
            // Create a workout configuration
            var workoutConfiguration = new HKWorkoutConfiguration () {
                ActivityType = HKWorkoutActivityType.Running,
                LocationType = HKWorkoutSessionLocationType.Outdoor
            };

            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion

        #region Override Methods
        public override void HandleWorkoutConfiguration (HKWorkoutConfiguration workoutConfiguration)
        {
            // Start the session
            StartWorkoutSession (workoutConfiguration);
        }
        #endregion
    }
}
```

### <a name="outdoorrundelegatecs"></a>OutdoorRunDelegate.cs

Die `OutdoorRunDelegate.cs` Datei in der WatchOS 3-Version der Trainings-app würde den folgenden Code enthalten:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;
using HealthKit;

namespace MonkeyWorkout.MWWatchExtension
{
    public class OutdoorRunDelegate : HKWorkoutSessionDelegate
    {
        #region Private Variables
        private HKAnchoredObjectQuery QueryActiveEnergyBurned;
        #endregion

        #region Computed Properties
        public HKHealthStore HealthStore { get; private set; }
        public HKWorkoutSession WorkoutSession { get; private set;}
        public float MilesRun { get; set; }
        public double ActiveEnergyBurned { get; set;}
        public List<HKWorkoutEvent> WorkoutEvents { get; set; } = new List<HKWorkoutEvent> ();
        public List<HKSample> WorkoutSamples { get; set; } = new List<HKSample> ();
        #endregion

        #region Constructors
        public OutdoorRunDelegate (HKHealthStore healthStore, HKWorkoutSession workoutSession)
        {
            // Initialize
            this.HealthStore = healthStore;
            this.WorkoutSession = workoutSession;

            // Attach this delegate to the session
            workoutSession.Delegate = this;

        }
        #endregion

        #region Private Methods
        private void ObserveHealthKitSamples ()
        {
            // Get the starting date of the required samples
            var datePredicate = HKQuery.GetPredicateForSamples (WorkoutSession.StartDate, null, HKQueryOptions.StrictStartDate);

            // Get data from the local device
            var devices = new NSSet<HKDevice> (new HKDevice [] { HKDevice.LocalDevice });
            var devicePredicate = HKQuery.GetPredicateForObjectsFromDevices (devices);

            // Assemble compound predicate
            var queryPredicate = NSCompoundPredicate.CreateAndPredicate (new NSPredicate [] { datePredicate, devicePredicate });

            // Get ActiveEnergyBurned
            QueryActiveEnergyBurned = new HKAnchoredObjectQuery (HKQuantityType.Create (HKQuantityTypeIdentifier.ActiveEnergyBurned), queryPredicate, null, HKSampleQuery.NoLimit, (query, addedObjects, deletedObjects, newAnchor, error) => {
                // Valid?
                if (error == null) {
                    // Yes, process all returned samples
                    foreach (HKSample sample in addedObjects) {
                        // Accumulate totals
                        var quantitySample = sample as HKQuantitySample;
                        ActiveEnergyBurned += quantitySample.Quantity.GetDoubleValue (HKUnit.Joule);

                        // Save samples
                        WorkoutSamples.Add (sample);
                    }

                    // Inform caller
                    RaiseHealthKitSamplesUpdated ();
                }
            });

            // Start Query
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
                                                  
        }

        private void StopObservingHealthKitSamples ()
        {
            // Stop query
            HealthStore.StopQuery (QueryActiveEnergyBurned);
        }

        private void ResumeObservingHealthkitSamples ()
        {
            // Resume current queries 
            HealthStore.ExecuteQuery (QueryActiveEnergyBurned);
        }

        private void NotifyUserOfReachedMileGoal (float miles)
        {
            // Play haptic feedback
            WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);

            // Raise event
            RaiseReachedMileGoal (miles);
        }

        private void SaveWorkoutSession ()
        {
            // Build required workout quantities
            var energyBurned = HKQuantity.FromQuantity (HKUnit.Joule, ActiveEnergyBurned);
            var distance = HKQuantity.FromQuantity (HKUnit.Mile, MilesRun);

            // Create any required metadata
            var metadata = new NSMutableDictionary ();
            metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));

            // Create workout
            var workout = HKWorkout.Create (HKWorkoutActivityType.Running, 
                                            WorkoutSession.StartDate, 
                                            NSDate.Now, 
                                            WorkoutEvents.ToArray (), 
                                            energyBurned, 
                                            distance, 
                                            metadata);

            // Save to HealthKit
            HealthStore.SaveObject (workout, (successful, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (successful) {
                        // Add samples to workout
                        SaveWorkoutSamples (workout);
                    }
                } else {
                    // Report error
                    ...
                }
            });

        }

        private void SaveWorkoutSamples (HKWorkout workout)
        {
            // Add samples to saved workout
            HealthStore.AddSamples (WorkoutSamples.ToArray (), workout, (success, error) => {
                // Handle any errors
                if (error == null) {
                    // Was the save successful
                    if (success) {
                        ...
                    }
                } else {
                    // Report error
                    ...
                }
            });
        }
        #endregion

        #region Public Methods
        public void PauseWorkout ()
        {
            // Pause the current workout
            HealthStore.PauseWorkoutSession (WorkoutSession);
        }

        public void ResumeWorkout ()
        {
            // Pause the current workout
            HealthStore.ResumeWorkoutSession (WorkoutSession);
        }

        public void ReachedNextMile ()
        {
            // Create and save marker event
            var markerEvent = HKWorkoutEvent.Create (HKWorkoutEventType.Marker, NSDate.Now);
            WorkoutEvents.Add (markerEvent);

            // Notify user
            NotifyUserOfReachedMileGoal (++MilesRun);
        }

        public void EndOutdoorRun ()
        {
            // End the current workout session
            HealthStore.EndWorkoutSession (WorkoutSession);
        }
        #endregion

        #region Override Methods
        public override void DidFail (HKWorkoutSession workoutSession, NSError error)
        {
            // Handle workout session failing
            RaiseFailed ();
        }

        public override void DidChangeToState (HKWorkoutSession workoutSession, HKWorkoutSessionState toState, HKWorkoutSessionState fromState, NSDate date)
        {
            // Take action based on the change in state
            switch (toState) {
            case HKWorkoutSessionState.NotStarted:
                break;
            case HKWorkoutSessionState.Paused:
                StopObservingHealthKitSamples ();
                RaisePaused ();
                break;
            case HKWorkoutSessionState.Running:
                if (fromState == HKWorkoutSessionState.Paused) {
                    ResumeObservingHealthkitSamples ();
                } else {
                    ObserveHealthKitSamples ();
                }
                RaiseRunning ();
                break;
            case HKWorkoutSessionState.Ended:
                StopObservingHealthKitSamples ();
                SaveWorkoutSession ();
                RaiseEnded ();
                break;
            }

        }

        public override void DidGenerateEvent (HKWorkoutSession workoutSession, HKWorkoutEvent @event)
        {
            base.DidGenerateEvent (workoutSession, @event);

            // Save HealthKit generated event
            WorkoutEvents.Add (@event);

            // Take action based on the type of event
            switch (@event.Type) {
            case HKWorkoutEventType.Lap:
                ...
                break;
            case HKWorkoutEventType.Marker:
                ...
                break;
            case HKWorkoutEventType.MotionPaused:
                ...
                break;
            case HKWorkoutEventType.MotionResumed:
                ...
                break;
            case HKWorkoutEventType.Pause:
                ...
                break;
            case HKWorkoutEventType.Resume:
                ...
                break;
            }
        }
        #endregion

        #region Events
        public delegate void OutdoorRunEventDelegate ();
        public delegate void OutdoorRunMileGoalDelegate (float miles);

        public event OutdoorRunEventDelegate Failed;
        internal void RaiseFailed ()
        {
            if (this.Failed != null) this.Failed ();
        }


        public event OutdoorRunEventDelegate Paused;
        internal void RaisePaused ()
        {
            if (this.Paused != null) this.Paused ();
        }

        public event OutdoorRunEventDelegate Running;
        internal void RaiseRunning ()
        {
            if (this.Running != null) this.Running ();
        }

        public event OutdoorRunEventDelegate Ended;
        internal void RaiseEnded ()
        {
            if (this.Ended != null) this.Ended ();
        }

        public event OutdoorRunMileGoalDelegate ReachedMileGoal;
        internal void RaiseReachedMileGoal (float miles)
        {
            if (this.ReachedMileGoal != null) this.ReachedMileGoal (miles);
        }

        public event OutdoorRunEventDelegate HealthKitSamplesUpdated;
        internal void RaiseHealthKitSamplesUpdated ()
        {
            if (this.HealthKitSamplesUpdated != null) this.HealthKitSamplesUpdated ();
        }
        #endregion
    }
}
```

## <a name="best-practices"></a>Bewährte Methoden

Apple empfiehlt, verwenden die folgenden bewährten Methoden beim Entwerfen und Implementieren von Trainings-apps in WatchOS 3 und iOS 10:

- Stellen Sie sicher, dass die WatchOS 3-Trainings-app weiterhin funktionsfähig ist, selbst wenn es keine Verbindung mit dem iPhone und der iOS 10-Version der app möglich ist.
- Verwenden Sie HealthKit Entfernung aus, wenn GPS nicht verfügbar ist, da der Abstand ohne GPS generiert werden kann.
- Ermöglicht dem Benutzer das Thema über die Apple Watch oder dem iPhone zu starten.
- Ermöglichen der app, fitnessaktivitäten aus anderen Quellen (z. B. andere 3. apps von Drittanbietern) in die historische Datenansichten anzuzeigen.
- Stellen Sie sicher, dass die app nicht anzeigen, die gelöscht fitnessaktivitäten in historischen Daten durchführt.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt Apple Trainings-Apps in WatchOS 3 und wie sie in Xamarin implementiert wurde.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Einführung in HealthKit](~/ios/platform/healthkit.md)
