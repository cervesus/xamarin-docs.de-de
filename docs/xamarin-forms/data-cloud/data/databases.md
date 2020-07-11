---
title: Xamarin.FormsLokale Datenbanken
description: Xamarin.Formsunterstützt datenbankgesteuerte Anwendungen mithilfe der SQLite-Datenbank-Engine, die das Laden und Speichern von Objekten in frei gegebenem Code ermöglicht. In diesem Artikel wird beschrieben Xamarin.Forms , wie Anwendungen mithilfe von SQLite.NET Daten in einer lokalen SQLite-Datenbank lesen und schreiben können.
ms.prod: xamarin
ms.assetid: F687B24B-7DF0-4F8E-A21A-A9BB507480EB
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 12/05/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 2268f9034a4b09adce697f5fb7b6652baa4feed6
ms.sourcegitcommit: 898ba8e5140ae32a7df7e07c056aff65f6fe4260
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2020
ms.locfileid: "86226819"
---
# <a name="xamarinforms-local-databases"></a>Xamarin.FormsLokale Datenbanken

[![Beispiel herunterladen](~/media/shared/download.png) Das Beispiel herunterladen](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)

Die SQLite-Datenbank-Engine ermöglicht Xamarin.Forms Anwendungen das Laden und Speichern von Datenobjekten in frei gegebenem Code. Die Beispielanwendung verwendet eine SQLite-Datenbanktabelle zum Speichern von TODO-Elementen. In diesem Artikel wird beschrieben, wie Sie sqlite.net in frei gegebenem Code verwenden, um Informationen in einer lokalen Datenbank zu speichern und abzurufen.

[![Screenshots der App "App-Liste" unter IOS und Android](databases-images/todo-list-sml.png)](databases-images/todo-list.png#lightbox "App-Liste unter IOS und Android")

Integrieren Sie sqlite.net in Mobile Apps, indem Sie die folgenden Schritte ausführen:

1. [Installieren Sie das nuget-Paket](#install-the-sqlite-nuget-package).
1. [Konfigurieren von Konstanten](#configure-app-constants).
1. [Erstellen Sie eine Datenbankzugriffs Klasse](#create-a-database-access-class).
1. [Zugreifen auf Daten Xamarin.Forms in ](#access-data-in-xamarinforms).
1. [Erweiterte Konfiguration](#advanced-configuration).

## <a name="install-the-sqlite-nuget-package"></a>Installieren des SQLite-nuget-Pakets

Verwenden Sie den nuget-Paket-Manager, um nach **SQLite-net-PCL** zu suchen und die neueste Version dem Projekt mit dem freigegebenen Code hinzuzufügen.

Es gibt eine Reihe von NuGet-Paketen mit ähnlichen Namen. Das richtige Paket verfügt über die folgenden Attribute:

- **ID:** sqlite-net-pcl
- **Autor (e):** SQLite-net
- **Besitzer:** praeclarum
- **Projekt-URL:**https://github.com/praeclarum/sqlite-net
- **NuGet-Link:** [sqlite-net-pcl](https://www.nuget.org/packages/sqlite-net-pcl/)

> [!NOTE]
> Verwenden Sie trotz des Paketnamens in .NET Standard-Projekten das NuGet-Paket **sqlite-net-pcl**.

## <a name="configure-app-constants"></a>Konfigurieren von App-Konstanten

Das Beispiel Projekt enthält eine **Constants.cs** -Datei, die allgemeine Konfigurationsdaten bereitstellt:

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

Die Konstanten Datei gibt Standardenumerationswerte `SQLiteOpenFlag` an, die verwendet werden, um die Datenbankverbindung zu initialisieren. Die Enumeration `SQLiteOpenFlag` unterstützt diese Werte:

- `Create`: Die Datenbankdatei wird von der Verbindung automatisch erstellt, wenn Sie nicht vorhanden ist.
- `FullMutex`: Die Verbindung wird im serialisierten Threading Modus geöffnet.
- `NoMutex`: Die Verbindung wird im Multithreadmodus geöffnet.
- `PrivateCache`: Die Verbindung wird nicht am freigegebenen Cache beteiligt, auch wenn Sie aktiviert ist.
- `ReadWrite`: Die Verbindung kann Daten lesen und schreiben.
- `SharedCache`: Die Verbindung wird am freigegebenen Cache beteiligt, wenn Sie aktiviert ist.
- `ProtectionComplete`: Die Datei ist verschlüsselt und nicht zugänglich, solange das Gerät gesperrt ist.
- `ProtectionCompleteUnlessOpen`: Die Datei wird so lange verschlüsselt, bis Sie geöffnet ist, aber auch dann zugänglich ist, wenn der Benutzer das Gerät sperrt.
- `ProtectionCompleteUntilFirstUserAuthentication`: Die Datei wird verschlüsselt, bis der Benutzer das Gerät gestartet und entsperrt hat.
- `ProtectionNone`: Die Datenbankdatei ist nicht verschlüsselt.

Je nachdem, wie die Datenbank verwendet wird, müssen Sie möglicherweise unterschiedliche Flags angeben. Weitere Informationen zu `SQLiteOpenFlags` finden Sie unter [Öffnen einer neuen Datenbankverbindung](https://www.sqlite.org/c3ref/open.html) auf sqlite.org.

## <a name="create-a-database-access-class"></a>Erstellen einer Datenbankzugriffs Klasse

Eine Datenbank-Wrapper Klasse abstrahiert die Datenzugriffs Ebene vom Rest der app. Mit dieser Klasse wird die Abfrage Logik zentralisiert, und die Verwaltung der Daten Bank Initialisierung wird vereinfacht, sodass Daten Vorgänge einfacher umgestalten oder erweitert werden können, wenn die APP wächst. Die ToDo-APP definiert `TodoItemDatabase` zu diesem Zweck eine Klasse.

### <a name="lazy-initialization"></a>Verzögerte Initialisierung

Der `TodoItemDatabase` verwendet die .net `Lazy` -Klasse, um die Initialisierung der Datenbank zu verzögern, bis der erste Zugriff erfolgt. Die Verwendung der verzögerten Initialisierung verhindert, dass der Daten Bank Ladevorgang den App-Start verzögert. Weitere Informationen finden Sie unter [Lazy &lt; T &gt; Class](xref:System.Lazy`1).

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

Die Datenbankverbindung ist ein statisches Feld, mit dem sichergestellt wird, dass eine einzelne Datenbankverbindung für die Lebensdauer der APP verwendet wird. Die Verwendung einer permanenten, statischen Verbindung bietet eine bessere Leistung als das mehrfach öffnen und Schließen von Verbindungen während einer einzelnen App-Sitzung.

Die- `InitializeAsync` Methode ist dafür verantwortlich, zu überprüfen, ob bereits eine Tabelle zum Speichern von Objekten vorhanden ist `TodoItem` . Diese Methode erstellt die Tabelle automatisch, wenn Sie nicht vorhanden ist.

### <a name="the-safefireandforget-extension-method"></a>Die safefireandforget-Erweiterungsmethode

Wenn die `TodoItemDatabase` Klasse instanziiert wird, muss Sie die Datenbankverbindung initialisieren, bei der es sich um einen asynchronen Prozess handelt. Dabei gilt jedoch Folgendes:

- Klassenkonstruktoren dürfen nicht asynchron sein.
- Eine Async-Methode, die nicht erwartet wird, löst keine Ausnahmen aus.
- Durch die Verwendung der `Wait` -Methode werden der Thread blockiert _und_ Ausnahmen werden verschluckt.

Um die asynchrone Initialisierung zu starten, eine blockierende Ausführung zu vermeiden und die Möglichkeit zum Abfangen von Ausnahmen zu haben, verwendet die Beispielanwendung eine Erweiterungsmethode mit dem Namen `SafeFireAndForget` . Die- `SafeFireAndForget` Erweiterungsmethode bietet zusätzliche Funktionen für die- `Task` Klasse:

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

Die `SafeFireAndForget` -Methode wartet auf die asynchrone Ausführung des bereitgestellten `Task` -Objekts und ermöglicht das `Action` Anfügen eines, das aufgerufen wird, wenn eine Ausnahme ausgelöst wird.

Weitere Informationen finden Sie unter [Task basiertes asynchrones Muster (Tap)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap).

### <a name="data-manipulation-methods"></a>Daten Bearbeitungsmethoden

Die- `TodoItemDatabase` Klasse enthält Methoden für die vier Arten von Datenbearbeitung: erstellen, lesen, bearbeiten und löschen. Die SQLite.NET-Bibliothek stellt eine einfache Objekt relationale Zuordnung (ORM) bereit, die das Speichern und Abrufen von Objekten ohne das Schreiben von SQL-Anweisungen ermöglicht.

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

## <a name="access-data-in-xamarinforms"></a>Datenzugriff inXamarin.Forms

Die- Xamarin.Forms `App` Klasse macht eine Instanz der- `TodoItemDatabase` Klasse verfügbar:

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

Diese Eigenschaft ermöglicht es Xamarin.Forms Komponenten, Datenabruf-und Bearbeitungsmethoden für die- `Database` Instanz als Reaktion auf eine Benutzerinteraktion aufzurufen. Beispiel:

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

SQLite bietet eine robuste API mit mehr Funktionen als in diesem Artikel und der Beispiel-App behandelt werden. In den folgenden Abschnitten werden Features behandelt, die für die Skalierbarkeit wichtig sind.

Weitere Informationen finden Sie in der [SQLite-Dokumentation](https://www.sqlite.org/docs.html) auf sqlite.org.

### <a name="write-ahead-logging"></a>Write-Ahead-Protokollierung

Standardmäßig verwendet SQLite ein herkömmliches Rollback-Journal. Eine Kopie des unveränderten Daten Bank Inhalts wird in eine separate Rollback-Datei geschrieben. die Änderungen werden dann direkt in die Datenbankdatei geschrieben. Der Commit tritt auf, wenn das Rollback-Journal gelöscht wird.

Bei der Write-Ahead-Protokollierung (Wal) werden Änderungen zuerst in eine separate Wal-Datei geschrieben. Im Wal-Modus ist ein Commit ein spezieller Datensatz, der an die Wal-Datei angehängt wird. Dadurch können mehrere Transaktionen in einer einzelnen Wal-Datei auftreten. Eine Wal-Datei wird in einem speziellen Vorgang, der als Prüfpunkt bezeichnet wird, in der Datenbankdatei zusammen _geführt._

Wal kann für lokale Datenbanken schneller sein, da Reader und Writer einander nicht blockieren, sodass Lese-und Schreibvorgänge gleichzeitig durchführen können. Der Wal-Modus lässt jedoch keine Änderungen an der _Seitengröße_zu, fügt der Datenbank zusätzliche Dateizuordnungen hinzu und fügt den zusätzlichen _Prüf_ Punkt Vorgang hinzu.

Um Wal in SQLite.net zu aktivieren, müssen Sie die- `EnableWriteAheadLoggingAsync` Methode für die-Instanz aufzurufen `SQLiteAsyncConnection` :

```csharp
await Database.EnableWriteAheadLoggingAsync();
```

Weitere Informationen finden Sie unter [SQLite Write-Ahead Logging](https://www.sqlite.org/wal.html) on sqlite.org.

### <a name="copying-a-database"></a>Kopieren einer Datenbank

Es gibt mehrere Fälle, in denen es erforderlich sein kann, eine SQLite-Datenbank zu kopieren:

- Eine Datenbank wurde mit Ihrer Anwendung ausgeliefert, muss jedoch kopiert oder in beschreibbaren Speicher auf dem mobilen Gerät verschoben werden.
- Sie müssen eine Sicherung oder Kopie der Datenbank erstellen.
- Sie müssen die Datenbankdatei versieren, verschieben oder umbenennen.

Im Allgemeinen ist das Verschieben, umbenennen oder Kopieren einer Datenbankdatei der gleiche Prozess wie jeder andere Dateityp mit einigen zusätzlichen Überlegungen:

- Alle Datenbankverbindungen sollten geschlossen werden, bevor versucht wird, die Datenbankdatei zu verschieben.
- Wenn Sie die [Write-Ahead-Protokollierung](#write-ahead-logging)verwenden, erstellt SQLite eine Datei für den freigegebenen Speicherzugriff (. SHM) und eine Datei (. Wal) (Write Ahead Log). Stellen Sie sicher, dass Sie auch alle Änderungen an diesen Dateien anwenden.

Weitere Informationen finden Sie unter [Datei Behandlung in Xamarin.Forms ](~/xamarin-forms/data-cloud/data/files.md).

## <a name="related-links"></a>Verwandte Links

- [TODO-Beispielanwendung](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo)
- [SQLite.net nuget-Paket](https://www.nuget.org/packages/sqlite-net-pcl/)
- [SQLite-Dokumentation](https://www.sqlite.org/docs.html)
- [Verwenden von SQLite mit Android](~/android/data-cloud/data-access/using-sqlite-orm.md)
- [Verwenden von SQLite mit IOS](~/ios/data-cloud/data/using-sqlite-orm.md)
- [Aufgabenbasiertes asynchrones Muster (tippen)](https://docs.microsoft.com/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
- [Lazy &lt; T- &gt; Klasse](xref:System.Lazy`1)
