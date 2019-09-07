---
title: Xamarin. Android-Datenzugriff
description: Bei den meisten Anwendungen ist es erforderlich, dass Daten lokal auf dem Gerät gespeichert werden. Wenn die Menge der Daten nicht trivial klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung, um den Zugriff auf die Datenbank zu verwalten.  In Android ist die SQLite-Datenbank-Engine "integriert", und der Zugriff zum Speichern und Abrufen von Daten wird durch die xamarin-Plattform vereinfacht. In diesem Dokument wird gezeigt, wie Sie plattformübergreifend auf eine SQLite-Datenbank zugreifen können.
ms.prod: xamarin
ms.assetid: 6B47E864-C6E7-4AA2-8DEF-2C8BF551D17C
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 2343603199661ea39b1f0af172ce0ccf48a2cd66
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754586"
---
# <a name="xamarinandroid-data-access"></a>Xamarin. Android-Datenzugriff

_Bei den meisten Anwendungen ist es erforderlich, dass Daten lokal auf dem Gerät gespeichert werden. Wenn die Menge der Daten nicht trivial klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung, um den Zugriff auf die Datenbank zu verwalten.  In Android ist die SQLite-Datenbank-Engine "integriert", und der Zugriff zum Speichern und Abrufen von Daten wird durch die xamarin-Plattform vereinfacht. In diesem Dokument wird gezeigt, wie Sie plattformübergreifend auf eine SQLite-Datenbank zugreifen können._

## <a name="data-access-overview"></a>Übersicht über den Datenzugriff

Bei den meisten Anwendungen ist es erforderlich, dass Daten lokal auf dem Gerät gespeichert werden. Wenn die Menge der Daten nicht trivial klein ist, erfordert dies in der Regel eine Datenbank und eine Datenschicht in der Anwendung, um den Zugriff auf die Datenbank zu verwalten. In Android ist die SQLite-Datenbank-Engine "integriert", und der Zugriff auf die Daten wird durch die xamarin-Plattform vereinfacht, die die SQLite-Datenanbieter enthält.

Xamarin. Android unterstützt Datenbankzugriffs-APIs wie z. b.:

- ADO.NET Framework.
- SQLite-net-Drittanbieter Bibliothek.

Der Großteil des Codes in diesem Abschnitt ist vollständig plattformübergreifend und kann ohne Änderungen unter IOS oder Android ausgeführt werden. Es gibt zwei Beispiel-apps, die erläutert werden:

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) &ndash; Einfache Daten Vorgänge schreiben die Ergebnisse in ein Textanzeige Steuerelement;

- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) &ndash; Integriert Daten Vorgänge in eine kleine funktionierende Anwendung, die eine einfache Datenstruktur auflistet und bearbeitet.

Beide Beispiel Lösungen enthalten IOS-und Android-Beispiel Anwendungsprojekte.

Informationen zu xamarin. Forms-Anwendungen finden Sie unter [Arbeiten mit Datenbanken](~/xamarin-forms/data-cloud/data/databases.md) , in denen erläutert wird, wie mit sqlite in einer PCL-Bibliothek mit xamarin. Forms gearbeitet wird.

In den Themen in diesem Abschnitt wird der Datenzugriff in xamarin. Android mithilfe von SQLite als Datenbank-Engine erörtert. Der Zugriff auf die Datenbank kann direkt mithilfe der ADO.NET-Syntax erfolgen, oder Sie können den sqlite.net ORM einschließen und Daten Vorgänge C#in ausführen.

Zwei Beispiele werden überprüft: eine, die sehr einfachen Datenzugriffs Code enthält, der in ein Textfeld ausgibt, und eine einfache Anwendung, die Funktionen zum Erstellen, lesen, aktualisieren und löschen umfasst. Das Threading und der Ausgangswert für Ihre Anwendung mit einer vorab aufgefüllten SQLite-Datenbank werden ebenfalls erläutert.

Weitere Beispiele für plattformübergreifenden Datenzugriff finden Sie in unserer [Tasky pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) -Fallstudie.

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
