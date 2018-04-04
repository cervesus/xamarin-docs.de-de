---
title: iOS-Datenzugriff
description: Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS hat die SQLite-Datenbank-Engine "integriert" und Zugriff zum Speichern und Abrufen von Daten durch die Xamarin Plattform vereinfacht. Dieses Dokument wird gezeigt, wie eine SQLite-Datenbank zuzugreifen.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: 47f2567d81f61568aad639330dc5133856e31936
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="ios-data-access"></a>iOS-Datenzugriff

_Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. iOS hat die SQLite-Datenbank-Engine "integriert" und Zugriff zum Speichern und Abrufen von Daten durch die Xamarin Plattform vereinfacht. Dieses Dokument wird gezeigt, wie eine SQLite-Datenbank zuzugreifen._

Xamarin.iOS unterstützt Datenbankzugriff APIs, wie beispielsweise:

-  ADO.NET Framework.
-  SQLite-NET-3rd party Bibliothek.

Dieses Handbuch enthält einen allgemeinen Überblick über die Datenbanken in der Regel vor, die zum Einrichten von ADO.NET und SQLite.NET Zugriff auf die Datenbanken SQLite in Ihren Anwendungen Xamarin.iOS beschreibt. 

Der Großteil des Codes in diesem Dokument ist vollständig plattformübergreifende sowie auf iOS oder Android ausgeführt werden, die ohne Änderung. Es gibt zwei beispielapps erläutert:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – einfache Datenvorgänge schreibt die Ergebnisse in einem Text-Anzeigesteuerelement;
-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – Datenvorgänge in eine kleine funktionierende Anwendung, die aufgelistet und eine einfache Datenstruktur bearbeitet integriert werden kann.

Beide Beispielprojektmappen enthalten IOS- und Android-Beispielprojekte für die Anwendung.

Xamarin.Forms-Anwendungen finden Sie unter [arbeiten mit Datenbanken](~/xamarin-forms/app-fundamentals/databases.md) dort zum Arbeiten mit SQLite in einer PCL-Bibliothek mit Xamarin.Forms.

## <a name="sections"></a>Abschnitte

-  [Introduction (Einführung)](introduction.md)
-  [Konfiguration](configuration.md)
-  [Verwenden von SQLite.NET ORM](using-sqlite-orm.md)
-  [Verwenden von ADO.NET](using-adonet.md)
-  [Verwenden von Daten in einer App](using-data-in-an-app.md)


## <a name="summary"></a>Zusammenfassung

In diesem Kapitel erläutert-Datenzugriff in Xamarin.iOS SQLite als Datenbankmodul verwenden. Die Datenbank "direkt" mithilfe von ADO.NET-Syntax zugegriffen werden kann oder Sie umfassen die SQLite.NET ORM und Ausführen von Datenvorgängen in C# geschrieben.

Wir gesehen, dass zwei Beispiele: eine, die sehr einfache Datenzugriffscode enthält, die Ausgaben in einem Textfeld und einer einfachen Anwendung, enthält erstellen, lesen, aktualisieren und Löschen von Funktionen. Außerdem wird besprochen threading und wie Sie den Ausgangswert für der Anwendung mit einer vorgegebenen SQLite-Datenbank.

Zusätzliche Beispiele für plattformübergreifende Datenzugriff finden Sie unsere [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) Fallstudie.

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS Daten Rezepte](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
