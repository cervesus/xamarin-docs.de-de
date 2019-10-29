---
title: Xamarin. IOS-Datenzugriff
description: Dieses Dokument ist mit Anleitungen verknüpft, die beschreiben, wie lokale Datenbanken in einer xamarin. IOS-Anwendung verwendet werden. In verknüpften Inhalten werden sqlite.net, ADO.net und mehr erläutert.
ms.prod: xamarin
ms.assetid: 3AEDFD8D-FB10-4CEF-BE04-CCD14E95F02C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: 163bd88349ee55af5f518a20364bbce7fe6052b0
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73008436"
---
# <a name="xamarinios-data-access"></a>Xamarin. IOS-Datenzugriff

Xamarin. IOS unterstützt Datenbankzugriffs-APIs wie z. b.:

- ADO.NET Framework.
- SQLite-net-Drittanbieter Bibliothek.

Dieses Handbuch bietet eine allgemeine Übersicht über Datenbanken im Allgemeinen, bevor beschrieben wird, wie Sie ADO.net und SQLite.net für den Zugriff auf SQLite-Datenbanken in ihren xamarin. IOS-Anwendungen einrichten. 

Der Großteil des Codes in diesem Dokument ist vollständig plattformübergreifend und kann ohne Änderungen unter IOS oder Android ausgeführt werden. Es gibt zwei Beispiel-apps, die erläutert werden:

- [**DataAccess_Basic**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic) – einfache Daten Vorgänge schreiben die Ergebnisse in ein Textanzeige Steuerelement;
- [**DataAccess_Advanced**](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced) – integriert Daten Vorgänge in eine kleine funktionierende Anwendung, die eine einfache Datenstruktur auflistet und bearbeitet.

Beide Beispiel Lösungen enthalten IOS-und Android-Beispiel Anwendungsprojekte.

Informationen zu xamarin. Forms-Anwendungen finden Sie unter [Arbeiten mit Datenbanken](~/xamarin-forms/data-cloud/data/databases.md) , in denen erläutert wird, wie mit sqlite in einer PCL-Bibliothek mit xamarin. Forms gearbeitet wird.

## <a name="sections"></a>Abschnitte

- [Introduction (Einführung)](introduction.md)
- [Konfiguration](configuration.md)
- [Verwenden von SQLite.NET ORM](using-sqlite-orm.md)
- [Verwenden von ADO.NET](using-adonet.md)
- [Verwenden von Daten in einer App](using-data-in-an-app.md)

## <a name="summary"></a>Zusammenfassung

In diesem Kapitel wurde der Datenzugriff in xamarin. IOS mithilfe von SQLite als Datenbank-Engine erläutert. Der Zugriff auf die Datenbank kann direkt mithilfe der ADO.NET-Syntax erfolgen, oder Sie können den sqlite.net ORM einschließen und Daten C#Vorgänge in ausführen.

Wir haben zwei Beispiele überprüft: einen, der sehr einfachen Datenzugriffs Code enthält, der in ein Textfeld ausgibt, und eine einfache Anwendung, die Funktionen zum Erstellen, lesen, aktualisieren und löschen umfasst. Außerdem wurde das Threading erläutert und erläutert, wie Sie Ihre Anwendung mit einer vorab aufgefüllten SQLite-Datenbank als Ausgangs Ausgangs festlegen.

Weitere Beispiele für plattformübergreifenden Datenzugriff finden Sie in unserer [Tasky pro](~/cross-platform/app-fundamentals/building-cross-platform-applications/case-study-tasky.md) -Fallstudie.

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [IOS-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
