---
title: Aktualisieren einer xamarin. IOS-App im Hintergrund
description: In diesem Dokument werden verschiedene Möglichkeiten beschrieben, wie Sie eine xamarin. IOS-App im Hintergrund aktualisieren, z. b. Regions Überwachung, Hintergrund Abruf und Remote Benachrichtigungen.
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: ae08d7d2d8d9de700570311f2294df737240b73f
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/09/2020
ms.locfileid: "84572155"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>Aktualisieren einer xamarin. IOS-App im Hintergrund

Die Hintergrund Aktualisierung ist der Vorgang, bei dem eine Anwendung, die angehalten oder nicht ausgeführt wird, ausgeführt und mit neuem Inhalt aktualisiert wird. IOS bietet drei Optionen zum Aktualisieren von Inhalten im Hintergrund:

1. *Regions Überwachung* und *signifikanter Speicherort ändern Dienst* -Standort-kompatible APIs löst Hintergrund Aktualisierungen basierend auf Änderungen am Speicherort des Benutzers aus. Diese APIs können mit Bedacht verwendet werden, um Inhalte in nicht auf dem Standort basierenden IOS 6-Anwendungen zu aktualisieren, in denen keine anderen Optionen zur Verfügung stehen.
1. *Abrufen im Hintergrund (IOS 7* und höher): ein Temporaler Ansatz zum Aktualisieren von *nicht kritischen* Inhalten, die *häufig* aktualisiert werden.
1. *Remote Benachrichtigungen (IOS 7* und höher): Anwendungen, die Pushbenachrichtigungen empfangen, können die Benachrichtigungen verwenden, um Aktualisierungen von Hintergrund Inhalten zu initiieren. Diese Methode kann verwendet werden, um mit *wichtigen, zeitsensiblen* Inhalten zu aktualisieren, die *sporadisch* aktualisiert werden.

In den folgenden Abschnitten werden die Grundlagen dieser Optionen behandelt.

## <a name="region-monitoring-and-significant-location-changes"></a>Regions Überwachung und bedeutende Standortänderungen

IOS bietet zwei standortabhängige APIs mit Hintergrund Funktionen:

1. Die *Regions Überwachung* ist der Prozess der Einrichtung von Bereichen mit Grenzen und dem Reaktivieren des Geräts, wenn der Benutzer eine Region betritt oder verlässt. Bereiche sind zirkulär und können eine unterschiedliche Größe aufweisen. Wenn der Benutzer eine Regions Grenze überschreitet, wird das Gerät aktiviert, um das Ereignis zu verarbeiten, normalerweise durch Auslösen einer Benachrichtigung oder Auslösen einer Aufgabe. Die Regions Überwachung erfordert GPS und erhöht die Akku-und Datennutzung.
1. Der *bedeutende Standort Änderungs Dienst* ist eine einfachere, energiesparende Option, die für Geräte mit Mobil Funk Radios verfügbar ist. Eine Anwendung, die auf bedeutende Änderungen am Standort lauscht, wird benachrichtigt, wenn das Gerät Zellen Türme wechselt. Dieser Dienst kann verwendet werden, um eine angehaltene oder beendete Anwendung zu aktivieren, und bietet die Möglichkeit, im Hintergrund nach neuen Inhalten zu suchen. Hintergrund Aktivitäten sind auf ungefähr 10 Sekunden beschränkt, es sei denn, Sie sind mit einer [Hintergrundaufgabe](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) gekoppelt.

Eine Anwendung benötigt nicht den Speicherort `UIBackgroundMode` , um diese Standort abhängigen APIs zu verwenden. Da IOS die Typen von Tasks, die ausgeführt werden können, wenn das Gerät durch Änderungen am Speicherort des Benutzers reaktiviert wird, nicht nachverfolgt, stellen diese APIs eine Problem Umgehung zum Aktualisieren von Inhalten im Hintergrund unter IOS 6 dar. *Beachten Sie, dass das Auslösen von Hintergrund Aktualisierungen mit standortbasierten APIs auf Geräte Ressourcen zurückgreifen wird und Benutzer möglicherweise verwirren, die nicht verstehen, warum eine Anwendung Zugriff auf ihren Speicherort benötigt*. Verwenden Sie bei der Implementierung von Regions Überwachung oder wichtigen Orts Änderungen für die Hintergrundverarbeitung in Anwendungen, die nicht bereits die Location-APIs verwenden, die Diskretion.

Apps, die die Standortüberwachung für die Hintergrundverarbeitung verwenden, machen einen Fehler in ios 6 verfügbar: Wenn die Anforderungen einer Anwendung nicht in eine für den Hintergrund erforderliche Kategorie passen, haben Sie eingeschränkte backgroundoptionen. Mit der Einführung von zwei neuen APIs, dem *Hintergrund* Abruf und *Remote Benachrichtigungen*, bietet IOS 7 (und höher) die Möglichkeit, sich für weitere Anwendungen gegenseitig zu unterstellen. In den nächsten beiden Abschnitten werden diese neuen APIs eingeführt.

<a name="background_fetch"></a>

## <a name="background-fetch-ios-7-and-greater"></a>Abrufen im Hintergrund (IOS 7 und höher)

In ios 6 hat eine Anwendung, die in den Vordergrund gewechselt ist, Zeit benötigt, um neuen Inhalt zu laden, um Benutzern eine kurze Darstellung von Inhalten zu geben. Der Hintergrund Abruf ermöglicht Anwendungen, neue Daten zu laden, *bevor* ein Benutzer die Anwendung aufruft, und dem Benutzer die aktuellsten Inhalte bereitzustellen.

Um das Abrufen im Hintergrund zu implementieren, bearbeiten Sie " *Info. plist* ", und aktivieren Sie die Kontrollkästchen **hintergrundmodi** und **Hintergrund** Abruf aktivieren:

 [![](updating-an-application-in-the-background-images/fetch.png "Edit the Info.plist and check the Enable Background Modes and Background Fetch check boxes")](updating-an-application-in-the-background-images/fetch.png#lightbox)

`AppDelegate`Überschreiben Sie als nächstes die- `FinishedLaunching` Methode, um das minimale Abruf Intervall festzulegen. In diesem Beispiel können Sie festlegen, wie oft neue Inhalte abgerufen werden sollen:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Führen Sie schließlich den Abruf Vorgang aus, indem Sie die `PerformFetch` -Methode in der überschreiben `AppDelegate` und einen *Vervollständigungs Handler*übergeben. Der Abschluss Handler ist ein Delegat, der Folgendes annimmt `UIBackgroundFetchResult` :

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Wenn Sie die Inhalts Aktualisierung abgeschlossen haben, wird das Betriebssystem durch Aufrufen des Vervollständigungs Handlers mit dem entsprechenden Status informiert. IOS bietet drei Optionen für den Beendigungs handlerstatus:

1. `UIBackgroundFetchResult.NewData`: Wird aufgerufen, wenn neuer Inhalt abgerufen wurde und die Anwendung aktualisiert wurde.
1. `UIBackgroundFetchResult.NoData`: Wird aufgerufen, wenn das Abrufen für neuen Inhalt durchlaufen wurde, aber kein Inhalt verfügbar ist.
1. `UIBackgroundFetchResult.Failed`-Nützlich für die Fehlerbehandlung. Dies wird aufgerufen, wenn der Abruf Vorgang nicht durchlaufen werden konnte.

Anwendungen, die im Hintergrund abrufen, können Aufrufe ausführen, um die Benutzeroberfläche aus dem Hintergrund zu aktualisieren. Wenn der Benutzer die APP öffnet, ist die Benutzeroberfläche auf dem neuesten Stand und zeigt neue Inhalte an. Dadurch wird auch die APP-switchermomentaufnahme der Anwendung aktualisiert, sodass der Benutzer sehen kann, wann die Anwendung neuen Inhalt hat.

> [!IMPORTANT]
> Nachdem `PerformFetch` aufgerufen wurde, hat die Anwendung ungefähr 30 Sekunden Zeit, den Download neuer Inhalte zu starten und den Vervollständigungs Handler-Block aufzurufen. Wenn dies zu lange dauert, wird die APP beendet. Verwenden Sie beim Herunterladen von Medien oder anderen großen Dateien das Abrufen von Hintergrunddaten mit dem _Hintergrund Übertragungs Dienst_ .

### <a name="backgroundfetchinterval"></a>Backgroundfetchinterval

Im obigen Beispielcode legen wir fest, wie oft neue Inhalte abgerufen werden sollen, indem Sie das minimale Abruf Intervall auf festlegen `BackgroundFetchIntervalMinimum` . IOS bietet drei Optionen für das Abruf Intervall:

1. `BackgroundFetchIntervalNever`-Weisen Sie das System an, nie neuen Inhalt abzurufen. Verwenden Sie diese Option, um das Abrufen in bestimmten Situationen zu deaktivieren, z. b. wenn der Benutzer nicht angemeldet ist. Dies ist der Standardwert für das Abruf Intervall. 
1. `BackgroundFetchIntervalMinimum`: Lassen Sie das System entscheiden, wie oft basierend auf den Benutzer Mustern, der Akku Lebensdauer, der Datennutzung und den Anforderungen anderer Anwendungen abgerufen werden sollen.
1. `BackgroundFetchIntervalCustom`Wenn Sie wissen, wie oft der Inhalt einer Anwendung aktualisiert wird, können Sie nach jedem Abruf Vorgang ein "standbyintervall" angeben, in dem die Anwendung daran gehindert wird, neue Inhalte abzurufen. Sobald das Intervall aktiv ist, bestimmt das System, wann Inhalt abgerufen werden soll.

Sowohl `BackgroundFetchIntervalMinimum` als auch `BackgroundFetchIntervalCustom` verlassen sich auf das System, um Abruf Vorgänge zu planen. Dieses Intervall ist dynamisch, und die Anpassung an die Anforderungen des Geräts und die Gewohnheiten der einzelnen Benutzer. Wenn ein Benutzer beispielsweise jeden Morgen eine Anwendung prüft und jede Stunde eine andere Prüfung durchführt, stellt IOS sicher, dass der Inhalt für beide Benutzer jedes Mal auf dem neuesten Stand ist, wenn Sie die Anwendung öffnen.

Der Hintergrund Abruf sollte für Anwendungen verwendet werden, die häufig mit nicht kritischem Inhalt aktualisiert werden. Bei Anwendungen mit wichtigen Updates sollten Remote Benachrichtigungen verwendet werden. Remote Benachrichtigungen basieren auf dem Abrufen des Hintergrunds und verwenden denselben Vervollständigungs Handler. Wir werden uns weiter mit Remote Benachrichtigungen beschäftigen.

 <a name="remote_notifications"></a>

## <a name="remote-notifications-ios-7-and-greater"></a>Remote Benachrichtigungen (IOS 7 und höher)

Pushbenachrichtigungen sind JSON-Nachrichten, die über den *Apple Push Notification Service (APNs)* von einem Anbieter an ein Gerät gesendet werden.

In ios 6 weist eine eingehende Pushbenachrichtigung das System an, den Benutzer darüber zu informieren, dass in einer Anwendung etwas Interessantes passiert ist. Wenn Sie auf die Benachrichtigung klicken, wird die Anwendung aus dem Zustand "angehalten" oder "beendet" abgerufen, und die APP beginnt mit dem Aktualisieren von Inhalt IOS 7 (und höher) erweitert normale Pushbenachrichtigungen, indem Anwendungen die Möglichkeit geben, Inhalte im Hintergrund zu aktualisieren, *bevor* der Benutzer benachrichtigt wird, damit der Benutzer die Anwendung öffnen und sofort neuen Inhalt präsentieren kann.

Bearbeiten Sie zum Implementieren von Remote Benachrichtigungen *Info. plist* , und aktivieren Sie die Kontrollkästchen **hintergrundmodi** und **Remote Benachrichtigungen** aktivieren:

 [![](updating-an-application-in-the-background-images/remote.png "Background Mode set to Enable Background Modes and Remote notifications")](updating-an-application-in-the-background-images/remote.png#lightbox)

Legen Sie als nächstes das `content-available` Flag für die Pushbenachrichtigung selbst auf 1 fest. Dadurch kann die Anwendung wissen, wie neue Inhalte abgerufen werden können, bevor die Warnung angezeigt wird:

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

Überschreiben Sie im *appdelegat*die- `DidReceiveRemoteNotification` Methode, um die Benachrichtigungs Nutzlast auf verfügbaren Inhalt zu überprüfen, und rufen Sie den entsprechenden Vervollständigungs Handler auf.

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Remote Benachrichtigungen sollten für seltene Updates mit Inhalten verwendet werden, die für die Funktionalität der Anwendung von entscheidender Bedeutung sind. Weitere Informationen zu Remote Benachrichtigungen finden Sie im Leitfaden für xamarin- [Pushbenachrichtigungen in ios](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) .

> [!IMPORTANT]
> Da der Aktualisierungsmechanismus in Remote Benachrichtigungen auf dem Abruf im Hintergrund basiert, muss die Anwendung den Download neuer Inhalte starten und den Beendigungs Handlerblock innerhalb von 30 Sekunden nach dem Empfang der Benachrichtigung aufzurufen, oder die Anwendung wird von IOS beendet. Es empfiehlt sich, Remote Benachrichtigungen mit dem _Hintergrund Übertragungs Dienst_ zu koppeln, wenn Medien oder andere große Dateien im Hintergrund heruntergeladen werden.

### <a name="silent-remote-notifications"></a>Automatische Remote Benachrichtigungen

Remote Benachrichtigungen sind eine einfache Möglichkeit, Anwendungen über Updates zu benachrichtigen und neue Inhalte abzurufen. es gibt jedoch Fälle, in denen wir den Benutzer nicht benachrichtigen müssen, dass sich etwas geändert hat. Wenn ein Benutzer beispielsweise eine Datei für die Synchronisierung ausstellt, müssen wir Sie nicht jedes Mal Benachrichtigen, wenn die Datei aktualisiert wird. Die Datei Synchronisierung ist kein überraschendes Ereignis, und es ist nicht erforderlich, dass die Benutzer sofortige Aufmerksamkeit erhalten. Benutzer erwarten nur, dass die Datei auf dem neuesten Stand ist, wenn Sie Sie öffnen.

In Fällen wie der oben beschriebenen ermöglicht IOS, dass Pushbenachrichtigungen im Hintergrund gesendet werden, d. h. ohne eine Warnung. Wenn Sie eine reguläre Benachrichtigung in eine unbeaufsichtigte Benachrichtigung umwandeln möchten, entfernen Sie einfach die Warnung aus der Benachrichtigungs Nutzlast:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Begrenzung der Bandbreite

Der größte Unterschied zwischen normaler und automatischer Benachrichtigung aus der Perspektive des Entwicklers besteht darin, dass für automatische Pushvorgänge eine Begrenzung der Datenübertragungsrate APNs verzögert die Übermittlung von stillen pushvorgängen an das Gerät, wenn die pushrate zu hoch wird. Dadurch wird sichergestellt, dass Anwendungen Geräte Ressourcen nicht mit zu vielen automatischen Benachrichtigungen entladen.

Allerdings lassen APNs automatische Benachrichtigungen zusammen mit einer normalen Remote Benachrichtigung oder Keep-Alive-Antwort "Piggyback" zu. Da reguläre Benachrichtigungen nicht raten gebunden sind, können Sie verwendet werden, um per Push gespeicherte unbeaufsichtigte Benachrichtigungen von APNs an das Gerät zu übersetzen, wie in der folgenden Abbildung veranschaulicht:

 [![](updating-an-application-in-the-background-images/silent.png "Regular notifications can be used to push stored silent notifications from the APNs to the device, as illustrated by this diagram")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple ermutigt Entwickler, automatische Pushbenachrichtigungen zu senden, wenn die Anwendung dies erfordert, und ermöglicht es dem APNs, ihre Übermittlung zu planen.

In diesem Abschnitt haben wir die verschiedenen Optionen zum Aktualisieren von Inhalten im Hintergrund behandelt, um Aufgaben auszuführen, die nicht in eine im Hintergrund erforderliche Kategorie passen. Sehen wir uns nun einige dieser APIs in Aktion an.

 [Nächstes: Teil 4: Exemplarische Vorgehensweisen für IOS-Back-Ends](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
