---
title: Xamarin.Android Data Access
description: Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten.  Android bietet "integriert" SQLite-Datenbank-Engine und Zugriff zum Speichern und Abrufen von Daten durch die Xamarin Plattform vereinfacht. Dieses Dokument wird gezeigt, wie eine SQLite-Datenbank auf eine Weise plattformübergreifende zuzugreifen.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 508a8f54723bfdd30b1c8ea8d4b6c1d422ae24e9
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android Data Access

_Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten.  Android bietet "integriert" SQLite-Datenbank-Engine und Zugriff zum Speichern und Abrufen von Daten durch die Xamarin Plattform vereinfacht. Dieses Dokument wird gezeigt, wie eine SQLite-Datenbank auf eine Weise plattformübergreifende zuzugreifen._

## <a name="data-access-overview"></a>Data Access (Übersicht)

Die meisten Anwendungen auf eine Anforderung zum Speichern von Daten auf dem Gerät lokal. Wenn die Menge der Daten im Grunde klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. Android beide bietet die Sqlite-Datenbank-Engine "integriert" und Zugriff auf die Daten durch die Xamarin Plattform dies mit dem Datenanbieter SQLite weitestgehend vereinfacht.

Xamarin.Android unterstützen Datenbankzugriff APIs wie z. B.:

-  ADO.NET Framework.
-  SQLite-NET-3rd party Bibliothek.

Der Großteil des Codes in diesem Abschnitt ist vollständig plattformübergreifende sowie auf iOS oder Android ausgeführt werden, die ohne Änderung. Es gibt zwei beispielapps erläutert:

-  [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; einfache Datenvorgänge schreibt die Ergebnisse in einem Text-Anzeigesteuerelement;

-  [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; Datenvorgänge in eine kleine funktionierende Anwendung, die aufgelistet und eine einfache Datenstruktur bearbeitet integriert werden kann.

Beide Beispielprojektmappen enthalten IOS- und Android-Beispielprojekte für die Anwendung.

Xamarin.Forms-Anwendungen finden Sie unter [arbeiten mit Datenbanken](~/xamarin-forms/app-fundamentals/databases.md) dort zum Arbeiten mit SQLite in einer PCL-Bibliothek mit Xamarin.Forms.

Die Themen in diesem Abschnitt erläutern-Datenzugriff in Xamarin.Android SQLite als Datenbankmodul verwenden. Die Datenbank "direkt" mithilfe von ADO.NET-Syntax zugegriffen werden kann oder Sie umfassen die SQLite.NET ORM und Ausführen von Datenvorgängen in C# geschrieben.

Zwei Beispiele geprüft werden: eine, die sehr einfache Datenzugriffscode enthält, die Ausgaben in einem Textfeld und einer einfachen Anwendung, enthält erstellen, lesen, aktualisieren und Löschen von Funktionen. Threading und wie Sie den Ausgangswert für der Anwendung mit einer vorgegebenen SQLite-Datenbank wird ebenfalls erläutert.

Zusätzliche Beispiele für plattformübergreifende Datenzugriff finden Sie unsere [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) Fallstudie.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android Daten Rezepte](https://developer.xamarin.com/recipes/android/data/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
