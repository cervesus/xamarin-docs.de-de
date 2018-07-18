---
title: Xamarin.Forms lokale Datenbanken
description: Xamarin.Forms unterstützt Datenbank datengesteuerten Anwendungen, die mit dem Datenbankmodul SQLite, wodurch es möglich ist, laden und Speichern von Objekten im freigegebenen Code. In diesem Artikel wird beschrieben, wie Xamarin.Forms lesen und Schreiben von Daten in einer lokalen SQLite.Net mit SQLite-Datenbank.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: feec4993a0719a083d713e084552b18aead8ee42
ms.sourcegitcommit: eac092f84b603958c761df305f015ff84e0fad44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2018
ms.locfileid: "36310139"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms lokale Datenbanken

_Xamarin.Forms unterstützt Datenbank datengesteuerten Anwendungen, die mit dem Datenbankmodul SQLite, wodurch es möglich ist, laden und Speichern von Objekten im freigegebenen Code. In diesem Artikel wird beschrieben, wie Xamarin.Forms lesen und Schreiben von Daten in einer lokalen SQLite.Net mit SQLite-Datenbank._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Anwendungen können die [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) Paket integrieren Sie Datenbankvorgängen in freigegebenen Code durch Verweisen auf die `SQLite` Klassen, die in der NuGet geliefert. In der .NET Standard-Bibliotheksprojekt von Xamarin.Forms-Projektmappe können Datenbankvorgänge definiert werden.

Der zugehörige [beispielanwendung](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) ist eine einfache Aufgabenlisten Anwendung. Die folgenden Screenshots zeigen, wie das Beispiel auf jeder Plattform wird angezeigt:

[![Xamarin.Forms Datenbank Beispiel Screenshots](databases-images/todo-list-sml.png "\"Todolist\" erste Page Screenshots")](databases-images/todo-list.png#lightbox "\"Todolist\" erste Page Screenshots") [ ![ Xamarin.Forms Datenbank Beispiel Screenshots](databases-images/todo-list-sml.png "\"Todolist\" erste Page Screenshots")](databases-images/todo-list.png#lightbox "\"Todolist\" erste Page Screenshots")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Verwenden von SQLite

Um eine Xamarin.Forms .NET Standardbibliothek SQLite-Unterstützung hinzugefügt haben, verwenden Sie NuGet Search-Funktion gefunden **Sqlite-Net-Pcl** und das aktuellste Paket installieren:

![Hinzufügen von NuGet SQLite.NET PCL Paket](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet SQLite.NET PCL Paket hinzufügen")

Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen, das richtige Paket wurde diese Attribute:

- **Erstellt von:** Frank A. Krueger
- **ID:** Sqlite-Net-Pcl
- **NuGet-Link:** [Sqlite-Net-Pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Verwenden Sie ungeachtet der Paketname der **Sqlite-Net-Pcl** NuGet-Paket auch in .NET Standard-Projekten.

Nachdem der Verweis hinzugefügt wurde, fügen Sie eine Eigenschaft, die `App` Klasse, die einen lokalen Pfad zum Speichern der Datenbank zurückgibt:

```csharp
static TodoItemDatabase database;

public static TodoItemDatabase Database
{
  get
  {
    if (database == null)
    {
      database = new TodoItemDatabase(
        Path.Combine(Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData), "TodoSQLite.db3"));
    }
    return database;
  }
}
```

Die `TodoItemDatabase` -Konstruktor, der den Pfad für die Datenbankdatei als Argument akzeptiert, wird unten gezeigt:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Führt der Vorteil, dass die Datenbank verfügbar machen, wie ein Singleton ist, dass eine einzelne datenbankverbindung erstellt wird, der beim Ausführen der Anwendung geöffnet ist, daher und einen Datenbankvorgang vermeidet die Kosten öffnen und Schließen der Datei jedes Mal ausgeführt wird.

Im weiteren Verlauf der `TodoItemDatabase` Klasse enthält SQLite-Abfragen, die über Plattformen hinweg ausführen. Beispielcode für die Abfrage wird unten angezeigt (Weitere Informationen zur Syntax finden Sie der [SQLite.NET verwenden](~/cross-platform/app-fundamentals/index.md) Artikel):

```csharp
public Task<List<TodoItem>> GetItemsAsync()
{
  return database.Table<TodoItem>().ToListAsync();
}

public Task<List<TodoItem>> GetItemsNotDoneAsync()
{
  return database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
}

public Task<TodoItem> GetItemAsync(int id)
{
  return database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
}

public Task<int> SaveItemAsync(TodoItem item)
{
  if (item.ID != 0)
  {
    return database.UpdateAsync(item);
  }
  else {
    return database.InsertAsync(item);
  }
}

public Task<int> DeleteItemAsync(TodoItem item)
{
  return database.DeleteAsync(item);
}
```

> [!NOTE]
> Der Vorteil der Verwendung der asynchronen SQLite.Net-API wird diese Datenbank, die Vorgänge in Hintergrundthreads verschoben werden. Darüber hinaus besteht keine Notwendigkeit, zusätzliche Verarbeitung von Code, da die API es übernimmt Parallelität zu schreiben.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms unterstützt Datenbank datengesteuerten Anwendungen, die mit dem Datenbankmodul SQLite, wodurch es möglich ist, laden und Speichern von Objekten im freigegebenen Code.

Dieser Artikel konzentriert sich auf **Zugriff auf** einer SQLite-Datenbank, indem Sie xamarin.Forms verwenden. Weitere Informationen zum Arbeiten mit SQLite.Net selbst finden Sie in der [SQLite.NET unter Android](~/android/data-cloud/data-access/using-sqlite-orm.md) oder [SQLite.NET auf iOS](~/ios/data-cloud/data/using-sqlite-orm.md) Dokumentation.

## <a name="related-links"></a>Verwandte Links

- [TODO-Beispiel](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)
- [Datenbank-Arbeitsmappe](https://developer.xamarin.com/workbooks/xamarin-forms/application-fundamentals/database/database.workbook)
