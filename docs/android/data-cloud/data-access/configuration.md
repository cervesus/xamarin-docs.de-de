---
title: Konfiguration
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/11/2016
ms.openlocfilehash: 5ebafa70239305210da631c3e9c34278f83b272b
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70754666"
---
# <a name="configuration"></a>Konfiguration

Um sqlite in ihrer xamarin. Android-Anwendung zu verwenden, müssen Sie den richtigen Datei Speicherort für die Datenbankdatei festlegen.

## <a name="database-file-path"></a>Datenbankdatei Pfad

Bevor Sie Daten in einer SQLite-Datenbank speichern können, müssen Sie eine Datenbankdatei erstellen. Diese muss immer erstellt werden, unabhängig von der verwendeten Datenzugriffsmethode. Der Speicherort unterscheidet sich lediglich je nach Zielplattform. Für Android können Sie die Umgebungs Klasse verwenden, um einen gültigen Pfad zu erstellen, wie im folgenden Code Ausschnitt gezeigt:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Bei der Speicherung und dem Pfad der Datenbankdatei gibt es noch weitere Dinge zu betrachten. Unter Android können Sie z. b. auswählen, ob interner oder externer Speicher verwendet werden soll.

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

Hinweise zur Verwendung des Dateisystems in Android finden Sie in der Anleitung zum [Durchsuchen von Dateien](https://github.com/xamarin/recipes/tree/master/Recipes/android/data/files/browse_files) . Der Artikel [Erstellen von plattformübergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) enthält weitere Informationen über die Verwendung von Compilerdirektiven für plattformübergreifenden Code.

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

Der gesamte Datenbankzugriff (Lese-, Schreib-, Update-usw.) sollte mit derselben Sperre umschließt werden. Vermeiden Sie eine Deadlocksituation, indem Sie sicherstellen, dass die Arbeit in der Sperre-Klausel einfach gehalten ist. Methoden in der Sperre sollten nicht auf andere Methoden zugreifen, die jeweils andere Sperren einrichten.

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
