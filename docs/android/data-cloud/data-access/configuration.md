---
title: Konfiguration
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 10/11/2016
ms.openlocfilehash: b3a7858361d25f26807ea328e8bfdd30ca8d483b
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/25/2018
ms.locfileid: "39241876"
---
# <a name="configuration"></a>Konfiguration

Verwendung von SQLite in Ihre Xamarin.Android-Anwendung müssen Sie den richtigen Speicherort für die Datenbankdatei zu bestimmen.

## <a name="database-file-path"></a>Pfad der Datenbankdatei

Unabhängig davon, welche Datenzugriffsmethode, die Sie verwenden, müssen Sie eine Datei erstellen, bevor Sie Daten mit SQLite gespeichert werden können. Speicherort der Datei wird sich unterscheiden, je nach Zielplattform, die Sie verwenden möchten. Für Android können "Environment"-Klasse Sie zum Erstellen eines gültigen Pfads an, wie im folgenden Codeausschnitt gezeigt:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Es gibt andere Dinge berücksichtigen, bei der Entscheidung, wo die Datenbankdatei gespeichert. Beispielsweise können unter Android Sie auswählen, ob internen oder externen Speicher verwenden.

Wenn Sie einen anderen Speicherort auf jeder Plattform in Ihrer plattformübergreifenden Anwendung verwenden möchten können eine Compiler-Anweisung Sie wie gezeigt um einen anderen Pfad für jede Plattform zu generieren:

```csharp
var sqliteFilename = "MyDatabase.db3";
#if __ANDROID__
// Just use whatever directory SpecialFolder.Personal returns
string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
// we need to put in /Library/ on iOS5.1 to meet Apple's iCloud terms
// (they don't want non-user-generated data in Documents)
string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder instead
#endif
var path = Path.Combine (libraryPath, sqliteFilename);
```

Hinweise zur Verwendung von im Dateisystems in Android, finden Sie in der [Dateien durchsuchen](https://github.com/xamarin/recipes/tree/master/Recipes/android/data/files/browse_files) Anleitung. Finden Sie unter den [Building Cross Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument Weitere Informationen zur Verwendung von Compiler-Direktiven für jede Plattform spezifischen Code zu schreiben.

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
- [Rezepte für Android-Daten](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
