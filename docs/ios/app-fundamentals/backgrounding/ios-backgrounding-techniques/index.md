---
title: Techniken für die iOS-Hintergrundverarbeitung
description: 'Dieses Dokument ist mit Anleitungen verknüpft, die verschiedene Hintergrund Techniken in ios beschreiben: Hintergrundaufgaben, Hintergrund Übertragungs Dienst, Hintergrund Abruf und Remote Benachrichtigungen.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: a81d6a651ee980b444da19341a89206afa8c4375
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73010819"
---
# <a name="ios-backgrounding-techniques"></a>Techniken für die iOS-Hintergrundverarbeitung

In den folgenden Abschnitten werden die folgenden IOS-Features zusammen mit den vorhandenen backerden-Optionen erläutert:

- [Opportunistische Hintergrundaufgaben](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) : behalten Sie die Akku Lebensdauer bei, indem Sie Hintergrundaufgaben in opportunistischen Blöcken ausführen, wenn das Gerät für die andere Verarbeitung aktiviert ist.
- [Hintergrund Übertragungs Dienst](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) : Sie können Dateien unabhängig vom Netzwerkstatus oder der Dateigröße zuverlässig hochladen und herunterladen.
- [Hintergrundfetch](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) : Aktualisieren einer Anwendung aus dem Hintergrund in vom System festgelegten Intervallen.
- [Remote Benachrichtigungen](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) : Verwenden Sie Pushbenachrichtigungen, um Inhalts Aktualisierungen im Hintergrund zu initiieren, bevor der Benutzer die Anwendung öffnet, mit der Option, den Benutzer zu benachrichtigen oder automatisch zu aktualisieren.
- **Aktualisierungen** der Benutzeroberfläche für die Benutzeroberfläche: bereiten Sie die Benutzeroberfläche der Anwendung für den Benutzer vor, und aktualisieren Sie die Momentaufnahme der Anwendung im Hintergrund.
