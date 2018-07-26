---
title: Einführung in die Datenspeicherung in Xamarin.iOS-Apps
description: Dieses Dokument beschreibt verschiedene Möglichkeiten der Speicherung von Daten in einer Xamarin.iOS-Anwendung, und finden Sie ausführliche Informationen zu den Vorteilen von SQLite.
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: c5324d7e7daa8fbefde1776743dbdc595463fe33
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241148"
---
# <a name="introduction-to-data-storage-in-xamarinios-apps"></a>Einführung in die Datenspeicherung in Xamarin.iOS-Apps

## <a name="when-to-use-a-database"></a>Wenn eine Datenbank verwenden

Während der Speicherung und Verarbeitung von Funktionen der mobilen Geräte zunehmen, Smartphones und Tablets weiterhin zurückfallen ihren Desktop &amp; Laptop-Entsprechungen. Aus diesem Grund lohnt es dauert einige Zeit in die datenspeicherungsarchitektur für Ihre app planen, anstatt nur vorausgesetzt, dass eine Datenbank immer über die richtige Antwort ist. Es gibt eine Reihe von verschiedenen Optionen, die unterschiedliche Anforderungen, wie z. B. entsprechen:

-  **Voreinstellungen** – iOS bietet einen integrierten Mechanismus für die Speicherung von einfachen Schlüssel-Wert-Paare von Daten. Wenn Sie einfache benutzerdefinierte Einstellungen oder kleine Mengen an Daten (z. B. Personalisierungsinformationen) speichern verwenden Sie native Funktionen der Plattform für diese Art von Informationen zu speichern. Für iOS können Sie auch der iCloud-Synchronisierung für diese Daten, sowohl für die Sicherung und Synchronisierung für Benutzer mit mehreren Geräten nutzen.
-  **Textdateien** – Benutzereingaben oder Caches der heruntergeladene Inhalt (z. b. HTML) kann direkt auf das Dateisystem gespeichert werden. Verwenden Sie eine geeignete Benennungskonvention für Dateinamen, können Sie die Dateien zu organisieren und Suchen von Daten.
-  **Serialisiert die Datendateien** – Objekte als XML oder JSON im Dateisystem beibehalten werden können. .NET Framework enthält Clientbibliotheken, serialisieren und Deserialisieren Objekte einfach an. Verwenden Sie geeignete Namen, um Datendateien zu organisieren.
-  **Datenbank** – die SQLite-Datenbank-Engine verfügbaren iOS ist, und eignet sich für die Speicherung von strukturierten Daten, die Abfragen, sortieren oder in anderer Weise bearbeitet werden sollen. Datenbankspeicher eignet sich für Listen mit Daten mit vielen Eigenschaften.
-  **Bilddateien** – Obwohl es möglich, speichern Binärdaten in der Datenbank auf einem mobilen Gerät ist, wird empfohlen, diese direkt in das Dateisystem zu speichern. Bei Bedarf können Sie die Dateinamen in einer Datenbank an, ordnen Sie das Image mit anderen Daten speichern. Beim Umgang mit großen Bildern oder viele Images ist es empfiehlt sich, Planen Sie eine cachingstrategie auf, die Dateien, die Sie nicht mehr benötigen löscht, um zu vermeiden, Nutzung von Speicherplatz des Benutzers.


Wenn eine Datenbank den richtigen Speichermechanismus für Ihre app ist, wird im weiteren Verlauf dieses Dokuments erläutert, wie SQLite auf der Xamarin-Plattform zu verwenden.

## <a name="advantages-of-using-a-database"></a>Vorteile der Verwendung einer Datenbank

Es gibt eine Reihe von Vorteilen, die mit einer SQL-Datenbank in Ihrer mobilen app ein:

-  SQL-Datenbanken ermöglichen die effiziente Speicherung von strukturierten Daten.
-  Daten können in komplexen Abfragen extrahiert werden.
-  Abfrageergebnisse können sortiert werden.
-  Abfrageergebnisse können aggregiert werden.
-  Entwickler, mit der vorhandenen datenbankfähigkeiten können ihr Wissen zum Entwurf der Datenbank und Data Access Code nutzen.
-  Das Datenmodell aus die Komponente von einer verbundenen Anwendung möglicherweise (vollständig oder teilweise) in der mobilen Anwendung erneut verwendet werden.


## <a name="sqlite-database-engine"></a>SQLite-Datenbank-Engine

SQLite ist eine Open-Source-Datenbank-Engine, die von Apple für den jeweiligen mobilen Plattform übernommen wurde. Die SQLite-Datenbank-Engine ist für iOS integriert, daher keine zusätzliche Arbeit für Entwickler ist, die sie nutzen. SQLite eignet sich gut für plattformübergreifende mobile Entwicklung, da:

-  Die Datenbank-Engine ist klein, schnell und einfach.
-  Eine Datenbank befindet sich in einer einzigen Datei handelt es sich einfach zu auf mobilen Geräten verwalten.
-  Das Dateiformat ist einfach zu verwenden, über Plattformen hinweg: ob 32- oder 64-Bit, und big oder little-Endian-Systeme.
-  Die meisten von der standardmäßigen SQL92 implementiert.


SQLite ist darauf ausgelegt, klein und schnell sein, gibt es einige Einschränkungen zur Nutzung:

-  Einige OUTER Join-Syntax wird nicht unterstützt.
-  Nur Tabelle umbenennen und ADDCOLUMN werden unterstützt. Andere Änderungen an das Schema ist nicht möglich.
-  Ansichten sind schreibgeschützt.


Sie können weitere Informationen zu SQLite auf der Website - [SQLite.org](http://SQLite.org) – jedoch alle Informationen, die Sie SQLite mit Xamarin verwenden müssen, die in diesem Dokument enthalten ist und die zugehörigen Beispiele. Die SQLite-Datenbank-Engine ist in allen Versionen von iOS integriert.
Obwohl in diesem Kapitel nicht behandelt wird, ist SQLite auch auf Windows Phone und Windows-Anwendungen zur Verfügung.

## <a name="windows-and-windows-phone"></a>Windows und Windows Phone

SQLite kann auch auf Windows-Plattformen verwendet werden, obwohl diese Plattformen in diesem Dokument nicht behandelt werden.
Erfahren Sie mehr in der [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) und [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) Fallstudien, und überprüfen Sie [Tim Heuers Blog](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS-Daten-Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
