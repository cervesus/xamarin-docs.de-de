---
title: Xamarin.iOS-Datenzugriff
description: Dieses Dokument enthält Links zu Anleitungen, die beschreiben, wie Sie mit lokalen Datenbanken in einer Xamarin.iOS-Anwendung arbeiten. Verknüpfter Inhalt wird von SQLite.NET ADO.NET und vieles mehr erläutert.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 10/11/2016
ms.openlocfilehash: 8d2513ba1c2ae2769e81659c98f3897f33d83fbf
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/23/2019
ms.locfileid: "61218037"
---
# <a name="xamarinios-data-access"></a>Xamarin.iOS-Datenzugriff

Xamarin.iOS unterstützt, z.B. Datenbankzugriff APIs:

-  ADO.NET-Framework.
-  SQLite-NET-3rd party Bibliothek.

Dieses Handbuch bietet einen allgemeinen Überblick über die Datenbanken in der Regel vor beschreibt, wie ADO.NET und von SQLite.NET auf SQLite-Datenbanken in Ihrer Xamarin.iOS-Anwendungen einrichten. 

Der Großteil des Codes in diesem Dokument ist plattformübergreifend und ohne Änderungen unter iOS oder Android ausgeführt wird. Es gibt zwei Beispiel-apps erläutert:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – einfache Datenvorgänge schreibt Anzeigen der Ergebnisse in einem Text-Steuerelement
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – Datenvorgänge in eine kleine Anwendung, die aufgelistet und eine einfache Datenstruktur bearbeitet integriert.

Sowohl vollständige, gepackte Anwendungen enthalten, iOS und Android-Beispiel-Anwendungsprojekte.

Xamarin.Forms-Anwendungen finden Sie in [arbeiten mit Datenbanken](~/xamarin-forms/app-fundamentals/databases.md) das Arbeiten mit SQLite in einer PCL-Bibliothek mit Xamarin.Forms erläutert.

## <a name="sections"></a>Abschnitte

-  [Introduction (Einführung)](introduction.md)
-  [Konfiguration](configuration.md)
-  [Verwenden von SQLite.NET ORM](using-sqlite-orm.md)
-  [Verwenden von ADO.NET](using-adonet.md)
-  [Verwenden von Daten in einer App](using-data-in-an-app.md)

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel erläutert-Datenzugriff in Xamarin.iOS mithilfe von SQLite als die Datenbank-Engine. Die Datenbank "direkt" mit ADO.NET-Syntax zugegriffen werden kann oder Sie enthalten die von SQLite.NET ORM und Ausführen von Datenvorgängen in C# geschrieben.

Wir gesehen, dass zwei Beispiele: sehr einfach den Datenzugriffscode enthält, die Ausgaben in einem Textfeld und eine einfache Anwendung, die enthält erstellen, lesen, aktualisieren und Löschen von Funktionen. Außerdem wird erläutert, threading und wie Sie Ihre Anwendung mit einer voraufgefüllten SQLite-Datenbank ein Seeding.

Weitere Beispiele für plattformübergreifende Datenzugriff finden Sie in unserem [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) Fallstudie.

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS-Daten-Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
