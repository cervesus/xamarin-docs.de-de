---
title: Xamarin.Forms lokale Datenbanken
description: Xamarin.Forms unterstützt die Datenbank-gestützten Anwendungen, die die SQLite-Datenbank-Engine, die es ermöglicht, laden und Speichern von Objekten in freigegebenem Code verwenden. In diesem Artikel wird beschrieben, wie Xamarin.Forms-Anwendungen lesen und Schreiben von Daten in eine lokale SQLite-Datenbank, die mithilfe von SQLite.Net können.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 05c77c4c47841a897d0d1de5c3ba004db524ea2d
ms.sourcegitcommit: 79313604ed68829435cfdbb530db36794d50858f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2018
ms.locfileid: "36310139"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.Forms lokale Datenbanken

_Xamarin.Forms unterstützt die Datenbank-gestützten Anwendungen, die die SQLite-Datenbank-Engine, die es ermöglicht, laden und Speichern von Objekten in freigegebenem Code verwenden. In diesem Artikel wird beschrieben, wie Xamarin.Forms-Anwendungen lesen und Schreiben von Daten in eine lokale SQLite-Datenbank, die mithilfe von SQLite.Net können._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Anwendungen können die [von SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) Paket integrieren Datenbankoperationen in freigegebenen Code durch Verweisen auf die `SQLite` Klassen, die in NuGet ausgeliefert. Datenbankvorgänge können in der .NET Standard Library-Projekts die Xamarin.Forms-Projektmappe definiert werden.

Die zugehörigen [beispielanwendung](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) ist eine einfache Aufgabenlisten Anwendung. Die folgenden Screenshots zeigen, wie das Beispiel auf jeder Plattform angezeigt wird:

[![Xamarin.Forms-Datenbank-Beispielscreenshots](databases-images/todo-list-sml.png "\"Todolist\" erste Page Screenshots")](databases-images/todo-list.png#lightbox "\"Todolist\" erste Page Screenshots") [ ![ Xamarin.Forms-Datenbank-Beispielscreenshots](databases-images/todo-list-sml.png "\"Todolist\" erste Page Screenshots")](databases-images/todo-list.png#lightbox "\"Todolist\" erste Page Screenshots")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Mithilfe von SQLite

Verwenden Sie die NuGet Search-Funktion zum Hinzufügen der SQLite-Unterstützung in einer Xamarin.Forms .NET Standard-Bibliothek finden **Sqlite-Net-Pcl** und das neueste Paket zu installieren:

![Hinzufügen von NuGet-SQLite.NET PCL-Pakets](databases-images/vs2017-sqlite-pcl-nuget.png "NuGet von SQLite.NET PCL-Paket hinzufügen")

Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen, das richtige Paket verfügt über diese Attribute:

- **Erstellt von:** Frank A. Krueger
- **ID:** Sqlite-Net-Plc
- **NuGet-Link:** [Sqlite-Net-Plc](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Trotz der Paketname, verwenden Sie die **Sqlite-Net-Pcl** NuGet-Paket auch in .NET Standard-Projekte.

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

Die `TodoItemDatabase` -Konstruktor, der den Pfad für die Datenbankdatei als Argument akzeptiert, wird im folgenden dargestellt:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Der Vorteil die Datenbank verfügbar zu machen, da ein Singleton ist, dass eine einzelne datenbankverbindung erstellt wird, der beim Ausführen der Anwendung geöffnet gehalten wird ausgeführt wird, aus diesem Grund und ein Datenbankvorgangs vermeidet die Kosten für öffnen und schließen die Datenbankdatei jedes Mal ausgeführt wird.

Der Rest der `TodoItemDatabase` Klasse enthält die SQLite-Abfragen, die plattformübergreifend ausgeführt. Beispielcode für die Abfrage wird unten angezeigt (Weitere Details zur Syntax finden Sie im [Verwenden von SQLite.NET, mit Xamarin.iOS](~/ios/data-cloud/data/using-sqlite-orm.md).

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
> Der Vorteil der Verwendung der asynchronen von SQLite.Net-API ist diese Datenbank, die Vorgänge in Hintergrundthreads verschoben werden. Darüber hinaus besteht keine Notwendigkeit zum Schreiben von Code behandeln, da die-API es übernimmt Erhöhung der Parallelität zur Verfügung.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms unterstützt die Datenbank-gestützten Anwendungen, die die SQLite-Datenbank-Engine, die es ermöglicht, laden und Speichern von Objekten in freigegebenem Code verwenden.

Dieser Artikel konzentriert sich auf **den Zugriff auf** eine SQLite-Datenbank mithilfe von Xamarin.Forms. Weitere Informationen zum Arbeiten mit von SQLite.Net selbst finden Sie in der [von SQLite.NET unter Android](~/android/data-cloud/data-access/using-sqlite-orm.md) oder [von SQLite.NET unter iOS](~/ios/data-cloud/data/using-sqlite-orm.md) Dokumentation.

## <a name="related-links"></a>Verwandte Links

- [Beispiel "TODO"](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)

