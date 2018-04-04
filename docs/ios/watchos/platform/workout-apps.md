---
title: Workout Apps
description: Dieser Artikel behandelt die Erweiterungen Apple hat versucht, Trainings-apps unter WatchOS 3 und wie diese in Xamarin implementiert.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 96eb2eaca15ed0bccbb4c5cdb6a855fc7e0e3bb1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="workout-apps"></a>Workout Apps

_Dieser Artikel behandelt die Erweiterungen Apple hat versucht, Trainings-apps unter WatchOS 3 und wie diese in Xamarin implementiert._


Neue WatchOS 3, Trainings bezüglich apps haben die Möglichkeit, im Hintergrund ausgeführt werden, auf der Apple Watch und erhalten Zugriff auf HealthKit Daten. Ihre übergeordnete iOS 10-basierten app hat zudem die Möglichkeit zum Starten der app WatchOS 3 basierend ohne Eingreifen des Benutzers.

Die folgenden Themen werden im Detail behandelt:

## <a name="about-workout-apps"></a>Zu Trainings-Apps

Benutzer, der Eignung und Trainings-App möglich hoch dedizierten macht mehrere Stunden eines Tages für ihre Ziele System- und Eignung. Daher erwarten, dass sie reagiert, einfach zu bedienenden apps, die genau erfassen und Anzeigen von Daten und nahtlose Integration mit Apple-Integrität.

Eine sorgfältig konzipierte Eignung oder Trainings-app können Benutzer, die ihre Aktivitäten zur Erreichung ihrer Ziele Eignung Diagramm. Mit der Apple Watch, gibt es Eignung und Trainings apps sofortigen Zugriff auf Herzfrequenz, Fettanteilen Burn und Aktivität Erkennung.

[![](workout-apps-images/workout01.png "Eignung und Trainings-app-Beispiel")](workout-apps-images/workout01.png#lightbox)

Neu bei WatchOS 3, _im Hintergrund ausgeführt_ bietet Trainings bezogene apps die Möglichkeit, im Hintergrund ausgeführt werden, auf der Apple Watch und erhalten Zugriff auf HealthKit Daten.

Dieses Dokument wird führen Sie die Funktion im Hintergrund ausgeführt wird, behandelt den Trainings-app-Lebenszyklus und zeigen, wie eine app Trainings beitragen kann, für des Benutzers _Aktivität Ringe_ auf der Apple Watch.

## <a name="about-workout-sessions"></a>Zu Trainings-Sitzungen

Das Kernstück von jedem Trainings-app ist eine _Trainings Sitzung_ (`HKWorkoutSession`), die der Benutzer starten und beenden kann. Die API des Trainings-Sitzung ist einfach zu implementieren und erhalten eine app Trainings bieten zahlreiche Vorteile wie z. B.:

- Während des Verschiebens und Fettanteilen brennen Erkennung basierend auf den Typ der Aktivität.
- Automatische Beitrag zum des Benutzers Aktivität Ringe.
- Klicken Sie in einer Sitzung wird die app automatisch angezeigt, wenn der Benutzer das Gerät (entweder durch ihre Handgelenk auslösen oder Interaktion mit der Apple Watch) aktiviert wird.

## <a name="about-background-running"></a>Zum Hintergrund ausführen

Wie bereits erwähnt, kann mit WatchOS 3 Trainings-app eingerichtet werden, im Hintergrund ausgeführt werden. Mithilfe der im Hintergrund ausgeführt wird, eine Trainings-app kann Daten aus dem Apple Watch-Sensoren verarbeiten, während der Ausführung im Hintergrund. Beispielsweise kann eine app weiterhin Herzfrequenz des Benutzers, zu überwachen, obwohl es nicht mehr auf dem Bildschirm angezeigt wird.

Im Hintergrund ausgeführte bietet auch die Möglichkeit zum Präsentieren von live Feedback an den Benutzer zu einem beliebigen Zeitpunkt während einer aktiven Trainings-Sitzung, wie etwa das Versenden einer haptic Warnung, die den Benutzer von ihren aktuellen Fortschritt zu informieren.

Darüber hinaus erlaubt die im Hintergrund ausgeführt, die app schnell seine Benutzeroberfläche aktualisieren, damit der Benutzer die neuesten Daten hat, wenn sie auf ihrer Apple Watch einen schnellen Überblick.

Um die hohe Leistung auf Apple Watch zu gewährleisten, sollte eine Watch-app mithilfe der im Hintergrund ausgeführt wird die Verarbeitung im Hintergrund zur Einsparung von Akku begrenzen. Wenn eine app eine übermäßige CPU während im Hintergrund verwendet wird, kann es vom WatchOS angehalten abrufen.

### <a name="enabling-background-running"></a>Aktivieren im Hintergrund ausgeführt wird

Damit kann im Hintergrund ausgeführt wird, führen Sie folgende Schritte aus:

1. In der **Projektmappen-Explorer**, doppelklicken Sie auf der Watch-Erweiterung Companion iPhone app `Info.plist` Datei zur Bearbeitung zu öffnen.
2. Wechseln Sie zu der **Quelle** anzeigen: 

    [![](workout-apps-images/plist01.png "Die Datenquellensicht")](workout-apps-images/plist01.png#lightbox)
3. Hinzufügen ein neues Schlüssels namens `WKBackgroundModes` und legen Sie die **Typ** auf `Array`: 

    [![](workout-apps-images/plist02.png "Fügen Sie einen neuen Schlüssel namens WKBackgroundModes hinzu.")](workout-apps-images/plist02.png#lightbox)
4. Hinzufügen eines neuen Elements in das Array mit den **Typ** von `String` und den Wert `workout-processing`: 

    [![](workout-apps-images/plist03.png "Hinzufügen eines neuen Elements in das Array mit der Zeichenfolge und einem Wert des Trainings-Verarbeitung")](workout-apps-images/plist03.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

## <a name="starting-a-workout-session"></a>Trainings-Sitzung starten

Es gibt drei grundlegende Schritte Trainings-Sitzung starten:

[![](workout-apps-images/workout02.png "Die drei wichtigsten Schritte zum Starten einer Sitzung Trainings")](workout-apps-images/workout02.png#lightbox)

1. Die app muss die Autorisierung für den Datenzugriff in HealthKit anfordern.
2. Erstellen Sie ein Konfigurationsobjekt Trainings für den Typ des Trainings gestartet wird.
3. Erstellen und Starten einer Trainings-Sitzung mit der neu erstellten Trainings-Konfiguration.

### <a name="requesting-authorization"></a>Autorisierung anfordern

Bevor eine app HealthKit-Daten des Benutzers zugreifen kann, müssen sie anfordern und Autorisierung vom Benutzer erhalten. Je nach Art der Trainings-app kann es die folgenden Typen von Anforderungen sinnvoll:

- Die Autorisierung zum Schreiben von Daten:
    - Training
- Die Autorisierung zum Lesen von Daten:
    - Energie gebrannt
    - Abstand
    - Herzfrequenz  

Bevor eine app Autorisierung anfordern kann, muss er für den Zugriff auf HealthKit konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Entitlements.plist`, um sie zur Bearbeitung zu öffnen.
2. Führen Sie einen Bildlauf nach unten, und überprüfen Sie **aktivieren HealthKit**: 

    [![](workout-apps-images/auth01.png "Check Enable HealthKit")](workout-apps-images/auth01.png#lightbox)
3. Speichern Sie die Änderungen in der Datei.
4. Befolgen Sie die Anweisungen in der [explizite App-ID und Bereitstellungsprofil](~/ios/platform/healthkit.md) und [Zuordnen der App-ID und die Provisioning-Profil mit der Xamarin.iOS App](~/ios/platform/healthkit.md) Abschnitte der [Einführung in HealthKit](~/ios/platform/healthkit.md) Artikel, um die app richtig bereitstellen.
5. Verwenden Sie abschließend die Anweisungen in der [Programmierung Integrität Kit](~/ios/platform/healthkit.md) und [Berechtigung aus der Benutzer anfordern](~/ios/platform/healthkit.md) Abschnitte der der [Einführung in HealthKit](~/ios/platform/healthkit.md) Artikel auf Anforderung die Autorisierung zum Zugriff auf die HealthKit Datenspeicher des Benutzers.

### <a name="setting-the-workout-configuration"></a>Festlegen der Trainings-Konfiguration

Trainings-Sitzungen werden mithilfe eines Objekts Trainings-Konfiguration erstellt (`HKWorkoutConfiguration`), die den Trainings-Typ angibt (z. B. `HKWorkoutActivityType.Running`) und den Speicherort des Trainings (z. B. `HKWorkoutSessionLocationType.Outdoor`):

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
    ActivityType = HKWorkoutActivityType.Running,
    LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Ein Delegat des Trainings-Sitzung erstellen 

Um die Ereignisse behandeln, die während einer Sitzung Trainings auftreten können, müssen die app Delegatinstanz einer Trainings-Sitzung zu erstellen. Das Projekt eine neue Klasse hinzu, und der Basis der `HKWorkoutSessionDelegate` Klasse. Beispielsweise ein den ausführen kann er wie folgt aussehen:

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

Diese Klasse erstellt mehrere Ereignisse, die ausgelöst werden, wie der Status des Trainings Sitzung ändert (`DidChangeToState`) und wenn die Sitzung Trainings fehlschlägt (`DidFail`). 

### <a name="creating-a-workout-session"></a>Erstellen einer Sitzung Trainings

Mit dem Trainings-Konfiguration und die Trainings Sitzung Delegaten erstellt zum Erstellen einer neuen Trainings-Sitzung, und starten Sie ihn anhand des Benutzers HealthKit Standardspeicher oben:

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

Wenn die app diese Sitzung Trainings startet und der Benutzer wieder zu ihren Zifferblatt Watch wechselt, wird ein sehr kleines grünes "running Man"-Symbol oben Schriftart angezeigt:

[![](workout-apps-images/workout03.png "Ein sehr kleines grünes ausgeführten Man Symbol über die Oberfläche angezeigt")](workout-apps-images/workout03.png#lightbox)

Wenn der Benutzer auf dieses Symbol tippt, wird die zurück an die app verwendet.

## <a name="data-collection-and-control"></a>Datensammlung und -Steuerelement

Nachdem eine Sitzung Trainings konfiguriert und gestartet wurde, müssen die app Sammeln von Daten über die Sitzung (z. B. der Benutzer Herzfrequenz) und den Status der Sitzung zu steuern:

[![](workout-apps-images/workout04.png "Datensammlung und Steuerelement-Diagramm")](workout-apps-images/workout04.png#lightbox)

1. **Prüfen die Beispiele** -app Abrufen von Informationen aus HealthKit, die Reaktionen bewirkten und dem Benutzer angezeigt werden müssen.
2. **Beobachten von Ereignissen** – die app auf Ereignisse reagieren, die durch eine HealthKit oder der Benutzeroberfläche der Anwendung (z. B. der Benutzer das Anhalten der Trainings) generiert werden müssen.
3. **Geben Sie Ausführzustand** -Sitzung gestartet wurde und ausgeführt wird.
4. **Geben Sie die Status "angehalten"** -der Benutzer hat die aktuelle Trainings-Sitzung angehalten und kann es zu einem späteren Zeitpunkt neu starten. Der Benutzer kann zwischen der ausgeführten und angehaltenen Zustand mehrmals in einer einzelnen Sitzung des Trainings wechseln.
5. **Trainings-Sitzung beenden** – zu einem beliebigen Zeitpunkt kann der Benutzer die Trainings-Sitzung beendet oder laufen ab und beendet, selbst wenn es sich um einen gemessenen Trainings (z. B. einer Ausführung zwei Meilen) war.

Der letzte Schritt werden die Ergebnisse der Trainings Sitzung des Benutzers HealthKit Datenspeicher speichern.

### <a name="observing-healthkit-samples"></a>Beobachten von HealthKit-Beispiele

Öffnen Sie die app müssen eine _Anker Objektabfrage_ für jede HealthKit, zeigt es ist interessant sind, z. B. Herzfrequenz oder aktiven Energie gebrannt. Für jeden Datenpunkt, der beobachtet wird müssen ein updatehandler erstellt werden, um neue Daten zu erfassen, während sie an die app gesendet werden.

Diese Datenpunkte kann die app Summen (z. B. die Ausführung Gesamtabstand) sammeln und -Benutzeroberfläche nach Bedarf aktualisieren. Die app darüber hinaus kann Benutzer benachrichtigen, bei Erreichen eines bestimmten Ziels bzw. beim erreichen, z. B. die nächste Meile eines Testlaufs abschließen.

Betrachten Sie folgenden Code:

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

Erstellt ein Prädikat, das Startdatum festlegen, die er zum Abrufen von Daten möchte für die Verwendung der `GetPredicateForSamples` Methode. Erstellt eine Reihe von Geräten zum Abrufen von HealthKit Informationen aus der Verwendung der `GetPredicateForObjectsFromDevices` in diesem Fall den lokalen Apple Watch-Methode (`HKDevice.LocalDevice`). Die beiden Prädikate werden in eine zusammengesetzte Prädikat kombiniert (`NSCompoundPredicate`) mithilfe der `CreateAndPredicate` Methode.

Ein neues `HKAnchoredObjectQuery` wird für den Datenpunkt gewünscht erstellt (in diesem Fall `HKQuantityTypeIdentifier.ActiveEnergyBurned` für den Datenpunkt Active Energie gebrannt), keine Beschränkung für die Menge der zurückgegebenen Daten (`HKSampleQuery.NoLimit`) und ein updatehandler wird definiert, um die Daten werden zurück an die app verarbeiten aus HealthKit. 

Vom updatehandler wird jedes Mal aufgerufen werden, die neue Daten an die app für den angegebenen Datenpunkt übermittelt werden. Wenn kein Fehler zurückgegeben wird, die app kann sicher lesen Sie die Daten, stellen alle erforderlichen Berechnungen und seine Benutzeroberfläche nach Bedarf aktualisieren.

Der Code durchläuft alle Beispiele (`HKSample`) zurückgegeben, die der `addedObjects` array erstellt und wandelt diese zu einer Menge Stichprobe (`HKQuantitySample`). Dann wird von der double-Wert als eine Joule-Beispiel (`HKUnit.Joule`) und akkumuliert diese in die laufende Summe der aktiven Energie, die für den Trainings gebrannt und die Benutzeroberfläche aktualisiert.

### <a name="achieved-goal-notification"></a>Erzielten Ziel-Benachrichtigung

Wie oben erwähnt Wenn der Benutzer ein Ziel in der Trainings-app (z. B. beim Abschluss der ersten Meile eines Testlaufs) erreicht können sie haptic Feedback an den Benutzer über das Modul Taptic senden. Die app sollte der Benutzeroberfläche auch an diesem Punkt aktualisieren, da der Benutzer mehr als wahrscheinlich ihre Handgelenk, um das Ereignis, das das Feedback dazu aufgefordert werden, finden Sie unter ausgelöst wird.

Um das haptic Feedback wiederzugeben, verwenden Sie den folgenden Code ein:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Beobachten von Ereignissen

Ereignisse sind Zeitstempel definieren, die die app verwenden kann, um bestimmten Punkten während des Trainings eines Benutzers zu markieren. Einige Ereignisse werden direkt von der app erstellt und gespeichert werden, in dem Trainings und einige Ereignisse vom HealthKit automatisch erstellt werden.

Um Ereignisse zu beobachten, die von HealthKit erstellt werden, wird die app Überschreiben der `DidGenerateEvent` Methode der `HKWorkoutSessionDelegate`:

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

- `HKWorkoutEventType.Lap` -Ereignisse, die jeweils denselben Abstand Teile der Trainings unterbrochen werden. Z. B. für das Markieren einer Lap um eine Spur während der Ausführung an.
- `HKWorkoutEventType.Marker` – Für beliebige interessenschwerpunkte innerhalb der Trainings sind. Erreichen z. B. einem bestimmten Zeitpunkt auf der Route, der im freien-Ausführung.

Diese neuen Typen können von der app erstellt und in den Trainings für die spätere Verwendung bei der Erstellung von Diagrammen und Statistiken gespeichert werden.

Um ein Ereignis Marker zu erstellen, führen Sie folgende Schritte aus:

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

Dieser Code erstellt eine neue Instanz eines Ereignisses Marker (`HKWorkoutEvent`) und speichert es in einer privaten Sammlung von Ereignissen (das später mit dem Trainings-Sitzung geschrieben wird), und benachrichtigt den Benutzer, der das Ereignis über Haptics.

### <a name="pausing-and-resuming-workouts"></a>Anhalten und Fortsetzen der Training

Der Benutzer kann jederzeit während einer Sitzung Trainings vorübergehend anhalten der Trainings und es zu einem späteren Zeitpunkt fortsetzen. Beispielsweise können sie anhalten, ein den ausführen, um eine wichtige Anruf annehmen und die Ausführung fortsetzen, nachdem der Aufruf abgeschlossen wurde.

Benutzeroberfläche der Anwendung sollte Möglichkeit bietet, Anhalten und Fortsetzen von den Trainings (durch Aufrufen der HealthKit), damit der Apple Watch können sowohl Daten als auch Power Platz sparen, während der Benutzer ihre Aktivität angehalten hat. Außerdem sollten die app alle neuen Datenpunkte ignorieren, die empfangen werden kann, wenn die Trainings-Sitzung in einem angehaltenen Zustand ist.

HealthKit werden Antworten, zum Anhalten und Fortsetzen der Aufrufe durch Anhalten und Fortsetzen von Ereignissen generieren. Während der Sitzung Trainings angehalten wird, werden keine neuen Ereignisse oder Daten gesendet, um die app durch HealthKit, bis die Sitzung fortgesetzt wird.

Verwenden Sie den folgenden Code zum Anhalten und Fortsetzen einer Trainings-Sitzung aus:

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

Das Anhalten und fortsetzen Ereignisse, die von HealthKit generiert werden, können bearbeitet werden, durch Überschreiben der `DidGenerateEvent` Methode der `HKWorkoutSessionDelegate`:

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

### <a name="motion-events"></a>Während des Verschiebens-Ereignisse

Auch neue WatchOS 3, sind die Bewegung angehalten (`HKWorkoutEventType.MotionPaused`) und während des Verschiebens fortgesetzt (`HKWorkoutEventType.MotionResumed`) Ereignisse. Diese Ereignisse werden automatisch von HealthKit während einer laufenden Trainings ausgelöst, wenn der Benutzer wird gestartet und beendet, verschieben.

Wenn die app eine Bewegung angehalten-Ereignis empfängt, sollte diese angehalten, das Sammeln von Daten, bis der Benutzer fortgesetzt, während der Übertragung wird und das Ereignis wird während des Verschiebens fortgesetzt empfangen wird. App-app sollte die Trainings-Sitzung in Reaktion auf ein Ereignis Bewegung angehalten nicht anhalten.

> [!IMPORTANT]
> Die Bewegung angehalten und während des Verschiebens Resume-Ereignisse werden nur unterstützt, für den Aktivitätstyp RunningWorkout (`HKWorkoutActivityType.Running`).

Erneut, können diese Ereignisse verarbeitet werden, durch Überschreiben der `DidGenerateEvent` Methode der `HKWorkoutSessionDelegate`:

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

## <a name="ending-and-saving-the-workout-session"></a>Beenden und speichern die Trainings-Sitzung

Wenn der Benutzer ihre Trainings abgeschlossen hat, müssen die app die aktuelle Trainings-Sitzung beendet, und speichern Sie sie mit der HealthKit-Datenbank. Training HealthKit gespeichert werden automatisch in der Liste des Trainings-Aktivität angezeigt.

Neu für iOS-10, die diese enthält die Liste Trainings Aktivitätsliste stammen auf auch iPhone des Benutzers. Daher wird die Trainings, selbst wenn der Apple Watch nicht in Reichweite ist, auf dem Telefon angezeigt.

Fitnessaktivitäten, die Energie Samplings werden des Benutzers verschieben Ring in der app Aktivitäten aktualisiert, damit 3rd Party apps jetzt für die Benutzer täglich verschieben Ziele beitragen können.

Die folgenden Schritte sind erforderlich, Ende und das Speichern einer Trainings-Sitzung:

[![](workout-apps-images/workout05.png "Beenden und speichern das Diagramm des Trainings-Sitzung")](workout-apps-images/workout05.png#lightbox)

1. Zunächst müssen die app die Trainings-Sitzung zu beenden.
2. Trainings-Sitzung wird HealthKit gespeichert.
3. Fügen Sie alle Beispiele (z. B. Energie gebrannt oder Abstand) gespeicherte Trainings-Sitzung hinzu.

### <a name="ending-the-session"></a>Beim Beenden der Sitzung

Um die Trainings-Sitzung zu beenden, rufen die `EndWorkoutSession` Methode der `HKHealthStore` übergibt der `HKWorkoutSession`:

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

Dadurch wird die Geräte-Sensoren ihren normalen Modus zurückgesetzt. Wenn HealthKit abgeschlossen ist, endet die Trainings, erhält er einen Rückruf an den `DidChangeToState` Methode der `HKWorkoutSessionDelegate`:

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

### <a name="saving-the-session"></a>Speichern der Sitzungs

Sobald die app die Trainings-Sitzung beendet ist, müssen sie eine Trainings erstellen (`HKWorkout`) und speichern Sie sie (zusammen mit einem Ereignisse) mit dem Datenspeicher HealthKit (`HKHealthStore`):

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

Dieser Code erstellt die Gesamtmenge an Energie gebrannt und Abstand für den Trainings als verlangen `HKQuantity` Objekte. Ein Wörterbuch von Metadaten zur Definition der Trainings wird erstellt, und der Speicherort des der Trainings angegeben wird:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Ein neues `HKWorkout` -Objekt wird erstellt, mit dem gleichen `HKWorkoutActivityType` als die `HKWorkoutSession`, die Start- und Enddatum Datumsangaben, die Liste der Ereignisse (wird in den Abschnitten oben kumuliert), gebrannt, Energie insgesamt, Entfernung und Metadaten Wörterbuch. Dieses Objekt wird in der Health Store gespeichert, und alle Fehler behandelt.  

### <a name="adding-samples"></a>Beispiele hinzufügen

Wenn die Anwendung eine Reihe von Beispielen zu einer Herausforderung speichert, generiert HealthKit eine Verbindung zwischen den Beispielen und den Trainings selbst, damit die app zu einem späteren Zeitpunkt für alle mit einem bestimmten Trainings verbundenen Beispiele HealthKit abgefragt werden kann. Mithilfe dieser Informationen kann die app Diagramme aus den Trainings-Daten zu generieren und anhand einer Zeitachse Trainings zu zeichnen.

Für eine app, die für der Activity-app verschieben Ring beitragen muss sie Energie Beispiele mit der gespeicherten Trainings enthalten. Darüber hinaus müssen die Gesamtwerte für Entfernung und Energie die Summe der Beispiele übereinstimmen, die die app mit einer gespeicherten Trainings zuordnet.

Um eine gespeicherte Trainings Beispiele hinzugefügt haben, führen Sie folgende Schritte aus:

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

Optional kann die app zu berechnen und erstellen eine kleinere Teilmenge des Beispiele oder nur eine Megapixel Probe (auf den gesamten Bereich der dem Trainings), die die gespeicherten Trainings zugeordnet wird, ruft er.

## <a name="workouts-and-ios-10"></a>Training und iOS 10

Jede WatchOS 3 Trainings-app besitzt eine übergeordnete iOS 10 basierend Trainings-app und für iOS-10, neue dieses iOS-app kann verwendet werden, um eine Herausforderung zu starten, platzieren die Apple Watch im Trainings-Modus (ohne Benutzereingriff) und WatchOS-app im Hintergrund ausführen ausgeführt wird (finden Sie unter [zu im Hintergrund ausgeführten](#About-Background-Running) oben Weitere Details).

Während die app WatchOS ausgeführt wird, können sie WatchConnectivity für Messaging- und Kommunikationsinfrastruktur, die mit der übergeordneten iOS-app verwenden.

Sehen Sie sich die Funktionsweise dieses Prozesses:

[![](workout-apps-images/workout06.png "iPhone und Apple Watch-Kommunikation Diagramm")](workout-apps-images/workout06.png#lightbox)

1. Die iPhone-app erstellt eine `HKWorkoutConfiguration` -Objekt und legt den Trainings-Typ und Speicherort.
2. Die `HKWorkoutConfiguration` Objekt der Apple Watch-Version der app wird gesendet, und wenn er nicht bereits ausgeführt wird, wird er vom System gestartet.
3. Verwenden die übergebene in Trainings-Konfiguration, die app WatchOS 3 startet eine neue Trainings-Sitzung (`HKWorkoutSession`).

> [!IMPORTANT]
> Damit für die übergeordnete iPhone-app eine Trainings auf der Apple Watch starten können muss die app WatchOS 3 im Hintergrund ausgeführt wird, aktiviert sein. Finden Sie unter [aktivieren im Hintergrund ausgeführt wird](#Enabling-Background-Running) oben Weitere Details.

Dieser Vorgang ist weitgehend mit der des Trainings-Sitzung direkt in der WatchOS 3-app zu starten. Verwenden Sie auf dem iPhone den folgenden Code:

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

Dieser Code stellt sicher, dass die WatchOS Version der app installiert ist und die iPhone-Version, wenn sie zuerst herstellen kann:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
    ...
}
```

Anschließend er erstellt eine `HKWorkoutConfiguration` wie gewohnt und verwendet die `StartWatchApp` Methode der `HKHealthStore` , senden sie an der Apple Watch und Starten der app, und die Trainings-Sitzung.

Und für die Überwachung OS-app verwenden Sie den folgenden Code in die `WKExtensionDelegate`:

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

Dauert die `HKWorkoutConfiguration` und erstellt einen neuen `HKWorkoutSession` und fügt eine Instanz des benutzerdefinierten `HKWorkoutSessionDelegate`. Trainings-Sitzung wird anhand des Benutzers HealthKit Health Store gestartet.

## <a name="bringing-all-the-pieces-together"></a>Kombinieren von allen die Angaben

Alle Informationen in diesem Dokument dargestellten geschaltet wurde, können eine WatchOS 3 basierend Trainings-app und seiner übergeordneten iOS 10 basierend Trainings-app die folgenden Teile enthalten:

1. **iOS 10 `ViewController.cs`**  -das Starten einer Sitzung Watch-Konnektivität und eine Trainings auf der Apple Watch-behandelt.
2. **WatchOS 3 `ExtensionDelegate.cs`**  -WatchOS 3-Version der app Trainings behandelt.
3. **WatchOS 3 `OutdoorRunDelegate.cs`**  -ein benutzerdefiniertes `HKWorkoutSessionDelegate` Ereignisse für den Trainings-Handle.

> [!IMPORTANT]
> Der Code in den folgenden Abschnitten dargestellten enthält nur die Teile, die zum Implementieren der neuen, erweiterten Funktionen bereitgestellt, um Trainings-apps in WatchOS 3 erforderlich. Alle unterstützenden Code und den Code vorhanden und aktualisiert die Benutzeroberfläche nicht enthalten ist, jedoch können problemlos erstellt werden, anhand der übrigen WatchOS-Dokumentation.<p/>



### <a name="viewcontrollercs"></a>ViewController.cs

Die `ViewController.cs` Datei in der übergeordneten iOS 10-Version der app Trainings würde den folgenden Code enthalten:

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

Die `ExtensionDelegate.cs` Datei in der WatchOS 3-Version der app Trainings würde den folgenden Code enthalten:

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

Die `OutdoorRunDelegate.cs` Datei in der WatchOS 3-Version der app Trainings würde den folgenden Code enthalten:

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

Apple empfiehlt, die folgenden bewährten Methoden beim Entwerfen und Implementieren von Trainings-apps in WatchOS 3 und iOS 10:

- Stellen Sie sicher, dass die WatchOS 3 Trainings app noch funktionsfähig ist, auch wenn es keine Verbindung mit dem iPhone und die iOS-10-Version der app möglich ist.
- Verwenden Sie HealthKit Abstand, wenn GPS nicht verfügbar ist, da der Abstand Beispielen ohne GPS generieren kann.
- Ermöglicht dem Benutzer die Trainings aus dem Apple Watch oder dem iPhone zu starten.
- Ermöglichen der app, fitnessaktivitäten aus anderen Quellen (z. B. andere 3rd Party-apps) in ihre Verlaufsdaten Ansichten anzuzeigen.
- Stellen Sie sicher, dass die app nicht anzeigen, die gelöscht Training in Vergangenheitsdaten verwendet wird.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt Apple hat versucht, Trainings-apps unter WatchOS 3 und wie diese in Xamarin implementiert.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
- [Einführung in HealthKit](~/ios/platform/healthkit.md)
