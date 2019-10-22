---
title: Einführung in die Datenspeicherung unter Android
ms.prod: xamarin
ms.assetid: FDAC0771-4749-4758-865A-F1BD190CA54B
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/28/2017
ms.openlocfilehash: 69d5222bb6c50870d0c42bea6ff71236e3d1580c
ms.sourcegitcommit: 9bfedf07940dad7270db86767eb2cc4007f2a59f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/21/2019
ms.locfileid: "70754565"
---
# <a name="introduction"></a>Einführung

## <a name="when-to-use-a-database"></a>Verwendung einer Datenbank

Die Speicher-und Verarbeitungsfunktionen mobiler Geräte erhöhen sich, aber Smartphones und Tablets bleiben weiterhin hinter den Desktop-und Laptop-Gegenstücken zurück. Aus diesem Grund lohnt es sich, die Datenspeicher Architektur für Ihre APP zu planen, anstatt nur davon auszugehen, dass eine Datenbank immer die richtige Antwort ist. Es gibt eine Reihe verschiedener Optionen, die unterschiedliche Anforderungen erfüllen, wie z. b.:

- **Einstellungen** – Android bietet einen integrierten Mechanismus zum Speichern einfacher Schlüssel-Wert-Paare von Daten. Wenn Sie einfache Benutzereinstellungen oder kleine Datenelemente (z. b. Personalisierungs Informationen) speichern, verwenden Sie die systemeigenen Features der Plattform zum Speichern dieser Art von Informationen.
- **Text Dateien** – Benutzereingaben oder Caches heruntergeladener Inhalte (z. b. HTML) kann direkt auf dem Dateisystem gespeichert werden. Verwenden Sie eine geeignete Datei Benennungs Konvention, um die Dateien zu organisieren und Daten zu suchen.
- **Serialisierte Datendateien** – Objekte können im Dateisystem als XML oder JSON persistent gespeichert werden. .NET Framework enthält Bibliotheken, die das Serialisieren und Deserialisieren von Objekten erleichtern. Verwenden Sie geeignete Namen, um Datendateien zu organisieren.
- **Database** – die SQLite-Datenbank-Engine ist auf der Android-Plattform verfügbar und ist nützlich zum Speichern strukturierter Daten, die Sie Abfragen, Sortieren oder anderweitig bearbeiten müssen. Der Daten Bank Speicher ist für Listen von Daten mit vielen Eigenschaften geeignet.
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

SQLite ist eine Open-Source-Datenbank-Engine, die von Google für Ihre Mobile Plattform übernommen wurde. Das SQLite-Datenbankmodul ist in beide Betriebssysteme integriert, sodass Entwickler es nicht mehr nutzen können. SQLite eignet sich gut für die plattformübergreifende Mobile Entwicklung:

- Die Datenbank-Engine ist klein, schnell und leicht portierbar.
- Eine Datenbank wird in einer einzelnen Datei gespeichert, die auf mobilen Geräten einfach zu verwalten ist.
- Das Dateiformat ist plattformübergreifend einfach zu verwenden: ob 32-oder 64-Bit-Systeme und Big-oder Little-Endian-Systeme.
- Es implementiert den größten Teil des SQL92-Standards.

Da SQLite so konzipiert ist, dass es klein und schnell ist, gibt es einige Einschränkungen bei der Verwendung:

- Einige äußere joinsyntax wird nicht unterstützt.
- Es werden nur Tabellen umbenennen und AddColumn unterstützt. Sie können keine anderen Änderungen am Schema vornehmen.
- Sichten sind schreibgeschützt.

Weitere Informationen zu SQLite finden Sie in der Website [sqlite.org](http://SQLite.org) alle Informationen, die Sie benötigen, um SQLite mit xamarin zu verwenden, sind in diesem Dokument und den zugehörigen Beispielen enthalten. Die SQLite-Datenbank-Engine wurde seit Android 2 in Android unterstützt.
Obwohl es in diesem Kapitel nicht behandelt wird, steht SQLite auch für Windows Phone-und Windows-Anwendungen zur Verfügung.

## <a name="windows-and-windows-phone"></a>Windows und Windows Phone

SQLite kann auch auf Windows-Plattformen verwendet werden, auch wenn diese Plattformen in diesem Dokument nicht behandelt werden.
Weitere Informationen finden Sie in den Fallstudien [Tasky](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) und [Tasky pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) und im [Blog von Tim Heuer](http://timheuer.com/blog/archive/2012/06/28/seeding-your-metro-style-app-with-sqlite-database.aspx).

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
