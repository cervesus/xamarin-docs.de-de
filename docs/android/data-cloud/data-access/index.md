---
title: Xamarin.Android-Datenzugriff
description: Die meisten Anwendungen müssen einige Anforderungen, um Daten auf dem Gerät lokal zu speichern. Wenn die Menge der Daten unbedeutend klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten.  Android hat die SQLite-Datenbank-Engine, die "integriert" und den Zugriff auf das Speichern und Abrufen von Daten durch die Xamarin Plattform vereinfacht. Dieses Dokument veranschaulicht, wie plattformübergreifende eine SQLite-Datenbank zuzugreifen.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 08720734de73af12d8a7383fa7d523dc350c4462
ms.sourcegitcommit: 729035af392dc60edb9d99d3dc13d1ef69d5e46c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2018
ms.locfileid: "50674767"
---
# <a name="xamarinandroid-data-access"></a>Xamarin.Android-Datenzugriff

_Die meisten Anwendungen müssen einige Anforderungen, um Daten auf dem Gerät lokal zu speichern. Wenn die Menge der Daten unbedeutend klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten.  Android hat die SQLite-Datenbank-Engine, die "integriert" und den Zugriff auf das Speichern und Abrufen von Daten durch die Xamarin Plattform vereinfacht. Dieses Dokument veranschaulicht, wie plattformübergreifende eine SQLite-Datenbank zuzugreifen._

## <a name="data-access-overview"></a>Data Access (Übersicht)

Die meisten Anwendungen müssen einige Anforderungen, um Daten auf dem Gerät lokal zu speichern. Wenn die Menge der Daten unbedeutend klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung Zugriff auf die Datenbank zu verwalten. Android sowohl hat die SQLite-Datenbank-Engine, die "integriert" und Zugriff auf die Daten durch Xamarin Plattform mit SQLite-Anbieter Daten vereinfacht.

Xamarin.Android unterstützt die Datenbank Datenzugriffs-APIs wie z. B.:

- ADO.NET-Framework.
- SQLite-NET-3rd party Bibliothek.

Der Großteil des Codes in diesem Abschnitt ist plattformübergreifend und ohne Änderungen unter iOS oder Android ausgeführt wird. Es gibt zwei Beispiel-apps erläutert:

- [**DataAccess_Basic** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; einfache Datenvorgänge schreibt Anzeigen der Ergebnisse in einem Text-Steuerelement

- [**DataAccess_Advanced** ](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; Datenvorgänge in eine kleine Anwendung, die aufgelistet und eine einfache Datenstruktur bearbeitet integriert.

Sowohl vollständige, gepackte Anwendungen enthalten, iOS und Android-Beispiel-Anwendungsprojekte.

Xamarin.Forms-Anwendungen finden Sie in [arbeiten mit Datenbanken](~/xamarin-forms/app-fundamentals/databases.md) das Arbeiten mit SQLite in einer PCL-Bibliothek mit Xamarin.Forms erläutert.

In die Themen in diesem Abschnitt erläutern den Datenzugriff in Xamarin.Android mithilfe von SQLite als die Datenbank-Engine. Die Datenbank "direkt" mithilfe von ADO.NET-Syntax zugegriffen werden kann oder Sie enthalten die von SQLite.NET ORM und Ausführen von Datenvorgängen in C# geschrieben.

Zwei Beispiele werden überprüft: sehr einfach den Datenzugriffscode enthält, die Ausgaben in einem Textfeld und eine einfache Anwendung, die enthält erstellen, lesen, aktualisieren und Löschen von Funktionen. Threading und wie Sie Ihre Anwendung mit einer voraufgefüllten SQLite-Datenbank ein Seeding wird ebenfalls erläutert.

Weitere Beispiele für plattformübergreifende Datenzugriff finden Sie in unserem [Tasky Pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) Fallstudie.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Rezepte für Android-Daten](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
