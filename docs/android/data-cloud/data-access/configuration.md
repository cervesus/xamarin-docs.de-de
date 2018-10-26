---
title: Konfiguration
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/11/2016
ms.openlocfilehash: d418cb174861b962060757d4d1e0914e82ecd6fb
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "50119799"
---
# <a name="configuration"></a>Konfiguration

Verwendung von SQLite in Ihre Xamarin.Android-Anwendung müssen Sie den richtigen Speicherort für die Datenbankdatei zu bestimmen.

## <a name="database-file-path"></a>Pfad der Datenbankdatei

Bevor Sie Daten in einer SQLite-Datenbank speichern können, müssen Sie eine Datenbankdatei erstellen. Diese muss immer erstellt werden, unabhängig von der verwendeten Datenzugriffsmethode. Der Speicherort unterscheidet sich lediglich je nach Zielplattform. Für Android können "Environment"-Klasse Sie zum Erstellen eines gültigen Pfads an, wie im folgenden Codeausschnitt gezeigt:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Bei der Speicherung und dem Pfad der Datenbankdatei gibt es noch weitere Dinge zu betrachten. Beispielsweise können unter Android Sie auswählen, ob internen oder externen Speicher verwenden.

Bei plattformübergreifendenden Anwendungen können Sie eine Compileranweisung verwenden, um je nach Plattform den richtigen Pfad zu verwenden:

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

Hinweise zur Verwendung von im Dateisystems in Android, finden Sie in der [Dateien durchsuchen](https://github.com/xamarin/recipes/tree/master/Recipes/android/data/files/browse_files) Anleitung. Der Artikel [Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) enthält weitere Informationen über die Verwendung von Compilerdirektiven für plattformübergreifenden Code.

## <a name="threading"></a>Threading

Sie sollten die gleiche Verbindung zur SQLite-Datenbank nicht über mehrere Threads hinweg verwenden. Achten Sie darauf, dass das Öffnen, Lesen, Schreiben und Schließen einer Verbindung im selben Thread passiert.

Um sicherzustellen, dass Ihr Code nicht versucht, auf die SQLite-Datenbank von mehreren Threads gleichzeitig aus zuzugreifen, können Sie eine Sperre anwenden:

```csharp
object locker = new object(); // class level private field
// rest of class code
lock (locker){
    // Do your query or insert here
}
```

Jeglicher Datenbankzugriff (Lesevorgänge, Schreibvorgänge, Updates usw.) sollten mit der gleichen Sperre gekapselt werden. Vermeiden Sie eine Deadlocksituation, indem Sie sicherstellen, dass die Arbeit in der Sperre-Klausel einfach gehalten ist. Methoden in der Sperre sollten nicht auf andere Methoden zugreifen, die jeweils andere Sperren einrichten.


## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess-erweitert (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Rezepte für Android-Daten](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin.Forms-Datenzugriff](~/xamarin-forms/app-fundamentals/databases.md)
