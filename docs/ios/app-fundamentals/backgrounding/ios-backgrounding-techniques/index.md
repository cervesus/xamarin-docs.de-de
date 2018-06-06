---
title: iOS Backgrounding Techniken
description: 'Dieses Dokument enthält links zu Anleitungen, die verschiedene backgrounding Techniken in iOS beschrieben: Hintergrundaufgaben, Hintergrundübertragungsdienst abrufen im Hintergrund und remote-Benachrichtigungen.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ebf3c07a319a79994093f89f8e54f4cba7402533
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783754"
---
# <a name="ios-backgrounding-techniques"></a>iOS Backgrounding Techniken

In den folgenden Abschnitten werden die folgenden iOS-Funktionen zusammen mit den vorhandenen backgrounding Optionen untersucht:

-  [Opportunistische Hintergrundaufgaben](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -Akkulebensdauer durch Ausführen von Hintergrundaufgaben in opportunistische Segmenten, wenn das Gerät für die weitere Verarbeitung aktiv wird.
-  [Im Hintergrund ist im Datenübertragungsdienst](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) - zuverlässig hochladen und Herunterladen von Dateien unabhängig von der Netzwerkgröße Status oder Datei.
-  [Abrufen von Daten im Hintergrund](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -Aktualisieren von einer Anwendung über den Hintergrund in Intervallen System bestimmt.
-  [Remote Benachrichtigungen](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -Verwendung Pushbenachrichtigungen Inhaltsupdates im Hintergrund ausgelöst, bevor der Benutzer geöffnet wird, wird die Anwendung mit einer Option Benutzer benachrichtigen, oder im Hintergrund aktualisiert werden.
-  **UI-Updates im Hintergrund** – Vorbereiten der Benutzeroberfläche die Anwendung für den Benutzer, und aktualisieren Sie die Anwendung Momentaufnahme, über den Hintergrund.
