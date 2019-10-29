---
title: Konfiguration
ms.prod: xamarin
ms.assetid: 44526226-4E4E-4FFF-9A16-CA7B1E01BB8F
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 10/11/2016
ms.openlocfilehash: 29bbd5c52a5af6c0ac9f9790dc182d3d806c6b5f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/29/2019
ms.locfileid: "73024027"
---
# <a name="configuration"></a>Konfiguration

Um sqlite in ihrer xamarin. Android-Anwendung zu verwenden, müssen Sie den richtigen Datei Speicherort für die Datenbankdatei festlegen.

## <a name="database-file-path"></a>Datenbankdatei Pfad

Unabhängig davon, welche Datenzugriffs Methode Sie verwenden, müssen Sie eine Datenbankdatei erstellen, bevor Daten mit SQLite gespeichert werden können. Je nachdem, welche Plattform Sie als Ziel haben, unterscheidet sich der Datei Speicherort. Für Android können Sie die Umgebungs Klasse verwenden, um einen gültigen Pfad zu erstellen, wie im folgenden Code Ausschnitt gezeigt:

```csharp
string dbPath = Path.Combine (
        Environment.GetFolderPath (Environment.SpecialFolder.Personal),
        "database.db3");
// dbPath contains a valid file path for the database file to be stored
```

Bei der Entscheidung, wo die Datenbankdatei gespeichert werden soll, sind andere Aspekte zu berücksichtigen. Unter Android können Sie z. b. auswählen, ob interner oder externer Speicher verwendet werden soll.

Wenn Sie auf jeder Plattform in ihrer plattformübergreifenden Anwendung einen anderen Speicherort verwenden möchten, können Sie eine Compilerdirektive verwenden, wie gezeigt, um für jede Plattform einen anderen Pfad zu generieren:

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

Hinweise zur Verwendung des Dateisystems in Android finden Sie in der Anleitung zum [Durchsuchen von Dateien](https://github.com/xamarin/recipes/tree/master/Recipes/android/data/files/browse_files) . Weitere Informationen zur Verwendung von Compilerdirektiven zum Schreiben von Code für die einzelnen Plattformen finden Sie im Dokument erstellen von Platt [Form übergreifenden Anwendungen](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) .

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

Der gesamte Datenbankzugriff (Lese-, Schreib-, Update-usw.) sollte mit derselben Sperre umschließt werden. Es muss darauf geachtet werden, eine Deadlocksituation zu vermeiden, indem sichergestellt wird, dass die Arbeit innerhalb der Lock-Klausel einfach gehalten wird und keine anderen Methoden aufruft, die möglicherweise auch eine Sperre annehmen!

## <a name="related-links"></a>Verwandte Links

- [DataAccess Basic (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Basic)
- [DataAccess (erweitert) (Beispiel)](https://github.com/xamarin/mobile-samples/tree/master/DataAccess/Advanced)
- [Android-Daten Rezepte](https://github.com/xamarin/recipes/tree/master/Recipes/android/data)
- [Xamarin. Forms-Datenzugriff](~/xamarin-forms/data-cloud/data/databases.md)
