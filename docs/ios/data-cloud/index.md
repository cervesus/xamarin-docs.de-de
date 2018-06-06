---
title: Daten und Cloud-Dienste in Xamarin.iOS-Apps
description: Dieses enthält Dokumentenlinks zu Anleitungen, die zum Arbeiten mit lokalen Daten, iCloud und CloudKit in einer app Xamarin.iOS beschreiben.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 06/13/2017
ms.openlocfilehash: a733c1af34b577786a7e18eeafa13da4327dddc6
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/05/2018
ms.locfileid: "34784259"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Daten und Cloud-Dienste in Xamarin.iOS-Apps

##  <a name="data-accessiosdata-clouddataindexmd"></a>[Datenzugriff](~/ios/data-cloud/data/index.md)

Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS hat die Sqlite-Datenbank-Engine "integriert", und Zugriff auf die Daten durch die Xamarin Plattform dies mit dem Datenanbieter SQLite weitestgehend vereinfacht.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Die iCloud-Speicher-API in Ios5 kann Anwendungen Benutzerdokumente und anwendungsspezifische Daten an einem zentralen Ort speichern und diese Elemente von Geräten des Benutzers zugreifen.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Das Framework CloudKit optimiert der Entwicklung von Anwendungen, Zugriff iCloud. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch die Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit bietet Benutzern eine Ebene Anonymität durch Zulassen des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne die persönlichen Informationen gemeinsam nutzen.