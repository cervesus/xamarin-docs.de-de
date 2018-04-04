---
title: Daten und Cloud-Dienste
description: Leitfäden zu Stabilisierung und Bereitstellung
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 81aebe5fd7431e578b75c5b61e1d2c92ce546909
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="data-and-cloud-services"></a>Daten und Cloud-Dienste


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Datenzugriff](~/ios/data-cloud/data/index.md)

Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS hat die Sqlite-Datenbank-Engine "integriert", und Zugriff auf die Daten durch die Xamarin Plattform dies mit dem Datenanbieter SQLite weitestgehend vereinfacht.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Die iCloud-Speicher-API in Ios5 kann Anwendungen Benutzerdokumente und anwendungsspezifische Daten an einem zentralen Ort speichern und diese Elemente von Geräten des Benutzers zugreifen.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Das Framework CloudKit optimiert der Entwicklung von Anwendungen, Zugriff iCloud. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch die Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit bietet Benutzern eine Ebene Anonymität durch Zulassen des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne die persönlichen Informationen gemeinsam nutzen.