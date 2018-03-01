---
title: "Einführung"
ms.topic: article
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f270248ad3b43d7a4c818b29390b3391d89ddd14
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/27/2018
---
# <a name="introduction"></a>Einführung

## <a name="when-to-use-a-database"></a>Wenn eine Datenbank verwenden

Während die Speicher- und Verarbeitungsvorgängen Funktionen der mobilen Geräte zu erhöhen, werden Telefone und Tablets weiterhin zurückfallen Ihrem eigenen Schreibtisch &amp; Laptop-Entsprechungen. Aus diesem Grund lohnt es dauert einige Zeit in die Speicherarchitektur Daten für die app planen, statt nur vorausgesetzt, dass die richtige Antwort immer um eine Datenbank handelt. Es gibt eine Reihe von verschiedenen Optionen, die unterschiedliche Anforderungen, z. B. anzupassen:

-  **Voreinstellungen** – iOS bietet einen eingebauten Mechanismus zum Speichern von einfachen Schlüssel-Wert-Paare von Daten. Wenn Sie einfache benutzereinstellungen oder kleine Mengen an Daten (z. B. Personalisierungsinformationen) speichern verwenden Sie das systemeigene Plattformfunktionen für diese Art von Informationen zu speichern. Für iOS können Sie auch iCloud-Synchronisierung für diese Daten können sowohl für die Sicherung und Synchronisierung für Benutzer mit mehreren Geräten nutzen.
-  **Textdateien** – Benutzereingaben oder Caches des heruntergeladenen Inhalts (z. b. HTML) kann direkt auf dem Dateisystem gespeichert werden. Verwenden Sie eine entsprechende Dateinamenskonvention helfen Ihnen beim Organisieren der Dateien und Daten suchen.
-  **Serialisiert die Datendateien** – Objekte als XML oder JSON im Dateisystem beibehalten werden können. .NET Framework enthält Bibliotheken, die Stellen serialisieren und Deserialisieren die Objekte einfach an. Verwenden Sie zum Organisieren von Datendateien entsprechende Namen ein.
-  **Datenbank** – die SQLite-Datenbankmodul verfügbar iOS ist und eignet sich zum Speichern strukturierter Daten, die Sie Abfragen, sortieren oder anderweitig bearbeiten müssen. Datenbankspeicher eignet sich für Listen mit Daten mit zahlreichen Eigenschaften.
-  **Bilddateien** – Obwohl es möglich, binäre Daten in der Datenbank auf einem mobilen Gerät gespeichert ist, wird empfohlen, diese direkt in das Dateisystem zu speichern. Bei Bedarf können Sie die Dateinamen in einer Datenbank an, ordnen Sie das Bild mit anderen Daten speichern. Beim Umgang mit großen Bilder oder große Bilder ist es empfiehlt sich, eine zwischenspeicherstrategie planen, die Dateien, die Sie nicht mehr benötigen löscht, um zu vermeiden, des Benutzers Speicher belegt.


Wenn eine Datenbank die richtige Speichermechanismus für Ihre app ist, wird im weiteren Verlauf dieses Dokuments erläutert, wie SQLite auf der Xamarin-Plattform zu verwenden.

## <a name="advantages-of-using-a-database"></a>Vorteile der Verwendung einer Datenbank

Es gibt eine Reihe von Vorteilen mit einer SQL-Datenbank in Ihrer mobilen app ein:

-  SQL-Datenbanken ermöglichen die effiziente Speicherung von strukturierten Daten.
-  Bei komplexen Abfragen können bestimmte Daten extrahiert werden.
-  Die Abfrageergebnisse können sortiert werden.
-  Abfrageergebnisse aggregiert werden können.
-  Entwickler, mit der vorhandenen Datenbank Fähigkeiten können ihre Kenntnisse zum Entwerfen der Zugriffscode Datenbank und die Daten nutzen.
-  Das Datenmodell aus der Serverkomponente der eine verbundene Anwendung möglicherweise (ganz oder teilweise) in der mobilen Anwendung erneut verwendet werden.


## <a name="sqlite-database-engine"></a>SQLite Database Engine

SQLite ist ein Open Source-Datenbankmodul, die von Apple für ihre mobile-Plattform übernommen wurden. Das Datenbankmodul SQLite ist für iOS-integriert, damit es keine zusätzliche Arbeit für Entwickler ist zu nutzen. SQLite eignet sich gut für plattformübergreifende mobile Entwicklung, da:

-  Das Datenbankmodul ist klein, schnell und einfach portabel.
-  Eine Datenbank wird in einer einzigen Datei gespeichert, was leicht auf mobilen Geräten zu verwalten ist.
-  Das Dateiformat ist einfach zu verwendende plattformübergreifend: ob 32- oder 64-Bit, und Big- oder little-Endian-Systeme.
-  Die meisten der SQL92-standard implementiert.


SQLite ist darauf ausgelegt, kleiner und schneller sein, sind einige Einschränkungen auf seine Verwendung:

-  Einige OUTER Join-Syntax wird nicht unterstützt.
-  Nur Tabelle umbenennen und ADDCOLUMN werden unterstützt. Sie können keine andere Änderungen an Ihrem Schema ausführen.
-  Ansichten sind schreibgeschützt.


Sie können weitere Informationen zu SQLite auf der Website - [SQLite.org](http://SQLite.org) - jedoch alle Informationen, die Sie mit Xamarin SQLite verwenden müssen, die in diesem Dokument enthalten ist und die zugehörigen Beispiele. Das Datenbankmodul SQLite ist für alle Versionen von iOS integriert.
Obwohl in diesem Kapitel nicht behandelt wird, ist SQLite auch auf Windows Phone und Windows-Anwendungen zur Verfügung.

## <a name="windows-and-windows-phone"></a>Windows und Windows Phone

SQLite kann auch auf Windows-Plattformen verwendet werden, obwohl diese Plattformen in diesem Dokument nicht behandelt werden.
Erfahren Sie mehr in der [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) und [Tasky Pro](http://docs.xamarin.com/guides/cross-platform/application_fundamentals/building_cross_platform_applications/case_study%3A_tasky) Fallstudien, und überprüfen Sie [Tim Heuers Blog](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).



## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS Daten Rezepte](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
