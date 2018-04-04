---
title: Einführung in Backgrounding in iOS
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 3a82a34a37e53e0c6922ef47717a4100576c2277
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-backgrounding-in-ios"></a>Einführung in Backgrounding in iOS

iOS regelt die sehr eng Verarbeitung im Hintergrund, und bietet drei Ansätze, um es zu implementieren:

-  **Eine Hintergrundaufgabe registrieren** – Wenn eine Anwendung eine wichtige Aufgabe abzuschließen muss, können sie Fragen iOS verwenden, nicht um die Aufgabe unterbrochen, wenn die Anwendung in den Hintergrund bewegt wird. Beispielsweise kann eine Anwendung müssen abgeschlossen Anmelden eines Benutzers ist, oder Laden einer großen Datei abgeschlossen.
-  **Registrieren als Hintergrund erforderlichen Anwendung** – Registrieren einer app kann als eine bestimmte Art von Anwendung, von dem bekannt ist, bestimmte backgrounding Anforderungen, z. B. *Audio* , *VoIP* ,  *Externe Zubehör* , *Newsstand* , und *Speicherort* . Diese Anwendungen sind kontinuierliche hintergrundverarbeitung Berechtigungen so lange sie Tasks ausführen, die innerhalb der Parameter des Typs registrierten Anwendung sind zulässig.
-  **Hintergrund Updates aktivieren** -Anwendungen können im Hintergrund Updates mit auslösen *Region Überwachung* oder Lauschen *erhebliche Änderungen am Speicherort* . Ab iOS 7, Anwendungen können auch registrieren, um update im Hintergrund mit *Hintergrund Fetch* oder *Remote Benachrichtigungen* .


## <a name="application-states-and-application-delegate-methods"></a>Status der Anwendung und Delegaten Anwendungsmethoden

Bevor wir uns in den Code für die hintergrundverarbeitung in iOS beschäftigen, müssen wir verstehen, wie backgrounding wirkt sich auf den Lebenszyklus einer iOS-Anwendung.

Der Lebenszyklus der iOS-Anwendung ist eine Sammlung von Anwendungsstatus und Methoden zum Navigieren zwischen ihnen. Eine Anwendung Übergänge zwischen Zuständen basierend auf das Verhalten des Benutzers und die backgrounding Anforderungen der Anwendung. Die Verschiebung wird durch das folgende Diagramm veranschaulicht:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Status der Anwendung und Delegaten Anwendungsmethoden-Diagramm")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

-  **Nicht ausgeführt** -Anwendung wurde noch nicht auf dem Gerät gestartet.
-  **Wird ausgeführt/aktiv-** -die Anwendung auf dem Bildschirm, und Code im Vordergrund ausgeführt wird.
-  **Inaktive** -Anwendung durch einen eingehenden Telefonanruf, Text oder andere Unterbrechung unterbrochen wird.
-  **Backgrounded** -die Anwendung wechselt in den Hintergrund und setzt die Ausführung von Code im Hintergrund.
-  **Suspended** – Wenn die Anwendung verfügt nicht über keinen Code für die im Hintergrund ausgeführt werden oder wenn der gesamte Code abgeschlossen ist, wird die app an sich *Suspended* vom Betriebssystem. Eine angehaltene Anwendungsprozess wird beibehalten, aber die Anwendung kann nicht zum Ausführen von Code in diesem Status.
-  **Zurückgeben, nicht ausgeführt/Beendigung (seltene)** – in einigen Fällen Prozess der Anwendung zerstört wird und der Anwendung zurückgesendet werden, um die *nicht ausgeführt* Zustand. Dies geschieht in Situationen mit wenig Arbeitsspeicher, oder wenn der Benutzer die Anwendung manuell beendet.


Seit der Einführung der Unterstützung für Multitasking, iOS selten im Leerlauf Anwendungen beendet wurde, und stattdessen wird die Prozesse *Suspended* im Arbeitsspeicher. Wie eine Anwendung Prozess beibehalten wird sichergestellt, dass die Anwendung schnell wird das nächste Mal gestartet, das wird. Es bedeutet auch, dass Anwendungen können frei verschieben, von der *angehalten* Zustands zurück in die *Backgrounded* Status ohne Zeichnen auf Systemressourcen verfügbar sind. iOS 7 nutzt dieses Feature mit neuen APIs, mit denen Anwendungen Hintergrundaufgaben anhalten, wenn das Gerät wird in den Ruhezustand und Updateinhalt direkt vom Hintergrund ohne Eingreifen des Benutzers und vieles mehr. Wir befasst sich mit der neuen APIs in [iOS Backgrounding Techniken](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Application Lifecycle-Methoden

Bei einer statusänderung von eine app iOS über darüber informiert die Anwendung Ereignismethoden in die `AppDelegate` Klasse:

-  `OnActivated` -Dies heißt erstmalig die Anwendung gestartet wird, und jedes Mal, wenn die app in den Vordergrund eintrifft. Dies ist der Ort zum Code zu konzentrieren, die bei jedem der app öffnen ausgeführt werden muss.
-  `OnResignActivation` – Wenn der Benutzer eine Unterbrechung, z. B. eine Text- oder phonefactor-Anruf erhält, ruft diese Methode aufgerufen, und die app ist vorübergehend deaktiviert. Der Benutzer den Telefonanruf akzeptieren soll, wird die app auf den Hintergrund gesendet.
-  `DidEnterBackground` -Diese Methode ermöglicht wird aufgerufen, wenn die app die backgrounded Zustand wechselt, es einer Anwendung ungefähr fünf Sekunden zur Vorbereitung der möglichen beenden. Verwenden Sie dieses Mal zum Speichern von Benutzerdaten und Aufgaben und entfernen vertraulichen Informationen aus dem Bildschirm aus.
-  `WillEnterForeground` – Wenn ein Benutzer an einer Anwendung backgrounded "oder" angehalten zurückgegeben und in den Vordergrund gestartet `WillEnterForeground` aufgerufen wird. Dies ist die Zeit zum Vorbereiten der app, die Vordergrundfarbe von einem beliebigen Zustand gespeichert, während der Aktivierung `DidEnterBackground` .  `OnActivated` wird unmittelbar nach dem Abschluss dieser Methode wird aufgerufen.
-  `WillTerminate` -Die Anwendung wird heruntergefahren, und einem Prozess zerstört wird. Diese Methode ruft nur aufgerufen, wenn Multitasking nicht auf das Gerät oder die Version des Betriebssystems verfügbar ist, wenn Arbeitsspeicher zu gering ist, oder wenn der Benutzer manuell eine backgrounded Anwendung beendet. Beachten Sie, dass angehaltene Anwendungen, die beendet wird, erhalten nicht angerufen `WillTerminate` .


Das folgende Diagramm veranschaulicht, wie die Anwendung Zustände und Lebenszyklusmethoden zusammenpassen:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "Dieses Diagramm veranschaulicht, wie die Anwendung Zustände und Lebenszyklusmethoden zusammenpassen")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Benutzersteuerelemente für Backgrounding in iOS

iOS 7, um mehr Kontrolle über ein backgrounded Anwendungsstatus Benutzern mehrere Funktionen eingeführt. Der App Switcher und die App zu aktualisieren, Hintergrund-Einstellung Auswirkungen auf den Lebenszyklus der Anwendung.

### <a name="app-switcher"></a>App-Switcher

Der App Switcher ist eine wichtige Steuerelement-Funktion, die unter iOS 7 eingeführt. Er wird gestartet, Doppeltippen der **Home** aus, und zeigt die Anwendungen, deren Prozesse aktiv sind:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Verschieben zwischen apps, die mithilfe der App wechseln")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Mithilfe der App wechseln, können Benutzer über Snapshots aller backgrounded erfolgreichen und ausgesetzten Anwendungen blättern. Tippen Sie auf eine Anwendung in den Vordergrund gestartet. Streichen entfernt die Anwendung aus den Hintergrund der Prozess wird beendet. Es dauert eine genauere Betrachtung der App Switcher in der [iOS Application Lifecycle Demo](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) im nächsten Abschnitt.

> [!IMPORTANT]
> Der App Switcher werden einen Unterschied zwischen backgrounded erfolgreichen und ausgesetzten Anwendungen nicht angezeigt werden.



### <a name="background-app-refresh-settings"></a>Einstellungen für die App-Aktualisierung im Hintergrund

iOS 7 erhöht, dass Benutzerkontrolle über den Lebenszyklus der Anwendung ermöglichen, dass Benutzer für Applikationen backgrounding abwählen [zur hintergrundverarbeitung registriert](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md). *Dies verhindert nicht, dass Anwendungen, die von der Ausführung der Hintergrundaufgaben*.

Benutzer können diese Einstellung ändern, navigieren Sie zur <span class="uiitem">Einstellungen > Allgemein > App zu aktualisieren, Hintergrund</span> und backgrounding Bearbeitungsrechte für eine ausgewählte Anwendung. Wenn die App zu aktualisieren, im Hintergrund auf off festgelegt ist, wird die Anwendung sofort beim Eintritt in des Hintergrunds angehalten und sämtliche hintergrundverarbeitung gehindert werden:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Einstellungen für die App-Aktualisierung im Hintergrund")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Entwickler können den Status der Hintergrundanwendung aktualisieren Überprüfen der `BackgroundRefreshStatus` API. Ein Beispiel finden Sie in der [überprüfen im Hintergrund aktualisieren Einstellung Rezept](https://developer.xamarin.com/recipes/ios/multitasking/check_background_refresh_setting/).

Wir haben die Grundlagen der iOS-Anwendungslebenszyklus und Funktionen zur Steuerung des Lebenszyklus der Anwendung behandelt. Als Nächstes sehen wir uns die iOS-Anwendungslebenszyklus in Aktion.

