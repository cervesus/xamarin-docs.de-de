---
title: Lokale Datenbanken von Xamarin.Forms
description: Xamarin.Forms unterstützt datenbankgesteuerte Anwendungen über die SQLite-Datenbank-Engine. Dadurch ist es möglich, Objekte in freigegebenem Code zu laden und zu speichern. In diesem Artikel wird beschrieben, wie Xamarin.Forms-Apps Daten mithilfe von SQLite.Net in eine lokale SQLite-Datenbank lesen und schreiben können.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
ms.openlocfilehash: 2369afc693940d83971a43877da363e2c66b31b2
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2020
ms.locfileid: "80992404"
---
# <a name="xamarinforms-local-databases"></a>Lokale Datenbanken von Xamarin.Forms

[![Beispiel](~/media/shared/download.png) herunterladen Download des Beispiels](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

Das SQLite-Datenbankmodul ermöglicht Xamarin.Forms-Anwendungen das Laden und Speichern von Datenobjekten in freigegebenem Code. Die Beispielanwendung verwendet eine SQLite-Datenbanktabelle zum Speichern von Aufgabenelementen. In diesem Artikel wird beschrieben, wie SQLite.Net in freigegebenem Code zum Speichern und Abrufen von Informationen in einer lokalen Datenbank verwendet wird.

[![Screenshots der Todolist-App auf iOS und Android](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "Todolist-App auf iOS und Android")

Integrieren Sie SQLite.NET in mobile Apps, indem Sie die folgenden Schritte ausführen:

1. [Installieren Sie das NuGet-Paket](#install-the-sqlite-nuget-package).
1. [Konfigurieren von Konstanten](#configure-app-constants).
1. [Erstellen Sie eine Datenbankzugriffsklasse](#create-a-database-access-class).
1. [Zugriff auf Daten in Xamarin.Forms](#access-data-in-xamarinforms).
1. [Erweiterte Konfiguration](#advanced-configuration).

## <a name="install-the-sqlite-nuget-package"></a>Installieren des SQLite NuGet-Pakets

Verwenden Sie den NuGet-Paket-Manager, um nach **sqlite-net-pcl** zu suchen und dem projektfürs freigegebenen Code die neueste Version hinzuzufügen.

Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:

- **Erstellt von:** Frank A. Krueger
- **ID:** sqlite-net-pcl
- **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Verwenden Sie trotz des Paketnamens in .NET Standard-Projekten das NuGet-Paket **sqlite-net-pcl**.

## <a name="configure-app-constants"></a>Konfigurieren von App-Konstanten

Das Beispielprojekt enthält eine **Constants.cs** Datei, die allgemeine Konfigurationsdaten bereitstellt:

```csharp
public static class Constants
{
    public const string DatabaseFilename = "TodoSQLite.db3";

    public const SQLite.SQLiteOpenFlags Flags =
        // open the database in read/write mode
        SQLite.SQLiteOpenFlags.ReadWrite |
        // create the database if it doesn't exist
        SQLite.SQLiteOpenFlags.Create |
        // enable multi-threaded database access
        SQLite.SQLiteOpenFlags.SharedCache;

    public static string DatabasePath
    {
        get
        {
            var basePath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
            return Path.Combine(basePath, DatabaseFilename);
        }
    }
}
```

Die Konstantendatei gibt `SQLiteOpenFlag` Standard-Enumerumwerte an, die zum Initialisieren der Datenbankverbindung verwendet werden. Die `SQLiteOpenFlag` Enumerat unterstützt die folgenden Werte:

- `Create`: Die Verbindung erstellt automatisch die Datenbankdatei, wenn sie nicht vorhanden ist.
- `FullMutex`: Die Verbindung wird im serialisierten Threading-Modus geöffnet.
- `NoMutex`: Die Verbindung wird im Multithreading-Modus geöffnet.
- `PrivateCache`: Die Verbindung wird nicht am freigegebenen Cache beteiligt, auch wenn sie aktiviert ist.
- `ReadWrite`: Die Verbindung kann Daten lesen und schreiben.
- `SharedCache`: Die Verbindung wird am freigegebenen Cache beteiligt, sofern sie aktiviert ist.
- `ProtectionComplete`: Die Datei ist verschlüsselt und unzugänglich, während das Gerät gesperrt ist.
- `ProtectionCompleteUnlessOpen`: Die Datei wird verschlüsselt, bis sie geöffnet wird, ist dann aber auch dann zugänglich, wenn der Benutzer das Gerät sperrt.
- `ProtectionCompleteUntilFirstUserAuthentication`: Die Datei wird verschlüsselt, bis der Benutzer das Gerät gebootet und entsperrt hat.
- `ProtectionNone`: Die Datenbankdatei ist nicht verschlüsselt.

Möglicherweise müssen Sie je nach Verwendung der Datenbank unterschiedliche Flags angeben. Weitere Informationen `SQLiteOpenFlags`zu finden Sie unter [Öffnen einer neuen Datenbankverbindung](https://www.sqlite.org/c3ref/open.html) auf sqlite.org.

## <a name="create-a-database-access-class"></a>Erstellen einer Datenbankzugriffsklasse

Eine Datenbank-Wrapperklasse abstrahiert die Datenzugriffsebene vom Rest der App. Diese Klasse zentralisiert die Abfragelogik und vereinfacht die Verwaltung der Datenbankinitialisierung, wodurch Datenvorgänge bei wachsendem Wachstum einfacher gestaltet oder erweitert werden können. Die Todo-App `TodoItemDatabase` definiert eine Klasse für diesen Zweck.

### <a name="lazy-initialization"></a>Verzögerte Initialisierung

Der `TodoItemDatabase` verwendet die `Lazy` .NET-Klasse, um die Initialisierung der Datenbank zu verzögern, bis sie zum ersten Mal aufgerufen wird. Die Verwendung einer verzögerten Initialisierung verhindert, dass der Ladevorgang der Datenbank den Start der App verzögert. Weitere Informationen finden Sie unter [Lazy&lt;T&gt; Class](xref:System.Lazy`1).

```csharp
public class TodoItemDatabase
{
    static readonly Lazy<SQLiteAsyncConnection> lazyInitializer = new Lazy<SQLiteAsyncConnection>(() =>
    {
        return new SQLiteAsyncConnection(Constants.DatabasePath, Constants.Flags);
    });

    static SQLiteAsyncConnection Database => lazyInitializer.Value;
    static bool initialized = false;

    public TodoItemDatabase()
    {
        InitializeAsync().SafeFireAndForget(false);
    }

    async Task InitializeAsync()
    {
        if (!initialized)
        {
            if (!Database.TableMappings.Any(m => m.MappedType.Name == typeof(TodoItem).Name))
            {
                await Database.CreateTablesAsync(CreateFlags.None, typeof(TodoItem)).ConfigureAwait(false);
                initialized = true;
            }
        }
    }

    //...
}
```

Die Datenbankverbindung ist ein statisches Feld, das sicherstellt, dass eine einzelne Datenbankverbindung für die Lebensdauer der App verwendet wird. Die Verwendung einer persistenten, statischen Verbindung bietet eine bessere Leistung als das Mehrfachöffnen und Schließen von Verbindungen während einer einzelnen App-Sitzung.

Die `InitializeAsync` Methode ist dafür verantwortlich, zu `TodoItem` überprüfen, ob bereits eine Tabelle zum Speichern von Objekten vorhanden ist. Diese Methode erstellt die Tabelle automatisch, wenn sie nicht vorhanden ist.

### <a name="the-safefireandforget-extension-method"></a>Die SafeFireAndForget-Erweiterungsmethode

Wenn `TodoItemDatabase` die Klasse instanziiert wird, muss sie die Datenbankverbindung initialisieren, bei der es sich um einen asynchronen Prozess handelt. Allerdings:

- Klassenkonstruktoren können nicht asynchron sein.
- Eine asynchrone Methode, die nicht erwartet wird, löst keine Ausnahmen aus.
- Die `Wait` Methode blockiert den Thread _und_ schluckt Ausnahmen.

Um die asynchrone Initialisierung zu starten, die Blockierung der Ausführung zu vermeiden und Ausnahmen `SafeFireAndForget`abfangen zu können, verwendet die Beispielanwendung eine Erweiterungsmethode namens . Die `SafeFireAndForget` Erweiterungsmethode bietet `Task` zusätzliche Funktionen für die Klasse:

```csharp
public static class TaskExtensions
{
    // NOTE: Async void is intentional here. This provides a way
    // to call an async method from the constructor while
    // communicating intent to fire and forget, and allow
    // handling of exceptions
    public static async void SafeFireAndForget(this Task task,
        bool returnToCallingContext,
        Action<Exception> onException = null)
    {
        try
        {
            await task.ConfigureAwait(returnToCallingContext);
        }

        // if the provided action is not null, catch and
        // pass the thrown exception
        catch (Exception ex) when (onException != null)
        {
            onException(ex);
        }
    }
}
```

Die `SafeFireAndForget` Methode wartet auf die asynchrone Ausführung des bereitgestellten `Task` Objekts und ermöglicht das Anfügen eines, `Action` das aufgerufen wird, wenn eine Ausnahme ausgelöst wird.

Weitere Informationen finden Sie unter [Task-basiertes asynchrones Muster (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap).

### <a name="data-manipulation-methods"></a>Methoden zur Datenmanipulation

Die `TodoItemDatabase` Klasse enthält Methoden für die vier Arten der Datenmanipulation: Erstellen, Lesen, Bearbeiten und Löschen. Die SQLite.NET-Bibliothek bietet eine einfache Object Relational Map (ORM), mit der Sie Objekte speichern und abrufen können, ohne SQL-Anweisungen zu schreiben.

```csharp
public class TodoItemDatabase {

    // ...

    public Task<List<TodoItem>> GetItemsAsync()
    {
        return Database.Table<TodoItem>().ToListAsync();
    }

    public Task<List<TodoItem>> GetItemsNotDoneAsync()
    {
        // SQL queries are also possible
        return Database.QueryAsync<TodoItem>("SELECT * FROM [TodoItem] WHERE [Done] = 0");
    }

    public Task<TodoItem> GetItemAsync(int id)
    {
        return Database.Table<TodoItem>().Where(i => i.ID == id).FirstOrDefaultAsync();
    }

    public Task<int> SaveItemAsync(TodoItem item)
    {
        if (item.ID != 0)
        {
            return Database.UpdateAsync(item);
        }
        else
        {
            return Database.InsertAsync(item);
        }
    }

    public Task<int> DeleteItemAsync(TodoItem item)
    {
        return Database.DeleteAsync(item);
    }
}
```

## <a name="access-data-in-xamarinforms"></a>Zugriff auf Daten in Xamarin.Forms

Die Xamarin.Forms-Klasse `App` macht eine `TodoItemDatabase` Instanz der Klasse verfügbar:

```csharp
static TodoItemDatabase database;
public static TodoItemDatabase Database
{
    get
    {
        if (database == null)
        {
            database = new TodoItemDatabase();
        }
        return database;
    }
}
```

Mit dieser Eigenschaft können Xamarin.Forms-Komponenten Datenabruf- und -manipulationsmethoden für die `Database` Instanz als Reaktion auf Benutzerinteraktionaufrufen aufrufen. Beispiel:

```csharp
var saveButton = new Button { Text = "Save" };
saveButton.Clicked += async (sender, e) =>
{
    var todoItem = (TodoItem)BindingContext;
    await App.Database.SaveItemAsync(todoItem);
    await Navigation.PopAsync();
};
```

## <a name="advanced-configuration"></a>Erweiterte Konfiguration

SQLite bietet eine robuste API mit mehr Funktionen, als in diesem Artikel und der Beispiel-App behandelt werden. In den folgenden Abschnitten werden Features behandelt, die für die Skalierbarkeit wichtig sind.

Weitere Informationen finden Sie unter [SQLite-Dokumentation](https://www.sqlite.org/docs.html) auf sqlite.org.

### <a name="write-ahead-logging"></a>Write-Ahead-Protokollierung

Standardmäßig verwendet SQLite ein herkömmliches Rollback-Journal. Eine Kopie des unveränderten Datenbankinhalts wird in eine separate Rollbackdatei geschrieben, dann werden die Änderungen direkt in die Datenbankdatei geschrieben. Das COMMIT tritt auf, wenn das Rollback-Journal gelöscht wird.

Write-Ahead Logging (WAL) schreibt Änderungen zuerst in eine separate WAL-Datei. Im WAL-Modus ist ein COMMIT ein spezieller Datensatz, der an die WAL-Datei angehängt wird, wodurch mehrere Transaktionen in einer einzelnen WAL-Datei ausgeführt werden können. Eine WAL-Datei wird in einem speziellen Vorgang, der als _Prüfpunkt_bezeichnet wird, wieder in die Datenbankdatei zusammengeführt.

WAL kann für lokale Datenbanken schneller sein, da Leser und Writer einander nicht blockieren, sodass Lese- und Schreibvorgänge gleichzeitig durchgeführt werden können. Der WAL-Modus lässt jedoch keine Änderungen an der _Seitengröße_zu, fügt der Datenbank zusätzliche Dateizuordnungen hinzu und fügt den zusätzlichen _Prüfvorgang_ hinzu.

Um WAL in SQLite.NET `EnableWriteAheadLoggingAsync` zu aktivieren, rufen Sie die Methode für die `SQLiteAsyncConnection` Instanz auf:

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

Weitere Informationen finden Sie unter [SQLite Write-Ahead Logging](https://www.sqlite.org/wal.html) on sqlite.org.

### <a name="copying-a-database"></a>Kopieren einer Datenbank

Es gibt mehrere Fälle, in denen es erforderlich sein kann, eine SQLite-Datenbank zu kopieren:

- Eine Datenbank wurde mit Ihrer Anwendung ausgeliefert, muss jedoch kopiert oder in den beschreibbaren Speicher auf dem mobilen Gerät verschoben werden.
- Sie müssen eine Sicherung oder Kopie der Datenbank erstellen.
- Sie müssen die Datenbankdatei versionieren, verschieben oder umbenennen.

Im Allgemeinen ist das Verschieben, Umbenennen oder Kopieren einer Datenbankdatei derselbe Prozess wie jeder andere Dateityp mit einigen zusätzlichen Überlegungen:

- Alle Datenbankverbindungen sollten geschlossen werden, bevor versucht wird, die Datenbankdatei zu verschieben.
- Wenn Sie [Write-Ahead Logging](#write-ahead-logging)verwenden, erstellt SQLite eine Shared Memory Access (.shm)-Datei und eine Datei (Write Ahead Log) (.wal). Stellen Sie sicher, dass Sie auch Änderungen an diesen Dateien vornehmen.

Weitere Informationen finden Sie [unter Dateibehandlung in Xamarin.Forms](~/xamarin-forms/data-cloud/data/files.md).

## <a name="related-links"></a>Verwandte Links

- [Todo-Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.NET NuGet-Paket](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite-Dokumentation](https://www.sqlite.org/docs.html)
- [Verwenden von SQLite mit Android](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [Verwenden von SQLite mit iOS](~/ios/data-cloud/data/using-sqlite-orm.md)
- [Taskbasiertes asynchrones Muster (TAP)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [Lazy&lt;&gt; T-Klasse](xref:System.Lazy`1)
