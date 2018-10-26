---
title: Einführung in den Datenspeicher auf Android
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 8c90e1c3013ec61cbb4641f19af3424f55b1a465
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50103815"
---
# <a name="introduction"></a>Einführung

## <a name="when-to-use-a-database"></a>Wenn eine Datenbank verwenden

Während der Speicherung und Verarbeitung von Funktionen der mobilen Geräte zunehmen, zurückfallen Smartphones und Tablets weiterhin ihren Desktop- und Laptopcomputer-Entsprechungen. Aus diesem Grund lohnt es dauert einige Zeit in die datenspeicherungsarchitektur für Ihre app planen, anstatt nur vorausgesetzt, dass eine Datenbank immer über die richtige Antwort ist. Es gibt eine Reihe von verschiedenen Optionen, die unterschiedliche Anforderungen, wie z. B. entsprechen:

-  **Voreinstellungen** – Android bietet einen integrierten Mechanismus für die Speicherung von einfachen Schlüssel-Wert-Paare von Daten. Wenn Sie einfache benutzerdefinierte Einstellungen oder kleine Mengen an Daten (z. B. Personalisierungsinformationen) speichern verwenden Sie native Funktionen der Plattform für diese Art von Informationen zu speichern.
-  **Textdateien** – Benutzereingaben oder Caches der heruntergeladene Inhalt (z. b. HTML) kann direkt auf das Dateisystem gespeichert werden. Verwenden Sie eine geeignete Benennungskonvention für Dateinamen, können Sie die Dateien zu organisieren und Suchen von Daten.
-  **Serialisiert die Datendateien** – Objekte als XML oder JSON im Dateisystem beibehalten werden können. .NET Framework enthält Clientbibliotheken, serialisieren und Deserialisieren Objekte einfach an. Verwenden Sie geeignete Namen, um Datendateien zu organisieren.
-  **Datenbank** – die SQLite-Datenbank-Engine auf der Android-Plattform verfügbar ist, und ist nützlich für die Speicherung von strukturierten Daten, die Abfragen, sortieren oder in anderer Weise bearbeitet werden sollen. Datenbankspeicher eignet sich für Listen mit Daten mit vielen Eigenschaften.
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

SQLite ist eine Open-Source-Datenbank-Engine, die von Google für die jeweiligen mobilen Plattform eingeführt worden ist. Die SQLite-Datenbank-Engine ist für beide Betriebssysteme integriert, daher keine zusätzliche Arbeit für Entwickler ist, die sie nutzen. SQLite eignet sich gut für plattformübergreifende mobile Entwicklung, da:

-  Die Datenbank-Engine ist klein, schnell und einfach.
-  Eine Datenbank befindet sich in einer einzigen Datei handelt es sich einfach zu auf mobilen Geräten verwalten.
-  Das Dateiformat ist einfach zu verwenden, über Plattformen hinweg: ob 32- oder 64-Bit, und big oder little-Endian-Systeme.
-  Die meisten von der standardmäßigen SQL92 implementiert.


SQLite ist darauf ausgelegt, klein und schnell sein, gibt es einige Einschränkungen zur Nutzung:

-  Einige OUTER Join-Syntax wird nicht unterstützt.
-  Nur Tabelle umbenennen und ADDCOLUMN werden unterstützt. Andere Änderungen an das Schema ist nicht möglich.
-  Ansichten sind schreibgeschützt.


Sie können weitere Informationen zu SQLite auf der Website - [SQLite.org](http://SQLite.org) – jedoch alle Informationen, die Sie SQLite mit Xamarin verwenden müssen, die in diesem Dokument enthalten ist und die zugehörigen Beispiele. Seit Android 2 wurde die SQLite-Datenbank-Engine unter Android unterstützt.
Obwohl in diesem Kapitel nicht behandelt wird, ist SQLite auch auf Windows Phone und Windows-Anwendungen zur Verfügung.

## <a name="windows-and-windows-phone"></a>Windows und Windows Phone

SQLite kann auch auf Windows-Plattformen verwendet werden, obwohl diese Plattformen in diesem Dokument nicht behandelt werden.
Erfahren Sie mehr in der [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) und [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) Fallstudien, und überprüfen Sie [Tim Heuers Blog](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Rezepte für Android-Daten](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
