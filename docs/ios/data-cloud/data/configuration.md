---
title: Konfigurieren von SQLite in Xamarin.iOS
description: Dieses Dokument beschreibt, wie Sie den Speicherort für eine SQLite-Datenbank in einer Xamarin.iOS-Anwendung zu bestimmen. Diese Konzepte sind unabhängig von der Mechanismus für die ausgewählten Daten relevant.
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f170aa753ff490b75ab66ac858051516935876fe
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241265"
---
# <a name="configuring-sqlite-in-xamarinios"></a>Konfigurieren von SQLite in Xamarin.iOS

Verwendung von SQLite in Ihre Xamarin.iOS-Anwendung müssen Sie den richtigen Speicherort für die Datenbankdatei zu bestimmen.

## <a name="database-file-path"></a>Pfad der Datenbankdatei

Unabhängig davon, welche Datenzugriffsmethode, die Sie verwenden, müssen Sie eine Datei erstellen, bevor Sie Daten mit SQLite gespeichert werden können. Speicherort der Datei wird sich unterscheiden, je nach Zielplattform, die Sie verwenden möchten. Für iOS können "Environment"-Klasse Sie zum Erstellen eines gültigen Pfads an, wie im folgenden Codeausschnitt gezeigt:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Es gibt andere Dinge berücksichtigen, bei der Entscheidung, wo die Datenbankdatei gespeichert. Unter iOS sollten Sie die Datenbank gesichert wurden automatisch (oder nicht).

Wenn Sie einen anderen Speicherort auf jeder Plattform in Ihrer plattformübergreifenden Anwendung verwenden möchten können eine Compiler-Anweisung Sie wie gezeigt um einen anderen Pfad für jede Plattform zu generieren:

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

Finden Sie in der [arbeiten mit dem Dateisystem](~/ios/app-fundamentals/file-system.md) Artikel, um mehr über welche Speicherorte auf iOS verwenden. Finden Sie unter den [Building Cross Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument Weitere Informationen zur Verwendung von Compiler-Direktiven für jede Plattform spezifischen Code zu schreiben.

## <a name="threading"></a>Threading

Sie sollten die gleiche Verbindung der SQLite-Datenbank nicht über mehrere Threads hinweg verwenden. Achten Sie darauf, dass öffnen, verwenden Sie aus, und schließen Sie alle Verbindungen, die Sie auf dem gleichen Thread erstellen.

Um sicherzustellen, dass Ihr Code nicht versucht, die die SQLite-Datenbank von mehreren Threads gleichzeitig zugreifen, führen Sie manuell eine Sperre, wenn Sie beabsichtigen, den Zugriff auf die Datenbank, wie folgt:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Jeglicher Datenbankzugriff (Lesevorgänge, Schreibvorgänge, Updates usw.) sollten mit die gleiche Sperre eingeschlossen werden. Muss darauf geachtet werden, die eine Deadlocksituation zu vermeiden, indem Sie sicherstellen, dass die Arbeit in der Sperre-Klausel einfach gehalten ist und wird Sie an andere Methoden, die auch sperren können nicht aufgerufen.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS-Daten-Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/ios/data/sqlite)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
