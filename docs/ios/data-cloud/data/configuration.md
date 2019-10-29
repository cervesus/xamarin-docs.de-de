---
title: Konfigurieren von sqlite in xamarin. IOS
description: In diesem Dokument wird beschrieben, wie Sie den Speicherort für eine SQLite-Datenbankdatei in einer xamarin. IOS-Anwendung bestimmen. Diese Konzepte sind unabhängig vom ausgewählten Datenzugriffsmechanismus relevant.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: 9140b98511aae5e24ee8aaf069668a793221c1a7
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73009052"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Konfigurieren von sqlite in xamarin. IOS

Um sqlite in ihrer xamarin. IOS-Anwendung verwenden zu können, müssen Sie den richtigen Datei Speicherort für die Datenbankdatei festlegen.

## <a name="database-file-path"></a>Datenbankdatei Pfad

Unabhängig davon, welche Datenzugriffs Methode Sie verwenden, müssen Sie eine Datenbankdatei erstellen, bevor Daten mit SQLite gespeichert werden können. Je nachdem, welche Plattform Sie als Ziel haben, unterscheidet sich der Datei Speicherort. Für IOS können Sie die Umgebungs Klasse verwenden, um einen gültigen Pfad zu erstellen, wie im folgenden Code Ausschnitt gezeigt:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Bei der Entscheidung, wo die Datenbankdatei gespeichert werden soll, sind andere Aspekte zu berücksichtigen. Unter IOS kann es sein, dass die Datenbank automatisch gesichert werden soll (oder nicht).

Wenn Sie auf jeder Plattform in ihrer plattformübergreifenden Anwendung einen anderen Speicherort verwenden möchten, können Sie eine Compilerdirektive verwenden, wie gezeigt, um für jede Plattform einen anderen Pfad zu generieren:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Weitere Informationen zu den Datei Standorten, die unter IOS verwendet werden sollen, finden Sie im Artikel [Arbeiten mit dem Datei System](~/ios/app-fundamentals/file-system.md) . Weitere Informationen zur Verwendung von Compilerdirektiven zum Schreiben von Code für die einzelnen Plattformen finden Sie im Dokument erstellen von Platt [Form übergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) .

## <a name="threading"></a>Threading

Sie sollten nicht dieselbe SQLite-Datenbankverbindung für mehrere Threads verwenden. Achten Sie darauf, alle Verbindungen, die Sie im gleichen Thread erstellen, zu öffnen, zu verwenden und dann zu schließen.

Um sicherzustellen, dass Ihr Code nicht gleichzeitig von mehreren Threads aus auf die SQLite-Datenbank zugreifen kann, nehmen Sie manuell eine Sperre vor, wenn Sie wie folgt auf die Datenbank zugreifen:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Der gesamte Datenbankzugriff (Lese-, Schreib-, Aktualisierungs-usw.) sollte mit derselben Sperre umschließt werden. Es muss darauf geachtet werden, eine Deadlocksituation zu vermeiden, indem sichergestellt wird, dass die Arbeit innerhalb der Lock-Klausel einfach gehalten wird und keine anderen Methoden aufruft, die möglicherweise auch eine Sperre annehmen!

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [IOS-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
