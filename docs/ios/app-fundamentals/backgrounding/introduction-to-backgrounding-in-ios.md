---
title: Einführung in die Hintergrundverarbeitung in iOS
description: 'Dieses Dokument beschreibt die hintergrundverarbeitung in iOS: Anwendungszustand, Application Lifecycle-Methoden und Aktualisierung im Hintergrund-app.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 07/24/2018
ms.openlocfilehash: c533dd54e3b6b11465cfd7daf5b9a93265dbe7b7
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119058"
---
# <a name="introduction-to-backgrounding-in-ios"></a>Einführung in die Hintergrundverarbeitung in iOS

iOS hintergrundverarbeitung streng reguliert und bietet drei Ansätze, um es zu implementieren:

-  **Eine Hintergrundaufgabe registrieren** – Wenn eine Anwendung eine wichtige Aufgabe abzuschließen muss, können sie iOS nicht für die Aufgabe unterbrochen wird, in den Hintergrund wechselt die Anwendung aufgefordert. Beispielsweise kann eine Anwendung müssen Fertig stellen, Anmelden eines Benutzers aus, oder Laden einer großen Datei abgeschlossen.
-  **Registrieren als Hintergrund-Bedarf-Anwendung** – Registrieren einer app kann als eine bestimmte Art von Anwendung, von denen bekannt ist, bestimmte Anforderungen für die hintergrundverarbeitung, z. B. *Audio* , *VoIP* ,  *Externes Zubehör* , *Zeitungskiosk* , und *Speicherort* . Diese Anwendungen sind kontinuierliche hintergrundverarbeitung Berechtigungen so lange sie Vorgänge ausführen, die innerhalb der Parameter des registrierten Anwendungstyps sind zulässig.
-  **Hintergrund-Updates aktivieren** -Anwendungen können die Aktualisierung im Hintergrund mit auslösen *Region Überwachung* oder durch Lauschen auf *erhebliche Standortänderungen* . Ab iOS 7, Anwendungen können auch registrieren, um im Hintergrund mit update *abrufen im Hintergrund* oder *Remotebenachrichtigungen* .


## <a name="application-states-and-application-delegate-methods"></a>Status der Anwendung und Stellvertretermethoden Anwendung

Bevor wir in den Code für die hintergrundverarbeitung in iOS beschäftigen, müssen wir wie hintergrundverarbeitung wirkt sich auf den Lebenszyklus einer iOS-Anwendung zu verstehen.

Der Lebenszyklus der iOS-Anwendung ist eine Sammlung von Anwendungszustand und Methoden zum Navigieren zwischen ihnen. Eine Anwendung Übergänge zwischen Zuständen, die basierend auf das Verhalten des Benutzers und die backgrounding Anforderungen der Anwendung. Die Bewegung wird das folgende Diagramm veranschaulicht:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Status der Anwendung und Delegaten Anwendungsmethoden-Diagramm")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Nicht ausgeführt.** -Anwendung wurde noch nicht auf dem Gerät gestartet wurde.
-  **Ausführung/aktiv-** – die Anwendung auf dem Bildschirm, und Code im Vordergrund ausgeführt wird.
-  **Inaktive** -durch einen eingehenden Telefonanruf, Text oder andere Unterbrechungen die Anwendung unterbrochen wird.
-  **In diesem Fall** -Anwendung in den Hintergrund verschoben und setzt die Ausführung Background-Code.
-  **Suspended** : Wenn die Anwendung keinen Code im Hintergrund ausgeführt, oder wenn der gesamte Code abgeschlossen wurde, kann die Anwendung kann *Suspended* vom Betriebssystem. Ein angehaltener Anwendung Prozess bleibt aktiv, aber die Anwendung zum Ausführen von Code in diesem Zustand nicht möglich ist.
-  **Zurückgeben, nicht ausgeführt wird bzw. der Beendigung (seltene)** – in einigen Fällen der Anwendungsprozess wird zerstört, und die Anwendung gibt, an die *nicht ausgeführt.* Zustand. Dies geschieht im Speicher knapp ist, oder wenn der Benutzer die Anwendung manuell beendet wird.


Seit der Einführung der Unterstützung von Multitasking wird iOS wird nur selten im Leerlauf befindliche Anwendungen beendet und stattdessen bleibt ihre Prozesse *Suspended* im Arbeitsspeicher. Wie eine Anwendung beibehalten wird sichergestellt, dass die Anwendung schnell auf das nächste Mal startet, das der Benutzer sie öffnet. Es bedeutet auch, dass Anwendungen können frei verschieben, aus der *Suspended* Status wieder der *Backgrounded* Zustand ohne zu zeichnen, Systemressourcen verfügbar sind. iOS 7 nutzt dieses Feature mit neuen APIs, mit denen Anwendungen Aufgaben im Hintergrund angehalten, wenn das Gerät wird in den Ruhezustand und Update von Inhalten direkt aus dem Hintergrund ohne Eingreifen des Benutzers und vieles mehr. Wir werden die neuen APIs im behandeln [iOS-Hintergrundverarbeitung Techniken](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Application Lifecycle-Methoden

Wenn eine app Zustand ändert, benachrichtigt iOS die Anwendung über Event-Methoden in der `AppDelegate` Klasse:

-  `OnActivated` – Dies ist zum ersten Mal aufgerufen die Anwendung gestartet wird, und jedes Mal, wenn die app im Vordergrund wieder verfügbar ist. Dies ist der Ort, an den Code einfügen, die jedes Mal ausgeführt, die app geöffnet wird.
-  `OnResignActivation` – Wenn der Benutzer eine Unterbrechung, z. B. eine Text- oder den Telefonanruf empfängt, diese Methode wird aufgerufen, und die app ist vorübergehend deaktiviert. Der Benutzer den Telefonanruf akzeptieren soll, wird die app in den Hintergrund gesendet.
-  `DidEnterBackground` -Diese Methode ermöglicht wird aufgerufen, wenn die app den backgrounded wechselt, es einer Anwendung ungefähr fünf Sekunden zur Vorbereitung der mögliche Beendigung. Verwenden Sie dieses Mal zum Speichern von Benutzerdaten und Aufgaben und Entfernen von vertraulichen Informationen auf dem Bildschirm.
-  `WillEnterForeground` – Wenn ein Benutzer gibt Sie zurück zu einer Anwendung in diesem Fall oder angehalten und startet Sie es in den Vordergrund geholt `WillEnterForeground` aufgerufen wird. Dies ist die Zeit zum Vorbereiten der app, im Vordergrund durch Aktivieren von einem beliebigen Zustand gespeichert, während der `DidEnterBackground` .  `OnActivated` wird unmittelbar nach Abschluss dieser Methode wird aufgerufen.
-  `WillTerminate` – Die Anwendung wird heruntergefahren, und ein Prozess zerstört wird. Diese Methode wird nur aufgerufen, wenn Multitasking nicht auf dem Gerät oder die Betriebssystemversion aufweist, verfügbar ist, wenn der Arbeitsspeicher zu gering ist oder wenn der Benutzer manuell eine backgrounded Anwendung beendet wird. Beachten Sie, dass angehaltene Anwendungen, die beendet nicht angerufen `WillTerminate` .


Das folgende Diagramm veranschaulicht, wie die Anwendung gibt an, und Lebenszyklusmethoden Komponenten arbeiten folgendermaßen zusammen:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Dieses Diagramm veranschaulicht, wie die Anwendung gibt und Lebenszyklusmethoden zusammenarbeiten")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Benutzersteuerelemente für die Hintergrundverarbeitung in iOS

iOS 7 wurden zahlreiche Funktionen, die Benutzer mehr Kontrolle über den Zustand einer Anwendung backgrounded haben eingeführt. Sowohl die Aktualisierung im Hintergrund-App-Einstellung als auch der App-Switcher Auswirkungen auf den Lebenszyklus der Anwendung.

### <a name="app-switcher"></a>App-Switcher

Der App-Switcher ist eine wichtige Steuerelement-Funktion, die in iOS 7 eingeführt wurden. Er wird gestartet, Doppeltippen der **Startseite** Schaltfläche, und zeigt die Anwendungen, deren Prozesse aktiv sind:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Verschieben zwischen apps, die mithilfe der App wechseln")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Mithilfe der App wechseln, können Benutzer über Momentaufnahmen aller Anwendungen, in diesem Fall und angehaltene scrollen. Durch Tippen auf eine Anwendung, wird es in den Vordergrund gestartet. Streichen, wird die Anwendung im Hintergrund, und Beenden des Prozesses entfernt. Es dauert einen genaueren Blick auf die App-Switcher in die [iOS-Demo zum Anwendungslebenszyklus](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) im nächsten Abschnitt.

> [!IMPORTANT]
> Der App-Switcher werden einen Unterschied zwischen den in diesem Fall und angehaltene Anwendungen nicht angezeigt werden.



### <a name="background-app-refresh-settings"></a>Hintergrund-Einstellungen für App-Aktualisierung

iOS 7 wird erhöht, dass Benutzer die Steuerung über den Lebenszyklus der Anwendung Benutzer können sich gegen die hintergrundverarbeitung für Anwendungen entscheiden, [für die Verarbeitung im Hintergrund registriert](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Dies verhindert nicht, dass Anwendungen über die Ausführung von Hintergrundaufgaben*.

Benutzer können diese Einstellung ändern, indem Sie die Navigation zu <span class="uiitem">Einstellungen > Allgemein > Aktualisierung im Hintergrund App</span> und bearbeiten die backgrounding Berechtigungen für eine ausgewählte Anwendung. Wenn App Aktualisierung im Hintergrund auf off gesetzt ist, wird die Anwendung sofort nach Eingabe des Hintergrunds angehalten und Hintergrund Verarbeitung verhindert werden:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Hintergrund-Einstellungen für App-Aktualisierung")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Entwickler können den Status des Hintergrundanwendung Aktualisieren mit überprüfen die `BackgroundRefreshStatus` API. Ein Beispiel finden Sie in der [überprüfen Hintergrund aktualisieren Einstellung Rezept](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting).

Wir haben die Grundlagen der iOS-Lebenszyklus von Anwendungen und Funktionen zum Steuern des Lebenszyklus der Anwendung behandelt. Als Nächstes sehen wir uns an den iOS-Lebenszyklus der Anwendung in Aktion.

