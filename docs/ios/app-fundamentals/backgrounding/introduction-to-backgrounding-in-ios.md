---
title: Einführung in die Hintergrundverarbeitung in iOS
description: 'In diesem Dokument wird die Rückstellung in ios beschrieben: Anwendungs Zustände, Anwendungslebenszyklus-Methoden und Aktualisierung der Hintergrund-app.'
ms.prod: xamarin
ms.assetid: E214F2C7-E74E-46C7-B5BA-080B30D61250
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 07/24/2018
ms.openlocfilehash: 78751f53808ffa62589fdc57fe4cf59912849e00
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010842"
---
# <a name="introduction-to-backgrounding-in-ios"></a>Einführung in die Hintergrundverarbeitung in iOS

IOS regelt die Hintergrundverarbeitung sehr eng und bietet drei Ansätze für die Implementierung:

- **Registrieren einer Hintergrundaufgabe** : Wenn eine Anwendung eine wichtige Aufgabe ausführen muss, kann Sie von IOS aufgefordert werden, die Aufgabe nicht zu unterbrechen, wenn die Anwendung in den Hintergrund wechselt. Beispielsweise muss eine Anwendung möglicherweise die Protokollierung eines Benutzers abschließen oder das Herunterladen einer großen Datei abschließen.
- **Registrieren als eine für den Hintergrund erforderliche Anwendung** : eine APP kann sich als eine bestimmte Art von Anwendung registrieren, die bekannte, spezifische Anforderungen für die zurück Setzung hat, wie z. b. *Audiodateien* , *VoIP* , *externes Zubehör* , *NewsStand* , und *Speicherort* . Diese Anwendungen sind bei der Ausführung von Aufgaben, die in den Parametern des registrierten Anwendungs Typs in den Parametern des registrierten Anwendungs Typs ausgeführt werden, von kontinuierlichen Hintergrund Verarbeitungs Berechtigungen.
- **Hintergrund Aktualisierungen aktivieren** : Anwendungen können Hintergrund Aktualisierungen mit der *Regions Überwachung* oder durch lauschen auf *bedeutende Änderungen am Standort* auslöst. Ab IOS 7 können Anwendungen auch zum Aktualisieren von Inhalten im Hintergrund mithilfe von *Hintergrund* Abruf oder *Remote Benachrichtigungen* registriert werden.

## <a name="application-states-and-application-delegate-methods"></a>Anwendungs Zustände und Anwendungs Delegatmethoden

Bevor wir uns mit dem Code für die Hintergrundverarbeitung in ios beschäftigen, müssen wir verstehen, wie sich die Rückstellung auf den Lebenszyklus einer IOS-Anwendung auswirkt.

Der Lebenszyklus der IOS-Anwendung ist eine Sammlung von Anwendungs Zuständen und-Methoden für den Wechsel zwischen diesen. Eine Anwendung wechselt zwischen Zuständen, die auf dem Verhalten des Benutzers und den Anforderungen der zurück setzung der Anwendung basieren. Die Bewegung wird anhand des folgenden Diagramms veranschaulicht:

 [![](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png "Application States and Application Delegate Methods diagram")](introduction-to-backgrounding-in-ios-images/applicationlifecycle-.png#lightbox)

- Wird **nicht ausgeführt** : die Anwendung wurde noch nicht auf dem Gerät gestartet.
- Wird **ausgeführt/aktiv** : die Anwendung befindet sich auf dem Bildschirm und führt Code im Vordergrund aus.
- **Inaktiv** : die Anwendung wird durch einen eingehenden Telefonanruf, Text oder eine andere Unterbrechung unterbrochen.
- **Kulisse** : die Anwendung wird in den Hintergrund verschoben und setzt die Ausführung von Hintergrund Code fort.
- Angeh **alten: Wenn** die Anwendung nicht über Code verfügt, der im Hintergrund ausgeführt wird, oder wenn der gesamte Code abgeschlossen ist *, wird die APP vom Betriebs* System angehalten. Der Prozess einer angehaltenen Anwendung wird aktiv gehalten, aber die Anwendung kann keinen Code in diesem Zustand ausführen.
- **Zurück zur Nichtausführung/Beendigung (selten)** : Gelegentlich wird der Prozess der Anwendung zerstört, und die Anwendung kehrt in den Zustand " *nicht ausgeführt* " zurück. Dies geschieht in Situationen mit geringem Arbeitsspeicher oder, wenn der Benutzer die Anwendung manuell beendet.

Seit der Einführung der Multitasking-Unterstützung beendet IOS selten *Anwendungen im Leerlauf* und hält Ihre Prozesse stattdessen im Speicher angehalten. Wenn Sie den Prozess einer Anwendung beibehalten, wird sichergestellt, dass die Anwendung beim nächsten Öffnen des Benutzers schnell gestartet wird. Es bedeutet auch, dass Anwendungen ohne das Zeichnen auf Systemressourcen frei vom angehaltenen *Zustand zurück in den Zustand mit* *backstem* verschoben werden können. IOS 7 nutzt dieses Feature mit neuen APIs, die es Anwendungen ermöglichen, Hintergrundaufgaben anzuhalten, wenn das Gerät in den Standbymodus wechselt, Inhalte direkt aus dem Hintergrund ohne Benutzerinteraktion aktualisieren und vieles mehr. Wir behandeln die neuen APIs in [IOS-Hintergrund Techniken](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/index.md).

## <a name="application-lifecycle-methods"></a>Anwendungslebenszyklus-Methoden

Wenn sich der Status einer APP ändert, benachrichtigt IOS die Anwendung über Ereignis Methoden in der `AppDelegate`-Klasse:

- `OnActivated`: Diese wird aufgerufen, wenn die Anwendung zum ersten Mal gestartet wird, und jedes Mal, wenn die APP wieder in den Vordergrund steht. Dies ist der Ort, an dem Code abgelegt werden muss, der jedes Mal ausgeführt werden muss, wenn die APP geöffnet wird.
- `OnResignActivation`: Wenn der Benutzer eine Unterbrechung wie z. b. einen Text oder einen Telefonanruf erhält, wird diese Methode aufgerufen, und die APP ist vorübergehend deaktiviert. Wenn der Benutzer den Telefonanruf annimmt, wird die APP an den Hintergrund gesendet.
- `DidEnterBackground` aufgerufen, wenn die app in den Zustand "Background" wechselt, gibt diese Methode eine Anwendung ungefähr fünf Sekunden an, um eine mögliche Beendigung vorzubereiten. Verwenden Sie diese Zeit zum Speichern von Benutzerdaten und-Tasks, und entfernen Sie vertrauliche Informationen auf dem Bildschirm.
- `WillEnterForeground`: Wenn ein Benutzer zu einer zurückgesetzten oder angehaltenen Anwendung zurückkehrt und ihn im Vordergrund startet, wird `WillEnterForeground` aufgerufen. Dies ist der Zeitpunkt, an dem die APP für den Vordergrund vorbereitet wird, indem Sie alle während `DidEnterBackground` gespeicherten Zustände reaktiviert.  `OnActivated` unmittelbar nach Abschluss dieser Methode aufgerufen wird.
- `WillTerminate`: die Anwendung wird heruntergefahren, und der Prozess wird zerstört. Diese Methode wird nur aufgerufen, wenn Multitasking auf dem Gerät oder der Betriebssystemversion nicht verfügbar ist, wenn der Arbeitsspeicher gering ist, oder wenn der Benutzer eine Anwendung mit umgekehrtem Arbeitsspeicher manuell beendet. Beachten Sie, dass Angehaltene Anwendungen, die beendet werden, `WillTerminate` nicht aufruft.

Im folgenden Diagramm wird veranschaulicht, wie die Anwendungs Zustände und Lebenszyklus Methoden aufeinander abgestimmt sind:

 [![](introduction-to-backgrounding-in-ios-images/image2.png "This diagram illustrates how the application states and lifecycle methods fit together")](introduction-to-backgrounding-in-ios-images/image2.png#lightbox)

## <a name="user-controls-for-backgrounding-in-ios"></a>Benutzer Steuerelemente für die Rück Setzung in ios

IOS 7 hat mehrere Features eingeführt, mit denen Benutzer eine bessere Kontrolle über den Zustand einer Anwendung haben. Sowohl der APP-Switcher als auch die Einstellung für die Aktualisierung der Hintergrund App beeinflussen den Anwendungslebenszyklus.

### <a name="app-switcher"></a>App-Switcher

Der APP-Switcher ist ein wichtiges Steuerungs Feature, das in ios 7 eingeführt wurde. Der Dienst wird durch Doppel Tippen auf die **Start** Schaltfläche gestartet und zeigt die Anwendungen an, deren Prozesse aktiv sind:

 [![](introduction-to-backgrounding-in-ios-images/app-switcher-.png "Moving between apps using the App Switcher")](introduction-to-backgrounding-in-ios-images/app-switcher-.png#lightbox)

Mithilfe der APP-Umschaltung können Benutzer durch die Momentaufnahmen aller abgedeckten und angehaltenen Anwendungen scrollen. Wenn Sie auf eine Anwendung tippen, wird Sie im Vordergrund gestartet. Durch das Schwenken wird die Anwendung aus dem Hintergrund entfernt, und der Prozess wird beendet. Im nächsten Abschnitt wird der APP-Switcher in der Demo des [IOS-Anwendungslebenszyklus](~/ios/app-fundamentals/backgrounding/application-lifecycle-demo.md) näher betrachtet.

> [!IMPORTANT]
> Der APP-Switcher zeigt keinen Unterschied zwischen umgekehrten und angehaltenen Anwendungen an.

### <a name="background-app-refresh-settings"></a>Aktualisierungs Einstellungen für die Hintergrund App

IOS 7 erhöht die Benutzerkontrolle über den Anwendungslebenszyklus, indem Benutzern ermöglicht wird, die Rückstellung für Anwendungen zu abonnieren, die [für die Hintergrundverarbeitung registriert](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)sind. *Dadurch wird nicht verhindert, dass Anwendungen Hintergrundaufgaben ausführen*.

Benutzer können diese Einstellung ändern, indem Sie zu **Einstellungen > Allgemein > Hintergrund-App-Aktualisierung** navigieren und die Sicherungs Berechtigungen für eine ausgewählte Anwendung bearbeiten. Wenn die Aktualisierung der Hintergrund-App auf OFF festgelegt ist, wird die Anwendung sofort nach der Eingabe des Hintergrunds angehalten und verhindert, dass eine Hintergrundverarbeitung durchgeführt wird:

 [![](introduction-to-backgrounding-in-ios-images/settings-.png "Background App Refresh Settings")](introduction-to-backgrounding-in-ios-images/settings-.png#lightbox)

Entwickler können den Anwendungs Status der Hintergrund Aktualisierung mit der `BackgroundRefreshStatus`-API überprüfen. Ein Beispiel finden Sie in der Anleitung zum [Überprüfen der Einstellung für die Hintergrund Aktualisierung](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/check_background_refresh_setting).

Wir haben die Grundlagen des Lebenszyklus der IOS-Anwendung und Features zum Steuern des Anwendungslebenszyklus behandelt. Als nächstes sehen wir uns den Lebenszyklus der IOS-Anwendung an.
