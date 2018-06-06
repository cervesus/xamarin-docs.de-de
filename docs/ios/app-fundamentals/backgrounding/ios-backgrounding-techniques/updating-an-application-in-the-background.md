---
title: Eine App Xamarin.iOS im Hintergrund aktualisiert.
description: Dieses Dokument beschreibt die verschiedenen Möglichkeiten, eine Xamarin.iOS-app aktualisieren, die im Hintergrund, z. B. Region zu überwachen, Abrufen im Hintergrund und remote-Benachrichtigungen.
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 973c18528eee2096b29ba86e82ceff31ecf3e207
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784064"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>Eine App Xamarin.iOS im Hintergrund aktualisiert.

Hintergrundaktualisierungen versteht man anhand der Ergebnisse einer Anwendung, die angehalten wurde oder nicht ausgeführt, und es durch neuen Inhalt aktualisieren. iOS bietet drei Optionen zum Aktualisieren von Inhalt im Hintergrund:

1.  *Region-Überwachung* und *erhebliche Änderungen-Speicherortdienst* -APIs mit Standortbestimmung Trigger im Hintergrund aktualisiert, basierend auf Änderungen an den Standort des Benutzers. Diese APIs können mit Vorsicht verwendet werden zum Aktualisieren von Inhalt im Speicherort basierende iOS 6-Anwendungen, andere Optionen sind nicht verfügbar.
1.  *Fetch (iOS 7 und höher) im Hintergrund* -einen temporale Ansatz für die Aktualisierung *unkritische* Inhalte, die aktualisiert *häufig* .
1.  *Remote-Benachrichtigungen (iOS 7 und höher)* -Anwendungen, die Pushbenachrichtigungen empfangen, können die Benachrichtigungen verwenden, um Inhalt hintergrundaktualisierungen auslösen. Diese Methode kann zum Aktualisieren mit *wichtige, zeitkritische* Inhalt, der aktualisiert *sporadisch* .


In den folgenden Abschnitten werden die Grundlagen dieser Optionen behandelt.

## <a name="region-monitoring-and-significant-location-changes"></a>Überwachen der Region und erhebliche Speicherort Änderungen

iOS stellt zwei Standortbestimmung APIs mit backgrounding Funktionen bereit:

1.  *Region-Überwachung* versteht man Bereiche mit Grenzen einrichten und das Gerät aktiviert, wenn der Benutzer eingibt, oder einen Bereich verlässt. Bereiche sind kreisförmige und verschiedener Größe werden können. Wenn der Benutzer eine Bereichsgrenze überschreitet, wird das Gerät auf die Behandlung des Ereignisses, in der Regel durch das Auslösen einer benachrichtigungs oder eine Aufgabe Informationssammlung reaktiviert. Region-Überwachung GPS erfordert, und erhöht Akku und Datennutzung.
1.  Die *erhebliche Änderungen-Speicherortdienst* ist eine einfachere, Energie sparen-Option für die Geräte mithilfe Mobilfunk Sender verfügbar. Eine Anwendung überwacht Änderungen am erhebliche Speicherort wird benachrichtigt, wenn das Gerät Zelle Towers wechselt. Dieser Dienst kann verwendet werden, um die Aktivierung einer Anwendung angehalten oder beendeten, und bietet die Möglichkeit zur Prüfung auf neue Inhalte im Hintergrund. Hintergrundaktivität ist ca. 10 Sekunden auf, es sei denn, die zusammen mit einem [Hintergrundtask](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .


Eine Anwendung benötigt keine Speicherort `UIBackgroundMode` , die diese Location Awareness APIs verwenden. Da iOS Tasktypen verfolgen nicht, die ausgeführt werden kann, wenn das Gerät von Änderungen in den Standort des Benutzers reaktiviert ist, geben diese APIs zum Aktualisieren von Inhalt im Hintergrund auf iOS 6 zu umgehen. *Beachten Sie, dass im Hintergrund Updates mit APIs standortbasierte auslösen Geräteressourcen Zeichnen wird und Benutzer, die nicht verstehen, warum eine Anwendung den Zugriff auf ihren Standort benötigt möglicherweise verwechseln*. Seien Sie vorsichtig beim Implementieren von Überwachung Region oder erhebliche Änderungen am Speicherort für die hintergrundverarbeitung in Anwendungen, die bereits nicht den Speicherort-APIs verwenden.

Apps mit Speicherort für die Verarbeitung im Hintergrund Überwachung verfügbar machen, einen Fehler in iOS 6: Wenn eine Anwendung muss in einer Kategorie Hintergrund erforderlichen groß sind, ist die eingeschränkt backgrounding Optionen. Mit der Einführung von zwei neue APIs *Hintergrund Fetch* und *Remote Benachrichtigungen*, iOS 7 (und höher) bietet backgrounding Möglichkeiten mehr Programmen. In den nächsten beiden Abschnitten einführen, diesen neuen APIs.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Abrufen im Hintergrund (iOS 7 und höher)

IOS 6 benötigt eine Anwendung im Vordergrund eingeben Zeit zum Laden neue Inhalte, die kurz präsentieren von Benutzern mit Inhalt, den sie bereits gesehen haben. Abrufen im Hintergrund ermöglicht Anwendungen die neue Daten laden *vor* ein Benutzer startet die Anwendung, und geben Sie den Benutzer mit der neuesten Inhalte.

Bearbeiten Sie zum Implementieren der abrufen im Hintergrund *"Info.plist"* und überprüfen Sie die **Aktivieren von Hintergrundmodi** und **abrufen im Hintergrund** Kontrollkästchen:

 [![](updating-an-application-in-the-background-images/fetch.png "Bearbeiten Sie der Datei \"Info.plist\", und prüfen Sie die Kontrollkästchen Aktivieren von Hintergrundmodi und Fetch-Hintergrund")](updating-an-application-in-the-background-images/fetch.png#lightbox)

Im nächsten Schritt in der `AppDelegate`, überschreiben die `FinishedLaunching` Methode, um das Intervall für die minimale Fetch festgelegt. In diesem Beispiel können wir das Betriebssystem entscheiden, wie häufig neue Inhalte abgerufen werden sollen:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Führen Sie schließlich die Fetch durch Überschreiben der `PerformFetch` Methode in der `AppDelegate`, und übergibt eine *vervollständigungshandler*. Der Abschlusshandler ist ein Delegat, der akzeptiert eine `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Wenn wir aktualisieren Inhalt fertig sind, können wir das Betriebssystem, die durch Aufrufen der Abschlusshandler mit den entsprechenden Status kennen. iOS bietet drei Optionen für die Handler Abschlussstatus:

1.  `UIBackgroundFetchResult.NewData` -Wird aufgerufen, wenn neuer Inhalte abgerufen wurden, und die Anwendung aktualisiert wurde.
1.  `UIBackgroundFetchResult.NoData` -Wird aufgerufen, wenn gab es das Abrufen von Daten für den neuen Inhalt, aber es kein Inhalt verfügbar ist.
1.  `UIBackgroundFetchResult.Failed` -Nützlich für die Fehlerbehandlung, ist dies aufgerufen, wenn der Abruf durchlaufen wurde.


Anwendungen, die mit Fetch Hintergrund können Aufrufe an die Benutzeroberfläche von Hintergrund aktualisieren vornehmen. Wenn der Benutzer die Anwendung öffnet, werden die Benutzeroberfläche bis zum Datum und der neue Inhalt anzuzeigen. Die Anwendung App Switcher Momentaufnahme werden auch aktualisiert, damit der Benutzer sehen kann, wenn die Anwendung neuen Inhalte aufweist.

> [!IMPORTANT]
> Einmal `PerformFetch` wird aufgerufen, die Anwendung muss ungefähr 30 Sekunden Kickoff Download neuer Inhalte, und rufen den Handlerblock Abschluss. Wenn diese zu lange dauert, wird die app beendet. Erwägen Sie Hintergrund abgerufen, mit der _Hintergrundübertragungsdienst_ beim Herunterladen von Medien oder anderen großen Dateien.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

Im obigen Beispielcode können wir das Betriebssystem entscheiden, wie häufig neue Inhalte abgerufen werden sollen, durch Festlegen des Intervalls für die minimale Fetch auf `BackgroundFetchIntervalMinimum`. iOS bietet drei Optionen für das Intervall abrufen:

1.  `BackgroundFetchIntervalNever` -Teilen des Systems nie neue Inhalte abgerufen werden sollen. Verwenden Sie diese Option deaktivieren, die abrufen in bestimmten Situationen, z. B. wenn der Benutzer nicht angemeldet ist. Dies ist der Standardwert für die Fetch-Intervall. 
1.  `BackgroundFetchIntervalMinimum` : Zulassen, dass das System entscheiden, wie oft die fetch basierend auf Benutzermuster, Akkulaufzeit, Datennutzung und die Anforderungen von anderen Anwendungen.
1.  `BackgroundFetchIntervalCustom` – Wenn Sie wissen, wie oft der Inhalt einer Anwendung aktualisiert wird, können Sie angeben "Ruheintervall" nach jeder Fetch währenddessen wird die Anwendung daran gehindert werden neuen Inhalte abrufen. Sobald dieses Intervall aktiviert ist, bestimmt das System beim Inhalte abgerufen werden sollen.


Beide `BackgroundFetchIntervalMinimum` und `BackgroundFetchIntervalCustom` basieren auf dem System um Abrufvorgänge zu planen. Dieses Intervall ist dynamisch, zur Anpassung an das Gerät muss als auch die einzelnen Benutzer Praktiken. So ist z. B. wenn ein Benutzer eine Anwendung jede Nacht überprüft, und eine andere Überprüfungen stündlich iOS sicher, des Inhalts dass stellt auf dem neuesten Stand für beide Benutzer jedes Mal, wenn die Anwendung zu öffnen.

Abrufen im Hintergrund sollte für Anwendungen verwendet werden, die häufig mit unkritische Inhalt aktualisieren. Für Anwendungen mit wichtigen Updates sollte die Remote-Benachrichtigungen verwendet werden. Remote Benachrichtigungen basieren auf den Hintergrund abgerufen, und geben den gleichen Abschlusshandler. Wir müssen mit der Remote-Benachrichtigungen als Nächstes beschäftigen.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>Remote-Benachrichtigungen (iOS 7 und höher)

Pushbenachrichtigungen sind JSON-Nachrichten, die von einem Anbieter gesendet wird, auf einem Gerät über die *Apple Push Notification Service (APNs)*.

In iOS 6 eine eingehende Pushbenachrichtigungen weist auf das System die Benachrichtigung des Benutzers, die etwas Interessantes in einer Anwendung erfolgt ist. Durch Klicken auf die Benachrichtigung, die Anwendung beendet den Zustand angehalten oder beendet abruft, und die app würde beginnen Inhalt aktualisieren. iOS 7 (und höher) erweitert gewöhnliche Pushbenachrichtigungen durch die Vergabe von Anwendungen eine Möglichkeit, Inhalte im Hintergrund aktualisieren *vor* Benachrichtigen des Benutzers, damit der Benutzer die Anwendung öffnen kann und mit neuem Inhalt angezeigt sofort.

Bearbeiten Sie zum remote-Benachrichtigungen zu implementieren, *"Info.plist"* und überprüfen Sie die **Aktivieren von Hintergrundmodi** und **Remote Benachrichtigungen** Kontrollkästchen:

 [![](updating-an-application-in-the-background-images/remote.png "Hintergrundmodus legen Sie zum Aktivieren von Hintergrundmodi und Remote-Benachrichtigungen")](updating-an-application-in-the-background-images/remote.png#lightbox)

Legen Sie anschließend die `content-available` Flag für die Pushbenachrichtigung selbst auf 1. Dies ermöglicht die Anwendung neue Inhalte abgerufen werden sollen, ehe Sie die Warnung kennen:

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

In der *AppDelegate*, überschreiben die `DidReceiveRemoteNotification` Methode, um die benachrichtigungsnutzlast nach verfügbaren Inhalten zu überprüfen, und rufen den entsprechenden Abschluss Handlerblock:

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Remote-Benachrichtigungen auf unregelmäßiger Updates mit Inhalt vorgesehen, die für die Funktionalität der Anwendung von entscheidender Bedeutung ist. Weitere Informationen zu remote Benachrichtigungen, finden Sie unter den Xamarin [Pushbenachrichtigungen in iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) Handbuch.

> [!IMPORTANT]
> Da der Aktualisierungsmechanismus in Remote-Benachrichtigungen auf Hintergrund Fetch basiert, die Anwendung muss Kickoff Download neuer Inhalte und rufen Sie den Handlerblock Abschluss innerhalb von 30 Sekunden empfangen die Benachrichtigung oder iOS wird die Anwendung beendet. Betrachten Sie die Kopplung Remote Benachrichtigungen mit _Hintergrundübertragungsdienst_ beim Herunterladen von Medien oder anderen großen Dateien im Hintergrund.


### <a name="silent-remote-notifications"></a>Automatische Remote-Benachrichtigungen

Remote-Benachrichtigungen sind eine einfache Möglichkeit, benachrichtigen Sie die Anwendung von Updates und Kickoff neuen Inhalte abrufen, aber es gibt Fälle, in denen brauchen wir den Benutzer benachrichtigen, den dass etwas geändert hat. Wenn ein Benutzer eine Datei für den wird synchronisiert gekennzeichnet wird, müssen wir sie zu benachrichtigen, jedes Mal, wenn die Datei aktualisiert. Datei wird synchronisiert, ist kein Ereignis überraschend, noch ist Eingreifen des Benutzers erforderlich. Benutzer erwarten, dass nur die Datei auf dem neuesten Stand sein, wenn sie es zu öffnen.

Bei Fällen wie die oben stehende ermöglicht iOS Pushbenachrichtigungen im Hintergrund - d. h., ohne eine Warnung gesendet werden. Um eine reguläre Benachrichtigung in einem unbeaufsichtigten zu aktivieren, müssen Sie die Warnung einfach aus der benachrichtigungsnutzlast entfernt:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Begrenzung der Bandbreite

Der größte Unterschied zwischen den normalen und automatische Benachrichtigungen aus Sicht der Entwickler ist, dass automatische Pushvorgänge Rate begrenzt sind. APNs werden die Übermittlung im Hintergrund Push an das Gerät verzögert, wenn die Push-Rate zu hoch abruft. Dadurch wird sichergestellt, dass Anwendungen Geräteressourcen mit zu viele automatische Benachrichtigungen ausgleichen nicht.

Allerdings können APNs automatische Benachrichtigungen "Programme", zusammen mit einer normalen Remote Benachrichtigung oder Keep-alive-Antwort. Da reguläre Benachrichtigungen nicht Rate beschränkt sind, können sie gespeicherte, um automatische Benachrichtigungen vom APNs an das Gerät mithilfe von Push übertragen verwendet werden, wie das folgende Diagramm veranschaulicht:

 [![](updating-an-application-in-the-background-images/silent.png "Reguläre Benachrichtigungen können gespeicherte automatische Benachrichtigungen vom APNs an das Gerät mithilfe von Push übertragen verwendet werden, wie in diesem Diagramm veranschaulicht")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple vertraut zu machen, Entwicklern das automatische Pushbenachrichtigungen zu senden, wenn die Anwendung erfordert, und lassen den APNs ihre Bereitstellung planen.


In diesem Abschnitt haben wir die verschiedenen Optionen für das Aktualisieren von Inhalt im Hintergrund zum Ausführen von Aufgaben, die nicht passen in einer Kategorie Hintergrund erforderlichen behandelt. Jetzt sehen wir uns einige dieser APIs in Aktion.

 [Weiter: Teil 4 – iOS Backgrounding Exemplarische Vorgehensweisen](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
