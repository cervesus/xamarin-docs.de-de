---
title: Lokale Datenbanken von Xamarin.Forms
description: Xamarin.Forms unterstützt datenbankgesteuerte Anwendungen über die SQLite-Datenbank-Engine. Dadurch ist es möglich, Objekte in freigegebenem Code zu laden und zu speichern. In diesem Artikel wird beschrieben, wie Xamarin.Forms-Apps Daten mithilfe von SQLite.Net in eine lokale SQLite-Datenbank lesen und schreiben können.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/21/2018
ms.openlocfilehash: 235a30293939333555c52b8d9e00bcf25ddd4dbd
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/07/2018
ms.locfileid: "53055960"
---
# <a name="xamarinforms-local-databases"></a>Lokale Datenbanken von Xamarin.Forms

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://developer.xamarin.com/samples/xamarin-forms/Todo/)

_Xamarin.Forms unterstützt datenbankgesteuerte Anwendungen über die SQLite-Datenbank-Engine. Dadurch ist es möglich, Objekte in freigegebenem Code zu laden und zu speichern. In diesem Artikel wird beschrieben, wie Xamarin.Forms-Apps Daten mithilfe von SQLite.Net in eine lokale SQLite-Datenbank lesen und schreiben können._

## <a name="overview"></a>Übersicht

Xamarin.Forms-Apps können das Paket [SQLite.NET PCL NuGet](https://www.nuget.org/packages/sqlite-net-pcl/) verwenden, um Datenbankvorgänge in freigegebenen Code zu integrieren, indem auf die `SQLite`-Klassen verwiesen wird, die im NuGet-Paket enthalten sind. Datenbankvorgänge können im Projekt der .NET Standard-Bibliothek der Xamarin.Forms-Projektmappe definiert werden.

Bei der zugehörigen [Beispielanwendung](https://github.com/xamarin/xamarin-forms-samples/tree/master/Todo) handelt es sich um eine einfache App für To-do-Listen. Auf den folgenden Screenshots wird veranschaulicht, wie das Beispiel auf jeder Plattform angezeigt wird:

[![Screenshots der Xamarin.Forms-Beispieldatenbank](databases-images/todo-list-sml.png "Screenshots der ersten Seite von TodoList")](databases-images/todo-list.png#lightbox "Screenshots der ersten Seite von TodoList") [![Screenshots der Xamarin.Forms-Beispieldatenbank](databases-images/todo-list-sml.png "Screenshots der ersten Seite von TodoList")](databases-images/todo-list.png#lightbox "Screenshots der ersten Seite von TodoList")

<a name="Using_SQLite_with_PCL" />

## <a name="using-sqlite"></a>Verwendung von SQLite

Verwenden Sie zum Hinzufügen des SQLite-Support zu einer .NET Standard-Bibliothek von Xamarin.Forms die Suchfunktion von NuGet, um **sqlite-net-pcl** zu finden und das neueste Paket zu installieren:

![Hinzufügen des Pakets von NuGet SQLite.NET PCL](databases-images/vs2017-sqlite-pcl-nuget.png "Hinzufügen des Pakets von NuGet SQLite.NET PCL")

Es gibt mehrere NuGet-Pakete mit ähnlichen Namen. Das richtige Paket verfügt über folgende Attribute:

- **Erstellt von:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Verwenden Sie trotz des Paketnamens in .NET Standard-Projekten das NuGet-Paket **sqlite-net-pcl**.

Wenn der Verweis hinzugefügt wurde, fügen Sie eine Eigenschaft zur `App`-Klasse hinzu, die einen lokalen Dateipfad zum Speichern der Datenbank zurückgibt:

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

Der Konstruktor `TodoItemDatabase`, der den Pfad zur Datenbankdatei als Argument verwendet, wird nachfolgend gezeigt:

```csharp
public TodoItemDatabase(string dbPath)
{
  database = new SQLiteAsyncConnection(dbPath);
  database.CreateTableAsync<TodoItem>().Wait();
}
```

Durch das Bereitstellen der Datenbank als Singleton kann eine einzelne Datenbankverbindung erstellt werden, die während der Ausführung der App offen bleibt, sodass der Aufwand für das Öffnen und Schließen der Datenbank beim Ausführen des Datenbankvorgangs vermieden wird.

Der Rest der `TodoItemDatabase`-Klasse enthält SQLite-Abfragen, die plattformübergreifend ausgeführt werden. Nachfolgend wird ein Beispielcode für eine Abfrage angezeigt. Weitere Informationen finden Sie unter [Using SQLite.NET with Xamarin.iOS (Verwenden von SQLite.NET mit Xamarin.iOS)](~/ios/data-cloud/data/using-sqlite-orm.md).

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
> Durch die Verwendung der asynchronen SQLite.Net-API können Datenbankvorgänge in Hintergrundthreads verschoben werden. Darüber hinaus muss kein zusätzlicher Parallelitätsbearbeitungscode geschrieben werden, da sich die API darum kümmert.

## <a name="summary"></a>Zusammenfassung

Xamarin.Forms unterstützt datenbankgesteuerte Anwendungen über die SQLite-Datenbank-Engine. Dadurch ist es möglich, Objekte in freigegebenem Code zu laden und zu speichern.

Im Artikel wird hauptsächlich der **Zugriff** auf eine SQLite-Datenbank mit Xamarin.Forms beschrieben. Weitere Informationen über das Arbeiten mit SQLite.Net finden Sie in den Dokumentationen [Using SQLite.NET with Android (Verwenden von SQLite.NET mit Android)](~/android/data-cloud/data-access/using-sqlite-orm.md) oder [Using SQLite.NET with Xamarin.iOS (Verwenden von SQLite.NET mit Xamarin.iOs)](~/ios/data-cloud/data/using-sqlite-orm.md).

## <a name="related-links"></a>Verwandte Links

- [Todo Sample (Todo (Beispiel))](https://developer.xamarin.com/samples/xamarin-forms/Todo/)
- [Xamarin.Forms Samples (Beispiele für Xamarin.Forms)](https://developer.xamarin.com/samples/xamarin-forms/all/)

