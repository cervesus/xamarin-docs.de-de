---
title: watchos-Hintergrundaufgaben in xamarin
description: In diesem Dokument wird beschrieben, wie Hintergrundaufgaben mit watchos in xamarin verwendet werden. dabei werden die Arten von Hintergrundaufgaben, die Verwendung von Ressourcen, die Implementierung von Hintergrundaufgaben, die Planung, bewährte Methoden und vieles mehr beschrieben.
ms.prod: xamarin
ms.assetid: 2049C430-7566-45F8-9E3D-1446F484981E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/13/2017
ms.openlocfilehash: 84953ce2ec09cc757b5719991e499dc24b708cae
ms.sourcegitcommit: 952db1983c0bc373844c5fbe9d185e04a87d8fb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/23/2020
ms.locfileid: "86996109"
---
# <a name="watchos-background-tasks-in-xamarin"></a>watchos-Hintergrundaufgaben in xamarin

Mit watchos 3 gibt es drei Hauptmethoden, mit denen eine Watch-app Ihre Informationen auf dem neuesten standhalten kann:

- Mithilfe einer der verschiedenen neuen Hintergrundaufgaben.
- Eine seiner Komplikationen auf der Seite "Watch" (mehr Zeit zum Aktualisieren).
- Festlegen, dass der Benutzer die APP an das neue Dock anheften soll (wo er im Arbeitsspeicher gespeichert und häufig aktualisiert wird).

## <a name="keeping-an-app-up-to-date"></a>Beibehalten einer APP auf dem neuesten Stand

Vor der Erörterung aller Möglichkeiten, mit denen Entwickler die Daten und die Benutzeroberfläche einer watchos-App auf dem neuesten standhalten und aktualisieren können, wird in diesem Abschnitt ein typischer Satz von Verwendungs Mustern behandelt, und es wird erläutert, wie ein Benutzer über den Tag und die Aktivität, die aktuell ausgeführt wird, auf das iPhone und seine Apple Watch wechselt (z. b. das Fahrverhalten).

Sehen Sie sich das folgende Beispiel an.

[![Art und Weise, wie ein Benutzer im Laufe des Tages zwischen dem iPhone und dessen Apple Watch wechseln kann](background-tasks-images/update00.png)](background-tasks-images/update00.png#lightbox)

1. Am Morgen, während ich auf einen Kaffee wartete, browst der Benutzer die aktuellen Nachrichten einige Minuten lang auf dem iPhone.
2. Bevor Sie das Kaffeegeschäft verlässt, überprüfen Sie das Wetter schnell mit einer Komplikation auf die Uhr.
3. Vor dem Mittagessen verwenden Sie die Maps-APP auf dem iPhone, um ein Restaurant in der Nähe zu finden und eine Reservierung zur Erfüllung eines Clients zu buchen.
4. Wenn Sie zum Restaurant Reisen, erhalten Sie eine Benachrichtigung über Ihre Apple Watch. mit einem kurzen Blick wissen Sie, dass Ihr mittagstermin später ausgeführt wird.
5. Am Abend verwenden Sie die Maps-APP auf dem iPhone, um den Datenverkehr vor dem Start zu überprüfen.
6. Auf der Startseite erhalten Sie eine IMESS-Benachrichtigung zu Ihrer Apple Watch in der Sie aufgefordert werden, Milch zu übernehmen, und Sie verwenden die Quick Reply-Funktion, um die Antwort "OK" zu senden.

Aufgrund der "kurzen Übersicht" (weniger als drei Sekunden), wie ein Benutzer eine Apple Watch-App verwenden möchte, ist es in der Regel nicht ausreichend, dass die APP gewünschte Informationen abrufen und die Benutzeroberfläche aktualisieren kann, bevor Sie für den Benutzer angezeigt wird.

Mithilfe der neuen APIs, die Apple in watchos 3 enthalten hat, kann die APP eine _Hintergrund Aktualisierung_ planen und die gewünschten Informationen bereitstellen, bevor Sie vom Benutzer angefordert werden. Betrachten Sie das Beispiel für die oben beschriebene Wetter Komplikation:

[![Ein Beispiel für die Wetter Komplikation](background-tasks-images/update01.png)](background-tasks-images/update01.png#lightbox)

1. Die APP plant die Aktivierung durch das System zu einem bestimmten Zeitpunkt.
2. Die App Ruft die Informationen ab, die zum Generieren des Updates erforderlich sind.
3. Die APP generiert die Benutzeroberfläche erneut, um die neuen Daten widerzuspiegeln.
4. Wenn sich der Benutzer mit der Komplikation der APP vertraut, verfügt er über aktuelle Informationen, ohne dass der Benutzer auf die Aktualisierung warten muss.

Wie oben gezeigt, aktiviert das watchos-System die App mithilfe einer oder mehrerer Tasks, von denen ein sehr begrenzter Pool verfügbar ist:

[![Das watchos-System aktiviert die App mithilfe von mindestens einer Aufgabe.](background-tasks-images/update02.png)](background-tasks-images/update02.png#lightbox)

Apple empfiehlt, den größten Teil dieser Aufgabe zu nutzen (da es sich um eine solche beschränkte Ressource für die APP handelt), indem Sie darauf warten kann, bis die APP den Prozess der Aktualisierung selbst beendet hat.

Das System stellt diese Aufgaben durch Aufrufen der neuen-Methode des Delegaten bereit `HandleBackgroundTasks` `WKExtensionDelegate` . Beispiel:

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

Wenn die APP die angegebene Aufgabe abgeschlossen hat, wird Sie an das System zurückgegeben, indem Sie als abgeschlossen markiert wird:

[![Die Aufgabe wird zum System zurückgegeben, indem Sie als abgeschlossen markiert wird.](background-tasks-images/update03.png)](background-tasks-images/update03.png#lightbox)

<a name="New-Background-Tasks"></a>

## <a name="new-background-tasks"></a>Neue Hintergrundaufgaben

watchos 3 führt mehrere Hintergrundaufgaben ein, die eine APP zum Aktualisieren Ihrer Informationen verwenden kann, um sicherzustellen, dass Sie die Inhalte der Benutzer benötigt, bevor Sie die APP öffnen, wie z. b.:

- Aktualisierung der **Hintergrund-App** : die Aufgabe " [wkapplicationrefresh backgroundtask](https://developer.apple.com/reference/watchkit/wkapplicationrefreshbackgroundtask) " ermöglicht der APP, ihren Status im Hintergrund zu aktualisieren. Dies schließt in der Regel eine andere Aufgabe ein, z. b. das Herunterladen von neuem Inhalt aus dem Internet mithilfe von [nsurlsession](https://developer.apple.com/reference/foundation/nsurlsession)
- **Aktualisieren von Moment** Aufnahmen von Momentaufnahmen: die [wksnapshotrefreshbackgroundtask](https://developer.apple.com/reference/watchkit/wksnapshotrefreshbackgroundtask) -Aufgabe ermöglicht der APP, den Inhalt und die Benutzeroberfläche zu aktualisieren, bevor das System eine Momentaufnahme erstellt, die zum Auffüllen des Andock Werts verwendet wird.
- **Konnektivität bei der Hintergrundüberwachung** : die Aufgabe " [wkwatchconnectivityaktubackgroundtask](https://developer.apple.com/reference/watchkit/wkwatchconnectivityrefreshbackgroundtask) " wird für die APP gestartet, wenn Sie Hintergrunddaten von dem gekoppelten iPhone empfängt.
- **URL-URL-Sitzung** : die [wkurlsessionerfrischendes backgroundtask](https://developer.apple.com/reference/watchkit/wkurlsessionrefreshbackgroundtask) -Aufgabe wird für die APP gestartet, wenn eine Hintergrund Übertragung eine Autorisierung erfordert oder abgeschlossen ist (erfolgreich oder fehlerhaft).

Diese Aufgaben werden in den folgenden Abschnitten ausführlich beschrieben.

<a name="WKApplicationRefreshBackgroundTask"></a>

### <a name="wkapplicationrefreshbackgroundtask"></a>Wkapplicationerfrischendes backgroundtask

`WKApplicationRefreshBackgroundTask`Bei handelt es sich um eine generische Aufgabe, bei der geplant werden kann, dass die APP zu einem späteren Zeitpunkt aktiviert wird:

[![Ein wkapplicationerfrischendes backgroundtask wird an einem späteren Zeitpunkt reaktiviert.](background-tasks-images/update04.png)](background-tasks-images/update04.png#lightbox)

Innerhalb der Laufzeit der Aufgabe kann die APP jede Art lokaler Verarbeitung durchführen, z. b. eine komplikationszeitachse aktualisieren oder einige erforderliche Daten mit einem abrufen `NSUrlSession` .

<a name="WKURLSessionRefreshBackgroundTask"></a>

### <a name="wkurlsessionrefreshbackgroundtask"></a>Wkurlsessionerfrischendes backgroundtask

Das System sendet eine `WKURLSessionRefreshBackgroundTask` , wenn die Daten den Download abgeschlossen haben und für die Verarbeitung durch die APP bereit sind:

[![Wkurlsessionerfrischendes backgroundtask, wenn der Download der Daten abgeschlossen ist](background-tasks-images/update05.png)](background-tasks-images/update05.png#lightbox)

Eine APP wird nicht ausgeführt, während die Daten im Hintergrund heruntergeladen werden. Stattdessen plant die APP die Anforderung für Daten und wird dann angehalten, und das System übernimmt das Herunterladen der Daten, wobei die app nur wiederholt wird, wenn der Download beendet ist.

<a name="WKSnapshotRefreshBackgroundTask"></a>

### <a name="wksnapshotrefreshbackgroundtask"></a>Wksnapshotrefreshbackgroundtask

In watchos 3 hat Apple das Dock hinzugefügt, mit dem Benutzer Ihre bevorzugten apps anheften und schnell darauf zugreifen können. Wenn der Benutzer die Seiten Schaltfläche auf dem Apple Watch drückt, wird ein Katalog mit angehefteten App-Momentaufnahmen angezeigt. Der Benutzer kann nach links oder rechts schwenken, um die gewünschte APP zu suchen, und dann auf die APP tippen, um die Momentaufnahme durch die Benutzeroberfläche der laufenden app zu ersetzen.

[![Ersetzen der Momentaufnahme durch die Schnittstelle für laufende Apps](background-tasks-images/update06.png)](background-tasks-images/update06.png#lightbox)

Das System erstellt in regelmäßigen Abständen Momentaufnahmen der App-Benutzeroberfläche (durch Senden eines `WKSnapshotRefreshBackgroundTask` ) und verwendet diese Momentaufnahmen, um das Dock aufzufüllen. watchos bietet der APP die Möglichkeit, den Inhalt und die Benutzeroberfläche zu aktualisieren, bevor die Momentaufnahme erstellt wird.

Momentaufnahmen sind in watchos 3 sehr wichtig, da Sie sowohl als Vorschau-als auch als Start Abbild für die APP fungieren. Wenn sich der Benutzer in einer APP im Dock befindet, wird er auf den Vollbildmodus erweitert. Geben Sie den Vordergrund ein, und starten Sie die Ausführung. Daher ist es zwingend erforderlich, dass die Momentaufnahme auf dem neuesten Stand ist:

[![Wenn der Benutzer eine APP im Dock eingibt, wird er auf den Vollbildmodus erweitert.](background-tasks-images/update07.png)](background-tasks-images/update07.png#lightbox)

Auch hier gibt das System einen `WKSnapshotRefreshBackgroundTask` aus, sodass die APP (durch Aktualisieren der Daten und der Benutzeroberfläche) vor dem Erstellen der Momentaufnahme vorbereitet werden kann:

[![Die APP kann vorbereitet werden, indem die Daten und die Benutzeroberfläche aktualisiert werden, bevor die Momentaufnahme erstellt wird.](background-tasks-images/update08.png)](background-tasks-images/update08.png#lightbox)

Wenn die APP den Vorgang als `WKSnapshotRefreshBackgroundTask` abgeschlossen markiert, erstellt das System automatisch eine Momentaufnahme der App-Benutzeroberfläche.

> [!IMPORTANT]
> Es ist wichtig, dass Sie immer eine planen, `WKSnapshotRefreshBackgroundTask` nachdem die APP neue Daten empfangen und die Benutzeroberfläche aktualisiert hat, oder wenn der Benutzer die geänderten Informationen nicht sieht.

Wenn der Benutzer außerdem eine Benachrichtigung von der APP erhält und ihn tippt, um die app in den Vordergrund zu bringen, muss die Momentaufnahme auf dem neuesten Stand sein, da Sie auch als Startbildschirm fungiert:

[![Der Benutzer erhält eine Benachrichtigung von der APP und tippt darauf, um die app in den Vordergrund zu bringen.](background-tasks-images/update09.png)](background-tasks-images/update09.png#lightbox)

Wenn es mehr als eine Stunde vergangen ist, da der Benutzer mit einer watchos-App interagiert hat, kann er in seinen Standardzustand zurückkehren. Der Standardstatus kann unterschiedliche Dinge für verschiedene apps bedeuten. basierend auf dem Entwurf einer APP hat Sie möglicherweise keinen Standardzustand.

<!--TODO - Possibly link to Apple's Designing Great Apple Watch Experiences video or add our own version here...-->

<a name="WKWatchConnectivityRefreshBackgroundTask"></a>

### <a name="wkwatchconnectivityrefreshbackgroundtask"></a>Wkwatchconnectivityerfrischendes backgroundtask

In watchos 3 hat Apple die Überwachung von Verbindungen mit der API für die Hintergrund Aktualisierung über den neuen integriert `WKWatchConnectivityRefreshBackgroundTask` . Mit dieser neuen Funktion kann eine iPhone-App neue Daten an die APP für die Watch-App liefern, während die watchos-App im Hintergrund ausgeführt wird:

[![Eine iPhone-App kann für Ihre Watch-App-Entsprechung neue Daten bereitstellt, während die watchos-App im Hintergrund ausgeführt wird.](background-tasks-images/update10.png)](background-tasks-images/update10.png#lightbox)

Wenn Sie einen komplikations-Push, App-Kontext initiieren, eine Datei senden oder Benutzerinformationen von der iPhone-App aktualisieren, wird die Apple Watch-App im Hintergrund aktiviert.

Wenn die Watch-App über eine reaktiviert wird `WKWatchConnectivityRefreshBackgroundTask` , muss Sie die API-Standardmethoden verwenden, um die Daten von der iPhone-App zu empfangen.

[![Der wkwatchconnectivityerfrischendes backgroundtask-Datenfluss](background-tasks-images/update11.png)](background-tasks-images/update11.png#lightbox)

1. Stellen Sie sicher, dass die Sitzung aktiviert wurde.
2. Überwachen Sie die neue `HasContentPending` Eigenschaft, solange der Wert ist `true` , die APP weiterhin Daten für die Verarbeitung enthält. Wie zuvor sollte die APP die Aufgabe anhalten, bis die Verarbeitung aller Daten abgeschlossen ist.
3. Wenn keine weiteren zu verarbeitenden Daten vorhanden sind ( `HasContentPending = false` ), markieren Sie die Aufgabe abgeschlossen, um Sie an das System zurückzugeben. Wenn Sie dies nicht tun, wird die zugewiesene Hintergrund Laufzeit der APP, die zu einem Absturz Bericht führt, erschöpft.

<a name="The-Background-API-Lifecycle"></a>

## <a name="the-background-api-lifecycle"></a>Der Hintergrund-API-Lebenszyklus

Wenn Sie alle Teile der neuen API für Hintergrundaufgaben zusammen platzieren, würde ein typischer Satz an Interaktionen wie folgt aussehen:

[![Der Hintergrund-API-Lebenszyklus](background-tasks-images/update12.png)](background-tasks-images/update12.png#lightbox)

1. Zuerst plant die watchos-APP einen Hintergrund Task, der als ein anderer Zeitpunkt in der Zukunft nicht mehr angezeigt wird.
2. Die APP wird vom System reaktiviert und eine Aufgabe gesendet.
3. Die APP verarbeitet die Aufgabe, um die erforderliche Arbeit abzuschließen.
4. Aufgrund der Verarbeitung der Aufgabe muss die APP möglicherweise weitere Hintergrundaufgaben planen, um in Zukunft mehr Arbeit zu erledigen, z. b. das Herunterladen von mehr Inhalten mithilfe von `NSUrlSession` .
5. Die APP markiert die abgeschlossene Aufgabe und gibt Sie an das System zurück.

<a name="Using-Resources-Responsibly"></a>

## <a name="using-resources-responsibly"></a>Verantwortungsbewusstes Verwenden von Ressourcen

Es ist wichtig, dass sich eine watchos-APP innerhalb dieses Ökosystems verantwortungsbewusst verhält, indem Sie den Ausgleich der freigegebenen Ressourcen des Systems einschränkt.

Sehen Sie sich das folgende Szenario an:

[![Eine watchos-App schränkt deren Ableitung auf die freigegebenen Ressourcen des Systems ein.](background-tasks-images/update13.png)](background-tasks-images/update13.png#lightbox)

1. Der Benutzer öffnet eine watchos-App um 1:00 Uhr.
2. Die APP plant die Aktivierung und den Download neuer Inhalte innerhalb einer Stunde um 2:00 Uhr.
3. Um 1:50 Uhr wird der Benutzer die APP erneut öffnen, um die Daten und die Benutzeroberfläche zu diesem Zeitpunkt aktualisieren zu können.
4. Anstatt die Aufgabe in 10 Minuten erneut zu aktivieren, sollte die APP die Aufgabe neu planen, damit Sie eine Stunde später um 2:50 Uhr ausgeführt wird.

Obwohl jede APP anders ist, empfiehlt Apple, die Verwendungs Muster zu finden, wie oben gezeigt, um Systemressourcen zu sparen.

<a name="Implementing-Background-Tasks"></a>

## <a name="implementing-background-tasks"></a>Implementieren von Hintergrundaufgaben

Zum Beispiel wird in diesem Dokument die gefälschte monkeyfußball-Sport-App verwendet, die dem Benutzer Fußball Bewertungen meldet.

Sehen Sie sich das folgende typische Verwendungs Szenario an:

[![Das typische Verwendungs Szenario](background-tasks-images/update14.png)](background-tasks-images/update14.png#lightbox)

Das bevorzugte Fußballteam des Benutzers spielt eine große Entsprechung zwischen 7:00 Uhr und 9:00 Uhr, sodass die APP erwartet, dass der Benutzer das Ergebnis regelmäßig überprüft und ein Update Intervall von 30 Minuten entscheidet.

1. Der Benutzer öffnet die APP und plant 30 Minuten später eine Aufgabe für das Hintergrund Update. Mit der Background-API kann jeweils nur eine Art von Hintergrundaufgabe ausgeführt werden.
2. Die App empfängt die Aufgabe und aktualisiert die Daten und die Benutzeroberfläche und plant dann 30 Minuten später eine weitere Hintergrundaufgabe. Es ist wichtig, dass der Entwickler daran erinnert, eine andere Hintergrundaufgabe zu planen, oder die APP wird nie erneut aktiviert, um weitere Updates zu erhalten.
3. Auch hier empfängt die APP die Aufgabe und aktualisiert Ihre Daten, aktualisiert die Benutzeroberfläche und plant eine weitere Hintergrundaufgabe 30 Minuten später.
4. Derselbe Prozess wird wiederholt.
5. Die letzte Hintergrundaufgabe wird empfangen, und die APP aktualisiert die Daten und die Benutzeroberfläche erneut. Da dies das abschließende Ergebnis ist, wird keine neue Hintergrund Aktualisierung geplant.

<a name="Scheduling-for-Background-Update"></a>

## <a name="scheduling-for-background-update"></a>Planen der Aktualisierung im Hintergrund

Angesichts des obigen Szenarios kann die monkeysoccer-App den folgenden Code verwenden, um ein Hintergrund Update zu planen:

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

`NSDate`In der Zukunft werden neue 30 Minuten erstellt, wenn die APP erneut aktiviert werden soll, und eine erstellt `NSMutableDictionary` , um die Details der angeforderten Aufgabe zu speichern. Die- `ScheduleBackgroundRefresh` Methode von `SharedExtension` wird verwendet, um anzufordern, dass der Task geplant wird.

Das System gibt einen zurück, `NSError` Wenn die angeforderte Aufgabe nicht geplant werden konnte.

<a name="Processing-the-Update"></a>

## <a name="processing-the-update"></a>Die Aktualisierung wird verarbeitet.

Sehen Sie sich als nächstes das 5-Minuten-Fenster an, in dem die zum Aktualisieren der Bewertung erforderlichen Schritte angezeigt werden:

[![Das 5-Minuten-Fenster mit den Schritten, die zum Aktualisieren der Bewertung erforderlich sind.](background-tasks-images/update15.png)](background-tasks-images/update15.png#lightbox)

1. Um 7:30:02 Uhr, wird die app durch das System und den Task "Hintergrund aktualisieren" geöffnet. Die erste Priorität besteht darin, die neuesten Ergebnisse vom Server zu erhalten. Weitere Informationen finden Sie unten unter [Planen einer nsurlsession](#Scheduling-a-NSUrlSession) .
2. Bei 7:30:05 wird die ursprüngliche Aufgabe von der APP abgeschlossen, die APP wird in den Standbymodus versetzt, und die angeforderten Daten werden weiterhin im Hintergrund heruntergeladen.
3. Wenn das System den Download abgeschlossen hat, wird eine neue Aufgabe erstellt, um die APP zu aktivieren, damit die heruntergeladenen Informationen verarbeitet werden können. Weitere Informationen finden Sie unter [Behandeln von Hintergrundaufgaben](#Handling-Background-Tasks) und [Verarbeiten des unten abgeschlossenen Downloads](#Handling-the-Download-Completing) .
4. Die APP speichert die aktualisierten Informationen und markiert die abgeschlossene Aufgabe. Der Entwickler ist möglicherweise versucht, die Benutzeroberfläche der APP zu diesem Zeitpunkt zu aktualisieren, aber Apple schlägt vor, eine Momentaufnahme Aufgabe zu planen, um diesen Prozess zu verarbeiten. Weitere Informationen finden Sie unten unter [Planen einer Momentaufnahme Aktualisierung](#Scheduling-a-Snapshot-Update) .
5. Die App empfängt die Momentaufnahme Aufgabe, aktualisiert Ihre Benutzeroberfläche und markiert die abgeschlossene Aufgabe. Weitere Informationen finden Sie weiter unten unter [Behandeln von Moment](#Handling-a-Snapshot-Update) Aufnahmen.

<a name="Scheduling-a-NSUrlSession"></a>

## <a name="scheduling-a-nsurlsession"></a>Planen einer nsurlsession

Der folgende Code kann verwendet werden, um das Herunterladen der neuesten Bewertungen zu planen:

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

Er konfiguriert und erstellt einen neuen und `NSUrlSession` verwendet dann diese Sitzung, um mithilfe der-Methode eine neue Download Aufgabe zu erstellen `CreateDownloadTask` . Die- `Resume` Methode der Download Aufgabe wird aufgerufen, um die Sitzung zu starten.

<a name="Handling-Background-Tasks"></a>

## <a name="handling-background-tasks"></a>Behandeln von Hintergrundaufgaben

Durch Überschreiben der- `HandleBackgroundTasks` Methode von `WKExtensionDelegate` kann die APP die eingehenden Hintergrundaufgaben verarbeiten:

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

Die- `HandleBackgroundTasks` Methode durchläuft alle Aufgaben, die das System an die APP gesendet hat (in), die `backgroundTasks` nach einem suchen `WKUrlSessionRefreshBackgroundTask` . Wenn ein solcher gefunden wird, wird die Sitzung erneut verknüpft und ein `NSUrlSessionDownloadDelegate` zum Verarbeiten des heruntergeladenen Downloads angefügt (Weitere Informationen finden Sie unter [Verarbeiten des heruntergeladenen Downloads](#Handling-the-Download-Completing) ):

```csharp
// Create new session
var backgroundSession = NSUrlSession.FromConfiguration (configuration, new BackgroundSessionDelegate (this, task), null);
```

Sie behält die Aufgabe bei, bis Sie abgeschlossen wurde, indem Sie Sie zu einer Auflistung hinzugefügt hat:

```csharp
public List<WKRefreshBackgroundTask> PendingTasks { get; set; } = new List<WKRefreshBackgroundTask> ();
...

// Keep track of all pending tasks
PendingTasks.Add (task);
```

Alle Aufgaben, die an die APP gesendet werden, müssen abgeschlossen werden, damit alle Aufgaben, die derzeit nicht verarbeitet werden, abgeschlossen werden:

```csharp
if (urlTask != null) {
  ...
} else {
  // Ensure that all tasks are completed
  task.SetTaskCompleted ();
}
```

<a name="Handling-the-Download-Completing"></a>

## <a name="handling-the-download-completing"></a>Der Download wird abgeschlossen

Die monkeysoccer-App verwendet den folgenden Delegaten `NSUrlSessionDownloadDelegate` , um den Download abzuschließen und die angeforderten Daten zu verarbeiten:

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

Wenn die Initialisierung initialisiert ist, wird ein Handle sowohl für das `ExtensionDelegate` als auch für das `WKRefreshBackgroundTask` , das es erzeugt hat. Er überschreibt die- `DidFinishDownloading` Methode, um den Download abzuschließen. Dann verwendet die- `CompleteTask` Methode von `ExtensionDelegate` , um die Aufgabe zu informieren, dass Sie abgeschlossen wurde, und Sie aus der Auflistung der ausstehenden Tasks zu entfernen. Weitere Informationen finden Sie oben unter [Behandeln von Hintergrundaufgaben](#Handling-Background-Tasks)

<a name="Scheduling-a-Snapshot-Update"></a>

## <a name="scheduling-a-snapshot-update"></a>Planen einer Momentaufnahme Aktualisierung

Der folgende Code kann verwendet werden, um eine Momentaufnahme Aufgabe zu planen, um die Benutzeroberfläche mit den neuesten Bewertungen zu aktualisieren:

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

Wie `ScheduleURLUpdateSession` oben beschrieben, wird eine neue erstellt, `NSDate` Wenn die App aktiviert werden soll, und eine erstellt, `NSMutableDictionary` um die Details der angeforderten Aufgabe zu speichern. Die- `ScheduleSnapshotRefresh` Methode von `SharedExtension` wird verwendet, um anzufordern, dass der Task geplant wird.

Das System gibt einen zurück, `NSError` Wenn die angeforderte Aufgabe nicht geplant werden konnte.

<a name="Handling-a-Snapshot-Update"></a>

## <a name="handling-a-snapshot-update"></a>Behandeln eines Momentaufnahme Updates

Zum Verarbeiten der Momentaufnahme Aufgabe wird die- `HandleBackgroundTasks` Methode (siehe [Behandeln von Hintergrundaufgaben](#Handling-Background-Tasks) oben) so geändert, dass Sie wie folgt aussieht:

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

Die-Methode testet, welche Art von Task verarbeitet wird. Wenn er ein ist `WKSnapshotRefreshBackgroundTask` , erhält er Zugriff auf die Aufgabe:

```csharp
var snapshotTask = task as WKSnapshotRefreshBackgroundTask;
```

Die-Methode aktualisiert die Benutzeroberfläche und erstellt dann eine `NSDate` , um das System zu informieren, wenn die Momentaufnahme veraltet ist. Er erstellt eine `NSMutableDictionary` mit Benutzerinformationen, um die neue Momentaufnahme zu beschreiben, und markiert die abgeschlossene Momentaufnahme Aufgabe mit den folgenden Informationen:

```csharp
// Mark task complete
snapshotTask.SetTaskCompleted (false, expirationDate, userInfo);
```

Außerdem weist Sie die Momentaufnahme Aufgabe an, dass die APP nicht in den Standardzustand zurückkehrt (im ersten Parameter). Für apps, die kein Konzept eines Standard Zustands haben, sollte diese Eigenschaft immer auf festgelegt werden `true` .

<a name="Working-Efficiently"></a>

## <a name="working-efficiently"></a>Effizientes Arbeiten

Wie im obigen Beispiel des fünfminütigen Fensters gezeigt, das die monkeysoccer-App zum Aktualisieren der Ergebnisse benötigt hat, indem Sie effizient und mithilfe der neuen watchos 3-Hintergrundaufgaben arbeitet, war die app nur für insgesamt 15 Sekunden aktiv:

[![Die APP war insgesamt 15 Sekunden lang aktiv.](background-tasks-images/update16.png)](background-tasks-images/update16.png#lightbox)

Dadurch werden die Auswirkungen der APP auf verfügbare Apple Watch Ressourcen und die Akku Lebensdauer verringert, und die APP kann auch besser mit anderen apps arbeiten, die auf der Uhr ausgeführt werden.

<a name="How-Scheduling-Works"></a>

## <a name="how-scheduling-works"></a>Funktionsweise der Planung

Während eine watchos 3-APP im Vordergrund steht, ist Sie immer für die Ausführung geplant und kann jede beliebige Art von Verarbeitung durchführen, wie z. b. das Aktualisieren von Daten oder das Neuzeichnen der Benutzeroberfläche. Wenn die app in den Hintergrund wechselt, wird Sie normalerweise vom System angehalten, und alle Lauf Zeit Vorgänge werden angehalten.

Obwohl sich die APP im Hintergrund befindet, kann Sie vom System für eine schnelle Durchführung einer bestimmten Aufgabe eingesetzt werden. Daher kann das System in watchos 2 vorübergehend eine Hintergrund-App reaktivieren, um Aufgaben wie das Behandeln einer langen Anzeige Benachrichtigung oder das Aktualisieren der Komplikationen der APP auszuführen. In watchos 3 gibt es mehrere neue Möglichkeiten, wie eine APP im Hintergrund ausgeführt werden kann.

Während eine APP im Hintergrund ist, erzwingt das System mehrere Beschränkungen:

- Es sind nur wenige Sekunden für die Ausführung einer bestimmten Aufgabe angegeben. Das System berücksichtigt nicht nur die Zeitspanne, die vergangen ist, sondern auch die Menge an CPU-Leistung, die die APP zum Ableiten dieses Limits beansprucht.
- Jede APP, die ihre Grenzwerte überschreitet, wird mit den folgenden Fehlercodes abgebrochen:
  - **CPU** -0xc51bad01
  - **Zeit** -0xc51bad02
- Das System setzt basierend auf dem Typ der Hintergrundaufgabe, die die app ausführen muss, unterschiedliche Grenzwerte ein. Beispielsweise `WKApplicationRefreshBackgroundTask` werden-und- `WKURLSessionRefreshBackgroundTask` Tasks etwas längere Laufzeiten als andere Typen von Hintergrundaufgaben erhalten.

<a name="Complications-and-App-Updates"></a>

### <a name="complications-and-app-updates"></a>Komplikationen und APP-Updates

Zusätzlich zu den neuen Hintergrundaufgaben, die Apple watchos 3 hinzugefügt hat, können sich die Komplikationen einer watchos-App auf die Art und Weise auswirken, in der die APP Hintergrund Aktualisierungen empfängt.

Komplikationen sind kleine visuelle Elemente, die hilfreiche Informationen auf einen Blick bereitstellen. Je nach ausgewähltem Überwachungs Gesicht hat der Benutzer die Möglichkeit, ein Überwachungs Gesicht mit einer oder mehreren Komplikationen anzupassen, die von einer Watch-app in watchos 3 bereitgestellt werden können.

Wenn der Benutzer eine der Komplikationen der APP auf dem Überwachungs Gesicht enthält, bietet er der APP die folgenden aktualisierten Vorteile:

- Das System bewirkt, dass die app in einem betriebsbereiten Zustand ist, in dem Sie versucht, die APP im Hintergrund zu starten, Sie im Arbeitsspeicher speichert und zusätzliche Zeit zum Aktualisieren erhält.
- Komplikationen sind mindestens 50 pushupdates pro Tag garantiert.

Der Entwickler sollte sich immer darauf konzentrieren, überzeugende Komplikationen für Ihre apps zu erstellen, um den Benutzer in die oben aufgeführten Gründe zu bringen, Sie in seine Überwachungs Fläche einzufügen.

In watchos 2 waren Komplikationen die primäre Art und Weise, wie eine APP im Hintergrund eine Laufzeit empfangen hat. In watchos 3 wird immer noch sichergestellt, dass eine komplikations-App mehrere Updates pro Stunde empfängt. es kann jedoch verwendet werden, `WKExtensions` um mehr Laufzeit zum Aktualisieren der Komplikationen anzufordern.

Sehen Sie sich den folgenden Code an, der verwendet wird, um die Komplikation aus der verbundenen iPhone-App zu aktualisieren:

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

Er verwendet die- `RemainingComplicationUserInfoTransfers` Eigenschaft des `WCSession` , um zu sehen, wie viele Übertragungen mit hoher Priorität die APP für den Tag übrig hat, und führt dann basierend auf dieser Zahl Maßnahmen aus. Wenn die Übertragung der APP niedrig ist, kann es beim Senden von geringfügigen Updates und beim Durchführen einer signifikanten Änderung nur Informationen senden.

<a name="Scheduling-and-Dock"></a>

### <a name="scheduling-and-the-dock"></a>Planen und Andocken

In watchos 3 hat Apple das Dock hinzugefügt, mit dem Benutzer Ihre bevorzugten apps anheften und schnell darauf zugreifen können. Wenn der Benutzer die Seiten Schaltfläche auf dem Apple Watch drückt, wird ein Katalog mit angehefteten App-Momentaufnahmen angezeigt. Der Benutzer kann nach links oder rechts schwenken, um die gewünschte APP zu suchen, und dann auf die APP tippen, um die Momentaufnahme durch die Benutzeroberfläche der laufenden app zu ersetzen.

[![Das Dock](background-tasks-images/dock01.png)](background-tasks-images/dock01.png#lightbox)

Das System erstellt in regelmäßigen Abständen Momentaufnahmen der App-Benutzeroberfläche und verwendet diese Momentaufnahmen, um die Dokumente aufzufüllen. watchos bietet der APP die Möglichkeit, den Inhalt und die Benutzeroberfläche zu aktualisieren, bevor die Momentaufnahme erstellt wird.

Apps, die an das Dock angeheftet wurden, können Folgendes erwarten:

- Sie erhalten mindestens ein Update pro Stunde. Dies schließt sowohl eine APP-Aktualisierungs Aufgabe als auch eine Momentaufnahme Aufgabe ein.
- Das Update Budget wird zwischen allen apps im Dock verteilt. Je weniger apps der Benutzer fixiert hat, desto mehr potenzielle Updates werden jede APP erhalten.
- Die APP wird im Arbeitsspeicher beibehalten, sodass die app schnell fortgesetzt wird, wenn Sie vom Dock ausgewählt wird.

Die letzte APP, die der Benutzer ausgeführt hat, wird als zuletzt _verwendete_ App angesehen und belegt den letzten Slot im Dock. Von dort aus kann der Benutzer die Datei dauerhaft an das Dock anheften. Die zuletzt verwendete wird wie jede andere bevorzugte App behandelt, die der Benutzer bereits an das Dock angeheftet hat.

> [!IMPORTANT]
> Apps, die nur dem Startbildschirm hinzugefügt wurden, erhalten keine reguläre Zeitplanung. Um reguläre Zeitplanung und Hintergrund Aktualisierungen zu erhalten, _muss_ dem Dock eine app hinzugefügt werden.

Wie bereits weiter oben in diesem Dokument erwähnt, sind Momentaufnahmen in watchos 3 sehr wichtig, da Sie sowohl als Vorschau-als auch als Start Abbild für die APP fungieren. Wenn der Benutzer eine APP im Dock eingibt, wird er auf den Vollbildmodus erweitert. Geben Sie den Vordergrund ein, und starten Sie die Ausführung. Daher ist es zwingend erforderlich, dass die Momentaufnahme auf dem neuesten Stand ist.

Es kann vorkommen, dass das System entscheidet, dass es eine neue Momentaufnahme der Benutzeroberfläche der APP benötigt. In diesen Fällen wird die Momentaufnahme Anforderung nicht für das Lauf Zeitbudget der APP gezählt. Der folgende Vorgang löst eine System Momentaufnahme-Anforderung aus:

- Ein Komplikation-Zeitachsen Update.
- Benutzerinteraktion mit einer APP-Benachrichtigung.
- Wechsel von der Vordergrund-in den Hintergrund Zustand.
- Nachdem eine Stunde im Hintergrund Zustand ist, kann die app in den Standardzustand zurückkehren.
- Beim ersten Start von watchos.

<a name="Best-Practices"></a>

## <a name="best-practices"></a>Bewährte Methoden

Apple schlägt bei der Arbeit mit Hintergrundaufgaben die folgenden bewährten Methoden vor:

- Planen Sie so oft, wie die APP aktualisiert werden muss. Jedes Mal, wenn die app ausgeführt wird, sollte Sie die zukünftigen Anforderungen neu auswerten und diesen Zeitplan nach Bedarf anpassen.
- Wenn das System eine Hintergrund Aktualisierungs Aufgabe sendet und die APP kein Update erfordert, verschieben Sie die Arbeit so lange, bis ein Update tatsächlich erforderlich ist.
- Denken Sie an alle Lauf Zeit Chancen einer APP:
  - Andocken und Vordergrund Aktivierung.
  - Anbot.
  - Komplikationsupdates.
  - Hintergrund Aktualisierungen.
- Verwenden Sie `ScheduleBackgroundRefresh` für die allgemeine Hintergrund Laufzeit, z. b.:
  - Abrufen des Systems auf Informationen.
  - Planen `NSURLSessions` Sie die Zukunft, um Hintergrunddaten anzufordern.
  - Bekannte Zeit Übergänge.
  - Auslösen von komplikationsupdates.

<a name="Snapshot-Best-Practices"></a>

## <a name="snapshot-best-practices"></a>Bewährte Methoden für Momentaufnahmen

Beim Arbeiten mit momentaufnahmenaktualisierungen führt Apple die folgenden Vorschläge aus:

- Ungültig machen von Momentaufnahmen nur bei Bedarf, z. b. bei signifikanter Inhalts Änderung.
- Vermeiden Sie eine hoch Häufigkeits momentaufnahmenvalidierung. Beispielsweise sollte eine Timer-APP die Momentaufnahme nicht jede Sekunde aktualisieren, sondern nur, wenn der Timer beendet wurde.

<a name="App-Data-Flow"></a>

## <a name="app-data-flow"></a>App-Datenfluss

Apple empfiehlt Folgendes für die Arbeit mit dem Datenfluss:

[![App-Datenfluss Diagramm](background-tasks-images/update17.png)](background-tasks-images/update17.png#lightbox)

Ein externes Ereignis (z. b. "Verbindung überwachen") aktiviert die app. Dadurch wird die APP gezwungen, das Datenmodell zu aktualisieren (das den aktuellen App-Zustand darstellt). Aufgrund der Änderung des Datenmodells muss die APP Ihre Komplikationen aktualisieren, eine neue Momentaufnahme anfordern, ggf. einen Hintergrund starten, `NSURLSession` um weitere Daten abzurufen und weitere Hintergrund Aktualisierungen zu planen.

<a name="The-App-Lifecycle"></a>

## <a name="the-app-lifecycle"></a>Der APP-Lebenszyklus

Aufgrund der Andock Funktion und der Möglichkeit, bevorzugte apps an Sie anzuheften, geht Apple davon aus, dass sich die Benutzer sehr viel häufiger mit dem Wechsel zu watchos 2 bewegen werden. Folglich sollte die APP bereit sein, diese Änderung zu verarbeiten und schnell zwischen Vordergrund-und Hintergrund Zuständen zu wechseln.

Apple hat folgende Vorschläge:

- Stellen Sie sicher, dass die APP alle Hintergrundaufgaben so schnell wie möglich beendet, wenn Sie die Vordergrund Aktivierung aktivieren.
- Stellen Sie sicher, dass Sie die gesamte Vordergrund Arbeit abschließen, bevor Sie den Hintergrund aufrufen `NSProcessInfo.PerformExpiringActivity` .
- Beim Testen einer APP im watchos-Simulator wird keines der Aufgaben Budgets erzwungen, sodass eine APP so viel wie erforderlich aktualisiert werden kann, um ein Feature ordnungsgemäß zu testen.
- Testen Sie immer auf der Real Apple Watch-Hardware, um sicherzustellen, dass die APP vor dem Veröffentlichen in iTunes Connect nicht über das Budget hinausläuft.
- Apple schlägt vor, die Apple Watch beim Testen und Debuggen auf das Lade Gerät zu behalten.
- Stellen Sie sicher, dass sowohl der Kaltstart als auch das Fortsetzen einer APP gründlich getestet werden.
- Überprüfen Sie, ob alle App-Aufgaben abgeschlossen sind.
- Variieren Sie die Anzahl der apps, die im Dock angeheftet sind, um sowohl die besten als auch die schlechtesten Szenarien zu testen.

<a name="Summary"></a>

## <a name="summary"></a>Zusammenfassung

In diesem Artikel wurden die Verbesserungen erläutert, die Apple an watchos vorgenommen hat, und wie Sie verwendet werden können, um eine Watch-App auf dem neuesten Stand zu halten. Zuerst wurde die neue Hintergrundaufgabe behandelt, die Apple in watchos 3 hinzugefügt hat. Anschließend wurde der API-Lebenszyklus des Hintergrunds behandelt, und es wird erläutert, wie Hintergrundaufgaben in einer xamarin watchos-App implementiert werden. Schließlich haben Sie erfahren, wie die Planung funktioniert und einige bewährte Methoden erhalten.

## <a name="related-links"></a>Verwandte Links

- [IOS 10-Beispiele](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
