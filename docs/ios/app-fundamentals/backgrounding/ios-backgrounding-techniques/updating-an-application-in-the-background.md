---
title: Aktualisieren einer Xamarin.iOS-App im Hintergrund
description: Dieses Dokument beschreibt verschiedene Möglichkeiten, eine Xamarin.iOS-app aktualisieren, die im Hintergrund, z. B. Region zu überwachen, Abrufen im Hintergrund und remotebenachrichtigungen befindet.
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 835dccaea79467582f56fd4b8b6b3b8f42acd632
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61392350"
---
# <a name="updating-a-xamarinios-app-in-the-background"></a>Aktualisieren einer Xamarin.iOS-App im Hintergrund

Aktualisierung im Hintergrund ist der Prozess der Reaktivieren einer Anwendung, die angehalten wird oder nicht ausgeführt, und es mit neuem Inhalt aktualisiert. iOS bietet drei Optionen für das Aktualisieren von Inhalt im Hintergrund:

1.  *Region Überwachung* und *erhebliche Ortsdienst Änderungen* -Ortung unterstützenden APIs Trigger Hintergrund wird basierend auf Änderungen an den Standort des Benutzers aktualisiert. Diese APIs können nur mit Bedacht verwendet werden zum Aktualisieren von Inhalt in nicht-Standort-basierten iOS 6-Anwendungen, in dem sind andere Optionen nicht verfügbar.
1.  *Fetch (iOS 7 und höher) im Hintergrund* -eine temporale Vorgehensweise aktualisieren *unkritische* Inhalt, der aktualisiert *häufig* .
1.  *Remotebenachrichtigungen (iOS 7 und höher)* -Anwendungen, die Pushbenachrichtigungen empfangen können die Benachrichtigungen verwenden, um Inhalt hintergrundaktualisierungen auslösen. Diese Methode kann verwendet werden, zum Aktualisieren mit *wichtige, zeitkritische* Inhalt, der aktualisiert *sporadisch* .


In den folgenden Abschnitten werden die Grundlagen dieser Optionen behandelt.

## <a name="region-monitoring-and-significant-location-changes"></a>Region zu überwachen und erhebliche Standortänderungen

iOS bietet zwei standortbasierte APIs mit hintergrundverarbeitung Funktionen:

1.  *Region Überwachung* ist der Prozess der Regionen hinweg einrichten und das Gerät wird aktiviert, wenn der Benutzer eingibt oder einen Codebereich beendet. Regionen sind zirkulär, und können von unterschiedlicher Größe sein. Wenn der Benutzer eine Region-Grenze überschreitet, wird das Gerät auf die Behandlung des Ereignisses, in der Regel durch das Auslösen einer benachrichtigungs oder eine Aufgabe routinesicherungen reaktiviert. Region-Überwachung erfordert GPS und erhöht die Batterie und Datennutzung.
1.  Die *erhebliche Änderungen-Standortdienst* ist eine einfachere, Strom sparen-Option für Geräte mit datenverbindungen Radios verfügbar. Eine Anwendung auf wichtige Speicherort Änderungen überwacht werden benachrichtigt, wenn das Gerät Zelle Towers wechselt. Dieser Dienst kann verwendet werden, um die Aktivierung einer Anwendung angehalten oder beendeten, und bietet die Möglichkeit, neue Inhalte im Hintergrund zu überprüfen. Hintergrundaktivität ist ca. 10 Sekunden auf, es sei denn, ein Paar mit einem [Hintergrundaufgabe](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .


Den Speicherort muss nicht von eine Anwendung können `UIBackgroundMode` diese standortbasierte APIs verwenden. Da iOS die Arten von Aufgaben nachverfolgen nicht, die ausgeführt werden kann, wenn das Gerät durch Änderungen an den Standort des Benutzers aktiviert ist, bieten diese APIs zu umgehen, zum Aktualisieren von Inhalt im Hintergrund unter iOS 6. *Beachten Sie, dass Hintergrundupdates mit standortbasierte APIs auslösen Geräteressourcen Zeichnen wird und kann Benutzer, die nicht verstehen, warum eine Anwendung Zugriff auf ihren Standort erfordert verwechseln*. Seien Sie vorsichtig bei der Implementierung von Region zu überwachen oder erhebliche Änderungen der Speicherort für die hintergrundverarbeitung in Anwendungen, die nicht bereits mit den Orts-APIs verwenden.

Apps, die mithilfe von Standort zu überwachen, für die hintergrundverarbeitung verfügbar machen, eine Schwachstelle in iOS 6: Wenn Sie nicht in einer Kategorie Hintergrund-Bedarf einer Anwendung Bedürfnissen passen, bietet nur eine begrenzte Optionen für die hintergrundverarbeitung. Mit der Einführung von zwei neue APIs *abrufen im Hintergrund* und *Remotebenachrichtigungen*, iOS 7 (und höher) Technologieprogramm hintergrundverarbeitung auf Weitere Anwendungen. In den nächsten beiden Abschnitten stellen Sie diese neuen APIs vor.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Abrufen im Hintergrund (iOS 7 und höher)

In iOS 6 benötigt eine Anwendung im Vordergrund eingeben, Zeit zum Laden neuen Inhalts, Darstellung kurz Benutzer mit Inhalten, die sie bereits gesehen haben. Abrufen im Hintergrund ermöglicht Anwendungen das Laden neuer Daten *vor* ein Benutzer startet die Anwendung, und bieten dem Benutzer die aktuellsten Inhalte.

Bearbeiten Sie zum Abrufen im Hintergrund zu implementieren, *"Info.plist"* und überprüfen Sie die **Hintergrundmodi aktivieren** und **abrufen im Hintergrund** Kontrollkästchen:

 [![](updating-an-application-in-the-background-images/fetch.png "Bearbeiten Sie die Datei \"Info.plist\" aus, und überprüfen Sie die Kontrollkästchen Hintergrundmodi aktivieren und Abrufen im Hintergrund")](updating-an-application-in-the-background-images/fetch.png#lightbox)

Als Nächstes wird in der `AppDelegate`, außer Kraft setzen der `FinishedLaunching` Methode, um das Intervall für die minimale Fetch festgelegt. In diesem Beispiel können wir das Betriebssystem entscheiden, wie häufig Sie neuen Inhalte abgerufen werden sollen:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Führen Sie zuletzt den Abruf durch Überschreiben der `PerformFetch` -Methode in der die `AppDelegate`, und übergeben Sie einen *Abschlusshandler*. Der Abschlusshandler ist ein Delegat, der akzeptiert eine `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Wenn wir die Aktualisierung von Inhalten fertig sind, können wir das Betriebssystem mit, indem Sie den Abschlusshandler mit den entsprechenden Status aufrufen. iOS bietet drei Optionen für den Abschlussstatus für Handler:

1.  `UIBackgroundFetchResult.NewData` -Wird aufgerufen, wenn neuer Inhalte abgerufen wurden, und die Anwendung aktualisiert wurde.
1.  `UIBackgroundFetchResult.NoData` -Wird aufgerufen, wenn der Abruf auf neue Inhalte durcharbeiten, aber es kein Inhalt verfügbar ist.
1.  `UIBackgroundFetchResult.Failed` – Nützlich für die Fehlerbehandlung, wird dieses aufgerufen, wenn das Abrufen nicht durchlaufen wurde.


Anwendungen mit abrufen im Hintergrund können es sich um die Aufrufe zur Aktualisierung der Benutzeroberfläche im Hintergrund ausführen. Wenn der Benutzer die app öffnet, werden die Benutzeroberfläche bis zum Datum und die neuen Inhalt anzuzeigen. Dadurch wird auch der Anwendung App-Switcher Momentaufnahme aktualisiert, damit der Benutzer sehen, wenn die Anwendung neuen Inhalte aufweist.

> [!IMPORTANT]
> Einmal `PerformFetch` wird aufgerufen, die Anwendung muss ungefähr 30 Sekunden Download des neuen Inhalts starten, und rufen die Handler Completion-Block. Wenn diese zu lange dauert, wird die app beendet. Erwägen Sie die Verwendung der abrufen im Hintergrund mit dem _Hintergrundübertragungsdienst_ beim Herunterladen von Medien oder andere großen Dateien.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

In der oben dargestellten Beispielcode können wir das Betriebssystem entscheiden, wie häufig Sie neuen Inhalte abgerufen werden sollen, durch Festlegen des Intervalls für die minimale Fetch auf `BackgroundFetchIntervalMinimum`. iOS bietet drei Optionen für das Intervall abrufen:

1.  `BackgroundFetchIntervalNever` -Teilen des Systems nie neuen Inhalte abgerufen werden sollen. Verwenden Sie diese Option deaktivieren, werden abgerufen, in bestimmten Situationen, z. B. wenn der Benutzer nicht angemeldet ist. Dies ist der Standardwert für das Intervall abrufen. 
1.  `BackgroundFetchIntervalMinimum` : Zulassen, dass das System entscheiden, wie oft Sie abgerufen werden basierend auf Benutzermuster, Akkuverbrauch gering zu halten, Verwendung und die Anforderungen von anderen Anwendungen.
1.  `BackgroundFetchIntervalCustom` – Wenn Sie wissen, wie oft der Anwendung Inhalt aktualisiert wird, können Sie angeben "Intervall" nach jeder Fetch, während die werden die Anwendung daran gehindert, neuen Inhalte abrufen. Sobald dieses Intervall aktiviert ist, bestimmt das System beim Inhalte abgerufen werden sollen.


Beide `BackgroundFetchIntervalMinimum` und `BackgroundFetchIntervalCustom` basieren auf dem System um Abrufvorgänge zu planen. Dieses Intervall ist dynamisch und an das Gerät die Anforderungen als auch der einzelnen Benutzer Gewohnheiten angepasst. So ist z. B. wenn ein Benutzer eine Anwendung jeden Morgen überprüft, und eine andere Überprüfungen einmal pro Stunde iOS sichergestellt, des Inhalts dass werden auf dem neuesten Stand für beide Benutzer jedes Mal, wenn sie die Anwendung zu öffnen.

Abrufen im Hintergrund sollte für Anwendungen verwendet werden, die häufig mit nicht kritischen Inhalt aktualisieren. Für Anwendungen mit kritischen Updates sollten Remotebenachrichtigungen verwendet werden. Remotebenachrichtigungen basieren auf Abrufen im Hintergrund, und teilen den gleichen Abschlusshandler. Wir beschäftigen wir uns in Remotebenachrichtigungen weiter.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>Remotebenachrichtigungen (iOS 7 und höher)

Pushbenachrichtigungen sind JSON-Nachrichten von einem Anbieter auf einem Gerät mit der *Apple Push Notification Service (APNs)*.

In iOS 6 eine eingehende Pushbenachrichtigungen weist auf das System den Benutzer darauf aufmerksam, den etwas Interessantes in einer Anwendung aufgetreten sind. Klicken Sie auf die Benachrichtigung auf die Anwendung aus dem Status angehalten oder beendet abruft und die app würde Aktualisieren von Inhalten beginnen. iOS 7 (und höher) erweitert die normale Pushbenachrichtigungen durch, sodass Anwendungen die Möglichkeit zum Aktualisieren von Inhalt im Hintergrund *vor* benachrichtigen den Benutzer, damit die Benutzer die Anwendung öffnen kann und mit neuem Inhalt angezeigt sofort.

Um remotebenachrichtigungen zu implementieren, bearbeiten *"Info.plist"* und überprüfen Sie die **Hintergrundmodi aktivieren** und **remotebenachrichtigungen** Kontrollkästchen:

 [![](updating-an-application-in-the-background-images/remote.png "Legen Sie den Hintergrundmodus Hintergrundmodi aktivieren und remotebenachrichtigungen")](updating-an-application-in-the-background-images/remote.png#lightbox)

Legen Sie als Nächstes die `content-available` Flag für die Pushbenachrichtigung selbst auf 1. Dadurch wird die Anwendung wissen, dass neuen Inhalte abgerufen werden sollen, bevor die Warnung angezeigt:

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

In der *AppDelegate*, überschreiben die `DidReceiveRemoteNotification` Methode zum Überprüfen Sie die Nutzlast der Benachrichtigung nach verfügbaren Inhalten, und rufen die entsprechenden Completion-Handler-Block:

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Remotebenachrichtigungen sollte nur selten Updates mit Inhalt verwendet werden, die die Funktionalität der Anwendung von entscheidender Bedeutung ist. Weitere Informationen zu remotebenachrichtigungen, finden Sie unter dem Xamarin [Pushbenachrichtigungen in iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) Guide.

> [!IMPORTANT]
> Da der Updatemechanismus in Remotebenachrichtigungen abrufen im Hintergrund basiert, muss die Anwendung starten Download neuer Inhalte und rufen den Handler Completion-Block innerhalb von 30 Sekunden der Empfang der Benachrichtigung oder iOS wird die Anwendung beendet. Betrachten Sie die Kopplung von Remotebenachrichtigungen mit _Hintergrundübertragungsdienst_ beim Herunterladen von Medien oder andere großen Dateien im Hintergrund.


### <a name="silent-remote-notifications"></a>Automatische Remotebenachrichtigungen

Remote-Benachrichtigungen sind eine einfache Möglichkeit zum Benachrichtigen von Anwendungen von Updates, und starten Sie neuen Inhalte abgerufen, aber es gibt Fälle, in denen wir müssen nicht den Benutzer zu benachrichtigen, den dass etwas geändert hat. Wenn ein Benutzer eine Datei für den wird synchronisiert gekennzeichnet wird, müssen wir beispielsweise informiert, jedes Mal, wenn die Datei aktualisiert. Datei wird synchronisiert ist nicht überraschend, noch muss Eingreifen des Benutzers. Benutzer erwarten, dass nur die Datei auf dem neuesten Stand sein, wenn sie diese öffnen.

Für eine der oben genannten Fällen können iOS-Pushbenachrichtigungen im Hintergrund – d. h. ohne eine Warnung gesendet werden. Um eine reguläre Benachrichtigung in einer automatischen aktivieren, müssen Sie die Warnung einfach aus der Nutzlast der Benachrichtigung entfernt:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Begrenzung der Bandbreite

Der größte Unterschied zwischen normalen und automatischen Benachrichtigungen aus der Perspektive eines Entwicklers ist, dass automatische Pushvorgänge Rate begrenzt sind. APNs werden verzögert die Übermittlung von automatischen pushübertragungen an das Gerät, wenn die Push-Rate zu hoch wird. Dadurch wird sichergestellt, dass Anwendungen Geräteressourcen mit zu vielen automatische Benachrichtigungen zu leeren nicht.

Allerdings können APNs automatische Benachrichtigungen "Programme" zusammen mit einer normalen Remotebenachrichtigung oder die Keep-alive-Antwort. Da reguläre Benachrichtigungen nicht Rate beschränkt sind, können sie gespeicherte, um automatische Benachrichtigungen vom APNs auf das Gerät mithilfe von Push übertragen verwendet werden, wie im folgenden Diagramm dargestellt:

 [![](updating-an-application-in-the-background-images/silent.png "Reguläre Benachrichtigungen können gespeicherte automatische Benachrichtigungen vom APNs auf das Gerät mithilfe von Push übertragen verwendet werden, wie in diesem Diagramm dargestellt")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> Apple ermöglicht es Entwicklern, automatische Pushbenachrichtigungen senden, wenn die Anwendung erfordert, und teilen den APNs, deren Bereitstellung planen.


In diesem Abschnitt haben wir verschiedene Optionen für das Aktualisieren von Inhalt im Hintergrund zu Ausführen von Aufgaben, die nicht passen, in einer Kategorie Hintergrund-erforderlichen behandelt. Nun sehen wir uns einige dieser APIs in Aktion.

 [Weiter: Teil 4 – iOS-Hintergrundverarbeitung Exemplarische Vorgehensweisen](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
