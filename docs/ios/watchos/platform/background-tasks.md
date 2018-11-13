---
title: WatchOS Hintergrundaufgaben in Xamarin
description: Dieses Dokument beschreibt, wie Sie Aufgaben im Hintergrund mit WatchOS in Xamarin ein Blick auf die Typen von Hintergrundaufgaben, die Verwendung von Ressourcen, Implementieren von Hintergrundtasks, Planung, bewährte Methoden und mehr zu verwenden.
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/13/2017
ms.openlocfilehash: 45886d787ecc40c9e11ce0c713ffa22819e29db2
ms.sourcegitcommit: 849bf6d1c67df943482ebf3c80c456a48eda1e21
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/12/2018
ms.locfileid: "51528818"
---
# <a name="watchos-background-tasks-in-xamarin"></a>WatchOS Hintergrundaufgaben in Xamarin

Bei WatchOS 3 gibt es drei Hauptmethoden, eine Watch-app die Informationen auf dem neuesten Stand halten kann: 

- Verwenden eine der mehrere neue Hintergrundaufgaben. 
- Müssen Sie eine der schwierigkeiten bei der die Art, auf dem Zifferblatt Ihrer Apple Watch (So erkennen sie zusätzliche Zeit, zum Aktualisieren). 
- Neue Dock müssen die Benutzer-Pin an app (, in dem die im Arbeitsspeicher beibehalten und häufig aktualisiert). 

## <a name="keeping-an-app-up-to-date"></a>Eine App auf dem neuesten Stand halten

Vor der Erläuterung aller Methoden, mit denen, die Entwickler Daten und die Benutzeroberfläche einer WatchOS-app laufend beibehalten kann, wird in diesem Abschnitt sehen Sie sich eine Reihe von Verwendungsmuster und wie ein Benutzer zwischen ihrer iPhone und im Laufe des Tages basierend auf ihrer Apple Watch-verschieben können  die Uhrzeit und der Aktivität sind diese derzeit (z. B. das steuernde) ausführen.

Betrachten Sie das folgende Beispiel:

[![](background-tasks-images/update00.png "Wie kann ein Benutzer zwischen eigene iPhones und ihre Apple Watch im Laufe des Tages verschieben")](background-tasks-images/update00.png#lightbox)

1. Am Vormittag während des Wartens in der Zeile für einen Kaffee durchsucht der Benutzer die aktuelle Nachricht auf ihrer iPhone für mehrere Minuten.
2. Vor dem Verlassen der kaffeehaus, können sie schnell das Wetter mit eine Komplikation auf ihre Zifferblatt Ihrer Apple Watch überprüft.
3. Vor dem Mittagessen verwenden sie den Karten-app auf dem iPhone finden eine nahegelegenen Restaurants und Buchen einer Reservierung aus, um einen Client zu erfüllen.
4. Sie erhalten eine Benachrichtigung, und ein kurzer Blick auf ihrer Apple Watch, sie wissen, dass ihre Mittagessen Termin spät ausgeführt wird, Reisen an das Restaurant.
5. Am Abend verwenden sie den Karten-app auf dem iPhone-Methode, um Datenverkehr zu überprüfen, bevor der Start einbringen.
6. Auf der Startseite, erhalten sie eine iMessage-Benachrichtigung auf ihrer Apple Watch-aufgefordert, um einige Milch zu übernehmen, und sie verwenden die kurze Antwort-Funktion zum Senden der Antwort "OK".

Aufgrund der "kurzer Blick" (weniger als drei Sekunden) Art, wie ein Benutzer entwickelt wird, mit einer Apple Watch-app gibt es in der Regel ist nicht genügend Zeit für die gewünschten Informationen abzurufen, und aktualisieren die Benutzeroberfläche vor der Anzeige für den Benutzer-app.

Mithilfe der neuen APIs von Apple ist enthalten in WatchOS 3, kann für die app Planen einer _Aktualisierung im Hintergrund_ und haben Sie die gewünschte Informationen bereit, bevor der Benutzer es anfordert. Betrachten Sie beispielsweise die weiter oben erläuterten Wetter-Komplikation:

[![](background-tasks-images/update01.png "Ein Beispiel für das Wetter-Komplikation")](background-tasks-images/update01.png#lightbox)

1. Die app-Zeitpläne auf einem bestimmten Zeitpunkt über das System reaktiviert werden. 
2. Die app Ruft die Informationen, die sie benötigen, um das Update zu generieren.
3. Die app generiert die Benutzeroberfläche entsprechend die neuen Daten neu.
4. Wenn der Benutzer auf die app Komplikation Überblick, hat es aktuelle Informationen, ohne dass der Benutzer warten, bis das Update.

Wie oben gezeigt, aktiviert der WatchOS-System die app mit einer oder mehrerer Aufgaben, von denen hat einen sehr begrenzten Pool zur Verfügung:

[![](background-tasks-images/update02.png "Die WatchOS-System aktiviert, die app mit einer oder mehrerer Aufgaben")](background-tasks-images/update02.png#lightbox)

Apple sollten daran, die meisten dieser Task (da es sich um eine eingeschränkte Ressource für die app ist), darum gedrückt halten, bis die app den Prozess der Aktualisierung selbst abgeschlossen ist.

Übermittelt das System diese Aufgaben durch Aufrufen der neuen `HandleBackgroundTasks` Methode der `WKExtensionDelegate` delegieren. Zum Beispiel:

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

Wenn die app auf die angegebene Aufgabe abgeschlossen ist, wird der Vorgang mit dem System durch abgeschlossen zu markieren:

[![](background-tasks-images/update03.png "Die Aufgabe zurückgegeben an das System, indem markiert wurde")](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks" />

## <a name="new-background-tasks"></a>Neue Aufgaben im Hintergrund

WatchOS 3 führt mehrere Aufgaben im Hintergrund, die eine app verwenden können, aktualisieren Sie die zugehörigen Informationen stellt sicher, dass sie den Inhalt, die der Benutzer muss vor dem Öffnen der app, wie z. B. ein:

- **App-Aktualisierung im Hintergrund** : die [WKApplicationRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) Task ermöglicht der app, die den Zustand im Hintergrund zu aktualisieren. Dazu gehören in der Regel eine andere Aufgabe wie das Herunterladen neuen Inhalts aus dem Internet mit einer [NSUrlSession](https://developer.apple.com/reference/foundation/nsurlsession).
- **Aktualisieren Sie die Momentaufnahme im Hintergrund** : die [WKSnapshotRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) Task ermöglicht der app, dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor das System eine Momentaufnahme darstellt, die zum Auffüllen der Dock verwendet werden.
- **Watch-Konnektivität im Hintergrund** : die [WKWatchConnectivityRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) Aufgabe wird für die app gestartet, wenn der Empfang von Hintergrunddaten aus dem gekoppelten iPhone.
- **URL-Sitzung im Hintergrund** : die [WKURLSessionRefreshBackgroundTask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) Aufgabe wird für die app gestartet, wenn eine hintergrundübertragung erfordert eine Autorisierung oder abgeschlossen ist (erfolgreich oder Fehler).

Diese Aufgaben werden in den folgenden Abschnitten ausführlich behandelt.

<a name="WKApplicationRefreshBackgroundTask" />

### <a name="wkapplicationrefreshbackgroundtask"></a>WKApplicationRefreshBackgroundTask

Die `WKApplicationRefreshBackgroundTask` ist eine allgemeine Aufgabe, die geplant werden kann, die app zu einem späteren Zeitpunkt aktiviert sein:

[![](background-tasks-images/update04.png "Eine zu einem späteren Zeitpunkt reaktiviert WKApplicationRefreshBackgroundTask")](background-tasks-images/update04.png#lightbox)

Innerhalb der Laufzeit der Aufgabe, die app werden alle Arten von lokalen Verarbeitungsmodus, z. B. Update eine Zeitachse Komplikation oder mit bestimmten erforderlichen Daten abrufen kann eine `NSUrlSession`.


<a name="WKURLSessionRefreshBackgroundTask" />

### <a name="wkurlsessionrefreshbackgroundtask"></a>WKURLSessionRefreshBackgroundTask

Das System sendet eine `WKURLSessionRefreshBackgroundTask` Wenn die Daten wurde heruntergeladen und von der Anwendung verarbeitet werden:

[![](background-tasks-images/update05.png "Die WKURLSessionRefreshBackgroundTask, wenn die Daten herunterladen abgeschlossen ist")](background-tasks-images/update05.png#lightbox)

Eine app nicht weiterhin ausgeführt wird, während Daten im Hintergrund heruntergeladen werden. Stattdessen plant die Anforderung für Daten, die die app, und er angehalten wurde, und das System behandelt das Herunterladen von Daten, die app nur reawakening, wenn der Download abgeschlossen ist.

<a name="WKSnapshotRefreshBackgroundTask" />

### <a name="wksnapshotrefreshbackgroundtask"></a>WKSnapshotRefreshBackgroundTask

In WatchOS 3 hat Apple das Dock hinzugefügt, in denen Benutzer heften Sie ihre bevorzugten apps und schnell darauf zugreifen können. Wenn der Benutzer auf der Seite auf der Apple Watch-drückt, wird ein Katalog von angehefteten app Momentaufnahmen angezeigt. Der Benutzer kann streichen Sie nach links oder rechts an die gewünschte app zu finden, und tippen Sie dann die app, um die Datei und Ersetzen Sie dabei die Momentaufnahme mit der ausgeführten app-Benutzeroberfläche zu starten.

[![](background-tasks-images/update06.png "Ersetzen die Momentaufnahme mit der ausgeführten apps-Schnittstelle")](background-tasks-images/update06.png#lightbox)

Das System erstellt in regelmäßigen Abständen Momentaufnahmen der Benutzeroberfläche der Anwendung (durch Senden einer `WKSnapshotRefreshBackgroundTask`) und verwendet Sie diese Momentaufnahmen, um dem Dock zu füllen. WatchOS erhält die app die Möglichkeit, dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor diese Momentaufnahme erstellt wird.

Momentaufnahmen sind sehr wichtig in WatchOS 3, da sie als der Vorschauversion und der Start-Bilder für die app funktionieren. Wenn der Benutzer für eine app im Dock hat, wird es auf Vollbildmodus erweitern, geben Sie im Vordergrund gestartet und ausgeführt wird, es unumgänglich ist, dass die Momentaufnahme auf dem neuesten Stand sein:

[![](background-tasks-images/update07.png "Wenn der Benutzer für eine app im Dock hat, wird es in den Vollbildmodus erweitert.")](background-tasks-images/update07.png#lightbox)

In diesem Fall gibt das System eine `WKSnapshotRefreshBackgroundTask` , damit die app (durch Aktualisierung der Daten und die Benutzeroberfläche) vorbereiten kann, bevor die Momentaufnahme erstellt wird:

[![](background-tasks-images/update08.png "Die app kann vorbereiten, indem Sie die Daten und die Benutzeroberfläche zu aktualisieren, bevor die Momentaufnahme erstellt wird")](background-tasks-images/update08.png#lightbox)

Wenn die app markiert die `WKSnapshotRefreshBackgroundTask` abgeschlossen, das System wird automatisch eine Momentaufnahme von der Benutzeroberfläche der Anwendung.

> [!IMPORTANT]
> Es ist wichtig, Planen Sie stets eine ` WKSnapshotRefreshBackgroundTask` nach dem die app verfügt über neue Daten empfangen und aktualisiert die Benutzeroberfläche oder dem Benutzer die geänderte Informationen nicht angezeigt.




Darüber hinaus, wenn der Benutzer eine Benachrichtigung über die app erhält und tippt, um die app im Vordergrund anzuzeigen, muss die Momentaufnahme auf dem neuesten Stand sein, da er als auch dem Startbildschirm fungiert:

[![](background-tasks-images/update09.png "Der Benutzer erhält eine Benachrichtigung über die app und tippt, um die app in den Vordergrund zu bringen.")](background-tasks-images/update09.png#lightbox)

Wenn es mehr als eine Stunde seit einer WatchOS-app der Benutzer interagiert hat, werden sie auf den Standardzustand zurückgeben können. Der Standardzustand kann verschiedene Dinge auf andere apps bedeuten und, basierend auf den Entwurf einer App, möglicherweise keine Standardstatus überhaupt.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask" />

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>WKWatchConnectivityRefreshBackgroundTask

In WatchOS 3, Apple Watch-Konnektivität mit der Hintergrund aktualisieren-API über das neue integriert hat `WKWatchConnectivityRefreshBackgroundTask`. Mit dieser neuen Funktion kann eine iPhone-app übermitteln neue Daten in Watch-app Entsprechung, während die WatchOS-app im Hintergrund ausgeführt wird:

[![](background-tasks-images/update10.png "Eine iPhone-app kann neue Daten für sein Pendant Watch-app bereitstellen, während die WatchOS-app im Hintergrund ausgeführt wird")](background-tasks-images/update10.png#lightbox)

Initiieren eine Komplikation mithilfe von Push übertragen, werden App-Kontext, senden eine Datei, oder Aktualisieren von Benutzerinformationen aus der iPhone-app auf die Apple Watch-app im Hintergrund aktiviert.

Wenn die Watch-app aktiviert ist, über eine `WKWatchConnectivityRefreshBackgroundTask` muss der standard-API-Methoden verwenden, um die Daten aus der iPhone-app zu empfangen.

[![](background-tasks-images/update11.png "Der Datenfluss WKWatchConnectivityRefreshBackgroundTask")](background-tasks-images/update11.png#lightbox)

1. Stellen Sie sicher, dass die Sitzung aktiviert hat.
2. Überwachen Sie die neue `HasContentPending` Eigenschaft, die so lange, wie der Wert ist `true`, die app immer noch zu verarbeitenden Daten. Als sollte vor, die app auf die Aufgabe halten, bis alle Daten verarbeitet wurden.
3. Wenn keine weiteren Daten verarbeitet werden (`HasContentPending = false`), markieren Sie die Aufgabe abgeschlossen wird, um das System wiederherzustellen. Wenn diese Änderung erschöpft der app zugewiesene Hintergrund Common Language Runtime führt ein Absturzbericht.

<a name="The-Background-API-Lifecycle" />

## <a name="the-background-api-lifecycle"></a>Die Hintergrund-API-Lebenszyklus

Alle Teile der neuen Hintergrund Aufgaben-API zusammen würde, eine typische Reihe von Interaktionen wie folgt aussehen:

[![](background-tasks-images/update12.png "Die Hintergrund-API-Lebenszyklus")](background-tasks-images/update12.png#lightbox)

1. Zunächst plant die WatchOS-app einen Hintergrund Aufgabe werden wurde aktiviert als bestimmten Zeitpunkt in der Zukunft.
2. Die app wird vom System aktiviert und eine Aufgabe gesendet.
3. Die app verarbeitet den Vorgang zum Abschließen der Arbeit erforderlich war.
4. Daher der Verarbeitung der Aufgabe, die app muss möglicherweise planen Sie weitere Hintergrundinformationen mehr Arbeit in der Zukunft abgeschlossen wird, wie das Herunterladen von Inhalt mit einem `NSUrlSession`.
5. Die app markiert die Aufgabe abgeschlossen und gibt sie an das System zurück.

<a name="Using-Resources-Responsibly" />

## <a name="using-resources-responsibly"></a>Verantwortungsbewusst mithilfe von Ressourcen

Es ist wichtig, dass es sich bei eine WatchOS-app innerhalb dieses Ökosystem verantwortungsbewusst verhält sich, indem die Beanspruchung von freigegebenen Ressourcen des Systems beschränkt.

Sehen Sie sich das folgende Szenario:

[![](background-tasks-images/update13.png "WatchOS-app beschränkt die Beanspruchung von freigegebenen Ressourcen des Systems")](background-tasks-images/update13.png#lightbox)

1. Der Benutzer startet eine WatchOS-app, um 13:00 Uhr.
2. Die app plant eine Aufgabe ein, reaktiviert, und Laden neuen Inhalte in einer Stunde um 14:00 Uhr.
3. Um 13:50 Uhr öffnet erneut der Benutzer die app Dadurch können sie die Daten und die Benutzeroberfläche zu diesem Zeitpunkt zu aktualisieren.
4. Anstatt die Task-Aktivierung der app erneut in 10 Minuten, sollte die app neu berechnen, die Aufgabe, um eine Stunde später noch Mal um 14:50 Uhr ausgeführt.

Während jede app anders ist, empfiehlt Apple, suchen Sie nach der Auslastung wie denen des oben beschriebenen Muster, um Systemressourcen zu sparen.

<a name="Implementing-Background-Tasks" />

## <a name="implementing-background-tasks"></a>Implementieren von Hintergrundaufgaben

Als Beispiel verwendet in diesem Dokument die gefälschte MonkeySoccer Sport-app, die Fußball-Ergebnisse für den Benutzer angibt. 

Sehen Sie sich das folgende typische Verwendungsszenario:

[![](background-tasks-images/update14.png "Das typische Verwendungsszenario")](background-tasks-images/update14.png#lightbox)

Bevorzugte Fußball-Team des Benutzers wird eine große Übereinstimmung aus 19:00 Uhr bis 9:00 Uhr gespielt, damit die app erwarten, dass des Benutzers die Bewertung regelmäßig überprüft werden, und es entscheidet sich für ein Aktualisierungsintervall von 30 Minuten.

1. Das Öffnen der app, und es plant eine Aufgabe für den Hintergrund aktualisieren auf 30 Minuten später noch mal. Die Hintergrund-API können nur eine Art von Hintergrund Aufgabe zu einem bestimmten Zeitpunkt ausgeführt werden.
2. Die app die Aufgabe empfängt und aktualisiert die Daten und die Benutzeroberfläche, und dann die Zeitpläne für einen anderen Hintergrund Vorgang 30 Minuten später. Es ist wichtig, dass der Entwickler speichert, um einen anderen Hintergrund Aufgabe zu planen oder die app nie neu gestartet wird, um weitere Updates zu erhalten.
3. In diesem Fall der app die Aufgabe empfängt und aktualisiert die Daten, aktualisiert die Benutzeroberfläche und plant einen anderen Hintergrund Vorgang 30 Minuten später.
4. Der gleiche Prozess wiederholt.
5. Die letzten erforderlichen Hintergrundinformationen, die Aufgabe empfangen wird und die app erneut aktualisiert die Daten und Benutzeroberfläche. Da dies das Endergebnis ist Planung nicht für eine neue Aktualisierung im Hintergrund. 

<a name="Scheduling-for-Background-Update" />

## <a name="scheduling-for-background-update"></a>Planung für den Hintergrund aktualisieren

Wenn das oben beschriebene Szenario ist, kann die MonkeySoccer-app den folgenden Code verwenden, so planen Sie für den Hintergrund aktualisieren:

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

Er erstellt ein neues `NSDate` 30 Minuten in der zukünftigen When die app gestartet wird, werden möchte, und erstellt eine `NSMutableDictionary` um die Details des angeforderten Vorgangs zu speichern. Die `ScheduleBackgroundRefresh` Methode der `SharedExtension` wird verwendet, um anzufordern, die Aufgabe geplant werden.

Das System gibt einen `NSError` Wenn es nicht so planen Sie die angeforderte Aufgabe konnte.

<a name="Processing-the-Update" />

## <a name="processing-the-update"></a>Das Update verarbeiten

Als Nächstes führen Sie eine genauere Betrachtung der 5-Minuten-Fenster, das die erforderlichen Schritte zum Aktualisieren der Bewertung angezeigt:

[![](background-tasks-images/update15.png "Die 5-Minuten-Fenster, die mit die erforderlichen Schritte zum Aktualisieren der Bewertung")](background-tasks-images/update15.png#lightbox)

1. Um 7:30:02 Uhr die app befinden, vom System aktiviert, und erhalten die Update-Hintergrund-Aufgabe. Die erste Priorität ist, um die neuesten punktstände vom Server abzurufen. Finden Sie unter [Planen einer NSUrlSession](#Scheduling-a-NSUrlSession) unten.
2. Unter 7:30:05 die app das ursprüngliche Task abgeschlossen ist, das System die app in den Ruhezustand versetzt, und zum Herunterladen der angeforderten Daten im Hintergrund fortgesetzt.
3. Wenn das System den Download abgeschlossen ist, erstellt es einen neuen Task für die app zu aktivieren, damit die heruntergeladene Daten verarbeitet werden können. Finden Sie unter [Behandlung von Hintergrundaufgaben](#Handling-Background-Tasks) und [behandeln, die Downloads abgeschlossen](#Handling-the-Download-Completing) unten. 
4. Die app speichert die aktualisierte Informationen und markiert die Aufgabe abgeschlossen. Der Entwickler möglicherweise versucht, die Elemente der Benutzeroberfläche der app zu diesem Zeitpunkt zu aktualisieren, jedoch Apple Planen einer Aufgabe Momentaufnahme empfiehlt, um diesen Prozess zu verarbeiten. Finden Sie unter [planen ein Update der Momentaufnahme](#Scheduling-a-Snapshot-Update) unten.
5. Der app empfangen die Snapshot-Aufgabe, die Benutzeroberfläche aktualisiert und markiert die Aufgabe abgeschlossen. Finden Sie unter [behandeln ein Update der Momentaufnahme](#Handling-a-Snapshot-Update) unten.

<a name="Scheduling-a-NSUrlSession" />

## <a name="scheduling-a-nsurlsession"></a>Planen eine von NSUrlSession

Der folgende Code kann verwendet werden, um das Herunterladen von die neuesten punktstände zu planen:

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

Es konfiguriert und erstellt ein neues `NSUrlSession`, verwendet dann diese Sitzung zum Erstellen einer neuen Aufgabe mit der `CreateDownloadTask` Methode. Ruft die `Resume` Methode im Download-Task zum start der Sitzungs.

<a name="Handling-Background-Tasks" />

## <a name="handling-background-tasks"></a>Behandlung von Hintergrundaufgaben

Durch Überschreiben der `HandleBackgroundTasks` Methode der `WKExtensionDelegate`, die app kann die eingehende Hintergrundaufgaben behandeln:

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

Die `HandleBackgroundTasks` Methode durchläuft alle Aufgaben, die das System die app gesendet hat (in `backgroundTasks`) Suche nach einem `WKUrlSessionRefreshBackgroundTask`. Wenn eines gefunden wird, wird er wieder an der Sitzung teilnimmt und fügt eine `NSUrlSessionDownloadDelegate` , behandeln die Downloads abgeschlossen (finden Sie unter [behandeln, das Herunterladen abgeschlossen](#Handling-the-Download-Completing) unten):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Es bleibt ein Handle für den Task aus, bis er abgeschlossen wurde, indem sie Sie einer Sammlung hinzufügen:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Die Aufgaben, die an die app gesendet müssen abgeschlossen werden, für jede Aufgabe, die derzeit nicht verarbeitet werden, erledigt markieren:

```csharp
if (urlTask != null) {
    ...
} else {
    // Ensure that all tasks are completed
    task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing" />

## <a name="handling-the-download-completing"></a>Behandeln der Download abgeschlossen

Die app MonkeySoccer verwendet die folgenden `NSUrlSessionDownloadDelegate` Delegat zum Behandeln der Download abgeschlossen und die angeforderten Daten zu verarbeiten:

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

Bei der Initialisierung, bleibt es ein Handle, sowohl die `ExtensionDelegate` und `WKRefreshBackgroundTask` , die erzeugt es. Es überschreibt die `DidFinishDownloading` Methode zum Behandeln der Download abgeschlossen. Anschließend verwendet der `CompleteTask` -Methode der der `ExtensionDelegate` , informieren die Aufgabe, die er abgeschlossen wurde, und entfernen sie aus der Auflistung der ausstehenden Aufgaben. Finden Sie unter [Behandlung von Hintergrundaufgaben](#Handling-Background-Tasks) oben.

<a name="Scheduling-a-Snapshot-Update" />

## <a name="scheduling-a-snapshot-update"></a>Planen ein Update der Momentaufnahme

Der folgende Code kann verwendet werden, um einen Momentaufnahme-Vorgang zur Aktualisierung der Benutzeroberfläche mit den neuesten Bewertungen zu planen:

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

Genau wie `ScheduleURLUpdateSession` oben genannte Methode, wird ein neuer `NSDate` für die app gestartet wird, werden möchte und erstellt eine `NSMutableDictionary` um die Details des angeforderten Vorgangs zu speichern. Die `ScheduleSnapshotRefresh` Methode der `SharedExtension` wird verwendet, um anzufordern, die Aufgabe geplant werden.

Das System gibt einen `NSError` Wenn es nicht so planen Sie die angeforderte Aufgabe konnte.

<a name="Handling-a-Snapshot-Update" />

## <a name="handling-a-snapshot-update"></a>Behandeln ein Update der Momentaufnahme

Behandelt die Momentaufnahmenaufgabe der `HandleBackgroundTasks` Methode (finden Sie unter [Behandlung von Hintergrundaufgaben](#Handling-Background-Tasks) oben) wird geändert, um wie folgt aussehen:

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

Die Methode sucht nach den Typ der Aufgabe, die verarbeitet werden. Ist dies ein `WKSnapshotRefreshBackgroundTask` greift auf die Aufgabe:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Die Methode erstellt dann die Benutzeroberfläche aktualisiert eine `NSDate` auf das System darauf hinzuweisen, wenn die Momentaufnahme ist veraltet. Erstellt eine `NSMutableDictionary` mit Benutzerinformationen aus, um die neue Momentaufnahme und markiert die Snapshot-Aufgabe abgeschlossen mit diesen Informationen beschreiben:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Darüber hinaus weist es auch die Momentaufnahmenaufgabe, dass die app nicht auf die Standardwerte (in den ersten Parameter) zurückgibt. Diese Eigenschaft sollte immer auf von Apps, die über kein Konzept für einen Standardzustand festgelegt `true`.

<a name="Working-Efficiently" />

## <a name="working-efficiently"></a>Effizient

Wie im obigen Beispiel des Fensters fünf Minuten, die die app MonkeySoccer ausgeführt hat, um seine Spielstände, zu aktualisieren, indem Sie effizient und mithilfe der neuen WatchOS 3 Aufgaben im Hintergrund, wird bei die app nur für einen Zeitraum von 15 Sekunden lang aktiv war: 

[![](background-tasks-images/update16.png "Die app war nur für einen Zeitraum von 15 Sekunden lang aktiv, weil")](background-tasks-images/update16.png#lightbox)

Dies verringert die Auswirkungen, die die app auf verfügbare Ressourcen für Apple Watch und Akkulaufzeit verfügt und außerdem ermöglicht der app, funktionieren besser mit anderen apps, die auf der Apple Watch ausgeführt wird.

<a name="How-Scheduling-Works" />

## <a name="how-scheduling-works"></a>Funktionsweise von planen

Während eine WatchOS 3-app im Vordergrund ist, wird er immer geplant, ausgeführt und können jede Art von Verarbeitung, die erforderlich sind, z. B. Aktualisieren von Daten oder die Benutzeroberfläche neu zu zeichnen. Wechselt die app in den Hintergrund, es wird in der Regel vom System angehalten, und alle laufzeitvorgänge angehalten werden. 

Während die app im Hintergrund ist, kann es das System schnell auf eine bestimmte Aufgabe ausführen verwendet werden. Daher kann das System in WatchOS 2, vorübergehend eine Hintergrund-app für Aktionen wie behandeln eine Benachrichtigung lange Anzeige oder beim Aktualisieren der app-Komplikation aktivieren. In WatchOS 3 gibt es mehrere neue Möglichkeiten, die eine app im Hintergrund ausgeführt werden kann.

Während eine app im Hintergrund ist, erzwingt das System mehrere Einschränkungen auf:

- Es erhält nur wenige Sekunden, um eine bestimmte Aufgabe abzuschließen. Das System berücksichtigt nicht nur die Menge an Zeit vergangen ist, sondern auch wie viel CPU-Leistung der app verbraucht ist, um diese Beschränkung abzuleiten.
- Jede app, die die Grenzwerte überschreitet, wird mit der folgenden Fehlercodes beendet werden:
    - **CPU** -0xc51bad01
    - **Zeit** -0xc51bad02
- Das System bedeuten verschiedene Grenzwerte, die basierend auf den Typ der Hintergrundaufgabe, die sie die app ausführen gestellt hat. Z. B. `WKApplicationRefreshBackgroundTask` und `WKURLSessionRefreshBackgroundTask` Aufgaben erhalten etwas längere Laufzeiten gegenüber anderen Typen von Hintergrundaufgaben.

<a name="Complications-and-App-Updates" />

### <a name="complications-and-app-updates"></a>Schwierigkeiten und App-Updates

Zusätzlich zu den neuen Hintergrund-Aufgaben, die Apple watchos 3 hinzugefügt hat, haben eine WatchOS-app-Komplikationen beeinflusst wie und wann die app Hintergrundupdates empfängt.

Komplikationen sind kleine visuelle Elemente, die nützliche Informationen auf einen Blick zu bieten. Je nach dem Zifferblatt Ihrer Apple Watch ausgewählt haben hat der Benutzer die Möglichkeit, eine Zifferblatt Ihrer Apple Watch mit ein oder mehrere Komplikation anzupassen, die durch eine Watch-app in WatchOS 3 übergeben werden kann.

Wenn der Benutzer eine der schwierigkeiten bei der Art der app auf ihre Zifferblatt Ihrer Apple Watch enthält, er erhält die app die folgenden aktualisiert Vorteile:

- Es bewirkt, dass das System in einem bereit zum Start der app zu Zustand, in dem sie die app im Hintergrund zu starten versucht, sieht es immer im Arbeitsspeicher und gibt es zusätzliche Zeit, um zu aktualisieren.
- Komplikationen garantiert mindestens 50 Push Aktualisierungen pro Tag.

Der Entwickler empfiehlt es sich immer, erstellen Sie überzeugende Komplikationen für ihre apps, um den Benutzer dazu verleiten, in den oben genannten Gründen auf ihre Zifferblatt Ihrer Apple Watch hinzufügen.

In WatchOS 2 wurden die Schwierigkeiten der einfachste Weg, dass eine app über Common Language Runtime im Hintergrund empfangen. In WatchOS 3, wird eine Komplikation app weiterhin erhalten Sie mehrere Updates pro Stunde sichergestellt werden, es können jedoch `WKExtensions` Weitere Runtime zum Aktualisieren der Komplikationen anfordern.

Sehen Sie sich den folgenden Code verwendet, um die Schwierigkeit besteht aus den verbundenen iPhone-app zu aktualisieren:

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

Er verwendet die `RemainingComplicationUserInfoTransfers` Eigenschaft der `WCSession` angezeigt, wie viele hoher Priorität, die app überträgt die für den Tag und klicken Sie dann die Aktion ausführt, die Grundlage dieser Zahl verlassen hat. Wenn die app Übertragungen erschöpft ist beginnt, kann für kleinere Aktualisierungen zu senden, warten und nur Informationen senden, wenn eine signifikante Änderung vorliegt.

<a name="Scheduling-and-Dock" />

### <a name="scheduling-and-the-dock"></a>Planung und dem Dock

In WatchOS 3 hat Apple das Dock hinzugefügt, in denen Benutzer heften Sie ihre bevorzugten apps und schnell darauf zugreifen können. Wenn der Benutzer auf der Seite auf der Apple Watch-drückt, wird ein Katalog von angehefteten app Momentaufnahmen angezeigt. Der Benutzer kann streichen Sie nach links oder rechts an die gewünschte app zu finden, und tippen Sie dann die app, um die Datei und Ersetzen Sie dabei die Momentaufnahme mit der ausgeführten app-Benutzeroberfläche zu starten.

[![](background-tasks-images/dock01.png "Das Andocken")](background-tasks-images/dock01.png#lightbox)

Das System wird in regelmäßigen Abständen erstellt Momentaufnahmen die Benutzeroberfläche der app und verwenden diese Momentaufnahmen, um die Dokumentation zu füllen. WatchOS erhält die app die Möglichkeit, dessen Inhalt und die Benutzeroberfläche zu aktualisieren, bevor diese Momentaufnahme erstellt wird.

Apps, die an die Dockingstation angeheftet wurden, können Folgendes erwarten:

- Sie erhalten eine pro Stunde aktualisiert. Dies schließt sowohl eine App aktualisieren Aufgabe und eine Momentaufnahme-Aufgabe.
- Alle apps im Dock wird das Budget Update verteilt. Also, desto weniger apps, die der Benutzer angeheftet hat, die mehr potenzielle Updates, die jede app erhält.
- Die app wird im Arbeitsspeicher beibehalten werden, damit die app schnell wird fortgesetzt, wenn aus dem Dock ausgewählt.

Die letzte app den Benutzer ausgeführten betrachtet die _zuletzt verwendet_ app und den letzten Slot im Dock belegt. Dort gibt es Benutzer dauerhaft auf dem Dock anheften können. Die zuletzt verwendet werden wie jede andere bevorzugte app den Benutzer an die Dockingstation bereits angeheftet wurde behandelt.

> [!IMPORTANT]
> Apps, die nur auf dem Startbildschirm hinzugefügt wurden keine regulären Planung erhält. Zum Empfangen von regulären planen und Hintergrund aktualisiert, eine app _müssen_ Dock hinzugefügt werden.

Wie weiter oben in diesem Dokument erwähnt, sind Momentaufnahmen sehr wichtig in WatchOS 3, da sie als der Vorschauversion und der Start-Bilder für die app funktionieren. Wenn der Benutzer für eine app im Dock hat, wird es auf Vollbildmodus erweitern, geben Sie im Vordergrund gestartet und ausgeführt wird, es unumgänglich ist, dass die Momentaufnahme auf dem neuesten Stand sein.

Es gibt möglicherweise Zeiten, wenn das System entscheidet, die Benutzeroberfläches der app eine neue Momentaufnahme erforderlich. In diesen Situationen wird die Snapshot-Anforderung für die app Laufzeit Budget nicht gezählt. Im folgenden wird die Anforderung eines Momentaufnahme ausgelöst:

- Ein Update des Komplikation-Zeitachse.
- Benutzerinteraktion mit der app-Benachrichtigung.
- Wechseln in den Hintergrund-Zustand in den Vordergrund gestellt.
- Nach einer Stunde nach der im Hintergrund Zustand befinden kann also die app auf den Standardstatus zurück.
- Bei WatchOS zuerst gestartet wird.

<a name="Best-Practices" />

## <a name="best-practices"></a>Bewährte Methoden 

Apple empfiehlt die folgenden bewährten Methoden bei der Arbeit mit Hintergrundaufgaben:

- Planen Sie so oft wie nötig ab, die app aktualisiert werden. Jedes Mal, wenn die app ausgeführt wird sollte die zukünftigen Anforderungen neu zu bewerten und passen Sie diesen Zeitplan nach Bedarf.
- Wenn das System eine Hintergrundaufgabe zu aktualisieren sendet, und kein Update für die app muss, verzögern Sie die Arbeit, bis ein Update tatsächlich erforderlich sind.
- Berücksichtigen Sie alle Common Language Runtime-Möglichkeiten für eine app zur Verfügung:
    - Vorder- und Dock-Aktivierung.
    - Benachrichtigungen.
    - Komplikation aktualisiert.
    - Hintergrundaktualisierungen.
- Verwendung `ScheduleBackgroundRefresh` für allgemeine Hintergrund-Laufzeit, wie z. B.:
    - Abrufen des Systems für die Informationen an.
    - Planen Sie zukünftige `NSURLSessions` Hintergrund Daten. 
    - Bekannte Zeit geht.
    - Auslösen von Updates der Komplikation.

<a name="Snapshot-Best-Practices" />

## <a name="snapshot-best-practices"></a>Snapshot bewährten Methoden

Bei der Arbeit mit Updates von Momentaufnahmen ist Apple die folgenden Vorschläge:

- Ungültig werden Sie Momentaufnahmen nur im Bedarfsfall, z. B. wenn eine erhebliche inhaltsänderung vorhanden ist.
- Vermeiden Sie häufige Momentaufnahme invalidierung. Z. B. eine Timer-app darf nicht die Momentaufnahme pro Sekunde aktualisiert, es sollte nur vorgenommen werden, wenn der Timer abgelaufen ist.

<a name="App-Data-Flow" />

## <a name="app-data-flow"></a>App-Datenfluss

Apple empfehlen Folgendes für die Arbeit mit den Datenfluss:

[![](background-tasks-images/update17.png "Flussdiagramm für die App-Daten")](background-tasks-images/update17.png#lightbox)

Ein externes Ereignis (z. B. Watch-Konnektivität) aktiviert die app. Dies erzwingt, dass die app das Datenmodell zu aktualisieren (die den aktuellen Status der apps steht). Daher der Änderung Datenmodell die app muss zum Aktualisieren der schwierigkeiten bei der Art, eine neue Momentaufnahme anfordern, starten Sie ggf. einen Hintergrund `NSURLSession` zum Abrufen von mehr Daten, und Planen Sie weitere Hintergrund aktualisiert.

<a name="The-App-Lifecycle" />

## <a name="the-app-lifecycle"></a>Der App-Lebenszyklus

Aufgrund der Andocken und die Möglichkeit, Lieblings-apps, denkt, dass Apple anheften, die Benutzer zwischen wesentlich mehr apps zu verschieben, viel häufiger, dann war dies der Fall bei WatchOS 2. Daher sollte die app bereit sein, behandeln Sie diese Änderung und schnell zwischen den Zuständen Vordergrund- und bewegen.

Apple hat die folgenden Vorschläge:

- Stellen Sie sicher, dass die app Hintergrundtasks so bald wie möglich nach der Eingabe der Vordergrund-Aktivierung abgeschlossen wird.
- Um alle Foreground-Aufgaben fertig zu stellen Sie sicher, dass vor dem Hintergrund durch den Aufruf eingeben `NSProcessInfo.PerformExpiringActivity`.
- Beim Testen einer app in der WatchOS-Simulator wird keines der Aufgabe Budgets erzwungen werden, damit eine app aktualisieren kann, wie erforderlich, um ein Feature ordnungsgemäß zu testen.
- Testen Sie stets auf echter Apple Watch-Hardware, um sicherzustellen, dass die app nicht hinter die Budgets vor der Veröffentlichung an iTunes Connect ausgeführt wird.
- Apple empfiehlt, halten die Apple Watch-auf dem Ladegerät beim Testen und Debuggen.
- Stellen Sie sicher, dass sowohl kalte starten und Fortsetzen einer app gründlich getestet werden.
- Stellen Sie sicher, dass alle app-Vorgänge abgeschlossen werden.
- Variieren Sie die Anzahl der apps, die im Dock, sowohl die am besten und schlechtesten testen festgesetzt sind Anwendungsszenarien.

<a name="Summary" />

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen behandelt, die Apple vorgenommen hat, WatchOS und wie sie verwendet werden können, um eine Watch-app auf dem neuesten Stand zu halten. Erstens behandelt alle der neuen Hintergrund Aufgabe Apple in WatchOS 3 hinzugefügt wurde. Klicken Sie dann konnte damit die Hintergrund-API-Lebenszyklus und wie Sie Hintergrundaufgaben in einer Xamarin-WatchOS-app zu implementieren. Schließlich behandelt wie zeitplanung funktioniert, und haben einige bewährten Methoden.



## <a name="related-links"></a>Verwandte Links

- [iOS 10-Beispiele](https://developer.xamarin.com/samples/ios/iOS10/)
