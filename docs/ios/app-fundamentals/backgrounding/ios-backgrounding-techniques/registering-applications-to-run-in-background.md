---
title: Registrieren von Xamarin.iOS-Apps im Hintergrund ausführen
description: In diesem Dokument wird beschrieben, wie zum Registrieren einer Xamarin.iOS-Anwendung im Hintergrund ausgeführt wird. Es wird erläutert, Audio-apps, VoIP-apps, externes Zubehör und Bluetooth und mehr.
ms.prod: xamarin
ms.assetid: 8F89BE63-DDB5-4740-A69D-F60AEB21150D
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: a0a66571d0249ef6fd65ff382f14c38f48a8af37
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61393701"
---
# <a name="registering-xamarinios-apps-to-run-in-the-background"></a>Registrieren von Xamarin.iOS-Apps im Hintergrund ausführen

Registrieren Sie einzelne Aufgaben für Hintergrund Berechtigungen funktioniert für einige Anwendungen, aber was geschieht, wenn eine Anwendung ständig aufgerufen wird, um wichtige, langer Aufgaben wie das Anfordern einer Wegbeschreibung für den Benutzer über GPS ausführen? Solchen Anwendungen sollte stattdessen als bekannte Hintergrund-erforderlichen Anwendungen registriert werden.

Registrieren einer app für iOS-signalisiert, dass die Anwendung spezielle Berechtigungen zum Durchführen der Aufgaben im Hintergrund zugewiesen werden soll.

## <a name="application-registration-categories"></a>Anwendungskategorien-Registrierung

Registrierte apps können in verschiedene Kategorien fallen:

-  **Audio** -Musik-Player und andere Anwendungen, die mit Audioinhalte funktionieren möglicherweise registriert werden, um weiterhin die Wiedergabe von Audiodaten auch wenn die app nicht mehr im Vordergrund ist. Wenn eine app in dieser Kategorie versucht, die nichts weiter tun, als die Wiedergabe einer Audiodatei oder Download im Hintergrund, wird es iOS beendet werden.
-  **VoIP** -Sprache über Internet Protocol (VoIP)-Anwendungen zu erhalten, der den gleichen Berechtigungen, die audio-Anwendungen, die Audio im Hintergrund weiterverarbeiten gewährt. Sie dürfen auch nach Bedarf auf die VoIP-Dienste zu reagieren, die power, um ihre Verbindungen aufrechtzuerhalten.
-  **Externes Zubehör und Bluetooth-** -Registrierung unter dieser Kategorien für Anwendungen, die für die Kommunikation mit Bluetooth-Geräten und anderen externen Hardwarezubehör reserviert, ermöglicht es der app in Verbindung mit der Hardware zu bleiben.
-  **Zeitungskiosk** -ein aus dem Zeitungskiosk-Anwendung kann weiterhin Inhalte im Hintergrund zu synchronisieren.
-  **Speicherort** - Anwendungen nutzen von GPS oder Netzwerkdaten senden und Empfangen von speicherortaktualisierungen im Hintergrund werden können.
-  **FETCH (iOS 7 und höher)** – eine Anwendung, die registriert wird, für die Fetch-Berechtigungen Hintergrund einen Anbieter für den neuen Inhalt überprüfen können, in regelmäßigen Abständen, die den Benutzer mit aktualisiertem Inhalt darstellen, wenn sie an die Anwendung zurückgegeben.
-  **Remotebenachrichtigungen (iOS 7 und höher)** -Anwendungen können für die Benachrichtigungen von einem Anbieter registrieren und verwenden Sie die Benachrichtigung, um die ein Update zu starten, bevor der Benutzer die Anwendung wird geöffnet. Benachrichtigungen können kommt in Form von Pushbenachrichtigungen, oder Sie angeben, dass um die Anwendung im Hintergrund zu aktivieren.


Anwendungen können registriert werden, durch Festlegen der **erforderliche Hintergrundmodi** -Eigenschaft in der Anwendung *"Info.plist"*. In so viele Kategorien, da es erfordert, kann eine Anwendung registrieren:

 [![](registering-applications-to-run-in-background-images/bgmodes.png "Festlegen der hintergrundmodi")](registering-applications-to-run-in-background-images/bgmodes.png#lightbox)

Eine schrittweise Anleitung zum Registrieren einer Anwendung für die Aktualisierung im Hintergrund Speicherort, finden Sie unter den [Background-Position Exemplarische Vorgehensweise](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/location-walkthrough.md).

## <a name="application-does-not-run-in-background-property"></a>Anwendung wird nicht in der Background-Eigenschaft ausgeführt werden.

Eine andere Eigenschaft, die festgelegt werden kann, in *"Info.plist"* ist die *Anwendung kann nicht ausgeführt, im Hintergrund*, oder `UIApplicationExitsOnSuspend` Eigenschaft:

 [![](registering-applications-to-run-in-background-images/plist.png "Deaktivieren von Hintergrund ausführen")](registering-applications-to-run-in-background-images/plist.png#lightbox)

Dies ist genaue die gleiche Wirkung wie das Festlegen der Aktualisierung im Hintergrund-App-Einstellung Off in iOS 7 und höher, außer es nur auf der Seite für die Entwickler geändert werden kann, und ist für iOS 4 und höher verfügbar. Die Anwendung sofort nach der Eingabe von Hintergrund angehalten, und wird nicht in der Lage, Verarbeitungsvorgänge ausführen.

Verwenden Sie diese Eigenschaft aus, wenn Ihre Anwendung nicht, helfen Ihnen, vermeiden von unerwartetem Verhalten behandelt hintergrundverarbeitung, konzipiert ist.

## <a name="background-fetch-and-remote-notifications"></a>Abrufen im Hintergrund und Remotebenachrichtigungen

Abrufen im Hintergrund und remotebenachrichtigungen sind besondere Registrierung-Kategorien, die in iOS 7 eingeführt wurden. Diese Kategorien können Anwendungen zum neuen Inhalt von einem Anbieter empfangen und im Hintergrund aktualisieren. Im nächste Abschnitt untersucht FETCH- und remotebenachrichtigungen ausführlicher und führt außerdem Netzwerkadressinformationen als Mittel zum Aktualisieren einer Anwendung im Hintergrund unter iOS 6.
