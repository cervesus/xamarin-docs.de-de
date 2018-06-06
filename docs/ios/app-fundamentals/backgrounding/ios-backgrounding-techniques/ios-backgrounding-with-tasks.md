---
title: iOS Backgrounding mit Aufgaben
description: Dieses Dokument beschreibt, wie Hintergrundaufgaben lang andauernden Aufgaben ausführen, nachdem eine Anwendung im Hintergrund angeordnet ist.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: a95ca128bc6de7b2adc75511a581f5d2779d9c06
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784354"
---
# <a name="ios-backgrounding-with-tasks"></a>iOS Backgrounding mit Aufgaben

Die einfachste Möglichkeit zum Ausführen von IOS-backgrounding ist backgrounding Erfordernissen in Aufgaben aufteilen und die Aufgaben im Hintergrund ausgeführt. Aufgaben sind unter einen flexiblen Grenzwert für die Zeit, und erhalten Sie in der Regel ungefähr 600 Sekunden (10 Minuten) Verarbeitungszeit, nachdem eine Anwendung im Hintergrund auf Ios6 verschoben wurde, und weniger als 10 Minuten auf iOS 7 und höher.

Hintergrundaufgaben können in drei Kategorien unterteilt werden:

1.  **Hintergrund threadsichere Vorgänge** - wird aufgerufen, an einer beliebigen Stelle in der die Anwendung, in dem Sie eine Aufgabe haben, Sie möchten schließlich nicht unterbrochenen sollten die Anwendung eingeben, den Hintergrund.
1.  **DidEnterBackground Aufgaben** - wird aufgerufen, während die `DidEnterBackground` Application Lifecycle-Methode bei der Bereinigung und Status speichern.
1.  **Im Hintergrund Übertragungen (iOS 7 und höher)** -eine besondere Art von Hintergrundtask verwendet wird, um netzwerkübertragungen für iOS 7 ausführen. Im Gegensatz zu regulären Aufgaben Übertragungen im Hintergrund eine vorher festgelegte Zeitlimit keine.


Hintergrund threadsichere und `DidEnterBackground` Aufgaben sind sicher auf iOS 6 und iOS 7, mit einigen kleineren unterschieden verwendet. Wir untersuchen diese beiden Arten von Aufgaben genauer.

## <a name="creating-background-safe-tasks"></a>Erstellen von Aufgaben im Hintergrund threadsichere

Einige Anwendungen enthalten Aufgaben, die von iOS darf nicht unterbrochen werden Zustand die Anwendung geändert werden sollte. Eine Möglichkeit, die verhindern, dass diese Vorgänge unterbrochen wird, besteht darin sie mit iOS als lang andauernden Aufgaben zu registrieren. Sie können dieses Muster an einer beliebigen Stelle in der Anwendung nicht werden soll, dass eine Aufgabe, die er unterbrochen wird Benutzer bereitstellen, die app in den Hintergrund sollten. Prädestiniert für dieses Muster wäre Aufgaben wie z. B. Registrierungsinformationen für einen neuen Benutzer an den Server gesendet, oder überprüfen die Anmeldeinformationen an.

Der folgende Codeausschnitt veranschaulicht, registrieren einen Task im Hintergrund ausgeführt:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Während der Registrierung-Paare für eine Aufgabe mit einem eindeutigen Bezeichner `taskID`, und umschließt ihn dann in den Abgleich `BeginBackgroundTask` und `EndBackgroundTask` aufrufen. Um den Bezeichner zu generieren, stellen wir einen Aufruf der `BeginBackgroundTask` Methode für die `UIApplication` -Objekt, und starten Sie den lang dauernde Aufgabe in der Regel auf einen neuen Thread. Wenn die Aufgabe abgeschlossen ist, nennen wir `EndBackgroundTask` und im gleichen Bezeichner übergeben. Dies ist wichtig, da iOS die Anwendung beendet, wenn eine `BeginBackgroundTask` Aufruf verfügt nicht über einen übereinstimmenden `EndBackgroundTask`.

> [!IMPORTANT]
> Hintergrund threadsichere Vorgänge können auf der Haupt-Thread oder einem Hintergrundthread, je nach den Anforderungen der Anwendung ausführen.


## <a name="performing-tasks-during-didenterbackground"></a>Ausführen von Aufgaben während DidEnterBackground

Zusätzlich zu den vorgenommen einer lang dauernde Aufgabe Hintergrund-sicher, kann Registrierung verwendet werden, um Aufgaben wie eine Anwendung im Hintergrund versetzt wird, wird starten. iOS bietet eine Ereignismethode in der *AppDelegate* Klasse mit dem Namen `DidEnterBackground` , die verwendet werden kann, Anwendungszustand speichern, Speichern von Benutzerdaten und vertraulichem Inhalt zu verschlüsseln, bevor eine Anwendung den Hintergrund eintritt. Eine Anwendung hat ungefähr fünf Sekunden, die von dieser Methode zurückgeben, oder er abgebrochen. Aus diesem Grund können Cleanuptasks auszuführen, die länger als fünf Sekunden in Anspruch nehmen können aufgerufen werden, von innerhalb der `DidEnterBackground` Methode. Diese Aufgaben müssen auf einem separaten Thread aufgerufen werden.

Der Prozess ist nahezu identisch mit der Registrierung einer lang dauernde Aufgabe. Der folgende Codeausschnitt veranschaulicht dies in Aktion:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Beginnen wir damit, durch Überschreiben der `DidEnterBackground` Methode in der `AppDelegate`, in der wir unsere Aufgabe über registrieren `BeginBackgroundTask` wie im vorherigen Beispiel. Als Nächstes erstellen einen neuen Thread und unsere zeitintensive Aufgabe ausführen. Beachten Sie, dass die `EndBackgroundTask` jetzt-Aufruf aus innerhalb der lang dauernde Aufgabe, da die `DidEnterBackground` Methode wird bereits zurückgegeben hat.

> [!IMPORTANT]
> iOS verwendet eine [Watchdog-Mechanismus](http://developer.apple.com/library/ios/qa/qa1693/_index.html) , stellen Sie sicher, dass Benutzeroberfläches einer Anwendung reaktionsfähig bleibt. Eine Anwendung, die zu viel Zeit in verbringt `DidEnterBackground` werden in der Benutzeroberfläche nicht mehr reagiert. Ermöglicht das Informationssammlung Aufgaben, die im Hintergrund ausgeführt `DidEnterBackground` in kürzester Zeit, die Benutzeroberfläche reaktionsfähig zu halten und verhindert, dass der Watchdog beendet die Anwendung zurückgegeben.


## <a name="handling-background-task-time-limits"></a>Verarbeitung im Hintergrund Task-Zeitlimits

iOS platziert strenge Einschränkungen auf, wie lange ein Hintergrundtask ausgeführt werden kann, und wenn die `EndBackgroundTask` Aufruf erfolgt nicht innerhalb der zugeteilten Zeit, die Anwendung wird beendet. Verfolgen von der die verbleibende Zeit backgrounding und Ablauf Handler bei Bedarf verwenden, können wir iOS Beenden der Anwendung vermeiden.

### <a name="accessing-background-time-remaining"></a>Zugreifen auf die verbleibende Zeit der Hintergrund

Wenn eine Anwendung mit registrierten Tasks im Hintergrund verschoben wurde, erhalten die registrierten Tasks ungefähr 600 Sekunden ausgeführt. Wir sehen, wie lange der Vorgang hat, mithilfe der statischen `BackgroundTimeRemaining` Eigenschaft von der `UIApplication` Klasse. Im folgende Code wird die Zeit in Sekunden, uns zu senden, die unsere Hintergrundtask verlassen hat:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Vermeiden von App-Tunnelabschlusses mit Ablauf-Handler

Zusätzlich zur Vergabe von Zugriff auf die `BackgroundTimeRemaining` Eigenschaft iOS bietet eine ordnungsgemäße Möglichkeit zum Verarbeiten von Hintergrund Zeit ablaufen von Kennwörtern durch eine **Ablauf Handler**. Dies ist eine optionale Codeblock, der ausgeführt wird, wenn die für einen Vorgang vorgesehene Zeit ist abgelaufen. Ruft der Code im Ereignishandler Ablauf `EndBackgroundTask` auf und übergibt die Task-ID, der angibt, dass die app auch verhält wird und verhindert, dass iOS Beenden der app, selbst wenn die Aufgabe nicht genügend Zeit. `EndBackgroundTask` muss innerhalb der Ablauf Handler sowie des normalen Betriebs der Ausführung aufgerufen werden. 

Der Ablauf-Handler wird als einer anonymen Funktion mit einem Lambda-Ausdruck ausgedrückt werden, wie unten gezeigt:

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

Beim Ablauf Handler nicht für die Ausführung des Codes erforderlich sind, sollten Sie immer einen Ablaufdatum Handler mit einer Hintergrundaufgabe verwenden.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Hintergrundaufgaben in iOS 7 und höher

Die größte Änderung in iOS 7 im Hinblick auf Hintergrundaufgaben ist nicht wie die Aufgaben implementiert werden, aber bei der Ausführung.

Beachten Sie, dass vor iOS 7, eine Aufgabe, die im Hintergrund ausgeführte mussten 600 Sekunden abgeschlossen. Ein Grund für diese Grenze ist, dass eine Aufgabe, die im Hintergrund ausgeführte das Gerät aktiv für die Dauer des Vorgangs beibehalten möchten:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Diagramm des Tasks, halten die app aktiv vor iOS 7")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

die Verarbeitung im Hintergrund iOS 7 ist für Akkulaufzeit optimiert. In iOS 7 backgrounding wird opportunistische: anstelle von halten das Gerät aktiv, Aufgaben berücksichtigen, wenn das Gerät wechselt in den Energiesparmodus, und führen Sie stattdessen ihre Verarbeitung in Blöcken auf, wenn Anrufe, Benachrichtigungen, eingehende e-Mails und andere behandeln reaktiviert das Gerät Allgemeine Unterbrechungen. Das folgende Diagramm bietet einen Einblick in die wie eine Aufgabe unterbrochen werden, können Sie:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Diagramm der Aufgabe wird in Blöcke nach der iOS 7 unterteilt")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Da die Aufgabe, die zur Laufzeit nicht mehr kontinuierlich ist, müssen Aufgaben, die netzwerkübertragungen ausführen in iOS 7 anders behandelt werden. Entwicklern wird empfohlen, verwenden Sie die `NSURlSession` -API, um netzwerkübertragungen zu behandeln. Im nächste Abschnitt bietet eine Übersicht über Übertragungen im Hintergrund.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Übertragungen im Hintergrund

Das Rückgrat für Übertragungen im Hintergrund in iOS 7 ist der neue `NSURLSession` API. `NSURLSession` ermöglicht es, Aufgaben zu erstellen:

1.  Übertragung von Inhalt über Unterbrechungen von Netzwerk- und Geräte.
1.  Hochladen und herunterladen große Dateien ( *Hintergrundübertragungsdienst* ).


Werfen Sie näher an, wie dies funktioniert.

### <a name="nsurlsession-api"></a>NSURLSession API

 `NSURLSession` ist eine leistungsstarke API für die Übertragung von Inhalt über das Netzwerk an. Es bietet eine Reihe von Tools zur Übertragung von Daten über netzwerkunterbrechungen und Änderungen in den Anwendungsstatus behandelt wird.

Die `NSURLSession` API erstellt eine oder mehrere Sitzungen, die wiederum Aufgaben aus, um schrittweise Blöcke verknüpfter Daten über das Netzwerk zu erzeugen. Aufgaben werden asynchron zum Übertragen von Daten schnell und zuverlässig ausgeführt. Da `NSURLSession` ist asynchron, jede Sitzung erfordert einen Abschluss Handlerblock, lassen das System und die Anwendung zu signalisieren, wenn eine Übertragung abgeschlossen ist.

Um eine netzwerkübertragung auszuführen, die auf iOS 7-vor und nach der iOS 7 gültig ist, überprüfen Sie, ob ein `NSURLSession` Übertragungen in die Warteschlange einreihen kann, und verwenden Sie eine reguläre Hintergrundaufgabe für die Übertragung verwenden, ist er nicht:

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
> Vermeiden Sie Aufrufe an die Benutzeroberfläche von der Hintergrundfarbe iOS 6-kompatiblem Code, aktualisieren, iOS 6 Hintergrund UI Updates nicht unterstützt, und die Anwendung wird beendet.


Die `NSURLSession` -API umfasst einen umfangreichen Satz von Features zum Behandeln von Authentifizierung, fehlerhafte Übertragungen verwalten und die clientseitige - aber keine serverseitige - Fehler gemeldet. Es hilft der Bridge, die Unterbrechung Aufgabe Zeit eingeführt in iOS 7 ausführen, und bietet auch Unterstützung für das übertragen großer Dateien schnell und zuverlässig. Im nächste Abschnitt wird erklärt, diese zweite Funktion.

### <a name="background-transfer-service"></a>Hintergrundübertragungsdienst

Hoch- oder Herunterladen von Dateien im Hintergrund wurde vor iOS 7 nicht zuverlässig. Hintergrundaufgaben erhalten einen begrenzten Zeitraum ausführen, aber der Zeitaufwand für das Übertragen einer Datei mit dem Netzwerk und die Größe der Datei variiert. In iOS 7 verwenden wir ein `NSURLSession` für die erfolgreiche hochladen und herunterladen große Dateien. Die jeweilige `NSURLSession` verbindungssitzungsmanager-Typ, der netzwerkübertragungen großer Dateien im Hintergrund verarbeitet wird als bezeichnet den *Hintergrundübertragungsdienst*.

Übertragungen mithilfe der Hintergrundübertragungsdienst initiiert werden vom Betriebssystem verwaltet und APIs für Authentifizierung und Fehler behandelt. Da Übertragungen nicht durch eine beliebige Zeitlimit gebunden sind, können sie zum Hochladen oder Herunterladen große Dateien AutoUpdate Inhalt im Hintergrund und vieles mehr verwendet werden. Finden Sie in der [Exemplarische Vorgehensweise zum Übertragen Hintergrund](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) Weitere Informationen zum Implementieren des Diensts.

Der Hintergrundübertragungsdienst wird häufig im Hintergrund abgerufen oder Remote Benachrichtigungen zum Anwendungen, die Aktualisierung des Inhalts im Hintergrund zugeordnet. In den nächsten beiden Abschnitten führen wir das Konzept der Registrierung des Gesamter Anwendungen unter iOS 6 und iOS 7 im Hintergrund ausgeführt.

