---
title: watchos-Trainings-apps in xamarin
description: In diesem Artikel werden die Verbesserungen erläutert, die Apple zum Trainieren von apps in watchos 3 und deren Implementierung in xamarin vorgenommen hat.
ms.prod: xamarin
ms.assetid: F1D19635-A738-43E5-9873-1FC1BA44EEDF
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 03/17/2017
ms.openlocfilehash: f5a2b17491b026e08abf2262a998576cbb4356c5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767832"
---
# <a name="watchos-workout-apps-in-xamarin"></a>watchos-Trainings-apps in xamarin

_In diesem Artikel werden die Verbesserungen erläutert, die Apple zum Trainieren von apps in watchos 3 und deren Implementierung in xamarin vorgenommen hat._

Neu bei watchos 3: mit dem Training verbundene Apps können im Hintergrund des Apple Watch ausgeführt werden, und Sie erhalten Zugriff auf healthkit-Daten. Ihre übergeordnete IOS 10-basierte App bietet auch die Möglichkeit, die watchos 3-basierte App ohne Benutzereingriff zu starten.

Die folgenden Themen werden im Detail behandelt:

## <a name="about-workout-apps"></a>Informationen zu Trainings-apps

Benutzer von Fitness-und Trainings-Apps können stark dediziert sein und mehrere Stunden des Tages für Ihre Integritäts-und Fitnessziele aufwenden. Daher erwarten Sie reaktionsschnelle und benutzerfreundliche apps, die Daten genau erfassen und anzeigen und nahtlos in die Apple Health integrieren können.

Eine gut entworfene Fitness-oder Fitness-app hilft Benutzern dabei, ihre Aktivitäten so zu gestalten, dass Sie Ihre Eignung erreichen. Wenn Sie die Apple Watch verwenden, haben Fitness-und Workout-apps sofortigen Zugriff auf die Herzfrequenz, die Kalorienverbrennung und die Aktivitäts Erkennung.

[![](workout-apps-images/workout01.png "Beispiel für Fitness-und Trainings-App")](workout-apps-images/workout01.png#lightbox)

Neu bei watchos 3: bei der _Ausführung im Hintergrund_ können apps im Hintergrund auf dem Apple Watch ausgeführt werden, und Sie erhalten Zugriff auf healthkit-Daten.

Dieses Dokument enthält eine Einführung in die Funktion für das Hintergrund Feature, deckt den Lebenszyklus der APP-Lebensdauer ab und zeigt, wie eine-Trainings-APP an den _Aktivitäts Ringen_ des Benutzers im Apple Watch mitwirken kann.

## <a name="about-workout-sessions"></a>Informationen zu Trainings Sitzungen

Das Herzstück jeder Trainings-APP ist eine _Trainings Sitzung_ (`HKWorkoutSession`), die der Benutzer starten und Abbrechen kann. Die Trainings Sitzung-API ist einfach zu implementieren und bietet eine Reihe von Vorteilen für eine Fitness-app, wie z. b.:

- Bewegungserkennung und Kalorienverbrennung basierend auf dem Aktivitätstyp.
- Automatischer Beitrag zu den Aktivitäts Ringen des Benutzers.
- In einer Sitzung wird die APP automatisch angezeigt, wenn der Benutzer das Gerät aktiviert (indem Sie entweder das Handgelenk oder eine Interaktion mit dem Apple Watch).

## <a name="about-background-running"></a>Informationen zum Ausführen von Hintergrund

Wie oben bereits erwähnt, kann mit watchos 3 eine APP für das trainieren festgelegt werden, um im Hintergrund ausgeführt zu werden. Wenn Sie im Hintergrund eine Fitness-app ausführen, können Sie Daten aus den Apple Watch Sensoren verarbeiten, während Sie im Hintergrund ausgeführt werden. Beispielsweise kann eine APP die Frequenz Rate des Benutzers weiterhin überwachen, auch wenn Sie nicht mehr auf dem Bildschirm angezeigt wird.

Das Ausführen von Hintergrund Funktionen bietet auch die Möglichkeit, dem Benutzer während einer aktiven Trainings Sitzung Live Feedback zu präsentieren, wie z. b. das Senden einer willkürlichen Warnung, um den Benutzer über den aktuellen Fortschritt zu informieren.

Außerdem ermöglicht es der APP, die Benutzeroberfläche schnell zu aktualisieren, sodass der Benutzer die neuesten Daten hat, wenn er sich schnell einen Blick auf die Apple Watch hat.

Um eine hohe Leistung Apple Watch zu gewährleisten, sollte eine Watch-APP, die im Hintergrund ausgeführt wird, die Menge der Hintergrundarbeit begrenzen, um Akku zu sparen. Wenn eine APP übermäßige CPU-Auslastung im Hintergrund verwendet, kann Sie von watchos angehalten werden.

### <a name="enabling-background-running"></a>Aktivieren von Hintergrund Ausführung

Gehen Sie folgendermaßen vor, um den Hintergrund zu aktivieren:

1. Doppelklicken Sie im **Projektmappen-Explorer**auf die `Info.plist` Datei der begleitenden iPhone-App der Überwachungs Erweiterung, um Sie für die Bearbeitung zu öffnen.
2. Wechseln Sie zur **Quell** Ansicht: 

    [![](workout-apps-images/plist01.png "Die Quell Ansicht")](workout-apps-images/plist01.png#lightbox)
3. Fügen Sie einen neuen Schlüssel `WKBackgroundModes` namens hinzu, und legen `Array`Sie den **Typ** auf fest: 

    [![](workout-apps-images/plist02.png "Neuen Schlüssel mit dem Namen \"wkbackgroundmodes\" hinzufügen")](workout-apps-images/plist02.png#lightbox)
4. Fügen Sie dem Array ein neues Element mit dem **Typ** `String` `workout-processing`und dem Wert hinzu: 

    [![](workout-apps-images/plist03.png "Fügen Sie dem Array ein neues Element mit dem Typ der Zeichenfolge und dem Wert \"Training-Processing\" hinzu.")](workout-apps-images/plist03.png#lightbox)
5. Speichern Sie die Änderungen in der Datei.

## <a name="starting-a-workout-session"></a>Starten einer Trainings Sitzung

Zum Starten einer Trainings Sitzung sind drei Hauptschritte erforderlich:

[![](workout-apps-images/workout02.png "Die drei wichtigsten Schritte zum Starten einer Trainings Sitzung")](workout-apps-images/workout02.png#lightbox)

1. Die APP muss eine Autorisierung für den Zugriff auf Daten im healthkit anfordern.
2. Erstellen Sie ein Konfigurationsobjekt für den Betrieb für den Typ des Trainings, der gestartet wird.
3. Erstellen und starten Sie eine Trainings Sitzung mithilfe der neu erstellten Trainings Konfiguration.

### <a name="requesting-authorization"></a>Anfordern der Autorisierung

Bevor eine APP auf die healthkit-Daten des Benutzers zugreifen kann, muss Sie die Autorisierung vom Benutzer anfordern und empfangen. Abhängig von der Art der Trainings-App werden möglicherweise die folgenden Arten von Anforderungen vorgenommen:

- Autorisierung zum Schreiben von Daten:
  - Training
- Autorisierung zum Lesen von Daten:
  - Energie verbraucht
  - Flüge
  - Herzsatz  

Bevor eine APP eine Autorisierung anfordern kann, muss Sie für den Zugriff auf healthkit konfiguriert werden.

Führen Sie folgende Schritte aus:

1. Doppelklicken Sie im **Projektmappen-Explorer** auf die Datei `Entitlements.plist`, um sie zur Bearbeitung zu öffnen.
2. Scrollen Sie nach unten, und **Aktivieren Sie healthkit aktivieren**: 

    [![](workout-apps-images/auth01.png "Aktivieren von healthkit aktivieren")](workout-apps-images/auth01.png#lightbox)
3. Speichern Sie die Änderungen in der Datei.
4. Befolgen Sie die Anweisungen in den Abschnitten [explizite APP-ID und Bereitstellungs Profil](~/ios/platform/healthkit.md) und [Zuordnen der APP-ID und des Bereitstellungs Profils zu ihrer xamarin. IOS-App](~/ios/platform/healthkit.md) im Artikel [Introduction to healthkit](~/ios/platform/healthkit.md) , um die APP ordnungsgemäß bereitzustellen.
5. Verwenden Sie abschließend die Anweisungen im Artikel [Programmieren](~/ios/platform/healthkit.md) des Integritäts Kits und [anfordern der Berechtigung aus den Benutzern](~/ios/platform/healthkit.md) im Artikel [Introduction to healthkit](~/ios/platform/healthkit.md) , um die Autorisierung für den Zugriff auf den healthkit-Datenspeicher des Benutzers anzufordern.

### <a name="setting-the-workout-configuration"></a>Festlegen der Konfiguration für das Training

Trainings Sitzungen werden mithilfe eines Konfigurations Objekts für das Training`HKWorkoutConfiguration`() erstellt, das den trainingtyp `HKWorkoutActivityType.Running`(z. b.) und den `HKWorkoutSessionLocationType.Outdoor`Speicherort (z. b.) angibt:

```csharp
using HealthKit;
...

// Create a workout configuration
var configuration = new HKWorkoutConfiguration () {
  ActivityType = HKWorkoutActivityType.Running,
  LocationType = HKWorkoutSessionLocationType.Outdoor
};
```

### <a name="creating-a-workout-session-delegate"></a>Erstellen eines Trainings Sitzungs Delegaten 

Um die Ereignisse zu behandeln, die während einer Trainings Sitzung auftreten können, muss die APP eine trainingsitzungs-Delegatinstanz erstellen. Fügen Sie dem Projekt eine neue Klasse hinzu, und legen Sie Sie `HKWorkoutSessionDelegate` als Grundlage der-Klasse ab. Ein Beispiel für einen Outdoor-Testlauf könnte wie folgt aussehen:

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

Diese Klasse erstellt mehrere Ereignisse, die ausgelöst werden, wenn sich der Zustand der Trainings Sitzung ändert`DidChangeToState`(), und wenn die Trainings Sitzung`DidFail`fehlschlägt (). 

### <a name="creating-a-workout-session"></a>Erstellen einer Trainings Sitzung

Verwenden Sie den oben erstellten trainingkonfigurations-und trainingsitzungs Delegaten, um eine neue Trainings Sitzung zu erstellen und Sie mit dem standardmäßigen healthkit-Speicher des Benutzers zu starten:

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

Wenn die APP diese Trainings Sitzung startet, und der Benutzer wechselt zurück zu seiner Uhr, wird ein kleines grünes "ausgeführte Mann"-Symbol oberhalb des Gesichts angezeigt:

[![](workout-apps-images/workout03.png "Ein kleines grünes ausgeführte man-Symbol, das über dem Gesicht angezeigt wird")](workout-apps-images/workout03.png#lightbox)

Wenn der Benutzer auf dieses Symbol tippt, wird er an die APP zurückgeleitet.

## <a name="data-collection-and-control"></a>Datenerfassung und-Steuerung

Nachdem eine Trainings Sitzung konfiguriert und gestartet wurde, muss die APP Daten über die Sitzung erfassen (z. b. die Herzblut-Rate des Benutzers) und den Status der Sitzung Steuern:

[![](workout-apps-images/workout04.png "Diagramm zur Datenerfassung und-Steuerung")](workout-apps-images/workout04.png#lightbox)

1. **Beobachten von Beispielen** : die APP muss Informationen aus dem healthkit abrufen, die für den Benutzer bearbeitet und angezeigt werden.
2. **Beobachten von Ereignissen** : die APP muss auf Ereignisse reagieren, die von healthkit oder über die Benutzeroberfläche der APP (z. b. der Benutzer, der das Training anhält) generiert wird.
3. **Status** "wird ausgeführt": die Sitzung wurde gestartet und wird zurzeit ausgeführt.
4. **Status "angeh** alten" eingeben: der Benutzer hat die aktuelle Trainings Sitzung angehalten und kann Sie zu einem späteren Zeitpunkt neu starten. Der Benutzer kann in einer einzelnen Trainings Sitzung mehrmals zwischen den Status "wird ausgeführt" und "angehalten" wechseln.
5. **Beenden der Trainings Sitzung** : an jedem Punkt kann der Benutzer die Trainings Sitzung beenden, oder er kann ablaufen und seine eigenen Enden, wenn es sich um ein getaktetes Training (z. b. eine zwei-Kilometer-Laufzeit) handelt.

Der letzte Schritt besteht darin, die Ergebnisse der Trainings Sitzung im healthkit-Datenspeicher des Benutzers zu speichern.

### <a name="observing-healthkit-samples"></a>Beobachten von healthkit-Beispielen

Die APP muss eine _Anker Objekt Abfrage_ für jeden der gewünschten healthkit-Datenpunkte öffnen, wie z. b. Herz Raten oder aktive Energie. Für jeden Datenpunkt, der beobachtet wird, muss ein Update Handler erstellt werden, um neue Daten zu erfassen, wenn Sie an die APP gesendet werden.

Von diesen Datenpunkten kann die APP Gesamtsummen (z. b. die gesamte Laufdistanz) sammeln und die Benutzeroberfläche des Benutzers nach Bedarf aktualisieren. Darüber hinaus kann die App Benutzer benachrichtigen, wenn Sie ein bestimmtes Ziel oder eine bestimmte Aufgabe erreicht haben, z. b. die nächste Kilometer lange Ausführung.

Sehen Sie sich den folgenden Beispielcode an:

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

Es wird ein Prädikat erstellt, um das Startdatum festzulegen, mit dem Daten für die `GetPredicateForSamples` Verwendung der-Methode erstellt werden sollen. Er erstellt eine Gruppe von Geräten, von denen healthkit-Informationen mithilfe `GetPredicateForObjectsFromDevices` der-Methode abgerufen werden, in diesem Fall nur`HKDevice.LocalDevice`der lokale Apple Watch (). Die beiden Prädikate werden mithilfe der`NSCompoundPredicate` `CreateAndPredicate` -Methode in einem Verbund Prädikat () kombiniert.

Für den `HKAnchoredObjectQuery` gewünschten Datenpunkt wird ein neuer erstellt (in diesem Fall `HKQuantityTypeIdentifier.ActiveEnergyBurned` für den aktiven Strom Daten-Datenpunkt), und es wird keine Beschränkung für die Menge der zurück`HKSampleQuery.NoLimit`gegebenen Daten () festgelegt, und es wird ein Update Handler definiert, um Daten zu verarbeiten, die an die APP zurückgegeben werden. aus healthkit. 

Der Update Handler wird immer dann aufgerufen, wenn neue Daten für den angegebenen Datenpunkt an die APP übermittelt werden. Wenn kein Fehler zurückgegeben wird, kann die APP die Daten sicher lesen, alle erforderlichen Berechnungen durchführen und die Benutzeroberfläche nach Bedarf aktualisieren.

Der Code durchläuft alle`HKSample` `addedObjects` im Array zurückgegebenen Beispiele () und wandelt sie in eine Mengen Stichprobe (`HKQuantitySample`) um. Anschließend wird der Double-Wert des Beispiels als "Joule (`HKUnit.Joule`)" abgerufen und in die laufende Summe der aktiven Energie für das Training akkumuliert und die Benutzeroberfläche aktualisiert.

### <a name="achieved-goal-notification"></a>Ziel Benachrichtigung erreicht

Wie bereits erwähnt, kann der Benutzer, wenn er ein Ziel in der Trainings-App erreicht (z. b. die erste Kilometer der Ausführung), über das Taptic Engine haptisches Feedback an den Benutzer senden. Die APP sollte die Benutzeroberfläche zu diesem Zeitpunkt ebenfalls aktualisieren, da der Benutzer mehr als wahrscheinlich sein Handgelenk erhöht, um das Ereignis anzuzeigen, das das Feedback angefordert hat.

Verwenden Sie den folgenden Code, um das haptische Feedback wiederzugeben:

```csharp
// Play haptic feedback
WKInterfaceDevice.CurrentDevice.PlayHaptic (WKHapticType.Notification);
```

### <a name="observing-events"></a>Beobachten von Ereignissen

Ereignisse sind Zeitstempel, die von der APP verwendet werden können, um bestimmte Punkte während des Trainings des Benutzers hervorzuheben. Einige Ereignisse werden direkt von der App erstellt und im Training gespeichert, und einige Ereignisse werden automatisch durch healthkit erstellt.

Um Ereignisse zu beobachten, die von healthkit erstellt werden, überschreibt die `DidGenerateEvent` APP die- `HKWorkoutSessionDelegate`Methode des:

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

Apple hat die folgenden neuen Ereignis Typen in watchos 3 hinzugefügt:

- `HKWorkoutEventType.Lap`-Sind für Ereignisse, die das Training in gleiche Entfernungs Teile zerlegen. Beispielsweise zum Markieren einer Runde um eine Spur während der Ausführung.
- `HKWorkoutEventType.Marker`-Sind für beliebige relevante Punkte innerhalb des Trainings. Beispielsweise das Erreichen eines bestimmten Punkts auf der Route eines Outdoor-Laufs.

Diese neuen Typen können von der App erstellt und im Training gespeichert werden, um Sie später zum Erstellen von Diagrammen und Statistiken zu verwenden.

Gehen Sie folgendermaßen vor, um ein Markerereignis zu erstellen:

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

Dieser Code erstellt eine neue Instanz eines markerereignisses (`HKWorkoutEvent`) und speichert Sie in einer privaten Auflistung von Ereignissen (die später in die Trainings Sitzung geschrieben wird) und benachrichtigt den Benutzer über das Ereignis über die Haptik.

### <a name="pausing-and-resuming-workouts"></a>Anhalten und Fortsetzen von Workouts

An einem beliebigen Punkt in einer Trainings Sitzung kann der Benutzer das Training vorübergehend anhalten und zu einem späteren Zeitpunkt fortsetzen. Beispielsweise können Sie eine Indoor-Ausführung anhalten, um einen wichtigen-Befehl auszuführen und die Ausführung fortzusetzen, nachdem der-Vorgang abgeschlossen wurde.

Die Benutzeroberfläche der APP sollte eine Möglichkeit bieten, das Training anzuhalten und fortzusetzen (durch Aufrufen von healthkit), damit das Apple Watch sowohl Strom-als auch Datenspeicher Platz einsparen kann, während der Benutzer seine Aktivität angehalten hat. Außerdem sollte die APP alle neuen Datenpunkte ignorieren, die möglicherweise empfangen werden, wenn sich die Trainings Sitzung im angehaltenen Zustand befindet.

Healthkit antwortet auf Anhalten und Fortsetzen von Aufrufen durch das Erstellen von Pausen-und Fortsetzungs Ereignissen. Während die Trainings Sitzung angehalten wird, werden keine neuen Ereignisse oder Daten vom healthkit an die APP gesendet, bis die Sitzung fortgesetzt wird.

Verwenden Sie den folgenden Code, um eine Trainings Sitzung anzuhalten und fortzusetzen:

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

Die Pausen-und Fortsetzungs Ereignisse, die von healthkit generiert werden, können durch über `DidGenerateEvent` Schreiben der- `HKWorkoutSessionDelegate`Methode von verarbeitet werden:

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

### <a name="motion-events"></a>Bewegungs Ereignisse

Auch in watchos 3 neu ist, dass die Bewegung angehalten`HKWorkoutEventType.MotionPaused`() und Motion-`HKWorkoutEventType.MotionResumed`Fortsetzung ()-Ereignisse sind. Diese Ereignisse werden automatisch durch healthkit während eines laufenden Trainings ausgelöst, wenn der Benutzer startet und die Verschiebung beendet.

Wenn die APP ein Bewegungs angehaltene Ereignis empfängt, sollte Sie das Sammeln von Daten beenden, bis der Benutzer die Bewegung fortsetzt und das Motion setzt-Ereignis empfangen wird. Die APP sollte die Trainings Sitzung nicht als Reaktion auf ein Bewegungs angehaltene Ereignis anhalten.

> [!IMPORTANT]
> Die Bewegung angehalten und Motion Resume-Ereignisse werden nur für den runningworkout-Aktivitätstyp (`HKWorkoutActivityType.Running`) unterstützt.

Diese Ereignisse können wiederum durch Überschreiben der `DidGenerateEvent` -Methode `HKWorkoutSessionDelegate`von behandelt werden:

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

## <a name="ending-and-saving-the-workout-session"></a>Beenden und Speichern der Trainings Sitzung

Wenn der Benutzer das Training abgeschlossen hat, muss die APP die aktuelle Trainings Sitzung beenden und in der healthkit-Datenbank speichern. In healthkit gespeicherte Workouts werden automatisch in der Liste der Trainingsaktivitäten angezeigt.

Neu bei IOS 10. Dies schließt auch die Liste der Liste der Trainingsaktivitäten auf dem iPhone des Benutzers ein. Auch wenn der Apple Watch nicht in der Nähe ist, wird das Training auf dem Telefon angezeigt.

Durch das Training, das Energie Beispiele umfasst, wird der Verschiebungs Ring des Benutzers in der App "Activities" aktualisiert, damit apps von Drittanbietern nun zu den täglichen Bewegungs Zielen des Benutzers beitragen können.

Die folgenden Schritte sind erforderlich, um eine Trainings Sitzung zu beenden und zu speichern:

[![](workout-apps-images/workout05.png "Beenden und Speichern des Trainings Sitzungs Diagramms")](workout-apps-images/workout05.png#lightbox)

1. Zuerst muss die APP die Trainings Sitzung beenden.
2. Die Trainings Sitzung wird im healthkit gespeichert.
3. Fügen Sie der gespeicherten Trainings Sitzung beliebige Beispiele (z. b. Energie gespart oder Entfernung) hinzu.

### <a name="ending-the-session"></a>Beenden der Sitzung

Um die Trainings Sitzung zu beenden, wenden `EndWorkoutSession` Sie die- `HKHealthStore` Methode des an `HKWorkoutSession`, das übergibt:

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

Dadurch werden die Geräte Sensoren auf den normalen Modus zurückgesetzt. Wenn healthkit das Beenden des Trainings beendet, empfängt es einen Rückruf für die `DidChangeToState` -Methode `HKWorkoutSessionDelegate`des:

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

### <a name="saving-the-session"></a>Speichern der Sitzung

Nachdem die APP die Trainings Sitzung beendet hat, muss Sie ein Training (`HKWorkout`) erstellen und Sie (zusammen mit einem Ereignis) im healthkit-Datenspeicher (`HKHealthStore`) speichern:

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

Dieser Code erstellt die erforderliche Gesamtmenge an Energie, die für das Training als `HKQuantity` Objekte verbrannt ist. Ein Wörterbuch mit Metadaten, die das Training definieren, wird erstellt, und der Speicherort des Trainings wird angegeben:

```csharp
metadata.Add (new NSString ("HKMetadataKeyIndoorWorkout"), new NSString ("NO"));
```

Ein neues `HKWorkout` -Objekt wird mit `HKWorkoutActivityType` dem `HKWorkoutSession`, dem Start-und Enddatum, der Liste der Ereignisse (aus den obigen Abschnitten gesammelt), dem verbrannten Energieverbrauch, dem Total Distance-und dem Metadata-Wörterbuch erstellt. Dieses Objekt wird im Integritäts Speicher und alle behandelten Fehler gespeichert.  

### <a name="adding-samples"></a>Hinzufügen von Beispielen

Wenn die APP eine Reihe von Beispielen für ein Training speichert, generiert healthkit eine Verbindung zwischen den Beispielen und dem Training selbst, sodass die APP healthkit zu einem späteren Zeitpunkt für alle einem gegebenen Training zugeordneten Beispiele Abfragen kann. Mithilfe dieser Informationen kann die APP Diagramme aus den Trainingsdaten generieren und Sie mit einer Trainings Zeitachse zeichnen.

Damit eine APP zum Verschiebungs Ring der Aktivitäts-App beiträgt, muss Sie Energie Beispiele mit dem gespeicherten Training enthalten. Außerdem müssen die Summen für die Entfernung und die Energie der Summe aller Samplings entsprechen, die die APP einem gespeicherten Training zuordnet.

Gehen Sie folgendermaßen vor, um einem gespeicherten Training Beispiele hinzuzufügen:

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

Optional kann die APP eine kleinere Teilmenge von Beispielen oder ein Beispiel für ein Beispiel (das den gesamten Trainingsbereich umfasst) berechnen und erstellen, das dann dem gespeicherten Training zugeordnet wird.

## <a name="workouts-and-ios-10"></a>Workouts und IOS 10

Jede WatchOS 3 Trainings-app besitzt eine übergeordnete iOS 10 basierend Trainings-app und für iOS-10, neue dieses iOS-app kann verwendet werden, um eine Herausforderung zu starten, platzieren die Apple Watch im Trainings-Modus (ohne Benutzereingriff) und WatchOS-app im Hintergrund ausführen ausgeführt wird (finden Sie unter [zu im Hintergrund ausgeführten](#about-background-running) oben Weitere Details).

Während die watchos-app ausgeführt wird, kann Sie watchconnectivity für das Messaging und die Kommunikation mit der übergeordneten IOS-App verwenden.

Sehen Sie sich an, wie dieser Prozess funktioniert:

[![](workout-apps-images/workout06.png "Diagramm der iPhone-und Apple Watch Kommunikation")](workout-apps-images/workout06.png#lightbox)

1. Die iPhone-App erstellt `HKWorkoutConfiguration` ein-Objekt und legt den trainingtyp und den Speicherort fest.
2. Das `HKWorkoutConfiguration` Objekt wird Apple Watch Version der APP gesendet, und wenn es nicht bereits ausgeführt wird, wird es vom System gestartet.
3. Mithilfe der in der Trainings Konfiguration bestandenen Konfiguration wird von der watchos 3-App eine`HKWorkoutSession`neue Trainings Sitzung () gestartet.

> [!IMPORTANT]
> Damit die übergeordnete iPhone-App ein Training auf der Apple Watch starten kann, muss für die watchos 3-App eine Hintergrund Ausführung aktiviert sein. Weitere Informationen finden Sie unter Aktivieren des obigen [Hintergrunds](#enabling-background-running) .

Dieser Prozess ähnelt dem Vorgang, eine Trainings Sitzung direkt in der watchos 3-APP zu starten. Verwenden Sie auf dem iPhone den folgenden Code:

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

Mit diesem Code wird sichergestellt, dass die watchos-Version der APP installiert ist und die iPhone-Version zuerst eine Verbindung damit herstellen kann:

```csharp
if (ConnectivitySession.ActivationState == WCSessionActivationState.Activated && ConnectivitySession.WatchAppInstalled) {
  ...
}
```

Anschließend wird ein `HKWorkoutConfiguration` wie üblich erstellt und die `StartWatchApp` -Methode des `HKHealthStore` verwendet, um Sie an den Apple Watch zu senden und die APP und die Trainings Sitzung zu starten.

Verwenden Sie in der Watch OS-App den folgenden Code in `WKExtensionDelegate`:

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

Er erstellt ein neues `HKWorkoutSession` und fügt eine Instanz des benutzerdefinierten `HKWorkoutSessionDelegate`an. `HKWorkoutConfiguration` Die Trainings Sitzung wird für den Integritäts Speicher des Benutzers gestartet.

## <a name="bringing-all-the-pieces-together"></a>Zusammenführen aller Teile

Alle in diesem Dokument dargestellten Informationen, eine watchos 3-basierte Trainings-APP und die übergeordnete IOS 10-basierte Trainings-App können folgende Teile enthalten:

1. **IOS 10 `ViewController.cs`**  : übernimmt den Start einer Überwachungs Verbindungs Sitzung und ein Training auf der Apple Watch.
2. **watchos 3 `ExtensionDelegate.cs`**  : behandelt die watchos 3-Version der Workout-app.
3. **watchos 3 `OutdoorRunDelegate.cs`**  : eine Benutzer `HKWorkoutSessionDelegate` definierte zum Behandeln von Ereignissen für das Training.

> [!IMPORTANT]
> Der in den folgenden Abschnitten gezeigte Code umfasst nur die Teile, die zum Implementieren der neuen, erweiterten Features, die für das Training von apps in watchos 3 bereitgestellt werden, erforderlich sind. Der gesamte unterstützende Code und der Code zum darstellen und Aktualisieren der Benutzeroberfläche ist nicht enthalten, kann aber problemlos erstellt werden, indem wir unsere weitere watchos-Dokumentation befolgen.<p/>

### <a name="viewcontrollercs"></a>ViewController.cs

Die `ViewController.cs` Datei in der übergeordneten IOS 10-Version der Trainings-APP würde den folgenden Code enthalten:

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

Die `ExtensionDelegate.cs` Datei in der watchos 3-Version der Workout-APP würde den folgenden Code enthalten:

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

Die `OutdoorRunDelegate.cs` Datei in der watchos 3-Version der Workout-APP würde den folgenden Code enthalten:

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

Apple schlägt beim Entwerfen und Implementieren von Workout-apps in watchos 3 und IOS 10 die folgenden bewährten Methoden vor:

- Stellen Sie sicher, dass die watchos 3-Trainings-App weiterhin funktionsfähig ist, auch wenn keine Verbindung mit dem iPhone und der IOS 10-Version der APP möglich ist.
- Verwenden Sie healthkit Distance, wenn GPS nicht verfügbar ist, da es in der Lage ist, Entfernungs Beispiele ohne GPS zu generieren.
- Ermöglicht es dem Benutzer, das Training entweder über die Apple Watch oder das iPhone zu starten.
- Ermöglicht der APP das Anzeigen von Workouts aus anderen Quellen (z. b. anderen Drittanbieter-Apps) in den Verlaufs Datenansichten.
- Stellen Sie sicher, dass in der APP gelöschte Workouts in Verlaufs Daten nicht angezeigt werden.

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen erläutert, die Apple zum Trainieren von apps in watchos 3 und deren Implementierung in xamarin vorgenommen hat.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [Einführung in healthkit](~/ios/platform/healthkit.md)
