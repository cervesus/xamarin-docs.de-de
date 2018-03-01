---
title: Daten und Cloud-Dienste
description: "Leitfäden zu Stabilisierung und Bereitstellung"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: 5814936289164f14a07b33c219ad1fd01b00473b
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="data-and-cloud-services"></a>Daten und Cloud-Dienste


##  <a name="data-accessiosdata-clouddataindexmd"></a>[Datenzugriff](~/ios/data-cloud/data/index.md)

Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS hat die Sqlite-Datenbank-Engine "integriert", und Zugriff auf die Daten durch die Xamarin Plattform dies mit dem Datenanbieter SQLite weitestgehend vereinfacht.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Die iCloud-Speicher-API in Ios5 kann Anwendungen Benutzerdokumente und anwendungsspezifische Daten an einem zentralen Ort speichern und diese Elemente von Geräten des Benutzers zugreifen.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Das Framework CloudKit optimiert der Entwicklung von Anwendungen, Zugriff iCloud. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch die Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit bietet Benutzern eine Ebene Anonymität durch Zulassen des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne die persönlichen Informationen gemeinsam nutzen.