---
title: iOS Hintergrundverarbeitung mit Aufgaben
description: Dieses Dokument beschreibt, wie Sie Aufgaben im Hintergrund zu verwenden, um lang andauernden Aufgaben auszuführen, nachdem eine Anwendung im Hintergrund platziert wird.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 48859afe2c988c1afe67d5c4350cef734f879fdf
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50120995"
---
# <a name="ios-backgrounding-with-tasks"></a>iOS Hintergrundverarbeitung mit Aufgaben

Die einfachste Möglichkeit zum Ausführen der hintergrundverarbeitung in iOS ist Ihre backgrounding Anforderungen in Aufgaben unterteilen und die Aufgaben im Hintergrund ausgeführt. Aufgaben eine strenge zeitliche Begrenzung unterliegen, und erhalten in der Regel ca. 600 Sekunden (10 Minuten) der Verarbeitungszeit, nachdem eine Anwendung in den Hintergrund, unter iOS 6 verschoben wurde und weniger als 10 Minuten unter iOS 7 und höher.

Hintergrundaufgaben können in drei Kategorien unterteilt werden:

1.  **Aufgaben von Hintergrund-sichere** - wird aufgerufen an einer beliebigen Stelle in der Anwendung, in dem Sie eine Aufgabe haben, Sie nicht möchten unterbrochenen sollten die Anwendung eingeben, der den Hintergrund.
1.  **DidEnterBackground Aufgaben** - wird aufgerufen, während die `DidEnterBackground` Application Lifecycle-Methode zur Unterstützung der Bereinigung und den Status speichern.
1.  **(IOS 7 und höher)-Übertragungen im Hintergrund** – ein spezieller Typ von Hintergrundtasks verwendet wird, um die netzwerkübertragungen unter iOS 7 ausführen. Im Gegensatz zu regulären Aufgaben-Übertragungen im Hintergrund ein vorher festgelegten Zeitraums keine.


Hintergrund-sicher und `DidEnterBackground` Aufgaben sicher ist, verwenden für iOS 6 und iOS 7, mit einigen kleineren unterschieden werden. Wir untersuchen nun, diese beiden Arten von Aufgaben ausführlich.

## <a name="creating-background-safe-tasks"></a>Erstellen von Aufgaben von Hintergrund-sicher

Einige Anwendungen enthalten Aufgaben, die von iOS unterbrochen werden dürfen nicht Zustand die Anwendung geändert werden sollte. Eine Möglichkeit, die verhindern, dass diese Aufgaben unterbrochen ist, registrieren sie mit iOS als lang andauernden Aufgaben. Sie können verwenden, dieses Muster an einer beliebigen Stelle in Ihrer Anwendung, in denen nicht, dass eine Aufgabe, die er unterbrochen wird der Benutzer Put der app in den Hintergrund sollten. Ein hervorragender Kandidat für dieses Muster wäre Aufgaben wie das Senden von Informationen zur produktregistrierung des neuen Benutzers an den Server und das Überprüfen der Anmeldeinformationen.

Der folgende Codeausschnitt veranschaulicht, registrieren eine Aufgabe, die im Hintergrund ausgeführt:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Eine Aufgabe mit einem eindeutigen Bezeichner, der während der Registrierung-Paaren `taskID`, und umschließt ihn dann in den Abgleich `BeginBackgroundTask` und `EndBackgroundTask` aufrufen. Um den Bezeichner zu generieren, stellen wir einen Aufruf der `BeginBackgroundTask` Methode für die `UIApplication` Objekt aus, und starten Sie den lang andauernden Task, in der Regel in einem neuen Thread. Wenn die Aufgabe abgeschlossen ist, rufen wir `EndBackgroundTask` und übergeben Sie den gleichen Bezeichner. Dies ist wichtig, da iOS die Anwendung beendet wird Wenn eine `BeginBackgroundTask` Aufruf verfügt nicht über ein entsprechendes `EndBackgroundTask`.

> [!IMPORTANT]
> Hintergrund-sichere Vorgänge können auf den Hauptthread oder einem Hintergrundthread, je nach die Anforderungen der Anwendung ausführen.


## <a name="performing-tasks-during-didenterbackground"></a>Ausführen von Aufgaben während der DidEnterBackground

Zusätzlich zum einen lang andauernden Task Hintergrund-sicher ist kann Registrierung verwendet werden, um die Aufgaben zu starten, wie eine Anwendung im Hintergrund weiterzuentwickeln. iOS bietet eine Ereignismethode in der *AppDelegate* Klasse mit dem Namen `DidEnterBackground` , die verwendet werden kann, um den Anwendungszustand speichern, speichern Benutzerdaten und vertraulichen Inhalt zu verschlüsseln, bevor eine Anwendung in den Hintergrund wechselt. Eine Anwendung hat ungefähr fünf Sekunden von dieser Methode zurückgeben, oder er abgebrochen. Aus diesem Grund können Cleanuptasks auszuführen, die mehr als fünf Sekunden dauern können aufgerufen werden, von innerhalb der `DidEnterBackground` Methode. Diese Aufgaben müssen auf einem separaten Thread aufgerufen werden.

Der Prozess ist nahezu identisch mit der Registrierung eines lang andauernden Tasks. Der folgende Codeausschnitt veranschaulicht dies in Aktion:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Wir beginnen, indem das Überschreiben der `DidEnterBackground` -Methode in der die `AppDelegate`, bei dem wir unsere Aufgabe über registrieren `BeginBackgroundTask` wie im vorherigen Beispiel. Als Nächstes einen neuen Thread erzeugen und unsere lang andauernden Task ausführen. Beachten Sie, dass die `EndBackgroundTask` wird jetzt aufgerufen von innerhalb der lang andauernden Task, da die `DidEnterBackground` Methode wird bereits zurückgegeben hat.

> [!IMPORTANT]
> iOS verwendet eine [watchdog-Mechanismus](http://developer.apple.com/library/ios/qa/qa1693/_index.html) , stellen Sie sicher, dass Benutzeroberfläches einer Anwendung reaktionsfähig bleibt. Eine Anwendung, die zu viel Zeit in verbringt `DidEnterBackground` reagieren nicht mehr in der Benutzeroberfläche. Aufgaben im Hintergrund ausgeführt. daher kann `DidEnterBackground` rechtzeitig, die Benutzeroberfläche reaktionsfähig halten, und verhindert, dass des Watchdogs beenden die Anwendung zurückgegeben.


## <a name="handling-background-task-time-limits"></a>Behandlung von Hintergrund Task-Zeitlimits

iOS platziert strenge Einschränkungen auf, wie lange eine Hintergrundaufgabe ausgeführt werden kann, und ob die `EndBackgroundTask` Aufruf erfolgt, nicht innerhalb der vorgesehenen Zeit, die Anwendung wird beendet. Durch Aufzeichnen der die verbleibende Zeit hintergrundverarbeitung und Ablauf-Handler bei Bedarf verwenden, können wir vermeiden, iOS, die die Anwendung beenden.

### <a name="accessing-background-time-remaining"></a>Zugreifen auf die verbleibende Zeit der Hintergrund

Wenn eine Anwendung mit registrierten Tasks in den Hintergrund verschoben wurde, erhalten die registrierten Tasks ungefähr 600 Sekunden ausgeführt. Wir sehen, wie viel Zeit die Aufgabe abgeschlossen wird, verwenden Sie die statische `BackgroundTimeRemaining` Eigenschaft der `UIApplication` Klasse. Der folgende Code wird die Zeit in Sekunden, uns zu senden, die unsere Hintergrundaufgabe verlassen hat:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Vermeiden die App beenden Ablauf-Handler

Zusätzlich zum Erteilen von Zugriff auf die `BackgroundTimeRemaining` Eigenschaft iOS bietet eine ordnungsgemäße Möglichkeit zum Verarbeiten von Hintergrund-Ablaufzeitpunkt über eine **Ablauf Handler**. Dies ist eine optionale Block des Codes, die ausgeführt werden wird, wenn die für einen Vorgang vorgesehene Dauer ist in Kürze abläuft. Ruft der Code im Ereignishandler Ablauf `EndBackgroundTask` auf und übergibt die Task-ID, der angibt, dass die app auch verhält wird und verhindert, dass iOS Beenden der app, auch wenn die Aufgabe Zeit ausgeführt wird. `EndBackgroundTask` muss innerhalb der Ablauf-Handler sowie während der normalen Ausführung aufgerufen werden. 

Der Ablauf-Handler wird als eine anonyme Funktion, die mit einem Lambda-Ausdruck ausgedrückt, wie unten gezeigt:

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.TimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

Während des Ablaufs Handler nicht für die Ausführung des Codes erforderlich sind, sollten Sie immer einen Handler für Ablauf mit eine Hintergrundaufgabe verwenden.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Hintergrundaufgaben in iOS 7 und höher

Die größte Änderung in iOS 7 in Bezug auf die Aufgaben im Hintergrund ist nicht an, wie die Aufgaben implementiert werden, aber wenn sie ausgeführt.

Denken Sie daran, dass vor iOS 7, eine Aufgabe, die im Hintergrund ausgeführte mussten 600 Sekunden abgeschlossen. Ein Grund für diesen Grenzwert ist, dass eine Aufgabe, die im Hintergrund ausgeführte das Gerät aktiv für die Dauer des Vorgangs beibehalten möchten:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Diagramm für die Aufgabe, halten die app aktiv vor iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

iOS 7 Verarbeitung im Hintergrund ist für längere Akkulaufzeiten optimiert. Hintergrundverarbeitung in iOS 7 opportunistische wird: statt jedoch ständig das Gerät aktiv, Aufgaben berücksichtigt werden soll, wenn das Gerät, die in den Standbymodus, und führen Sie stattdessen deren Verarbeitung in kleinen Blöcken auf, bei Aktivierung des Geräts zum Behandeln von Telefonanrufen, Benachrichtigungen, eingehende e-Mails und andere wechselt Häufige Unterbrechungen. Das folgende Diagramm bietet einen Einblick in die wie eine Aufgabe beschädigt werden kann um:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Diagramm für die Aufgabe unterbrochen wird, in Blöcke nach der iOS 7")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Da die Aufgabe, die zur Laufzeit nicht mehr fortlaufenden ist, müssen die Aufgaben, die netzwerkübertragungen Ausführen anders in iOS 7 behandelt werden. Entwicklern wird empfohlen, verwenden Sie die `NSURlSession` -API, um die netzwerkübertragungen zu behandeln. Im nächste Abschnitt bietet eine Übersicht über Übertragungen im Hintergrund.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Übertragungen im Hintergrund

Das Rückgrat von Übertragungen im Hintergrund in iOS 7 ist der neue `NSURLSession` API. `NSURLSession` Dank des Tasks zu erstellen:

1.  Übertragen von Inhalt über das Netzwerk und Gerät Unterbrechungen.
1.  Hoch- und herunterladen große Dateien ( *Hintergrundübertragungsdienst* ).


Werfen Sie einen genaueren Blick auf die Funktionsweise an.

### <a name="nsurlsession-api"></a>Von NSURLSession API

 `NSURLSession` ist eine leistungsstarke API zum Übertragen von Inhalten über das Netzwerk an. Es bietet eine Reihe von Tools, die Übertragung von Daten über netzwerkunterbrechungen und Änderungen in den Anwendungsstatus behandelt wird.

Die `NSURLSession` -API erstellt eine oder mehrere Sitzungen bereit, die wiederum die Aufgaben aus, um Blöcke von verwandten Daten über das Netzwerk shuttle erzeugen. Aufgaben werden asynchron zum Übertragen von Daten schnell und zuverlässig ausgeführt. Da `NSURLSession` ist asynchron, jede Sitzung erfordert einen Block in Abschluss können Sie die System- und wissen, wann eine Übertragung abgeschlossen ist.

Um ein Netzwerk übertragen, die für iOS 7-vor und nach der iOS 7 gültig ist, überprüfen Sie, ob ein `NSURLSession` steht zum Einreihen in die Warteschlange übertragen, und Verwenden von regulären Hintergrundaufgabe für die Übertragung verwenden, ist dies nicht:

```csharp
if ([NSURLSession class]) {
  // Create a background session and enqueue transfers
}
else {
  // Start a background task and transfer directly
  // Do NOT make calls to update the UI here!
}
```

> [!IMPORTANT]
> Vermeiden Sie Aufrufe an die Benutzeroberfläche im Hintergrund in iOS 6-kompatiblen Code zu aktualisieren, iOS 6 Hintergrundupdates für die Benutzeroberfläche nicht unterstützt, und es wird die Anwendung beendet.


Die `NSURLSession` -API umfasst einen umfangreichen Satz von Funktionen, behandelt die Authentifizierung und Verwalten von fehlerhaften Übertragungen clientseitige - aber keine serverseitige - Fehler gemeldet. Es trägt das zur Überbrückung, die die Unterbrechungen im Task, eingeführt in iOS 7 während der Laufzeit, und bietet auch Unterstützung für die Übertragung von großer Dateien schnell und zuverlässig. Im nächste Abschnitt untersucht diese zweite Funktion.

### <a name="background-transfer-service"></a>Übertragungsdiensts im Hintergrund

Vor iOS 7 war die hoch- oder Herunterladen von Dateien im Hintergrund nicht zuverlässig. Aufgaben im Hintergrund erhalten eine begrenzte Zeit ausgeführt, aber der Zeitaufwand für das Übertragen einer Datei mit dem Netzwerk und die Größe der Datei variiert. In iOS 7 verwenden wir eine `NSURLSession` erfolgreich hochladen und herunterladen große Dateien. Bestimmte `NSURLSession` Sitzungstyp, das Netzwerk übertragen großer Dateien im Hintergrund behandelt wird als bezeichnet die *Hintergrundübertragungsdienst*.

Datenübertragungen, die mit dem Hintergrundübertragungsdienst initiiert, die vom Betriebssystem verwaltet werden und bieten APIs, um Authentifizierung und Fehler zu behandeln. Da die Übertragung nicht durch ein beliebiges Zeitlimit gebunden sind, können sie verwendet werden, hochladen oder Herunterladen große Dateien, Inhalt in den Hintergrund und vieles mehr automatisch aktualisiert. Finden Sie in der [Exemplarische Vorgehensweise zum Übertragen von Hintergrund](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) ausführliche Anleitungen zum Implementieren des Diensts.

Abrufen im Hintergrund oder Remotebenachrichtigungen können Anwendungen, die Aktualisierung von Inhalt im Hintergrund wird häufig dem Hintergrundübertragungsdienst zugeordnet. In den nächsten beiden Abschnitten führen wir das Konzept der registrieren ganze Anwendungen unter iOS 6 und iOS 7 im Hintergrund ausgeführt.

