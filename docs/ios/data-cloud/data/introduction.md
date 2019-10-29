---
title: Einführung in die Datenspeicherung in xamarin. IOS-apps
description: In diesem Dokument werden verschiedene Möglichkeiten der Datenspeicherung in einer xamarin. IOS-Anwendung beschrieben, und es werden spezifische Informationen zu den Vorteilen von SQLite bereitstellt.
ms.prod: xamarin
ms.assetid: B1994468-FD06-4FD9-96B3-FCEBB13A972A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: eefe57abd4ebf4986411a1d717aebd131ebf408f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008334"
---
# <a name="introduction-to-data-storage-in-xamarinios-apps"></a>Einführung in die Datenspeicherung in xamarin. IOS-apps

## <a name="when-to-use-a-database"></a>Verwendung einer Datenbank

Während sich die Speicher-und Verarbeitungsfunktionen mobiler Geräte erhöhen, hinken Telefone und Tablets weiterhin hinter den Desktop-&amp; Laptop-Gegenstücke. Aus diesem Grund lohnt es sich, die Datenspeicher Architektur für Ihre APP zu planen, anstatt nur davon auszugehen, dass eine Datenbank immer die richtige Antwort ist. Es gibt eine Reihe verschiedener Optionen, die unterschiedliche Anforderungen erfüllen, wie z. b.:

- **Einstellungen** – IOS bietet einen integrierten Mechanismus zum Speichern einfacher Schlüssel-Wert-Paare von Daten. Wenn Sie einfache Benutzereinstellungen oder kleine Datenelemente (z. b. Personalisierungs Informationen) speichern, verwenden Sie die systemeigenen Features der Plattform zum Speichern dieser Art von Informationen. Für IOS können Sie die icloud-Synchronisierung für diese Daten auch für die Sicherung und Synchronisierung für Benutzer mit mehreren Geräten nutzen.
- **Text Dateien** – Benutzereingaben oder Caches heruntergeladener Inhalte (z. b. HTML) kann direkt auf dem Dateisystem gespeichert werden. Verwenden Sie eine geeignete Datei Benennungs Konvention, um die Dateien zu organisieren und Daten zu suchen.
- **Serialisierte Datendateien** – Objekte können im Dateisystem als XML oder JSON persistent gespeichert werden. .NET Framework enthält Bibliotheken, die das Serialisieren und Deserialisieren von Objekten erleichtern. Verwenden Sie geeignete Namen, um Datendateien zu organisieren.
- **Database** – die SQLite-Datenbank-Engine ist als IOS verfügbar und eignet sich zum Speichern strukturierter Daten, die Sie Abfragen, Sortieren oder anderweitig bearbeiten müssen. Der Daten Bank Speicher ist für Listen von Daten mit vielen Eigenschaften geeignet.
- **Bilddateien** – obwohl es möglich ist, Binärdaten in der Datenbank auf einem mobilen Gerät zu speichern, empfiehlt es sich, diese direkt im Dateisystem zu speichern. Falls erforderlich, können Sie die Dateinamen in einer Datenbank speichern, um das Image anderen Daten zuzuordnen. Beim Umgang mit großen Images oder vielen Bildern empfiehlt es sich, eine cachingstrategie zu planen, mit der Dateien gelöscht werden, die nicht mehr benötigt werden, um zu vermeiden, dass der gesamte Speicherplatz des Benutzers beansprucht wird.

Wenn eine Datenbank der richtige Speichermechanismus für Ihre APP ist, wird im restlichen Teil dieses Dokuments erläutert, wie SQLite auf der xamarin-Plattform verwendet wird.

## <a name="advantages-of-using-a-database"></a>Vorteile der Verwendung einer Datenbank

Die Verwendung einer SQL-Datenbank in Ihrem Mobile App bietet eine Reihe von Vorteilen:

- SQL-Datenbanken ermöglichen eine effiziente Speicherung strukturierter Daten.
- Bestimmte Daten können mit komplexen Abfragen extrahiert werden.
- Abfrageergebnisse können sortiert werden.
- Abfrageergebnisse können aggregiert werden.
- Entwickler mit vorhandenen Daten Bank Kenntnissen können Ihr Wissen nutzen, um den Daten Bank-und Datenzugriffs Code zu entwerfen.
- Das Datenmodell aus der Serverkomponente einer verbundenen Anwendung kann in der mobilen Anwendung (ganz oder teilweise) wieder verwendet werden.

## <a name="sqlite-database-engine"></a>SQLite-Datenbank-Engine

SQLite ist eine Open-Source-Datenbank-Engine, die von Apple für Ihre Mobile Plattform übernommen wurde. Die SQLite-Datenbank-Engine ist in ios integriert, sodass Entwickler keine weiteren Aufgaben für die Nutzung durchführen können. SQLite eignet sich gut für die plattformübergreifende Mobile Entwicklung:

- Die Datenbank-Engine ist klein, schnell und leicht portierbar.
- Eine Datenbank wird in einer einzelnen Datei gespeichert, die auf mobilen Geräten einfach zu verwalten ist.
- Das Dateiformat ist plattformübergreifend einfach zu verwenden: ob 32-oder 64-Bit-Systeme und Big-oder Little-Endian-Systeme.
- Es implementiert den größten Teil des SQL92-Standards.

Da SQLite so konzipiert ist, dass es klein und schnell ist, gibt es einige Einschränkungen bei der Verwendung:

- Einige äußere joinsyntax wird nicht unterstützt.
- Es werden nur Tabellen umbenennen und AddColumn unterstützt. Sie können keine anderen Änderungen am Schema vornehmen.
- Sichten sind schreibgeschützt.

Weitere Informationen zu SQLite finden Sie in der Website [sqlite.org](https://SQLite.org) alle Informationen, die Sie benötigen, um SQLite mit xamarin zu verwenden, sind in diesem Dokument und den zugehörigen Beispielen enthalten. Die SQLite-Datenbank-Engine ist in alle Versionen von IOS integriert.
Obwohl es in diesem Kapitel nicht behandelt wird, steht SQLite auch für Windows Phone-und Windows-Anwendungen zur Verfügung.

## <a name="windows-and-windows-phone"></a>Windows und Windows Phone

SQLite kann auch auf Windows-Plattformen verwendet werden, auch wenn diese Plattformen in diesem Dokument nicht behandelt werden.
Weitere Informationen finden Sie in der [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) -Fallstudie und im [Blog von Tim Heuer](https://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [IOS-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
