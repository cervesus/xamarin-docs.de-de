---
title: Registrieren von xamarin. IOS-apps, die im Hintergrund ausgeführt werden sollen
description: In diesem Dokument wird beschrieben, wie Sie eine xamarin. IOS-Anwendung registrieren, um Sie im Hintergrund auszuführen. Hier werden Audioanwendungen, VoIP-Apps, externes Zubehör und Bluetooth und mehr erläutert.
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: bef29bfc526a5f378368390c1ec25b1bbf1d8a5a
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/22/2020
ms.locfileid: "86932963"
---
# <a name="registering-xamarinios-apps-to-run-in-the-background"></a>Registrieren von xamarin. IOS-apps, die im Hintergrund ausgeführt werden sollen

Das Registrieren einzelner Aufgaben für Hintergrund Berechtigungen funktioniert für einige Anwendungen. Was geschieht jedoch, wenn eine Anwendung ständig aufgerufen wird, um wichtige Aufgaben mit langer Ausführungszeit auszuführen, wie z. b. das Abrufen von Anweisungen für den Benutzer über GPS? Anwendungen wie diese sollten stattdessen als bekannte Hintergrundanwendungen registriert werden.

Das Registrieren einer APP signalisiert IOS, dass der Anwendung spezielle Berechtigungen für die Ausführung von Aufgaben im Hintergrund erteilt werden sollen.

## <a name="application-registration-categories"></a>Anwendungs Registrierungs Kategorien

Registrierte Apps können in verschiedene Kategorien unterteilt werden:

- **Audiomusik** -Player und andere Anwendungen, die mit Audioinhalten arbeiten, können registriert werden, um weiterhin Audioinhalte zu spielen, auch wenn sich die APP nicht mehr im Vordergrund befindet. Wenn eine app in dieser Kategorie versucht, etwas anderes zu tun, als die Audiowiedergabe oder den Download im Hintergrund durchzuführen, wird Sie von IOS beendet.
- **VoIP-Voice** over Internet Protocol (VoIP)-Anwendungen erhalten dieselben Berechtigungen wie Audioanwendungen, um die Verarbeitung von Audiodaten im Hintergrund beizubehalten. Sie können auch nach Bedarf für die VoIP-Dienste reagieren, die Sie unterstützt, um Ihre Verbindungen aufrechtzuerhalten.
- **Externe Zubehör und Bluetooth** -reserviert für Anwendungen, die mit Bluetooth-Geräten und anderen externen Hardware Zubehör kommunizieren müssen. bei der Registrierung in diesen Kategorien kann die APP mit der Hardware verbunden bleiben.
- **NewsStand** : eine NewsStand-Anwendung kann weiterhin Inhalte im Hintergrund synchronisieren.
- **Standort** : Anwendungen, die GPS-oder Netzwerk Speicherort Daten verwenden, können Standort Aktualisierungen im Hintergrund senden und empfangen.
- **Fetch (IOS 7** und höher): eine Anwendung, die für das Abrufen von Hintergrund Berechtigungen registriert ist, kann in regelmäßigen Abständen einen Anbieter auf neue Inhalte überprüfen. dabei wird dem Benutzer der aktualisierte Inhalt angezeigt, wenn er zur Anwendung zurückkehrt.
- **Remote Benachrichtigungen (IOS 7** und höher): Anwendungen können sich für den Empfang von Benachrichtigungen von einem Anbieter registrieren und die Benachrichtigung verwenden, um ein Update zu starten, bevor der Benutzer die Anwendung öffnet. Benachrichtigungen können in Form von Pushbenachrichtigungen erfolgen, oder Sie können die Anwendung im Hintergrund aktivieren.

Anwendungen können durch Festlegen der Eigenschaft **erforderliche hintergrundmodi** in der Datei " *Info. plist*" der Anwendung registriert werden. Eine Anwendung kann in beliebig vielen Kategorien registriert werden:

 [![Festlegen der hintergrundmodi](registering-applications-to-run-in-background-images/bgmodes.png)](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

Eine Schritt-für-Schritt-Anleitung zum Registrieren einer Anwendung für Aktualisierungen im Hintergrund finden Sie unter Exemplarische Vorgehensweise für den [Hintergrund Speicherort](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md).

## <a name="application-does-not-run-in-background-property"></a>Die Anwendung wird nicht in der Background-Eigenschaft ausgeführt.

Eine andere Eigenschaft, die in " *Info. plist* " festgelegt werden kann, ist die Anwendung, die *nicht im Hintergrund ausgeführt*wird, oder die `UIApplicationExitsOnSuspend` Eigenschaft:

 [![Deaktivieren der Hintergrund Ausführung](registering-applications-to-run-in-background-images/plist.png)](registering-applications-to-run-in-background-images/plist.png#lightbox)

Dies hat die gleiche Auswirkung wie das Festlegen der Einstellung für die Aktualisierung der Hintergrund-app in ios 7 und höher, mit der Ausnahme, dass Sie nur auf der Entwicklerseite geändert werden kann und für IOS 4 und höher verfügbar ist. Die Anwendung wird sofort nach dem Eintreten in den Hintergrund angehalten und kann keine Verarbeitung durchführen.

Verwenden Sie diese Eigenschaft, wenn die Anwendung nicht für die Verarbeitung von Hintergrundverarbeitung konzipiert ist, da Sie dabei hilft, unerwartetes Verhalten zu vermeiden.

## <a name="background-fetch-and-remote-notifications"></a>Hintergrund Abruf und Remote Benachrichtigungen

Das Abrufen von Hintergrundinformationen und Remote Benachrichtigungen sind spezielle Registrierungs Kategorien, die in ios 7 eingeführt wurden. Diese Kategorien ermöglichen es Anwendungen, neuen Inhalt von einem Anbieter zu empfangen und im Hintergrund zu aktualisieren. Im nächsten Abschnitt werden Abruf-und Remote Benachrichtigungen ausführlicher erläutert. Außerdem wird die Standorterkennung als Mittel zum Aktualisieren einer Anwendung im Hintergrund unter IOS 6 vorgestellt.
