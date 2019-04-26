---
title: Daten und Clouddienste in Xamarin.iOS-Apps
description: Dieses Dokument enthält Links zu Leitfäden, die Arbeit mit lokalen Daten, iCloud und CloudKit in einer Xamarin.iOS-app beschrieben.
ms.prod: xamarin
ms.assetid: 945719F7-7CE6-4207-BF0F-23195125FC84
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 06/13/2017
ms.openlocfilehash: 21a6c1c0c0ceb5596a056f0818dec39041808504
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218244"
---
# <a name="data-and-cloud-services-in-xamarinios-apps"></a>Daten und Clouddienste in Xamarin.iOS-Apps

##  <a name="data-accessiosdata-clouddataindexmd"></a>[Datenzugriff](~/ios/data-cloud/data/index.md)

Die meisten Anwendungen müssen einige Anforderungen, um Daten auf dem Gerät lokal zu speichern. Wenn die Menge der Daten unbedeutend klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS enthält die Sqlite-Datenbank-Engine, die "integriert", und Zugriff auf die Daten durch Xamarin Plattform mit SQLite-Anbieter Daten vereinfacht.

##  <a name="icloudiosdata-cloudintroduction-to-icloudmd"></a>[iCloud](~/ios/data-cloud/introduction-to-icloud.md)

Die iCloud-Speicher-API unter iOS 5 kann Benutzerdokumente und anwendungsspezifische Daten an einem zentralen Ort speichern und diese Elemente über Geräte des Benutzers zugreifen.

##  <a name="cloudkitiosdata-cloudintro-to-cloudkitmd"></a>[CloudKit](~/ios/data-cloud/intro-to-cloudkit.md)

Das CloudKit-Framework wird der Entwicklung von Anwendungen, iCloud Zugriff optimiert. Dies schließt den Abruf von Anwendungsdaten und Asset Rechte als auch Möglichkeit zum sicheren Speichern von Anwendungsinformationen. Dieses Kit, erhalten Benutzer eine Ebene der Anonymität durch Gewähren des Zugriffs auf Anwendungen mit ihren iCloud IDs ohne Weitergabe persönlicher Informationen.