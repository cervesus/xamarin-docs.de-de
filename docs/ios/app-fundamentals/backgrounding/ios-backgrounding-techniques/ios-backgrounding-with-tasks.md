---
title: iOS-Hintergrundverarbeitung mit Aufgaben
description: In diesem Dokument wird beschrieben, wie Hintergrundaufgaben verwendet werden, um Aufgaben mit langer Ausführungszeit auszuführen, nachdem eine Anwendung im Hintergrund abgelegt wurde.
ms.prod: xamarin
ms.assetid: 205D230E-C618-4D69-96EE-4B91D7819121
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 56ee93146bb84de0b48885d80407316e81cb512c
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2019
ms.locfileid: "69521366"
---
# <a name="ios-backgrounding-with-tasks"></a>iOS-Hintergrundverarbeitung mit Aufgaben

Die einfachste Möglichkeit zum Durchführen von backerden für IOS besteht darin, die Anforderungen für die Rückstellung in Aufgaben zu unterbrechen und die Aufgaben im Hintergrund auszuführen. Tasks unterliegen einem strengen Zeit Limit und erhalten in der Regel ungefähr 600 Sekunden (10 Minuten) Verarbeitungszeit, nachdem eine Anwendung auf IOS 6 in den Hintergrund gewechselt ist, und unter IOS 7 und weniger als 10 Minuten.

Hintergrundaufgaben können in drei Kategorien unterteilt werden:

1. Im **Hintergrund sichere Aufgaben** , die an beliebiger Stelle in der Anwendung aufgerufen werden, wenn Sie eine Aufgabe haben, die nicht unterbrochen werden soll, wenn die Anwendung in den Hintergrund gelangt
1. **Didenterbackground-Tasks** , die während `DidEnterBackground` der Anwendungslebenszyklus-Methode aufgerufen werden, um die Bereinigung und Zustands Speicherung zu unterstützen.
1. **Hintergrund Übertragungen (IOS 7** und höher): eine besondere Art von Hintergrundaufgabe für die Durchführung von Netzwerkübertragungen unter IOS 7. Im Gegensatz zu regulären Aufgaben haben Hintergrund Übertragungen keine vorab festgelegte Zeitbeschränkung.


Hintergrund sichere Aufgaben und `DidEnterBackground` Aufgaben sind auf IOS 6 und IOS 7 sicher, mit einigen geringfügigen Unterschieden. Sehen wir uns diese zwei Arten von Aufgaben genauer an.

## <a name="creating-background-safe-tasks"></a>Erstellen von Hintergrund sicheren Aufgaben

Einige Anwendungen enthalten Tasks, die von IOS nicht unterbrochen werden sollten, wenn sich der Zustand der Anwendung ändert. Eine Möglichkeit, diese Aufgaben vor der Unterbrechung zu schützen, besteht darin, Sie bei IOS als Aufgaben mit langer Laufzeit zu registrieren. Sie können dieses Muster an beliebiger Stelle in der Anwendung verwenden, wenn Sie nicht möchten, dass eine Aufgabe unterbrochen wird, wenn der Benutzer die APP im Hintergrund ablegen soll. Ein guter Kandidat für dieses Muster wären z. b. das Senden der Registrierungsinformationen eines neuen Benutzers an Ihren Server oder das Überprüfen der Anmelde Informationen.

Der folgende Code Ausschnitt veranschaulicht das Registrieren eines Tasks, der im Hintergrund ausgeführt wird:

```csharp
nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});

//runs on main or background thread
FinishLongRunningTask(taskID);

UIApplication.SharedApplication.EndBackgroundTask(taskID);
```

Der Registrierungsprozess verknüpft eine Aufgabe mit einem eindeutigen Bezeichner `taskID`, und umschließt Sie dann `BeginBackgroundTask` in Abgleich und `EndBackgroundTask` Aufrufe. Um den Bezeichner zu generieren, rufen Sie die `BeginBackgroundTask` -Methode für das `UIApplication` -Objekt auf, und starten Sie dann die Aufgabe mit langer Ausführungszeit, in der Regel in einem neuen Thread. Wenn die Aufgabe abgeschlossen ist, wird aufgerufen `EndBackgroundTask` und der gleiche Bezeichner übergeben. Dies ist wichtig, da die Anwendung von IOS beendet wird `BeginBackgroundTask` , wenn ein-Vorgang nicht `EndBackgroundTask`übereinstimmt.

> [!IMPORTANT]
> Hintergrund sichere Aufgaben können abhängig von den Anforderungen der Anwendung entweder im Hauptthread oder in einem Hintergrund Thread ausgeführt werden.


## <a name="performing-tasks-during-didenterbackground"></a>Ausführen von Aufgaben während didenterbackground

Wenn Sie eine Aufgabe mit langer Ausführungszeit verwenden, kann die Registrierung verwendet werden, um Aufgaben zu starten, während eine Anwendung im Hintergrund abgelegt wird. IOS stellt in der appdelegatklasse eine Ereignis Methode `DidEnterBackground` namens zur Verfügung, die zum Speichern des Anwendungs Zustands, zum Speichern von Benutzerdaten und zum Verschlüsseln von sensiblen Inhalten verwendet werden kann, bevor eine Anwendung in den Hintergrund wechselt. Eine Anwendung hat ungefähr fünf Sekunden Zeit, um von dieser Methode zurückzukehren, oder Sie wird beendet. Daher können Bereinigungs Tasks, die möglicherweise mehr als fünf Sekunden dauern, innerhalb der `DidEnterBackground` -Methode aufgerufen werden. Diese Tasks müssen in einem separaten Thread aufgerufen werden.

Der Prozess ist beinahe identisch mit dem, um eine Aufgabe mit langer Ausführungszeit zu registrieren. Dies wird im folgenden Code Ausschnitt veranschaulicht:

```csharp
public override void DidEnterBackground (UIApplication application) {
  nint taskID = UIApplication.SharedApplication.BeginBackgroundTask( () => {});
  new Task ( () => {
    DoWork();
    UIApplication.SharedApplication.EndBackgroundTask(taskID);
  }).Start();
}
```

Wir beginnen mit dem über `DidEnterBackground` `BeginBackgroundTask` Schreiben der- `AppDelegate`Methode in der, in der wir unsere Aufgabe wie im vorherigen Beispiel registriert haben. Als nächstes erzeugen wir einen neuen Thread und führen unsere Aufgabe mit langer Ausführungszeit aus. Beachten Sie, `EndBackgroundTask` dass der-Befehl jetzt innerhalb der Aufgabe mit langer Ausführungszeit ausgeführt wird `DidEnterBackground` , da die-Methode bereits zurückgegeben wurde.

> [!IMPORTANT]
> IOS verwendet einen [Watchdog-Mechanismus](https://developer.apple.com/library/ios/qa/qa1693/_index.html) , um sicherzustellen, dass die Benutzeroberfläche einer Anwendung weiterhin reaktionsfähig ist. Eine Anwendung, die zu viel Zeit in `DidEnterBackground` verbringt, reagiert in der Benutzeroberfläche nicht mehr. Das Auslösen von Aufgaben, die im Hintergrund ausgeführt `DidEnterBackground` werden können, ermöglicht eine rechtzeitige Rückgabe, sodass die Benutzeroberfläche reaktionsfähig bleibt und verhindert, dass der Watchdog die Anwendung beendet.


## <a name="handling-background-task-time-limits"></a>Behandeln von Zeit Limits für Hintergrundaufgaben

IOS legt strikte Beschränkungen fest, wie lange eine Hintergrundaufgabe ausgeführt werden kann. Wenn `EndBackgroundTask` der Aufruf nicht innerhalb der vorgesehenen Zeit erfolgt, wird die Anwendung beendet. Wenn Sie die verbleibende Zeit für die Rückstellung nachverfolgen und bei Bedarf Ablauf Handler verwenden, können wir das Beenden der Anwendung durch IOS vermeiden.

### <a name="accessing-background-time-remaining"></a>Zugreifen auf die verbleibende Hintergrund Zeit

Wenn eine Anwendung mit registrierten Tasks in den Hintergrund verschoben wird, werden die registrierten Tasks ungefähr 600 Sekunden ausgeführt. Wir können mithilfe der statischen `BackgroundTimeRemaining` -Eigenschaft `UIApplication` der-Klasse überprüfen, wie lange die Aufgabe ausgeführt werden muss. Der folgende Code gibt die Zeit in Sekunden an, die unsere Hintergrundaufgabe verbleiben kann:

```csharp
double timeRemaining = UIApplication.SharedApplication.BackgroundTimeRemaining;
```

### <a name="avoiding-app-termination-with-expiration-handlers"></a>Vermeiden der Beendigung der APP mit Ablauf Handlern

Zusätzlich zum Gewähren von Zugriff auf die `BackgroundTimeRemaining` -Eigenschaft bietet IOS eine ordnungsgemäße Möglichkeit, den Ablauf der Hintergrund Zeit mithilfe eines **Ablauf Handlers**zu verarbeiten. Dies ist ein optionaler Codeblock, der ausgeführt wird, wenn die Zeit, die für eine Aufgabe zugewiesen wird, bald abläuft. Der Code im Ablauf Handler ruft `EndBackgroundTask` die Task-ID auf und übergibt Sie, was darauf hinweist, dass sich die APP gut verhält, und verhindert, dass IOS die APP beendet, auch wenn die Aufgabe nicht länger ausgeführt wird. `EndBackgroundTask`muss innerhalb des Ablauf Handlers und im normalen Ausführungs Verlauf aufgerufen werden. 

Der Ablauf Handler wird mithilfe eines Lambda-Ausdrucks als anonyme Funktion ausgedrückt, wie unten dargestellt:

```csharp
Task.Factory.StartNew( () => {

    //expirationHandler only called if background time allowed exceeded
    var taskId = UIApplication.SharedApplication.BeginBackgroundTask(() => {
        Console.WriteLine("Exhausted time");
        UIApplication.SharedApplication.EndBackgroundTask(taskId); 
    });
    while(myFlag == true)
    {
        Console.WriteLine(UIApplication.SharedApplication.BackgroundTimeRemaining);
        myFlag = SomeCalculationNeedsMoreTime();
    }
    //Only called if loop terminated due to myFlag and not expiration of time
    UIApplication.SharedApplication.EndBackgroundTask(taskId);
});
```

Während Ablauf Handler nicht erforderlich sind, damit der Code ausgeführt wird, sollten Sie immer einen Ablauf Handler mit einer Hintergrundaufgabe verwenden.

 <a name="background_tasks_in_iOS_7" />

## <a name="background-tasks-in-ios-7"></a>Hintergrundaufgaben in ios 7 und höher

Die größte Änderung in ios 7 in Bezug auf Hintergrundaufgaben ist nicht, wie die Aufgaben implementiert werden, sondern wann Sie ausgeführt werden.

Erinnern Sie sich daran, dass eine im Hintergrund ausgeführten Aufgaben vor IOS 7 600 Sekunden dauern musste. Ein Grund für diese Beschränkung ist, dass eine im Hintergrund ausgeführten Aufgabe das Gerät für die Dauer der Aufgabe wach hält:

 [![](ios-backgrounding-with-tasks-images/ios6.png "Diagramm der Aufgabe, die die APP vor IOS 7 aktiv hält")](ios-backgrounding-with-tasks-images/ios6.png#lightbox)

Die IOS 7-Hintergrundverarbeitung ist für eine längere Akku Lebensdauer optimiert. In ios 7 ist die hinterstellung opportunistisch: anstatt das Gerät wach zu halten, berücksichtigen Aufgaben, wann das Gerät in den Standbymodus wechselt, und führen stattdessen die Verarbeitung in Blöcken aus, wenn das Gerät aktiviert wird, um Telefonanrufe, Benachrichtigungen, eingehende e-Mails und andere zu verarbeiten. häufige Unterbrechungen. Das folgende Diagramm bietet einen Einblick in die Art und Weise, wie eine Aufgabe aufgegliedert werden kann:

 [![](ios-backgrounding-with-tasks-images/ios7.png "Diagramm der Aufgabe, die in Blöcke nach IOS 7 aufgeteilt wird")](ios-backgrounding-with-tasks-images/ios7.png#lightbox)

Da die Task Laufzeit nicht länger kontinuierlich ist, müssen Tasks, die Netzwerkübertragungen ausführen, in ios 7 anders gehandhabt werden. Entwicklern wird empfohlen, die API `NSURlSession` für die Handhabung von Netzwerkübertragungen zu verwenden. Der nächste Abschnitt stellt eine Übersicht über die Hintergrund Übertragungen dar.

 <a name="background-transfers" />

## <a name="background-transfers"></a>Hintergrund Übertragungen

Das Rückgrat von Hintergrund Übertragungen in ios 7 ist die neue `NSURLSession` API. `NSURLSession`ermöglicht es uns, Aufgaben für Folgendes zu erstellen:

1. Übertragen von Inhalten durch Netzwerk-und Geräte Unterbrechungen.
1. Hochladen und Herunterladen großer Dateien ( *Hintergrund Übertragungs Dienst* ).


Werfen wir einen genaueren Blick darauf, wie dies funktioniert.

### <a name="nsurlsession-api"></a>Nsurlsession-API

 `NSURLSession`ist eine leistungsstarke API zum Übertragen von Inhalten über das Netzwerk. Es stellt eine Reihe von Tools bereit, um die Übertragung von Daten durch Netzwerkunterbrechungen und Änderungen in Anwendungs Zuständen zu verarbeiten.

Die `NSURLSession` API erstellt eine oder mehrere Sitzungen, die wiederum Aufgaben zum über das Netzwerk gehörigen Datenblöcke zusammenbringen. Tasks werden asynchron ausgeführt, um Daten schnell und zuverlässig zu übertragen. Da `NSURLSession` asynchron ist, erfordert jede Sitzung einen Beendigungs Handlerblock, damit das System und die Anwendung erkennen können, wann eine Übertragung abgeschlossen ist.

Um eine Netzwerkübertragung durchzuführen, die sowohl für Pre-IOS 7 als auch nach IOS 7 gültig ist, über `NSURLSession` prüfen Sie, ob eine verfügbar ist, um Übertragungen in die Warteschlange einzureihen, und verwenden Sie einen regulären Hintergrund Task, um die Übertragung auszuführen

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
> Vermeiden Sie Aufrufe zur Aktualisierung der Benutzeroberfläche aus dem Hintergrund in ios 6-Kompatibilitäts Code, da IOS 6 keine Aktualisierungen der Hintergrund Benutzeroberfläche unterstützt und die Anwendung beendet wird.


Die `NSURLSession` API umfasst einen umfangreichen Satz von Features zur Handhabung der Authentifizierung, zum Verwalten von fehlgeschlagenen Übertragungen und zum Client seitigen melden, aber nicht serverseitiger Fehler. Er hilft dabei, die Unterbrechungen in der in ios 7 eingeführten Task Laufzeit zu überbrücken und bietet zudem Unterstützung für die schnelle und zuverlässige Übertragung großer Dateien. Im nächsten Abschnitt wird dieses zweite Feature untersucht.

### <a name="background-transfer-service"></a>Hintergrund Übertragungs Dienst

Vor IOS 7 war das Hochladen oder Herunterladen von Dateien im Hintergrund unzuverlässig. Hintergrundaufgaben werden zeitlich eingeschränkt ausgeführt, aber die für die Übertragung einer Datei benötigte Zeit variiert je nach Netzwerk und Größe der Datei. In ios 7 können wir mit einem `NSURLSession` erfolgreich große Dateien hochladen und herunterladen. Der bestimmte `NSURLSession` Sitzungstyp, der die Netzwerkübertragungen von großen Dateien im Hintergrund verarbeitet, wird als *Hintergrund Übertragungs Dienst*bezeichnet.

Über den Hintergrund Übertragungs Dienst initiierte Übertragungen werden vom Betriebssystem verwaltet und stellen APIs bereit, um Authentifizierung und Fehler zu behandeln. Da die Übertragungen nicht durch ein beliebiges Zeit Limit gebunden sind, können Sie zum Hochladen oder Herunterladen großer Dateien, zum automatischen Aktualisieren von Inhalten im Hintergrund und vieles mehr verwendet werden. Ausführliche Informationen zum Implementieren des Dienstanbieter finden Sie in der exemplarischen Vorgehensweise zur [Hintergrund Übertragung](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/background-transfer-walkthrough.md) .

Der Hintergrund Übertragungs Dienst wird häufig mit dem Abrufen im Hintergrund oder Remote Benachrichtigungen gekoppelt, damit Anwendungen Inhalte im Hintergrund aktualisieren können. In den nächsten beiden Abschnitten wird das Konzept der Registrierung ganzer Anwendungen eingeführt, um Sie sowohl auf IOS 6 als auch auf IOS 7 im Hintergrund auszuführen.

