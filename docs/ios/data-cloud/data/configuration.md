---
title: Konfiguration
ms.prod: xamarin
ms.assetid: E5582F4B-AD74-420F-9E6D-B07CFB420B3A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 10/11/2016
ms.openlocfilehash: f4a1bc65fbba68b59196702978633eaf46bc44c5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/04/2018
---
# <a name="configuration"></a>Konfiguration

Um SQLite in Ihrer Anwendung Xamarin.iOS zu verwenden, müssen Sie den richtigen Speicherort für die Datenbankdatei zu ermitteln.

## <a name="database-file-path"></a>Pfad der Datenbankdatei

Unabhängig davon, welche Datenzugriffsmethode, die Sie verwenden, müssen Sie eine Datei erstellen, bevor Daten mit SQLite gespeichert werden können. Je nach Plattform anvisierten der Dateispeicherort unterschiedlich sind. Für iOS können "Environment"-Klasse Sie so erstellen Sie einen gültigen Pfad an, wie im folgenden Codeausschnitt gezeigt:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Es sind weitere Aspekte berücksichtigt werden, bei der Entscheidung, wo die Datenbankdatei gespeichert. Auf iOS empfiehlt sich die Datenbank gesichert automatisch (oder nicht).

Wenn Sie einen anderen Speicherort auf den verschiedenen Plattformen in Ihrer Anwendung plattformübergreifende verwenden möchten können eine Compiler-Direktive Sie wie gezeigt auf um einen anderen Pfad für jede Plattform zu generieren:

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

Finden Sie in der [arbeiten mit dem Dateisystem](~/ios/app-fundamentals/file-system.md) Artikel finden Sie weitere Informationen über welche Dateispeicherorte auf iOS verwenden. Finden Sie unter der [Building Cross Platform Applications](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) Dokument Weitere Informationen zur Verwendung von Compiler-Direktiven, um bestimmten Code für die jeweilige Plattform zu schreiben.

## <a name="threading"></a>Threading

Sie sollten die gleiche Verbindung der SQLite-Datenbank nicht über mehrere Threads hinweg verwenden. Achten Sie darauf, dass zu öffnen, verwenden und schließen Sie alle Verbindungen, die Sie auf dem gleichen Thread erstellen.

Um sicherzustellen, dass der Code nicht versucht, auf die SQLite-Datenbank aus mehreren Threads gleichzeitig zuzugreifen, führen Sie manuell eine Sperre, wenn Sie beabsichtigen, den Zugriff auf die Datenbank wie folgt:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Jeglicher Datenbankzugriff (Lesevorgänge, Schreibvorgänge, Updates usw.), sollten mit der gleichen Sperre eingeschlossen werden. Muss darauf geachtet werden eine Deadlocksituation vermeiden, indem Sie sicherstellen, dass die Arbeit in der Lock-Klausel einfach gehalten ist und nicht auf an anderen Methoden, die auch eine Sperre annehmen können.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess erweiterte (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [iOS Daten Rezepte](https://developer.xamarin.com/recipes/ios/data/sqlite/)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
