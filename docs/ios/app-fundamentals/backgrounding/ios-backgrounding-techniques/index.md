---
title: iOS-Hintergrundverarbeitung Techniken
description: 'In diesem Dokument verknüpft werden, in den Anleitungen, die beschreiben, verschiedene backgrounding Techniken in iOS: Aufgaben im Hintergrund, übertragungsdiensts im Hintergrund, Abrufen im Hintergrund und remotebenachrichtigungen.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/18/2017
ms.openlocfilehash: 00cca0c75cc79858f6edda5d6fb954611d81161b
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119740"
---
# <a name="ios-backgrounding-techniques"></a>iOS-Hintergrundverarbeitung Techniken

In den folgenden Abschnitten wird die folgenden iOS-Funktionen zusammen mit den vorhandenen backgrounding Optionen vorgestellt:

-  [Opportunistische Hintergrundaufgaben](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -Akkulebensdauer durch Ausführen von Hintergrundaufgaben in Blöcken von opportunistische, wenn das Gerät für die weitere Verarbeitung aktiv wird.
-  [Im Hintergrund Datenübertragungsdienst](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) – hoch- und Herunterladen von Dateien unabhängig von Netzwerk-Status oder Dateigröße.
-  [Abrufen im Hintergrund](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) – aktualisieren eine Anwendung im Hintergrund in Intervallen bestimmt.
-  [Remotebenachrichtigungen](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -Push-Benachrichtigungen zum Auslösen von Inhaltsupdates im Hintergrund, bevor der Benutzer öffnet die Anwendung mit einer Option zum Benachrichtigen des Benutzers oder im Hintergrund aktualisieren verwenden.
-  **Aktualisierungen der Benutzeroberfläche im Hintergrund** : Vorbereiten der Benutzeroberfläche die Anwendung für den Benutzer, und Aktualisieren der Anwendung Momentaufnahme, aus der Hintergrund.
