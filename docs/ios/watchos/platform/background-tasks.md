---
title: Hintergrundaufgaben
description: "Verwenden Sie die neue Hintergrundaufgaben WatchOS 3 sicherstellen, dass eine Watch-app immer die neuesten Daten und Andocken von Momentaufnahmen verfügt."
ms.topic: article
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/13/2017
ms.openlocfilehash: 83841e62d863bf4be4edef5c0b6b7d486f192f4d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/09/2018
---
# <a name="background-tasks"></a>Hintergrundaufgaben

_Verwenden Sie die neue Hintergrundaufgaben WatchOS 3 sicherstellen, dass eine Watch-app immer die neuesten Daten und Andocken von Momentaufnahmen verfügt._

Mit WatchOS 3 gibt es drei Hauptmethoden, eine Watch-app die Informationen auf dem neuesten Stand halten kann: 

- Verwenden eines der mehrere neue Hintergrundaufgaben. 
- Kann eine seiner Komplikationen auf dem Zifferblatt der Uhr (wodurch sie zusätzliche Zeit zum Aktualisieren). 
- Neue Docks müssen die Benutzer-Pin-App (, in denen die im Arbeitsspeicher beibehalten und häufig aktualisiert). 

## <a name="keeping-an-app-up-to-date"></a>Eine App auf dem neuesten Stand

Bevor die Erörterung der Möglichkeiten, die ein Entwickler einer WatchOS app Daten und zur Benutzeroberfläche laufend speichern kann, wird in diesem Abschnitt sehen Sie sich eine Reihe von Mustern verwenden und wie ein Benutzer zwischen ihrer iPhone und im Laufe des Tages basierend auf ihrer Apple Watch-verschoben werden können  die Zeit des Tages und der Aktivität sind sie derzeit (z. B. driving) ausführen.

Betrachten Sie das folgende Beispiel:

[![](background-tasks-images/update00.png "Wie ein Benutzer zwischen ihrer iPhone und ihre Apple Watch tagsüber verschoben werden können")](background-tasks-images/update00.png#lightbox)

1. In am Morgen während des Wartens in der Zeile für einen Kaffee durchsucht der Benutzer die aktuellen News auf ihrer iPhone mehrere Minuten dauern.
2. Vor dem Verlassen der Café, überprüfen sie schnell Wetter mit einem Komplikation auf ihre Zifferblatt der Uhr.
3. Vor Mittag verwenden sie die Maps-app auf dem iPhone-Methode, um einen nahe gelegenen Restaurant suchen und book eine Reservierung aus, um einen Client zu erfüllen.
4. Unterwegs an das Restaurant, erhalten sie eine Benachrichtigung auf ihrer Apple Watch und einen Blick, sie wissen, dass ihre Essen Termin spät ausgeführt wird.
5. Am Abend verwenden sie die Maps-app auf dem iPhone Datenverkehr zu überprüfen, bevor die Startseite beitragen.
6. Klicken Sie auf dem Weg home, erhält er eine Benachrichtigung iMessage auf ihrer Apple Watch bitten, damit einige Milch übernommen, und die schnelle Antwort-Funktion zum Senden der Antwort "OK" verwenden.

Aufgrund der "kurzer Blick" (weniger als drei Sekunden) Charakter wie ein Benutzer möchte in der Regel verwenden eine Apple Watch-app ist nicht genügend Zeit für die gewünschten Informationen abzurufen und aktualisieren ihre Benutzeroberfläche vor der Anzeige des Benutzers die app.

Mithilfe der neuen APIs Apple noch Bestandteil in WatchOS 3, kann die app planen, für eine _Aktualisierung im Hintergrund_ und haben Sie die gewünschte Informationen bereit, bevor der Benutzer fordert. Nehmen Sie das Beispiel weiter oben erläuterten Weather Problemen:

[![](background-tasks-images/update01.png "Ein Beispiel für das Wetter-Komplikation")](background-tasks-images/update01.png#lightbox)

1. Die app-Zeitpläne vom System zu einem bestimmten Zeitpunkt reaktiviert werden. 
2. Die app Ruft die Information, dass es benötigen, um das Update zu generieren.
3. Die app wird erneut generiert der Benutzeroberfläche, um die neuen Daten widerzuspiegeln.
4. Wenn der Benutzer auf die app-Komplikation Überblick, liegen auf dem neuesten Stand Informationen ohne dass der Benutzer warten, bis das Update.

Wie oben erwähnt, reaktiviert WatchOS System die app mithilfe einer oder mehrerer Aufgaben, von denen kann einen sehr eingeschränkten Pool zur Verfügung:

[![](background-tasks-images/update02.png "Das System WatchOS aktiviert wird, die app mithilfe einer oder mehrerer Aufgaben")](background-tasks-images/update02.png#lightbox)

Apple vorschlagen daran, die meisten dieser Task (da es einer beschränkten Ressource für die app ist) auf diesem gedrückt, bis die app aktualisiert selbst beendet ist.

Das System stellt diese Aufgaben durch Aufrufen der neuen `HandleBackgroundTasks` Methode der `WKExtensionDelegate` delegieren. Zum Beispiel:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeyWatch.MonkeySeeExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Constructors
        public ExtensionDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request here
            ...
        }
        #endregion
    }
}
```

Wenn die Anwendung die angegebene Aufgabe abgeschlossen ist, wird der Vorgang ausgeführt, das System durch die Markierung wurde:

[![](background-tasks-images/update03.png "Die Aufgabe zurückgibt, das System durch die Markierung wurde")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>Neue Hintergrundaufgaben

WatchOS 3 führt mehrere Hintergrundaufgaben, die die app nicht verwenden kann, die der Benutzer muss, bevor sie die app, z. B. Öffnen sicherstellen, dass den Inhalt kann Informationen aktualisieren:

- **App-Aktualisierung im Hintergrund** – der [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) Aufgabe ermöglicht der app, dessen Status im Hintergrund aktualisiert werden. Dazu gehören i. d. r. eine andere Aufgabe einschließlich des Herunterladens neuen Inhalts aus dem Internet mit einer [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Im Hintergrund aktualisieren Momentaufnahme** – der [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) Aufgabe ermöglicht der app, dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor das System eine Momentaufnahme darstellt, die zum Auffüllen des Docks verwendet werden.
- **Im Hintergrund Überwachungsfenster Konnektivität** – der [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) Task wird beim Empfang von Hintergrunddaten im mit gepaarten iPhone für die app gestartet.
- **URL-Sitzung im Hintergrund** – der [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) Vorgang wird für die app gestartet, wenn ein hintergrundübertragung erfordert eine Autorisierung oder abgeschlossen ist (erfolgreich oder fehlerhaft).

Diese Aufgaben werden in den folgenden Abschnitten im Detail behandelt werden.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

Die `WKApplicationRefreshBackgroundTask` ist eine allgemeine Aufgabe, die geplant werden kann, um die app zu einem späteren Zeitpunkt reaktiviert haben:

[![](background-tasks-images/update04.png "A WKApplicationRefreshBackgroundTask woken at a future date")](background-tasks-images/update04.png#lightbox)

Innerhalb der Runtime der Aufgabe, die app führen Sie beliebige lokale Verarbeitung z. B. Update eine Zeitachse Komplikation oder einige erforderliche Daten mit fetch kann eine `NSUrlSession`.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

Das System sendet eine `WKURLSessionRefreshBackgroundTask` Wenn die Daten wurde heruntergeladen und von der Anwendung verarbeitet werden:

[![](background-tasks-images/update05.png "Die WKURLSessionRefreshBackgroundTask, wenn die Daten, dessen Download abgeschlossen ist")](background-tasks-images/update05.png#lightbox)

Eine app ist nicht weiterhin ausgeführt, während Daten im Hintergrund heruntergeladen werden. Stattdessen die app plant die Anforderung für Daten, und klicken Sie dann er angehalten wurde, und das System behandelt das Herunterladen der Daten, die app nur reawakening, wenn der Download abgeschlossen ist.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

In WatchOS 3 fügte Apple Docks, in denen Benutzer ihre bevorzugten apps anheften und schnell darauf zugreifen können. Wenn der Benutzer auf der Seite auf der Apple Watch drückt, wird ein Katalog von angeheftete app Momentaufnahmen angezeigt. Der Benutzer kann Wischen Sie nach links oder rechts an die gewünschte app suchen, und tippen Sie dann die app zum Ersetzen der Momentaufnahme mit der ausgeführten app-Benutzeroberfläche starten.

[![](background-tasks-images/update06.png "Ersetzen die Momentaufnahme mit der ausgeführten apps-Schnittstelle")](background-tasks-images/update06.png#lightbox)

In regelmäßigen Abständen Momentaufnahmen der Benutzeroberfläche der Anwendung benötigte (durch Senden einer `WKSnapshotRefreshBackgroundTask`) und mithilfe dieser Momentaufnahmen Docks ausfüllt. WatchOS ermöglicht es der app auf dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor diese Momentaufnahme erstellt wird.

Momentaufnahmen sind in WatchOS 3 sehr wichtig, da sie als sowohl beim Preview und starten Sie Bilder für die app funktionieren. Wenn der Benutzer auf eine app im Dock hat, wird er in den Vollbildmodus zu erweitern, geben Sie den Vordergrund und start ausgeführt, daher ist es zwingend erforderlich, dass die Momentaufnahme auf dem neuesten Stand sein:

[![](background-tasks-images/update07.png "Wenn der Benutzer auf eine app im Dock hat, werden sie in den Vollbildmodus erweitert.")](background-tasks-images/update07.png#lightbox)

Das System erneut, das eine `WKSnapshotRefreshBackgroundTask` , damit die app (durch das Aktualisieren von Daten und der Benutzeroberfläche) vorbereiten kann, bevor die Momentaufnahme erstellt wird:

[![](background-tasks-images/update08.png "Die app kann vorbereiten, indem Sie die Daten und die Benutzeroberfläche zu aktualisieren, bevor die Momentaufnahme erstellt wird")](background-tasks-images/update08.png#lightbox)

Wenn die app kennzeichnet die `WKSnapshotRefreshBackgroundTask` abgeschlossen, das System wird automatisch eine Momentaufnahme der Benutzeroberfläche der Anwendung.

> [!IMPORTANT]
> **Hinweis:** unbedingt so planen Sie immer eine ` WKSnapshotRefreshBackgroundTask` nach dem die app haben neue Daten empfangen hat und aktualisiert seine Benutzeroberfläche oder dem Benutzer die geänderte Informationen nicht angezeigt.




Darüber hinaus, wenn der Benutzer eine Benachrichtigung über die app erhält und tippt, um die app wechselt in den Vordergrund zu bringen, muss die Momentaufnahme auf dem neuesten Stand sein, da er als auch der Startbildschirm fungiert:

[![](background-tasks-images/update09.png "Der Benutzer erhält eine Benachrichtigung aus der app, und tippt, um die app wechselt in den Vordergrund bringen")](background-tasks-images/update09.png#lightbox)

Wenn es mehr als einer Stunde seit der Benutzer mit einer app WatchOS interagiert hat, werden kann auf den Standardzustand zurückgeben. Der Standardstatus kann bedeuten, dass unterschiedliche Dinge an andere apps, und basierend auf den Entwurf einer App kann es möglicherweise keinen Standardstatus überhaupt.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

In WatchOS 3, Apple Watch-Konnektivität mit der im Hintergrund aktualisieren-API über das neue integriert hat `WKWatchConnectivityRefreshBackgroundTask`. Mit dieser neuen Funktion kann eine iPhone-app übermitteln, neue Daten mit seinem Pendant Watch-app während die app WatchOS im Hintergrund ausgeführt wird:

[![](background-tasks-images/update10.png "IPhone-app kann neue Daten mit seinem Pendant Watch-app übermitteln, während die app WatchOS im Hintergrund ausgeführt wird")](background-tasks-images/update10.png#lightbox)

Initiieren ein Komplikation Push, wird Kontext der App, senden eine Datei oder Benutzerinformationen aus der iPhone-app aktualisieren die Apple Watch-app im Hintergrund aktiviert.

Wenn die Watch-app über reaktiviert ist eine `WKWatchConnectivityRefreshBackgroundTask` müssen sie die standard-API-Methoden zu verwenden, um die Daten aus der iPhone-app zu erhalten.

[![](background-tasks-images/update11.png "Der Datenfluss WKWatchConnectivityRefreshBackgroundTask")](background-tasks-images/update11.png#lightbox)

1. Stellen Sie sicher, dass die Sitzung aktiviert hat.
2. Überwachen Sie die neue `HasContentPending` Eigenschaft so lange wie der Wert ist `true`, dem die app ist immer noch Daten für die Verarbeitung. Als sollte vor, die app der Aufgabe festzuhalten, bis alle Daten verarbeitet wurden.
3. Wenn keine weitere Daten verarbeitet werden (`HasContentPending = false`), markieren Sie die Aufgabe abgeschlossen wird, um das System wiederherzustellen. Wenn diese Änderung wird die app zugewiesenen Hintergrund Common Language Runtime führt einen Absturzbericht ausgeschöpft.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>Der Hintergrund-API-Lebenszyklus

Platzieren alle Teile des der neuen API zum Hintergrund Aufgaben gemeinsam, würde eine Reihe von Interaktionen wie folgt aussehen:

[![](background-tasks-images/update12.png "Der Hintergrund-API-Lebenszyklus")](background-tasks-images/update12.png#lightbox)

1. Die app WatchOS plant zunächst einen Hintergrund Aufgabe werden wurde aktiviert bei als bestimmten Zeitpunkt in der Zukunft.
2. Die app wird vom System und einen Task gesendet.
3. Die app verarbeitet den Vorgang zum Abschließen der Arbeit erforderlich war.
4. Als Ergebnis der Verarbeitung der Aufgabe an, die app Umständen muss Weitere Hintergrundinformationen Aufgaben mehr Arbeit in der Zukunft abgeschlossen wird, etwa ein Download von Inhalt mithilfe einer `NSUrlSession`.
5. Die app die Aufgabe abgeschlossen markiert und wird an das System zurückgegeben.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>Verwenden von Ressourcen bittet

Es ist wichtig, dass eine app WatchOS innerhalb dieses Ökosystem bittet verhält sich, durch die Begrenzung der zahlreiche freigegebene Systemressourcen.

Betrachten Sie das folgende Szenario:

[![](background-tasks-images/update13.png "Eine app WatchOS beschränkt, der zahlreiche freigegebene Ressourcen für das system")](background-tasks-images/update13.png#lightbox)

1. Der Benutzer startet eine app WatchOS 13:00 Uhr.
2. Die app plant eine Aufgabe zum reaktiviert und zum Herunterladen neuen Inhalts in einer Stunde 2:00 Uhr.
3. Um 13:50 Uhr öffnet erneut der Benutzer die app zu diesem Zeitpunkt die Daten und die Benutzeroberfläche aktualisieren kann.
4. Anstatt die Aufgabe Wake der app erneut in 10 Minuten sollte die app neu berechnen, die Aufgabe, um eine Stunde später um 2:50 Uhr ausgeführt.

Während jeder app abweicht, schlägt Apple beim Suchen von Mustern Nutzung wie angezeigten oben, um Systemressourcen zu sparen.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>Implementieren von Hintergrundaufgaben

Beispiel verwendet dieses Dokument die gefälschte MonkeySoccer Sport-app, die für den Benutzer Fußball Ergebnisse meldet. 

Betrachten Sie das folgende typische Verwendungsszenario:

[![](background-tasks-images/update14.png "Das typische Verwendungsszenario")](background-tasks-images/update14.png#lightbox)

Favoriten des Benutzers verfügen spielt eine big Übereinstimmung von 19:00 Uhr, 9:00 Uhr, damit die app erwarten, dass des Benutzers die Bewertung regelmäßig überprüft werden, und er entscheidet sich für ein Intervall von 30 Minuten aktualisieren.

1. Der Benutzer öffnet der app, und es plant eine Aufgabe für das Update im Hintergrund auf 30 Minuten später. Der Hintergrund-API können nur eine Art von Hintergrund Aufgabe zu einem bestimmten Zeitpunkt ausgeführt werden.
2. Die app die Aufgabe empfängt und aktualisiert die Daten und die Benutzeroberfläche und Zeitpläne für eine andere Hintergrundfarbe Task 30 Minuten später. Es ist wichtig, dass der Entwickler speichert, um eine andere Hintergrundfarbe Aufgabe zu planen, oder die app nie neu gestartet wird, weitere Updates abrufen.
3. Erneut, die app den Task empfängt und aktualisiert die Daten, aktualisiert die Benutzeroberfläche und plant eine andere Hintergrundfarbe Task 30 Minuten später.
4. Dasselbe Verfahren wird noch einmal wiederholt.
5. Der letzte Hintergrund, den Vorgang empfangen wird und die app erneut aktualisiert die Daten und die Benutzeroberfläche. Da dies das Endergebnis ist planen nicht es eine neue Hintergrund Aktualisierung. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>Planung für den Hintergrund aktualisieren

Angesichts der oben beschriebene Szenario, können die app MonkeySoccer den folgenden Code verwenden, um für ein Update im Hintergrund zu planen:

```csharp
private void ScheduleNextBackgroundUpdate ()
{
    // Create a fire date 30 minutes into the future
    var fireDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

    // Create 
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("LastActiveDate"), NSDate.FromTimeIntervalSinceNow(0));
    userInfo.Add (new NSString ("Reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleBackgroundRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Er erstellt ein neues `NSDate` 30 Minuten in der Zukunft When die app gestartet wird, werden möchte und erstellt eine `NSMutableDictionary` , halten Sie die Details des angeforderten Vorgangs. Die `ScheduleBackgroundRefresh` Methode der `SharedExtension` wird verwendet, um anzufordern, die Aufgabe geplant werden.

Das System eine `NSError` Wenn die angeforderte Aufgabe zu planen konnte.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>Das Update verarbeiten

Übernehmen Sie danach eine genauere Betrachtung der 5-Minuten-Fenster die erforderlichen Schritte zum Aktualisieren der Bewertung angezeigt:

[![](background-tasks-images/update15.png "Die 5-Minuten-Fenster, die mit die erforderlichen Schritte zum Aktualisieren der Bewertung")](background-tasks-images/update15.png#lightbox)

1. Um 7:30:02 Uhr wird die app vom System aktiviert und erhält die Update-Hintergrund-Aufgabe. Die erste Priorität werden die neuesten Ergebnisse vom Server abrufen. Finden Sie unter [Planen einer NSUrlSession](#Scheduling-a-NSUrlSession) unten.
2. Auf 7:30:05 die app die ursprüngliche Aufgabe abgeschlossen ist, das System die app in den Ruhezustand versetzt, und zum Herunterladen von der angeforderten Daten im Hintergrund fortgesetzt.
3. Wenn das System der Download abgeschlossen ist, erstellt es einen neuen Task für die app zu reaktivieren, damit die heruntergeladene Daten verarbeitet werden können. Finden Sie unter [Behandlung von Hintergrundaufgaben](#Handling-Background-Tasks) und [Behandlung der Download abgeschlossen](#Handling-the-Download-Completing) unten. 
4. Die app speichert die aktualisierte Informationen und die Aufgabe abgeschlossen markiert. Der Entwickler möglicherweise jedoch Apple vorgeschlagen Planen eines Tasks Momentaufnahme dieses Prozesses behandeln möchten, aktualisieren Sie der app-Benutzeroberfläche zu diesem Zeitpunkt. Finden Sie unter [planen ein Update der Momentaufnahme](#Scheduling-a-Snapshot-Update) unten.
5. Die app empfängt die Snapshot-Aufgabe, die Benutzeroberfläche aktualisiert und markiert die Aufgabe wurde abgeschlossen. Finden Sie unter [behandeln ein Update der Momentaufnahme](#Handling-a-Snapshot-Update) unten.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Planen einer NSUrlSession

So planen Sie das Herunterladen der aktuellsten Ergebnisse, kann der folgende Code verwendet werden:

```csharp
private void ScheduleURLUpdateSession ()
{
    // Create new configuration
    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration ("com.example.urlsession");

    // Create new session
    var backgroundSession = NSUrlSession.FromConfiguration (configuration);

    // Create and start download task
    var downloadTask = backgroundSession.CreateDownloadTask (new NSUrl ("https://example.com/gamexxx/currentScores.json"));
    downloadTask.Resume ();
}
```

Er konfiguriert und erstellt einen neuen `NSUrlSession`, dann mithilfe dieser Sitzung einen neuen Download Aufgabe mithilfe der `CreateDownloadTask` Methode. Ruft die `Resume` Methode von der Download-Task zum start der Sitzungs.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>Behandlung von Hintergrundaufgaben

Durch Überschreiben der `HandleBackgroundTasks` Methode der `WKExtensionDelegate`, die Anwendung kann die eingehende Hintergrundaufgaben verarbeiten:

```csharp
using System;
using System.Collections.Generic;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class ExtensionDelegate : WKExtensionDelegate
    {
        #region Computed Properties
        public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
        #endregion

        ...
        
        #region Public Methods
        public void CompleteTask (WKRefreshBackgroundTask task)
        {
            // Mark the task completed and remove from the collection
            task.SetTaskCompleted ();
            PendingTasks.Remove (task);
        }
        #endregion 

        #region Override Methods
        public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
        {
            // Handle background request
            foreach (WKRefreshBackgroundTask task in backgroundTasks) {
                // Is this a background session task?
                var urlTask = task as WKUrlSessionRefreshBackgroundTask;
                if (urlTask != null) {
                    // Create new configuration
                    var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

                    // Create new session
                    var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

                    // Keep track of all pending tasks
                    PendingTasks.Add (task);
                } else {
                    // Ensure that all tasks are completed
                    task.SetTaskCompleted ();
                }
            }
        }
        #endregion
        
        ...
    }
}
```

Die `HandleBackgroundTasks` Methode durchläuft alle Aufgaben, die das System die app gesendet hat (in `backgroundTasks`) Suche nach einem `WKUrlSessionRefreshBackgroundTask`. Falls vorhanden, wieder an der Sitzung teilnimmt und fügt eine `NSUrlSessionDownloadDelegate` Behandeln der Download abgeschlossen (finden Sie unter [behandeln das Herunterladen abgeschlossen](#Handling-the-Download-Completing) unten):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Darin sind ein Handle für den Task aus, bis er abgeschlossen wurde, indem sie Sie einer Sammlung hinzufügen:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Alle Aufgaben, die an die Anwendung gesendet müssen abgeschlossen werden, für jede Aufgabe, die nicht gerade verarbeitet wird, es abgeschlossen kennzeichnen:

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>Behandlung von der Download abgeschlossen

Die MonkeySoccer-app verwendet die folgenden `NSUrlSessionDownloadDelegate` Delegaten zu behandeln, der Download abgeschlossen und die angeforderten Daten zu verarbeiten:

```csharp
using System;
using Foundation;
using WatchKit;

namespace MonkeySoccer.MonkeySoccerExtension
{
    public class BackgroundSessionDelegate : NSUrlSessionDownloadDelegate
    {
        #region Computed Properties
        public ExtensionDelegate WatchExtensionDelegate { get; set; }

        public WKRefreshBackgroundTask Task { get; set; }
        #endregion

        #region Constructors
        public BackgroundSessionDelegate (ExtensionDelegate extensionDelegate, WKRefreshBackgroundTask task)
        {
            // Initialize
            this.WatchExtensionDelegate = extensionDelegate;
            this.Task = task;
        }
        #endregion

        #region Override Methods
        public override void DidFinishDownloading (NSUrlSession session, NSUrlSessionDownloadTask downloadTask, NSUrl location)
        {
            // Handle the downloaded data
            ...

            // Mark the task completed
            WatchExtensionDelegate.CompleteTask (Task);

        }
        #endregion
    }
}
```

Wenn initialisiert wird, können Sie ein Handle sowohl die `ExtensionDelegate` und `WKRefreshBackgroundTask` , die erzeugt es. Es überschreibt die `DidFinishDownloading` Methode zum Behandeln der Download abgeschlossen. Verwendet dann die `CompleteTask` Methode der `ExtensionDelegate` informiert die der Task, dass er abgeschlossen wurde, und entfernen Sie es aus der Auflistung der ausstehenden Aufgaben. Finden Sie unter [Behandlung von Hintergrundaufgaben](#Handling-Background-Tasks) oben.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>Planen ein Update der Momentaufnahme

So planen Sie eine Momentaufnahme-Aufgabe, die Benutzeroberfläche mit den neuesten Ergebnissen zu aktualisieren, kann der folgende Code verwendet werden:

```csharp
private void ScheduleSnapshotUpdate ()
{
    // Create a fire date of now
    var fireDate = NSDate.FromTimeIntervalSinceNow (0);

    // Create user info dictionary
    var userInfo = new NSMutableDictionary ();
    userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
    userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

    // Schedule for update
    WKExtension.SharedExtension.ScheduleSnapshotRefresh (fireDate, userInfo, (error) => {
        // Was the Task successfully scheduled?
        if (error == null) {
            // Yes, handle if needed
        } else {
            // No, report error
        }
    });
}
```

Ebenso wie `ScheduleURLUpdateSession` oben genannten Methode wird ein neuer `NSDate` für Wenn die app gestartet wird, werden möchte und erstellt eine `NSMutableDictionary` , halten Sie die Details des angeforderten Vorgangs. Die `ScheduleSnapshotRefresh` Methode der `SharedExtension` wird verwendet, um anzufordern, die Aufgabe geplant werden.

Das System eine `NSError` Wenn die angeforderte Aufgabe zu planen konnte.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>Behandeln ein Update der Momentaufnahme

Der Momentaufnahme-Vorgang behandelt den `HandleBackgroundTasks` Methode (finden Sie unter [Behandlung von Hintergrundaufgaben](#Handling-Background-Tasks) oben) wurde geändert, um wie folgt aussehen:

```csharp
public override void HandleBackgroundTasks (NSSet<WKRefreshBackgroundTask> backgroundTasks)
{
    // Handle background request
    foreach (WKRefreshBackgroundTask task in backgroundTasks) {
        // Take action based on task type
        if (task is WKUrlSessionRefreshBackgroundTask) {
            var urlTask = task as WKUrlSessionRefreshBackgroundTask;

            // Create new configuration
            var configuration = NSUrlSessionConfiguration.CreateBackgroundSessionConfiguration (urlTask.SessionIdentifier);

            // Create new session
            var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);

            // Keep track of all pending tasks
            PendingTasks.Add (task);
        } else if (task is WKSnapshotRefreshBackgroundTask) {
            var snapshotTask = task as WKSnapshotRefreshBackgroundTask;

            // Update UI
            ...

            // Create a expiration date 30 minutes into the future
            var expirationDate = NSDate.FromTimeIntervalSinceNow (30 * 60);

            // Create user info dictionary
            var userInfo = new NSMutableDictionary ();
            userInfo.Add (new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0));
            userInfo.Add (new NSString ("reason"), new NSString ("UpdateScore"));

            // Mark task complete
            snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
        } else {
            // Ensure that all tasks are completed
            task.SetTaskCompleted ();
        }
    }
}
```

Die Methode überprüft den Typ der Aufgabe, die verarbeitet werden. Es ist ein `WKSnapshotRefreshBackgroundTask` greift auf die Aufgabe:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Die Methode aktualisiert die Benutzeroberfläche und erstellt dann ein `NSDate` im System zu informieren, wenn die Momentaufnahme als veraltet sein wird. Erstellt eine `NSMutableDictionary` mit Benutzerinformationen zur Beschreibung der neuen Momentaufnahme und markiert die Snapshot-Aufgabe, die mit diesen Informationen abgeschlossen:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Darüber hinaus weist er auch der Momentaufnahme-Task, dass die app nicht auf den Standardzustand zurückgesetzt (in den ersten Parameter) zurückgibt. Diese Eigenschaft sollte immer auf von Apps, die über kein Konzept für Standardstatus festgelegt `true`.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>Effizient ausgeführt

Wie im obigen Beispiel des Fensters fünf Minuten, die die app MonkeySoccer gedauert um seine Ergebnisse zu aktualisieren hat, indem Sie effizient und mithilfe der neuen WatchOS 3 Hintergrundaufgaben, wird bei die app nur insgesamt 15 Sekunden lang aktiv war: 

[![](background-tasks-images/update16.png "Die app hat nur für insgesamt 15 Sekunden aktiv")](background-tasks-images/update16.png#lightbox)

Dies verringert die Auswirkungen, die die app auf der Apple Watch-Ressourcen verfügbar sind und Akkulaufzeit haben und außerdem erhält die app besser funktioniert mit anderen apps, die auf der Apple Watch ausgeführt wird.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>Funktionsweise von planen

Während eine WatchOS 3-app im Vordergrund ist, wird er immer zu, ausgeführt und können jede Art von Verarbeitung erforderlich, z. B. Aktualisierungsdaten oder neu zeichnen der Benutzeroberflächenautomatisierungs geplant. Wenn die app in den Hintergrund verschoben werden, er ist in der Regel vom System angehalten, und alle laufzeitvorgänge angehalten werden. 

Während die app im Hintergrund ist, kann es durch das System schnell auf eine bestimmte Aufgabe ausführen vorgesehen. Daher kann das System in WatchOS 2, vorübergehend eine app im Hintergrund Dinge behandeln eine lange aussehen-Benachrichtigung zu erledigen oder die app-Komplikation aktualisiert aktiviert. In WatchOS 3 gibt es mehrere neue Methoden, die eine app im Hintergrund ausgeführt werden kann.

Während eine app im Hintergrund ist, erzwingt das System einige Beschränkungen:

- Es erhält nur wenige Sekunden, um jede beliebige Aufgabe abzuschließen. Das System berücksichtigt nicht nur die Zeitspanne, die übergeben, sondern auch wie viel CPU-Leistung die app genutzt wird, um dieses Limit abgeleitet werden.
- Jeder app, die seine Grenzen überschreitet, wird mit den folgenden Fehlercodes aufgehoben:
    - **CPU** -0xc51bad01
    - **Zeit** -0xc51bad02
- Das System bedeuten verschiedene Grenzwerte, die basierend auf den Typ der Hintergrundtask, die sie zu die app gestellt hat. Beispielsweise `WKApplicationRefreshBackgroundTask` und `WKURLSessionRefreshBackgroundTask` Aufgaben etwas längere Laufzeiten gegenüber anderen Typen von Hintergrundaufgaben angegeben werden.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>Komplikationen und App-Updates

Zusätzlich zu der neuen Hintergrundaufgaben, die Apple WatchOS 3 hinzugefügt wurde, können eine WatchOS app Komplikationen beeinflusst wie und wann die app im Hintergrund aktualisiert wird.

Komplikationen sind kleine visuelle Elemente, die nützliche Informationen auf einen Blick zu bieten. Je nach dem Zifferblatt Watch ausgewählt ist hat der Benutzer die Möglichkeit, einem Überwachungsfenster Gesicht mit mindestens Komplikation anzupassen, die durch eine Watch-app in WatchOS 3 bereitgestellt werden können.

Wenn der Benutzer eines der app Komplikationen auf ihre Zifferblatt der Uhr enthält, sie bietet der app aktualisiert folgende Vorteile:

- Es bewirkt, dass das System eine kann jetzt zum Starten der app zu Status, in denen es versucht, die app im Hintergrund starten, bleibt sie im Arbeitsspeicher und es ausreichend zusätzliche Zeit aktualisiert.
- Komplikationen garantiert mindestens 50 Push-Updates pro Tag.

Der Entwickler sollte immer bemühen, erstellen überzeugende Komplikationen für ihre apps, um den Benutzer hinzufügen zu ihren Zifferblatt Watch den oben genannten Gründen verleiten.

In WatchOS 2 wurden Komplikationen die primäre Methode, dass eine app Common Language Runtime, während im Hintergrund erhalten. In WatchOS 3, wird eine app Komplikation noch mehrere Updates pro Stunde empfangen sichergestellt werden, es können jedoch `WKExtensions` weitere zur Laufzeit aktualisieren seiner Komplikationen anfordern.

Sehen Sie sich den folgenden Code zum Aktualisieren der Komplikation aus der verbundenen iPhone-app:

```csharp
using System;
using WatchConnectivity;
using UIKit;
using Foundation;
using System.Collections.Generic;
using System.Linq;
...

private void UpdateComplication ()
{

    // Get session and the number of remaining transfers
    var session = WCSession.DefaultSession;
    var transfers = session.RemainingComplicationUserInfoTransfers;

    // Create user info dictionary
    var iconattrs = new Dictionary<NSString, NSObject>
        {
            {new NSString ("lastActiveDate"), NSDate.FromTimeIntervalSinceNow (0)},
            {new NSString ("reason"), new NSString ("UpdateScore")}
        };

    var userInfo = NSDictionary<NSString, NSObject>.FromObjectsAndKeys (iconattrs.Values.ToArray (), iconattrs.Keys.ToArray ());

    // Take action based on the number of transfers left
    if (transfers < 1) {
        // No transfers left, either attempt to send or inform
        // user of situation.
        ...
    } else if (transfers < 11) {
        // Running low on transfers, only send on important updates
        // else conserve for a significant change.
        ...
    } else {
        // Send data
        session.TransferCurrentComplicationUserInfo (userInfo);
    }
}
```

Verwendet die `RemainingComplicationUserInfoTransfers` Eigenschaft von der `WCSession` um anzuzeigen, wie viele hoher Priorität die app übertragen hat für den Tag und dann basierend auf diese Zahl nimmt-Aktion verlassen. Wenn die app beginnt, Übertragungen erschöpft ist, können Sie halten aus weniger wichtigen Updates zu senden und nur Informationen senden, wenn eine signifikante Änderung aufweist.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>Planung und das Andocken

In WatchOS 3 fügte Apple Docks, in denen Benutzer ihre bevorzugten apps anheften und schnell darauf zugreifen können. Wenn der Benutzer auf der Seite auf der Apple Watch drückt, wird ein Katalog von angeheftete app Momentaufnahmen angezeigt. Der Benutzer kann Wischen Sie nach links oder rechts an die gewünschte app suchen, und tippen Sie dann die app zum Ersetzen der Momentaufnahme mit der ausgeführten app-Benutzeroberfläche starten.

[![](background-tasks-images/dock01.png "Das Andocken")](background-tasks-images/dock01.png#lightbox)

Das System wird in regelmäßigen Abständen Momentaufnahmen der Benutzeroberfläche der Anwendung akzeptiert und diese Momentaufnahmen zum Auffüllen von Dokumente verwendet wird. WatchOS ermöglicht es der app auf dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor diese Momentaufnahme erstellt wird.

Apps, die an die Dockingstation angeheftet wurden, können Folgendes erwarten:

- Sie erhalten eine Länge von mindestens 1 pro Stunde aktualisiert. Dies schließt eine App aktualisieren Aufgabe und eine Momentaufnahme-Aufgabe.
- Alle apps im Dock wird das Budget Update verteilt. Daher desto weniger apps, die der Benutzer angeheftet hat, die jede app erhält, mehr potenzielle Updates.
- Die app wird im Arbeitsspeicher beibehalten werden, damit die app schnell fortgesetzt wird, wenn das Andocken aktiviert.

Die letzte app, die der Benutzer ausgeführt wurde berücksichtigt werden die _zuletzt verwendeten_ app und den letzten Slot Docks belegt. Ab diesem Zeitpunkt gibt es Benutzer kann wählen, um es dauerhaft an die Dockingstation anheften. Die zuletzt verwendete Dateien werden wie jede andere bevorzugte app dem Benutzer bereits an die Dockingstation angeheftet hat behandelt.

> [!IMPORTANT]
> **Hinweis:** Apps, die nur auf dem Startbildschirm hinzugefügt wurden nicht erhält keine regulären Planung. Zum Empfangen von regulären planen und im Hintergrund aktualisiert, eine app _müssen_ Docks hinzugefügt werden.

Wie weiter oben in diesem Dokument erwähnt, sind Momentaufnahmen WatchOS 3 sehr wichtig, da sie als sowohl beim Preview und starten Sie Bilder für die app funktionieren. Wenn der Benutzer auf eine app im Dock hat, wird es in den Vollbildmodus zu erweitern, geben Sie den Vordergrund und start ausgeführt, daher ist es zwingend erforderlich, dass die Momentaufnahme auf dem neuesten Stand sein.

Es gibt möglicherweise Zeiten, wenn das System entscheidet, eine neue Momentaufnahme der Benutzeroberfläche der Anwendung benötigt. In diesen Situationen wird die Momentaufnahme-Anforderung für die Common Language Runtime das Budget für die app nicht gezählt. Im folgenden wird eine System-Momentaufnahme-Anfrage auszulösen:

- Eine Schwierigkeit Zeitachse-Update.
- Benutzerinteraktion mit einer app-Benachrichtigung.
- Wechseln in den Hintergrund-Zustand in den Vordergrund.
- Nach einer Stunde im Status "Hintergrund", kann die app so auf den Standardstatus zurück.
- Wenn WatchOS zuerst gestartet wird.

<a name="Best-Practices" />

## <a name="best-practices"></a>Bewährte Methoden 

Apple empfiehlt die folgenden bewährten Methoden bei der Arbeit mit Aufgaben im Hintergrund:

- Planen Sie so oft wie die app muss aktualisiert werden. Jedes Mal, wenn die app ausgeführt wird sollte es der zukünftigen Bedürfnisse neu zu bewerten und diesen Zeitplan nach Bedarf anpassen.
- Wenn das System ein Hintergrundtask zu aktualisieren sendet und die app ein Update erfordert, verzögern Sie die Arbeit, bis ein Update tatsächlich erforderlich sind.
- Berücksichtigen Sie alle Runtime Verkaufschancen an eine app zur Verfügung:
    - Andocken und Vordergrund-Aktivierung.
    - Benachrichtigungen werden gesendet.
    - Komplikation aktualisiert.
    - Im Hintergrund aktualisiert.
- Verwendung `ScheduleBackgroundRefresh` für allgemeine Hintergrund-Laufzeit, z. B.:
    - Das System Informationen abgerufen wird.
    - Planen Sie zukünftige `NSURLSessions` um Anforderungsdaten Hintergrund verwendet werden. 
    - Bekannte Zeit geht.
    - Auslösende Komplikation aktualisiert.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>Momentaufnahme bewährte Methoden

Bei der Arbeit mit Updates von Momentaufnahmen werden die folgenden Vorschläge Apple vorgenommen:

- Ungültig werden Sie Momentaufnahmen nur im Bedarfsfall, z. B. wenn eine erhebliche Änderung des Zertifikatinhalts vorhanden ist.
- Vermeiden Sie hochfrequente Momentaufnahme invalidierung. Z. B. eine Zeitgeber-app darf nicht die Momentaufnahme pro Sekunde aktualisiert, es sollte nur vorgenommen werden, wenn der Zeitgeber abgelaufen ist.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>App-Datenfluss

Apple empfehlen die folgenden für die Arbeit mit Datenfluss:

[![](background-tasks-images/update17.png "Flussdiagramm der App-Daten")](background-tasks-images/update17.png#lightbox)

Ein externes Ereignis (z. B. Überwachung Connectivity) aktiviert, wird die app. Dies erzwingt, dass die app ihre Datenmodell aktualisieren (die den aktuellen Status der apps darstellt). Daher der Datenmodell Änderung die app müssen die Komplikationen aktualisieren eine neue Momentaufnahme anfordern, starten Sie möglicherweise einen Hintergrund `NSURLSession` zum Abrufen von mehr Daten, und planen weiter im Hintergrund aktualisiert.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>Die App-Lebenszyklus

Aufgrund der Andocken sowie die Möglichkeit zum bevorzugten apps, Apple denkt, dass anheften, die Benutzer zwischen jedoch weitaus höher apps zu verschieben, hat viel häufiger, dann sie mit WatchOS 2. Die app sollte daher bereit, behandeln diese Änderung und schnell zwischen den Zuständen Vordergrund- und verschieben.

Apple hat die folgenden Vorschläge:

- Stellen Sie sicher, dass die app Hintergrundtasks so bald wie möglich nach der Eingabe Vordergrund Aktivierung abgeschlossen ist.
- Achten Sie darauf, die Fertigstellung aller Vordergrund Aufgaben vor dem Wechsel in des Hintergrunds durch Aufrufen von `NSProcessInfo.PerformExpiringActivity`.
- Beim Testen einer app in den WatchOS Simulator wird keines der Aufgabe Budgets erzwungen werden, damit eine app aktualisieren kann, wie erforderlich, um eine Anwendung ordnungsgemäß zu testen.
- Testen Sie stets auf echter Apple Watch-Hardware, um sicherzustellen, dass die app nicht hinter die Budgets vor der Veröffentlichung iTunes Connect ausgeführt wird.
- Apple wird vorgeschlagen, mit der Apple Watch auf dem Ladegerät beibehalten, beim Testen und Debuggen.
- Stellen Sie sicher, dass sowohl kalte starten und Fortsetzen einer app throughly getestet werden.
- Stellen Sie sicher, dass alle app-Aufgaben abgeschlossen werden.
- Die Anzahl der apps, die im Dock So testen Sie die am besten und schlechtesten festgesetzt sind variieren case-Szenarien.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt, die Apple gestellt, WatchOS und wie sie verwendet werden können, um eine Watch-app auf dem neuesten Stand zu halten. Zuerst behandelt alle der neuen Hintergrund Aufgabe Apple in WatchOS 3 hinzugefügt wurde. Es behandelt klicken Sie dann den Hintergrund-API-Lebenszyklus und wie Aufgaben im Hintergrund in einer Xamarin-app WatchOS implementiert. Schließlich behandelt wie scheduling funktioniert, und beim Anfügen erhalten hat einige bewährten Methoden.



## <a name="related-links"></a>Verwandte Links

- [iOS-10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
