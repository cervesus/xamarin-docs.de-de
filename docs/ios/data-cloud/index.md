---
title: Daten und Cloud-Dienste
description: "Leitfäden zu Stabilisierung und Bereitstellung"
ms.topic: article
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: b5e250c7c505958c8293970321b6173e6086e7b1
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/12/2018
---
# <a name="data-and-cloud-services"></a>Daten und Cloud-Dienste


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Datenzugriff](~/ios/data-cloud/data/index.md)

Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS hat die Sqlite-Datenbank-Engine "integriert", und Zugriff auf die Daten durch die Xamarin Plattform dies mit dem Datenanbieter SQLite weitestgehend vereinfacht.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Die iCloud-Speicher-API in Ios5 kann Anwendungen Benutzerdokumente und anwendungsspezifische Daten an einem zentralen Ort speichern und diese Elemente von Geräten des Benutzers zugreifen.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Das Framework CloudKit optimiert der Entwicklung von Anwendungen, Zugriff iCloud. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch die Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit bietet Benutzern eine Ebene Anonymität durch Zulassen des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne die persönlichen Informationen gemeinsam nutzen.